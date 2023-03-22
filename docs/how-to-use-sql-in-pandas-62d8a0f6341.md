# 如何在熊猫中使用 SQL

> 原文：<https://towardsdatascience.com/how-to-use-sql-in-pandas-62d8a0f6341?source=collection_archive---------5----------------------->

## 将另一项关系数据库技能添加到您的数据科学工具包中

![](img/ab9552e6618d9d4d33a7d0173459a151.png)

[https://images . unsplash . com/photo-1489875347897-49 f 64 b 51 C1 f 8？IX lib = r b-1 . 2 . 1&ixid = eyjhchbfawqiojeymdd 9&auto = format&fit = crop&w = 800&q = 60](https://images.unsplash.com/photo-1489875347897-49f64b51c1f8?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=800&q=60)

如果您考虑熊猫数据帧的结构和来自 SQL 数据库的表的结构，它们的结构非常相似。它们都由数据点或值组成，每一行都有唯一的索引，每一列都有唯一的名称。因此，SQL 允许您快速访问您正在处理的任何项目所需的特定信息。但是，非常相似的查询可以使用熊猫！在这篇博文中，我将向您展示如何做到这一点，并解释您需要哪个库来实现它。

## 。查询()

使用 SQL 时，获取我们需要的信息叫做**查询**数据。在 Pandas 中，有一个内置的查询方法可以让您做完全相同的事情，它被称为*。查询()*。这既节省了时间，又使您的查询在代码中更加连贯，因为您不必使用切片语法。例如，一个使用*查询 Pandas 中数据的简单示例。query()* 方法会是:

```
query_df **=** df.query("Col_1 > Col_2")
```

否则，如果您不使用此方法获取数据，而是使用切片语法，它看起来会像这样:

```
query_df **=** df[df[df['Col_1'] **>** df['Col_2']]]
```

就像我说的，*。query()* 方法让你的代码看起来更专业，更高效。我想注意的一件重要的事情是，如果/当你决定在你的 Pandas 查询中使用“and”或“or”时，你实际上不能使用单词“and”或“or”——你必须使用符号“and”(&)和“or”(|)。下面是一个使用“&”帮助澄清的示例:

```
query_df **=** df.query("Col_1 > Col_2 & Col_2 <= Col_3")
```

## pandasql 库

众所周知，使用 SQL 和/或其所有变体的能力是数据科学家在市场上最需要的工作技能之一，即使是在疫情时期。幸运的是，现在 Python 中有一个名为 *pandasql* 的库，它允许您编写 sql 风格的语法来从 Pandas DataFrames 中收集数据！这对于希望练习 SQL 技能的有抱负的数据科学家和习惯于使用 SQL 风格的语法收集数据的有经验的数据科学家来说都是非常好的。要将它安装到您的计算机上，只需使用！pip 安装:

```
!pip install pandasql
```

然后，要将它导入到您的笔记本中，您需要从 *pandasql:* 中导入一个 *sqldf* 对象

```
**from** pandasql **import** sqldf
```

导入所有内容后，编写一个快速的 lambda 函数是一个好主意，它可以使编写查询更容易。这样做的原因是，每次使用一个对象时，不必传入全局变量。下面是我所学的 lambda 函数，我用它取得了成功:

```
pysqldf **=** **lambda** q: sqldf(q, globals())
```

现在，每当你向 *pysqldf* 传递一个查询时，全局变量将在 lambda 中传递，这样你就不必为每个使用的对象一遍又一遍地做这些了。

现在您已经做好了一切准备，可以使用与 SQL 相同的语法查询数据帧中的数据了！下面是一个例子—这个查询将从一个 df 中返回前 10 个名字:

```
q = """SELECT Name 
       FROM df 
       LIMIT 10;"""

names = pysqldf(q)
names
```

查询的复杂性取决于您的需求和您作为数据科学家的技能。因此，如果你习惯使用 SQL 风格的语法，或者希望提高你的 SQL 语法技能，使用 *pandasql* 可以是一个继续组织你的数据&练习你的技能的好方法。感谢您的阅读！

[领英](https://www.linkedin.com/in/acusio-bivona-7a315818b/)