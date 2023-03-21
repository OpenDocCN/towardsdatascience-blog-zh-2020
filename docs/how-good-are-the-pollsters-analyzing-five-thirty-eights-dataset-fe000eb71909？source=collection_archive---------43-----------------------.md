# 民意测验专家有多好？分析 538 的数据集

> 原文：<https://towardsdatascience.com/how-good-are-the-pollsters-analyzing-five-thirty-eights-dataset-fe000eb71909?source=collection_archive---------43----------------------->

## 我们分析了来自历史悠久的政治预测网站 538 的民调数据。

![](img/9c48c0739cb9bcac94a7ee25699f17bb.png)

图片来源:作者拼贴创作(免费图片)

# 介绍

这是一个选举年，围绕选举(总统大选和众议院/参议院)的投票正在升温。这将在未来几天变得越来越令人兴奋，推文，反推文，社交媒体斗争，以及电视上无休止的专家评论。

我们知道，不是所有的民意测验都是同样的质量。那么，如何理解这一切呢？如何使用数据和分析来识别值得信赖的民意调查者？

![](img/8c234eb03893cc3d08f750b587cb29ff.png)

图片来源: [Pixabay](https://pixabay.com/photos/confused-hands-up-unsure-perplexed-2681507/) (免费用于商业用途)

在政治领域(以及其他一些领域，如体育、社会现象、经济等)。)预测分析， [**五三八**](https://fivethirtyeight.com/) 是一个令人生畏的名字。

自 2008 年初以来，该网站已经发表了关于当前政治和政治新闻的各种主题的文章，通常是创建或分析统计信息。该网站由 rockstar 数据科学家和统计学家 Nate Silver 运营，在 2012 年总统大选期间获得了特别的关注和广泛的声誉，当时其模型正确预测了所有 50 个州和哥伦比亚特区的获胜者。

![](img/d11aef88855a745c571e5d4f5c677cb2.png)

图片来源:[维基百科](https://en.wikipedia.org/wiki/2012_United_States_presidential_election)(创意常见)

而在你嗤之以鼻说“ ***但是 2016 年大选呢？*** ”，你或许应该读一读这篇关于唐纳德·川普(Donald Trump)的当选如何在统计建模的正常误差范围内的文章。

[](https://fivethirtyeight.com/features/trump-is-just-a-normal-polling-error-behind-clinton/) [## 特朗普只是落后于克林顿的一个正常的民调误差

### 即使在总统竞选结束时，民意调查也不能完美地预测选举的最终差距。有时候…

fivethirtyeight.com](https://fivethirtyeight.com/features/trump-is-just-a-normal-polling-error-behind-clinton/) 

对于对政治更感兴趣的读者来说，这里有一整袋关于 2016 年大选的文章。

数据科学从业者应该会喜欢上 Five-38，因为它不回避用高度技术性的术语解释他们的预测模型(至少对于外行来说足够复杂)。

![](img/be17d445b3b14632fc95e49da8409355.png)

图片来源:[本文](https://fivethirtyeight.com/features/election-update-why-our-model-is-more-bullish-than-others-on-trump/)

在这里，他们正在谈论采用著名的 **t 分布**，而大多数其他民调聚合者可能只是对无处不在的正态分布感到满意。

[](https://rpsychologist.com/d3/tdist/) [## 了解学生的 t 分布

### 大多数学生被告知，随着样本量的增加，t 分布接近正态分布。

rpsychologist.com](https://rpsychologist.com/d3/tdist/) 

然而，除了使用复杂的统计建模技术，Silver 领导下的团队还以一种独特的方法为傲，即**民意测验专家评级**，以帮助他们的模型保持高度准确和可信。

在这篇文章中，我们分析了这些评级方法的数据。

> five-38 并不回避用高度技术性的术语解释他们的预测模型(至少对于外行来说足够复杂)。

# 民调评级和排名

这个国家有许多民意测验专家。阅读和评估它们的质量可能非常费力和棘手。据该网站称，“*阅读民意测验可能对你的健康有害。症状包括*[](https://fivethirtyeight.com/features/the-art-of-cherry-picking-polls/)**[*过度自信*](https://fivethirtyeight.com/features/the-media-has-a-probability-problem/)*[*为瘾君子数字*](https://fivethirtyeight.com/features/is-don-blankenship-really-surging-in-west-virginia/) *，以及* [*急于判断*](https://fivethirtyeight.com/features/should-we-take-these-early-general-election-polls-seriously-no/) *。谢天谢地，我们有治疗方法。*([来源](https://fivethirtyeight.com/features/how-to-read-2020-polls-like-a-fivethirtyeighter/))***

**有民调。然后，还有[的民调](https://www.cnn.com/2020/06/05/politics/cnn-poll-of-polls-may-trump-biden/index.html)的民调。然后，还有民调的加权民调。最重要的是，有一个民意调查的权重是统计建模和动态变化的权重。**

**[](https://www.pewresearch.org/methods/2019/11/19/a-field-guide-to-polling-election-2020-edition/) [## 选举 2020 投票现场指南-皮尤研究中心方法

### 虽然美国的调查研究是一项全年的事业，但公众对投票的关注从来没有像现在这样多…

www.pewresearch.org](https://www.pewresearch.org/methods/2019/11/19/a-field-guide-to-polling-election-2020-edition/) 

作为一名数据科学家，你对其他著名的排名方法是否耳熟能详？亚马逊的产品排名还是网飞的电影排名？可能吧，是的。

本质上，538 使用这种评级/排名系统来衡量民意测验结果(排名高的民意测验者的结果被给予更高的重要性等等)。他们还**积极跟踪每个民意调查结果背后的准确性和方法，并在全年调整他们的排名**。

[](https://fivethirtyeight.com/features/how-fivethirtyeight-calculates-pollster-ratings/) [## FiveThirtyEight 如何计算民调评分

### 见 FiveThirtyEight 的民意测验评分。民意测验是 FiveThirtyEight 的创始特色之一。我在……

fivethirtyeight.com](https://fivethirtyeight.com/features/how-fivethirtyeight-calculates-pollster-ratings/) 

> 有民调。然后，还有民调的民调。然后，还有民调的加权民调。最重要的是，有一个民意调查的权重是统计建模和动态变化的权重。

有趣的是注意到**他们的排名方法并不一定把样本量大的民意测验专家评为更好的**。下面来自他们网站的截图清楚地证明了这一点。虽然 Rasmussen Reports 和 HarrisX 这样的民意调查机构有更大的样本量，但事实上，Marist College 以适中的样本量获得了+评级。

![](img/a07365b7a07aa8a7a2952006cb4e0c1c.png)

图片来源:[网站](https://projects.fivethirtyeight.com/polls/)作者于 2020 年 6 月 6 日截屏。

幸运的是，他们还在 Github 上开源了他们的民意调查排名数据(以及几乎所有其他数据集)[。如果你只对好看的桌子感兴趣，](https://github.com/fivethirtyeight/data/tree/master/pollster-ratings)[这里有](https://projects.fivethirtyeight.com/pollster-ratings/)。

自然，作为一名数据科学家，您可能希望更深入地研究原始数据，并理解诸如以下内容:

*   他们的数字排名如何与民意测验专家的准确性相关联
*   如果他们对选择特定的民意测验专家有党派偏见(在大多数情况下，他们可以被归类为倾向民主党或倾向共和党)
*   谁是**排名最高的民意测验专家**？他们进行了很多民意调查，还是有选择性的？

我们试图分析数据集来获得这样的见解。让我们深入研究代码和发现，好吗？

# 分析

你可以在我的 Github 回购 T3 上找到 Jupyter 笔记本 [**。**](https://github.com/tirthajyoti/Web-Database-Analytics/blob/master/Pollster-ratings-538.ipynb)

## 源头

首先，您可以直接从他们的 Github 中提取数据，放入 Pandas 数据框架中，如下所示:

![](img/a4e770e45d574cfea52ed107a69883fd.png)

该数据集中有 23 列。这是它们的样子，

![](img/6ce75b7e3f413f8704ae2d7fcdfb9ce1.png)

## 一些改造和清理

我们注意到一列有一些额外的空间。其他一些可能需要一些提取和数据类型转换。

![](img/6bae2815f086fbce4650628aaa74ab54.png)![](img/570aa3ea4e9d53c02ad9b0c8a5c1e08d.png)![](img/e87c08ec111d726ed33d1a46c2aa4475.png)

在应用该提取之后，新的数据帧具有额外的列，这使得它更适合于过滤和统计建模。

![](img/2ccc80fcea261cedae31dd5ab9475aab.png)

## “538 等级”栏目的考核与量化

列“538 等级”包含了数据集的关键—民意测验者的字母等级。就像正规考试一样，A+比 A 好，A 比 B+好。如果我们画出字母等级的数量，我们观察到总共 15 个等级，从 A+到 f。

![](img/92cdaff2d10b08ed390215e95f557ac3.png)

我们不需要处理这么多的分类等级，我们可以把它们组合成少量的数字等级——4 代表 A+/A/A，3 代表 B，等等。

![](img/5a6bd032464d17183ea1d1c9d90afe9f.png)

## 箱线图

进入视觉分析，我们可以从箱线图开始。

假设我们想要检查**哪种轮询方法在预测误差**方面表现更好。数据集有一个名为“ ***简单平均误差*** ”的列，它被定义为“*公司的平均误差，计算为投票结果和实际结果之间的差值，用于区分比赛中前两名参赛者。*

![](img/8ee0a9e8bcc0f37e2e0750d730011b90.png)

然后，我们可能会有兴趣去检查是否有一定党派偏见的民意调查者比其他人更成功地正确预测选举。

![](img/6e86dfe41c37908058236af2ab91c86f.png)

注意到上面有趣的东西了吗？如果你是一个进步的、自由的思想家，十有八九，你可能是民主党的党员。但是，一般来说，倾向共和党的民意调查者认为选举更准确，可变性更小。最好小心那些投票！

数据集中另一个有趣的列叫做“***”NCPP/阿泊尔/罗珀* 【T3”。它“*表明这家民调公司是不是全国民意调查委员会*[](http://www.ncpp.org/)**的成员、* [*美国公众舆论研究协会透明度倡议*](https://www.aapor.org/Standards-Ethics/Transparency-Initiative/Current-Members.aspx) *的签署者，还是罗珀公众舆论研究中心数据档案* *的贡献者。实际上，成员资格表明遵循了更可靠的轮询方法*”([来源](https://projects.fivethirtyeight.com/pollster-ratings/))。***

*如何判断前述论断的有效性？数据集有一个名为“ ***高级加减*** ”的列，这是“*一个将一个民调机构的结果与调查相同种族的其他民调公司进行比较的分数，它对最近的结果加权更大。负分是有利的，表示质量高于平均水平*”([来源](https://projects.fivethirtyeight.com/pollster-ratings/))。*

*这是这两个参数之间的箱线图。与 NCCP/阿泊/罗珀相关的民意测验专家不仅表现出较低的误差分数，而且还表现出相当低的可变性。他们的预测似乎是稳健的。*

*![](img/83827fbe90eae6ec33638e00b92adec7.png)*

> *如果你是一个进步的、自由的思想家，十有八九，你可能是民主党的党员。但是，平均而言，倾向共和党的民意调查者认为选举更准确，可变性更小。*

## *散点图和回归图*

*为了了解参数之间的相关性，我们可以通过回归拟合来查看散点图。我们使用 Seaborn 和 Scipy Python 库以及一个定制的函数来生成这些图。*

*例如，我们可以将正确称为 的“ ***种族”与“ ***”预测正负*** ”联系起来。根据 538，“ ***预测正负*** ”是“*对民意调查者在未来选举中准确性的预测。它是通过将民意测验专家的高级正负分数还原为基于我们的方法论质量代理的平均值来计算的*。([来源](https://projects.fivethirtyeight.com/pollster-ratings/))****

*![](img/5ca08d8f656a539ae8d6552a31a77d55.png)*

*或者，我们可以检查我们定义的“ ***数字等级*** ”如何与轮询误差平均值相关联。负趋势表示较高的数字等级与较低的轮询错误相关联。*

*![](img/1ed9115b62d43f5bb6dd65aa6a0e3fc7.png)*

*我们还可以检查“ ***”、*** “用于偏见分析的民调数字”是否有助于减少分配给每个民调者的“*”党派偏见程度。我们可以观察到一种向下的关系，表明大量民意调查的可用性确实有助于降低党派偏见的程度。然而，这种关系看起来**高度非线性**并且**对数标度**会更好地拟合曲线。**

**![](img/9e554c7172aec5cedd1abed16dfb3d8f.png)**

****越活跃的民意调查者越值得信任吗**？我们绘制了民意测验数量的直方图，看到它遵循负幂律。我们可以**过滤掉民意测验数量非常低和非常高的民意测验者**并创建一个自定义散点图。然而，我们观察到在民意测验的数量和预测的正负分数之间几乎不存在相关性。因此，大量的民意调查并不一定导致高的民意调查质量和预测能力。**

**![](img/8a3d81ddfee09e1c6b6c19750d639378.png)****![](img/873562aa8a7b24162094c31fc98421df.png)**

> **…大量民意调查的可用性确实有助于降低党派偏见的程度。**

## **过滤和排序顶级民意测验专家**

**最后，我们可以通过定制的过滤逻辑，执行简单的数据帧操作来提取排名最高的民意测验者列表。例如，我们可以提出问题“**做过 50 次以上民意调查的前 10 名民意调查者中，谁的高级加减分最好**？”。**

**![](img/75545826fc45e195a7b3b3ecb3ac3e5d.png)**

**这是结果。请注意，我们没有按照“538 等级”或“数字等级”进行排序，但是因为它们与“高级加减”分数相关，所以在这个提取的列表中，大多数民意调查者都有 A+或 A 评级。**

**![](img/4e483715c693b132c6da3bfdac0365bb.png)**

> **因此，大量的民意调查并不一定导致高的民意调查质量和预测能力。**

## **其他因素**

**数据集包含其他参数，如' ***、豪斯效应*** '和' ***、均值回复偏差*** '，其中也包含党派偏差信息。它们肯定被用于五点三八预测的内部建模，可以进一步探索。**

# **摘要**

**在本文中，我们展示了如何从久负盛名的 538 门户网站获取民意测验者评级的原始数据，并编写一个简单的 Python 脚本来进行适当的转换和可视化分析数据。**

**同样，你可以在我的 Github repo 上找到 Jupyter 笔记本 [**。**](https://github.com/tirthajyoti/Web-Database-Analytics/blob/master/Pollster-ratings-538.ipynb)**

**答同样，你可以查看作者的 [**GitHub**](https://github.com/tirthajyoti?tab=repositories) **知识库**获取机器学习和数据科学方面的代码、思想和资源。如果你像我一样，对人工智能/机器学习/数据科学充满热情，请随时[在 LinkedIn](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/) 上添加我，或者[在 Twitter](https://twitter.com/tirthajyotiS) 上关注我。**

**[](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/) [## Tirthajyoti Sarkar - Sr .首席工程师-半导体、人工智能、机器学习- ON…

### 通过写作使数据科学/ML 概念易于理解:https://medium.com/@tirthajyoti 开源和有趣…

www.linkedin.com](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/)****