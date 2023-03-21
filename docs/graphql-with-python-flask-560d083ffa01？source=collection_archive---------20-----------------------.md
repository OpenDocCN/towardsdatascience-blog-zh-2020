# 带 Python Flask 的 GraphQL

> 原文：<https://towardsdatascience.com/graphql-with-python-flask-560d083ffa01?source=collection_archive---------20----------------------->

## GraphQL、石墨烯、Flask 和 Mysql

![](img/459488c324f88053df18a24842a6d90b.png)

照片由 [Michael Dziedzic](https://unsplash.com/@lazycreekimages?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

在我们开始之前，让我们先谈谈我们的目标。

我们正在尝试开发一个在 Flask 上运行的初学者工具包项目，它具有以下功能:

*   GraphQL 支持
*   数据库集成

# 先决条件

对于本教程，我们需要:

```
- Python
- Pip
- Virtualenv
- Docker
```

# 安装烧瓶

> *Flask 是一个基于 Werkzeug、Jinja 2 和 good intentions 的 Python 微框架。—烧瓶文件*

建立一个准系统 Flask 项目非常简单。然而，由于我们希望内置相当多的功能，下面的步骤将比通常花费稍长的时间。

首先创建一个虚拟环境。

```
$ virtualenv venv
$ source venv/bin/activate
```

接下来，我们可以安装所需的软件包。为了不使每个人感到困惑，我将只分别包括每个步骤所需的包。

```
$ pip install flask flask_script
$ mkdir -p app/__init__.py
$ touch manage.py
```

# MySQL 与 Flask 的集成

我们首先必须安装所需的依赖项

```
$ pip install flask-sqlalchemy pymysql
```

我们现在可以开始设置我们的 Flask 应用程序来连接我们的 MySQL 服务。为此，我们必须添加一个`config.py`文件，并更新我们的`app/__init__.py`文件。

使用一个配置文件，我们能够分离我们的环境变量，以及根据需要存储我们的常量变量。

这样，当我们运行`docker-compose up`时，应用程序将开始运行，并与 MySQL 服务连接。

# 设置 GraphQL

GraphQL 是一种针对 API 和运行时的查询语言，用于使用现有数据完成这些查询。GraphQL 为 API 中的数据提供了一个完整且易于理解的描述，让客户能够准确地了解他们的需求，使 API 更容易随时间发展，并支持强大的开发工具。

```
$ pip install graphene Flask-GraphQL
```

让我们分析一下我们刚刚安装的软件包。

# 烧瓶图

[Flask-GraphQL](https://github.com/graphql-python/flask-graphql) 是一个包装层，为 Flask 应用程序增加了 GraphQL 支持。

我们必须为客户端添加一个`/graphql`路由来访问 API。为此，我们可以简单地更新我们的`app/__init__.py`文件。

在这种情况下，我们从`Flask-GraphQL`模块导入了`GraphQLView`，并使用它来实现我们的`/graphql`路线。

# 石墨烯

接下来，我们将使用[石墨烯-Python](https://graphene-python.org/) 包。Graphene 是一个开源库，允许开发人员使用 Python 中的 GraphQL 构建简单但可扩展的 API。

如果你已经从我们最新的`app/__init__.py`文件中注意到，我们实际上缺少一个`app/schema.py`文件来让我们的 Flask 应用程序工作。GraphQL 模式是任何 GraphQL 服务器实现的核心，它描述了连接到它的客户机可用的功能。

在这种情况下，我们将创建一个简单的模式，这样我们的应用程序将能够运行，没有任何错误。

这样，尝试运行您的应用程序。当你访问[http://localhost:5000/graph QL](http://localhost:5000/graphql%60)时，你应该能看到这个页面。如果你熟悉 GraphQL，这个页面你应该很熟悉。这是一个允许你输入 GraphQL 查询的界面。

# 结论

我希望本文能让您更好地理解和概述如何使用 GraphQL 构建 Python 应用程序。这也可以通过 Django 框架来实现。

我还在这里添加了一个 Github 知识库供参考。这是 GraphQL 最佳实践的另一个故事，您可能会感兴趣。

[](/graphql-best-practices-3fda586538c4) [## GraphQL 最佳实践

### 在使用 GraphQL 6 个月之后，我分享了我对创建 graph QL 服务器的良好实践的想法

towardsdatascience.com](/graphql-best-practices-3fda586538c4)