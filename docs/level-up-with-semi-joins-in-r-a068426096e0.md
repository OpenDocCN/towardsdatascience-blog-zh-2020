# R 中带半连接的升级

> 原文：<https://towardsdatascience.com/level-up-with-semi-joins-in-r-a068426096e0?source=collection_archive---------31----------------------->

## 变异连接和过滤连接的区别

![](img/d24661e86c970f3ce8a267e0acf8df02.png)

图片由 [inmorino](https://pixabay.com/users/inmorino-13522936/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4587474) 来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4587474)

# 介绍

假设您已经对其他更常见的连接类型(内、左、右和外)有了一些了解；添加 semi 和 anti 可以证明非常有用，可以节省您原本可以采取的多个步骤。

在这篇文章中，我将只关注半连接；也就是说，semi 和 anti 之间有很多重叠，所以准备好了解一下这两者。

# 过滤连接

半连接和反连接与我刚才强调的其他四个有很大的不同；最大的区别是它们实际上被归类为所谓的过滤连接。

从语法上看，它与任何其他连接都非常相似，但其目的不是用额外的列或行来增强数据集，而是使用这些连接来执行过滤。

过滤连接不是通过添加新的信息列来分类的，而是有助于保留或减少给定数据集中的记录。

# 半连接

使用半连接的目的是获取一个数据集，并根据某个公共标识符是否位于某个附加数据集中对其进行过滤。

一个很好的方法就是编写替代方案。

# 机会数据集示例

假设我们有一个来自 salesforce 的数据集，其中包含我们已经处理或正在处理的所有交易或机会。

这个机会数据集为我们提供了许多关于交易本身的非常有用的信息。让我们假设它看起来像这样:

| opp_id | account_id |创建日期|结束日期|金额|阶段|

现在，假设我们需要过滤该数据集，以便只包括企业客户。碰巧的是，我们的机会数据集中没有可用于筛选的细分字段…我们必须利用其他地方的信息。

让我们假设企业帐户被跟踪的唯一位置是在一个随机的 excel 文件中。假设数据集看起来像这样:

|帐户标识|客户细分|

# 在一个没有半连接的世界里

根据我们现在对左连接的了解，我们可以做以下事情:

```
opportunities %>%
left_join(enterprise_accounts, by = 'account_id')%>%
filter(!is.na(customer_segment))
```

如您所见，我们可以将企业客户数据集与我们的主 opps 数据集左连接，如果没有匹配值，客户细分将为空，因此您可以添加一个筛选器语句，说明您只需要非空的案例。

这很好，并且有效地执行了我上面解释的相同功能。一件烦人的事情是它给了你一个新的字段，customer_segment，它对每条记录都是一样的。

之后，我们还可以添加一条 select 语句来提取该字段，这只是增加了另一行代码，您可以编写这些代码来实现该功能。

```
opportunities %>%
left_join(enterprise_accounts, by = 'account_id')%>%
filter(!is.na(customer_segment))%>%
select(-customer_segment)
```

假设您已经学习了内部连接，我们也可以用稍微简单一点的代码实现类似的功能。

```
opportunities %>%
inner_join(enterprise_accounts, by = 'account_id')%>%
select(-customer_segment)
```

# 使用半连接简化

现在让我们用一个半连接来进一步简化事情。

```
opportunities %>%
semi_join(enterprise_accounts, by = 'account_id')
```

这将使我们得到与上面每个例子完全相同的输出。它将筛选出在企业客户表中没有匹配 account_id 的机会记录。它不会向数据集中添加列或行。它专门存在，并用于过滤目的。

# 结论

现在，在短短的几分钟内，我们已经介绍了很多内容，并提供了一些 dplyr 功能，可以简化您的代码和工作流程。

我们了解到:

*   变异连接和过滤连接之间的区别
*   如何在没有半连接的情况下执行“过滤连接”
*   半连接的特定输出和意图
*   如何使用半连接

我希望这能对你作为数据专家的日常工作有所帮助。

祝数据科学快乐！