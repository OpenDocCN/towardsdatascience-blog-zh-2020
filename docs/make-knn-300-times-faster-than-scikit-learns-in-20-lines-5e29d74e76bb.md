# 用 20 行代码让 kNN 比 Scikit-learn 快 300 倍！

> 原文：<https://towardsdatascience.com/make-knn-300-times-faster-than-scikit-learns-in-20-lines-5e29d74e76bb?source=collection_archive---------7----------------------->

## 使用脸书·费斯库实现快速 kNN

![](img/56ad832356bfa6d762de6ad6af2df6bd.png)

我们可以通过 faiss 库([来源](https://pl.wikipedia.org/wiki/K_najbli%C5%BCszych_s%C4%85siad%C3%B3w#/media/Plik:KnnClassification.svg))更快地搜索最近邻居

## **简介**

k 近邻(kNN)是一种简单的用于分类和回归的 ML 算法。Scikit-learn 以一个非常简单的 API 为两个版本的特点，使其在机器学习课程中很受欢迎。它有一个问题——太慢了！但是不要担心，我们可以通过脸书·费斯库让它适用于更大的数据集。

kNN 算法必须为被分类的样本在训练集中找到最近的邻居。随着数据维度(要素数量)的增加，查找最近邻所需的时间会迅速增加。为了加速预测，在训练阶段(`.fit()`方法)，kNN 分类器创建数据结构，以更有组织的方式保持训练数据集，这将有助于最近邻搜索。

## **Scikit-learn vs faiss**

在 Scikit-learn 中，默认的“自动”模式会根据训练数据的大小和结构自动选择算法。它要么是强力搜索(针对非常小的数据集)，要么是最近邻查找的流行数据结构之一，k-d 树或球树。它们很简单，经常在计算几何课程中讲授，但是在 Scikit-learn 中实现它们的效率是值得怀疑的。例如，您可能已经在 kNN 教程中看到只选择 MNIST 数据集的一小部分，大约 10k 这是因为对于整个数据集(60k 图像)来说，这太慢了。今天，这甚至还没有接近“大数据”！

幸运的是，脸书人工智能研究所(FAIR)提出了最近邻搜索算法的优秀实现，可以在 faiss 库中找到。faiss 提供 CPU 和 GPU 支持，许多不同的指标，利用多个核心，GPU 和机器等等。有了它，我们可以实现比 Scikit-learn 的不是快几倍，而是快几个数量级的 k 近邻分类器！

## **用 faiss 实现 kNN 分类器**

如果你对下面的 Github 要点有困难，代码也可以在我的 Github ( [链接](https://github.com/j-adamczyk/Towards_Data_Science/blob/master/faiss/kNN.py))获得。

faiss 的一个很大的特点是它既有安装和构建说明([安装文档](https://github.com/facebookresearch/faiss/blob/master/INSTALL.md))又有一个优秀的带示例的文档([g](https://github.com/facebookresearch/faiss/wiki/Getting-started)etting[started docs](https://github.com/facebookresearch/faiss/wiki/Getting-started))。安装完成后，我们可以编写实际的分类器。代码非常简单，因为我们只是模仿了 Scikit-learn API。

这里有一些有趣的元素:

*   属性保存 faiss 创建的数据结构，以加速最近邻搜索
*   我们放入索引**的数据**是 Numpy float32 类型
*   我们这里用的`IndexFlatL2`，是最简单的欧氏距离(L2 范数)的精确最近邻搜索，非常类似于默认的 Scikit-learn`KNeighborsClassifier`；您还可以使用其他指标([指标文档](https://github.com/facebookresearch/faiss/wiki/MetricType-and-distances))和索引类型([索引文档](https://github.com/facebookresearch/faiss/wiki/Faiss-indexes))，例如用于近似最近邻搜索
*   `.search()`方法返回到 k 个最近邻居的距离及其索引；我们只关心这里的索引，但是你可以用附加信息实现距离加权最近邻
*   `.search()`返回的索引是 2D 矩阵，其中第 n 行包含 k 个最近邻的索引；使用`self.y[indices]`，我们将这些索引转化为最近邻的类别，因此我们知道每个样本的投票数
*   `np.argmax(np.bincount(x))`从 x 数组中返回最受欢迎的数字，即预测类；我们对每一行都这样做，也就是说，对我们必须分类的每个样本都这样做

## **时间对比**

我选择了 Scikit-learn 中的一些流行数据集进行比较。比较训练时间和预测时间。为了便于阅读，我明确地写了基于 faiss 的分类器比 Scikit-learn 的快多少倍。

所有这些时间都是用`time.process_time()`函数测量的，该函数测量进程时间而不是挂钟时间，以获得更准确的结果。结果是 5 次运行的平均值。

![](img/ff41650c42b47a1c5d6e961a2e3b7057.png)

火车时间(图片由作者提供)

![](img/f7f0b8788b7a385a2d61dab94f0f22e6.png)

预测时间(图片由作者提供)

训练平均快了差不多 300 倍，而预测平均快了 7.5 倍左右。另请注意，对于 MNIST 数据集，其大小与现代数据集相当，我们获得了 17 倍的加速，这是巨大的。10 分钟后我就准备放弃用 Scikit-learn 了，差不多花了 15 分钟 CPU 时间(挂钟更长)！

## 摘要

有了 20 行代码，我们用 faiss 库获得了 kNN 分类器的巨大速度提升。如果你需要，你可以用 GPU、多个 GPU、近似最近邻搜索等等做得更好，这在 faiss 文档中有很好的解释。