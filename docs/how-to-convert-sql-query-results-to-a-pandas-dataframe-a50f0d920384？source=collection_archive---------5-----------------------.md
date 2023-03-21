# 如何将 SQL 查询结果转换为熊猫数据框架

> 原文：<https://towardsdatascience.com/how-to-convert-sql-query-results-to-a-pandas-dataframe-a50f0d920384?source=collection_archive---------5----------------------->

## 使用 Python 读取 Jupyter 笔记本中的 SQL 查询来创建熊猫数据框架

![](img/9ee10d34aec537faf5a52cc8a4455ca5.png)

Tobias Fischer 在[Unsplash](https://unsplash.com/s/photos/database?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上拍摄的照片。

# 目录

1.  介绍
2.  工具
3.  例子
4.  摘要
5.  参考

# 介绍

作为一名数据科学家，您可能经常需要从数据库表中提取数据。建模过程中收集数据集的第一步通常是从 SQL 代码的结果中获取。SQL 通常不是成为数据科学家所需的主要语言；但是，在某些情况下，练习和利用来获取数据集是很重要的。不过，也有一些问题——来回使用 Python 代码、SQL，有时还有 Jupyter Notebook，可能会加重问题。有一个非常简单的过程可以帮助解决这个问题。解决方案是在 Jupyter 笔记本中编写 SQL 查询，然后通过将其转换为 pandas 数据帧来保存输出。下面，我将提供代码和一个例子来展示这个简单而有益的过程。

# 工具

![](img/5c63acf00277c662fb96b2ff7fafea0e.png)

照片由[克里斯里德](https://unsplash.com/@cdr6934?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【2】拍摄。

这个过程由几个关键工具组成。首先，您将使用您原来已经有的 SQL 查询，然后，使用 Python，将引用 pandas 库来将输出转换成数据帧，所有这些都在您的 Jupyter 笔记本中。

**SQL** —结构化查询语言，大多数数据分析师和数据仓库/数据库工程师使用这种语言来拉取数据，用于报表和数据集开发。

```
--return all columns from table
SELECT * FROM TABLE
```

**Python** —数据科学家使用的主要编程语言之一。

```
# display text
print('Hello, world!')
```

**Pandas** —数据科学家用来从各种来源读取数据的流行图书馆。静态数据可以作为 CSV 文件读入。还可以使用 pandas 连接一个活动的 SQL 连接，然后将其从输出转换为数据帧。在下面的例子中对此进行了解释。

```
# creating and renaming a new a pandas dataframe column
df['new_column_name'] = df['original_column_name']
```

**Jupyter Notebook** —为您的数据科学模型运行 Python 代码(以及 SQL)的平台/环境。

# 例子

在这个例子中，我将使用一个模拟数据库作为 SQL 查询将引用的存储环境。

首先，导入 pandas 库，如果需要，创建一个别名' *pd'* ,用于速记符号。接下来，创建一个凭据变量来存储:

*   数据库/SQL 环境— PostgreSQL
*   用户名:密码
*   数据仓库:数据库 URL (IP 地址)
*   通道数
*   数据库名称

这个变量将是一个用引号括起来的长字符串。Jupyter 笔记本中的下一个单元格将是 SQL 查询本身。Pandas 将用于执行查询，同时将输出转换为数据帧。该查询的格式是包含带有三重引号的语句。在最后一个引号之后，逗号后面会跟一个连接参数，该参数将等于您的凭据变量。

> 以下是该示例的代码(您必须使用自己的凭据):

```
# import python library
import pandas as pd# assign a variable that contains a string of your credentials
credentials = "postgresql://username:password@your_sql_connection_url:port_number/database_name"# read in your SQL query results using pandas
dataframe = pd.read_sql("""
            SELECT column_1, column_2
            FROM Table
            WHERE column_1 > number
            ORDER BY column_1
            """, con = credentials)# return your first five rows
dataframe.head()
```

仔细看看 Jupyter 笔记本中的代码:

![](img/fc80003a5e60b68c699f6cf455668de6.png)

Jupyter 笔记本的例子(改为 read_dql 改为 read_sql)。作者截图[3]。

更多关于熊猫功能的文档，点击[这里](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_sql.html)【4】。您还可以在那里找到更多关于参数的有益信息。

# 摘要

当创建将用作数据集的数据框时，有大量选项可用于收集该数据。有时读入 CSV，而数据帧可以通过定义列和值来生成。但是，在本例中，我们看到您可以使用 SQL 从数据库中查询，并将这些结果作为数据返回，最终作为新的数据帧读入。现在你有了数据框架，你就可以遵循数据科学的正常流程了。希望这篇文章对您有所帮助，感谢您的阅读！

# 参考

[1]照片由[托比亚斯·菲舍尔](https://unsplash.com/@tofi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/database?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2017)拍摄

[2]Chris Ried 在 [Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2018)

[3] M.Przybyla，Jupyter 笔记本截图，(2020)

[4]熊猫，(2008 年至 2014 年)