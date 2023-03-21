# 音乐流派分析——理解不同流派中的情感和主题

> 原文：<https://towardsdatascience.com/music-genre-analysis-understanding-emotions-and-topics-in-different-genres-110377d8888c?source=collection_archive---------31----------------------->

## 理解不同音乐流派中的主题，并创建简单的预测和推荐应用程序

供稿人 : Dimple Singhania，Adwait Toro，Sushrut Shendre，Chavi Singal，Anuja Dixit

![](img/97eb851f4a5809cb38b4f415431900e6.png)

由[本·威恩斯](https://unsplash.com/@benwiens?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## **简介**

音乐是连接世界各地人们的东西之一。无论歌词使用何种语言，好的音乐都能得到全球的认可。但是即使音乐没有语言，音乐中的歌词在定义音乐类型中起着重要的作用。理解歌曲背后的主题是至关重要的，这样才能让即将到来的歌唱天才创作出符合当前流行主题的歌曲。

许多音乐平台，如 Spotify、亚马逊音乐(Amazon music)都依靠流派预测来更好地向用户推荐类似的歌曲。Spotify 使用流派定位，允许我们在用户听完特定流派的歌曲后立即发送消息。

因此，我们决定建立一个模型，从歌词中预测歌曲的流派。

## **数据**

我们的数据集由大约 100k 条记录组成，有 4 列:艺术家、歌曲、歌词和流派。

**EDA**

为了挖掘出一些关于数据的有趣事实，我们做了一些探索性的数据分析。

1.  为了查看流派的样本，数据集严重偏向摇滚(52k 歌曲；52%).R&B、独立、民谣和电子音乐仅占数据的 6%，总共有近 6000 首歌曲。

数据中的这种偏差导致我们的模型比其他流派更准确地预测摇滚流派。

![](img/7febe065b7042efddfee683ef5b922fe.png)

2.接下来，我们想看看数据集中的顶级艺术家。令人惊讶的是，尽管摇滚在数据集中占据了主导地位，美国标志性乡村歌手多莉·帕顿却高居榜首。

![](img/2c162a0ed4457a025b88a734215e70f5.png)

3.接下来，我们想看看不同体裁的字符和单词的长度有什么不同。Hip-Hop 拥有最高的平均字符数和单词数。这可能是因为 Hip-hop 有说唱歌曲，这些歌曲在很短的时间内唱了很多词。

![](img/64ddd43fd7c7f4786980bfe799a79fca.png)

## **自定义功能**

首先，我们从歌词中提取了一些我们认为有助于流派预测的特征。

1.  **情绪**

我们使用了一个外部数据集，将歌词分为不同的情绪。我们在这个数据集上训练了一个逻辑回归分类器，并预测了给定数据集的情绪。下面是在外部数据上安装分类器并将其下载为的函数。用于预测的 pkl。

[https://gist . github . com/chiko Rita 5/7b 70 be 4746 e 54 a 23 BF 4 e 227 a 4 aeed 63 e](https://gist.github.com/chikorita5/7b70be4746e54a23bf4e227a4aeed63e)

在没有外部数据集来训练的情况下，也可以尝试提取情绪的替代方法。IBM watson 音调分析器 API 可用于获取情绪。

[https://gist . github . com/chiko Rita 5/359625 Fe 595 DC 0 c 0 e 34 Fe 65 e5d 922 f 58](https://gist.github.com/chikorita5/359625fe595dc0c0e34fe65e5d922f58)

2.歌词的字数

3.歌词中的行数

## **分类**

首先，我们将数据集分为训练和测试两部分。我们使用 TFIDF 矢量器、定制特性和分类器创建了一个简单的`Pipeline`对象。我们使用逻辑回归、决策树和朴素贝叶斯进行预测。

![](img/1faac6b260aaa72579610704f66e686e.png)

如你所见，逻辑具有最高的准确性。决策树的准确率较低，但召回率和 f-1 分数略高于 logistic。

## **过采样**

因为我们的数据有不平衡的流派分类。为了提高模型的准确性，我们对训练数据集中的少数类进行了过采样。我们使用过采样数据再次执行朴素贝叶斯和逻辑回归，性能提高了很多。

![](img/d0e9365e08983d8b70960958ad4b3655.png)

混淆矩阵-朴素贝叶斯:准确率 73.5%

经过过采样后，朴素贝叶斯模型的性能显著提高，准确率为 73%，召回率为 74%。在上面的混淆矩阵中，可以看到摇滚流派被错误地预测为流行、乡村、金属好几次。

![](img/3ae70400acd3df85c8c940a5a82ebd8e.png)

混淆矩阵—逻辑回归:准确率 80.6%

过采样后，逻辑回归和 CountVectorizer 提供了最高的精度。尽管如此，摇滚和流行音乐之间还是有些混淆。

## **主题建模**

我们还做了一些主题建模，以确定不同体裁中的常见主题。对于主题建模，我们使用 Gensim 包中的潜在 Dirichlet 分配(LDA)以及 Mallet 的实现(通过 Gensim)。Mallet 有一个高效的 LDA 实现。众所周知，它运行速度更快，主题分离更好。

在应用主题建模后，我们发现我们有 6 个主导主题，模型的最佳一致性分数为 53%，困惑分数为-10。困惑是模型在将文档分类为主题时不会混淆的度量。困惑分数越低，模型越好。

在查看了 6 个主题的关键词后，我们决定这些类型有 6 个潜在的主题，如**调情、爱情、青春、悲伤、快乐/舞蹈、奉献/死亡**。为了以一种更具交互性和用户友好的方式来描述这些，我们创建了一个 Tableau 仪表板，显示主题及其相关的关键字。仪表板还提供了选择您最喜爱的艺术家，他们的流派和歌曲，以查看潜在的主题和相关的关键字，以及改变关键字频率的规定。

![](img/c9ad010beb7d5d27eb76a4c2fe99ec84.png)

主题和相关关键字的概述

例如，我们选择了演唱《爱你更多》的歌手阿肯。它被归类为在其流行类型下有一个悲伤的主题，显示在 tableau 仪表板上。

![](img/028be1631fddf2b001c2bd37bff7cf36.png)

## **应用**

使用我们的最终分类器，我们使用 Python 的 tkinter 库创建了一个应用程序。任何歌曲的歌词都可以粘贴到应用程序中，以检查其流派。

流派预测应用程序演示

## **推荐系统**

很多时候，我们听了一首歌，非常喜欢，但不知何故，我们错过了它的名字，只有它的一句歌词整天萦绕在我们的脑海里！为了解决这个问题，我们使用了一种距离测量技术来找出与用户输入的歌曲最相似的歌曲。我们提供了一个文本框，用户可以在其中键入歌曲的歌词，系统将根据歌词推荐最接近输入歌曲的前 10 首歌曲。我们使用 Jaccard 相似性设计了这一功能，首先用户输入歌曲，然后将其标记为单词列表，并与数据集中出现的每首歌词进行比较。相似性被计算为两个标记化列表的交集/两个列表的并集，并且相似性接近 1 的歌曲被显示。

推荐系统演示

## 开源代码库

[https://github.com/chikorita5/nlp-genre-classification](https://github.com/chikorita5/nlp-genre-classification)

## **参考文献**

1.  Bird，Steven，Edward Loper 和 Ewan Klein (2009)，*用 Python 进行自然语言处理*。奥莱利媒体公司
2.  [https://towards data science . com/the-triune-pipeline-for-three-major-transformers-in-NLP-18c 14 e 20530](/the-triune-pipeline-for-three-major-transformers-in-nlp-18c14e20530)
3.  [sci kit-learn:Python 中的机器学习](http://jmlr.csail.mit.edu/papers/v12/pedregosa11a.html)，Pedregosa *等人*，JMLR 12，第 2825–2830 页，2011 年
4.  [https://www . machine learning plus . com/NLP/topic-modeling-gensim-python](https://www.machinelearningplus.com/nlp/topic-modeling-gensim-python/)/