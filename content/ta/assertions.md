---
title: Assertions
weight: 1
---

## Overview
Assertions serve as a quality control measure ensuring the completeness and correctness of the expected data as defined in a Step.

## View
In the upper left corner, select your project from the **Select Project** dropdown list and the version from the **Version** dropdown list. Only the assertions associated with the selected project and version are displayed on the left pane.

## Edit an Assertion
To view or modify the details of an assertion, select the assertion from the left pane that lists all assertions associated with the selected project and version.  
You can change any detail by directly editing the corresponding field.  
**Ensure to click `Update` after making your modifications.**

## Search
The search feature allows you to filter the list of assertions by providing a keyword or the full name of an assertion.  
Only the assertions that match the provided keyword or name will be displayed in the view.

## Create an Assertion
To create an assertion, click on the ➕ icon on the top left pane and enter valid values in the following fields:

### Fields

| Field Name            | Description                                                                 |
|-----------------------|-----------------------------------------------------------------------------|
| **Assertion Service Name** | Name of the assertion. It should be a minimum of 5 characters long. <br>**Mandatory:** Yes |
| **Assertion Type**         | Specifies the type of step. Possible values: <br>- API <br>- Query <br>- File <br>- File to File <br>**Mandatory:** Yes |

---

## Create an API Assertion

Enter valid values in the following fields:

| Field         | Description |
|---------------|-------------|
| **HTTP Method** | Specifies the action to be performed by the external system. Possible values: <br>- GET <br>- POST <br>- PUT <br>- PATCH <br>- DELETE <br>**Mandatory:** Yes |
| **URL**        | Specifies the API URL used for the validation. It consists of 2 parts: <br>**Environment** — displays the default environment. Click the dropdown to select a different environment. <br>**URL** — specify the API service URL. <br>**Mandatory:** Yes |
| **Params**     | Parameters for query execution and data retrieval. <br>Types: <br>- **Query Params** — key-value pairs; editable via URL or table. <br>- **Path Params** — keys in the API URL; values updated via the URL. <br>**Mandatory:** No |
| **Body**       | Payload details of the assertion. Formats: <br>- **Form Data** — key-value pairs. <br>- **JSON** — define request/response bodies. <br>**Mandatory:** No |
| **Headers**    | HTTP headers such as content type, authentication, and processing instructions. |

---

## Create a Query Assertion

| Section          | Description |
|------------------|-------------|
| **Schema**       | Database schema where the query will be executed. Auto-populated from the environment. |
| **Query**        | Query to be executed for validation. |
| **Parameters**   | Input key-value pairs from the query. |
| **Results**      | Expected result after running the query. |
| **Output Columns** | List of database columns in the expected result, with data types. |
