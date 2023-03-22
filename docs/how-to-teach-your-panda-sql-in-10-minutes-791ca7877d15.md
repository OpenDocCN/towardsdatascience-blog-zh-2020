# 如何在 10 分钟内教会你的熊猫 SQL

> 原文：<https://towardsdatascience.com/how-to-teach-your-panda-sql-in-10-minutes-791ca7877d15?source=collection_archive---------32----------------------->

## 将 SQL 查询集成到 Pandas 数据框架的快速概述

![](img/4f4575629c596936133dcff125cb3bef.png)

伊洛娜·弗罗利希在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

对于任何数据科学家或分析师来说，以一种易于理解和有组织的方式管理数据是一项绝对必要的技能。Pandas DataFrames 允许对非常大的数据集进行清晰的视图和操作，同时它也可能是使用最广泛的 Python 包之一。SQL 可能是一个更广泛适用的工具，用于数据库管理和直接从数据库模式中的几个数据表中获取数据。

任何时候捕获、组织或操作数据时，通常都有大量的标准需要考虑。Pandas 通常需要更多的步骤，在一个多步骤的过程中利用各种打包方法和函数来清理数据。SQL 查询需要更多的全局思维，因为构造的查询只需一步就可以获取数据并根据标准进行聚合。

作为一个从 Python 开始然后转向 SQL 的人，我倾向于在清理数据时使用 Pandas，但是将 SQL 查询集成到我的数据清理过程中有助于减轻我的数据工作工具箱中的一些细微差别。让我们看看如何将查询集成到您的 Pandas 数据框架中。

# 导入库(sqlite3)

在 Jupyter 笔记本环境中，我首选的 SQL 查询包是“sqlite3”。让我们用 pandas 和 SQL 导入必要的库进行数据清理:

```
import Pandas as pd
import sqlite3
from pandasql import sqldf
```

如果您的系统尚未安装 pandasql，您必须通过终端安装:

```
pip install pandasql
```

# 基本熊猫查询

Pandas 已经支持一个非常基本的查询选项。通常我们必须对 pandas 数据帧进行切片和索引，但是我们可以通过一个简单的查询很容易地得到相同的结果。

让我们从使用传统的切片语法开始:

```
df2 = df1[df1[(df1[df1[‘Column1'] != df1['Column2']]) OR (df1['Column1'] == 'X')]]
```

请注意这个查询非常复杂，因为它需要多层切片和看似无限多的括号。

这可以很容易地用一个。“query()”方法:

```
df2 = df.query("Column1 != Column2 | Column1 = 'X'")
```

请注意我们是如何显著地精简了必要的语法，并且现在可以清楚地理解我们试图查询的是什么标准。另外，请注意“|”的使用。值得注意的是，在编写查询语法时，我们可以用“|”代替“或”，而用“&”代替“和”。句法

# 使用 pandasql 查询

上面的查询非常简单，因为我们只查询了两个条件。在真正的数据库管理中，会有大量的标准，充满了聚合函数、表连接和子查询。如果我们只是使用。query()方法，所以我们将创建一个函数，以更有组织的方式运行我们的查询。

我们首先创建一个 lambda 函数，它会将全局变量传递给查询对象，这样我们就不必每次运行查询时都这样做。

```
pysqldf = lambda q: sqldf(q, globals())
```

这个函数采用一个查询 q(我们将在稍后编写)和一个通用的 globals()参数来加快查询过程并节省计算开销。

现在我们要做的就是编写我们想要使用的实际查询:

```
q = SELECT c.Name, c.Age, c.Address, o.Income, c.Height
    FROM customers c
    JOIN occupation o
    USING(CustomerId) 
```