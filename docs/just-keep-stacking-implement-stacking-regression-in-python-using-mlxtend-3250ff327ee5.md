# 保持冷静和堆叠—使用 mlxtend 在 Python 中实现堆叠回归

> 原文：<https://towardsdatascience.com/just-keep-stacking-implement-stacking-regression-in-python-using-mlxtend-3250ff327ee5?source=collection_archive---------28----------------------->

![](img/5c63a64a009d5676e1bc5e08e35155be.png)

[王占山](https://unsplash.com/@jdubs?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

如果你曾经在一些 Kaggle 比赛中结合多个 ML 模型来提高你在排行榜上的分数，你知道这是怎么回事。事实上，这些 Kaggle 竞赛的许多获胜解决方案使用集合模型，而不仅仅是单一的微调模型。

集成模型背后的直觉非常简单:有效地组合不同的 ML 算法可以降低不幸选择一个差模型的风险。

在这篇文章中，我将讨论**堆叠**，一种流行的集成方法，以及如何使用 *mlxtend* 库在 Python 中实现一个简单的 2 层堆叠回归模型。我选择的示例任务是 Airbnb 价格预测。

# 古玩目录

1.  [什么是堆叠，它是如何工作的？](#ce45)
2.  [与基本型号相比，堆叠型号的性能如何？](#7224)
3.  [堆叠有哪些注意事项？](#16b4)
4.  [接下来我们能做什么？](#8ac8)

# 概观

堆叠，或堆叠泛化，是一种元学习算法，它学习如何以最佳方式组合每个基础算法的预测[1]。

简单来说，这就是如何建立一个两层堆叠模型。第一层，有些人称之为基础层，包括基础模型。在分类的上下文中，您可以将基本模型视为任何可用于进行预测的分类器，如神经网络、SVM、决策树等。类似的逻辑也适用于回归。在我们使用训练集训练这些模型之后，第二层通过从这些模型中获取预测以及对**样本外**数据的预期输出来构建元模型。你可以想到这种元模型的最简单的情况是平均出基础模型的预测。

避免过度拟合的常见方法是在这些基础模型上执行[交叉验证](https://machinelearningmastery.com/k-fold-cross-validation/)，然后使用折叠外预测和输出来构建元模型。

查看大卫·沃尔菲特的论文，了解更多关于堆叠工作原理的技术细节。

我选择了 Kaggle ( [link](https://www.kaggle.com/c/digit-recognizer) )的 Airbnb 价格预测任务作为这里的示例，因为它是一个相对简单的数据集，具有较小的样本大小和一组功能。任务是根据卧室数量、租赁类型(整个公寓或私人房间或合租房间)等信息预测 Airbnb 上的公寓租赁挂牌价格。

要查看完整的 Python 代码，请查看我的 [Kaggle 内核](https://www.kaggle.com/dehaozhang/stacking-ensemble)。

事不宜迟，让我们进入细节！

# 探测

出于本文的目的，我不会讨论预处理步骤，但请参考 Kaggle 内核了解全部细节。

在高层次上，我检查了每个特性的分布，删除了异常值，为分类“room_type”创建了虚拟变量(因为只有三个类别)，并对特性进行了标准化。

**训练/测试分割**

我将数据集的 70%设置为训练集，剩下的 30%将用作测试集。

```
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=1)
```

**基本型号**

让我们首先建立一些基础模型。这里我选择了 **Xgboost** 、**套索回归**和 **KNN** 作为三个基础模型。原则上，当模型在不同的范围内，并且它们的预测误差尽可能不相关时，叠加效果最好，这样单个模型的弱点和偏差可以被其他模型的优点抵消[2]。

**绩效指标**

在评估模型的性能度量方面，我选择使用**平均绝对误差(MAE)** ，它衡量预测值与测量值的差距。

**基本型号的性能**

让我们快速检查三个基本模型的性能(所有默认超参数):

```
Model: XGBRegressor, MAE: 35.80629559761084
Model: Lasso, MAE: 35.0691542225316
Model: KNeighborsRegressor, MAE: 38.35917874396135
```

MAEs 在 35-38 之间，这意味着预测价格平均与真实价格相差 35-38 美元。

**使用 mlxtend** 构建堆叠模型

现在，我们可以为元学习者建立基础模型。或者，我们可以使用 *mlxtend* 库([文档](http://rasbt.github.io/mlxtend/user_guide/regressor/StackingCVRegressor/#api))中的‘StackingCVRegressor’来创建快捷方式:

```
from mlxtend.regressor import StackingCVRegressorstack = StackingCVRegressor(regressors=(XGBRegressor(), 
                            Lasso(), KNeighborsRegressor()),
                            meta_regressor=Lasso(), cv=10,
                            use_features_in_secondary=True,
                            store_train_meta_features=True,
                            shuffle=False,
                            random_state=1)
```

使用相同的三个基础模型，对于元学习者，我选择使用 Lasso。我们也可以尝试其他回归模型作为元学习者，然后在上面构建另一个元学习者(三层堆叠！)注意，在这种配置中，我还选择使用原始训练数据作为元模型的输入，因为它可以为元模型提供更多的上下文。

现在，让我们将训练数据放入模型中，并在测试集上检查性能:

```
stack.fit(X_train, y_train)
X_test.columns = ['f0', 'f1', 'f2', 'f3', 'f4', 'f5', 'f6', 'f7', 'f8', 'f9', 'f10', 'f11'] # Xgboost internally renames the features
pred = stack.predict(X_test)
score = mean_absolute_error(y_test, pred)
print('Model: {0}, MAE: {1}'.format(type(stack).__name__, score))
--------------------------------------------------------------------
Model: StackingCVRegressor, MAE: 34.01755981865483
```

我们看到新的 MAE 大约是 34，与最佳基础模型 Lasso 的结果相比减少了 1。

虽然这种差异看起来微不足道，但像这样的小收获实际上可以在排行榜上产生巨大的差异。

如果你想研究集成神经网络，看看这篇论文。

# 警告

尽管有堆叠功能，但还是要记住以下几点:

1.  有效地使用堆栈有时需要一些尝试和错误，并且不能保证堆栈在所有情况下都会提高性能。
2.  堆叠在计算上可能是昂贵的，尤其是当堆叠层数很高时。
3.  随着叠加层数的增加，模型的可解释性降低。对于 Kaggle 竞争来说，这不是一个大问题，但是对于商业案例来说，理解特性的重要性及其对结果变量的增量影响可能更重要。

# 后续步骤

以下是我们接下来可以尝试的几件事:

1.  超参数调整—我们可以在选定的模型中调整一些超参数，以进一步优化性能。在这个过程中,“GridSearchCV”可能是一个有用的工具。
2.  继续堆叠—尝试构建额外的堆叠层，并绘制性能指标与层数的关系图，以检查边际回报递减是否成立(应该成立！).

# 摘要

让我们快速回顾一下。

我们使用 *mlxtend* 库在 Python 中实现了一个简单的两层堆叠回归模型，将其测试 MAE 与三个基本模型的测试 MAE 进行了比较，并观察到了改进。

我希望你喜欢这篇博文，并请分享你的想法:)

查看我的另一篇关于使用 t-SNE 降维的文章:

[](/dimensionality-reduction-using-t-distributed-stochastic-neighbor-embedding-t-sne-on-the-mnist-9d36a3dd4521) [## 在 MNIST 上使用 t-分布随机邻居嵌入(t-SNE)进行降维…

### t-SNE vs PCA vs PCA & t-SNE

towardsdatascience.com](/dimensionality-reduction-using-t-distributed-stochastic-neighbor-embedding-t-sne-on-the-mnist-9d36a3dd4521) 

# 参考

[1][https://machinelementmastery . com/stacking-ensemble-machine-learning-with-python/](https://machinelearningmastery.com/stacking-ensemble-machine-learning-with-python/)
[http://support . SAS . com/resources/papers/proceedings 17/SAS 0437-2017 . pdf](http://support.sas.com/resources/papers/proceedings17/SAS0437-2017.pdf)