# 主题建模的潜在狄利克雷分配解释:算法和 Python Scikit-Learn 实现

> 原文：<https://towardsdatascience.com/latent-dirichlet-allocation-for-topic-modelling-explained-algorithm-and-python-scikit-learn-c65a82e7304d?source=collection_archive---------17----------------------->

用于主题建模的潜在狄利克雷分配算法和 Python Scikit-Learn 实现。

**潜在狄利克雷分配**是一种无监督的机器学习形式，通常用于自然语言处理任务中的主题建模。对于这些类型的任务，它是一个非常受欢迎的模型，其背后的算法非常容易理解和使用。此外，Scikit-Learn 库有一个非常好的算法实现，所以在本文中，我们将关注使用潜在 Dirichlet 分配的**主题建模。**

![](img/148872c70c66ddc670b8ae32b6de17d2.png)

拉斐尔·沙勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 潜在狄利克雷分配概述

*   什么是主题建模
*   什么是无监督机器学习
*   潜在的狄利克雷分配应用
*   潜在狄利克雷分配算法
*   构建潜在的狄利克雷分配数据集
*   使用 Scikit-Learn 实现潜在的狄利克雷分配

# 什么是主题建模

**主题建模**是一项无监督的机器学习任务，我们试图发现能够描述一组文档的“抽象主题”。这意味着我们有一个文本集，我们试图找到单词和短语的模式，这些模式可以帮助我们[对文档](https://programmerbackpack.com/k-means-clustering-explained/)进行聚类，并按照“主题”对它们进行分组。

我把*主题*放在引号中，我称它们为抽象主题，因为这些不是显而易见的主题，我们不需要它们。我们假设相似的文档会有相似的单词和短语模式。

例如，假设我们有 100 个文本的集合。我们浏览了每一篇文章，发现其中有 10 篇包含“机器学习”、“训练”、“有监督的”、“无监督的”、“数据集”等词汇。我们可能不知道这些词是什么意思，也不在乎。

我们在这里只看到一种模式，即 10%的文章包含这些词，我们得出结论，它们应该包含在同一主题中。我们实际上不能命名主题，同样，这是不必要的。我们可以把这 10 篇文章归纳成同一个主题。当我们得到一个我们从未见过的新文本时，我们查看它，我们发现它包含这些单词中的一些，然后我们将能够说"*嘿，这与其他 10 篇文章属于同一类别！*

# 什么是无监督机器学习

**无监督机器学习**是一种机器学习模型，在这种模型中，我们试图在没有任何先验知识，也不知道我们是否正确的情况下推断数据模式。使用这种类型的模型，我们试图在数据中找到模式，然后我们可以使用它们来聚类、分类或描述我们的数据。

**潜在狄利克雷分配是一种无监督的机器学习。**我们在开始之前并不知道文档的主题，只能指定要找多少个主题。在解析的最后，我们可以查看结果并判断它们是否有帮助。

# 潜在的狄利克雷分配应用

**潜在狄利克雷分配主要用于主题建模。**现在我们可以考虑为什么我们需要主题建模。

通过主题建模，我们可以对一组文档进行聚类，从而将更多相似的文档分组在一起，将更少相似的文档放入不同的类别中。这可以用来分析和理解数据集。

我们还可以根据这种算法自动组织我们的文档，然后，当一个新文档出现在数据集中时，我们可以自动将其放入正确的类别中。

此外，这可以用于改进处理文本文档的应用程序中的文本搜索和文本相似性特征。

# 潜在狄利克雷分配算法

**潜在狄利克雷分配算法**只需几个简单的步骤就能工作。我们需要做的唯一预处理是我们在几乎所有的文本处理任务中所做的:从我们所有的文档中删除停用词(很可能在大多数文档中出现并且不会带来任何价值的词)。

1.  建立将由 LDA 算法识别的多个 *n* 主题。怎样才能找到完美的题目数量？嗯，这不是很容易，通常是一个反复试验的过程:我们尝试不同的值，直到我们对结果满意。或者，也许我们很幸运，我们有关于数据集的其他信息，允许我们建立完美的主题数量。
2.  将每个文档中的每个单词分配给一个临时主题。这个临时话题一开始会是随机的，但是下一步会更新。
3.  对于这一步，我们将遍历每个文档，然后遍历文档中的每个单词，并计算 2 个值

*   该文档属于某个主题的概率；这基于该文档中有多少单词(除了当前单词)属于当前单词的主题
*   因为当前单词而被分配给当前单词的主题的文档的比例。

我们将运行步骤 3 一定次数(在开始运行算法之前建立)。最后，我们将查看每个文档，根据单词找到最流行的主题，并将该文档分配给该主题。

# 构建潜在的狄利克雷分配数据集

有时，当我了解一个新概念时，我喜欢建立自己的小数据集，以便更快地学习。我喜欢这样有两个原因:

*   没有浪费时间清理数据。我知道这对于一个机器学习工程师或者数据科学家来说是非常重要的技能，但是这个话题不是这里的重点。如果我想学习一个算法，我会建立我自己的小而干净的数据集，让我可以玩它。
*   更快的试错过程:建立我自己的数据集将允许我使它足够大以提供结果，但又足够小以快速运行。

对于 LDA 算法，我将获得 6 个维基百科页面的摘要部分(2 个关于城市，2 个关于技术，2 个关于书籍),并将它们用作 LDA 算法要聚类的文档。然后，我将提供另一页的第 7 个摘要，并观察它是否被放在正确的类别中。

为了从维基百科中提取文本，我将使用[维基百科](https://pypi.org/project/wikipedia/) python 包。

```
pip3 install wikipedia
```

我将使用一个小类来下载摘要。

```
import wikipedia

class TextFetcher:

    def __init__(self, title):
        self.title = title
        page = wikipedia.page(title)
        self.text = page.summary

    def getText(self):
        return self.text
```

然后我可以用这个类提取数据。正如我前面提到的，唯一需要做的预处理是删除停用词。为此，我将使用 [nltk 包。](https://pypi.org/project/nltk/)

```
def preprocessor(text):
    nltk.download('stopwords')
    tokens = word_tokenize(text)
    return (" ").join([word for word in tokens if word not in stopwords.words()])

if __name__ == "__main__":

    textFetcher = TextFetcher("London")
    text1 = preprocessor(textFetcher.getText())
    textFetcher = TextFetcher("Natural Language Processing")
    text2 = preprocessor(textFetcher.getText())
    textFetcher = TextFetcher("The Great Gatsby")
    text3 = preprocessor(textFetcher.getText())
    textFetcher = TextFetcher("Machine Learning")
    text4 = preprocessor(textFetcher.getText())
    textFetcher = TextFetcher("Berlin")
    text5 = preprocessor(textFetcher.getText())
    textFetcher = TextFetcher("For Whom the Bell Tolls")
    text6 = preprocessor(textFetcher.getText())

    docs = [text1, text2, text3, text4, text5, text6]
```

既然我们已经准备好了数据集，我们就可以继续算法实现了。

# 使用 Scikit-Learn 实现潜在的狄利克雷分配

scikit-learn 包很好地实现了 LDA 算法。我们今天要用这个。

```
pip3 install scikit-learn
```

# 单词矢量化

第一步是把我们的单词转换成数字。虽然我们正在与文字打交道，但许多文本处理任务都是用数字来完成的，因为它们对计算机来说更容易理解。

scikit-learn 包中的 *CountVectorizer* 类可以将一个单词转换成实数向量。让我们用我们的数据集来做这件事。

```
countVectorizer = CountVectorizer(stop_words='english')
    termFrequency = countVectorizer.fit_transform(docs)
    featureNames = countVectorizer.get_feature_names()
```

现在，让我们将潜在的狄利克雷分配算法应用于我们的单词向量，并打印出我们的结果。对于每个主题，我们将打印前 10 个单词。

```
lda = LatentDirichletAllocation(n_components=3)
    lda.fit(termFrequency) for idx, topic in enumerate(lda.components_):
        print ("Topic ", idx, " ".join(featureNames[i] for i in topic.argsort()[:-10 - 1:-1]))
```

结果是这样的:

```
Topic  0 berlin city capital german learning machine natural germany world data
Topic  1 novel fitzgerald gatsby great american published war book following considered
Topic  2 london city largest world europe populous area college westminster square
```

正如我们之前所讨论的，这些信息可能不会告诉你太多，但它足以让我们正确地分类一个关于巴黎的新文本(在我们再次矢量化这个文本之后)。

```
text7 = preprocessor(TextFetcher("Paris").getText())
    print (lda.transform(countVectorizer.transform([text7])))
```

结果是这样的:

```
[[0.17424998 0.10191793 0.72383209]]
```

这 3 个是我们的文本属于 LDA 算法生成的 3 个主题之一的概率。我们可以看到，最高概率(72%)告诉我们，这段文字也应该属于第 3 个题目，所以在同一个讲城市的题目里。我们可以看到，这是一个从非常小的数据集获得的非常好的结果。

# 结束语

在本文中，我们讨论了潜在的狄利克雷分配算法的一般概述。然后，我们建立了一个自己的小数据集，并测试了算法。我对这个结果非常满意，希望你也是。

*本文原载于* [*程序员背包博客*](https://programmerbackpack.com/latent-dirichlet-allocation-for-topic-modelling-explained-algorithm-and-python-scikit-learn-implementation/) *。如果你想阅读更多这类的故事，一定要访问这个博客。*

*非常感谢您阅读本文！对更多这样的故事感兴趣？在 Twitter 上关注我，地址是*[*@ b _ dmarius*](https://twitter.com/b_dmarius)*，我会在那里发布每一篇新文章。*