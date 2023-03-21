# 我学习单词嵌入的第一次旅程

> 原文：<https://towardsdatascience.com/my-first-journey-into-learning-word-embeddings-3b88d218fe6?source=collection_archive---------26----------------------->

# 我的第一个帖子在这里！

[**单词嵌入**](https://en.wikipedia.org/wiki/Word_embedding) 和 NLP，更一般地说，以一种特殊的方式引起了我的注意。我很惊讶能从推文、评论、评论中提取多少价值；最奇怪的是，我们是一秒又一秒产生的相同数据的用户。

过去，我们已经学会了如何通过 [**线性回归**，](https://en.wikipedia.org/wiki/Linear_regression)等方法找到最符合给定数据的线的参数。我们开发了经典模型，如[**【ARIMA】**](https://en.wikipedia.org/wiki/Linear_regression)来拟合时间序列，以及如何构建帮助我们做出决策的树。

然而，当我考虑这些模型时(也许这是我的一种偏见)，我总是倾向于将建模的数据看作是严格的和结构化的，是在某种测量或底层过程之后收集的数据。
在我们逐渐不再使用口语的时候，书面语也越来越少，自然语言处理以某种方式让它们恢复了价值，让这个词再次变得有价值，这可能会带来很多积极的结果。

# 当我爱上 NLP 时

我在研究机器学习时发现的第一个文本表示是[**单词包**](https://en.wikipedia.org/wiki/Bag-of-words_model) (BoW)表示，其中文档被表示为其单词的集合，并且完全不考虑词序。此外，我遇到了 [**n-gram**](https://en.wikipedia.org/wiki/N-gram) 模型，其中考虑了 n 个单词的序列，以便在转换过程中保留空间信息。
一把弓可以看成 1 克的模型。

然后，您可以使用 [**一键编码**](https://en.wikipedia.org/wiki/One-hot) 或借助于 [**TF-IDF**](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) 表示对单词或单词序列进行编码，并且您可以使用这些生成的特征来训练机器学习分类器，例如 [**朴素贝叶斯模型**](https://en.wikipedia.org/wiki/Naive_Bayes_classifier) 或 [**支持向量机**](https://en.wikipedia.org/wiki/Support-vector_machine) ，以便执行正面与负面文本分类。

![](img/7e555025d9493a701521cd4e9833a831.png)

字云！

# 深度学习的力量

在机器学习之后，我为我的论文投入了深度学习，我发现了深度神经网络的迷人世界。
深度神经网络是数学模型，在给定足够数量的数据的情况下，由于 [**反向传播**](https://en.wikipedia.org/wiki/Backpropagation) 算法的形式化，深度神经网络有望通过试错法发现输入和输出数据之间的关系，其中网络参数相对于给定损失函数的梯度进行优化，该损失函数根据必须解决的问题进行选择。

跳过剩下的深度学习之旅，我第一次见到了 [**自动编码器**](https://en.wikipedia.org/wiki/Autoencoder) ，最终我对 [**递归神经网络**](https://en.wikipedia.org/wiki/Recurrent_neural_network) 有了更深入的了解，这还要感谢 Andrew NG 在 Coursera 上的 **Deeplearning.ai 课程**。

递归神经网络是具有内部循环的网络，允许信息持续存在。这使得它们非常适合于建模序列数据，例如时间序列，以及文本和语音。

# 单词嵌入，我来了

我使用递归神经网络建立的第一个模型是一个字符级模型，它通过 9000 个意大利名字进行训练。具体来说，这是一个序列到序列模型，其中给定第*个 i-* 个字符作为输入，该模型被训练来预测第*个 i+1-* 个字符。每一个意大利名字都是用一键编码的字符序列处理的。该模型表现良好，因为生成的名字几乎都以 *o* 或 *a* 结尾(这是意大利名字的标准)。后来，我继续进行单词级模型，使用固定长度的句子，其中单词是一个热点编码的，但是我不满意这样的事实，即对于所有的向量对，一个热点向量的余弦相似性是 0，即使是那些表示相似主题的向量。

下一步，我想看看单词嵌入实际上是如何工作的。

# 来自评论的训练嵌入

我从 Kaggle 下载了 [**亚马逊美食评论**](https://www.kaggle.com/snap/amazon-fine-food-reviews) 数据集，由~ 500k 评论组成，我下载了 SQLite 数据库文件。

首先，我导入了 [**sqlite3**](https://docs.python.org/2/library/sqlite3.html) 并构建了一个查询来检索 reviews 表，并查看行数和包含的列数，例如 Reviews 以及我在此阶段没有考虑的其他数据:

```
**import sqlite3
connection = sqlite3.connect('/content/gdrive/My Drive/Colab Notebooks/database.sqlite')
# Number of reviews
c.execute("SELECT COUNT(*) FROM Reviews")
c.fetchall()**
```

我找到了 568000 条评论。然后，我构建了 helper 函数来获取评论并对其进行处理，以便首先通过利用 [**NLTK**](https://www.nltk.org/) 包将它们拆分成一系列句子，然后将每个句子拆分成一系列单词:

```
**def fetch(limit):
    c.execute(f"SELECT Text FROM Reviews LIMIT {limit};" )
    records = c.fetchall()
    print(f"The number of fetched records is {len(records)}")
    return records****def tokenize(chunk):
    from nltk.tokenize import sent_tokenize, word_tokenize
    from tqdm import tqdm** **list_of_tokens = []
    print("Iterating over all the retrieved rows...")

    for row in tqdm(chunk):
    sentences_from_reviews = sent_tokenize(row[0])
    tokenized_sentences = [word_tokenize(sentence) for sentence in sentences_from_reviews]
    list_of_tokens.extend(tokenized_sentences)
    print("Completed.")** **return list_of_tokens**
```

正如你所看到的，我没有过滤停用词或网址或其他任何东西，我想直接有一个最小的单词嵌入的例子。

我验证了数据的结构是一个列表的列表，其中每个内部列表只包含字符串:

```
**all(isinstance(elem, list) and isinstance(word, str) for elem in test for word in elem)** *#returns True!* 
```

我提取了数据并进行处理:

```
**data = fetch(limit=-1)
token_data = tokenize(data)**
```

现在训练数据已经准备好适合一个 [**Word2Vec**](https://en.wikipedia.org/wiki/Word2vec) 模型。我不会详细介绍 Word2Vec，但是网上有很多资源。

```
**import multiprocessing # for number of workers
from gensim.models import Word2Vec****model = Word2Vec(token_data, size=300, min_count = 10, workers=multiprocessing.cpu_count(), iter=30, window=5)**
```

过了一会儿，我准备好了我的模型，我腌制并下载了它(我用的是 Google Colab 笔记本，可以在这里[](https://colab.research.google.com/drive/1Nsj2kHT4mxX1GYbVy-MrT7bG7ZXh9gfx)**查阅):**

```
**import pickle
from google.colab import files****with open('token_data.pkl','wb') as f:
     pickle.dump(token_data,f)****files.download('token_data.pkl')**
```

**然后，我将它导入我的本地 Jupyter 环境:**

```
**import pickle****with open('token_data.pkl','rb') as f:

    token_data=pickle.load(f)**
```

**并检查了与*【橙子】*和*【意大利面】*的相似性:**

```
**model.wv.most_similar('orange')****Out[16]:****[('lemon', 0.6406726241111755),
 ('grapefruit', 0.6343103647232056),
 ('pomegranate', 0.6024238467216492),
 ('citrus', 0.5851069688796997),
 ('blackberry', 0.5831919312477112),
 ('cherry', 0.5785644054412842),
 ('strawberry', 0.5701724290847778),
 ('grape', 0.5649375319480896),
 ('elderberry', 0.5609756708145142),
 ('guava', 0.5601833462715149)]****model.wv.most_similar('pasta')****Out[17]:****[('spaghetti', 0.7852049469947815),
 ('couscous', 0.6740388870239258),
 ('pastas', 0.6413708329200745),
 ('noodles', 0.6318570971488953),
 ('linguine', 0.6257054805755615),
 ('vermicelli', 0.6104354858398438),
 ('penne', 0.6094731092453003),
 ('lasagna', 0.5939062833786011),
 ('macaroni', 0.5886696577072144),
 ('bread', 0.5816901922225952)]**
```

**所以它似乎抓住了食物的相似之处！接下来，我将使用 [**t-SNE**](https://it.wikipedia.org/wiki/T-distributed_stochastic_neighbor_embedding) 来可视化嵌入，但是我将在下一篇文章中保留它。**