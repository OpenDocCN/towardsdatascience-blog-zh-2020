# 不同语言的文本分类

> 原文：<https://towardsdatascience.com/bert-text-classification-in-a-different-language-6af54930f9cb?source=collection_archive---------19----------------------->

## 用 HuggingFace 和简单的变形金刚构建非英语(德语)BERT 多类文本分类模型。

![](img/8bc8b3cb605e1f2da324fefb1ac87848.png)

*原载于 2020 年 5 月 22 日*[*https://www . philschmid . de*](https://www.philschmid.de/bert-text-classification-in-a-different-language)*。*

# 介绍

目前，我们有 75 亿人生活在世界上大约 200 个国家。其中只有 12 亿人以英语为母语。这导致了大量非结构化的非英语文本数据。

大多数教程和博客帖子都用英语演示了如何使用基于 BERT 的架构来构建文本分类、情感分析、问答或文本生成模型。为了克服这种缺失，我将向大家展示如何建立一个非英语的多类文本分类模型。

![](img/aed8767fcc646f8ef56a8312b0d3d724.png)

英语母语者的世界

打开我的文章，让我来猜猜你是否听说过伯特。如果你还没有，或者你想更新一下，我推荐你阅读这篇[文章](https://arxiv.org/pdf/1810.04805.pdf)。

在深度学习中，对于如何建立语言模型，目前有两种选择。您可以构建单语模型或多语言模型。

> *“使用多种语言，还是不使用多种语言，这是个问题”——就像莎士比亚会说的那样*

多语言模型描述了可以理解不同语言的机器学习模型。来自 Google research 的 [mBERT](https://storage.googleapis.com/bert_models/2018_11_23/multi_cased_L-12_H-768_A-12.zip) 就是一个多语言模型的例子。[该型号支持并理解 104 种语言。](https://github.com/google-research/bert/blob/master/multilingual.md)单语模特，顾名思义能听懂一种语言。

多语言模型已经在某些任务上取得了良好的效果。但是这些模型更大，需要更多的数据，也需要更多的时间来训练。由于需要大量的数据和时间资源，这些特性导致了更高的成本。

由于这个事实，我将向你展示如何训练一个单语的非英语的基于 BERT 的多类文本分类模型。哇，那是一个长句子！

![](img/a20d5e8bc73784e7df33544cb7151b93.png)

伯特——得到了迷因

# 辅导的

我们将使用[简单变形金刚](https://github.com/ThilinaRajapakse/simpletransformers)——一个基于 HuggingFace 的[变形金刚](https://github.com/huggingface/transformers)库的 NLP 库。Simple Transformers 允许我们用几行代码来微调 Transformer 模型。

作为数据集，我们将使用由德语推文组成的 [Germeval 2019](https://projects.fzai.h-da.de/iggsa/projekt/) 。我们将检测和分类辱骂性语言的推文。这些推文分为四类:`PROFANITY`、`INSULT`、`ABUSE`和`OTHERS`。在这个数据集上获得的最高分是`0.7361`。

## 我们将:

*   安装简单的变压器库
*   选择预先训练好的单语模型
*   加载数据集
*   训练/微调我们的模型
*   评估培训的结果
*   保存训练好的模型
*   加载模型并预测一个真实的例子

在本教程中，我使用了带有 GPU 运行时的 Google Colab。如果你不确定如何使用 GPU 运行时，看看这里的。

# 安装简单的变压器库

首先，我们用 pip 安装`simpletransformers`。如果你没有使用 Google colab，你可以点击查看安装指南[。](https://github.com/ThilinaRajapakse/simpletransformers)

# 选择预先训练好的单语模型

接下来，我们选择预训练模型。如上所述，简单变形金刚库基于 HuggingFace 的变形金刚库。这使我们能够使用[变形金刚库](https://huggingface.co/transformers/pretrained_models.html)中提供的每个预训练模型和所有社区上传的模型。对于包括所有社区上传模型的列表，我指的是[https://huggingface.co/models](https://huggingface.co/models)。

我们将使用`distilbert-base-german-cased`型号，一种[更小、更快、更便宜的 BERT](https://huggingface.co/transformers/model_doc/distilbert.html) 版本。它使用的参数比`bert-base-uncased`少 40%,运行速度快 60%,同时仍然保留了 95%以上的 Bert 性能。

# 加载数据集

数据集存储在两个文本文件中，我们可以从[竞赛页面](https://projects.fzai.h-da.de/iggsa/)中检索。下载它们的一个选择是使用两个简单的`wget` CLI 命令。

之后，我们使用一些`pandas`魔法来创建一个数据帧。

因为我们没有测试数据集，所以我们分割数据集— `train_df`和`test_df`。我们将 90%的数据用于训练(`train_df`)，10%用于测试(`test_df`)。

# 加载预训练模型

下一步是加载预先训练好的模型。我们通过创建一个名为`model`的`ClassificationModel`实例来做到这一点。此实例采用以下参数:

*   架构(在我们的案例中是`"bert"`)
*   预训练模型(`"distilbert-base-german-cased"`)
*   类别标签的数量(`4`)
*   还有我们训练用的超参数(`train_args`)。

您可以在广泛的可能性范围内配置超参数。有关每个属性的详细描述，请参考[文档](https://simpletransformers.ai/docs/usage/#configuring-a-simple-transformers-model)。

# 训练/微调我们的模型

为了训练我们的模型，我们只需要运行`model.train_model()`并指定要训练的数据集。

# 评估培训的结果

在我们成功地训练了我们的模型之后，我们可以对它进行评估。因此，我们创建一个简单的辅助函数`f1_multiclass()`，用于计算`f1_score`。`f1_score`是对模型精度的测量。更多关于那个[这里](https://en.wikipedia.org/wiki/F1_score)。

我们取得了`0.6895`的`f1_score`。最初，这似乎相当低，但请记住:在 [Germeval 2019](https://projects.fzai.h-da.de/iggsa/submissions/) 的最高提交量是`0.7361`。如果不调整超参数，我们将获得前 20 名的排名。这是相当令人印象深刻的！

在以后的文章中，我将向您展示如何通过调优超参数来获得更高的`f1_score`。

# 保存训练好的模型

Simple Transformers 会在每一步`2000`和训练过程结束时自动保存`model`。默认目录是`outputs/`。但是`output_dir`是一个超参数，可以被覆盖。我创建了一个助手函数`pack_model()`，我们用它将所有需要的模型文件`pack`到一个`tar.gz`文件中进行部署。

# 加载模型并预测一个真实的例子

最后一步，我们加载并预测一个真实的例子。因为我们用`pack_model()`提前一步打包了文件，所以我们必须先用`unpack`打包它们。因此，我编写了另一个助手函数`unpack_model()`来解包我们的模型文件。

为了加载一个保存的模型，我们只需要为我们保存的文件提供`path`,并像我们在训练步骤中那样初始化它。*注意:在加载模型时，您需要指定正确的(通常与训练中使用的相同)参数。*

初始化之后，我们可以使用`model.predict()`函数对给定输入的输出进行分类。在这个例子中，我们从 Germeval 2018 数据集中提取了两条推文。

我们的模型预测了正确的类别`OTHER`和`INSULT`。

# 简历

总之，我们可以说我们实现了创建非英语的基于 BERT 的文本分类模型的目标。

我们的例子提到了德语，但可以很容易地转换成另一种语言。HuggingFace 为法语、西班牙语、意大利语、俄语、汉语、…

感谢阅读。你可以在这里找到带有完整代码[的 colab 笔记本。](https://colab.research.google.com/drive/1kAlGGGsZaFaFoL0lZ0HK4xUR6QS8gipn#scrollTo=JG2gN7KUqyjY)

如果你有任何问题，随时联系我。