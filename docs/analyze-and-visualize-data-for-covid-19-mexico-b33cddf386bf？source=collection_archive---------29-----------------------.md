# 受冠状病毒影响最严重的 10 个国家和墨西哥

> 原文：<https://towardsdatascience.com/analyze-and-visualize-data-for-covid-19-mexico-b33cddf386bf?source=collection_archive---------29----------------------->

## 深入了解新冠肺炎在墨西哥的传播情况，以及十大疫情最严重的国家

![](img/a13c903cb9b9dce2fbd68a29cfc76aca.png)

墨西哥确诊病例密度分布图(Diego Hurtado)

作者:[迭戈·乌尔塔多](https://medium.com/u/3b572bafb88a?source=post_page-----b33cddf386bf--------------------------------)

# 介绍

本说明的目的是描述墨西哥与受影响最严重的国家的总体现状，这可以为人们采取有益的社会和卫生措施以阻止新冠肺炎的传播服务。

自 2019 年 12 月下旬以来，一种新型冠状病毒疾病的爆发(新冠肺炎；此前被称为 2019-nCoV)在中国武汉被报道，随后影响了全球 210 个国家。一般来说，新冠肺炎是一种急性疾病，但它也可能是致命的，病死率为 29%。截至 2020 年 1 月 19 日，墨西哥**已确诊约 1.03 亿病例，超过 224 万人死亡，5730 万人康复。**

![](img/0ebbb118e65563bc75891b74bc45d088.png)

Covid 案例分布(Diego Hurtado)

# 症状

在出现症状之前，人们可能会感染病毒 1 至 14 天。冠状病毒疾病(新冠肺炎)最常见的症状是发烧、疲劳和干咳。大多数人(约 80%)无需特殊治疗即可康复[2]。

*   咳嗽
*   发热
*   疲劳
*   呼吸困难(严重病例)

# 推荐

阻断人与人之间的传播，包括减少密切接触者和医护人员之间的二次感染，防止传播放大事件，并防止进一步的国际传播[3]。

*   及早识别、隔离和护理患者，包括为受感染的患者提供优化的护理；
*   识别并减少动物源传播；
*   解决关于临床严重程度、传播和感染程度、治疗方案的关键未知问题，并加快诊断、治疗和疫苗的开发；
*   向所有社区传达关键风险和事件信息，并反驳错误信息；
*   通过多部门伙伴关系将社会和经济影响降至最低

# 数据

自 2020 年 1 月 22 日以来的新型冠状病毒(新冠肺炎)流行病学数据。从包括 DXY.cn 世界卫生组织(世卫组织)在内的各种来源的[约翰·霍普斯金 CSSE 数据仓库](https://github.com/CSSEGISandData/COVID-19/tree/master/csse_covid_19_data/csse_covid_19_time_series)加载数据。数据每天更新两次。

数据中可用的字段包括省/州、国家/地区、上次更新、确认、疑似、恢复、死亡。

2020 年 3 月 23 日，发布了新的数据结构。最新时间序列数据的现有资源有:

*   时间序列 _ covid19 _ 已确认 _ 全球. csv
*   时间序列 _ covid19 _ 死亡人数 _ 全球. csv
*   时间序列 _ covid19 _ 已恢复 _ 全局. csv

```
confirmed_df = pd.read_csv('[https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv'](https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv'))recovered_df = 
pd.read_csv('[https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_recovered_global.csv'](https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_recovered_global.csv'))country_df = pd.read_csv('[https://raw.githubusercontent.com/CSSEGISandData/COVID-19/web-data/data/cases_country.csv'](https://raw.githubusercontent.com/CSSEGISandData/COVID-19/web-data/data/cases_country.csv'))
```

# 墨西哥概述🇲🇽

![](img/58ac1b4db934d8660935ce0ed2c8edcd.png)

墨西哥🇲🇽科维德仪表板(迭戈乌尔塔多)

![](img/b13c3ee36d3fad65a4744c29de734429.png)

🇲🇽概览(迭戈·乌尔塔多)

# 🇲🇽墨西哥州(第三)在死亡案例中排名第三

![](img/ad7c30aa43e6166e279d9e09aefb2a05.png)

Choropleth 图死亡率

![](img/3d1861270da0b0d9d961ea7d1d43ea09.png)

墨西哥🇲🇽(第 3 位)死亡案例第三位(迭戈·乌尔塔多)

![](img/4db08af0d9b18ae1b0b0df4dababb0a5.png)

🇲🇽墨西哥(#3)死亡率第三(迭戈乌尔塔多)

![](img/d486a74a4d8193b525c85b60db8ef2e4.png)

墨西哥死亡病例的年龄分布

# 🇲🇽墨西哥州(第三位)在死亡率上排名第三

![](img/375ccfedc4467e8e3b42e193be27bbcf.png)

🇲🇽墨西哥(#3)死亡率第三(迭戈乌尔塔多)

![](img/27c0fa2bda7cd5c6eff1405b6651c5e1.png)

🇲🇽墨西哥(#3)死亡率第三(迭戈乌尔塔多)

# 墨西哥🇲🇽(排名第三)在危急情况中排名第四

![](img/5abb14c9937ca129406adeaa51db16ba.png)

墨西哥🇲🇽(第四)在死亡病例中排名第三

# 🇲🇽墨西哥各州概览

![](img/0626227a9276373635212ec220ffdb83.png)![](img/319b8b0678fe0418e53e637fc0abf112.png)![](img/858f697dbef9bb5d0431b325fba5cf98.png)![](img/a3854f504a09c1f2ec678cc9bff338ed.png)![](img/5f97a15bc88a230be3495e63a7b43b5d.png)![](img/3bde851874aeab65d2ae8059c5d4bf1d.png)![](img/0890373c0e3750e5bb6b357324d4c790.png)![](img/cfd9aaecd73c5919548f33bf24e8913a.png)![](img/10e9ea421d014a88faecdc117184ef27.png)![](img/6020b6de7f581126c92c780840fecb36.png)

墨西哥的新冠肺炎:各州确诊病例总数(迭戈·乌尔塔多)

![](img/0d17f36fd8317bd4726623479af49ae1.png)

墨西哥的新冠肺炎:各州死亡病例总数(Diego Hurtado)

![](img/4ccd6345f272dff2012f7c6f2fbc77ea.png)

死亡人数最多的州是墨西哥城

# 墨西哥分析🇲🇽

![](img/e4880426aafec63aee19fea12fd933e8.png)

[墨西哥霍乱地图](/make-a-covid-19-choropleth-map-in-mapbox-5c93ac86e907)死亡病例密度(迭戈·乌尔塔多)

# 墨西哥的移动🇲🇽

> 尽管感染人数激增，墨西哥总统在 5 月开始了“T2 新常态”以“重新激活”经济

![](img/652d694f150018e4fbfdc4f018431cc6.png)

在商店外面，顾客们在等待，没有注意到健康距离。Av。CDMX Eje Central 的华雷斯角。2020 年 5 月 21 日。**图片:***angelica Escobar/福布斯墨西哥版。*

**观察:**

我们可以看到，5 月份确诊和死亡病例开始成倍增长！！

![](img/baf7308a9b0564241b4b6440eb2db50d.png)

墨西哥确诊病例和死亡之间的关系(Diego Hurtado)

**确诊病例:**

![](img/e2d8fd1fed6700423948dc764238b891.png)

时间序列确诊病例墨西哥(迭戈乌尔塔多)

**死亡病例:**

![](img/e2d0995bf6e6689018da5ef9c6648230.png)

时间序列确诊病例墨西哥(迭戈乌尔塔多)

## 死亡案例最多的州(墨西哥州)的交通行为

我们可以看到，当新常态开始时，流动性在 5 月份开始增长

![](img/c1d1ffb0e6f293682a7306222a9745ec.png)![](img/db1f56ecddb0f74bf3d8d17df08d0ad3.png)

“新常态是一种幻觉，我们正处于流行病活动中，我认为在这个时候提议重启社会活动实际上是自杀行为，我认为这会以前所未有的水平重新激活患病和死亡人数。”——马拉奎亚斯·洛佩斯·塞万提斯

# 🇲🇽卫生部

由于大量肺泡损伤和进行性呼吸衰竭，严重疾病发作可能导致死亡。新冠肺炎疫情对长期血液透析患者来说是高风险的，因为他们处于免疫抑制状态，年龄较大，并且存在显著的共病，特别是心血管疾病、糖尿病等。同样值得注意的是，心血管风险因素在墨西哥人口中非常普遍，并以惊人的速度增长，心血管疾病是墨西哥的第一大死亡原因[2]。

## 墨西哥死亡病例的年龄分布

![](img/584612d9aea060a7416bf801ae11bee7.png)

墨西哥死亡病例的年龄分布

## 死亡人数最多的州(墨西哥州)死亡病例的年龄分布

![](img/934639886da2f1b1c25479e2596da1e6.png)

墨西哥死亡病例的年龄分布

![](img/6026b530edc365068a8f4e4dd68046fc.png)![](img/fece39b1872659700eba8757332a58c5.png)![](img/b073c4bedaecda4878128ffebfbc0cf4.png)

# 墨西哥疫苗🇲🇽

我们可以观察到，像以色列这样的国家 58%有第一剂，而墨西哥只有 0.5 %

![](img/728c66daab390f843181aacbcf0c9dc0.png)![](img/aa7e605986df34bea27952a4170320fa.png)

接种疫苗百分比最高的 10 个国家

今天，根据最保守的估计，大约有 9 亿人住在贫民窟。据估计，到 2030 年，地球上将有 1/4 的人生活在贫民窟或其他非正规居住区。让我们来参观一下[世界上最大的贫民窟](http://uk.reuters.com/article/uk-slums-united-nations-world-insight-idUKKBN12H1GK):

*   开普敦的哈耶利沙(南非):40 万人
*   内罗毕的基贝拉(肯尼亚):70 万人
*   孟买的达拉维(印度):100 万
*   **内扎(墨西哥):120 万**
*   卡拉奇的奥兰治镇(巴基斯坦):240 万人

墨西哥城

人口:110 万

虽然有些人认为内扎华尔科约特城，也被称为内扎，已经从一个贫民窟演变成了一个郊区，但砖砌的房子散布在临时搭建的棚屋中，即使以饱受毒品战争折磨的墨西哥的标准来看，这个社区也被认为是极其危险的。社区行动促使政府将土地所有权正式化，开始垃圾收集，并建设其他一些关键的基础设施。现在，大约 70%的居民在该地区工作，这是墨西哥人口最稠密的直辖市。

![](img/dd63874f4081fe4e71811a3403feee92.png)

2016 年 9 月 30 日，墨西哥墨西哥州内扎华科约特尔(Nezahualcóyotl)的鸟瞰图。汤森路透基金会/约翰尼·米勒

**观察:**

*显然，新冠肺炎在墨西哥的传播速度更快，死亡率更高，生活在贫民窟的人可能是这种情况下的一个关键因素。*也许这就是墨西哥冠状病毒病例不断增加的原因。

# 墨西哥需要采取行动

**对墨西哥新冠肺炎的预测性监控:**新冠肺炎预测对于合理化规划和心态至关重要，但也具有挑战性，因为复杂、动态和全球化的新冠肺炎疫情作为一个典型的[棘手问题](https://en.wikipedia.org/wiki/Wicked_problem)。传统的预测或预报努力的目标是现在做出准确的预测，以便在未来实现，但在这种极端不确定的情况下，这种努力可能会产生误导

# 墨西哥与拉丁美洲国家的相互关系

我从约翰霍普金斯大学新冠肺炎数据库中提取了新冠肺炎每日新增病例和拉丁美洲死亡病例。我使用线性回归来分别检验墨西哥和拉丁美洲国家之间的相关性。相关性最强的国家是巴西。

![](img/162dd8ff744363599d4be9dac812b079.png)

墨西哥和巴西之间的数据关系(Diego Hurtado)

***有趣的是，这两个国家由一个左翼国家执政。***

# 强占

流行病学家和健康经济学家 **Eric Feigl-Ding** 将墨西哥的冠状病毒阳性病例与纽约、马德里和伦巴第等城市的病例进行了比较，并警告说，墨西哥将经历的新冠肺炎检测阳性百分比是前所未有的。

这是因为半数以上的检测对病毒呈阳性。

![](img/9bbc84a1ab10debd33951fee2c9697d6.png)

德雷克丁。(2020 年 6 月 21 日)

> “我为墨西哥哭泣。超过 50%是阳性的百分比，超过一半的测试者是阳性的，”这位科学家在推特上谈到冠状病毒病例时说。

![](img/a484900080f6bbae3449208eb655f8a1.png)

德雷克丁。(2020 年 6 月 21 日)

> 墨西哥人均病例和死亡率与美国的对比。在墨西哥很明显诊断不足，治疗也很差。56%的阳性率反映了这一点。所以是的，该死的，我们需要更多的测试。

![](img/78e57f54e046bba270c1e0a3bce20205.png)

# 催单

1918 年的疫情流感是近年来最严重的疫情。它是由带有禽类基因的 H1N1 病毒引起的。尽管对病毒的起源没有普遍的共识，但它在 1918-1919 年间在世界范围内传播。

全世界死亡人数估计至少为 5000 万，其中约 675，000 例发生在美国。

5 岁以下、20-40 岁和 65 岁及以上人群的死亡率较高。健康人群的高死亡率，包括 20-40 岁年龄组的人群，是疫情的一个独特特征。

![](img/d06b9efd61f25dfb8e72ee841293bb7c.png)

流感疫情图表(Reeve 003143)，国家健康和医学博物馆

疫情流感| 1918 年和 1919 年期间美洲和欧洲的死亡率。图表:“疫情流感。1918 年和 1919 年间美国和欧洲的死亡率每周因各种原因造成的死亡，以每年每 1000 人的死亡率表示。包括代表纽约、伦敦、巴黎和柏林的线条。

> 我们到了。

![](img/09cecc72404ebaedf673fe74ba5f9fb0.png)

流感疫情图表(Reeve 003143)，国家健康和医学博物馆

**结论:**

这篇文章详细分析了新冠肺炎是如何影响墨西哥和世界的，以及由此得出的见解如何用于下游分析。这些图表还可以应用于其他场景，以推断关键的数据洞察力。

我希望你们都喜欢这篇文章。所有的图表都是使用 [Plotly](https://plot.ly/graphing-libraries/) 创建的。Plotly 是一个非常棒的可视化库，用于构建交互式的图形。他们有 Python 和 JavaScript 的图形库。

领英:【https://www.linkedin.com/in/diego-gustavo-hurtado-olivares/ 

> ***编者按:*** [*走向数据科学*](http://towardsdatascience.com/) *是一份以数据科学和机器学习研究为主的中型刊物。我们不是健康专家或流行病学家，本文的观点不应被解释为专业建议。想了解更多关于疫情冠状病毒的信息，可以点击* [*这里*](https://www.who.int/emergencies/diseases/novel-coronavirus-2019/situation-reports) *。*

# 参考

[1]徐，郑，石，李，王，杨，张，黄，李，张，陈，… &泰，杨(2020).急性呼吸窘迫综合征相关新冠肺炎的病理表现。*柳叶刀呼吸内科*， *8* (4)，420–422。

[2]m .卡斯塞拉、m .拉杰尼克、m .科莫、a .杜勒博恩、S. C .、r .迪那不勒斯(2020 年)。冠状病毒的特征、评估和治疗(新冠肺炎)。在 *Statpearls【互联网】*。StatPearls 出版社。

[3]世界卫生组织。(2020).冠状病毒疾病 2019(新冠肺炎):情况报告，72。

[4]“世界上最大的贫民窟:达拉维、基贝拉、Khayelitsha & Neza。”*人类家园 GB* ，2018 年 9 月 7 日，[www . habitatforhumanity . org . uk/blog/2017/12/the-worlds-larvi-kibera-khayelitsha-neza/。](http://www.habitatforhumanity.org.uk/blog/2017/12/the-worlds-largest-slums-dharavi-kibera-khayelitsha-neza/.)

[5]“拥有世界上最大贫民窟的 8 个城市。”*美国新闻&世界报道*，美国新闻&世界报道，[www . us News . com/News/cities/articles/2019-09-04/the-worlds-largest-贫民窟。](http://www.usnews.com/news/cities/articles/2019-09-04/the-worlds-largest-slums.)

[6]墨西哥。(未注明)。从 https://ddi.sutd.edu.sg/portfolio/items/448588[取回](https://ddi.sutd.edu.sg/portfolio/items/448588)

[7]美国和欧洲 1918-1919 年流感死亡率图表|奥的斯历史档案国家健康和医学博物馆(http://www . Flickr . com/photos/medical Museum/5857153474/in/set-72157614214049255)，由 CC-BY-NC-SA(http://creativecommons.org/licenses/by-nc-sa/2.0/uk/)授权

[8]德雷克丁。(2020 年 6 月 21 日)。神圣的 moly-我为墨西哥哭泣。[https://twitter.com/DrEricDing/status/1274816278197342209](https://twitter.com/DrEricDing/status/1274816278197342209)