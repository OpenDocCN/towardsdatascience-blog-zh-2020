# 如何使用 Python(带 SSH)通过 3 个步骤查询 PostgreSQL

> 原文：<https://towardsdatascience.com/how-to-query-postgresql-using-python-with-ssh-in-3-steps-cde626444817?source=collection_archive---------13----------------------->

![](img/c1c300d5a95b10e3037623bdc47a08d5.png)

图片由 Pixabay 提供

*-这个故事描述了如何使用[这个脚本](https://github.com/eyan02/simple_sqlalch_pgres)在有或没有 SSH 的情况下快速建立与远程 PostgreSQL 数据库的连接。pem 认证。-*

数据通常存储在远程服务器上的 PostgreSQL 等数据库中，这使得数据分析师很难快速访问数据。有时，有中间团队协助分析师从数据库中检索数据。在这个故事中，我们将讨论如何使用 Python 直接查询数据，而不需要中间媒介。

如果你知道如何使用 Python(主要用于分析和可视化)，但没有数据库经验或如何与它们交互，那么这篇文章就是为你准备的！

有各种各样的工具供我们使用，使您能够自己从数据库中交互/检索数据，并花更多的时间来提取见解。

*   **第一步:**安装所有需要的包。
*   **第二步:**将 *query.py* 的内容导入或粘贴到笔记本中。
*   **第三步:**开始查询！

**这个故事假设如下:**

*   您已经在本地环境中安装了 Python
*   您正在使用像 Jupyter 笔记本这样的实时代码环境
*   已经获得了 SSH 到远程服务器的必要凭证。pem 证书)并查询 PostgreSQL 数据库(用户名和密码)。

## 步骤 1:安装所有必需的软件包:

首先，我们需要从终端安装几个软件包。

```
pip3 install paramiko
pip3 install sshtunnel
pip3 install SQLAlchemy
pip3 install pandas
```

## 步骤 2:将 query.py 内容导入或粘贴到笔记本中

接下来，您需要将 repo 中的 query.py 文件保存到您的工作目录(Jupyter 笔记本文件所在的目录)中，或者直接将该文件的内容复制到您的笔记本中。

如果将 query.py 文件放在工作目录中，则在笔记本中包含以下导入行:

```
from query.py import *
```

或者，只需将下面的代码复制并粘贴到笔记本的代码单元格中:

## 第三步:查询！

现在我们可以开始查询了！定义的类只提供了少量的基本函数。让我们看一下如何使用这个类，以及我们可以用它做什么。

首先，我们需要指定 PostgreSQL 连接参数和 SSH 参数(如果访问远程服务器需要 SSH 隧道)。

我们将 *pgres* 定义为我们的连接，以简化每次我们想要查询数据库或探索数据库的组织结构。还会提示您输入 PostgreSQL 用户名和密码，它们存储为临时变量(最佳实践是将这些变量保存为环境变量)。

接下来，我们可以探索名为' *database_name* '的给定数据库的模式，使用*找到我们感兴趣的模式。schemas()* 函数。

如果我们想探索名为' *schema_name* '的模式，我们可以使用*返回模式中的表名。*tables()【函数。

最后可以用*。query()* 运行标准的 SQL 查询(针对 PostgreSQL)。在本例中，我们从名为' *ey_test_table* '的表中查询列名和数据类型

尝试用您自己的查询替换 *sql_statement* 的内容，并从中获得乐趣！