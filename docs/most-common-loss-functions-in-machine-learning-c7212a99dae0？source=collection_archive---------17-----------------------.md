# 机器学习中最常见的损失函数

> 原文：<https://towardsdatascience.com/most-common-loss-functions-in-machine-learning-c7212a99dae0?source=collection_archive---------17----------------------->

## 现实世界中的 DS

## 每个机器学习工程师都应该了解机器学习中这些常见的损失函数，以及何时使用它们。

> 在[数学优化](https://en.wikipedia.org/wiki/Mathematical_optimization)和[决策理论](https://en.wikipedia.org/wiki/Decision_theory)中，损失函数或成本函数是将[事件](https://en.wikipedia.org/wiki/Event_(probability_theory))或一个或多个变量的值映射到[实数](https://en.wikipedia.org/wiki/Real_number)上的函数，该实数直观地表示与该事件相关的一些“成本”。
> ——[维基百科](https://en.wikipedia.org/wiki/Loss_function)

![](img/d97bcee98a515aa9bb9675ce1727a9c2.png)

[约什·罗斯](https://unsplash.com/@joshsrose?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

作为一个核心元素，损失函数是一种评估机器学习算法的方法，它可以很好地模拟您的特征数据集。它被定义为**衡量你的模型在预测预期结果方面有多好。**

***成本函数*** 和 ***损失函数*** 指同一上下文。成本函数是作为所有损失函数值的平均值计算的函数。然而，损失函数是针对每个样本输出与其实际值的比较来计算的。

***损失函数与你已经建立的模型的预测直接相关。因此，如果您的损失函数值较小，您的模型将提供良好的结果。损失函数，或者更确切地说，用于评估模型性能的成本函数，需要最小化以提高其性能。***

现在让我们深入研究损失函数。

广泛地说，损失函数可以根据我们在现实世界中遇到的问题类型分为两大类—[](https://en.wikipedia.org/wiki/Loss_functions_for_classification)**分类和**回归**。在分类中，任务是预测问题所处理的所有类别各自的概率。相反，在回归中，任务是预测关于学习算法的一组给定独立特征的连续值。**

```
**Assumptions:**
    n/m — Number of training samples.
    i — ith training sample in a dataset.
    y(i) — Actual value for the ith training sample.
    y_hat(i) — Predicted value for the ith training sample.
```

# **分类损失**

## **1.二元交叉熵损失/对数损失**

**这是分类问题中最常用的损失函数。交叉熵损失随着预测概率收敛到实际标签而减少。它测量分类模型的性能，该模型的预测输出是介于 0 和 1 之间的概率值。**

****当类别数为 2 时，*二元分类*****

**![](img/17061658ec554151f795222e0d92b5e7.png)**

****当类别数大于 2 时，*多类别分类*****

**![](img/b8d780bb64fe7ebaa6c86b99ea005c8a.png)****![](img/b2222151ef3ac4767cee1b46eee9b070.png)**

**交叉熵损失公式是从正则似然函数中推导出来的，但是加入了对数。**

## **2.铰链损耗**

**用于分类问题的第二常见损失函数和交叉熵损失函数的替代函数是铰链损失，主要用于支持向量机(SVM)模型评估。**

**![](img/62c58841808e8c33ea3262e481d555a2.png)****![](img/adc53276a54fcb43e2d18bf77ac6212f.png)**

> ***铰链损失不仅惩罚错误的预测，也惩罚不自信的正确预测。它主要用于类别标签为-1 和 1 的 SVM 分类器。确保将恶性分类标签从 0 更改为-1。***

**![](img/16d28840de25c623ff766e4eaf468a9f.png)**

**照片由 [Jen Theodore](https://unsplash.com/@jentheodore?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄**

# **回归损失**

## **1.均方误差/二次损耗/ L2 损耗**

**MSE 损失函数被定义为实际值和预测值之间的平方差的平均值。它是最常用的回归损失函数。**

**![](img/8554425e7ceda1beb1c20886c5f74349.png)****![](img/c9132a1bc37272922980a83b62d5666f.png)**

**对应的代价函数是这些**平方误差**的**均值**。MSE 损失函数通过平方它们来惩罚产生大误差的模型，并且该属性使得 MSE 成本函数对异常值不太稳健。因此， ***如果数据容易出现很多离群值，就不应该使用。*****

## **2.平均绝对误差/ L1 损耗**

**MSE 损失函数被定义为实际值和预测值之间的绝对差值的平均值。这是第二个最常用的回归损失函数。它测量一组预测中误差的平均大小，不考虑它们的方向。**

**![](img/bd8348c28f43455c8ce8f2be610881e4.png)****![](img/9e5f098c21be00a0fb14df83b993a567.png)**

**对应的代价函数是这些**绝对误差(MAE)** 的**均值**。与 MSE 损失函数相比，MAE 损失函数对异常值更稳健。因此， ***在数据容易出现很多离群值的情况下应该使用。*****

## **3.Huber 损失/平滑平均绝对误差**

**Huber 损失函数被定义为当𝛿 ~ 0 时 MSE 和当𝛿 ~ ∞(大数)时 MAE 接近**时 MSE 和 MAE 损失函数的组合。它是平均绝对误差，当误差很小时，它变成二次误差。使误差为二次型取决于误差有多小，这是由一个可以调节的超参数𝛿(δ)控制的。****

**![](img/ae2b32a66feeac088062959b4f1c5568.png)****![](img/50b5cb58e6a4da4a78c5041cebb9d70a.png)**

**增量值的选择至关重要，因为它决定了您愿意将什么视为异常值。因此，取决于超参数值，与 MSE 损失函数相比，Huber 损失函数对异常值不太敏感。因此， ***如果数据容易出现异常值，可以使用它，并且*我们可能需要训练超参数 delta，这是一个迭代过程。****

## **4.对数损失**

**对数余弦损失函数被定义为预测误差的双曲余弦的对数。这是回归任务中使用的另一个函数，比 MSE 损失平滑得多。它具有 Huber 损失的所有优点，并且它在任何地方都是两次可微的，不像 Huber 损失，因为像 XGBoost 这样的一些学习算法使用牛顿法来寻找最优值，因此需要二阶导数( *Hessian* )。**

**![](img/139282adfe2a1985be9ca7e3e6266832.png)****![](img/eccf1cadf1be41b0178377d97a398f65.png)**

> **`*log(cosh(x))*` *约等于* `*(x ** 2) / 2*` *为小* `*x*` *，以* `*abs(x) - log(2)*` *为大* `*x*` *。这意味着“logcosh”的工作方式很大程度上类似于均方误差，但不会受到偶尔出现的严重错误预测的强烈影响。
> —* [*Tensorflow 文档*](https://www.tensorflow.org/api_docs/python/tf/keras/losses/logcosh)**

## ****5。分位数损失****

**分位数是一个值，低于这个值，一个组中的一部分样本就会下降。机器学习模型通过最小化(或最大化)目标函数来工作。顾名思义，分位数回归损失函数用于预测分位数。对于一组预测，损失将是其平均值。**

**![](img/3d744c02c693f84fd142f732c32cb38c.png)****![](img/a9ec9fda72373ed86f80ea982b0bff4a.png)**

**[分位数损失函数](/deep-quantile-regression-c85481548b5a)在我们对预测区间而不仅仅是点预测感兴趣时非常有用。**

**感谢您的阅读！希望这篇帖子有用。我感谢反馈和建设性的批评。如果你想谈论这篇文章或其他相关话题，你可以在这里或我的 [LinkedIn](https://www.linkedin.com/in/imsparsh/) 账户上给我发短信。**

**![](img/463b4304d8622ab8bd9075b5656497bc.png)**

**照片由[克劳福德乔利](https://unsplash.com/@crawford?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄**