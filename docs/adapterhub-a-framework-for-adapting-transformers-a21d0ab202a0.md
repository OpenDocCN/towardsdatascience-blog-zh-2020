# AdapterHub:一个适配变压器的框架

> 原文：<https://towardsdatascience.com/adapterhub-a-framework-for-adapting-transformers-a21d0ab202a0?source=collection_archive---------22----------------------->

## 没有更慢的微调:有效的迁移学习与拥抱脸变压器在 2 额外的代码行

![](img/b91e66e48f9d8af1d8673e599673dc1a.png)

韦斯利·廷吉在 [Unsplash](https://unsplash.com/s/photos/adapter?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

这篇博文是对 [**AdapterHub**](https://adapterhub.ml/) 的介绍，这是一个由 [Pfeiffer et al (2020b)](https://arxiv.org/pdf/2007.07779v1.pdf) 发布的新框架，它使你能够对**广义预训练的变形金刚如 BERT、RoBERTa 和 XLM-R** 进行**迁移学习**到**下游任务**如问答、分类等。**使用适配器代替微调**。

AdapterHub 建立在 HuggingFace 提供的流行的 transformer 包之上。这里可以找到拥抱脸变形金刚包[，这里](https://github.com/huggingface/transformers)可以找到 AdapterHub 的修改[。](https://adapterhub.ml/)

为了正确理解这篇文章，我建议你先阅读一下[变压器以及它们通常是如何微调的](https://www.analyticsvidhya.com/blog/2019/06/understanding-transformers-nlp-state-of-the-art-models/)！

## 为什么要用适配器而不是微调？

我将在“适配器的好处”一节中详细介绍这一点，但先睹为快:

[Houlsby 等人(2019)](http://proceedings.mlr.press/v97/houlsby19a/houlsby19a.pdf) 介绍了一种叫做适配器的东西。适配器**的作用与微调**相同，但是通过**将层**缝合到主预训练模型，并且**更新这些新层**的权重φ，同时**冻结预训练模型**的权重θ。

相比之下，您会记得在微调中，我们还需要更新预训练的权重。

正如你可能想象的那样，与微调相比，这使得适配器在时间和存储方面的效率更高。适配器也被证明能够**匹配最先进的微调方法**的性能！

## 你将从这篇文章中学到什么

我将简化伴随框架发布的 [AdapterHub paper](https://arxiv.org/pdf/2007.07779v1.pdf) 的内容，让你更容易开始开发。

AdapterHub 框架很重要，因为在这个框架之前，拼接已经训练过的**适配器**或**共享适配器**很困难，并且需要手动修改转换器架构。该框架支持

> 针对不同任务和语言的预培训适配器的动态“拼接”

简而言之，它使得使用适配器进行迁移学习变得更加容易。

在本帖中，我们将回顾在 [AdapterHub 白皮书](https://arxiv.org/abs/2007.07779)中讨论的**适配器**的各种优势，并解释新适配器 Hub 框架的**各种特性。**

这些特性将伴随着论文中的一些**示例代码**来帮助您入门！

如果你已经熟悉适配器及其各种好处，你可以直接跳到章节**‘adapter hub 的主要特性’**

## 适配器的优势

此处列出的优势与[适配器 Hub 文件](https://arxiv.org/pdf/2007.07779v1.pdf)第 2.2 节中列出的优势相对应。

***特定任务的分层表征学习***

正如前面提到的，在 GLUE benchmark(在自然语言处理界很流行)上比较微调和适配器的性能时，性能没有很大的差异。

这意味着适配器可以**达到与微调**相当的最先进水平，同时**保持被列为下一个功能的时间和空间效率**！

***小型、可扩展、可共享***

为了完全微调模型，我们需要为每个任务“存储”一个模型的副本。这也“阻碍了训练的项目化和平行化。”

相比之下，适配器需要更少的存储空间。为了说明这一点，Pfeiffer 等人(2020b)提供了以下例子:

> 对于大小为 440Mb 的流行 Bert-Base 模型，当使用 48 的瓶颈大小和 Pfeiffer 等人的适配器时，存储 2 个完全微调的模型相当于 125 个带适配器的模型所需的相同存储空间(2020a)

这样做的另一个好处是，我们可以通过简单地添加小型适配器而不是大量的微调来为应用程序添加更多的任务。

**研究人员之间的再现性**是存储需求降低的另一个美妙结果。

***模块化表征***

当我们缝入适配器时，我们固定转换器其余部分的表示，这意味着这些适配器是*封装的*，并且可以与其他适配器堆叠、移动或组合。

这种模块化允许我们组合来自不同任务的适配器——随着 NLP 任务变得越来越复杂，这是非常重要的。

***无干扰合成信息*** 。

自然语言处理通常涉及跨任务共享信息。我们经常使用一种叫做多任务学习(MTL)的方法，但是 MTL 有两个问题:

*   灾难性遗忘:在训练的早期阶段学习的信息被“覆盖”。
*   灾难性推理:当“增加新任务时，一组任务的性能恶化”(Pfeiffer 等人，2020b)。

对于适配器，我们为每个任务分别训练适配器，这意味着我们克服了上述两个问题。

## **适配器 Hub 的主要特性**

很好，现在让我们来看看这个框架的关键特性！

***变压器层中的适配器+如何训练适配器***

为了添加适配器，作者使用了由 HuggingFace transformer 继承的称为“Mix-in”的东西，以便保持代码库合理分离。

实际上，下面是添加适配器层的方法:

```
from adapter_transformers import AutoModelForSequenceClassification, AdapterTypemodel = AutoModelForSequenceClassification.from_pretrained("roberta-base")model.add_adapter("sst-2", AdapterType.text_task, config="pfeiffer") model.train_adapters(["sst-2"]) # Train model ... 
model.save_adapter("adapters/text-task/sst-2/", "sst")
# Push link to zip file to AdapterHub ...
```

您会注意到，代码主要对应于常规的 HuggingFace transformers，我们只添加了两行来添加和训练适配器。

这个 AdapterHub 框架的特别之处在于，您可以动态配置适配器，并更改架构。虽然您可以直接使用文献中的适配器，例如 Pfeiffer 等人(2020a)或 Houlsby 等人(2020)，但是您也可以使用**配置文件**轻松修改这些架构。在上面的代码中，我们使用默认的 Pfeiffer (2020a)配置。

***抽取和开源适配器***

您可以将您训练的适配器推送到 [AdapterHub.ml](https://adapterhub.ml/) ，并从其他人预先训练的适配器中受益。与必须共享整个大型模型的微调不同，这些适配器是轻量级的，易于共享！

***寻找预先训练好的适配器***

[AdapterHub.ml](https://adapterhub.ml/) 的搜索功能分级工作:

*   第一级:按任务/语言查看
*   第二级:分离成更高级别的 NLP 任务的数据集//分离成训练数据的语言(如果我们正在适应一种新的语言)
*   第三层:分成单独的数据集或域(如维基百科)

该网站还可以根据您指定的预先训练的转换器，帮助您识别兼容的适配器。

***在预训练的适配器中拼接***

使用以下代码来连接预训练的适配器，而不是自己训练(如前所述):

```
from adapter_transformers import AutoModelForSequenceClassification, AdapterType model = AutoModelForSequenceClassification.from_pretrained("roberta-base")model.load_adapter("sst", config="pfeiffer")
```

非常清楚地说，预训练的*适配器*加载在第三行代码中，而预训练的*转换器*加载在第二行代码中。

***用适配器推理***

就用常规的 HuggingFace 代码进行推理吧！加载适配器砝码时，您可以**选择加载预测头**。

回想一下，使用这个令人敬畏的框架，为组合任务等组合适配器是非常可能的！

## 结论

我鼓励你在这里亲自尝试 AdapterHub 框架。

这个框架是如此令人兴奋，我希望这篇文章能帮助你开始适应变形金刚的旅程！

如果你在帖子中发现任何错误，或者你有任何评论/批评，请告诉我！

## 参考

Pfeiffer，j .，Kamath，a .，Rücklé，a .，Cho，k .，Gurevych，I. (2020a)。AdapterFusion:迁移学习的非破坏性任务合成。arXiv 预印本。

法官 Pfeiffer、法官 Rücklé、法官 Poth、法官 Kamath、法官 Vuli、法官 Ruder、法官 Cho、法官 Gurevych、法官 I(2020 b)。AdapterHub:一个适配变压器的框架。arXiv 预印本。

Houlsby，a . Giurgiu，a .，Jastrzkebski，s .，Morrone，b .，de Laroussilhe，q .，Gesmundo，a .，Attariyan，m .，和 Gelly，S. (2019)。自然语言处理中的参数有效迁移学习。2019 年 ICML 会议录。