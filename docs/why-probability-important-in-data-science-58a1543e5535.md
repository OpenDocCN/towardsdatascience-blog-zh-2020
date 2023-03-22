# 为什么概率在数据科学中很重要

> 原文：<https://towardsdatascience.com/why-probability-important-in-data-science-58a1543e5535?source=collection_archive---------45----------------------->

## 数据科学

## 了解更多关于概率的知识，以及为什么我们数据科学家需要了解它

![](img/7eb3adc655a3c76dda40cf23bd3d5312.png)

Riho Kroll 在 [Unsplash](https://unsplash.com/s/photos/dice?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

L 运用概率基础知识应用数据科学概念。我们可以说出一些流行的术语，如“决策”、“推荐系统”、“深度学习”。像 [Tensorflow](https://www.tensorflow.org/) 或 [Pytorch](https://pytorch.org/) 这样著名的深度学习框架都是基于概率的概念来实现的。因此，了解什么是概率以及它是如何工作的，将有助于我们在学习数据科学的道路上走得更远。

# 依赖和独立

粗略地说，我们说两个事件 E 和 F 是相关的，如果知道一些关于 E 是否发生的信息，就给了我们关于 F 是否发生的信息(反之亦然)。否则他们就是独立的。

例如，如果我们掷一枚硬币两次，知道第一次掷的是正面并不能告诉我们第二次掷的是正面。这些事件是独立的。另一方面，知道第一次翻转是否是正面肯定会给我们关于两次翻转是否都是反面的信息。(如果第一次翻转是正面，那么肯定不是两次翻转都是反面。)这两个事件是相依的。数学上，我们说两个事件 E 和 F 是独立的，如果它们都发生的概率是每个事件发生的概率的乘积:

```
P (E, F) = P( E) * P( F)
```

在上面的例子中，“第一次翻转正面”的概率是 1/2，“两次翻转反面”的概率是 1/4，但是“第一次翻转正面和两次翻转反面”的概率是 0。

# 贝叶斯定理

要了解贝叶斯定理是如何工作的，请尝试回答下面的问题:

```
Steve is very **shy and withdrawn**, invariably helpful but **with very little interest in people** or in the world of reality. **A meek and tidy soul**, he has a need for order and structure, and **a passion for detail**. How likely Steve to be one of those:
1\. A librarian
2\. A farmer
```

很多时候，我们(非理性地)会认为史蒂夫“最有可能成为”一名图书管理员。嗯，如果我们了解农夫和图书管理员的比例，我们就不会这么想了。这么说吧，大概是 20/1。

在图书管理员类别中，假设 50%的图书管理员符合问题中的性格特征，而在农民类别中，假设只有 10%。

好吧，假设我们有 10 个图书管理员和 200 个农民。给定描述的农民的概率将是:

```
5/(5+20) = 1/5 ~ 20%
```

如果我们猜测候选人可能是图书管理员。我们可能错了。

下面是贝叶斯定理的公式。

```
P(H|E) = P(H)*P(E|H) / P(E)
```

其中:

```
P(H) = Probability of hypothesis is true, before any evidenceP(E|H) = Probability of seeing the evidence if hypothesis is trueP(E) = Probability of seeing the evidenceP(H|E) = Probability of hypothesis is true given some evidence
```

# 随机变量

是一个变量，其可能值具有相关的概率分布。一个非常简单的随机变量，如果硬币正面朝上，等于 1；如果硬币反面朝上，等于 0。一个更复杂的方法可能是测量投掷硬币 10 次时观察到的人头数，或者从范围(10)中选择一个数值，其中每个数字的可能性相等。相关分布给出了变量实现其每个
可能值的概率。硬币投掷变量在概率为 0.5 时等于 0，在概率为 0.5 时等于 1。range(10)变量的分布为 0 到 9 之间的每个数字分配概率 0.1。

# 连续分布

通常，我们会希望对结果的连续体进行分布建模。(出于我们的目的，这些结果将总是实数，尽管在现实生活中并不总是这样。)例如，均匀分布对 0 到 1 之间的所有数字赋予相等的权重。因为在 0 和 1 之间有无限多的数字，这意味着它分配给各个点的权重必然为零。为此，我们用概率密度函数(pdf)来表示连续分布，使得在某个区间看到某个值的概率等于该区间上密度函数的积分。

均匀分布的密度函数可以用 Python 实现，如下所示:

```
def uniform_pdf(x):
    return 1 if x >= 0 and x < 1 else 0
```

或者如果我们想为累积分布函数创建一个方法:

```
def uniform_cdf(x):
    if x < 0:
        return 0
    elif x < 1: return x
    else:
        return 1
```

# 结论

概率很有趣，但需要大量的学习。有很多关于概率的东西我在这篇文章中没有涉及，比如正态分布，中心极限定理，马尔可夫链或者泊松过程..所以花点时间去了解更多。

# 参考

[从零开始的数据科学](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwij2MHGqJXrAhWHH3AKHXDtB0cQFjAAegQIAhAB&url=https%3A%2F%2Fwww.amazon.com%2FData-Science-Scratch-Principles-Python%2Fdp%2F149190142X&usg=AOvVaw2zAwpQagSb7JA7HKl-KAew)书——乔尔·格鲁什著