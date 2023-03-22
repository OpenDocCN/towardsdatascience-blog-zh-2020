# 浓缩咖啡的咖啡渣新鲜度

> 原文：<https://towardsdatascience.com/coffee-grounds-freshness-for-espresso-78a447f84fcd?source=collection_archive---------29----------------------->

## 探索研磨和酿造之间的时间差

研磨后，你要等多久才能煮出浓缩咖啡？问一个咖啡师，任何一个咖啡师，我怀疑他们的回答是立即或者几分钟内。关于咖啡渣新鲜度的常识是，大约 30 分钟后，咖啡渣就变味了。即使储存了，大家都知道咖啡渣还是会变味(新鲜的时间长短没有明确定义)。总是这样吗？

![](img/5289862ce4c0b9a8d6cd07b6484dced9.png)

我的断奏经验让我相信，关于咖啡渣新鲜度的普遍想法是不正确的。我找过支持数据，并不多，可能是因为它看起来太明显了。因此，我将分享我的经验和数据，这些数据表明，如果储存在密封的容器中，咖啡渣至少可以使用几天而不会失去味道。在过去的几个月里，我一直把我的辣饮料放在冰箱里，但在我定期冷藏咖啡渣之前，我也会回头看看。

![](img/03dc51d35a3b2279424938b54959463e.png)

# 以往的经验

有一段时间，我磨新鲜，但当我开始开发断奏浓缩咖啡镜头时，这种情况发生了变化。每一杯咖啡的研磨和筛选都是乏味的，所以我会研磨足够多的咖啡来喝几杯，我会筛选并把咖啡储存在密封的容器里。

![](img/038f4025100e958e46b794d8ec4f6f59.png)

我发现这种容器的好处是，即使我不打算筛选咖啡，我也会研磨咖啡并储存在里面。为了让我的过程更有效率，我会在早上研磨，一小时后，我会在上班的路上筛选。研磨和酿造之间的时间开始延长。

说到某些事情，我也很懒，有时我会把磨好的东西放在密封的容器里好几天。我没有注意到品味的下降。

# 数据收集

我开始记录研磨日期和时间只是当我开始记录研磨时豆的温度。我在 grind 用[冷热豆](/spicy-espresso-grind-hot-tamp-cold-36bb547211ef)做实验，我想我会跟踪它。我通常会一次磨四杯咖啡，这需要 2 到 4 天，取决于我喝咖啡的时间。

我开始查看这些数据，看看能否告诉我味道或提取率是否有任何变化。当我看不到变化时(如下图所示)，我做了几个更长的实验，研磨 8 杯咖啡，喝一周。我还查阅了以前的数据，并根据筛过的粉末标注了估计的研磨日期，这些数据是我单独记录的。

这些镜头中的大部分都是使用第一次破裂后 1 到 1:30 分钟的家庭烘焙咖啡豆。我在 Rok 研磨机或 Lime 研磨机上研磨咖啡豆，这些数据来自于在 [Kim Express](https://medium.com/overthinking-life/kim-express-reborn-64472f5ded1e) 上拍摄的照片。此外，如果有人认为有一个更好的研磨机更重要，我会向他们推荐这项工作，我用了一个[叶片研磨机和一个筛子](https://medium.com/overthinking-life/a-blade-grinder-for-great-espresso-cf4f5a561ba6)来制作一杯好的浓缩咖啡，这表明筛子平衡了研磨机的领域。

# 绩效指标

我使用了两个指标来评估镜头之间的差异:[最终得分](https://link.medium.com/uzbzVt7Db7)和[咖啡萃取](https://link.medium.com/EhlakB9Db7)。

最终得分是 7 个指标(强烈、浓郁、糖浆、甜味、酸味、苦味和余味)记分卡的平均值。当然，这些分数是主观的，但它们符合我的口味，帮助我提高了我的拍摄水平。分数有一些变化。我的目标是保持每个指标的一致性，但有时粒度很难，会影响最终得分。

使用折射仪测量总溶解固体(TDS ),该数字用于确定提取到杯中的咖啡的百分比，并结合一杯咖啡的输出重量和咖啡的输入重量，称为提取率(EY)。

# 数据结果

首先，看数据，我把所有的数据都标绘出来，我没有注意到任何东西。这几天，口味(最终得分)和 EY 肯定有变化，但我没有看到任何模式。我会注意到，由于[辛辣研磨](/spicy-espresso-grind-hot-tamp-cold-36bb547211ef)，我将大部分粉末储存在冰箱中，但我没有注意到在室温下储存一两天的粉末味道会变差。

![](img/06d9adf6592a93e6ce41294278726e49.png)

所以我通过烘烤来分解它，我把时间窗口缩短到最多 3 天。同样，每一次烘烤似乎都不会随着时间的推移而变质。一天之内会有一些变化，这与我正在进行的一些实验的变化有关，但是没有一个趋势。

![](img/e618ea0cb0b590b9141344c29c2f0ace.png)

我决定简化数据。我把它切成两半，因为一半的数据的研磨时间不到 1 天 4 小时。我想在一天之内或一天以上完成，但这不是一个均匀的切割。在这种分裂中，有一些差异，但没有一个分布差异具有统计学意义。

![](img/83c28e7ec8bf42c9798fac943a0f1956.png)

我通常会研磨足够四个 18g 的镜头，我想在比以前更长的时间内测试研磨。我做了两次烘烤，我看到了变化，但没有趋势。咖啡渣也储存在冰箱里，这可能有助于保持它们的味道新鲜。

![](img/24f8e31b93fecbae4a66949ce72c6cca.png)

## 拍摄时间

拍摄时间似乎也不受研磨时间的影响。在不同的烘烤中，拍摄时间保持相当一致，我没有看到任何趋势。

![](img/eb4cab6a47e0531d7489e0b839675f1a.png)

# 更多数据

我拉了 359 张照片，我有研磨时间和拍摄时间，但随着数据的财富而来的是试图理解它的困难。所以我先把它画成散点图。没有明显的趋势。

![](img/8ef2d751da533ee519dd96e2cfde7e5d.png)

然后，我绘制了箱线图，看起来最终得分或品味有所改善，但我怀疑这种改善是由于改善了拍摄参数。此外，分布非常广泛，因为镜头是跨不同的烘烤和技术。中值分数略有变化，但分布仍然非常相似。

![](img/76fd682c3ce79af91ee1c36b81f05581.png)

为了提取学习，我通过烘焙将数据标准化(Z-norm)。对于每一次烘烤，我计算了平均值和标准偏差(std)。然后，对于每一个度量，每一次烘焙，我通过以下等式得到标准化数据:X _ norm =(X–烘焙 _ 均值)/烘焙 _ 标准。如果由于研磨时间而出现趋势，则应该清楚地显示为归一化分布的变化。

![](img/7866bcd4ca1e0886dd65bec9273f1bb4.png)

作为散点图，似乎没有太大的变化，所以让我们看看下面的箱线图。对于常规拍摄，最终得分保持不变，而 EY 增加了一点点。

![](img/fb2fee995c7fb4f9f0a8a6026b6da298.png)

对于断奏镜头，没有任何指标的趋势。在这种情况下，0 表示平均分数，正值表示高于平均值，负值表示低于平均值。

![](img/bc3bc8f6e70808fed7b9d83fe65d8d69.png)

在怀疑研磨年龄并不像建议的那样重要后，我查阅了尽可能多的数据。我没有发现研磨年龄和多个指标(最终得分(味道)、EY 或拍摄时间)之间有任何积极或消极的趋势。研磨咖啡失去味道的主要警告最有可能是基于让它暴露在空气中一段时间，但至少从我的经验来看，研磨得更少并没有对我的浓缩咖啡体验产生负面影响。

如果你愿意，可以在 Twitter 和 YouTube 上关注我，我会在那里发布不同机器上的浓缩咖啡视频和浓缩咖啡相关的东西。你也可以在 [LinkedIn](https://www.linkedin.com/in/robert-mckeon-aloe-01581595?source=post_page---------------------------) 上找到我。

# 我的进一步阅读:

[解构咖啡:分割烘焙、研磨、分层以获得更好的浓缩咖啡](/deconstructed-coffee-split-roasting-grinding-and-layering-for-better-espresso-fd408c1ac535)

[浓缩咖啡的预浸:更好的浓缩咖啡的视觉提示](/pre-infusion-for-espresso-visual-cues-for-better-espresso-c23b2542152e)

[咖啡的形状](/the-shape-of-coffee-fa87d3a67752)

[搅拌还是旋转:更好的浓缩咖啡体验](https://towardsdatascience.com/p/8cf623ea27ef)

[香辣意式浓缩咖啡:热磨，冷捣以获得更好的咖啡](/spicy-espresso-grind-hot-tamp-cold-36bb547211ef)

[断续浓缩咖啡:提升浓缩咖啡](https://towardsdatascience.com/overthinking-life/staccato-espresso-leveling-up-espresso-70b68144f94)

[用纸质过滤器改进浓缩咖啡](/the-impact-of-paper-filters-on-espresso-cfaf6e047456)

[浓缩咖啡中咖啡的溶解度:初步研究](/coffee-solubility-in-espresso-an-initial-study-88f78a432e2c)

[断奏捣固:不用筛子改进浓缩咖啡](/staccato-tamping-improving-espresso-without-a-sifter-b22de5db28f6)

[浓缩咖啡模拟:计算机模型的第一步](https://towardsdatascience.com/@rmckeon/espresso-simulation-first-steps-in-computer-models-56e06fc9a13c)

[更好的浓缩咖啡压力脉动](/pressure-pulsing-for-better-espresso-62f09362211d)

[咖啡数据表](https://towardsdatascience.com/@rmckeon/coffee-data-sheet-d95fd241e7f6)

工匠咖啡价格过高

[被盗咖啡机的故事](https://towardsdatascience.com/overthinking-life/the-tale-of-a-stolen-espresso-machine-6cc24d2d21a3)

[浓缩咖啡过滤器分析](/espresso-filters-an-analysis-7672899ce4c0)

[便携式浓缩咖啡:指南](https://towardsdatascience.com/overthinking-life/portable-espresso-a-guide-5fb32185621)

克鲁夫筛:一项分析