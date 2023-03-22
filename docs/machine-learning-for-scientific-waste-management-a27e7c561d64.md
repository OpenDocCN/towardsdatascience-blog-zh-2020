# 科学废物管理的机器学习

> 原文：<https://towardsdatascience.com/machine-learning-for-scientific-waste-management-a27e7c561d64?source=collection_archive---------55----------------------->

![](img/7724ac8ab05df5aec57d1ac04beacc78.png)

喀拉拉邦——上帝自己的国家(照片由 [Avin CP](https://unsplash.com/@avincp?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](/s/photos/kerala?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄)

## 顶点案例研究

## 了解 ML 如何帮助防止气候变化和保护社区健康

T 他的案例研究是通过 Coursera 完成的 IBM 应用数据科学顶点项目的一部分。本研究的目的是为印度南部喀拉拉邦的高知市列举合适的城市垃圾管理地点。我选择高知市进行这个案例研究，不仅是因为它是我的故乡，而且高知市过去和现在都在努力有效地管理城市垃圾。

# 高知市——概述

![](img/271b122c3db8d58333ea605cc10f5d30.png)

高知湖(来源:照片由 [Unsplash](/s/photos/kochi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的 [gaurav kumar](https://unsplash.com/@sd4ssm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄)

高知市(有时称为科钦市)是一个港口城市，至少有 600 年的贸易历史，主要是与中国、欧洲和阿拉伯商人，这使该市成为喀拉拉邦的金融中心。举几个例子，科钦证券交易所——喀拉拉邦唯一的证券交易所，国际集装箱转运站，造船工业，信息园——政府经营的信息技术园，露露购物中心——亚洲最大的购物中心证明了高知市的经济重要性[1]。高知市不仅是一个理想的商业场所，而且还提供负担得起的住房选择。高知也是一个旅游景点，吸引了来自世界各地的游客。所以基本上高知市都是限量版的全合一套餐。

# 废物管理危机

![](img/1bcb87cc28826048d0d79d0e7596f231.png)

高知市雅鲁藏布江废物管理设施(来源:[5])

高知市每天产生约 380 吨垃圾，其中 150 吨可生物降解，其余为塑料垃圾[2]。目前，城市垃圾被送往雅鲁藏布江垃圾管理设施进行处理。从这个工厂建立之初，公众、环保主义者的抱怨和担忧就成了报纸上反对工厂选址和工厂选址方式的头条新闻。许多居民被政府带着补偿金迁移了。河水污染，因为该工厂与两条淡水河流接壤，而且由于选择废物管理工厂的位置时采用了不科学的方法，工厂的生命遭到了破坏。目前的工厂位置已经让雅鲁藏布江居民的生活变得悲惨——来自食物垃圾的令人作呕的恶臭和令人讨厌的家蝇和蚊子。此外，用于处理的输入废物是可生物降解的和塑料废物的混合物，并且可生物降解的材料在户外分解，因为据报道在工厂中的处理过程缓慢，这导致另一个恐怖——气候变化，因为可生物降解的材料正在分解，从而释放出甲烷，一种温室气体。此外，巨大的火灾越来越频繁，有毒气体扩散到整个城市，扰乱城市生活数日[3]。

![](img/721ec9b8cebbe3eb42c4ccdb0b4d811e.png)

雅鲁藏布江发生火灾(来源:[《印度时报》](https://timesofindia.indiatimes.com/city/kochi/kochi-brahmapuram-waste-plant-continues-to-burn-parts-of-city-engulfed-in-smoke/articleshow/68121782.cms))

由于这一事实，市政当局已经暂停从家庭收集垃圾，直到找到合适的解决方案。喀拉拉邦污染控制委员会进行了一项关于城市垃圾管理的研究，报告称垃圾发电不是解决高知市垃圾管理危机的经济有效的解决方案[4]。由于高知正在增长，每天产生的垃圾也在增加。布拉马普拉姆设施目前超负荷运转，因为除了商业和住宅废物之外，还必须容纳医疗废物，这使危机进一步复杂化。即使找到了管理城市垃圾的理想解决方案，工厂目前的位置对地下水储备以及流经的两条河流的质量构成了严重威胁。目前的工厂位置是一场灾难，将影响高知居民的生活以及高知的经济健康。

# 案例研究路线图

有几份报告和关切涉及在雅鲁藏布江最终确定当前城市废物管理设施的位置时采用的不科学方法。[5]

![](img/e4a4f77ac4cd9ade10aafdaab6e6afb5.png)

雅鲁藏布江的人们在工厂建成后逃离家园(来源:参考文献[2])

确定当前工厂位置的谬误的一个重要证据是它靠近高知自己的 IT 园区 InfoPark，离工厂只有 5 公里。另一个证据是，该地区是沼泽，地下水渗透废物分解的化学副产品的风险。所有这些问题促使我寻找一个理想的厂址来建立一个新的废物处理厂——一个不会损害自然和社会的工厂。然后下一个问题来了，怎么做？

这个想法是找到世界上最干净的城市，并在选择的城市中模拟该城市的垃圾管理设施的位置/邻近区域。在这里，我使用的逻辑是——如果有一个清洁的城市，那么它就有一个高效的废物管理设施，背后将有强大和成功的管理实践，这些管理实践将受到强大的环保和社会承诺的规则和政策的约束。因此，模拟这种废物管理设施的位置将等同于模拟那些非常成功的规则和政策。然后，我去打猎，发现加拿大的卡尔加里市是世界上最干净的城市之一，我选择它是因为卡尔加里市一直在世界上最干净的 10 个城市中占有一席之地。

![](img/f3e4692c52464409853935733f3f0e4c.png)

卡尔加里市(来源:drpukka/Instagram)

卡尔加里市有两个商业废物管理设施，我选择了其中的一个，只是通过方便取样——东卡尔加里设施。然后，是时候用数学方法来表示东卡尔加里工厂了。这个问题的答案是，Foursquare API。它为全球数百万个场馆提供位置数据。使用 Foursquare API，获得了东卡尔加里设施 5000 米范围内的场馆，并使用场馆类别生成了东卡尔加里设施的向量。现在，要模拟的参考位置已准备就绪，是时候获取感兴趣的城市高知市所在的 Ernakulam 区的社区列表了。这些社区中的一些将被选为建立新的废物管理工厂的最佳选择。使用 BeautifulSoup 的 Webscraping 提取 Ernakulam 的所有邻居列表，使用 Geopy 提取所有这些邻居的地理位置数据。然后，就像东卡尔加里设施一样，Ernakulam 的每个街区都被表示为一个场地类别向量。然后是最后的预处理阶段——确保东卡尔加里社区和 Ernakulam 社区通过相同的场所类别来表示。用外行人的话来说，我确保所有的社区都在相同的基础上进行比较！

预处理后，东卡尔加里设施点矢量被合并到包含 Ernakulam 邻域的数据帧中，这就是我们的聚类数据集。

下一步是对邻域进行 K-均值聚类，并分割出不同的邻域，但在此之前，要进行肘形曲线分析，以确定最佳聚类数。有了最佳的聚类数，K-Means 被执行，并且我们的理想邻域(即东卡尔加里设施的邻域)被确定，并且 Ernakulam 的邻域属于相同的聚类。邻里模拟的最初部分已经结束。考虑到雅鲁藏布江现有废物管理设施的缺陷，我们对聚类后获得的 Ernakulam 社区列表进行了第二级细化。

# 最后结局

网络搜集和地理定位数据采集产生了 45 个基于邮政编码的 Ernakulam 社区。预处理阶段将邻域减少到 32 个。这样聚类数据集就准备好了，肘曲线分析表明聚类的最佳数量是 4。

![](img/ce76c2cba7539d66466a84dee676882f.png)

获取最佳聚类数的肘形曲线分析

K-means 聚类在下面的可视化中进行了描述:

![](img/e9dac8af4bd5160148ab781d7f21ee7f.png)

邻域聚类的可视化。蓝圈是高知中心，大头针标记是雅鲁藏布江废物管理设施

Ernakulam 有 16 个社区与东卡尔加里设施属于同一个集群。

但是，作为最后一步，考虑到雅鲁藏布江现有设施的一个重要缺点——靠近水体，我决定过滤掉获得的 Ernakulam 邻域。这一过滤过程进一步将选择的数量减少到 7 个(Palarivattom、Kaloor、Ernakulam North、Thammanam、Kadavanthra、Panampilly Nagar 和 Vyttila 是选择)，如下图所示:

![](img/1f50e5faca0c3d5475e164e18aa2ea6c.png)

# 结论

![](img/dd6ff39670b2b3ef8861526153ec5da6.png)

高知回水邮轮旅游([来源](https://www.shoreexcursions.asia/cochin-backwater-cruise-with-kerala-lunch/))

长期以来，高知市在废物管理方面正经历着一场重大危机，这项研究提出了 7 个适合使用机器学习科学地建立新工厂的地点。现在，城市管理人员可以评估这些建议，并从中找出最佳点，从而加快城市管理，减少决策失误。由于模拟了世界上最干净的城市之一的垃圾管理设施，城市管理者可以使用卡尔加里市的垃圾管理政策和规则，并根据高知市的要求进行一些调整。因此，所有这些决定将使废物管理过程更加有效，从而可以防止废物在设施中堆积，从而最大限度地减少由此产生的甲烷排放量——这是对联合国可持续发展目标(目标 11) [7]的一种贡献。使用这种方法可以缓解严重的公共健康问题，如由于雨水停滞导致的疟疾等流行病，以及由于工厂位置不科学导致的肺部或皮肤疾病。本研究未考虑地理、政治和社区行为差异，考虑这些变量后，结果可能会有所不同。未来可能的扩展包括:基于密度的空间聚类应用研究(DBSCAN)、环境热点或人口密度变量包含、不同地理定位数据供应商的使用。这个项目的另一个优点是，它可以适用于政府和企业的目的，以及无论州或国家。

# 参考

1.  [高知经济](https://en.wikipedia.org/wiki/Economy_of_Kochi)
2.  [布拉马普拉姆邦火灾频发](https://timesofindia.indiatimes.com/city/kochi/4th-major-fire-at-brahmapuram-in-two-months/articleshow/68118744.cms)
3.  [高知废物管理危机](https://www.thenewsminute.com/article/walk-through-ghost-village-brahmapuram-deserted-thanks-kochis-garbage-41040)
4.  [雅鲁藏布江的垃圾发电不划算](https://www.thehindu.com/news/cities/Kochi/waste-to-energy-plant-may-not-be-most-cost-effective-option/article27190210.ece)
5.  [公众对雅鲁藏布江电站的担忧](https://www.deccanchronicle.com/nation/in-other-news/011116/kochi-residents-say-no-to-waste-treatment-plant.html)
6.  [全球十大最干净城市](https://kenyaprime.com/top-10-cleanest-cities-in-the-world-2020-latest-ranking-things-you-need-to-know-about-the-cities-2/)
7.  [联合国可持续发展目标](https://sustainabledevelopment.un.org/?menu=1300)