# 引导前沿的自然语言处理模型

> 原文：<https://towardsdatascience.com/bootstrapping-cutting-edge-nlp-models-baf62405a2b5?source=collection_archive---------24----------------------->

## 如何在 5 分钟内启动并运行 XLNet 和 Pytorch

![](img/cf594bfcf81f22c4ae57e47d9a932414.png)

由 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的 [Pietro Jeng](https://unsplash.com/@pietrozj?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

# 什么是 XLNet

XLNet 是基于 Transformers (BERT、RoBERTa、TinyBERT 等)的现代 NLP 语言模型。)XLNet 在各种自然语言理解任务上的结果接近人类的表现。XLNet 可以生成高中生级别的文本，它可以回答简单的问题。它可以理解狗和猫不一样，但它们都是人类的宠物。
总的来说，XLNet 是一个建立在 BERT 进步基础上的模型。

XLNet 解决了 3 大类 NLP 问题:分类、序列标记和文本生成

## 分类:

分类任务是自然语言处理中最常见的任务类型。分类任务给一段文本分配一个类别。更广泛地说，他们回答一个问题*给定一段文本，告诉我该文本属于哪个类别*。
分类领域中的任务通常会回答如下问题:

> 这次就诊我们应该使用什么医疗账单代码？(提供访问描述)
> 这条短信是垃圾短信吗？(文字已提供)
> 这个用户感兴趣吗？(提供内容和用户简介)

## 序列标签:

NLP 中的另一类问题是序列标记。在序列标记中，我们试图在提供的文本中找到一些东西。通常，这种类型的任务将包括在所提供的文本中查找人(NER)或查找一个实体的所有共同引用，即，如果在句子*“玛丽跳过一只蟾蜍。* ***它*** *没动*算法会找出*的‘它’指的是玛丽，不是蛤蟆。序列标记的另一个例子是检测哪个股票行情自动收录器与一个公司的每次提及相关联—*

> *NVDA 定于 8 月 15 日报告 2020 财年第二季度业绩。*
> 
> *在随后的四个季度中，该公司的 **(NVDA)** 收益三次超过咤克斯共识预测，一次错过同样的**(咤克斯)**，平均正惊喜为 3.94%。*

## *文本生成:*

*XLNet 的第三种也是最后一种用途是文本生成。这里，给定一小段上下文，XLNet 将预测下一个单词。并且它将继续预测下一个单词，直到被指示停止。在下面的例子中，给定*的输入，快速棕色* XLNet 将首先预测*狐狸*，然后整体查看上下文并预测下一个单词*跳过*等等。*

> *快速棕色 <fox><jumped><over>…</over></jumped></fox>*

# *运行 XLNet*

*现在，让我们来看看如何设置和运行 XLNet。基于 XLNet 操作的 3 种模式—*

## *`XLNetLMHeadModel`*

*XLNet 模型，顶部有一个语言建模头(线性层，权重与输入嵌入绑定)——`XLNetLMHeadModel`。*

*Daulet Nurmanbetov 提供的示例代码*

*在这个例子中，你可以看到给定一个序列*“快速的棕色狐狸跳过了懒惰的* ***<面具>*******<面具>******成为一个*****

*****这种类型的 XLNet Head 通常用于 AI 完成人类句子或根据简短问题打出句子的演示。*****

## *****`XLNetForSequenceClassification`*****

*****顶部带有序列分类/回归头的 XLNet 模型(汇集输出顶部的线性层)，例如用于胶合任务— `XLNetForSequenceClassification`。*****

*****Daulet Nurmanbetov 提供的示例代码由 Hugginface 的 [Transformers 提供](https://github.com/huggingface/transformers)*****

*****如上所述，使用 XLNet 开箱即用而不先进行微调会给我们带来相当大的损失值 ***1.19********

****这种类型的 XLNet 头用于微调一个模型来为我们分类 NLP 任务。即。，我们可以使用上面的代码对文本或段落进行分类。为了做好分类工作，我们将需要标记的数据进行微调，并获得接近 **0** 的损失值。通常，根据我的经验，只需要 2k–5k 标记的样本就可以得到一个像样的模型。****

## ****`XLNetForQuestionAnswering`****

****XLNet 模型，顶部有一个 span 分类头，用于提取问题回答任务，如 SQuAD(隐藏状态输出顶部的线性层，用于计算“span 开始逻辑”和“span 结束逻辑”)。
****

****Daulet Nurmanbetov 提供的示例代码由 Hugginface 的 [Transformers 提供](https://github.com/huggingface/transformers)****

****这种类型的 XLNet Head 用于回答问题或从文本中提取一条信息。同样，与上一个 XLNet 方法一样，我们需要为我们的特定任务提供训练数据，这样才能很好地工作。****

# ****我们可以用这些模式做什么？****

****有了上面提供的 3 种 XLNet 模式，我们几乎可以解决任何类型的 NLP 问题。以下是我使用 XLNet 和其他 Transformer 模型解决的一些问题—
*—相关性检测
—共指消解*
*—NER*
*—序列分类*
*—文本生成*
*—下一个单词预测*
*—问题回答*
*—释义检测*
*—文本分类*****

****我还见过 XLNet 被用来执行图形数据库的 [**关系提取**](https://openreview.net/forum?id=rkgqm0VKwB) 和 [**汇总**](https://github.com/santhoshkolloju/Abstractive-Summarization-With-Transfer-Learning) 任务。****

# ****结论****

****在当今时代，任何人都可以在几个小时内开始使用前沿模型。这是激动人心的时刻，因为其中一些模型**在一些特定的任务中与人类能力**相匹敌。****

****重要的是，任何开发人员或喜欢代码的个人都可以使用这些模型，比如 XLNet 模型。****

****这都要归功于持续的努力，让人工智能对每个人都是免费和可访问的**。这些模型的纯 GPU 培训成本有时高达 50 万美元。使用运行在 Kubernetes-like 上的分布式计算和 GPU 集群来生产模型训练，需要花费更多的工程和研究时间。******

****公开共享模型权重以促进行业研究和应用的趋势确实令人惊讶。****