# 调整参数。以下是方法。

> 原文：<https://towardsdatascience.com/tuning-parameters-heres-how-39a4d1956f79?source=collection_archive---------27----------------------->

## 对你的机器学习模型有益的常见和不太常见的参数。

![](img/3d137104ecbcf7f4508d58500771e1af.png)

Gabriel Gurrola 在[Unsplash](https://unsplash.com/s/photos/tune?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上的照片。

# 目录

1.  介绍
2.  因素
3.  例子
4.  密码
5.  摘要
6.  参考

# 介绍

参数可能令人望而生畏，令人困惑，令人不知所措。本文将概述常用机器学习算法中使用的关键参数，包括:随机森林、多项式朴素贝叶斯、逻辑回归、支持向量机和 K 近邻。还有称为超参数的特定参数，我们将在后面讨论。参数调整有利于提高模型精度，减少模型运行时间，并最终减少模型的货币支出。

# 因素

来自[sk learn](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)【2】和其他库的每种型号都有不同的参数；但是，这些常用算法之间有相当多的重叠。开发模型时，您将拥有默认参数。调整这些有助于提高您的整体准确性。这些参数将在分类代码中找到(我将展示 Python 中的例子)，例如，假设您使用 *clf* 作为分类器的别名，您可以输入这些参数并在那里更改或调整它们的值，最终更改您的模型的输出。

最有效的调优方法之一是网格搜索[3]。这些参数是不同的，因为它们被认为是超参数，并且不是在估计器本身中直接学习的。在建立了上述分类器之后，您将创建一个参数网格，该网格将进行搜索以找到超参数的最大优化。请记住，这个调优过程计算量很大，如果您添加几个超参数的话，可能会非常昂贵。该模型将引用您列出的所有网格项目的组合，因此，几乎就好像您有几个模型在运行，而不是一个。

# 例子

![](img/fbcba1ce4535925c28ea1017c5f328a9.png)

照片由 [Fabian Grohs](https://unsplash.com/@grohsfabian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【4】上拍摄。

下面，我会把我在整个数据科学硕士期间所学的常用机器学习算法，以及我的职业生涯包括在内。虽然这些都是需要学习和练习的重要方法，但还有无数其他方法可以帮助你提高准确度。

> 这些参数不仅有助于提高准确性，而且有助于降低计算机的计算时间和工作量，因为您将降低随机森林的数量，从而降低树的最大深度，最终也将节省资金。

下面列出的是来自 sklearn 的常见机器学习算法，其中包括几个可编辑的参数。以下是所有记录的参数及其各自的机器学习算法的链接:

[随机森林](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)

[多项式朴素贝叶斯](https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.MultinomialNB.html)

[逻辑回归](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)

[支持向量机](https://scikit-learn.org/stable/modules/svm.html)

[K-最近邻](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html)

下面代码片段中的默认参数将在括号内——我还将给出我发现在过去有用的建议。

> 随机森林

```
**n_estimators**: number of trees in your forest (100)**max_depth**: maximum depth of your tree (None) - recommendation, change this parameter to be an actual number because this parameter could cause overfitting from learning your traning data too well**min_samples_split**: minimum samples required to split your node (2)**min_samples_leaf**: mimimum number of samples to be at your leaf node (1)**max_features**: number of features used for the best split ("auto")**boostrap**: if you want to use boostrapped samples (True)**n_jobs**: number of jobs in parallel run (None) - for using all processors, put -1**random_state**: for reproducibility in controlling randomness of samples (None)**verbose**: text output of model in process (None)**class_weight**: balancing weights of features, n_samples / (n_classes * np.bincount(y)) (None) - recommendation, use 'balanced' for labels that are unbalanced
```

> 多项式朴素贝叶斯

```
parameters - **alpha**: a paramter for smoothing (1.0)**class_prior**: looking at the previous class probability (None) attributes -**feature_count**: number of samples for each class or feature (number of classes, number of features)**n_features**: number of features for sample
```

> 逻辑回归

```
**penalty**: l1 or l2 (lasso or ridge), the normality of penalization (l2)**multi_class**: binary versus multiclass label data ('auto')
```

> 支持向量机

```
parameter **- decision_function_shape**: 'ovr' or 'one-versus-rest' approach
```

> k-最近邻

```
parameter - **n_neighbors**: number of neighbors (5)
```

# 密码

下面是一些有用的代码，可以帮助您开始参数调整。有几个模型可以从调优中受益，业务和团队也可以从调优带来的效率中受益。下面，是模型代码，以及可用于强大优化的网格搜索代码。

调谐参数的位置。作者代码[5]。

网格搜索示例。作者代码[6]。

# 摘要

![](img/1f5efe8cc8443040385d26ea5a0257e2.png)

照片由 [ThisisEngineering RAEng](https://unsplash.com/@thisisengineering?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/person-coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【7】上拍摄。

现在，希望您和您的团队能够从应用当前或下一个机器学习模型的参数更改中受益。从默认参数开始，确保在开始调优之前了解每个模型的参数，以及关于这些参数定义的官方文档。虽然它们可以改善你的模型，但是参数也可以以降低你的准确性或者过度拟合你的模型的方式被调整。小心谨慎，你会发现自己拥有一个成功的、复杂的数据科学模型。

我希望你觉得这篇文章有趣且有用。谢谢大家！

如果你想了解更多关于这些特定的机器学习算法，我写了另一篇文章，你可以在这里找到[8]:

[](/machine-learning-algorithms-heres-the-end-to-end-a5f2f479d1ef) [## 机器学习算法。这里是端到端。

### 通用算法的端到端运行；包括随机森林，多项式朴素贝叶斯，逻辑回归…

towardsdatascience.com](/machine-learning-algorithms-heres-the-end-to-end-a5f2f479d1ef) 

# 参考

[1]照片由 [Gabriel Gurrola](https://unsplash.com/@gabrielgurrola?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/tune?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2016)上拍摄

[2] sklearn， [sklearn.linear_model。后勤回归](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)(2007 年至 2019 年)

[3] sklearn， [3.2。调整估计器的超参数](https://scikit-learn.org/stable/modules/grid_search.html)(2007–2019)

[4]照片由 [Fabian Grohs](https://unsplash.com/@grohsfabian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄，(2018)

[5] M.Przybyla，[要点—模型](https://gist.github.com/mprzybyla123/461cc1513e83826f763949cf409a98d8)，(2020)

[6] M.Przybyla，[要点——网格搜索](https://gist.github.com/mprzybyla123/663dfc57b5cf6fcdd45b4a06ce82afb3)，(2020)

[7]照片由[this engineering RAEng](https://unsplash.com/@thisisengineering?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/person-coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2020)上拍摄

[8] M.Przybyla，[机器学习算法。这里是端到端的](/machine-learning-algorithms-heres-the-end-to-end-a5f2f479d1ef)。, (2020)