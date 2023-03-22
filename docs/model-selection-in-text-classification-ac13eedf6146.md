# 文本分类中的模型选择

> 原文：<https://towardsdatascience.com/model-selection-in-text-classification-ac13eedf6146?source=collection_archive---------5----------------------->

![](img/aef1de737181198d314923f26c3456a0.png)

来源:作者图片

> 更新 2020–11–24:结论中增加了资源
> 
> 更新 2020 年 8 月 21 日:管道现在可以用于二进制和多类分类问题。在处理模型的保存时，转换器仍然存在错误。
> 
> 更新 2020–07–16:管道现在可以保存模型，修正了主函数内部的错误。添加了 Adaboost、Catboost、LightGBM、extractree 分类器

# 简介:

一开始，有一个简单的问题。我的经理来问我是否可以用 NLP 方法对邮件和相关文档进行分类。

听起来并不奇怪，但我会从成千上万个样本开始。问的第一件事就是用“XGBoost”，因为:“我们可以用 XGBoost 做任何事情”。有趣的工作，如果数据科学都归结于 XGBoost…

在用不同的算法、指标和可视化实现了一个笔记本之后，我的脑海中仍然有一些东西。我无法一次在不同的型号中做出选择。仅仅是因为你可能对一个模型抱有侥幸心理，而不知道如何重现良好的准确度、精确度等…

所以此时此刻，我问自己，如何做一个模特选拔？我在网上寻找，阅读帖子，文章，等等。看到实现这种东西的不同方式是非常有趣的。但是，当考虑到神经网络时，它是模糊的。这时，我脑子里有一件事，如何比较经典方法(多项朴素贝叶斯、SVM、逻辑回归、boosting …)和神经网络(浅层、深层、lstm、rnn、cnn…)。

我在这里对笔记本做一个简短的解释。欢迎评论。

> 这个笔记本在 GitHub 上有:[这里](https://github.com/Christophe-pere/Model-Selection)T2【这个笔记本在 Colab 上有:[这里](https://colab.research.google.com/drive/1qeDOAvtlyBTq7K2eza45b8RZ1_FRrHw_?usp=sharing)

# 如何开始？

每个项目都是从**探索性数据分析**(简称 **EDA** )开始，然后直接是**预处理**(文本非常脏，邮件中的签名、url、邮件头等等……)。不同的功能将出现在 Github 库中。

查看预处理是否正确的快速方法是确定最常见的 **n 元文法** (uni，bi，tri… grams)。另一篇文章将会引导你走这条路。

# **数据**

我们将对 IMDB 数据集应用*模型选择*的方法。如果您不熟悉 IMDB 数据集，它是一个包含电影评论(文本)的数据集，用于情感分析(二元——正面或负面)。
更多详情可在这里找到[。要下载它:](http://ai.stanford.edu/~amaas/data/sentiment/)

```
$ wget http://ai.stanford.edu/~amaas/data/sentiment/aclImdb_v1.tar.gz$ tar -xzf aclImdb_v1.tar.gz
```

# 矢量化方法

**One-Hot encoding(count vectorizing)**:
这是一种将单词替换为 0 和 1 的向量的方法，目标是获取一个语料库(重要的单词量)，并为语料库中包含的每个唯一单词制作一个向量。之后，每个单词都会被投影到这个向量中，其中 **0 表示不存在，1 表示存在**。

```
 | bird | cat | dog | monkey |
bird   |  1   |  0  |  0  |    0   |
cat    |  0   |  1  |  0  |    0   |
dog    |  0   |  0  |  1  |    0   |
monkey |  0   |  0  |  0  |    1   |
```

相应的 python 代码:

```
# create a count vectorizer object 
count_vect = CountVectorizer(analyzer='word', token_pattern=r'\w{1,}')
count_vect.fit(df[TEXT])  # text without stopwords# transform the training and validation data using count vectorizer object
xtrain_count =  count_vect.transform(train_x)
xvalid_count =  count_vect.transform(valid_x)
```

**TF-IDF:**
*词频-逆文档频率*是信息检索和文本挖掘中经常用到的一个权重。这个权重是一个统计量，用于评估一个词对集合或语料库中的文档有多重要(来源: [tf-idf](http://www.tfidf.com/) )。
在处理大量停用词时，这种方法非常有效(这种类型的词与信息无关→ *我、我的、我自己、我们、我们的、我们的、我们自己、你* …对于英语)。IDF 术语允许显示重要词和罕见词。

```
# word level tf-idf
tfidf_vect = TfidfVectorizer(analyzer='word', token_pattern=r'\w{1,}', max_features=10000)
tfidf_vect.fit(df[TEXT])
xtrain_tfidf =  tfidf_vect.transform(train_x_sw)
xvalid_tfidf =  tfidf_vect.transform(valid_x_sw)
```

**TF-IDF n-grams** :
与之前的 *tf-idf* 的区别在于**一字**， *tf-idf n-grams* 兼顾了 *n* 连词。

```
# ngram level tf-idf
tfidf_vect_ngram = TfidfVectorizer(analyzer='word', token_pattern=r'\w{1,}', ngram_range=(2,3), max_features=10000)
tfidf_vect_ngram.fit(df[TEXT])
xtrain_tfidf_ngram =  tfidf_vect_ngram.transform(train_x_sw)
xvalid_tfidf_ngram =  tfidf_vect_ngram.transform(valid_x_sw)
```

**TF-IDF chars n-grams:**
与前面的方法相同，但重点是在字符级别，该方法将重点放在 n 个连续的字符上。

```
# characters level tf-idf
tfidf_vect_ngram_chars = TfidfVectorizer(analyzer='char',  ngram_range=(2,3), max_features=10000) 
tfidf_vect_ngram_chars.fit(df[TEXT])
xtrain_tfidf_ngram_chars =  tfidf_vect_ngram_chars.transform(train_x_sw) 
xvalid_tfidf_ngram_chars =  tfidf_vect_ngram_chars.transform(valid_x_sw)
```

**预训练模型—快速文本**

> FastText 是一个开源、免费、轻量级的库，允许用户学习文本表示和文本分类器。它在标准的通用硬件上工作。模型可以缩小尺寸，甚至适合移动设备。(来源:[此处](https://fasttext.cc/))

如何加载 fastText？来自官方文件:

```
$ git clone https://github.com/facebookresearch/fastText.git 
$ cd fastText 
$ sudo pip install . 
$ # or : 
$ sudo python setup.py install
```

下载合适的型号。这里有 157 种语言的模型。要下载英文模型:

```
$ wget https://dl.fbaipublicfiles.com/fasttext/vectors-english/crawl-300d-2M-subword.zip& unzip crawl-300d-2M-subword.zip
```

下载完成后，将其加载到 python 中:

```
pretrained = fasttext.FastText.load_model('crawl-300d-2M-subword.bin')
```

**单词嵌入或单词向量(WE):**

> 将向量与单词相关联的另一种流行且强大的方法是使用密集的“单词向量”，也称为“单词嵌入”。虽然通过一位热编码获得的向量是二进制的、稀疏的(主要由零组成)和非常高维的(与词汇表中的单词数量具有相同的维数)，但是“单词嵌入”是低维浮点向量(即，与稀疏向量相反的“密集”向量)。与通过一键编码获得的单词向量不同，单词嵌入是从数据中学习的。在处理非常大的词汇表时，经常会看到 256 维、512 维或 1024 维的单词嵌入。另一方面，一键编码单词通常导致 20，000 维或更高维的向量(在这种情况下捕获 20，000 个单词的词汇)。因此，单词嵌入将更多的信息打包到更少的维度中。(来源:[用 Python 进行深度学习，弗朗索瓦·乔莱 2017](https://nbviewer.jupyter.org/github/fchollet/deep-learning-with-python-notebooks/blob/master/6.1-using-word-embeddings.ipynb) )

如何用整数映射一个句子:

```
# create a tokenizer 
token = Tokenizer()
token.fit_on_texts(df[TEXT])
word_index = token.word_index# convert text to sequence of tokens and pad them to ensure equal length vectors 
train_seq_x = sequence.pad_sequences(token.texts_to_sequences(train_x), maxlen=300)
valid_seq_x = sequence.pad_sequences(token.texts_to_sequences(valid_x), maxlen=300)# create token-embedding mapping
embedding_matrix = np.zeros((len(word_index) + 1, 300))
words = []
for word, i in tqdm(word_index.items()):
    embedding_vector = pretrained.get_word_vector(word) #embeddings_index.get(word)
    words.append(word)
    if embedding_vector is not None:
        embedding_matrix[i] = embedding_vector
```

# 型号选择

计算机科学中的**选型**是什么？具体在 AI 领域？*模型选择*是在不同的机器学习方法之间进行选择的过程。所以简而言之，不同的模式。

但是，我们如何比较它们呢？要做到这一点，我们需要度量标准(更多细节见此[链接](https://scikit-learn.org/stable/modules/model_evaluation.html))。数据集将被分割成 ***训练*** 和 ***测试*** 部分(验证集将在深度学习模型中确定)。

在这个**二元或多类分类**模型选择中，我们使用什么样的度量标准？

为了分类，我们将使用以下术语:
- **tp** :真阳性预测
- **tn** :真阴性预测
- **fp** :假阳性预测
- **fn** :假阴性预测
这里有一个[链接](/understanding-confusion-matrix-a9ad42dcfd62)以了解更多细节。

*   [**准确度**](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html#sklearn.metrics.accuracy_score) :所有预测上的所有正预测
    (tp + tn) / (tp + tn + fp + fn)
*   [](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.balanced_accuracy_score.html#sklearn.metrics.balanced_accuracy_score)**:定义为不平衡数据集在每个类上获得的平均召回率**
*   **[**精度**](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html#sklearn.metrics.precision_score) :精度直观上是分类器不将 tp / (tp + fp)为阴性的样本标记为阳性的能力**
*   **[**召回**](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html#sklearn.metrics.recall_score) (或灵敏度):召回是分类器直观地找到所有阳性样本 tp / (tp + fn)的能力**
*   ****f1-得分**:f1 得分可以解释为精度和召回率的加权平均值- > 2 *(精度*召回率)/(精度+召回率)**
*   **[**Cohen kappa**](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.cohen_kappa_score.html#sklearn.metrics.cohen_kappa_score) :表示两个标注者对一个分类问题的一致程度的分数。因此，如果数值小于 0.4 就很糟糕，在 0.4 到 0.6 之间，它相当于人类，0.6 到 0.8 是很大的数值，超过 0.8 就是例外。**
*   **[**马修斯相关系数**](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.matthews_corrcoef.html#sklearn.metrics.matthews_corrcoef):****马修斯相关系数*** *(MCC)用于* [*机器学习*](https://en.wikipedia.org/wiki/Machine_learning) *作为二进制(两类)* [*分类*](https://en.wikipedia.org/wiki/Binary_classification) *[…]该系数考虑了真、假阳性和阴性，通常被视为可以使用的平衡度量 MCC 本质上是观察到的和预测的二元分类之间的相关系数；它返回 1 到+1 之间的值。系数+1 表示完美预测，0 表示不比随机预测好，1 表示预测和观察完全不一致。(来源:* [*维基百科*](https://en.wikipedia.org/wiki/Matthews_correlation_coefficient) *)****
*   ***[**受试者操作特征曲线下面积**](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html#sklearn.metrics.roc_auc_score) **(ROC AUC)*****
*   *****时间拟合**:训练模型所需的时间***
*   *****时间分数**:预测结果所需的时间***

***太好了！！我们是我们的衡量标准。下一步是什么？***

# ***交叉验证***

***为了能够实现模型之间的稳健比较，我们需要验证每个模型的稳健性。***

***下图显示了需要将全局数据集拆分为训练和测试数据。训练数据用于训练模型，测试数据用于测试模型。交叉验证是在 *k-fold* 中分离数据集的过程。k 是我们实现数据所需的比例数。***

***![](img/235a446a464ca1517d6054f109c7e16d.png)***

***来源: [sklearn](https://scikit-learn.org/stable/modules/cross_validation.html)***

***一般来说， *k* 为 5 或 10 这将取决于**数据集**的大小(小数据集小 *k* ，大数据集大 *k* )。***

***目标是计算每个折叠的每个指标，并计算它们的**平均值**(平均值)和**标准偏差**(标准偏差)。***

***在 python 中，这个过程将通过 *scikit-learn* 中的 [**cross_validate**](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_validate.html#sklearn.model_selection.cross_validate) 函数来完成。***

***我们会对比哪些车型？***

# *****评估的型号*****

***我们将测试机器学习模型、深度学习模型和 NLP 专门模型。***

*****机器学习模型*****

*   ***多项式朴素贝叶斯***
*   ***逻辑回归***
*   ***SVM (SVM)***
*   ***随机梯度下降***
*   ***k 近邻***
*   ***随机森林***
*   ***梯度增强(GB)***
*   ***XGBoost(著名的)(XGB)***
*   ***adaboost 算法***
*   ***Catboost***
*   ***LigthGBM***
*   ***树外分级机***

*****深度学习模型*****

*   ***浅层神经网络***
*   ***深度神经网络(和 2 个变体)***
*   ***递归神经网络(RNN)***
*   ***长短期记忆(LSTM)***
*   ***卷积神经网络(CNN)***
*   ***门控循环单元(GRU)***
*   ***CNN+LSTM***
*   ***CNN+GRU***
*   ***双向 RNN***
*   ***双向 LSTM***
*   ***双向 GRU***
*   ***递归卷积神经网络(RCNN)(和 3 个变体)***
*   ***变形金刚(电影名)***

***仅此而已，30 款还不错。***

***这可以在这里恢复:***

# ***让我们展示一些代码***

*****机器学习*****

***对于经典算法，我将使用*sk learn(0.23 版)*中的 [cross_validate()](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_validate.html#sklearn.model_selection.cross_validate) 函数来考虑多指标。下面的函数， *report* ，获取一个**分类器**， **X，y** 数据，以及一个**自定义指标列表**，并使用参数计算它们的**交叉验证**。它返回一个 dataframe，包含所有指标的**值**，以及每个指标的**平均值**和标准偏差( **std)** 。***

***使用机器学习算法计算不同指标的函数***

*****怎么用？*****

***下面是多项式朴素贝叶斯的一个例子:***

***术语 if multinomial_naive_bayes 之所以存在，是因为此代码是笔记本的一部分，开头带有参数(布尔型)。所有代码都在 [GitHub](https://github.com/Christophe-pere/Model-Selection) 和 [Colab](https://colab.research.google.com/drive/1qeDOAvtlyBTq7K2eza45b8RZ1_FRrHw_?usp=sharing) 中。***

*****深度学习*****

***我还没有找到深度学习的 cross_validate 这样的函数，只有关于对神经网络使用 *k 倍*交叉验证的帖子。这里我将分享一个用于深度学习的自定义 cross_validate 函数，其输入和输出与 *report* 函数相同。它将允许我们使用相同的指标，并将所有模型放在一起进行比较。***

***利用二元和多类分类的度量进行深度学习的 k-Folds 交叉验证***

***目标是将神经网络函数视为:***

***并在 cross_validate_NN 函数内部调用这个函数。所有的深度学习都获得了相同的实现，并将进行比较。有关不同型号的完整实现，请访问[笔记本](https://github.com/Christophe-pere/Model-Selection)。***

# ***结果***

***当所有模型计算出不同的折叠和度量时，我们可以很容易地将它们与数据框架进行比较。在 IMDB 数据集上，表现较好的模型是:***

***这里我只展示了准确率> 80%的结果，以及准确度、精确度和召回指标的结果。***

# ***如何改善？***

*   ***在模型字典的参数中构建一个函数，并将所有工作连接成一个循环***
*   ***使用分布式深度学习***
*   ***使用 TensorNetwork 加速神经网络***
*   ***使用 GridSearch 进行超调***

# ***结论***

***现在，您有了一个导入管道，可以为带有大量参数的文本分类进行模型选择。所有的模型都是自动的。享受吧。***

***另一个关于机器学习时代模型选择的伟大资源(更多理论文章)是由 [Samadrita Ghosh](https://www.linkedin.com/in/samadritaghosh/?originalSubdomain=in) 在 [Neptune.ai 博客](https://neptune.ai/blog/the-ultimate-guide-to-evaluation-and-selection-of-models-in-machine-learning)上写的。***