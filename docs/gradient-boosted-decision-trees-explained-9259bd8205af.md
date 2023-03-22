# 梯度推进决策树-解释

> 原文：<https://towardsdatascience.com/gradient-boosted-decision-trees-explained-9259bd8205af?source=collection_archive---------2----------------------->

详细解释了 boosting 和 scikit-learn 实现

决策树建立在反复询问问题以划分数据的基础上。用决策树的可视化表示来概念化分区数据更容易:

![](img/41ef58195f9ee8204775643aa6efc168.png)

[图源](https://jakevdp.github.io/PythonDataScienceHandbook/05.08-random-forests.html)

一个决策树容易过度拟合。为了降低过度拟合的风险，组合了许多决策树的模型是首选的。这些组合模型在准确性方面也具有更好的性能。**随机森林**使用一种叫做**装袋**的方法来组合许多决策树以创建集合。打包简单地说就是并行组合。如果想了解决策树和随机森林的详细信息，可以参考下面的帖子。

[](/decision-tree-and-random-forest-explained-8d20ddabc9dd) [## 决策树和随机森林—解释

### 详细的理论解释和代码示例

towardsdatascience.com](/decision-tree-and-random-forest-explained-8d20ddabc9dd) 

在这篇文章中，我将介绍**梯度提升决策树**算法，该算法使用**提升**方法来组合各个决策树。

Boosting 是指将一个学习算法串联起来，从许多顺序连接的弱学习器中实现一个强学习器。在梯度提升决策树算法的情况下，弱学习器是决策树。

每棵树都试图最小化前一棵树的错误。boosting 中的树是弱学习器，但是连续添加许多树，并且每个树集中于前一个树的错误，使得 boosting 成为高效且准确的模型。与装袋不同，增压不涉及自举取样。每次添加新树时，它都适合初始数据集的修改版本。

由于树是按顺序添加的，boosting 算法学习起来很慢。在统计学习中，学习速度慢的模型表现更好。

![](img/6742d7587a1ac34fb299d67429c8de37.png)

随机森林

![](img/ab6cee029ce0da12a94c90f42b1e015b.png)

梯度推进决策树

梯度提升算法以这样的方式顺序地组合弱学习器，即每个新的学习器适合于来自前一步骤的残差，从而改进模型。最终的模型汇总了每一步的结果，从而形成了一个强学习者。**梯度提升决策树**算法使用决策树作为弱学习器。损失函数用于检测残差。例如，均方误差(MSE)可用于回归任务，对数损失(log loss)可用于分类任务。值得注意的是，当添加新的树时，模型中现有的树不会改变。添加的决策树符合当前模型的残差。步骤如下:

![](img/7059c7a1282b17ac6b8b142f936de7b2.png)

梯度增强决策树算法

下图显示了这些步骤:

![](img/af0913ecb2c9e5e0420a04ed873fcaa1.png)

[Quora 上的图片来源](https://www.quora.com/How-would-you-explain-gradient-boosting-machine-learning-technique-in-no-more-than-300-words-to-non-science-major-college-students)

# **学习率和 n _ 估计器**

超参数是学习算法的关键部分，它影响模型的性能和准确性。**学习率**和 **n_estimators** 是梯度推进决策树的两个关键超参数。学习率，表示为α，简单地表示模型学习的速度。每增加一棵树都会修改整个模型。修改的幅度由学习速率控制。引入学习率的梯度推进决策树算法的步骤:

![](img/f7845eb74e274820c19c3bd3f7d93bcd.png)

学习率为(α)的梯度推进决策树算法

学习率越低，模型学习越慢。较慢的学习速率的优点是模型变得更加健壮和通用。在统计学习中，学习速度慢的模型表现更好。但是，慢慢的学习是要付出代价的。训练模型需要更多的时间，这就引出了另一个重要的超参数。 **n_estimator** 是模型中使用的树的数量。如果学习率低，我们需要更多的树来训练模型。然而，我们需要非常小心地选择树的数量。使用太多的树会产生过度适应的高风险。

**关于过拟合的注释**

随机森林和梯度推进决策树之间的一个关键区别是模型中使用的树的数量。增加随机森林中的树木数量不会导致过度拟合。在某个点之后，模型的准确性不会因为添加更多的树而增加，但是也不会因为添加过多的树而受到负面影响。由于计算原因，您仍然不希望添加不必要数量的树，但是没有与随机森林中的树的数量相关联的过度拟合的风险。

然而，梯度推进决策树中的树的数量在过度拟合方面非常关键。添加太多的树会导致过度拟合，所以在某个时候停止添加树是很重要的。

# **利弊**

优点:

*   分类和回归任务都非常高效
*   与随机森林相比，预测更准确。
*   可以处理混合类型的特征，不需要预处理

骗局

*   需要仔细调整超参数
*   如果使用太多的树，可能会过拟合(n_estimators)
*   对异常值敏感

# **sci kit-学习示例**

我将带您浏览使用 scikit-learn 上的一个示例数据集[创建 GradientBoostingClassifier()的步骤。](https://scikit-learn.org/stable/datasets/index.html)

让我们从导入依赖项开始:

![](img/3d8aeb309b4a3e5ace011efda357c5ee.png)

加载我们刚刚导入的数据集:

![](img/9362cd337839d6c8b7d9ea27e6527d0d.png)

使用 train_test_split 模块将数据集分为训练集和测试集:

![](img/ad6b895650e88eab29db799c66ebbfbd.png)

然后，我们创建一个 GradientDescentClassifier()对象并拟合训练数据:

![](img/fae81e33ea694e284f78648a1fc98d71.png)

然后使用**预测**方法预测测试集的目标值，并测量模型的准确性；

![](img/3a116da12523513e333addf762bb6dea.png)

值得注意的是，与这个非常简单的例子相比，现实生活项目中的数据准备、模型创建和评估是极其复杂和耗时的。我只是想向你们展示模型创建的步骤。在现实生活中，你的大部分时间将花在数据清理和准备上(假设数据收集是由别人完成的)。您还需要花费大量时间对超参数模型的准确性进行多次调整和重新评估。

# **关于 XGBOOST** 的一个注记

XGBOOST(Extreme Gradient Boosting)是由陈天琦(Tianqi Chen)创立的梯度增强决策树的一个高级实现。它速度更快，性能更好。XGBOOST 是一个非常强大的算法，最近在机器学习竞赛中占据主导地位。我也会写一篇关于 XGBOOST 的详细文章。

感谢您的阅读。如果您有任何反馈，请告诉我。

# **我关于其他机器学习算法的文章**

*   [朴素贝叶斯分类器——解释](/naive-bayes-classifier-explained-50f9723571ed)
*   [支持向量机—解释](/support-vector-machine-explained-8d75fe8738fd)
*   [决策树和随机森林—解释](/decision-tree-and-random-forest-explained-8d20ddabc9dd)