# 将 Dockerized Flask 应用程序部署到 Google 云平台

> 原文：<https://towardsdatascience.com/deploy-a-dockerized-flask-app-to-google-cloud-platform-71d91b39b25e?source=collection_archive---------23----------------------->

## 云部署

## 使用 Cloud Run 和 SQL 实例将 Dockerized Python 应用程序部署到 Google 云平台的简短指南。

作者:[爱德华·克鲁格](https://www.linkedin.com/in/edkrueger/)和[道格拉斯·富兰克林](https://www.linkedin.com/in/douglas-franklin-1a3a2aa3/)。

![](img/e58d9a55f26f9efc6d167027d3827f06.png)

照片由安德里亚斯·韦兰在 Unsplash 上拍摄

*在本文中，我们将介绍如何使用 Pipfile.lock 将应用程序部署到云，并将应用程序连接到云数据库。有关虚拟环境的更多信息或开始使用环境和包管理器 Pipenv，请查看本文* *！*

## 部署问题

由于缺乏对虚拟环境的了解或经验，新开发人员通常在系统级别安装所有东西。用 pip 安装的 Python 包放在系统级。以这种方式为每个项目检索需求会在您的机器上创建一个无法管理的全局 Python 环境。虚拟环境允许你划分你的软件，同时保持一个依赖清单。

Pipenv 是一个用于虚拟环境和包管理的工具，它允许开发人员创建更容易部署、构建和修改的独立软件产品。

## Pipenv 是什么？

Pipenv 将软件包管理和虚拟环境控制结合到一个工具中，用于安装、删除、跟踪和记录您的依赖关系；以及创建、使用和管理您的虚拟环境。Pipenv 本质上是将 pip 和 virtualenv 包装在一个产品中。

对于应用程序部署，GCP 可以从 Pipfile 构建环境。当我们添加和删除依赖项时，Pipenv 将自动更新我们的 Pipfile。

# Docker 和 docker 文件

![](img/cc4d3f1c9b734fae82eb7bf8ecd25c10.png)

卡梅伦·文蒂在 Unsplash 上拍摄的照片

Docker 是将应用程序投入生产的最佳方式。Docker 使用 docker 文件来构建容器。构建的容器存储在 Google Container Registry 中，在那里可以部署它。Docker 容器可以在本地构建，并将在任何运行 Docker **的系统上运行。**

GCP 云构建允许您使用 docker 文件中包含的指令远程构建容器。远程构建很容易集成到 CI/CD 管道中。由于 Docker 使用大量 RAM，它们还节省了本地计算时间和能量。

这是我们在这个项目中使用的 docker 文件:

示例 Dockerfile 文件

每个 docker 文件的第一行都以`FROM.`开始，这是我们导入操作系统或编程语言的地方。下一行以 ENV 开始，将我们的环境变量 ENV 设置为`APP_HOME / app.`

这些行是 Python 云平台结构的一部分，你可以在 [*文档*](https://cloud.google.com/run/docs/quickstarts/build-and-deploy#python) 中读到更多关于它们的内容。

`WORKDIR`行将我们的工作目录设置为`/app`。然后，复制行使本地文件在 docker 容器中可用。

接下来的三行涉及设置环境并在服务器上执行它。`RUN`命令可以跟随着您想要执行的任何 bash 代码。我们使用`RUN`来安装 pipenv。然后使用 pipenv 安装我们的依赖项。最后，`CMD`行执行我们的 HTTP 服务器 gunicorn，将我们的容器绑定到 `$PORT`，为端口分配一个 worker，指定该端口使用的线程数量，最后将应用程序的路径声明为`app.main:app`。

您可以添加一个`.dockerignore`文件来从您的容器映像中排除文件。`.dockerignore`用于将文件保存在容器之外。例如，您可能不希望在您的容器中包含您的测试套件。

要排除上传到云构建的文件，添加一个`.gcloudignore` 文件。由于 Cloud Build 将您的文件复制到云中，您可能希望忽略图像或数据以降低存储成本。

如果你想使用这些，一定要查看`[.dockerignore](https://docs.docker.com/engine/reference/builder/#dockerignore-file)`和`[**.gcloudignore**](https://cloud.google.com/sdk/gcloud/reference/topic/gcloudignore)`文件的文档，但是，要知道模式和`.gitignore`是一样的！

# 应用部署

我们需要对我们的项目文件做一些最后的修改，为部署做准备。

我们需要将 gunicorn 和 Pymysql 添加到 Pipfile 中，如下所示。

```
pipenv install gunicorn pymysql
```

Git 将您之前创建的 Pipfile、Pipfile.lock 和 Dockerfile 添加到您的存储库中。

## Docker 图像和 Google 容器注册表

现在，一旦我们准备好 docker 文件，通过从包含 docker 文件的目录运行以下命令，使用云构建来构建您的容器映像:

```
gcloud builds submit --tag gcr.io/**PROJECT-ID**/**container-name**
```

注意:用您的 GCP 项目 ID 替换项目 ID，用您的容器名称替换容器名称。您可以通过运行命令`gcloud config get-value project`查看您的项目 ID。

该 Docker 图像现在可在 GCP 集装箱注册处或 GCR 访问，并可通过云运行的 URL 访问。

# 使用 CLI 部署容器映像

1.  使用以下命令进行部署:

```
gcloud run deploy --image gcr.io/**PROJECT-ID**/**container-name** --platform managed
```

注意:用您的 GCP 项目 ID 替换项目 ID，用您的容器名称替换容器名称。您可以通过运行命令`gcloud config get-value project`来查看您的项目 ID。

2.将提示您输入服务名称和区域:选择您所选择的服务名称和区域。

3.您将被提示**允许未认证的调用**:如果您想要公共访问，响应`y`，并且`n` 限制对同一 google 项目中的资源的 IP 访问。

4.稍等片刻，直到部署完成。如果成功，命令行会显示服务 URL。

5.通过在 web 浏览器中打开服务 URL 来访问您部署的容器。

# 使用 GUI 部署容器映像

现在我们已经在 GCR 存储了一个容器映像，我们已经准备好部署我们的应用程序了。访问 [GCP 云运行](https://console.cloud.google.com/run?_ga=2.112811590.313737761.1591368161-572251819.1590763098&amp;_gac=1.61558494.1591368161.CjwKCAjw2uf2BRBpEiwA31VZj5hm5tgEHH-Ldim6HaH954LjVPoeEdbL9XkMUnSw3yKCOv1UYdvGdRoCzasQAvD_BwE)并点击创建服务，确保按要求设置计费。

![](img/f8492c02ad7b5029ed1efa3b8741d2f0.png)

选择您想服务的地区，并指定一个唯一的服务名称。然后通过分别选择未经身份验证或经过身份验证，在对应用程序的公共访问和私有访问之间进行选择。

现在我们使用上面的 GCR 容器图像 URL。将 URL 粘贴到空白处，或单击“选择”并使用下拉列表查找。检查高级设置以指定服务器硬件、容器端口和附加命令、最大请求数和扩展行为。

当您准备好构建和部署时，请单击创建！

![](img/312ea5a121f8d3576d3f1ae47a7bfe52.png)

从 GCR 选择一个容器图像

您将进入 GCP 云运行服务详细信息页面，在此您可以管理服务、查看指标和构建日志。

![](img/14000eeedc6498283335dc216927f5b0.png)

服务详情

单击 URL 查看您部署的应用程序！

![](img/ca25c22fded8781105a20d3deba15d87.png)

呜哇！

恭喜你！您刚刚将打包在容器映像中的应用程序部署到云运行。Cloud Run 自动并水平地扩展您的容器映像以处理收到的请求，然后在需求减少时缩小规模。您只需为请求处理过程中消耗的 CPU、内存和网络资源付费。

也就是说，当你不想付费时，一定要关闭你的服务！

# GCP 数据库的建立和部署

转到[云控制台](https://console.cloud.google.com/storage/)并设置计费(如果您还没有设置的话)。现在您可以创建一个 SQL 实例。

选择您想要使用的 SQL 方言，我们正在使用 MySQL。

![](img/d3b2d52873a43aa2c76689731a7a289b.png)

设置实例 ID、密码和位置。

## 将 MySQL 实例连接到您的云运行服务

与任何配置更改一样，设置新的云 SQL 连接会导致创建新的云运行版本。要将云服务连接到云数据库实例:

1.  [去云茹](https://console.cloud.google.com/run) n
2.  配置服务:

如果您正在**向新服务**添加云 SQL 连接:

*   您需要将您的服务容器化并上传到容器注册中心。
*   点击**创建服务**。

如果您正在**向现有服务添加云 SQL 连接**:

*   单击服务名称。
*   点击**部署新版本**。

3.启用连接到云 SQL:

*   点击**显示可选设置**:

![](img/731320672b3928c5d6b2ee642d3b705e.png)

*   如果要在项目中添加云 SQL 实例的连接，单击**添加连接**后，从下拉菜单中选择所需的云 SQL 实例。
*   如果您正在使用来自另一个项目的云 SQL 实例，请在下拉列表中选择**连接字符串**，然后以 PROJECT-ID:REGION:INSTANCE-ID 格式输入完整的实例连接名称。

4.点击**创建**或**部署**。

无论哪种情况，我们都希望我们的连接字符串看起来像下面这样。

```
mysql://ael7qci22z1qwer:nn9keetiyertrwdf@c584asdfgjnm02sk.cbetxkdfhwsb.us-east-1.rds.gcp.com:3306/fq14casdf1rb3y3n
```

我们需要对 DB 连接字符串进行更改，以便它使用 Pymysql 驱动程序。

在文本编辑器中，删除`mysql`并在它的位置添加`mysql+pymysql`，然后将更新后的字符串保存为您的 SQL 连接。

```
mysql+pymysql://ael7qci22z1qwer:nn9keetiyertrwdf@c584asdfgjnm02sk.cbetxkdfhwsb.us-east-1.rds.gcp.com:3306/fq14casdf1rb3y3n
```

请注意，您不必使用 GCP 的 SQL。如果使用的是第三方数据库，可以将连接字符串作为 VAR 而不是 Cloud SQL 添加，并输入您的连接字符串。

## 隐藏连接字符串。包封/包围（动词 envelop 的简写）

在本地创建一个名为`.env`的新文件，并为您的云数据库添加如下所示的`DB_CONN,`连接字符串。

```
DB_CONN=”mysql+pymysql://root:PASSWORD@HOSTNAME:3306/records_db”
```

注意:运行`pipenv shell`可以让我们接触到这些隐藏的环境变量。同样，我们可以用 os 访问 Python 中的隐藏变量。

```
MySQL_DB_CONN = os.getenv(“DB_CONN”)
```

确保将上面的行添加到您的 database.py 文件中，以便它准备好连接到云！

这个。env 文件现在包含敏感信息，应该被添加到您的`.gitignore`中，这样它就不会出现在公开可见的地方。

既然我们的应用程序和数据库都在云中，让我们确保我们的系统正常工作。

## 正在加载数据库

一旦可以看到 GCP 上列出的数据库，就可以用加载脚本加载数据库了。以下要点包括我们的 load.py 脚本。

load.py

让我们运行这个加载脚本，看看我们是否可以发送到我们的数据库。

首先，运行以下代码进入您的虚拟环境。

```
pipenv shell
```

然后运行 load.py 脚本。

```
python load.py
```

访问远程应用程序地址，查看您的数据是否已添加到云数据库。如果您遇到任何问题，请务必检查您的构建日志以查找回溯！

关于这个加载过程或以模块化方式设置你的应用程序的更多说明，请访问我们的[Medium guide to building a data API](/fastapi-cloud-database-loading-with-python-1f531f1d438a)！那篇文章详细解释了上面的代码。

# 结论

在本文中，我们学习了一些关于使用 pipenv 进行环境管理以及如何对应用程序进行 Dockerize 的知识。然后，我们介绍了如何在 Google 容器注册表中存储 Docker 容器，并使用云构建 CLI 和 GUI 部署容器。接下来，我们建立了一个云 SQL 数据库，并将其连接到我们的应用程序。最后，我们讨论了一种装载数据库的方法，在本地运行 load.py。请注意，如果您的应用程序本身收集数据，您只需要部署应用程序和数据库，然后部署的应用程序将在收集数据时填充数据库。

这里有一个到 [GitHub 库](https://github.com/edkrueger/sars-fastapi)的链接，里面有我们这个项目的代码。请务必查看这段代码，看看我们是如何设置整个代码库的！