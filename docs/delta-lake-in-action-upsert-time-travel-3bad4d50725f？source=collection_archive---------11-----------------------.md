# 三角洲湖在行动:Upsert &时间旅行

> 原文：<https://towardsdatascience.com/delta-lake-in-action-upsert-time-travel-3bad4d50725f?source=collection_archive---------11----------------------->

## 在 Apache spark 中使用 Delta lake 的初学者指南

![](img/afccadae5c1abdfdc43a902b4025ac48.png)

由[卢卡斯·布拉塞克](https://unsplash.com/@goumbik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

这是我介绍 Delta Lake with Apache Spark [文章](/delta-lake-with-spark-what-and-why-6d08bef7b963)的后续文章，请继续阅读，了解如何使用 Delta Lake with Apache Spark 来执行操作，如更新现有数据、检查以前版本的数据、将数据转换为 Delta 表等。

在深入研究代码之前，让我们试着理解**什么时候**将 Delta Lake 与 Spark 一起使用，因为这并不像我某天醒来就将 Delta Lake 包含在架构中:P

*可使用三角洲湖泊:*

*   当处理同一个数据集的*【覆盖】*时，这是我处理过的最头疼的问题，Delta Lake 在这种情况下真的很有帮助。
*   当处理有更新的数据时，Delta Lake 的*“merge”*功能有助于处理数据中的更新(再见，混乱的连接/过滤操作！！)

当然，有许多使用 Delta Lake 的用例，但这是两个场景，它们真正帮助了我。

说够了，让我们深入代码！

# 如何用 Apache Spark 运行 Delta Lake

首先，要开始使用 Delta Lake，需要将其添加为 Spark 应用程序的一个依赖项，可以这样做:

*   对于`pyspark`外壳

```
pyspark --packages io.delta:delta-core_2.11:0.6.1 --conf "spark.sql.extensions=io.delta.sql.DeltaSparkSessionExtension" --conf "spark.sql.catalog.spark_catalog=org.apache.spark.sql.delta.catalog.DeltaCatalog"
```

*   作为 maven 的依赖项，delta lake 可以包含在`pom.xml`中，如下所示

```
<dependency>
  <groupId>io.delta</groupId>
  <artifactId>delta-core_2.11</artifactId>
  <version>0.6.1</version>
</dependency>
```

*注意事项:*

*   在这里，`2.11`是`scala`版本，如果使用 scala `2.12`相应地修改版本。
*   `0.6.1`是 Delta Lake 版本，是支持`Spark 2.4.4`的版本。
*   截止到`20200905`，delta lake 的最新版本是`0.7.0`，支持`Spark 3.0`
*   *AWS EMR 特定:*不要将 delta lake 与`EMR 5.29.0`一起使用，它有已知问题。建议升级或降级 EMR 版本，以便与 Delta Lake 配合使用。

# 如何将数据写成 Delta 表？

Delta lake 使用存储为 Delta 表的数据，因此数据需要以 Delta 表的形式写入。

*   要将现有数据转换为增量表，需要一次完成。

```
df = spark.read.parquet("path/of/some/data") # Read existing datadf.coalesce(5).write.format("delta").save("path/of/some/deltaTable")
```

*   将新数据写入增量表

```
df.coalesce(5).write.format("delta").save("path/of/some/deltaTable")
```

*注意事项:*

*   当数据保存为增量表时，该路径包含跟踪增量表元数据的`_delta_log`目录。
*   编写使用某些列进行分区的 DeltaTable，如下所示:

```
df.coalesce(5).write.partitionBy("country").format("delta").save("path/of/some/deltaTable")
```

# 如何读取增量表？

*   作为火花数据帧:

```
df = spark.read.format("delta").load("path/of/some/deltaTable")As DeltaTable:
```

将 DeltaTable 作为 Spark 数据帧读取允许对 DeltaTable 进行所有数据帧操作。

*   作为增量表:

```
deltaTable = DeltaTable.*forPath*("path/of/some/deltaTable")
```

将数据读取为 DeltaTable 仅支持 DeltaTable 特定函数。此外，DeltaTable 可以转换为 DataFrame，如下所示:

```
df = deltaTable.toDF
```

# 阿帕奇斯帕克的三角洲湖

DeltaTable 上的 UPSERT 操作允许数据更新，这意味着如果 DeltaTable 可以与一些新的数据集合并，并且在一些`join key`的基础上，可以在 delta 表中插入修改后的数据。这是在 DeltaTable 上进行向上插入的方法:

```
val todayData = # new data that needs to be written to deltaTableval deltaTable = DeltaTable.*forPath*("path/of/some/deltaTable")deltaTable.alias("oldData")
  .merge(
    todayData.alias("newData"),
    "oldData.city_id = newData.city_id")
  .whenMatched()
  .updateAll()
  .whenNotMatched()
  .insertAll()
  .execute()
```

似乎很简单，对吗？就是这样！所以接下来会发生的是，如果城市已经存在于`deltaTable`中，那么`todayData`中带有城市更新信息的行将在`deltaTable`中被修改，如果城市不存在，那么行将被插入到`deltaTable`中，这是我们的基本更新！

*注意事项:*

*   可以使用分区来修改 Upsert 操作，例如，如果需要合并特定的分区，可以指定它来优化合并操作，如:

```
deltaTable.alias("oldData")
  .merge(
    todayData.alias("newData"),
    "(country = 'India') AND oldData.city_id = newData.city_id")
  .whenMatched()
  .updateAll()
  .whenNotMatched()
  .insertAll()
  .execute()
```

*   当`deltaTable`中的行与连接键匹配或不匹配时，要执行的操作是可配置的，详情请参考文档。
*   默认情况下，合并操作会写很多小文件，为了控制小文件的数量，相应地设置下面的`spark conf`属性，详细信息请参考文档。

```
spark.delta.merge.repartitionBeforeWrite true
spark.sql.shuffle.partitions 10
```

*   在成功的合并操作之后，将在`_delta_log`目录中创建一个日志提交，浏览其内容以了解 Delta Lake 的工作方式可能会很有趣。
*   合并只支持一对一的映射，即只有一行应该尝试更新`DeltaTable`中的一行，如果多行尝试更新`DeltaTable`中的同一行，则合并操作失败。预处理数据来解决这个问题。

# 阿帕奇星火中的三角洲湖时光旅行

Delta Lake 支持返回到先前数据版本的功能，同样可以通过指定要读取的数据版本来实现:

```
df= spark.read.format("delta").option("versionAsOf", 0).load("path/of/some/deltaTable")
```

版本化从`0`开始，在`delta_log` 目录中有多少日志提交，就会有多少数据版本。

似乎，简单吧？

供参考:

 [## 欢迎来到三角洲湖文档

### 学习如何使用三角洲湖。

文档增量 io](https://docs.delta.io/latest/index.html) 

*下次见，
Ciao。*