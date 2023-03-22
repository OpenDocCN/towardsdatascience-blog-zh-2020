# Excel 的限制导致了 16，000 个阳性 COVID 病例的丢失

> 原文：<https://towardsdatascience.com/excels-limitation-caused-loss-of-16-000-positive-covid-cases-a2be4b654c8f?source=collection_archive---------25----------------------->

## 追踪人员漏掉了 50，000 名潜在的感染者，却没有告诉他们进行自我隔离

![](img/af225da25ed917220ae5b6549664f418.png)

米卡·鲍梅斯特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 缺失数据

如果你打开一个 Excel 文件，滚动到底部(提示: *CTRL +南箭头*)，你会发现它的结尾是 1，048，576。一行也不能多。

据《卫报》报道，英国公共卫生部将 15841 例阳性 COVID 测试结果的丢失归咎于这一限制。反过来，根据每个非复杂病例有 3 名密切接触者的估计，这导致至少 47，000 名潜在感染者未被告知并被要求自我隔离，并可能被追踪。这一数字可能超过 50，000，因为据报道，15，841 例失踪阳性病例中有少数是复杂的，*即*来自医院、监狱、无家可归者收容所等环境，这些环境平均有 7 名密切接触者。

# 它是如何发生的

英国公共卫生(PHE)组织收集公共和私人实验室的结果。正如《卫报》所报道的，一个实验室已经把它的日常测试报告作为 CSV 文件发送给了 PHE。然后，PHE 将实验室的结果与从其他实验室获得的结果合并到 Excel 中。然后，本组织将报告官方数字，并对追踪和通报个人阳性病例采取后续行动。

然而，似乎手边的实验室已经在 PHE 数据表的 100 多万行中了。添加另一个每日批次导致一些结果被截断，因此没有报告和采取行动。

# 如何预防

Excel 是一个非常受欢迎的工具。它可以用于多种任务，而这些任务与它的初衷相去甚远:一个电子表格。它的易用性也意味着它是可访问的，因此许多用户远不是*超级用户，*不知道也不遵循好的实践。我会想到一些建议:

*   实施检查和平衡，例如在导入之前和之后验证记录的数量，将很容易消除这种事件，
*   关于 Excel 局限性的提醒或信息会议，针对在关键部门工作的用户，
*   或者，使用另一个工具，一个可以绕过一百万行限制的工具。使用 SQL Server 可以快速处理这项工作。但是，在时间和资源短缺的疫情，向报告和跟踪案件的团队引入新软件的前景是不可取的。这种介绍也可能通过电话会议而不是面对面进行，这是另一个缺点。

# 以前发生过

2012 年，一个 Excel 错误导致摩根大通亏损 60 亿美元。他们在报告中描述了这个错误:

> 在审查过程中，更多的业务问题变得很明显。例如，该模型通过一系列 Excel 电子表格运行，这些表格必须通过从一个电子表格向另一个电子表格复制和粘贴数据的过程手动完成。

此外，据 Business Insider 当时报道:

> 有证据表明，由于这种压力，模型审查小组加快了审查，这样做可能更愿意忽略批准过程中明显的操作缺陷。

其他昂贵的活动 were⁴:

*   TransAlta 因复制粘贴错误损失了 2400 万美元，
*   柯达因遣散费打印错误损失 1100 万美元，
*   伦敦奥运会花样游泳比赛超售了 2 万张门票，比赛还有 1 万个剩余座位，

在每种情况下，手动操作都是错误的原因。

![](img/c88539d80d0c1cd972a41d8c05af6ed9.png)

迈克尔·斯科特在办公室——Gif 发自[男高音](https://tenor.com/search/michael-scott-no-gifs)

# 我爱你，但你有缺点

Excel 是一个伟大的工具，但也有难以忽视的缺陷。首先想到的自然是它的 100 万行的限制，尽管它的主要目的不是作为一个接近大数据的工具。

*   它也缺乏一个可审计的概述。任何人都可以有意或无意地调整未锁定的文件。即使上锁，也缺乏控制和安全性，容易受到欺诈或腐败。
*   很难排除故障或进行测试，
*   难以整合并且不是为协作工作而设计的，尽管近年来在这方面有所改进，
*   缺乏权威来源。许多员工可能正在使用同一文件的不同快照，但是具有不同的值，

这只是列举了几个例子。

# 后果

Excel 首先承担了责任，PHE 组织引用了一个 IT 问题。软件本身对它的能力是直截了当的，无论谁想知道，列和行的数目都会显示出来。后来发现人为错误也是原因之一。PHE 组织将于明年 3 月解散，这一宣布发生在 cases⁵.失守之前

如果这个故事提醒了我们一个重要的因素，那就是需要为手头的工作使用正确的工具，保持对细节的高度关注，以及经理或领导者确保关键用户得到适当的装备来完成他们的工作。

希望每一个相关的案件都会有好的结果。

## 快乐的数据争论！

感谢阅读！喜欢这个故事吗？ [**加入媒介**](https://medium.com/@maximegodfroid/membership) 获取完整的我的所有故事。

# 参考

[1] Alex Hern (2020)，"*Excel 如何可能导致英格兰 16000 次 COVID 测试的丢失*"，[https://www . the guardian . com/politics/2020/oct/05/How-Excel-may-have-induced-loss-of-16000-COVID-tests-in-England](https://www.theguardian.com/politics/2020/oct/05/how-excel-may-have-caused-loss-of-16000-covid-tests-in-england)

[2]乔希·哈利迪，丹尼斯·坎贝尔，彼得·沃克和伊恩·萨姆特(2020)，“*英格兰 COVID 病例错误意味着 5 万个接触者可能没有被追踪到*”，[https://www . the guardian . com/world/2020/oct/05/England-COVID-cases-error-unknown-how-many-contacts-not-tracted-says-minister](https://www.theguardian.com/world/2020/oct/05/england-covid-cases-error-unknown-how-many-contacts-not-traced-says-minister)

[3] Linette Lopez (2013)，“*伦敦鲸的崩溃如何部分是使用 Excel* 的错误的结果”，[https://www . business insider . com/Excel-部分归咎于交易损失-2013-2？IR=T](https://www.businessinsider.com/excel-partly-to-blame-for-trading-loss-2013-2?IR=T)

[4] Teampay (2018)，"*有史以来最大的 7 个 Excel 错误*"[https://www . Teampay . co/insights/maximum-Excel-errors-of-All-Time/](https://www.teampay.co/insights/biggest-excel-mistakes-of-all-time/)

[5]丹尼斯·坎贝尔(2020)，“废除公共卫生英格兰只是‘转嫁冠状病毒错误的责任’”，[https://www . the guardian . com/world/2020/aug/19/废除公共卫生英格兰只是转嫁冠状病毒错误的责任](https://www.theguardian.com/world/2020/aug/19/abolition-of-public-health-england-just-passing-of-blame-for-coronavirus-mistakes)