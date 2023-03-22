# 终极 PySpark 备忘单

> 原文：<https://towardsdatascience.com/ultimate-pyspark-cheat-sheet-7d3938d13421?source=collection_archive---------5----------------------->

![](img/c753bbee3b813343b0aafd9508f86da1.png)

照片由[Genessa pana intet](https://unsplash.com/@genessapana?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/spark?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

## 数据科学

## PySpark 数据帧 API 的简短指南

S park 是当今数据工程、数据科学领域的主要参与者之一。随着处理更多数据的需求不断增加，企业经常将 Spark 集成到数据堆栈中，以解决快速处理大量数据的问题。由 Apache 维护，Spark 生态系统中的主要商业参与者是 Databricks(由 Spark 的最初创建者所有)。Spark 已经被各种公司和机构广泛接受——本地的和云中的。一些最受欢迎的底层使用 Spark 的云产品有 [AWS Glue](https://aws.amazon.com/glue/) 、 [Google Dataproc](https://codelabs.developers.google.com/codelabs/cloud-dataproc-starter/#0) 、 [Azure Databricks](https://azure.microsoft.com/en-us/services/databricks/) 。

没有一种技术，没有一种编程语言对所有用例都足够好。Spark 是用于解决大规模数据分析和 ETL 问题的众多技术之一。[在 Spark 上工作了一段时间后](https://linktr.ee/kovid)，我想到用真实的例子来编写一份备忘单。尽管有很多关于在 Scala 中使用 Spark 的参考资料，但除了 Datacamp 上的[之外，我找不到一个像样的备忘单，但我认为它需要更新，需要比一页纸更广泛一点。](https://www.datacamp.com/community/data-science-cheatsheets?page=2)

首先，一个关于 Spark 如何工作的体面介绍—

[](https://mapr.com/blog/how-spark-runs-your-applications/) [## 这就是 Spark 运行应用程序的方式

### 回想一下之前的 Spark 101 博客，您的 Spark 应用程序是作为一组并行任务运行的。在这篇博文中…

mapr.com](https://mapr.com/blog/how-spark-runs-your-applications/) 

# 配置和初始化

在开始编写哪几行代码来启动和运行 PySpark 笔记本/应用程序之前，您应该了解一点关于`SparkContext`、`SparkSession`和`SQLContext`的知识。

*   `SparkContext` —提供与 Spark 的连接，能够创建 rdd
*   `SQLContext` —提供与 Spark 的连接，能够对数据运行 SQL 查询
*   `SparkSession` —包罗万象的上下文，包括对`SparkContext`、`SQLContext`和`HiveContext`的覆盖。

我们将在一些例子中使用 MovieLens 数据库。这是那个数据库的链接。你可以从 Kaggle 下载。

[](https://www.kaggle.com/rounakbanik/the-movies-dataset) [## 电影数据集

### 超过 45，000 部电影的元数据。超过 270，000 名用户的 2，600 万次评分。

www.kaggle.com](https://www.kaggle.com/rounakbanik/the-movies-dataset) 

# 读取数据

Spark 支持从各种数据源读取数据，比如 CSV、Text、Parquet、Avro、JSON。它还支持从 Hive 和任何具有可用 JDBC 通道的数据库中读取数据。以下是如何在 Spark 中阅读 CSV 的方法—

在您的 Spark 之旅中，您会发现有许多方法可以编写相同的代码来获得相同的结果。许多函数都有别名(例如`dropDuplicates`和`drop_duplicates`)。下面的例子展示了在 Spark 中读取文件的几种方法。

# 写入数据

一旦你完成了数据转换，你会想把它写在某种持久存储上。这里有一个例子，展示了将一个拼花文件写入磁盘的两种不同方式—

显然，基于您的消费模式和需求，您也可以使用类似的命令将其他文件格式写入磁盘。写入 Hive 表时，可以用`bucketBy`代替`partitionBy`。

[](https://luminousmen.com/post/the-5-minute-guide-to-using-bucketing-in-pyspark) [## Pyspark 中使用分桶的 5 分钟指南

### 世界上有许多不同的工具，每一种都可以解决一系列问题。他们中的许多人是如何判断…

luminousmen.com](https://luminousmen.com/post/the-5-minute-guide-to-using-bucketing-in-pyspark) 

`bucketBy`和`partitionBy`背后的思想都是拒绝不需要查询的数据，即修剪分区。这是一个来自传统关系数据库分区的老概念。

# 创建数据框架

除了您在上面的**读取数据**部分看到的直接方法`df = spark.read.csv(csv_file_path)`之外，还有一种创建数据帧的方法，那就是使用 SparkSQL 的行构造。

还有一个选项，您可以使用 Spark 的`.paralellize`或`.textFile`特性将文件表示为 RDD。要将其转换成数据帧，显然需要指定一个模式。这就是`pyspark.sql.types`出现的原因。

我们将在 PySpark 中使用大量类似 SQL 的功能，请花几分钟时间熟悉下面的[文档](https://spark.apache.org/docs/latest/sql-programming-guide.html)。

# 修改数据帧

数据帧抽象出 rdd。数据集做同样的事情，但是数据集没有表格、关系数据库表那样的 rdd 表示。数据帧有。因此，DataFrames 支持类似于您通常在数据库表上执行的操作，即通过添加、删除、修改列来更改表结构。Spark 提供了 DataFrames API 中的所有功能。事情是这样的—

除了创建新列之外，我们还可以使用以下方法重命名现有的列

如果我们必须删除一列或多列，我们可以这样做—

# 连接

Spark 使用类似 SQL 的接口背后的整个想法是，有许多数据可以用一个松散的关系模型来表示，即一个没有 ACID、完整性检查等的表模型。考虑到这一点，我们可以预期会发生很多连接。Spark 完全支持连接两个或多个数据集。这里是如何—

# 过滤

过滤器就像 SQL 中的子句一样。事实上，你可以在 Spark 中互换使用`filter`和`where`。下面是一个在 MovieLens 数据库电影元数据文件中过滤分级在 7.5 和 8.2 之间的电影的示例。

过滤器支持所有类似 SQL 的特性，例如使用比较运算符、正则表达式和按位运算符进行过滤。

过滤掉空值和非空值是查询中最常见的用例之一。Spark 对列对象提供了简单的`isNULL`和`isNotNull`操作。

# 聚集

聚合是处理大规模数据的巨大工作的核心，因为这通常归结为 BI 仪表板和 ML，这两者都需要某种聚合。使用 SparkSQL 库，您几乎可以实现在传统关系数据库或数据仓库查询引擎中所能实现的一切。这里有一个例子展示了 Spark 是如何进行聚合的。

# 窗口功能和排序

与大多数分析引擎一样，窗口函数已经成为`rank`、`dense_rank`等的标准。，被大量使用。Spark 利用传统的基于 SQL 的窗口函数语法`rank() over (partition by something order by something_else desc)`。

请注意`sort`和`orderBy`在 Spark 中可以互换使用，除非是在窗口函数中。

这些是我收集的一些例子。很明显，除了一张小抄，还有很多东西值得一试。如果您感兴趣或者在这里没有找到任何有用的东西，可以去看看文档——它相当不错。