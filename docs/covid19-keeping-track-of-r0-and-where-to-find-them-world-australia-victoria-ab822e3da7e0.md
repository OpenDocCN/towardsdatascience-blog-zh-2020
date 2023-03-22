# COVID19:跟踪生长因子、每日新病例以及在哪里找到它们。(世界、澳大利亚、维多利亚)

> 原文：<https://towardsdatascience.com/covid19-keeping-track-of-r0-and-where-to-find-them-world-australia-victoria-ab822e3da7e0?source=collection_archive---------25----------------------->

## TLDR:人们很容易迷失在 COVID19 的噪音中，哪些新闻是真的，哪些是假的，应该关注哪些统计数据。答案:[增长因素](https://en.wikipedia.org/wiki/Exponential_growth#Basic_formula)、[世卫组织](https://www.who.int/emergencies/diseases/novel-coronavirus-2019)，以及你的政府(如果你信任你的政府的话)。请访问本文末尾的链接。

*认知状态:统计学学位？没有。流行病学学位？没有。写这篇文章时，我不够资格，也过于固执己见。(最后一次内容更新:2020 年 3 月 27 日)。(我之所以关注澳大利亚和维多利亚州的数据，是因为我目前在墨尔本工作。虽然我来自印度尼西亚，但我不太相信印尼的数据，无法从中做出任何分析。)*

勘误表:为了表明我是多么不合格，我把 R0 误认为生长因子。

![](img/92585792cd2e90b4f7bcc8a64986871d.png)

要跟踪 COVID19，需要注意哪些统计数据？照片由[粘土堤](https://unsplash.com/@claybanks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/covid-data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄。(我真的很怀念总死亡人数只有 6 000 人的那一天)。

# 什么是生长因子？

[生长因子](https://en.wikipedia.org/wiki/Exponential_growth#Basic_formula)，告诉你[情况是变得更糟还是更好](https://youtu.be/Kas0tIxDvrg)。增长因子=1 一般意味着我们已经走了一半，事情会变得更好。你可以停止恐慌，但要保持警惕。然而，增长因子并没有被大量发表，因为当数据真的很嘈杂时，很难推断出 R。[我试过](/the-messiness-of-real-world-data-tracking-r0-of-covid19-with-logistic-function-31cb4d6edd98)。

所以，让我们记录每天的新病例。问自己一个问题:它已经达到顶峰还是正在下降？记住，这个数据是有噪音的，所以不要根据几天的时间就妄下结论。我通常会说，我们需要等待一到两个星期才能下定论。

# 在哪里，怎么做？

对于**世界**的数据，我相信[约翰·霍普斯金](https://coronavirus.jhu.edu/map.html)。他们并不完美，所以也不要完全相信。这是链接:【https://coronavirus.jhu.edu/map.html】T4

[](https://coronavirus.jhu.edu/map.html) [## 约翰霍普金斯冠状病毒资源中心

### 约翰霍普金斯大学全球公共卫生、传染病和应急准备方面的专家一直在…

coronavirus.jhu.edu](https://coronavirus.jhu.edu/map.html) 

转至右下方，点击如下图所示的“**每日增加**”。

![](img/4b6bae4d7c7e420a17759d26fcc8d915.png)![](img/c30b53e242ff2f7a64c0591ffea41f9c.png)

[https://www.reddit.com/r/uselessredcircle/](https://www.reddit.com/r/uselessredcircle/)

![](img/ebabe618c37952b2961f566b9166446a.png)

这是 2020 年 3 月 27 日的数据。还在涨，不好。[(记住，无论今天推出什么样的社交距离政策，其影响都只能在两周内看到。此外，无论这里展示的是什么，都是两周前世界正在做的事情的影响。)](https://medium.com/@tomaspueyo/coronavirus-the-hammer-and-the-dance-be9337092b56)

# 澳大利亚

对于澳洲，我相信卫生部门:[https://www . health . gov . au/news/health-alerts/novel-coronavirus-2019-ncov-health-alert/coronavirus-新冠肺炎-current-situation-and-case-numbers](https://www.health.gov.au/news/health-alerts/novel-coronavirus-2019-ncov-health-alert/coronavirus-covid-19-current-situation-and-case-numbers)

向下滚动一点，你会发现:

![](img/d2e4747e632e460ed2e341c540cb72f8.png)

每日新例是轴在左边的浅蓝色条形图。

好消息是，截至 2020 年 3 月 27 日上午 6:30，增长因子<1 for 3 days straight. Which means that the bar chart is steadily going down. Definitely too early to celebrate. But if this trends continues, the worse has come to pass. **假设这一趋势持续**，峰值约为 2 000 例。因此，估计总数大约是这个数字的两倍，即 4 000 宗。[澳洲有 90 000 张病床](https://www.news.com.au/lifestyle/health/health-problems/coronavirus-will-australias-health-system-get-overrun-by-covid19/news-story/c15cf18a8269e442eed669c3f069a5a9)，[有 2000 台呼吸机](https://www.abc.net.au/news/2020-03-27/do-we-have-enough-ventilators-to-fight-coronavirus/12092432)，只有 [19%表现出严重或更严重的症状(基于中国数据)](https://ourworldindata.org/uploads/2020/03/Severity-of-coronavirus-cases-in-China-1-639x550.png)。2 000 x 19% = 380 名严重或更严重的患者> 2 000 台可用呼吸机(尽管我们现有的患者已经因为非 COVID19 原因使用呼吸机)。所以看起来这是可以控制的。但是这只有在人们保持社交距离的情况下才是真的！人能做的最愚蠢的事情就是看着数据，把它曲解为事情已经结束，放松自己的社交距离，把之前的所有努力都付诸东流。

(也许在不久的将来(几周)，政府可能会稍微放松社会距离，非常精确地控制即将到来的第二波，以可控的方式慢慢建立群体免疫。但是让我们把所有的决定都交给专家吧。)

# 维多利亚

对于维多利亚，也是该州的卫生部门:[https://app.powerbi.com/view?r = eyjrijoiodbmmme 3 nwqtzwnlnc 00 owrkltk 1 njytmjm 2 yty 1 mji 2 nzdjiiiwidci 6 immwzta 2 mdfmlmtbmywmtndq 5 YY 05 yzg 4 lwex MDR jngviowyyocj 9](https://app.powerbi.com/view?r=eyJrIjoiODBmMmE3NWQtZWNlNC00OWRkLTk1NjYtMjM2YTY1MjI2NzdjIiwidCI6ImMwZTA2MDFmLTBmYWMtNDQ5Yy05Yzg4LWExMDRjNGViOWYyOCJ9)

 [## 电源 BI 报告

### 由 Power BI 支持的报告

app.powerbi.com](https://app.powerbi.com/view?r=eyJrIjoiODBmMmE3NWQtZWNlNC00OWRkLTk1NjYtMjM2YTY1MjI2NzdjIiwidCI6ImMwZTA2MDFmLTBmYWMtNDQ5Yy05Yzg4LWExMDRjNGViOWYyOCJ9) ![](img/b362d1103a271ee3be805f2b57fce232.png)

每日新例是左边有轴的深蓝色条形图。

真的慢下来了吗？真的很难讲。

![](img/9ff7e437ae4c37e319b2b7e8258f265f.png)

真的慢下来了吗？

有三次，我们的增长因子连续两三天小于 0，但第二天就飙升，达到一个新的高度，比以往任何时候都糟糕。这就是为什么我们必须等待一到两周才能得出任何结论，也不要对澳大利亚的数据过于乐观。

# 参考文献和致谢

所有图片均基于以下来源(除非另有说明。):

*   世界(约翰·霍普斯金):[https://coronavirus.jhu.edu/map.html](https://coronavirus.jhu.edu/map.html)
*   澳洲(卫生署):[https://www . health . gov . au/news/health-alerts/novel-coronavirus-2019-ncov-health-alert/coronavirus-新冠肺炎-current-situation-and-case-numbers](https://www.health.gov.au/news/health-alerts/novel-coronavirus-2019-ncov-health-alert/coronavirus-covid-19-current-situation-and-case-numbers)
*   维多利亚(卫生部):【https://app.powerbi.com/view? r = eyjrijoiodbmmme 3 nwqtzwnlnc 00 owrkltk 1 njytmjm 2 yty 1 mji 2 nzdjiiiwidci 6 immwzta 2 mdfmlmtbmywmtndq 5 YY 05 yzg 4 lwex MDR jngviowyyocj 9