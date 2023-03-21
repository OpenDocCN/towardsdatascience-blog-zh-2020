# 为机器学习寻找最佳特征的艺术

> 原文：<https://towardsdatascience.com/the-art-of-finding-the-best-features-for-machine-learning-a9074e2ca60d?source=collection_archive---------1----------------------->

![](img/ec797eebe0c8858041c9cca043901ad9.png)

照片由[埃里克·沃德](https://unsplash.com/@ericjamesward?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/art?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

## 使用 python 深入研究特征选择和工程

机器学习模型将一组数据输入(称为特征)映射到预测器或目标变量。此过程的目标是让模型学习这些输入和目标变量之间的模式或映射，以便在目标未知的情况下，给定新数据，模型可以准确预测目标变量。

对于任何给定的数据集，我们都希望开发一个模型，能够以尽可能高的精确度进行预测。在机器学习中，有许多影响模型性能的杠杆。一般来说，这些包括以下内容:

*   算法选择。
*   算法中使用的参数。
*   数据集的数量和质量。
*   用于训练模型的功能。

通常在数据集中，原始形式的给定特征集不能提供足够的或最优的信息来训练性能模型。在某些情况下，移除不必要或冲突的特征可能是有益的，这被称为**特征选择**。

在其他情况下，如果我们将一个或多个特征转换成不同的表示，为模型提供更好的信息，模型性能可能会得到改善，这被称为**特征工程**。

## 特征选择

在许多情况下，使用数据集中所有可用的特征不会产生最具预测性的模型。根据所用模型的类型，数据集的大小和各种其他因素(包括过多的要素)都会降低模型的性能。

特征选择有三个主要目标。

*   提高模型预测新数据的准确性。
*   降低计算成本。
*   产生一个更容易理解的模型。

您可能会因为多种原因而删除某些功能。这包括特征之间存在的关系，与目标变量的统计关系是否存在或足够显著，或者特征中包含的信息的值。

特征选择可以通过分析训练前后的数据集来手动执行，或者通过自动统计方法来执行。

## 手动特征选择

有许多原因可以解释为什么您想要从培训阶段删除某个功能。其中包括:

*   与数据集中的另一个特征高度相关的特征。如果是这种情况，那么这两个特征实质上提供了相同的信息。一些算法对相关特征敏感。
*   几乎不提供任何信息的功能。一个示例是一个特征，其中大多数示例具有相同的值。
*   与目标变量几乎没有统计关系的特征。

可以通过在训练模型之前或之后执行的数据分析来选择特征。这里有一些手动执行特征选择的常用技术。

***相关情节***

执行特征选择的一种手动技术是创建一个可视化，该可视化为数据集中的每个特征绘制相关性度量。 [Seaborn](https://seaborn.pydata.org/) 是一个很好的 python 库。以下代码为 scikit-learn API 提供的乳腺癌数据集中的特征生成了一个相关图。

```
# library imports
import pandas as pd
from sklearn.datasets import load_breast_cancer
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
import numpy as np# load the breast_cancer data set from the scikit-learn api
breast_cancer = load_breast_cancer()
data = pd.DataFrame(data=breast_cancer['data'], columns = breast_cancer['feature_names'])
data['target'] = breast_cancer['target']
data.head()# use the pands .corr() function to compute pairwise correlations for the dataframe
corr = data.corr()
# visualise the data with seaborn
mask = np.triu(np.ones_like(corr, dtype=np.bool))
sns.set_style(style = 'white')
f, ax = plt.subplots(figsize=(11, 9))
cmap = sns.diverging_palette(10, 250, as_cmap=True)
sns.heatmap(corr, mask=mask, cmap=cmap, 
        square=True,
        linewidths=.5, cbar_kws={"shrink": .5}, ax=ax)
```

在最终的可视化中，我们可以识别一些密切相关的特征，因此我们可能想要移除其中的一些，以及一些与目标变量相关性非常低的特征，我们也可能想要移除这些特征。

![](img/ca2b686f553a21798ee2ad2cb7a7793f.png)

***特征重要性***

一旦我们训练了一个模型，就有可能应用进一步的统计分析来理解特征对模型输出的影响，并由此确定哪些特征是最有用的。

有许多工具和技术可以用来确定特征的重要性。一些技术是特定算法独有的，而其他技术可以应用于广泛的模型，被称为**模型不可知**。

为了说明特性重要性，我将使用 scikit-learn 中随机森林分类器的内置特性重要性方法。下面的代码适合分类器并创建一个显示特征重要性的图。

```
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split# Spliiting data into test and train sets
X_train, X_test, y_train, y_test = train_test_split(data.drop('target', axis=1), data['target'], test_size=0.20, random_state=0)# fitting the model
model = RandomForestClassifier(n_estimators=500, n_jobs=-1, random_state=42)
model.fit(X_train, y_train)# plotting feature importances
features = data.drop('target', axis=1).columns
importances = model.feature_importances_
indices = np.argsort(importances)plt.figure(figsize=(10,15))
plt.title('Feature Importances')
plt.barh(range(len(indices)), importances[indices], color='b', align='center')
plt.yticks(range(len(indices)), [features[i] for i in indices])
plt.xlabel('Relative Importance')
plt.show()
```

这提供了一个很好的指示器，指示出那些对模型有影响的特性和那些没有影响的特性。在分析这张图表后，我们可以选择去掉一些不太重要的特征。

![](img/3f8af4a1f0a12fcd146c1b51c333fc03.png)

## 自动特征选择

有多种机制使用统计方法来自动查找要在模型中使用的最佳特征集。scikit-learn 库包含许多方法，这些方法为许多这些技术提供了非常简单的实现。

***方差阈值***

在统计学中，方差是变量与其均值的平方偏差，换句话说，对于给定的变量，数据点分布有多远？

假设我们正在建立一个机器学习模型来检测乳腺癌，数据集有一个性别布尔变量。该数据集可能几乎完全由一种性别组成，因此几乎所有的数据点都是 1。这个变量的方差极低，对预测目标变量毫无用处。

这是最简单的特征选择方法之一。scikit-learn 库有一个名为`VarianceThreshold`的[方法](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.VarianceThreshold.html#sklearn.feature_selection.VarianceThreshold)。该方法采用一个阈值，当适合一个特征集时，将移除低于该阈值的任何特征。阈值的默认值为 0，这将移除任何方差为零的要素，换句话说，所有值都相同。

让我们将这个默认设置应用于我们之前使用的乳腺癌数据集，以找出是否有任何特征被删除。

```
from sklearn.feature_selection import VarianceThresholdX = data.drop('target', axis=1)
selector = VarianceThreshold()
print("Original feature shape:", X.shape)new_X = selector.fit_transform(X)
print("Transformed feature shape:", new_X.shape)
```

输出显示变换后的要素形状相同，因此所有要素至少有一些差异。

![](img/26222aaf6a237b3790c36065596ebb94.png)

***单变量特征选择***

单变量特征选择将单变量统计测试应用于特征，并选择在这些测试中表现最佳的特征。单变量检验是只涉及一个因变量的检验。这包括方差分析(ANOVA)、线性回归和均值的 t 检验。

同样，scikit-learn 提供了许多特征选择方法，这些方法应用各种不同的单变量测试来寻找机器学习的最佳特征。

我们将把其中的一个称为 **SelectKBest** 的应用于乳腺癌数据集。该函数基于单变量统计测试选择 k 个最佳特征。k 的默认值是 10，所以会保留 10 个特征，默认测试是 **f_classif** 。

**f_classif** 检验用于分类目标，如乳腺癌数据集的情况。如果目标是连续变量**，应使用 f_regression** 。f_classif 检验基于[方差分析](https://statistics.laerd.com/statistical-guides/one-way-anova-statistical-guide.php) (ANOVA)统计检验，该检验比较多个组的平均值或我们案例中的特征，并确定这些平均值之间是否具有统计显著性。

以下代码使用默认参数将该函数应用于乳腺癌数据集。

```
from sklearn.feature_selection import SelectKBestX = data.drop('target', axis=1)
y = data['target']
selector = SelectKBest()
print("Original feature shape:", X.shape)new_X = selector.fit_transform(X, y)
print("Transformed feature shape:", new_X.shape)
```

输出显示该函数已经将特性减少到 10 个。我们可以用不同的 k 值进行实验，训练多个模型，直到我们找到最佳数量的特征。

![](img/2e5c69837a83ab19236e741c182cdeec.png)

***递归特征消除***

这个[方法](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.RFECV.html#sklearn.feature_selection.RFECV)在逐渐变小的特征集合上执行模型训练。每次计算特征重要性或系数，并且移除具有最低分数的特征。在这个过程的最后，最佳的特征集是已知的。

由于这种方法涉及重复训练模型，我们需要首先实例化一个估计器。如果数据是第一次缩放，这种方法也是最好的，所以我添加了一个预处理步骤来归一化特征。

```
from sklearn.feature_selection import RFECV
from sklearn.svm import SVR
from sklearn import preprocessingX_normalized = preprocessing.normalize(X, norm='l2')
y = yestimator = SVR(kernel="linear")
selector = RFECV(estimator, step=1, cv=2)
selector = selector.fit(X, y)
print("Features selected", selector.support_)
print("Feature ranking", selector.ranking_)
```

下面的输出显示了已选择的功能及其排名。

![](img/e56bf1f2fc23c02e36bb3e60c30336eb.png)

## 特征工程

特征选择的目标是通过移除不必要的特征来降低数据集的维度，而特征工程是关于转换现有特征和构建新特征以提高模型的性能。

我们可能需要执行特征工程来开发最佳模型，这有三个主要原因。

1.  **功能不能以原始形式使用。**这包括诸如日期和时间之类的特征，其中机器学习模型只能利用包含在其中的信息，如果它们被转换成数字表示，例如星期几的整数表示。
2.  **要素可以以原始形式使用，但如果数据以不同的方式聚合或表示，则要素中包含的信息会更强。**这里的一个例子可能是包含人的年龄的特征，将年龄聚集到桶或箱中可以更好地表示与目标的关系。
3.  **一个特征本身与目标没有足够强的统计关系，但是当与另一个特征结合时，具有有意义的关系。**假设我们有一个数据集，它有许多基于一组客户信用历史的特征，以及一个表明他们是否拖欠贷款的目标。假设我们有一笔贷款和一个工资值。如果我们把这些结合成一个新的特征，叫做“贷款与工资比率”，这可能比单独的那些特征提供更多或更好的信息。

## 手动特征工程

可以通过数据分析、直觉和领域知识来执行特征工程。通常执行与用于手动特征选择的技术类似的技术。

例如，观察特征如何与目标相关联，以及它们在特征重要性方面的表现如何，可以指示哪些特征需要在分析方面进一步探索。

如果我们回到贷款数据集的例子，让我们假设我们有一个年龄变量，从相关图来看，它似乎与目标变量有某种关系。当我们进一步分析这种关系时，我们可能会发现，那些违约者倾向于某个特定的年龄组。在这种情况下，我们可以设计一个功能来挑选出这个年龄组，这可以为模型提供更好的信息。

## 自动化特征工程

手动特征工程可能是一个非常耗时的过程，并且需要大量的人类直觉和领域知识来正确处理。有一些工具能够自动合成大量的新特征。

[Featuretools](https://www.featuretools.com/) python 库是可以对数据集执行自动化要素工程的工具示例。让我们安装这个库，并在乳腺癌数据集上完成一个自动化特征工程的例子。

功能工具可以通过 pip 安装。

```
pip install featuretools
```

Featuretools 设计用于处理关系数据集(可以用唯一标识符连接在一起的表或数据帧)。featuretools 库将每个表称为一个**实体**。

乳腺癌数据集只包含一个实体，但我们仍然可以使用 featuretools 来生成特征。但是，我们需要首先创建一个包含唯一 id 的新列。下面的代码使用数据帧的索引创建一个新的 **id** 列。

```
data['id'] = data.index + 1
```

接下来，我们导入 featuretools 并创建实体集。

```
import featuretools as ftes = ft.EntitySet(id = 'data')
es.entity_from_dataframe(entity_id = 'data', dataframe = data, index = 'id')
```

这给出了以下输出。

![](img/358d2ba930a6e522c5d5917e018bfc25.png)

我们现在可以使用该库来执行特征合成。

```
feature_matrix, feature_names = ft.dfs(entityset=es, 
target_entity = 'data', 
max_depth = 2, 
verbose = 1, 
n_jobs = 3)
```

Featuretools 已经创建了 31 个新功能。

![](img/97c6a952f994815674243f8f625140b6.png)

我们可以通过运行以下命令来查看所有的新特性。

```
feature_matrix.columns
```

![](img/f951c0696ec509a9750053c33e68b836.png)

机器学习模型的好坏取决于它接受训练的数据。因此，在这篇关于特征选择和工程的文章中讨论的步骤是机器学习模型开发中最重要的，也是最耗时的部分。这篇文章对这一领域的理论、工具和技术进行了广泛的综述。然而，这是一个非常广阔的领域，需要对各种不同的数据集进行大量的练习，才能真正学会为机器学习找到最佳特征的艺术。

感谢阅读！

我每月都会发一份简讯，如果你想加入，请点击此链接注册。期待成为您学习旅程的一部分！