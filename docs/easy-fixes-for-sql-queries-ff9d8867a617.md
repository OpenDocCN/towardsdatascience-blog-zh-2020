# SQL 查询的简单修复

> 原文：<https://towardsdatascience.com/easy-fixes-for-sql-queries-ff9d8867a617?source=collection_archive---------18----------------------->

![](img/45a6722933091f23598be85f845356e5.png)

由[思想目录](https://unsplash.com/@thoughtcatalog?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/improvement?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片。不是 101 篇文章，但是这里有一些提示和想法将会改变你写 SQL 的方式。他们可能不会改变你的想法。为了改变你的思维方式，你应该多读书，与真实的人交谈，并确定你需要改变你的思维方式。我可以一直唠叨下去。如果你能读一读这篇关于 SQL 的该死的文章，那不是很好吗！我已经花了将近十年的时间学习 SQL，并且仍然很强大。

## 更好的 SQL

## 查询任何传统关系数据库的经验法则

SQL 对于任何角色来说都是一个强大的工具。数据驱动着公司的所有决策。数据只使用一种语言，那就是 SQL。写 SQL(或者说，如果你是个书呆子的话)很简单。大多数人都能做到。只有一些人做得很好。这个会超级短超级有用。

不管您使用的是什么关系数据库管理系统或 RDBMS (MySQL、PostgreSQL、SQL Server、Oracle ),一些经验法则都适用于所有这些。以下是在你的数据库上做`SELECT`的规则

# 1.总是使用 [WHERE](https://use-the-index-luke.com/sql/where-clause) 子句(总是使用索引)

换句话说，只获取您绝对需要的数据——这可以通过使用古老的`WHERE`子句来完成。只有在极少数情况下，您会发现不使用索引可以获得更好的性能。不要担心罕见的情况。索引强制数据做二分搜索法(尽管不同数据库的二分搜索法实现不同)。本质上，你将能够在对数时间而不是线性时间内`SELECT`你的数据。

> 只获取你绝对需要的数据

例如，对一个有 10 亿条记录的表进行查询 ***而不使用*** 索引，可能需要多达 10 亿次才能得到您想要的记录。然而，使用 索引对您正在搜索的列进行查询 ***将花费更少的次数，并且将更快地获得结果，因为 10 亿的对数以 2 为底大约是 30(准确地说，是 29.897352854)。这是对搜索过程的过度简化。***

*感谢纠正日志上以 2 为基数的值不正确* [*普拉莫诺·温纳塔*](https://medium.com/u/2236cf1a63ba?source=post_page-----ff9d8867a617--------------------------------) *。*

# 2.确保使用了[索引](https://dataschool.com/sql-optimization/how-indexing-works/)

当查询引擎无法使用索引时，可能会出现问题。这将导致查询速度缓慢。速度的快慢取决于查询所扫描的数据量。当查询引擎无法理解或映射带有表数据定义的搜索值的字符集(在`WHERE`子句或`JOIN`中)时，通常会出现问题。在这种情况下，显式类型转换通常会有所帮助。

> 不使用索引时，使用显式强制转换

来自 MySQL 文档

> *对于字符串列与数字的比较，MySQL 不能使用列上的索引来快速查找值。避免这类问题的一个方法是使用* `[*CAST()*](https://dev.mysql.com/doc/refman/5.5/en/cast-functions.html#function_cast)` *。*

所有的数据库都非常相似。问题的严重性取决于任何数据库中查询引擎的实现。

# 3.明智地使用[子查询](https://dataschool.com/how-to-teach-people-sql/how-sql-subqueries-work/)

在许多数据库中，使用子查询会强制将数据复制到磁盘，并且对您的[查询数据的计算将发生在磁盘](https://www.percona.com/blog/2007/08/16/how-much-overhead-is-caused-by-on-disk-temporary-tables/)上(数据将被一次又一次地读取和写入磁盘)，而不是内存中。如果发生这种情况，如果数据很大，它会成为一个严重的开销。当这种情况发生时,[延迟会成为性能杀手。查看由格蕾丝·赫柏上将制作的关于一纳秒意味着什么的惊人视频。延迟通常以纳秒为单位。](http://norvig.com/21-days.html#answers)

> 当心子查询和磁盘的使用

事实上，明智地使用任何在磁盘上创建(强制创建)临时表的方法—

*   `GROUP BY`和`ORDER BY` —是的，这也会导致数据子集被写入磁盘。在所有数据库中，您可以预定义一块内存(RAM)用于写入查询数据。如果您的数据集很大，并且您的查询数据的子集超过了这个限制，那么它肯定会被写入磁盘并从那里进行管理。
*   `JOINS` —是的，即使是`JOINS`也可以将数据写入磁盘，以便进一步检索。一个`JOIN`本质上是一个`SEARCH`和`MAP`操作。

# 4.避免[排序/分类](https://use-the-index-luke.com/sql/sorting-grouping)数据

排序操作成本极高。每个数据库都有自己的排序算法实现。他们可能使用了传统的排序算法，也可能想出了自己的排序算法。在任何情况下，由于这是一个非常耗费资源的操作，而且还涉及到将数据推入磁盘和从磁盘中取出数据，因此应该谨慎地使用排序。

# 5.不要使用`[LIKE](https://mode.com/sql-tutorial/sql-like/)`查询

当您长时间查询数据时，传统的关系数据库会导致争用问题。这正是你做`LIKE`查询时发生的事情。它们会影响整个数据库和其他正在运行的查询的性能。除了使用`LIKE`，还有其他选择

*   关系数据库的全文功能。现在几乎所有的数据库都支持全文搜索。
*   如果全文搜索不是一个选项，你可以使用像 Elasticsearch，Solr 或 Algolia 这样的全文搜索引擎。它们是为文本分析而设计的。虽然，对于这些新系统，你没有一个像传统数据库那样强大的 SQL 接口。

> 使用关系数据库的全文功能，或者使用类似 Elasticsearch 的工具进行文本搜索

# 如果你想成为 SQL 超人

*   了解关系数据库的内部结构——数据库如何使用内存和磁盘
*   了解查询的执行顺序——首先评估查询的哪个子句。是`JOIN`先发生还是`WHERE`从句先发生还是`GROUP BY`先发生
*   了解查询引擎如何准确解析、估计、优化、重写和执行查询的底层实现细节。
*   了解基于云的关系数据库实现，如 Amazon Aurora、Amazon RDS 和 Azure SQL 数据库等等。

[这并不是一份详尽的清单](https://linktr.ee/kovid)。还有更多关于`DISTINCT`和`OR`以及`EXISTS`和`IN`子句的用法，但我选择的这些似乎是最有问题的，也是最容易修复的。这些只是编写和重写查询时应该记住的一些通用规则。