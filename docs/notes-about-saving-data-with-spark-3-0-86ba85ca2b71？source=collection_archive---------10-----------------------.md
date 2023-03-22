# 关于使用 Spark 3.0 保存数据的注意事项

> 原文：<https://towardsdatascience.com/notes-about-saving-data-with-spark-3-0-86ba85ca2b71?source=collection_archive---------10----------------------->

## 使用 Spark SQL 以文件格式存储数据的不同选项。

Apache Spark 是一个计算引擎，经常在大数据环境中用于数据处理，但它不提供存储，因此在典型情况下，数据处理的输出必须存储在外部存储系统中。Spark SQL 为文件格式(CSV、JSON、text、Parquet、ORC)或 JDBC 等数据源提供了一些连接器。从 2.4 开始也支持 Apache Avro，从 3.0 开始也支持二进制文件。此外，还有几个库允许连接到其他数据源，例如 MongoDB、ElasticSearch 或 DynamoDB 等等。

在本文中，我们将讨论将数据保存为 Apache Parquet 等文件格式时的一些可能性，因为 Spark 和 Parquet 的结合提供了非常好的体验，尤其是对于分析查询。我们将首先解释为什么 Parquet 是 Hadoop 生态系统中如此受欢迎的数据存储格式，然后我们将描述如何使用 Spark 保存数据的不同方法，同时特别关注官方文档中缺少或难以找到的功能。我们要研究的一个重要特性是如何以预先排序的状态保存数据，以便可以利用适当的过滤器跳过数据。

所提供的代码片段使用 Python API，并对照 Spark 3.0 和 2.4 进行了检查。

## 阿帕奇拼花地板

Apache Parquet 是一种开源文件格式，最初由 Twitter 和 Cloudera 开发。它是一种自描述格式，这意味着它包含带有模式信息的元数据(以及其他信息)。它使用混合模型在磁盘上存储数据，所谓混合，我们的意思是它不是严格面向行的(如 JSON 或 CSV ),也不是严格的列，而是首先将数据垂直划分为所谓的行组，然后将每个行组存储在列表示中。这些行组允许将数据集分成多个文件(每个文件可以有一个或多个行组)，这样我们就不会以一个巨大的文件结束，这在大数据环境中尤其重要。

每个行组的列表示也是一个非常重要的特征，因为它允许所谓的列修剪。在分析查询中，我们通常对数据集的所有列都不感兴趣，而是选择几列并对其进行聚合。这种列删减让我们可以跳过所有其他列，只扫描那些在查询中选择的列，因此与必须扫描所有列的面向行的格式相比，它使读取更加高效。列格式的另一个优点是压缩——列中的每个值都具有相同的数据类型，并且这些值甚至可能重复，这可以用于使用各种编码和压缩技术(Parquet 支持的技术)更有效地存储数据。

Parquet 文件支持不同级别的数据跳过，即分区级别和行组级别。因此，数据集可以通过某个(通常是低基数的)键进行分区，这意味着对于该字段的每个不同值，根目录中都会有一个子文件夹，其中包含具有该特定键值的所有记录(每个分区仍可以被划分为多个文件)。分区的用例是在读取时减少数据量，因为当在过滤器中使用分区键时，Spark 将应用分区修剪，它将跳过所有与查询无关的分区(这也支持其他文件格式，如 JSON 或 CSV)。

行-组级别的数据跳过是基于 parquet 元数据的，因为每个 parquet 文件都有一个页脚，该页脚包含关于每个行-组的元数据，并且该元数据包含统计信息，例如行-组中每个列的*最小值*和*最大值*值。当读取 parquet 文件时，Spark 将首先读取页脚，并使用这些统计信息来检查给定的行组是否可能包含与查询相关的数据。这将非常有用，特别是如果 parquet 文件是按照我们用于过滤的列排序的话。因为，如果文件没有被排序，那么大小值可能分散在所有行组中，因此每个行组都必须被扫描，因为它可能包含一些过滤器满足的行。这里的排序是至关重要的，正如我们将在后面看到的，以排序后的状态保存数据是很重要的。

## 1.[救人](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html#pyspark.sql.DataFrameWriter.save)()

将 Spark 中的计算输出保存为文件格式的选项之一是使用 *save* 方法

```
( 
  df.write
  .mode('overwrite') # or append
  .partitionBy(col_name) # this is optional
  .format('parquet') # this is optional, parquet is default
  .option('path', output_path)
  .save()
)
```

如您所见，如果您希望在保存数据的文件系统中对数据进行分区，它允许您指定分区列。默认格式是 parquet，所以如果您不指定它，它将被假定。

## 2.[保存表](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html#pyspark.sql.DataFrameWriter.saveAsTable)()

如果您使用*保存表*方法保存数据，将使用数据的数据分析师可能会更加感激，因为这将允许他/她使用

```
df = spark.table(table_name)
```

*保存表*功能也允许使用分桶，其中每个桶也可以(可选地)排序:

```
( 
  df.write
  .mode('overwrite') # or append
  .partitionBy(col_name) # this is optional
  .bucketBy(n, col_name) # n is number of buckets
  .sortBy(col_name)
  .format('parquet') # this is optional, parquet is default
  .option('path', output_path)
  .saveAsTable(table_name)
)
```

我们不会在这里深入探讨分桶，但一般来说，这是一种关于如何预混洗数据并将其保存在这种状态下的技术，以便后续查询可以利用它来避免混洗。如果使用 Spark 正确设置了 metastore，这将会起作用，因为关于分桶的信息将保存在这里。这种方法还有其他好处，因为当您稍后调用 *ANALYZE TABLE* 命令时，metastore 还可以保存关于数据集的其他信息，比如统计数据。 *sortBy* 只能在 *bucketsBy* 之后使用，因为要排序的是创建的 bucket。即使表还不存在，两种模式*覆盖*和*追加*在这里都有效，因为函数会简单地创建它。

## 3.[插入](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html#pyspark.sql.DataFrameWriter.insertInto)()

如何保存数据的另一种可能性是使用 *insertInto* 功能。与前一种情况不同，该表必须首先存在，因为如果不存在，您将得到一个错误:*analysis exception:Table not found*。使用这个函数的语法相当简单

```
(
  df.write
  .insertInto(table_name)
)
```

但是有一些需要注意的警告:

*   您不必指定输出路径。Spark 将在 metastore 中找到该信息。如果您仍然指定路径，它将被忽略。
*   默认模式是 *append* ，所以它会简单地将您的数据添加到现有的表中。
*   数据框架的模式必须与表的模式相匹配。如果数据帧中的列顺序不同于表中的顺序，Spark 将在数据类型不同且无法安全转换时抛出异常。但是如果数据类型没有不匹配，Spark 会以这种错误的顺序将数据插入到表中。因此这是非常危险的，最好确保模式是相同的，例如:

```
(
  df.select(spark.table(table_name).columns)
  .write
  .insertInto(table_name)
)
```

*   如果您的表是分区的或分桶的，您不需要指定这一点(您会得到一个错误)，因为 Spark 将从 metastore 中选取该信息。

函数 *insertInto* 比 *saveAsTable* 有一个很大的优势，因为它允许所谓的*动态覆盖*。此功能允许您覆盖分区表中的特定分区。例如，如果您的表按 *year* 进行分区，并且您只想更新一个 *year，*，那么使用 *saveAsTable* 您将不得不覆盖整个表，但是使用 *insertInto，*您可以只覆盖这一个分区，因此这将是一个非常便宜的操作，尤其是在有许多大分区的情况下。要使用此功能，您必须设置相应的配置设置:

```
spark.conf.set("spark.sql.sources.partitionOverwriteMode", "dynamic")(
  df # having data only for specific partitions
  .write
  .insertInto(table_name, overwrite=True)
)
```

在这里，还是要小心，因为如果将 *partitionOverwriteMode* 设置为 *static* (这是默认值)，它将覆盖整个表，因此所有其他分区都将变空。*动态*值确保 Spark 只覆盖数据帧中有数据的分区。

## 以排序状态保存数据

正如我们上面提到的，有时希望根据某一列以排序状态保存数据，以便在使用该列作为过滤器的分析查询中跳过数据。在 Spark SQL 中，有三个函数用于排序。有 *orderBy* (或等价的 *sort* )、 *sortWithinPartitions、*和 *sortBy。让我们看看它们之间有什么区别，以及如何使用它们:*

1.  [*orderBy*](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html#pyspark.sql.DataFrame.orderBy) —这是一个将调用全局排序的数据帧转换。这将首先运行一个单独的作业，该作业将对数据进行采样，以检查排序列中值的分布。然后，该分布用于创建分区的边界，数据集将被混洗以创建这些分区。这是一个非常昂贵的操作，如果您将数据保存到一个分区表中，并且如果集群上的数据分布与文件系统中的表的最终分布非常不同，则可能会创建许多小文件(例如，如果您按*日期*排序，并且您的表按*国家*分区，则 Spark 作业最后阶段的每个任务都可能携带每个*国家*的数据，因此每个任务都会向每个文件系统分区中写入一个新文件)。
2.  [*sort within partitions*](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html#pyspark.sql.DataFrame.sortWithinPartitions)—这也是一个数据帧转换，与前面的情况不同，Spark 不会尝试实现全局排序，而是将每个分区分别排序。所以在这里，您可以使用 *repartition()* 函数(这也将创建一个 shuffle)在 Spark 集群上分发您所需要的数据，然后调用 *sortWithinPartitions* 对每个分区进行排序。与前一种方法相比，这种方法的优点是您可以避免拥有大量小文件，并且仍然可以对输出进行排序。
3.  [*sortBy*](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html#pyspark.sql.DataFrameWriter.sortBy) —如果我们也调用 *bucketBy* 并使用 *saveAsTable* 方法进行保存，则可以在*data frame writer*上调用该函数(在调用 *df.write* 之后)。它将确保每个存储桶都被排序(一个存储桶可以是一个文件，也可以是多个文件，这取决于写本文时 Spark 集群上的数据分布)。

使用前两个函数有一个**大问题**，你在文档中找不到(写于 2020 年 10 月)。让我们看一个简单的例子。为了简单起见，我们希望按照*年*对数据进行分区，并按照 *user_id* 对每个分区进行排序，为了保存，我们使用 *saveAsTable()* 。如果你只是打电话

```
(
  df.repartition('year')
  .sortWithinPartitions('user_id')
  .write
  .mode('overwrite')
  .partitionBy('year')
  .option('path', output_path)
  .saveAsTable(table_name)
)
```

这是行不通的！不会产生任何错误，会保存数据，但订单不会被保留！重点是，当将数据写入文件格式时，Spark 要求这种排序:

(`partitionColumns + bucketIdExpression + sortColumns`)

这里的 *partitionColumns* 是我们将数据分区到文件系统所依据的列，*bucketingiexpression*是从 bucketing 列派生而来的(与我们的查询无关，因为我们在这里没有使用 bucketing)，而 *sortColumns* 是在带有 bucketing 的 *sortBy* 中使用的列(同样与我们的查询无关)。如果数据是按照这些列(可能还有更多)排序的，那么这个要求将会得到满足，Spark 将会保持这个顺序。但是，如果不满足这个要求，Spark 将会忘记之前的顺序，并在写入数据时根据这个要求再次对数据进行排序。在我们的示例中，所需的排序是( *year* )，这是分区列，我们在这里没有任何存储桶。然而，这一要求并没有得到满足，因为实际的排序是( *user_id* )，这是我们对数据进行排序的列，这就是为什么 Spark 不会保留我们的顺序，而是按照 *year* 列再次对数据进行排序。

```
required_orering = (year)
actual_ordering = (user_id) ... this doesn't satisfy the requirement
```

为了实现我们的目标并保存按 *user_id* 排序的数据(每个分区),我们可以这样做:

```
(
  df.repartition('year')
  **.sortWithinPartitions('year', 'user_id')**
  .write
  .mode('overwrite')
  .partitionBy('year')
  .option('path', output_path)
  .saveAsTable(table_name)
)
```

注意现在的区别，我们按照*年*和*用户 id* 对每个分区进行显式排序，但是由于数据将按照*年*进行分区，因此按照该列进行排序并不重要。重要的是，现在 Spark 的要求将得到满足，因此 Spark 将保留我们的顺序并保存按*年*和 *user_id* 排序的数据，由于我们按*年*分区，这基本上意味着每个分区将按 *user_id* 排序，这正是我们想要的。

```
required_orering = (year)
actual_ordering = (year, user_id) ... this satisfies the requirement
```

在每个分区排序的位置保存分区数据，将允许使用过滤器进行高效的分析查询，该过滤器将跳过两个级别上的数据—分区级别和行组级别。想象这样一个查询:

```
(
  spark.table(table_name)
  .filter(col('year') == 2020)
  .filter(col('user_id') == 1)
  .collect()
)
```

这将首先使用分区过滤器来修剪分区，并且在该单个分区 2020 内，它将检查来自每个行组的镶木地板脚的元数据。基于元数据中的统计数据，Spark 将挑选最小*小于等于 1* 且最大*大于等于 1* 的行组，并且只扫描这些行组，因此这将加快查询速度，尤其是在有许多行组可以跳过的情况下。

## 结论

在本文中，我们描述了如何使用 Spark 将数据保存为文件格式(特别是 Apache Parquet)的三种不同方式，还解释了为什么 Parquet 是如此流行的数据格式，尤其是在分析查询中。我们还指出了一些不太明显的 Spark 特性，比如 *insertInto* 函数的行为或者如何在排序状态下保存数据。