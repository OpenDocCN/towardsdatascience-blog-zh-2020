# 将“上下文”嵌入配方成分

> 原文：<https://towardsdatascience.com/embedding-contexts-into-recipe-ingredients-709a95841914?source=collection_archive---------50----------------------->

## 利用食材的上下文向量对菜肴进行分类

![](img/9a9536e65147e174dd810baedc211c64.png)

图片:Pixabay

看着一系列的配料，我们可以确定这种菜肴来自哪里。

但是，机器也能做到吗？

如果我将配料列表转换成一个简单的单词包矩阵，我会得到一个一次性编码的矩阵，因为每种配料在给定的食谱中只出现一次。使用简单的线性回归，我在[yummy recipes 数据集](https://www.kaggle.com/c/whats-cooking)上获得了 78%的分类准确率，甚至没有清理文本。有趣的是，基于树的方法和神经网络也被成功地应用于这项任务，并取得了良好的效果。

# 我们能使用单词嵌入来改善这些结果吗？

因为 NLP 现在很流行，我想知道单词嵌入向量是否可以用于分类任务？我在硕士项目的一个小组项目中回答了这个问题。

首先，我们清理了成分列表，删除了停用词、符号、数量标记，只保留了名词。它看起来是这样的:

```
Before cleaning: 
['(2 oz.) tomato sauce', 'ground black pepper', 'garlic', 'scallions', 'chipotles in adobo', 'dried thyme', 'instant white rice', 'cilantro leaves', 'coconut milk', 'water', 'red beans', 'chopped celery', 'skinless chicken thighs', 'onions', 'lime wedges', 'carrots']After cleaning: 
['tomato sauce', 'pepper', 'garlic', 'scallion', 'chipotle adobo', 'thyme', 'rice', 'cilantro leaf', 'coconut milk', 'bean', 'celery', 'skinless chicken thigh', 'onion', 'lime wedge', 'carrot']
```

接下来，我们使用 [Gensim 的 Word2Vec 实现](https://radimrehurek.com/gensim/models/word2vec.html)为数据中的每个独特成分获取一个 300 长度的向量。我们通过添加配方成分的向量来形成配方的向量。因此，数据中的每个配方都有一个向量。让我们看看这些向量在使用 tSNE 转换成二维表示时是什么样子:

![](img/b6dcfe85c4015ee7cfcfbd4e51eebf28.png)

二维表示的向量

虽然有很多噪音，我们确实看到一些集群。例如，中文、泰文、日文和韩文聚集在左下角。写这篇文章的时候，我发现了一个有趣的事实:摩洛哥菜和印度菜有一些相似之处！从上图的右下部分可以明显看出这一点。

接下来，我们使用这种向量表示法进行分类，这种表示法应该具有食谱的“上下文”。使用简单的线性回归，干净的配方列表和上下文向量表示给出的分类准确度仅为 65%，低于基线 78%。

悲伤，是吧？

我想让我们详细看看一些菜系和它们的顶级食材。

我注意到实际上是法国的食谱被错误地归类为意大利的。我认为这没什么，考虑到阶级不平衡，意大利菜在数据中的出现率最高。然而，一个值得注意的观察是，意大利菜被误传为法国菜最多。考虑到法国菜不在数据中出现率较高的食物之列，这令人惊讶。此外，意大利菜并没有被大量误认为是爱尔兰菜，但是法国菜却被误认为是爱尔兰菜。

因此，我们仔细观察了这三种菜肴的顶级配料。

![](img/af377f63991c8173ecbed6577077e446.png)

法国、意大利和爱尔兰菜肴的顶级配料

可以看出，法国菜和意大利菜有两种共同的成分，油和丁香。法国菜和爱尔兰菜也有两种主要成分，奶油和黄油，尽管一小部分黄油也有助于意大利菜。有趣的是，意大利菜和爱尔兰菜除了所有三种菜系共有的配料之外，并没有太多的共同成分，那就是胡椒、洋葱、鸡蛋和其他。这进一步加强了我们的观察，即法国菜与意大利菜和爱尔兰菜有相似之处，然而，意大利菜和爱尔兰菜并不相似。因此，意大利菜和法国菜之间的错误分类是很自然的，因为它们基于食谱中使用的配料而彼此非常相似。

# 那么，既然分类准确率从 78%降到了 65%，那么嵌词是不是不好呢？

大概吧。

大概不会。

有一些警告。

首先，数据有不平衡的类别。意大利菜和墨西哥菜比其他菜更常见。然而，这肯定也是估计基线时的问题！

可能，更彻底(或者不彻底？)需要清理数据。我们最初的想法是在数据中只保留名词。也许我们也需要保留其他词类。也许副词和连词在数据中保留没有意义，但确实发挥了作用？

也许向量需要更多的调整。用于生成单词向量的超参数可能需要优化。我们将菜谱中的单个单词向量相加，形成菜谱向量。考虑到一份食谱中的配料数量差异很大，也许我们需要对食谱向量进行标准化。

最后，应该注意的是，上面对烹饪组成的解释表明，向量确实成功地将相似的烹饪聚集在一起。我也解释了分类错误的原因。这暗示了单词嵌入对于聚类应用是有益的可能性。而对于分类，还需要做更多的工作。

# 反正得到差的结果也是好的研究，对吧？

查找代码@:[https://github . com/mohanni shant 6/Recipe-Ingredients-as-Word-embedding](https://github.com/mohannishant6/Recipe-Ingredients-as-Word-Embeddings)

在 [LinkedIn](https://www.linkedin.com/in/mohannishant/) 上和我联系！

在 [GitHub](https://github.com/mohannishant6) 上看看我的一些有趣的项目吧！