# 用 NLTK 进行文本预处理

> 原文：<https://towardsdatascience.com/nlp-preprocessing-with-nltk-3c04ee00edc0?source=collection_archive---------8----------------------->

## 自然语言处理的常用预处理方法

![](img/550025b343bedf37e2ed8f9c1c27f369.png)

照片由[卡洛斯·穆扎](https://unsplash.com/@kmuza?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 介绍

几乎每个 [**自然语言处理(NLP)**](https://en.wikipedia.org/wiki/Natural_language_processing) 任务都需要在训练模型之前对文本进行预处理。深度学习模型不能直接使用原始文本，因此需要我们研究人员自己清理文本。根据任务的性质，预处理方法可以不同。本教程将使用 [**NLTK(自然语言工具包)**](https://www.nltk.org/) 教授最常见的预处理方法，以适应各种 NLP 任务。

为什么是 NLTK？

*   **流行度** : NLTK 是处理语言数据的领先平台之一。
*   **简单性**:为多种文本预处理方法提供易于使用的 API
*   社区:它有一个庞大而活跃的社区来支持和改进图书馆
*   **开源**:免费开源，适用于 Windows、Mac OSX 和 Linux。

现在你知道了 NLTK 的好处，让我们开始吧！

# 教程概述

1.  小写字母
2.  删除标点符号
3.  标记化
4.  停用词过滤
5.  堵塞物
6.  词性标注器

本教程中显示的所有代码都可以在[我的 Github repo](https://github.com/itsuncheng/text_preprocessing_with_NLTK) 中访问。

# 导入 NLTK

在预处理之前，我们需要先下载 [NLTK 库](https://www.nltk.org/install.html)。

```
pip install nltk
```

然后，我们可以在 Python 笔记本中导入这个库，并下载它的内容。

# 小写字母

举个例子，我们从《傲慢与偏见》一书中抓取第一句话作为正文。我们通过`text.lower()`将句子转换成小写。

# 删除标点符号

要去掉标点符号，我们只保存不是标点符号的字符，可以用`[string.punctuation](https://docs.python.org/3/library/string.html)`来检查。

# 标记化

可以通过`[nltk.word_tokenize](https://www.nltk.org/api/nltk.tokenize.html)`将字符串标记成标记。

# 停用词过滤

我们可以使用`[nltk.corpus.stopwords.words(‘english’)](https://www.nltk.org/book/ch02.html)`来获取英语词典中的停用词列表。然后，我们删除停用词。

# 堵塞物

我们使用`[nltk.stem.porter.PorterStemmer](https://www.nltk.org/_modules/nltk/stem/porter.html)`对标记进行词干处理，以获得经过词干处理的标记。

# POS 标签

最后，我们可以使用`[nltk.pos_tag](https://www.nltk.org/book/ch05.html)`来检索列表中每个单词的词性。

完整的笔记本可以在这里看到[。](https://github.com/itsuncheng/text_preprocessing_with_NLTK/blob/master/preprocessing.ipynb)

# 结合在一起

我们可以结合上面所有的预处理方法，创建一个接受一个*的`preprocess`函数。txt* 文件并处理所有的预处理。我们打印出标记、过滤的单词(在停用词过滤之后)、词干单词和词性，其中一个通常被传递给模型或用于进一步处理。我们使用*傲慢与偏见*这本书(此处可访问[)并对其进行预处理。](https://www.gutenberg.org/files/1342/1342-0.txt)

此笔记本可在处[访问。](https://github.com/itsuncheng/text_preprocessing_with_NLTK/blob/master/pride_preprocessing.ipynb)

# 结论

对于任何 NLP 应用程序来说，文本预处理都是重要的第一步。在本教程中，我们讨论了几种流行的使用 NLTK 的预处理方法:小写、删除标点、标记化、停用词过滤、词干提取和词性标记。