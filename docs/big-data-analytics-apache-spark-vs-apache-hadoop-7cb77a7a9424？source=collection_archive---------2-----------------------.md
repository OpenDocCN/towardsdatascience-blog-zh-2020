# 大数据分析:Apache Spark 与 Apache Hadoop

> 原文：<https://towardsdatascience.com/big-data-analytics-apache-spark-vs-apache-hadoop-7cb77a7a9424?source=collection_archive---------2----------------------->

## 了解为什么创建 Apache Spark，以及它如何解决 Apache Hadoop 的缺点。

![](img/20f9706f5b6a51f2c7855d9a8d57e426.png)

# **什么是大数据？**

> “大数据是高容量、高速度和高多样性的信息资产，需要经济高效、创新的信息处理形式来增强洞察力和决策能力”[11]

如果没有合适的工具、框架和技术，大数据分析可能会非常耗时、复杂且计算量大。当数据量过大，无法在单台机器上处理和分析时，Apache Spark 和 Apache Hadoop 可以通过并行处理和分布式处理来简化任务。要了解大数据分析中对并行处理和分布式处理的需求，首先了解什么是“大数据”非常重要。大数据的高速生成要求数据的处理速度也非常快，大数据的多样性意味着它包含各种类型的数据，包括结构化、半结构化和非结构化数据[4]。大数据的数量、速度和多样性需要新的创新技术和框架来收集、存储和处理数据，这就是 Apache Hadoop 和 Apache Spark 诞生的原因。

# **并行处理与分布式处理**

了解什么是并行处理和分布式处理，将有助于理解 Apache Hadoop 和 Apache Spark 如何用于大数据分析。因为并行处理和分布式处理都涉及到将计算分解成更小的部分，所以两者之间可能会有混淆。并行计算和分布式计算的区别在于内存架构[10]。

> “并行计算是同时使用一个以上的处理器来解决一个问题”[10]。
> 
> “分布式计算是同时使用多台计算机来解决一个问题”[10]。

并行计算任务访问相同的内存空间，而分布式计算任务不访问，因为分布式计算是基于磁盘的，而不是基于内存的。一些分布式计算任务在一台计算机上运行，一些在其他计算机上运行[9]。

# **Apache Spark vs . Apache Hadoop**

Apache Hadoop 和 Apache Spark 都是用于大数据处理的开源框架，但有一些关键区别。Hadoop 使用 MapReduce 来处理数据，而 Spark 使用弹性分布式数据集(rdd)。Hadoop 有一个分布式文件系统(HDFS)，这意味着数据文件可以存储在多台机器上。文件系统是可扩展的，因为可以添加服务器和机器来容纳不断增长的数据量[2]。Spark 不提供分布式文件存储系统，所以它主要用于计算，在 Hadoop 之上。Spark 不需要 Hadoop 来运行，但可以与 Hadoop 一起使用，因为它可以从存储在 HDFS 中的文件创建分布式数据集[1]。

# 性能差异

Hadoop 和 Spark 的一个关键区别是性能。加州大学伯克利分校的研究人员意识到 Hadoop 非常适合批处理，但对于迭代处理来说效率很低，所以他们创建了 Spark 来解决这个问题[1]。 **Spark 程序在内存中迭代运行速度比 Hadoop 快 100 倍左右，在磁盘上快 10 倍[3]** 。Spark 的内存处理负责 Spark 的速度。相反，Hadoop MapReduce 将数据写入磁盘，在下一次迭代中读取。因为每次迭代后都要从磁盘重新加载数据，所以它比 Spark [7]慢得多。

# **阿帕奇 Spark RDDs**

Apache Spark 的基本数据结构是弹性分布式数据集(RDD ),它是一个容错的、不可变的分布式对象集合，可以跨集群并行处理。rdd 由于其容错能力而具有弹性，这意味着它可以从节点故障中恢复。如果一个节点未能完成其任务，RDD 可以在其余节点上自动重建，以完成任务[6]。rdd 的不变性有助于防止和避免数据不一致，即使在对非结构化数据执行转换时也是如此[9]。大数据通常是可变的，可以包括文本数据、关系数据库、视频、音频、照片和图形数据[2]。RDD 使高效处理结构化和非结构化数据成为可能。构建在 rdd 之上的数据帧也是不可变的、分布式的数据集合，但是被组织成列和行，类似于关系数据库[5]。Spark 数据框架使得处理和分析来自各种来源和格式的数据变得更加容易，比如 JSON 文件和 Hive 表[1]。

# **火花流**

Apache Spark 的堆栈包括多个大数据处理框架，如 Spark Streaming。Spark 流框架用于流处理容错的实时数据流，以处理大数据的速度[8]。在接收到实时输入数据后，Spark Streaming 将数据划分为多个批次，以便对数据进行批量处理，最终结果是一个经过处理的批次流[5]。火花流数据被称为离散流(数据流)，实质上是 rdd 序列[6]。数据流是一致的和容错的。由于能够实时或接近实时地处理大数据，因此大数据对企业和决策者来说非常有价值。Spark Streaming 的一个使用案例是一家投资公司需要监控可能影响其投资和持股的社交媒体趋势。及时了解关于公司客户或他们持有的公司的最新消息会极大地影响业务成果。

# **火花 MLlib**

Apache Spark 有一个名为 MLlib 的机器学习库，它提供了主要的机器学习算法，如分类、聚类、回归、降维、转换器和协同过滤[7]。一些机器学习算法可以应用于流数据，这对于使用 Spark 流的应用很有用[6]。Spark 流应用中使用的分类算法的一个例子是信用卡欺诈检测。一旦处理了新的传入交易，分类算法就会对交易是否属于欺诈进行分类。尽快识别潜在的欺诈交易非常重要，这样银行就可以尽快联系客户。Spark 流的挑战在于输入数据量的不可预测性[8]。传入数据在流的生命周期中的不一致分布会使机器学习模型的流学习变得相当困难。

# **推荐系统**

Spark Streaming 和 MLlib 的一个流行的真实用途是构建基于机器学习的推荐系统。推荐系统已经影响了我们网上购物、看电影、听歌和阅读新闻的方式。最著名的推荐系统公司包括网飞、YouTube、亚马逊、Spotify 和谷歌。像这样的大公司，产生和收集的数据如此之多，大数据分析是他们的首要任务之一。MLlib 包括一个迭代协作过滤算法，称为交替最小二乘法来构建推荐系统[6]。协同过滤系统基于用户和项目之间的相似性度量来进行推荐[8]。相似的商品会推荐给相似的用户。系统不知道关于项目的细节，只知道相似的用户查看或选择了相同的项目。

# **阿帕奇看象人**

MapReduce 曾经有过自己的机器学习库，然而，由于 MapReduce 对于迭代处理的效率很低，它很快就失去了与 Apache Spark 的库的兼容性。Apache Mahout 是建立在 Apache Hadoop 之上的机器学习库，它最初是一个 MapReduce 包，用于运行机器学习算法。由于机器学习算法是迭代的，MapReduce 遇到了可扩展性和迭代处理问题[6]。由于这些问题，Apache Mahout 停止支持基于 MapReduce 的算法，并开始支持其他平台，如 Apache Spark。

# **结论**

从一开始，Apache Spark 就被开发得很快，并解决了 Apache Hadoop 的缺点。Apache Spark 不仅速度更快，而且使用内存处理，并在其上构建了许多库，以适应大数据分析和机器学习。尽管 Hadoop 有缺点，但 Spark 和 Hadoop 都在大数据分析中发挥了重要作用，并被世界各地的大型科技公司用来为客户或客户量身定制用户体验。

# **作品被引用**

[1]安卡姆，文卡特。大数据分析。Packt 出版公司，2016 年。EBSCOhost，search . EBSCOhost . com/log in . aspx direct = true & db = nle bk & AN = 1364660 & site = eho ST-live。

[2]“阿帕奇 Hadoop。”hadoop.apache.org/.*阿帕奇 Hadoop*

[3]“阿帕奇火花。”【阿帕奇火花】*https://spark.apache.org/*。​

[4]巴拉金斯卡、玛格达和丹·苏休。*并行数据库和 MapReduce。【2016 年春季，http://pages . cs . wisc . edu/~ Paris/cs 838-s16/lecture-notes/lecture-parallel . pdf。PowerPoint 演示文稿。*

[5]德拉巴斯、托马兹等人*学习 PySpark* 。Packt 出版公司，2017 年。,search.ebscohost.com/login.aspx？direct = true&db = nle bk&AN = 1477650&site = ehost-live。

[6] Dua，Rajdeep 等*用 Spark 进行机器学习—第二版*。Vol .第二版，Packt 出版社，2017 年。 *EBSCOhost* ，search.ebscohost.com/login.aspxdirect=true&db = nle bk&AN = 1513368&site = eho ST-live。

[7] Hari，Sindhuja。" Hadoop vs Spark:面对面的比较选择正确的框架."2019 年 12 月 17 日，https://hackr.io/blog/hadoop-vs-spark。

8 佛朗哥·加莱亚诺·曼努埃尔·伊格纳西奥。*用 Apache Spark 处理大数据:用 Spark 和 Python 高效处理大数据集和大数据分析*。Packt，2018。

[9]普苏库里，纪梭。CS5412 /第 25 讲阿帕奇 Spark 和 RDDs。2019 年春季，http://www . cs . Cornell . edu/courses/cs 5412/2019 sp/slides/Lecture-25 . pdf。PowerPoint 演示文稿。

10 弗朗切斯科·皮尔费德里西。*用 Python 实现分布式计算*。Packt 出版公司，2016 年。search.ebscohost.com/login.aspx?，EBSCOhostdirect = true&db = nle bk&AN = 1220461&site = ehost-live。

[11]“什么是大数据。”*大数据联盟*，[https://www.bigdata-alliance.org/what-is-big-data/.](https://www.bigdata-alliance.org/what-is-big-data/.)