# 任何东西都有它的价格——如何给单词和短语定价，在线广告竞价等等

> 原文：<https://towardsdatascience.com/everything-has-its-price-how-to-price-words-for-ad-bidding-etc-7df38e1d152?source=collection_archive---------36----------------------->

这篇文章概述了自然语言单词或短语定价的 NLP 方法。它创造性地利用了(1)模型 word2vec，它从给定的语料库中学习上下文和单词之间的关联；(Mondovo 数据集，它为我们进一步引导我们的应用程序提供了基本的构建块。该解决方案将在诸如在线广告竞价、在线营销、搜索引擎优化等领域具有有趣的应用。这篇文章是定价问题的初始基线解决方案的一个示例，渴望了解更多关于我在实践中是如何做的以及对该主题更深入的处理的读者欢迎收听我的后续出版物。

![](img/0ecc3631e43ebaca44d6bee1b114c047.png)

由[马库斯·温克勒](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

人们正在量化一切。当我们无法做到这一点时，我们称之为无价值的或神秘的，或熟练地将其视为幻觉；爱情、忠诚、诚实等等就是如此。

在线广告竞价行业绝对不是例外，他们最大的问题之一是如何为他们选择的广告关键词或短语提供准确的竞价价格，以确保出版商网站上的一些热点广告。困境是这样的:如果出价太高，你可能肯定会得到广告位，但你也将不得不支付你出价的高昂价格；如果你把出价定得太低，你可能很难得到那个广告位。显然，这种微妙的权衡需要创造性地解决将单词/短语量化为价格的问题。

幸运的是，我们可以放心一个响亮的好消息:**单词也可以定价！对于这个问题，我们可能没有像 Black-Scholes 期权定价模型那样精心制作的处方，但我们有多种方法可以解决这个问题。**

在本文中，我将为关键词定价问题草拟一个简单的解决方案，它基本上使用了一种叫做 **word2vec 的自然语言处理技术。**接下来的部分将展示如何处理数据，在哪里使用 word2vec，如何将我们的问题转化为一个回归任务，最后展示整个管道的性能。

让我们开始吧。

![](img/dea6b03ed9b0fa772dea4af686b6621c.png)

[约翰·金](https://unsplash.com/@johnking?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# word2vec 简介

追溯统计语言模型的演变可能是有帮助的。首先，我们有简单的**词袋模型**，在这个模型中，我们离散地对待语料库中的每个词；没有上下文，没有依赖，只有独立的词。对于这样一个模型，你能做的最好的事情就是想出一个词频图表。

接下来是 **n 元模型**。单字，即单个单词，没有那么强大，但我们可以扩展到双字、三字、四字等，其中每 N (2、3、4 或更多)个连续的单词被视为一个整体(作为单个单词)。可以说，这样的模型将能够捕捉大小为 N 的单词上下文，并使我们能够进行更复杂的预测和推理。例如，我们可以轻松地构建更强大的概率状态转移模型，如马尔可夫链，它支持日常应用，如单词自动暗示或自动完成。

相比之下， **word embedding** 是一个语言模型家族，其中使用向量来描述/表示词汇表中的单词或短语，而 **word2vec** 是最流行的技术之一。一般来说，它使用神经网络从给定的语料库中学习单词关联/关系，并使用给定长度的向量来表示每个单词，使得单词之间的语义相似性将与它们的向量表示之间的向量相似性相关。维基百科页面将提供一个很好的初始指针，对于这个主题的更深入的处理，请继续关注我未来的帖子。

# 数据处理

这是极其重要的一步。为了让我们提出任何模型，我们首先需要数据。此外，为了让我们的模型学习数据之间任何有意义的关系，我们希望数据包含从自然语言单词到价格的示例映射。不幸的是，互联网上有许多这样的数据集，我能找到的一个来自 [Mondovo](https://www.mondovo.com/keywords/most-asked-questions-on-google) 。这个特定的数据集包含了 Google 上最常问的 1000 个问题及其相关的全球每次点击成本，尽管数据集相当小，但它提供了我们需要的基本成分:单词及其价格。

将这 1000 行数据打包成一个包含两列的 pandas dataframe 是相当容易的:*关键字*和*价格*，从现在开始我们称这个 dataframe 为 *df* 。

然后，让我们执行以下步骤，以确保数据的顺序确实是随机的:

```
df = df.sample(frac=1).reset_index(drop=True)
```

这就是我们的数据预处理。

# **模型导入**

现在让我们稍微关心一下 word2vec。在这项任务中，我们将依赖于一些现成的向量表示，而不是从我们自己的语料库(即 1000 个短语)中学习单词向量表示。以下代码片段将介绍 Google 的开箱即用解决方案:

```
import gensim.downloader as apiwv = api.load('word2vec-google-news-300')
```

据[这位消息人士](https://github.com/mmihaltz/word2vec-GoogleNews-vectors)称，该模型建立在‘预先训练好的谷歌新闻语料库(30 亿个运行词)，(并包含)词向量模型(300 万个 300 维英文词向量)’。

# 从单词到句子

这里有一个问题:**模型 word2vec 只包含单个单词的向量表示，但是我们需要像我们数据集中的那些短句/短语**的向量表示。

至少有三种方法可以解决这个问题:

(1)取短句中所有单词的向量的平均值；

(2)类似地，取平均值，但是使用单词的 idf(逆文档频率)分数对每个向量进行加权；

(3)使用 doc2vec，而不是 word2vec。

在这里，我很想看看基线模型的表现如何，所以让我们暂时使用(1 ),将其他选项留给将来的探索。

以下代码片段将提供一个简单的示例来实现平均函数:

```
def get_avg(phrase, wv):
    vec_result = []
    tokens = phrase.split(' ') for t in tokens:
        if t in wv:
            vec_result.append(wv[t].tolist())
        else:
            #300 is the dimension of the Google wv model
            vec_result.append([0.0]*300) return np.average(vec_result, axis=0)
```

请注意*如果*条件在某个“停用词”(给定语言中极其常见且通常无信息的词)中是必要的。在英语中，认为“the”、“it”、“which”等已被排除在谷歌模式之外。在上面的片段中，我留了一些余地，跳过了详细处理缺少单词或停用词的主题。在我以后的文章中，将会有更深入的讨论。请继续收看！

![](img/f175f7859208dfe7211ad3d5b8c8b26c.png)

照片由[米卡·鲍梅斯特](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 回归问题设置

**请记住，从根本上讲，几乎所有的机器学习算法都期望数字输入**:例如，在图像处理问题中，黑白图片作为 0–1 的矩阵提供给算法，彩色图片作为 RGB 张量。我们的问题也不例外，这就是为什么我们不厌其烦地介绍 word2vec。

考虑到这一点，让我们看看机器学习算法中使用的特征矩阵和目标向量:

```
X = np.array([get_avg(phrase, wv) for phrase in df['keyword']])y = df['price']
```

由于我们预测的是一些数值，这是一个回归问题。让我们为此任务选择一些简便的回归算法:

```
from sklearn.ensemble import RandomForestRegressor#leaving out all params tuning to show absolute baseline performance
reg = RandomForestRegressor(random_state=0)
```

# 表演

现在我们终于能够看到我们的绝对基线模型的表现了。让我们建立如下 10 重交叉验证方案:

```
from sklearn.model_selection import KFoldfrom sklearn.metrics import mean_absolute_error#set up 10-fold Cross Validation:
kf = KFold(n_splits=10)#loop over each fold and retrieve result
for train_index, test_index in kf.split(X):
    X_train, X_test = X[train_index], X[test_index]
    y_train, y_test = y[train_index], y[test_index] reg.fit(X_train, y_train)

    print(mean_absolute_error(y_test, reg.predict(X_test)))
```

在我的实验中，运行上面的代码给出了 MAE 分数 1.53、0.98、1.06、1.23、1.02、1.01、1.06、1.19、0.96 和 0.96，导致平均 MAE 为 1.1，这意味着我们的估计价格平均可能会偏离真实价值 1.1 美元。

考虑到可用的数据稀少，训练数据中缺乏单词冗余，样本内数据点稀疏，以及我们在没有任何参数优化的情况下的绝对基线假设，我对我们目前的方法能够推进的程度印象深刻。不难想象，一些热心的读者自己做实验一定会取得更好的结果。