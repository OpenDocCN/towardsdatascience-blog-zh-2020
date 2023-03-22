# 评估回归模型的 3 个最佳指标？

> 原文：<https://towardsdatascience.com/what-are-the-best-metrics-to-evaluate-your-regression-model-418ca481755b?source=collection_archive---------1----------------------->

## R 平方，校正的 R 平方，均方差，缅因州 RMSE

![](img/0dbbb0e1b6c25fde4ef79f3e106e504c.png)

来源:Issac Smith 在 Splash 上拍摄的照片

模型评估在数据科学中非常重要。它有助于您了解模型的性能，并使向其他人展示您的模型变得容易。有许多不同的评估指标，但只有其中一些适合用于回归。本文将涵盖回归模型的不同度量以及它们之间的差异。希望在你读完这篇文章后，你能清楚哪些指标适用于你未来的回归模型。

每次当我告诉我的朋友:“嘿，我建立了一个机器学习模型来预测 XXX。”他们的第一反应会是:“酷，那么你的模型预测的准确度是多少？”与分类不同，回归模型中的准确性稍微难以说明。你不可能预测准确的值，而是**你的预测与真实值**有多接近。

## 回归模型评估有 3 个主要指标:

> 1.R 平方/调整后的 R 平方
> 
> 2.均方差(MSE)/均方根误差(RMSE)
> 
> 3.平均绝对误差

## R 平方/调整后的 R 平方

r 平方衡量模型可以解释因变量的多少可变性。它是相关系数(R)的平方，这就是它被称为 R 平方的原因。

![](img/836a54fc508daa6692b610e1c56d4889.png)

r 平方公式

通过预测误差的平方和除以用平均值代替计算预测的平方和来计算 r 平方。r 平方值在 0 到 1 之间，较大的值表示预测值和实际值之间的拟合较好。

r 平方是确定模型拟合因变量的好方法。**但是没有考虑过拟合问题**。如果您的回归模型有许多独立变量，由于模型太复杂，它可能非常适合定型数据，但对于测试数据来说表现很差。这就是引入调整后的 R 平方的原因，因为它将惩罚添加到模型中的额外独立变量，并调整度量以防止过度拟合问题。

```
#Example on R_Square and Adjusted R Square
import statsmodels.api as sm
X_addC = sm.add_constant(X)
result = sm.OLS(Y, X_addC).fit()
print(result.rsquared, result.rsquared_adj)
# 0.79180307318 0.790545085707
```

在 Python 中，可以使用 [Statsmodel](https://www.statsmodels.org/stable/index.html) 或者 [Sklearn 包](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.r2_score.html)计算 R 平方

从样本模型中，我们可以解释大约 79%的相关可变性可以由模型来解释，并且调整后的 R 平方与 R 平方大致相同，这意味着模型相当稳健。

## 均方差(MSE)/均方根误差(RMSE)

虽然 R 平方是模型拟合因变量的相对度量，但均方差是拟合优度的绝对度量。

![](img/dd4d038156f90d74d096c3ff35ceb32e.png)

均方差公式

MSE 的计算方法是预测误差的平方和，即实际输出减去预测输出，然后除以数据点数。它给你一个绝对的数字，告诉你你的预测结果与实际数字有多大的偏差。您无法从一个单一的结果中解读许多见解，但它给了您一个真实的数字来与其他模型结果进行比较，并帮助您选择最佳的回归模型。

均方根误差(RMSE)是 MSE 的平方根。它比 MSE 更常用，因为首先有时 MSE 值可能太大而不容易比较。第二，MSE 是通过误差的平方计算的，因此平方根使它回到预测误差的相同水平，并使它更容易解释。

```
from sklearn.metrics import mean_squared_error
import math
print(mean_squared_error(Y_test, Y_predicted))
print(math.sqrt(mean_squared_error(Y_test, Y_predicted)))
# MSE: 2017904593.23
# RMSE: 44921.092965684235
```

可以使用 [Sklearn 包](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)在 Python 中计算 MSE

## 平均绝对误差

平均绝对误差(MAE)类似于均方误差(MSE)。然而，MAE 取的不是 MSE 中误差的平方和，而是误差绝对值的和。

![](img/13c6e2d1a8d50399d17b949bbdcf00c8.png)

平均绝对误差公式

与 MSE 或 RMSE 相比，MAE 是误差项和更直接的表示。 **MSE 通过平方对大的预测误差给予较大的惩罚，而 MAE 对所有误差一视同仁**。

```
from sklearn.metrics import mean_absolute_error
print(mean_absolute_error(Y_test, Y_predicted))
#MAE: 26745.1109986
```

可以使用 [Sklearn 包](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)用 Python 计算 MAE

## 总体建议/结论

R 平方/调整后的 R 平方更适合用于向其他人解释模型，因为您可以将数字解释为输出可变性的百分比。MSE、RMSE 或 MAE 更适合用来比较不同回归模型之间的性能。就我个人而言，我更喜欢使用 RMSE，我认为 Kaggle 也使用它来评估提交。但是，如果值不太大，使用 MSE 是完全有意义的，如果不想惩罚大的预测误差，则使用 MAE 也是完全有意义的。

调整后的 R 平方是这里唯一考虑过拟合问题的度量。R square 在 Python 中有一个直接的库可以计算，但是除了使用 statsmodel 结果之外，我没有找到一个直接的库来计算调整后的 R Square。如果你真的想计算调整后的 R 平方，你可以使用 statsmodel 或者直接使用它的数学公式。

有兴趣了解用于评估分类模型的顶级指标吗？请参考下面的链接:

[](https://medium.com/@songhaowu/top-5-metrics-for-evaluating-classification-model-83ede24c7584) [## 评估分类模型的 5 大指标

### 与回归模型不同的是，当我们评估分类模型的结果时，我们不应该只关注预测…

medium.com](https://medium.com/@songhaowu/top-5-metrics-for-evaluating-classification-model-83ede24c7584)