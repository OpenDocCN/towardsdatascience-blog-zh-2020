# 疫情(新冠肺炎)期间的巨大潜在投资机会

> 原文：<https://towardsdatascience.com/huge-potential-investment-opportunities-during-a-pandemic-covid-19-14013803eba4?source=collection_archive---------13----------------------->

## 识别大幅折价的股票，在管理风险的同时重新思考投资策略

*在这样一个前所未有的时代，这篇文章旨在揭示当前市场的一些趋势/机会，让任何潜在投资者具备他们的* ***数据驱动决策*** *。每个投资者的风险偏好都是独特的，本文的目的是* ***告知*** *(而非建议)你的投资策略。*

![](img/79750e437451a9386c9add26f14e85c1.png)

[来源](https://www.azquotes.com/quote/1084420) : AZ 报价，彼得·林奇

> *“25000 年了，人性没怎么变。有些东西会从左场出来，市场会下跌，或者上涨。波动将会发生，市场将继续出现这些起伏——我认为这是一个很好的机会，如果人们能够明白他们拥有什么的话”。*
> 
> **《我爱波动》**
> 
> —彼得·林奇对塔可钟的投资在 6 年内获得 600%的回报。

科技泡沫正在破裂，市场已经开始走下坡路，我可以自信地说，这不是任何人认为他们会走的路。在担心天文数字的初创企业估值会拖垮初创企业经济后，一个全球化的疫情正威胁要这样做。裁员在加速，股价在暴跌，普通民众中存在不确定性。对经济来说，这是绝望的时刻，然而，正是在这样的时刻，机会就在那里。问题是，你打算怎么办？

# 价值投资背后的动机

我的背景是工程学，尽管我可能擅长数字，但我上了一堂金融分析课，才意识到金融知识是多么基本和重要。当我想到投资市场时，主要是投资 ETF(因为本质上我想模仿市场，并与市场一起增加投资)，关于新冠肺炎的新闻爆发了。所有人突然开始呆在家里，寻找温度计、口罩和卫生纸。

在做研究、与其他投资者交谈和狂看 YouTube 视频时，我意识到这可能是一个不常出现的机会。虽然新闻充斥着对哪些股票需要关注的预测，而且推理可能是有效的，但作为一个新手投资者，不可能试图预测短期内会发生什么。 因此，我决定专注于长期价值投资。从逻辑上来说，我把注意力集中在世纪之交的公司上。(这个股市指数衡量在美国证券交易所上市的 500 家大公司的股票表现。)投资这类公司的理由是 ***，因为它们不太可能去任何地方*** 。它们对经济至关重要，如果情况出现，政府最好会出手救助它们。

# 收集数据和了解 S&P

为了我的分析，我使用了下面的维基百科[页面](https://en.wikipedia.org/wiki/List_of_S%26P_500_companies) *来收集所有的股票名称及其符号。

**注:从维基百科页面*收集了 502 个公司名称

从维基百科抓取信息的 Python 代码

我分析的第一件事是 S&P 投资组合指数由多少个行业组成，以及它们各自的代表性。

![](img/03b805c319c4f2063452ee5e3c45ea72.png)

按部门表示指数。按作者分类的图表

使用雅虎金融的 API，使用相应的股票代号来摄取从 2019 年 12 月 1 日到当前日期的每日股票价格。选择特定的日期没有任何特定的原因，这个想法是为了获取足够的历史数据来了解崩溃前发生了什么。2020 年 1 月和 2 月初是股市最好的阶段之一。在分析回报时，我想确保尽最大努力捕捉市场波动。

![](img/26ac9194bb985a5e029b76ad527b044c.png)

[来源](https://au.finance.yahoo.com/chart/AAPL#eyJpbnRlcnZhbCI6ImRheSIsInBlcmlvZGljaXR5IjoxLCJjYW5kbGVXaWR0aCI6MjMuODIzNTI5NDExNzY0NzA3LCJ2b2x1bWVVbmRlcmxheSI6dHJ1ZSwiYWRqIjp0cnVlLCJjcm9zc2hhaXIiOnRydWUsImNoYXJ0VHlwZSI6Im1vdW50YWluIiwiZXh0ZW5kZWQiOmZhbHNlLCJtYXJrZXRTZXNzaW9ucyI6e30sImFnZ3JlZ2F0aW9uVHlwZSI6Im9obGMiLCJjaGFydFNjYWxlIjoibGluZWFyIiwicGFuZWxzIjp7ImNoYXJ0Ijp7InBlcmNlbnQiOjEsImRpc3BsYXkiOiJBQVBMIiwiY2hhcnROYW1lIjoiY2hhcnQiLCJpbmRleCI6MCwieUF4aXMiOnsibmFtZSI6ImNoYXJ0IiwicG9zaXRpb24iOm51bGx9LCJ5YXhpc0xIUyI6W10sInlheGlzUkhTIjpbImNoYXJ0Iiwidm9sIHVuZHIiLCLigIx2b2wgdW5kcuKAjCJdfX0sImxpbmVXaWR0aCI6Miwic3RyaXBlZEJhY2tncm91bmQiOnRydWUsImV2ZW50cyI6dHJ1ZSwiY29sb3IiOiIjMDA4MWYyIiwic3RyaXBlZEJhY2tncm91ZCI6dHJ1ZSwicmFuZ2UiOm51bGwsImV2ZW50TWFwIjp7ImNvcnBvcmF0ZSI6eyJkaXZzIjp0cnVlLCJzcGxpdHMiOnRydWV9LCJzaWdEZXYiOnt9fSwic3ltYm9scyI6W3sic3ltYm9sIjoiQUFQTCIsInN5bWJvbE9iamVjdCI6eyJzeW1ib2wiOiJBQVBMIiwicXVvdGVUeXBlIjoiRVFVSVRZIiwiZXhjaGFuZ2VUaW1lWm9uZSI6IkFtZXJpY2EvTmV3X1lvcmsifSwicGVyaW9kaWNpdHkiOjEsImludGVydmFsIjoiZGF5Iiwic2V0U3BhbiI6bnVsbH1dLCJzdHVkaWVzIjp7InZvbCB1bmRyIjp7InR5cGUiOiJ2b2wgdW5kciIsImlucHV0cyI6eyJpZCI6InZvbCB1bmRyIiwiZGlzcGxheSI6InZvbCB1bmRyIn0sIm91dHB1dHMiOnsiVXAgVm9sdW1lIjoiIzAwYjA2MSIsIkRvd24gVm9sdW1lIjoiI0ZGMzMzQSJ9LCJwYW5lbCI6ImNoYXJ0IiwicGFyYW1ldGVycyI6eyJ3aWR0aEZhY3RvciI6MC40NSwiY2hhcnROYW1lIjoiY2hhcnQiLCJwYW5lbE5hbWUiOiJjaGFydCJ9fSwi4oCMdm9sIHVuZHLigIwiOnsidHlwZSI6InZvbCB1bmRyIiwiaW5wdXRzIjp7ImlkIjoi4oCMdm9sIHVuZHLigIwiLCJkaXNwbGF5Ijoi4oCMdm9sIHVuZHLigIwifSwib3V0cHV0cyI6eyJVcCBWb2x1bWUiOiJyZ2JhKDIwMCwgMjQwLCAyMjAsIDAuOCkiLCJEb3duIFZvbHVtZSI6InJnYmEoMjU1LCA0OCwgNjAsIDAuOCkifSwicGFuZWwiOiJjaGFydCIsInBhcmFtZXRlcnMiOnsid2lkdGhGYWN0b3IiOjAuNDUsImNoYXJ0TmFtZSI6ImNoYXJ0In19fSwiY3VzdG9tUmFuZ2UiOnsic3RhcnQiOjE1NzUyOTE2MDAwMDAsImVuZCI6MTU4NTA1NDgwMDAwMH0sInNldFNwYW4iOm51bGx9):雅虎财经，符号——AAPL

收集每日股票价格的 Python 代码

## 计算回报

每只股票的[调整后收盘价](https://www.investopedia.com/terms/a/adjusted_closing_price.asp)用于计算该股票的每日回报。这个价格准确地反映了股息后的股票价值，被认为是股票的真实价格。财务回报，简单地说，就是在一段特定的时间内投资所赚或赔的钱。

![](img/a27ed9327529d305dd34e85c222934e2.png)

计算回报的公式

关于退货需要注意的关键事项:

1.  在这个分析中，回报是以股票价格的百分比变化来计算的
2.  正回报代表盈利，负回报代表亏损

在计算了每天的回报后，我决定检查股票回报最低的那几天的频率。

![](img/eb5559571ae309b4e60c46c13e6e7d30.png)

2020 年 3 月滴滴最多也就不足为奇了。一半的公司在 2020 年 3 月 16 日(T1)经历了最低的回报率。出于好奇，我决定找出当天的[新闻头条](https://www.bloomberg.com/news/articles/2020-03-15/yen-gains-on-virus-worries-kiwi-slides-on-rates-markets-wrap)。

1.  布伦特原油自 2016 年以来首次跌破每桶 30 美元
2.  世界各地的公司都缩减了活动
3.  美联储将利率下调至零
4.  亚洲和欧洲的国债收益率下降，股市暴跌

这个练习是检查计算和分析输出的好方法。这些数据有助于证实，新冠肺炎确实影响了 S&P 指数中几乎所有的基础资产。

Python 代码计算回报并确定最低回报

# 识别机会——股票价格折扣

这一分析的主要驱动力是识别熊市中的潜在机会。作为一名投资者，我希望这些信息能指引我走向正确的方向。这些数据应该能够回答一些问题，

1.  *我现在应该买什么股票？*
2.  有没有另一只股票的估值更低？
3.  相比之下，它的竞争对手表现如何？

![](img/a3ff530448a209d88ebcabe45464dd43.png)

照片由[杰米街](https://unsplash.com/@jamie452?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

对每只股票的调整后每日收盘价进行了分析。我没有查看从最高价到最低价的变化，而是决定查看股票的第三个四分位数[值与其最低值的差值。通过这种方式计算，估算的折扣更为保守。](https://en.wikipedia.org/wiki/Interquartile_range)

![](img/7e0e3528c19741c40a81ad36b7c1c762.png)

计算股票价格折扣的公式

![](img/ed03e8e1bd770d9c70bbab2dae37e8a1.png)

谷歌调整后收盘价的方框图。按作者分类的图表

在相应的调整后收盘价方框图中，谷歌股价的第三个四分位值为**1447.07 美元**，跌至最低**1056.52 美元**

通过应用上面提到的公式，**谷歌目前的交易折价为 26.9%**

对其余的股票进行了类似的操作，并计算了它们的折扣价。

Python 代码来计算股票的折价

## 按部门的折扣

![](img/961b1d6d60474592c650ef34a04ba2e0.png)

按部门比较折扣。按作者分类的图表

从上图可以看出，能源类股的平均折让率最高，约为 66% ，相比之下，消费类股仅下跌了 **28%** 。**这一信息符合当前市场的行为。**

1.  医疗保健和消费品等行业目前面临巨大压力，这些领域的专业人士在这段时间不知疲倦地工作。但这反过来意味着这些业务正在产生稳定的现金流。
2.  谷歌和苹果等公司分别隶属于**通信服务**和**信息技术**。利用当前的技术，这些巨头会发现远程工作的创新方式，并相应地改变他们的商业模式。相比之下，对他们业务的负面影响不会那么严重。
3.  另一方面，能源行业和非必需品行业的公司受到了严重影响。随着社交距离措施和消费者限制支出，这将直接影响这些企业的日常运营。

下面给出的是每个行业的前 3 只折价股票。他们强调了在各自的行业中哪些公司目前正以巨大的折扣进行交易。

![](img/acf3f4734acabe9794604e92f9781ed8.png)

每个板块 3 只折价最多的股票。按作者分类的图表

# 量化风险

每一个机会都有一定程度的风险。金融领域最大的挑战之一是能够准确量化这种风险。对于本文， [**波动率**](https://www.investopedia.com/terms/v/volatility.asp) 内的每只股票的收益被用来作为风险的度量。

1.  波动性代表一种资产的价格在均值附近波动的幅度——这是对其回报率离差的一种统计度量
2.  在这个分析中，我用**回报率的标准差**来衡量波动性
3.  波动性越高，资产风险越大

Python 代码计算收益的标准偏差

![](img/892e06e0f0405ec5a7447c9c6c840c73.png)

一段时间内亚马逊的回报率。按作者分类的图表

亚马逊(股票代码:AMZN)回报的标准偏差是 0.0252，这意味着亚马逊股票价格的每日变化幅度为 **2.5%**

![](img/0baad47330496169cf17b67e2f7a2154.png)

波音航空公司在这段时间内的收益。按作者分类的图表

波音航空公司(股票代码:BA)收益的标准差是 0.0696，这意味着其股票价格的每日变化大约为 **7 %**

从上面的图表可以看出，亚马逊的可变性低于波音，这意味着亚马逊是一个 ***风险较低的投资选择。***

## 按部门划分的风险

![](img/d4c0f6819fcaba4d492d1b295501956e.png)

按部门比较风险。按作者分类的图表

与折扣趋势类似，可以看出，能源和非必需消费品等风险行业的波动性较高，而消费品和医疗保健的波动性较低。能源行业的平均波动幅度为 **6.38%** ，而另一方面，消费品行业的平均波动幅度为 **3.36%**

有趣的是，折扣和风险之间有很高的相关性。大幅折价的行业(如能源)也面临更高的风险。这种分析的目的是 ***观察这些高折价股票，挑选风险较低的股票。***

![](img/11e3502c32c58658ccae970c9d5b49bc.png)

每个板块 3 只风险最小的股票。按作者分类的图表

从上图可以看出，亚马逊、威瑞森和 Charter Communications 等巨头构成了最稳定的股票。这进一步加强了市场的理解，即这类企业拥有坚实的商业模式和强大的传统，能够在这一动荡时期继续前行。

# 结论

![](img/8ad3983e4227f85f8f13e221bd2a4818.png)

折扣与风险。按作者分类的图表

当我接近这篇文章的结尾时，下面给出了我在研究过程中获得的一些重要知识，我想总结一下，以供大家参考。

1.  从上面的数据可以看出，贴现和风险之间有一个趋势。就投资策略而言，**同样的风险，选择折价较高的股票更有效率。**
2.  一家公司的股票价格反映了该公司未来的预期价值。有些公司的股价高于竞争对手是有原因的，作为投资者，理解这一点很重要。
3.  个人对风险的偏好是不同的，这些指标只能指导你的研究。如何投资由你自己决定。
4.  所有伟大的投资者(如沃伦·巴菲特、比尔·盖茨等)都有一个共同的投资秘诀——购买股票意味着你也在购买该业务的一部分。你必须了解这个行业，并且熟悉它的商业模式。
5.  能够编码并不意味着能够进行这种分析或处理数字。一个数学模型的好坏取决于你的专业水平。编程的真正力量在于分析的可重复性和可伸缩性。对我来说，手动收集和分析这些数据非常耗时。我现在可以在未来的任何时候重用这些代码，节省我的时间、精力和精力。

> ***注:*** *公司的完整列表及其折扣、风险和当前价格以 excel 表格形式提供，供您自己参考* [*此处*](https://github.com/wiredtoserve/datascience/blob/master/PortfolioTheory/stocks_analysis.xlsx) *。*

## 个人投资策略

我的专业领域和背景是信息技术和通信服务行业。所以我决定把我的注意力集中在这些领域。从上面的分析中，我直观地知道这两个行业都没有被严重低估，因此波动性相对较小。

![](img/bd052d55fe83ba931531374b4f5a4593.png)

所需行业的报告指标

从上表可以看出，与信息技术相比，通信服务的平均折扣更高，波动性更小。我决定在试图识别当前提供重大投资机会的股票时，给予 ***同等权重的折扣和风险*** 。使用期望的权重，我得出一个分数来排列这些股票，下面给出的是这个自定义评分的输出。

![](img/1e6f9baad1684fb43ff9115a534941c2.png)

指标排名的表格结果

![](img/7ae4877ce18d57794f404c4f13568e87.png)

我会投资的潜在股票组合。按作者分类的图表

该图显示了这两个行业中每只潜在股票的机会与风险。泡沫的大小是股票的相对股价。根据评分标准，这些股票是每个领域的前 5 名。我个人已经跟踪了一些公司，但现在我会在下一次投资决策时考虑其他公司。

# GitHub 链接，谢谢

谢谢你一直读到最后。我希望这篇文章能在这样的时候给你一些鼓励，并为你在下一步投资旅程中提供思考的食粮。我祝你一切顺利，对此的任何反馈都将不胜感激。

所有的图表和代码都可以在我的 GitHub 资源库中找到，请随意下载并分析您的用例信息。

[](https://github.com/wiredtoserve/datascience/tree/master/PortfolioTheory) [## 有线服务/数据科学

### 存放我所有杂物的地方。通过在…上创建帐户，为 wiredtoserve/datascience 的发展做出贡献

github.com](https://github.com/wiredtoserve/datascience/tree/master/PortfolioTheory) 

***编者按:*** [*走向数据科学*](http://towardsdatascience.com/) *是一份以数据科学和机器学习研究为主的中型刊物。我们不是健康专家，这篇文章的观点不应被解释为专业建议。*

![](img/1ed893a2c8d5125cf4250a329d452d0e.png)

[皮特·佩德罗萨](https://unsplash.com/@peet818?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片