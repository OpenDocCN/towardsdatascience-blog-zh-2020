# 图形处理:一个没有明确胜利者的问题

> 原文：<https://towardsdatascience.com/graph-processing-a-problem-with-no-clear-victor-3683c483f5dc?source=collection_archive---------46----------------------->

## [意见](https://towardsdatascience.com/tagged/opinion)

## 你知道最流行的图形处理解决方案吗？没有吗？别担心。现在还没有。

![](img/71985cd9d03bb0ab764d56c0064d4a8d.png)

由[鲁伯特·布里顿](https://unsplash.com/@rupert_britton?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我们都依赖互联网来寻找技术问题的潜在解决方案。例如，对于大数据问题，在谷歌五分钟后，你会发现 Spark 可能会帮助你。即使你不知道什么是火花，你也会碰到这个名字。TensorFlow 在搜索深度学习解决方案时也会出现类似的情况，Kubernetes 用于云，Docker 用于容器… **似乎计算机科学中的每个流行词都有一个平台/框架/库**。但是，尝试寻找一个图形处理解决方案。你会发现没有**明确的胜利者**。我觉得这很令人惊讶。

2015 年，我和我在 Inria 的同事发表了一篇[文章](https://hal.inria.fr/hal-01111459/document)，提出了一种中间件，可以激励开发人员提供一个通用框架来实现分布式图形处理解决方案。我们有一种强烈的感觉，没有一个一致的建议来加速大规模图形处理解决方案的开发。如果我们考虑到 Graph500 基准测试在使用图形时存在一些计算密集型问题，这就令人惊讶了。脸书和 Twitter 诞生后社交网络的爆炸吸引了研究界的注意，并提出了计算和可扩展性方面的新问题。此外，使用图形作为底层数据结构存在大量问题。图形用于欺诈检测、博弈论和大量与数据相关的问题。

有一个庞大的图库生态系统。仅举几个可用的我有经验的，我们有: [GraphTool](https://graph-tool.skewed.de/) ， [SNAP](https://snap.stanford.edu/snap/) ， [igraph](https://igraph.org/) ， [NetworkX](https://networkx.github.io/) ， [BGL](https://www.boost.org/doc/libs/1_66_0/libs/graph/doc/) ， [network for R](https://cran.r-project.org/web/packages/network/network.pdf) 等。此外，我们还有其他更受欢迎的平台的扩展模块，如 Spark 的 GraphX、graphs 的 Hadoop 扩展、Neo4j，这是一个数据库，而不是库，尽管它可以用于图形操作。

然后，我们进入一个更模糊的世界，在某些方面声称是最有效的解决方案。研究机构和/或大学已经开展了大量的工作。然而，我不建议任何开发人员采用这些解决方案。纯粹。为什么？因为它们很难维护，没有记录，最终会消失。有一些臭名昭著的例子，如谷歌的 Pregel，它启发了 Apache Giraph，但从未公开发表过(如果我错了，请纠正我)。GraphLab 代码是可用的，慢慢消失，直到成为 [Turi](https://turi.com/) 的一部分。

如果您是一名开发人员，并且需要包括一些图形处理，请考虑以下几点:

*   你想使用的编程语言可能会减少你的选择。
*   **您是在寻找特定的算法，还是想要探索和/或实现自己的解决方案？第一种方法在某种程度上简化了搜索，因为您可以寻找性能最佳的算法实现。如果你想做自己的探索，看看下一点。**
*   你的图表有多大？这是极其重要的。我上面提到的大多数库在处理数百万个顶点和边时都会有问题。如果你的图形不适合内存，那就更糟了。

不幸的是，在写这篇文章的时候，**在图形处理解决方案的世界里，我们还没有一个明确的胜利者。公司使用的大多数解决方案都是从零开始量身定制的。开发人员一次又一次地编写相同的算法，导致了现有实现的混乱。我坚信有一个新的范例的空间(和需要),可以帮助开发人员建模分布式图算法，简化开发和可维护性，同时提高性能。类似于 TensorFlow 对神经网络所做的事情，通过抽象公共片段并将社区的努力集中到一个公共解决方案中。**

感谢阅读。