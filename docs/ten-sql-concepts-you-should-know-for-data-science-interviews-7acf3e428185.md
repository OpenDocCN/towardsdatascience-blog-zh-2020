# 数据科学面试中你应该知道的十个 SQL 概念

> 原文：<https://towardsdatascience.com/ten-sql-concepts-you-should-know-for-data-science-interviews-7acf3e428185?source=collection_archive---------2----------------------->

## 学习聪明，不努力。

![](img/777b908f63140207bf4dc5f9faba91f3.png)

由宏向量创建的设计向量—[www.freepik.com](http://www.freepik.com)

SQL 非常强大，有很多功能。然而，当谈到数据科学面试时，大多数公司测试的核心概念真的很少。这 10 个概念出现的频率最高，因为它们在现实生活中应用最多。

在这篇文章中，我将回顾我认为的 10 个最重要的 SQL 概念，这些概念是你在准备面试时应该花大部分时间关注的。

说到这里，我们开始吧！

> **请务必点击** [**订阅此处**](https://terenceshin.medium.com/membership) **或我的** [**个人简讯**](https://terenceshin.substack.com/embed) **千万不要错过另一篇关于数据科学指南、技巧和提示、生活经验等的文章！**

# **1。案例当**

您很可能会看到许多问题需要使用 CASE WHEN 语句，这仅仅是因为它是一个如此通用的概念。

如果您想根据其他变量分配某个值或类，它允许您编写复杂的条件语句。

鲜为人知的是，它还允许您透视数据。例如，如果您有一个月列，并且希望为每个月创建一个单独的列，则可以使用 CASE WHEN 语句透视数据。

*示例问题:编写一个 SQL 查询来重新格式化该表，以便每个月都有一个收入列。*

```
Initial table:
+------+---------+-------+
| id   | revenue | month |
+------+---------+-------+
| 1    | 8000    | Jan   |
| 2    | 9000    | Jan   |
| 3    | 10000   | Feb   |
| 1    | 7000    | Feb   |
| 1    | 6000    | Mar   |
+------+---------+-------+

Result table:
+------+-------------+-------------+-------------+-----+-----------+
| id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
+------+-------------+-------------+-------------+-----+-----------+
| 1    | 8000        | 7000        | 6000        | ... | null        |
| 2    | 9000        | null        | null        | ... | null        |
| 3    | null        | 10000       | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-----------+
```

> **更多类似问题，** [**查看 StrataScratch**](https://platform.stratascratch.com/coding)**100 个 SQL 问题。**

# 2.选择不同

选择独特是你应该永远记住的。在聚合函数中使用 SELECT DISTINCT 语句非常常见(这是第三点)。

例如，如果您有一个显示客户订单的表，可能会要求您计算每个客户的平均订单数。在这种情况下，您可能希望计算订单总数，而不是客户总数。它可能看起来像这样:

```
SELECT
   COUNT(order_id) / COUNT(DISTINCT customer_id) as orders_per_cust
FROM
   customer_orders
```

# 3.聚合函数

关于第二点，你应该对 min、max、sum、count 等聚合函数有很深的理解。这也意味着你应该对 GROUP BY 和 HAVING 子句有很深的理解。我强烈建议您花时间来完成练习题，因为有一些创造性的方法可以使用聚合函数。

*示例问题:编写一个 SQL 查询，在一个名为* `*Person*` *的表中查找所有重复的电子邮件。*

```
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+**ANSWER:**
SELECT
    Email
FROM
    Person
GROUP BY
    Email
HAVING
    count(Email) > 1
```

> **请务必点击** [**订阅此处**](https://terenceshin.medium.com/membership) **或我的** [**个人简讯**](https://terenceshin.substack.com/embed) **千万不要错过另一篇关于数据科学指南、技巧和提示、生活经验等的文章！**

# 4.左连接与内连接

对于那些对 SQL 比较陌生或者已经有一段时间没有使用它的人来说，很容易混淆左连接和内连接。请确保您清楚地了解每个连接如何产生不同的结果。在许多面试问题中，你会被要求做一些连接，在某些情况下，选择一个对另一个是正确和错误答案之间的区别。

# 5.自连接

现在我们开始更有趣的东西了！SQL 自联接将表与其自身联接起来。你可能认为这没有用，但是你会惊讶于这是多么的普遍。在许多实际设置中，数据存储在一个大表中，而不是许多较小的表中。在这种情况下，可能需要自联接来解决独特的问题。

让我们看一个例子。

*示例问题:给定下面的* `*Employee*` *表，编写一个 SQL 查询，找出收入高于其经理的雇员。在上表中，Joe 是唯一一个收入高于其经理的员工。*

```
+----+-------+--------+-----------+
| Id | Name  | Salary | ManagerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |
+----+-------+--------+-----------+**Answer:**
SELECT
    a.Name as Employee
FROM
    Employee as a
        JOIN Employee as b on a.ManagerID = b.Id
WHERE a.Salary > b.Salary
```

# 6.子查询

子查询也称为内部查询或嵌套查询，是查询中的查询，嵌入在 WHERE 子句中。这是解决需要按顺序进行多次查询才能产生给定结果的独特问题的好方法。查询时，子查询和 WITH AS 语句都非常常用，所以您应该绝对确保知道如何使用它们。

例题:假设一个网站包含两个表，`Customers`表和`Orders`表。编写一个 SQL 查询来查找从不订购任何东西的所有客户。

```
Table: Customers.+----+-------+
| Id | Name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+Table: Orders.
+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+**Answer:**
SELECT
    Name as Customers
FROM 
    Customers
WHERE
    Id NOT IN (
        SELECT 
            CustomerId 
        FROM Orders
    )
```

# 7.字符串格式

字符串函数非常重要，尤其是在处理不干净的数据时。因此，公司可能会测试你的字符串格式和操作，以确保你知道如何操作数据。

字符串格式包括以下内容:

*   左，右
*   整齐
*   位置
*   SUBSTR
*   串联
*   上、下
*   联合

如果你对这些不确定，可以查看一下[模式关于清理数据的字符串函数教程](https://mode.com/sql-tutorial/sql-string-functions-for-cleaning/)。

> **请务必点击** [**订阅此处**](https://terenceshin.medium.com/membership) **或我的** [**个人简讯**](https://terenceshin.substack.com/embed) **千万不要错过另一篇关于数据科学指南、技巧和提示、生活经验等的文章！**

# 8.日期时间操作

您肯定会遇到一些涉及日期时间数据的 SQL 问题。例如，您可能需要按月对数据进行分组，或者将变量格式从 DD-MM-YYYY 转换为月份。

您应该知道的一些功能有:

*   提取
*   DATEDIFF

*例题:给定一个* `*Weather*` *表，编写一个 SQL 查询，找出所有与前一个(昨天的)日期相比温度更高的日期的 id。*

```
+---------+------------------+------------------+
| Id(INT) | RecordDate(DATE) | Temperature(INT) |
+---------+------------------+------------------+
|       1 |       2015-01-01 |               10 |
|       2 |       2015-01-02 |               25 |
|       3 |       2015-01-03 |               20 |
|       4 |       2015-01-04 |               30 |
+---------+------------------+------------------+**Answer:**
SELECT
    a.Id
FROM
    Weather a,
    Weather b
WHERE
    a.Temperature > b.Temperature
    AND DATEDIFF(a.RecordDate, b.RecordDate) = 1
```

# 9.窗口功能

窗口函数允许您对所有行执行聚合值，而不是只返回一行(这是 GROUP BY 语句所做的)。如果您想对行进行排序、计算累积和等等，这是非常有用的。

*例题:写一个查询得到工资最高的* `*empno*` *。确保您的解决方案能够处理领带！*

```
 depname  | empno | salary |     
-----------+-------+--------+
 develop   |    11 |   5200 | 
 develop   |     7 |   4200 | 
 develop   |     9 |   4500 | 
 develop   |     8 |   6000 | 
 develop   |    10 |   5200 | 
 personnel |     5 |   3500 | 
 personnel |     2 |   3900 | 
 sales     |     3 |   4800 | 
 sales     |     1 |   5000 | 
 sales     |     4 |   4800 |**Answer:**
WITH sal_rank AS 
  (SELECT 
    empno, 
    RANK() OVER(ORDER BY salary DESC) rnk
  FROM 
    salaries)
SELECT 
  empno
FROM
  sal_rank
WHERE 
  rnk = 1;
```

# 10.联盟

作为奖励，#10 是工会！虽然这个问题不经常出现，但偶尔会有人问你这个问题，大体了解一下是有好处的。如果有两个列相同的表，并且想要合并它们，这时就应该使用 UNION。

同样，如果你不是 100%确定它是如何工作的，我会做一些快速的谷歌搜索来了解它。:)

# 感谢阅读！

仅此而已！我希望这对你的面试准备有所帮助，并祝你在未来的努力中好运。我确信，如果你对这 10 个概念了如指掌，那么在处理大多数 SQL 问题时，你会做得很好。

# 特伦斯·申

*   *如果你喜欢这个，* [*在 Medium 上关注我*](https://medium.com/@terenceshin) *了解更多*
*   [***查看更多 SQL 练习题***](https://platform.stratascratch.com/coding)