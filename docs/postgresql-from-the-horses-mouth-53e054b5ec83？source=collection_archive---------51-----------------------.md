# PostgreSQL —出自马嘴

> 原文：<https://towardsdatascience.com/postgresql-from-the-horses-mouth-53e054b5ec83?source=collection_archive---------51----------------------->

![](img/8af74115bc6400cf3cff8788d5632631.png)

在 [Unsplash](https://unsplash.com/s/photos/education?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由[伊尼基·德尔·奥尔莫](https://unsplash.com/@inakihxz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片

## 阅读材料汇编 PostgreSQL 专家的书籍和博客

这篇文章的灵感来自于我一年多前写的这篇关于 MySQL 的广泛的[文章。在所有四个主要的关系数据库上工作过之后，我阅读了大量的官方文档、博客、白皮书和书籍。这个书籍和博客的集合是我在 PostgreSQL](https://medium.com/crowdbotics/mysql-from-the-horses-mouth-582dbfca2abb) 上读到的所有内容的精华**。它可能不像 MySQL 那样广泛和准确，因为我没有广泛地使用 PostgreSQL。但是，不管怎样，这就是了。**

# 要读的书

只有一本书是我广泛使用过的关于 PostgreSQL 的内容，所以我建议这样做。我的大部分知识来自博客和我在 PostgreSQL 领域关注的人——PostgreSQL:Up Running，第三版，作者 Regina Obe 和 Leo Hsu 。可能会有更多值得阅读的 Postgres 原创作者和开发者的书。

# 要关注的博客

## [戴夫的 Postgres 博客](https://pgsnake.blogspot.com/2010/05/comparing-voltdb-to-postgres.html)

作为 EnterpriseDB 的核心团队成员之一，Dave 对 Postgres 内部发生的事情有着深刻的见解。尽管他的博客自去年 7 月以来一直处于休眠状态，但如果你想深入了解 Postgres，还是有一些博客帖子可供使用。当 VoltDB 在 2010 年第一次出现时，Postgres 和 VoltDB 的比较见此。顺便说一下，戴夫被称为“T21”。

## 宰泽的博客

长期 Postgres 用户，Zaiste 博客作为一个技术用户，而不是来自内部团队的人。他的一系列名为《大忙人入门》的帖子非常棒。在这一点上，他确实有一本为忙碌的人准备的 Postgres 初级读本——只是为了让你开始使用 Postgres。没什么高级的。他是 [Nukomeet](https://nukomeet.com) 、 [RuPy Conf](http://polyconf.com) 和 [PolyConf](http://polyconf.com) 的创始人。

## [安德鲁·邓斯坦的 PostgreSQL 博客](http://adpgtech.blogspot.com/search/label/PostgreSQL/)

作为 PostgreSQL 的核心提交者，Andrew 发表了关于 PostgreSQL 的特性、问题和性能技巧的文章。这个博客已经不活跃很多年了，但是它仍然有非常有用的信息，这些信息来自真正了解内部的人。

## [Bruce Momijan 的 PostgreSQL 博客](http://momjian.us/main/blogs/pgblog.html)

他是 EnterpriseDB(商业 PostgreSQL)的副总裁，从 2008 年开始写作。如果你想专攻 PostgreSQL，他的文章简短扼要，非常有用。他每周发布几次帖子，主题涉及从理解预写日志的内部到复制再到 SQL 查询的方方面面。

## [乔希·伯克思的数据库汤](http://www.databasesoup.com/search/label/postgresql/)

他从事数据库工作已经超过 20 年了。他不怎么发表关于 PostgreSQL 的文章，但无论何时他发表，通常都是关于一些新特性或性能优化技巧。

## [米歇尔·帕奎尔的博客](https://paquier.xyz/)

作为一名 PostgreSQL 提交者，他已经为许多不同的项目做出了贡献。他在会议上发言，并不时在博客上发表关于数据库的文章。我很喜欢 PostgreSQL 12 中的新特性，可以选择是否实现 CTE。cte 广泛用于数据工程和分析领域。这无疑会对查询性能产生重大影响。

## [斯蒂芬·费科特的博客](https://pgstef.github.io)

这个博客主要是关于管理——备份、恢复、复制，特别是许多不同的 Postgres 扩展，如[pg _ back sleeve](https://pgstef.github.io/2019/07/19/pgbackrest_s3_configuration.html)。

## [Hubert Lubaczewski 的博客](https://www.depesz.com/tag/postgresql/)

除了 Postgres 内部的大量知识、新特性和最佳实践，这个博客还有最好的在线查询计划可视化工具。你可以去这里了解一下[。](https://explain.depesz.com)

## [克雷格·克斯廷斯的每周时事通讯](http://www.postgresweekly.com)

对于 Postgres 爱好者来说，这是一个阅读 Postgres 的好地方。这份时事通讯经过精心策划，涵盖了数据库的所有方面——软件开发、数据库管理、分析、扩展等等。查看最新一期的新闻简报[。](https://postgresweekly.com/issues/359)

## [德米特里·方丹的博客](https://tapoueh.org/blog/2018/07/postgresql-concurrency-isolation-and-locking/)

这篇博客来自《PostgreSQL 的艺术》一书的作者。我还没有抽出时间来读这本书，但打算很快读。Dmitri Fontaine 是 PostgreSQL 的长期撰稿人。

## 罗伯特·哈斯的博客

最后但同样重要的是，来自 EnterpriseDB 首席架构师、长期 Postgres 贡献者的关于 Postgres 的最全面的博客之一。数据库一点也不简单。它们是复杂的软件，需要指导和调整，这样你就可以充分利用它们。这个博客对任何想深入研究 Postgres 的人来说都是很好的文献。我已经断断续续关注这个博客很多年了。

我希望你发现这个信息有价值。虽然 PostgreSQL 文档非常简洁，但是光靠文档并不能教会你一切。这些博客中的大部分填补了精通 PostgreSQL 实际需要的东西和文档中提供的东西之间的空白。