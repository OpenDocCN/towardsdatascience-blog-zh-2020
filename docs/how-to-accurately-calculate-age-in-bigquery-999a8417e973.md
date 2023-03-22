# 如何在 BigQuery 中准确计算年龄

> 原文：<https://towardsdatascience.com/how-to-accurately-calculate-age-in-bigquery-999a8417e973?source=collection_archive---------8----------------------->

![](img/6c81352a56791dadda8e20fff5d7ca00.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Helloquence](https://unsplash.com/@helloquence?utm_source=medium&utm_medium=referral) 拍摄的照片

在分析客户数据中，年龄是基本且重要的人口统计数据字段之一。通常，年龄不是从客户那里收集的数据点，而是他们的出生日期。然后据此计算年龄——要么是客户某一时刻的年龄，要么是他们今天的年龄。

不幸的是，在 Bigquery 上没有计算年龄的标准方法，这导致了派生一个基本概念的不同方法。对于开发者和分析师之间的每一个独特的计算，结果之间会有差异。

下面我介绍不同的年龄计算，以及如何使用 SQL 在 BigQuery 中精确计算。在这些例子中，我将使用相同的出生日期(我的)。

# 基本年龄计算

计算年龄的最基本方法是使用 DATE_DIFF 函数来获得两个日期之间的年数。但是，这个函数只是减去了年份，而不管出生日期是否已经过去，这是完全不准确的。

```
WITH data AS (SELECT CAST('1993-04-29' AS DATE) AS date_of_birth)SELECT 
  DATE_DIFF('2020-03-21',date_of_birth, YEAR) AS age
FROM data
```

结果:27

我们可以使用相同的函数来计算日期和出生日期之间的天数差，然后除以 365，而不是使用年数差。FLOOR 用于删除小数位。这在快速分析和准确度不太重要的情况下非常有用。

```
SELECT 
  FLOOR(DATE_DIFF('2020-03-21',date_of_birth, DAY)/365) AS age
FROM data
```

结果:26

此计算假设所有年份都有 365 天。由于这没有考虑闰年，年龄将在实际出生日期前几天取整。年龄向上取整的天数就是已经过去的闰年数。

```
FLOOR(DATE_DIFF('2020-04-27',date_of_birth, DAY)/365) AS age
```

结果:27

# 针对闰年进行调整

将除数中的 365 替换为 365.25 会得到更准确的结果。

```
FLOOR(DATE_DIFF('2020-04-27',date_of_birth, DAY)/365.25) AS age
```

结果:26

然而，在闰年有影响的特定年龄，结果变得不太准确。

```
FLOOR(DATE_DIFF('2011-04-29',date_of_birth, DAY)/365.25) AS age
```

结果:17

在这里，年龄实际上被向下舍入了。在这种情况下，结果应该是 18。虽然误差影响极小，并且这被证明是更好的选择，但在某些情况下，精度非常重要。

# 准确无误

在市场营销中，不正确地计算年龄可能会有法律影响，或者只是非常糟糕的客户体验。在这个看似复杂(其实不是)的计算中，它使用了几个函数。查询的第一部分减去两个日期之间的年份。如果通过比较月份和日期，出生日期已经过了另一个日期，则第二部分将减去 1 年。

```
DATE_DIFF('2020-03-21',date_of_birth, YEAR)
- 
IF(EXTRACT(MONTH FROM date_of_birth)*100 + EXTRACT(DAY FROM date_of_birth) > EXTRACT(MONTH FROM '2020-03-21')*100 + EXTRACT(DAY FROM '2020-03-21'),1,0) AS age
```

结果:26

使用这种计算方法的缺点是，每次需要计算年龄时都要重复计算，时间很长，而且相当麻烦(除非您将查询保存在某个地方)。

# 执着的 UDF

BigQuery 现在允许持久的用户定义函数。这使得重用相同的代码并在整个项目中保持一致变得更加容易。作为一个已经编码了几年的人，每次需要获得年龄时，我都必须使用不同的函数并编写一个很长的查询来获得一个准确的年龄，这是一种挣扎。相反，我创建了一个 UDF，这样我就可以在每次分析时引用相同的代码。

```
CREATE OR REPLACE FUNCTION workspace.age_calculation(as_of_date DATE, date_of_birth DATE) AS (
DATE_DIFF(as_of_date,date_of_birth, YEAR) - 
IF(EXTRACT(MONTH FROM date_of_birth)*100 + EXTRACT(DAY FROM date_of_birth) > EXTRACT(MONTH FROM as_of_date)*100 + EXTRACT(DAY FROM as_of_date),1,0))
```

在这个 UDF 中，有两个输入-出生日期和您想要计算年龄的截止日期。这为用户提供了重用代码和不同用例的灵活性。

```
SELECT workspace.age_calculation('2020-03-21','1993-04-29')
```

结果:26

有趣的是，年龄是一个如此重要的属性，却没有标准的计算方法，无论是在分析还是锁定客户方面。然而，使用 BigQuery 函数的组合给出了更好的选择。我希望这有助于分析师在他们的数据中使用标准和准确的方法计算年龄。