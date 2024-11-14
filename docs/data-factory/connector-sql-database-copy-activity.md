---
title: Configure SQL database in a copy activity
description: This article explains how to copy data using SQL database.
author: jianleishen
ms.author: jianleishen
ms.topic: how-to
ms.date: 11/14/2024
ms.custom:
  - template-how-to
  - build-2023
  - ignite-2023
---

# Configure SQL database in a copy activity

This article outlines how to use the copy activity in data pipeline to copy data from and to SQL database.

## Supported configuration

For the configuration of each tab under copy activity, go to the following sections respectively.

- [General](#general)  
- [Source](#source)
- [Destination](#destination)
- [Mapping](#mapping)
- [Settings](#settings)

### General

Refer to the [**General** settings](activity-overview.md#general-settings) guidance to configure the **General** settings tab.

### Source

The following properties are supported for SQL database under the **Source** tab of a copy activity.

:::image type="content" source="./media/connector-sql-database/sql-database-source.png" alt-text="Screenshot showing the source tab and the list of properties." lightbox="./media/connector-sql-database/sql-database-source.png":::

The following properties are **required**:

- **Connection**: Select an SQL database connection from the connection list. If no connection exists, then create a new SQL database connection by selecting **More** at the bottom of the connection list. If you apply **Use dynamic content** to specify your SQL database, add a parameter and specify the SQL database object ID as the parameter value. To get your SQL database object ID, open your SQL database in your workspace, and the ID is after `/sqldatabases/` in your URL.

    :::image type="content" source="./media/connector-sql-database/sql-database-object-id.png" alt-text="Screenshot showing the SQL database object ID.":::
 
- **Use query**: You can choose **Table**, **Query**, or **Stored procedure**. The following list describes the configuration of each setting:

  - **Table**: Specify the name of the SQL database to read data. Choose an existing table from the drop-down list or select **Enter manually** to enter the schema and table name.

  - **Query**: Specify the custom SQL query to read data. An example is `select * from MyTable`. Or select the pencil icon to edit in code editor.

    :::image type="content" source="./media/connector-sql-database/query.png" alt-text="Screenshot showing choosing query.":::

  - **Stored procedure**: Select the stored procedure from the drop-down list.

Under **Advanced**, you can specify the following fields:

- **Query timeout (minutes)**: Specify the timeout for query command execution, default is 120 minutes. If a parameter is set for this property, allowed values are timespan, such as "02:00:00" (120 minutes).

    :::image type="content" source="./media/connector-sql-database/query-timeout.png" alt-text="Screenshot showing Query timeout settings.":::

- **Isolation level**: Specifies the transaction locking behavior for the SQL source. The allowed values are: **None**, **ReadCommitted**, **ReadUncommitted**, **RepeatableRead**, **Serializable**, or **Snapshot**. If not specified, **None** isolation level is used. Refer to [IsolationLevel Enum](/dotnet/api/system.data.isolationlevel) for more details.

    :::image type="content" source="./media/connector-sql-database/isolation-level.png" alt-text="Screenshot showing Isolation level settings.":::

- **Partition option**: Specify the data partitioning options used to load data from SQL database. Allowed values are: **None** (default), **Physical partitions of table**, and **Dynamic range**. When a partition option is enabled (that is, not **None**), the degree of parallelism to concurrently load data from an SQL database is controlled by the [parallel copy](/azure/data-factory/copy-activity-performance-features#parallel-copy) setting on the copy activity.

  :::image type="content" source="./media/connector-sql-database/partition-option-1.png" alt-text="Screenshot showing Partition option settings." lightbox="./media/connector-sql-database/partition-option-1.png":::

  - **None**: Choose this setting to not use a partition.
  - **Physical partitions of table**: When you use a physical partition, the partition column and mechanism are automatically determined based on your physical table definition.
  - **Dynamic range**: When you use a query with parallel enabled, the range partition parameter(`?DfDynamicRangePartitionCondition`) is needed. Sample query: `SELECT * FROM <TableName> WHERE ?DfDynamicRangePartitionCondition`.

    - **Partition column name**: Specify the name of the source column in **integer or date/datetime** type (`int`, `smallint`, `bigint`, `date`, `smalldatetime`, `datetime`, `datetime2`, or `datetimeoffset`) that's used by range partitioning for parallel copy. If not specified, the index or the primary key of the table is autodetected and used as the partition column.
    - **Partition upper bound**: Specify the maximum value of the partition column for partition range splitting. This value is used to decide the partition stride, not for filtering the rows in table. All rows in the table or query result are partitioned and copied.
    - **Partition lower bound**: Specify the minimum value of the partition column for partition range splitting. This value is used to decide the partition stride, not for filtering the rows in table. All rows in the table or query result are partitioned and copied.

- **Additional columns**: Add more data columns to store source files' relative path or static value. Expression is supported for the latter. For more information, go to [Add additional columns during copy](/azure/data-factory/copy-activity-overview#add-additional-columns-during-copy).

### Destination

The following properties are supported for SQL database under the **Destination** tab of a copy activity.

:::image type="content" source="./media/connector-sql-database/sql-database-destination.png" alt-text="Screenshot showing Destination tab." lightbox="./media/connector-sql-database/sql-database-destination.png":::

The following properties are **required**:

- **Connection**: Select a SQL database connection from the connection list. If no connection exists, then create a new SQL database connection by selecting **More** at the bottom of the connection list. If you apply **Use dynamic content** to specify your SQL database, add a parameter and specify the SQL database object ID as the parameter value. To get your SQL database object ID, open your SQL database in your workspace, and the ID is after `/sqldatabases/` in your URL.

    :::image type="content" source="./media/connector-sql-database/sql-database-object-id.png" alt-text="Screenshot showing the SQL database object ID.":::

- **Table option**: Select from **Use existing** or **Auto create table**.

  - If you select **Use existing**:
    - **Table**: Specify the name of the SQL database to write data. Choose an existing table from the drop-down list or select **Enter manually** to enter the schema and table name.

  - If you select **Auto create table**:
    - **Table**: It automatically creates the table (if nonexistent) in source schema, which is not supported when stored procedure is used as write behavior.

Under **Advanced**, you can specify the following fields:

- **Write behavior**: Defines the write behavior when the source is files from a file-based data store. You can choose **Insert**, **Upsert** or **Stored procedure**.

    :::image type="content" source="./media/connector-sql-database/writer-behavior.png" alt-text="Screenshot of write behavior tab.":::

  - **Insert**: Choose this option if your source data has inserts.

  - **Upsert**: Choose this option if your source data has both inserts and updates.

    - **Use TempDB**: Specify whether to use a global temporary table or physical table as the interim table for upsert. By default, the service uses global temporary table as the interim table and this checkbox is selected.

      :::image type="content" source="./media/connector-sql-database/use-tempdb.png" alt-text="Screenshot showing select Use TempDB.":::

    - **Select user DB schema**: When the **Use TempDB** checkbox isn't selected, specify the interim schema for creating an interim table if a physical table is used.

      >[!Note]
      > You must have the permission for creating and deleting tables. By default, an interim table will share the same schema as a destination table.

      :::image type="content" source="./media/connector-sql-database/not-use-tempdb.png" alt-text="Screenshot showing not select Use TempDB.":::

    - **Key columns**: Specify the column names for unique row identification. Either a single key or a series of keys can be used. If not specified, the primary key is used.

  - **Stored procedure**: Use the stored procedure that defines how to apply source data into a target table. This stored procedure is *invoked per batch*.
    - **Stored procedure name**: Select the stored procedure or specify the stored procedure name manually when checking the **Edit** box to read data from the source table.
    - **Stored procedure parameters**: Specify values for stored procedure parameters. Allowed values are name or value pairs. The names and casing of parameters must match the names and casing of the stored procedure parameters.

      :::image type="content" source="./media/connector-sql-database/stored-procedure.png" alt-text="Screenshot showing stored procedure settings." lightbox="./media/connector-sql-database/stored-procedure.png":::

- **Bulk insert table lock**: Choose **Yes** or **No**. Use this setting to improve copy performance during a bulk insert operation on a table with no index from multiple clients. For more information, go to [BULK INSERT (Transact-SQL)](/sql/t-sql/statements/bulk-insert-transact-sql)

- **Table option**: Specifies whether to [automatically create the destination table](/azure/data-factory/copy-activity-overview#auto-create-sink-tables) if the table doesn't exist based on the source schema. Choose **None** or **Auto create table**. Auto table creation isn't supported when destination specifies a stored procedure.

- **Pre-copy script**: Specify a script for Copy Activity to execute before writing data into a destination table in each run. You can use this property to clean up the preloaded data.

- **Write batch timeout**: Specify the wait time for the batch insert operation to finish before it times out. The allowed value is timespan. The default value is "00:30:00" (30 minutes).

- **Write batch size**: Specify the number of rows to insert into the SQL table per batch. The allowed value is integer (number of rows). By default, the service dynamically determines the appropriate batch size based on the row size.

- **Max concurrent connections**: Specify the upper limit of concurrent connections established to the data store during the activity run. Specify a value only when you want to limit concurrent connections.

### Mapping

For the **Mapping** tab configuration, if you don't apply SQL database with auto create table as your destination, go to [Mapping](copy-data-activity.md#configure-your-mappings-under-mapping-tab).

If you apply SQL database with auto create table as your destination, except the configuration in [Mapping](copy-data-activity.md#configure-your-mappings-under-mapping-tab), you can edit the type for your destination columns. After selecting **Import schemas**, you can specify the column type in your destination.

For example, the type for *ID* column in source is int, and you can change it to float type when mapping to the destination column.

:::image type="content" source="media/connector-sql-database/configure-mapping-destination-type.png" alt-text="Screenshot of mapping destination column type.":::

### Settings

For **Settings** tab configuration, go to [Configure your other settings under settings tab](copy-data-activity.md#configure-your-other-settings-under-settings-tab).

## Table summary

The following tables contain more information about the copy activity in SQL database.

### Source

|Name |Description |Value|Required |JSON script property |
|:---|:---|:---|:---|:---|
|**Connection** |Your connection to the source data store.|\<your connection> |Yes|connection|
|**Use query** |The way to read data. Apply **Table** to read data from the specified table or apply **Query** to read data using SQL queries.|• **Table** <br>• **Query**<br>• **Stored procedure**| Yes | / |
| *For **Table*** |  |  |  |  |
| **schema name** | Name of the schema. |< your schema name >  | No | schema |
| **table name** | Name of the table. | < your table name > | No |table |
| *For **Query*** |  |  |  |  |
| **Query** | Specify the custom SQL query to read data. For example: `SELECT * FROM MyTable`. |  < SQL queries > |No | sqlReaderQuery|
| *For **Stored procedure*** |  |  |  |  |
| **Stored procedure name** | Name of the stored procedure. | < your stored procedure name > | No |sqlReaderStoredProcedureName |
|  |  |  |  |  |
|**Query timeout** |The timeout for query command execution, default is 120 minutes. |timespan |No |queryTimeout|
|**Isolation level** |Specifies the transaction locking behavior for the SQL source.|• None<br>• ReadCommitted<br>• ReadUncommitted<br>• RepeatableRead<br>• Serializable<br>• Snapshot|No |isolationLevel|
|**Partition option** |The data partitioning options used to load data from SQL database. |• None<br>• Physical partitions of table<br>• Dynamic range |No |partitionOption:<br>• PhysicalPartitionsOfTable<br>• DynamicRange|
|**Additional columns** |Add more data columns to store source files' relative path or static value. Expression is supported for the latter.|• Name<br>• Value|No |additionalColumns:<br>• name<br>• value |

### Destination

|Name |Description |Value|Required |JSON script property |
|:---|:---|:---|:---|:---|
|**Connection** |Your connection to the destination data store.|\<your connection >|Yes|connection|
|**Table option**|Your destination data table. Select from **Use existing** or **Auto create table**.| \<name of your destination table\> | Yes |schema <br> table|
|**Write behavior** |Defines the write behavior when the source is files from a file-based data store.|• Insert<br>• Upsert<br>• Stored procedure|No |writeBehavior:<br>• insert<br>• upsert<br>• sqlWriterStoredProcedureName, sqlWriterTableType, storedProcedureParameters|
|**Bulk insert table lock** |Use this setting to improve copy performance during a bulk insert operation on a table with no index from multiple clients.|Yes or No |No |sqlWriterUseTableLock:<br>true or false|
|**Pre-copy script**|A script for Copy Activity to execute before writing data into a destination table in each run. You can use this property to clean up the preloaded data.| \<pre-copy script><br>(string)|No |preCopyScript|
|**Write batch timeout**|The wait time for the batch insert operation to finish before it times out. The allowed value is timespan. The default value is "00:30:00" (30 minutes).|timespan |No |writeBatchTimeout|
|**Write batch size**|The number of rows to insert into the SQL table per batch. By default, the service dynamically determines the appropriate batch size based on the row size.|\<number of rows><br>(integer) |No |writeBatchSize|
|**Max concurrent connections**|The upper limit of concurrent connections established to the data store during the activity run. Specify a value only when you want to limit concurrent connections.| \<upper limit of concurrent connections><br>(integer)|No |maxConcurrentConnections|

## Related content

- [SQL database overview](connector-sql-database-overview.md)
