# 不要给死人寄钱

> 原文：<https://towardsdatascience.com/dont-send-money-to-dead-people-7653bed80892?source=collection_archive---------55----------------------->

以及数据行业的其他经验。

![](img/bb5dbd12828ee8ad8a7ba19223ab976f.png)

图片由 [Unsplash](http://unsplash.com) 上的 [Precondo CA](https://unsplash.com/photos/GuXhiz45lq8) 提供

【2020 年 6 月，有报道称 [*糟糕的数据*](https://fcw.com/articles/2020/06/25/johnson-gao-cares-waste-report.aspx) *阻碍了美国政府推出其新冠肺炎经济复苏计划的能力。除了其他令人痛心疾首的错误，这次* [***数据宕机***](/the-rise-of-data-downtime-841650cedfd5) *事件发过来***14 亿美元在新冠肺炎刺激支票要死人了。**

*数据宕机——数据不完整、错误、丢失或不准确的时间段——不仅仅是联邦政府的问题。几乎每个组织都在与之斗争。*

*在这个由两部分组成的系列文章的第一部分中，我提出了一个解决数据宕机的方法:数据可靠性，这是一个从 [***站点可靠性工程(SRE)***](https://landing.google.com/sre/) *借用来的概念，已经被业内一些最好的数据团队所采用。**

# *我们如何解决数据宕机问题？*

*前几天，我在一家受欢迎的上市科技公司采访了一位我非常看重的数据副总裁，他告诉我数据宕机对他的公司的影响，从财务报告和监管报告到营销分析，甚至是客户参与度指标。*

*他厌倦了用传统的数据质量方法来解决数据问题。*

> *“数据质量检查只能到此为止，”他说(是的，同意匿名引用)。“我想要一种能让我在任何人(包括我的老板)知道之前就知道数据宕机的东西。说真的，让我这么说吧:我认为这是**‘让我们的首席财务官远离监狱’**的首要任务。*

*他不是一个人。在过去的几年里，我与数百名数据领导者谈论过他们的[数据停机](/the-rise-of-data-downtime-841650cedfd5)问题，从几个空值到完全不准确的数据集。他们的个人问题包罗万象，从浪费时间(一个显而易见的问题)到[浪费金钱](https://hbr.org/2020/02/data-driven-decisions-start-with-these-4-questions?utm_source=linkedin&utm_campaign=hbr&utm_medium=social)，甚至是重大的[合规风险](https://deloitte.wsj.com/riskandcompliance/2018/04/23/making-data-risk-a-top-priority/)。*

*为了解决数据停机问题，我提出了一种方法，它利用了我们的朋友，即“坏软件”的鼓吹者的一些最佳实践: [**站点可靠性工程师**](https://www.infoworld.com/article/3537551/what-is-an-sre-the-vital-role-of-the-site-reliability-engineer.html) **。***

# *SRE 的崛起*

*自 21 世纪初以来，谷歌([该术语的来源](https://landing.google.com/sre/))和其他公司的[站点可靠性工程](https://en.wikipedia.org/wiki/Site_Reliability_Engineering) (SRE)团队不仅在修复中断方面至关重要，而且通过构建可扩展和高度可用的系统从一开始就防止了中断。然而，随着软件系统变得越来越复杂，工程师们开发了新的自动化方法来扩展和操作他们的技术堆栈，以平衡他们对可靠性和创新的双重需求。*

*站点可靠性工程师(sre)通常被描述为勇敢的消防员，他们在晚上的所有时间都被呼叫来解决隐藏的错误、滞后的应用程序和系统中断。除此之外，SRE 团队通过自动化解决方案帮助自动化流程，从而促进无缝软件部署、配置管理、监控和度量，首先[消除辛劳](https://landing.google.com/sre/sre-book/chapters/eliminating-toil/)并最大限度地减少[应用停机时间](https://en.wikipedia.org/wiki/Downtime)。*

> *在软件工程中，每个团队都有类似于 [New Relic](https://newrelic.com/) 、 [DataDog](https://www.datadoghq.com/) 或[page duty](https://www.pagerduty.com/)的 SRE 解决方案来衡量应用程序的健康状况并确保可靠性。数据团队怎么会盲目飞行？*

# *数据团队的可靠性*

*在现场可靠性工程中，[“希望不是策略”](https://landing.google.com/sre/sre-book/chapters/introduction/)这句话很流行。它告诉 SRE 精神，系统不会自己运行，每一个软件的背后都有一个工程师，他可以尽自己最大的能力确保某种程度的可靠性。*

*希望不会使你的公司免于因错误的数字而做出决策。数据可靠性会。*

*正如 SRE 团队最先知道应用程序崩溃或性能问题一样，数据工程和运营团队也应该最先知道坏管道和数据停机问题。仅在六年前，[数据停机每年给公司造成的损失累计达 1.7 万亿美元](https://corporate.delltechnologies.com/en-us/newsroom/announcements/2014/12/20141202-01.htm#:~:text=EMC%20Corporation%20(NYSE%3A%20EMC),nearly%2050%25%20of%20Germany's%20GDP.)；在一个数据无处不在的时代，数据管理工具还没有跟上时代的步伐，这些数字可能会变得更糟。*

*然而，为了使数据完全可用、可信和自助，数据团队必须专注于通过实现完全可靠性来减少[数据停机时间](/the-rise-of-data-downtime-841650cedfd5)。*

*毫无疑问，这种新方法是行业的游戏规则改变者，我很高兴看到公司加入数据可靠性运动。毕竟:当你可以信任你的数据时，谁还需要希望？*

****如果你想了解更多，*** [***伸出手***](http://montecarlodata.com?utm_source=blog&utm_medium=medium&utm_campaign=dead_people) ***给巴尔摩西。****