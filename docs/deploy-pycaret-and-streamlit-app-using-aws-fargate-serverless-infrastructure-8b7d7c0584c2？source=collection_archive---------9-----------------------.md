# 使用 AWS Fargate(无服务器基础设施)部署 PyCaret 并简化应用程序

> 原文：<https://towardsdatascience.com/deploy-pycaret-and-streamlit-app-using-aws-fargate-serverless-infrastructure-8b7d7c0584c2?source=collection_archive---------9----------------------->

![](img/847e7cf4f6d632365b15369c6c2725ee.png)

在 AWS Fargate 上封装和部署 ML pipeline serverless 的循序渐进的初学者指南

# 概述

在我们的[上一篇文章](/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)中，我们展示了如何使用 PyCaret 开发机器学习管道，并将其作为部署在 Google Kubernetes 引擎上的 Streamlit web 应用程序。如果你以前没有听说过 PyCaret，你可以阅读这个[公告](/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46)来开始。

在本教程中，我们将使用我们之前构建的相同的 web 应用程序和机器学习管道，并演示如何使用 AWS Fargate 部署它，这是一种用于容器的无服务器计算。

在本教程结束时，您将能够在 AWS 上构建和托管一个全功能的容器化 web 应用程序，而无需提供任何服务器基础设施。

![](img/3aa2029acb7e59053be5e224a4a7f40f.png)

网络应用

# 👉本教程的学习目标

*   什么是容器？Docker 是什么？什么是 Kubernetes？
*   什么是亚马逊弹性容器服务(ECS)、AWS Fargate 和无服务器部署？
*   构建一个 Docker 映像并将其推送到 Amazon 弹性容器注册中心。
*   使用无服务器基础设施(如 AWS Fargate)部署 web 应用程序。

本教程将涵盖整个工作流程，从在本地构建 docker 映像开始，将其上传到 Amazon Elastic Container Registry，创建一个集群，然后使用 AWS 管理的基础设施定义和执行任务。

过去，我们已经讨论过在其他云平台上的部署，比如 Azure 和 Google。如果您有兴趣了解更多相关信息，可以阅读以下教程:

*   [在谷歌 Kubernetes 引擎上部署 Streamlit 应用](/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)
*   [使用 PyCaret 和 Streamlit 构建和部署机器学习 web 应用](/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104)
*   [在 AWS Fargate 上部署机器学习管道](/deploy-machine-learning-pipeline-on-aws-fargate-eb6e1c50507)
*   [在 Google Kubernetes 引擎上部署机器学习管道](/deploy-machine-learning-model-on-google-kubernetes-engine-94daac85108b)
*   [在 AWS Web 服务上部署机器学习管道](/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01)
*   [在 Heroku PaaS 上构建和部署您的第一个机器学习 web 应用](/build-and-deploy-your-first-machine-learning-web-app-e020db344a99)

# 💻本教程的工具箱

# PyCaret

[PyCaret](https://www.pycaret.org/) 是 Python 中的开源、低代码机器学习库，用于训练和部署机器学习管道和模型到生产中。PyCaret 可以使用 pip 轻松安装。

```
pip install pycaret
```

# 细流

[Streamlit](https://www.streamlit.io/) 是一个开源的 Python 库，可以轻松地为机器学习和数据科学构建漂亮的定制 web 应用。使用 pip 可以轻松安装 Streamlit。

```
pip install streamlit
```

# Windows 10 家庭版 Docker 工具箱

[Docker](https://www.docker.com/) 是一个工具，旨在通过使用容器来更容易地创建、部署和运行应用程序。容器用于打包应用程序及其所有必需的组件，如库和其他依赖项，并作为一个包发送出去。如果你之前没有用过 docker，本教程还涵盖了在 **Windows 10 Home** 上安装 Docker 工具箱(legacy)。在[之前的教程](/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01)中，我们介绍了如何在 **Windows 10 Pro edition** 上安装 Docker Desktop。

# 亚马逊网络服务(AWS)

亚马逊网络服务(AWS)是由亚马逊提供的一个全面且广泛采用的云平台。它拥有来自全球数据中心的超过 175 项全功能服务。如果你以前没用过 AWS，你可以[注册](https://aws.amazon.com/)免费账号。

# ✔️Let's 开始吧…..

# 什么是容器？

在我们开始使用 AWS Fargate 实现之前，让我们了解一下什么是容器，为什么我们需要容器？

![](img/de136ffefe9f778acb5c2d8d14641d88.png)

[https://www.freepik.com/free-photos-vectors/cargo-ship](https://www.freepik.com/free-photos-vectors/cargo-ship)

您是否遇到过这样的问题:您的代码在您的计算机上运行得很好，但是当一个朋友试图运行完全相同的代码时，却无法运行？如果你的朋友重复完全相同的步骤，他们应该得到相同的结果，对不对？对此，一个词的答案是 ***环境*。你朋友的环境和你不同。**

环境包括什么？Python 等编程语言以及所有库和依赖项，以及构建和测试应用程序时使用的确切版本。

如果我们可以创建一个可以转移到其他机器上的环境(例如:你朋友的电脑或者谷歌云平台这样的云服务提供商)，我们就可以在任何地方重现结果。因此，*****容器*** 是一种将应用程序及其所有依赖项打包的软件，因此应用程序可以从一个计算环境可靠地运行到另一个计算环境。**

# **Docker 是什么？**

**Docker 是一家提供软件(也叫 **Docker** )的公司，允许用户构建、运行和管理容器。虽然 Docker 的集装箱是最常见的，但还有其他不太出名的替代品，如 LXD 的[和 LXC 的](https://linuxcontainers.org/lxd/introduction/)。**

**![](img/4f2f499e4bea87156b17080c49e81550.png)**

**现在，您已经从理论上了解了什么是容器，以及 Docker 如何用于容器化应用程序，让我们想象一个场景，其中您必须在一组机器上运行多个容器，以支持一个企业级机器学习应用程序，该应用程序在白天和晚上都有不同的工作负载。这在现实生活中很常见，尽管听起来很简单，但手动完成的工作量很大。**

**您需要在正确的时间启动正确的容器，弄清楚它们如何相互通信，处理存储考虑事项，处理失败的容器或硬件以及数以百万计的其他事情！**

**管理成百上千个容器以保持应用程序正常运行的整个过程被称为**容器编排**。先不要纠结于技术细节。**

**在这一点上，您必须认识到管理现实生活中的应用程序需要不止一个容器，并且管理所有的基础设施以保持容器正常运行是繁琐的、手动的和管理负担。**

**这就把我们带到了 T21。**

# **什么是 Kubernetes？**

**Kubernetes 是谷歌在 2014 年开发的一个开源系统，用于管理容器化的应用程序。简而言之，Kubernetes 是一个跨机器集群运行和协调容器化应用的系统。**

**![](img/760158f6412ec2e143e2723d50525259.png)**

**[丘特尔斯纳](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照**

**虽然 Kubernetes 是谷歌开发的开源系统，但几乎所有主要的云服务提供商都将 Kubernetes 作为托管服务提供。比如:亚马逊**提供的**亚马逊弹性 Kubernetes 服务(EKS)** ，谷歌**提供的**谷歌 Kubernetes 引擎**，微软提供的 **Azure Kubernetes 服务(AKS)** 。**

**到目前为止，我们已经讨论并理解了:**

**✔️一 ***集装箱*****

**✔️码头工人**

**✔️·库伯内特**

**在介绍 AWS Fargate 之前，只剩下一件事要讨论，那就是亚马逊自己的容器编排服务**亚马逊弹性容器服务(ECS)。****

# **AWS 弹性集装箱服务**

**亚马逊弹性容器服务(Amazon ECS)是亚马逊自己开发的容器编排平台。ECS 背后的思想类似于 Kubernetes *(两者都是编排服务)*。**

**ECS 是 AWS 本地服务，这意味着它只能在 AWS 基础设施上使用。另一方面， **EKS** 基于 Kubernetes，这是一个开源项目，用户可以在多云(AWS、GCP、Azure)甚至本地运行。**

**亚马逊还提供基于 Kubernetes 的容器编排服务，称为**亚马逊弹性 Kubernetes 服务(亚马逊 EKS)。**尽管 ECS 和 EKS 的目的非常相似，即*编排容器化的应用程序*，但在定价、兼容性和安全性方面还是有相当大的差异。没有最佳答案，解决方案的选择取决于使用案例。**

**无论您使用哪种容器编排服务(ECS 或 EKS ),都有两种方法可以实现底层基础架构:**

1.  **手动管理群集和底层基础架构，如虚拟机/服务器/(也称为 EC2 实例)。**
2.  **无服务器—完全不需要管理任何东西。只需上传容器就可以了。← **这是 AWS Fargate。****

**![](img/67472e9fda912beabc965aa96c244819.png)**

**亚马逊 ECS 底层基础设施**

# **AWS Fargate —集装箱的无服务器计算**

**AWS Fargate 是一个用于容器的无服务器计算引擎，它与亚马逊弹性容器服务(ECS)和亚马逊弹性库本内特服务(EKS)一起工作。Fargate 使您可以轻松地专注于构建应用程序。Fargate 消除了供应和管理服务器的需要，允许您为每个应用程序指定和支付资源，并通过设计应用程序隔离来提高安全性。**

**Fargate 分配适当的计算量，消除了选择实例和扩展集群容量的需要。您只需为运行容器所需的资源付费，因此不存在过度配置和为额外的服务器付费的问题。**

**![](img/38322de7a7834ab3b05300d5a3c01672.png)**

**AWS Fargate 如何工作—[https://aws.amazon.com/fargate/](https://aws.amazon.com/fargate/)**

**对于哪种方法更好，没有最佳答案。选择无服务器还是手动管理 EC2 集群取决于使用案例。有助于这一选择的一些要点包括:**

****ECS EC2(人工进近)****

*   **你全押在 AWS 上。**
*   **您有专门的运营团队来管理 AWS 资源。**
*   **您在 AWS 上已经有了足迹，即您已经在管理 EC2 实例**

****AWS 法盖特****

*   **您没有庞大的运营团队来管理 AWS 资源。**
*   **你不想要操作责任或想要减少它。**
*   **您的应用程序是无状态的*(无状态应用程序不保存在一个会话中生成的客户端数据，以便在下一个与该客户端的会话中使用)*。**

# **设置业务环境**

**一家保险公司希望通过使用住院时的人口统计和基本患者健康风险指标来更好地预测患者费用，从而改善其现金流预测。**

**![](img/0d35a8d2ddf1693e80660f604ba756ba.png)**

***(* [*数据源*](https://www.kaggle.com/mirichoi0218/insurance#insurance.csv) *)***

# **目标**

**构建和部署一个 web 应用程序，将患者的人口统计和健康信息输入到基于 web 的表单中，然后输出预测的收费金额。**

# **任务**

*   **使用 PyCaret 训练、验证和开发机器学习管道。**
*   **构建一个前端 web 应用程序，具有两个功能:(一)在线预测和(二)批量预测。**
*   **创建 Dockerfile 文件**
*   **创建并执行任务，使用 AWS Fargate 无服务器基础架构部署应用程序。**

**由于我们已经在上一篇教程中介绍了前两项任务，我们将快速回顾一下，然后将注意力集中在上面列表中的剩余项目上。如果您有兴趣了解更多关于使用 PyCaret 在 Python 中开发机器学习管道以及使用 Streamlit 框架构建 web 应用程序的信息，请阅读本教程。**

# **👉任务 1 —模型训练和验证**

**我们正在使用 Python 中的 PyCaret 进行训练，并开发一个机器学习管道，它将作为我们 web 应用程序的一部分。机器学习管道可以在集成开发环境(IDE)或笔记本中开发。我们使用笔记本运行了以下代码:**

**当您在 PyCaret 中保存一个模型时，基于在 **setup()** 函数中定义的配置的整个转换管道被创建。所有的相互依赖都是自动编排的。查看存储在“deployment_28042020”变量中的管道和模型:**

**![](img/a9845c2631e671c47ec93a2f730018ec.png)**

**使用 PyCaret 创建的机器学习管道**

# **👉任务 2 —构建前端 web 应用程序**

**现在，我们的机器学习管道和模型已经准备好开始构建一个前端 web 应用程序，它可以在新的数据点上生成预测。该应用程序将通过 csv 文件上传支持“在线”以及“批量”预测。让我们将应用程序代码分成三个主要部分:**

# **页眉/布局**

**该部分导入库，加载训练模型，并创建一个基本布局，顶部有一个徽标，一个 jpg 图像，边栏上有一个下拉菜单，用于在“在线”和“批量”预测之间切换。**

**![](img/a0a3fb317ae6bc377163799607f0afb2.png)**

**app.py —代码片段第 1 部分**

## **在线预测**

**本节处理初始 app 功能，在线逐一预测。我们正在使用 streamlit 小部件，如*数字输入、文本输入、下拉菜单和复选框*来收集用于训练模型的数据点，如年龄、性别、身体质量指数、儿童、吸烟者、地区。**

**![](img/4cf119b32a91f815e163a73f613006a4.png)**

**app.py —代码片段第 2 部分**

## **批量预测**

**批量预测是该应用的第二层功能。streamlit 中的 **file_uploader** 小部件用于上传 csv 文件，然后从 PyCaret 调用本机 **predict_model()** 函数来生成预测，使用 streamlit 的 write()函数显示这些预测。**

**![](img/aec59199125171120d49ba7ca4b1ded9.png)**

**app.py —代码片段第 3 部分**

****测试应用程序** 在 AWS Fargate 上部署应用程序之前的最后一步是在本地测试应用程序。打开 Anaconda 提示符，导航到您的项目文件夹并执行以下代码:**

```
**streamlit run app.py**
```

**![](img/3aa2029acb7e59053be5e224a4a7f40f.png)**

**简化应用测试—在线预测**

# **👉任务 3 —创建 Dockerfile 文件**

**为了将我们的应用程序进行容器化部署，我们需要一个 docker 映像，它在运行时成为一个容器。使用 docker 文件创建 docker 映像。Dockerfile 只是一个包含一组指令的文件。该项目的 docker 文件如下所示:**

**这个 Dockerfile 文件的最后一部分(从第 23 行开始)是特定于 Streamlit 的。Dockerfile 区分大小写，必须与其他项目文件位于项目文件夹中。**

# **👉任务 4–在 AWS Fargate 上部署:**

**按照以下 9 个简单步骤在 AWS Fargate 上部署应用程序:**

## **👉步骤 1 —安装 Docker 工具箱(适用于 Windows 10 家庭版)**

**为了在本地构建 docker 映像，您需要在您的计算机上安装 Docker。如果您使用的是 64 位 Windows 10:Pro、Enterprise 或 Education (Build 15063 或更高版本)，您可以从 [DockerHub](https://hub.docker.com/editions/community/docker-ce-desktop-windows/) 下载 Docker Desktop。**

**但是，如果你使用的是 Windows 10 Home，你需要从 [Dockers GitHub 页面](https://github.com/docker/toolbox/releases)安装旧版 Docker 工具箱的最新版本(v19.03.1)。**

**![](img/a7494136fd44e74c4cc0a6cd374e82b8.png)**

**https://github.com/docker/toolbox/releases**

**下载并运行 DockerToolbox-19.03.1.exe 文件**。****

**检查安装是否成功的最简单方法是打开命令提示符并键入“docker”。它应该打印帮助菜单。**

**![](img/13713c5e30cad31fb82c0129e4f28e4b.png)**

**Anaconda 提示检查 docker**

## **👉步骤 2 —在弹性容器注册中心(ECR)中创建一个存储库**

****(a)登录您的 AWS 控制台并搜索弹性容器注册表:****

**![](img/096f291d81c2ff48f3c676ffbf55893e.png)**

**AWS 控制台**

****(b)创建一个新的存储库:****

**![](img/a9bd16e189111e603cec17b2994754c5.png)**

**在 Amazon 弹性容器注册表上创建新的存储库**

**![](img/00b8ae018117718eef7ca70216bd8b25.png)**

**创建存储库**

**点击“创建存储库”。**

****(c)点击“查看推送命令”:****

**![](img/a49ff3e01ea4baddee2d71f2f04bb6f4.png)**

**pycaret-streamlit-aws 存储库的推送命令**

## **👉步骤 3—执行推送命令**

**使用 Anaconda 提示符导航到您的项目文件夹，并执行您在上一步中复制的命令。在执行这些命令之前，您必须位于 docker 文件和其余代码所在的文件夹中。**

**这些命令用于构建 docker 映像，然后将其上传到 AWS ECR。**

## **👉步骤 4-检查您上传的图像**

**单击您创建的存储库，您将看到上一步中上传的图像的图像 URI。复制图像 URI(这将需要在下面的步骤 6)。**

**![](img/29bdf86a4a53ef11965935998ba06ddf.png)**

## **👉步骤 5 —创建和配置集群**

****(a)点击左侧菜单上的“集群”:****

**![](img/adfa7500c896096ef8fbeb825a224ef5.png)**

**创建集群—步骤 1**

****(b)选择“仅联网”并点击下一步:****

**![](img/b984d2c9b748e7c3c4293972e1900dba.png)**

**选择仅网络模板**

****(c)配置集群(输入集群名称)并点击创建:****

**![](img/222587fb8c20f023166f95f92bd26a2b.png)**

**配置集群**

**点击“创建”。**

****(d)集群创建:****

**![](img/49e1c880acc243e173e9c20d8614434e.png)**

**集群已创建**

## **👉步骤 6 —创建新的任务定义**

**在 Amazon ECS 中运行 Docker 容器需要一个**任务**定义。您可以在**任务**定义中指定的一些参数包括:Docker 图像，用于您的**任务**中的每个容器。每个**任务**或**任务**中的每个容器使用多少 CPU 和内存。**

****(a)点击“创建新任务定义”:****

**![](img/8126749319ad2764d4d3204cf15b8555.png)**

**创建新的任务定义**

****(b)选择“FARGATE”作为发射类型:****

**![](img/7b1d46b57c039428fc0e4de1e7e0c0b3.png)**

**选择启动类型兼容性**

****(c)详细填写:****

**![](img/d92bb06ca06a858d798e500fe6b69483.png)**

**配置任务和容器定义(第 1 部分)**

**![](img/c4cd582f3944673fde2dd3e7d87f0f7c.png)**

**配置任务和容器定义(第 2 部分)**

****(d)点击“添加容器”并填写详细信息:****

**![](img/74986fcf724eeed4aa5f0c7b1d8b7b3a.png)**

**在任务定义中添加容器**

**点击右下角的“创建任务”。**

**![](img/28dcc011d872c4d5ead7e905b90f4c69.png)**

## **👉步骤 7-执行任务定义**

**在最后一步中，我们创建了一个启动容器的任务。现在我们将通过点击动作下的**“运行任务”**来执行任务。**

**![](img/05154dac6072baafde0f325d082a15ac.png)**

****(a)点击“切换到发射类型”将类型切换到 Fargate:****

**![](img/0571172ded58c382eacde29ea075d6ea.png)**

****(b)从下拉菜单中选择 VPC 和子网:****

**![](img/436316698c49cbb007f6ed1e02511e3a.png)**

**点击右下角的“运行任务”。**

## **👉步骤 8-从网络设置中允许入站端口 8501**

**在我们看到我们的应用程序在公共 IP 地址上运行之前，最后一步是通过创建一个新规则来允许端口 8501(由 streamlit 使用)。为此，请遵循以下步骤:**

****(a)点击任务****

**![](img/630a584fe92ae6b8cf327a370dbc3227.png)**

****(b)点击 ENI Id:****

**![](img/d3ba3d21d1b54308b9cf991b69b22603.png)**

****(c)点击安全组****

**![](img/12c979b2ba607c9d2696cf0f8f199c0e.png)**

****(d)向下滚动并点击“编辑入站规则”****

**![](img/c903d217108fdf6993a0f6fed261df91.png)**

****(e)添加端口 8501 的自定义 TCP 规则****

**![](img/24ec240875cea8a40eb7e0c1663d63a0.png)**

# **👉恭喜你！您已经在 AWS Fargate 上发布了您的无服务器应用程序。使用带有端口 8501 的公共 IP 地址访问应用程序。**

**![](img/17d09394619185f0e0a908dfd2188295.png)**

**在 99.79.189.46:8501 发布的应用程序**

****注意:**在这篇文章发表时，该应用程序将从公共地址中删除，以限制资源消耗。**

**[链接到本教程的 GitHub 库](https://www.github.com/pycaret/pycaret-streamlit-aws)**

**[Google Kubernetes 部署的 GitHub 知识库链接](/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)**

**[链接到 Heroku 部署的 GitHub 存储库](/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104)**

# **PyCaret 2.0.0 来了！**

**我们收到了来自社区的大力支持和反馈。我们正在积极改进 PyCaret，并准备我们的下一个版本。 **PyCaret 2.0.0 会更大更好**。如果您想分享您的反馈并帮助我们进一步改进，您可以[在网站上填写此表格](https://www.pycaret.org/feedback)或者在我们的 [GitHub](https://www.github.com/pycaret/) 或 [LinkedIn](https://www.linkedin.com/company/pycaret/) 页面上发表评论。**

**关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 并订阅我们的 [YouTube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g) 频道，了解更多关于 PyCaret 的信息。**

# **想了解某个特定模块？**

**从第一个版本 1.0.0 开始，PyCaret 有以下模块可供使用。点击下面的链接，查看 Python 中的文档和工作示例。**

**[分类](https://www.pycaret.org/classification)
[回归](https://www.pycaret.org/regression) [聚类](https://www.pycaret.org/clustering)
[异常检测](https://www.pycaret.org/anomaly-detection) [自然语言处理](https://www.pycaret.org/nlp)
关联规则挖掘**

# **另请参见:**

**笔记本中的 PyCaret 入门教程:**

**[分类](https://www.pycaret.org/clf101)
[回归](https://www.pycaret.org/reg101)
[聚类](https://www.pycaret.org/clu101)
异常检测
[自然语言处理](https://www.pycaret.org/nlp101)
[关联规则挖掘](https://www.pycaret.org/arul101)**

# **你愿意投稿吗？**

**PyCaret 是一个开源项目。欢迎每个人都来投稿。如果您愿意投稿，请随时关注[未决问题](https://github.com/pycaret/pycaret/issues)。dev-1.0.1 分支上的单元测试接受拉请求。**

**如果你喜欢 PyCaret，请给我们 GitHub 回购的⭐️。**

**中等:【https://medium.com/@moez_62905/】T42**

**领英:[https://www.linkedin.com/in/profile-moez/](https://www.linkedin.com/in/profile-moez/)**

**推特:[https://twitter.com/moezpycaretorg1](https://twitter.com/moezpycaretorg1)**