# 什么是数据可靠性？

> 原文：<https://towardsdatascience.com/what-is-data-reliability-66ec88578950?source=collection_archive---------26----------------------->

## 以及如何使用它来开始信任您的数据。

![](img/0da155e291cf07573c61bec70cee1699.png)

图片由 [Unsplash](http://unsplash.com) 上的 [Nico E.](https://unsplash.com/photos/AAbjUJsgjvE) 提供。

*作为数据专业人员，我们可以从软件工程中学到很多关于构建健壮的、高度可用的系统的知识。* [*在之前的文章*](/dont-send-money-to-dead-people-7653bed80892?source=friends_link&sk=68581aea28e947ab718a2d182dfd4bdb) *中，我讨论了为什么数据可靠性对于数据团队来说是必须的，在这里，我分享了我们如何将这个概念应用到数据管道中。*

本世纪初，谷歌 Site 本杰明·特雷诺·斯洛斯创造了 [**站点可靠性工程**](https://landing.google.com/sre/#sre) ，这是 [DevOps](https://en.wikipedia.org/wiki/DevOps) 的一个子集，指的是*“当你要求一个软件工程师设计一个操作功能时会发生什么。”*换句话说，站点可靠性工程师(简称 SREs)构建自动化软件来优化应用程序的正常运行时间，同时[最大限度地减少辛劳](https://landing.google.com/sre/sre-book/chapters/eliminating-toil/)并减少[停机时间](https://landing.google.com/sre/sre-book/chapters/availability-table/)。除了这些职责之外，sre 还被称为工程界的“消防员”，致力于解决隐藏的错误、落后的应用程序和系统中断。

现在，随着数据系统在组织中达到相似的复杂程度和更高的重要性水平，我们可以将这些相同的概念应用到我们的领域，如**数据可靠性——组织在整个数据生命周期中提供高数据可用性和健康的能力。**

# 从应用停机到数据停机

虽然救火无疑是一项核心责任，但 sre 也有责任通过了解新功能和其他创新的机会成本来寻找新的方法[以周到地管理风险](https://landing.google.com/sre/sre-book/chapters/embracing-risk/)。为了推动这种数据驱动的决策制定，建立明确的[服务水平目标(SLO)](https://landing.google.com/sre/workbook/chapters/implementing-slos/#:~:text=Service%20level%20objectives%20(SLOs)%20specify,the%20core%20of%20SRE%20practices.)来定义这种可靠性在现实世界中是什么样子，由[服务水平指标(SLIs)](https://landing.google.com/sre/sre-book/chapters/service-level-objectives/#:~:text=An%20SLI%20is%20a%20service,of%20service%20that%20is%20provided.&text=Other%20common%20SLIs%20include%20the,measured%20in%20requests%20per%20second.) 来衡量。

一旦 SLO 和 SLIs(比如说快 10 倍…)建立起来，SREs 就可以很容易地确定可靠性和风险之间的平衡。即使有最智能的解决方案和最有经验的 sre，实现 100%的系统正常运行时间也不是不可能的。创新依赖于迭代，消除停机的唯一方法是保持静态，但这不会给你带来竞争优势。**正如我的一位 SRE 朋友恰当地指出的那样:*“如果*网站关闭，这不是一个*的问题，而是一个*何时*的问题。”***

就像 SREs 在可靠性和创新性之间取得平衡一样，我们也必须确保我们的数据管道足够可靠和灵活，以允许引入新的数据源、业务逻辑、转换和其他对我们的公司和客户都有益的变量。

> 与我们精心管理**应用程序停机时间**一样，我们必须专注于减少[数据停机时间](/the-rise-of-data-downtime-841650cedfd5) —数据不准确、丢失或出现其他错误的时间段。

对于各种各样的公司来说，已经出现了许多重大的应用程序宕机事件，如 [GitHub](https://www.theverge.com/2020/6/29/21306674/github-down-errors-outage-june-2020) 、 [IBM](https://www.zdnet.com/article/ibm-cloud-outage-downs-customer-websites-globally/) 、 [DoorDash](https://www.thedenverchannel.com/news/national/door-dash-plagued-by-outages-friday-night) 和[Slack](https://www.theverge.com/2020/5/12/21256717/slack-is-down-disruption-outage)——数据宕机也是一个类似的严重威胁。

![](img/a1f4f0e44c357163466e5525fd2c865a.png)

*救火不仅仅是为了 SREs。作为数据专业人员，我们也要处理我们应得的数据停机火灾，但我们不必这样做。图片由* [*周杰伦海克*](https://unsplash.com/@jayrheike) *上*[*un splash*](https://unsplash.com/photos/6AwjjrzIgRs)*。*

坏数据不仅会导致糟糕的决策，而且监控和解决数据可靠性问题会耗费团队宝贵的时间和金钱。如果你从事数据方面的工作，你可能知道在应对数据宕机上花了多少时间。事实上，许多数据领导者告诉我们，他们的数据科学家和数据工程师花了 30%或更多的时间来解决数据问题——精力最好用在创新上。

# 比别人先知道

在过去的几年里，我和 150 多位数据主管谈过他们的数据停机时间，从几个空值到完全不准确的数据集。他们的个人问题包罗万象，但有一点是明确的:有更多的利害关系比一些丢失的数据点。

一家受欢迎的高端服装租赁公司的工程副总裁告诉我，在他的团队开始监控数据宕机之前，他们的整个客户信息数据库都是 8 小时停机，显示出巨大的技术债务。更糟糕的是，他们几个月都没有发现这个问题，只是在数据仓库迁移过程中发现了它。虽然它最终是一个相对简单的修复(和一个令人尴尬的发现)，但如果能尽快知道并解决它就好了。

数据停机对他们的业务造成了损失。依赖及时数据为客户做出明智决策的分析师对他们的渠道缺乏信心。随之而来的是收入损失。这类事件经常发生，没有一家公司能够幸免。

就像 SRE 团队最先知道应用程序崩溃或性能问题一样，数据团队也应该最先知道糟糕的管道和数据质量问题。仅在六年前， [**数据丢失和停机每年给公司造成累计 1.7 万亿美元的损失**](https://corporate.delltechnologies.com/en-us/newsroom/announcements/2014/12/20141202-01.htm#:~:text=EMC%20Corporation%20(NYSE%3A%20EMC),nearly%2050%25%20of%20Germany's%20GDP.)；在一个数据无处不在的时代，而[数据管理工具不一定能跟上](https://www.cnbc.com/2020/07/06/ppp-small-business-coroniavirus-loan-database-contains-errors.html)，这些数字可能会变得更糟。

为了避免数据停机，在数据的整个生命周期(从来源到消费的整个过程)中保持对数据的 [**完全可观察性**](/what-is-data-observability-40b337971e3e) 非常重要。强大的渠道带来准确而及时的洞察，从而做出更好的决策，[真正的治理](/what-we-got-wrong-about-data-governance-365555993048)，让客户更加满意。

# 我如何使我的数据可靠？

我提出了数据团队在其组织中实现高数据可靠性的两种主要方法:1)设置数据 SLO，2)投资于减少数据停机时间的自动化解决方案。

## 为数据设置 SLO 和 sli

为系统可靠性设置 SLO 和 SLIs 是任何 SRE 团队的预期和必要功能，在我看来，也是我们将它们应用于数据的时候了。一些公司也已经在这么做了。

在数据环境中，SLO 指的是数据团队希望在一组给定的 sli 上实现的目标值范围。您的 SLO 看起来会有所不同，这取决于您的组织和客户的需求。例如，B2B 云存储公司每 100 小时的正常运行时间可能有 1 小时或更少的停机时间，而拼车服务的目标是尽可能延长正常运行时间。

> 以下是如何考虑定义您的数据 sli。在之前的帖子中，我已经讨论过[数据可观察性的五大支柱](/what-is-data-observability-40b337971e3e?source=friends_link&sk=db5d16e059c4c1c84f9d436f1e686c5b)。重新构建后，这些支柱就是你的五个关键数据 sli:新鲜度、分布、容量、模式和血统。

*   **Freshness** : Freshness 试图了解你的数据表有多新，以及你的表更新的频率。
*   **分布**:分布，换句话说，是数据可能值的函数，告诉你数据是否在可接受的范围内。
*   **Volume:** Volume 指的是您的数据表的完整性，并提供关于您的数据源的健康状况的见解。
*   **模式:**模式变化通常表示数据损坏。
*   **世系**:数据世系通过告诉你哪些上游来源和下游祖先受到了影响，以及哪些团队正在生成数据和谁正在访问数据来提供答案。

我工作过的许多数据团队都对集成最新、最棒的数据基础设施和商业智能工具的前景感到兴奋，但是，正如我以前写的那样，这些解决方案的好坏取决于为它们提供动力的数据。这些 sli 将使你更好地了解这些数据实际上有多好，以及你是否可以信任它。

## 投资于数据可靠性

事实是，以这样或那样的方式，您已经在投资数据可靠性。无论是您的团队正在进行的验证数据的手动工作，还是您的工程师正在编写的自定义验证规则，或者仅仅是基于破碎数据或未被注意的无声错误所做决策的成本。这是巨大的代价。

但是有一个更好的方法。正如站点可靠性工程师使用自动化来确保应用正常运行时间并提高效率一样，数据团队也应该依靠支持机器学习的平台来使数据可靠性更容易、更易获得，从而做出更好的决策、获得更大的信任和更好的结果。

与任何优秀的 SRE 解决方案一样，最强大的数据可靠性平台将为您的管道提供自动化、可扩展、ML 驱动的[可观察性](/what-is-data-observability-40b337971e3e),使其易于仪表化、监控、警报、故障排除、解决和协作处理数据问题，最终降低您的数据停机率，从而提高您整个数据管道的可靠性。

现在，有了清晰的 sli、SLO 和新的数据可靠性方法，我们终于可以把救火工作留给专业人员了。

***如果你想了解更多，请联系*** [***巴尔摩西***](http://montecarlodata.com?utm_source=blog&utm_medium=medium&utm_campaign=data_reliability) ***。***