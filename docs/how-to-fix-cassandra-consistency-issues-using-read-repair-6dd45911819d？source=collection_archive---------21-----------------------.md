# 如何使用读取修复修复 Cassandra 一致性问题

> 原文：<https://towardsdatascience.com/how-to-fix-cassandra-consistency-issues-using-read-repair-6dd45911819d?source=collection_archive---------21----------------------->

![](img/7c9db59e61d6d1cd5879a93c91dd9ab8.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Louis Hansel @shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) 拍摄的照片

# 为什么 CAP 定理中存在一致性问题或 C

很多人可能都知道，Cassandra 是 AP 大数据存储。换句话说，当网络分区发生时，Cassandra 仍然可用，并放松了一致性属性。人们总是说它最终是一致的，或者换句话说，它将在未来的某个时间点上是一致的。

需要知道的不太明显的重要事项是:

*   **集群**变**经常不一致**。当然，有许多因素会影响群集的稳定性，例如适当的配置、专用资源、生产负载、运营人员的专业水平等，但事实是，节点不时出现故障从而导致数据变得不一致的可能性非常高。
*   该集群不会自动**或**再次变得一致**。**这与上帝对现代成熟分布式系统的感觉背道而驰。除非您有企业版的 Datastax 并启用了 DSE v6 的最新功能，否则您必须手动修复不一致问题**。**

# 修复不一致的方法

幸运的是，有一些方法可以解决不一致的问题。这里有几个选项:

*   **nodetool 维修工具**。这可能是要使用的主要和默认方法。在所有表或特定表关闭的节点上运行命令。不过有一点需要注意:当您运行命令时，所有的节点都应该处于运行状态。
*   **阅读修复**卡珊德拉特写。这是一个重要的特征，意味着在读取请求期间，集群有机体正在自我修复，更准确地说，它修复正确的数据副本。如果读取请求中涉及的复制副本不一致，则会再次对齐它们

# 何时及为何阅读修复

如上所述，修复不一致的默认和主要方法是 nodetool 修复工具，因此自然的问题是何时以及为什么使用 read 修复方法。让我用我的一个项目的经验来回答这个问题。

在某个时候，我们进入了一个时期，此时我们的 Cassandra 集群开始变得非常不稳定，并且花费了大量的时间，直到所有节点再次返回到运行状态。这导致了两个主要后果:

*   数据不一致
*   在此期间，无法使用 nodetool 修复工具来修复不一致

在这种情况下，我们能做的最好的事情就是使用读取修复功能来确保至少在大多数复制副本中，在仲裁级别，数据是一致的，因此对于使用仲裁一致性级别的所有读取，数据都是一致的和最新的

# 如何使用读取修复

读修复特性只修复读操作所涉及的记录的一致性，那么如何修复整个表呢？

可以使用以下方法:

*   选择一个最窄的列进行阅读
*   使用 copy 命令读取整个表，将数据导出到文件中

因此，让我们考虑在一个 keyspace“test”中有一个 Cassandra 表“event”，其中一个最窄的列称为“id”；复制命令如下所示:

```
cqlsh -e "consistency QUORUM; copy test.event(fid) to '/tmp/tid'"
```

或者，您可以读取整个记录并将它们发送到'/dev/null ':

```
cqlsh -e "consistency QUORUM; copy test.event to '/dev/null'"
```

当然，当所有节点都启动时，您可以使用 consistency ALL，但在这种情况下，最好使用 nodetool 修复工具，如下所示:

```
nodetool repair test event
```

# 结论

Cassandra 是伟大的大数据存储，但为了充分利用它，它需要很好地理解主要原则，它是如何工作的，就像任何美丽的东西一样，它需要一些小心:)