# 举例说明 KMeans 超参数

> 原文：<https://towardsdatascience.com/kmeans-hyper-parameters-explained-with-examples-c93505820cd3?source=collection_archive---------5----------------------->

![](img/fea0e0d146dbaba197869177162a1255.png)

图片来源:失眠症患者/索尼

如今，任何高中学历的人都可以实现机器学习算法，这是一件好事。大多数人都能接触到的东西总是一件好事。有很多公共库可以用来简化算法部分。但是权力越大，责任越大。从头开始编写库所节省的时间应该用来微调您的模型，这样您就可以获得不错的结果。

今天我们来看看 [Scikit KMeans 模型](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html)的调校。KMeans 是一种广泛使用的数据聚类算法:如果您希望根据客户的购买行为将大量客户聚类到相似的组中，您可以使用 KMeans。如果希望根据人口统计和兴趣对所有加拿大人进行聚类，可以使用 KMeans。如果你想根据植物或葡萄酒的特征对它们进行分类，你可以使用 KMeans。所有这些问题都需要无监督聚类，也就是说，我们事先不知道聚类看起来是什么样子。还有其他无监督聚类的方法，如 DBScan、层次聚类等，它们各有优点，但在这篇文章中，我将讨论 KMeans，因为它是一种计算量很小的聚类方法，你可以经常在笔记本电脑上运行，特别是使用[迷你批处理 KMeans](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.MiniBatchKMeans.html) 。

完整代码的链接在最后。

# 示例的样本数据

我们生成 3 个具有 2 个特征的聚类(0，1，2)(*xx*和 *yy* )。聚类由以下 3 对 x，y 值生成:

```
mu1 = 3.0
sigma1 = 0.5
mu2 = 20.0
sigma2 = 5.5
mu3a = 1.0
sigma3a = 0.8
mu3b = 5.0
sigma3b = 0.4
numpoints = 1000
np.random.seed(1234)
x1 = np.random.normal(mu1, sigma1, numpoints)
y1 = np.random.normal(mu1, sigma1, numpoints)
x2 = np.random.normal(mu2, sigma2, numpoints)
y2 = np.random.normal(mu2, sigma2, numpoints)
x3 = np.random.normal(mu3a, sigma3a, numpoints)
y3 = np.random.normal(mu3b, sigma3b, numpoints)
```

![](img/8ed96fa2b54a6c4ad3ba5ad45ac6c6fb.png)

我们有三组蓝色、栗色和红色的数据。左图显示了所有数据。右图显示了一个放大的版本，因此我们可以清楚地看到有两个集群。

# 超参数

超参数来自 [Scikit 的 KMeans](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html?highlight=kmeans#sklearn.cluster.KMeans) :

```
*class* sklearn.cluster.**KMeans**(*n_clusters=8*, *init='k-means++'*, *n_init=10*, *max_iter=300*, *tol=0.0001*, *precompute_distances='auto'*, *verbose=0*, *random_state=None*, *copy_x=True*, *n_jobs=None*, *algorithm='auto'*)
```

## 随机状态

这是设置一个随机种子。如果我们想要一次又一次地复制精确的集群，这是很有用的。我们可以把它设置成任何我们想要的数字。下面我设置为 *random_state=1234* 。

## n _ 簇

我们需要为算法提供我们想要的聚类数。标准文献建议我们使用[肘方法](https://www.scikit-yb.org/en/latest/api/cluster/elbow.html)来确定我们需要多少个集群，并且它对于 Scikits 的干净的理论数据集很有效。实际上，这只是一个初步的猜测。在这个例子中，我们知道我们有 3 个集群。所以让我们用 *n_clusters=3* 试试:

```
km = KMeans(n_clusters=3, random_state=1234).fit(dftmp.loc[:, dftmp.columns != ‘group’])
```

![](img/7e7e06315f9de33d651974f113375a5f.png)

这是怎么回事？我们预测了 3 个簇，但是它们不在我们最初的 3 个簇附近。

我们确实得到了 3 个集群，但是它们与我们最初的集群非常不同。最初，我们在左下角有两个集群，但它们都被归入一个集群，黄色的圆圈。这是因为 KMeans 随机指定初始聚类质心，然后根据点到质心的距离尝试对尽可能多的点进行分组。当然，它重复这个过程直到收敛，但是没有什么能阻止它陷入局部最小值。事实上，众所周知，KMeans 依赖于质心初始化。这是我们的第一个线索，我们不能盲目地使用知识手段。如果我们要求 3 个聚类，那么我们需要有一些概念，我们期望聚类中心在所有特征中的位置。

## 初始化

这是您可以设置初始簇质心的地方。在我们的例子中，我们有 3 个集群，所以我们需要 3 个质心阵列。因为我们有两个特征，每个数组的长度都是 2。所以在我们的例子中，我们必须有 3 对聚类中心。因为这是一个模拟，所以我们知道准确的聚类中心，所以让我们试试。

```
centroids = np.asarray([[mu1,mu1],[mu2,mu2], [mu3a,mu3b]])
km = KMeans(n_clusters=3, init=centroids, random_state=1234).fit(dftmp.loc[:, dftmp.columns != 'group'])
```

![](img/c7a820f5e4086e1d3beceaf76a18b5e7.png)

最后，我们有了最初的 3 个集群！(通过将我们的聚类中心初始化为原始值)

啊！我们走吧。对于大多数数据点，我们都有原始的 3 个聚类！

但是等等！那是作弊！我们永远不会知道我们星团的精确质心。没错。我们只能推测我们的聚类中心大概是什么。所以你不必知道精确的中心，但是近似值会有帮助。

如果我们只有近似的聚类中心，我们还能做什么来改进我们的聚类？接下来，我们将看看另一种方法，我们可以得到我们的原始集群。

## 更改群集的数量

我知道！你想，但是，但是，但是，我们最初有 3 个集群，我们将集群的数量设置为 3。我们还能做什么？不如我们把集群的数量设为预期的两倍。什么？我知道，请原谅我。

```
km = KMeans(n_clusters=numclusters, random_state=1234).fit(dftmp.loc[:, dftmp.columns != 'group'])
```

![](img/523e035b68d62215193c85e428508b02.png)

啊！左下方有两个分开的集群，如底部一行图所示。我们不需要给出初始的聚类中心。

请看右下方显示的原始群集 0 和 2，它们或多或少被很好地分开了。所以这是一件好事。但是原来的集群 1(右上)现在被分成 4 个集群。但是我们知道这只是我们模拟的一个集群。因此，接下来我们只需将这些集群整合在一起。类似集群(紫色、棕色、深绿色和浅绿色)的东西将成为单个集群。

因此，如果我们不知道我们的聚类中心在哪里，这可能是许多特征的情况，那么我们可以使用这个技巧-高估我们的聚类数量。有人可能会说这是过度拟合——这对于右上的簇来说是正确的，但这是在没有好的质心的情况下，我们可以让左下的簇分离的唯一方法。即使我们有近似的质心，这种方法也能更好地增强聚类分离。

## 标准化数据

你说:“但是等等！我知道你必须对你的数据进行标准化。事实上这是真的。无论何时进行任何涉及欧几里得空间的操作，您都必须对数据进行归一化，以便所有的特征都在相同的范围内，KMeans 就是这样做的。在这种情况下，我能够逃脱，因为我的原始数据的 x 和 y 范围看起来差不多:0–30(参见上面样本数据部分的第一个图表)。但是让我们将数据标准化，看看是否有所不同。

```
scl = StandardScaler()
dftmparray = (dftmp.loc[:, dftmp.columns != 'group']).values
dfnorm = scl.fit_transform(dftmparray)
dfnorm = pd.DataFrame(dfnorm)
km = KMeans(n_clusters=3, random_state=1234).fit(dfnorm)
```

![](img/f4533f626aab6a6e4694cb3061266ec5.png)

我们不能预测底部坐标较低的独立星团。右上角显示了原始空间中两个聚类的分离，但右下角显示这两个聚类在预测中没有很好地分离。

我们看到，即使我们对数据进行了归一化，我们仍然没有成功地分离原始聚类 0 和 2(在原始数据的左下方)。

## 其他人

还有其他超参数，如 tol、max_iter，有助于缩短计算时间。这些参数在一个更复杂的问题中变得比这个例子中显示的更重要，所以我不会试图通过例子来展示它们。

但是让我们看看它们的含义:

n_init =默认为 10，因此算法将初始化质心 10 次，并将选择最收敛的值作为最佳拟合。增加该值以扫描整个特征空间。注意，如果我们提供了质心，那么算法将只运行一次；事实上，它会在运行时警告我们这一点。如果我们设置了初始质心，或者如果我们设置的聚类数比我们预期的要多(为了以后合并一些聚类，如上所述)，那么我们可以保留默认值。

tol =如果我们将它设置为一个更高的值，那么它意味着在我们宣布收敛之前，我们愿意容忍惯性的更大变化，或损失的变化(有点像我们收敛的速度)。因此，如果惯性的变化小于 tol 指定的值，那么算法将停止迭代，并宣布收敛，即使它已经完成了少于 max_iter 轮次。将其保持在较低的值，以扫描整个特征空间。

max_iter =通常有 n_init 次运行，并且每次运行迭代 max_iter 次，即，在一次运行中，点将被分配给不同的簇，并且针对 max_iter 次计算损失。如果将 max_iter 保持在一个较高的值，那么可以保证您已经探索了整个特征空间，但这通常是以收益递减为代价的。

其他变量决定计算效率，因此如果您有一个非常大的数据集，最好保持默认值。

## 结论

仅仅运行一个肘方法、确定集群的数量以及仅仅运行标准的 KMeans 通常是不够的。一般来说，我们必须研究数据，并获得主题专家对必须有多少个群集以及它们的近似质心应该是多少的意见。一旦我们有了这些，我们就可以将它们放在一起调整 KMeans:

1.  通过提供初始聚类中心
2.  通过要求更多的集群，以便我们可以在事后整合一些集群。
3.  我们还可以通过增加一些特征的权重来增加它们的贡献，试图用 [Mahlanobis](https://en.wikipedia.org/wiki/Mahalanobis_distance) 距离来代替欧几里德距离。例如，在上面的例子中，如果我们认为 *xx* 在分离中比 *yy、*重要 10 倍，那么我们将在归一化步骤后将 *xx* 乘以 10。虽然这并不总是可取的——还有其他方法来处理这个问题，例如 PCA。

重现上述情节的完整 python 代码[可以在这里找到。Jupyter 笔记本](https://bitbucket.org/sujeewak/jupyternotebooks/src/dev/kmeans_0511.py)的 pdf 版本可以在这里找到。