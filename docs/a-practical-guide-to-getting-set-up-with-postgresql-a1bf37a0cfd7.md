# PostgreSQL 入门实用指南🐘

> 原文：<https://towardsdatascience.com/a-practical-guide-to-getting-set-up-with-postgresql-a1bf37a0cfd7?source=collection_archive---------27----------------------->

## Python 的 PostgreSQL、pgAdmin 和 SQLAlchemy 入门。将 SQL 融入 Python 项目的核心。

![](img/cc14bfa254930f50c320e8e5d76b6ea2.png)

[南安](https://unsplash.com/@bepnamanh?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

作为当前的或者有抱负的数据专业人士，我们经常被提醒具有扎实的 SQL 基础的重要性(如果你不相信，可以看看[这个](https://www.dataquest.io/blog/why-sql-is-the-most-important-language-to-learn/)、[这个](/the-most-in-demand-skills-for-data-scientists-4a4a8db896db)和[这个](https://learnsql.com/blog/four-reasons-aspiring-data-scientists-must-learn-sql/))。

![](img/217e1610bb243fa1ecaa2a6e6cadc346.png)

来源:“[想要一份数据方面的工作？学习 SQL”，DataQuest 博客](https://www.dataquest.io/blog/why-sql-is-the-most-important-language-to-learn/)

虽然，也许没有 Python 或机器学习那么吸引人，但你很难找到一个不引用 SQL 的数据科学招聘广告(见上图)。也就是说，有多种语言可供选择，有许多不同的工具可以用来与数据库进行交互，学习 SQL 时很难知道从哪里开始。

虽然尝试通过嵌入式 ide(如 [DataCamp](https://www.datacamp.com/) 或 [DataQuest](https://www.dataquest.io/) )的在线课程来学习 SQL 可能很诱人，但真正的学习过程是在我们能够玩和试验自己的个人项目时。要做到这一点，我们必须能够:建立我们自己的数据库，加载相关的数据集，并查询我们的数据。这篇文章的目的是展示我们如何在几分钟内设置好所有这些事情。

# 安装 PostgreSQL 和 pgAdmin

![](img/a20297aa7fb216b4123918fb54b9a8f0.png)

PostgreSQL 官方 logo，来源:[https://wiki . PostgreSQL . org/wiki/File:PostgreSQL _ logo . 3 colors . SVG](https://wiki.postgresql.org/wiki/File:PostgreSQL_logo.3colors.svg)

有一系列数据库管理系统(DBMS)可供选择，特别是:PostgreSQL、MySQL、Oracle、SQL Server、SQLite。这里我们将使用 **PostgreSQL** :一个流行的开源解决方案([*PostgreSQL 优于其他开源 SQL 数据库*](https://www.compose.com/articles/what-postgresql-has-over-other-open-source-sql-databases/) )。

我们可以在这里 安装 PostgreSQL [**。**](https://www.postgresql.org/)

![](img/314c894b934d040d92a159ab9767ad0e.png)

在安装过程中，确保勾选 **pgAdmin 4** 框；这将是我们机构的一个重要补充。如果您需要单独安装 pgAdmin，您可以在此处[安装](https://www.pgadmin.org/)。

# pgAdmin 入门

pgAdmin 是 Postgres 的管理工具；它本质上位于我们的 PostgreSQL 安装之上，为我们提供了一个优雅的 GUI 来可视化数据库对象并与之交互。一旦我们安装了 Postgres 和 pgAdmin(如上所述),我们就可以打开 pgAdmin 4 自己看了。如果这是我们第一次使用它，它应该会在浏览器窗口中弹出，并提示我们设置主密码。

![](img/18d446cbc9fbc6fa630de581f41e7435.png)

探索 pgAdmin

正如我们所看到的，有一个服务器可供我们连接，对应于我们的本地 PostgreSQL 12 安装。在这里，我们可以看到默认的“postgres”数据库和一个包含数据库对象的嵌套列表。对于我们的设置，我们不需要太担心所有这些对象是什么，但是[这篇](https://www.postgresqltutorial.com/postgresql-server-and-database-objects/)文章很好地解释了基本概念，并且 [pgAdmin 文档](https://www.pgadmin.org/docs/pgadmin4/3.x/managing_database_objects.html)更详细。

现在让我们创建一个新的数据库，并尝试导入一些数据，这样我们就可以开始查询它了。我们假设没有 SQL 命令的先验知识，所以现在我们将使用 pgAdmin 的用户界面与 PostgreSQL 进行交互。为了保持话题性，我们将使用由欧洲疾病预防和控制中心[提供的*新冠肺炎地理病例分布*数据集，可以在](https://www.ecdc.europa.eu/en/publications-data/download-todays-data-geographic-distribution-covid-19-cases-worldwide)[这里](https://opendata.ecdc.europa.eu/covid19/casedistribution)找到该数据集作为 CSV。

创建数据库非常简单，只需右键单击'*数据库*对象，选择'*创建*，然后选择'*数据库*，并选择一个合适的名称。同样，我们可以通过右键单击' *Tables* '对象来创建一个表。现在唯一的区别是我们需要指定表中每一列的名称和数据类型。将我们的列的顺序与我们想要导入的数据集中的顺序相匹配是很重要的，尽管名称不一定必须相同。

![](img/3676c8d6e7eb78cd88944966aaca3b04.png)

在 pgAdmin 中创建新的数据库和表

现在我们已经创建了数据库和一个表来存放 CSV 数据集，我们可以继续导入它了。右键单击我们刚刚创建的表对象，我们可以选择“导入/导出”对话框，指定我们要导入数据，选择我们选择的文件，并让 pgAdmin 知道数据有一个标题，我们希望在导入时跳过它。

![](img/fa26965b3f7946609cc92a76ef26b7e6.png)

使用 pgAdmin 导入 CSV 数据集

现在，我们已经具备了开始查询项目数据所需的一切。导航到窗口的最顶端，我们可以选择'*工具*'菜单，然后是'*查询工具*'选项。这将在我们的窗口中打开一个新的选项卡，我们可以在其中编写并执行 SQL 代码，以便查看数据输出。我们甚至可以选择将输出下载为 CSV 文件。

![](img/62e0839f0a134a0fa664b9cd372716fd.png)

在 pgAdmin 中查询数据

现在我们有了一个基本的设置，允许我们使用 pgAdmin GUI 与 PostgreSQL 数据库进行交互，让我们看看如何进一步扩展它。在下一节中，我们将看到如何以编程方式与 PostgreSQL 数据库进行交互:一种将 Postgres 嵌入项目核心的更实用的方法。

# SQLAlchemy 和 pandas——PostgreSQL 的 Pythonic 接口

![](img/ffe746fdc59f213f58b908f56f38eef6.png)

Artem Maltsev 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

SQLAlchemy 是一个整洁的 Python 库:自称为“Python 的数据库工具包”，它使我们能够以干净、Python 化的方式访问数据库的功能。我们可以用 SQLAlchemy 做的事情超出了这个设置的范围，但是如果你有兴趣进一步探索的话，官方的[文档](https://www.sqlalchemy.org/)和[本教程](https://www.tutorialspoint.com/sqlalchemy/index.htm)应该是足够的。在这里，我们将了解如何使用 SQLAlchemy 在 Python 脚本中向我们的数据库运行原始 SQL 命令，以及从 pandas Dataframe 加载数据。

## 创建表格

让我们从尝试复制上面创建一个表并加载到 CSV 数据集中的过程开始。创建表的 SQL 命令很简单；我们只需要提供一个表名和一个我们希望包含在表中的所有列的列表，以及它们对应的数据类型:

添加`DROP TABLE IF EXISTS`意味着如果我们的 *worldwide_cases* 表已经存在，那么我们将在运行`CREATE TABLE`命令之前删除它。

要从 Python 脚本中运行这些 SQL 命令，我们首先需要安装 SQLAlchemy，这可以通过带有`pip install SQLAlchemy`的 pip 简单地完成。关于替代安装方法，请查阅[官方安装指南](https://docs.sqlalchemy.org/en/13/intro.html#installation-guide)。

我们现在可以使用 SQLAlchemy 的`create_engine`函数连接到 Postgres 数据库:

```
*# Connecting to Postgres*from sqlalchemy import create_engineconnection_uri = 'postgres://username:password@host:port/database'
engine = create_engine(connection_uri)
```

我们传递给`create_engine`的`connection_uri`字符串让 SQLAlchemy 知道在哪里可以找到我们的数据库，它是使用上面给出的一般结构组成的。首先，我们让 SQLAlchemy 知道我们正在使用' *postgres'* ,而不是 SQL 的其他风格。然后，我们传递我们的'*用户名'*和'*密码'*(除非您专门创建了一个新用户，否则您可以使用默认用户名' postgres '和您最初设置的密码)。“*主机*和“*端口”*的值决定了数据库运行的服务器；鉴于我们已经在本地安装了 Postgres，这很可能是`localhost:5432`的默认设置。最后，我们指定我们试图连接的数据库的名称。关于构建 URI 的更多信息，你可以在这里找到文档。

一旦我们为数据库创建了一个引擎实例，我们就可以简单地使用以下命令来执行 SQL 命令:

```
# Executing SQL command
with engine.connect() as con:
    con.execute(sql)
```

将所有这些放在一起，我们可以编写一个`create_table.py`脚本，它将连接到我们的数据库，并从我们的`create_table.sql`文件中执行所需的 SQL 命令:

我们现在应该在我们的`covid-19`数据库中有一个名为`worldwide_cases`的空白表(如果您按照上一节中的步骤操作，它将会覆盖我们之前创建的表)。您可以在 pgAdmin 中通过右键单击数据库并选择“Refresh”来验证这一点，这将使我们能够看到嵌套在“Schemas/public/Tables”下的表。或者，打开查询工具并运行命令`SELECT * FROM worldwide_cases`。

现在让我们将一些数据加载到我们的表中！

## 将数据加载到表中

将 SQLAlchemy 与熟悉的 pandas 库结合使用，加载数据再简单不过了。我们可以使用 pandas DataFrame 类的`df.to_sql()`方法将数据直接加载到指定的表中:

```
*# Load DataFrame into database*df.to_sql(
    name='table_name',
    con=engine,
    if_exists='append',
    index=False)
```

我们指定希望填充的表的名称，以及将我们连接到数据库的 SQLAlchemy 引擎。我们还可以用`if_exists`参数来规定表已经存在的情况下的行为。在我们的例子中，由于我们的表已经建立，我们将'*'追加 T10'我们的数据帧到表中。然而，我们也可以将该值设置为' *fail* '，如果该表已经存在，这将在我们的代码中引发一个异常，或者设置为' *replace* '，在这种情况下，该表将被删除并从数据中重新创建。*

事实上，在前面的部分中，我们不需要麻烦地预先创建一个表。这是一个很有价值的演示，展示了我们如何使用 SQLAlchemy 运行原始 SQL 命令；然而，我们可以简单地运行我们的`df.to_sql()`命令，这个表就会神奇地为我们创建。我们可以使用`dtype`参数指定表中列的数据类型，该参数采用列名和 SQL 数据类型的字典，类似于我们在`CREATE TABLE`命令中指定它们的方式。如果我们不指定这一点，pandas 将从我们的数据帧中的数据推断出数据类型。

将数据集加载到`worldwide_cases`表中的完整脚本如下所示:

这里唯一增加的是几行代码，用于根据我们在表中选择的列标题重命名 CSV 数据集的列。我使用了一些 SQLAlchemy 技巧，很好地演示了如何用它以 Pythonic 方式访问数据库对象。导入元数据和表类，我们可以实例化一个表对象，反映我们的`worldwide_cases`表；由此，我们可以访问它的`columns`属性。这只是保存再次写出所有列名列表的快捷方式，但是我们也可以等效地通过`df.columns = list_of_column_names`，所以如果您愿意的话，可以随意这么做。

## 查询数据库

最后，我们可以重新创建我们在 pgAdmin 中演示的过程的最后一部分:查询我们的数据集。

假设我们有兴趣了解哪些国家目前的病例总数最高，我们可能会生成类似于下面的查询。

我们在上面看到，我们可以通过 SQLAlchemy 运行原始 SQL 命令来创建一个表。让我们用我们的`SELECT`查询尝试一下，看看会发生什么:

运行上面的脚本来执行我们的 SQL 查询并存储结果(可以使用`fetchall()`方法以列表的形式访问结果)，返回以下输出:

```
('United_States_of_America', 1920061, 109802)
('Brazil', 645771, 35026)
('Russia', 458689, 5725)
('United_Kingdom', 284868, 40465)
('India', 246628, 6929)
         ...
('Papua_New_Guinea', 8, 0)
('British_Virgin_Islands', 8, 1)
('Bonaire, Saint Eustatius and Saba', 7, 0)
('Lesotho', 4, 0)
('Anguilla', 3, 0)
```

现在，我们的数据集中的每一行都有一个类似元组的对象，我们可以用任何我们喜欢的方式来解析和操作它，以便将这些数据合并到我们的 Python 脚本中。这太好了；正是我们需要的。尽管如此，如果我们能把数据直接放入熊猫的数据框架，而不需要任何事先的处理，那不是很好吗？我们很幸运。与 pandas 提供的将数据加载到数据库的`df.to_sql()`方法相同，我们也可以使用它的`pd.read_sql()`通过我们的数据库连接执行查询。

`pd.read_sql()`方法实际上是两个独立方法的组合:`pd.read_sql_table()`和`pd.read_sql_query()`。前者将第一个参数作为我们希望读取的表的名称，后者是一个 SQL 查询。因此，使用`pd.read_sql()`,我们可以提供两者之一，这取决于我们是想读取单个表还是执行查询。现在，让我们开始实践，执行上面的`query.sql`查询:

现在，我们的查询结果有了一个简洁的 DataFrame 对象，使我们可以立即访问 pandas 提供的用于操作数据的资源套件:

```
 countries_and_territories   cases  deaths
0  United_States_of_America  1920061  109802
1                    Brazil   645771   35026
2                    Russia   458689    5725
3            United_Kingdom   284868   40465
4                     India   246628    6929
```

# 包扎

![](img/d48f0f58b405b7209533d7cdfbfb1f1d.png)

Wolfgang Hasselmann 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

这篇文章的目的是演示一个简单的设置，使我们能够开始学习 SQL，并给我们在自己的项目和数据探索中使用它的自由。我们现在已经在系统上安装了 PostgreSQL、pgAdmin，以便通过 GUI 与它进行交互，我们已经看到可以使用 SQLAlchemy 从 Python 脚本中加载和查询数据。

现在轮到您了:去学习更多关于 SQL 的知识和它的能力，询问您选择的数据集以找到见解，并将 PostgreSQL 融入您的 Python 项目的核心！

***感谢阅读！***

这个博客的所有源代码都可以在这里找到:

[](https://github.com/emilegill743/postgres-setup) [## Emile gill 743/postgres-设置

### Python 的 PostgreSQL、pgAdmin 和 SQLAlchemy 入门。-Emile gill 743/postgres-设置

github.com](https://github.com/emilegill743/postgres-setup) 

如果你喜欢这篇文章，可以看看我的其他文章:

[](/finding-a-grocery-delivery-slot-the-smart-way-f4f0800c4afe) [## 聪明的方法是找到一个杂货递送点

### 使用 Python、Heroku 和 Twilio 从一个杂货网站的 API 中抓取数据，并在出现插槽时获得文本通知…

towardsdatascience.com](/finding-a-grocery-delivery-slot-the-smart-way-f4f0800c4afe)