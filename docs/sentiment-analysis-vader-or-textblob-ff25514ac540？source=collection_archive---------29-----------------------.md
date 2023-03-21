# 情感分析:VADER 还是文本斑点？

> 原文：<https://towardsdatascience.com/sentiment-analysis-vader-or-textblob-ff25514ac540?source=collection_archive---------29----------------------->

## 两种常用库及其性能比较。

![](img/905c0cad4713bc0e4ca85908cd369666.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Austin Distel](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral) 拍摄的照片

# 什么是情感分析？

对于大多数企业来说，了解客户对其产品/服务的感受是非常有价值的信息，可用于推动业务改进、流程变革，并最终提高盈利能力。
情感分析是通过使用自然语言处理(NLP)来分析信息并确定负面、正面或中性情感的过程。

**非常简单地概括这个过程:**
1)将输入的内容标记成它的组成句子或单词。
2)用词性成分(即名词、动词、限定词、句子主语等)识别和标记每个标记。3)指定一个从-1 到 1 的情感分数。
4)返回分数和可选分数如复合分数、主观性分数等。

我将研究两个常见的库在这个任务中的表现——text blob 和 VADER。

对于每个图书馆，我将使用来自 IMDB 的更一般的评论声明以及一个包含更多俚语、表情符号等的 Twitter 帖子。

我们要分析的情感陈述将是:

[IMDB 审查](https://www.imdb.com/title/tt2527338/?pf_rd_m=A2FGELUUNOQJNL&pf_rd_p=99ed26bb-89a8-4157-8c78-3cd5b17286cd&pf_rd_r=B5RA8RG46HZSMZVV8MCZ&pf_rd_s=right-7&pf_rd_t=15061&pf_rd_i=homepage&ref_=hm_cht_t0):

```
I am a life long Star Wars fan and this was the first time I came out disappointed. And I am not picky, I was mostly happy even with the last two movies, but this one is the worst Star Wars movie yet.
```

推特声明:

```
I cannot stop watching the replays of this **incredible** goal. THE perfect strike 💘😁💘😁💘😁
```

# 文本 Blob

> " *TextBlob* 是一个用于处理文本数据的 Python (2 和 3)库。它提供了一个简单的 API，用于处理常见的自然语言处理(NLP)任务，如词性标注、名词短语提取、情感分析、分类、翻译等

从 TextBlob 的网站[这里](https://textblob.readthedocs.io/en/dev/)。

# VADER

> VADER (Valence Aware 字典和情感推理器)是一个基于词典和规则的情感分析工具，专门针对社交媒体中表达的情感*。*

来自 VADER 的 Github [这里](https://github.com/cjhutto/vaderSentiment)。

通过 **TextBlob** 运行，我们可以看到如下输出:

```
IMDB: Sentiment(polarity=-0.125, subjectivity=0.5916666666666667)
Twitter: Sentiment(polarity=0.95, subjectivity=0.95)
```

极性是一个介于-1 和 1 之间的浮点数，其中-1 是一个否定的陈述，1 是一个肯定的陈述。从上面，我们可以看到 IMDB 的声明被认为是负面的，但并不严重，Twitter 的声明是非常积极的。
主观性是 **TextBlobs** 对陈述是否被视为更多观点或基于事实的评分。较高的主观性分数意味着它不太客观，因此会非常固执己见。

而如果我们用 **VADER** 的情绪来代替:

```
IMDB:{'neg': 0.267, 'neu': 0.662, 'pos': 0.072, 'compound': -0.9169}
Twitter:{'neg': 0.026, 'neu': 0.492,'pos': 0.482,'compound': 0.9798}
```

VADER 的操作略有不同，将输出 3 个分类级别的得分，以及一个复合得分。
从上面，我们可以看到 IMDB 评论中有大约 66%的单词属于中性情感类别，然而它的复合分数——即“标准化、加权、综合分数”——将其标记为非常负面的陈述。基于 0.9798 的复合得分，Twitter 声明再次被评为非常积极。

两个库输出相对相似的结果，但是 VADER 从 IMDB 评论中发现了更多的负面论调，而 TextBlob 忽略了这一点。
这两个库都具有高度的可扩展性，可以查看与自然语言处理相关的许多其他类别，例如:

## 词性标注

将句子转换成元组列表(单词、标签)的过程。的标记是词性标记，表示这个词是名词、形容词还是动词等。

```
('I', 'PRP')
('can', 'MD')
('not', 'RB')
('stop', 'VB')
('watching', 'VBG')
('the', 'DT')
('replays', 'NN')
('of', 'IN')
('this', 'DT')
('incredible', 'JJ')
('goal', 'NN')
('THE', 'DT')
('perfect', 'JJ')
('strike', 'NN')
('💘😁💘😁💘😁', 'NN')
```

## 标记化

将句子或文本块分解成单独的“记号”进行分析。

```
['I', 'can', 'not', 'stop', 'watching', 'the', 'replays', 'of', 'this', 'incredible', 'goal', 'THE', 'perfect', 'strike', '💘😁💘😁💘😁']
```

## N-grams

将句子分成大小为 n 的块。在下面的例子中，我使用 n=5，因此它输出所有可能的连续 5 个标记的块。

```
[WordList(['I', 'can', 'not', 'stop', 'watching']),
WordList(['can', 'not', 'stop', 'watching', 'the']), WordList(['not', 'stop', 'watching', 'the', 'replays']), WordList(['stop', 'watching', 'the', 'replays', 'of']), WordList(['watching', 'the', 'replays', 'of', 'this']), WordList(['the', 'replays', 'of', 'this', 'incredible']), WordList(['replays', 'of', 'this', 'incredible', 'goal']), WordList(['of', 'this', 'incredible', 'goal', 'THE']), WordList(['this', 'incredible', 'goal', 'THE', 'perfect']), WordList(['incredible', 'goal', 'THE', 'perfect', 'strike']), WordList(['goal', 'THE', 'perfect', 'strike', '💘😁💘😁💘😁'])]
```

还有很多很多！

你真正受到限制的只是你的创造力和你想要深入你的陈述的程度。
这两个库都提供了大量的特性，最好尝试运行一些关于你的主题的样本数据，看看哪个最能满足你的需求。根据我的测试，VADER 似乎在俚语、表情符号等方面表现更好，而 TextBlob 在更正式的语言使用方面表现强劲。

![](img/ad18e7aa587395cdd7c270c5c4799f1f.png)

照片由 [Prateek Katyal](https://unsplash.com/@prateekkatyal?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄