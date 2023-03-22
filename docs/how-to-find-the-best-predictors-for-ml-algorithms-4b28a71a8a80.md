# 如何找到最大似然算法的最佳预测器

> 原文：<https://towardsdatascience.com/how-to-find-the-best-predictors-for-ml-algorithms-4b28a71a8a80?source=collection_archive---------7----------------------->

## 了解特征选择及其各种技术，以提高机器学习算法的预测能力

![](img/d44a7c933d9db072e14a63156b3afb30.png)

图片由 [JrCasas](https://www.shutterstock.com/g/JrCasas) 在 [Shutterstock](http://www.shutterstock.com) 拍摄

所以现在，你已经清理了你的[数据](/practical-guide-to-data-cleaning-in-python-f5334320e8e)，并准备好将其输入机器学习(ML)算法。但是，如果您有大量的输入要素呢？对于模型学习来说，它们都足够有用和有预测性吗？如果你使用了所有的特性，是否会使你的模型缺乏解释力？拥有大量预测器还可能会增加开发和模型训练时间，同时会占用大量系统内存。

特征选择技术旨在为模型训练系统地选择输入特征的最佳子集，以预测目标变量。不要将特征选择与其他降维技术(如 PCA 或使用信息论)相混淆。特征选择技术不修改预测器的原始语义，而只是选择它们的一个子集，从而产生一个更易解释的模型。

Sayes 等人认为:“特征选择的目标是多方面的，其中最重要的是:

1.  为了**避免过拟合**和**提高模型性能**，即在监督分类情况下的预测性能和在聚类情况下的更好的聚类检测，
2.  提供**更快**和更多**性价比高的车型**，以及
3.  **更深入地了解**生成数据的底层流程。"

虽然特征选择适用于有监督的和无监督的学习，但我在这里将重点放在有监督的特征选择上，其中目标变量是预先已知的。

在监督特征选择的环境中，我们可以定义三大类别:

*   嵌入式方法:有时也称为内在方法，这些技术内置于分类或回归模型的模型训练阶段。例子包括惩罚回归模型(例如，Lasso)和基于树的模型。因为这样的嵌入方法是模型训练的一个组成部分，所以我们不会在这篇文章中讨论这些。
*   过滤方法:这些技术着眼于要素的内在属性，并使用统计技术来评估预测值和目标变量之间的关系。然后，最佳排序或评分特征的子集被用于模型训练。在这篇文章中，我们将回顾这些不同的统计技术。
*   包装器方法:这些方法利用 ML 算法作为特性评估过程的一部分，根据特定的性能指标迭代地识别和选择最佳的特性子集。例如，scikit-learn 的[递归特征消除(RFE)](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.RFE.html) 类，我们将在本文稍后介绍。

# 过滤特征选择方法

在选择输入特征的最佳子集之前，过滤器特征选择技术利用各种统计测试和测量来推断输入和目标变量之间的关系。

请注意，这些统计测量是单变量的，因此，没有考虑各种输入特征对之间的相关性。首先从相关矩阵开始剔除冗余的相关特征可能是个好主意。或者，可以利用特定的高级多元滤波器技术，例如，基于相关性的特征选择(CFS)、马尔可夫毯式滤波器(MBF)和快速基于相关性的特征选择(FCBF)。然而，目前还没有得到广泛使用和支持的 python 包支持这些高级多元要素选择技术。

## 分类特征选择

可用于分类问题中的分类特征的两种常规统计特征技术是:

1.  独立性卡方检验
2.  交互信息

卡方检验用于确定两个分类变量之间的关系或依赖程度，在我们的示例中，一个是分类输入要素，另一个是分类目标变量。

在概率论和信息论中，两个随机变量的互信息度量它们之间的独立程度。更高的值意味着更高的依赖性。

对于编码为整数的分类特征(例如，通过`[OrdinalEncoder](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OrdinalEncoder.html)`)，scikit-learn 的`[SelectKBest](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectKBest.html)`类与`[chi2](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.chi2.html)`或`[mutual_info_classif](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.mutual_info_classif.html)`函数结合使用，以识别和选择前 *k* 个最相关的特征。`SelectKBest`返回的分数越高，该特征的预测能力就越强。入围的特征然后被馈送到用于模型开发的 ML 算法中。

显示`chi2`和`mutual_info_classif`的完整工作示例如下:

注意`SelectKBest`也可以作为`Pipeline`的一部分用于交叉验证。然而，根据输入特性的基数，当模型在测试集中遇到训练集中没有的新值，因此没有用于编码时，在`Pipeline`中使用`OrdinalEncoder`有时是不可行的。在这种情况下，一个解决方法是使用不同的`test_size`和`random_state`运行`train_test_split`多次，然后取所选验证指标的平均值。

对于尚未进行数字编码的`object`类型分类特征(可能因为它们将被[一次性编码](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html)或在稍后阶段转换为[虚拟变量](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.get_dummies.html)，SciPy 的`[chi2_contingency](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.chi2_contingency.html)`类用于计算每对分类特征和目标变量的 chi 统计量和 p 值。然后，p 值最低的要素将被列入建模候选名单。

一个完整的工作示例如下:

## 数字特征选择——分类问题

可用于分类问题中的数字特征的两种常规统计特征技术是:

1.  方差分析 F 统计量
2.  相互信息(如上所述)

方差分析(ANOVA) F 统计量计算两个或多个数据样本均值的方差比率。数字输入特征和分类目标特征之间的比率越高，两者之间的独立性越低，越可能对模型训练有用。参见[这篇](https://statisticsbyjim.com/anova/f-tests-anova/)优秀文章，了解 ANOVA F 统计的详细信息。

通过 sci-kit learn 的`[f_classif](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.f_classif.html)`函数，用于特征选择的 ANOVA F-statistic 在 Python 中类似地实现为独立性的卡方检验。假设我们这次处理的是数字特性，我们可以通过管道轻松实现交叉验证，如下所示:

## 数字特征选择—回归问题

啊，这是最简单的特征选择类型:为一个数字目标变量识别最合适的数字输入特征(回归问题)。可能的技术包括:

1.  相关统计
2.  相互信息(与前面解释的相同，只是使用了不同的`score_func`:`mutual_info_regression`)

相关性衡量一个变量因另一个变量而发生变化的程度。也许最著名的相关性度量是假设数据为高斯分布的皮尔逊相关性。相关系数的范围在-1(完全负相关)和 1(完全正相关)之间，0 表示变量之间没有任何关系。

除了绘制相关矩阵之外，还可以使用 sci-kit learn 的`[f_regression](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.f_regression.html)`函数和`SelectKBest`类，实现基于相关性的自动特征选择，类似于独立性卡方检验或 ANOVA F 统计。此外，就像 ANOVA F-statistic 函数一样，我们可以通过管道轻松实现交叉验证，如下所示:

# 包装特征选择

来自 scikit-learn 的[递归特征消除](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.RFE.html) (RFE)是实践中最广泛使用的包装器特征选择方法。RFE 是特征类型不可知的，它通过给定的监督学习模型(估计器)迭代地选择最佳数量的特征。

来自 scikit-learn 文档:“首先，在初始特征集上训练估计器，并且通过`coef_`属性或`feature_importances_`属性获得每个特征的重要性。然后，从当前特征集中删除最不重要的特征。该过程在删减集上递归重复，直到最终达到要选择的特征的期望数量。”

如上所述，RFE 只能与具有`coef_`或`feature_importances_`属性的算法一起使用来评估特征重要性。由于 RFE 是一种包装器特征选择技术，它可以使用任何给定的估计器(可以不同于应用于所选特征的算法)来识别最合适的特征。

假定`RFE`可用于分类和数字特征；在分类和回归问题中，它的实现是相似的。

需要调谐的`RFE`的主要参数是`n_features_to_select`。使用`DecisionTreeClassifier`进行特征选择和分类算法，基于`RFE`确定最佳特征数量的实际演示如下:

上面的代码片段将打印出`RFE`中使用的每个特性的平均和标准差，如下所示:

```
>2: 0.710
>3: 0.815
>4: 0.872
>5: 0.884
>6: 0.891
>7: 0.888
>8: 0.888
>9: 0.884
```

纯粹基于准确度分数，选择六个特征在这种情况下似乎是理想的。

## 自动特征选择以及对 RFE 多个模型的评估

然而，我们怎么知道`DecisionTreeClassifier`是 RFE 使用的最佳算法呢？如果我们想用 RFE 评估各种算法呢？来帮助我们了。

`RFECV`的主要目的是通过自动交叉验证选择最佳数量的特征。由于理想数量的特性将由`RFECV`自动选择(因此，在最后的代码片段中不需要`for`循环)，我们可以有效地训练和评估用于`RFECV`特性选择的多个模型。然后，可以使用具有最佳验证度量的模型向前进行预测。

现在让我们看一个实际的例子:

上面的代码片段将打印出`RFECV`中使用的每个模型的准确性得分的平均值和标准值，如下所示:

```
>LR: 0.891
>DT: 0.882
>RF: 0.888
>XGB: 0.886
```

简单的逻辑回归模型赢得了包装特征选择。然后我们可以用这个来做预测:

```
# create pipeline
rfecv = RFECV(estimator = LogisticRegression(), cv = 10, scoring = 
    'accuracy')
model = DecisionTreeClassifier()
pipeline = Pipeline(steps=[('features', rfecv), ('model', model)])# fit the model on all available data
pipeline.fit(X, y)# make a prediction for one example
data = #load or define any new data unseen data that you want to make predictions uponyhat = pipeline.predict(data)
print('Predicted: %.3f' % (yhat))
```

# 额外小费

大多数 ML 算法都有一个内置属性，用于查看模型学习过程中使用的特征的相对重要性。一些例子包括:

*   线性回归:`coef_`给出系数列表。系数越高，输入特征对目标变量的影响越大
*   逻辑回归:`coef_`提供系数列表，解释与线性回归相同
*   决策树和随机森林:`feature_importanes_` —越高越好

# 结论

这次我就这样了。如果您想讨论任何与数据分析、传统机器学习或信用分析相关的问题，请随时联系我。

下次见，摇滚起来！

# 参考

受[机器学习大师](http://www.machinelearningmastery.com)的杰森·布朗利博士的启发

[1] Saeys Y，Inaki I，Larranaga P .生物信息学中的特征选择
技术综述。生物信息学。2007;23(19): 2507-
2517。