# 6 分钟解释所有机器学习模型

> 原文：<https://towardsdatascience.com/all-machine-learning-models-explained-in-6-minutes-9fe30ff6776a?source=collection_archive---------0----------------------->

## 最流行的机器学习模型的直观解释。

![](img/bfc00d82d634aa2321ee143432b006fe.png)

> ***如果您想测试这些 ML 算法，请查看*** [***土星云***](https://saturncloud.io/?utm_source=Medium+&utm_medium=TDS&utm_campaign=Terence+Shin&utm_term=All+Machine+Learning+Models+Explained+in+6+Minutes) ***，这是一个可扩展的、灵活的数据科学平台，有免费层和企业层。***

![](img/fd13aca99c18c99353984cf8f8f33b9f.png)

[在我之前的文章](https://medium.com/swlh/predicting-life-expectancy-w-regression-b794ca457cd4)中，我解释了什么是**回归**，并展示了如何在应用程序中使用它。本周，我将回顾实践中使用的大多数常见机器学习模型，以便我可以花更多的时间来建立和改进模型，而不是解释其背后的理论。让我们深入研究一下。

![](img/05eb8fa9613c5d2fb4e59d8a2e12d7f8.png)

机器学习模型的基本分段

所有的机器学习模型被分类为**监督的**或**非监督的**。如果模型是监督模型，那么它被细分为**回归**或**分类**模型。我们将讨论这些术语的含义以及下面每个类别中对应的模型。

> ***请务必*** [***订阅此处***](https://terenceshin.medium.com/membership) ***或至我的*** [***独家快讯***](https://terenceshin.substack.com/embed) ***千万不要错过另一篇关于数据科学的文章指南、技巧和提示、生活经验等！***

# 监督学习

**监督学习**涉及学习一个函数，该函数基于示例输入-输出对将输入映射到输出【1】。

例如，如果我有一个包含两个变量的数据集，年龄(输入)和身高(输出)，我可以实现一个监督学习模型，根据年龄预测一个人的身高。

![](img/4469c2929dc4299e691df01aa9d7db83.png)

监督学习的例子

重复一下，在监督学习中，有两个子类别:回归和分类。

# 回归

在**回归**模型中，输出是连续的。下面是一些最常见的回归模型。

## 线性回归

![](img/fdb1eb19ae2ed98560be41be9391f0af.png)

线性回归的例子

线性回归的概念就是找到一条最符合数据的直线。线性回归的扩展包括多元线性回归(例如，找到最佳拟合的平面)和多项式回归(例如，找到最佳拟合的曲线)。你可以在我的[上一篇文章](https://medium.com/swlh/predicting-life-expectancy-w-regression-b794ca457cd4)中了解更多关于线性回归的知识。

## 决策图表

![](img/89991e805ee972142726c35a7ffd91c9.png)

图片来自 Kaggle

**决策树**是一种流行的模型，用于运筹学、战略规划和机器学习。上面的每个方块被称为一个**节点**，节点越多，你的决策树就越精确(一般来说)。决策树的最后一个节点，也就是做出决策的地方，被称为树的**叶**。决策树直观且易于构建，但在准确性方面有所欠缺。

> ***如果您想测试这些 ML 算法，请查看*** [***土星云***](https://saturncloud.io/?utm_source=Medium+&utm_medium=TDS&utm_campaign=Terence+Shin&utm_term=All+Machine+Learning+Models+Explained+in+6+Minutes) ***，这是一个可扩展、灵活的数据科学平台，有免费层和企业层。***

![](img/fd13aca99c18c99353984cf8f8f33b9f.png)

## 随机森林

**随机森林**是一种基于决策树的[集成学习](https://en.wikipedia.org/wiki/Ensemble_learning)技术。随机森林包括使用原始数据的[自举数据集](https://machinelearningmastery.com/a-gentle-introduction-to-the-bootstrap-method/)创建多个决策树，并在决策树的每一步随机选择一个变量子集。然后，该模型选择每个决策树的所有预测的模式。这有什么意义？依靠“多数获胜”模型，它降低了单个树出错的风险。

![](img/37d302e5755ad3c13eb03d397cfc0f25.png)

例如，如果我们创建一个决策树，第三个，它会预测 0。但是如果我们依赖所有 4 个决策树的模式，预测值将是 1。这就是随机森林的力量。

StatQuest 做了一项了不起的工作，更详细地说明了这一点。见[此处](https://www.youtube.com/watch?v=J4Wdy0Wc_xQ&vl=en)。

## 神经网络

![](img/9d560ce9742fc78499d8ddcaad9737cb.png)

神经网络的可视化表示

神经网络本质上是数学方程的网络。它接受一个或多个输入变量，并通过一个方程式网络，产生一个或多个输出变量。你也可以说一个神经网络接受一个输入向量并返回一个输出向量，但是我不会在本文中讨论矩阵。

蓝色圆圈代表**输入层，**黑色圆圈代表**隐藏层，**绿色圆圈代表**输出层。**隐藏层中的每个节点代表前一层中的节点经历的线性函数和激活函数，最终导致绿色圆圈中的输出。

*   如果你想了解更多，看看我对神经网络的初学者友好的解释。

> ***请务必*** [***订阅此处***](https://terenceshin.medium.com/membership) ***或至我的*** [***独家快讯***](https://terenceshin.substack.com/embed) ***千万不要错过另一篇关于数据科学的指南、窍门和技巧、生活经验等！***

# 分类

在分类模型中，输出是离散的。下面是一些最常见的分类模型。

## 逻辑回归

逻辑回归类似于线性回归，但用于模拟有限数量结果的概率，通常为两个。在对结果的概率建模时，使用逻辑回归而不是线性回归的原因有很多(见[此处](https://stackoverflow.com/questions/12146914/what-is-the-difference-between-linear-regression-and-logistic-regression))。实质上，逻辑方程是以这样一种方式创建的，即输出值只能在 0 和 1 之间(见下文)。

![](img/4e1289edda5acd0ce4741587c097e46b.png)

> ***如果您想测试这些 ML 算法，请查看*** [***土星云***](https://saturncloud.io/?utm_source=Youtube+&utm_medium=TDS&utm_campaign=Terence+Shin&utm_term=All+Machine+Learning+Models+Explained+in+6+Minutes) ***，这是一个可扩展、灵活的数据科学平台，有免费层和企业层。***

## 支持向量机

一个**支持向量机**是一种监督分类技术，实际上可以变得非常复杂，但在最基本的层面上是非常直观的。

让我们假设有两类数据。支持向量机将找到一个**超平面**或两类数据之间的边界，以最大化两类数据之间的差距(见下文)。有许多平面可以分隔这两个类别，但只有一个平面可以最大化类别之间的边距或距离。

![](img/d6dbc0b7a28c7c62f684687fd5be0fc2.png)

如果你想了解更多细节，Savan 写了一篇关于支持向量机的文章[在这里](https://medium.com/machine-learning-101/chapter-2-svm-support-vector-machine-theory-f0812effc72)。

## 朴素贝叶斯

朴素贝叶斯是数据科学中使用的另一种流行的分类器。背后的想法是由贝叶斯定理驱动的:

![](img/4101429e618eafa24e19fa9ac4526bae.png)

说白了，这个等式就是用来回答下面这个问题的。“给定 X，y(我的输出变量)的概率是多少？因为假设变量是独立的，你可以说:

![](img/138fa2bee820af2e316f817610adb500.png)

同样，通过去掉分母，我们可以说 P(y|X)与右侧成正比。

![](img/5aec35d9f44a1824ba3241c417d87818.png)

因此，目标是找到具有最大比例概率的类 y。

*查看我的文章《* [*一个朴素贝叶斯的数学解释*](/a-mathematical-explanation-of-naive-bayes-in-5-minutes-44adebcdb5f8?source=friends_link&sk=595e755de72efcec9313b4888bdc6214) *》如果想要更深入的解释！*

## 决策树，随机森林，神经网络

这些模型遵循与前面解释的相同的逻辑。唯一区别是输出是离散的而不是连续的。

# 无监督学习

![](img/307a89c0453c3da15af0ac0d702ac587.png)

与监督学习不同，**非监督学习**用于从输入数据中进行推断和发现模式，而不参考标记的结果。无监督学习中使用的两种主要方法包括聚类和降维。

> ***如果您想测试这些 ML 算法，请查看*** [***土星云***](https://saturncloud.io/?utm_source=Youtube+&utm_medium=TDS&utm_campaign=Terence+Shin&utm_term=All+Machine+Learning+Models+Explained+in+6+Minutes) ***，这是一个可扩展、灵活的数据科学平台，有免费层和企业层。***

# 使聚集

![](img/114b4d851e53c735225a1778e4274d12.png)

摘自 GeeksforGeeks

聚类是一种无监督的技术，涉及数据点的分组或**聚类**。它经常用于客户细分、欺诈检测和文档分类。

常见的聚类技术有 **k-means** 聚类、**分层**聚类、**均值漂移**聚类、**基于密度的**聚类。虽然每种技术在寻找聚类时有不同的方法，但它们的目标都是一样的。

# 降维

降维是通过获得一组主变量来减少所考虑的随机变量的数量的过程[2]。简单来说，就是减少特性集的维数的过程(更简单来说，就是减少特性的数量)。大多数降维技术可以分为**特征消除**或**特征提取。**

一种流行的降维方法叫做**主成分分析。**

## 主成分分析

从最简单的意义上来说， **PCA** 涉及到将高维数据(如 3 维)投影到更小的空间(如 2 维)。这导致数据的维度降低(2 维而不是 3 维)，同时保持模型中的所有原始变量。

这涉及到相当多的数学问题。如果你想了解更多…

点击查看这篇关于 PCA [的精彩文章。](/a-one-stop-shop-for-principal-component-analysis-5582fb7e0a9c)

如果你更想看视频，StatQuest 在 5 分钟内解释 PCA[这里](https://www.youtube.com/watch?v=HMOI_lkzW08&vl=en)。

> ***如果您想测试这些 ML 算法，请查看*** [***土星云***](https://saturncloud.io/?utm_source=Medium+&utm_medium=TDS&utm_campaign=Terence+Shin&utm_term=All+Machine+Learning+Models+Explained+in+6+Minutes) ***，这是一个可扩展、灵活的数据科学平台，有免费层和企业层。***

# 感谢阅读！

> ***请务必*** [***订阅此处***](https://terenceshin.medium.com/membership) ***千万不要错过另一篇关于数据科学指南、技巧和提示、生活经验等的文章！***

我希望你觉得这很有趣，很有见地。在评论里让我知道你最喜欢的 2022 年的数据可视化是什么！

不确定接下来要读什么？我为你挑选了另一篇文章:

[](/over-100-data-scientist-interview-questions-and-answers-c5a66186769a) [## 超过 100 个数据科学家面试问题和答案！

### 来自亚马逊、谷歌、脸书、微软等公司的面试问题！

towardsdatascience.com](/over-100-data-scientist-interview-questions-and-answers-c5a66186769a) 

**或者你可以查看我的媒体页面:**

[](https://terenceshin.medium.com/) [## 特伦斯·申—中号

### 阅读特伦斯·申在媒体上的文章。数据科学@ KOHO，SaturnCloud |理学硕士，MBA |…

terenceshin.medium.com](https://terenceshin.medium.com/) 

# 特伦斯·申

*   [***同道@土星***](https://saturncloud.io/?utm_source=TerenceShinMedium+&utm_medium=Medium&utm_campaign=TerenceShinFellow&utm_term=the-10-best-data-visualizations-of-2022)
*   [***订阅我的媒体***](https://terenceshin.medium.com/membership)
*   [***跟我上媒***](https://medium.com/@terenceshin)
*   [***在 LinkedIn 上关注我***](https://www.linkedin.com/in/terenceshin/)