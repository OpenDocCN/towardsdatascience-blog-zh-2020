# 熊猫数据科学基础

> 原文：<https://towardsdatascience.com/pandas-essentials-for-data-science-15378edef18b?source=collection_archive---------35----------------------->

## 数据导入和检查、缺失值、列和行操作等等

![](img/116fa2cff681352193f3dcce5e4330c8.png)

照片由[马腾·范登赫维尔](https://unsplash.com/@mvdheuvel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Python 是数据科学中的一种流行语言，当然，*也是生产中最流行的机器学习语言。*

然而，如果你看一下数据科学、分析和商业智能在各行各业和学术界的整体情况，你会发现 Python 有机会利用新的工具和技术成长。

作为一个例子，时间序列分析在 R 环境中取得了巨大的进展。这是因为它进入像`fpp2`这样的丰富库的门槛很低。Python 在时间序列的某些领域仍然很受欢迎，但在预测领域要达到与 T1 相当的水平还有很长的路要走。

首先，它需要越来越多对用 Python 做数据科学感兴趣的从业者。对于新手来说，最重要的是确保进入门槛低。

这篇文章的目的是谈论 Python `pandas`的实用程序来设置您的数据分析环境。这是为了展示学习少量命令就能开始更高级的建模。

# 熊猫是什么？

`pandas`是 Python 中一个功能强大、灵活易用的数据挖掘库。它最初是在一家财务管理公司开发的。任何熟悉金融行业的人都知道，金融行业的许多数据科学实际上是时间序列分析。

事实上，熊猫这个名字来源于 *panel* *data* ，是计量经济学中使用的一种特殊类型的时间序列数据。如果你对计量经济学及其在数据科学中的应用感兴趣，看看这个:

[](/econometrics-101-for-data-scientists-584f4f879c4f) [## 数据科学家的计量经济学 101

### 数据科学家如何利用经济学家的工具箱

towardsdatascience.com](/econometrics-101-for-data-scientists-584f4f879c4f) 

# 熊猫争夺数据

数据科学家花费大量时间(有人说是 80%)争论和准备数据进行分析。由于`pandas`是专门为满足分析管道的这一部分而设计的，如果你知道它是如何工作的，以及如何最好地利用它进行数据准备，剩下的就很容易了。

因此，这是一条让数据分析做好准备的分析管道——从基本分析到高级建模。

## **导入数据**

第一件事当然是安装`pandas`库。

```
import pandas as pd
```

现在，您可以从各种来源导入数据。它可以是你电脑上的文件，网页上的文本，或者通过查询 SQL 数据库。

数据也有多种格式，如 csv、excel、json 等。

所以知道从哪里导入以及它是哪种格式将决定使用什么命令。这里有几个例子。

```
# improt a csv from the local machine or from the web
df = pd.read_csv("Your-Data-Path.csv")# importing an excel file from the computer
df = pd.read_excel("Your-Data-Path.xlsx")
```

## 数据检查

导入数据后，您需要检查一些东西，如数据结构、行数和列数、唯一值、NaN 值等。

```
# description of index, entries, columns, data types, memory info
df.info()# know the number of rows and columns
df.shape# check out first few rows
df.head()# if too many columns, list all of them
df.columns# number of unique values of a column
df["column_name"].nunique()# show all unique values of ONE column
df["column_name"].unique()# number of unique values in ALL columns altogether
df.columns.nunique()
```

## 缺少值

数据集中缺少值并不奇怪。首先，您需要检查是否有丢失的值:

```
# checking out number of missing values in each column
df.isnull().sum()# number of missing values as a percentage of total observations
df.isnull().sum()*100/len(df)
```

现在，一旦您发现有缺失值，您可以做一些事情—删除缺失值行、删除整个列、替换值—所有这些都取决于您的分析/建模需求。以下是一些基本命令:

```
# drop all rows containing null
df.dropna()# fill na values with strings
df.fillna("data missing")# fill na values with mean of columns
df.fillna(df.mean())
```

我写了一整篇关于处理缺失值的文章，如果你想看的话:

[](/dealing-with-missing-data-in-data-science-projects-e8ac7a4efdff) [## 处理数据科学项目中的缺失数据

### 如何不因丢失数据而丢失有价值的信息

towardsdatascience.com](/dealing-with-missing-data-in-data-science-projects-e8ac7a4efdff) 

## 列操作

我所说的列操作是指几件事情中的一件——选择列、删除列、重命名、添加新列、排序等等。在高级分析中，您可能希望创建一个基于现有列计算的新列(例如，基于现有的“出生日期”列创建“年龄”)。

```
# select a column by name
df["column_name"]# select multiple columns by column name
df[["column_name1", "column_name2"]] # notice the double brackets# select first 3 columns based on column locations
df.iloc[:, 0:4]# select columns 1, 2, 5
df.iloc[:, [1, 2, 5]]# drop a column
df.drop("column_name", axis = 1)# create a list of all columns in a dataframe
df.columns.tolist()# rename a column
df.rename(columns = {"old_name": "new_name"})# create a new column by multiplying an old column by 2
df["new_column_name"] = df["existing_column"] * 2# sorting a column value in an ascending order
df.sort_values(by = "column_name", ascending = True)
```

## 行操作

一旦你处理好了列，接下来就是行。出于多种原因，您需要使用行，最明显的是过滤或切片数据。此外，您可能希望在数据框架中添加新的观察值或移除一些现有的观察值。以下是过滤数据所需的一些命令:

```
# select rows 3 to 10
df.iloc[3:10, ]# select 3 to 10 rows AND columns 2 to 4
df.iloc[3:10, 2:5]# take a random sample of 10 rows 
df.sample(10)# select rows with specific string
df[df["colum_name"].isin(["Batman"])]# conditional filtering: filter rows with >5 
df.query("column_name > 5")
```

## 特例:准备时间序列数据

时间序列是一种不同的对象，不像*的任何*数据框架。原始数据通常没有为时间序列分析进行格式化，因此`pandas`库将它们视为普通的数据帧，其中时间维度存储为字符串，而不是一个`datetime`对象。因此，您需要将普通的数据帧转换为时间序列对象。

```
# convert Date column to a datetime object
df["Date"] = pd.to_datetime(df["Date"])# set Date as the index
df = df.set_index("Date")# add new columns by splitting index
df["Year"] = df.idex.year
df["Month"] = df.index.month
df["Weekday"] = df.index.weekday_name
```

关于时间序列数据准备的更多信息，这里有一篇文章:

[](/preparing-data-for-time-series-analysis-cd6f080e6836) [## 为时间序列分析准备数据

### 一些简单的技巧和窍门来获得分析就绪的数据

towardsdatascience.com](/preparing-data-for-time-series-analysis-cd6f080e6836) 

## 一锤定音

每个人都从不同的起点开始他们的数据科学之旅。然而，理解的程度和达到目标所需的时间有很大的不同，因为每个人采取不同的学习途径。如果有人遵循逻辑学习过程，使用 Python 学习数据争论的基础知识应该不难。在本文中，我用一些最常用的命令概述了逻辑顺序——数据导入、检查、缺失值、列操作、行操作。我希望这对您的数据科学之旅有用。