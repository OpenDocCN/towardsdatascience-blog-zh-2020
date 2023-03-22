# Boruta 特征选择(Python 中的一个例子)

> 原文：<https://towardsdatascience.com/simple-example-using-boruta-feature-selection-in-python-8b96925d5d7a?source=collection_archive---------8----------------------->

## Python 中强大的 Boruta 特征选择库的快速示例

![](img/4a79ca08c1d79bbdfbf93f4995e7c077.png)

威廉·费尔克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

如果您没有使用 Boruta 进行特性选择，那么您应该尝试一下。它可以用于任何分类模型。Boruta 是一种基于随机森林的方法，因此它适用于像随机森林或 XGBoost 这样的树模型，但也适用于像逻辑回归或 SVM 这样的其他分类模型。

Boruta 迭代地去除统计上不如随机探针相关的特征(由 Boruta 算法引入的人工噪声变量)。在每次迭代中，被拒绝的变量在下一次迭代中不被考虑。它通常以一个良好的特征选择全局优化而告终，这也是我喜欢它的原因。

我们将看一个简单的随机森林示例来进行特性选择。这个故事的唯一目的是向您展示一个简单的工作示例，这样您也可以使用 Boruta。

## 导入数据集

这个例子将使用 sklearn 附带的 breast_cancer 数据集。您可以使用可选的 return_X_y 让它直接输出数组，如图所示。

## 创建您的分类器或回归对象

只需将数据与您选择的模型相匹配，现在就可以为 Boruta 做准备了。

**注意**:在进行特征选择时，我会拟合整个数据集。消除特征后，您可以进行训练/测试分割。

## 创建你的 Boruta 对象

现在创建一个 BorutaPy 特征选择对象，并使您的整个数据适合它。在拟合过程中，Boruta 将根据数据集的大小进行多次迭代的特性测试。Boruta 创建你的特征(噪波)的随机阴影副本，并根据这些副本测试该特征，以确定它是否比噪波更好，因此值得保留。它会自动检查可能会损害模型的交互。对于每次迭代，Boruta 将输出*确认的*、*暂定的*和*拒绝的*变量。

拟合后，Boruta 对象具有有用的属性和方法:

*   。support_ attribute 是一个布尔数组，用于回答-是否应该保留特征？
*   。ranking_ attribute 是等级的 int 数组(1 是最佳特征)
*   。transform(X)方法应用建议并返回调整后的数据数组。通过这种方式，你可以让博鲁塔管理整个考验。

**注意**:如果你得到一个错误(TypeError: invalid key)，在将 X 和 y 装配到选择器之前，尝试将它们转换成 numpy 数组。如果你遇到这个错误，需要帮助，请告诉我。

## 查看功能

下面是我写的一些快速代码，用来查看 Boruta 的输出结果。看起来我的 30 个特性中有 5 个被建议删除。代码的输出如下所示。

```
Feature: mean radius               Rank: 1,  Keep: True
Feature: mean texture              Rank: 1,  Keep: True
Feature: mean perimeter            Rank: 1,  Keep: True
Feature: mean area                 Rank: 1,  Keep: True
Feature: mean smoothness           Rank: 1,  Keep: True
Feature: mean compactness          Rank: 1,  Keep: True
Feature: mean concavity            Rank: 1,  Keep: True
Feature: mean concave points       Rank: 1,  Keep: True
Feature: mean symmetry             Rank: 1,  Keep: True
Feature: mean fractal dimension    Rank: 2,  Keep: False
Feature: radius error              Rank: 1,  Keep: True
Feature: texture error             Rank: 2,  Keep: False
Feature: perimeter error           Rank: 1,  Keep: True
Feature: area error                Rank: 1,  Keep: True
Feature: smoothness error          Rank: 3,  Keep: False
Feature: compactness error         Rank: 1,  Keep: True
Feature: concavity error           Rank: 1,  Keep: True
Feature: concave points error      Rank: 1,  Keep: True
Feature: symmetry error            Rank: 2,  Keep: False
Feature: fractal dimension error   Rank: 2,  Keep: False
Feature: worst radius              Rank: 1,  Keep: True
Feature: worst texture             Rank: 1,  Keep: True
Feature: worst perimeter           Rank: 1,  Keep: True
Feature: worst area                Rank: 1,  Keep: True
Feature: worst smoothness          Rank: 1,  Keep: True
Feature: worst compactness         Rank: 1,  Keep: True
Feature: worst concavity           Rank: 1,  Keep: True
Feature: worst concave points      Rank: 1,  Keep: True
Feature: worst symmetry            Rank: 1,  Keep: True
Feature: worst fractal dimension   Rank: 1,  Keep: True
```

## 后续步骤

既然我们已经确定了要删除的特性，我们就可以放心地删除它们，继续我们的正常程序。您甚至可以使用`.transform()`方法来自动删除它们。你可能想尝试其他的特性选择方法来满足你的需求，但是 Boruta 使用了一种最强大的算法，并且快速易用。

祝你好运！

如果你遇到困难，请随时回复，我会尽我所能帮助你。