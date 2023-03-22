# 数据挖掘中的降维

> 原文：<https://towardsdatascience.com/dimensionality-reduction-in-data-mining-f08c734b3001?source=collection_archive---------16----------------------->

大数据是具有多级变量的大规模数据集，并且增长非常快。体量是大数据最重要的方面。随着数据处理和计算机科学领域最近的技术进步，最近大数据在记录数量和属性方面的爆炸式增长给数据挖掘和数据科学带来了一些挑战。大数据中极大规模的数据形成多维数据集。在大型数据集中有多个维度，这使得分析这些维度或在数据中寻找任何类型的模式的工作变得非常困难。高维数据可以从各种来源获得，这取决于人们对哪种过程感兴趣。自然界中的任何过程都是许多不同变量的结果，其中一些变量是可观察或可测量的，而另一些是不可观察或可测量的。当我们要准确地获得任何类型的模拟数据时，我们就要处理更高维度的数据。

![](img/ff56c2e98cecd95eb51bca04b2c37c03.png)

克林特·王茂林在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

降维是减少所考虑的随机变量或属性数量的过程。作为数据预处理步骤的一部分，高维数据约简在许多现实应用中是极其重要的。高维降维已经成为数据挖掘应用中的重要任务之一。例如，您可能有一个包含数百个要素(数据库中的列)的数据集。那么，降维就是通过组合或合并来减少数据属性的特征，这样就不会丢失原始数据集的许多重要特征。高维数据的一个主要问题是众所周知的“维数灾难”。如果我们想使用数据进行分析，这就促使我们降低数据的维度。

## 维度的诅咒

维数灾难指的是当[对](https://deepai.org/machine-learning-glossary-and-terms/classifier)进行分类、组织和分析[高维数据](https://deepai.org/machine-learning-glossary-and-terms/high-dimensional-data)时出现的现象，这种现象在低维空间中不会出现，具体来说就是数据稀疏性和数据的“紧密性”问题。

![](img/a47526b03f6f7de14c5ec7999c2dbdfe.png)

获得的空间与总空间之间的差异(1/3、1/9、1/27)

上面的图表序列显示了当基础维度增加时数据的紧密性问题。随着上面看到的数据空间从一维移动到二维并最终移动到三维，给定的数据填充的数据空间越来越少。为了保持空间的精确表示所需的数据量随着维度成指数增长。

![](img/2ff1d07fd834e632cfbcf507c344f19f.png)

数据稀疏导致相似性降低

当维数增加时，随着稀疏度的增加，两个独立点之间的距离增加。这导致数据点之间的相似性降低，这将导致大多数机器学习和数据挖掘中使用的其他技术出现更多错误。为了进行补偿，我们将不得不输入大量的数据点，但是如果维数更高，这实际上是不可能的，即使有可能，效率也会很低。

为了克服上述高维数据带来的挑战，需要降低计划分析和可视化的数据的维度。

## 降维技术

基于**特征选择**或**特征提取**完成维度缩减。特征选择基于从可用测量中省略那些对类别可分性没有贡献的特征。换句话说，冗余和不相关的特征被忽略。另一方面，特征提取考虑整个信息内容，并将有用的信息内容映射到较低维度的特征空间。

人们可以将用于降维的技术区分为线性技术和非线性技术。但是这里将基于特征选择和特征提取的观点来描述这些技术。

## 特征选择技术

![](img/de3791397fa5897073272e9ef013f6f5.png)

特征选择过程

作为一项独立的任务，特征选择可以是无监督的(例如方差阈值)或有监督的(例如遗传算法)。如果需要，还可以结合使用多种方法。

## 1)方差阈值

这种技术寻找给定特征的从一个观察到另一个观察的变化，然后如果根据给定的阈值，在每个观察中的变化没有不同，则导致该观察的特征被移除。变化不大的功能并没有增加多少有效信息。在建模过程开始时，使用方差阈值是一种简单且相对安全的降维方法。但是，如果您想要减少维度，这本身是不够的，因为它非常主观，您需要手动调整方差阈值。这种特性选择可以使用 Python 和 r 来实现。

## 2)相关阈值

这里考虑这些特征，并检查这些特征是否彼此紧密相关。如果是这样的话，那么这两个特性对最终输出的总体影响甚至与我们使用其中一个特性时得到的结果相似。你应该删除哪一个？你首先要计算所有的成对相关。然后，如果一对要素之间的相关性高于给定的阈值，您将移除与其他要素具有较大平均绝对相关性的要素。像前面的技术一样，这也是基于直觉，因此以不忽略有用信息的方式调整阈值的负担将落在用户身上。由于这些原因，具有内置特征选择的算法或像 PCA 这样的算法比这种算法更受欢迎。

## 3)遗传算法

它们是受进化生物学和自然选择启发的搜索算法，结合变异和交叉来有效地遍历大型解决方案空间。遗传算法用于寻找最佳二进制向量，其中每个比特与一个特征相关联。如果该向量的比特等于 1，则该特征被允许参与分类。如果该位为 0，则相应的特征不参与。在特征选择中，“基因”代表个体特征，“有机体”代表一组候选特征。“群体”中的每一种生物都根据适合度评分进行分级，例如在坚持组中的模型表现。最适合的生物生存并繁殖，如此重复，直到几代后种群收敛于一个解。在这里你可以对遗传算法[有进一步的了解。](https://www.obitko.com/tutorials/genetic-algorithms/)

```
**[Start]** Generate random population of *n* chromosomes (suitable solutions for the problem)**[Fitness]** Evaluate the fitness *f(x)* of each chromosome *x* in the population**[New population]** Create a new population by repeating following steps until the new population is complete : **[Selection]** Select two parent chromosomes from a population according to their fitness (the better fitness, the bigger chance to be selected) **[Crossover]** With a crossover probability cross over the parents to form a new offspring (children). If no crossover was performed, offspring is an exact copy of parents. **[Mutation]** With a mutation probability mutate new offspring at each locus (position in chromosome). **[Accepting]** Place new offspring in a new population**[Replace]** Use new generated population for a further run of algorithm**[Test]** If the end condition is satisfied, **stop**, and return the best solution in current population**[Loop]** Go to step **2 [Fitness]**
```

这可以有效地从非常高维的数据集中选择特征，而穷举搜索是不可行的。但在大多数情况下，人们可能会认为这不值得争论，这取决于上下文，因为使用 PCA 或内置选择会简单得多。

## 4)逐步回归

在统计学中，逐步回归是一种拟合回归模型的方法，其中预测变量的选择是通过自动程序进行的。在每一步中，基于一些预先指定的标准，考虑将一个变量添加到解释变量集或从解释变量集中减去。通常，这采取一系列[*F*-测试](https://en.wikipedia.org/wiki/F-test)或[*t*-测试](https://en.wikipedia.org/wiki/T-test)的形式，但是其他技术也是可能的，例如[调整的 *R* 2](https://en.wikipedia.org/wiki/Adjusted_R-squared) 、[阿凯克信息准则](https://en.wikipedia.org/wiki/Akaike_information_criterion)、[贝叶斯信息准则](https://en.wikipedia.org/wiki/Bayesian_information_criterion)、[马洛斯的 *Cp*](https://en.wikipedia.org/wiki/Mallows%27s_Cp) 、[按下](https://en.wikipedia.org/wiki/PRESS_statistic)或[这里](https://en.wikipedia.org/wiki/False_discovery_rate)可以更好的理解逐步回归[。](https://gerardnico.com/data_mining/stepwise_regression)

这有两种味道:向前和向后。对于向前逐步搜索，您开始时没有任何功能。然后，您将使用每个候选特征训练一个单特征模型，并保留具有最佳性能的版本。您将继续添加特性，一次添加一个，直到您的性能改进停止。向后逐步搜索是相同的过程，只是顺序相反:从模型中的所有特征开始，然后一次删除一个特征，直到性能开始显著下降。

这是一种贪婪算法，并且通常比诸如正则化等监督方法具有更低的性能。

## 特征提取技术

![](img/21ab818b458470de509e022c26601272.png)

特征提取过程

特征提取是为了创建一个新的、更小的特征集，该特征集仍然捕获大部分有用的信息。这可以是有监督的(例如 LDA)和无监督的(例如 PCA)方法。

## 1)线性判别分析(LDA)

LDA 使用来自多个要素的信息创建一个新的轴，并将数据投影到新的轴上，以便最小化方差并最大化类平均值之间的距离。LDA 是一种监督方法，只能用于标记数据。它包含为每个类计算的数据的统计属性。对于单个输入变量(x ),这是每类变量的平均值和方差。对于多个变量，这是在多元高斯上计算的相同属性，即均值和协方差矩阵。LDA 变换也依赖于比例，因此您应该首先归一化您的数据集。

LDA 是有监督的，所以需要有标签的数据。它提供了各种变化(如二次 LDA)来解决特定的障碍。但是正在创造的新特征很难用 LDA 来解释。

## 2)主成分分析

PCA 是一种降维方法，它可以识别我们数据中的重要关系，根据这些关系转换现有数据，然后量化这些关系的重要性，以便我们可以保留最重要的关系。为了记住这个定义，我们可以把它分成四个步骤:

1.  我们通过一个[协方差矩阵](https://en.wikipedia.org/wiki/Covariance_matrix) **来识别特征之间的关系。**
2.  通过协方差矩阵的线性变换或[特征分解](https://en.wikipedia.org/wiki/Eigendecomposition_of_a_matrix)，得到[特征向量和特征值](https://en.wikipedia.org/wiki/Eigenvalues_and_eigenvectors)。
3.  然后，我们使用特征向量将数据转换成主分量。
4.  最后，我们使用特征值量化这些关系的重要性，并保留重要的主成分**。**

PCA 创建的新特征是正交的，这意味着它们是不相关的。此外，它们按照“解释方差”的顺序排列第一个主成分(PC1)解释了数据集中最大的方差，PC2 解释了第二大方差，依此类推。您可以通过根据累计解释方差限制要保留的主成分数量来降低维度。PCA 变换也依赖于比例，因此您应该首先归一化您的数据集。PCA 是寻找给定特征之间的线性相关性。这意味着，只有当数据集中有一些线性相关的变量时，这才会有所帮助。

![](img/166deb23a9497720a00c5831425bc671.png)

主成分分析

## 3) t 分布随机邻居嵌入(t-SNE)

t-SNE 是一种非线性降维技术，通常用于可视化高维数据集。t-SNE 的一些主要应用是自然语言处理(NLP)、语音处理等。

> t-SNE 通过最小化由原始高维空间中的输入特征的成对概率相似性构成的分布与其在缩减的低维空间中的等价物之间的差异来工作。t-SNE 然后利用 Kullback-Leiber (KL)散度来测量两种不同分布的不相似性。然后使用梯度下降最小化 KL 散度。

这里，低维空间使用 t 分布建模，而高维空间使用高斯分布建模。

## 4)自动编码器

自动编码器是一系列机器学习算法，可用作降维技术。自动编码器也使用非线性变换将数据从高维投影到低维。自动编码器是被训练来重建其原始输入的神经网络。基本上，自动编码器由两部分组成。

1.  **编码器**:获取输入数据并进行压缩，以去除所有可能的噪音和无用信息。编码器级的输出通常被称为瓶颈或潜在空间。
2.  **解码器**:将编码后的潜在空间作为输入，并尝试使用其压缩形式(编码后的潜在空间)再现原始自动编码器输入。

![](img/1610400f47ceb3f76f7f0552e7cff189.png)

自动编码器的结构

在上图中，中间层代表数量较少的神经元中的大量输入特征，因此给出了密集且较小的输入表示。由于这是一种基于神经网络的特征提取解决方案，因此可能需要大量数据进行训练。

*我希望你喜欢这篇文章，谢谢你的阅读！*