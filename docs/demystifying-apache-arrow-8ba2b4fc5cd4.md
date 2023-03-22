# 揭开阿帕奇箭的神秘面纱

> 原文：<https://towardsdatascience.com/demystifying-apache-arrow-8ba2b4fc5cd4?source=collection_archive---------29----------------------->

## 了解关于一种工具的更多信息，该工具可以在两秒钟内在您的笔记本电脑上过滤和聚合 20 亿行

在我作为数据科学家的工作中，我在一系列看似不相关的情况下遇到过 Apache Arrow。然而，我总是很难准确地描述它是什么，它做什么。

Arrow 的官方描述是:

> 用于内存分析的跨语言开发平台

这很抽象——而且有充分的理由。该项目雄心勃勃，旨在为大范围的数据处理任务提供主干。这意味着它位于较低的级别，为更高级的、面向用户的分析工具(如 pandas 或 dplyr)提供构建模块。

因此，对于那些在日常工作中偶尔遇到这个项目的用户来说，这个项目的重要性可能很难理解，因为它的大部分工作都是在幕后进行的。

在这篇文章中，我描述了我在工作中遇到的 Apache Arrow 的一些面向用户的特性，并解释了为什么它们都是 Apache Arrow 旨在解决的更基本问题的各个方面。

通过连接这些点，可以清楚地了解为什么 Arrow 不仅是解决当今一些实际问题的有用工具，而且是最令人兴奋的新兴工具之一，有可能成为未来数据科学工作流的大部分背后的引擎。

## 更快的 csv 读取

Arrow 的一个显著特点是，它可以将 CSV 读入熊猫图[，](https://youtu.be/fyj4FyH3XdU?t=1030)比`pandas.read.csv`快 10 倍以上。

这实际上是一个两步过程:Arrow 将数据读入内存中的 Arrow 表，该表实际上只是记录批次的集合，然后将 Arrow 表转换为 pandas 数据帧。

因此，加速是 Arrow 底层设计的结果:

*   Arrow 有自己的内存存储格式。当我们使用 Arrow 将数据加载到 pandas 时，我们实际上是将数据加载到 Arrow 格式(数据帧的内存格式)，然后将其转换为 Pandas 内存格式。因此，读取 CSV 的部分加速来自于箭头列格式本身的精心设计。
*   箭头中的数据以记录批的形式存储在内存中，记录批是一种 2D 数据结构，包含等长的连续数据列。可以从这些批处理中创建一个'[表](http://arrow.apache.org/docs/python/data.html#tables)，而不需要额外的内存复制，因为表可以有“分块”列(即数据段，每个部分代表一个连续的内存块)。这种设计意味着可以并行读取数据，而不是 pandas 的单线程方法。

## PySpark 中更快的用户定义函数(UDF)

在 Apache Spark 中运行 Python 用户定义函数一直非常慢——慢到通常建议不要在任何大规模的数据集上运行。

最近，Apache Arrow 使得在 JVM 和 Python 进程之间高效地传输数据成为可能。结合矢量化 UDF，这导致了[巨大的加速](https://spark.apache.org/docs/latest/sql-pyspark-pandas-with-arrow.html#apache-arrow-in-pyspark)。

这是怎么回事？

在引入 Arrow 之前，将数据从 Java 表示转换为 Python 表示的过程非常缓慢——包括序列化和解序列化。它也是一次一行，速度较慢，因为许多优化的数据处理操作对以列格式保存的数据最有效，每列中的数据保存在内存的连续部分中。

Arrow 的使用几乎完全消除了序列化和解序列化步骤，还允许以列批处理方式处理数据，这意味着可以使用更高效的矢量化算法。

Arrow 能够在不同语言之间传输数据，而不会产生很大的序列化/反序列化开销，这是一个关键特性，使得用一种语言实现的算法更容易被使用其他语言的数据科学家使用。

## 将文件夹中的文件作为单个表格读取

Arrow 能够将包含许多数据文件的文件夹读入单个数据帧。Arrow 还支持读取数据被[划分到子文件夹](https://blog.cloudera.com/improving-query-performance-using-partitioning-in-apache-hive/)的文件夹。

此功能说明了记录批次周围的箭头的高级设计。如果所有东西都是一个记录批，那么源数据是存储在一个文件中还是多个文件中就无关紧要了。

## 读取拼花文件

Arrow 可用于将 parquet 文件读入 Python 和 R 等数据科学工具，正确表示目标工具中的数据类型。由于 parquet 是一种自描述格式，并且在模式中指定了列的数据类型，因此获得正确的数据类型似乎并不困难。但是仍然需要一个翻译的过程来将数据加载到不同的数据科学工具中，这是令人惊讶的复杂。

例如，在历史上，pandas 整数类型不允许整数为空，尽管在 parquet 文件中这是可能的。类似的怪癖也适用于其他语言。Arrow 为我们处理这个翻译过程。数据总是首先被加载到 Arrow 格式中，但是 Arrow 提供了翻译器，然后能够将其转换成特定于语言的内存格式，比如 pandas dataframe。这意味着对于每个目标格式，需要一个格式到箭头的翻译器，这是对当前需要“n 取 2”个翻译器(每对工具一个)的巨大改进。

Arrow 的目标是最终完全消除这个翻译问题:理想情况下，数据帧应该有单一的内存格式，而不是每个工具都有自己的表示形式。

## 编写拼花文件

Arrow 可以很容易地将 pandas 和 R 等工具中保存在内存中的数据以 Parquet 格式写入磁盘。

为什么不直接将数据以 Arrow 格式保存到磁盘上，从而拥有一个在磁盘上和内存中相同的单一的跨语言数据格式呢？一个最大的原因是，Parquet 通常产生较小的数据文件，如果您是 IO 绑定的，这是更可取的。如果你从像 AWS S3 这样的云存储中加载数据，情况尤其如此。

Julien LeDem 在一篇讨论两种[格式](https://www.kdnuggets.com/2017/02/apache-arrow-parquet-columnar-data.html)的博客文章中进一步解释了这一点:

> 列数据和内存中数据的权衡是不同的。对于磁盘上的数据，通常 IO 主导延迟，这可以通过以 CPU 为代价的激进压缩来解决。在内存中，访问速度要快得多，我们希望通过关注缓存局部性、流水线和 SIMD 指令来优化 CPU 吞吐量。

这使得在内存和磁盘格式的开发中密切合作，在两种表示之间进行可预测和快速的转换是可取的——这正是 Arrow 和 Parquet 格式所提供的。

# 这些想法是如何联系在一起的？

虽然非常有用，但迄今为止讨论的面向用户的特性并不是革命性的。真正的力量在于底层构建模块的潜力，它可以实现数据科学工具的新方法。目前(2020 年秋季)，Arrow 背后的团队仍在进行一些工作。

## 记录批次和分块

在 Apache Arrow 中，数据帧由记录批次组成。如果存储在磁盘上的 Parquet 文件中，这些批处理可以非常快地读入内存中的 Arrow 格式。

我们已经看到这可以用来将 csv 文件读入内存。但更广泛地说，这一概念打开了整个数据科学工作流的大门，这些工作流是并行的，并对记录批次进行操作，从而消除了将整个表存储在内存中的需要。

## 数据帧的一种常见(跨语言)内存表示

目前，数据科学工具及其数据帧的内存表示的紧密耦合意味着用一种语言编写的算法不容易移植到其他语言。这意味着相同的标准操作，如过滤器和聚合，会被无休止地重写，优化不容易在工具之间转换。

Arrow 提供了在内存中表示数据帧的标准，以及允许多种语言和引用相同内存数据的机制。

这[为](https://docs.google.com/document/d/10RoUZmiMQRi_J1FcPeVAUAMJ6d_ZuiEbaM2Y33sNPu4/edit)的发展创造了机会

C++中的一个原生箭头列查询执行引擎，不仅用于 C++中，也用于 Python、R 和 Ruby 等用户空间语言中。

这将提供一个单一的、高度优化的代码库，可用于多种语言。对维护和贡献这个库感兴趣的开发人员将会更多，因为它可以在各种工具上使用。

# 伟大的想法

这些想法在 Arrow 作为“数据 API”的描述中结合在一起。其思想是 Arrow 提供了一种跨语言的内存数据格式，以及一种相关的查询执行语言，为分析库提供了构建块。像任何好的 API 一样，Arrow 为常见问题提供了一个高性能的解决方案，用户不需要完全理解实现。

借助分析库，这为数据处理能力的重大变革铺平了道路:

*   默认情况下被并行化
*   在数据帧上应用高度优化的计算
*   不再需要处理整个数据集必须适合内存的约束

同时通过在单台机器上或通过网络在工具之间更快地传输数据，打破工具之间共享数据的障碍。

我们可以在 R 中 Arrow 包的[插图中一窥这种方法的潜力，其中一个 37gb 的数据集有 20 亿行，在一台笔记本电脑上处理和聚合不到 2 秒钟。](https://arrow.apache.org/docs/r/articles/dataset.html)

这将现有的高级数据科学工具和语言(如 SQL、pandas 和 R)置于何处？

正如插图所示，Arrow 的目的不是直接与这些工具竞争。相反，更高级的工具可以使用箭头，对它们面向用户的 API 施加很少的约束。因此，它允许在不同工具的表达能力和特性方面继续创新，同时提高性能并为作者节省重新发明通用组件的任务。

# 进一步阅读

[https://arrow.apache.org/overview/](https://arrow.apache.org/overview/)
https://arrow.apache.org/use_cases/
https://arrow.apache.org/powered_by/
[https://arrow.apache.org/blog/](https://arrow.apache.org/blog/)
[https://ursalabs.org/blog/](https://ursalabs.org/blog/)
[https://wesmckinney.com/archives.html](https://wesmckinney.com/archives.html)
[https://www.youtube.com/watch?v=fyj4FyH3XdU](https://www.youtube.com/watch?v=fyj4FyH3XdU)

箭头设计文档:

[文件系统 API](https://github.com/apache/arrow/pull/4225)
[数据集 API](https://docs.google.com/document/d/1bVhzifD38qDypnSjtf8exvpP3sSB5x_Kw9m-n66FB2c/edit?usp=sharing)
[数据框架 API](https://docs.google.com/document/d/1XHe_j87n2VHGzEbnLe786GHbbcbrzbjgG8D0IXWAeHg/edit#heading=h.g70gstc7jq4h)
[C++查询引擎](https://docs.google.com/document/d/10RoUZmiMQRi_J1FcPeVAUAMJ6d_ZuiEbaM2Y33sNPu4/edit?usp=sharing)