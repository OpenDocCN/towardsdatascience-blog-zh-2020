# 新冠肺炎时期时装业股票的表现和弹性

> 原文：<https://towardsdatascience.com/milan-paris-fashion-industry-stock-performance-resilience-during-covid-19-49502ec040fe?source=collection_archive---------32----------------------->

## 商业智能

## 探索米兰和巴黎证券交易所的趋势

![](img/225214f0695dd8ba360228e8754c98f6.png)

图片由[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4880802)的 Gerd Altmann 提供

世界正在遭受一个看不见的敌人的袭击，新冠肺炎已经永远改变了我们的生活，我们被完全封锁，我们不得不对我们的生活方式和业务做出重大改变，病毒的影响正在蔓延，扰乱了米兰和世界其他地方的日常生活，切断了购物，扰乱了全球供应链，房地产市场，威胁着经济，无论几周前可能存在的危机管理应对指南现在似乎来自另一个时代，这种情况迫使每个人实时发展和调整他们的策略和创造力。

根据[麦肯锡&公司](https://www.mckinsey.com/industries/retail/our-insights/a-perspective-for-the-luxury-goods-industry-during-and-after-coronavirus)发布的一份报告，全球超过 40%的奢侈品生产发生在意大利，所有的意大利工厂已经暂时关闭。

因为股票市场不得不自己处理一些重大的变化。在这个项目中，我们将构建一个 Python 脚本来分析新冠肺炎对米兰和巴黎证券交易所股票价格的影响。我们将探索市场上一些最大的时尚公司的表现，看看它们在新冠肺炎风暴中表现得好坏。我们将把意大利疫情爆发前的股票价格作为参考，并与几周后至 4 月 21 日的相同股票价格进行比较。我们将通过进行不同的分析和计算多种股票的每日价格变化来做到这一点。之后，我们将尝试探索股票价格与意大利每日新增冠状病毒病例数之间的关系。

接受调查的公司有:

1.  AEF。MI ( [Aeffe](https://www.linkedin.com/company/aeffe-s-p-a-/) 米兰证券交易所)
2.  MONC。MI(米兰证券交易所)
3.  公元前。米([布鲁内洛·库奇内利](https://www.linkedin.com/company/brunello-cucinelli-s-p-a-/about/)在米兰证券交易所上市)
4.  SFER。米([在米兰证券交易所](https://www.linkedin.com/company/salvatore-ferragamo/))
5.  托德。米( [Tod's](https://www.linkedin.com/company/tod's/) 集团在米兰证券交易所上市)
6.  MC。PA ( [LVMH](https://www.linkedin.com/company/lvmh/) 集团在巴黎证券交易所上市)
7.  CDI。PA ( [克里斯汀·迪奥](https://www.linkedin.com/company/christian-dior-couture/)在巴黎证券交易所上市)
8.  KER。PA ( [开云](https://www.linkedin.com/company/kering/)集团在巴黎证券交易所上市)

*注:要查看代码并接触数据集，请访问我在*[*GitHub*](https://github.com/YasserElsedawy/italy-france-covid19-stocks-analysis/blob/master/README.md)*上的个人资料，如果您有兴趣探索更多项目，请查看我的第一个项目关于* [*机器学习*](https://www.linkedin.com/pulse/machine-learning-model-forecast-european-cities-demand-elsedawy/) *，第二个项目关于* [*房地产市场新冠肺炎的颠覆。*](https://www.linkedin.com/pulse/real-estate-market-milan-disrupted-due-covid-19-kijiji-elsedawy/)

让我们开始，看看我们的数据集。我们选择调查接近的价格。

调整后的收盘价准确地反映了扣除任何公司行为后的股票价值。它被认为是该股票的真实价格，经常在检查历史回报或对历史回报进行详细分析时使用。

请注意，由于雅虎财经有时会面临网络问题，可能会遗漏一些日子，如一年的第一天。

![](img/67c3fb10bd79cb3c0f03d4be5d2b1191.png)

*   我们要做的第一件事是试图调查整个时间段内每家公司股票的最高和最低调整收盘价。我们将使用一个柱状图来显示数据，我们还将使用一个对数标度来表示可调收盘价格，从而将这些值标准化为一个统一的标度。通常通过数据规范化，数据集内的信息可以以可视化和分析的方式进行格式化。

![](img/90b37f1878f9e1567e8a7c3dca335366.png)

在上面的表示中，股票可以分为两组:可靠的股票和不稳定的股票。可靠的股票是指在指定的时间段内，最小调整收盘价和最大调整收盘价之间没有太大差异的股票。例如:开云、LVMH、克里斯汀·迪奥、Moncler。然而，不稳定的股票是那些在最低和最高收盘价之间变化最大的股票。例如:Aeffe

*   现在让我们试着把数据分成两部分，爆发前和爆发后，以更深入地研究趋势。

![](img/866aa8154121b3cfbda0016dbd81285c.png)

现在很清楚的是，一旦新冠肺炎风暴来袭，大多数新冠肺炎时代之前的股票收盘价的波动性都会发生变化。

*   在接下来的分析中，我们将尝试获取回报。回报被定义为股票价格随时间的变化，可以用价格变化或百分比变化来表示。正回报代表利润，而负回报标志着亏损。我们将使用 Python 库中的一个内置函数来完成这项工作。

请注意，分析中的第一天(30–12–2019)被删除，因为我们测量的是第二天(02–01–2020)的回报

![](img/cad82561e8f172b0e443c9046e2c5f85.png)

现在让我们探索最小和最大回报日期。

![](img/78e726ba1125c6b4acca6c8a8d59708c.png)

有趣的是，这些股票中的大多数在 2020 年 3 月 12 日达到了最低回报。在那一天，美国宣布全国进入紧急状态，而意大利在之前的四天里每天的死亡人数都是世界上最高的(包括高峰期的中国)。

![](img/19425b7df27b30d7d0b3e1bd6bd4d6e8.png)

所有公司的最高回报率也是在 3 月份。有趣的是，在 3 月 24 日 [Trump 宣布美国将“很快开始营业”](https://www.wsj.com/articles/trump-hopes-to-have-u-s-reopened-by-easter-despite-health-experts-guidance-11585073462)。

现在，我们将尝试把收益分成两个数据集，新冠肺炎前后，然后我们将对股票收益应用标准差。

在接下来的图表中，如果股票出现在图表的右端，它们被认为是不稳定的，具有很大的标准差，而如果它们出现在左边，它们应该被认为是可靠的。

![](img/92ce8ea3a23f65b4e9c0e373081afd41.png)![](img/11a7b7d125f8ec006e97ee2402b50fa1.png)

很明显，克里斯汀·迪奥(Christian Dior)受到的影响最大，由于疫情，它从最可靠的股票变成了最不稳定的股票。然而，Brunello Cucinelli (BC)在此期间取得了最大的进步，反过来，更积极的举措也发生在 Moncler，Kering 身上，而 LVMH 保持不变。

为了详细说明新冠肺炎对每只股票价格的影响，我们将每只股票的每日价格除以参考行，在我们的示例中，参考行是 2 月 21 日(注意第一行)。这将使我们对他们每个人的表现有更清楚的了解。

![](img/4c802a77e742bc9a1733090a7e9ebcde.png)![](img/b3e89087438c17866ba35c4eb93e077c.png)

我们可以很容易地看到新冠肺炎对股票价格的影响。例如，我们看到像 Aeffe 这样的公司在 3 月 13 日的收盘价比 2 月 21 日的收盘价低 44%。

同样，Brunello Cucinelli 的股价折价 19 %, Moncler 的折价 21%,表现优于路威酩轩集团和开云集团。

现在我们有数据支持的证据表明新冠肺炎对股票价格有巨大的影响。但是之前的时期呢？那是病毒第一次从中国武汉传播到意大利的时候？

让我们重新创建从 12 月 30 日到 2 月 21 日的相同分析。

![](img/89b01c64f24c5126e87d93d5f30ba4ae.png)

除了 Aeffe，似乎对于大多数股票来说，病毒的爆发对股票价格的影响最小，因为欧洲几乎没有病例，而中国有数千例。同样值得注意的是，Brunello Cucinelli 取得了巨大的增长，这可能与 1 月 7 日他们发布的去年初步数据有关，净收入增长了 9.9%[，这使得该公司的股票在意大利证券交易所上涨了 7.6%。](https://wwd.com/business-news/financial/brunello-cucinelli-preliminary-sales-gain-1203413780/)

然而，新冠肺炎停止了布鲁内洛·库奇内利的增长，因为他们最近报告第一季度收入下降。

现在让我们一起想象整个时期。

![](img/65a3f64d6eaea0fb733e37971dddae41.png)

令人惊讶的是，在面对新冠肺炎风暴时，Moncler 的股价表现令人印象深刻。

现在是时候将新冠肺炎的数据纳入我们的分析了。我们将添加 2 月 24 日至 4 月 21 日期间意大利每日新增感染病例的相关数据(因为没有关于 21 日、22 日、23 日每日数字的数据)。

![](img/cbf62f715a7d31d6b7d13de05797e962.png)

我们可以清楚地看到这样的趋势，在大多数情况下，新冠肺炎案例的数量增加了，股票价格降低了。

最后，我们可以用热图来表示这种相关性。

![](img/762804a071b89137a5937f15cf386380.png)

在这张热图中，如果我们检查最后一行，我们可以看到，有些公司，如克里斯汀·迪奥和布鲁内洛·库奇内利，这种关系强于 70%，这意味着当每日感染病例数增加时，这些公司的股价会下降。

# 结论

由于新冠肺炎，股票市场波动很大，由于许多公司对其风险敞口或供应链的可见性有限，这种影响更大，因此，困难时期可能是你自身实力的反映，今天的公司明白他们对客户、员工和利益相关者负有责任，要在财务和社会方面取得成功。

许多品牌以大胆的方式做出回应，让利益相关者有理由相信他们将度过这场危机，共同前进。尽管 3 月 13 日大多数股票价格都出现了大幅下跌，但迄今为止，来自 [Moncler](https://www.linkedin.com/company/moncler/) 股价的数据显示了出色的表现，这表明业务和品牌的弹性和灵活性，其次是巨头 [LVMH](https://www.linkedin.com/company/lvmh/) 和[开云](https://www.linkedin.com/company/kering/)集团。

> “目标感”:“你公司的战略必须阐明实现财务业绩的途径。然而，为了保持这种表现，你还必须了解你的业务对社会的影响，以及广泛的结构性趋势——从工资增长缓慢到自动化程度提高到气候变化——对你的增长潜力的影响。”—贝莱德首席执行官拉里·芬克在 2018 年致首席执行官的年度信中

# 参考

*   [3 月 12 日消息](https://www.google.com/search?rlz=1C5CHFA_enIT873IT873&biw=1440&bih=740&tbs=cdr%3A1%2Ccd_min%3A3%2F12%2F2020%2Ccd_max%3A3%2F13%2F2020&tbm=nws&sxsrf=ALeKk03mAJpORAIx4uMBksZrogjBCg5KuQ%3A1587643967896&ei=P4ahXr-SNoOAk74Pl-Kr2A0&q=trump&oq=trump&gs_l=psy-ab.3...9875.10602.0.10841.5.5.0.0.0.0.157.532.0j4.4.0....0...1c.1.64.psy-ab..1.0.0....0.cLPKrc0XNk4)
*   [3 月 24 日消息](https://www.google.com/search?q=trump&rlz=1C5CHFA_enIT873IT873&biw=1440&bih=740&sxsrf=ALeKk02-MbIOqRDrk4VjwDL_Vu6zQLoxRw%3A1587643979535&source=lnt&tbs=cdr%3A1%2Ccd_min%3A3%2F24%2F2020%2Ccd_max%3A3%2F25%2F2020&tbm=nws)
*   [Github 科技股分析](https://github.com/atreyid/COVID_19_Tech_Stocks_Analysis/blob/master/index.ipynb)

***编者注:*** [*走向数据科学*](http://towardsdatascience.com/) *是一份以数据科学和机器学习研究为主的中型刊物。我们不是健康专家或流行病学家，本文的观点不应被解释为专业建议。想了解更多关于疫情冠状病毒的信息，可以点击* [*这里*](https://www.who.int/emergencies/diseases/novel-coronavirus-2019/situation-reports) *。*