# 使用 Docker、GCP 云运行和 Flask 部署 Scikit-Learn NLP 模型

> 原文：<https://towardsdatascience.com/deploy-a-scikit-learn-nlp-model-with-docker-gcp-cloud-run-and-flask-ba958733997a?source=collection_archive---------25----------------------->

构建一个服务于自然语言处理模型的应用程序，将其容器化并部署的简要指南。

作者:[爱德华·克鲁格](https://www.linkedin.com/in/edkrueger/)和[道格拉斯·富兰克林](https://www.linkedin.com/in/douglas-franklin-1a3a2aa3/)。

![](img/a9c593fa4b8f574f1f5cd7b3c4cc8a3c.png)

照片由 Adi Goldstein 在 Unsplash 上拍摄

如果您需要帮助构建 NLP 管道或评估模型，请查看我们上一篇[文章](/build-a-nlp-pipeline-with-scikit-learn-ham-or-spam-b2cd0b3bc0c1)。我们介绍了一些 NLP 基础知识以及如何用 Scikit-Learn 构建 NLP 管道。然后，我们评估了一些模型指标，并为我们的问题和数据决定了最佳模型。

请务必查看我们的 [GitHub 资源库](https://github.com/edkrueger/spam-detector)中的自述文件和代码，了解如何使用 Docker 在本地设置该应用程序！

[](/build-a-nlp-pipeline-with-scikit-learn-ham-or-spam-b2cd0b3bc0c1) [## 用 SciKit-Learn 构建 NLP 管道:火腿还是垃圾邮件？

### 使用 Scikit-Learn 的自然语言处理库制作简单的垃圾邮件检测器模型的初学者指南。

towardsdatascience.com](/build-a-nlp-pipeline-with-scikit-learn-ham-or-spam-b2cd0b3bc0c1) 

在将我们的模型部署到云之前，我们需要构建一个应用程序来服务我们的模型。

## 构建应用程序

服务于这个模型的应用程序很简单。我们只需要将我们的模型导入到应用程序中，接收 POST 请求并返回模型对该 POST 的响应。

这是应用程序代码。

注意，`data_dict`是对应于通过 POST 请求发送的有效负载的 Python 字典。从这本词典中我们可以提取要分类的文本。这意味着我们的帖子需要相同的键。所以我们将发送带有`{key: value}`为`{"text": "message")`的 JSONs。

因为 Scikit-Learn 管道需要一个字符串列表，所以我们必须将文本包装成一个列表。此外，当从模型的`.predict` 方法接收结果时，我们接收一个包含单个元素的列表，并对其进行解包以访问预测。

一旦我们让应用程序在本地正常运行，我们就可以对我们的存储库做一些修改，为部署做准备了。

请记住，我们使用包和环境管理器 **pipenv** 来处理我们的应用程序的依赖性。如果您不熟悉虚拟环境和包管理，您可能需要安装它，并使用 Github 存储库中的 Pipfile 设置一个虚拟环境。查看这篇文章来获得帮助！

[](/virtual-environments-for-data-science-running-python-and-jupyter-with-pipenv-c6cb6c44a405) [## 数据科学的虚拟环境:使用 Pipenv 运行 Python 和 Jupyter

### 为 Python 设置虚拟环境。

towardsdatascience.com](/virtual-environments-for-data-science-running-python-and-jupyter-with-pipenv-c6cb6c44a405) 

# 将应用容器化

我们需要对我们的项目文件做一些最后的修改，为部署做准备。在我们的项目中，我们使用 pipenv 和 pipenv-to-requirements 来处理依赖关系并生成一个`requirements.txt`。对于 Docker 容器，您需要的只是`requirements.txt`文件。

确保将您之前创建的 docker 文件添加到您的存储库中。

这是我们 Pipfile 的链接，它是关于这个项目的依赖关系的。

[](https://github.com/edkrueger/spam-detector/blob/master/Pipfile) [## edkrueger/垃圾邮件检测器

github.com](https://github.com/edkrueger/spam-detector/blob/master/Pipfile) 

## Docker 和 docker 文件

在我们让应用程序在云中运行之前，我们必须首先对它进行 Dockerize。查看我们在 [GitHub 资源库](https://github.com/edkrueger/spam-detector)中的自述文件，了解如何使用 Docker 在本地设置这个应用程序。

![](img/5d32d7825c515ab0068fccc40283a5ae.png)

史蒂夫·哈拉马在 Unsplash 上的照片

Docker 是将应用程序投入生产的最佳方式。Docker 使用 docker 文件来构建容器。构建的容器存储在 Google Container Registry 中，在那里可以部署它。Docker 容器可以在本地构建，并将在任何运行 Docker **的系统上运行。**

这是我们在这个项目中使用的 docker 文件:

每个 docker 文件的第一行都以`FROM.`开始，这是我们导入操作系统或编程语言的地方。下一行从 ENV 开始，将环境变量 ENV 设置为`APP_HOME / app`。这模仿了我们的项目目录的结构，让 Docker 知道我们的应用程序在哪里。

这些行是 Python 云平台结构的一部分，你可以在 Google 的 cloud [*文档*](https://cloud.google.com/run/docs/quickstarts/build-and-deploy#python) 中读到更多关于它们的内容。

`WORKDIR`行将我们的工作目录设置为`$APP_HOME`。然后，复制行使本地文件在 docker 容器中可用。

接下来的两行涉及设置环境并在服务器上执行它。`RUN`命令可以跟随着您想要执行的任何 bash 代码。我们使用`RUN`来 pip 安装我们的需求。然后`CMD`运行我们的 HTTP 服务器 gunicorn。最后一行中的参数将我们的容器绑定到`$PORT`，为端口分配一个 worker，指定该端口使用的线程数量，并将应用程序的路径声明为`app.main:app`。

您可以添加一个`.dockerignore`文件来从您的容器映像中排除文件。`.dockerignore`用于将文件隔离在容器之外。例如，您可能不希望在您的容器中包含您的测试套件。

要排除上传到云构建的文件，添加一个`.gcloudignore` 文件。由于 Cloud Build 将您的文件复制到云中，您可能希望忽略图像或数据以降低存储成本。

如果你想使用这些，一定要检查一下`[.dockerignore](https://docs.docker.com/engine/reference/builder/#dockerignore-file)`和`[.gcloudignore](https://cloud.google.com/sdk/gcloud/reference/topic/gcloudignore)`文件的文档，但是，要知道模式和`.gitignore`是一样的！

## 在本地构建并启动 Docker 容器

用这一行命名并构建容器。我们称我们的容器为`spam-detector`。

```
docker build . -t spam-detector
```

要启动我们的容器，我们必须使用这一行来指定容器将使用哪些端口。我们将内部端口设置为 8000，外部端口设置为 5000。我们还将环境变量`PORT`设置为 8000，并输入容器名。

```
PORT=8000 && docker run -p 5000:${PORT} -e PORT=${PORT} spam-detector
```

现在我们的应用程序应该在本地 Docker 容器中启动并运行了。

让我们在运行构建的终端中提供的本地主机地址向应用程序发送一些 JSONs。

## 用 Postman 测试应用程序

Postman 是一个软件开发工具，使人们能够测试对 API 的调用。邮递员用户输入数据。数据被发送到一个 web 服务器地址。信息以响应或错误的形式返回，由邮递员呈现给用户。

邮递员让测试我们的路线变得很容易。打开图形用户界面

*   选择发布并粘贴 URL，根据需要添加路线
*   单击正文，然后单击原始
*   从右边的下拉列表中选择 JSON

确保使用“文本”作为 JSON 中的关键字，否则应用程序将会抛出错误。将您希望模型处理的任何文本作为值放置。现在点击发送！

![](img/5499f4fbd520b3ccbe3388eb7cdbdc15.png)

用 Postman 发送 JSON post 请求

然后在 Postman 中查看结果！看起来我们的邮件被归类为火腿。如果您收到一个错误，请确保您使用了正确的键，并在 POST URL 中添加了路由扩展名`/predict`。

![](img/a2be0f4fd9582016c4194b0d7d21e808.png)

电子邮件是安全的\_(ツ)_/

让我们试试我的 Gmail 垃圾邮件文件夹中的电子邮件。

![](img/4cee603b067869d34cf732b1c28ca4a1.png)

嗯，看起来我们运行的模式与谷歌不同。

现在让我们在没有 Postman 的情况下，只使用命令行来测试这个应用程序。

## 用 curl 测试应用程序

Curl 可以是一个简单的测试工具，允许我们保持在 CLI 中。我不得不做一些调整，让命令与应用程序一起工作，但添加下面的标志解决了错误。

打开终端，插入以下内容。更改文本值以查看模型将哪些内容分类为垃圾邮件和火腿。

```
curl -H "Content-Type: application/json" --request POST -d '{"text": "Spam is my name now give all your money to me"}' [http://127.0.0.1:5000/predict](http://127.0.0.1:5000/predict)
```

结果或错误将会出现在终端中！

```
{"result":"ham"}
```

现在让我们将应用程序部署到谷歌云平台，这样任何人都可以使用它。

## Docker 图像和谷歌云注册表

GCP 云构建允许您使用 docker 文件中包含的指令远程构建容器。远程构建很容易集成到 CI/CD 管道中。由于 Docker 使用大量 RAM，它们还节省了本地计算时间和能量。

一旦我们准备好 docker 文件，我们就可以使用 Cloud Build 构建我们的容器映像。

从包含 Dockerfile 文件的目录中运行以下命令:

```
gcloud builds submit --tag gcr.io/**PROJECT-ID**/**container-name**
```

注意:用您的 GCP 项目 ID 替换项目 ID，用您的容器名称替换容器名称。您可以通过运行命令`gcloud config get-value project`来查看您的项目 ID。

该 Docker 图像现在可在 GCP 集装箱注册处或 GCR 访问，并可通过云运行的 URL 访问。

# 使用 CLI 部署容器映像

1.  使用以下命令进行部署:

```
gcloud run deploy --image gcr.io/**PROJECT-ID**/**container-name** --platform managed
```

注意:用您的 GCP 项目 ID 替换项目 ID，用您的容器名称替换容器名称。您可以通过运行命令`gcloud config get-value project`来查看您的项目 ID。

2.将提示您输入服务名称和区域:选择您所选择的服务名称和区域。

3.您将被提示**允许未认证的调用**:如果您想要公共访问，请响应`y`，以及`n` 限制对同一个 google 项目中的资源的 IP 访问。

4.稍等片刻，直到部署完成。如果成功，命令行会显示服务 URL。

5.通过在 web 浏览器中打开服务 URL 来访问您部署的容器。

# 使用 GUI 部署容器映像

现在我们已经在 GCR 存储了一个容器映像，我们已经准备好部署我们的应用程序了。访问 [GCP 云运行](https://console.cloud.google.com/run?_ga=2.112811590.313737761.1591368161-572251819.1590763098&amp;_gac=1.61558494.1591368161.CjwKCAjw2uf2BRBpEiwA31VZj5hm5tgEHH-Ldim6HaH954LjVPoeEdbL9XkMUnSw3yKCOv1UYdvGdRoCzasQAvD_BwE)并点击创建服务，确保按要求设置计费。

![](img/55ea0a71aed2898350fb0ec4b8b22dc4.png)

选择您想服务的地区，并指定一个唯一的服务名称。然后通过分别选择未经身份验证或经过身份验证，在对应用程序的公共访问和私有访问之间进行选择。

现在我们使用上面的 GCR 容器图像 URL。粘贴 URL 或单击选择并使用下拉列表查找。检查高级设置以指定服务器硬件、容器端口和附加命令、最大请求数和扩展行为。

当您准备好构建和部署时，请单击创建！

![](img/2810bc08ae62f8b2ea06708289e0a6ee.png)

从 GCR 选择一个容器图像

您将进入 GCP 云运行服务详细信息页面，在此您可以管理服务、查看指标和构建日志。

![](img/e6b1bfb20a6b9da7507e4a14107cd3cb.png)

服务详情

单击 URL 查看您部署的应用程序！

![](img/d31ee60c4408f5a974fb213e701ebf07.png)

呜哇！

恭喜你！您刚刚将一个打包在容器中的应用程序部署到云环境中。

您只需为请求处理过程中消耗的 CPU、内存和网络资源付费。也就是说，当你不想付费时，一定要关闭你的服务！

## 结论

我们已经介绍了如何设置一个应用程序来为一个模型提供服务，以及如何在本地构建 docker 容器。然后我们对接我们的应用程序，并在本地进行测试。接下来，我们将 docker 映像存储在云中，并使用它在 Google Cloud Run 上构建一个应用程序。

快速开发出任何像样的好模型都有巨大的商业和技术价值。拥有人们可以立即使用的东西和部署数据科学家可以稍后调整的软件的价值。

我们希望这些内容是信息丰富和有帮助的，让我们知道你想在软件、开发和机器学习领域了解更多的东西！