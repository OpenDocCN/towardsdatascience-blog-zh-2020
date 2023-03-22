# 为什么随机森林和梯度提升决策树的最优 max_depth 差别很大？

> 原文：<https://towardsdatascience.com/why-do-random-forest-and-gradient-boosted-decision-trees-have-vastly-different-optimal-max-depth-a64c2f63e127?source=collection_archive---------33----------------------->

## 尽管它们都是由决策树组成的

![](img/85a7b34b806aaad2d57498eea7f2b944.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Kishore Ragav Ganesh Kumar](https://unsplash.com/@k15hore?utm_source=medium&utm_medium=referral) 拍摄

自 2014 年引入 XGBoost 以来，梯度增强决策树(GBDT)因其预测能力和易用性而广受欢迎。特别是，它已经取代了随机森林，成为异构表格数据的最佳算法。如今，许多数据科学家至少熟悉 GBDT 的一种实现，比如 XGBoost、LightGBM、CatBoost，甚至是 [scikit-learn](https://scikit-learn.org/stable/modules/ensemble.html#histogram-based-gradient-boosting) 。

然而，我们这些有随机森林经验的人可能会惊讶地发现，随机森林和 GBDT 具有非常不同的最优超参数，尽管它们都是决策树的集合。特别是，它们在最重要的超参数之一 *max_depth* 上有很大的不同。随机森林通常 *max_depth* ≥ 15，而 GBDT 通常为 4 ≤ *max_depth* ≤ 8。在本文中，我们将探究这种差异的原因。

## 偏差-方差权衡

我们简单回忆一下偏差-方差权衡的性质。模型的预期损失可以分解为三个部分:偏差、方差和噪声。

*   偏差衡量模型的系统损失。具有高偏差的模型表达能力不足以很好地拟合数据(拟合不足)。
*   方差衡量由于模型对数据集波动的敏感性而造成的损失。具有高方差的模型对数据中的虚假模式过于敏感(过度拟合)。
*   噪声是损耗中不可避免的部分，与我们的模型无关。这也被称为贝叶斯误差。

理想情况下，我们希望最小化偏差和方差，但通常这不能同时实现。因此，在偏差和方差之间有一个权衡。例如，使我们的模型更有表现力会减少偏差，但会增加方差。

## 决策图表

决策树是集成方法的优秀基础学习器，因为它可以通过简单地调整 *max_depth* 来轻松执行偏差-方差权衡。原因是决策树非常擅长捕捉不同特征之间的交互，一棵树捕捉到的交互顺序是由其 *max_depth* 控制的。例如， *max_depth* = 2 表示捕获了 2 个特征之间的交互，但没有捕获 3 个特征之间的交互。

*   浅树只能捕获低阶相互作用，因此它们具有高偏差和低方差。
*   深树可以模拟复杂的交互，并且容易过度拟合。它们具有低偏差和高方差。

通过调整 *max_depth* 来查看偏差-方差权衡的另一种方法是估计叶节点的数量。决策树最多可以有*个 2^max_depth* 个叶节点。叶节点越多，树划分不同数据点的容量就越大。

## 随机森林

随机森林使用 bagging 的修改来构建去相关的树，然后对输出进行平均。由于这些树是同分布的，随机森林的偏差与任何单个树的偏差相同。因此，我们希望随机森林中的树具有低偏差。

另一方面，这些树单独具有高方差是好的。这是因为平均树减少了方差。结合这两个结果，我们最终得到具有低偏差和高方差的深度树。

## 梯度推进决策树

梯度增强以连续的方式构建树。粗略地说，它试图通过将新的树拟合到损失函数的负梯度来减少集合的损失，从而有效地执行梯度下降。每当集合增加一个新的树，它的模型复杂度增加，它的整体偏差减少，即使新的树相当简单。

另一方面，虽然没有太多关于梯度增强的方差的理论研究，但是经验结果表明，集成的方差在增强过程中没有被有效地减小。因此，对于 GBDT，我们使用具有高偏差和低方差的浅树。

## 进一步阅读

1.  [这](https://devblogs.nvidia.com/bias-variance-decompositions-using-xgboost/)是一篇关于 XGBoost 和随机森林关于各种超参数的偏差-方差分解的博文。
2.  偏差-方差分解的一个很好的参考文献是[1]。
3.  [2]的第 10、15 章非常详细地讨论了随机森林和 GBDT 的理论背景。

## 参考

1.  页（page 的缩写）多明戈斯，[一个统一的偏差-方差分解及其应用](https://homes.cs.washington.edu/~pedrod/papers/mlc00a.pdf) (2000)，ICML 2000。
2.  J.H. Friedman，R. Tibshirani 和 T. Hastie，*统计学习的要素*，第二版，2009 年，施普林格。