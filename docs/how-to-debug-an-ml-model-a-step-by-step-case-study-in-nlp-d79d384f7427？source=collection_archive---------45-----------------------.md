# 如何调试 ML 模型:NLP 中的分步案例研究

> 原文：<https://towardsdatascience.com/how-to-debug-an-ml-model-a-step-by-step-case-study-in-nlp-d79d384f7427?source=collection_archive---------45----------------------->

## 虽然有这么多文章介绍如何开始学习 NLP 或向您提供指导，但最难学习的课程之一是如何调试模型或任务实现。

![](img/7d4e61ec4546feb77023ed5095946706.png)

模型/任务实施前的精神状态，通过 Pixabay (CC0)的 [StockSnap](https://pixabay.com/users/StockSnap-894430/)

![](img/c5707cfef9a6aa30a3b7dd7e660d415f.png)

模型/任务实施后的精神状态，[Free-Photo](https://pixabay.com/users/Free-Photos-242387/)via pix abay(CC0)

不要担心！本文将介绍一系列相当微妙(和不那么微妙)的错误的调试过程，以及我们如何修复它们，并通过一个案例研究来引导您完成这些课程。**如果你想看提示列表，向下滚动到最后！**

为了做到这一点，让我带你回到几个月前，那时我们(我的研究合作者 Phu 和我)第一次将[屏蔽语言建模](/bert-explained-state-of-the-art-language-model-for-nlp-f8b21a9b6270)实现到 [jiant](http://jiant.info) 中，这是一个开源的 NLP 框架，目标是在 RoBERTa 模型上进行多任务训练。如果这听起来像是一种陌生的语言，我建议你先看看这篇关于[迁移学习和多任务学习的文章，](/a-comprehensive-hands-on-guide-to-transfer-learning-with-real-world-applications-in-deep-learning-212bf3b2f27a)和这篇关于[罗伯塔模型的文章。](https://medium.com/towards-artificial-intelligence/a-robustly-optimized-bert-pretraining-approach-f6b6e537e6a6)

## **设置场景**

屏蔽语言建模是 BERT、RoBERTa 和许多 BERT 风格变体的预训练目标之一。它由一个输入噪声目标组成，其中给定一个文本，该模型必须预测给定上下文的 15%的标记。更困难的是，这些预测的标记有 80%的时间被替换为“[MASK]”，10%被另一个随机标记替换，10%是正确的、未被替换的标记。

例如，该模型如下所示

![](img/491ab0eecfa9e6e2c1677a239c40b4ba.png)

为 MLM 培训更改的文本示例。这里，模型将学习预测当前被“[MASK]”占据的令牌的“tail”

## 设计初始实施

我们首先查看了其他人以前是否实现过 MLM，发现了 Google 的最初实现和 AllenNLP 的 Pytorch 实现。我们几乎使用了所有的 [Huggingface 实现](https://github.com/huggingface/transformers/blob/master/examples/run_language_modeling.py) n(它已经被移动了，因为它看起来像是以前在那里的文件已经不存在了)来实现转发功能。根据 RoBERTa 的论文，我们在每个时间步动态地屏蔽了批次。此外，Huggingface 在这里暴露了预训练的 MLM 头[，我们使用如下。](https://huggingface.co/transformers/model_doc/bert.html#bertformaskedlm)

因此，我们代码中的 MLM 流变成了下面这样:

> 加载 MLM 数据->预处理和索引数据->加载模型->在模型训练的每个步骤中，我们:
> 
> 1.动态屏蔽批处理
> 
> 2.计算每个屏蔽令牌的 NLL 损耗

jiant 框架主要使用 AllenNLP 进行词汇创建和索引，以及实例和数据集管理。

我们首先用一个包含 100 个数据集示例的玩具数据集进行测试，以确保 AllenNLP 的加载是正确的。在我们经历了一些非常明显的错误之后，比如一些标签类型与 AllenNLP 不匹配，我们遇到了一个更大的错误。

## 麻烦的最初迹象

在确保我们的预处理代码与 AllenNLP 一起工作后，我们发现了一个奇怪的 bug。

```
TypeError: ~ (operator.invert) is only implemented on byte tensors. Traceback (most recent call last):
```

这是因为我们从 Huggingface 复制粘贴的代码是用旧版本的 Python 编写的，并且是在您需要使用的 Pytorch 中。byte()而不是 bool()。

因此，我们简单地修改了一行，从

`indices_replaced = torch.bernoulli(torch.full(labels.shape, 0.8)).bool() & masked_indices`

到

```
bernoulli_mask = torch.bernoulli(torch.full(labels.shape, 0.8)).to( device=inputs.device, dtype=torch.uint8 )
```

## 麻烦来了

现在，我们终于能够运行一个正向函数而不出错了！庆祝了几分钟后，我们开始验证更微妙的错误。我们首先通过调用 model.eval()并通过 MLM 转发函数运行模型来测试我们实现的正确性。因为这个模型，在这个例子中是 RoBERTa-large，已经在 MLM 进行了预训练，我们希望它在 MLM 会有很好的表现。事实并非如此，我们损失惨重。

原因很明显:预测总是与黄金标签相差 2%。例如，如果标记“tail”被分配了索引 25，那么“狗看到零食时摆动它的[面具]”和“[面具]”的标签将是 25，但是预测将是 27。

我们是打了这个错误才发现的。

```
`/pytorch/aten/src/THCUNN/ClassNLLCriterion.cu:105: void cunn_ClassNLLCriterion_updateOutput_kernel(Dtype *, Dtype *, Dtype *, long *, Dtype *, int, int, int, int, long) [with Dtype = float, Acctype = float]: block: [0,0,0], thread: [19,0,0] Assertion `t >= 0 && t < n_classes` failed.`
```

这个错误意味着预测空间大于类的数量。

经过大量的 pdb 跟踪，我们意识到我们没有使用 AllenNLP 标记/标签名称空间。在 AllenNLP 中，您可以使用名称空间(如标签名称空间和输入名称空间)跟踪 AllenNLP 词汇对象中需要的所有词汇。我们发现 AllenNLP 词汇对象 a [自动插入](https://github.com/allenai/allennlp/blob/master/allennlp/data/vocabulary.py#L97) @@PADDING@@和@ @ UNKOWN @ @标记来索引除标签名称空间(所有以“_tag”或“_labels”结尾的名称空间)之外的所有名称空间的 0 和 1 因为我们没有使用标签名称空间，所以我们的索引向前移动了两个，预测空间(由标签词汇大小定义)大了两个！发现这一点后，我们重新命名了标签索引，这种特殊的威胁得到了遏制。

## 最后一个隐藏的 Bug 和一个支点

到目前为止，我们认为我们已经捕获了所有或者大部分的错误，并且 MLM 工作正常。然而，当模型现在变得越来越不困惑时，一周后，当与第三个人一起查看代码时，我们发现了另一个隐藏的 bug。

```
if self._unk_id is not None: 
     ids = (ids — 2) * valid_mask + self._pad_id * pad_mask + self._unk_id * unk_mask 
else: 
     ids = (ids — 2) * valid_mask + self._pad_id * pad_mask
```

在我们几个月前编写的代码的某个单独部分，我们将任何基于 Transformer 的模型的输入向后移动了 2，因为 AllenNLP 将其向前移动了 2。因此，从本质上来说，模型看到的是胡言乱语，因为它看到的是距离正确输入索引两个索引的词汇标记。

我们是如何解决这个问题的？

我们结束了对以前的 bug 的修复，并且没有使用 label 名称空间作为输入，因为所有的东西都向后移动了 2。并且简单地确保动态生成的 mask_idx 在被馈送到模型中之前向前移位 2。为了修复之前预测和标签空间大小不匹配的错误，我们将标签的数量设为预训练模型的标记器的大小，因为这包括该模型预训练的所有词汇。

经过无数个小时的调试和初步实验，我们终于摆脱了 bug。*唷！*

因此，作为对我们所做的事情的回顾，以确保代码没有错误，也是为了回顾我们所看到的错误类型，这里有一个漂亮的列表。

## 调试模型的关键要点

1.  从玩具数据集开始测试。对于本案例研究，预处理整个数据集需要大约 4 个小时。
2.  如果可能，使用已经创建的基础设施。
3.  请注意，如果您正在使用其他人的代码，请确保您确切地知道它如何适合您的代码，以及在将外部代码集成到您自己的代码中时可能出现的任何不兼容性，无论是微妙的还是不明显的。
4.  如果您正在使用预训练的模型，并且如果有意义，请尝试加载一个训练过的模型，并确保它在任务中表现良好。
5.  注意代码之间 Pytorch 版本(以及其他依赖版本)的差异。
6.  索引时要非常小心。有时勾画出索引的流程是非常混乱的，会导致很多令人头疼的问题，为什么你的模型执行得不好。
7.  让其他人也看看你的代码。
8.  为了理解 bug 的来源，您可能需要更深入地了解这些杂草，并查看用于预处理的包的源代码(如 AllenNLP)。
9.  创建单元测试来跟踪细微的错误，并确保您不会因为代码更改而依赖这些错误。

现在你有了它，一个调试案例研究和一些教训。我们都在一起进行更好的调试，我知道我远不是模型调试的专家，但是希望这篇文章对你有帮助！特别感谢 Phu Mon Htut 编辑这篇文章。

如果你想看最终的实现，点击这里查看！