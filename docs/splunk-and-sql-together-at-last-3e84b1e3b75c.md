# Splunk 和 SQL，终于走到一起了？

> 原文：<https://towardsdatascience.com/splunk-and-sql-together-at-last-3e84b1e3b75c?source=collection_archive---------70----------------------->

![](img/8f739e07d2b9453930b749e3e4b74b9f.png)

[photo-1501743411739-de 52 ea 0 ce 6a 0](https://images.unsplash.com/photo-1501743411739-de52ea0ce6a0?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80)

过去几年，我一直参与 Splunk 工程。我觉得这有点讽刺，因为我已经很久没有用 Splunk 了。出于各种原因，我从来都不是 Splunk 查询语言(SPL)的粉丝，其中一个主要原因是我不想花时间去学习一种专有语言，这种语言就像 1974 年的[福特 Pinto](https://en.wikipedia.org/wiki/Ford_Pinto) 一样优雅。多年来，我参与了几个项目，涉及对 Splunk 中的数据进行机器学习。这是一个重大的挑战。虽然 Splunk 确实有一个机器学习工具包(MLTK)，它基本上是 scikit-learn 的包装器，但在 SPL 进行功能工程是一场噩梦。

# 那么，如何在 Splunk 中进行机器学习呢？

你不知道。

# 不，真的 Splunk 怎么做机器学习？

好吧…人们实际上在 Splunk 中进行机器学习，但我认为这不是最好的方法，原因有几个。我在 Splunk 中看到的大多数 ML 工作都涉及到从 Splunk 中获取数据。在这种情况下，我向任何试图在 Splunk 中进行 ML 的人推荐的第一件事是看看 huntlib(【https://github.com/target/huntlib】)这是一个 python 模块，有助于从 Splunk 中获取数据。Huntlib 使这变得相对容易，你可以将 Splunk 中的数据直接放入 Pandas 数据框架中。但是，你仍然必须知道 SPL，或者你做一个基本的 SPL 搜索，用 Python 做你所有的数据辩论。还有更好的方法吗？

# SQL/Splunk 连接器

我一直在做的一个项目是 Apache Drill 到 Splunk 的连接器，它允许用户使用 ANSI SQL 查询 Splunk。这种方法有几个优点:

*   **SQL:** 面对现实吧。SQL 可能是表达复杂查询的最佳语言之一。自 20 世纪 80 年代以来，它被广泛使用，并在无数系统中实现。从技能的角度来看，学习 SQL 比学习一种可能在几年内过时的专有查询语言更划算。总而言之，SQL 对于定义表来说也是极具表现力和高效的。我的想法是，如果你可以用 SQL 查询 Splunk，这意味着不懂 SQL 的数据科学家现在可以访问 Splunk 中的数据。
*   **查询优化:**当您开始使用 Splunk 时，您首先会了解到 Splunk 不会优化查询。这意味着这个任务落在了作为查询作者的您身上。这里的问题是，这需要您了解 Splunk 的实际工作方式。这与大多数为您做这种优化的 SQL 数据库相反。此外，许多 SQL 引擎可以提供如何改进查询的建议。因此，Drill/SQL 连接器将为您执行这些优化，因此您不必学习如何优化 Splunk 查询。
*   **外部数据集:**Splunk 的一个大问题是，当您的一些数据在 Splunk 中，而另一些不在时，该怎么办。现在，Splunk 的人会说，“简单..放入 Splunk。问题解决了。”但在企业中，就没这么简单了。在企业中，用户很可能永远没有权限这样做，用户必须让工程团队参与进来，才能将外部数据导入 Splunk。由于 Splunk 按接收的数据量收费，企业不能让用户将随机的、未经验证的数据放入生产系统，因为这会影响预算。这也需要时间。据我观察，企业通常需要数周时间才能将数据源导入 Splunk。借助 Drill 和 SQL，您可以通过简单的 SQL JOIN 语句轻松地将外部数据集与 Splunk 中的数据连接起来。

# 这一切和机器学习有什么关系？

任何 ML 项目的第一阶段都是收集数据和提取特征。完成后，您就可以研究数据了。Splunk 非常擅长数据探索，但是在我看来，使用 Splunk 提取特征是一场噩梦。也有很多效率低下的地方，但我们会把它留到下次再讲。无论如何，当你把 Splunk/Drill 和 [John Omernik 的棒极了的 Jupyter 笔记本集成用于 Drill](https://github.com/JohnOmernik/jupyter_drill) (我将在以后的博客文章中讨论)时，你可以简单地:

*   在 Jupyter 笔记本单元格中键入 SQL 查询
*   集成将查询钻取和管道您的数据直接进入一个数据框架！

搞定了。你要去比赛了！

# Drill/Splunk 连接器如何工作？

在 pull request 中可以获得[完整的文档，但是它相当简单。您需要配置 Drill 安装以连接到 Splunk。Splunk 使用端口 8089 进行编程访问，因此该端口必须打开。总之，要配置 Drill，只需导航到存储插件页面，单击添加新插件，将其命名为 splunk，然后粘贴到以下配置中:](https://github.com/apache/drill/pull/2089)

```
{ 
   "type":"splunk", 
   "username": "admin", 
   "password": "changeme", 
   "hostname": "localhost", 
   "port": 8089, 
   "earliestTime": "-14d", 
   "latestTime": "now", 
   "enabled": false 
}
```

单击提交后，您现在应该能够通过钻取来查询 Splunk。

# Drill/Splunk 数据模型

Drill 将 Splunk 索引视为表。Splunk 的访问模式似乎不会限制对目录的访问，但会限制对实际数据的访问。因此，您可能会看到您无权访问的索引的名称。您可以通过`SHOW TABLES IN splunk`查询查看可用索引的列表。

```
apache drill> SHOW TABLES IN splunk; 
+--------------+----------------+ 
| TABLE_SCHEMA | TABLE_NAME     | 
+--------------+----------------+ 
| splunk       | summary        | 
| splunk       | splunklogger   |
| splunk       | _thefishbucket | 
| splunk       | _audit         | 
| splunk       | _internal      | 
| splunk       | _introspection |
| splunk       | main           | 
| splunk.      | history        | 
| splunk       | _telemetry     | 
+--------------+----------------+ 
9 rows selected (0.304 seconds)
```

要从 Drill 查询 Splunk，只需使用以下格式:

```
SELECT <fields> FROM splunk.<index>
```

Drill 将执行查询，并在一个漂亮干净的表中返回结果！

# 限制您的查询

当您学习通过 Splunk 的界面查询 Splunk 时，您首先要学习的是绑定您的查询，以便它们查看尽可能最短的时间跨度。当使用 Drill 来查询 Splunk 时，建议执行相同的操作，Drill 提供了两种方法来完成此操作:通过配置和在查询时。

# 在查询时绑定查询

绑定查询最简单的方法是在 querytime 通过`WHERE`子句中的特殊过滤器来完成。有两个特殊的字段，`earliestTime`和`latestTime`，可以设置它们来绑定查询。如果没有设置它们，查询将被绑定到配置中设置的默认值。

您可以在此处使用 Splunk 文档中指定的任何时间格式:
[https://docs . Splunk . com/Documentation/Splunk/8 . 0 . 3/search reference/search time modifiers](https://docs.splunk.com/Documentation/Splunk/8.0.3/SearchReference/SearchTimeModifiers)

因此，如果您想查看过去 15 分钟的数据，可以执行以下查询:

```
SELECT <fields> 
FROM splunk.<index> 
WHERE earliestTime='-15m' AND latestTime='now'
```

查询中设置的变量会覆盖配置中的默认值。

# 向 Splunk 发送任意 SPL

您可能有一个 Splunk 查询没有映射到该模型，对于这些情况，有一个名为`spl`的特殊表，您可以使用它向 Splunk 发送任意 SPL 查询。如果使用该表，必须在`spl`过滤器中包含一个查询，如下所示:

```
SELECT * 
FROM splunk.spl 
WHERE spl='<your SPL query>'
```

这在很大程度上是一项正在进行的工作，所以如果你尝试这个，我将欢迎反馈它如何为你工作。文档解释了如何处理来自 Splunk 的嵌套数据等。祝你好运！

*原载于 2020 年 7 月 24 日 https://thedataist.com*[](https://thedataist.com/splunk-and-sql-together-at-last/)**。**