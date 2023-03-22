# 软件设计在数据科学中的重要性

> 原文：<https://towardsdatascience.com/importance-of-software-design-in-data-science-249ef7fbafc?source=collection_archive---------51----------------------->

## 意见

## 为什么不应该忽视数据科学和机器学习中的软件设计和开发原则

![](img/6bd1f16a6b6207e06a907c1ed418a76a.png)

[ev](https://unsplash.com/@ev?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/data-science?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

**更新，2021 年 2 月 26 日**:我会继续在帖子底部添加链接，以证明我的观点，这篇博文看起来可能像一篇观点文章，但它不是。当涉及到生产中的数据科学时，软件设计是一个事实。

# 背景

大家都把 R、Python、统计学、数据分析、机器学习的需求作为求职数据科学家的首要技能。我学过很多入门和高级的数据科学和机器学习课程。是的，你需要知道如何编码，但没人谈论的是“软件设计原则”。

# 软件设计和开发

我们要在任何数据科学或机器学习项目中写代码。生产环境需要大量代码。人们不能认为“加入几个 for 循环，创建一些变量，写一个列表理解”就是数据科学中编码的终结。数据科学中的大多数课程都是一样的，它们侧重于基本的 Python，并以列表理解结束。我们需要知道一般的软件设计原则，还需要了解我们正在使用的编程语言的不同特性。你可以创建一个工具，你可以分析一些东西，单个源代码文件可以是 100-500 行代码(或者更多)。你不能仅仅用基本变量和循环把这么多行代码拼凑在一起。实际上你可以，但这不会对所有人都有好处:对你、你的团队和你的组织(这反而会是一场噩梦)。这就是像面向对象编程这样的设计的用武之地。我每天都读书，到目前为止，唯一写过这方面文章的人是丽贝卡·维克里:

[](/how-to-learn-data-science-for-free-eda10f04d083) [## 如何免费学习数据科学

### 一个完整的学习路径，包括不花你一分钱的资源

towardsdatascience.com](/how-to-learn-data-science-for-free-eda10f04d083) 

这就是为什么我们也需要理解生成器和上下文管理器。随着数据科学和机器学习变得自动化，焦点转向云，我们甚至可能不需要知道编程语言(再过 15 年)。但是现在，我认为一个人需要知道基本的软件设计原则。您可以从一些资源开始:

 [## 大 O 批注|面试蛋糕

### 最后简单解释一下大 O 记数法。我会向你展示你在技术面试中所需要的一切…

www.interviewcake.com](https://www.interviewcake.com/article/python/big-o-notation-time-and-space-complexity) [](https://en.wikipedia.org/wiki/Object-oriented_programming) [## 面向对象编程

### 编辑描述

en.wikipedia.org](https://en.wikipedia.org/wiki/Object-oriented_programming)  [## 过程程序设计

### 过程化编程是一种编程范式，源于结构化编程，基于…

en.wikipedia.org](https://en.wikipedia.org/wiki/Procedural_programming) 

# 生产软件

然后就是数据科学行业的现实工作。一个很好的例子就是生产中的机器学习。是的，我们知道模型，知道如何在我们的机器和云上运行它们，知道如何让它们在黑客马拉松中更有效，这很好。但是生产环境是完全不同的。如果你是一名软件开发人员，那么你应该知道我在说什么:分段故障、崩溃、集成问题、版本控制问题等等。幸运的是，有几个人讨论过这个问题，我甚至看到了 Luigi Patruno 的博客:

[](https://mlinproduction.com/why-i-started-mlinproduction/) [## 为什么我开始生产 MLinProduction - ML

### 有没有发现自己在开始前应该多读一本书或者多上几门在线课程…

mlinproduction.com](https://mlinproduction.com/why-i-started-mlinproduction/) 

# 结论

是的，更大的图景是关于数据的。我们能从中得到什么，以及我们如何向利益相关者展示我们的见解，并告诉决策者从数据中能获得什么样的商业价值。所以你需要首先关注这一点。完成这些之后，不要忘记编码部分，它是产生所有这些结果的部分。它经常被忽视，但却是重要的一部分。您的预测和您的数据一样好，运行这些预测的软件和您对软件设计和开发的理解一样好。你不需要像软件开发人员一样掌握它，但你肯定需要知道基础知识。

**2020 年 11 月 12 日更新:**我刚刚发现了[丽贝卡·维克里](https://medium.com/u/8b7aca3e5b1c?source=post_page-----249ef7fbafc--------------------------------)的一个帖子，她在里面详细讲述了软件工程原理。比我写的还要好。在这里找到它:

[](/the-essential-skills-most-data-science-courses-wont-teach-you-5ceb0c4d17ce) [## 大多数数据科学课程不会教给你的基本技能

### …以及如何学习它们

towardsdatascience.com](/the-essential-skills-most-data-science-courses-wont-teach-you-5ceb0c4d17ce) 

**2021 年 2 月 26 日更新:**找到了软件工程(我称之为软件设计)在数据科学中真正重要的另一个证据。弗拉基米尔·伊格洛维科夫:

[](https://ternaus.blog/interview/2020/08/27/interview-to-analyticsindia-full-version.html) [## 《分析印度》杂志采访完整版:机器学习掌握之路…

### 本文是对 analyticsindiamag.com 的采访全文。从 2002 年到 2004 年，我在俄罗斯服役…

ternaus.blog](https://ternaus.blog/interview/2020/08/27/interview-to-analyticsindia-full-version.html) 

**2021 年 3 月 12 日更新:**这是 [Kurtis Pykes](https://medium.com/u/5ba760786877?source=post_page-----249ef7fbafc--------------------------------) 关于软件工程实践对数据科学有多重要的另一个观点:

[](/data-scientist-should-know-software-engineering-best-practices-f964ec44cada) [## 数据科学家应该知道软件工程的最佳实践

### 成为不可或缺的数据科学家

towardsdatascience.com](/data-scientist-should-know-software-engineering-best-practices-f964ec44cada)