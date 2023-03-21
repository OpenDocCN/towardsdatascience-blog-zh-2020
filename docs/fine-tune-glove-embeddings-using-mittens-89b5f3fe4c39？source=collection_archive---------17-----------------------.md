# 使用连指手套微调手套嵌入

> 原文：<https://towardsdatascience.com/fine-tune-glove-embeddings-using-mittens-89b5f3fe4c39?source=collection_archive---------17----------------------->

![](img/a9cd686a8d8488e2bf1d1401b97d11aa.png)

[亚历克斯](https://unsplash.com/@worthyofelegance?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/mittens?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

2013 年后，单词嵌入甚至在 NLP 社区之外也变得非常流行。 [Word2vec](http://jalammar.github.io/illustrated-word2vec/) 和 [GloVe](https://nlp.stanford.edu/projects/glove/) 属于静态单词嵌入家族。然后是一系列的动态嵌入伯特，ELMO，罗伯塔，阿尔伯特，XLNET..所有这些嵌入都依赖于语境词。在这篇文章中，让我们看看如何微调静态嵌入。

您是否曾经遇到过这样的情况:当您拥有一个非常小的数据集，并且想要应用静态单词嵌入时，却面临以下问题:

*   预训练模型中不存在数据集词汇
*   无法从数据集中训练整个模型，因为它太小

解决方案是加载预先训练好的模型，并使用来自数据集的新数据对它们进行微调，这样，看不见的词汇也被添加到模型中。

**为什么不能微调 word2vec:**

[Gensim](https://radimrehurek.com/gensim/) 是 word2vec 使用最多的库，微调这些嵌入有一些问题。新数据集中词汇的嵌入将在不对旧嵌入进行任何改变的情况下被训练。这导致预训练嵌入和新嵌入之间的差异。

[fasttext](https://fasttext.cc/) 也不提供微调功能。

# 微调手套

[Mittens](https://github.com/roamanalytics/mittens) 是一个用于微调手套嵌入的 python 库。这个过程包含 3 个简单的步骤。加载预训练的模型，建立新数据集的共生矩阵，并训练新的嵌入。

***加载预训练模型***

Mittens 需要将预训练的模型作为字典加载。所以，让我们做同样的事情。从[https://nlp.stanford.edu/projects/glove](https://nlp.stanford.edu/projects/glove)获得预训练模型

```
def glove2dict(glove_filename):
    with open(glove_filename, encoding='utf-8') as f:
        reader = csv.reader(f, delimiter=' ',quoting=csv.QUOTE_NONE)
        embed = {line[0]: np.array(list(map(float, line[1:])))
                for line in reader}
    return embedglove_path = "glove.6B.50d.txt"
pre_glove = glove2dict(glove_path)
```

***数据预处理***

在构建单词的共现矩阵之前，让我们对数据集做一些预处理。

```
sw = list(stop_words.ENGLISH_STOP_WORDS)
brown_data = brown.words()[:200000]
brown_nonstop = [token.lower() for token in brown_data if (token.lower() not in sw)]
oov = [token for token in brown_nonstop if token not in pre_glove.keys()]
```

我们已经使用布朗语料库作为样本数据集，并且 *oov* 表示未在预训练手套中出现的词汇。共生矩阵是从 *oov* s 构建的。它是一个稀疏矩阵，需要 O(n^2).的空间复杂度因此，有时为了节省空间，必须过滤掉真正罕见的单词。这是一个可选步骤。

```
def get_rareoov(xdict, val):
    return [k for (k,v) in Counter(xdict).items() if v<=val]oov_rare = get_rareoov(oov, 1)
corp_vocab = list(set(oov) - set(oov_rare))
```

如果需要，删除那些罕见的*oov*，并准备数据集

```
brown_tokens = [token for token in brown_nonstop if token not in oov_rare]
brown_doc = [' '.join(brown_tokens)]
corp_vocab = list(set(oov))
```

***构建共生矩阵:***

我们需要单词-单词共现，而不是通常的术语-文档矩阵。sklearn 的 *CountVectorizer* 将文档转化为 word-doc 矩阵。矩阵乘法`Xt*X`给出单词-单词共现矩阵。

```
cv = CountVectorizer(ngram_range=(1,1), vocabulary=corp_vocab)
X = cv.fit_transform(brown_doc)
Xc = (X.T * X)
Xc.setdiag(0)
coocc_ar = Xc.toarray()
```

***微调连指手套型号***

要安装连指手套，请尝试`pip install -U mittens`查看[完整文档](https://github.com/roamanalytics/mittens)了解更多信息。只需实例化模型并运行 fit 函数。

```
mittens_model = Mittens(n=50, max_iter=1000)
new_embeddings = mittens_model.fit(
    coocc_ar,
    vocab=corp_vocab,
    initial_embedding_dict= pre_glove)
```

将模型保存为 pickle 供将来使用。

```
newglove = dict(zip(corp_vocab, new_embeddings))
f = open("repo_glove.pkl","wb")
pickle.dump(newglove, f)
f.close()
```

这是完整的代码。

感谢您阅读这篇文章。欢迎通过 [Github](https://github.com/chmodsss) 、 [Twitter](https://twitter.com/chmodsss) 和 [Linkedin](https://www.linkedin.com/in/sivasuryas/) 联系我。干杯！

*来源:* 1。*[https://github.com/roamanalytics/mittens](https://github.com/roamanalytics/mittens)
2。[https://surancy . github . io/co-occurrence-matrix-visualization](https://surancy.github.io/co-occurrence-matrix-visualization/)*