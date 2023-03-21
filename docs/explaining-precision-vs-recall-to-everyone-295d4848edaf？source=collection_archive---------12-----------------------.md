# 精确和召回:你应该使用哪一个？

> 原文：<https://towardsdatascience.com/explaining-precision-vs-recall-to-everyone-295d4848edaf?source=collection_archive---------12----------------------->

## 数据科学概念

## 精确度、召回率、准确度和 F1 分数之间的差异

![](img/d2a472ecfca3f0da6b8d316dd9f609f0.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [engin akyurt](https://unsplash.com/@enginakyurt?utm_source=medium&utm_medium=referral) 拍摄的照片

随着你在数据科学的不同方面取得进展，你会遇到各种用于评估机器学习模型的评估指标。为了确定机器学习模型的有效性，必须对其进行评估。不评估机器学习模型，就无法运行它。可用于验证模型的评估指标有:

*   精确
*   回忆
*   F1 分数
*   准确(性)

每个指标都有自己的优点和缺点。确定使用哪一个是数据科学过程中的重要一步。

> [在这里注册一个中级会员，可以无限制地访问和支持像我这样的内容！在你的支持下，我赚了一小部分会费。谢谢！](https://marco-santos.medium.com/membership)

# 评估指标

我们将解释上面列出的评估指标之间的差异。但是首先，为了理解这些指标，你需要知道什么是 ***假阳性和假阴性*** 以及两者之间的区别。请参阅下面的文章，了解两者之间的区别:

[](/false-positives-vs-false-negatives-4184c2ff941a) [## 假阳性还是假阴性:哪个更糟？

### 解释第一类和第二类错误

towardsdatascience.com](/false-positives-vs-false-negatives-4184c2ff941a) 

为了计算不同的评估指标，您需要知道*假阳性(FP)、假阴性(FN)、真阳性(TP)和真阴性(TN)* 的数量。了解它们之间的区别也是有帮助的。

## 准确(性)

让我们从四个评估指标中最简单的一个开始——准确性。准确性是对我们的模型在观察总数中正确预测的观察数的简单衡量:

> 准确度= (TP + TN) / (TP + TN + FP + FN)

![](img/8464bbfb26e2cb1f9cd1e7bea4c1995e.png)

照片由 [Igal Ness](https://unsplash.com/@igalness?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

例如，假设我们有一台机器可以对水果进行分类，看它是不是苹果。在数百个苹果和桔子的样本中，机器的精确度将是它正确分类为*苹果*的苹果数量和它分类为*非苹果*的桔子数量除以苹果和桔子的总数。只要苹果和橘子的数量相同，这就是一个简单有效的测量方法。否则，我们可能不得不使用不同的评估标准。

## 精确

精度是对我们的模型正确预测的观察值与正确和不正确预测值之比的度量。这里的观察意味着我们试图预测的事情。

![](img/d27c02f85e37ac87f83b85b54156d4d9.png)

[李仁港](https://unsplash.com/@laniel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

> 精度= TP / (TP + FP)

使用我们的苹果和橙子的例子，precision 将测量正确分类的苹果的数量除以正确标记为苹果的苹果*和错误标记为苹果的橙子*的数量。换句话说，精度衡量了我们分类的苹果中有多少实际上是橙子。**

## 回忆

召回是对我们的模型在观察总量中正确预测的观察数量的度量。在这里，观察也意味着我们试图预测的事情。

![](img/817756c027d9bbf6865c28d693743a54.png)

Benjamin Wong 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

> 召回= TP / (TP + FN)

在我们的苹果和橘子的例子中，召回测量正确标记的苹果数量除以存在的苹果总量。换句话说，回忆衡量的是在整个水果样本中我们可能漏掉了多少个苹果。

## F1 分数

如果我们把注意力放在一个分数上，我们可能会忽略另一个分数。为了解决这个问题，我们可以使用 F1 分数，它在精确度和召回分数之间取得了平衡。要计算 F1 分数，您需要知道精确度和召回分数，并将它们输入到以下公式中:

> F1 得分= 2 *(精确度*召回率)/(精确度+召回率)

使用我们的苹果和橘子的例子，F1 分数将计算精度和召回之间的平衡。它将测量被错误分类为苹果的桔子的数量(*假阳性*)和没有被正确分类为苹果的苹果的数量(*假阴性*)。

# 使用哪个指标？

在构建和评估机器学习模型时，您必须能够确定要使用的最有效的评估指标。最合适的度量标准完全取决于问题。

## 不是准确性

大部分时间你会处理 ***不平衡*** 数据集。使用我们的苹果和橘子的例子，在现实世界中，你很可能会有一个不平衡的样本，可能是 1 比 3 的苹果和橘子。在这种情况下，使用准确性作为评估标准是不合适的，因为准确性会歪曲我们模型的有效性。

如果数据集是真正平衡的，那么准确性可能是最合适的度量标准。然而，大多数时候情况并非如此。

# 不平衡数据的度量

由于您最有可能处理不平衡的数据，因此精确度和召回率(以及 F1 分数)将是您的首选评估指标。使用哪一个将再次完全取决于您的模型的目标。

## 有毒的苹果

![](img/14e3d395a3053cb7b7bd011311e9b5f9.png)

照片由[玛利亚·特内娃](https://unsplash.com/@miteneva?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

假设我们试图检测一个苹果是否有毒。在这种情况下，我们希望*减少*假阴性的数量，因为我们希望不遗漏该批次中的任何毒苹果。**回忆**将是这里使用的最佳评估指标，因为它衡量了我们可能遗漏了多少毒苹果。我们并不太在意给苹果贴上有毒的标签，因为我们宁愿安全也不愿后悔。

## 股票投资

![](img/e2b72989f89d5abb21002f0e943a9e6a.png)

[杰米街](https://unsplash.com/@jamie452?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

抛开苹果的例子，假设我们开发了一个机器学习模型，来确定一只股票是否是一个好的投资。如果我们要把我们的全部净值都投入到这只股票中，我们最好希望我们的模型是正确的。**精度**将是这里使用的最佳度量，因为它决定了我们模型的正确性。只要我们的钱流向了由我们的模型正确预测的升值股票，我们就可以承受错过一些有利可图的股票投资。

如果您想了解使用精选评估指标预测股票投资未来的真实机器学习模型，请查看以下文章:

[](https://medium.com/better-programming/teaching-a-machine-to-trade-stocks-like-warren-buffett-part-ii-5d06427b13f7) [## 教机器像沃伦·巴菲特一样交易股票，第二部分

### 利用机器学习进行股票基本面分析

medium.com](https://medium.com/better-programming/teaching-a-machine-to-trade-stocks-like-warren-buffett-part-ii-5d06427b13f7) 

## F1 成绩？

有时候，最大化模型的精确度和召回分数是最好的。我们什么时候想最大化这两个分数？如果我们希望我们的模型是正确的，不会错过任何正确的预测。基于这一事实，你可能会得出这样的结论:F1 分数是最值得使用的分数。虽然 F1 分数在精确度和召回率之间取得了平衡，但它并没有优先考虑其中一个。有时，根据我们面临的问题，优先考虑一个可能是最好的。

# 结论

正如您在本文中所了解到的，有几种不同的评估指标可用于您的机器学习模型。每种度量标准都有其特定的优点和缺点。由您决定使用哪一个来评估您的模型。在本文结束时，我们希望您了解了使用哪种评估指标以及何时应用它们。

[*在 Twitter 上关注我:@_Marco_Santos_*](https://twitter.com/_Marco_Santos_)