# 简化多标签分类的变压器。

> 原文：<https://towardsdatascience.com/transformers-for-multilabel-classification-71a1a0daf5e1?source=collection_archive---------6----------------------->

## 伯特、XLNet、罗伯塔等。对于多标签分类—逐步指南

作为一名数据科学家，我一直在学习文本分类的最新技术，我发现没有太多容易的例子来适应 transformers (BERT，XLNet 等。)进行**多标签分类**…所以我决定自己试试，这就是！

作为对其他多标签文本分类博客帖子的敬意，我将使用[有毒评论分类挑战](https://www.kaggle.com/c/jigsaw-toxic-comment-classification-challenge/data)数据集。

这篇文章附有一个互动的[谷歌 Colab 笔记本](https://colab.research.google.com/github/rap12391/transformers_multilabel_toxic/blob/master/toxic_multilabel.ipynb)，所以你可以自己尝试一下。您所要做的就是将 *train.csv、test.csv 和 test_labels.csv* 文件上传到实例中。让我们开始吧。

![](img/d1576b3d4cdd46ff3be271893b627f6d.png)

在本教程中，我将使用[拥抱脸的变形金刚库](https://huggingface.co/transformers/)以及 **PyTorch(带 GPU)** ，尽管这可以很容易地适应 TensorFlow — *如果这与多类分类教程*一起获得牵引力，我可能稍后会为此编写一个单独的教程。下面我将训练一个 BERT 模型，但是**我将向你展示在这个过程中为其他 transformer 模型改编这个代码是多么容易。**

# 导入库

# 加载和预处理训练数据

毒性数据集已经被清理并被分成训练集和测试集，因此我们可以加载训练集并直接使用它。

每个 transformer 模型需要不同的标记化编码，这意味着句子的标记化方式和注意力屏蔽的使用可能会因您使用的 transformer 模型而异。令人欣慰的是，HuggingFace 的变形金刚库使得为每个模型实现变得极其容易。在下面的代码中，我们加载了一个预训练的 BERT 记号赋予器，并使用“batch_encode_plus”方法来获取记号、记号类型和注意掩码。随意加载适合您想要用于预测的模型的记号赋予器。例如，

```
BERT:
tokenizer = BertTokenizer.from_pretrained('bert-base-uncased', do_lower_case=True) XLNet:
tokenizer = XLNetTokenizer.from_pretrained('xlnet-base-cased', do_lower_case=False) RoBERTa:
tokenizer = RobertaTokenizer.from_pretrained('roberta-base', do_lower_case=False)
```

接下来，我们将使用 10%的训练输入作为验证集，这样我们就可以在训练时监控分类器的性能。在这里，我们希望确保我们利用了“分层”参数，这样就不会有看不见的标签出现在验证集中。为了进行适当的分层，我们将所有只在数据集中出现一次的标签，并强制它们进入训练集。我们还需要创建 PyTorch 数据加载器来加载用于训练/预测的数据。

# 加载模型和设置参数

加载适当的模型可以如下所示，每个模型已经包含了一个单一的密集层用于分类。

```
BERT:
model = BertForSequenceClassification.from_pretrained("bert-base-uncased", num_labels=num_labels)XLNet:
model = XLNetForSequenceClassification.from_pretrained("xlnet-base-cased", num_labels=num_labels)RoBERTa:
model = RobertaForSequenceClassification.from_pretrained('roberta-base', num_labels=num_labels)
```

优化器参数可以通过几种方式进行配置。这里我们使用了一个定制的优化参数(我在这方面做得更成功)，但是，您可以只传递注释中所示的“model.parameters()”。

# 火车模型

使用**“分类交叉熵”**作为损失函数，将 HuggingFace 库配置为开箱即用的多类分类。因此，变压器模型的输出类似于:

```
outputs = model(batch_input_ids, token_type_ids=None, attention_mask=batch_input_mask, labels=batch_labels)**loss, logits = outputs[0], outputs[1]**
```

**但是，如果我们避免传入标签参数，模型将只输出逻辑值，我们可以使用这些逻辑值来计算多标签分类的损失。**

```
outputs = model(batch_input_ids, token_type_ids=None, attention_mask=batch_input_mask, labels=batch_labels)**logits = outputs[0]**
```

下面是这样做的代码片段。这里我们使用**“具有 Logits 的二元交叉熵”**作为我们的损失函数。我们可以很容易地使用标准的“二元交叉熵”、“汉明损失”等。

为了验证，我们将使用微 F1 精度来监控跨时代的训练表现。为此，我们必须利用模型输出中的逻辑值，将它们传递给 sigmoid 函数(给出[0，1]之间的输出)，并对它们进行阈值处理(0.50)以生成预测。然后，这些预测可用于计算相对于真实标签的准确度。

维奥拉。我们已经准备好训练，现在运行它…我的训练时间在 20-40 分钟之间，取决于最大令牌长度和使用的 GPU。

# 预测和指标

我们的测试集的预测类似于我们的验证集。在这里，我们将加载、预处理和预测测试数据。

# 输出数据帧

创建显示句子及其分类的输出数据框架。

# 奖励—优化微 F1 精度的阈值

迭代阈值以最大化微 F1 精度。

就是这样！有问题请评论。这里是 [Google Colab 笔记本](https://colab.research.google.com/github/rap12391/transformers_multilabel_toxic/blob/master/toxic_multilabel.ipynb)的链接，以防你错过。如果您有任何私人问题，请随时通过 LinkedIn 或 Twitter 与我联系。

# 参考资料:

[https://electricenjin . com/blog/look-out-Google-Bert-is-here-to-shake-up-search-queries](https://github.com/google-research/bert)

[https://github.com/google-research/bert](https://github.com/google-research/bert)

[https://github.com/huggingface/transformers](https://github.com/huggingface/transformers)

【https://pytorch.org/docs/stable/nn.html#bcewithlogitsloss 号

【https://arxiv.org/abs/1706.03762 