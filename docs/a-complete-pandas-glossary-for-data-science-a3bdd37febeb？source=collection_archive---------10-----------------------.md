# 完整的熊猫数据科学术语表

> 原文：<https://towardsdatascience.com/a-complete-pandas-glossary-for-data-science-a3bdd37febeb?source=collection_archive---------10----------------------->

## 学习熊猫基础知识的最佳资源

![](img/ce826d4dc083c1a5d3a6f393d61ddba0.png)

像大多数其他人一样，我试图通过新兵训练营学习熊猫——不幸的是，新兵训练营的问题是，如果你不练习你所学的东西，你会很快忘记一切！

与此同时，我发现需要一个中央熊猫资源，以便我在从事个人数据科学项目时可以参考。这就是为什么会有这样的结果。把这个作为学习熊猫的资源，也作为参考！

# 目录

1.  [设置](#fb36)
2.  [创建和读取数据](#2b4b)
3.  [操纵数据帧](#3e9a)
4.  [汇总功能](#f416)
5.  [映射功能](#83dc)
6.  [对变量进行分组和排序](#a263)
7.  [处理缺失数据](#520b)

# 设置

## 导入熊猫库

```
import pandas as pd
```

# 创建和读取数据

## 创建数据框架

数据帧只是一个由多个数组组成的表。在下面的示例中，代码将创建一个包含 ABC 和 DEF 两列的表。

```
pd**.DataFrame**({'ABC':[1,2,3],'DEF':[4,5,6]},index=[1,2,3])
```

## 创建一个系列

系列是一系列值，也称为列表。从视觉角度来看，想象它是表格中的一列。

```
pd**.Series**([1,2,3],index=[], name ='ABC')
```

## **将 CSV 文件读入数据帧**

获取数据的最常见方式。这将 CSV 文件转换为数据帧。

```
# example
df = pd**.read_csv**("filename.csv", index_col=0)
```

## 将数据帧转换为 CSV 文件

反之亦然，如果您想将 DataFrame 转换成 CSV，可以使用下面的代码:

```
# example
df**.to_csv**("filename.csv", index_col=0)
```

## 确定数据帧的形状

这将告诉您数据帧有多大，格式是什么(行，列)。

```
df**.shape()**
```

## 查看数据帧的前 5 行

如果你想对数据帧有一个直观的了解。head() 返回给定数据帧的前 5 行。

```
df**.head()**
```

## 查看一列或多列的数据类型

```
# For one column
df.variable**.dtype**# For all columns
df**.dtypes**
```

## 将列转换为另一种数据类型

如果您想将整数转换成浮点数(反之亦然)，这是很有用的。

```
df.variable**.astype()**
```

# 操作数据帧

## 从数据帧中选择系列

```
# a) Method 1
df.property_name# b) Method 2
df['property_name']
```

## 索引系列

```
# if you want to get the first value in a series
df['property_name'][0]
```

## 基于索引的选择

基于索引的选择根据数据在数据帧中的数字位置检索数据。它遵循行优先，列第二的格式。Iloc 的索引方案是这样的:第一个数字**包含**，最后一个数字**不包含**。

```
df**.iloc[]**
```

## 基于标签的选择

基于标签的选择是索引数据帧的另一种方式，但它基于实际数据值而不是数字位置来检索数据。Loc 的索引方案使得**的第一个和最后一个值都包含在内。**

```
df**.loc[]**
```

## 使用现有列设置索引

因为基于标签的选择依赖于数据帧的索引，所以可以使用**。set_index()** 将一列分配给索引。

```
df.**set_index**("variable")
```

## 基于条件标签的选择

我们也可以使用基于标签的选择来过滤出数据帧。

```
# a) Single Condition 
df.loc[df.property_name == 'ABC']# b) Multiple conditions using AND
df.loc[df.property_name == 'ABC' & df.property_name == 'DEF']# c) Multiple conditions using OR
df.loc[df.property_name == 'ABC' | df.property_name == 'DEF']
```

## 选择值在值列表中的位置

我们也可以使用 **isin()** 来过滤数据帧。如果你懂 SQL，它类似于 WHERE ___ IN()语句。

```
df.loc[df.property_name **isin**(['ABC','DEF'])
```

## 选择值为空/不为空的位置

第一行代码将过滤 DataFrame，只显示属性名为 null 的行。
反之亦然，第二行代码用 filter it 使属性名不为空。

```
df.loc[df.property_name**.isnull**()]df.loc[df.property_name**.notnull()**]
```

## 添加新列

```
df['new_column'] = 'ABC'
```

## 重命名列

您通常会希望将列重命名为更容易引用的名称。使用下面的代码，列 ABC 将被重命名为 DEF。

```
df.**rename**(columns={'ABC': 'DEF'})
```

# 汇总函数

## 。描述()

这给出了数据帧或变量的高级摘要。它是类型敏感的，这意味着与字符串变量相比，数字变量的输出是不同的。

```
df**.describe()**
df.variable**.describe()**
```

## 。平均值()

这将返回变量的平均值。

```
df.variable**.mean()**
```

## 。唯一()

这将返回变量的所有唯一值。

```
df.variable**.unique()**
```

## 。值计数()

这显示了唯一值的列表以及数据帧中出现的频率。

```
df.variable.**value_counts()**
```

# 映射函数

## 。地图()

映射用于通过函数将一组初始值转换为另一组值。例如，我们可以使用映射将列的值从米转换为厘米，或者我们可以将这些值标准化。

。map()用于转换一个序列。

```
df.numerical_variable**.map()**
```

## 。应用()

。apply()类似于。map()，只是它转换整个数据帧。

```
df.numerical_variable**.apply()**
```

# 分组和排序

## 。groupby()

**获取变量的每个值的计数(*与 value_counts* 相同)**

```
df**.groupby**('variable').variable**.count()**
```

**获取变量的每个值的最小值**

```
df**.groupby**('variable').variable**.min()**
```

**获取变量**的每个值的汇总(长度、最小值、最大值)

```
df**.groupby**(['variable']).variable.**agg([len, min, max])**
```

**多重索引**

```
df.groupby(['variable_one', 'variable_two'])
```

## 对数据帧排序

**按一个变量排序**

```
df.**sort_values**(by='variable', ascending=False)
```

**多变量排序**

```
df.sort_values(by=['variable_one', 'variable_two'])
```

**按指标排序**

```
df**.sort_index()**
```

# 处理缺失数据

处理缺失数据是 EDA 中最重要的步骤之一。下面是一些处理缺失数据的方法。

## 计算每列中空值的数量

```
df.isna().sum()
```

## 删除包含空值的行

如果您有一个包含大量行的数据帧，并且您能够完全删除包含空值的行，那么。dropna()是一个有用的工具。

```
df.**dropna()**
```

## 删除包含空值的列

这与上面的类似，除了它删除任何具有空值的**列**而不是行。

```
df.**dropna(axis=1)**
```

## 填充缺失值

如果您希望填充缺少的值，而不是完全删除行或列，可以使用下面的代码:

```
df.variable**.fillna**("n/a")
```

## 替换值

假设有一个数据帧，其中有人已经用“n/a”填充了缺失值，但是您希望用“unknown”填充缺失值。那么您可以使用下面的代码:

```
df.variable**.replace**("n/a", "unknown")
```

# 组合数据

## 。concat()

当您想要合并具有相同列的两个数据帧时，这很有用。例如，如果我们想将一月份的销售额和二月份的销售额结合起来分析长期趋势，您可以使用以下代码:

```
Jan_sales = pd.read_csv("jan_sales.csv")
Feb_sales = pd.read_csv("feb_sales.csv")

**pd.concat**([Jan_sales, Feb_sales])
```

## 。加入()

如果您想要合并具有公共索引的两个列(例如 customer_id)，那么您可以使用。加入()。

使用上的参数**来确定要连接的列。**

要确定它是左连接、右连接、内连接还是外连接，可以使用参数***。***

```
# example
table_1.**join**(table_2, on='customer_id', *how='left')*
```

***如果你不了解 SQL joins，在这里阅读*[](https://www.w3schools.com/sql/sql_join.asp)**。本质上是一样的想法。****

***希望这有帮助！如果你觉得我遗漏了什么或者有什么不清楚的地方，请提供意见。谢谢！***

# ***感谢阅读！***

***如果你喜欢我的工作，想支持我…***

1.  ***支持我的最好方式就是在**媒体**这里[关注我](https://medium.com/@terenceshin)。***
2.  ***在成为第一批在**Twitter**上关注我的人之一。我会在这里发布很多更新和有趣的东西！***
3.  ***此外，成为第一批订阅我的新 **YouTube 频道** [这里](https://www.youtube.com/channel/UCmy1ox7bo7zsLlDo8pOEEhA?view_as=subscriber)！***
4.  ***关注我**LinkedIn**这里。***
5.  ***在我的**邮箱列表** [这里](https://forms.gle/UGdTom9G6aFGHzPD9)报名。***
6.  ***看看我的网站，[**terenceshin.com**](https://terenceshin.com/)。***