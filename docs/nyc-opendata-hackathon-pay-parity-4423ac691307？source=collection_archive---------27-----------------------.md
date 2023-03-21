# 通过纽约市开放数据门户支付平价

> 原文：<https://towardsdatascience.com/nyc-opendata-hackathon-pay-parity-4423ac691307?source=collection_archive---------27----------------------->

这个周末，我有幸参加了“[像姐姐一样爱你”](https://loveyalikeasis.com/)黑客马拉松，这是[纽约开放数据周](https://opendata.cityofnewyork.us/)的一部分。与 [DataKind 的](https://www.datakind.org/) DataDeepDive 周末活动类似，我们得到了一个由不同专家组成的团队，一个特定的问题，以及大约 8 个小时来解决它。

虽然我们的 Github repo 和更具体的工作流将很快上传，但这篇文章将是对可用数据资源的简短探索，以解决我的团队面临的问题。

**挑战:**

> 纽约市性别平等委员会(CGE 纽约市)制定了消除性别薪酬差距的政策，包括企业在“平衡薪酬领域”的最佳做法为企业开发一个解决方案来理解和实现这些实践。

随着我们深入杂草，并咨询我们的主题专家，很明显纽约市 CGE 区面临的障碍与最新信息流*有关，就像与雇主合规信息流有关一样。企业——特别是需要适应的基础架构较少的小型企业——没有提供关于其策略合规性的反馈，也没有在策略强化方面加倍努力。因此，我开始梳理纽约市的 OpenData 门户网站，试图找到可以作为政策合规性代理的数据集，为向雇主通报政策提供基础，或者为我们的挑战提供一些其他见解。*

下面我提供了 4 个相关数据集的链接，以及它们的:

*   描述(来自 OpenData 门户)
*   可能的用途
*   相关栏目
*   主键(在可能与其他数据集合并时使用)

# 相关数据集:

## [合法经营的企业](https://data.cityofnewyork.us/Business/Legally-Operating-Businesses/w7w3-xahh)

**描述**:该数据集以持有 DCA(消费者事务部)执照的企业/个人为特征，因此他们可以在纽约市合法经营。

![](img/1aeb4b895395413e4fcadeeae4a675de.png)

照片由[迈克·彼得鲁奇](https://unsplash.com/@mikepetrucci?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**用途**:这个数据集可以为 CGE 想要联系的企业生成一个初始模式。通过提供关于预先存在的许可的基本信息，它可以用于减少雇主初始登录的负荷，提供更安全的登录协议以确保雇主连接到其验证的企业，或者为特定行政区的企业预先填充其他位置相关的政策信息。

**相关栏目** : DCA 牌照号、牌照类型、牌照类别、企业名称、地址区

**主键** : DCA 牌照号

## [WBE、LBE 和 EBE 认证企业列表](https://data.cityofnewyork.us/Business/M-WBE-LBE-and-EBE-Certified-Business-List/ci93-uc8s)

![](img/df56dad0d3ade78222259db0415358a3.png)

照片由[阿里·叶海亚](https://unsplash.com/@ayahya09?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**描述**:该市的认证计划，包括少数族裔和妇女所有的商业企业(M/WBE)计划、新兴商业企业(EBE)计划和本地商业企业(LBE)计划，认证、促进和扶持该市少数族裔和妇女所有的企业以及符合条件的小型建筑和建筑相关企业的发展。该列表包含认证公司的详细信息，包括其工作历史的简要描述、联系信息以及公司销售的详细信息。

**用途**:该数据集将帮助 CGE 瞄准并为拥有这些认证类型的企业提供额外的支持/信息，或者为这些公司创建一个比上述 LOB 数据库中提供的更具体的方案。

**相关栏目**:成立日期、供应商正式名称、建设类型、项目、货物、材料、供应商

**主键**:供应商 _ 正式 _ 名称

## [市议会立法项目](https://data.cityofnewyork.us/City-Government/City-Council-Legislative-Items/6ctv-n46c/data)

![](img/2d17817e986d6d0f78c8eef70b891ddf.png)

照片由[迈克尔](https://unsplash.com/@michael75?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**说明**:此清单载有由立法会提出及制定的立法项目的资料。

**用途**:这些项目可以通知相关立法项目的运行列表，以填充站点内的“资源”选项卡，以及连接到关于相关法律对企业的期望的外部文献。“标题”栏将需要清理和关键字的功能工程，以便有更多的功能，CGE 可能有一个更新的相关立法项目的数据库。

**相关栏目**:最终行动(日期/时间)、标题(包含法律本身的信息)

**主键** : File_#

## 【2018 财年收到的问询

![](img/5c072f4a402bd3acd68d7fb768425169.png)

[ev](https://unsplash.com/@ev?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**说明**:该数据集包含上一财政年度公众成员向委员会提出的关于歧视事件和涉嫌违反《城市人权法》的[总]次数。

**用途**:虽然这个数据集有点稀疏，但它可以提供一个基础，为指称的基于性别的歧视侵权行为的频率建模，帮助收集这些侵权行为的模式，以防止它们在未来几年发生。

**相关栏目**:类别、就业、公共住宿、总计

**主键**:类别

# 未来数据集

虽然我们仍在使这个项目的 MVP 成为现实，但在未来，包含更广泛的信息可能是有用的。更全面的解决方案包括为雇主、雇员以及负责执法的政府机构提供一个平台。这些利益相关者的一些潜在数据源包括:

## 对于机构:

商业位置数据:这有助于为信息组织或执法机构创建一个地理信息系统地图。

商业解决方案:这种位置信息有助于确定市场拓展的目标，也有助于鼓励特定行政区更积极的雇主提供支持。

[许可证申请](https://data.cityofnewyork.us/Business/License-Applications/ptev-4hud):这将有助于从外部监控许可证申请流程。随着城市了解许可证申请的发展趋势，他们可以调整他们的执法或教育工作。

## 对于员工:

[纽约市妇女资源网络数据库](https://data.cityofnewyork.us/Social-Services/NYC-Women-s-Resource-Network-Database/pqg4-dm6b):这将是一个很好的资源，可以将员工与该市的组织联系起来，帮助他们了解并倡导自己的权利。

[2014 年纽约市工作和家庭假期调查(WFLS)](https://data.cityofnewyork.us/Health/New-York-City-Work-and-Family-Leave-Survey-WFLS-20/grnn-mvqe):通过监测在职父母带薪家庭假期的可用性和可获得性的变化，员工可以了解劳动力中性别平等的关键问题，以及它如何随着新政策而变化。

***Rebecca Rosen****是 Flatiron 的数据科学 Immersive 的应届毕业生，具有认知科学和非营利管理的背景。她目前居住在纽约，在业余时间制作音乐。想看更多她的作品，或者打个招呼，搜索 Medium，Instagram 或者标签为*[*@ rebeccahhrosen*](http://twitter.com/rebeccahhrosen)*的脸书。*