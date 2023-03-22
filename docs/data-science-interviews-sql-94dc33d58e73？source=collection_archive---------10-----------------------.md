# 数据科学访谈:SQL

> 原文：<https://towardsdatascience.com/data-science-interviews-sql-94dc33d58e73?source=collection_archive---------10----------------------->

## SQL 技术指南和 10 个需要检查的问题

![](img/115564ee3ff5f27df1eafd5cd1ff515c.png)

图片由 Alexandra Acea 在 [Unsplash](https://unsplash.com/photos/XEB8y0nRRP4) 上拍摄

# 概观

当你听到“数据科学家”时，你会想到建模、有见地的分析、机器学习和其他酷词。所以，让我们不要拐弯抹角:数据库和 SQL 不是数据科学家最“有趣”的部分。这可能不是你面试时最担心的。但是也就是说，因为您将大量使用数据，这意味着您将编写许多查询来检索数据并将其转换为有意义的数据。

这篇文章将在数据科学访谈中提供 SQL 的技术指南。讨论的问题来自这个数据科学采访[时事通讯](https://datascienceprep.com/)，其中有来自顶级科技公司的问题，并将涉及到即将出版的[书](http://acethedatascienceinterview.com/)。

# 一般工作流程

大多数工作流分析的第一步都涉及到快速分割 SQL 中的数据，因此能够高效地编写基本查询是一项非常重要的技能。尽管许多人可能认为 SQL 只涉及选择和连接，但强大的 SQL 工作流还涉及许多其他操作符和细节。例如，利用子查询很重要，它允许您操作数据的子集，通过这些子集可以执行后面的操作，而窗口函数允许您在不使用 GROUP BY 显式组合行的情况下剪切数据。SQL 中提出的问题通常对手头的公司非常实用——像脸书这样的公司可能会问各种用户或应用分析，而像亚马逊这样的公司会问产品和交易。

# 基本命令

这里有一些你绝对应该知道的基本但重要的 SQL 命令。让我们首先讨论它们是做什么的——稍后您将通过示例看到实际的语法和代码。

*   CREATE TABLE:该命令在关系数据库中创建一个表。您可以使用这个命令定义一个表的模式(这是可选的，取决于您使用的数据库，例如 MySQL)。
*   INSERT:这将一次向给定的表或一组行中插入一行。
*   更新:当您需要修改已经存在的数据时，使用该命令。
*   删除:从数据中删除一行(或多行)。
*   SELECT:从表中选取和获取某些列。这在大多数查询中使用。
*   内部连接:将多个表组合在一起，它将保留两个表中列值匹配的行。单词 INNER 是可选的，很少使用(通常省略)。
*   左外连接:通过匹配提供的列将多个表组合在一起，但保留连接的第一个表中的所有行
*   完全外部连接:通过匹配列将多个表组合在一起，但保留所有行(外部连接是可选的)。
*   GROUP BY:对指定列或一组列中具有相同值的行进行分组/聚合。
*   其中:在应用任何分组之前，提供筛选条件。
*   HAVING:在应用任何分组后，提供筛选条件。
*   ORDER BY:按特定的一列或多列对结果进行升序或降序排序。
*   DISTINCT:仅返回不同的值。

在实践中，面试官会评估你能否回答一个简单的问题(通常是一个业务用例)，并提出正确的问题。

# 有用的花絮

一般来说，SQL 查询会涉及到以下几个部分:

1.  聚合:对结果行集使用 COUNT()、SUM()或任何聚合函数

```
SELECT COUNT(*) FROM users..
```

2.连接:常用于多个表之间的查询。例如，下面是用户名花费的总时间:

```
SELECT users.name, SUM(sessions.time_spent) AS total_time_spent  FROM users u JOIN sessions s ON u.user_id = s.user_id GROUP BY 1
```

3.过滤:有各种方法来测试相等性，其中最常见的是使用=和<>(不等于)、or >和

```
SELECT * FROM users  WHERE username <> 'unknown' OR username LIKE '%123%'
```

4.公用表表达式(cte)和子查询:

CTEs 允许您定义一个查询，然后在以后使用别名引用它——对于分解大型查询或者对于编写的各种查询有自然顺序的情况非常有用。例如，我们可以对用户子集进行快速 CTE，然后使用这些特定用户进行简单分析:

```
WITH (SELECT * FROM users WHERE join_date > '2020-01-01') AS u   SELECT u.name, SUM(sessions.time_spent) AS total_time_spent  FROM u JOIN sessions s ON u.user_id = s.user_id GROUP BY 1
```

子查询内嵌在查询本身中，只能使用一次:

```
SELECT * FROM users AS u JOIN  (SELECT user_id, time_spent FROM sessions   WHERE session_date > '2020-01-01') AS s  ON u.user_id = s.user_id
```

查询问题通常围绕着一组与公司相关的表——在脸书是用户和使用情况表，在优步是乘客和司机表，等等。因此，除了熟悉查询之外，理解数据库的基本概念也很重要(将在下面介绍):主键、外键、索引等等。这样做的一个好方法是考虑公司应该使用的潜在表，以及哪些操作是有用的。

例如，想象你在脸书工作，有用户和帖子以及对帖子的反应。这些表看起来会是什么样子，你用来连接表的外键是什么？表中的哪些列将被索引，为什么？

# 窗口功能

窗口函数是跨一组行进行计算的函数(像聚合函数一样)，但不将这些行分组(像聚合函数一样)。它们涉及一个聚合函数和一个 OVER 子句，您可以通过使用 PARTITION BY 来缩小窗口:

```
SELECT *, SUM(time_spent) OVER 
(PARTITION BY user_id ORDER BY join_time ASC) 
FROM sessions
```

在这个特定的示例中，我们获得了会话数据中行级别花费的总时间(不需要按每个 user_id 分组)。

请注意，您不能在同一个查询中使用窗口函数和聚合函数，但是它们在各种情况下都很方便。当您看到需要聚合时，您会希望让它们参与进来，但是要维护行。

# 10 个 SQL 面试问题

1.  假设你有一个关于应用分析的表格(应用标识，事件标识，时间戳)。写一个查询，得到 2020 年每个 app 的点击率。
2.  假设给你一个按设备分类的观众情况表(用户标识、设备类型、观看时间)。将“移动”定义为平板电脑和手机的总和。编写一个查询来比较笔记本电脑和移动设备上的收视率。
3.  假设您有一个来自用户的收入表(user_id，spend，transaction_date)。编写一个查询来获取每个用户的第三次购买。
4.  假设您有一个按产品类型(order_id、user_id、product_id、spend、date)划分的支出活动表。编写一个查询，按时间顺序计算每种产品在一段时间内的累计花费。
5.  假设给你一个用户发推文的表格(tweet_id，user_id，msg，tweet_date)。编写一个查询来获取 2020 年内发布的 tweet 计数的直方图。
6.  假设您有两个表:1)用户(user_id，date)和 2)用户帖子(post_id，user_id，body，date)。编写一个查询来获取用户发布的帖子数量的分布。
7.  假设您有一个按产品类型(order_id、user_id、product_id、spend、date)划分的支出活动表。编写一个查询，按时间顺序计算每种产品在一段时间内的累计花费。
8.  假设您有一个关于用户操作的表(user_id、event_id、timestamp)。编写一个查询，按月获取活动用户保持率。
9.  假设您从用户那里得到一个会话表(session_id，start_time，end_time)。如果一个会话与另一个会话在开始和结束时间上重叠，则它们是并发的。编写一个查询来输出与最大数量的其他会话并发的会话。
10.  假设您有两个表，分别是 1)用户注册(user_id，signup_date)和 2)用户购买(user_id，product_id，purchase_amount，purchase_date)。编写一个查询来获取上周加入的用户的转换率，这些用户从那以后至少购买了一件商品。

# 感谢阅读！

如果您有兴趣在数据科学访谈中进一步探索 SQL，请查看这份[简讯](https://datascienceprep.com/)，它每周三次向您发送练习题。另外，请留意即将出版的[书](http://acethedatascienceinterview.com/)！