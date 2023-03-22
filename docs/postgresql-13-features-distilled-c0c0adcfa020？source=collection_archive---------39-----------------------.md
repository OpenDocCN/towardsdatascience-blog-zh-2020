# PostgreSQL 13 特性精华

> 原文：<https://towardsdatascience.com/postgresql-13-features-distilled-c0c0adcfa020?source=collection_archive---------39----------------------->

![](img/8dc4c92bd0341ac6834beae79db80c47.png)

[南安](https://unsplash.com/@bepnamanh?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/elephant?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

## 数据工程

## PostgreSQL 最新版本中的索引优化、增量排序、并行清空和更多最新特性

J 就在几周前，[我写了关于 MySQL 8.0 在 Google Cloud](https://linktr.ee/kovid) 上正式发布，在所有主要云平台上完成它的旅程。MySQL 8.0 已经问世一段时间了。开源数据库社区急切地等待 PostgreSQL 的主发布版本 13。PostgreSQL 是仅次于 MySQL 的第二大流行的开源数据库，由于其固有的可扩展特性，它在所有地区和行业都被越来越多的人采用。随着 AWS 红移走上 PostgreSQL 路线，PostGIS 中的世界级地理空间支持和令人惊叹的核心功能集，PostgreSQL 几乎不是一个可以忽略的数据库。

一周前，9 月 24 日，PostgreSQL 13 发布了。这个新版本有很多特性，但我们只介绍了 PostgreSQL 官方版本中列出的一些主要特性。

# 平行分度真空

清空是一个缓慢而乏味的过程，但与其他数据库不同，PostgreSQL 使用它来回收磁盘上任何未使用的空间。当 vacuum 在任何给定的表上运行时，在该表上创建的索引也会被清空。PostgreSQL 13 引入了一个特性，可以并行清空一个表上的多个索引。

可以使用名为`**max_parallel_maintenance_workers**`的数据库变量设置并行化的程度。这应该设置为表拥有的索引数。

这一点，再加上[许多其他索引改进](https://www.percona.com/blog/2020/09/10/index-improvements-in-postgresql-13/)，使得这个版本的 PostgreSQL 13 更有吸引力。[也许优步终究会考虑搬回 PostgreSQL，开个玩笑](https://eng.uber.com/postgres-to-mysql-migration/)！

注:请注意，autovacuum 尚不支持此功能。这里列出了对 autovacuum 的一些其他改进。

# 为查询优化器和规划器提供更好的统计数据

我们所知道的关于优化器的一件事是，如果他们有更新和更好的关于表、索引、分区等的统计数据，他们会优化、计划和重写更好的查询。几年前，[随着 PostgreSQL 10 的发布，它引入了用户定义的自定义统计数据的概念，名为扩展统计数据](https://pganalyze.com/blog/postgres13-better-performance-monitoring-usability#extended-statistics-improvements-in-postgres-13)。使用扩展的统计数据，用户可以创建自己的统计数据来定义高级关系和依赖关系，以捕获行为统计数据并将其提供给优化器。

在 PostgreSQL 13 中，我们看到了更多的改进。以前，规划器只能使用一组定制的统计数据来规划和优化查询。用户可以通过`OR`、`IN`或`ANY`子句使用多个统计数据。

# 增量排序

在这个主要版本之前，如果您在一个表中有一个针对`(column_1, column_2)`的索引，它将自己具体化为一个 B 树，并且您发出一个使用`(column_1, column_2, column_3)`排序的查询，PostgreSQL 不会使用现有的索引，该索引已经对 where 子句中的前两列的数据进行了排序。

在这个新版本中，这个问题已经得到了解决。现在，PostgreSQL 不会对已经为列`column_1`和`column_2`排序的数据重新排序。在亚历山大·库兹曼科夫[的演讲中了解更多。](https://www.postgresql.eu/events/pgconfeu2018/sessions/session/2124/slides/122/Towards%20more%20efficient%20query%20plans%20(2).pdf)

# 使用重复数据删除的 b 树成本优化

考虑一个您想要索引的表列，但是它有许多重复的值。在该列上创建索引时，所有值都绘制在索引的 B 树结构上。很多重复出现在你的索引中。这导致索引很大，并且索引的处理时间也很长。

> ***主键或唯一索引*** *显然不会发生这种情况——请参考*[*Andrew Kozin*](https://medium.com/u/2790f889fe05?source=post_page-----c0c0adcfa020--------------------------------)*对这篇帖子的评论，了解为什么这句话不成立。*

非唯一列上的索引对于聚合查询非常重要。为了从这些查询和数据库中获得更好的性能，PostgreSQL 13 引入了一个新的重复值删除过程。PostgreSQL 现在只保存对列值的引用，而不是存储所有重复的值。

重复数据删除过程显然节省了大量空间，因为这是一个常见问题。节省空间的同时，也节省了在更大的索引上花费的额外处理时间。在这里了解更多关于 B 树实现的信息

[](https://www.postgresql.org/docs/13/btree-implementation.html#BTREE-DEDUPLICATION) [## PostgreSQL:文档:13: 63.4。履行

### 本节涵盖了可能对高级用户有用的 B 树索引实现细节。看…

www.postgresql.org](https://www.postgresql.org/docs/13/btree-implementation.html#BTREE-DEDUPLICATION) 

# 聚合的性能优化

PostgreSQL 10 引入的另一个特性是散列聚合。Postgres 在聚合时有两个选项——散列或组。如果哈希表可以放在内存中，则使用哈希聚合。[在 PostgreSQL 13](https://info.crunchydata.com/blog/why-postgresql-13-is-a-lucky-release) 之前，如果哈希表不适合内存，PostgreSQL 只会选择组聚合。但是现在，在这个版本中，如果哈希表不适合内存，它将溢出到磁盘。

对于那些已经在使用 PostgreSQL 的人来说，如果你打算[迁移到 PostgreSQL 13](https://www.percona.com/blog/2020/07/28/migrating-to-postgresql-version-13-incompatibilities-you-should-be-aware-of/) ，这里有一篇关于 Percona 博客的好文章。