# Kompresso 能得到 9 巴压力的浓缩咖啡吗？

> 原文：<https://towardsdatascience.com/can-kompresso-get-9-bars-of-pressure-for-espresso-9aeff301b943?source=collection_archive---------35----------------------->

## 用一个简单的实验探索 Kompresso

几年来，我一直在努力寻找[便携式浓缩咖啡](https://towardsdatascience.com/overthinking-life/portable-espresso-a-guide-5fb32185621)的解决方案。到目前为止，我的迭代已经到达 [Kompresso](http://cafflano.com/product_kompresso.php?TM=2) 。最大的卖点是在旅途中获得杠杆机体验。Kompresso 重量轻，相对较小的外形，可以产生正确的压力，或者可以吗？

![](img/901dd14aeedc94c5f2180fbff1237920.png)

[Cafflano 声称 Kompresso](https://www.kickstarter.com/projects/cafflano/cafflano-kompresso-a-portable-authentic-espresso-m) 可以达到 9 巴的压力，因为他们的柱塞直径比圆盘小，这意味着根据帕斯卡原理，柱塞上的力相当于液体通过咖啡的压力。推动小柱塞更容易获得所需的压力。

流体力学中的帕斯卡原理或帕斯卡定律指出[“在封闭容器中静止的流体中，一部分的压力变化无损耗地传递到流体的每一部分和容器壁。”](https://www.britannica.com/science/Pascals-principle)因此，如果你有一根管子，一边有一个小开口，另一边有一个大开口，对一边施加一定的力，就会在另一边产生相等的力。

![](img/c138c63b0df79d6c8d9e1d529be874ef.png)

这里有一个很好的解释:https://www.britannica.com/science/Pascals-principle

你真的能用这个小机器达到 9 巴的压力吗？9 巴是 130 磅/英寸，活塞是 6 厘米或 0.93 英寸。所以 9 巴的压力意味着在活塞上施加 120 磅的力。

![](img/85003dcdc1338a0a87d11f9fd6aa077b.png)

我开始用一个简单的模拟体重秤实验来测试 9 巴的说法。

**主要警告**:我不认为 9 格的浓缩咖啡是必要的，我通常在冲泡过程中使用较少。我还做了长时间的预输注和压力脉冲，所以我对 Kompresso 很满意。这是一个很棒的旅行浓缩咖啡机，我真的很喜欢它。我做这个实验的唯一原因是为了更好地理解他们通过数据得出的结论。在 espresso 社区已经有了关于这一说法的讨论，但是到目前为止，还没有人用数据来证明这一点。

# 实验

我把 Kompresso 放在模拟秤上，然后按下顶部的活塞。这与 Kompresso 的典型用法不同，因为通常是从底部向上拉的同时向下推。然后，我使用视频记录整个镜头的输出重量，并使用视频记录面部后的数据。改进这个实验的唯一方法是使用应变仪。

![](img/5fc9b91d6171949004a076c6b64419dd.png)![](img/eef2eb59e39117f02633956b4a9ea690.png)![](img/d63065c48fa396eb464bd5d5e4d3c5dd.png)![](img/a743722b7e470c5412115eb9f0c53451.png)

我做了很长时间的预注入，在 30 秒时，我注入了足够的水来浸泡冰球。然后 60s 开始输液。我稍微休息了一下，因为我在 90 左右收集了我要喝的那部分，然后我尽可能舒服地用力。

![](img/d7dd8147f865d820de5ae2581b571779.png)

最多的时候，我只能打 5 小节，这很不舒服。设置是颤抖的，它肯定觉得我可以设备。很难保持这种压力，保持 30 秒的输液时间会很不舒服。

我无法让 Kompresso 达到 9 巴的压力，我不建议试图让它超过 5 巴，因为它似乎不稳定。我可以坐在机器上，但我不认为 9 巴是好的浓缩咖啡所必需的，我很少承受那种压力。然而，Kompresso 仍然是最好的浓缩咖啡旅行伴侣，我相信它本质上是一个便携式调平机。

这个实验并没有毫无疑问地证明可以达到 9 格，但由于不舒服，我在以前的拍摄中没有比这更用力。

如果你愿意，可以在 Twitter 和 YouTube 上关注我，我会在那里发布不同机器上的浓缩咖啡视频和浓缩咖啡相关的东西。你也可以在 [LinkedIn](https://www.linkedin.com/in/robert-mckeon-aloe-01581595?source=post_page---------------------------) 上找到我。

# 我的进一步阅读:

[咖啡的形状](/the-shape-of-coffee-fa87d3a67752)

[搅拌还是旋转:更好的浓缩咖啡体验](https://towardsdatascience.com/p/8cf623ea27ef)

[香辣浓缩咖啡:热磨，冷捣以获得更好的咖啡](/spicy-espresso-grind-hot-tamp-cold-36bb547211ef)

[断续浓缩咖啡:提升浓缩咖啡](https://towardsdatascience.com/overthinking-life/staccato-espresso-leveling-up-espresso-70b68144f94)

[用纸质过滤器改进浓缩咖啡](/the-impact-of-paper-filters-on-espresso-cfaf6e047456)

[浓缩咖啡中咖啡溶解度的初步研究](/coffee-solubility-in-espresso-an-initial-study-88f78a432e2c)

[断奏捣固:不用筛子改进浓缩咖啡](/staccato-tamping-improving-espresso-without-a-sifter-b22de5db28f6)

[浓缩咖啡模拟:计算机模型的第一步](https://towardsdatascience.com/@rmckeon/espresso-simulation-first-steps-in-computer-models-56e06fc9a13c)

[压力脉动带来更好的浓缩咖啡](/pressure-pulsing-for-better-espresso-62f09362211d)

[咖啡数据表](https://towardsdatascience.com/@rmckeon/coffee-data-sheet-d95fd241e7f6)

[工匠咖啡价格过高](https://towardsdatascience.com/overthinking-life/artisan-coffee-is-overpriced-81410a429aaa)

[被盗浓缩咖啡机的故事](https://towardsdatascience.com/overthinking-life/the-tale-of-a-stolen-espresso-machine-6cc24d2d21a3)

[浓缩咖啡过滤器分析](/espresso-filters-an-analysis-7672899ce4c0)

[便携式浓缩咖啡:指南](https://towardsdatascience.com/overthinking-life/portable-espresso-a-guide-5fb32185621)

[克鲁夫筛:分析](https://towardsdatascience.com/overthinking-life/kruve-coffee-sifter-an-analysis-c6bd4f843124)