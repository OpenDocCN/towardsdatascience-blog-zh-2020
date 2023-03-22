# Spark 3 中的谓词与投影下推

> 原文：<https://towardsdatascience.com/predicate-vs-projection-pushdown-in-spark-3-ac24c4d11855?source=collection_archive---------7----------------------->

## 谓词和投影下推及其在 PySpark 3 中实现的比较

![](img/9fd66b41f4ef2eff9d45b6935cb7160e.png)

([来源](https://unsplash.com/photos/LqKhnDzSF-8))

处理大数据伴随着大量处理成本和花费大量时间扩展这些成本的挑战。It 需要了解适当的大数据过滤技术，以便从中受益。对于希望在过滤子领域(尤其是在嵌套结构化数据上)使用 [Apache Spark](https://spark.apache.org/) 的最新增强功能的数据科学家和数据工程师来说，这篇文章是必不可少的。

# Spark 2.x 和 Spark 3.0 在嵌套过滤方面的差异

用 **Spark 2.x** ，用**最大 2 级嵌套结构的文件。json** 和**。拼花地板**扩展可以被读取。

> **示例:**
> 
> 过滤器(col(' **library.books** ')。isNotNull())

使用 [**Spark 3**](https://spark.apache.org/releases/spark-release-3-0-0.html) ，现在可以同时使用*和 ***读取文件。2+级嵌套结构中的 snappy parquet*** 扩展，无需任何模式展平操作。*

> ***例如:***
> 
> *过滤器(col('**library . books . title**')。isNotNull())*

## *用于下推过滤的火花会话配置*

*创建 spark 会话时，应启用以下配置，以使用 Spark 3 的下推功能。默认情况下，链接到*下推过滤*活动的设置值被激活。*

```
*"spark.sql.parquet.filterPushdown", "true"
"spark.hadoop.parquet.filter.stats.enabled", "true"
"spark.sql.optimizer.nestedSchemaPruning.enabled","true"
"spark.sql.optimizer.dynamicPartitionPruning.enabled", "true"*
```

*Spark 中有**和**两种下推过滤技术，分别是**谓词下推**和**投影下推**，它们具有以下不同的特性，如本文以下部分所述。*

# *1.谓词下推*

***谓词下推**指向影响返回行数的 **where** 或**过滤器**子句。基本上关系到**哪些行** **会被过滤，而不是哪些列**。由于这个原因，在嵌套列上应用过滤器作为***【library . books】***仅仅返回值不为空的记录时，谓词下推函数将使 parquet 读取指定列中不包含空值的块。*

## *1.1.带有分区修剪的谓词下推*

*分区消除技术允许在从相应的文件系统读取文件夹时优化性能，以便可以读取指定分区中的所需文件。它将致力于将数据过滤转移到尽可能靠近源的位置，以防止将不必要的数据保存在内存中，从而减少磁盘 I/O。*

*下面，可以观察到 filter of partition 的下推动作，也就是*' library . books . title ' = ' HOST '*filter 被下推进入 *parquet* 文件扫描。该操作能够最小化文件和扫描数据。*

```
*data.filter(col('library.books.title') == 'THE HOST').explain()*
```

*对于更详细的输出，包括*解析的逻辑计划、分析的逻辑计划、优化的逻辑计划、物理计划、****‘扩展’***参数可以添加到 explain()函数中，如下所示。*****

```
*****data.filter(col('library.books.title') == 'THE HOST').explain(**'extended'**)*****
```

*****它还可以减少传递回 Spark 引擎的数据量，以便在' *price'* 列的聚合函数中进行平均运算。*****

```
*****data.filter(col('library.books.title') == 'THE HOST').groupBy('publisher').agg(avg('price')).explain()*****
```

*****Parquet 格式的文件为每一列保留了一些不同的统计指标，包括它们的最小值和最大值。谓词下推有助于跳过不相关的数据，处理所需的数据。*****

# *****2.投影下推*****

*******Projection Pushdown** 代表带有 **select** 子句的选定列，该子句影响返回的列数。它将数据存储在**列**中，因此当您的投影将查询限制到指定的列时，将会返回这些列。*****

```
*****data.select('library.books.title','library.books.author').explain()*****
```

*****这意味着对*‘library . books . title’，‘library . books . author’*
列的扫描意味着在将数据发送回 Spark 引擎之前，将在文件系统/数据库中进行扫描。*****

## *******结论*******

*****对于**投影**和**谓词下推**，有一些关键点需要强调。**下推过滤**作用于分区的列，这些列是根据拼花格式文件的性质计算的。为了能够从中获得最大的好处，分区列应该携带具有足够匹配数据的较小值，以便将正确的文件分散到目录中。*****

*****防止过多的小文件会导致扫描效率因过度并行而降低。此外，阻止接受太少的大文件可能会损害并行性。*****

*******投影下推**功能通过消除表格扫描过程中不必要的字段，使文件系统/数据库和 Spark 引擎之间的数据传输最小化。它主要在数据集包含太多列时有用。*****

*****另一方面，**谓词下推**通过在过滤数据时减少文件系统/数据库和 Spark 引擎之间传递的数据量来提高性能。*****

*******投影下推**通过**基于列的**和**谓词下推**通过**基于行的过滤**来区分。*****

*****非常感谢您的提问和评论！*****

# *******参考文献*******

1.  *****[火花释放 3.0.0](https://spark.apache.org/releases/spark-release-3-0-0.html)*****
2.  *****[下推析取谓词](https://issues.apache.org/jira/browse/SPARK-27699)*****
3.  *****[一般化嵌套列修剪](https://issues.apache.org/jira/browse/SPARK-25603)*****
4.  *****[嵌套字段的 Parquet 谓词下推](https://issues.apache.org/jira/browse/SPARK-17636)*****