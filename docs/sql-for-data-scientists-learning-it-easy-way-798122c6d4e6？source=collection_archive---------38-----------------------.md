# 面向数据科学家的 SQL:轻松学习

> 原文：<https://towardsdatascience.com/sql-for-data-scientists-learning-it-easy-way-798122c6d4e6?source=collection_archive---------38----------------------->

## 用于数据准备、过滤、连接等的 SQL 语句

![](img/036c494aae503f0e27b88361cdef7098.png)

乔安娜·科辛斯卡在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

我们不会拿起锤子去找钉子——那是解决问题的不寻常的方式。通常做生意的方式是先确定问题，然后寻找合适的工具。

我一次又一次地看到人们通过选择 SQL 语句来学习 SQL，然后学习如何使用它。在我看来，这种基于工具的心态是一种低效的学习方式，另一方面，翻转这种心态可以产生巨大的差异。先有问题，后有工具！

如果你对数据科学感兴趣，你会知道`pandas`和`tidyverset`在过滤、排序、分组、合并等各种数据处理操作中的能力。使用 SQL，您可以做类似的事情，但是是在数据库环境中，并且使用不同的语言。

本文的目的是演示如何采用数据科学家在编程环境中通常遵循的类似方法来解决 SQL 中的数据处理问题。在 SQL 上你不会学到所有的东西，相反，目标是展示“如何”学习。

## 适合您实践的 SQL 编辑器

如果您的计算机上安装了关系数据库管理系统，启动它。如果没有， [w3schools](https://www.w3schools.com/sql/trysql.asp?filename=trysql_op_in) 有一个在线 SQL 编辑器，你可以马上在你的浏览器上使用。

您还会注意到，屏幕右侧有相当多的数据集，您可以一起使用和练习。

现在让我们进入“如何”使用 SQL 解决实际的数据处理问题。

# 理解数据

就像你用你最喜欢的编程库如`pandas`所做的一样，你需要做的第一件事就是在 SQL 环境中加载数据集。

就像典型数据科学项目中的基本探索性数据分析(EDA)一样，您可以检查前几行，计算总行数，查看列名、数据类型等。下面是一些命令。

```
# import data into editor
SELECT * # import all columns with *, else specify column name
FROM table_name
LIMIT 10 #to show 10 rows# import and save data as a separate table
SELECT *
INTO new_table_name
FROM table_name# count number of rows in the dataset
SELECT 
COUNT(*)
FROM table_name# count unique values of a single column
SELECT 
COUNT(DISTINCT column_name) 
FROM table_name
```

# 使用列

数据库通常很大，运行查询需要很长时间。因此，如果您知道您感兴趣的具体列，您可以通过选择这些列来创建数据的子集。

您可能还想执行列操作，如重命名、创建新列等。

```
# select two columns from a multi-column dataset
SELECT column1, column2
FROM tableName# rename a column
SELECT
ProductName AS name
FROM productTable# new conditional column (similar to if statment)
SELECT ProductName, Price,(CASE
WHEN Price > 20 AND Price <41 THEN 'medium '
WHEN Price >40 THEN 'high'
ELSE 'low'
END) AS newColNameFROM Products
```

# 筛选行

过滤行可能是您经常使用 SQL 执行的最重要的任务。从大型数据集中，您通常会根据产品类型、值的范围等来过滤行。

如果你正在学习 SQL，你应该花大量的时间学习过滤数据的不同方法和你需要的 SQL 语句。

```
# select all records that starts with the letter "S"
SELECT * FROM Products
WHERE ProductName like 'S%'# select all records that end at "S"
SELECT * FROM Products
WHERE ProductName like '%S'# select all records that does NOT start at "S"
SELECT * FROM Products
WHERE ProductName like '[^S]%'# filter rows with specific value
SELECT * FROM table_nameWHERE firstName = 'Pilu'
OR lastName != 'Milu'
AND income <= 100
AND city IN ('Arlington', 'Burlington', 'Fairfax')# filter rows within a range of numbers
SELECT *
FROM tableName
WHERE income BETWEEN 100 AND 200 # filter null values
SELECT * FROM tableName
WHERE columnName IS NULL # opposite "IS NOT NULL"
```

# 连接数据集

您在关系数据库管理系统(RDBMS)中使用 SQL，这意味着您将同时处理多个表，因此在您能够进行高级建模之前，需要将它们连接起来。

基本上有四种连接数据的方法——左连接、右连接、内连接、全外连接——您需要稍微搜索一下，看看每种方法是如何工作的，但是我在下面给出了执行这些连接的所有代码。

```
# inner join (for matching records only)
SELECT * FROM
table1 INNER JOIN table2
ON table1.ID = tbale2.ID# full outer join (all left + all right)
SELECT * FROM
table1 FULL OUTER JOIN table2
ON table1.ID = tbale2.ID# left join (all records from left + matching records from right)
SELECT * FROM
table1 LEFT JOIN table2
ON table1.ID = tbale2.ID# left join (matching records from left + all records from right)
SELECT * FROM
table1 RIGHT JOIN table2
ON table1.ID = tbale2.ID
```

# 做计算

创建汇总统计数据、数学运算和构建模型是数据科学家每天要做的事情。SQL 并不是一个合适的工具，但是，如果你需要创建一个快速的汇总统计数据，你可以使用聚合函数来计算列的平均值、总数、最小值/最大值等。

```
# new calculated column
SELECT Price,
(Price * 2) AS NewCol
FROM Products# aggregation by group
SELECT CategoryID, SUM(Price) 
FROM Products
GROUP BY CategoryID# min/max values of a column
SELECT ProductName, MIN(Price)
FROM Products
```

## 最终注释

本文的目的是介绍一些基本的 SQL 概念和语句，用于从关系数据库管理系统中查询数据。但主要目的是展示如何作为一名数据科学家学习 SQL，以解决特定的问题，而不是专注于 SQL 语句。