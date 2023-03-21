# 数据科学家的典型一天

> 原文：<https://towardsdatascience.com/a-typical-day-as-a-data-scientist-7509f8e51b5b?source=collection_archive---------12----------------------->

## 来自德克萨斯州的数据科学家的一天。

![](img/852d6a0b564a1a71234199e910ed5d7b.png)

[丹](https://unsplash.com/@dann?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/person?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上拍照。

# 目录

1.  介绍
2.  站立
3.  吉拉—冲刺
4.  代码审查
5.  模型过程
6.  摘要
7.  参考

# 介绍

数据科学家的一天可能会有所不同，但日常工作有一个遵循一定顺序的总体流程。你可以期待有一个站立，用吉拉(或类似的任务管理平台)组织你的任务，和一个代码审查。还有其他更大的项目里程碑，但这包括更大的时间范围，在本文中，我们将重点关注典型数据科学家的日常工作。

我曾在几家较大的科技公司和一家初创公司工作过，包括 Indeed、home away(Vrbo-Expedia)和 ScaleFactor。虽然我不会具体说明哪家公司有哪种流程，但我会从我过去的职位中总结我的经验。

# 站立

大多数科技公司都有一个惯例，那就是“起立”。通常在早上进行，团队会聚在一起，大多数情况下，顾名思义，站起来讨论三个主要问题。站立的目的是确保同一个团队的每个人都在同一页上。这是突出单个成员的工作，同时强调流程的哪些部分被暂停，以便尽快就决议采取行动的有效方式。以下是站立时提出的主要问题。

*   *你最近在做什么？*

—作为一名数据科学家，我通常会提到我目前正在从事的项目，以及我前一天做了什么。一个例子是:“昨天，我在调整集合机器学习模型的参数”。

*   你今天在做什么？

—您将在与关键流程的会议以及与利益相关者、数据工程师、软件工程师、产品经理或主题专家的其他会议之后，讨论您正在从事的工作。此步骤的一个例子是:“我将输出结果，并概述预测变化之间的差异以及我调整的所有参数各自的准确性”。

*   *有没有屏蔽器？*

—首先，这一步可能看起来令人尴尬或不舒服，因为你在强调你不知道的东西，或者在其他情况下，因为你当前控制之外的外部过程而不起作用的东西。但是，当你与你的团队会面时，你会变得更容易敞开心扉，这最终会成为你一天中的一个关键部分，为高效和有效的变革创造一个环境。拦截器的一个例子是:“对于我正在调整的一个参数，我的 Jupyter 笔记本一直坏，我想不出测试这个重要参数的方法”。也许，有人会在几秒钟内知道这个问题的答案，你就不必浪费一整个星期的时间试图自己找出答案。

# 吉拉—冲刺

有几个像吉拉一样组织任务的平台，所以如果你在未来的公司不使用它，你可能会遇到类似的事情。它的工作方式是显示你的 sprint(*通常一两周长*)，并根据任务在你的数据科学过程中所处的位置对其进行分组。

首先，您将创建票证。票据本质上是一个项目，它概括了一个问题、一个数据拉取、一个请求，并且有一个带有注释的描述。基于完成任务的难度或预计时间，您可以应用故事点，以便利益相关者或其他数据科学家知道您预计需要多少天来完成任务。门票也可以细分。任务单或故事单就是一个例子。一个故事是一个项目的更大的总结，比如“流失预测模型”。它将链接与该故事相关的任务单，以便您可以将项目组织成更小的块，其他工程师、科学家和经理可以被分配到这些任务单。任务单的一个例子是:“代码/。随机森林参数网格的 py 文件”。

还有其他类似的票证类型，但命名约定不同，称为问题类型:

```
 **sub-task** - a smaller drill down of a task **bug** - an error in the code **epic** - the overarching theme of a data science team ("Churn") **improvement** - addition to an existing product (usually - engineers) **new feature** - a new part of a product (usually - engineers) **story** - logs the associated task tickets **task** - a main task assigned to a team member
```

在描述 sprint 的特性之前，你可以把你的票放在一个“backlog”里。这个区域存放下一个票据，或者在 sprint 中没有当前票据重要的票据。

吉拉及其 sprint 的一个例子如下:

*   规划/可用

—您将进行本次冲刺的入场券

*   在发展中

—您当前正在处理的票证

*   测试

—您为其编写代码并正在测试的票证

*   回顾

—票证已完成测试，但正在审核中，通常带有 GitHub 拉取请求

*   完成的

—一旦你最终完成了任务

一个完整而典型的大纲将包括这些步骤或类似的内容:

```
1\. Planning/Available2\. In Progress3\. Testing4\. Review5\. Done
```

虽然吉拉和 sprints 可能看起来势不可挡，但关键是这些工具和过程能让你的项目和团队更有效率。因此，这些带有许多定义的票证可以按照您想要的任何方式使用。这取决于您和您的团队使用这些标签和功能来改善您的数据科学流程。你可以在这里找到更多关于吉拉和短跑的信息。

# 代码审查

![](img/f7b3e8de36fced75cb1c4e1798d9f248.png)

由[李·坎贝尔](https://unsplash.com/@leecampbell?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/code-review?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【3】上拍摄的照片。

代码审查对于拥有一个成功的数据科学团队非常重要。一些公司或团队将只整合一个 GitHub pull 请求，而其他公司或团队仍将面对面(*或电话会议*)，并共享他们的屏幕和代码。此外，一些团队可能每隔一天而不是每天执行一次代码审查。

代码审查有助于确保您的团队了解您的代码变更。在你花了大部分时间编写和组织代码之后，现在是时候与你的队友分享了。这是有益的，就好像你被困在一些代码片段上，其他数据科学家可以帮助你，你也可以帮助他们。

总而言之，代码审查有以下好处:

*   *团队意见一致*
*   *代码正确*
*   *代码高效*
*   *合作创新*
*   *向他人解释代码有助于你更好地了解你的代码*

# 模型过程

虽然建模过程需要几天、几周甚至几个月的时间来完成，但是这个过程的每个部分都有一个日常的方面。我将为一个数据科学家描述模型过程的主要步骤；提出问题，举例回答。数据科学家的典型建模过程如下所示:

**业务问题陈述** —出现了什么问题，数据科学如何解决？有没有一些我们可以自动化的手动流程？

例子:客户在不断变化。

**需求收集** —这个项目什么时候到期？谁会参与其中？涉众想要什么样的可交付成果？结果需要每天、每周或每月更新吗？

*例如:我们希望客户流失的 CSV 输出，以及他们每周流失的概率。*

**数据位置** —数据在哪里？我们需要访问 API 来获取更多数据吗？这已经在本地数据库中了吗？

例:大量客户的数据在我们的本地 SQL 数据库中。

**探索性数据分析** —分布是什么样的？是否存在缺失值？我们有足够的数据吗？我们需要更多的观察吗？

*示例:当缺少值时，我们可以估算该字段的平均值。*

**特征工程** —哪些特征是重要的？哪些是多余的？我们需要查看相关性/多重共线性吗？

*示例:使用 L1 特征选择移除冗余的、不重要的特征。*

**基础模型** —我们需要一个用 Python 制作的通用算法，还是需要一个实际的分类模型？最小可行产品是什么？

*示例:导入没有参数调整的随机森林模型。*

最终模型——我们需要多个模型吗？有些人比其他人做得更好？我们将使用哪些成功指标，为什么？

*示例—我们可以对这些观察使用随机森林，对其他观察使用 xgboost，精确度是关键指标。*

**模型迭代** —我们需要调整参数吗？之前的结果看起来怎么样？我们能让模型火车更快更省钱吗？

*示例—我们使用了一个参数网格，发现我们可以获得很高的准确度，同时将训练时间减少 20%。*

**结果输出** —输出是什么样子的？它应该只是一个 CSV 还是插入到 SQL 数据库中的数据？

*示例—我们发现，我们可以在这个 SQL 数据库中用预测的客户流失标签和概率来更新我们的客户。*

**结果解释** —您将如何向利益相关者以及其他数据科学家解释您的模型及其结果？你会使用什么可视化工具或平台？

*示例—我们可以使用 Tableau 在一个漂亮的彩色编码图表中显示我们的预测和概率得分。利益相关者可以访问这个仪表板。*

# 摘要

![](img/164d213d586a18b6a947f13a5d956341.png)

Arnaud Mesureur 在[Unsplash](https://unsplash.com/s/photos/sunset-city?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【4】上拍摄的照片。

有大量的数据科学家和公司。其中许多地方会有所不同，但它们的一些流程会有所重叠。我已经讨论了公司如何形成数据科学过程的类似方法。你可以期望有一个站立会议，使用吉拉或类似的平台来组织你的 sprint，并在代码评审中分享你的代码变更。数据科学家的日常工作可能会有所不同，但我们已经介绍了一天中最常见的步骤以及典型数据科学家的建模流程。

总体而言，数据科学可以涵盖工程和产品等业务的多个方面。数据科学家的职位可以有很多变化，但我希望我能阐明数据科学家每天将参与的主要流程和任务。

如果你想知道数据科学家通常会在哪里出错，以便你可以在未来避免同样的错误，请查看我的另一篇文章[5]。

[](/10-mistakes-ive-made-as-a-data-scientist-f6118184e69) [## 我作为数据科学家犯下的 10 个错误

### 可能出错的地方以及如何解决的例子

towardsdatascience.com](/10-mistakes-ive-made-as-a-data-scientist-f6118184e69) 

*我希望你觉得这篇文章有趣并且有用。感谢您的阅读！*

# 参考

[1]照片由[丹](https://unsplash.com/@dann?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/person?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2018)上拍摄

[2]阿特兰西斯，[吉拉软件](https://www.atlassian.com/software/jira?&aceid=&adposition=&adgroup=102002879695&campaign=10150459643&creative=429724101024&device=c&keyword=jira&matchtype=e&network=g&placement=&ds_kids=p54278903580&ds_e=GOOGLE&ds_eid=700000001558501&ds_e1=GOOGLE&gclid=Cj0KCQjwoub3BRC6ARIsABGhnyY6ykqQJVsukE7DIfcjL0LBHRMY6U08bYVDDhnHxV0LKwVV8F1Z7JQaAraBEALw_wcB&gclsrc=aw.ds)，(2020)

[3]李·坎贝尔在 [Unsplash](https://unsplash.com/s/photos/code-review?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2016)

[4]Arnaud Mesureur 在 [Unsplash](https://unsplash.com/s/photos/sunset-city?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2016)

[5] M.Przybyla，[我作为数据科学家犯下的 10 个错误](/10-mistakes-ive-made-as-a-data-scientist-f6118184e69)，(2020)