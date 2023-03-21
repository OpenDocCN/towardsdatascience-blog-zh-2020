# 数据科学家的实用火花技巧

> 原文：<https://towardsdatascience.com/practical-spark-tips-for-data-scientists-145d85e9b2d8?source=collection_archive---------13----------------------->

![](img/9bcf72e7835f7b4d71c63cb03eeb9f28.png)

图片由[安德鲁·马丁](https://pixabay.com/users/aitoff-388338/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1984421)来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1984421)

## 与 pySpark 一起工作时，让您的生活更加舒适

我知道——Spark 有时令人沮丧。

***虽然有时候我们可以使用***[***Rapids***](/minimal-pandas-subset-for-data-scientist-on-gpu-d9a6c7759c7f?source=---------5------------------)***或*** [***并行化***](/add-this-single-word-to-make-your-pandas-apply-faster-90ee2fffe9e8?source=---------11------------------)*等工具来管理我们的大数据，但是如果您正在处理数 TB 的数据，就无法避免使用Spark。*

*在我关于 Spark 的最后几篇文章中，我解释了如何使用 PySpark RDDs 和 T42 数据帧。尽管这些帖子解释了很多关于如何使用 rdd 和 Dataframe 操作的内容，但它们仍然不够。*

*为什么？因为 spark 经常出现内存错误，而且只有当您真正使用 Spark 处理大数据集时，您才能真正使用 Spark。*

***这篇文章的主题是“数据科学家实用的火花和内存管理技巧”***

# *1.地图端连接*

*![](img/ba7d9015b8e349b56479e1b43a934a9f.png)*

*连接数据框架*

*Spark 中的连接语法与 pandas 非常相似:*

```
*df3 = df1.join(df2, df1.column == df2.column,how='left')*
```

*但是我面临一个问题。`df1`大约有 10 亿行，而`df2`大约有 100 行。当我尝试上述连接时，它不起作用，并在运行 20 分钟后出现内存耗尽错误。*

*我在一个非常大的集群上编写这段代码，这个集群有 400 多个执行器，每个执行器都有 4GB 以上的 RAM。当我尝试使用多种方案对数据帧进行重新分区时，我被难住了，但似乎没有任何效果。*

*那我该怎么办呢？Spark 不能处理仅仅十亿行吗？不完全是。我只需要使用 Spark 术语中的地图端连接或广播。*

```
***from** **pyspark.sql.functions** **import** broadcast
df3 = df1.join(broadcast(df2), df1.column == df2.column,how='left')*
```

*使用上面简单的广播代码，我能够将较小的`df2`发送到所有节点，这并没有花费很多时间或内存。后端发生的事情是将`df2`的副本发送到所有分区，每个分区使用该副本进行连接。这意味着 df1 没有数据移动，它比 df2 大很多。*

# *2.火花簇构型*

*![](img/27ac86d90e518426adac14008fa85b98.png)*

*根据您的任务大小设置并行度和工作节点*

*当我开始使用 Spark 时，让我的生活变得困难的还有 Spark 集群需要配置的方式。基于您想要运行的作业，您的 spark 集群可能需要大量定制配置和调优。*

*一些最重要的配置和选项如下:*

## *a.spark.sql.shuffle.partitions 和 spark.default.parallelism:*

*`spark.sql.shuffle.partitions`配置为连接或聚合而重排数据时要使用的分区数量。`spark.default.parallelism`是 rdd 中由`join`、`reduceByKey`和`parallelize`等转换返回的默认分区数量，当用户没有设置时。这些值的默认值是 200。*

****简而言之，这些设置了您想要在集群中拥有的并行度。****

*如果您没有很多数据，值 200 没问题，但是如果您有大量数据，您可能希望增加这些数字。也要看你有多少遗嘱执行人。我的集群相当大，有 400 个执行者，所以我把它保持在 1200。一个经验法则是保持它是执行人数量的倍数，这样每个执行人最终都有多个工作。*

```
*sqlContext.setConf( "spark.sql.shuffle.partitions", 800)
sqlContext.setConf( "spark.default.parallelism", 800)*
```

## *b.spark . SQL . parquet . binaryasstring*

*我在 Spark 中处理`.parquet`文件，我的大部分数据列都是字符串。但不知何故，每当我在 Spark 中加载数据时，字符串列都会被转换成二进制格式，在这种格式下，我无法使用任何字符串操作函数。我解决这个问题的方法是使用:*

```
*sqlContext.setConf("spark.sql.parquet.binaryAsString","true")*
```

*上述配置在加载拼花文件时将二进制格式转换为字符串。现在这是我在使用 Spark 时设置的默认配置。*

## *c.纱线配置:*

*您可能需要调整其他配置来定义您的集群。但是这些需要在集群启动时设置，不像上面的那些那样动态。我想放在这里的几个是用于管理 executor 节点上的内存溢出。有时，执行程序核心会承担大量工作。*

*   *spark . yarn . executor . memory overhead:8192*
*   *yarn . node manager . vmem-check-enabled:False*

*在设置 spark 集群时，您可能需要调整很多配置。你可以在[官方文件](https://spark.apache.org/docs/latest/configuration.html)里看看。*

# *3.分配*

*![](img/43cf4c75b56870192f6f891488d5a599.png)*

*让员工处理等量的数据，让他们满意*

*如果在处理所有转换和连接时，您觉得数据有偏差，您可能需要对数据进行重新分区。最简单的方法是使用:*

```
*df = df.repartition(1000)*
```

*有时，您可能还希望按照已知的方案进行重新分区，因为该方案可能会在以后被某个连接或聚集操作使用。您可以使用多个列通过以下方式进行重新分区:*

```
**df = df.repartition('cola', 'colb','colc','cold')**
```

*您可以使用以下公式获得数据框中的分区数量:*

```
*df.rdd.getNumPartitions()*
```

*您还可以通过使用`glom`函数来检查分区中记录的分布。这有助于理解在处理各种转换时发生的数据偏差。*

```
**df.glom().map(len).collect()**
```

# *结论*

*有很多事情我们不知道，我们不知道。这些被称为未知的未知。只有通过多次代码失败和读取多个堆栈溢出线程，我们才明白我们需要什么。*

*在这里，我尝试总结了一些我在使用 Spark 时遇到的内存问题和配置问题，以及如何解决这些问题。Spark 中还有很多其他的配置选项，我没有介绍，但是我希望这篇文章能让你对如何设置和使用它们有所了解。*

*现在，如果你需要学习 Spark 基础知识，看看我以前的帖子:*

*[](/the-hitchhikers-guide-to-handle-big-data-using-spark-90b9be0fe89a) [## 使用 Spark 处理大数据的指南

### 不仅仅是介绍

towardsdatascience.com](/the-hitchhikers-guide-to-handle-big-data-using-spark-90b9be0fe89a) 

还有，如果你想了解更多关于 Spark 和 Spark DataFrames 的知识，我想调出这些关于 [**大数据精要的优秀课程:Coursera 上的 HDFS、MapReduce 和 Spark RDD**](https://coursera.pxf.io/4exq73) 。

谢谢你的阅读。将来我也会写更多初学者友好的帖子。在 [**中**](https://medium.com/@rahul_agarwal?source=post_page---------------------------) 关注我或者订阅我的 [**博客**](http://eepurl.com/dbQnuX?source=post_page---------------------------) 了解他们。一如既往，我欢迎反馈和建设性的批评，可以通过 Twitter[**@ mlwhiz**](https://twitter.com/MLWhiz?source=post_page---------------------------)联系

此外，一个小小的免责声明——这篇文章中可能会有一些相关资源的附属链接，因为分享知识从来都不是一个坏主意。*