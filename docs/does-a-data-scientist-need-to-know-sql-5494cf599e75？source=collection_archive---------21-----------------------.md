# 一个数据科学家需要懂 SQL 吗？

> 原文：<https://towardsdatascience.com/does-a-data-scientist-need-to-know-sql-5494cf599e75?source=collection_archive---------21----------------------->

## 意见

## …还是有其他东西更适合您的数据需求？

![](img/c5a9cf42c7c8a4b918af32b4f0ecdbf7.png)

照片由[贝瑟尼·莱格](https://unsplash.com/@bkotynski?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/office-person?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上拍摄。

# 目录

1.  介绍
2.  结构化查询语言
3.  替代方法
4.  摘要
5.  参考

# 介绍

你可以把 10 个人放在一个房间里，很可能有 5 个数据科学家说他们需要 SQL 来工作，而另一半会回答说还有其他方法来争论他们的数据。那么，作为一名数据科学家，你用什么呢？可以用别的吗？是数据科学家的责任吗？下面，我将讨论 SQL 在数据科学中的作用，以及结构化查询语言的替代品。

# 结构化查询语言

![](img/3791cb3b54cf7b5cca5cfbd0345d93e6.png)

照片由[阿尔瓦罗·雷耶斯](https://unsplash.com/@alvarordesign?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【2】上拍摄。

您可能最终需要了解 SQL，这取决于您的公司和数据科学团队。一些团队有几名数据工程师和数据分析师，以及机器学习工程师，而一些团队可能只由一名数据科学家组成。如果你作为数据科学家需要了解 SQL，你很可能知道这个问题的答案是*。然而，讨论为什么你需要它，为什么你不需要它，以及什么时候你不需要它是有趣和重要的。此外，讨论这个问题也有助于新数据科学家在进入职场时熟悉期望值。*下面是我使用 SQL 的原因:**

*   我需要使用 SQL 查询表以获得有用的数据集
*   自主的感觉很好(*尽管帮助是受感激的*)
*   可以在现有的 SQL 查询中随时发现和创建新功能

虽然数据科学可以被视为一项职业，在这项职业中，您只关注 Python 和 R，以及复杂的机器学习算法，但如果不以某种方式利用 SQL 的优势，可能很难作为一个团队来执行您的数据科学过程。然而，根据您在数据科学中的特定角色，有时您并不完全需要它。

您可以参考替代方法的时候是，如果您从数据工程师或数据分析师以及类似职位的其他人那里获得了一些帮助。此外，您可能会发现自己不需要 SQL，因为 SQL 查询的角色根本不是您专业角色的一部分，因为您更专注于数据科学模型开发，例如在您已经收到的数据上测试各种机器学习算法。

# 替代方法

![](img/614c2748e898595603e2faad28b5267e.png)

[Bruce Hong](https://unsplash.com/@hongqi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/pandas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【3】上的照片。

当您已经通过从数据分析师或数据工程师处获取数据集来查询数据集时，数据集的下一步改进可以是创建新要素，而不仅仅是数据表中的字段。例如，如果您的数据集中有 10 个字段，您可以开发几个新的度量作为字段，而不是通过计算第 1 列和第 2 列来创建新的第 11 列。除了 SQL 本身，在[pandas](https://pandas.pydata.org/)【4】中有一个更简单的地方可以进行这种计算。这个库被数据分析师和数据科学家广泛使用是有原因的。

> 有了 pandas，你就能快速完成复杂的计算，而且只用一行代码。

> 有时我发现很难使用 SQL 对我的数据进行计算，因为它在视觉上显示为多行(仅是我的观点)*。*

*以下是一些常见的 pandas 数据框架操作，使您的数据集要素工程更加方便。*

```
* groupby* items* loc* iloc* iteritems* keys* iterrows* query (*this operation is quite similar to SQL quering, I highly recommend*)* aggregate* corr* mean, median, min, and max* quantile* rank* sum* std* var* append* merge* join* sort_values* isnull* notna* between_time
```

正如你所看到的，有大量的操作可以应用到你的熊猫数据帧上。完整的清单在这里【5】。

> 我最喜欢使用的是:

*   **groupby** —对数据进行分组时，对这些分组执行进一步的操作
*   **查询** —类似 SQL 的另一种查询方式，但在您的数据框架内

我个人发现计算新的字段或指标更容易，这些字段或指标最终将用于您的熊猫数据科学模型。然而，有些人喜欢只在 SQL 中执行他们的计算(*在一个地方管理所有的字段可能更简单*)。对我来说，好处是我不必一直追加到一个很长的查询中，这样当我想添加一个新的特性时，就非常简单和有效。

# 摘要

![](img/cd5b7da0e4adebacbff3cdf39a42462e.png)

在[Unsplash](https://unsplash.com/s/photos/happy-work?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【6】上由[米米·蒂安](https://unsplash.com/@mimithian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片。

一个数据科学家需要懂 SQL 吗？答案既有也有。这取决于你的公司、团队，有时还取决于你的偏好。您可以从使用 SQL 查询自己中受益，如果您不知道它，您可以学习。如果你喜欢像熊猫这样的替代方法，那么你可能会发现自己在一个更大的数据科学团队中。一些数据科学家同时使用 SQL 和 Python 来为他们各自的模型创建最终的数据集。pandas 最独特的部分是有一个类似 SQL 的查询操作，所以在 pandas 数据框架中可以混合使用这两种方法。

*如果你想了解更多的话，这是我的一篇讨论熊猫查询功能的文章*

[](/pandas-query-for-sql-like-querying-279dc8cbfe3f) [## 熊猫查询类似 SQL 的查询

### 使用 pandas 查询函数查询数据帧的数据科学家 python 教程

towardsdatascience.com](/pandas-query-for-sql-like-querying-279dc8cbfe3f) 

作为一名数据科学家，请随意在下面评论您更喜欢哪种或哪些方法。我希望你觉得我的文章有趣并且有用。谢谢你的阅读，我很感激！

# 参考

[1]照片由 [Bethany Legg](https://unsplash.com/@bkotynski?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/office-person?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2015)上拍摄

[2]阿尔瓦罗·雷耶斯[在](https://unsplash.com/@alvarordesign?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2018)

[3]Bruce Hong 在 [Unsplash](https://unsplash.com/s/photos/pandas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2018)

[4]熊猫发展团队， [*熊猫主页*](https://pandas.pydata.org/) ，(2008–2020 年)

[5]熊猫开发团队，[熊猫数据框架运营](https://pandas.pydata.org/pandas-docs/stable/reference/frame.html)，(2008–2020)

[6]照片由[米米·蒂安](https://unsplash.com/@mimithian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/happy-work?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2018)上拍摄

[7] M.Przybyla，[Pandas Query for SQL-like Query](/pandas-query-for-sql-like-querying-279dc8cbfe3f?sk=c446b545dabe94ce47aa588a0aa65d30)，(2020)