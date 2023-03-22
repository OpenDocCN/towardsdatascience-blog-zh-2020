# 自然语言处理词干初学者指南

> 原文：<https://towardsdatascience.com/a-beginners-guide-to-stemming-in-natural-language-processing-34ddee4acd37?source=collection_archive---------26----------------------->

## 通过减少单词的词干来降低维数并改善结果

![](img/5ad3ed88dd4a4c66d26969af43affebc.png)

照片由 [**Pixabay**](https://www.pexels.com/@pixabay?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 发自 [**Pexels**](https://www.pexels.com/photo/close-up-of-eyeglasses-256273/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

我的工作几乎完全专注于 NLP。所以我用文字工作。很多。

文本 ML 有它自己的挑战和工具。但一个反复出现的问题是维度过多。

在现实生活中(不是 Kaggle)，我们通常只有有限数量的带注释的例子来训练模型。并且每个例子通常包含大量的文本。

这种稀疏性和缺乏共同特征使得很难在数据中发现模式。我们称之为[维度诅咒](https://en.wikipedia.org/wiki/Curse_of_dimensionality)。

一种降维技术是**词干**。

它从一个单词中去掉后缀，得到它的“词干”。

于是`fisher`变成了`fish`。

让我们来学习词干是如何工作的，为什么要使用它，以及如何使用它。

# 什么是词干？

## 后缀

词干提取通过去除后缀来减少单词的词干(词根)。

**后缀**是修饰词义的词尾。这些包括但不限于...

```
-able
-ation
-ed
-er
-est
-iest
-ese
-ily
-ful
-ing
...
```

显然，`runner`和`running`这两个词有不同的意思。

但当涉及到文本分类任务时，这两个词都暗示着一个句子在谈论类似的东西。

## 该算法

词干算法是如何工作的？

简而言之，算法包含一系列删除后缀的规则，这些规则有条件地应用于输入令牌。

这里没有任何统计数据，所以算法是非常基本的。这使得词干分析可以在大量文本中快速运行。

如果你感兴趣，NLTK 的波特词干算法是免费的，这里是的[，解释它的论文是](https://www.nltk.org/_modules/nltk/stem/porter.html)[这里是](https://tartarus.org/martin/PorterStemmer/def.txt)。

也就是说，有不同的词干算法。仅 NLTK 就包含波特、斯诺鲍和兰开斯特。一个比一个更积极地阻止。

# 阻止文本的原因

## 语境

NLP 的很大一部分是弄清楚一个文本主体在说什么。

虽然不总是正确的，但包含单词`planting`的句子经常谈论与包含单词`plant`的另一个句子相似的事情。

考虑到这一点，为什么不在训练一个分类模型之前，先把所有的单词归到它们的词干中呢？

还有另一个好处。

## 维度

大量的文本包含大量不同的单词。结合有限数量的训练示例，稀疏性使得模型很难找到模式并进行准确的分类。

减少单词的词干减少了稀疏性，使发现模式和做出预测变得更容易。

词干分析允许每个文本字符串用一个更小的单词包来表示。

**举例:**词干化后的句子，`"the fishermen fished for fish"`，可以用这样一包词来表示。

```
[the, fisherman, fish, for]
```

而不是。

```
[the, fisherman, fished, for, fish]
```

这既降低了样本之间的稀疏性，又提高了训练算法的速度。

**根据我的经验**，与使用单词嵌入相比，通过词干处理丢失的信号非常少。

# Python 示例

我们将从一串单词开始，每个单词的词干都是“鱼”。

```
'fish fishing fishes fisher fished fishy'
```

然后对该字符串中的标记进行词干处理。

```
from nltk.tokenize import word_tokenize
from nltk.stem.porter import PorterStemmer text = 'fish fishing fishes fisher fished fishy' # Tokenize the string
tokens = word_tokenize(text)
print(tokens) 
#=> ['fish', 'fishing', 'fishes', 'fisher', 'fished', 'fishy'] stemmer = PorterStemmer()
stems = [stemmer.stem(w) for w in tokens]
print(stems)
#=> ['fish', 'fish', 'fish', 'fisher', 'fish', 'fishi']
```

结果是。

```
['fish', 'fish', 'fish', 'fisher', 'fish', 'fishi']
```

请注意，它并没有将所有记号都简化为“fish”这个词干。波特算法是最保守的词干提取算法之一。也就是说，这仍然是一个进步。

# 结论

有趣的是，预处理是 NLP 管道中最重要的(也是被忽略的)部分。

它决定了最终馈入 ML 模型的数据的形状，以及馈入模型质量数据和垃圾之间的区别。

正如他们所说，垃圾进，垃圾出。

在删除标点符号和停用词之后，词干是大多数 NLP 管道的关键组成部分。

词干化(和它的表亲[词汇化](https://en.wikipedia.org/wiki/Lemmatisation))会给你更好的结果，使用更少的数据，并减少模型训练时间。