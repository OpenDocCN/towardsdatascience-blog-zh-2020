# 如何将数百万条记录从 Mysql 导出到 AWS S3？

> 原文：<https://towardsdatascience.com/how-to-export-millions-of-records-from-mysql-to-aws-s3-fe30e80832e4?source=collection_archive---------14----------------------->

## 构建自我恢复和可扩展的系统

![](img/5b60be114da7a0978d6d9208c4ce125b.png)

Apache Spark 生态系统信用:[数据块](https://databricks.com/spark/about)

在 [Twilio](http://www.twilio.com) ，我们每天处理世界各地发生的数百万个电话。一旦通话结束，就会被记录到 MySQL 数据库中。客户能够通过 API 查询 [**调用**](https://www.twilio.com/docs/voice/api/call-resource) 的详细信息。(是的，Twilio 是 API 驱动的公司)

我最近做的一个任务是建立一个系统，允许客户导出他们的历史通话数据。这将允许他们导出直到最近的所有历史通话记录

乍一看，这似乎是微不足道的，但是如果我们更深入地思考，一个系统将如何为从一开始就与我们合作的一些最大的客户进行扩展，这个问题可以归类为*构建一个可扩展的系统*。我们典型的客户规模从每天打几百个电话到几百万个不等。

当一个每天打 100 万个电话的大客户请求过去 5 年的数据时，这个问题突然变成了一个大数据问题。呼叫总数可以在 1，000，000 * 5 * 365 的范围内。

*我不得不考虑如何优化从 Mysql 读取数据，并高效地向 S3 发送数据，以便下载文件。*

# 可能的解决方案

1.*编写一个 cron 作业*，查询 Mysql 数据库中的特定帐户，然后将数据写入 S3。这对于获取较小的记录集可能很有效，但是为了使作业能够很好地存储大量记录，我需要构建一种机制来在失败时重试，并行读取和写入以实现高效下载，添加监控来衡量作业的成功。我将不得不编写连接器或使用库来连接 MySql 和 S3。

2.*使用* [*火花流*](https://spark.apache.org/docs/latest/streaming-programming-guide.html)

我决定使用 **Apache Spark** 来处理这个问题，因为在我的团队中( [**Voice Insights**](http://www.twilio.com/voice/insights) )我们已经大量使用它来进行实时数据处理和构建分析。 *Apache Spark* 是一个用于构建可扩展实时处理应用的流行框架，在行业中广泛用于解决大数据和机器学习问题。Spark 的一个关键特性是它能够从 Kafka、Kinesis、S3、Mysql、文件等各种来源产生/消费数据。

*Apache Spark 也是容错的，并提供了一个处理失败和优雅地重试的框架*。它*使用检查点机制来存储所执行任务的中间偏移量，以便在任务失败的情况下，任务可以从最后保存的位置重新开始。为水平扩展配置作业效果很好。*

假设读者对 Spark 有基本的了解(Spark 官方 [**文档**](https://spark.apache.org/docs/2.4.0/) 是一个很好的起点)。我将深入研究代码。

让我们看看如何从 MySQL 数据库中读取数据。

```
**val** jdbcDF **=** spark.read
  .format("jdbc")
  .option("url", "jdbc:mysql://localhost:port/db")
  .option("driver", "com.mysql.jdbc.Driver")
  .option("dbtable", "schema.tablename")
  .option("user", "username")
  .option("password", "password")
  .load()
```

与 JDBC 连接的另一种方法是将配置作为地图。

```
**val** dbConfig = Map("username" -> "admin", 
                   "password" -> "pwd", 
                   "url" -> "http://localhost:3306")
**val** query = "select * FROM CallLog where CustomerId=1"**val** jdbcDF **=** spark.read
                  .format("jdbc")      
                  .options(dbConfig)      
                  .option("**dbtable**", s"${query} AS tmp")      
                  .load()
**OR****val** jdbcDF **=** spark.read
                  .format("jdbc")      
                  .options(dbConfig)      
                  .option("**query**", s"${query} AS tmp")      
                  .load()
```

使用**查询**和 **dbtable 有细微的区别。**这两个选项都会在 FROM 子句中创建一个子查询。上述查询将被转换为

```
SELECT * FROM (SELECT * FROM CallLog where CustomerId=1) tmp WHERE 1=0
```

这里最重要的一点是`query`不支持`partitionColmn`，而`dbTable`支持分区，这通过并行实现了更好的吞吐量。

比方说，如果我们必须为我们的一个大客户导出**通话记录**，我们将需要利用分而治之的方法，并且需要一种更好的方法来并行化这项工作。

# 按列值对 SQL 查询进行分区

这意味着 Spark 可以同时对同一个表执行多个查询，但是每个查询通过为一个列(分区列)设置不同的范围值来运行。这可以在 Spark 中通过多设置几个参数来实现。让我们再看几个参数:

`numPartitions`选项定义了表中可用于并行读取的最大分区数。这也决定了并发 JDBC 连接的最大数量。

`partitionColumn`必须是相关表中的数值、日期或时间戳列。这些参数描述了在多个工作线程并行读取时如何对表进行分区。

`lowerBound`和`upperBound`只是用来决定分区步距，而不是用来过滤表中的行。因此表中的所有行将被分区并返回。此选项仅适用于阅读。

`fetchSize`JDBC 提取大小，决定每次往返要提取多少行。这有助于提高默认为低读取大小的 JDBC 驱动程序的性能(例如，Oracle 有 10 行)。此选项仅适用于阅读。

上例中的`partitionColumn`可以是`CallId`。让我们试着把所有的参数联系起来。

```
**val** dbConfig = Map("username" -> "admin", 
                   "password" -> "pwd", 
                   "url" -> "http://localhost:3306",
                   "numPartitions" -> 10,
                   "paritionColumn" -> "CallId",
                   "lowerBound" -> 0, 
                   "upperBound" -> 10,000,000)
**val** query = "select * from CallLog where CustomerId=1"**val** jdbcDF **=** spark.read
  .format("jdbc")      
  .options(dbConfig)      
  .option("dbtable", s"(${query}) AS tmp")      
  .load()
```

上述配置将导致运行以下并行查询:

```
SELECT * from CallLog where CustomerId =1 AND CallId >=0 AND CallId <1,000,000
SELECT * from CallLog where CustomerId =1 AND CallId >= 1,000,000 AND CallId <2,000,000
SELECT * from CallLog where CustomerId =1 AND CallId>= 2,000,000 AND CallId <3,000,000
....
SELECT * from CallLog where CustomerId =1 AND CallId>= 10,000,000
```

上述查询并行化将有助于更快地从表中读取结果。

一旦 Spark 能够从 Mysql 读取数据，将数据转储到 S3 就变得轻而易举了。

```
jdbcDF.write
      .format("json")      
      .mode("append")
      .save("${s3path}")
```

***结论:***

上面的方法让我们有机会使用 **Spark** 来解决一个经典的批处理作业问题。我们正在用 Apache Spark 做更多的事情，这是众多用例中的一个。我很想听听你用火花在做什么。