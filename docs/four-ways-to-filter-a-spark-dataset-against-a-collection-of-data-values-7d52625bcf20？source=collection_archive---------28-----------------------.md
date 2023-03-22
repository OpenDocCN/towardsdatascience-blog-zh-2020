# 根据数据值集合过滤 Spark 数据集的四种方法

> 原文：<https://towardsdatascience.com/four-ways-to-filter-a-spark-dataset-against-a-collection-of-data-values-7d52625bcf20?source=collection_archive---------28----------------------->

![](img/3292e473603453024b403bf1680d4eb6.png)

Ref: [Pixabay](https://pixabay.com/photos/filter-photo-effect-glass-407151/)

## [实践教程](https://towardsdatascience.com/tagged/hands-on-tutorials)，SPARK 执行指南

## 根据数据值集合过滤 Spark 数据集是许多数据分析流程中经常遇到的问题。这个特别的故事将解释四种不同的方法来达到同样的目的。

假设有一个非常大的数据集“A ”,其模式如下:

```
root:
| — empId: Integer
| — sal: Integer
| — name: String
| — address: String
| — dept: Integer
```

数据集“A”需要根据一组员工 id(empid)、“B”(可以广播给执行者)进行过滤，以获得过滤后的数据集“A”。过滤操作可以表示为:

```
A` = A.filter(A.empId contains in 'B')
```

为了实现这种最常见的过滤场景，您可以在 Spark 中使用四种类型的转换，每一种都有其优缺点。这里描述了使用所有这四种转换来执行这个特定的过滤场景，并详细说明了每种转换的可靠性和效率。

**Filter:**Filter transformation(在布尔条件表达式或布尔返回过滤函数上过滤数据集记录)，在数据集上，可以用[以下方式](https://spark.apache.org/docs/latest/api/java/index.html?org/apache/spark/sql/Dataset.html):

```
1\. Dataset<T> A` = A.filter(Column condition)
2\. Dataset<T> A` = A.filter(FilterFunction<T> func)
3\. Dataset<T> A` = A.filter(String conditionExpr)
```

对于筛选场景，如前所述，可以对“A”使用“Filter”转换，将“FilterFunction”作为输入。对相应数据集的分区中包含的每个记录调用“FilterFunction ”,并返回“true”或“false”。在我们的过滤场景中，将对数据集“A”的每个记录调用 FilterFunction，并检查记录的“empId”是否存在于广播的 empId 集“B”中(“B”由相应的哈希表支持)。

> 不管数据集“A”的大小如何，如上所述的过滤器变换的使用都是非常简单、健壮和有效的。这是因为，转换是逐记录调用的。此外，由于所广播的 empIds 组由执行器上的哈希表支持，所以在过滤函数中对每个记录的过滤查找保持有效。

**映射:**映射转换(对数据集的每条记录应用一个函数，以返回空的、相同的或不同的记录类型)，在数据集上以下列方式用于:

```
Dataset<U> A` = A.map(MapFunction<T,U> func, Encoder<U> encoder)
```

对于过滤场景，如前所述，可以对“A”使用“Map”转换，将“MapFunction”作为输入。在我们的过滤场景中，将对数据集“A”的每个记录调用“MapFunction ”,并检查记录的“empId”是否存在于广播的 empId 集“B”中(由相应的哈希表支持)。如果记录存在，MapFunction 将返回相同的结果。如果记录不存在，*将返回 NULL* 。此外，MapFunction 的编码器输入将与数据集“A”的相同。

> 尽管‘Map function’的语义类似于‘Filter function ’,但是对于过滤场景，如上所述的‘Map’转换的使用与直接‘Filter’转换方法相比并不简单和优雅。必须在转换中明确提供额外的编码器输入。此外，在调用“Map”转换后，需要过滤输出中的空值，因此,“Map”方法不如“Filter”方法有效。然而，该方法的可靠性类似于“过滤”方法，因为无论“A”的大小如何，它都不会出现问题。这是因为,“映射”转换也是逐记录调用的。

**map partitions**:[map partitions](https://ajaygupta-spark.medium.com/apache-spark-mappartitions-a-powerful-narrow-data-transformation-d635964526d6)转换(在数据集的每个[分区](https://www.amazon.com/dp/B08KJCT3XN/)上应用一个函数，返回 null 或迭代器到相同或不同记录类型的新集合)，在数据集上，以下面的[方式使用](https://spark.apache.org/docs/latest/api/java/index.html?org/apache/spark/sql/Dataset.html):

```
Dataset<U> A` = A.map(MapPartitionsFunction<T,U> func, Encoder<U> encoder)
```

对于筛选方案，如前所述，还可以对“A”使用“MapPartitions”转换，该转换将“MapPartitionsFunction”作为输入。在我们的过滤场景中，将在数据集“A”的每个分区上调用“MapPartitionsFunction ”,迭代该分区的所有记录，并检查每个记录，如果记录的“empId”存在于广播的 empId 集“B”中(由相应的哈希表支持)。在记录存在的情况下，相同的记录将被添加到在“MapPartitionsFunction”中初始化的可返回集合中。最后，从“MapPartitionsFunction”返回可返回集合的迭代器。

> 与“映射”和“过滤”方法相比，“映射分区”方法通常更有效，因为它是分区方式操作，而不是记录方式操作。然而，与“映射”类似，必须在转换中明确提供编码器输入。此外，在数据集‘A’的某些分区的大小超过为执行每个分区计算任务而提供的存储器的情况下,‘map partitions’方法可能变得非常不可靠。这是因为更大的分区可能导致潜在的更大的可返回集合，从而导致内存溢出。

**内连接**:内连接转换应用于两个输入数据集，A & B，采用[的方式](https://spark.apache.org/docs/latest/api/java/index.html?org/apache/spark/sql/Dataset.html):

```
Dataset<Row> A` = A.join(Dataset<?> B, Column joinExprs)
```

对于筛选场景，如前所述，还可以对“A”使用“内部连接”转换，该转换根据连接条件连接“B”的数据集表示(A.empId 等于 B.empId ),并从每个连接的记录中只选择“A”的字段。

> “内部连接”方法返回一般“行”对象的数据集，因此需要使用编码器将其转换回 A 的记录类型的数据集，以匹配精确的过滤器语义。然而，类似于“过滤”方法，“内部连接”方法是有效和可靠的。效率来自于这样一个事实，因为“B”是可广播的，[Spark 将选择最有效的“广播散列连接”方法](/demystifying-joins-in-apache-spark-38589701a88e)来执行连接。此外，可靠性来自于“内部连接”方法适用于“A”的大型数据集，就像“过滤”方法一样。

考虑到所有的方法，从可靠性和效率的角度来看，我会选择“过滤”方法作为最安全的选择。此外，要注意的是,“过滤器”方法还允许我以类似的效率和健壮性执行反搜索，这是“内部连接”所不允许的。

如果对这个故事有反馈或疑问，请写在评论区。我希望，你会发现它有用。这里是我在 Apache Spark 上的其他综合报道的链接。 *还有，*拿一份我最近出版的关于 Spark Partitioning 的书:《Spark Partitioning 指南: [*深入讲解 Spark Partitioning*](https://www.amazon.com/dp/B08KJCT3XN/)》