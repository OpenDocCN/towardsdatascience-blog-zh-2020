# 用 Presto 在 S3 加载和查询 CSV 文件

> 原文：<https://towardsdatascience.com/load-and-query-csv-file-in-s3-with-presto-b0d50bc773c9?source=collection_archive---------12----------------------->

## 如何在 S3 用 Presto 加载和查询 CSV 文件

在大数据领域，这是一项如此简单而常见的任务，我想人们肯定已经做过一千次了，所以当一位客户问我这个问题时，我直接上网，试图找到一些好的例子与客户分享。你猜怎么着？我找不到！所以我决定自己写一个。

使用 Presto 和 S3 的典型数据 ETL 流程如下所示:

1.  上传 CSV 文件到 S3。
2.  把 S3 的 CSV 文件加载到 Presto。
3.  (可选)转换为 Parquet 或 ORC 中的分析优化格式。
4.  对 Parquet 或 ORC 表运行复杂的查询。

在这篇博客中，我使用了[纽约市 2018 黄色出租车旅行数据集](https://data.cityofnewyork.us/Transportation/2018-Yellow-Taxi-Trip-Data/t29m-gskq)。数据集有 1.12 亿行，每行 17 列，采用 CSV 格式。总大小为 9.8GB。

以下是一些示例数据:

```
head -n 3 tlc_yellow_trips_2018.csvVendorID,tpep_pickup_datetime,tpep_dropoff_datetime,passenger_count,trip_distance,RatecodeID,store_and_fwd_flag,PULocationID,DOLocationID,payment_type,fare_amount,extra,mta_tax,tip_amount,tolls_amount,improvement_surcharge,total_amount
2,05/19/2018 11:51:48 PM,05/20/2018 12:07:31 AM,1,2.01,1,N,48,158,2,11.5,0.5,0.5,0,0,0.3,12.8
1,05/19/2018 11:22:53 PM,05/19/2018 11:35:14 PM,1,1.3,1,N,142,164,2,9,0.5,0.5,0,0,0.3,10.3
1,05/19/2018 11:37:02 PM,05/19/2018 11:52:41 PM,1,2.2,1,N,164,114,1,11,0.5,0.5,3.05,0,0.3,15.35
```

我假设你已经完成了一个基本的普雷斯托和 S3 设置。您还需要在 Presto 中设置 Hive 目录，以便它查询 S3 的数据。如果你还没有，请看看我的博客[与 Kubernetes 和 S3-部署](https://medium.com/swlh/presto-with-kubernetes-and-s3-deployment-4e262849244a)。

# 上传 CSV 文件到 S3

在 S3 创建一个目录来存储 CSV 文件。我们可以使用任何 S3 客户端来创建 S3 目录，这里我简单地使用了`hdfs`命令，因为它在 Hive Metastore 节点上是可用的，是上述博客中 Hive 目录设置的一部分。

从配置单元 Metastore 节点运行以下命令。更改存储桶名称以匹配您的环境。注意，我将`s3a://`指定为目录路径模式，以便`hdfs`命令在 S3 而不是 HDFS 上创建目录。

```
hdfs dfs -mkdir -p s3a://deephub/warehouse/nyc_text.db/tlc_yellow_trips_2018
```

将 CSV 文件上传到我们刚刚创建的目录下的 S3。任何 S3 客户端都可以工作，我使用 [s5cmd](https://github.com/peak/s5cmd) ，一个非常快的 S3 客户端，上传 CSV 文件到我的 S3 目录。这里我使用 [FlashBlade](https://www.purestorage.com/products/flashblade.html) S3，所以我将`endpoint-url`指定给我的 FlashBlade 数据 VIP。

```
s5cmd --endpoint-url=[http://192.168.170.12:80](http://192.168.170.12:80) cp tlc_yellow_trips_2018.csv s3://deephub/warehouse/nyc_text.db/tlc_yellow_trips_2018/tlc_yellow_trips_2018.csv
```

# 将 CSV 文件加载到 Presto

为了在 S3 查询数据，我需要在 Presto 中创建一个表，并将其模式和位置映射到 CSV 文件。

启动 Presto CLI:

```
presto-cli --server <coordinate_node:port> --catalog hive
```

使用 Presto CLI 为文本数据创建新的模式。

```
presto> CREATE SCHEMA nyc_text WITH (LOCATION = 's3a://deephub/warehouse/nyc_text.db');
```

为 CSV 数据创建外部表。您可以在一个模式下创建多个表。注意事项:

*   CSV 格式的表格目前只支持`VARCHAR`数据类型。
*   我将`skip_header_line_count = 1`设置为 table 属性，以便跳过 CSV 文件中的第一行标题。如果您的 CSV 文件不包含标题，请删除此属性。
*   Presto 的 CSV 格式支持需要`metastore-site.xml` Hive Metastore 配置文件中的`metastore.storage.schema.reader.impl=org.apache.hadoop.hive.metastore.SerDeStorageSchemaReader`。

```
presto> CREATE TABLE hive.nyc_text.tlc_yellow_trips_2018 (
    vendorid VARCHAR,
    tpep_pickup_datetime VARCHAR,
    tpep_dropoff_datetime VARCHAR,
    passenger_count VARCHAR,
    trip_distance VARCHAR,
    ratecodeid VARCHAR,
    store_and_fwd_flag VARCHAR,
    pulocationid VARCHAR,
    dolocationid VARCHAR,
    payment_type VARCHAR,
    fare_amount VARCHAR,
    extra VARCHAR,
    mta_tax VARCHAR,
    tip_amount VARCHAR,
    tolls_amount VARCHAR,
    improvement_surcharge VARCHAR,
    total_amount VARCHAR)
WITH (FORMAT = 'CSV',
    skip_header_line_count = 1,
    EXTERNAL_LOCATION = 's3a://deephub/warehouse/nyc_text.db/tlc_yellow_trips_2018')
;
```

现在我可以查询 CSV 数据了。

```
presto> SELECT * FROM nyc_text.tlc_yellow_trips_2018 LIMIT 10;
```

此时，我可以将 Tableau 连接到[并可视化 Presto 表中的](https://help.tableau.com/current/pro/desktop/en-us/examples_presto.htm)数据。但由于 CSV 格式表只支持`VARCHAR`数据类型，可能会暴露对 Tableau 的限制。如果是这种情况，请将 CSV 转换为拼花或 ORC 格式(见下文)。拼花或 ORC 表通常比文本/CSV 表具有更好的性能。

# (可选)将 CSV 转换为拼花格式

这是一个**可选的**任务，但是如果数据将被多次查询，则建议使用**任务**。通过在 Parquet 或 ORC 中将文本数据转换为分析优化格式，它不仅提高了查询性能，还减少了服务器和存储资源消耗。

Presto 适用于可以在 SQL 中完成的简单转换。对于那些使用 SQL 无法轻松完成的复杂业务逻辑(例如，需要 Java/Python 编程)，最好使用 Apache Spark。在这个例子中，我只是将文本转换成 Parquet 格式，而没有引入任何复杂的业务逻辑，所以我将使用 Presto 进行转换。

在 Presto 中管理数据的一个常见做法是对原始文本(CSV/TSV)表和优化的(Parquet/ORC)表使用不同的模式。所以我将为镶木地板桌子创建一个新的模式`nyc_parq`。

为新模式创建一个 S3 目录。更改存储桶名称以匹配您的环境。

```
hdfs dfs -mkdir -p s3a://deephub/warehouse/nyc_parq.db
```

在 Presto CLI 中创建`nyc_parq`模式。

```
presto> CREATE SCHEMA nyc_parq WITH (LOCATION = 's3a://deephub/warehouse/nyc_parq.db');
```

创建镶木地板表，将 CSV 数据转换为镶木地板格式。您可以更改`SELECT`原因来添加简单的业务和转换逻辑。

```
presto> CREATE TABLE hive.nyc_parq.tlc_yellow_trips_2018
COMMENT '2018 Newyork City taxi data'
WITH (FORMAT = 'PARQUET')
AS
SELECT 
    cast(vendorid as INTEGER) as vendorid,
    date_parse(tpep_pickup_datetime, '%m/%d/%Y %h:%i:%s %p') as tpep_pickup_datetime,
    date_parse(tpep_dropoff_datetime, '%m/%d/%Y %h:%i:%s %p') as tpep_dropoff_datetime,
    cast(passenger_count as SMALLINT) as passenger_count,
    cast(trip_distance as DECIMAL(8, 2)) as trip_distance,
    cast(ratecodeid as INTEGER) as ratecodeid,
    cast(store_and_fwd_flag as CHAR(1)) as store_and_fwd_flag,
    cast(pulocationid as INTEGER) as pulocationid,
    cast(dolocationid as INTEGER) as dolocationid,
    cast(payment_type as SMALLINT) as payment_type,
    cast(fare_amount as DECIMAL(8, 2)) as fare_amount,
    cast(extra as DECIMAL(8, 2)) as extra,
    cast(mta_tax as DECIMAL(8, 2)) as mta_tax,
    cast(tip_amount as DECIMAL(8, 2)) as tip_amount,
    cast(tolls_amount as DECIMAL(8, 2)) as tolls_amount,
    cast(improvement_surcharge as DECIMAL(8, 2)) as improvement_surcharge,
    cast(total_amount as DECIMAL(8, 2)) as total_amount
FROM hive.nyc_text.tlc_yellow_trips_2018
;
```

根据数据大小，此转换可能需要一些时间。一旦完成，我就可以查询拼花地板数据。

```
presto> SELECT * FROM nyc_parq.tlc_yellow_trips_2018 LIMIT 10;
```

确认拼花桌的模式。注意此表中的列具有所需的类型。

```
presto> describe nyc_parq.tlc_yellow_trips_2018;
        Column         |     Type     | Extra | Comment
-----------------------+--------------+-------+---------
 vendorid              | integer      |       |
 tpep_pickup_datetime  | timestamp    |       |
 tpep_dropoff_datetime | timestamp    |       |
 passenger_count       | smallint     |       |
 trip_distance         | decimal(8,2) |       |
 ratecodeid            | integer      |       |
 store_and_fwd_flag    | char(1)      |       |
 pulocationid          | integer      |       |
 dolocationid          | integer      |       |
 payment_type          | smallint     |       |
 fare_amount           | decimal(8,2) |       |
 extra                 | decimal(8,2) |       |
 mta_tax               | decimal(8,2) |       |
 tip_amount            | decimal(8,2) |       |
 tolls_amount          | decimal(8,2) |       |
 improvement_surcharge | decimal(8,2) |       |
 total_amount          | decimal(8,2) |       |
(17 rows)
```

最后，我配置我的分析应用程序/ Tableau 来使用优化的`nyc_parq`模式和拼花表。

# 高级主题

随着数据变得越来越大(例如超过 TB)，以对查询性能最佳的方式组织数据变得越来越重要。使用拼花地板或 ORC 共振峰是一种优化，还有其他优化，例如:

*   谓词下推。
*   分区表。
*   拼花地板和 ORC 中的数据排序。
*   加入战略和基于成本的优化。

这些主题不是 Presto 特定的，它们适用于大多数存储和查询引擎，包括 S3 和 Presto。这些话题的细节超出了本博客的范围。请继续关注我的博客。