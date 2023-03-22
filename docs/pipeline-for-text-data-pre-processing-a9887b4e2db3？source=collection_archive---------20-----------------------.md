# 用于文本数据预处理的流水线

> 原文：<https://towardsdatascience.com/pipeline-for-text-data-pre-processing-a9887b4e2db3?source=collection_archive---------20----------------------->

## 使用 Python 简化使用文本数据建模的所有准备工作

![](img/82812f8fdc5719e38c4073cc63f5ac9a.png)

迈克·本纳在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## GitHub [链接](https://github.com/caseywhorton/Text-Pre-Processing)

# 介绍

如果你对这个比了解其背后的快速动机更感兴趣，请跳到实际的管道部分:**文本预处理管道(博客中途)。**

我一直在研究对文本数据执行机器学习，但有一些数据预处理步骤是我不习惯的文本数据所特有的。因此，我的 Python 代码包含了许多转换步骤，在这些步骤中，我会与数据争论，进行转换，然后转换训练数据，转换测试数据，然后对我想要进行的每种类型的转换重复这个过程。我记得曾经读到过 Python 有一种包装转换的便捷方法，但在此之前从未有过深入研究的理由。通常，我会对数字数据进行标准化缩放，或者创建一些虚拟变量，仅此而已。现在我进入了一个对我来说有点新的领域，我想保持这些转变并学习一些新的东西。

# 导入必要的包

```
import numpy as np
import pandas as pd
from scipy import sparse as spfrom sklearn.feature_extraction.text import CountVectorizer
from sklearn.feature_extraction.text import TfidfTransformerfrom sklearn.pipeline import Pipeline
from sklearn.preprocessing import LabelEncoderfrom sklearn.feature_selection import chi2
from sklearn.feature_selection import SelectKBest
```

如果你像我一年前一样是使用 NLTK 的新手，[这个网站将帮助你启动并运行](https://pythonprogramming.net/tokenizing-words-sentences-nltk-tutorial/)，并且被保存到我的收藏夹中，因为我在使用 NLTK 时经常使用它作为参考。

```
# Import Natural language Toolkit example data and 'stopwords' setfrom nltk.corpus import movie_reviews
from nltk.corpus import stopwords
```

# 导入示例“电影评论”数据集

“电影评论”数据有 2000 个电影评论，每个评论都有一个相应的类别(标签)，用于表示它是负面的(“负面”)还是正面的(“正面”)。

```
docs = [(str(movie_reviews.raw(fileid)), category)
 for category in movie_reviews.categories()
 for fileid in movie_reviews.fileids(category)]
```

现在只是把“文档”改成熊猫数据框架，主要是因为我习惯了这种格式。数据转换管道不仅仅适用于熊猫数据帧。

```
reviews = pd.DataFrame(docs)
reviews.columns=('X','y')
```

电影评论的“类别”最初是“负面”或“正面”，在这里分别更改为 0 和 1:

```
bin_encoder=LabelEncoder()
reviews.y=bin_encoder.fit_transform(reviews.y)
```

此时，评审在一个数据框架中，其中 **X** 列作为评审文本， **y** 列作为目标变量，评审类别。这更符合我目前习惯的机器学习项目的风格。以下是前 5 个观察结果的视图:

```
reviews.head(5)
```

故事情节:两对青少年夫妇去参加一个教堂聚会，… 0 1 快乐私生子的快速电影评论\ndamn … 0 2 正是这样的电影使一部令人厌倦的电影… 0 3《寻找卡梅洛特》是华纳兄弟公司出品的

需要做的是将 **X** 列标记化，删除停用词，并将每个观察结果矢量化，以便机器学习如何区分正面和负面评论。

# 停用词

NLTK 和 Scikit-Learn 函数 *CountVectorizer* 都有内置的停用词集合或列表，它们基本上是一串我们不希望在数据中出现的词。像“a”、“of”和“the”这样的词通常是没有用的，它们在句子或段落中出现的频率比其他词高。不过，有时你想添加一些你自己的自定义停用词，所以我在这里创建了我的列表，custom_stopwords :

```
mystopwords = (stopwords.words())
custom_stopwords = ('the','an','a','my','0','''''','!','nt','?','??','?!','%','&','UTC','(UTC)')
```

现在，电影评论已经在一个数据帧中，并且有一个可以从电影评论中删除的停用词列表，我将展示我之前使用的文本预处理管道和我使用 Sci-kit Learn 的*管道*制作的管道。

# 文本预处理管道

## 用于文本转换的陈旧生锈的“管道”

这个想法是将所有的句子放入一个大矩阵中，矩阵中的列代表单个单词，术语频率或术语频率逆文档频率作为相应单词下的观察值。
通常，在机器学习项目中，你需要重复你对训练数据到测试数据所做的转换，所以我明确地保存了转换。这就是为什么我用一行来保存 *TfidfTransform* 和另一行来实际转换训练数据。后来，我在测试数据集上使用了那个 *TfidfTransform* 。我用*计数矢量器*做了同样的事情。

```
count_vect = CountVectorizer(stop_words=mystopwords,lowercase=True)
X_train_counts = count_vect.fit_transform(X)
tf_transformer = TfidfTransformer(use_idf=True).fit(X_train_counts)
X_train_tfidf = tf_transformer.transform(X_train_counts)
```

类似地，为了选择数据集的前 k 个特征(单词),我确保保存转换并将其应用于训练和测试数据集。

```
chi2(X_res,y_res)
k=1000
ch2_score = SelectKBest(chi2, k=k)
toxic_feature_tran = ch2_score.fit(X,y)
X_train_k = ch2_score.fit_transform(X, y)
X_test_k = ch2_score.transform(X_test)
```

这不仅是一个麻烦，而且如果我不得不在任何时间点重复这个过程，那么我将不得不创建新的变量，并确保所有这些步骤按顺序进行，不要覆盖任何内容。这不是好的编码。这就是*管道*进入并清理一切的地方。

## 新文本预处理转换

[如果你的机器学习项目开始处理几个转换](http://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html)，这是你需要下载和利用的包。对于 Python 中的每一个具有' **fit_transform()'** 方法的转换(我在这里使用的所有方法都是如此)，我们可以将它们包装在一个实际的管道中，该管道按顺序执行它们，甚至可以返回并查看每个转换的属性。此外，您可以为每个转换设置参数，语法在我刚刚分享的链接中。如果有什么不同的话，使用这个管道已经清理了我的代码，并且更好地组织了我的思维。

使用这三个转换:

*   **“计数矢量化器”**:从句子转换为所有小写单词，删除停用词，矢量化
*   **【chi 2s score】**:基于卡方检验统计选择与目标相关的前 k 个特征的转换
*   **‘TF _ transformer’**:将顶部特征的向量转换为 tf-idf 表示的转换

管道看起来像这样:

```
# Using a variable for the top k features to be selected
top_k_features=1000text_processor = Pipeline([
 ('count vectorizer',CountVectorizer(stop_words=mystopwords,lowercase=True)),
 ('chi2score',SelectKBest(chi2,k=top_k_features)),
 ('tf_transformer',TfidfTransformer(use_idf=True))
])
```

现在，一旦这适合于训练数据， *text_preprocessor* 管道具有 transform 方法，该方法对数据执行**所有三个包含的转换**。

## 适应和转换

与使用 **fit_transform()** 方法的任何其他转换一样， *text_processor* 管道的转换是 fit，并且数据被转换。 *proc_fit* 可以用来以同样的方式转换测试数据。

```
proc_text = text_processor.fit_transform(reviews.X,reviews.y)
proc_fit = text_processor.fit(reviews.X,reviews.y)
```

## 检查变化

仍然可以通过以下两个步骤来返回特定评论的最后 1000 个单词中的原始单词:

1.  查找从“chi2score”转换返回的前 1000 个功能的索引
2.  找到“功能名称”，即原文中的单词

```
proc_fit.named_steps['chi2score'].get_support(indices=True)[616]
```

我从第一个变形的影评词中选出了数字 616。原来这对应于原始特征中的特征号 23078。(经过“chi2score”转换后，只剩下 1000 个特性，特性的数量也发生了变化。)

```
Out[49]:23078
```

返回的内容:单词“music”是对应于来自原始电影评论文本数据的特征号 **23078** 的单词，该文本数据被转换成计数向量。它现在是前 1000 个特性中的第 616 个特性。我们总是可以打印整个电影评论，看看这个词是否出现在那里。

```
proc_fit.named_steps['count vectorizer'].get_feature_names()[23078]
'music'
print(reviews.iloc[0,0])
```

检查第一篇评论，我们可以看到“音乐”这个词确实存在，而且这个词出现在整个电影评论语料库的前 1000 个精选特征中。

> 工作室从导演那里拿走了这部电影，他们自己把它切碎了，然后它就出现了。在这里的某个地方可能会有一部相当不错的青少年心灵操电影，但我想《西装革履》决定将它变成一部没有多少棱角的 ***音乐*** *视频，会更有意义。大部分演员都很出色，尽管韦斯·本特利似乎只是在扮演他在《美国丽人》中扮演的那个角色……*

通常，不需要在原始数据中查找单个单词，并确保它在新的转换数据中具有特征。在这里，我花时间完成了这些步骤，以确保我构建的管道按预期工作，并展示了管道的所有部分如何显示它们的属性。

# 结论

非结构化文本数据在机器学习项目中非常有用。需要将文本数据转换为机器可以学习的更结构化的格式，通常使用预构建的转换，如删除停用词和 tf-idf 转换。可以构建数据需要经过的转换管道来简化代码，并为文本预处理阶段提供一致性。利用*管道*函数可以帮助将几个转换打包成一个。

# 参考

[](http://www.nltk.org/) [## 自然语言工具包- NLTK 3.5 文档

### NLTK 是构建 Python 程序来处理人类语言数据的领先平台。它提供了易于使用的…

www.nltk.org](http://www.nltk.org/) [](https://pythonprogramming.net/tokenizing-words-sentences-nltk-tutorial/) [## Python 编程教程

### 欢迎来到自然语言处理教程系列，使用自然语言工具包(NLTK)模块…

pythonprogramming.net](https://pythonprogramming.net/tokenizing-words-sentences-nltk-tutorial/) 

[http://scikit learn . org/stable/modules/generated/sk learn . pipeline . pipeline . html](http://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html)