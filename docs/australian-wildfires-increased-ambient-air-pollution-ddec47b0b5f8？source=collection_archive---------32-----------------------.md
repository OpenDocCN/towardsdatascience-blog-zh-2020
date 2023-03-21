# 澳大利亚野火增加了环境空气污染

> 原文：<https://towardsdatascience.com/australian-wildfires-increased-ambient-air-pollution-ddec47b0b5f8?source=collection_archive---------32----------------------->

我使用来自新南威尔士的监测数据来显示该州的环境空气污染是如何增加的。下载数据，自己看！

现在，你一定已经听说了澳大利亚东南部两个州新南威尔士和维多利亚发生的灾难性野火。到目前为止，森林大火已经烧毁了 800 多万公顷的土地，使 10 多万人流离失所，并导致至少 29 人和 10 亿只动物死亡。

[令人心碎的照片](https://www.nytimes.com/2020/01/03/world/australia/wildfires-pictures.html)在互联网上随处可见，其中大多数都有一个共同的特征:一片红色的天空。这种启示性的黄色过滤器是有机物燃烧产生的浓烟的产物。

野火是颗粒物(PM)的强大来源，PM 是由悬浮在空气中的有机(和无机)物质的混合物组成的颗粒。PM 10 和 PM 2.5 是两种常见的报告污染物；它们的数字表示颗粒的最大直径(微米)。这些微小颗粒只有人类头发的一小部分，非常危险。

事实上，空气污染(以 PM 的形式)可能是野火最大的*无形*后果，因为与空气污染相关的短期和长期成本不像火焰那样明显。

数据显示，在遭受火灾的地区，环境空气污染已经恶化；这些地区的个人定期**暴露在被认为是'*危险'*或'T10 非常*不健康'T13 的浓度中；一些监测站记录的浓度水平甚至超过了任何健康标准的极限。***

**虽然火灾结束后，严重的环境空气污染会很快恢复到正常水平，但它的一些无形成本可能会持续几十年。**

> ***滚动至底部，查看如何下载数据并自行进行分析的说明。***

# **野火增加了环境空气污染**

**新南威尔士州(NSW)规划、发展和环境部[报告](https://www.dpie.nsw.gov.au/air-quality/air-quality-concentration-data-updated-hourly)他们每个监测站的多种污染物的日平均空气质量指数(AQI)。**

**![](img/fa9349b35f2a08903be58ea3d92cf0de.png)**

**资料来源:[新南威尔士州规划、发展和环境部](https://www.environment.nsw.gov.au/topics/air/understanding-air-quality-data/air-quality-index)**

**AQI 将空气污染的*剂量*(即给定时间段内污染物的浓度)转换为基于国家空气质量标准的指数值。新南威尔士州政府使用的空气质量指数见下表。**

**澳大利亚和(通常使用的)美国环境保护局(EPA)标准的比较可以在[这里](https://aqicn.org/faq/2014-09-06/australian-air-quality-comparison-with-the-us-epa-aqi-scale/)找到。**

**我通过对分布在该州的 40 多个监测站的数据进行平均，得出了新南威尔士州 PM 2.5 的每日 AQI 估计值。结果(如下图所示)显示，自 9 月份以来，环境空气污染增加了一倍多——主要是由于 11 月份火灾加剧。事实上，环境空气污染的这种程度的峰值在以前的野火季节中并不存在(**附录**T6【A】，并且可以直接归因于丛林大火(**附录 B** )。**

**![](img/333d51105adaeac8ab09b500782d72a6.png)**

**新南威尔士州 40+监测站 PM 2.5 日平均 AQI 读数，采用澳大利亚标准。资料来源:[新南威尔士州规划、发展和环境部](https://www.dpie.nsw.gov.au/air-quality/air-quality-concentration-data-updated-hourly)；作者的计算。**

**该地区的平均空气污染已经从平均**非常好/良好**上升到平均**差/非常差**。美国的等效 AQI 是**中度**——即空气质量可以接受，但可能对一些人构成风险，特别是那些对空气污染异常敏感的人。**

**这种增加也可以从野火通常排放的其他污染物(如 PM 10、CO 和臭氧)中观察到。我在**附录 c**中绘制了多种污染物的行为**

**然而，分析州一级的数据，掩盖了成千上万接近森林大火的人对非常危险的空气污染浓度的严重暴露；相反，独立地分析台站提供了对事件的更有启发性的见解。在下图中，我绘制了八个站点的数据，这些站点的每日 PM 2.5 浓度有显著差异。**

**![](img/cc8696e38f49034107830643bc4af695.png)**

**8 个监测站(3 个应急站)PM 2.5 的日平均 AQI 读数，采用澳大利亚标准。资料来源:[新南威尔士州规划、发展和环境部](https://www.dpie.nsw.gov.au/air-quality/air-quality-concentration-data-updated-hourly)；作者的计算。**

**靠近火灾的两个城镇 Katoomba 和 Goulburn 的空气污染浓度经常远远超过可接受的水平(AQI 在 0-66 之间)。**

**到目前为止，北瓦加瓦加经历了环境空气污染最严重的一天。世界上污染最严重的城市是印度的古鲁格拉姆，其日均(PM 2.5) EPA AQI 为 145。1 月 5 日，北瓦加瓦加经历了日均 500+的 EPA AQI！**

**![](img/bf38376d5b63f80286ed78d9ae18b6f6.png)**

**新南威尔士州监测站 PM 2.5 的十大日平均 AQI 读数。访问日期:2020 年 1 月 14 日。资料来源:[新南威尔士州规划、发展和环境部](https://www.dpie.nsw.gov.au/air-quality/air-quality-concentration-data-updated-hourly)；作者的计算。**

**临近的表格详细列出了空气污染最严重的 10 天(根据澳大利亚和美国环保局的空气质量指数)。Goulburn 不成比例地出现在十个最差的地方，接下来是 Katoomba 和 Orange。**

**12 月 31 日，古尔本的平均空气质量指数为 1335(环保局的空气质量指数为 385)，当时古尔本邮报[上传了这张照片](https://www.goulburnpost.com.au/story/6562301/smoke-chokes-city-and-power-goes-out/)并报告停电。要比较你所在城市的环境污染与如此高的浓度，请查看 NYT 互动[看看世界上污染最严重的空气与你所在城市的](https://www.nytimes.com/interactive/2019/12/02/climate/air-pollution-compare-ar-ul.html)相比如何。**

**如果 2240 的空气质量指数看起来很高——与监测器报告的最高空气质量指数相比，这是微不足道的。记录的前十个浓度对应的 AQI 值在 4500 到 7200 之间(其中八个大于 5000)。**

**下面的直方图详细描述了一月份前两周的小时数(y 轴),监测器记录的空气质量指数(x 轴)被 EPA 标准标记为*非常* *不健康*(即 EPA 空气质量指数为 200+)。它报告澳大利亚的等效空气质量指数。虽然空气质量指数超过 4000 不会持续超过几个小时(如下所示)，但它们会对人产生可怕的(如果不是致命的)后果。**

**![](img/f51e8dff2ad4bc8ab2e025dd0e101c62.png)**

**直方图:1 月 1 日至 1 月 14 日，40 多个监测站记录了总计 331 个小时的空气质量指数超过 200。资料来源:[新南威尔士州规划、发展和环境部](https://www.dpie.nsw.gov.au/air-quality/air-quality-concentration-data-updated-hourly)；作者的计算。**

***跟踪 NSW.gov*[](https://www.dpie.nsw.gov.au/air-quality/air-quality-concentration-data-updated-hourly/daily-air-quality-data)**[*aqicn.org*](http://aqicn.org/map/australia/)*或*[*IQAir*](https://www.airvisual.com/air-quality-map?lat=-33.886374&lng=151.185934&zoomLevel=6)*的实时空气污染读数；关注*[*my fire watch*](https://myfirewatch.landgate.wa.gov.au/)*现场直播更新。*****

*****点击* [*阅读更多关于森林大火的信息。阅读更多关于*](https://www.telegraph.co.uk/news/2020/01/02/australian-bushfires-numbers-highlight-sheer-scale-unfolding/) [*大火造成的空气污染的对话*](https://theconversation.com/bushfire-smoke-is-everywhere-in-our-cities-heres-exactly-what-you-are-inhaling-129772?utm_source=twitter&utm_medium=twitterbutton) *。*****

# ****如何提供帮助****

> *****NYT 发表了一篇关于* [*如何帮助澳洲火灾受害者*](https://www.nytimes.com/2020/01/06/world/australia/help-australia-fires.html) *的文章。考虑为新南威尔士州农村消防服务局*[](https://quickweb.westpac.com.au/OnlinePaymentServlet?cd_community=NSWRFS&cd_currency=AUD&cd_supplier_business=DONATIONS&action=EnterDetails)**或维多利亚中央消防局*[](https://www.cfa.vic.gov.au/about/supporting-cfa#donate-cfa)**做贡献。* [*救助儿童会*](https://www.savethechildren.org.au/donate/more-ways-to-give/current-appeals/bushfire-emergency) *致力于管理儿童友好空间，让儿童能够在一个安全、受支持的环境中处理他们的经历。最后，考虑支持野生动物救援工作，例如来自世界野生动物基金会的。*******

# *****附录 A*****

*****附录 A 显示了 2017 年 6 月至 2020 年 1 月新州 PM 2.5 的日平均 AQI 读数。下图显示，这一野火季节的环境空气污染明显高于以往季节。*****

*****![](img/8cc74e120555e91e68803e17d847d552.png)*****

*****新南威尔士州 40+监测站 PM 2.5 日平均 AQI 读数，采用澳大利亚标准。资料来源:[新南威尔士州规划、发展和环境部](https://www.dpie.nsw.gov.au/air-quality/air-quality-concentration-data-updated-hourly)；作者的计算。*****

*****下图显示了相同的点，但使用的是 PM 10。*****

*****![](img/45cde3ba68b658aadaf855bc4278d0d4.png)*****

*****新南威尔士州 30+监测站 PM 10 日平均 AQI 读数，采用澳大利亚标准。资料来源:[新南威尔士州规划、发展和环境部](https://www.dpie.nsw.gov.au/air-quality/air-quality-concentration-data-updated-hourly)；作者的计算。*****

# *****附录 B*****

*****附录 B 提供了进一步的证据，证明环境空气污染的增加与野火有关。2019 年的森林火灾季节开始于 9 月(比往常早)，由于[更高的温度和热空气库](https://www.bbc.com/news/world-australia-50428865)，11 月期间[加剧。到月底，消防队员已经扑灭了 120 多处森林大火。](https://www.bbc.com/news/world-australia-50951043)*****

*****我在 11 月初选择了一个任意的日期，然后进行了一个基本的[回归不连续时间](https://www.annualreviews.org/doi/abs/10.1146/annurev-resource-121517-033306?journalCode=resource)。结果可以在下图中找到。*****

*****![](img/e311c13c325fa72f58233bb3e2e9d568.png)*****

*****新南威尔士州 40+监测站 PM 2.5 日平均 AQI 读数，采用澳大利亚标准。资料来源:[新南威尔士州规划、发展和环境部](https://www.dpie.nsw.gov.au/air-quality/air-quality-concentration-data-updated-hourly)；作者的计算。*****

*****虽然我没有考虑自回归或风等天气模式，但在数据中可以观察到约 40 个单位的具有统计显著性处理估计的明显不连续性。处理的右侧受到多种因素的影响，包括长期的自回归(因为更稠密的空气污染需要更长的时间来稀释)、风的模式、相互作用的影响以及野火的扩展和收缩。*****

*****下图描绘了处理前后样本的平均值(11 月火灾前后)。虽然它不能准确估计处理效果，但它表明野火是环境空气污染增加的根源。t 检验确定两个样本的平均值之间的统计显著差异。*****

*****![](img/cdc7f42d6a47329fe954b2151eb5194b.png)*****

# *****附录 C*****

*****附录 C 介绍了对其他污染物的见解。野火排放一次污染物(PM、CO、NOx)和二次污染物。臭氧等二次污染物是 NOx 和 PM 与其他污染物(如碳氢化合物)相互作用的产物。下图显示一氧化碳和臭氧的浓度增加了(左)，而一氧化氮和二氧化硫的浓度没有变化(右)。*****

*****![](img/484e2ec28c2ee0fa07dfc9a198f31933.png)*****

*****新南威尔士州记录的 CO、NO、SO2 和臭氧的日平均 AQI 读数。资料来源:[新南威尔士州规划、发展和环境部](https://www.dpie.nsw.gov.au/air-quality/air-quality-concentration-data-updated-hourly)；作者的计算。*****

*****虽然预计二氧化硫浓度不会发生变化，但野火是氮氧化物的主要排放源。[me bust(2013)](https://digitalassets.lib.berkeley.edu/etd/ucb/text/Mebust_berkeley_0028E_13817.pdf)【1】将澳洲野火氮氧化物排放的可变性归因于在火灾季节植物在地下重新分配氮的事实。我们的数据确实显示了 NO 的季节性变化，在火灾季节浓度较低。*****

*****[1] A.K. Mebust，[从太空观察到的野火氮氧化物排放的可变性](https://digitalassets.lib.berkeley.edu/etd/ucb/text/Mebust_berkeley_0028E_13817.pdf) (2013)，加州大学伯克利分校。*****

# *****自己分析数据*****

*****您可以下载数据并自己进行分析！前往[新南威尔士州规划、发展和环境部](https://www.dpie.nsw.gov.au/air-quality/search-for-and-download-air-quality-data)并遵循以下步骤:*****

*****![](img/09b1e3c0d6956e169baad521daf510ba.png)*****

*******第一步:**选择数据类别和参数(污染物)。*****

*****选择“每日”或“每小时”**现场平均值**以及您感兴趣的**污染物**。这将提供浓度的平均值(g/m3 或 ppm)。*****

*****有了浓度方面的数据，您就可以轻松地将空气质量转换为任何 AQI(无论是 EPA、澳大利亚还是您所在国家的环境机构使用的 AQI)。*****

*****要将浓度转换为澳大利亚 AQI，请遵循此处的说明。污染物目标浓度是给定污染物的 NEPM 空气标准。这个标准是由国家政府制定的。*****

*****![](img/bd25854032edc44a31c36302e7285e5b.png)*****

*******第二步:**选择地区或站点。*****

*****由于数据下载时间不长，我建议您选择所有站点(站)。这可以通过选择粗体显示的区域来轻松完成。*****

*****不要忘记紧急站点！它们位于底部。*****

*****![](img/32b6c67f5dff881050ab6520467ea2b8.png)*****

*******第三步:**下载数据。*****

*****点击 **'** 下载成文件 **'** 下载一个 xls (excel)文件。*****

*****选择所需的开始和结束日期。每小时数据的每次下载限制为 365*24*15 条记录。*****

*****下载数据后，您可以创建整个新南威尔士州地区的日平均值，如下所示:*****

```
***Daily_Average$mean = rowMeans(Daily_Average[2:nrow(Daily_Average], na.rm = T)***
```

*****您可以使用新南威尔士州使用的 AQI **(仅用于 PM 2.5)**生成一个变量，如下所示:*****

```
***Daily_Average$AQI_mean = (100*Daily_Average$mean)/25***
```

*****您可以使用 ggplot 绘制数据:*****

```
***main = ggplot(data=Daily_Average, aes(x=Date, y=AQI_mean)) + geom_line() + geom_smooth(span=0.6, col="steelblue4")*** 
```

*****制作一个最小的背景，如下所示:*****

```
***main + 
theme_minimal() + theme(panel.grid.minor = element_blank(), panel.grid.major = element_blank())***
```

*****插入澳大利亚 AQI 行作为参考，如下所示:*****

```
***main + 
geom_hline(yintercept = (0), col="steelblue2", alpha=0.6, linetype =   "dashed", size=.65) + # Very good
geom_hline(yintercept = (33), col="green4", alpha=0.6, linetype = "dashed", size=.65) + # Good
geom_hline(yintercept = (67), col="gold", alpha=0.6, linetype = "dashed", size=.65) + # Fair
geom_hline(yintercept = (100), col="orange", alpha=0.6, linetype = "dashed", size=.65) + # Poor
geom_hline(yintercept = (150), col="brown4", alpha=0.6, linetype = "dashed", size=.65) + # Very Poor
geom_hline(yintercept = (200), col="orangered2", alpha=0.6, linetype = "dashed", size=.65) # Hazardous***
```

*****按如下方式添加标签:*****

```
***main +
labs(title = "Ambient Air Pollution in New South Wales",
subtitle = "Daily average AQI readings of PM 2.5 (Australian standards)", 
caption = "Source: NSW Department of Planning, Industry and Environment; author's calculations.
Note: The mean is based on 40+ stations. The colored dashed lines are hazard labels based on NSW recommendations.") +
xlab("Date") + 
ylab("AQI") + 
theme(text = element_text(size=12.5)) + # Adjust font size
theme(plot.subtitle = element_text(hjust = 0.5)) + # Adjust text
theme(plot.title = element_text(hjust = 0.5)) # Adjust text***
```

*****可以通过 **rdd** 包进行时间上的回归中断:*****

```
***library(rdd)
library(dplyr)Daily_Average = Daily_Average %>% mutate(n = row_number())plot(RDestimate(data = Daily_Average, cutpoint = 876, AQI_mean~n))title(main="Bushfires Cause a Sharp Discontinuity in Ambient PM 2.5", ylab="AQI", xlab="Days since the start of time series")***
```