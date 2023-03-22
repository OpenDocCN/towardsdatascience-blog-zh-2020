# Python 中数据清理的实用指南

> 原文：<https://towardsdatascience.com/practical-guide-to-data-cleaning-in-python-f5334320e8e?source=collection_archive---------25----------------------->

## 了解如何为机器学习算法准备和预处理数据

![](img/da6dc64315c085d3444a06346cdd859a.png)

Ashwini Chaudhary 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在你进入机器学习(ML)算法的奇妙世界之前，有一个小问题，你可以通过它来预测未来:**数据准备**或**数据预处理**。

*数据准备是臭名昭著的*[*21 世纪最性感的工作*](https://hbr.org/2012/10/data-scientist-the-sexiest-job-of-the-21st-century) *中不性感的部分。*

由于 Python 和 r 中提供了各种专用的库和包，训练 ML 算法并利用它们来预测目标变量是很容易的事情。然而，在数据科学和 ML 世界中，垃圾进，垃圾出(GIGO)的古老格言仍然适用。

数据准备活动将原始数据转换成能够被 ML 算法有效和高效使用的形式、形状和格式。它是 ML 流程的重要组成部分，可以决定 ML 流程的成败。

> 从业者一致认为，构建机器学习管道的绝大部分时间都花在了特征工程和数据清理上。然而，尽管这个话题很重要，却很少被单独讨论。

**数据清理**只是数据准备活动的一个组成部分，数据准备活动还包括特征选择、数据转换、特征工程和降维。

根据定义，数据处理活动和数据清理对于每组原始数据都是独一无二的，因为实际的 ML 项目都有其固有的特性。尽管如此，某些活动是标准的，应该应用，或者至少在模型训练之前检查原始数据。

> 不管要修复的数据错误的类型如何，数据清理活动通常包括两个阶段:(1)错误检测，其中各种错误和违规被识别并可能由专家验证；以及(2)错误修复，其中应用对数据库的更新(或向人类专家建议)以将数据带到适合于下游应用和分析的更干净的状态。

# 警告！

在我们探索各种标准数据清理活动之前，需要注意一点。正如我在[之前的一篇文章](/how-to-avoid-potential-machine-learning-pitfalls-a08781f3518e)中提到的:在将你的整个数据分割成训练/测试/验证子集之后，应该对数值训练数据集执行以下所有操作，以避免数据泄露。只有这样，一旦您有了一个干净的训练数据集，如果需要，在测试(和验证)数据集和目标变量上重复相同的活动集。

在下面的所有代码片段中，按照常规符号，`X_train`指的是训练输入数据集。

# 基本数据清理

下面的操作应该成为你的 ML 项目的起点，应用到你得到的每一个数据。

## 识别并删除零方差预测值

零方差预测值是指在整个观测范围内包含单个值的输入要素。因此，它们不会给预测算法增加任何值，因为目标变量不受输入值的影响，这使得它们是多余的。一些 ML 算法也可能遇到意外的错误或输出错误的结果。

Pandas 提供了一个简短的函数来计算和列出 Pandas 数据帧中每一列的唯一值的数量:

```
X_train.nunique()
```

当要删除的列不多时，通过`X_train.drop(columns=['column_A', 'column_B'], inplace=True)`从 Pandas 数据帧中删除特定的列非常简单。使用多个零方差列实现相同结果的更可靠的方法是:

```
X_train.drop(columns = X_train.columns[X_train.nunique() == 1],
    inplace = True)
```

上面的代码将删除所有只有一个值的列，并更新`X_train`数据帧。

## 评估具有很少唯一值的列

对于唯一值很少的列(即低方差或接近零的方差)，应该给予特别的考虑。这样的列不是要从数据集中删除的自动候选列。例如，按照设计，[序数](https://en.wikipedia.org/wiki/Ordinal_data)或[分类](https://en.wikipedia.org/wiki/Categorical_variable)列不应该有大量的唯一值。

直觉上，我们可以天真地删除低方差列，但如果这样的预测器实际上对模型学习有帮助呢？例如，假设分类问题中的二元特征有许多 0 和几个 1(接近零的方差预测值)。当这个输入特征等于 1 时，目标变量总是相同的；然而，在该特征为零的情况下，它可以是任何一个可能的目标值。保留这个专栏作为我们的预测指标之一当然是有道理的。

因此，应该利用上下文考虑和领域知识来评估是否应该从我们的数据集中删除这样的低方差列。例如:

*   考虑将它们编码为序数变量
*   考虑将它们编码为分类变量
*   考虑通过一些降维技术(如主成分分析(PCA ))来组合这些低方差列

我们可以手动计算每列中唯一值的数量占观察总数的百分比，如下所示:

```
from numpy import uniquefor i in range(X_traain.shape[1]):
    num = len(unique(X_train.iloc[:, i]))
    percentage = float(num) / X_train.shape[0] * 100
    print('%d, %d, %.1f%%' % (i, num, percentage))
```

上面的代码将打印出所有列的列表，以及唯一值的计数及其占观察总数的百分比。

或者，我们可以利用 scikit-learn 库的`[VarianceThreshold](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.VarianceThreshold.html)` 类来识别**并删除**低方差列。使用`VarianceThreshold`类时要格外小心，因为它被设计用来删除低于参数化阈值的列。

## 处理重复数据

最有可能的是，应该从数据中删除重复的行，因为它们很可能是多余的。

Pandas 提供了一个简单的函数来检查数据帧中的重复行。请注意，这将只显示第二行(以及任何更高一级的行)，因为被复制的第一行不被认为是重复的。

```
X_train[X_train.duplicated()]
```

如果在分析完重复记录后，您想继续删除它们，一行代码就可以实现:

```
X_train.drop_duplicates(inplace = True)
```

# 离群点检测

离群值是在给定数据集的上下文中看起来很少或不太可能的任何数据点。领域知识和主题专业知识对于识别异常值很有用。尽管考虑到异常值的上下文性质，没有标准的异常值定义，但是可以使用各种统计和绘图方法来识别和适当地处理它们。

与低方差列类似，异常值观察值不会自动删除。相反，这些应该进一步分析，以确定它们是否真的是异常的。

盒须图是直观显示数据框架中数值异常值的简单而基本的方法:

```
X_train.boxplot()
```

现在让我们来看看一些识别异常值的统计技术。

## 使用标准偏差(SD)

用于异常值检测的 SD 技术可以应用于呈现高斯或类高斯分布的所有数值特征。作为复习，回想一下:

*   68%的正态分布观测值位于平均值的±1 SD 范围内
*   95%的正态分布观测值位于平均值的±2sd 范围内
*   99.7%的正态分布观测值位于平均值的±3 SD 范围内

以下代码行将选择并删除值大于每个数字列的指定 SDx 的所有行。3 SDs 是标准阈值；然而，2 个 SDs 可用于小数据集，4 个 SDs 可用于相对大的数据集。

## 使用四分位数间距(IQR)

我们可以使用 [IQR](https://en.wikipedia.org/wiki/Interquartile_range) 来识别输入要素中不符合正态或类正态分布的异常值。超出指定阈值的值(通常，高于第 75 百分位但低于第 25 百分位的 1.5 倍 IQR)将被过滤掉，以供进一步分析。

下面的代码片段将过滤掉阈值为 1.5 的所有数值列中的异常值。

## 本地异常因素(LOF)

从 scikit-learn 的[文档](https://scikit-learn.org/stable/auto_examples/neighbors/plot_lof_outlier_detection.html)，“LOF 算法是一种无监督的异常检测方法，它计算给定数据点相对于其邻居的局部密度偏差。它将密度远低于其相邻样本的样本视为异常值。”。原文可以在这里找到[。](https://www.dbs.ifi.lmu.de/Publikationen/Papers/LOF.pdf)

`[LocalOutlierFactor](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.LocalOutlierFactor.html)`的`fit_predict`方法预测数据帧中的每一行是否包含数值异常值(-1)或(1)。有异常值的行标记为-1，否则标记为 1。下面是一个简单实用的例子:

关于 LOF 有一点需要注意:它不允许在传递给 predict 方法的数据中有空值或缺失值。

# 统计估算缺失值

只是重申一个显而易见的事实，丢失值在现实生活中并不少见。大多数 ML 算法无法处理缺失值，并且会在模型训练期间抛出错误。因此，数据集中这种丢失的值应该被丢弃(最天真的方法，并且要尽可能避免——数据是新的货币！)或以某种方式进行逻辑上的估算。

通常用于此类插补的某些有效描述性统计数据包括:

*   列的平均值
*   列的中值
*   列的模式值
*   其他一些共同的价值观

查找和检测缺失值或空值的一些简单方法是执行以下函数之一:

```
# display the count of non-null values in each column of a DF
df.info()# display the count of null values in each column of a DF
df.isnull().sum()
```

使用 Python 的内置函数统计输入缺失值非常简单。以下函数计算每一列的描述性统计数据，并使用计算出的统计数据填充该特定列中的所有空值:

```
# using mean to fill missing values
df.fillna(df.mean(), inplace = True)# using median to fill missing values
df.fillna(df.median(), inplace = True)# using mode to fill missing values
df.fillna(df.mode().iloc[0], inplace = True)# using a specific constant to fill missing values
df.fillna(0, inplace = True)
```

但是，请记住，描述性统计数据只能在训练数据集上计算；然后应该使用相同的统计来估算训练和测试数据中的任何缺失值。上述方法适用于简单的训练/测试分割；然而，当使用 k-fold 交叉验证评估模型时，这实际上变得不可能——因为分裂是在单个函数调用中多次完成和重复的。

Scikit-learn 的`Pipeline`和`SimpleImputer`来拯救我们了。

## `SimpleImputer`的独立实现

`[SimpleImputer](https://scikit-learn.org/stable/modules/generated/sklearn.impute.SimpleImputer.html)`类通过以下步骤进行数据转换:

*   使用要使用的描述性统计类型定义分类。均支持均值、中值、众数和特定的常数值
*   拟合定型数据集，根据定型数据计算每列的统计信息
*   将拟合的估算值应用于训练和测试数据集，以将第二步中计算的统计值分配给这两个数据集中的缺失值

下面是一个简单的演示:

## 用管道实现简单输入

Scikit-learn 的[管道](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html)允许我们在单个步骤中应用最终估计器模型之前顺序执行多个数据转换。通过确保使用相同的样本来训练变压器和预测器，这可以防止数据“在交叉验证中从测试数据泄漏到训练模型中”(来自[文档](https://scikit-learn.org/stable/modules/compose.html#pipeline-chaining-estimators))。

最佳实践是在使用交叉验证时利用 Pipeline，如下所示:

一个简单的`for`循环也可用于分析`SimpleImputer`中所有四种不同的插补策略:

上面列出了每种估算策略的平均准确度分数。

使用 Pipeline，通过新的、具有潜在缺失值的实时数据来预测目标变量，如下所示:

```
# define pipeline
pipeline = Pipeline(steps=['i', SimpleImputer(strategy='mean')),
    ('m', RandomForestClassifier())])# fit the model
pipeline.fit(X, y)# predict on new data using the fitted model
pipeline.predict(new_data)
```

# kNN 缺失值插补

除了使用描述性统计来估算缺失值，还可以使用 ML 算法来预测缺失值。简单的回归模型可用于预测缺失值；然而，k-最近邻(kNN)模型在实践中也被发现是有效的。

Scikit-learn 的`[KNNImputer](https://scikit-learn.org/stable/modules/generated/sklearn.impute.KNNImputer.html)`类支持 kNN 插补——其用法与`SimpleImputer`非常相似。就像`SimpleImputer`一样，在管道中实现之前，我将首先演示`KNNImputer`的独立使用。

## 用流水线实现 KNNImputer

现在，让我们看看如何在管道中实现它以进行有效的验证:

我们还可以应用一个`for`循环来检查多个`k_neighbors`参数，以根据我们的精度指标确定最佳参数:

使用包含缺失值的新实时数据来预测目标变量的方法与上述`SimpleImputer`中的方法相同。

# 结论

这就是我这次在 Python 中针对机器学习项目的具体数据清理任务。

如果您想讨论任何与数据分析和机器学习相关的问题，请随时联系 [me](http://www.finlyticshub.com) 。

下次见——摇滚起来！

## 灵感和参考

[机器学习大师](https://machinelearningmastery.com/)的杰森·布朗利

[1]郑，a .，&卡萨里，A. (2018)。前言。在*机器学习的特征工程:数据科学家的原则和技术*(第 vii 页)。塞瓦斯托波尔，加利福尼亚州:奥赖利。

[2]，国际货币基金组织，&楚，X. (2019)。引言。在*数据清理*(第 1 页)中。纽约:计算机协会。