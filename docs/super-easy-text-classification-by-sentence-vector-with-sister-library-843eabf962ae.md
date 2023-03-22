# 超级简单的文本分类句子向量与姐妹(库)

> 原文：<https://towardsdatascience.com/super-easy-text-classification-by-sentence-vector-with-sister-library-843eabf962ae?source=collection_archive---------39----------------------->

![](img/2acd793d478c8535ed54f9c7532ee56b.png)

照片由[阿尔方斯·莫拉莱斯](https://unsplash.com/@alfonsmc10?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

在[之前的帖子](/super-easy-way-to-get-sentence-embedding-using-fasttext-in-python-a70f34ac5b7c)中，我介绍了一个我写的库——[姐姐](https://github.com/tofunlp/sister)，它可以很容易地将一个句子转换成一个向量。我们看到了用很少的代码就能做到这一点是多么简单。

然而，获得一个向量只是创建一个 ML 增强应用程序的第一步，我们需要在它上面放一个模型来做一些很酷的事情。

在本帖中，我们将通过几个简单的步骤，利用句子向量创建文本分类。

为了向您展示这样做有多简单，我将把整个代码放在前面。

就这些了。包括空行，也就 27 行！！！

这个程序主要做四件事。

1.  加载数据集。
2.  将每个句子转换成一个向量。
3.  训练一个模特。
4.  评估。

让我一步一步地解释它们

# 加载数据集

主函数中的前几行，

```
train = datasets.Imdb("train")test = datasets.Imdb("test")train_texts, train_labels = zip(*train.all())test_texts, test_labels = zip(*test.all())
```

正在加载一个著名的文本分类数据集，IMDb 数据集。这个数据集由电影评论和相应的情感标签组成。它有一个正面的标签—如果评论是正面的，则为 1。这意味着数据集中的所有样本都属于两个类中的一个，因此这是一个二元分类问题。

为了加载数据集，我使用了一个轻量级的 NLP 数据加载器库 [LineFlow](https://github.com/tofunlp/lineflow) 。要了解更多这方面的信息，我建议你参考[开发者写的一篇中型文章](/lineflow-introduction-1caf7851125e)。

# 将句子转换成向量

现在酷的部分，通过使用[姐妹](https://github.com/tofunlp/sister)，我们将获得电影评论的数字表示。

```
sentence_embedding = sister.MeanEmbedding("en")train_x = np.array([sentence_embedding(t) for t in train_texts])test_x = np.array([sentence_embedding(t) for t in test_texts])
```

在第一行中，我们创建了一个类的实例，它获取一个句子，并将其中的单词转换成单词向量，最后是句子向量。想知道它是如何从一个单词向量序列中获取一个句子向量的，请参考[我之前的文章](/super-easy-way-to-get-sentence-embedding-using-fasttext-in-python-a70f34ac5b7c)。

# 训练模特

现在，我们已经为构建 ML 模型准备好了句子。为此，我将使用机器学习库之王， [scikit-learn](https://scikit-learn.org/) 。通过使用这个，我们可以只用两行代码训练一个[支持向量机](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html#sklearn.svm.SVC)。

```
clf = SVC(kernel="linear")clf.fit(train_x, train_labels)
```

# 评价

最后，我们可以用测试数据来测试模型训练得有多好。

```
print(clf.score(test_x, test_labels))
```

通过使用 scikit-learn，这也可以非常简单地完成。在我的本地机器上，它的准确率超过 80%。这不是一个很好的结果，但却是一个很好的起点。

在本文中，我们看到了如何构建一个以句子向量作为输入的 SVM 文本分类模型，并且我们用不到 30 行代码就实现了它。

我将以整个程序的 GitHub 要点 T4 来结束这篇文章。

感谢您的阅读！