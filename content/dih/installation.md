---
title: Installation
Weight: 1
---

```markdown
# Data Integration Hub (DIH) Installation Guide

## Overview
Data Integration Hub consists of a web application with an embedded Tomcat container, and a standalone Java runtime server.  
Except for the pre-requisites listed below, no additional third-party downloads or installations should be required.

---

## DIH Components

The components comprising DIH include the following (see **Architecture** for more details):

- DIH web application  
- Runtime server  
- Angular UI application  

---

## Prerequisites

Before installing DIH, ensure the following:

### OS Compatibility
- Linux or Windows

### Resource Recommendations

| Component     | Java Heap Size     |
|---------------|--------------------|
| Webserver     | 512MB              |
| SSO Server (optional) | 512MB      |
| Runtime Server | 1024MB per instance |

### Disk Space Requirements

- Without embedded OUAF application jars: **80MB**  
- With embedded OUAF application jars: **700MB**

### Hardware Configurations

- 8 CPUs  
- 64GB Memory  
- 20GB Disk Space (primarily for logging)

---

## Installation Files

The installation package is distributed as a zip file:

```
ria-dmi-<version>.zip
```

This contains all the required software and scripts.

---

## SQL Scripts

```sql
-- Create DIH tables, views, and DBMS Scheduler objects
sql/create-dmi-tables.sql
```

```sql
-- Create the PL/SQL package required by the tool
sql/RIA_DMI.sql
```

---

## DataLoader Utility

Included in:
```
/tools/dl
```

Use this utility to install initial configuration data.

---

## Installation Steps

### 1. Unzip the installation package.

### 2. (Linux Only) Make Shell Scripts Executable
Navigate to:
```
ria-dmi/bin
```
Make bash scripts executable.

### 3. Create Database Schema (if needed)

```sql
CREATE TABLESPACE DIHTS_01 LOGGING DATAFILE '/opt/oracle/oradata/XE/dihts01.dbf' 
SIZE 512M REUSE AUTOEXTEND ON NEXT 8192K MAXSIZE UNLIMITED 
EXTENT MANAGEMENT LOCAL UNIFORM SIZE 256K;

CREATE USER DIHADM IDENTIFIED BY DIHADM 
DEFAULT TABLESPACE DIHTS_01 
TEMPORARY TABLESPACE TEMP 
PROFILE DEFAULT;

GRANT UNLIMITED TABLESPACE TO DIHADM WITH ADMIN OPTION;
GRANT SELECT ANY TABLE TO DIHADM;
GRANT CREATE DATABASE LINK TO DIHADM;
GRANT CONNECT TO DIHADM;
GRANT RESOURCE TO DIHADM;
GRANT DBA TO DIHADM WITH ADMIN OPTION;
GRANT CREATE ANY SYNONYM TO DIHADM;
GRANT SELECT ANY DICTIONARY TO DIHADM;
ALTER USER DIHADM DEFAULT ROLE ALL;
GRANT CREATE SESSION TO DIHADM;
```

### 4. Grant SYSDBA Privileges to DIH Schema

```sql
GRANT CREATE JOB TO &1;
GRANT EXECUTE ANY CLASS TO &1;
GRANT MANAGE SCHEDULER TO &1;

BEGIN
    DBMS_RULE_ADM.grant_system_privilege(
        privilege => DBMS_RULE_ADM.create_rule_set_obj,
        grantee => '&1',
        grant_option => false);

    DBMS_RULE_ADM.grant_system_privilege(
        privilege => DBMS_RULE_ADM.create_evaluation_context_obj,
        grantee => '&1',
        grant_option => false);

    DBMS_RULE_ADM.grant_system_privilege(
        privilege => DBMS_RULE_ADM.create_rule_obj,
        grantee => '&1',
        grant_option => false);
END;
/
```

### 5. Optional: Setup Email Notifications in DBMS Scheduler

```sql
DBMS_SCHEDULER.set_scheduler_attribute('email_server', 'SMTP-server-name:port');
```

### 6. Create Database Objects

```bash
sql/create-dmi-tables.sql
```

### 7. Configure the Tool

Copy the config files:

```bash
config/ria-boot.properties -> config/override/ria-boot.properties
config/hazelcast.xml       -> config/override/hazelcast.xml
```

---

## Configuration Properties

### Environment Name

```properties
com.ria.core.env.name = <user-defined-name>
```

### Server Address and Port

```properties
com.ria.core.host.address
com.ria.core.host.port = 16080
```

### Database Connection

```properties
com.ria.core.db.url
com.ria.core.db.user
com.ria.core.db.password (optional)
```

### OUAF (Optional)

```properties
com.ria.converter.service.language = ENG
com.ria.converter.service.user = SYSUSER
```

### Enable Security

```properties
com.ria.core.authz.fileBased = false
```

### Email Configuration

```properties
com.ria.mail.debug = false
com.ria.mail.smtp.socketFactory.class = javax.net.ssl.SSLSocketFactory
com.ria.mail.smtp.host = smtp.gmail.com
com.ria.mail.smtp.port = 465
com.ria.mail.smtp.auth = true
com.ria.mail.user = user@gmail.com
com.ria.mail.password = password
```

> **Tip:** Passwords can be encrypted using `bin/encrypt`.

---

## Hazelcast Configuration

Edit: `config/override/hazelcast.xml`

```xml
<cluster-name>ria-dmi-dev</cluster-name>
<network>
    <port auto-increment="true">5701</port>
    <join>
        <multicast enabled="false">
            <multicast-group>224.2.2.3</multicast-group>
            <multicast-port>54327</multicast-port>
        </multicast>
        <tcp-ip enabled="true">
            <interface>localhost</interface>
        </tcp-ip>
    </join>
</network>
```

---

## Load Initial Configuration

Navigate to the tools directory and run:

```bash
bin/dlload -script lds/init-config.lds
```

---

## OUAF Integration (Optional)

If DIH needs to call OUAF services directly, ensure OUAF JARs are embedded in the DIH JVM.

---

## Start the Web Application Server

```bash
bin/startws
```

---

## Access the Web Application

Open your browser and navigate to:

```
http://<host>:<port>
```

> Chrome and Firefox browsers are recommended.
```