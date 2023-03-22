# 数据科学家匆忙进行变压器快速实验

> 原文：<https://towardsdatascience.com/fast-experimentation-with-transformers-for-data-scientists-in-a-rush-da0f5240dce3?source=collection_archive---------34----------------------->

## 用 huggingface/transformers repo 创建低代码、有用的实验管道比你想象的要容易

![](img/0968728c8a2c9f1f39192084eebad479.png)

杰克·吉文斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 用 NLP 快速实验让你产生很多价值

如果你正在读这篇文章，你可能在某个时候遇到了和我一样的问题:你有一个 NLP 数据集和任务要解决，需要执行快速实验来产生价值，并把一个模型投入生产。

你打开了拥抱脸/变形金刚，看到了很多例子——它们都起作用了。当它适应你自己的数据集和任务时，代码太长，太具体，不能用于你的目的。

这篇文章将帮助你利用 HuggingFace 预训练转换器和库，为你自己的 NLP 任务和数据集创建低代码、有用且易于适应的管道。今天我们将集中讨论分类，但是，如果你们都喜欢这个帖子，我可以继续这个系列的任务，如问答、翻译等…

所以让我们把手放上去！

# 开始之前

在你开始之前，确保你有所有的要求，在[我们的回购](https://github.com/piEsposito/transformers-low-code-experiments)上声明。我们将使用 Kaggle 的数据集[，其中包含被标记为垃圾邮件或非垃圾邮件的电子邮件。为了对它进行预处理，我将 csv 文件作为 Pandas dataframe 导入，重命名列并将它分成两个 csv 文件，用于训练和测试目的。](https://www.kaggle.com/team-ai/spam-text-message-classification)

我已经为您预处理过了，所以您可以在 [repo](https://github.com/piEsposito/transformers-low-code-experiments/tree/main/classification) 上访问它。无论如何，如果你愿意，这里是我对数据集做的操作:

为了运行我们将在这里构建的脚本，请确保将 text 列命名为“sentence ”,将 labels 列命名为“label”。

我们现在准备出发了。

# 实施路线图

对于本文，我们的实施路线图将是:

*   导入我们将在这里使用的每个库，并解释我们为什么要使用它；
*   在 hugging face/transformers auto model 类的顶部创建一个多标签序列分类器；
*   创建一个辅助函数来标记文本数据集；
*   从已训练的模型中创建辅助函数来训练、预测和度量；
*   创建一个 optuna 目标函数来帮助自动超参数调整；
*   将其包装成一个可参数化、可重用的脚本，以便在不同的 NLP 任务上进行快速实验。

## 进口

以下是我们今天使用的进口产品:

现在，我们如何使用它们:

*   所有 PyTorch 导入都与创建数据加载器、操纵张量、创建可训练模型和损失函数有关。
*   我们从 Transformers 导入 AutoModel、优化器、标记器和配置，以便能够从它们的 repo 加载任何预先训练的语言模型。这意味着我们可以使用不同的语言和数据集，只要文件符合我们之前所做的预处理。
*   我们导入 nlp，HuggingFace 的另一个包来创建数据集。csv 文件。
*   Optuna 用于自动优化超参数，帮助我们实现更好的模型指标。
*   Sklearn.metrics 和 Numpy 是用来度量计算的，tqdm 只是用来把东西变漂亮的。

## 扩展用于多类分类的转换器

HuggingFace Transformers 内置了 AutoModelForSequenceClassification(FYI，这是实际名称)，但它只支持二进制分类。因为我们想把它扩展到更多的类，我们必须自己创建这个模型。

那不会很难。正如你在下面看到的，我们将创建一个`nn.Module`对象，它创建一个自动模型基础，然后是一个 Dropout 和线性层，带有所需数量的输出节点。请注意，我们仅使用预训练模型的名称和标签数量对其进行参数化:

## 创建令牌化函数

使用训练模型的相同键(名称)，我们可以从 HuggingFace repo 导入一个标记化器。我们现在将创建一个函数，根据给定的标记器对数据集进行标记。稍后，我们将使用 map 方法将这种标记化应用于整个数据集。

就这么简单:

## 培训、评估和度量功能

我们现在进入了本帖最重要的部分:训练和评估功能。

为此，给定数据集，我们使用一个非常简单的向前训练函数，只对 huggingface/nlp 数据集对象做了一些修改:

请注意，我们在每个训练时段创建数据加载器，它是一个生成器，并且我们为每个批次获得这个`input_ids`。这就是数据集的符号化，必须将其转换为 LongTensor，然后放入我们正在进行训练的设备中。

我们将评估该模型，并分两步获取指标。首先，我们将整个测试数据集通过模型并获得其预测，将一个张量和相应的张量保存在一边:

在我们得到标签和预测张量进行比较之后，我们就可以得到度量。在这个函数中，我决定让用户决定要计算的指标。请注意，在后面的内容中，我们将设置 optuna 来调整这个特定指标的超参数:

我们决定支持所有标签评估的准确性和平均精度，并支持二进制化+ f1，以及要调整的特定标签的召回和精度指标。

当我们将所有这些构建部分放在一起时，我们可以创建 optuna 目标函数。为了让您理解，这是一个使用一些超参数执行训练并获得客观指标的函数。Optuna 将使用此目标函数，根据我们想要的度量找到最佳超参数:

由于我们将使用上面设置的所有函数，我们将根据数据集的特性使其可参数化。这保证了你能够用你自己的数据集来测试我们正在构建的脚本。

## 参数化脚本

在创建我们的主脚本之前，我们将使用`argparse`为它的运行设置参数。这意味着通过命令行上的一些标志，你可以用你自己的文本分类数据集进行实验。让我们看看:

所以这里我们参数化为:

*   模型名称，因此我们可以从 HuggingFace 预训练模型库中获取任何模型；
*   训练和测试数据路径，只是为了决定文件的名称和路径；
*   最大序列长度，所以我们不会浪费内存，如果工作与小文本大小；
*   指标名称 optuna 将优化，支持准确性、average_precision_score、f1_score、precision_score、recall_score(最后三个上的标签相对于 reference-class 二进制化)；和
*   标签号，这样我们就可以设置模型的输出节点数。

## 最后，主要功能

设置好之后，我们就可以编写主函数了。它创建数据集和目标函数，从 optuna 设置一个研究来优化超参数，以最大化所选的度量。

使用默认参数，如果你喜欢使用它，你可以适应谷歌 Collab 的 GPU，它应该产生大约 0.9 的 F1 分数。这是:

注意，我们使用 HuggingFace 的 nlp 库创建数据集，然后将编码数据集函数映射到整个数据集。这就创建了`input_ids`属性，我们用它在训练和推理中获得标记化的文本。

## 结论

正如你所看到的，利用 HuggingFace 预训练的 transformers 库和库，创建一个可重复的、可参数化的脚本是非常可能的。有了正确形状上的数据，您可以用简单、易懂和易读的代码，以非常快速的方式运行实验并调整超参数和指标。

我希望 post 和 script 能帮助你快速地用 NLP 创造价值，产生良好可靠的结果，并在许多语言中使用最先进的预训练模型。

如果这篇文章对你有所帮助，如果你能[在 Github](https://github.com/piEsposito/transformers-low-code-experiments) 上发起回购，我会非常高兴，如果能让更多人看到并帮助消息传递给更多人。

如果你有任何评论、批评或者只是想谈谈 NLP、深度学习或者甚至和我一起写这个系列，[在 LinkedIn](https://www.linkedin.com/in/piesposito/) 上联系我，以便我们可以交谈。

谢谢你的时间，我希望我已经帮助了你。

# 参考

[](https://github.com/piEsposito/transformers-low-code-experiments) [## piEsposito/变压器-低代码-实验

### 低代码预建管道，用于数据科学家匆忙进行的 huggingface/transformers 实验。这个…

github.com](https://github.com/piEsposito/transformers-low-code-experiments) [](https://www.linkedin.com/posts/thomas-wolf-a056857_nlp-ai-opensource-activity-6702500587939868672-YLmC/) [## LinkedIn 上的 Thomas Wolf:# NLP # ai # open source | 49 条评论

### 我们正在考虑添加非常明确和简单的例子🤗像这样的变形金刚。只有 45 行……

www.linkedin.com](https://www.linkedin.com/posts/thomas-wolf-a056857_nlp-ai-opensource-activity-6702500587939868672-YLmC/) [](https://github.com/huggingface/transformers) [## 拥抱脸/变形金刚

### PyTorch 和 TensorFlow 2.0 的最新自然语言处理技术🤗变形金刚提供了成千上万的…

github.com](https://github.com/huggingface/transformers) [](https://www.kaggle.com/team-ai/spam-text-message-classification) [## 垃圾短信分类

### 让我们用数据科学与恼人的垃圾邮件制造者战斗。

www.kaggle.com](https://www.kaggle.com/team-ai/spam-text-message-classification) [](https://optuna.org/) [## Optuna -超参数优化框架

### Optuna 是一个自动超参数优化软件框架，专门为机器学习而设计。它…

optuna.org](https://optuna.org/)