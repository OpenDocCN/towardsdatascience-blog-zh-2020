# 实体解析实用指南—第 3 部分

> 原文：<https://towardsdatascience.com/practical-guide-to-entity-resolution-part-3-1b2c262f50a7?source=collection_archive---------12----------------------->

## *特征化和阻塞密钥生成*

![](img/1173e2a9dbae68588f6531060777a69a.png)

照片由 [Honey Yanibel Minaya Cruz](https://unsplash.com/@honeyyanibel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

*这是关于实体解析的小型系列的第 3 部分。查看* [*第一部分*](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-1-f7893402ea7e) *，* [*第二部分*](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-2-ab6e42572405) *如果你错过了*

**什么是特征化和阻塞，为什么它很重要？**

在 ER 的上下文中，特征化意味着将现有的列转换为派生的特征，这些派生的特征可以通知不同的记录是否引用同一事物。分块意味着选择一个目标特征子集作为自连接键，以有效地创建潜在的匹配候选对。在我们的讨论中将这两个步骤分组的原因是块密钥通常是从特征中派生出来的。

良好的特征化能够实现高效的阻塞以及下游良好的匹配精度，因此它是 ER 过程的关键部分。这也是定义最少的一步，在这个过程中可以注入大量的创造力。

良好的分块可以极大地提高 ER 过程的效率，并且可以扩展到许多输入源和大型数据集。正如我们在 [*第一部分*](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-1-f7893402ea7e) 中所讨论的，潜在候选对的论域是 N 与记录数。然而，并不是所有的候选对都值得评估。比如`learning quickbooks 2007`和`superstart fun with reading writing`显然不是一回事，不应该包含在候选对中。解决这个问题的一种方法是使用更具体的阻塞键，例如产品的确切字符串名称，来创建候选对。这样，只有具有完全相同的标准化名称字符串的产品才会包含在候选对中。然而，这显然限制太多，会错过像`learning quickbooks 2007`和`learning qb 2007`这样的潜在匹配。好的阻塞键选择对于有效地平衡好的效率和低的假阴性率之间的折衷是至关重要的。

**如何实现特征化和模块化？**

特征化，也许比 ER 中前面的步骤更重要，取决于源数据的类型和所需的下游比较算法。在这里，我们不会尝试提供一组详尽的可能技术，而是专注于 Amazon vs Google 产品的示例用例的说明性实现。在规范化的数据集中，我们有 4 列要处理，它们的特征是`name, description, manufacturer, price`。4 列中有 3 列是基于文本的，大部分识别信息都存储在其中。因此，我们将在这里集中大部分的特性化工作。有许多方法来特征化文本数据，但是在高层次上，它们通常归结为

1.  标记化——例如[将句子分解成单词](https://nlp.stanford.edu/IR-book/html/htmledition/tokenization-1.html)
2.  令牌标准化——例如[词干](https://en.wikipedia.org/wiki/Stemming)或[词汇化](https://en.wikipedia.org/wiki/Lemmatisation)
3.  嵌入——例如 [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) 、[单词嵌入](https://en.wikipedia.org/wiki/Word_embedding)、[句子嵌入](https://en.wikipedia.org/wiki/Sentence_embedding)

对于我们的特定示例，我们将使用一个基本的 TF-IDF 模型，在该模型中，我们对文本进行标记，删除英语停用词，并应用 TF-IDF 矢量化。我们还将使用 Tensorflow 的 U [通用语句编码器](https://tfhub.dev/google/universal-sentence-encoder/4)模型，将名称和描述字符串转换成 512 维向量。

通过将这两个转换应用到`name, description, manufacturer`中的文本列，我们最终得到了许多丰富的特性，可以用来评估候选对之间的潜在匹配，以及帮助选择阻塞键。具体来说，TF-IDF 向量和句子编码向量在生成良好的块密钥方面非常有用。

正如我们上面讨论的，一个好的阻塞键是平衡特异性和假阴性率的东西。一个产品的全称是特定的，但也有非常高的假阴性率。我们可以通过在产品名称或描述中选择单个单词来解决这个问题。但是，重要的是要注意，就阻塞的目的而言，有些标记比其他标记更好。例如在`learning quickbooks 2007`中，`quickbooks`直观上是最好的阻挡键，其次是`2007`，再其次是`learning`。这是因为`quickbooks`是定义产品最有意义的关键词，`2007`是特定于产品版本的，而`learning`是一个非常通用的描述符。方便的是，这也正是 TF-IDF 试图系统测量的，并且`learning quickbooks 2007`的归一化 TF-IDF 令牌权重是`quickbooks` : 0.7，`2007` : 0.51，`learning` : 0.5。如您所见，令牌的 TF-IDF 权重是一个不错的代理指标，可用于区分优先级和选择良好的阻塞密钥。

类似地，句子编码向量权重也告诉我们潜在向量空间中的哪个维度对句子的意义最显著，并且可以用于选择分块密钥。然而，在句子编码的情况下，具有最高权重的维度不直接映射到单词标记，因此可解释性较低。

实现上述内容的 PySpark 代码示例如下

上面的代码利用`pyspark.ml`库来

1.  将 TF-IDF 变换应用于`name`、`description`和`manufacturer`
2.  根据 TF-IDF 令牌权重挑选出前 5 个令牌作为阻止密钥
3.  使用通用语句编码器将`name`和`descriptions`转换为 512 维向量表示。我们在这里省略了`manufacturer`,因为它是稀疏的，并且作为一个英语句子也没有太多的语义意义。请注意，我们实际上并没有从编码中生成块密钥，但是类似于 TF-IDF 令牌的方法也可以很容易地应用到这里
4.  将 top name 标记、top description 标记和 top manufacturer 组合成每个记录的一个块键数组。请注意，每条记录可以有一个以上的阻止键。这也是为了减少潜在匹配的假阴性率。

应用通用语句编码器的用户定义函数值得快速解包

```
# magic function to load model only once per executor
MODEL = None
def get_model_magic():
  global MODEL
  if MODEL is None:
      MODEL = hub.load("[https://tfhub.dev/google/universal-sentence-encoder/4](https://tfhub.dev/google/universal-sentence-encoder/4)")
  return MODEL[@udf](http://twitter.com/udf)(returnType=VectorUDT())
def encode_sentence(x):
  model = get_model_magic()
  emb = model([x]).numpy()[0]
  return Vectors.dense(emb)
```

这到底是在做什么，为什么有必要？Tensorflow hub 提供了一个预训练的模型，可以帮助用户将文本转换为矢量表示。这个模型不是分布式的，这意味着为了利用 Spark 这样的分布式计算框架，我们必须将它封装在一个用户定义的函数中，并使它在每个执行器节点上都可用。然而，我们不希望每次转换一段文本时都执行昂贵的`load`调用。我们只希望每个执行器加载一次，然后在分配给该节点的所有任务中重用。`get_model_magic`方法本质上是实现这一点的技巧。

现在我们已经有了我们的特征和阻塞键，我们准备好处理[候选对生成和匹配评分](https://yifei-huang.medium.com/practical-guide-to-entity-resolution-part-4-299ac89b9415)。