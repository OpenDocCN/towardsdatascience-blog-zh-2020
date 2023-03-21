# 动手 PCA 数据预处理系列。第二部分:异常值处理

> 原文：<https://towardsdatascience.com/scaling-outliers-handling-categorical-data-encoding-the-pca-hands-on-skills-series-part-ii-46e712e240eb?source=collection_archive---------51----------------------->

## 尝试使用 PCA 但停留在处理阶段？看看这个。

![](img/f9d47e6f2a3bb4cc65823f78acc89faa.png)

[王思韵](https://medium.com/u/9377014bd7ca?source=post_page-----46e712e240eb--------------------------------)(本文合著者)。树枝上毛茸茸的小鸟【水彩】(2020)。我们用低聚艺术来表示 PCA 的感觉，就是降维。但是 PCA 能做的不止这些。

# 系列介绍

在本系列中，我们将探索缩放数据和 PCA 的结合。我们希望看到，每当我们遇到新的数据集时，我们如何才能更好地为机器学习任务准备数据。旅程由三部分组成。

*   第一部分:定标器和 PCA
*   第二部分:认识离群值
*   第三部分:分类数据编码

# 我们将在这篇文章中做什么

1.  介绍/回顾要处理的数据集和任务
2.  向原始数据集添加合成异常值
3.  对修改后的数据集执行缩放变换
4.  对缩放变换后的数据集进行主成分分析并评估性能

# 你将学到什么

*   理解定标器的重要性及其与 PCA 的密切关系
*   明智地选择定标器，尤其是当存在异常值时
*   使相关和漂亮的可视化:)

> *在你开始阅读之前，我们强烈建议你先玩玩这个笔记本(在 Colab 和 Github 上都有。请在这篇文章的末尾找到链接。)*

# 遇到离群值

通过 scaler + PCA 组合继续我们的旅程，我们现在到达站 II。这一次，我们将用一些现实生活中经常遇到的障碍，比如离群值，来挑战缩放工具。

我们将向 wine 数据集添加几个合成异常值，并展示我们工具包中的每个定标器将如何反应。

> 请注意，异常值处理背后有丰富的理论。然而，探究科学的细节并不是我们这里的重点。

像往常一样，让我们通过导入 Python 包和定义我们的助手函数来做准备。你可能想看看这个系列的第一部分。

[](https://medium.com/coffee-sucre/pca-a-practical-journey-preprocessing-encoding-and-inspiring-applications-64371cb134a) [## 缩放变压器。动手 PCA 数据预处理系列。(第一部分)

### 第一部分:定标器和 PCA

medium.com](https://medium.com/coffee-sucre/pca-a-practical-journey-preprocessing-encoding-and-inspiring-applications-64371cb134a) 

# 第一眼

我们继续使用[葡萄酒数据集](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_wine.html)。这些数据是对意大利同一地区三个不同种植者种植的葡萄酒进行化学分析的结果。对于三种类型(产地)葡萄酒中的各种成分，有十三种不同的测量方法。数据集仅包含数字要素。**我们的目标是使用 13 种不同的测量方法找出目标标签，例如，原点在哪里。**

winds 数据集是一个相当友好的数据集，如此友好，以至于使用任何缩放器进行预处理都几乎不会出错。但是，如果我们在集合中添加一些合成噪声/异常值来折磨我们的定标器会怎么样呢？所以我们随机选择三个特征，分别加入十个离群值。对于其余的特征，我们合成了与真实数据“相似”的十个值。但是首先，让我们在添加合成数据之前回顾一下原始数据集。

```
df_wine.describe()g = sns.pairplot(data=df_wine, 
                 vars=df_features.columns, 
                 hue='target_original',            
                 corner=True, palette=customer_palette,   
                 hue_order=target_order)
```

![](img/de7af56fb74f37d41c45051787cfacf3.png)

(提示:在单独的窗口中打开以获得更好的视图)

现在，修改的数据集(添加合成噪声/异常值)。

```
df_wine_new.describe()g = sns.pairplot(data=df_wine_new, vars=df_features.columns, hue='target_original',
                 corner=True, palette=customer_palette, hue_order=target_order)
```

![](img/82c0e4ee7431f303331ce2af08906521.png)

(提示:在单独的窗口中打开以获得更好的视图)

> 由于人工痕迹，合成数据往往太容易被发现，有时使用此类数据得出的结论可能不太适用于真实数据集。(博客的作者之一非常不喜欢他们；p)。但是出于实验的目的，我们仍然使用 Numpy 的选择函数来生成异常值。我们尽最大努力使合成数据尽可能真实。

![](img/bb1b9ba6274a06115d711d041e1a552c.png)

**思考时间**

> 想知道哪三个是被选中的吗？留下评论或玩提供的笔记本检查。；P

让我们画出协方差矩阵。

```
X_train_o = df_wine_new.iloc[:,:-4].copy()
y_train_o = df_wine_new.iloc[:,-4:].copy()# the following are customer functions. 
# refer to the notebook to see what the do.X_trans_dict_o, X_pca_dict_o = transformer_bundle(X_train_o)trans_heat_plot(X_trans_dict_o, y_axis_labels=df_features.columns)
```

![](img/8d01c5e8c993ecd34fbe674e66e78b89.png)

回想一下，在本系列的第一部分，我们提到过:**“经验法则是，热图越丰富多彩，PCA 结果越好。通常情况下，主成分分析不喜欢简单的热图，会输出不太有趣的主成分。**这一次，我们邀请您特别关注鲁棒的定标器。请注意，热图的颜色条上升到最大值 17.5，添加了异常值的三个选定变量从图中突出，压倒了其余变量。这是因为鲁棒定标器采用每个变量的中间 50%部分(第 1 至第 3 个四分位数)来计算方差，并根据中值将值居中。

虽然健壮的定标器与标准定标器一样是线性定标器，但它排除了异常值的影响。因此，它保留了数据“正常”部分的结构，并强调了异常值，因此三个扰动变量的方差很大。热图的其余部分看起来很枯燥，这表明缩放器将所有变量的非异常值信息保持在相似的比例，而不管异常值。

另一方面，它的标准 scaler 表兄弟几乎破坏了三个所选列的非异常值的方差，使得三个列的信息部分与其他列相比不可用。然而，主成分分析会被这种转换所迷惑，对这三个变量给予了错误的关注(理论上，超过了应该给予的关注)。

> 即使我们试图描述舞台背后发生的事情，但最好还是玩玩笔记本，确认你的直觉，尤其是检查转换前后每个特征的方差。

# 开始任务

接下来，我们对缩放变换后的数据执行 PCA。我们将了解不同的定标器如何影响 PCA 结果。

让我们想象一下关于每个定标器的前两个主要部分。

```
# the following are customer functions. 
# refer to the notebook to see what the do.X_trans_dict_o, X_pca_dict_o = transformer_bundle(X_train_o)
pca_scatter_plot(X_pca_dict_o, y_train_o.iloc[:,1:],    
                 highlight_list=np.arange(-10,0))
```

![](img/38a32877e5032b99488276b6f8cc3f12.png)

由于异常值的存在，不同变压器的性能比较变得更加令人兴奋。；)

**在多类分类的情况下，我们希望结果少受离群点的影响。(即，离群值不会独立于现有的三个类而形成新的类，并且“自然地”融入到这些类中。)**两个转换器通过了这个测试:分位数转换器和最小-最大转换器。

**另一方面，如果我们要做某种异常检测(比如说目标是识别不合格的葡萄酒，这是无监督学习的一个分支)，我们会更喜欢让异常值更加突出的变压器，**像标准缩放器和鲁棒缩放器。

**或者，如果我们希望转换后的数据不被粉碎，换句话说，保持球形，**那么我们还应该包括一个电源变压器。

请注意，这不是一个普遍的结论，因为我们所做的非常有限，我们没有涵盖合成异常值的其他可能方法。我们可以有把握地建议测试一大堆变压器(线性或非线性的),找到更适合问题的那一个。

在接下来的部分中，我们使用真实标签和 K-means 聚类标签来可视化增强的数据集。

首先，让我们把它看作一个分类问题，因为它本来就是这样的。

```
# set n_clusters=3, plot using customer function
kmeans = KMeans(n_clusters=3, random_state=RANDOM_STATE)
pca_cluster_contour_plot(X_pca_dict_o, y_train_o, kmeans)
```

![](img/1d303e0219817eef86d7a27acf9e4a30.png)

等高线是每个聚类的区域，彩色点是带有真实标签的数据点。

从这个测试来看，几乎所有的线性转换器都会导致一个“坏”的结果:真实的数据点被异常值挤压得太厉害，以至于 K-means 甚至找不到 3 个聚类。相反，它只返回 2。在这个测试中，我们可能更喜欢分位数转换器、幂转换器和最小最大值转换器。

> **详细说明:**像 K-means 这样的聚类方法是无监督的学习模型，不需要地面真实(即真实标签)的信息来学习。这样做的副作用是模型看不到绝对标签值的差异。例如，预测结果[0，0，1，2，2]与[1，1，2，0，0]基本相同。当我们使用指数评估结果时，这不是问题，比如 [NMI](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.normalized_mutual_info_score.html) 、 [AMI](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.adjusted_mutual_info_score.html) 、 [ARI](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.adjusted_rand_score.html#sklearn.metrics.adjusted_rand_score) 或 [V-measure score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.v_measure_score.html) 。但是这对于可视化来说就成了一个问题，因为……好吧，标签[0，0，1，2，2]肯定不等同于[1，1，2，0，0]当每个标签都与某种视觉辅助相关联时，比如颜色或形状。

![](img/e2ac478f43bb22498cf2c21345ef14ed.png)

资源:[https://i.imgur.com/](https://i.imgur.com/)

> 在这里，我(饰演柯飞)试图以一种工程化的方式将预测标签与事实标签相匹配。我不得不承认，这种做法远非“科学”的方法。事实上，我不认为有一种“科学”的方法可以做到这一点，特别是当唯一的预测标签的数量与唯一的真实标签的数量不同时。但我对匹配的结果已经足够满意了。如果你对我如何匹配标签感到好奇，去玩笔记本吧。如果你想出了更复杂的标签匹配方法，请在评论中分享你的想法。

现在，让我们考虑一个异常检测问题。注意，我们通常不使用 K-means 来解决异常检测任务。相反，我们可以使用 DBSCAN、孤立森林或其他算法来检测数据集中的噪声。但是由于我们先验地*知道有三个类，在这种情况下，可以通过增加 K-means 中的 K 值来检测离群值(例如，原始目标标签的三个聚类，离群值的额外的一个或多个)。下面是相同的图，但是 K=4。*

```
# set n_clusters=4, plot using customer function
kmeans = KMeans(n_clusters=4, random_state=RANDOM_STATE)
pca_cluster_contour_plot(X_pca_dict_o, y_train_o, kmeans)
```

![](img/fb6d34ef02e2ca61ac6d74dd9e384b72.png)

在评估性能之前，让我们先解决这个问题。

## **为什么剧情里有诡异的标签？(例如，' pred_1001.0 '，' pred_999.0')**

如前所述，我们试图将 k-means 预测标签值与地面真实标签值进行匹配。其方法是找出在预测的标签区域(轮廓)内关联的真实标签(聚类)是什么。例如，查看 QuantileTransformer 图，在红色轮廓区域内，红色散射占主导地位，因此该轮廓将被命名为与红色相关联的“0”。

但是‘pred _ 1001.0’标签怎么了？你可能注意到，在紫色的“pred_1001.0”轮廓中，蓝色散射占主导地位。但是由于相关联的标签值“1”已经被蓝色轮廓“pred_1”占用，我们不能将紫色和蓝色轮廓都命名为“pred_1 ”,否则它们将合并为一个。(你可能已经猜对了:‘pred _ 1001.0’来自‘pred _ 1000+1’，dummy 来自‘pred _ 1’；' pred_999.0 '来自' pred_1000+-1 '，一个来自' pred_-1 '的哑元。)

## **那么，我们如何从剧情上分析表演呢？**

如果我们最关心的是异常值，那么焦点就在“pred_-1”轮廓上。例如,‘pred _-1’轮廓是否包含大多数离群值。以异常值检测为目的，我们更喜欢标准的定标器、max-abs 定标器和电源变压器(甚至规格化器也不错；)).

请注意，电源变压器的性能非常出色。它不仅检测到了所有的异常值(虽然得到了一个额外的非异常值)，而且在非异常值中实现了非常高的准确性。正因如此，这次测试的赢家是 PowerTransformer。

**好的，分析中有很多信息。我会给你一些时间来评估情节。**

![](img/3a93d850a1943807f1db087dca5c907c.png)

资源:mem-arsenal.ru

> **备注:**值得一提的是，由于我们是通过进一步推取一些变量的最大值来生成异常值的，因此异常值会在原始数据的一侧形成一组，如上图所示。然而，如果我们将合成数据的方式从单调改变为多方向，也就是说使一些最小值变得更小，我们将会看到异常值遍布整个特征空间。在这种情况下，K-means 不再适合，因为我们不知道离群值会形成多少个组。

# 摘要

我们已经使用合成异常值测试了我们的定标器；每个定标器对异常值的反应方式各不相同。根据您的任务来判断每个缩放器的性能会更加谨慎。

正如您所看到的，变压器区分异常值(即，不像使用 minMaxScaler 时那样混合异常值和非异常值)和保持非异常值方差(即，不像使用 standardScaler 时那样被粉碎)的能力是一种权衡。很多时候，根据个人喜好和具体情况来决定哪种转换器最合适。

![](img/399036369b71f09965d58cd8b4d0d2f4.png)

**扩展知识**

对于离群值来说足够了。下一次，我们将测试 PCA 的极限，看看它对分类数据的反应。(是的，PCA 可以处理分类数据。)

我希望你喜欢这篇文章。请随时留下评论。

> 声明:该系列由莫克菲和[王思韵](https://medium.com/u/9377014bd7ca?source=post_page-----46e712e240eb--------------------------------)合作。第一次发布:2020 年 6 月 9 日。

# 笔记本链接:

 [## blog _ PCA _ part II-1 _ kefei _ 0605 . ipynb

### 编辑描述

drive.google.com](https://drive.google.com/file/d/15EWUv-h9qlEg86Q7GGg6la-mzs31xEV4/view?usp=sharing) [](https://github.com/kefeimo/DataScienceBlog/blob/master/1.%20PCA/blog_PCA_part%20II-1_kefei_0605.ipynb) [## kefeimo/数据科学博客

### permalink dissolve GitHub 是超过 5000 万开发人员的家园，他们一起工作来托管和审查代码，管理…

github.com](https://github.com/kefeimo/DataScienceBlog/blob/master/1.%20PCA/blog_PCA_part%20II-1_kefei_0605.ipynb)