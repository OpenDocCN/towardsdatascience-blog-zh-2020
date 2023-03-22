# 测试的 p 值和功效

> 原文：<https://towardsdatascience.com/p-value-and-power-of-a-test-fde61dd8c742?source=collection_archive---------14----------------------->

## 解释 p 值

我们在统计类中都用过这个:如果 p <0.05\. This short blog is about an explanation of p-value, and how it is connected to the confidence interval and power of a test.

***p 值定义(来自统计 101/维基百科)*** ，则零假设被拒绝

p 值是在假设零假设正确的情况下，获得至少与实际观察到的结果一样极端的测试结果的概率。(难以轻易理解)

***尝试另一种方法***

零假设是我们想要检验的世界的描述(给一张优惠券是否会增加销售)。在这个世界上，我们相信赠送优惠券不会增加销售额。我们收集数据样本(希望它能代表总体)并获得我们的统计数据(均值、方差、中值等)。p 值是我们想要检查的世界丢弃我们从样本集合中获得的数字的概率。更多解释见下图。

![](img/a17c265d25180cc9e4b28d540a9bdcb4.png)

图一。解释 p 值

p 值只是告诉我们，我们的零假设是否为真，以便我们可以拒绝它，或者我们不能拒绝它。但是，我们不应该满足于 p 值的局限性

p 值没有给出零假设真实程度的概率。它只是给出一个二元决策，决定是否可以拒绝

1.  p 值不考虑效果的精确程度(因为它假设我们知道样本大小，并不能说明太多关于样本大小的信息)
2.  第一类错误会导致机会成本，但第二类错误可能更有害。考虑医学发展。拒绝一种可能有效的药物会让公司在投资上赔钱。然而，接受一种不起作用的药物会使人们的生命处于危险之中。我记得第一类和第二类错误是生产者的风险和消费者的风险。因此，检验的功效也很重要(检验的功效= 1-类型 II 错误=当零假设为假时拒绝零假设的概率)。更多关于测试能力的信息，请参见图 2。

图二。检验的势

![](img/2ebd7212107900873b26310afcc9cbcc.png)

置信区间也是根据*α计算的。置信区间*被解释为:(1)如果我们收集了 100 个样本(并为每个样本的统计量创建了一个置信区间)，这些包含统计量真实值(如总体平均值)的置信区间的频率趋向于 1-*α*。(2)当我们计算一个置信区间时，我们说在未来的实验中，真实的统计值将位于该置信区间内(这相当于现在在(1)中讨论的进行多次重复)。

The confidence interval is also calculated from *alpha. The confidence* interval is interpreted as: (1)if we collect 100 samples (and create a confidence interval for a statistic for each of the sample), the frequency of these confidence intervals which will contain the true value of the statistic (e.g. population mean) tends to be 1-*alpha*. (2)When we calculate one confidence interval, we say that in future experiments, the true statistic value will lie within that confidence interval (which is equivalent to doing multiple replications now a discussed in (1)).