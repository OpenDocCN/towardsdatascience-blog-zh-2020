# 停止使用 p=0.05

> 原文：<https://towardsdatascience.com/stop-using-p-0-05-4a059e622c75?source=collection_archive---------24----------------------->

![](img/6a18dd61cd1ba54964906141b972e754.png)

布雷特·乔丹在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 停止使用任意的统计显著性临界值

你们中有多少人用 p=0.05 作为绝对临界值？p ≥ 0.05 表示不显著。没有证据。没有。然后 p < 0.05 great it’s significant. This is a crude way of using p-values, and hopefully I will convince you of this.

# What is a p-value?

A lot of us use p-values following this arbitrary cut off but don’t actually know the theoretical background of a p-value. A p-value is the probability, under the null hypothesis, of observing data at least as extreme as the observed data. It is not, for example, the probability that some population parameter x = 0\. x either equals 0 or it does not (in a frequentist setting).

So, the smaller the p-value, the more unlikely it is that this data would have been observed under the null hypothesis. In essence, the smaller the p-value, the stronger the evidence against the null hypothesis.

# What affects p-values?

Two things mainly. The first is the strength of effect. The greater the difference from the null hypothesis. The smaller the p-value will be.

The second is the sample size. The larger the sample, the smaller the p-value will be (if in fact the null hypothesis is false).

So, this means that if p ≥ 0.05, it could be because the effect isn’t that strong (or doesn’t exist) or that our sample is too small, resulting in our test being *动力不足*检测差异。

# 一些例子

## 致命的药物

假设我们正在研究一种新药的副作用。现在假设药物增加死亡率的证据 p=0.051。现在，如果我们用 p=0.05 作为临界值，那就太好了。没有证据表明这种药物会增加死亡率——让我们把它投入生产吧。现在想象死亡率增加的 p=0.049。哦不！有证据表明这种药物有害。我们不要把它投入生产。

从数学上来说，这两者没有什么区别。它们本质上是一样的。但是通过使用这个任意的截止值，我们得到了非常不同的结论。

## 这种药有效吗

现在想象另一种药物。我们有一个非常大的样本(n=10，000)，我们想知道这种药物是否能治愈癌症。所以我们得到 p=0.049，它能治愈癌症。太好了！这种药物治愈癌症的重要证据。给大家看看吧。

不过，这是一个大样本。难道我们不会期望 p 更小吗？这不是反对无效假设的有力证据。我们的结果有大约二十分之一的概率是偶然的。现在假设这种药真的很贵。我们真的要根据一些相当薄弱的证据就开始把它分发给每个人吗？大概不会。

当然，如果 p=0.001，这将是百分之一的几率，我们的结果是偶然的。这将是证明药物有效的更有力的证据。

# 那么我们应该如何解读 p 值呢？

作为一个连续的音阶。p 值越小，证据越强。但是，你应该考虑样本大小和效应大小。你也应该考虑你正在看的东西是积极的还是消极的。如果看看像我们的致命药物例子，我们应该关注，即使证据非常薄弱。然而，就像想知道一种药物是否有效一样，我们可以对我们的结果持更加怀疑的态度。

因此，希望在未来，你将不再使用 p=0.05 作为从阈值中挑选出来的某个阈值，而是考虑它的真实情况——反对零假设的证据权重。当然，如果你没有你需要的证据，这不一定是因为它不存在，这可能是因为你缺乏统计能力来检测一个影响。