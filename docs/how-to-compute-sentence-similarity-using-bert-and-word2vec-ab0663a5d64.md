# 如何使用 BERT 和 Word2Vec 计算句子相似度

> 原文：<https://towardsdatascience.com/how-to-compute-sentence-similarity-using-bert-and-word2vec-ab0663a5d64?source=collection_archive---------6----------------------->

## NLP-生产中

## 以及防止计算句子嵌入时常见错误的一些见解

![](img/d23ecb3afaf7fa56a4fba33b6e4188df.png)

卡蒂亚·奥斯丁在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我们经常需要将文本数据，包括单词、句子或文档编码成高维向量。句子嵌入是各种自然语言处理任务中的一个重要步骤，例如情感分析和文摘。**需要一个灵活的句子嵌入库来快速原型化，并针对各种上下文进行调整。**

过去，我们大多使用 one-hot、term-frequency 或 TF-IDF(也称为归一化 term-frequency)等编码器。然而，在这些技术中没有捕获单词的语义和句法信息。最近的进步使我们能够以更有意义的形式对句子或单词进行编码。word2vec 技术和 BERT 语言模型是其中两个重要的技术。注意，在这个上下文中，我们交替使用嵌入、编码或矢量化。

开源的 [sent2vec](https://github.com/pdrm83/Sent2Vec) Python 库可以让你高度灵活地编码句子。您目前可以访问库中的标准编码器。更高级的技术将在以后的版本中添加。在本文中，我想介绍这个库，并分享我在这方面学到的经验。

如果您不熟悉 Word2Vec 模型，我推荐您先阅读下面的文章。**你会发现为什么 Word2Vec 模型在机器学习中既简单又具有革命性。**

[](/word2vec-models-are-simple-yet-revolutionary-de1fef544b87) [## Word2Vec 模型简单而具有革命性

### Gensim 还是 spaCy？不了解 Word2Vec 机型的基础知识也没关系。

towardsdatascience.com](/word2vec-models-are-simple-yet-revolutionary-de1fef544b87) 

# —如何使用“sent 2 vec”Python 包

## 如何安装

由于 [sent2vec](https://github.com/pdrm83/Sent2Vec) 是一个高级库，它依赖于 [spaCy](https://spacy.io/) (用于文本清理) [Gensim](https://radimrehurek.com/gensim/) (用于 word2vec 模型) [Transformers](https://huggingface.co/transformers/) (用于各种形式的 BERT 模型)。因此，确保在使用下面的代码安装 sent2vec 之前安装这些库。

```
pip3 install sent2vec
```

## 如何使用伯特方法

如果要使用`BERT`语言模型(更确切地说是`distilbert-base-uncased`)为下游应用程序编码句子，必须使用下面的代码。目前， [sent2vec](https://github.com/pdrm83/Sent2Vec) 库只支持 DistilBERT 模型。未来将支持更多型号。由于这是一个开源项目，您还可以深入研究源代码，找到更多的实现细节。

[**sent 2 vec**](https://github.com/pdrm83/Sent2Vec)**——**如何使用 BERT 计算句子嵌入

你可以用句子的向量来计算句子之间的距离。在示例中，正如所料，`vectors[0]`和`vectors[1]`之间的距离小于`vectors[0]`和`vectors[2]`之间的距离。

注意，默认的矢量器是`distilbert-base-uncased`，但是可以传递参数`pretrained_weights`来选择另一个`BERT`模型。例如，您可以使用下面的代码来加载基本的多语言模型。

```
vectorizer = Vectorizer(pretrained_weights='distilbert-base-multilingual-cased')
```

## 如何使用 Word2Vec 方法

如果您想使用 Word2Vec 方法，您必须向模型权重传递一个有效的路径。在引擎盖下，使用来自`Splitter`类的`sent2words`方法将句子拆分成单词列表。该库首先提取句子中最重要的单词。然后，它使用与这些单词相对应的向量的平均值来计算句子嵌入。你可以使用下面的代码。

[**sent 2 vec**](https://github.com/pdrm83/Sent2Vec)**—**如何使用`word2vec`计算句子嵌入

可以通过在默认列表中添加或删除来自定义停用词列表。当调用矢量器的方法`.run`时，必须传递两个附加参数(两个列表):`remove_stop_words`和`add_stop_words`。在进行任何计算之前，研究停用词表是至关重要的。在这一步中稍有变化，最终结果很容易出现偏差。

请注意，您可以使用预先训练的模型或定制的模型。这对于获得有意义的结果至关重要。你需要一个上下文化的向量化，Word2Vec 模型可以解决这个问题。你只需要在初始化矢量器类的时候，把路径发送给 Word2Vec 模型(即`PRETRAINED_VECTORS_PATH`)。

# —什么是最好的句子编码器

句子编码或嵌入技术的最终结果植根于各种因素，如相关的停用词表或语境化的预训练模型。你可以在下面找到更多的解释。

*   **文本清理—** 假设您将 spaCy 用于文本清理步骤，因为我也在 sent2vec 库中使用了它。如果您错误地忘记从默认停用词表中删除“Not ”,那么句子嵌入结果可能会完全误导。一个简单的“不”字，就能彻底改变一句话的情调。每个环境中的默认停用词表都不同。因此，在进行任何计算之前，您必须根据您的需求来筛选这个列表。
*   **情境化模型—** 你必须使用情境化模型。例如，如果目标数据是金融数据，则必须使用在金融语料库上训练的模型。否则，句子嵌入的结果可能不准确。所以，如果用`word2vec`的方法，想用一般的英文模型，句子嵌入结果可能会不准确。
*   **聚合策略—** 当您使用`word2vec`方法计算句子嵌入时，您可能需要使用更高级的技术来聚合单词向量，而不是取它们的平均值。目前，sent2vec 库只支持“平均”技术。使用加权平均来计算句子嵌入，是一种可以改善最终结果的简单增强。未来的版本将支持更高级的技术。

为了强调 word2vec 模型的重要性，我使用两种不同的 word2vec 模型对一个句子进行编码(即`glove-wiki-gigaword-300`和`fasttext-wiki-news-subwords-300`)。然后，我计算两个向量之间的余弦相似度:`0.005`这可能解释为“两个独特的句子非常不同”。不对！通过这个例子，我想证明如果我们使用两个不同的 word2vec 模型，句子的向量表示甚至可以是垂直的。换句话说，**如果你用一个随机的 word2vec 模型来盲目计算句子嵌入，你可能会在过程中大吃一惊。**

# — Sent2Vec 是一个开源库，所以…

sent2vec 是一个开源库。这个项目的主要目标是**加快 NLP 项目中概念验证的构建**。大量的自然语言处理任务需要句子矢量化，包括摘要和情感分析。所以，请考虑贡献力量，推动[这个项目](https://github.com/pdrm83/Sent2Vec)向前发展。我也希望你能在你激动人心的 NLP 项目中使用这个库。

[](https://github.com/pdrm83/Sent2Vec) [## pdrm83/Sent2Vec

### 在过去，我们主要使用，例如，一个热点，术语频率，或 TF-IDF(标准化术语…

github.com](https://github.com/pdrm83/Sent2Vec) 

# 感谢阅读！

如果你喜欢这个帖子，想支持我…

*   *跟我上* [*中*](https://medium.com/@pedram-ataee) *！*
*   *在* [*亚马逊*](https://www.amazon.com/Pedram-Ataee/e/B08D6J3WNW) *上查看我的书！*
*   *成为* [*中的一员*](https://pedram-ataee.medium.com/membership) *！*
*   *连接上*[*Linkedin*](https://www.linkedin.com/in/pedrama/)*！*
*   *关注我* [*推特*](https://twitter.com/pedram_ataee) *！*

[](https://pedram-ataee.medium.com/membership) [## 通过我的推荐链接加入 Medium—Pedram Ataee 博士

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

pedram-ataee.medium.com](https://pedram-ataee.medium.com/membership)