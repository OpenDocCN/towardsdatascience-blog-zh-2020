# 用 FastAPI 和 SQLAlchemy 构建数据 API

> 原文：<https://towardsdatascience.com/fastapi-cloud-database-loading-with-python-1f531f1d438a?source=collection_archive---------1----------------------->

## 如何使用模块化数据库逻辑将数据加载到数据库中，并在应用程序中执行 CRUD 操作

*由:* [*爱德华·克鲁格*](https://www.linkedin.com/in/edkrueger/) *数据科学家兼讲师和* [*道格拉斯·富兰克林*](https://www.linkedin.com/in/douglas-franklin-1a3a2aa3/) *助教兼技术作家组成。*

链接到 Github repo，app 代码:[https://github.com/edkrueger/sars-fastapi](https://github.com/edkrueger/sars-fastapi)

![](img/57b0d72aa3ba5adef35ec46d7f460433.png)

克里斯·利维拉尼在 [Unsplash](https://unsplash.com/s/photos/fast?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

## 什么是 FastAPI？

FastAPI 是基于 Pydantic 和 Starlette 的高性能 API。FastAPI 与许多包集成得很好，包括许多 ORM。使用 FastAPI，您可以使用大多数关系数据库。FastAPI 很容易与 SQLAlchemy 集成，SQLAlchemy 支持 PostgreSQL、MySQL、SQLite、Oracle、Microsoft SQL Server 等。

其他 python 微服务框架如 Flask 不容易与 SQLAlchemy 集成。通常使用 Flask 和一个名为 Flask-SQLAlchemy 的包。Flask-SQLAlchemy 是不必要的，它有自己的问题。关于这方面的更多信息，请阅读这篇文章。

没有 FastAPI-SQLAlchemy，因为 FastAPI 与 vanilla SQLAlchemy 集成得很好！

## 模块化应用程序结构

写 app 的时候，最好创建独立的、模块化的 python 代码。这里我们将讨论我们的应用程序的以下组成文件，database.py、models.py、schemas.py、main.py 和 load.py。

理想情况下，您应该只需要定义一次数据库模型！使用 SQLAlchemy 的`**declarative_base()**`和`**Base.metadata.create_all()**`允许您为每个表编写一个类，以便在应用程序中使用，在应用程序外的 Python 中使用，以及在数据库中使用。使用单独的 **database.py** 和 **models.py** 文件，我们一次性建立数据库表类和连接，然后在需要时调用它们。

为了避免 SQLAlchemy *模型*和 Pydantic *模型*之间的混淆，我们将拥有包含 SQLAlchemy 模型的文件`**models.py**`，以及包含 Pydantic 模型的文件`**schemas.py**`。同样值得注意的是，SQLAlchemy 和 Pydantic 使用稍微不同的语法来定义模型，如下面的文件所示。

## Database.py

下面是使用 SQLAlchemy 定义数据库连接的文件。

**database.py**

## 声明性库和元数据

`**declarative_base()**`基类包含一个`**MetaData**`对象，其中收集了新定义的`**Table**`对象。当我们调用行`**models.Base.metadata.create_all()**`来创建我们所有的表时，这个元数据对象被访问。

## 会话本地:处理线程问题

SQLAlchemy 包含一个帮助对象，帮助建立用户定义的`**Session**`范围。这有助于消除应用程序中的线程问题。

为了创建一个会话，下面我们使用`**sessionmaker**` 函数，并给它传递几个参数。Sessionmaker 是一个初始化新会话对象的工厂。Sessionmaker 通过从引擎的连接池中请求连接并将连接附加到新的会话对象来初始化这些会话。

![](img/cc747c7f76e35f7d23987a3f97dbefd2.png)

来自 database.py 的会话本地

初始化新的会话对象也称为“检出”连接。数据库存储这些连接/过程的列表。因此，当您开始一个新的会话时，请注意您也在数据库中启动了一个新的进程。如果数据库没有关闭这些连接，则存在可以达到的最大连接数。数据库最终会终止像过时连接这样的空闲进程；然而，这可能需要几个小时才会发生。

SQLAlchemy 有一些池选项来防止这种情况，但是最好在不再需要连接时删除它们！FastAPI 文档包含一个`**get_db()**`函数，该函数允许路由通过一个请求使用同一个会话，然后在请求完成时关闭它。然后`**get_db()**` 为下一个请求创建一个新的会话。

一旦我们建立了数据库连接和会话，我们就可以开始构建其他应用程序组件了。

## Models.py

注意，我们将上面 database.py 文件中定义的`**Base**`类导入到下面的 models.py 文件中，以使用`**declarative_base()**`。

**models.py**

该文件为数据库中的表`**Records**`创建模型或模式。

使用 SQLAlcehmy 的`**declarative_base()**`允许您为应用程序使用的每个表只编写一个模型。然后，在 Python 中，在应用程序之外和数据库中使用该模型。

拥有这些独立的 Python 文件很好，因为您可以使用相同的模型在应用程序之外查询或加载数据。此外，每个模型都有一个版本，这简化了开发。

这些模块化 Python 文件可用于在数据管道、报告生成和任何其他需要的地方引用相同的模型或数据库。

## Schemas.py

这里我们为 Pydantic 编写模式。记住 FastAPI 是建立在 Pydantic 之上的。在 Pydantic 中定义对象的主要方法是通过从`**BaseModel.**`继承的模型

Pydantic 保证结果模型的数据字段符合我们使用标准的现代 Python 类型为模型定义的字段类型。

**schemas.py**

第`**orm_mode = True**` 行允许应用程序获取 ORM 对象并自动将它们翻译成响应。这种自动化使我们不必手动从 ORM 中取出数据，将它放入字典，然后用 Pydantic 加载它。

# Main.py

这里是我们将所有模块化组件集合在一起的地方。

在导入我们所有的依赖项和模块化应用程序组件后，我们调用`**models.Base.metadata.create_all(bind=engine)**` 在数据库中创建我们的模型。

接下来，我们定义我们的应用程序并设置 CORS 中间件。

**main.py**

## CORS 中间件

CORS 或“跨来源资源共享”指的是在浏览器中运行的前端具有与后端通信的 JavaScript 代码，并且后端与前端处于不同的“来源”的情况。

在您的 **FastAPI** 应用程序中配置 CORS 中间件

*   导入`**CORSMiddleware**`。
*   创建一个字符串形式的允许源列表，例如“http://localhost”、“http://localhost:8080”
*   将它作为“中间件”添加到您的 **FastAPI** 应用程序中。

您还可以指定您的后端是否允许:

*   凭证(授权头、Cookies 等。).
*   特定的 HTTP 方法(`**POST**`、`**PUT**`)或所有带有通配符`"*****"`的方法。
*   特定的 HTTP 头或所有带有通配符`"*****"`的 HTTP 头。

为了一切正常工作，最好明确指定允许的来源。此外，出于安全原因，明确哪些来源可以访问您的应用程序是一种很好的做法。

正确设置 CORS 中间件将消除应用程序中的任何 CORS 问题。

`**get_db()**`函数确保任何通过该函数的路由在需要时都应该有我们的 SessionLocal 数据库连接，并且会话在使用后关闭。

`**/records**` 路线用于查看我们 app 的数据。注意，在这条路由中，我们使用 schemas.py 作为前端，models.py 查询后端。

fast API/记录路线

## 外部数据加载

我们没有使用应用程序加载数据，而是使用单独的 Python 文件加载数据库。

下面是一个示例 Python 文件，它从 CSV 读取数据，并将数据插入到数据库中。

**load.py**

请注意，我们导入了模型、我们的自定义会话 SessionLocal 和我们在其他 Python 文件中定义的引擎。然后我们读取 CSV 并使用`**models.Record**` 模式，通过`**SessionLocal()**` 连接将`**db_record**` 添加到数据库中。

## 数据库加载

如果您的应用程序设置正确，包括您的数据库连接字符串，您可以调用:

```
python load.py
```

这将加载您的本地或远程数据库，无需运行应用程序！

## 测试应用程序

要使用远程数据库在本地运行应用程序，请在终端中运行:

```
uvicorn app.main:app
```

这会在本地运行您的应用程序。这个本地实例连接到我们刚刚加载的云数据库，所以检查/records 路径来查看数据！

## 结论

所使用的模块化应用程序结构允许我们定义一个模型，然后根据需要使用它。这种做法使开发变得更加容易，因为如果出现问题，您只需要调试一个文件。此外，您的代码将更加可重用，并为另一个项目做好准备！

对于您的下一个数据项目，无论是仪表板、数据日志还是 API，一定要尝试一下 FastAPI！它很快也很容易上手，而且他们有很好的文档来指导你。

我们希望这对您有所帮助，感谢您的阅读，祝您好运！