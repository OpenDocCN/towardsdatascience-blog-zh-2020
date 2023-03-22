# 语言处理的文本表示介绍—第 1 部分

> 原文：<https://towardsdatascience.com/introduction-to-text-representations-for-language-processing-part-1-dc6e8068b8a4?source=collection_archive---------13----------------------->

![](img/d4572fbe3ad47896132e8e9b3716c3ae.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Jaredd Craig](https://unsplash.com/@jaredd_craig?utm_source=medium&utm_medium=referral) 拍照

## 计算机是如何理解和解释语言的？

计算机在处理数字时很聪明。它们在计算和解码模式方面比人类快很多个数量级。但是如果数据不是数字呢？如果是语言呢？当数据是字符、单词和句子时会发生什么？我们如何让计算机处理我们的语言？Alexa、Google Home &很多其他智能助手是如何理解&回复我们的发言的？如果你正在寻找这些问题的答案，这篇文章将是你走向正确方向的垫脚石。

自然语言处理是人工智能的一个子领域，致力于使机器理解和处理人类语言。大多数自然语言处理(NLP)任务的最基本步骤是将单词转换成数字，以便机器理解和解码语言中的模式。我们称这一步为文本表示。这一步虽然是迭代的，但在决定机器学习模型/算法的特征方面起着重要作用。

文本表示可以大致分为两部分:

*   离散文本表示
*   分布式/连续文本表示

本文将关注离散文本表示&我们将深入研究一些基本 Sklearn 实现中常用的表示。

# 离散文本表示:

这些表示中，单词由它们在字典中的位置的对应索引来表示，该索引来自更大的语料库。

属于这一类别的著名代表有:

*   一键编码
*   词袋表示法(BOW)
*   基本 BOW 计数矢量器
*   先进弓-TF-IDF

# 一键编码:

这是一种将 0 赋给向量中所有元素的表示形式，只有一个元素的值为 1。该值表示元素的类别。

例如:

如果我有一个句子，“我爱我的狗”，句子中的每个单词将表示如下:

```
I → [1 0 0 0], love → [0 1 0 0], my → [0 0 1 0], dog → [0 0 0 1]
```

然后，整个句子表示为:

```
sentence = [ [1,0,0,0],[0,1,0,0],[0,0,1,0],[0,0,0,1] ]
```

> *一键编码背后的直觉是每个比特代表一个可能的类别&如果一个特定的变量不能归入多个类别，那么一个比特就足以代表它*

正如您可能已经理解的，单词数组的长度取决于词汇量。这对于可能包含多达 100，000 个唯一单词甚至更多的非常大的语料库来说是不可扩展的。

现在让我们使用 Sklearn 来实现它:

```
from sklearn.preprocessing import OneHotEncoder
import itertools# two example documents
docs = ["cat","dog","bat","ate"]# split documents to tokens
tokens_docs = [doc.split(" ") for doc in docs]# convert list of of token-lists to one flat list of tokens
# and then create a dictionary that maps word to id of word,
all_tokens = itertools.chain.from_iterable(tokens_docs)
word_to_id = {token: idx for idx, token in enumerate(set(all_tokens))}# convert token lists to token-id lists
token_ids = [[word_to_id[token] for token in tokens_doc] for tokens_doc in tokens_docs]# convert list of token-id lists to one-hot representation
vec = OneHotEncoder(categories="auto")
X = vec.fit_transform(token_ids)print(X.toarray())
```

# 输出:

```
[[0\. 0\. 1\. 0.]
 [0\. 1\. 0\. 0.]
 [0\. 0\. 0\. 1.]
 [1\. 0\. 0\. 0.]]
```

# Sklearn 文档:

[](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html) [## sk learn . preprocessing . onehotencoder-sci kit-learn 0 . 23 . 1 文档

### 将分类特征编码为一个独热数值数组。这个转换器的输入应该是一个类似数组的…

scikit-learn.org](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html) 

# 一键编码的优势:

*   易于理解和实施

# 一键编码的缺点:

*   如果类别数量非常多，特征空间会爆炸
*   单词的矢量表示是正交的，并且不能确定或测量不同单词之间的关系
*   无法衡量一个单词在句子中的重要性，但可以理解一个单词在句子中是否存在
*   高维稀疏矩阵表示可能是存储器和计算昂贵的

# 词袋表示法

顾名思义，单词包表示法将单词放在一个“包”中，并计算每个单词的出现频率。它不考虑文本表示的词序或词汇信息

> *BOW 表示背后的直觉是，具有相似单词的文档是相似的，而不管单词的位置如何*

# 基本 BOW 计数矢量器

CountVectorizer 计算一个单词在文档中出现的频率。它将多个句子的语料库(比如产品评论)转换成评论和单词的矩阵，并用每个单词在句子中的出现频率填充它

让我们看看如何使用 Sklearn 计数矢量器:

```
from sklearn.feature_extraction.text import CountVectorizertext = ["i love nlp. nlp is so cool"]vectorizer = CountVectorizer()# tokenize and build vocab
vectorizer.fit(text)
print(vectorizer.vocabulary_)
# Output: {'love': 2, 'nlp': 3, 'is': 1, 'so': 4, 'cool': 0}# encode document
vector = vectorizer.transform(text)# summarize encoded vector
print(vector.shape) # Output: (1, 5)
print(vector.toarray())
```

# 输出:

```
[[1 1 1 2 1]]
```

如你所见，单词“nlp”在句子中出现了两次&也在索引 3 中。我们可以将其视为最终打印语句的输出

> *一个词在句子中的“权重”是它的出现频率*

作为 CountVectorizer 的一部分，可以调整各种参数来获得所需的结果，包括小写、strp_accents、预处理器等文本预处理参数

可以在下面的 Sklearn 文档中找到完整的参数列表:

[](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html) [## sk learn . feature _ extraction . text . count vectorizer-sci kit-learn 0 . 23 . 1 文档

### class sk learn . feature _ extraction . text . count vectorizer(*，input='content '，encoding='utf-8 '，decode_error='strict'…

scikit-learn.org](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html) 

# CountVectorizer 的优势:

*   CountVectorizer 还能给出文本文档/句子中单词的频率，这是一键编码所不能提供的
*   编码向量的长度就是字典的长度

# CountVectorizer 的缺点:

*   此方法忽略单词的位置信息。从这种表述中不可能领会一个词的意思
*   当遇到像“is，The，an，I”这样的停用词时&当语料库是特定于上下文的时，高频词更重要或提供关于句子的更多信息的直觉失效了。例如，在一个关于新冠肺炎的语料库中，冠状病毒这个词可能不会增加很多价值

# 高级弓

为了抑制非常高频率的单词&忽略低频率的单词，需要相应地标准化单词的“权重”

**TF-IDF 表示:**TF-IDF 的完整形式是术语频率-逆文档频率是 2 个因子的乘积

![](img/3fd6d7d83cd000c097f21a5e33997b7f.png)

其中，TF(w，d)是单词“w”在文档“d”中的频率

IDF(w)可以进一步细分为:

![](img/c1fa34d12c15a2e70abcbef99e390b0b.png)

其中，N 是文档总数，df(w)是包含单词“w”的文档的频率

> TF-IDF 背后的直觉是，分配给每个单词的权重不仅取决于单词频率，还取决于特定单词在整个语料库中的出现频率

它采用上一节中讨论的 CountVectorizer 并将其乘以 IDF 分数。对于非常高频率的词(如停用词)和非常低频率的词(噪声项)，从该过程得到的词的输出权重很低

现在让我们尝试使用 Sklearn 来实现这一点

```
from sklearn.feature_extraction.text import TfidfVectorizertext1 = ['i love nlp', 'nlp is so cool', 
'nlp is all about helping machines process language', 
'this tutorial is on baisc nlp technique']tf = TfidfVectorizer()
txt_fitted = tf.fit(text1)
txt_transformed = txt_fitted.transform(text1)
print ("The text: ", text1)# Output: The text:  ['i love nlp', 'nlp is so cool', 
# 'nlp is all about helping machines process language', 
# 'this tutorial is on basic nlp technique']idf = tf.idf_
print(dict(zip(txt_fitted.get_feature_names(), idf)))
```

# 输出:

```
{'about': 1.916290731874155, 'all': 1.916290731874155, 
'basic': 1.916290731874155, 'cool': 1.916290731874155, 
'helping': 1.916290731874155, 'is': 1.2231435513142097, 
'language': 1.916290731874155, 'love': 1.916290731874155, 
'machines': 1.916290731874155, 'nlp': 1.0, 'on': 1.916290731874155, 
'process': 1.916290731874155, 'so': 1.916290731874155, 
'technique': 1.916290731874155, 'this': 1.916290731874155, 
'tutorial': 1.916290731874155}
```

注意单词“nlp”的权重。因为它出现在所有的句子中，所以它被赋予 1.0 的低权重。同样，停用词“is”的权重也相对较低，为 1.22，因为它在给出的 4 个句子中有 3 个出现。

与 CountVectorizer 类似，可以调整各种参数来获得所需的结果。一些重要的参数(除了像小写、strip_accent、stop_words 等文本预处理参数。)是 max_df，min_df，norm，ngram_range & sublinear_tf。这些参数对输出权重的影响超出了本文的范围，将单独讨论。

您可以在下面找到 TF-IDF 矢量器的完整文档:

[](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html) [## sk learn . feature _ extraction . text . tfidf vectorizer-sci kit-learn 0 . 23 . 1 文档

### class sk learn . feature _ extraction . text . tfidf vectorizer(*，input='content '，encoding='utf-8 '，decode_error='strict'…

scikit-learn.org](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html) 

# TF-IDF 代表的优势:

*   简单、易于理解和解释的实施
*   构建 CountVectorizer 来惩罚语料库中的高频词和低频词。在某种程度上，IDF 降低了我们矩阵中的噪声。

# TF-IDF 代表的缺点:

*   这个单词的位置信息仍然没有在这个表示中被捕获
*   TF-IDF 高度依赖语料库。由板球数据生成的矩阵表示不能用于足球或排球。因此，需要有高质量的训练数据

# 让我们总结一下

离散表示是每个单词都被认为是唯一的&根据我们上面讨论的各种技术转换成数字。我们已经看到了各种离散表示的一些重叠的优点和缺点，让我们将其作为一个整体来总结

# 离散表示的优点:

*   易于理解、实施和解释的简单表示
*   像 TF-IDF 这样的算法可以用来过滤掉不常见和不相关的单词，从而帮助模型更快地训练和收敛

# 离散表示的缺点:

*   这种表现与词汇量成正比。词汇量大会导致记忆受限
*   它没有利用单词之间的共现统计。它假设所有的单词都是相互独立的
*   这导致具有很少非零值的高度稀疏的向量
*   它们没有抓住单词的上下文或语义。它不认为幽灵和恐怖是相似的，而是两个独立的术语，它们之间没有共性

离散表示广泛用于经典的机器学习和深度学习应用，以解决复杂的用例，如文档相似性、情感分类、垃圾邮件分类和主题建模等。

在下一部分中，我们将讨论文本的分布式或连续文本表示&它如何优于(或劣于)离散表示。

希望你喜欢这篇文章。回头见！

编辑:该系列的第二部分:[https://medium . com/@ sundareshchandran/introduction-to-text-representations-for-language-processing-Part-2-54fe 6907868](https://medium.com/@sundareshchandran/introduction-to-text-representations-for-language-processing-part-2-54fe6907868)

# 回购链接

[](https://github.com/SundareshPrasanna/Introduction-to-text-representation-for-nlp/tree/master) [## SundareshPrasanna/自然语言处理的文本表示介绍

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/SundareshPrasanna/Introduction-to-text-representation-for-nlp/tree/master) 

喜欢我的文章？给我买杯咖啡

 [## sundaresh 正在创作与数据科学相关的文章，并且热爱教学

### 嘿👋我刚刚在这里创建了一个页面。你现在可以给我买杯咖啡了！

www.buymeacoffee.com](https://www.buymeacoffee.com/sundaresh)