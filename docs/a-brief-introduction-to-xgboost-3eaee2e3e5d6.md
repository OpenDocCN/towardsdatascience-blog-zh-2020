# XGBoost 简介

> 原文：<https://towardsdatascience.com/a-brief-introduction-to-xgboost-3eaee2e3e5d6?source=collection_archive---------12----------------------->

## 用 XGBoost 实现极限梯度提升！

## XGBoost 简介

XGBoost 是一个优化的梯度增强机器学习库。它最初是用 C++编写的，但是有其他几种语言的 API。核心 XGBoost 算法是可并行化的，即它在单个树中进行并行化。使用 XGBoost 有一些缺点:

1.  它是最强大的算法之一，具有高速度和高性能。
2.  它可以利用现代多核计算机的所有处理能力。
3.  在大数据集上训练是可行的。
4.  始终优于所有单一算法方法。

这里有一个简单的源代码来理解 XGBoost 的基础知识。

## 代码:

步骤 1:导入所有必要的库，对于 xgboost 我们需要导入“XGBoost”库，然后使用 pandas 读取文件。

步骤 2:将整个数据集按照称为 X 的特征和称为 y 的目标向量分割成样本矩阵，如下所示:

第三步:分割数据集进行训练和测试，这里我把它分割成 80%训练和 20%测试，然后实例化 XGBClassifier(因为输出需要分类的形式，要么是 1，要么是 0)。它的一些超参数是“客观的”，它指定了所用算法的类型，这里我使用了“二元:逻辑”,这意味着二元分类的逻辑回归，输出概率和“n_estimators”，它调整了决策树的数量。

第四步:拟合和预测模型，然后计算精度。

精确度:0.78333

## 让我们更深入地了解这个超级强大的算法！

XGBoost 通常使用树作为基本学习器，决策树由一系列二元问题组成，最终预测发生在叶子上。XGBoost 本身就是一个系综方法。迭代地构造树，直到满足停止标准。

XGBoost 使用 CART(分类和回归树)决策树。CART 是在每个叶子中包含实值分数的树，不管它们是用于分类还是回归。如果有必要，实值分数可以被转换成用于分类的类别。

# XGBoost 中的模型评估

在这里，我们将看到模型评估过程与*交叉验证的过程。*那么，什么是交叉验证呢？

## 交叉验证

交叉验证是一种稳健的方法，通过将许多非重叠的训练/测试拆分为训练数据并报告所有数据拆分的平均测试集性能，来估计模型在未知数据上的性能。

下面是一个例子，

*下面是上面代码涉及的步骤:*

第 2 行和第 3 行包括必要的导入。第 6 行包括加载数据集。第 9 行包括将数据集转换成 XGBoost 的创建者创建的优化的数据结构，该数据结构赋予包性能和效率增益，称为 ***DMatrix。*** 为了使用 *XGBoost cv* 对象，这是 XGBoost 的学习 API 的一部分，我们必须首先明确地将我们的数据转换成一个 *DMatrix。*

第 12 行包括创建一个参数字典来传递给交叉验证，这是必要的，因为 cv 方法不知道使用了哪种 XGBoost 模型。第 15 行包括调用 cv 方法，并传入存储所有数据的 DMatrix 对象(参数字典、交叉验证折叠数和需要构建的树数、要计算的度量、是否将输出存储为 pandas 数据帧)。第 18 行包括将公制转换为精确度，结果是 **0.88315。**

# XGBoost 与梯度升压

XGBoost 是一种更加**规则化的渐变增强形式**。XGBoost 使用高级正则化(L1 & L2)，这提高了模型泛化能力。

与梯度增强相比，XGBoost 提供了高性能。它的训练速度非常快，可以跨集群**并行化**。

# 什么时候使用 XGBoost？

1.  当有大量训练样本时。理想情况下，多于 1000 个训练样本，少于 100 个特征，或者我们可以说特征的数量< number of training samples.
2.  When there is a mixture of categorical and numeric features or just numeric features.

# When not to use XGBoost?

1.  Image Recognition
2.  Computer Vision
3.  When the number of training samples is significantly smaller than the number of features.

*这是关于 XGBoost 算法的简要信息。*