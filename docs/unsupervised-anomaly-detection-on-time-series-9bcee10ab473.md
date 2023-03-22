# 时间序列上的无监督异常检测

> 原文：<https://towardsdatascience.com/unsupervised-anomaly-detection-on-time-series-9bcee10ab473?source=collection_archive---------3----------------------->

## 包括真实生活经历的概述

理解时间轴上任何流的正常行为并检测异常情况是数据驱动研究中的突出领域之一。这些研究大多是在无人监督的情况下进行的，因为在现实生活项目中标记数据是一个非常艰难的过程，如果你已经没有标签信息，就需要进行深入的回顾性分析。请记住，*异常值检测*和*异常值检测*在大多数时候可以互换使用。

没有一种神奇的银弹能在所有异常检测用例中表现良好。在这篇文章中，我谈到了一些基本的方法，这些方法主要用于以非监督的方式检测时间序列上的异常，并提到了它们的简单工作原理。在这个意义上，本文可以被认为是对包括现实生活经验在内的时间序列异常检测的一个综述。

![](img/46ac61f148d0136a384bc41a647d4e5b.png)

由[杰克·奈兹](https://unsplash.com/@jacknagz?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## **基于概率的方法**

使用[***Z-score***](https://en.wikipedia.org/wiki/Standard_score)是最直接的方法之一。z 分数基本上代表样本值低于或高于分布平均值的标准偏差的数量。它假设每个要素都符合正态分布，计算样本中每个要素的 z 值可以帮助我们发现异常。具有很多特征的样本，其值位于远离平均值的位置，很可能是异常。

在估计 z 分数时，您应该考虑影响模式的几个因素，以获得更可靠的推断。让我给你举个例子，你的目标是检测电信领域设备流量值的异常。小时信息、工作日信息、设备信息(如果数据集中存在多个设备)可能会形成流量值的模式。因此，在本例中，应该通过考虑每个设备、小时和工作日来估计 z 得分。例如，如果您预计设备 *A* 在周末晚上 8 点的平均流量为 2.5 mbps，那么您应该在决定相应的设备和时间信息时考虑该值。

这种方法的一个缺点是，它假设要素符合正态分布，而这并不总是正确的。另一个可以认为是忽略了上述解决方案中特征之间的相关性。*重要的一点是 z 分数也可以用作其他异常检测模型的输入。*

***基于四分位数的*** 解决方案实现了与 Z-score 非常相似的想法，不同的是它以简单的方式考虑了*中值*而不是*平均值*。有时，根据数据的分布情况，与 z 分数相比，它可以获得更好的结果。

[***椭圆包络***](https://scikit-learn.org/stable/modules/generated/sklearn.covariance.EllipticEnvelope.html) 是离群点检测的另一种选择，适合数据上的多元高斯分布。但是，它可能无法很好地处理高维数据。

## 基于预测的方法

在这种方法中，使用预测模型对下一个时间段进行预测，如果预测值超出置信区间，则样本被标记为异常。作为预测模型，可以使用[](/anomaly-detection-with-lstm-in-keras-8d8d7e50ab1b)*[***ARIMA***](/anomaly-detection-with-time-series-forecasting-c34c6d04b24a)模型。这些方法的优点是它们是时间序列上表现良好的模型，并且在大多数情况下可以直接应用于时间序列而无需特征工程步骤。另一方面，估计置信区间并不是一件简单的事情。此外，预测模型的准确性直接影响异常检测的成功。好消息是，即使在没有任何标签信息的情况下执行异常检测，您也能够以受监督的方式评估预测模型的准确性。*

*[***预言家***](https://medium.com/seismic-data-science/anomaly-detection-using-prophet-a5dcea2c5473)*也值得一看，它基本上是一个针对时间序列设计的预测算法，由脸书开发，但我在异常检测用例中遇到很多这种算法的实现。**

## **基于神经网络的方法**

**[***auto encoder***](/lstm-autoencoder-for-anomaly-detection-e1f4f2ee7ccf)*是一种无监督型神经网络，主要用于特征提取和降维。同时，对于异常检测问题，这是一个很好的选择。Autoencoder 由编码和解码部分组成。在编码部分，提取代表数据中模式的主要特征，然后在解码部分重构每个样本。正常样本的重建误差最小。另一方面，该模型不能重建表现异常的样本，导致高重建误差。因此，基本上，样本的重构误差越高，它就越有可能是异常的。***

***Autoencoder 对于时间序列是非常方便的，因此它也可以被认为是时间序列异常检测的优先选择之一。注意，自动编码器的层可以同时由 LSTMs 组成。因此，时序数据中的相关性就像时间序列中的相关性一样可以被捕获。***

******【SOM】***也是另一种基于无监督神经网络的实现，与其他神经网络模型相比，其工作原理更简单。尽管它在异常检测用例中没有广泛使用，但最好记住它也是一种替代方法。***

## **基于聚类的方法**

**在异常检测中使用聚类背后的思想是异常值不属于任何聚类或有自己的聚类。[***k-means***](https://neptune.ai/blog/anomaly-detection-in-time-series)是最著名的聚类算法之一，易于实现。然而，它带来了一些限制，如选择合适的 k 值。此外，它形成球形团簇，这并不适用于所有情况。另一个缺点是，它不能在将样本分配给聚类时提供概率，特别是考虑到在某些情况下聚类可能重叠。**

**[***高斯混合模型(GMM)***](/understanding-anomaly-detection-in-python-using-gaussian-mixture-model-e26e5d06094b) 针对 k-means 的上述弱点，提出一种概率方法。它试图在数据集中找到有限数量的高斯分布的混合。**

**[***DBSCAN***](https://medium.com/northraine/anomaly-detection-with-multi-dimensional-time-series-data-4fe8d111dee) 是一种基于密度的聚类算法。它确定数据集中的核心点，这些核心点在其周围*ε*距离内至少包含 *min_samples* ，并根据这些样本创建聚类。此后，它从聚类中的任何样本中找到所有密集可达(在*ε*距离内)的点，并将它们添加到聚类中。然后，迭代地对新添加的样本执行相同的过程，并扩展聚类。DBSCAN 自己确定聚类数，离群样本会被赋值为-1。换句话说，它直接服务于异常检测。请注意，对于大型数据集，它可能会遇到性能问题。**

## **基于邻近的方法**

**首先想到的算法是***k-最近邻(k-NN)*** 算法。背后的简单逻辑是异常值远离数据平面中的其余样本。估计所有样本到最近邻的距离，并且远离其他样本的样本可以被标记为异常值。k-NN 可以使用不同的距离度量，如*欧几里德距离、曼哈顿距离、闵可夫斯基距离、汉明距离*距离等。**

**另一种替代算法是*[***局部异常值因子(LOF)***](/local-outlier-factor-for-anomaly-detection-cc0c770d2ebe) ，其识别相对于局部邻居而非全局数据分布的局部异常值。它利用一个名为*局部可达性密度(lrd)* 的度量来表示每个点的密度水平。样本的 LOF 简单地说就是样本相邻样本的平均 *lrd* 值与样本本身的 *lrd* 值之比。如果一个点的密度远小于其相邻点的平均密度，那么它很可能是一个异常点。***

## **基于树的方法**

**[***隔离林***](/time-series-anomaly-detection-with-real-world-data-984c12a71acb) 是一种基于树的，非常有效的异常检测算法。它构建了多个树。为了构建树，它随机选取一个特征和相应特征的最小值和最大值内的分割值。此过程适用于数据集中的所有样本。最后，通过对森林中的所有树进行平均来构成树集合。**

**隔离森林背后的想法是，离群值很容易与数据集中的其余样本不同。由于这个原因，与数据集中的其余样本相比，我们预计异常样本从树根到树中的叶节点的路径更短(分离样本所需的分裂次数)。**

**[***扩展隔离林***](/outlier-detection-with-extended-isolation-forest-1e248a3fe97b) 对隔离林的分裂过程进行了改进。在隔离森林中，平行于轴进行分割，换句话说，以水平或垂直的方式在域中产生太多冗余区域，并且类似地在许多树的构造上。扩展隔离林通过允许在每个方向上发生分裂过程来弥补这些缺点，它不是选择具有随机分裂值的随机特征，而是选择随机法向量以及随机截取点。**

## **基于降维的方法**

*****主成分分析*** 主要用于高维数据的降维方法。基本上，通过提取具有最大特征值的特征向量，它有助于用较小的维度覆盖数据中的大部分方差。因此，它能够以非常小的维度保留数据中的大部分信息。**

**在异常检测中使用 PCA 时，它遵循与自动编码器非常相似的方法。首先，它将数据分解成一个更小的维度，然后再次从数据的分解版本中重建数据。异常样本往往具有较高的重建误差，因为它们与数据中的其他观测值具有不同的行为，因此很难从分解版本中获得相同的观测值。PCA 对于多变量异常检测场景是一个很好的选择。**

**![](img/4ab2a0af0b93c72837c762ef6b9adaf4.png)**

**时间序列的异常检测**

# ****真实生活经历****

*   **在开始研究之前，请回答以下问题:您有多少追溯数据？单变量还是多变量数据？进行异常检测的频率是多少？(接近实时，每小时，每周？)你应该对哪个单位进行异常检测？(例如，您正在研究流量值，您可能仅对设备或设备的每个插槽/端口进行异常检测)**
*   **您的数据有多项吗？让我澄清一下，假设您要对电信领域中上一个示例中的设备流量值执行异常检测。您可能有许多设备的流量值(可能有数千种不同的设备)，每种设备都有不同的模式，并且*您应该避免为每种设备设计不同的模型，以解决生产中的复杂性和维护问题*。在这种情况下，选择正确的功能比专注于尝试不同的模型更有用。考虑到小时、工作日/周末信息等属性，确定每个设备的模式，并从其模式中提取偏差(如 z 分数),并将这些特征提供给模型。注意*上下文异常*大多是在时间序列中处理的。所以，你可以只用一个真正珍贵的模型来处理这个问题。从预测的角度来看，基于[多头神经网络](https://nottingham-repository.worktribe.com/output/2315191/multi-head-cnnrnn-for-multi-time-series-anomaly-detection-an-industrial-case-study)的模型可以作为一种先进的解决方案。**
*   **在开始之前，如果可能的话，你必须向客户询问一些过去的异常例子。它会让你了解对你的期望。**
*   **异常的数量是另一个问题。大多数异常检测算法内部都有一个评分过程，因此您可以通过选择最佳阈值来调整异常的数量。大多数时候，客户不希望被太多的异常所打扰，即使它们是真正的异常。因此，*你可能需要一个单独的假阳性排除模块*。为简单起见，如果一个设备的流量模式为 10mbps，并且在某一点增加到 30mbps，那么这绝对是一个异常。然而，它可能不会比从 1gbps 提高到 1.3gbps 更受关注**
*   **在做出任何关于方法的决定之前，我建议至少对一个子样本的数据进行可视化，这将给出关于数据的深刻见解。**
*   **虽然有些方法直接接受时间序列，而没有任何预处理步骤，*您需要实施预处理或特征提取步骤，以便将数据转换成某些方法的方便格式*。**
*   **注意*新奇检测*和*异常检测*是不同的概念。简而言之，在新奇检测中，你有一个完全由正常观测值组成的数据集，并决定新接收的观测值是否符合训练集中的数据。与新奇检测不同，在异常检测中，训练集由正常样本和异常样本组成。 [***单类 SVM***](/outlier-detection-with-one-class-svms-5403a1a1878c) 对于新颖性检测问题可能是个不错的选择。**
*   **我鼓励看看 python 中的 [*pyod*](https://pyod.readthedocs.io/en/latest/) 和 [*pycaret*](https://pycaret.org/anomaly-detection/) 库，它们在异常检测方面提供了现成的解决方案。**

# **有用的链接**

**[](https://medium.com/learningdatascience/anomaly-detection-techniques-in-python-50f650c75aaf) [## Python 中的异常检测技术

### DBSCAN、隔离森林、局部异常因子、椭圆包络和一类 SVM

medium.com](https://medium.com/learningdatascience/anomaly-detection-techniques-in-python-50f650c75aaf) [](/detecting-the-onset-of-machine-failure-using-anomaly-detection-techniques-d2f7a11eb809) [## 使用异常检测技术检测机器故障的开始

### 介绍

towardsdatascience.com](/detecting-the-onset-of-machine-failure-using-anomaly-detection-techniques-d2f7a11eb809) [](/machine-learning-for-anomaly-detection-and-condition-monitoring-d4614e7de770) [## 用于异常检测和状态监控的机器学习

### 从数据导入到模型输出的分步教程

towardsdatascience.com](/machine-learning-for-anomaly-detection-and-condition-monitoring-d4614e7de770) [](/best-clustering-algorithms-for-anomaly-detection-d5b7412537c8) [## 用于异常检测的最佳聚类算法

### 让我首先解释一下任何通用的聚类算法是如何用于异常检测的。

towardsdatascience.com](/best-clustering-algorithms-for-anomaly-detection-d5b7412537c8) [](/anomaly-detection-for-dummies-15f148e559c1) [## 虚拟异常检测

### 单变量和多变量数据的无监督异常检测。

towardsdatascience.com](/anomaly-detection-for-dummies-15f148e559c1) [](/outlier-detection-theory-visualizations-and-code-a4fd39de540c) [## 异常值检测——理论、可视化和代码

### 五种算法来统治他们，五种算法来发现他们，五种算法来把他们带到黑暗中…

towardsdatascience.com](/outlier-detection-theory-visualizations-and-code-a4fd39de540c) [](/a-brief-overview-of-outlier-detection-techniques-1e0b2c19e561) [## 离群点检测技术概述

### 什么是离群值，如何处理？

towardsdatascience.com](/a-brief-overview-of-outlier-detection-techniques-1e0b2c19e561) [](/detecting-real-time-and-unsupervised-anomalies-in-streaming-data-a-starting-point-760a4bacbdf8) [## 检测流数据中的实时和无监督异常:一个起点

### 传感器通过在各种系统中收集数据以做出更明智的决策，实现了物联网(IoT)。数据…

towardsdatascience.com](/detecting-real-time-and-unsupervised-anomalies-in-streaming-data-a-starting-point-760a4bacbdf8) [](/time-series-of-price-anomaly-detection-13586cd5ff46) [## 价格异常检测的时间序列

### 异常检测会检测数据中与其余数据不匹配的数据点。

towardsdatascience.com](/time-series-of-price-anomaly-detection-13586cd5ff46)**