---
sidebar_position: 1
title: Connector queries
---

import Thumbnail from '@site/src/components/Thumbnail';
import VersionedLink from '@site/src/components/VersionedLink';

# Connector queries

DronaHQ enables your controls to interact with various data sources by using data queries. With DronaHQ, you can seamlessly query most databases and APIs.

Lets say you can set your Tablegrid's data to be ``{{data_query1.data}}`` where your data query is selecting data from an SQL, and the Tablegrid will then display data from data_query1. Also you can run a data query on click of a Button to trigger a specific action, such as {{update_data}}, which can initiate an API POST request.

<figure>
  <Thumbnail src="/img/data-queries/connector-query.png" alt="Connector Query" width='100%'/>
  <figcaption align = "center"><i>Connector Query</i></figcaption>
</figure>

## Prerequisites

To start using direct connector queries in your app builder, you need a connector in place already. It could be a Database or an API connector. Once the connector is in place, you can query into it with a choice of editors depending on what type of a connector is configured. 

<figure>
  <Thumbnail src="/img/data-queries/existing-connector.png" alt="Existing connector" width='100%'/>
  <figcaption align = "center"><i>Configured connectors' list</i></figcaption>
</figure>


:::caution Querying into an API

We also consider adding an API endpoint as a data query. The prerequisite being that the connector category needs to be already configured.

:::

## Sections in connector query

While you want to write a connector query which will be eventually referenced in any of the controls like a Tablegrid, you need to start off navigating to `Data queries -> New -> Connector query`. Please note that you need not have the query already added in the library, you can create these queries on the fly.
All the queries added will be available to edit or access only in that particular app and will not be available at the account level. 

While adding the connector query you will see the following sections

### Query editor

Depending on which connector you choose, the appropriate query editor will be initialized for the best experience of writing the query. In this example we have an SQL editor since the connector selected is a MySQL type of a connector. Similarly for graphQL, No SQL, REST APIS, Firebase etc. will have their own unique editors. An example of writing a query in the SQL editor is as below with a dynamic variable:
```sql
SELECT * FROM products WHERE id = {{dropdown}}
```

<figure>
  <Thumbnail src="/img/data-queries/query-editor.png" alt="Query Editor" width='100%'/>
  <figcaption align = "center"><i>Query Editor</i></figcaption>
</figure>

### Schema

While building your queries the schema of your tables (in case of SQL) comes handy. This section provides with a blueprint of the whole schema or the database down to the column type. 

:::caution Empty schema

There are times when some type of data sources don't allow fetching of schemas or the added credentials don't have the privilege of fetching schemas. In this case the section will be empty.

:::

<figure>
  <Thumbnail src="/img/data-queries/database-schema.png" alt="Database schema" width='100%'/>
  <figcaption align = "center"><i>Database schemas</i></figcaption>
</figure>

### Test values

When you have dynamic variables in your query like ``{{dropdown}}`` you might want to change the values in it and test the query to check your responses. This is the reason the test values can be modified to try out different cases with your query. In this example you can see the test value of the dropdown is set to ``Folder`` and the query is built with the same value. 

:::info Test values

This section comes in handy even while configuring the failure conditions for different scenarios and/or users and simulate. 

:::

<figure>
  <Thumbnail src="/img/data-queries/test-values.png" alt="Test values" width='100%'/>
  <figcaption align = "center"><i>Test values</i></figcaption>
</figure>

### Response

You will see the response of your query in this section when you test. You can toggle between viewing tabular data or JSON type of data depending on the format of the response data. In this case since we had an SQL SELECT query, we had an array of object type of data which we can view in tabular as well as json format. 

<figure>
  <Thumbnail src="/img/data-queries/tabular-response.png" alt="Tabular response data" width='100%'/>
  <figcaption align = "center"><i>Response in tabular format</i></figcaption>
</figure>

<figure>
  <Thumbnail src="/img/data-queries/json-response.png" alt="JSON response data" width='100%'/>
  <figcaption align = "center"><i>Response data in JSON format</i></figcaption>
</figure>

### Failure conditions

There are times when there would be non 2XX responses from an API with specific conditions (e.g. 403 forbidden response) and you might want to handle the same with custom response messages to the user. You can configure `Failure conditions` in this case. 

You have the following keywords available to build your failure conditions:
- **OUTPUT** - Output json keys
- **ERROR** - Error keys
- **STATUSCODE** - HTTP status code received from the target connector
- **STATUSMESSAGE** - HTTP status message received from the target connector

Typical example would be:
```json
{{STATUSCODE == 403}}
```
```json
{{OUTPUT[0].id == null}}
```

<figure>
  <Thumbnail src="/img/data-queries/failure-condition.png" alt="Failure condition" width='100%'/>
  <figcaption align = "center"><i>Example: Failure condition</i></figcaption>
</figure>

### Transform

This sections is exactly same as the one in the [connector library.](/binding-data/data-queries/connector-library/#transform-response)

<figure>
  <Thumbnail src="/img/data-queries/transform-section.png" alt="Transform response" width='100%'/>
  <figcaption align = "center"><i>Transform response section</i></figcaption>
</figure>


The Advanced Section of Data Query allows you to control specific behaviors such as pagination, conditional execution, error handling, and file management. Below is a comprehensive table explaining each setting and its purpose.

| Property                   | Description                                                                                                                                                                                                                                                                           |
|--------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cursor-Based Pagination    | This feature enables pagination by fetching data in chunks. Use this when your API provides data in paginated responses. You must define: - Offset Key: The key in the API response that indicates the offset or pointer to the next page. - Has More Key: A key that specifies if more data is available (TRUE/FALSE or 0/1). |
| When to Execute            | This setting allows conditional execution of the query based on a logical condition. For example, you can define a condition like `{{USERNAME == "John"}}`, which ensures that the query only runs if the condition evaluates to `TRUE`. This is useful for dynamic query control.                                              |
| Error Message              | You can specify a custom error message to display when the condition in the "When to Execute" field fails. For instance, if the condition is not met, the error message "Invalid username" can inform users about the issue, enhancing the clarity of the process.                                                             |
| Sanitize Special Characters| This option cleans special characters from response data keys to ensure compatibility with downstream processes or integrations. By toggling this ON, you can prevent issues that might arise from unsupported characters in response keys.                                                                                       |
| Allow Offline Submission   | Enabling this feature ensures the query runs even in offline mode. While offline, certain on-screen actions are skipped, and data submission becomes possible. However, some functionalities, such as file uploads and output variables from previous actions, may not work in this mode.                                       |
| Download as File           | If the API returns a file or attachment, enabling this setting ensures it is automatically downloaded. For example, when an API response includes a document or an image, the system will download it directly. Note that this applies only to file attachments, not JSON responses.                                         |
|Run on App open | This will make the dataquery to run on app open regardless of if it's referenced in any other control or dataquery.|


