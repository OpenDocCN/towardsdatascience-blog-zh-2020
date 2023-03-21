# SQL 备忘单

> 原文：<https://towardsdatascience.com/sql-cheat-sheet-776f8e3189fa?source=collection_archive---------0----------------------->

## PostgreSQL 中使用的标准 SQL 语法入门指南

![](img/3b86a4d7e21c2b65bec7ebcc44c8f36d.png)

来自 [Pexels](https://www.pexels.com/photo/bandwidth-close-up-computer-connection-1148820/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [panumas nikhomkhai](https://www.pexels.com/@cookiecutter?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的照片

SQL 是学习数据分析最重要的编码语言。有些人可能会认为 Python 和 R 同等重要，但说到分析师必须拥有的最常用工具，那就是 SQL。

根据 [Dataquest.io](https://www.dataquest.io/blog/why-sql-is-the-most-important-language-to-learn/) 的报道，几乎所有的技术巨头都使用 SQL。优步、网飞、Airbnb——这样的例子不胜枚举。即使在像脸书、谷歌和亚马逊这样已经建立了自己的高性能数据库系统的公司中，数据团队也使用 SQL 来查询数据和执行分析。

像每一种语言一样，你需要不断练习来理解和掌握概念。在我看来，一旦你理解了代码的基本结构，SQL 是最容易使用的语言之一。在本文中，我分享了开始使用 SQL 查询的必要步骤。

# 标准 SQL 结构

这是一系列 PostgreSQL 备忘单的第 1 部分，将涵盖`SELECT`、`FROM`、`WHERE`、`GROUP BY`、`HAVING`、`ORDER BY`和`LIMIT`。

从单个表中提取结果的查询的基本结构如下。

```
SELECT 
	COLUMN_NAME(S)
FROM
	TABLE_NAME
WHERE
	CONDITION
GROUP BY
	COLUMN_NAME(S)
HAVING
	AGGREGATE_CONDITION
ORDER BY
	COLUMN_NAME
LIMIT
	N
```

# 什么是 SQL？

[SQL](http://www.sqlcourse.com/intro.html) (读作“ess-que-el”)代表结构化查询语言。SQL 用于与数据库通信。它是关系数据库管理系统的标准语言。SQL 语句用于执行更新数据库数据或从数据库中检索数据等任务。

# 什么是关系数据库管理系统？

RDBMS 将数据组织成包含行和列的表格。术语“关系”意味着每个表中的值相互之间有关系。

*   行—也称为记录
*   列—也称为字段，具有描述性名称和特定的数据类型。

# PostgreSQL 是什么？

PostgreSQL 是一个通用的关系数据库管理系统，是最先进的开源数据库系统。

其他常见的数据库管理系统有 MySQL、Oracle、IBM Db2 和 MS Access。

我们开始吧！

# 挑选

SELECT 语句用于从数据库中选择数据。返回的数据存储在结果表中，称为结果集。

## 特定列

```
SELECT
	COLUMN_1,
	COLUMN_2
FROM
	TABLE_NAME
```

## 所有列

使用`*`可以查询表中的每一列

```
SELECT *
FROM
	TABLE_NAME
```

## 不同的列

查找列中所有唯一的记录

```
SELECT 
	DISTINCT(COLUMN_NAME)
FROM
	TABLE_NAME
```

## 统计所有行

如果你想知道整个表格中的所有值，使用`COUNT(*)`你将得到一个单一的数字。

```
SELECT
	COUNT(*)
FROM
	TABLE_NAME
```

## 计算不同的值

如果您想知道一列中不同值的数量，使用`COUNT`和`DISTINCT`，您将得到一个代表一列中唯一值总数的数字

```
SELECT 
	COUNT (DISTINCT COLUMN_NAME)
FROM
	TABLE_NAME
```

# 在哪里

使用`WHERE`子句，你可以创建条件来过滤掉你想要或不想要的值。

注意— `WHERE`总是用在`GROUP BY`之前(稍后会详细介绍)

```
SELECT *
FROM
	TABLE_NAME
WHERE
	CONDITION
```

## 情况

SQL 中可以使用多种条件。下面是一些包含学生在校成绩的表格示例。您只需要指定`WHERE`一次，为了便于举例，我在每个步骤中都包含了`WHERE`。

```
WHERE FIRSTNAME      = 'BOB'      -- exact match
WHERE FIRSTNAME     != 'BOB'     -- everything excluding BOB 
WHERE NOT FIRSTNAME  ='BOB'    -- everything excluding BOBWHERE FIRSTNAME IN ('BOB', 'JASON')       -- either condition is met
WHERE FIRSTNAME NOT IN ('BOB', 'JASON')   -- excludes both valuesWHERE FIRSTNAME = 'BOB' AND LASTNAME = 'SMITH'  -- both conditions 
WHERE FIRSTNAME = 'BOB' OR FIRSTNAME = 'JASON'  -- either conditionWHERE GRADES > 90           -- greater than 90
WHERE GRADES < 90           -- less than 90
WHERE GRADES  >= 90         -- greater than or equal to 90
WHERE GRADES  <= 90         -- less than or equal to 90WHERE SUBJECT IS NULL       -- returns values with missing values
WHERE SUBJECT NOT NULL      -- returns values with no missing values
```

## 条件—通配符

`LIKE`运算符用于在`WHERE`子句中搜索列中的指定模式。当您通过`LIKE`操作符时，在`''`中大写和小写都很重要。

有两个通配符经常与`LIKE`操作符一起使用:

*   `%` -百分号代表零个、一个或多个字符
*   `_` -下划线代表单个字符

```
WHERE FIRSTNAME LIKE ‘B%’ -- finds values starting uppercase BWHERE FIRSTNAME LIKE ‘%b’ -- finds values ending lowercase bWHERE FIRSTNAME LIKE ‘%an%’ -- find values that have “an” in any positionWHERE FIRSTNAME LIKE ‘_n%’ -- find values that have “n” in the second positionWHERE FIRSTNAME LIKE ‘B__%’ -- find values that start with “B” and have at least 3 characters in lengthWHERE FIRSTNAME LIKE ‘B%b’ -- find values that start with “B” and end with “b”WHERE FIRSTNAME LIKE ‘[BFL]’ -- find all values that start with ‘B’, ‘F’ OR ‘L’WHERE FIRSTNAME LIKE ‘[B-D]’ -- find all values that start with ‘B’, ‘C’, OR ‘D’WHERE FIRSTNAME LIKE ‘[!BFL]%’ -- find everything exlcusing values that start with ‘B’, ‘F’ OR ‘L’WHERE FIRSTNAME NOT LIKE ‘[BFL]%’ -- same as above. excludes values starting with ‘B’, ‘F’, OR ‘L’WHERE GRADES BETWEEN 80 and 90 -- find grades between 80 and 90
```

# 分组依据

`GROUP BY`函数帮助计算所选列的汇总值。它经常与聚合函数一起使用(`COUNT`、`SUM`、`AVG`、`MAX`、`MIN`)。

```
SELECT
	SUBJECT,	
	AVG(GRADES)
FROM
	STUDENTS
GROUP BY
	SUBJECT
```

上面的查询将对每个科目进行分组，并计算平均成绩。

```
SELECT
	SUBJECT,	
	COUNT(*)
FROM
	STUDENTS
GROUP BY
	SUBJECT
```

上面的查询会计算出各科学生的**数**(计数)。

# 拥有

`HAVING`子句与`WHERE`相似，但适用于**过滤集合函数**。`HAVING`功能在`GROUP BY`之后，相比之下`WHERE`在`GROUP BY`之前。

如果我们想知道哪一门课的平均成绩在 90 分以上，我们可以使用下面的例子。

```
SELECT
	SUBJECT,	
	AVG(GRADES)
FROM
	STUDENTS
GROUP BY
	SUBJECT
HAVING 
	AVG(GRADES) >= 90
```

# 以...排序

使用`ORDER BY`函数，您可以指定您想要的值排序方式。继续前面的学生表。

```
SELECT
	*
FROM
	STUDENTS
ORDER BY
	GRADES DESC
```

当默认使用`ORDER BY`时，排序将按照**升序**进行。如果要下行，需要在列名后面指定`DESC`。

# 限制

在 Postgres 中，我们可以使用`LIMIT`函数来控制在查询中输出多少行。例如，如果我们想找到分数最高的前 3 名学生。

```
SELECT
	*
FROM
	STUDENTS
ORDER BY
	GRADES DESC
LIMIT
	3
```

由于我们使用了`ORDER BY DESC`，我们将分数最高的学生排在最前面——现在限制为 3 个值，我们看到了**前 3 名**。

# 概观

希望您可以使用该入门指南来了解在从单个表中查询数据时使用的标准 SQL 语法。在 SQL 中你可以做更多的事情，我将分享更多的扩展高级语法的 SQL 备忘单。

如果你想学习具体的技术，可以看看我的其他教程。

*   [SQL 中的日期/时间函数](/date-time-functions-in-sql-1885e2cbdc1a)
*   [窗口功能介绍](/intro-to-window-functions-in-sql-23ecdc7c1ceb)
*   [如何在 SQL 中使用 CTEs](/using-ctes-to-improve-sql-queries-dfcb04b7edf0)
*   [在 SQL 中创建表格](/creating-tables-in-sql-96cfb8223827)