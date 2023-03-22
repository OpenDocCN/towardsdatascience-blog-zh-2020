# 熊猫简介

> 原文：<https://towardsdatascience.com/introduction-to-pandas-cc3bc6355155?source=collection_archive---------44----------------------->

![](img/b63328de5d5e89cd22052af1388bcc12.png)

达米安·帕特考斯基在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 使用 Pandas API 增强您的 Python 数据科学技能

andas 是一个用来分析、组织和结构化数据的 API。它在 Python 社区中被广泛接受，并在许多其他包、框架和模块中使用。Pandas 有各种各样的用例，非常灵活地为机器学习、深度学习和神经网络模型准备输入数据。使用 DataFrames 和 Series 等工具，您可以创建多维的集合和序列。这允许更大的索引和切片能力。Pandas 是开源的，是一个 BSD 许可的库。因此，让我们看看如何用 Pandas 构建、操作和组织您的数据。

[](https://pandas.pydata.org/) [## 熊猫

### pandas 是一个快速、强大、灵活且易于使用的开源数据分析和操作工具，构建于…

pandas.pydata.org](https://pandas.pydata.org/) 

本文中我的 GitHub repo 代码可以在这里找到:

[](https://github.com/third-eye-cyborg/Intro_to_Pandas) [## 第三只眼-电子人/熊猫简介

github.com](https://github.com/third-eye-cyborg/Intro_to_Pandas) 

# 推荐的先决条件

[](https://medium.com/python-in-plain-english/a-brief-history-of-the-python-programming-language-4661fcd48a04) [## Python 编程语言的简史

### Python 编程语言是一种通用的编程语言，它已经在主流编程语言中占有一席之地

medium.com](https://medium.com/python-in-plain-english/a-brief-history-of-the-python-programming-language-4661fcd48a04) [](https://medium.com/python-in-plain-english/python-basic-overview-76907771db60) [## Python 基本概述

### Python 有许多独特的特性，这些特性帮助它成为现在的样子。这些功能包括:

medium.com](https://medium.com/python-in-plain-english/python-basic-overview-76907771db60) [](https://medium.com/python-in-plain-english/python-beginners-reference-guide-3c5349b87b2) [## Python 初学者完全参考指南

### Python 是一种很好的初学者语言，但也适合高级用户。我将深入核心…

medium.com](https://medium.com/python-in-plain-english/python-beginners-reference-guide-3c5349b87b2) [](https://medium.com/analytics-vidhya/the-best-ides-and-text-editors-for-python-872ff1176c92) [## Python 的最佳 ide 和文本编辑器

### 我无法告诉你正确的 IDE(集成开发环境)对任何编程项目有多重要。只是…

medium.com](https://medium.com/analytics-vidhya/the-best-ides-and-text-editors-for-python-872ff1176c92) [](/how-to-interact-with-apis-in-python-10efece03d2b) [## 如何在 Python 中与 API 交互

### 本文介绍了如何用 Python 编程语言处理 API 调用。

towardsdatascience.com](/how-to-interact-with-apis-in-python-10efece03d2b) [](/an-overview-of-the-anaconda-distribution-9479ff1859e6) [## Anaconda 发行版概述

### 科学 Python 发行版将改变你研究数据科学的方式。

towardsdatascience.com](/an-overview-of-the-anaconda-distribution-9479ff1859e6) [](/an-overview-of-the-pep-8-style-guide-5672459c7682) [## PEP 8 风格指南概述

### 让您的 Python 代码具有风格。

towardsdatascience.com](/an-overview-of-the-pep-8-style-guide-5672459c7682) [](/exploring-design-patterns-in-python-be55fbcf8b34) [## 探索 Python 中的设计模式

### 如何在您的编程体验中实现可重用模型？

towardsdatascience.com](/exploring-design-patterns-in-python-be55fbcf8b34) 

# 目录

*   [**安装熊猫**](https://medium.com/p/cc3bc6355155#70f9)
*   [**探索熊猫系列**](https://medium.com/p/cc3bc6355155#feba)
*   [**探索熊猫数据帧**](https://medium.com/p/cc3bc6355155#53c9)
*   [**在 Pandas 中处理数据(示例)**](https://medium.com/p/cc3bc6355155#18a9)
*   [**结论**](https://medium.com/p/cc3bc6355155#d196)

# 安装熊猫

Pandas 通过 PIP 或 Anaconda 发行版进行安装。

```
pip install pandas
conda install pandas
```

Pandas 集成了许多表格文件类型，并且擅长通过将这些文件类型转换成 Pandas 数据帧来处理它们。Pandas 中的 DataFrames 是一种特定类型的数据表。您可以操作数据、处理时间序列数据、绘制图表、重塑数据结构、合并数据、排序数据和过滤数据。

您可以将熊猫导入到 Python 代码中，如下所示:

```
import pandas as pd # the 'as pd' part is not necessary but is typically the standard for importing this library. 
```

创建熊猫数据框有多种方法。您可以将字典、列表、表格数据和 Pandas 系列对象转换成数据框架，也可以使用`pd.DataFrame()`方法创建它们。Series 对象就像一个单列数据框架。因此，你可以想象数据帧是一个或多个系列的集合。您可以使用`pd.Series()`创建一个系列对象。

# 探索熊猫系列

熊猫系列的基本构造是这样的。

```
pd.Series([1, 2, 3, 4])
```

通常，您会希望通过分配一个变量名来命名您的系列。

```
my_series = pd.Series([1, 2, 3, 4])
```

您可以将命名的 Pandas 系列分配给数据帧，如下所示。

```
first_series = pd.Series([1, 2, 3, 4])
second_series = pd.Series(['one', 'two', 'three', 'four'])my_df = pd.DataFrame({ 'First': first_series, 'Second': second_series })print(my_df)[out]
   First Second
0      1    one
1      2    two
2      3  three
3      4   four
```

# 探索熊猫数据框架

你可以把许多文件类型转换成熊猫数据帧。这里有一些更常用的方法。

```
[**read_csv**](https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html#pandas.read_csv)(filepath_or_buffer[, sep, …])
[**read_excel**](https://pandas.pydata.org/docs/reference/api/pandas.read_excel.html#pandas.read_excel)(*args, **kwargs)
[**read_json**](https://pandas.pydata.org/docs/reference/api/pandas.read_json.html#pandas.read_json)(*args, **kwargs)
[**read_html**](https://pandas.pydata.org/docs/reference/api/pandas.read_html.html#pandas.read_html)(*args, **kwargs)
```

您还可以使用这些更常用的方法来操作文件类型的数据。

```
# Excel [**ExcelFile.parse**](https://pandas.pydata.org/docs/reference/api/pandas.ExcelFile.parse.html#pandas.ExcelFile.parse)([sheet_name, header, names, …])
[**ExcelWriter**](https://pandas.pydata.org/docs/reference/api/pandas.ExcelWriter.html#pandas.ExcelWriter)(path[, engine])# JSON
[**json_normalize**](https://pandas.pydata.org/docs/reference/api/pandas.json_normalize.html#pandas.json_normalize)(data[, record_path, meta, …])
[**build_table_schema**](https://pandas.pydata.org/docs/reference/api/pandas.io.json.build_table_schema.html#pandas.io.json.build_table_schema)(data[, index, …])
```

您还可以在数据帧上使用一些方法来帮助正确地组织和操作数据。我建议阅读文档，了解所有可以用数据帧做的事情，但这些是一些更常用的方法和属性。

 [## 数据框架- pandas 1.1.3 文档

### 返回表示数据帧维数的元组。

pandas.pydata.org](https://pandas.pydata.org/docs/reference/frame.html) 

```
# constructor [**DataFrame**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html#pandas.DataFrame)([data, index, columns, dtype, copy])[**DataFrame.head**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.head.html#pandas.DataFrame.head)([n])
[**DataFrame.tail**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.tail.html#pandas.DataFrame.tail)([n])
[**DataFrame.values**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.values.html#pandas.DataFrame.values)[**DataFrame.dtypes**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.dtypes.html#pandas.DataFrame.dtypes)[**DataFrame.columns**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.columns.html#pandas.DataFrame.columns)[**DataFrame.size**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.size.html#pandas.DataFrame.size)[**DataFrame.shape**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.shape.html#pandas.DataFrame.shape)[**DataFrame.axes**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.axes.html#pandas.DataFrame.axes) [**DataFrame.index**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.index.html#pandas.DataFrame.index)[**DataFrame.loc**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.loc.html#pandas.DataFrame.loc)[**DataFrame.iloc**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.iloc.html#pandas.DataFrame.iloc)[**DataFrame.keys**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.keys.html#pandas.DataFrame.keys)()
[**DataFrame.filter**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.filter.html#pandas.DataFrame.filter)([items, like, regex, axis])
[**DataFrame.dropna**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.dropna.html#pandas.DataFrame.dropna)([axis, how, thresh, …])
[**DataFrame.fillna**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.fillna.html#pandas.DataFrame.fillna)([value, method, axis, …])
[**DataFrame.sort_values**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.sort_values.html#pandas.DataFrame.sort_values)(by[, axis, ascending, …])
[**DataFrame.sort_index**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.sort_index.html#pandas.DataFrame.sort_index)([axis, level, …])
[**DataFrame.append**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.append.html#pandas.DataFrame.append)(other[, ignore_index, …])
[**DataFrame.join**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.join.html#pandas.DataFrame.join)(other[, on, how, lsuffix, …])
[**DataFrame.merge**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.merge.html#pandas.DataFrame.merge)(right[, how, on, left_on, …])
[**DataFrame.update**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.update.html#pandas.DataFrame.update)(other[, join, overwrite, …])
[**DataFrame.to_period**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_period.html#pandas.DataFrame.to_period)([freq, axis, copy])
[**DataFrame.tz_localize**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.tz_localize.html#pandas.DataFrame.tz_localize)(tz[, axis, level, …])
[**DataFrame.plot**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.plot.html#pandas.DataFrame.plot)([x, y, kind, ax, ….])
[**DataFrame.from_dict**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.from_dict.html#pandas.DataFrame.from_dict)(data[, orient, dtype, …])
[**DataFrame.to_pickle**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_pickle.html#pandas.DataFrame.to_pickle)(path[, compression, …])
[**DataFrame.to_csv**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_csv.html#pandas.DataFrame.to_csv)([path_or_buf, sep, na_rep, …])
[**DataFrame.to_sql**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_sql.html#pandas.DataFrame.to_sql)(name, con[, schema, …])
[**DataFrame.to_dict**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_dict.html#pandas.DataFrame.to_dict)([orient, into])
[**DataFrame.to_excel**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_excel.html#pandas.DataFrame.to_excel)(excel_writer[, …])
[**DataFrame.to_json**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_json.html#pandas.DataFrame.to_json)([path_or_buf, orient, …])
[**DataFrame.to_html**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_html.html#pandas.DataFrame.to_html)([buf, columns, col_space, …])
[**DataFrame.transpose**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.transpose.html#pandas.DataFrame.transpose)(*args[, copy])
```

数据帧有五个参数`data`、`index`、`columns`、`dtype`和`copy`。

数据帧被用作许多机器学习、深度学习和神经网络模型的输入。对 EDA(探索性数据分析)也有好处。至少了解和使用 Pandas 中的基本知识是大多数 Python 数据科学项目的必备条件。

# 在 Pandas 中处理数据(示例)

我将使用 Kaggle 上列出的太阳黑子数据集。月平均太阳黑子总数| 1749—2018 年 7 月

[](https://www.kaggle.com/) [## Kaggle:你的机器学习和数据科学社区

### Kaggle 是世界上最大的数据科学社区，拥有强大的工具和资源来帮助您实现您的数据…

www.kaggle.com](https://www.kaggle.com/) [](https://www.kaggle.com/robervalt/sunspots) [## 雀斑

### 月平均太阳黑子总数-1749 年至 2018 年 7 月

www.kaggle.com](https://www.kaggle.com/robervalt/sunspots) 

作者 GitHub 要点

## 确认:

SIDC 和康德尔。

数据库来自 SIDC——太阳影响数据分析中心——比利时皇家天文台太阳物理研究部。 [SIDC 网站](http://sidc.oma.be/)

[](https://creativecommons.org/publicdomain/zero/1.0/) [## 知识共享- CC0 1.0 通用版

### 此页面有以下语言版本:CC0 1.0 通用版(CC0 1.0)公共领域专用公地…

creativecommons.org](https://creativecommons.org/publicdomain/zero/1.0/) 

# 结论

熊猫库是 Python 中一个非常棒的工具。这篇文章只是介绍了使用 Pandas API 可以完成的事情的冰山一角。当开始使用 Python 处理数据时，您可以开始看到 Pandas 提供的真正功能。学习 Pandas 及其工作原理可以让您更好地控制输入数据，从而提高您在数据科学方面的 Python 体验。这不仅在探索数据时会给你更多的灵活性和能力，而且在直接使用它来实现你的编程、计算或科学目标时也会给你更多的灵活性和能力。我希望这能帮助任何想学习 Python 中的 Pandas API 的人。编码快乐！