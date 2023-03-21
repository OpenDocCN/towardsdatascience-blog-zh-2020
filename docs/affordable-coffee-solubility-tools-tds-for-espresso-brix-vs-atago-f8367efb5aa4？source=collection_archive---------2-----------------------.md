# 浓缩咖啡的经济咖啡溶解度工具(TDS ):白利糖度与 Atago

> 原文：<https://towardsdatascience.com/affordable-coffee-solubility-tools-tds-for-espresso-brix-vs-atago-f8367efb5aa4?source=collection_archive---------2----------------------->

## 通过一些数据收集，我发现一个便宜的白利糖度折射仪和一个更贵的 Atago 测量咖啡萃取物一样准确。

我犹豫要不要买 TDS 表，因为我不确定它是否值得。标准的 [VST 数字折射仪](https://store.vstapps.com/products/vst-lab-cof-esp-iii-refractometer)价格约为 700 美元，即使是价格较低的也要 100 美元。最终，我买了一个 20 美元的[白利糖度计](https://www.amazon.com/Refractometer-Scale-Specific-Hamh-Optics-Tools/dp/B076WZGMYT/ref=asc_df_B076WZGMYT/?tag=hyprod-20&linkCode=df0&hvadid=242010195246&hvpos=1o1&hvnetw=g&hvrand=10690488401580580868&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9032131&hvtargid=pla-420967809789&psc=1)，后来，我终于决定买一个更贵的(Atago)。我收集数据来帮助理解 [Atago](https://www.atago.net) 的工作情况，以及它与更便宜的工具相比如何。

长话短说:便宜的电表和数字电表一样精确，只是不太方便。

![](img/e0e10897093801e30b0fee465d88bcdb.png)

# 我的 TDS 历史

我不确定[总溶解固体(TDS)](https://fellowproducts.com/blogs/learn/the-beginner-s-guide-to-total-dissolved-solids-and-coffee) 是否与味道相关。这当然是一个很好的衡量标准，但我不喜欢这个价格。我发表了一篇关于[断续浓缩咖啡](https://medium.com/overthinking-life/staccato-espresso-leveling-up-espresso-70b68144f94)镜头的作品，主要的批评是我没有使用 TDS，而 TDS 是咖啡师世界的标准。相反，我使用一个包含 7 个指标的记分卡[(尖锐、浓郁、糖浆、甜味、酸味、苦味和余味)得出最终得分。这些分数当然是主观的，但它们符合我的口味，帮助我提高了我的投篮。](/coffee-data-sheet-d95fd241e7f6)

最终，我屈服了，我买了一个便宜的白利折光仪。在拍摄的时候，我在检查 [TDS 的过程中获得了很多乐趣，但是当然，有些人并不相信，因为这是通过目测完成的。所以我决定买一台 100 美元的折光仪，直到我看到评论很差，碰巧的是，我发现有人卖 200 美元的 Atago。通常情况下，一辆 Atago 要花费 300 美元或更多，而且在美国很难找到。](/coffee-solubility-in-espresso-an-initial-study-88f78a432e2c)

![](img/e7cecd5acbd4b08ab5f6144754cd897d.png)

将白利糖度转换为 TDS 可以使用一个简单的公式:

TDS = 0.85 *白利糖度

有些人认为这个度量标准太简单了，这个问题可以通过比较 Brix 和 Atago 来解决，如下所示。

![](img/ec6c2c48322cf888a294ae6f27051a1e.png)

# Atago 测试

![](img/28b69091fc160a860f079ba07eb23e54.png)

首先，我想测试 Atago 在镜头冷却时处理温度变化的能力。我从我的可靠的老 Odea Giro Superautomatic 中取出一张照片，因为我不担心高 TDS 照片。

我还想在测量 TDS 之前测试过滤样品。一些人声称你应该在测量 TDS 之前过滤样本，但是一项研究表明过滤器降低了 TDS。他们确实发现过滤后的样品在测量中的可变性更小。我也想做科学家做的事情，看看我是否能快速验证他们的结果，而不用购买昂贵的 VST 注射器过滤器。为了测试过滤，我使用了 Aeropress 滤纸。

**这是我的设置:**

![](img/d65113180bdcb7d93779dd11fbd9fba7.png)![](img/7b95e534bc4b26f517fd4e2388913a30.png)![](img/5fd49c12c9026e627daf1bd33cdf7fb9.png)

1)初始设置。2)正在使用的设置。3)过滤后的空气压力纸过滤器

起初，我测试了单个样本并读取了多个读数，这似乎表明差异很大，但接近 Atago 规范的+/- 0.03 TDS。

![](img/9ccb848928bc2fa3fcb41d887709e7d7.png)

然后我取了 10 个未经过滤和经过过滤的样本，结果似乎遵循了[的想法，即经过过滤的样本提供了更多可重复的测量](http://socraticcoffee.com/2016/02/examining-the-impact-of-particulate-matter-in-refractometry-for-coffee-assessment/)。

![](img/dd2ef33aad0471b146d9ac28d5db2b06.png)

然后随着镜头的冷却，我进行了测量。我将一个样品放在 Atago 上，进行多次测量，以消除抽取不同样品的变量。随着温度的下降，TDS 继续上升，直到在 22.5 时达到最大值。在网上讨论了结果之后，我通过更多的测试找到了原因。随着时间的推移，样品慢慢蒸发，导致 TDS 攀升，直至达到最大值。

![](img/9e47855c2fff7f04b644f65ec8400468.png)

总的来说，Atago 非常容易使用，尤其是与白利糖度计相比。通常，为了获得良好的白利糖度读数，我必须将闪光灯(通常来自我的手机)举到另一端，以确保有足够的光线通过。白利糖度每 0.2 个读数就有一条线，大量使用后，你能分辨的最佳读数是 0.1 或 0.085 TDS。

白利糖度计对于多个样品来说也不是很好，因为你必须打开顶部，擦拭干净，添加另一个样品，关闭它，然后拿着它对着光，而不是只向 Atago 中添加三滴。

# 白利糖度与 Atago

在过去的一个月里，我拍摄了 35 张照片，测量了白利糖度和阿塔戈糖度。我通常以 1:1 的输出输入比停止拍摄，但是由于我的实验[随着时间](/coffee-solubility-in-espresso-an-initial-study-88f78a432e2c)提取，我开始测量拍摄的剩余部分(我称之为秒)。这导致 35 个以上的读数。此外，我计算了一下，如果我把 1:1 的投篮和秒的投篮结合起来，打出 3:1 的投篮，我的得分是多少。

当我用 Atago 取样时，我每次用三滴。我的目标是尽可能保持这个过程的可重复性。

![](img/8e2322f780bac7c9d214f0e3a009bd1a.png)

从这些结果来看，似乎 Atago 和白利米给出了类似的结果。忽略一些异常值，误差甚至没有那么大。

从输出与输入的比率来看，似乎有一个与 TDS 相关的趋势，这是正常的，因为 TDS 在较小的拍摄中开始非常高，并随着时间的推移而降低。这与比率以及 Atago 和 Brix 之间的差异没有关系。

![](img/755323ec07ab93e007db2aec71cd416b.png)

Atago 提供的另一个度量是温度，所以我们可以看一下温度和 TDS，但似乎没有太大的相关性。温度似乎也不会影响白利糖度和 TDS 的比较。

![](img/41a422f6e072ab0b4307a2456b0f2a87.png)

最后，由于我们有超过 30 个样本，我们可以进行配对 t 检验来确定分布是否在统计上[不同](/statistical-significance-hypothesis-testing-the-normal-curve-and-p-values-93274fa32687)，这意味着检验的 p 值小于 0.05。在下表中，可以看到分布在统计上没有差异，因为所有的 p 值都大于 0.05。

![](img/615a07a2f5209b7c2191c7d23d72879e.png)

这些测试非常有助于信任 Atago 仪表，也信任我之前使用白利糖度(超过 200 个样本)得出的结果。在我的[实验](/measuring-espresso-extraction-across-the-filter-c9a4ccee117f)中，Atago 计量器已经成为我更好地理解拍摄的关键，我希望这些结果能够帮助那些没有足够钱购买工具来帮助测量和改善他们的浓缩咖啡以及其他形式的咖啡的人。

如果你愿意，可以在推特[和 YouTube](https://mobile.twitter.com/espressofun?source=post_page---------------------------)上关注我，我会在那里发布不同机器上的浓缩咖啡照片和浓缩咖啡相关的视频。你也可以在 [LinkedIn](https://www.linkedin.com/in/robert-mckeon-aloe-01581595?source=post_page---------------------------) 上找到我。

# 我的进一步阅读:

[断续浓缩咖啡:提升浓缩咖啡](/overthinking-life/staccato-espresso-leveling-up-espresso-70b68144f94?source=post_page---------------------------)

[通过过滤器测量浓缩咖啡萃取量](/measuring-espresso-extraction-across-the-filter-c9a4ccee117f?source=your_stories_page---------------------------)

[浓缩咖啡中咖啡溶解度的初步研究](/coffee-solubility-in-espresso-an-initial-study-88f78a432e2c)

[浓缩咖啡模拟:计算机模型的第一步](https://medium.com/@rmckeon/espresso-simulation-first-steps-in-computer-models-56e06fc9a13c)

[咖啡数据表](/@rmckeon/coffee-data-sheet-d95fd241e7f6)

[工匠咖啡价格过高](https://link.medium.com/PJwoAMYpuT?source=post_page---------------------------)

[被盗浓缩咖啡机的故事](https://link.medium.com/pxCY5yrquT?source=post_page---------------------------)

[平价咖啡研磨机:比较](https://link.medium.com/rzGxlDtquT?source=post_page---------------------------)

[浓缩咖啡:机头温度分析](https://link.medium.com/FMxfCmcOCT?source=post_page---------------------------)

[浓缩咖啡过滤器分析](https://link.medium.com/2T2tjpUsgU?source=post_page---------------------------)

便携式浓缩咖啡:指南

克鲁夫筛:一项分析