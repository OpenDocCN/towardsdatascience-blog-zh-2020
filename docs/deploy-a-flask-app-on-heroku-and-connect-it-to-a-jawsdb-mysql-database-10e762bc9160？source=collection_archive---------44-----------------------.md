# 在 Heroku 上部署一个 Flask 应用程序，并将其连接到 JawsDB-MySQL 数据库

> 原文：<https://towardsdatascience.com/deploy-a-flask-app-on-heroku-and-connect-it-to-a-jawsdb-mysql-database-10e762bc9160?source=collection_archive---------44----------------------->

## 使用 Heroku 将 MySQL 数据库和 Python 应用程序部署到云的简要指南

作者: [Edward Krueger](https://www.linkedin.com/in/edkrueger/) 数据科学家兼讲师和 [Douglas Franklin](https://www.linkedin.com/in/douglas-franklin-1a3a2aa3/) 助教兼技术作家。

![](img/5d265c4a53b16c6cbfca133e4be248c8.png)

Gerald schmbs 在 Unsplash 上拍摄的照片

*在本文中，我们将介绍如何使用 Pipfile.lock 将一个应用程序部署到云，并将该应用程序连接到云数据库。有关虚拟环境的更多信息或开始使用环境和包管理器 Pipenv，请查看本文*[](https://medium.com/@edkrueger_16881/virtual-environments-for-data-science-running-python-and-jupyter-with-pipenv-c6cb6c44a405)**！**

## *部署问题*

*由于缺乏对虚拟环境的了解或经验，新开发人员通常在系统级别安装所有东西。用 pip 安装的 Python 包放在系统级。以这种方式为每个项目检索需求会在您的机器上创建一个无法管理的全局 Python 环境。虚拟环境允许你划分你的软件，同时保持一个依赖清单。*

*Pipenv 是一个用于虚拟环境和 Python 包管理的工具，它允许开发人员创建更易于部署、构建和修改的独立软件产品。*

## *Pipenv 是什么？*

*Pipenv 将软件包管理和虚拟环境控制结合到一个工具中，用于安装、删除、跟踪和记录您的依赖关系；以及创建、使用和管理您的虚拟环境。Pipenv 本质上是将 pip 和 virtualenv 包装在一个产品中。*

## *什么是 Heroku？*

*Heroku 提供许多软件产品，我们需要 Heroku 云平台服务来托管一个应用程序，并需要 JawsDB 来使用 MySQL 数据库。别担心，创建一个帐户并使用这些功能是免费的！*

*我们将使用 Heroku GUI 部署一个数据库和一个 Python 应用程序。*

## *云数据库的优势*

*在我们之前的部署中，我们使用了 SQLite 数据库。当使用 SQLite 数据库时，每次应用程序重新部署都会重置您的数据库。Heroku 的 JawsDB 允许我们的数据通过应用程序更新来保持。此外，使用 JawsDB 可以托管、配置、修补和管理数据库。*

# *准备部署*

*Heroku 允许我们从 GitHub 分支部署应用程序。一旦我们有了一个 Pipfile 被推送到 GitHub 的工作应用程序，我们就可以对存储库做一些最后的修改，为部署做准备了。一定要把你的 Pipfile 放在项目的根目录下，这样 Heroku 就可以找到它了！*

*注意:接下来的这些变化允许我们的应用程序在 Unix 系统上运行。Gunicorn 与 PC 不兼容，因此如果您使用的不是 Linux 或 Unix 机器，您将无法在本地测试这些更改。*

## *安装 gunicorn*

*Gunicorn 是一个 Python WSGI HTTP 服务器，它将为 Heroku 上的 Flask 应用程序提供服务。通过运行下面的代码行，您将 gunicorn 添加到您的 Pipfile 中。*

```
*pipenv install gunicorn*
```

## *添加 Procfile*

*在项目根文件夹中创建一个`Procfile`,并添加下面一行:*

```
*web: gunicorn app:app*
```

*第一个`app`代表运行您的应用程序的 python 文件的名称或者应用程序所在的模块的名称。第二个代表您的应用程序名称，即 app.py。此 Procfile 与 gunicorn 和 Heroku 的 Dynos 配合使用，可以远程为您的应用程序提供服务。*

# *Heroku 云数据库的建立和部署*

*建立一个 Heroku 帐户如果你还没有，不要担心，我们在这里展示的所有功能都是免费的！*

*转到您在 Heroku.com 上的应用，然后单击资源。然后在 addons 框中输入“JawsDB MySQL ”,如下所示。*

*![](img/bc7082031e0656448d980321dfac3975.png)*

*选择免费版本，然后单击 provision。太好了，我们现在已经为我们的应用程序部署了一个 MySQL 数据库。接下来，我们需要将这个新数据库集成到我们的应用程序逻辑中。*

*首先，让我们用下面的代码将 Python SQL 库 Pymysql 添加到我们的 Pipfile 中。*

```
*pipenv install pymysql*
```

*现在让我们获取连接字符串，并为 pymysql 修改它。转到设置并查看配置变量。您会发现一个类似下面的连接字符串。*

```
*mysql://ael7qci22z1qwer:nn9keetiyertrwdf@c584asdfgjnm02sk.cbetxkdfhwsb.us-east-1.rds.amazonaws.com:3306/fq14casdf1rb3y3n*
```

*我们需要对 DB 连接字符串进行更改，以便它使用 Pymysql 驱动程序。*

*在文本编辑器中，删除`mysql`并在它的位置添加`mysql+pymysql`，然后保存更新后的字符串。*

```
*mysql+pymysql://ael7qci22z1qwer:nn9keetiyertrwdf@c584asdfgjnm02sk.cbetxkdfhwsb.us-east-1.rds.amazonaws.com:3306/fq14casdf1rb3y3n*
```

*您需要将它添加到 Heroku 上的配置变量中。为此，请转到设置，然后配置变量并更新连接字符串。*

## *隐藏连接字符串。包封/包围（动词 envelop 的简写）*

*创建一个名为`.env`的新文件，并为您的云数据库添加如下所示的`DB_CONN,`连接字符串。*

```
*DB_CONN=”mysql+pymysql://root:PASSWORD@HOSTNAME:3306/records_db”*
```

*注意:运行`pipenv shell`可以让我们访问这些隐藏的环境变量。同样，我们可以用 os 访问 Python 中的隐藏变量。*

```
*SQLALCHEMY_DB_URL = os.getenv(“DB_CONN”)*
```

*确保将上面的行添加到您的 database.py 文件中，以便它准备好连接到云！*

# *应用部署*

*一旦我们有我们的应用程序测试和工作在本地，我们把所有的代码推到主分支。然后在 Heroku 上，去部署一个新的 app 看看下面的页面。*

*![](img/a4b2cb7a519797fb962f4181ae3eb106.png)*

*选择 Github 并搜索您的存储库*

*接下来在 Heroku 上，选择 GitHub，输入存储库的名称并点击 search。一旦您的用户名和存储库出现，单击连接。然后选择所需的分支，并单击部署。*

*![](img/b7e1ef6c61b226075b6c254f6c2e3ee6.png)*

*选择主服务器并部署分支*

*构建日志将开始填充页面上的控制台。注意，Heroku 首先查找 requirements.txt 文件，然后从 Pipenv 的 Pipfile.lock 安装依赖项。同样，将这些文件放在项目的根目录下。*

*![](img/36a0faca46c9b836c352a2469e826b08.png)*

*一旦从 Pipfile.lock 构建了您的环境，并且构建成功，您将会看到下面的消息。*

*![](img/0c2cd6a5c3f5a9b046d1d4b5a3a53a26.png)*

*成功的应用部署*

*该应用程序已成功部署！点击查看按钮，查看 Heroku 上部署的应用程序。*

## *最初构建表格*

*您可能会遇到这样的错误:在尝试获取或提交数据之前，您的表尚未创建。为了解决这个问题，我们在 app.py 中使用下面一行代码。*

```
*@app.before_first_request
def setup():
     db.create_all()*
```

## *自动部署*

*我们可以启用自动部署，让 Github master 的更改在推送时显示在 Heroku 上。如果您使用这种方法，您将希望确保始终有一个工作的主分支。*

*![](img/c69bc92f255aaaaa6aa76aaeeaf0ac62.png)*

*启用自动部署*

## *结论*

*编码和构建有用的软件需要管理复杂性。我们讨论了作为版本控制工具的 Github，作为环境和包管理器的 Pipenv，以及让云公司管理数据库的一些好处。这些工具有助于降低构建软件的复杂性，因此开发人员可以专注于构建而不是管理。*

*对于希望在生产中部署、构建或使用代码的数据科学家和开发人员来说，实践适当的环境和包管理至关重要。使用像 Pipenv 这样的环境和包管理器可以使包括部署在内的许多过程变得更加舒适和高效！*

*此外，拥有一个管理良好的 GitHub 主分支和一个 Pipfile 允许 Heroku 的服务器用最少的故障排除来重建我们的应用程序。这让我们可以在几分钟内将一个应用从 GitHub 上的项目目录部署到 Heroku。*

*我们希望本指南对您有所帮助，欢迎评论和提问，谢谢！*