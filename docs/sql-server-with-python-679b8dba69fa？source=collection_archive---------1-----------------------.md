# 使用 Python 的 SQL Server

> 原文：<https://towardsdatascience.com/sql-server-with-python-679b8dba69fa?source=collection_archive---------1----------------------->

## 世界上最喜欢的数据库+世界上最喜欢的语言

![](img/7395ee0d677f0024bd146a8a85f0f868.png)

马丁·桑切斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

大家都用 SQL，大家都用 Python。SQL 是数据库事实上的标准。另一方面，Python 是全明星，是数据分析、机器学习和 web 开发的顶级语言。想象两者都在一起。

这实际上非常容易设置。我们可以快速利用 Python 的动态特性，在 SQL 中控制和构建查询。最精彩的部分？设置完成后，您不需要做任何事情。

这两个惊人的工具一起，让我们达到自动化和效率的新高度。

# pyodbc

我们在两种技术之间的桥梁是`pyodbc`。这个库允许轻松访问 ODBC 数据库。

ODBC 是开放式数据库连接的缩写，是一种用于访问数据库的标准化应用程序编程接口(API ),由 SQL Access group 在 90 年代早期开发。

兼容的数据库管理系统(DBMS)包括:

*   IBM Db2
*   MS Access
*   MS SQL Server
*   关系型数据库
*   神谕

在本文中，我们将使用 MS SQL Server。在很大程度上，这应该可以直接转移到任何兼容 ODBC 的数据库中使用。唯一需要更改的应该是连接设置。

# 连接

我们需要做的第一件事是创建一个到 SQL server 的连接。我们可以使用`pyodbc.connect`来做到这一点。在这个函数中，我们还必须传递一个连接字符串。

这个连接字符串必须指定 DBMS `Driver`、`Server`、要连接的特定`Database`以及我们的连接设置。

所以，让我们假设我们想要连接到服务器`UKXXX00123,45600`，数据库`DB01`，为此我们想要使用`SQL Server Native Client 11.0`。

我们将从内部连接，因此是可信的连接(我们不需要输入用户名和密码)。

```
cnxn_str = ("Driver={SQL Server Native Client 11.0};"
            "Server=UKXXX00123,45600;"
            "Database=DB01;"
            "Trusted_Connection=yes;")
```

我们的连接现在初始化为:

```
cnxn = pyodbc.connect(cnxn_str)
```

如果我们不通过可信连接访问数据库，我们将需要输入通常用于通过 SQL Server Management Studio (SSMS)访问服务器的用户名和密码。

举个例子，如果我们的用户名是`JoeBloggs`，我们的密码是`Password123`，我们应该立即更改密码。

但是在更改那个可怕的密码之前，我们可以这样连接:

```
cnxn_str = ("Driver={SQL Server Native Client 11.0};"
            "Server=UKXXX00123,45600;"
            "Database=DB01;"
            "UID=JoeBloggs;"
            "PWD=Password123;")cnxn = pyodbc.connect(cnxn_str)
```

现在我们连接到数据库，我们可以开始通过 Python 执行 SQL 查询。

# 运行查询

现在，我们在 SQL Server 上运行的每个查询都包括游标初始化和查询执行。此外，如果我们在服务器内部进行任何更改，我们还需要将这些更改提交给服务器(我们将在下一节中讨论)。

要初始化游标，请执行以下操作:

```
cursor = cnxn.cursor()
```

现在，每当我们想要执行查询时，我们就使用这个`cursor`对象。

让我们首先从名为`customers`的表中选择前 1000 行:

```
cursor.execute("SELECT TOP(1000) * FROM customers")
```

这是在服务器内部执行的操作，因此实际上没有任何东西返回给 Python。因此，让我们看看如何从 SQL 中提取这些数据。

# 提取数据

为了将 SQL 中的数据提取到 Python 中，我们使用了`pandas`。Pandas 为我们提供了一个非常方便的函数`read_sql`，这个函数，你可能已经猜到了，从 SQL 中读取数据。

`read_sql`需要一个查询和连接实例`cnxn`，就像这样:

```
data = pd.read_sql("SELECT TOP(1000) * FROM customers", cnxn)
```

这将从`customers`表中返回包含前 1000 行的数据帧。

# 在 SQL 中更改数据

现在，如果我们想要更改 SQL 中的数据，我们需要向原始的初始化连接添加另一个步骤，执行查询过程。

当我们在 SQL 中执行查询时，这些更改保存在临时存在的空间中，它们不是直接对数据进行的。

为了使这些变化永久化，我们必须`commit`它们。让我们连接`firstName`和`lastName`列，创建一个`fullName`列。

```
cursor = cnxn.cursor()# first alter the table, adding a column
cursor.execute("ALTER TABLE customer " +
               "ADD fullName VARCHAR(20)")# now update that column to contain firstName + lastName
cursor.execute("UPDATE customer " +
               "SET fullName = firstName + " " + lastName")
```

此时，`fullName`在我们的数据库中不存在。我们必须`commit`将这些改变永久化:

```
cnxn.commit()
```

# 后续步骤

一旦我们完成了需要完成的操作任务。我们可以将数据提取到 Python 中——或者，我们也可以将数据提取到 Python 中，并在那里进行操作。

无论您采用哪种方法，一旦数据出现在 Python 中，我们就可以用它做许多以前不可能做的有用的事情。

也许我们需要执行一些每日报告，我们通常会用它来查询 SQL server 中最新的一批数据，计算一些基本的统计数据，并通过电子邮件发送结果。

让我们实现自动化:

就这样，我们结束了！运行这段代码可以快速提取前一周的数据，计算我们的关键度量，并向我们的老板发送一份摘要。

因此，通过几个简单易行的步骤，我们已经初步了解了如何使用 SQL 和 Python 的集成来快速建立一个更高效、自动化的工作流。

我发现这非常有用，不仅仅是对于上面描述的用例。

Python 只是打开了我们以前仅用 SQL 无法通过的新路线。

让我知道你的观点、想法或用例，我很乐意听到它们！如果你想要更多这样的内容，我也会在 YouTube 上发布。

感谢阅读！

## [🤖《变形金刚》NLP 课程 70%的折扣](https://bit.ly/nlp-transformers)

如果您喜欢这篇文章，您可能有兴趣更深入地了解 Python 的电子邮件自动化。我在以前的一篇文章中讨论过这个问题，如果您感兴趣，请查看:

[](/notify-with-python-41b77d51657e) [## 用 Python 通知

### 使用 Python 构建的电子邮件通知让生活更轻松

towardsdatascience.com](/notify-with-python-41b77d51657e) 

[这个 GitHub repo](https://github.com/jamescalam/pysqlplus) 还包含文档和代码，展示了一些有用方法的实现。