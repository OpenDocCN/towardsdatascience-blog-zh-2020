# 冠状病毒:来自韩国的大数据教训

> 原文：<https://towardsdatascience.com/coronavirus-a-big-data-lesson-from-south-korea-5bb703b8b0ae?source=collection_archive---------24----------------------->

## 南韩如何用数据和人工智能对抗冠状病毒(新冠肺炎)

![](img/3782afb50e449ff848ccf6bac68a3f96.png)

图片由[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4938929)的 Gerd Altmann 提供

大多数国家仍然受到严格的检疫限制，而其他国家正在开放经济，回归正常生活。冠状病毒的影响因国家而异。一些国家正面临严峻的挑战，努力控制疫情。尽管如此，像韩国、新加坡和台湾这样的国家几乎能够立即对形势做出反应，并更快地恢复过来。

早在 2020 年 2 月，韩国报告了世界上最高的冠状病毒日传播率之一。除了 Mainland China，受感染的人数是最高的，天气预报预测了一切，但都是最坏的情况。

![](img/f6c6b875eb55af391cd62ae42ee14aa1.png)

自己可视化，数据来源:[约翰·霍普斯金大学](https://coronavirus.jhu.edu/map.html)

但正如图表所示，曲线开始变平的速度比最初预期的要快得多，这种情况得以维持。韩国怎么可能如此迅速有效地对此案做出反应？

## 吸取的教训

韩国已经面临类似的情况，但规模要小得多。2015 年，一名从中东地区返回首尔的韩国游客携带了一种被称为 MERS-CoV 的传染病，也是由冠状病毒引起的。由于病毒在该国的新颖性，直到疾病被正确诊断需要几天时间。在病毒被确认的时候，它已经被进一步传播，并且似乎几乎不可能追踪谁被感染。这种感染导致韩国出现 186 例 MERS-CoV 病例，并导致 16993 名韩国人被隔离 14 天，经济损失达 85 亿美元。除了沙特阿拉伯，韩国是全球感染人数最多的国家:

![](img/7b49a1e30dc3a2167bf3095fdab97c35.png)

自己可视化，数据来源:[世界卫生组织](https://www.who.int/emergencies/mers-cov/mers-summary-2016.pdf)

尽管这种病毒出现得相当出人意料，但它的进一步发展被相对较快地阻止了。人们的快速测试和各种先进技术、数据分析和人口信息的使用已被证明是切实可行的措施。

## 31 号病人与韩国冠状病毒的传播

韩国首例病毒病例于 1 月 20 日确诊。一名来自中国武汉的受感染女士被隔离，官员们设法控制住了局势。“31 号病人”改变了一切。在测试呈阳性之前，患者 31 发生了一次小事故，在医院检查了两次，并在教堂参加了一次服务。她与数百人有过接触，并成功地将病毒传染给了其中许多人。自那以后，南韩在几天的时间里确认了数千例病毒病例，并且数字急剧增长，这次爆发是 Mainland China 以外最大的一次。

# 借助大数据、人工智能和智能技术，在 20 天内拉平曲线

韩国在防止病毒进一步传播方面做得很好，原因有几个。从 MERC 的经验中，他们能够应用有效的措施，例如:

*   智能联系人跟踪
*   积极的测试和诊断
*   远距离医学
*   ICT 和智能技术

## 智能联系人跟踪

合同追踪是一个很好的例子，说明了如何让大数据大规模发挥作用，并带来可操作的见解。

![](img/5b59bbf60e49c99505357f6de0a59162.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=152575) (CC0)

当我们回到关于我们 31 号病人的故事时，官员们能够跟踪她的活动，她接触过的人，并警告每个可能处于危险中的人。

联系人追踪应用程序基于不同种类的数据:

*   地理空间的
*   时间序列
*   图像和视频
*   处理
*   其他人

关于位置的信息可能通过几个来源被跟踪:来自智能设备的 GPS 数据、信用卡交易或其他。一旦合并，一个确诊患者的运动的全面图片被公开共享。该应用程序提供了个人访问的历史、时间线和地点的信息。餐馆和酒店的访问，他们使用的交通工具，他们乘坐的线路或公共汽车号码，甚至电影院的确切座位都被跟踪。在监测和分析病毒传播时，闭路电视摄像机也被广泛使用。韩国每天有超过 110 万个可运行的闭路电视摄像头。根据 2010 年的统计数据，每个市民平均每天被公共闭路电视系统捕捉 83.1 次，并且这一趋势逐年增长。

智能联系人追踪可缩短有关部门的反应时间，以识别潜在威胁、隔离病毒的传播源并通知所有可能处于危险中的人员。这个公共应用程序还会告诉每位用户，确诊患者去过哪些地区，哪些地方被隔离，以及谁可能受到影响。该系统还能追踪病毒的活动，帮助医院和诊所更好地准备应对下一波病毒。

## 基于人工智能的测试套件开发和诊断

MERC 的经历给韩国上了另一课——“激进的测试”。在超级发射机事件大规模传播之前，只有 30 例确诊病例，而中国为 75，000 例。尽管数量少，韩国开始与生物科技公司合作开发冠状病毒的测试。他们几乎立即开发出了测试套件，并分发给全国每一家医院。韩国并不像其他国家那样面临测试数量不足的问题。多亏了谨慎的人工智能规划和利用，他们才得以以超快的速度来拉平曲线。

> “如果没有人工智能，在如此短的时间内开发一个测试是不可能的，”成均馆大学交互科学系教授钟泰明(Tai-myong Chung)说。

测试套件的设计是使用人工智能开发的。人工智能帮助缩短了设计和构建测试包所需的时间。病毒的基因组成通常需要 2-3 个月。这家名为 Seegene 的公司凭借自动化和人工智能，在不到三周的时间里就完成了测试开发。基于人工智能的大数据解决方案使科学家能够快速了解冠状病毒的遗传组成。

![](img/d7f933040d1cf50204d8e68945c696b2.png)

[疾病预防控制中心](https://unsplash.com/@cdc?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

此外，人工智能允许个人健康专家准确快速地筛选和诊断患者。人工智能的主要应用是对大规模的胸部 x 光数据进行处理，以便在几秒钟内识别冠状病毒的症状，并在危急状态下给予患者治疗。人工智能用于将患者分为四类:轻度、中度、重度和非常重度

## 远距离医学

除了基于人工智能的解决方案，还有远程医疗，支持社交距离，并使患者能够与医疗保健人员保持联系。远程医疗以潜在或确诊的患者为目标，以监控冠状病毒的症状和进一步发展。

![](img/0b282be0ff04b14672505775bfdee474.png)

[国家癌症研究所](https://unsplash.com/@nci?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

整个检查是通过使用智能手机的视频通话建立的。所有症状都包含在数据库中，并建议患者采取下一步措施。该解决方案使专业人员能够实时检查确诊患者的症状，并将需要医疗援助的患者转移到最近的医院。

## 信息共享应用

整个韩国系统对抗冠状病毒的其他关键部分是移动应用程序。有很多是在几天内开发出来的。主要目的是从不同角度让公众了解冠状病毒。他们中的一些人正在提供关于症状的一般信息。其他人会让你知道有口罩的地方或者最近的检测地点。也有人工智能应用程序使用聊天机器人给需要医疗护理的人打电话。

韩国是人工智能和大数据如何应用于社会公益的一个很好的例子——缩短反应时间，提高准确性，并在全国部署解决方案。许多事情需要解决。隐私、数据安全和信息共享不能放在一边。得益于该国透明一致的政府、文化、集体努力、技术成熟和渴望创新，韩国在面对这些挑战时成功克服了潜在风险。

在 LinkedIn 上关注我:

[](https://www.linkedin.com/in/filipdzuroska/) [## Filip Dzuroska -数据科学家- Erste Group IT | LinkedIn

### 在世界上最大的职业社区 LinkedIn 上查看 Filip Dzuroska 的个人资料。菲利普有一份工作列在他们的…

www.linkedin.com](https://www.linkedin.com/in/filipdzuroska/) 

文章来源:

[](https://www.thedailystar.net/online/news/south-korea-winning-the-fight-against-coronavirus-using-big-data-and-ai-1880737) [## 韩国利用大数据和人工智能赢得了与冠状病毒的斗争

### 南韩依靠其技术优势对抗新型冠状病毒(新冠肺炎)。这个国家有一个…

www.thedailystar.net](https://www.thedailystar.net/online/news/south-korea-winning-the-fight-against-coronavirus-using-big-data-and-ai-1880737) [](https://economictimes.indiatimes.com/tech/software/how-countries-are-using-technology-to-fight-coronavirus/articleshow/74867177.cms?from=mdr) [## 各国如何利用科技对抗冠状病毒

### 新冠肺炎的快速传播迫使各国使出浑身解数来遏制疾病。一些…

economictimes.indiatimes.com](https://economictimes.indiatimes.com/tech/software/how-countries-are-using-technology-to-fight-coronavirus/articleshow/74867177.cms?from=mdr) [](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5840604/) [## 中东呼吸综合征:我们从 2015 年大韩民国疫情中吸取的教训

### 中东呼吸综合征冠状病毒(MERS-CoV)首次从一名严重肺炎患者身上分离出来

www.ncbi.nlm.nih.gov](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5840604/) [](http://kosis.kr/statHtml/statHtml.do?orgId=127&tblId=DT_2014_82) [## 科西斯

### 编辑描述

kosis.kr](http://kosis.kr/statHtml/statHtml.do?orgId=127&tblId=DT_2014_82) [](https://www.humanrights.go.kr/site/program/board/basicboard/view?&boardtypeid=24&menuid=001004002001&boardid=600084) [## 보도자료 | 국가인권위원회

### 编辑描述

www.humanrights.go.kr](https://www.humanrights.go.kr/site/program/board/basicboard/view?&boardtypeid=24&menuid=001004002001&boardid=600084) [](https://theconversation.com/coronavirus-south-koreas-success-in-controlling-disease-is-due-to-its-acceptance-of-surveillance-134068) [## 冠状病毒:韩国成功控制疾病是因为它接受了监测

### 南韩因其对冠状病毒疾病新冠肺炎的爆发和传播的管理而受到广泛赞扬…

theconversation.com](https://theconversation.com/coronavirus-south-koreas-success-in-controlling-disease-is-due-to-its-acceptance-of-surveillance-134068) [](https://www.coe.int/en/web/artificial-intelligence/ai-and-control-of-covid-19-coronavirus) [## 禽流感与新冠肺炎冠状病毒的控制

### 本文档也可从以下网站获得:本出版物旨在提供来自……的文章的非详尽概述

www.coe.int](https://www.coe.int/en/web/artificial-intelligence/ai-and-control-of-covid-19-coronavirus) [](https://news.itu.int/covid-19-how-korea-is-using-innovative-technology-and-ai-to-flatten-the-curve/) [## 新冠肺炎:韩国如何利用创新技术和人工智能来拉平曲线

### 据国际电联消息，大韩民国迄今为止在不关闭其经济的情况下成功地遏制了新冠肺炎…

news.itu.int](https://news.itu.int/covid-19-how-korea-is-using-innovative-technology-and-ai-to-flatten-the-curve/) [](https://lunit.prezly.com/lunit-releases-its-ai-online-to-support-healthcare-professionals-manage-covid-19) [## Lunit 在线发布人工智能，支持医疗保健专业人员管理新冠肺炎

### Lunit 是一家医疗人工智能软件公司，通过胸部 x 光图像开发人工智能肺部疾病分析，今天…

lunit.prezly.com](https://lunit.prezly.com/lunit-releases-its-ai-online-to-support-healthcare-professionals-manage-covid-19)