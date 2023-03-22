# DuckDB —用于数据分析的 SQLite

> 原文：<https://towardsdatascience.com/duckdb-sqlite-for-data-analysis-a279bdf94399?source=collection_archive---------24----------------------->

## SQLite 是世界上最流行的数据库，非常适合事务处理，但对于分析来说，DuckDB 更快

![](img/cb7a72224ba9c701ecf7712cf3210422.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的[agency followeb](https://unsplash.com/@olloweb?utm_source=medium&utm_medium=referral)拍摄

当我写 [*Python Pandas 和 SQLite，*](/python-pandas-and-sqlite-a0e2c052456f)forward Data Science 的时候，我并不知道另一个嵌入式数据库系统叫做 DuckDB。在文章发表一天左右，我收到了一条关于它的推文，所以我想我应该看看它。

DuckDB 并不像 SQLite 一样内置于 Python 中，但是在简单安装之后，您几乎不会知道其中的区别。

除了有一点不同。DuckDB 开发人员声称他们的数据库系统比 SQLite 的分析应用快 10 倍。当然，在数据库的设计中有这样的技术原因，但主要原因是有两种应用程序类型的关系数据库:联机事务处理(OLTP)和联机分析处理(OLAP)。

OLTP 应用程序通常是为响应外部请求而修改数据库表的业务流程。一个银行系统就是一个例子:人们取出钱，放入钱，他们的余额一直上下波动，这意味着数据库中的数据不断变化，以响应正在发生的交易。跟踪销售和库存水平的数据库可能会在商品销售给客户或从供应商处收到商品时进行类似类型的数据更新。

本质上，OLTP 系统花费大部分时间在数据库表中插入、删除和更新行。

OLAP 的系统没有那么有活力。它们更像是数据仓库。数据被存储起来，但不会经常改变。主要的操作类型是选择、读取、比较和分析数据。在线零售商可能会分析客户的购买数据，将其与其他客户进行比较，并尝试分析相似性。通过这样做，他们可以尝试预测个人的偏好，这样网站的访问者就可以获得个性化的体验。

OLAP 系统倾向于把大部分时间花在处理数据列、求和、寻找均值之类的事情上。

尽管数据库系统都具有相同的基本功能，但那些为 OLTP 设计的数据库系统在 OLAP 应用程序方面不太好，反之亦然，这是由于数据存储的方式—更容易访问列或更容易访问行。

所以 SQLite 适合 OLTP，DuckDB 更适合 OLAP。

您可以通过进行 pip 安装来试用我的原始文章:

```
pip install duckdb
```

并将代码的第一行从

```
import sqlite3 as sql
```

到

```
import duckdb as sql
```

代码的其余部分应该像以前一样工作。

不要期望在我在那篇文章中介绍的简单代码的操作中看到太多的不同，但是如果您需要做任何繁重的数据分析，那么您可能会发现 DuckDB 是一个更好的选择。

我以后会写更多关于这个话题的文章，但是现在，感谢你的阅读和快乐的数据分析。

*声明:我与 DuckDB 或其开发者没有任何联系*

如果你想了解未来的文章，请订阅我的[免费简讯](https://technofile.substack.com)。