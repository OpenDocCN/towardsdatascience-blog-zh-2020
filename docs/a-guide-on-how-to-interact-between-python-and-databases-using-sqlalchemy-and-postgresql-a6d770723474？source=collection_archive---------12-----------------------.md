# 关于如何使用 SQLAlchemy 和 PostgreSQL 在 Python 和数据库之间进行交互的指南

> 原文：<https://towardsdatascience.com/a-guide-on-how-to-interact-between-python-and-databases-using-sqlalchemy-and-postgresql-a6d770723474?source=collection_archive---------12----------------------->

![](img/b4de7ae1df32d072b06402936e19b54e.png)

来源: [Toa Heftiba](https://unsplash.com/@heftiba) ， [unsplash](https://unsplash.com/photos/_UIVmIBB3JU)

## 使用 SQLAlchemy Python 库解释 Python 和 PostgreSQL 之间的联系，以及如何使用它的一些提示。

在进行数据科学项目时，您可能希望将 Python 脚本与数据库连接起来。一个名为 SQLAlchemy 的库在 SQL 和 Python 之间架起了一座桥梁。

存储数据库的一种方法是使用关系数据库管理系统，这是一种常见的数据库类型，数据存储在表中。但是，托管这些数据库需要软件。PostgreSQL 是允许这样做的开源软件。虽然不是必须的，但是在处理大量不同的复杂数据库时，可视化工具非常有用。可视化工具可以让您清楚地了解正在使用的数据库。在这些工具中，我选择了 TablePlus(Mac 版)和 phpMyAdmin(Windows 版)。

在本文中，我将向您展示配置您的系统需要遵循的不同步骤，以及在使用 Postgres 和 SQLAlchemy 时您需要知道的一些命令。

# 1.下载 SQLAlchemy 和 PostgreSQL

要安装 SQLAlchemy，您需要在计算机的终端中输入以下命令:

```
pip install SQLAlchemy
```

要安装 PostgreSQL，你需要进入那个[网站](https://www.postgresql.org/download/)。我也给你一个 [mac](https://www.google.com/search?rlz=1C5CHFA_enFR857FR858&biw=1440&bih=821&ei=cdi9Xo61M4Pi-gT1kJSQCg&q=install+postgresql+mac+tutorial&oq=install+postgresql+mac+tuto&gs_lcp=CgZwc3ktYWIQAxgAMggIIRAWEB0QHjoECAAQRzoECAAQQzoCCAA6BggAEBYQHjoFCCEQoAFQqxtYxyBg-SdoAHABeACAAXKIAdcDkgEDNC4xmAEAoAEBqgEHZ3dzLXdpeg&sclient=psy-ab#kpvalbx=_g9i9Xu0Wwcr6BNaGtvAL40) 和 [windows](https://www.youtube.com/watch?v=e1MwsT5FJRQ) 的教程。**在配置 PostgreSQL 时记下您使用的登录名和密码是非常重要的，因为您将在与 TablePlus 和 Python 的连接中需要它。**

# 2.下载可视化工具

它们是许多不同的可视化工具，但并不是所有的都有免费版本。此外，无论你用的是 mac 还是 window，用来可视化数据库的软件都会有所不同。我选择了 TablePlus，因为它非常容易使用，免费版本也非常好。[这里](https://tableplus.com/)是下载 TablePlus 的链接。

现在软件已经下载完毕，需要在 tablePlus 和 PostgreSQL 之间创建一个连接。[这里](https://help.scalegrid.io/docs/postgresql-connecting-to-gui-tableplus)是关于如何在 TablePlus 上做到的教程。

# 3.在 PostgresSQL 中创建数据库

**在通过 SQLAlchemy 使用 Python 与数据库交互之前，我们需要在 PostgreSQL** 中创建数据库。在这一节中，我还将向您展示一些与 PostgreSQL 交互的命令。

首先，您需要通过在终端中键入以下命令来连接到 PostgreSQL 服务器:

```
psql -U postgres
```

然后，你可以创建你的数据库，我们称之为“飞行”(避免使用大写字母)。

```
CREATE DATABASE flight*;*
```

既然已经创建了数据库，那么就可以创建表，使用 PostgresSQL 或 SQLAlchemy 插入值。我将向你展示这两种方法。

# 4.使用 PostgresSQL 创建表和插入值

您必须首先连接到要使用的数据库。以下命令必须在最后一节中的命令之后在同一终端中执行。所以这里我们要连接到数据库“航班”:

```
\connect flight;
```

现在我们想创建一个带有一些属性的表。我将把这个表称为“flights ”,其属性是航班的 **id** 、出发地、目的地**和持续时间**。

```
CREATE TABLE flights (
    id SERIAL PRIMARY KEY,
    origin VARCHAR NOT NULL,
    destination VARCHAR NOT NULL,
    duration INTEGER NOT NULL
);
```

如果要创建没有任何属性的表，需要输入以下命令:

```
CREATE TABLE flights();
```

现在我们已经创建了属性，我们需要插入一些值。但是，要插入值，您需要先创建属性。

```
INSERT INTO flights (origin, destination, duration) VALUES ('New York', 'London', 415);
INSERT INTO flights (origin, destination, duration) VALUES ('Shanghai', 'Paris', 760);
INSERT INTO flights (origin, destination, duration) VALUES ('Istanbul', 'Tokyo', 700);
INSERT INTO flights (origin, destination, duration) VALUES ('New York', 'Paris', 435);
```

然后，您可以在 TablePlus 中可视化您的表，您应该看到:

![](img/13b7cb6b05ec1cb651f3435acaf58814.png)

现在我将向您展示 Python 中的命令以及如何使用 SQLAlchemy 库。

# 5.使用 SQLAlchemy 创建表并插入值

如前所述，在与 Python 建立连接之前，您需要在 PostgreSQL 中创建一个数据库。因此，在本节中，所有代码都必须在 Python 脚本中实现。
首先要做的是创建一个引擎，它是一个用于管理数据库连接的对象。

```
from sqlalchemy import create_engine
from sqlalchemy.orm import scoped_session, sessionmakerengine = create_engine("postgresql://login:password@localhost:5432/flight")
```

创建引擎的一般格式是:
**create _ engine(" PostgreSQL://log in:passeword @ localhost:5432/name _ database ")**

在这里，由于我们想要在数据库 flight 上工作，我们将 **flight** 替换为 **name_database** 。*登录*和*密码*对应的是你设置 PostgreSQL 时的那个。并且 *5432* 是端口，但是因为它是本地服务器，所以它是 5432。

然后，我们需要创建一个表和属性，还要向该表中插入值。首先，我们需要通过点击下面一行来创建一个作用域会话:

```
db = scoped_session(sessionmaker(bind=engine))
```

它与我们的应用程序并不十分相关，但在很多例子中却非常重要。想象一下这样一种情况，当我们将 web 应用程序放到互联网上，有多人同时尝试使用我们的网站时，我们希望确保 A 对数据库的操作与 B 对数据库的操作是分开的。

接下来，我们需要运行下面一行来创建一个表 **flights** ，其属性为 **id、** **出发地**、**目的地**和**持续时间**。小心，语法与 PostgreSQL 中的有点不同:我们需要在末尾声明密钥。

```
# to create the table flights with all the attributes
db.execute("CREATE TABLE test (id INTEGER NOT NULL, origin VARCHAR NOT NULL, destination VARCHAR NOT NULL, duration INTEGER NOT NULL, PRIMARY KEY (id));")# to insert values into the table flights 
db.execute("INSERT INTO flights (origin, destination, duration) VALUES ('New York', 'London', 415);")db.execute("INSERT INTO flights (origin, destination, duration) VALUES ('Shanghai', 'Paris', 760);")db.execute("INSERT INTO flights (origin, destination, duration) VALUES ('Istanbul', 'Tokyo', 700);")db.execute("INSERT INTO flights (origin, destination, duration) VALUES ('New York', 'Paris', 435);")
```

然后，为了在 PostgreSQL 服务器上执行，我们需要提交更改:

```
db.commit()
```

在最后一步中，我们结束了会话:

```
db.close()
```

因此，整个代码总结如下:

```
from sqlalchemy import create_engine
from sqlalchemy.orm import scoped_session, sessionmaker# creation of an engine
engine = create_engine("postgresql://login:password@localhost:5432/flight")# creation of a session
db = scoped_session(sessionmaker(bind=engine))# creation of the table flights with all the attributes
db.execute("CREATE TABLE test (id INTEGER NOT NULL, origin VARCHAR NOT NULL, destination VARCHAR NOT NULL, duration INTEGER NOT NULL, PRIMARY KEY (id));")# insertion of values into the table flights 
db.execute("INSERT INTO flights (origin, destination, duration) VALUES ('New York', 'London', 415);")db.execute("INSERT INTO flights (origin, destination, duration) VALUES ('Shanghai', 'Paris', 760);")db.execute("INSERT INTO flights (origin, destination, duration) VALUES ('Istanbul', 'Tokyo', 700);")db.execute("INSERT INTO flights (origin, destination, duration) VALUES ('New York', 'Paris', 435);")# commit the changes 
db.commit()# close the session
db.close()
```

# 6.SQLAlchemy 中的一些实用方法

可以想象，如果您有一个 10000 行的 csv 文件，逐行插入值会非常漫长和痛苦。幸运的是，pandas 库能够将 pandas 数据帧转换成 SQL 表。此外，pandas 理解元素的类型，因此在创建表时不再需要指定类型，但是我建议仍然输入类型。**了解你正在处理的元素的类型极其重要**。

为了展示它是如何工作的，我使用了这个数据集。该数据集是关于 2015 年美国航班的。你可以在这里找到数据集。

我们将在相同的数据库上工作，而不是以前的飞行。我们将创建一个名为“airlines_2015”的表。
进口熊猫作为 pd

```
from sqlalchemy import create_engine
from sqlalchemy.orm import scoped_session, sessionmakerengine = create_engine("postgresql://login:password@localhost:5432/flight")db = scoped_session(sessionmaker(bind=engine))db.execute("CREATE TABLE airlines_2015();")dataframe.to_sql('airlines_2015', engine, if_exists='replace')db.commit()
db.close()
```

# 7.PostgreSQL 中的一些实用技巧

在这一节中，我将为您提供一些命令，您可以在终端中输入这些命令来与数据库进行交互。但首先，我将简要回顾一下我们到目前为止所看到的内容。

连接到 PostgreSQL 服务器:

```
psql -U postgres
```

创建数据库:

```
CREATE DATABASE *name_database ;*
```

连接到数据库:

```
\connect *name_database* ;
```

所有数据库的列表:

```
\l
```

创建表格:

```
CREATE TABLE name_table();
```

在下一个命令中，您需要连接到您想要查看的表的数据库:

```
\dt
```

如果您想要一个表格的描述:

```
\d table_name
```

最后，如果您想要关闭一些仍然打开的连接，您需要打开一个新的终端并运行以下命令(在每一行之后按 enter，因此您总共需要按 3 次 enter):

```
# first command
psql -U postgres# second command
\connect *nom_database*;# third command 
select pg_terminate_backend (pg_stat_activity.pid) from pg_stat_activity where datname = current_database() and pid <> pg_backend_pid();
```

# 8.结论和提示

我希望你在这篇文章中找到了你想要的东西。在从事数据科学项目时，您可能希望处理大量数据，并且需要通过创建数据库来组织数据。创建清晰架构的一种方法是绘制 UML 图。

如果你喜欢阅读这样的故事，并想支持我成为一名作家，考虑注册成为一名灵媒成员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你使用[我的链接](https://medium.com/@jonathan_leban/membership)注册，我将赚取一小笔佣金，你仍需支付 5 美元。谢谢大家！！

[](https://medium.com/@jonathan_leban/membership) [## 通过我的推荐链接加入媒体-乔纳森·莱班

### 阅读乔纳森·莱班的每一个故事(以及媒体上成千上万的其他作家)。您的会员费直接支持…

medium.com](https://medium.com/@jonathan_leban/membership) 

*PS:我现在是柏克莱大学的工程硕士，如果你想讨论这个话题，请随时联系我。* [*这里的*](http://jonathan_leban@berkeley.edu/) *是我的邮箱。*