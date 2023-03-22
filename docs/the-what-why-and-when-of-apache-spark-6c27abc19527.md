# Apache Spark 的内容、原因和时间

> 原文：<https://towardsdatascience.com/the-what-why-and-when-of-apache-spark-6c27abc19527?source=collection_archive---------6----------------------->

## 编写代码之前的 Spark 基础知识

![](img/7edd72ac4aabdd431de5e080016cc4ae.png)

伊森·胡佛在 [Unsplash](https://unsplash.com/s/photos/spark?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 什么是火花？

Spark 被称为“通用分布式数据处理引擎”1 和“用于大数据和机器学习的快速统一分析引擎” [](#a0b6) 。它通过将工作分成块并在计算资源之间分配这些块，让您更快地处理大数据集。

它可以处理高达千兆字节(那是数百万千兆字节！)的数据，并管理多达数千台物理机或虚拟机。

一些与众不同的特性包括它对内存的使用和对简单开发的关注。它会尝试将所有数据保存在内存中，只有在内存不足时才会写入磁盘。请注意，这确实有在失败时需要从头开始的风险，但好处是消除了耗时的读写过程。它通过强调 API 设计以及与几种语言和工具的集成，使开发变得容易。

# **您为什么会想要使用 Spark？**

Spark 有一些很大的优势:

*   大型数据集的高速数据查询、分析和转换。
*   与 MapReduce 相比，Spark 在 Java 虚拟机(JVM)进程中提供了更少的磁盘读写、多线程任务(来自 [Wikipedia](https://en.wikipedia.org/wiki/Multithreading_(computer_architecture)) :线程共享单核或多核资源)
*   非常适合迭代算法(使用基于先前估计的一系列估计)。
*   如前所述，易于使用的 API 在易于开发、可读性和维护方面有很大的不同。
*   超级快，特别是对于交互式查询。(比经典 Hadoop Hive 查询快 100 倍，无需重构代码！)
*   支持多种语言和与其他流行产品的集成。
*   有助于使复杂的数据管道变得连贯和简单。

# 【Spark 什么时候效果最好？

*   如果您已经在使用受支持的语言(Java、Python、Scala、R)
*   Spark 使分布式数据(亚马逊 S3、MapR XD、Hadoop HDFS)或 NoSQL 数据库(MapR 数据库、Apache HBase、Apache Cassandra、MongoDB)的工作变得无缝
*   当你使用函数式编程时(函数的输出只取决于它们的参数，而不是全局状态)

## **一些常见用途:**

*   使用大型数据集执行 ETL 或 SQL 批处理作业
*   处理来自传感器、物联网或金融系统的流式实时数据，尤其是与静态数据相结合的数据
*   使用流数据触发响应
*   执行复杂的会话分析(例如，根据网络活动对用户进行分组)
*   机器学习任务

# **什么时候你不想使用 Spark？**

对于多用户系统，使用共享内存，Hive 可能是更好的选择 [](#a0b6) 。对于实时、低延迟的处理，你可能更喜欢阿帕奇卡夫卡 [⁴](#d7fd) 。对于小数据集，它不会给你带来巨大的收益，所以你最好使用典型的库和工具。

如你所见，Spark 并不是每项工作的最佳工具，但它绝对是你在当今大数据世界工作时应该考虑的工具。Spark 的最后一个优点是，它是开源的，大牌贡献者(包括脸书、网飞、Palantir、优步、IBM、英特尔、红帽、英伟达、LinkedIn、苹果、谷歌、甲骨文、Stripe、腾讯、华为、阿里巴巴、Databricks 等)定期推出改进和新功能。

## 延伸阅读:

1.  MapR.com: [Spark 101:它是什么，它有什么作用，它为什么重要](https://mapr.com/blog/spark-101-what-it-what-it-does-and-why-it-matters/)，
2.  Quoble: [顶级 Apache Spark 用例](https://www.qubole.com/blog/apache-spark-use-cases/)
3.  data bricks:[Apache Spark—什么是 Spark](https://databricks.com/spark/about)
4.  Datanami: [何时使用开源的 Apache Cassandra、Kafka、Spark 和 Elasticsearch](https://www.datanami.com/2019/12/03/when-and-when-not-to-use-open-source-apache-cassandra-kafka-spark-and-elasticsearch/)