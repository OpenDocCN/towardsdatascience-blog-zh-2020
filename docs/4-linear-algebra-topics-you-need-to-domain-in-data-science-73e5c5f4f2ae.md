# 数据科学中需要掌握的 4 个线性代数主题

> 原文：<https://towardsdatascience.com/4-linear-algebra-topics-you-need-to-domain-in-data-science-73e5c5f4f2ae?source=collection_archive---------28----------------------->

![](img/387181f7763d59472a1ea9608368cc83.png)

来源:[路易斯·鲍尔斯](https://www.pexels.com/@louis-bauer-79024)@ Pexels-免费股票图片

## 以及在哪里可以学到它们

首先也是最重要的，我想为过去几周缺乏故事和内容向自己道歉…它们太忙了！在确认了[我们在 Ravelin](https://techcrunch.com/2020/07/15/ravelin/) 的最后 2000 万英镑的新成立之后，我们看到了一波新客户的到来，这总是好消息，但也有相当多的工作！好了，现在，我已经说够了这些废话，我将开始这个故事，介绍一件我打算从现在开始在我的所有内容中开始做的新事情:包括我最近阅读的一段引文或一段文字，我发现它对从事数据科学的人来说特别有趣和相关。

我们将从我亲爱的同事[亚当·格拉斯](https://www.linkedin.com/in/adam-glass/)最近在讨论模特的表现时告诉我的一件事开始这一部分。他说:

> 死亡，税收和误报？

他聪明的声明引用了最初的一句话“除了死亡和税收，没有什么是确定的”，这句话有时出自本·富兰克林，有时出自马克·吐温或丹尼尔·笛福。我特别喜欢亚当的信息，因为不知何故它减轻了我们的负担。怎么会？从这个意义上说，当我们在处理分类问题时，如果我们不仅要追求精确度，还要追求准确性和其他相关的分类指标，那么假阳性几乎是不可能避免的。如果你对阅读这方面的更多内容感兴趣，我的这个故事[讨论了分类问题中的类别不平衡，对这些度量和概念做了更多的解释。](/class-imbalance-a-classification-headache-1939297ff4a4)

敬请期待下一段引言！但是现在我们马上跳到我们今天的主题:你需要在数据科学领域的线性代数的 4 个主题。正如我在以前的故事中说过的，技术和语言可能来来去去，但这个领域的数学背景将永远存在。这就是为什么，很好地理解数据科学中使用的主要概念，尤其是机器学习，是理解我们从库中导入的算法实际上如何工作以及我们应该做什么(和避免什么)的关键。)当我们使用它们的时候。这就是为什么今天我们要从最基础的开始，一路向上，介绍一些我个人认为应该学习的东西。

# 1.线性代数

## 1.1.向量和矩阵

向量和矩阵是我们在数据科学领域所做的一切的关键部分。例如，当我们训练模型时，我们拟合特征矩阵，当我们查看数据中元素之间的距离时，我们通常会发现特征向量之间的某种几何距离，当我们使用降维技术时，我们通常只是找到一种简洁的方式来表示一组向量，而不会失去它们之间的潜在关系。这就是为什么我们需要从理解这些元素是什么开始。

资源:

*   [矢量介绍](https://mathinsight.org/vector_introduction)，来自《数学透视》
*   [向量和空间](https://www.khanacademy.org/math/linear-algebra/vectors-and-spaces)，来自可汗学院

## 1.2.矩阵运算

机器学习中，矩阵运算无处不在。矩阵只不过是一个具有一列或多列以及一行或多行的二维数组。听起来很熟悉？这听起来像熊猫的数据帧吗？答对了。你猜怎么着:每次我们对我们的任何特征执行加法、减法或乘法时，我们通常都是在执行矩阵运算。此外，我们使用最多的大多数算法在某种程度上都使用了矩阵运算。例如，神经网络是一系列一个接一个发生的矩阵运算。机器学习大师的以下文章包含了对该主题的很好的介绍，如果您想阅读更多内容，可以看看更多资源:

资源:

*   [矩阵和矩阵运算简介](https://machinelearningmastery.com/introduction-matrices-machine-learning)，来自机器学习大师

## 1.3.碱基和格拉姆-施密特过程

我们在数据科学中经常使用的一些算法使用逆矩阵作为它们求解过程的一部分。这就是为什么在数据科学中，我们总是希望矩阵尽可能正交。这使得逆变换更容易计算，这意味着变换之后是可逆的，因此投影也更容易计算。如果这一切听起来有点令人困惑，我强烈推荐以下链接。

资源:

*   [改变基底](https://www.youtube.com/watch?v=P2LTAUO1TdA)，从 YouTube 上的 3Blue1Brown
*   汗学院[备用坐标系](https://www.khanacademy.org/math/linear-algebra/alternate-bases)的前 4 章
*   帝国理工学院 ML 视频数学(视频: [1](https://www.youtube.com/watch?v=6wKATS8FYYw&list=PL2jykFOD1AWazz20_QRfESiJ2rthDF9-Z&index=21&t=0s) ， [2](https://www.youtube.com/watch?v=0qycdJWyr68&list=PL2jykFOD1AWazz20_QRfESiJ2rthDF9-Z&index=22&t=0s) ， [3](https://www.youtube.com/watch?v=mEIFs3BCBkE&list=PL2jykFOD1AWazz20_QRfESiJ2rthDF9-Z&index=23&t=1s) ， [4](https://www.youtube.com/watch?v=kx2bgfHvyZE&list=PL2jykFOD1AWazz20_QRfESiJ2rthDF9-Z&index=24&t=0s) )

## 1.4.特征向量和特征值

特征向量和特征值是我们线性代数旅程的最后一个里程碑。降维算法，正如主成分分析严重依赖于这些概念，一些现实世界的工具，如谷歌搜索算法，将它们作为其逻辑的关键部分。如果你已经对前面推荐的所有主题感到满意，那么理解最后一点将是非常重要的！

资源:

*   [最温和的主成分分析介绍](/the-most-gentle-introduction-to-principal-component-analysis-9ffae371e93b?source=friends_link&sk=9e037c65f96121373344299b2afbde46)，我之前的一个关于走向数据科学的故事
*   [谷歌如何在网络大海捞针](http://Feature Column  How Google Finds Your Needle in the Web's Haystack)，来自美国数学协会网站
*   [特征向量和特征值](https://www.youtube.com/watch?v=PFDu9oVAE-g&vl=en)，来自 YouTube 上的 3Blue1Brown

这就是线性代数。如果你认为我错过了任何相关的话题，请让我知道！在未来的故事中，我们将在微积分的世界中经历一个类似的列表！

同时，别忘了查看我之前的一些故事:)

[](/5-more-tools-and-techniques-for-better-plotting-ee5ecaa358b) [## 5 更好的绘图工具和技术

### 充分利用您的数据

towardsdatascience.com](/5-more-tools-and-techniques-for-better-plotting-ee5ecaa358b) [](/machine-learning-free-online-courses-from-beginner-to-advanced-f50982dce950) [## 机器学习和数据科学隔离免费在线课程

### 从初级到高级。充分利用 covid 19-锁定

towardsdatascience.com](/machine-learning-free-online-courses-from-beginner-to-advanced-f50982dce950) [](/should-i-inlcude-the-intercept-in-my-machine-learning-model-2efa2a3eb58b) [## 我应该在我的机器学习模型中包括截距吗？

### 并简要介绍了数据科学中的回归建模

towardsdatascience.com](/should-i-inlcude-the-intercept-in-my-machine-learning-model-2efa2a3eb58b) 

或者直接访问[我在 Medium](https://medium.com/@gonzaloferreirovolpi) 上的个人资料，查看我的其他故事。还有**如果你想直接在你的邮箱里收到我的最新文章，只需** [**订阅我的简讯**](https://gmail.us3.list-manage.com/subscribe?u=8190cded0d5e26657d9bc54d7&id=3e942158a2) **:)。**再见，感谢阅读！

媒体上见！