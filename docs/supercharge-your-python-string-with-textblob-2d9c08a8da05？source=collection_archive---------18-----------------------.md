# 使用 TextBlob 增强您的 Python 字符串

> 原文：<https://towardsdatascience.com/supercharge-your-python-string-with-textblob-2d9c08a8da05?source=collection_archive---------18----------------------->

## 在一行代码中获得更多关于文本的见解！

![](img/648c330b5c45e318d434baba8ba48713.png)

照片由[托马斯·凯利](https://unsplash.com/@thkelley?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 动机

数据不仅包含数字，还包含文本。了解如何快速处理文本将有助于您更快地分析数据，并从数据中获得更多见解。

文本处理不需要很难。如果我们只需要在**一行代码**中找到文本的情感、标记文本、找到单词和名词短语的频率，或者纠正拼写，这不是很好吗？这时 TextBlob 就派上用场了。

# 什么是 TextBlob？

[TextBlob](https://textblob.readthedocs.io/en/dev/quickstart.html) 旨在通过一个熟悉的界面提供对常见文本处理操作的访问。您可以将 TextBlob 对象视为学习如何进行自然语言处理的 **Python 字符串**。

NLTK 提供了一些方法来完成这些任务，但是您可能需要调用几个类来完成不同的任务。但是使用 TextBlob，您需要做的就是使用`TextBlob(text)`来访问 TextBlob 的不同方法！

使用安装 TextBlob

```
pip install -U textblob
python -m textblob.download_corpora
```

现在，我们需要做的就是用`TextBlob`对象包装文本来增强我们的字符串。

让我们看看我们可以用增压弦做什么。

# 单词标记化

我们将借用我的文章[如何在生活不给你喘息的时候学习数据科学](/how-to-learn-data-science-when-life-does-not-give-you-a-break-a26a6ea328fd)中的一些句子来学习如何使用 TextBlob。

单词标记化根据某些分隔符将一段文本分割成单个单词。

而不是根据不同的分隔符如“，”或“.”来分割字符串或者空格，我们只需要用`blob.words`来修饰我们的句子！

```
WordList(['When', 'I', 'was', 'about', 'to', 'give', 'up', 'I', 'told', 'myself', 'to', 'keep', 'going', 'It', 'is', 'not', 'about', 'working', 'harder', 'it', 'is', 'about', 'working', 'smarter'])
```

`WordList`可以作为 Python 列表使用。要访问第一个单词，请使用

```
>>> blob.words[0]
'When'
```

# 名词短语抽取

名词短语是以一个名词为中心的一组两个或多个单词(例如，“狗”、“女孩”、“男人”)，包括修饰语(例如，“the”、“a”、“None of”)。例如，“女孩”不是名词短语，但“漂亮的女孩”是名词短语。

有时候，提取句子中的所有名词短语而不是提取单个名词对我们来说很重要。TextBlob 让我们可以轻松做到这一点

```
WordList(['learning strategies'])
```

正如我们所看到的，只有“学习策略”是从句子中提取的，因为它是句子中唯一的名词短语。

# 情感分析

我们还可以使用`blob.sentiment`提取句子的情感

```
Sentiment(polarity=0.15833333333333333, subjectivity=0.48333333333333334)
```

极性是一个位于(-1，1)范围内的浮点数。如果极性低于 0，句子的否定性大于肯定性。如果极性在 0 以上，则该句肯定多于否定。因为我们的极性是 0.15，所以它是正的多于负的。

主观性是指个人观点。主观性是一个位于(0，1)范围内的浮点数。如果主观性值大于 0.5，则句子的主观性大于客观性，反之亦然。由于句子的主观性是 0.48，所以客观大于主观。

# 词汇化

词汇化是一组单词的词典形式。例如，‘eats’，‘eat’，‘eating’都是‘eat’这个词的形式。词汇化是在应用其他 NLP 模型之前处理文本的一种流行方法。

为了使单词词条化，将单词放在`Word`对象中，而不是`TextBlob`

```
know
that
i
can
complete
these
things
in
a
short
amount
of
time
make
me
excite
and
be
in
the
flow
```

厉害！一些单词被转换成它们的字典形式。'知'转换为'知'，'激'转换为'激'，'使'转换为'使'。现在，NLP 模型不会将“知道”和“知道”视为两个不同的单词！

# 拼写纠正

有时，我们可能会在文章中发现一两个拼写错误。如果我们分析 Twitter 等社交媒体的文本，这个拼写错误就更常见了。拼写错误会使我们的 NLP 模型很难知道单词`learnin`和单词`learning`基本上是同一个单词！

TextBlob 还允许我们用一行代码来纠正拼写

我故意拼错了句子中的几个单词，比如`neded, swicth,`和`learnin.`让我们看看 TextBlob 如何纠正这些单词

```
TextBlob("I just need to switch my learning strategics.")
```

相当酷！所有的拼写错误都以合理的方式得到了纠正。

我们也可以用`Word().spellcheck()`纠正个别单词

```
[('learning', 0.25384615384615383),
 ('warning', 0.24615384615384617),
 ('margin', 0.2153846153846154),
 ('latin', 0.13076923076923078),
 ('martin', 0.05384615384615385),
 ('lain', 0.046153846153846156),
 ('earning', 0.038461538461538464),
 ('marin', 0.007692307692307693),
 ('darwin', 0.007692307692307693)]
```

拼写错误的单词`lanin`可能是许多事物的拼写错误，因此 TextBlob 给了我们不同的结果，其中`learning`是正确率最高的单词。

# 词频

有时候，知道某个单词在句子中的出现频率是很重要的。TextBlob 允许我们轻松地找到文本中某个单词的频率。

我们将使用我的文章[中的所有文字，讲述如何在生活不给你休息的时候学习数据科学](/how-to-learn-data-science-when-life-does-not-give-you-a-break-a26a6ea328fd)。我们将使用[报纸](https://pypi.org/project/newspaper/)刮文章。

```
pip install newspaper
```

由于这篇文章是关于我的数据科学之旅，我预计句子中会有很多“我”这个词。让我们用`blob.word_counts`检查一下

```
81
```

81 是一个很高的数字，但是与我文章中的其他词相比，它有多高呢？让我们用柱状图来形象化它

正如我们所猜测的，像“我”、“to”、“the”、“my”这样的词是文章中最常见的词！

# 结论

恭喜你！您刚刚学习了如何用 TextBlob 处理 Python 字符串。使用上面列出的所有处理方法的唯一步骤是将字符串包装在`TextBlob`对象或`Word`对象中。多简单啊！

你可以在 [TextBlob 的文档](https://textblob.readthedocs.io/en/dev/quickstart.html)中探索更多处理和分析文本的方法。

本文的代码可以在这里找到

[](https://github.com/khuyentran1401/Data-science/blob/master/nlp/textblob.ipynb) [## khuyentran 1401/数据科学

### permalink dissolve GitHub 是超过 5000 万开发人员的家园，他们一起工作来托管和审查代码，管理…

github.com](https://github.com/khuyentran1401/Data-science/blob/master/nlp/textblob.ipynb) 

我喜欢写一些基本的数据科学概念，并尝试不同的算法和数据科学工具。你可以在 [LinkedIn](https://www.linkedin.com/in/khuyen-tran-1ab926151/) 和 [Twitter](https://twitter.com/KhuyenTran16) 上联系我。

如果你想查看我写的所有文章的代码，请点击这里。在 Medium 上关注我，了解我的最新数据科学文章，例如:

[](/find-common-words-in-article-with-python-module-newspaper-and-nltk-8c7d6c75733) [## 用 Python 模块 Newspaper 和 NLTK 查找文章中的常用词

### 使用 newspaper3k 和 NLTK 从报纸中提取信息和发现见解的分步指南

towardsdatascience.com](/find-common-words-in-article-with-python-module-newspaper-and-nltk-8c7d6c75733) [](/sentiment-analysis-of-linkedin-messages-3bb152307f84) [## 使用 Python 和情感分析探索和可视化您的 LinkedIn 网络

### 希望优化您的 LinkedIn 个人资料？为什么不让数据为你服务呢？

towardsdatascience.com](/sentiment-analysis-of-linkedin-messages-3bb152307f84) [](/how-to-solve-analogies-with-word2vec-6ebaf2354009) [## 如何用 Word2Vec 解决类比问题

### 美国之于加拿大，就像汉堡之于 _？

towardsdatascience.com](/how-to-solve-analogies-with-word2vec-6ebaf2354009) [](/what-is-pytorch-a84e4559f0e3) [## PyTorch 是什么？

### 想想 Numpy，但是有强大的 GPU 加速

towardsdatascience.com](/what-is-pytorch-a84e4559f0e3) [](/convolutional-neural-network-in-natural-language-processing-96d67f91275c) [## 自然语言处理中的卷积神经网络

### 什么是卷积神经网络，如何利用它进行情感分析？

towardsdatascience.com](/convolutional-neural-network-in-natural-language-processing-96d67f91275c)