# 如何服务您的 PyTorch 模型

> 原文：<https://towardsdatascience.com/how-to-deploy-your-pytorch-models-with-torchserve-2452163871d3?source=collection_archive---------15----------------------->

## TorchServe 的最新版本离 GA 又近了一步。

![](img/d69053d1a076d7039f6812bcc825c7cc.png)

由 BENCE BOROS 在 Unsplash 上拍摄的照片

你已经收集了你的数据，处理了它们，训练了你的模型，微调了它，结果是有希望的。接下来去哪里？你如何让公众获得它？

嗯，如果你是一个 [TensorFlow](https://www.tensorflow.org/) 用户，你可能想摆弄一下 [TFX](https://www.tensorflow.org/tfx) 。TFX 是一个优秀的工具集，帮助数据科学家创建和管理机器学习生产管道。但是如果你是 PyTorch 用户呢？嗯，你很幸运；截至 2020 年 4 月，你获得了[**torch serve**](https://pytorch.org/serve/)**。**

> [学习率](https://www.dimpo.me/newsletter)是我每周给那些对 AI 和 MLOps 世界好奇的人发的简讯。你会在每周五收到我关于最新人工智能新闻、研究、回购和书籍的更新和想法。在这里订阅！

# 火炬服务是什么

TorchServe 是一个灵活易用的工具，用于服务 PyTorch 模型。它没有 TFX 的复杂性，因此也没有提供那么多的功能。然而，这是完成工作的直接方法！

![](img/7d5161371851a821351fbf52eea33af9.png)

TorchServe 提供了一组必要的特性，比如服务器、模型归档工具、API 端点规范、日志记录、度量、批量推断和模型快照等等。它还提供了一系列高级特性，例如，支持定制推理服务、单元测试和通过 JMeter 收集基准数据的简单方法。目前，它还处于试验阶段，但它在大多数情况下都很有效。事实上，在本文的后面部分，我们将对它进行测试。

# 最新版本

TorchServe 在两天前(2020 年 6 月 10 日)发布了最新版本(0.1.1)。这是向 GA(全面上市)迈出的一步，包括一系列有希望的新功能。除其他外，TorchServe 的最新版本包括:

*   支持 [HuggingFace](https://huggingface.co/) ，这是一个深度学习库，其任务是为每个人推进和民主化 NLP，提供文档和示例
*   支持 [Nvidia Waveglow](https://github.com/NVIDIA/waveglow) ，一个基于流的语音合成生成网络，有文档和例子
*   与 Model Zoo 紧密集成，Model Zoo 是一个深度学习注册表，具有从流行的预训练模型创建的模型档案
*   支持 [AWS 云形成](https://aws.amazon.com/cloudformation/)，它提供了在 EC2 实例上运行服务器的能力，并提供了一个简单的配置文件(YAML 或 JSON)
*   支持通过 [snakevize](https://jiffyclub.github.io/snakeviz/) 分析器分析 TorchServe Python 执行，以获得详细的执行时间报告
*   具有清晰说明的重构文档

有关可用功能的详细列表，请阅读[发行说明](https://github.com/pytorch/serve/releases)。

# 装置

安装 TorchServe 很简单。您可以使用[文档](http://pytorch.org/serve/install.html)中提供的说明，但是让我们使用 [conda](https://www.anaconda.com/) 在 Ubuntu 上安装它:

1.  为您的 TorchServe 安装创建一个新环境(可选但推荐)。

```
conda create -n torch python=3.8
```

2.激活新环境。

```
conda activate torch
```

3.运行以下脚本来安装 TorchServe 及其依赖项。这个脚本在您的机器上安装 CPU 专用的 PyTorch、torchvision 和 torchtext 模块。如果你不需要的话，可以跳过这些。

```
conda install pytorch torchvision torchtext torchserve torch-model-archiver psutil future cpuonly -c pytorch -c powerai
```

您现在已经准备好开始使用 TorchServe，并通过 REST 端点使您的模型可用。

# 简单的例子

在这个例子中，我们部署了一个 [MNIST](http://yann.lecun.com/exdb/mnist/) 分类器。我们通过 TorchServe 文档中的代码示例来完成所需的步骤。

*   首先，我们需要定义一个 PyTorch 模型来解决 MNIST 挑战。下面的代码只是一种方法；将其复制并粘贴到一个名为`model.py`的文件中:

*   然后，我们需要创建一个训练模型的脚本。将以下代码复制到一个名为`main.py`的文件中。你可以运行`python main.py -h`来查看可用选项列表。训练新模型并保存(`python main.py --save-model True`)或在此下载可用的预训练模型[。不管怎样，把保存的模型移到一个名为`artefacts`的新文件夹中。](https://github.com/dpoulopoulos/medium/blob/master/torch_serve/artefacts/model.pt)

*   接下来，我们需要编写一个定制的处理程序来在您的模型上运行推理。将以下代码复制并粘贴到一个名为 handler.py 的新文件中。该代码对灰度图像进行推理，就像 MNIST 图像一样:

*   接下来，我们需要使用 torch-model-archiver 实用程序创建 torch 模型归档。该命令将提供的模型(`artefacts/model.pt`)归档到一个`.mar`文件中:

```
torch-model-archiver --model-name mnist --version 1.0 --model-file model.py --serialized-file artefacts/model.pt --handler handler.py
```

*   创建一个`model_store`文件夹，并将存档的模型移入其中:

```
mkdir model_store
mv mnist.mar model_store/
```

*   最后，启动服务器并向其查询答案。你可以在这里下载一些[的测试数据。要使用它们，只需将它们放在一个名为`test_data`的新文件夹中:](https://github.com/pytorch/serve/tree/master/examples/image_classifier/mnist/test_data)

```
torchserve --start --model-store model_store --models mnist=mnist.marcurl http://127.0.0.1:8080/predictions/mnist -T test_data/0.png
```

*   完成后，停止服务器:

```
torchserve --stop
```

# 结论

在这个故事中，我们介绍了 TorchServe，这是一个灵活易用的工具，用于服务 PyTorch 模型。我们看到了它是什么，它提供了什么，以及我们如何通过 REST 端点利用它的工具来服务 PyTorch 模型。最后，我们检查了它的最新版本(版本 0.1.1)和最新支持的特性，并用一个 MNIST 例子对它进行了测试。为了让您的手变脏，请阅读[文档](https://pytorch.org/serve/)和[官方示例](https://github.com/pytorch/serve/tree/master/examples)。

> [学习率](https://www.dimpo.me/newsletter)是我每周给那些对 AI 和 MLOps 世界好奇的人发的简讯。你会在每周五收到我关于最新人工智能新闻、研究、回购和书籍的更新和想法。在这里订阅！

***我叫 Dimitris Poulopoulos，是希腊比雷埃夫斯大学***[***BigDataStack***](https://bigdatastack.eu/)***和博士(c)的机器学习研究员。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲中央银行、经合组织和宜家等主要客户设计和实施人工智能和软件解决方案。如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和 DataOps 的帖子关注我上*** [***中***](https://medium.com/@dpoulopoulos) ***、***[***LinkedIn***](https://www.linkedin.com/in/dpoulopoulos/)***或***[***@ James 2pl***](https://twitter.com/james2pl)