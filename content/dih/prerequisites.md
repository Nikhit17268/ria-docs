---
 title: Conversion Run Overview
 Weight: 1
---

## Overview

As mentioned in other sections, a conversion run is usually divided into two parts:

1. **Convert all the data in the staging schema (`STGADM`).**
2. **Copy the converted data from the staging to the production schema (`CISADM`).**

These steps are repeatable, both individually and together. Multiple conversions can be performed before the copy step. Similarly, the copy-to-production process must also be repeatable in case issues arise during copying or if the first step is re-executed afterward.

> **Note:** The conversion in the staging schema should delete any previously converted data from the staging tables before populating them again. This should be built into the flow, as described under **Data Migration Flow**.

To make the **copy-to-production** process repeatable, the tool must delete previously copied data from the production schema (`CISADM`) before copying again. 

- In **new conversions**, target tables in production are typically empty and can be truncated.
- In **phased conversions**, precise deletion is necessary to avoid affecting existing production data.

---

## Data Tracking with `CX_CONVERTED_ID` Table

The precise replacement of converted data is achieved by tracking copied data in a separate table.  
The recommended approach is the `CX_CONVERTED_ID` table for moderate data volumes. For higher volumes, separate tables might offer better performance and manageability.

### Purpose

This table keeps track of which data was copied from `STGADM` to `CISADM`.

### SQL to Create the Table

```sql
create table cx_converted_id (
    table_name varchar2(128 byte),
    cx_id varchar2(128 byte),
    constraint cx_converted_id_pk primary key (cx_id)
);
```

### Key Notes

- `table_name`: The name of the table that was copied.
- `cx_id`: The primary ID (or the most significant part of a composite key).
    - For example, for `CI_PER_NAME` (which uses `PER_ID` + `SEQ_NUM`), only `PER_ID` is stored.
    - This ID is sufficient for identifying deletable rows.

### Tool Usage

- `SQL_DELETE_FROM_CISADM`: Deletes records in `CISADM` based on IDs in `CX_CONVERTED_ID`, then clears the tracking table.
- `SQL_COPY_TO_CISADM`: Copies relevant data from staging to production and populates `CX_CONVERTED_ID` accordingly.

---

## Informational Table: `CX_CONVERTED_TABLE`

The `CX_CONVERTED_TABLE` is used to map each table name to its primary key column.  
While `CX_CONVERTED_ID` stores just the values, this table provides the corresponding column names.

### SQL to Create the Table

```sql
create table cx_converted_table (
    table_name varchar2(128), 
    column_name varchar2(128), 
    constraint cx_converted_table_pk primary key (table_name)
);
```

### Population

Populated after data is copied to production using the executable `POST_PUSH_PROC`, which runs the following SQL:

```sql
insert into cx_converted_table (table_name, column_name)
select id.table_name, tc.column_id
from 
    (select distinct table_name from cx_converted_id) id,
    (select table_id, 
            column_id, 
            row_number() over(partition by table_id order by column_seq asc) row_num
       from ua_table_column 
      where primary_key = 1) tc
where id.table_name = tc.table_id
  and tc.row_num = 1
order by table_name;
```

---
