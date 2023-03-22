# XGBoost 是什么？以及如何优化？

> 原文：<https://towardsdatascience.com/what-is-xgboost-and-how-to-optimize-it-d3c24e0e41b4?source=collection_archive---------47----------------------->

![](img/a1ec26e8e0904f6373146af2feb3abb1.png)

来源:图片由 [Free-Photos](https://pixabay.com/photos/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=931706) 来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=931706)

## 在机器学习和 Kaggle 竞赛的世界中，XGBoost 算法占据了第一位。

## 介绍

像许多数据科学家一样，XGBoost 现在是我工具箱的一部分。该算法是数据科学领域(真实世界或竞争)最流行的算法之一。它的多任务方面允许它在回归或分类项目中使用。它可以用于表格、结构化和非结构化数据。

> [GitHub 上有一个包含代码的笔记本。](https://github.com/Christophe-pere/Hyperparameters_tuning_XGBoost)笔记本用于文件(文本)的分类。

## XGBoost

XGBoost 或极端梯度提升是一种基于树的算法(Chen and Guestrin，2016[2])。XGBoost 是树家族(决策树、随机森林、装袋、提升、梯度提升)的一部分。

Boosting 是一种集合方法，主要目的是减少偏差和方差。目标是顺序创建弱树，以便每个新树(或学习者)关注前一个树的弱点(错误分类的数据)。在添加弱学习者之后，数据权重被重新调整，称为“重新加权”。由于每个新学习者加入后的自动校正，在收敛后整体形成一个强模型。

XGBoost 的强项是并行性和硬件优化。数据存储在内存中，称为块，并以压缩列[CSC]格式存储。该算法可以执行树修剪，以便以低概率移除分支。模型的损失函数中有一项通过正则化来惩罚模型的复杂性，以平滑学习过程(降低过拟合的可能性)。

该模型即使在缺失值或大量零值的情况下也能很好地执行稀疏感知。XGBoost 使用一种称为“加权分位数草图算法”的算法，这允许算法专注于错误分类的数据。每个新学习者的目标是学习如何在每次迭代后对错误数据进行分类。该方法允许您按分位数对数据进行排序，以便找到正确的分割点。这是ϵ参数的目标，也就是分割的值(ϵ=0.1；分位数=[10%，20%，…，90%])。

升压过程的迭代次数由具有集成交叉验证方法的算法自动确定。

作者提供了不同树算法的比较表:

![](img/cb8b47dc8e88089a164e11860a202f20.png)

陈和 Guestin，2016。第 7 页，表 1。

## ***优化***

XGBoost 具有超参数(估计器不学习的参数)，必须通过参数优化来确定。这个过程很简单，每个要估计的参数由一个值列表表示，然后每个组合由模型进行测试，通过比较模型的度量来推断最佳组合。对参数的搜索需要通过交叉验证用指标来指导。不要害怕，`[sklearn](https://scikit-learn.org/stable/index.html)`有两个功能可以帮你做到这一点:`[RandomizedSearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.RandomizedSearchCV.html)` 和`[GridSearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html?highlight=gridsearchcv#sklearn.model_selection.GridSearchCV)`。

***网格搜索***

网格搜索是一种尝试计算超参数最佳值的调整技术。这是通过算法的超参数空间的手动指定子集的详尽搜索。

***随机搜索***

第二种方法被认为更有效( *Bergstra 和 Bengio，2012)* [1]，因为参数是随机选择的。随机搜索可以在连续和混合空间中使用，并且当只有少量超参数影响最终性能时非常有效。

这些方法怎么用？

## 笔记本

***随机搜索***

让我们从一个*随机搜索*开始。下面的代码显示了如何调用`RandomizedSearchCV()`。要估计的超参数存储在字典中。XGBoost 可以在训练期间考虑其他超参数，如早期停止和验证集。您可以用在`fit()`方法中传递的另一个字典来配置它们。

在接下来的代码中，我使用通过*随机搜索*获得的最佳参数(包含在变量`best_params_)`中)来初始化*网格搜索*的字典。

***网格搜索***

在下面的代码*、*中，您将找到*网格搜索*的配置。如前所示，交叉验证固定在 3 倍。

## 结论

关于 XGBoost 的超参数优化的这篇(小型)教程到此结束。但是这些方法可以被每一个机器学习算法利用。请记住，这些优化方法在计算方面是昂贵的(上面的第一个代码生成 3000 次运行)。

你现在可以在工作或比赛中使用它了。尽情享受吧！

## 参考

[1] [詹姆斯·伯格斯特拉和约舒阿·本吉奥，2012 年。*随机搜索超参数优化*，机器学习研究杂志 13(2012)281–305](https://jmlr.csail.mit.edu/papers/volume13/bergstra12a/bergstra12a.pdf)

[2]陈天琦和卡洛斯·盖斯特林，2016。【https://arxiv.org/pdf/1603.02754.pdf】XGBoost:一个可扩展的树增强系统，