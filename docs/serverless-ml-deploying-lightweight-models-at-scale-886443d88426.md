# 无服务器 ML:大规模部署轻量级模型

> 原文：<https://towardsdatascience.com/serverless-ml-deploying-lightweight-models-at-scale-886443d88426?source=collection_archive---------54----------------------->

## [理解大数据](https://towardsdatascience.com/tagged/making-sense-of-big-data)

![](img/3bc4b447419b4c6eb74dc78ef01f28cf.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Zbynek Burival](https://unsplash.com/@zburival?utm_source=medium&utm_medium=referral) 拍摄的照片

# 部署难题

将机器学习(ML)模型部署到生产中有时会成为数据科学(DS)团队的绊脚石。一种常见的部署模式是找到一个地方来托管您的模型，并通过 API 公开它们。在实践中，这可以让您的最终用户轻松地将您的模型输出直接集成到他们的应用程序和业务流程中。此外，如果客户*信任*你的 API 的输出*和*性能的有效性，这可以驱动巨大的商业价值:你的模型可以对目标商业问题产生*直接*和*持久*的影响。

但是，如果您无法获得 DevOps 或 MLOps 团队形式的持续技术支持，那么就要费力地通过云服务来设置负载平衡器、API 网关、持续集成和交付管道、安全设置等。开销会很大。此外，除非你对这些概念非常有信心，否则交付(和监控)一个 ML API 来保证大规模的安全性和性能，从而赢得用户的信任是很有挑战性的。

当然，有越来越多的服务可以帮助你完成这个过程。也许你的第一站会是主要云提供商的托管模型部署服务(比如 [SageMaker](https://aws.amazon.com/sagemaker/) )。或者，你可能会关注一个蓬勃发展的 MLOps 工具/平台，如 [Cortex](https://github.com/cortexlabs/cortex/tree/0.15) 或 [Seldon](https://github.com/SeldonIO/seldon-core) ，或者甚至你可能会选择一头扎进类似 [Kubeflow](https://www.kubeflow.org/docs/components/serving/kfserving/) 和 [TensorFlow Serving](https://www.tensorflow.org/tfx/guide/serving) 的东西。当你开始工作时，可能会有点困惑。

> 如果你想大规模地部署模型，你将不得不花费大量的时间学习一些技术，并且熟悉一些软件工程概念。

这里还有另一个问题。虽然这些工具和平台自动化了许多特定于 ML 的任务，并且通常减少了部署 ML 模型的认知负担，但是它们仍然涉及大量的开销:在您可能有信心在实践中使用这些工具之前，您仍然需要花费相当多的时间来阅读文档和使用示例。事实上，一般来说，如果您想要大规模地部署模型，您将不得不花费大量的时间来学习相当多的技术，并且熟悉相当多的软件工程概念。即使有了这些新工具，也没有捷径可走。

虽然这都是事实，但当然总会有特例。在这些特殊情况下，你*可以*走一些捷径，帮助你以最小的开销快速地将一个 ML 模型投入生产。这就是这篇文章的内容:它将给出一个关于什么是特例的高层次概述；简要介绍无服务器计算的概念；然后介绍一个代码例子(带回购！)来部署一个特例 ML 模型作为 Google Cloud 函数，只需要几行代码。

# 特殊情况

有一种说法由来已久，经常被引用，但偶尔会被误用(被忽略？)DS、软件工程和其他领域的概念，从小、干净、简单开始，然后随着时间的推移增加复杂性。从 ML 模型的角度来看，这意味着从最简单的 ML 模型开始(即产生有用的商业价值)，并从那里开始。例如，这可能意味着——如果给定的业务问题允许的话——在达到一个巨大的梯度提升树或深度神经网络之前，你可能想尝试玩一些简单的线性模型*。*

这些更复杂的建模方法自然对 DS/ML 领域的人有吸引力:它们在许多应用程序中显然非常强大。但是也有不利的一面。对一个人来说，解释可能很快变得困难。在某些情况下，对小数据集的不良概括也是一种真正的危险。从部署的角度来看，也存在一些潜在的问题。首先:提供这些功能的包和库通常很重。它们通常有很大的二进制文件(即占用大量磁盘空间)，它们有很高的内存和 CPU(还有 GPU/TPU)需求，它们有时会有(相对)差的推理性能，并且通常有臃肿的图像(例如，如果使用 Docker 这样的技术)。

相比之下，简单的线性模型通常具有最小的依赖性(如果需要，还需要几十行纯 [NumPy](https://github.com/numpy/numpy) 代码来从头实现一个轻量级的线性模型)，训练后几乎为零的资源需求，以及闪电般的推理性能。冒着听起来过时的风险:你可以用一堆设计良好的线性模型走很长的路。

> *冒着听起来过时的风险:你可以用一堆设计良好的线性模型走很长的路。*

所有这些都是在说:如果你能用线性模型很好地解决一个商业问题，你最好至少从*那里开始*。如果你从那里开始(并且使用 Python——无服务器的限制，而不是部落的东西！)，那你就是特例之一。恭喜你！将轻量级模型部署为无服务器功能可能会为您节省大量时间和痛苦，并且可能是您的用例的理想选择。

# 走向无服务器

在过去的几年里，无服务器计算的概念已经在软件工程领域得到了迅速发展。其基本思想是，通过在很大程度上消除工程师手动配置其服务器、负载平衡器、容器等的需求，云提供商可以在抽象出(隐藏)将应用部署到生产环境中的复杂性方面大有作为。*在*能够产生任何商业价值之前。

在某些方面，这类似于上面概述的一些数据科学家面临的问题:在稳定的服务中获得一个模型并最终产生商业价值通常需要大量的开销。无服务器计算旨在消除大部分这种开销:原则上，目标是让您，即开发人员，编写一个简单的函数，并立即以理论上无限可扩展和安全的方式将其部署到“生产”中。

> *…我们的目标是让开发人员编写一个简单的功能，并立即以理论上无限可扩展和安全的方式将其部署到“生产”中。*

此外，无服务器计算明确旨在支持“事件驱动”应用。例如，如果您需要一个函数(模型)在每次云存储中的特定文件发生变化时运行，或者在每天的特定时间运行，或者在每次新客户注册您的服务时运行，您可以轻松地配置您的函数。你可以免费获得这种功能。听起来很酷，对吧？

除了这里的讨论之外，这篇文章不会涉及无服务器的基础知识。如果您想更深入地了解无服务器的工作方式，以及它的相对优势和劣势，那么您应该看看以前的一篇帖子:

[](https://medium.com/@mark.l.douthwaite/a-brief-introduction-to-serverless-computing-e867eb71b54b) [## 无服务器计算简介

### 这篇文章介绍了无服务器计算的一些基本概念，并帮助您将无服务器应用程序部署到 GCP。

medium.com](https://medium.com/@mark.l.douthwaite/a-brief-introduction-to-serverless-computing-e867eb71b54b) 

现在举个例子！

# 编码时间到了

这个例子将向您展示如何构造和构建一个简单的 ML 管道，用于训练一个 [Scikit-Learn 模型](https://scikit-learn.org/stable/)(在本例中是一个简单的`LogisticRegression`模型)来使用 [UCI 心脏病数据集](https://www.kaggle.com/ronitf/heart-disease-uci)预测心脏病，然后将其部署为谷歌云功能。这不是数据分析的练习，所以不要期望对数据探索和建模决策进行太多的讨论！如果你想深入研究代码，可以这样做:

[](https://github.com/markdouthwaite/serverless-scikit-learn-demo) [## markdouthwaite/无服务器-scikit-learn-demo

### 一个提供演示代码的存储库，用于部署基于轻量级 Scikit-Learn 的 ML 管道心脏病建模…

github.com](https://github.com/markdouthwaite/serverless-scikit-learn-demo) 

对于其他人，请继续阅读！

# 开始之前…

你需要注册一个谷歌云账户，并确保你已经阅读了[上一篇介绍无服务器计算的文章](https://mark.douthwaite.io/a-brief-introduction-to-serverless-computing/)。谷歌目前提供 300 美元的“免费”积分，这对于本教程和你自己的一些项目来说已经足够了。只是记得当你完成后禁用你的帐户！声明:这篇文章与谷歌没有任何关系——它只是一个慷慨的提议，对于那些想增长云服务知识的人来说可能很方便！

[](https://cloud.google.com/free) [## GCP 免费等级免费扩展试用和永远免费|谷歌云

### 20 多种免费产品免费体验热门产品，包括计算引擎和云存储，最高可达…

cloud.google.com](https://cloud.google.com/free) 

此外，您需要在系统上安装 Python 3.7，并访问 GitHub。如果你经常使用 Python 和 GitHub，应该没问题。如果没有，您可以检查您安装的 Python 版本:

```
python --version
```

如果你没有看到`Python 3.7.x`(其中`x`将是一些次要版本)，你需要安装它。您可能会发现`pyenv`对此很有帮助。下面是用 pyenv 安装特定版本 Python 的指南。你可以通过 GitHub 的[“Hello World”指南](https://guides.github.com/activities/hello-world/)了解如何开始使用 GitHub。

# 克隆存储库

首先:您需要存储库。您可以通过以下方式克隆存储库:

```
git clone [https://github.com/markdouthwaite/serverless-scikit-learn-demo](https://github.com/markdouthwaite/serverless-scikit-learn-demo)
```

或者你可以[直接从 GitHub 克隆这个库](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository)。

# 盒子里面是什么？

现在，导航到这个新克隆的目录。存储库提供了一个简单的框架，用于构建您的项目和代码，以便将其部署为云功能。有几个文件你应该熟悉一下:

*   Python 捕捉项目依赖关系的古老(如果有缺陷的话)惯例。在这里，您可以找到运行云功能所需的软件包列表。Google 的服务会查找这个文件，并在运行您的功能之前自动安装其中列出的文件。
*   `steps/train.py` -这个脚本训练你的模型。它构建了一个 [Scikit-Learn 管道](https://scikit-learn.org/stable/modules/compose.html),提供了一种简洁的“规范”方式将您的预处理和后处理绑定到您的模型，作为一个单独的可移植块。这使得将它部署为云功能(以及一般的 API)变得更加容易和干净。).当它完成训练后，它将对模型进行简单的评估，并将一些统计数据打印到终端。生成的模型将用 [joblib](https://joblib.readthedocs.io/en/latest/) 进行[酸洗](https://docs.python.org/3/library/pickle.html#:~:text=%E2%80%9CPickling%E2%80%9D%20is%20the%20process%20whereby,back%20into%20an%20object%20hierarchy.)并作为`pipeline.joblib`保存在`artifacts`目录中。实际上，你可能会发现将这些文件保存在谷歌云存储中很有用，但现在将它们存储在本地也可以。
*   `main.py` -这个模块包含你的“处理程序”。这是用户在部署服务时调用您的服务时将与之交互的内容。您可能会注意到`init_predict_handler`函数的结构有点奇怪。这是因为函数需要在第一次加载`main`模块时加载您的模型，并且您的函数需要维护一个对已加载模型的引用。当然，您也可以简单地将它加载到函数范围之外，但是所示的结构通过将模型的“可见性”仅限于函数本身，而不是您可能编写的访问该模块的任何其他代码，缓解了我的强迫症。
*   `app.py` -该模块提供了一个最小的`Flask`应用程序设置，用于在本地(即在部署之前)测试您的云功能*。你可以用`python app.py`启动它，然后像平常一样调用它。*
*   `datasets/default.csv` -本例的“默认”数据集。这是 CSV 格式的 UCI 心脏病数据集。
*   `resources/payload.json` -一个示例负载，当它被部署时，你可以发送给 API。方便，嗯？
*   `notebooks/eda.ipynb` -一个虚拟笔记本，用于说明在模型开发过程中您可能需要存储探索性数据分析(EDA)代码的位置。

如果你已经阅读了上一篇关于无服务器概念的文章，这种结构应该对你有所帮助。无论如何，现在是有趣的部分，实际运行代码。

# 训练模型

自然，要部署一个模型，您首先需要一个模型。为此，您需要使用`train.py`脚本。这个脚本使用 [Fire](https://mark.douthwaite.io/fire-simple-clis-done-fast/) 来帮助您从命令行配置输入、输出和模型参数。您可以使用以下命令运行该脚本:

```
python steps/train.py --path=datasets/default.csv --tag=_example --dump
```

这是在做什么？它告诉脚本从`datasets/default.csv`加载数据，用标签`example`标记输出模型，并将模型文件转储(写入)到目标位置(应该是`artifacts/pipeline_example.joblib`)。您应该会看到如下输出:

```
Training accuracy: 86.78% Validation accuracy: 88.52% ROC AUC score: 0.95
```

不是一款*可怕的*车型呃？全都来自老派的逻辑回归。现在是派对部分:部署模型。

# 部署模型

现在您已经有了模型，您可以部署它了。你需要安装谷歌云的命令行工具。您可以使用他们的指南为您的系统[执行此操作。完成后，在项目目录内的终端中，您可以使用以下命令部署您的新模型:](https://cloud.google.com/sdk/docs/quickstarts)

```
gcloud functions deploy heart-disease --entry-point=predict_handler --runtime=python37 --allow-unauthenticated --project={project-id} --trigger-http
```

您需要用`{project-id}`替换您的项目 ID(您可以从您的 Google Cloud 控制台获得)。几分钟后，您的模型应该是活动的。您可以在以下网址查询您的新 API:

```
[https://{subdomain}.cloudfunctions.net/heart-disease](https://{subdomain}.cloudfunctions.net/heart-disease)
```

当您的模型被部署时，终端输出将给出您可以调用的特定 URL(替换您的`{subdomain}`)。简单吧？您新部署的模型基本上可以无限扩展，这很好。

# 查询模型

最后，是时候做一些预测了。您可以发送:

```
curl --location --request POST 'https://{subdomain}.cloudfunctions.net/heart-disease' --header 'Content-Type: application/json' -d @resources/payload.json
```

您应该得到:

```
{"diagnosis":"heart-disease"}
```

作为回应。就这样，你有了一个活的 ML API。恭喜你！

# 要记住的事情

正如您所看到的，将您的 ML 模型部署为无服务器功能可能是获得一个稳定的、可伸缩的 ML API 的最快、最简单的途径。但一如既往，这是有代价的。以下是一些需要记住的事情:

*   无服务器功能(如 Google Cloud 功能)通常会受到资源(CPU、RAM)的限制，因此在无服务器功能中加载和使用“大”模型通常会有问题。
*   无服务器函数在高响应性时工作得最好(例如，在 ML 情况下推理时间非常快)。如果您的模型有点慢，或者依赖于其他慢的服务(例如，慢的 SQL 查询)，这可能也不是最佳选择。
*   无服务器功能通常被设计用来处理大量的小请求。如果您的用例涉及大的、批量的查询，它们可能不是一个很好的选择——通常对请求有严格的超时限制。
*   一段时间未使用的无服务器功能会关闭。你一打电话给它们，它们就会转回来。当功能“预热”时，这会产生短暂的延迟。此外，例如，如果您正在使用 Python，Python 解释器众所周知的缓慢启动时间可能会成为问题
*   无服务器功能通常对常见的“基础设施”语言如 Node JS 和 Python 有一流的支持。如果你用的是 R，MATLAB，Julia 等。(包括通过`rpy2`作为 Python 的依赖项)支持从不存在到很差不等。有[种变通方法](https://ericjinks.com/blog/2019/serverless-R-cloud-run/)，但是这些方法通常会带来性能损失和复杂性增加(在某种程度上降低了“无服务器”概念的价值)。

*原载于 2020 年 8 月 13 日*[*https://mark . douthwaite . io*](https://mark.douthwaite.io/serverless-machine-learning/)*。*