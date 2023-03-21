# 如何根据语义相似度对文本内容进行排序

> 原文：<https://towardsdatascience.com/how-to-rank-text-content-by-semantic-similarity-4d2419a84c32?source=collection_archive---------1----------------------->

## 使用 NLP、TF-idf 和 GloVe 查找 Python 中的相关内容并进行排名

搜索是查找内容的标准工具——无论是在线还是设备上的——但是如果你想更深入地根据单词的*含义*来查找内容呢？为自然语言处理开发的工具会有所帮助。

![](img/a46c6143506243d12defeaa60511047f.png)

帕特里克·托马索在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 内容

在本文中，我们将介绍两种计算文本相似度的方法:

1.  **术语频率-逆文档频率(TF-idf)** : 这查看在两个文本中出现的单词，并根据它们出现的频率对它们进行评分。如果你希望两个文本中出现相同的单词，这是一个有用的工具，但是有些单词比其他的更重要。
2.  **语义相似度**:根据单词的相似程度来评分，即使它们并不完全匹配。它借用了自然语言处理(NLP)的技术，比如单词嵌入。如果文本之间的单词重叠是有限的，这是很有用的，例如，如果您需要将'*水果和蔬菜*'与'*西红柿*'联系起来。

对于第一部分，我们将单独使用`scikit-learn`中的 TF-idf 实现，因为它非常简单，只需要几行代码。对于语义相似性，我们将使用来自`gensim`的一些函数(包括它的 TF-idf 实现)和来自[手套算法](https://nlp.stanford.edu/projects/glove/)的预训练单词向量。此外，我们还需要一些来自`nltk`的工具。这些软件包可以使用 pip 安装:

```
pip install scikit-learn~=0.22
pip install gensim~=3.8
pip install nltk~=3.4
```

或者，您可以从 GitHub 存储库中获取示例代码，并从需求文件中安装:

```
git clone [https://github.com/4OH4/doc-similarity](https://github.com/4OH4/doc-similarity)cd doc-similaritypip install -r requirements.txt
```

[](https://github.com/4OH4/doc-similarity) [## 4oh 4/doc-相似性

### 使用 Python - 4OH4/doc-similarity 中的语义相似度对文档进行排序

github.com](https://github.com/4OH4/doc-similarity) 

## **快速入门**

1.  运行`examples.ipynb,`中的演示代码
2.  使用`tfidf.rank_documents(search_terms: str, documents: list)`功能根据重叠内容对文档进行评分，
3.  使用`docsim.DocSim()`类对使用`doc2vec`和`GloVe`单词嵌入模型的文档进行相似性评分。

## 术语

***文档*** :字符串形式的一段文本。这可能只是几句话，也可能是一整部小说。

***文集*** :文件的集合。

***术语*** :文档中的一个词。

# 1.TF-idf

*术语频率-逆文档频率*(或 TF-idf)是一种基于文档共享单词的重要性对文档相似性进行评分的既定技术。非常高层次的总结:

*   如果某个术语(单词)在文档中频繁出现，则该术语在该文档中可能很重要。
*   但是，如果一个术语在许多文档中频繁出现，则该术语通常可能不太重要。

TF-idf 分数根据一个单词出现的频率来估计这两种启发式方法之间的权衡。这里有更详细的[总结](http://www.tfidf.com/)。

我们可以用几行代码从`scikit-learn`中创建并装配一个 [TF-idf 矢量器模型](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html):

在这里，我们创建模型并使用文本语料库进行“拟合”。`TfidfVectorizer`使用其默认的记号赋予器处理预处理——这将字符串转换成单个单词“记号”的列表。它产生包含频率项的文档向量的稀疏矩阵。

然后，我们用文档的第一个向量(包含搜索项)的点积(线性核)来确定相似性。我们必须忽略第一个相似性结果(`[1:]`)，因为这是比较搜索词本身。

这对于一个简单的例子来说很有效，但是在现实世界中可能会因为一些原因而失败。

首先，在匹配过程中使用诸如['and '，' on '，' the '，' are']这样的词没有多大意义，因为这些*停用词*不包含上下文信息。在确定相似性之前，这些单词应该被剔除。

其次，我们希望“水果”和“水果”被认为是相关的，尽管上面的模型只能找到精确的匹配。解决这个问题的一个方法是将每个单词简化为最简单的 [*引理*](https://dictionary.cambridge.org/dictionary/english/lemma) — 为此我们需要一个*引理器*。

在这种情况下，第二个文档中的“tomatos”被 lemmatizer (tokenizer)简化为“tomato ”,然后与搜索词中的相同单词进行匹配。

从概念上讲，每个文档都是高维空间中的一个点，其中维度的数量等于文本语料库中唯一词的数量。这个空间通常是非常空的，因为文档包含各种各样的单词，所以点(文档)之间的距离很大。在这种情况下，点之间的角度作为相似性的度量比距离相关的度量(如欧几里德距离)更有意义。

这里的相似度被称为*余弦相似度*。`TfidfVectorizer`的输出(默认情况下)是 L2 归一化的，因此两个向量的点积是向量所表示的点之间角度的余弦。

## 摘要:TF-idf

*   当文档很大和/或有很多重叠时，它速度很快，效果很好。
*   它寻找精确的匹配，所以至少你应该使用一个 lemmatizer 来处理复数。
*   当比较短的文档和有限的术语种类时——比如搜索查询——有一个风险，就是在没有精确的单词匹配时，你会错过语义关系。

# 2.语义相似度

一种更高级的方法是根据单词的相似程度来比较文档。例如，“苹果”和“橙子”可能比“苹果”和“木星”更相似。大规模判断单词相似性是困难的——一种广泛使用的方法是分析大量文本，将经常一起出现的单词排序为更相似。

这是单词嵌入模型 [GloVe](https://nlp.stanford.edu/projects/glove/) 的基础:它将单词映射到数字向量——多维空间中的点，以便经常一起出现的单词在空间上彼此靠近。这是一种无监督学习算法，由斯坦福大学于 2014 年开发。

在前面的例子中，我们使用了来自`nltk`包的`WordNetLemmatizer`对我们的数据进行词干化和标记化(转换成单个单词串的`list`)。这里我们在`gensim`中做了类似的预处理，也删除了任何可能存在的 HTML 标签，比如我们从网上抓取了数据:

然后，我们创建一个相似性矩阵，其中包含每对单词之间的相似性，使用术语频率进行加权:

最后，我们计算查询和每个文档之间的软余弦相似度。与常规余弦相似度不同(对于没有重叠项的向量，它将返回零)，*软余弦相似度*也考虑单词相似度。

代码库中的笔记本中有完整的示例。还有一个自包含的`DocSim`类，可以作为模块导入，用于运行语义相似性查询，而无需额外的代码:

```
from docsim import DocSimdocsim = DocSim(verbose=True)similarities = docsim.similarity_query(query_string, documents)
```

GloVe word 嵌入模型可能非常大——在我的机器上，从磁盘加载大约需要 40 秒。然而，一旦它被加载，随后的操作会更快。该类的多线程版本在后台加载模型，以避免长时间锁定主线程。它以类似的方式使用，尽管如果模型仍在加载时会引发一个异常，所以应该首先检查`model_ready`属性的状态。

```
from docsim import DocSim_threadeddocsim = DocSim_threaded(verbose=True)similarities = docsim.similarity_query(query_string, documents)
```

## 概述:使用 GloVe 的语义相似性

*   它更加灵活，因为它不依赖于寻找精确的匹配。
*   涉及的计算要多得多，所以可能会慢一些，而且单词嵌入模型可能会相当大，需要一段时间来准备第一次使用。这很容易扩展，但是运行单个查询很慢。
*   大多数单词与其他单词有某种程度的相似性，因此几乎所有文档都会与其他文档有某种非零的相似性。**语义相似性有利于按顺序排列内容，而不是对文档是否与特定主题相关做出具体判断。**

我们已经研究了两种比较文本内容相似性的方法，例如可能用于搜索查询或内容推荐系统的方法。第一个(TF-idf)基于共有词的出现频率对文档关系进行评分。当文档很大和/或有很多重叠的术语时，这种方法速度很快，效果很好。第二种技术寻找表达相似概念的共有词，但不要求完全匹配:例如，它将“水果和蔬菜”与“番茄”联系起来。这种方法速度较慢，有时会产生不太清晰的结果，但对于较短的搜索查询或单词重叠度较低的文档很有用。

我在现实世界的应用程序中检查了这两种方法:我需要知道某个`<collection of documents>`是否与一小部分`<search terms>`相关。最初，我认为我需要使用语义相似性匹配——搜索词来自不受控制的用户输入，因此可能存在一些非常微妙的关系。文档排序是一个有用的功能，但总体目标是应用一个决策阈值，以便生成一个二进制的是/否结果。

对于那个特定的应用，我发现单独使用 TF-idf 就足够了。文档集合通常足够大，以至于在确实应该找到匹配的情况下，与搜索词有足够的重叠。以色列国防军工作队对正面和负面案例进行了更明确的区分，尽管它确实遗漏了一些也是相关的文件。

> 值得采取数据驱动的方法，看看什么最适合你的应用程序。

你有什么想法？你在工作中使用过这些工具吗，或者你喜欢不同的方法？在下面留下你的评论吧！

> [*鲁珀特·托马斯*](https://twitter.com/rupertthomas) *是一名技术顾问，专门研究机器学习、机器视觉和数据驱动产品。*[*@ Rupert Thomas*](https://twitter.com/rupertthomas)

# 参考

[Sci-kit 学习:tfidf 矢量器](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html)

[手套:单词表示的全局向量](https://nlp.stanford.edu/projects/glove/)

[Gensim:软余弦教程](https://github.com/RaRe-Technologies/gensim/blob/develop/docs/notebooks/soft_cosine_tutorial.ipynb)