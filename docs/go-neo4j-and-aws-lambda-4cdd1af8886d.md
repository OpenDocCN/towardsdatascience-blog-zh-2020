# Go，Neo4J，还有 AWS Lambda…

> 原文：<https://towardsdatascience.com/go-neo4j-and-aws-lambda-4cdd1af8886d?source=collection_archive---------64----------------------->

## 以及让它们一起工作的本质

我几乎每天都与 Neo4j 一起工作，作为一名 Python 开发人员，我通常将他们两人结合在一起编写我的大部分逻辑。然而，在过去的几天里，自从我开始使用 Golang，我似乎对它产生了强烈的亲和力。

我想使用 AWS Lambda 构建一个简单的服务，对我的 Neo4j 数据库执行简单的查询，并决定使用 Go 来改变一下。得知 Neo4j 有官方的 Go 驱动程序，我明显松了一口气。但是最初的几次尝试完全失败了。

不过不要担心，好消息是，你肯定可以一起使用 AWS Lambda、Go 和 Neo4j。只有几个额外的步骤需要首先执行。

# 问题是…

> 是的，Neo4j 有一个 [**官方驱动给 Golang**](https://github.com/neo4j/neo4j-go-driver) 。
> 
> 不，不是纯围棋写的。

这是什么意思？neo4j-go-driver 依赖名为“ [**Seabolt**](https://github.com/neo4j-drivers/seabolt) ”的 C 库通过 bolt 协议连接到数据库实例。

这意味着在开始执行 Neo4j 的 Go 代码之前，您必须显式地安装这个库。此外，seabolt 反过来要求在您的本地系统上安装 **OpenSSL(对于 Windows 是 TLS)**才能正常工作。

在本地机器上安装 OpenSSL 和 Seabolt 非常容易。事实上，OpenSSL 预装在许多操作系统中。(我使用的 Linux 系统已经有了 OpenSSL，我所要做的就是按照这些**指令** 安装 Seabolt)

当我们想要使用 AWS Lambda 复制同样的东西时，挑战就出现了。Go 作为一种静态类型语言**首先被编译成二进制文件，然后作为 Lambda 函数的处理程序。通常，如果所有的依赖项都是基于 Go 的，或者如果您在本地系统上工作，这不会导致任何问题。**

然而，在我们的例子中，Go 二进制依赖于外部 C 目标文件，并在运行时链接它们。但是我们如何让这些 C 文件对 lambda 函数可用呢？

# 解决方案

在花了相当多的时间在互联网上寻找可能的解决方案后，我找不到任何具体的东西，尤其是围棋。大多数解决方案都是针对 python 的(因为某些 python 库也使用 C 依赖)，我几乎想回到我的老朋友 Python。但是，即使语言不同，概念是相同的。

## 进入 AWS Lambda 层…

从最简单的意义上来说， [Lambda 层](https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html)是一段可以被不同服务共享的代码。通常，一个层将由大多数功能所需的重量级依赖项组成。在我们的例子中，我们将把我们的 C 依赖打包在一起，并使用层使它们对 lambda 函数可用。

## TL；速度三角形定位法(dead reckoning)

包括 lambda 函数示例和安装说明的完整代码在我的 GitHub 库的[中。](https://github.com/vedashree29296/neo4j-go-lambda-connector)

## 步骤 1:打包 C 库

由于 AWS 经常使用基于 **CentOS 的**容器来部署 lambda 函数， **Docker** 成为下载和编译 C 依赖项的明显选择。

我们使用**lambda-base**映像作为基础映像创建一个定制的 docker 映像，并在其上安装我们所有的依赖项。

安装海锚的文件

基本映像已经安装并配置了 OpenSSL。现在唯一需要的图书馆是 Seabolt。

Seabolt 作为**下载。tar 包**并提取到 docker 映像中的 **/usr/local/lib64** 目录。为了使它对 Lambda 函数可用，我们将内容移动到 **/opt/lib** 目录，这是运行时链接期间搜索的目录之一。(这是在名为 **LD_LIBRARY_PATH** 的环境变量中指定的)

一旦构建了映像，我们需要将 **/opt/lib** 文件夹的内容转储到本地系统上的一个文件夹中，该文件夹稍后将被转换为一个层

打包依赖项的脚本

运行这个脚本将创建一个名为**图层**的目录，其中包含。所以在**层/lib** 文件夹中的文件是针对 Seabolt 的。

## 步骤 2:创建一个层

现在，我们要做的就是创建一个层。有两种方法可以做到这一点。首先，我们使用 AWS 控制台或 CLI 创建一个层，并将层/目录的内容作为. zip 文件上传。

另一种方法是使用 [**无服务器框架**](https://www.serverless.com/) ，它会为你做所有的工作。你所需要的只是一个文件调用 **serverless.yml** 来指定你的层的细节。

用于创建图层的 serverlee.yml

从您的工作目录中运行 run `serverless deploy`。

你将为你新创建的层获得 **ARN URL** ，你可以很容易地将它附加到你的 lambda 函数上。

## 下一步:创建自己的 Go 处理函数

一旦成功地创建了这个层，您就可以继续创建您的自定义处理函数来与 Neo4J 通信。

GitHub 库包括一个示例处理函数，它在 Neo4j 中创建一个简单的节点，这个节点是使用无服务器框架部署的。关于如何使用 Go 和 serverless 创建 Lambda 函数的更多参考，这是一个[有用的链接，可以从](https://www.serverless.com/examples/aws-golang-http-get-post/)开始。

感谢阅读！