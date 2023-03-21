# 加快大型交叉连接火花数据帧的计算速度

> 原文：<https://towardsdatascience.com/make-computations-on-large-cross-joined-spark-dataframes-faster-6cc36e61a222?source=collection_archive---------24----------------------->

## 减少输入数据帧上的分区数量，以加快交叉连接数据帧的计算速度。

![](img/96b81eb54b4f916d810c8e7b1b5e5e49.png)

照片由[萨夫](https://unsplash.com/@saffu?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

[Apache Spark](https://spark.apache.org/) 将数据分割成分区，并在这些分区上并行执行任务，使您的计算并行运行。分区的数量对 Spark 计算的运行时间有直接影响。

Spark 计算通常涉及交叉连接两个 Spark [数据帧](https://spark.apache.org/docs/latest/sql-programming-guide.html#datasets-and-dataframes)，即创建一个新的数据帧，包含两个输入数据帧中每一行的组合。当交叉连接大型数据帧时，Spark 会成倍增加输入数据帧的分区数量。这可能导致交叉连接数据帧中的分区数量显著增加。因此，由于管理分区上许多小任务的额外开销，在该数据帧上运行计算会非常慢。

让我们考虑两个场景来理解交叉连接数据帧时分区是如何工作的:

## 场景 1:较小的数据帧

如果输入数据帧的大小较小，那么交叉连接数据帧的分区数将等于输入数据帧的分区数。

```
scala> val xDF = (1 to 1000).toList.toDF("x")
scala> xDF.rdd.partitions.size
res11: Int = 2scala> val yDF = (1 to 1000).toList.toDF("y")
scala> yDF.rdd.partitions.size
res12: Int = 2scala> val crossJoinDF = xDF.crossJoin(yDF)
scala> crossJoinDF.rdd.partitions.size
res13: Int = 2
```

在这种情况下，
xDF 的分区= = yDF 的分区= = crossJoinDF 的分区

如果输入数据帧的分区，即 xDF 或 yDF 不相等，那么交叉连接的数据帧的分区将等于输入数据帧之一。

## 场景 2:更大的数据框架

如果我们增加输入数据帧的数据大小，交叉连接数据帧上的分区行为会发生变化。
在以下示例中，我将输入数据帧中的行数从 1000 增加到 1，000，000

```
scala> val xDF = (1 to 1000000).toList.toDF("x")
scala> xDF.rdd.partitions.size
res15: Int = 2scala> val yDF = (1 to 1000000).toList.toDF("y")
scala> yDF.rdd.partitions.size
res16: Int = 2scala> val crossJoinDF = xDF.crossJoin(yDF)
scala> crossJoinDF.rdd.partitions.size
res17: Int = 4
```

在这种情况下，交叉连接的数据帧的分区大小等于输入数据帧分区

的乘积 crossJoinDF =(xDF 的分区)yDF 的分区)。

如果您的输入数据帧有更多的列或更大的数据类型，您也能够在只有几千行的数据帧上复制这种行为。

一个数据帧的确切分区数量因硬件而异，但交叉连接大型数据帧时分区的交叉乘法在所有类型的硬件上都是一致的。

# 那么，如果 Spark 将大输入数据帧的分区相乘来为交叉连接的数据帧创建分区，会有什么问题呢？

如果您的输入数据帧包含几百个分区(大约 100 个)，这是处理大数据时的典型情况，那么交叉连接的数据帧将包含大约 10，000 个分区。

数据帧分区的数量对计算的运行时间有影响:

*   如果分区太少，您的计算将无法利用集群中所有可用的并行性。
*   如果你有太多的分区，在管理许多小任务时会有过多的开销，使得你的计算运行起来非常慢。

交叉连接具有不到 100 个分区的大型数据帧属于后一种情况，这会导致数据帧具有 10，000 个数量级的太多分区。这使得对交叉连接数据帧的任何操作都非常慢。在具有大量分区的交叉连接数据帧上运行操作时，您可能会遇到以下异常:

```
org.apache.spark.SparkException Job aborted due to stage failure: 
Total size of serialized results of 147936 tasks (1024.0 MB) is bigger than 
spark.driver.maxResultSize (1024.0 MB)
```

这是因为 Spark 将每个任务的状态数据发送回驱动程序。由于有许多分区(或任务)，这些数据经常会超过默认的 1024 MB 的限制。

增加 Spark 配置*Spark . driver . max resultsize*的值将使您的计算运行时不会抛出上述异常，但不会使它更快。大量分区的固有问题仍然存在。

# 你如何使计算更快？

要加快计算速度，请在交叉连接之前减少输入数据帧的分区数量，以便交叉连接后的数据帧不会有太多分区。

作为一个真实世界的例子，我需要交叉连接两个数据帧 df1 和 df2，以计算两个数据帧的行的每个组合之间的余弦相似性。两个数据帧都由文本和长度为 500 的双精度数组组成，表示文本嵌入。数据帧 df1 由大约 60，000 行组成，数据帧 df2 由 130，000 行组成。使用 40 名 [G.1X](https://docs.aws.amazon.com/glue/latest/dg/add-job.html) 型工人在 [AWS 胶水](https://aws.amazon.com/glue/)上对交叉连接的数据帧进行连续计数需要大约 6 个小时。在交叉连接之前将 df1 和 df2 重新划分为更少的分区，将交叉连接数据帧的计算时间减少到 40 分钟！

为了便于说明，我将从 df1 和 df2 数据帧中抽取少量样本。将包含 17，000 行和 200 个分区的 df1 与包含 15，000 行和 200 个分区的 df2 交叉连接，可创建包含 40，000 个分区的交叉连接数据帧。对交叉连接的数据帧进行计数需要 285，163，427，988 ns，即 4.75 分钟。

> *以下代码是在 AWS Glue 上执行的，有 40 名 G1 类型的工人。x 使用 Spark 2.4*

```
scala> df1.count()
res73: Long = 17000scala> df1.show(4)
+---------------------------+--------------------+
|                       text|      text_embedding|
+---------------------------+--------------------+
|eiffel tower location      |[0.4, 0.02, 0.1, ...|
|pounds kilogram conversion |[0.01, 0.2, 0.1, ...|
|capital of spain           |[0.05, 0.2, 0.2, ...|
|mount everest height       |[0.07, 0.1, 0.1, ...|
+---------------------------+--------------------+scala> df1.rdd.partitions.size
res74: Int = 200scala> df2.count()
res75: Long = 15000scala> df2.rdd.partitions.size
res76: Int = 200scala> df2.show(4)
+------------------------------+--------------------+
|                          text|      text_embedding|
+------------------------------+--------------------+
|where is eiffel tower located |[0.3, 0.01, 0.1, ...|
|how many pounds in a kilogram |[0.02, 0.2, 0.1, ...|
|what is the capital of spain  |[0.03, 0.2, 0.2, ...|
|how tall is mount everest     |[0.06, 0.1, 0.1, ...|
+------------------------------+--------------------+scala> val finalDF = df1.crossJoin(df2)scala> finalDF.rdd.partitions.size 
res77: Int = 40000scala> time {finalDF.count()}
Elapsed time: 285163427988ns
res78: Long = 255000000
```

如果我们在执行交叉连接之前将 df1 和 df2 上的分区数量减少到 40，那么在交叉连接的数据帧上运行计数的时间将减少到 47，178，149，994 ns，即 47 秒！我们选择了 40 个分区来利用 40 个工作集群中所有可用的并行性。

```
scala> val df1 = df1.repartition(40)
scala> df1.rdd.partitions.size 
res80: Int = 40scala> val df2 = df2.repartition(40)
scala> df2.rdd.partitions.size 
res81: Int = 40scala> val finalDF = df1.crossJoin(df2)
scala> finalDF.rdd.partitions.size 
res82: Int = 1600scala> time {finalDF.count()}
Elapsed time: 47178149994ns
res86: Long = 255000000
```

在交叉连接数据帧之前减少分区数量，可以将计算交叉连接数据帧的时间减少 6 倍！

# 外卖

下次在执行交叉连接后发现 Spark 计算变慢时，一定要检查交叉连接数据帧上的分区数量。如果分区太多，请减少输入数据帧上的分区数量，以加快对结果交叉连接数据帧的操作。