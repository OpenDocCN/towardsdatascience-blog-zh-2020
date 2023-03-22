# 熊猫数据准备的基本命令

> 原文：<https://towardsdatascience.com/essential-commands-for-data-preparation-with-pandas-ed01579cf214?source=collection_archive---------25----------------------->

![](img/f8167b6f95b2eec4c8c55510d9729c65.png)

由[卡洛琳·阿特伍德](https://unsplash.com/@carolineattwood?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

## 熊猫“小抄”争夺数据

如果你喜欢烹饪，你会非常了解这一点。打开炉子烹饪食物是整个烹饪过程中很小的一部分。你的大部分汗水和泪水实际上都花在了准备正确的配料上。

老生常谈，但值得再说一遍——数据准备是任何数据科学项目中 80%的工作。无论是制作仪表板、简单的统计分析，还是拟合高级机器学习模型，都是从找到数据并将其转换为正确的格式开始，这样算法就可以处理其余的事情。

如果你是 Python 爱好者，那么`pandas`就是你数据科学之旅中最好的朋友。配备了所有的工具，它可以帮助您完成项目中最困难的部分。

也就是说，像任何新工具一样，你首先需要了解它的功能以及如何使用它们。许多数据科学的初学者仍然很难充分利用 Pandas，而是将大量时间花在堆栈溢出上。我认为，这主要是因为无法将 Pandas 的功能与其分析需求相匹配。

只要列出典型的数据准备问题清单，并为它们配备合适的 Pandas 工具，就可以解决大部分问题。下面，我将介绍一个典型的数据准备和探索性分析工作流程，以及必要的 Pandas 功能。我并不是要记录下世界上所有关于熊猫的事情，而是演示创建你自己的数据争论备忘单的过程。

# 建立

启动您最喜欢的 Python IDE 后，您可能想马上开始并导入必要的库。这很好，但是您仍然需要设置您的环境来设置工作目录、定位数据和其他文件等。

```
# find out your current directory
import os
os.getcwd()# if you want to set a different working directory
os.chdir("folder-path")# to get a list of all files in the directory
os.listdir()
```

# 数据导入

接下来是数据导入，这是你第一次使用熊猫。

您的数据可能位于世界上的任何地方—您的本地机器、SQL 数据库、云中甚至在线数据库中。数据可以保存为各种格式— csv、txt、excel、sav 等。

根据数据的来源和文件扩展名，您需要不同的 Pandas 命令。下面是几个例子。

```
# import pandas and numpy libraries
import pandas as pd
import numpy as np# import a csv file from local machine
df = pd.read_csv("file_path")# import a csv file from an online database
df = pd.read_csv("[https://raw.githubusercontent.com/uiuc-cse/data-fa14/gh-pages/data/iris.csv](https://raw.githubusercontent.com/uiuc-cse/data-fa14/gh-pages/data/iris.csv)")
```

## 数据检查

在导入数据后，您想要检查一些东西，如列和行的数量，列名等。

```
# description of index, entries, columns, data types, memory info
df.info() # check out first few rows
df.head(5) # head# number of columns & rows
df.shape # column names
df.columns # number of unique values of a column
df["sepal_length"].nunique()# show unique values of a column
df["sepal_length"].unique()# number of unique values alltogether
df.columns.nunique()# value counts
df['species'].value_counts()
```

## 处理 NA 值

接下来，检查 NA、NaN 或缺失值。一些算法可以处理缺失值，但其他算法要求在将数据投入使用之前处理缺失值。无论如何，检查缺失值并理解如何处理它们是“了解”数据的重要部分。

```
# show null/NA values per column
df.isnull().sum()# show NA values as % of total observations per column
df.isnull().sum()*100/len(df)# drop all rows containing null
df.dropna()# drop all columns containing null
df.dropna(axis=1)# drop columns with less than 5 NA values
df.dropna(axis=1, thresh=5)# replace all na values with -9999
df.fillna(-9999)# fill na values with NaN
df.fillna(np.NaN)# fill na values with strings
df.fillna("data missing")# fill missing values with mean column values
df.fillna(df.mean())# replace na values of specific columns with mean value
df["columnName"] = df["columnName"].fillna(df["columnName"].mean())# interpolation of missing values (useful in time-series)
df["columnName"].interpolate()
```

## 列操作

通常情况下，您可能需要执行各种列操作，例如重命名或删除列、对列值进行排序、创建新的计算列等。

```
# select a column
df["sepal_length"]# select multiple columns and create a new dataframe X
X = df[["sepal_length", "sepal_width", "species"]]# select a column by column number
df.iloc[:, [1,3,4]]# drop a column from dataframe X
X = X.drop("sepalL", axis=1)# save all columns to a list
df.columns.tolist()# Rename columns
df.rename(columns={"old colum1": "new column1", "old column2": "new column2"})# sorting values by column "sepalW" in ascending order
df.sort_values(by = "sepal_width", ascending = True)# add new calculated column
df['newcol'] = df["sepal_length"]*2# create a conditional calculated column
df['newcol'] = ["short" if i<3 else "long" for i in df["sepal_width"]] 
```

## 行操作(排序、过滤、切片)

到上一节为止，您已经清理了大部分数据，但数据准备的另一个重要部分是切片和过滤数据，以进入下一轮分析管道。

```
# select rows 3 to 10
df.iloc[3:10,]# select rows 3 to 49 and columns 1 to 3
df.iloc[3:50, 1:4]# randomly select 10 rows
df.sample(10)# find rows with specific strings
df[df["species"].isin(["Iris-setosa"])]# conditional filtering
df[df.sepal_length >= 5]# filtering rows with multiple values e.g. 0.2, 0.3
df[df["petal_width"].isin([0.2, 0.3])]# multi-conditional filtering
df[(df.petal_length > 1) & (df.species=="Iris-setosa") | (df.sepal_width < 3)]# drop rows
df.drop(df.index[1]) # 1 is row index to be deleted
```

## 分组

最后但同样重要的是，您通常需要按不同的类别对数据进行分组——这在探索性数据分析和深入了解分类变量时尤其有用。

```
# data grouped by column "species"
X = df.groupby("species")# return mean values of a column ("sepal_length" ) grouped by "species" column
df.groupby("spp")["sepal_length"].mean()# return mean values of ALL columns grouped by "species" category
df.groupby("species").mean()# get counts in different categories
df.groupby("spp").nunique() 
```

## 摘要

这篇文章的目的是展示一些必要的 Pandas 函数，这些函数用于数据分析。在这次演示中，我遵循了一个典型的分析过程，而不是以随机的方式显示代码，这将允许数据科学家在项目中以正确的顺序找到正确的工具。当然，我不打算展示处理数据准备中的每个问题所需的每个代码，而是展示如何创建一个基本的 Pandas cheatsheet。

希望这有用。如果你喜欢这篇文章，请在 Twitter 上关注我。