# cx_Oracle 的基础知识

> 原文：<https://towardsdatascience.com/the-basics-of-cx-oracle-92d70681917b?source=collection_archive---------34----------------------->

## 想用 Python 处理 Oracle 数据库？这篇文章可能会帮助你！

![](img/63dcb8fb73a172d6de1a2a640f0ba6c8.png)

泰勒·维克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

最近我一直在使用 Oracle 数据库和 Python。虽然 Oracle 是一家大公司，但我发现没有一个如此强大的社区来帮助他们处理数据库的 Python 包 cx_Oracle，而且我发现他们的文档也不是很好。

所以，我决定按照我的写作哲学写这篇文章，一个月前的 Thiago(我)会感谢我的。

# **1。连接到数据库**

首先要做的是连接到您的数据库。在这种情况下，您可能有一个用户和一个密码。此外，您应该有数据源名称。有了所有这些，您就可以连接到数据库:

很简单！

# **2。执行 SQL 命令**

使用 cx_Oracle，您可以执行任何 Oracle SQL 命令、选择、插入、更新等。为了执行命令，您必须创建一个游标。您可以使用同一个光标运行多个命令。我只是建议您在应用程序中为每个函数使用一个连接和一个光标。所以，你在函数的开头创建一个连接和一个游标，然后在函数的结尾关闭它们。

# 2.1 选择语句

这里需要提到的是，[**fetchone**](https://cx-oracle.readthedocs.io/en/latest/api_manual/cursor.html#Cursor.fetchone)**()**和 [**fetchall()**](https://cx-oracle.readthedocs.io/en/latest/api_manual/cursor.html#Cursor.fetchall) 方法只有在执行“选择”时才起作用。在执行 insert 或 update 语句时使用这些方法是没有意义的。此外，需要注意的是，如果您的查询非常大，[](https://cx-oracle.readthedocs.io/en/latest/api_manual/cursor.html#Cursor.fetchall)****可能会非常慢，因为它会将查询的数据加载到内存中。****

# ****2.2 插入语句****

****如果要将机器中的数据插入数据库的表中，请遵循类似的过程:****

****还有，如果要插入几行，与其多次运行[**【execute()**](https://cx-oracle.readthedocs.io/en/latest/api_manual/cursor.html#Cursor.execute)，不如使用[**execute many**](https://cx-oracle.readthedocs.io/en/latest/api_manual/cursor.html#Cursor.executemany)**():******

> ****这段代码只需要从客户端到数据库的一次 [**往返**](https://cx-oracle.readthedocs.io/en/latest/user_guide/tuning.html#roundtrips) ，而不是重复调用 [**execute()**](https://cx-oracle.readthedocs.io/en/latest/api_manual/cursor.html#Cursor.execute) 所需的五次往返。对于非常大的数据集，对于可以处理的行数，可能存在外部缓冲区或网络限制，因此可能需要重复调用[**execute 多次**](https://cx-oracle.readthedocs.io/en/latest/api_manual/cursor.html#Cursor.executemany) **()** 。这些限制基于正在处理的行数以及正在处理的每行的“大小”。多次调用[**execute 很多**](https://cx-oracle.readthedocs.io/en/latest/api_manual/cursor.html#Cursor.executemany) **()** 还是比多次调用 [**execute()**](https://cx-oracle.readthedocs.io/en/latest/api_manual/cursor.html#Cursor.execute) 要好。****

****现在，如果您要插入 select 语句的结果，最好先创建一条语句，用[**execute()**](https://cx-oracle.readthedocs.io/en/latest/api_manual/cursor.html#Cursor.execute)**执行它，然后执行 select 语句， [**fetchall()**](https://cx-oracle.readthedocs.io/en/latest/api_manual/cursor.html#Cursor.fetchall) ，然后用[**execute many()**](https://cx-oracle.readthedocs.io/en/latest/api_manual/cursor.html#Cursor.executemany)插入。尤其是当 select 语句的结果很大时，这种情况会发生。******

# ****3.使用参数****

****一个非常重要的特性是能够在 SQL 命令中使用参数。cx_Oracle 提供了一种简单的方法来实现这一点:****

****在 cx_Oracle 中，还有其他方法可以向 SQL 命令传递参数，但我认为这是最好的方法，所以我将继续使用它。****

# ****4.使用 cx_Oracle 进行多处理****

****当我使用 cx_Oracle 时，这无疑是最难实现的事情。在我的问题中，我希望在几个表格中进行几次插入，并且希望尽可能快。此外，插入的顺序并不重要，所以使用多重处理是合适的。****

****这里主要的事情是在流程内部创建连接*。进程池中的每个工人都需要有自己的*自己的*连接，这个连接是它自己*建立的*。*****

****记住这一点，在 cx_Oracle 中使用多处理是非常简单的。****

# ****结论****

****我真的不认为我在这里展示了复杂的东西。我很难让我的应用程序工作，因为在 Stackoverflow 上没有太多关于 cx_Oracle 的问题，而且我不认为 cx_Oracle 的文档是最好的。****

****我希望这篇文章能帮助使用 Oracle 数据库和 Python 的人更快地工作。****

****你可以通过访问我的 [GitHub](http://github.com/thiagodma) 查看更多关于我正在做的事情，并通过 [LinkedIn](https://www.linkedin.com/in/thiago-dantas-041b41167/) 取得联系。****

****感谢您的阅读！****