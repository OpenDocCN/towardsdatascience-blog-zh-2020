# t-SNE:数学背后

> 原文：<https://towardsdatascience.com/t-sne-behind-the-math-4d213b9ebab8?source=collection_archive---------27----------------------->

作为近年来谈论最多的降维算法之一，特别是对于可视化来说，我想我应该花一些时间来帮助其他人发展对 t-SNE 实际上在做什么的直觉。

由 Laurens van der Maatens 和 Geoffrey Hinton 于 2008 年开发的**t-Distributed random Neighbor Embedding**与主成分分析(PCA)不同，它是一种非线性方法，用于在人类可解释的维度(如 2D 或 3D)中可视化高维数据。

虽然主成分分析试图通过点的线性投影找到具有最大变化量的维度，但 PCA 有时无法在非线性情况下保持数据的内部结构，如正弦和圆柱关系。t-SNE 通过保存数据的内部结构克服了这一点。

t-SNE 试图保持点的邻域，即使它们是从高维转换到低维。该算法计算出彼此靠近的点和较远的点。邻域，即具有彼此接近的点的聚类被保留，并且即使存在多个聚类，这也保持良好。但是，变换后不会保留簇之间的距离。

**SNE 霸王龙在做什么？**

SNE 霸王龙试图找出这些点的相似程度，并根据相似程度对它们进行分组，因此能够保留数据的内部结构。

请看下图，t-SNE 计算出了点与点之间的距离，并将较近的点组合成较低维度的簇。

![](img/b39a5aff3368cb164b3fef711c1e5efc.png)

图一。使用 t-SNE l 从 2D 到 1D 的降维

**SNE 霸王龙是怎么做到的？**

对于一个查询点，假设是红色聚类中的一个点(图 1)，测量所有点之间的距离，并沿着学生的 T 分布绘制，该分布类似于高斯曲线，但具有更高的尾部。t 分布能够更好地区分相对于查询点更远的点，因为它们的尾部更高。

以查询点为中心，测量每个点相对于其他点的距离。一个点离查询点越远，它的距离就会远离曲线的峰值。位于峰值附近的点将被认为是查询点的邻居。

![](img/1eea593216fb448f1f4e67aaad92123e.png)

图 2: t-SNE 嵌入查询点的邻居

图 2 显示了 t-SNE 是如何计算出红色星团邻域的。对所有点重复这一过程，以找出多个聚类。

**使用 Python 的 PCA vs t-SNE**

我使用了 MNIST 的数据进行比较。MNIST 数据包括各种手写数字图像，有利于**光学字符识别。**

![](img/799f5fc9fbeddb8fbfe509f5ae6535c5.png)

图 3:执行 PCA 的代码

PCA 能够将 784 维数据转换成 2 维数据，但是可视化这是相当困难的。这些数字无法清楚地区分。

![](img/3f5e707d3605830cc0b18c27f512cb41.png)

图 MNSIT 数据上的 PCA |[图片由作者提供]

现在让我们试着用 SNE 霸王龙来想象。它应该能够比 PCA 更好地将相似的数字组合在一起。

![](img/44a34eb550267e14597e104e32b7eeb9.png)

图 5:执行 t-SNE 的代码

下面(图 6)是 t-SNE 的图，它比主成分分析更好地聚类了各种数字，我们可以清楚地看到哪个聚类属于哪个数字。

![](img/5ca1159dfee4da873cceb0b21b82b439.png)

图 6: t-SNE 在 MNSIT 数据上|[图片由作者提供]

SNE 霸王龙在哪里使用？

t-SNE 广泛用于高维数据的可视化，t-SNE 的一些应用领域如下:

1.癌症研究、生物信息学和音乐分析是 t-SNE 广泛用于可视化相似性和不可区分性的领域。

2.它广泛应用于图像处理、自然语言处理、基因组数据和语音处理中，用于从高维数据中获取相似性。

**t-SNE 的谬论**

1.作为一种随机算法，t-SNE 算法的结果在每次运行时都是不同的。

2.尽管 SNE 霸王龙能够保留数据的本地结构，但它可能无法保留数据的全局结构。

3.困惑度是邻域中要考虑的邻居数量，应该小于点数。理想情况下，范围从 5 到 50。

4.t-SNE 发现随机噪音的邻居，以及在低困惑，可以被误解。

**参考文献**

1.  YouTube。(2017 年 9 月 18 日)。StatQuest: t-SNE，解释清楚[视频文件]。从 https://www.youtube.com/watch?v=RJVL80Gg3lA[取回](https://www.youtube.com/watch?v=NEaUSP4YerM)
2.  如何有效使用 t 型 SNE:[https://distill.pub/2016/misread-tsne/](https://distill.pub/2016/misread-tsne/)
3.  t-SNE 算法综合指南及 R & Python 实现:[https://www . analyticsvidhya . com/blog/2017/01/t-SNE-implementation-R-Python/](https://www.analyticsvidhya.com/blog/2017/01/t-sne-implementation-r-python/)