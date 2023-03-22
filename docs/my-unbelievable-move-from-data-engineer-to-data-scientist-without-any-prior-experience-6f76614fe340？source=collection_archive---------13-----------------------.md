# 我从数据工程师到数据科学家的不可思议的转变，之前没有任何经验

> 原文：<https://towardsdatascience.com/my-unbelievable-move-from-data-engineer-to-data-scientist-without-any-prior-experience-6f76614fe340?source=collection_archive---------13----------------------->

## 通过掌握一项基本技能

![](img/b32ff9eee9c540be245c4879ff314d1a.png)

照片由 [PhotoMIX 公司](https://www.pexels.com/@wdnet?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从 [Pexels](https://www.pexels.com/photo/building-metal-house-architecture-101808/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

那是 2013 年，一年前[哈佛商业评论](https://hbr.org/2012/10/data-scientist-the-sexiest-job-of-the-21st-century)将数据科学家评为“21 世纪性别歧视的工作”。我曾在一家大型零售公司担任数据科学团队的数据工程顾问。当我接受这份工作时，顾问的生活并不是我想象的那样，我决定是时候继续前进了。与此同时，数据科学团队正在招聘人员来填补一年前离职人员的职位空缺。有一天，我和团队中的一位数据科学家谈论我的求职，谈话内容大致如下:

> **数据科学家:**这个空缺找个好人选太难了。我面试了这么多人，没有一个合适的。
> 
> 我:也许我应该申请数据科学家的职位。(开玩笑)
> 
> **数据科学家:**如果你感兴趣，我会说服我们的分析副总裁雇用你。
> 
> **我:**当真？我没有数据科学经验。你为什么想雇佣我？
> 
> **数据科学家:**因为你知道数据。

“因为我了解数据”我能够利用我作为数据工程师时掌握的这一基本技能，在没有任何经验的情况下成为一名数据科学家。这些是你需要遵循的关键步骤，以成为这一基本技能的大师。

## 1.不要只处理数据。对数据进行质量检查。

当您获得一个原始数据文件时，您的第一反应是查看文件分隔符，找出字段类型，并将其加载到数据库中，而不是实际查看数据吗？然后你会给数据科学家发电子邮件，告诉他们数据加载无误，然后继续下一个任务吗？这是*处理*的数据。

QA(质量保证)包括什么？这是检查数据不规则性的过程，例如主键的重复、意外丢失的值、文件包含历史数据时丢失的日期等等。学习如何对数据进行基本的 QA 检查，将培养您在数据提交给数据科学家之前发现上游数据问题的能力。这证明了[数据科学家花 80%](https://www.forbes.com/sites/gilpress/2016/03/23/data-preparation-most-time-consuming-least-enjoyable-data-science-task-survey-says/?sh=32097ddf6f63) 的时间做数据准备的能力。

如果您不知道要运行什么 QA 检查，请询问数据科学团队他们在查看新数据时通常会检查什么。还要记下数据科学家在处理数据后提到的问题，并在下次加载类似数据时进行检查。您可能无法找到所有问题，但数据科学家会感谢您在将问题交给他们之前花时间检查明显的问题，因为这可以腾出他们的时间来构建模型。

## 2.了解哪些数据是可用的，以及它们之间的关系。

作为一家零售公司的数据工程师，我处理了大量的数据，如客户信息、网站活动和购买情况。网站活动中缺少的一条关键信息是，如果客户在购买前浏览过产品，但没有登录网站，则该客户会有一个链接。

通过利用我对数据及其相互关系的了解，我能够使用不同的标识字段(如电子邮件地址和 [cookie ID](https://www.allaboutcookies.org/cookies/) 与客户 ID)将网站活动联系起来。当数据科学团队构建购买倾向模型时，这些数据被证明是非常宝贵的，因为以前的网页浏览是购买的主要指标。

## 3.了解利益相关者的需求。

当数据科学团队请求为模型开发开发一个[数据集市](https://www.ibm.com/cloud/learn/data-mart#toc-data-mart--VAnhWFYQ)时，我问他们需要如何构建数据。他们告诉我，客户的每个[特性](https://medium.com/@cogitotech/what-are-features-in-machine-learning-and-why-it-is-important-e72f9905b54d)都必须在一行中，这意味着要构建一个包含数百列的非常宽的表。对于数据工程师来说，这是一个难以管理的表结构。

我没有创建一个宽表，而是设计了一个需要有限数量的列的表结构，并开发了数据科学家可以调用的函数来聚合和转置数据以进行建模。通过处理数据并了解如何最好地聚合数据以用于模型开发，我能够构建一个可扩展的表结构，以便数据工程师支持，但允许快速添加新功能。数据科学数据集市的开发后来成为雇用我这个没有任何经验的数据科学家的主要理由。

“了解数据”是一项基本技能，对于任何与数据相关的工作都至关重要，无论是数据工程师、数据科学家还是数据分析师。作为一名数据工程师，我已经展示了数据科学家所需的 80%的数据准备技能。虽然我的经历是一个异数，但我希望我的故事能为你提供灵感，让你知道掌握一项基本技能是开启你进入数据科学职业生涯的关键。

## 你可能也会喜欢…

[](/my-experience-as-a-data-scientist-vs-a-data-analyst-91a41d1b4ab1) [## 我作为数据科学家和数据分析师的经历

### 我希望从一开始就知道的成功秘诀

towardsdatascience.com](/my-experience-as-a-data-scientist-vs-a-data-analyst-91a41d1b4ab1) [](/6-best-practices-i-learned-as-a-data-engineer-9d3ad512f5aa) [## 我作为数据工程师学到的 6 个最佳实践

### 以及作为数据科学家如何应用它们

towardsdatascience.com](/6-best-practices-i-learned-as-a-data-engineer-9d3ad512f5aa) [](/how-to-troubleshoot-etl-issues-like-a-data-engineer-d761f7d361d4) [## 数据科学家如何像数据工程师一样解决 ETL 问题

### 常见原因及其处理方法

towardsdatascience.com](/how-to-troubleshoot-etl-issues-like-a-data-engineer-d761f7d361d4)