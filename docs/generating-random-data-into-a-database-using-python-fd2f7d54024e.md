# 使用 Python 在数据库中生成随机数据

> 原文：<https://towardsdatascience.com/generating-random-data-into-a-database-using-python-fd2f7d54024e?source=collection_archive---------4----------------------->

## 使用 Pandas 用虚拟数据填充 MySQL 数据库

![](img/84d746e911c76cc238aa072ebdc65866.png)

来自 [pixabay](https://pixabay.com/photos/stormtrooper-star-wars-lego-storm-2899993/) 的[艾托夫](https://pixabay.com/users/aitoff-388338/)的照片

在本文中，我们将演示如何为 MySQL 数据库生成虚拟数据和输入。大多数情况下，我们需要创建一个数据库来测试我们构建的一些软件。为此，我们可以使用**的**结构化的 **Q** 查询 **L** 语言(SQL)来创建一个模拟数据库系统。但是，如果我们要测试数百甚至数千条记录，可能会出现问题。我们绝对不可能在我们的 SQL 脚本中编写这么多的 insert 语句来满足产品部署的测试截止日期，那将是非常乏味和耗时的。

为了帮助解决这个问题，我们值得信赖的朋友 ***Python*** 来帮忙了！

**巨蟒来救援了！**

您可能知道也可能不知道，python 是一种脚本语言，以其丰富的库而闻名。Python 还以其使用非常简单的语法自动执行任务的能力而闻名。

**必备知识:**

*   使用 ***pip 安装 python 包。***
*   对什么是数据库以及它们如何使用 SQL 有基本的了解。

即使您不具备上述要求，本文也将确保您理解其内容，以帮助您进一步发展您的 Python 事业。

所以让我们开始吧！

**我们将在这个项目中使用的 python 库是:**

*   [Faker](https://faker.readthedocs.io/en/master/) — *这是一个可以为你生成伪数据的包。我们将用它来生成我们的虚拟数据。*
*   [熊猫](https://pandas.pydata.org/pandas-docs/stable/index.html) —这是一个*数据分析工具。这将用于封装我们的虚拟数据，并将其转换为数据库系统中的表格。*
*   [SQLAlchemy](https://www.sqlalchemy.org/) —这是一个用于 SQL 的*对象关系映射器(ORM)。ORM 将被用作我们的数据库引擎，因为它将为我们的数据库提供连接。*

**我们将使用的其他工具包括:**

*   [XAMPP](https://www.apachefriends.org/index.html) —这是一个包含 MariaDB、PHP 和 Perl 数据库管理系统的*发行版。我们将使用 MariaDB，这是我们的 SQL 数据库。*

**注意:然而，可以使用不同类型的数据库，因为本文将关注 MySQL 连接。所以，对于任何其他数据库如 PostgreSQL、微软 SQL Server、Oracle 等。将需要替代配置。**

# 入门指南

我们开始吧！

首先，我们需要前面提到的那些 python 库。***Pip—P****IP***I*installs***P***packages(简称 Pip)。PIP 是 python 的包管理器，也是我们用来安装 python 包的工具。*

***注:康达也可以用于那些森蚺发行爱好者。***

```
*pip install **pandas Faker sqlalchemy mysqlclient***
```

*我们现在已经使用上面的代码安装了所有相关的包。*

# *导入库*

*首先，我们需要导入相关的库。*

```
*import pandas as pd
from faker import Faker
from collections import defaultdict
from sqlalchemy import create_engine*
```

*`defaultdict`(默认字典)来自 collections 包。这将作为我们的字典，因为它将提供比普通字典更多的功能，我们将在本文后面看到。*

# ***生成虚拟数据***

*为了生成我们的虚拟数据，我们将首先初始化我们将用来获取虚拟数据的`Faker`实例。*

```
*fake = Faker()*
```

*我们将使用`fake_data` 来创建我们的字典。`defaultdict(list)`将创建一个字典，该字典在被访问时将创建当前没有存储在字典中的键值对。本质上，您不需要在字典中定义任何键。*

```
*fake_data = defaultdict(list)*
```

*现在，让我们决定从`Faker`实例中获取哪种数据，并存储在`fake`变量中。这种数据的一个例子包括:*

1.  *西方人名的第一个字*
2.  *姓*
3.  *职业*
4.  *出生日期*
5.  *国家*

*我们可以在伪数据实例中通过不同的方法访问这些数据。*

*我们将要访问的方法是`first_name()`、`last_name()`、 `job()`、`date_of_birth()`和`country()`。*

*我们将循环一千次来访问这些方法，然后将它们添加到我们的字典中。*

```
*for _ in range(1000):
    fake_data["first_name"].append( fake.first_name() )
    fake_data["last_name"].append( fake.last_name() )
    fake_data["occupation"].append( fake.job() )
    fake_data["dob"].append( fake.date_of_birth() )
    fake_data["country"].append( fake.country() )*
```

***注意:因为使用了** `**defaultdict(list)**` **，所以每当我们访问一个不是我们创建的键时，就会自动添加一个空 list []作为它的值。***

*现在，因为我们的字典`fake_data`中有所有的随机数据。我们需要将这些数据打包到我们的熊猫数据框架中。*

```
*df_fake_data = pd.DataFrame(fake_data)*
```

*pandas 数据框架提供了许多分析和操作数据的功能。为了这个项目的目的，我们将操纵这个数据帧作为一个数据库条目。*

# *向数据库添加虚拟数据*

*首先，让我们建立数据库连接！我们将使用前面提到的 XAMPP 来启动我们的 SQL 实例。*

*![](img/bfc4480908561b096d6dc2646f4114c9.png)*

*我们已经成功启动了我们的数据库。现在，我们将创建一个数据库，其中包含由 python 脚本使用下面的代码生成的表。*

```
*mysql -u root
create database testdb;*
```

*![](img/4ad9aff944d27708d68c0798b5689b3d.png)*

*看啊！我们启动了我们的 MySQL 数据库(这里是 MariaDB ),并创建了我们的`testdb`数据库。*

***注意:Root 是我的用户名。没有为我的帐户配置(创建)密码。但是，如果您创建了密码，在** `**mysql -u root**` **后可能会有提示，要求您输入密码。***

**如你所见，我使用的是命令行版本的 MySQL。GUI 版本也是一个备选选项。**

*现在，我们的下一步是将我们创建的数据库连接到我们的 python 代码。为此，我们将使用来自`sqlalchemy` 库的引擎。*

```
*engine = create_engine('mysql://root:[@localhost/](http://twitter.com/localhost/foo)testdb', echo=False)*
```

*`create_engine`中的琴弦遵循`driver://username:password**@host**:port/database`的结构。如你所见，我没有密码，所以它是空白的。可以指定端口，但是如果您使用 MySQL 的默认配置，就没有必要了。*

*现在我们已经创建了我们的连接，我们现在可以将熊猫数据帧直接保存到我们的数据库`testdb`。*

```
*df_fake_data.to_sql('user', con=engine,index=False)*
```

*当执行上面的代码时，将在我们的数据库中创建一个**用户**表，并向其中添加一千条记录。*

*![](img/976223e136e71fcaaaaa83cfdac55db5.png)*

*作者截图*

*![](img/2baa1da8c0ab9a108d6853f386c73de2.png)*

*作者截图*

*通过查看数据和屏幕截图左下角的行号，您可以看到，我们刚刚生成了 1000 条记录！*

*![](img/db4de263d2f8dd8184ce6f0f07c95fc2.png)*

*按作者显示表中行数的屏幕截图。*

# *获取代码*

*该项目的完整代码位于以下要点中:*

# *奖金！*

# *让我们生成十万(100，000)条记录！*

*一个可以完成的任务是通过增加循环中的迭代次数来增加生成的假记录的数量。*

**注意。我们首先需要使用查询* `*drop table user;*`删除之前由我们的脚本创建的用户表*

```
*for _ in range(**100000**):
    fake_data["first_name"].append( fake.first_name() )
    fake_data["last_name"].append( fake.last_name() )
    fake_data["occupation"].append( fake.job() )
    fake_data["dob"].append( fake.date_of_birth() )
    fake_data["country"].append( fake.country() )*
```

*由于我的终端输出的记录太多，我将只说明行数:*

*![](img/b1431373af14993b272055a5493d1074.png)*

*下面的时间戳显示了在我的机器上生成十万条记录所用的时间:*

*![](img/39b4f6cf135e1f146e2f74727ee2ab9a.png)*

*不错的成绩对吧！*

*我们在短短 31 秒内就在数据库中创建了 100，000 条记录。31 秒！你能相信吗，疯狂的权利！*

# *摘要*

*我们能够创建一个由普通用户数据填充的数据库，如他们的名字、姓氏、出生日期、职业和国家。我们不仅创造了它，而且在很短的时间内创造了它。这个解决方案的一个好处是，它比我遇到的许多其他解决方案都要快。请随意使用 faker 库。它还可以用来生成许多其他类型的数据。*

# ***加入我们的邮件列表***

*如果您喜欢这篇文章，请注册，以便在我们发布新文章时收到电子邮件。使用此链接[输入您的电子邮件](https://tinyletter.com/Jordanjjo)，完全免费！*