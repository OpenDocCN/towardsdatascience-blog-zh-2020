# 从头开始的算法:K-最近邻

> 原文：<https://towardsdatascience.com/algorithms-from-scratch-k-nearest-neighbors-fe19b431a57?source=collection_archive---------32----------------------->

## [从零开始的算法](https://towardsdatascience.com/tagged/algorithms-from-scratch)

## 从头开始详述和构建 K-NN 算法

![](img/a6f6da1320afcb443e032f29f59c2ae0.png)

照片由[妮娜·斯特雷尔](https://unsplash.com/@ninastrehl?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 介绍

能够执行分类和回归的非参数算法；斯坦福大学教授 Thomas Cover 于 1967 年首次提出了 K 近邻算法的思想。

许多人经常把 K-NN 称为懒惰学习器或一种基于实例的学习器，因为所有的计算都推迟到函数求值。我个人认为，当我们开始概念化机器学习算法时，这将 K-最近邻推向了不太复杂的一端。

无论我们是在做分类还是回归风格问题，输入都将由原始特征空间中的 *k* 个最近的训练样本组成。然而，算法的输出当然取决于问题的类型——关于不同输出的更多信息，请参见术语部分。

链接到文章中生成的代码…

[](https://github.com/kurtispykes/ml-from-scratch/tree/master) [## kurtispykes/ml-从零开始

### 通过在 GitHub 上创建一个帐户，为 kurtispykes/ml 的从头开发做出贡献。

github.com](https://github.com/kurtispykes/ml-from-scratch/tree/master) 

## 术语

**K-最近邻分类** →输出将确定类成员，并且通过其邻居的多数投票来进行预测。因此，新实例将被分配到最常见的 k 个最近邻居的类中。

**K-最近邻回归** →输出将确定对象的属性值。因此，新实例将被分类为第 *k 个*最近邻居的平均值

**基于实例的学习** →一系列机器学习算法，不执行显式归纳，而是将新的问题实例与存储在内存中的训练中看到的实例进行比较。(来源: [**维基百科**](https://en.wikipedia.org/wiki/Instance-based_learning) )

**惰性学习** →一种机器学习方法，在这种方法中，训练数据的概括在理论上被延迟，直到向系统发出查询，这与系统在接收查询之前试图概括训练数据的急切学习相反。(来源: [**百科**](https://en.wikipedia.org/wiki/Lazy_learning) )

## 创建模型

创建 K-NN 算法非常简单。训练阶段实际上是存储训练样本的特征向量和标签，但是我们需要为 *k.* 通常为*，*确定一个正整数。当我们选择一个较大的值 *k* 时，我们会减少噪声对分类的影响，从而使类别之间的边界不那么明显。最终， *k* 的选择很大程度上受到数据的影响，这意味着我们无法知道，直到我们尝试了数据，然而我们可以使用许多不同的试探法来为我们的数据选择 *k* 。

> **注**:要阅读关于调整 *k* 超参数的更多信息，请参见维基百科页面的 [**超参数优化**](https://en.wikipedia.org/wiki/Hyperparameter_optimization) 。

太好了，我们选择了 k。为了对分类任务的新实例进行预测，识别出与新观察最接近的 k 个记录(来自训练数据)。在对 K 个近邻进行评估后，将进行预测——参见术语部分的**K-最近邻分类**,了解如何进行预测。

为了识别与新实例最接近的 k 个记录，我们必须对所有实例进行测量。这可以通过多种方式实现，尽管作为一种指导，当我们有连续变量时，许多从业者经常使用[欧几里德距离](https://en.wikipedia.org/wiki/Euclidean_distance)，而对于离散变量则使用[汉明距离](https://en.wikipedia.org/wiki/Euclidean_distance)。

图像汉明和欧氏距离

**分块算法**

1.  计算欧几里德距离
2.  定位邻居
3.  预测

**实现**

为了实现我们的 K-最近邻分类算法，我们将使用来自 Scikit-Learn 的[虹膜数据集](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_iris.html)。在这项任务中，我们面临的挑战是在给定花朵尺寸的情况下，预测花朵是`setosa`、`versicolor`还是`virginica`——这使它成为一项多类分类任务。

> **注意**:在这个实现中，我没有执行任何试探法来选择最佳的*k*——我只是随机选择了一个 k 值。

```
import numpy as np
import pandas as pd
from collections import Counterimport matplotlib.pyplot as plt
%matplotlib inlinefrom sklearn.metrics import accuracy_score
from sklearn.datasets import load_iris
from sklearn.neighbors import KNeighborsClassifier
from sklearn.model_selection import train_test_splitiris = load_iris()
X, y = iris.data, iris.targetX_train, X_test, y_train, y_test = train_test_split(X, y, test_size= 0.2, random_state=1810)X_train.shape, y_train.shape, X_test.shape, y_test.shape((120, 4), (120,), (30, 4), (30,))
```

我们现在有了数据，并使用了基于维持的交叉验证方案来拆分数据——如果您不熟悉这个术语，请参见下面的链接。

[](/cross-validation-c4fae714f1c5) [## 交叉验证

### 验证机器学习模型的性能

towardsdatascience.com](/cross-validation-c4fae714f1c5) 

第一步是计算两行之间的欧几里德距离。

```
def euclidean(x1, x2):
    return np.sqrt(np.sum((x1 - x2)**2))
```

为了测试这个函数，我从 Jason brown lee[那里取了一些代码，他用这些代码来测试他的距离函数。如果我们有正确的实现，那么我们的输出应该是相同的。](https://machinelearningmastery.com/tutorial-to-implement-k-nearest-neighbors-in-python-from-scratch/)

```
# dataset from [https://machinelearningmastery.com/tutorial-to-implement-k-nearest-neighbors-in-python-from-scratch/](https://machinelearningmastery.com/tutorial-to-implement-k-nearest-neighbors-in-python-from-scratch/)dataset = [[2.7810836,2.550537003,0],
[1.465489372,2.362125076,0],
[3.396561688,4.400293529,0],
[1.38807019,1.850220317,0],
[3.06407232,3.005305973,0],
[7.627531214,2.759262235,1],
[5.332441248,2.088626775,1],
[6.922596716,1.77106367,1],
[8.675418651,-0.242068655,1],
[7.673756466,3.508563011,1]]row0 = dataset[0]for row in dataset: 
    **print**(euclidean(np.array(row0), np.array(row)))0.0
1.3290173915275787
1.9494646655653247
1.5591439385540549
0.5356280721938492
4.952940611164215
2.7789902674782985
4.3312480380207
6.59862349695304
5.084885603993178
```

我们得到完全相同的输出——请随意查看提供的链接。

如前所述，新观察的 *k* —邻居是来自训练数据的 *k* 最近实例。使用我们的距离函数`euclidean`，我们现在可以计算训练数据中的每个观察值与我们已经传递的新观察值之间的距离，并从最接近我们的新观察值的训练数据中选择 *k* 个实例。

```
def find_neighbors(X_train, X_test, y_train, n_neighbors): 

    distances = [euclidean(X_test, x) for x in X_train]
    k_nearest = np.argsort(distances)[:n_neighbors]
    k_nearest_label = [y_train[i] for i in k_nearest]

    most_common = Counter(k_nearest_label).most_common(1)[0][0]

    return most_common
```

此函数计算新观察到定型数据中所有行的距离，并将其存储在一个列表中。接下来，我们使用 NumPy 模块`np.argsort()`找到 *k* 最低距离的索引—参见[文档](https://numpy.org/doc/stable/reference/generated/numpy.argsort.html)。然后我们使用索引来识别 k 个实例的类。之后，我们使用 Pythons 内置模块中的`Counter`函数计算`k_nearest_labels` 列表中实例的数量，并返回最常见的(计数最高的标签)。然而，在我们做出预测之前，我们不会看到它的实际运行，所以让我们来构建预测函数。

```
def predict(X_test, X_train, y_train, n_neighbors=3): 
    predictions = [find_neighbors(X_train, x, y_train, n_neighbors)                  for x in X_test]
    return np.array(predictions)predict(X_test, X_train, y_train, n_neighbors=3)
**array**([0, 0, 2, 2, 0, 1, 0, 0, 1, 1, 2, 1, 2, 0, 1, 2, 0, 0, 0, 2,           1, 2, 0, 0, 0, 0, 1, 1, 0, 2])
```

在`predict`函数中，我们使用列表理解来为测试集中的每个新实例找到最近的邻居，并返回一个数组。使用 3 个邻居，我们在任务上获得了 100%的准确性，并且我们可以将其与 scikit-learning 实现进行比较，以查看我们是否获得了相同的结果——确实如此。

> **注意**:K 近邻分类器的文档可以在[这里](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html#sklearn.neighbors.KNeighborsClassifier)找到

```
knn = KNeighborsClassifier(n_neighbors=3)
knn.fit(X_train, y_train)sklearn_preds = knn.predict(X_test)
preds = predict(X_test, X_train, y_train, n_neighbors=3)**print**(f"My Implementation: {accuracy_score(y_test, preds)}\nScikit-Learn Implementation: {accuracy_score(y_test, sklearn_preds)}")My Implementation: 1.0
Scikit-Learn Implementation: 1.0
```

**优点**

*   直观简单
*   没有训练步骤
*   可用于分类和回归(以及无监督学习)
*   对于多类问题易于实现

**缺点**

*   随着数据的增长，算法变得非常慢
*   对异常值敏感
*   不平衡的数据会导致问题—可以使用加权距离来克服这个问题。

## 包裹

在本故事中，您了解了 K-最近邻算法、如何在 Python 中从头开始实现 K-最近邻分类算法，以及使用 K-最近邻的利弊。

**让我们继续 LinkedIn 上的对话……**

[](https://www.linkedin.com/in/kurtispykes/) [## Kurtis Pykes -人工智能作家-走向数据科学| LinkedIn

### 在世界上最大的职业社区 LinkedIn 上查看 Kurtis Pykes 的个人资料。Kurtis 有一个工作列在他们的…

www.linkedin.com](https://www.linkedin.com/in/kurtispykes/)