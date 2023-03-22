# 在 Heroku 上部署 Flask 应用程序，并将其连接到 MongoDB 的 mLab

> 原文：<https://towardsdatascience.com/deploying-a-flask-app-on-heroku-and-connecting-it-to-mongodbs-mlab-e0022c4e6d1e?source=collection_archive---------32----------------------->

## 将带有 MongoDB mLab 数据库的 Python 应用程序部署到 Heroku 的简短指南。

*By:* [*爱德华克鲁格*](https://www.linkedin.com/in/edkrueger/)*[*爱德华* *哈尔曼四世*](http://linkedin.com/in/edward-haarmann-iv/)[*道格拉斯富兰克林*](https://www.linkedin.com/in/douglas-franklin-1a3a2aa3/) *。**

*![](img/d208b33f5ae0b910747d628a7fe1d98e.png)*

*美国宇航局在 Unsplash 上拍摄的照片*

**像我们以前的云部署文章一样，我们将介绍如何使用 Pipfile.lock 将应用程序部署到云，并将应用程序连接到云数据库。有关虚拟环境的更多信息或开始使用环境和包管理器 Pipenv，请查看* [*这篇文章*](https://medium.com/@edkrueger_16881/virtual-environments-for-data-science-running-python-and-jupyter-with-pipenv-c6cb6c44a405) *！**

## *部署问题*

*由于缺乏对虚拟环境的了解或经验，新开发人员通常在系统级别安装所有东西。用 pip 安装的 Python 包放在系统级。以这种方式为每个项目检索需求会在您的机器上创建一个无法管理的全局 Python 环境。虚拟环境允许你划分你的软件，同时保持一个依赖清单。*

*Pipenv 是一个用于环境和包管理的工具，它允许开发人员创建更容易部署、构建和修改的独立软件产品。*

## *Pipenv 是什么？*

*Pipenv 将包管理和虚拟环境控制结合到一个工具中，用于安装、删除、跟踪和记录您的依赖关系:以及创建、使用和管理您的虚拟环境。Pipenv 本质上是将 pip 和 virtualenv 包装在一个产品中。*

## *什么是 Heroku？*

*Heroku 提供许多软件服务产品。我们将需要 Heroku 云平台服务来托管一个应用程序和 mLab 来使用 MongoDB 数据库。别担心，创建一个帐户并使用这些功能是免费的！*

*云平台支持多种编程语言的应用，包括 Python、Node.js、Scala、Ruby、Clojure、Java、PHP 和 Go。我们将使用 Heroku GUI 部署一个数据库和一个 Python 应用程序。*

## *云数据库的优势*

*在我们以前的部署中，我们使用 SQLite 和 JawsDB 数据库。当使用 SQLite 数据库时，每次应用程序重新部署都会重置您的数据库。JawsDB 非常适合部署 MySQL 关系数据库，该数据库将通过应用程序更新来保持。这一次，我们想要一个基于文档的数据库系统，以 MongoDB 在 Heroku 的 mLab 插件的形式。*

## *准备部署*

*Heroku 允许我们从 GitHub 分支部署应用程序。一旦我们有了一个 Pipfile 被推送到 GitHub 的工作应用程序，请确保您的 Pipfile 在项目的根目录下，我们准备对存储库进行一些最终的更改，为部署做准备。*

***注意**:这些变化允许我们的应用程序在 Unix 系统上运行。Gunicorn 与 PC 不兼容，因此您将无法在本地测试这些更改。*

## *安装 gunicorn*

*Gunicorn 是一个 Python WSGI HTTP 服务器，它将为 Heroku 上的 Flask 应用程序提供服务。通过运行下面的代码行，您将 gunicorn 添加到您的 Pipfile 中。*

```
*pipenv install gunicorn*
```

## *添加 Procfile*

*在项目根文件夹中创建一个 Procfile，并添加以下行:*

```
*web: gunicorn app.app:app*
```

*第一个应用程序(在。)表示 repo 中包含运行应用程序所需的所有文件的目录。第二个应用程序(在。)是运行您的应用程序的 python 文件的名称或应用程序所在的模块的名称，即 app.py。最后，第三个应用程序(在:)之后)表示您在 app.py 中的 flask 应用程序。此 Procfile 与 gunicorn 和 Heroku 的 Dynos 一起工作，以远程服务您的应用程序。*

## *Heroku 云数据库的建立和部署*

*建立一个 Heroku 帐户如果你还没有，不要担心，我们在这里展示的所有功能都是免费的！*

*转到您在 Heroku.com 上的应用，然后单击资源。然后在 addons 框中输入“mLab MongoDB ”,如下所示。*

*![](img/0b055d013c24897600f3e4bb246d6c8e.png)*

*从下拉列表中选择“Sandbox-free”并单击“Provision”如果您还没有 mLab 的帐户，Heroku 将引导您创建一个帐户。我们现在已经为我们的应用程序部署了一个 MongoDB 数据库！现在我们需要将这个新数据库集成到我们的应用程序逻辑中。*

*![](img/6da60176e6921b013419565ddb2e8039.png)*

*我们需要将 py mongo(Python MongoDB 库)添加到我们的 Pipfile 中，如下所示:*

```
*pipenv install pymongo*
```

*现在让我们得到我们的连接字符串，并为 pymongo 修改它。转到设置，用“显示配置变量”按钮查看配置变量。您会在“MONGODB_URI”旁边找到一个类似于下面的连接字符串:*

```
*mongodb://heroku_l148hmql:tpna8npo399tsuuy82s7fgma2c@ds175222.mlab.com:51222/heroku_l148hmql*
```

*![](img/3921f6955a6b9bcc7c1642a08ee0dc2e.png)*

*与我们到 JawsDB 的连接不同，我们不需要通过在前面添加 pymongo 驱动程序来调整这个连接字符串。然而，我们仍然必须改变连接字符串。在文本编辑器中，将`?retryWrites=false`添加到字符串的末尾，使其类似于下面的字符串。*

```
*mongodb://heroku_0hk44bz1:vrha0rbf397na30dgigjdtkgnc@ds151993.mlab.com:51993/heroku_0hk44bz1?retryWrites=false*
```

*单击 Heroku 中配置变量旁边的 edit 按钮，然后复制并粘贴这个更新后的字符串，并单击“Save Changes”*

*![](img/48fc9e9acdafebc4f2b95c001d121b36.png)*

## *隐藏连接字符串。包封/包围（动词 envelop 的简写）*

*创建一个名为`.env`的新文件，并将云数据库的连接字符串添加为 MONGODB_URI，如下所示。*

```
*MONGODB_URI=”mongodb://root:PASSWORD@HOSTNAME:3306/db_name” [your connection string]*
```

***注意**:运行 pipenv shell 可以让我们访问这些环境变量。这种访问在下面一行中实现:*

```
*client = pymongo.MongoClient(os.getenv(“MONGODB_URI”, “mongodb://127.0.0.1:27017/database”))*
```

*当我们在 shell 中时，客户端会寻找 MONGODB_URI 环境变量。如果找不到，默认情况下，客户端连接到我们的本地数据库。*

## *应用部署*

*一旦我们有我们的应用程序测试和工作在本地，我们把所有的代码推到主分支。然后在 Heroku 上，转到部署选项卡。选择包含应用程序的存储库，然后点击连接。*

*![](img/2682d37474c8c589aba5160e58cc64bf.png)*

*最后一步是点击“部署分支”*

*![](img/cba7cb61ac68f8e197ad152969dc9aa1.png)*

*构建日志将开始填充页面上的控制台。注意，Heroku 首先查找 requirements.txt 文件，然后从 Pipenv 的 Pipfile.lock 安装依赖项。*

*一旦从 Pipfile.lock 构建了您的环境，并且构建成功，您将会看到下面的消息。*

*![](img/a6f0236fb455ad4537ae5b2656b7469c.png)*

*呜哇！该应用程序已成功部署！点击查看按钮，查看 Heroku 上部署的应用程序。*

*![](img/1e026a8c751069e27d7b65d7adf65e77.png)*

*成功发射！照片由 Tim Mossholder 在 Unsplash 上拍摄*

## *最初构建集合*

*最初，您需要在 mLab 中手动创建集合(MongoDB 的基于文档的模拟表)。如果第一个集合不存在，客户端将创建它。*

*但是，您可以选择在 mLab 中创建集合。如果你想这样做，请从 Heroku 的资源标签进入 mLab 网站。*

*![](img/4438db7f312a8657e2e36c2666d7b922.png)*

*在“收藏”下面，应该写着[目前没有]。在右边，点击“添加收藏”*

*![](img/b50eecae0ba8073ec510a08abcf35ef8.png)*

***重要提示**:确保你给这个收藏起的名字和你在应用程序中的名字一样，否则将无法连接。在这个应用程序中，我们将集合称为“帖子”*

*![](img/e22628517a498dfc8c457f0da9df94c0.png)*

## *自动部署*

*我们可以启用自动部署，让 Github master 的更改在推送时显示在 Heroku 上。如果您使用这种方法，您将希望确保始终有一个工作的主分支。*

## *结论*

*对于希望在生产中部署、构建或使用代码的数据科学家和开发人员来说，实践适当的环境和包管理至关重要。*

*拥有一个管理良好的 GitHub 主分支允许 Heroku 的服务器用最少的故障排除来重建我们的应用程序。这使得我们可以在几分钟内将一个项目目录从 GitHub 转移到一个可用的、已发布的应用程序中。*

*使用云服务资源很重要，这样你的应用就可以在你自己的机器之外的机器上运行。如果您正在构建一个应用程序，您必须能够使本地构建环境适应其他人将要与之交互的环境。Heroku 的 MongoDB 插件 mLab 允许我们这样做，除了一个连接字符串之外，不需要改变太多！*

*我们希望本指南对您有所帮助！*