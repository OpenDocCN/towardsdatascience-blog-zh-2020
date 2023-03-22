# 棒棒糖图表

> 原文：<https://towardsdatascience.com/lollipop-charts-2f748b90f6f0?source=collection_archive---------41----------------------->

## **为什么&怎么样，用棒棒糖讲故事**

![](img/34d493fb9aa13e92378147164bb321db.png)

图片来自 Unsplash 的 Amy Shamblen

**又名:**棒棒糖剧情

**为什么**:棒棒糖图(LC)是条形图的一种便捷变体，其中条形被一条线和一个点代替。就像条形图一样，棒棒糖图用于在不同的项目或类别之间进行比较。它们也用于**排名**或显示**随时间变化的趋势**。我们只比较每个项目或类别的一个数字变量。它们不适用于关系、分布或构成分析。

当您必须显示大量类似的高值时，LCs 比条形图更受欢迎。在标准条形图的情况下，您可能会得到一个混乱的图表，并体验到一种称为**莫尔条纹** (#1)的光学效果。图 1 在左侧显示了一个条形图，在右侧显示了一个棒棒糖图。两者都是基于相同的数据，但是很明显，棒棒糖的极简替代方案产生了更吸引人和更清晰的视觉效果。

![](img/41391b541b9a92fbb1110353dd468fc1.png)

图 1:条形图和棒棒糖图的比较。

莫尔效应是当观看叠加在另一组线或条上的一组线或条时出现的视觉感知，其中这些组在相对大小、角度或间距上不同。莫尔条纹是透过层层栅栏或者在拍摄电视或电脑屏幕时看到的黑色条纹。即使莫尔效应可以产生有趣和美丽的几何图案，但在数据可视化任务中应避免使用它们，以免混淆观众。

**如何:**棒棒糖图是二维的，有两个轴:一个轴显示类别或时间序列，另一个轴显示数值。这些数值由线末端的点的位置表示。垂直方向的 LC 在 y 轴上显示分类变量，而水平方向的 LC 在 x 轴上显示分类变量。图 2 显示了水平 LC 的示意图，其中细线末端的点表示每个类别的数值。使用 *Matplotlib* ，您必须使用 *stem* 功能来绘制水平棒棒糖。 *stem* 功能绘制从基线到 y 坐标的垂直线，并在顶部放置一个标记(#2)。

![](img/4d751ebaf69ec11296681063d404435d.png)

图 2:棒棒糖图的示意图。

LCs 是排名的一个很好的选择。标准程序是按降序排列类别，并垂直表示它们。使用 *Matplotlib* ，您必须结合使用函数 *hlines* 和 *plot* 来绘制垂直棒棒糖。 *hlines* 从标量 *xmin* 到 *xmax* (#3)在每个 *y* 处画水平线。函数图*有几种可能的标记，但我们建议使用经典的圆形符号，如本文中的图所示。*

![](img/f10e610307ea0d2c4dcaecbfbb255d07.png)

图 3:垂直有序的棒棒糖图。

您必须始终从 0 开始数轴:如果行被截断，实际值将无法正确反映。请记住，在比较数据时，我们的视觉对长度差异非常敏感。如果我们修改基线，我们不可避免地会扭曲视觉。如果其中一个变量是时间(年、月、日、小时)，请始终将其设置在水平轴上。时间总是从左到右，而不是从上到下。

LCs 等同于条形图，但这种等同只对**标准条形图**有效；不要试图将它扩展到堆积、聚集或重叠条形图(#4、#5)。

LCs 是两个数据可视化重量级人物之间一场有趣的辩论的主题，在理论和概念层面上都是如此:Stephen 少数人和 T21。很少通过讽刺的标题“棒棒糖排行榜:”谁爱你，宝贝？声称“LCs 的灵感来源于激发了如此多愚蠢图形的同一事物:对可爱和新奇的渴望”。他补充说，LCs 的主要问题是:“棒棒糖末端的圆心标记着值，但圆心的位置很难判断，与条形的直边相比，它不精确，而且圆的一半延伸到它所代表的值之外，使它不准确”。

对他来说，开罗站出来为图表辩护说:“我相信棒棒糖有它的用途。比如说，有 8 个或 9 个以上条形的条形图，看起来通常很忙，很笨重。棒棒糖图可以通过显著增加条形之间的空白来解决这个问题。他提出了一种解决方案，即减小圆的尺寸，甚至用圆的顶点来标记数值。

Eli Holder 在 Nightingale (#7)上发表了一篇非常有趣的文章，标题是:“解决争论:棒糖与棒棒糖(与点状图)的对比”，描述了一个实验，揭示了“棒棒糖图表如何影响读者的理解？为了好玩的美学，我们牺牲了多少认知责任？”最后，他得出结论，实验结果**显示条形图和棒棒糖图**之间没有显著差异，它们导致了大致**相同的准确性和相同的响应时间。**最后，LCs 为数据提供了一个很好的极简可视化，它们应该在与条形图完全相同的情况下使用。

# **用棒棒糖讲故事**

> **1。—*mtcars*数据集**的燃料消耗:人类活动增加了二氧化碳(CO2)和其他温室气体(GHC)的排放，推高了气温。大多数人为的二氧化碳排放来自燃烧化石燃料。GHG 交通运输的排放约占美国温室气体排放总量的 28%，是美国最大的温室气体排放源(排名第八)。美国环境保护署为 GHG 制定了一项国家计划，并为轻型车辆(乘用车和卡车)制定了燃油经济性标准。

之前对油耗的研究是基于通过 *mtcars* 数据集进行的汽车趋势道路测试。数据摘自 1974 年的《美国汽车趋势》杂志，包括 32 款汽车(1973-74 款)的油耗以及汽车设计和性能的 10 个方面。以下 LC 显示了 32 个汽车品牌的每加仑行驶里程。顶部的蓝色点划线对应于油耗低于平均值(20.09 mpg)的车辆，而红色点划线对应于油耗高于平均值的车辆。显然，对于比较如此大量的不同类别，棒棒糖图比条形图更好。

![](img/ad572deeea08c17752a86d81f6dabc70.png)

图 4:基于 *mtcars* 数据集的每加仑行驶里程。

> 2. - **阿根廷政府预算**:政府预算是政府收到的款项(税收和其他费用)和政府支付的款项(购买和转移支付)的分项核算。当政府支出大于收入时，就会出现预算赤字。预算赤字的对立面是预算盈余。

由于经常性的政府预算赤字，阿根廷是世界上通货膨胀率最高的国家之一。下图显示了该国 2012-2018 年期间的月度预算赤字。现在，棒棒糖图可以让我们以一种美观的方式跟踪一段时间内的预算模式。如上所述，时间在水平轴上从左向右运行。

![](img/2a30b37c978cbed14a37f9903effcc2d.png)

图 5:阿根廷月度预算赤字。

总之:棒棒糖图与标准条形图在相同的情况下使用，以相同的方式对数值进行编码:线条的长度和线条末端的点的位置相当于水平或垂直矩形条的长度或高度。当您处理大量相似的数值时，棒棒糖图比条形图更受欢迎。

如果你对这篇文章感兴趣，请阅读我以前的:

“气泡图，为什么&如何，用气泡讲故事”

[](/bubble-charts-why-how-f96d2c86d167) [## 气泡图，为什么和如何

### 用泡泡讲故事

towardsdatascience.com](/bubble-charts-why-how-f96d2c86d167) 

“平行坐标图，为什么&如何，用平行线讲故事”

[](/parallel-coordinates-plots-6fcfa066dcb3) [## 平行坐标图

### 为什么&如何:用类比讲故事

towardsdatascience.com](/parallel-coordinates-plots-6fcfa066dcb3) 

**参考文献**

第一名:【https://en.wikipedia.org/wiki/Moir%C3%A9_pattern 

# 2:[https://matplotlib . org/3 . 1 . 1/gallery/lines _ bars _ and _ markers/stem _ plot . html](https://matplotlib.org/3.1.1/gallery/lines_bars_and_markers/stem_plot.html)

# 3:[https://matplotlib . org/3 . 1 . 1/API/_ as _ gen/matplotlib . py plot . hlines . html](https://matplotlib.org/3.1.1/api/_as_gen/matplotlib.pyplot.hlines.html)

#4: Darío Weitz，“堆积条形图，为什么&如何，讲故事&警告”
[https://towardsdatascience . com/Stacked-Bar-Graphs-Why-How-f1 b 68 a 7454 b 7](/stacked-bar-graphs-why-how-f1b68a7454b7)

#5: Darío Weitz，“群集和重叠条形图，为什么和如何”
[https://towardsdatascience . com/Clustered-Overlapped-Bar-Charts-94 f1 db 93778 e](/clustered-overlapped-bar-charts-94f1db93778e)

#6:斯蒂芬·菲罗，《棒棒糖排行榜》:谁爱你，宝贝？
[https://www.perceptualedge.com/blog/?p=2642](https://www.perceptualedge.com/blog/?p=2642)

#7:伊莱·霍尔德，[https://medium . com/nightingale/bar-graphs-vs-lollipop-charts-vs-dot-plots-experiment-ba 0 BD 8 aad 5d 6](https://medium.com/nightingale/bar-graphs-vs-lollipop-charts-vs-dot-plots-experiment-ba0bd8aad5d6)

# 8:[https://www . EPA . gov/transportation-air-pollution-and-climate-change/carbon-pollution-transportation](https://www.epa.gov/transportation-air-pollution-and-climate-change/carbon-pollution-transportation)