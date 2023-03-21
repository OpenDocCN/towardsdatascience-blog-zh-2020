# 利用 Python 进行空间科学——太阳系中心

> 原文：<https://towardsdatascience.com/space-science-with-python-the-solar-system-centre-6b8ad8d7ea96?source=collection_archive---------28----------------------->

## [https://towards data science . com/tagged/space-science-with-python](https://towardsdatascience.com/tagged/space-science-with-python)

## Python 教程系列的第三部分

![](img/9b3cabe869345d79358ebf2aee14f17b.png)

我们太阳系的示意图(未按比例)，鸣谢: [NASA/JPL](https://images.nasa.gov/details-PIA11800)

## 前言

*这是我的 Python 教程系列“Python 的空间科学”的第三部分。这里显示的所有代码都上传到* [*GitHub*](https://github.com/ThomasAlbin/SpaceScienceTutorial) *上。尽情享受吧！*

## 上次…

…我们计算并可视化了太阳系重心(SSB)相对于太阳的运动。黄道平面上的二维投影揭示了 SSB 令人印象深刻的行为:重心的位置定期离开太阳，尽管太阳包含了太阳系 99 %以上的质量！人们可以理解，这种运动是由其他天体引起的，例如行星、小行星甚至尘埃。*但是主要的贡献者是什么，我们能想象出来吗？在本教程中，我们将针对这个科学问题，对上次的结果进行分析。*

## 我们开始吧

对于我们的分析，我们将使用与上次相同的时间间隔和方法。我们将额外使用另一个伟大的库: [*熊猫*](https://pandas.pydata.org/) 。*熊猫*常用于数据科学家、数据工程师以及科学家之间。人们可以创建包含表格形式数据的所谓数据框。过滤、添加列、应用函数或类似 SQL 的请求允许操作和分析存储在内存中的数据。如果您还没有安装 pandas，那么在您的虚拟环境中使用 *pip3* 包管理器来安装:

```
pip3 install pandas
```

如果你不熟悉*熊猫*，我会推荐一些教程，在那里你可以学习这个包的功能和优点。我将解释显示的功能，但是我强烈建议您深入研究这个数据驱动工具。

我们的第一个 SPICE 函数也是 [*furnsh*](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/furnsh_c.html) ，它加载 SPICE 元文件 *kernel_meta.txt* 中列出的所有请求的 SPICE 内核文件。我已经[下载了](https://naif.jpl.nasa.gov/pub/naif/)并为本文添加了相应的内核，所以你可以简单地从 [GitHub](https://github.com/ThomasAlbin/SpaceScienceTutorial) 获取最新的更新。后面的教程需要大的内核，需要手动下载。我们将在单独的教程中学习如何浏览官方库以及如何选择正确的内核。

同样，我们选择从 2000 年 1 月 1 日开始的时间间隔，并加上 10，000 天。我们使用函数 [*utc2et*](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/et2utc_c.html) 将 UTC 时间字符串转换为星历时间(et ),并创建一个包含所有 ET 时间步长的 *numpy* 数组。

第 1/8 部分

我们现在应该如何继续分析 SSB 的运动？首先，我们需要计算重心相对于太阳的轨迹。上一次，我们在 for 循环中做了这个。从现在开始，我们使用 *pandas* 数据帧在一个地方存储所有必要的结果。

SPICE 返回以 km 为单位的空间信息。同样，我们想用太阳的半径来缩放它。我们用函数 [*bodvcd*](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/bodvcd_c.html) 从内核中提取这些信息(注意:SPICE 知道每个内核的内容，加载后不需要指定内核的名称或位置):

第 2/8 部分

结果将存储在一个名为 *SOLAR_SYSTEM_DF* 的数据帧中，该数据帧在开始时被定义为空。第 6 行创建了一个名为 *ET* 的列，并用相应的 ET 值填充数据帧。 [*。loc*](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.loc.html) 在 *pandas* 中用于分配列，也用于过滤数据帧。在第 12+13 行，我们计算了 ETs 的日期时间对象。 *spiceypy* 函数 *et2datetime* 用于此目的，但是，它不是官方 SPICE 工具包的一部分。在 SPICE 中，函数 [*et2utc*](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/et2utc_c.html) 用于设置各种日期时间格式；输出本身是一个字符串。

第 3/8 部分

我们现在添加并计算三个新列。首先，*POS _ SSB _ WRT _ 太阳*被计算，其包含从太阳看到的 SSB 的位置向量。对于这个计算，我们需要 ET 列，并逐行应用(使用 [*)。通过 lambda 函数应用*](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.apply.html))SPICE 函数 [*spkgps*](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/spkgps_c.html) 。 [*spkgps*](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/spkgps_c.html) 需要目标 NAIF ID ( *targ* )，ET ( *et* )，参考帧(*ref*)；这里是黄道参考系*eclipse j2000*)和观测者的 NAIF ID ( *obs* )。SSB 和 Sun 的 ID 分别为 [0 和](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/req/naif_ids.html)10。

然后，计算出的位置向量与太阳的半径成比例(第 11 + 12 行)。我们按行应用一个 lambda 函数，用半径划分数组。

最后，使用 SPICE 函数 [*vnorm*](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/vnorm_c.html) 计算太阳和 SSB 之间的距离。该函数计算一个向量的长度(所谓的范数)，与 *numpy* 函数[*numpy . linalg . norm*()](https://numpy.org/doc/stable/reference/generated/numpy.linalg.norm.html)相同。

第 4/8 部分

无论是数据科学、空间科学、天文学还是任何其他科学领域:可视化和描述数据是正确分析的第一步。

所以让我们把 SSB 和太阳之间的距离想象成时间的函数。为此，我们使用上次介绍的模块 [*matplotlib*](https://matplotlib.org/) 。我们使用列*SSB _ WRT _ 太阳 _ 缩放 _DIST* 的数据，并根据 *UTC* 日期-时间绘制它。

以下代码创建了一个如下所示的图。

第 5/8 部分

![](img/992e5530a951106bd5d17adf64699ef2.png)

太阳系重心(SSB)与 UTC 日期时间之间的距离。这个距离是以太阳半径给出的。1 太阳半径对应 696000 公里。致谢:T. Albin (2020)

我们来描述和理解一下剧情。我们看到 SSB 和太阳之间的距离与 UTC 给出的日期时间。距离轴的刻度在 0 到 2 个太阳半径之间，时间轴从 2000 年开始，正好在 2028 年之前结束。我们看到，距离随时间变化，然而，这种变化似乎是调制的(不同的振幅；还有频率(？)).两个局部极大值之间的距离(大约 2009 年和 2022 年)大约是十年！

这些缓慢的时间变化意味着什么？理论上可以通过所有重力(行星、小行星、尘埃等)的总和计算出 SSB 相对于太阳的位置。).内行星(水星到火星)的轨道周期在 90 天到将近 2 年之间。如果这些行星的质量对 SSB 的计算很重要，我们会看到许多“尖峰”和快速变化的变化，但我们没有。

与内部行星相比，外部气体巨行星(木星、土星、天王星和海王星)的质量更大，轨道周期也更长(木星绕太阳一周大约 12 年，土星几乎需要 30 年)。虽然它们离我们很远(木星大约 5 天文单位，土星大约 9.5 天文单位)，但它们似乎严重影响了 SSB 的位置。

我们能简单地在数据中看到这种效应吗？

## 相位角

今天(2020 年 4 月 29 日),我们看到月亮正处于上弦月阶段。从地球上看，每秒钟被照亮的面积都在增加。几天后，月亮将被完全照亮，它的渐亏期开始了。在围绕我们旋转的过程中，月球相对于太阳以不同的相位角出现。考虑下面的草图:

![](img/afab0512a6d74bbe6c585955f2681c12.png)

从地球上看到的月亮和太阳之间的相位角。致谢:T. Albin (2020)

相角是从地球上看到的太阳和月亮的方向向量之间的角度。一般来说:相角是指从观察者的角度来看，两个方向向量所围成的角度。因此，可以计算任何天体之间的相角，相角定义在 0 到 180 度之间。

*这与 SSB 分析有什么关系？*如前所述，我们有一个理论，气体巨星的引力是 SSB 运动最相关的因素。SSB 和太阳中心之间的巨大距离可能意味着外围行星“排成一行”,从同一个方向产生更强的引力。

为了检查相位角的相关性，我们在数据框中增加了新的列，包含了所有气体巨星的角度。首先，我们建立了一个字典，其中包含行星(重心)的缩写名称以及相应的行星 NAIF ID 代码。

for 循环遍历字典，并为每个行星动态创建 2 个新列(子字符串%s 被行星的缩写替换):

*   *POS _ % s _ WRT _ 太阳*:从太阳上看到的行星位置矢量。对于这个计算，我们再次使用了*。在 ETs 上应用*功能，并为此使用 SPICE 函数 [*spkgps*](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/spkgps_c.html) 。
*   *PHASE _ ANGLE _ SUN _ % s2sb*:从太阳上看到的行星与 SSB 之间的相位角。*。应用*应用在*轴=1* 上，因为我们需要两列用于此计算，即*POS _ % s _ WRT _ 太阳*和来自太阳的 SSB 方向向量*POS _ SSB _ WRT _ 太阳*。这两个向量之间的角度通过 SPICE 函数 [*vsep*](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/vsep_c.html) 计算。结果以 rad 给出，因此使用 *numpy* 函数[*numpy . degrees()*](https://numpy.org/doc/stable/reference/generated/numpy.degrees.html)将其转换为度数。

第 6/8 部分

*两个向量(a，b)之间的角度(p)是如何计算的？*两个向量的点积等于两个向量的长度乘以夹角的余弦的乘积。我们将等式重新排列到角度，得到以下等式:

![](img/07707d43a2ed512b65807649bd914cc2.png)

致谢:T. Albin (2020)

现在，让我们验证一下 [*vsep*](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/vsep_c.html) 的结果。我们定义了一个 lambda 函数，其中使用了 *numpy* 的 [*numpy.arccos()*](https://numpy.org/doc/stable/reference/generated/numpy.arccos.html) ， [*。dot()*](https://numpy.org/doc/stable/reference/generated/numpy.dot.html) 和[*. linalg . norm()*](https://numpy.org/doc/stable/reference/generated/numpy.linalg.norm.html)函数。我们使用数据帧的第一个条目，计算从太阳看到的 SSB 和木星之间的相位角。*熊猫*功能[。iloc[]](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.iloc.html) 允许通过整数索引访问数据；这里:0。对于这两个函数，我们得到的结果大约是 14.9。

第 7/8 部分

我们现在有一个包含各种参数的复杂数据框架。我们得到了 UTC 和 ET 的时间，我们得到了太阳和 SSB 之间的距离以及所有气体巨星的相位角。现在我们要绘制数据，以查看 SSB-Sun 距离和相位角之间的联系。

## 气态巨行星的引力

我们该如何绘制这些数据？我们想要显示两种不同类型的数据。第一:SSB-太阳的距离(如前所示)以及从太阳上看到的 SSB 和每个行星之间的相位角。要查看任何链接，我们需要将它们绘制在一个图中，产生两个不同的 y 轴(x 轴是 UTC 时间)。我们有 4 个行星的相位角，结果在一个图中有 5 条曲线。由此产生的情节将过于拥挤，人们很容易忘记所显示的内容。

因此，我们为每个星球创建一个单独的子图。这些图垂直对齐，以共享日期-时间轴。第 3 行准备图形并创建 4 个 *matplotlib* 轴。

for 循环分别遍历所有 *matplotlib* 轴和行星数据。让我们深入探讨一下:

*   第 15 行:首先我们为每个子情节创建一个标题。这个名字就是这个星球的名字。因为每个星球都有自己的情节，我们不需要设定一个传奇。
*   **第 18–20 行:**这里，我们绘制了 SSB-Sun 距离，并选择蓝色来区分距离和相位角曲线。
*   **第 24–30 行:**一些绘图格式，我们添加了 y 标签、轴限制和颜色。
*   **第 33 行:**该命令告诉 *matplotlib* 在同一个 *matplotlib* 轴上创建一个孪生图。 [*。twinx()*](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.twinx.html) 表示 x 轴(UTC 中的日期时间)被复制。
*   **第 36–39 行:**现在我们绘制行星和 SSB(观测者:太阳)之间的相位角。这条曲线是橙色的…
*   **第 42–49 行:** …以及相应的 y 轴标签和刻度(y 轴上显示的数字)。
*   **第 52 行:**为了更好的可读性或视觉引导，我们沿着日期-时间添加了一个垂直网格。

最后，我们为时间轴设置一个标签，减少子图之间的空间并保存该图。

第八部分

所有支线剧情如下所示。你可以看到蓝色和橙色的曲线分别代表 SSB 和太阳的距离以及 SSB 和行星之间的相位角。标题显示行星名称，右侧 y 轴倒置。正如你所看到的，SSB 和太阳之间的大距离与 2020 年和 2024 年之间木星的小相位角相关。然而，这种影响对于 2008 年至 2012 年间的第一次局部最大值来说不太严重，并且与 2012 年至 2016 年间的最小值完全不相关。让我们来看看这个特殊的最小值。你可以看到土星、天王星和海王星的相位角要大得多，并产生了一个向另一个方向的“反引力”,导致 SSB 的位置停留在太阳内。在 2020 年和 2024 年之间，气态巨行星更倾向于同一个方向，导致最大距离几乎是 2 个太阳半径！

![](img/64b771c50721a2ea95475dd044ad77be.png)

这些子图显示了以太阳半径表示的 SSB-太阳距离与以 UTC 表示的日期-时间(左轴，蓝色)。橙色曲线(和相应的右轴)显示了从太阳上看到的行星和单边带之间的相位角，单位为度。每个子图的标题都显示了行星的名称。致谢:T. Albin (2020)

## 结论

今天我们看了一下 SSB 运动的原因。太阳和重心之间的距离随时间缓慢变化。振幅发生变化，最大值和最小值之间的时间间隔表明内行星不可能是这种引力的主要贡献者(否则我们会看到更多与较短轨道周期相关的短时间峰值和变化)。我们利用从太阳上看到的 SSB 和气态巨行星之间的相位角来分析太阳和 SSB 之间的距离。木星似乎是一个主要因素(因为它是最大的行星，距离我们只有 5 天文单位)。然而，考虑到 2012 年和 2016 年之间 0.5 倍太阳半径的最小距离，其他的巨行星也不能被忽略。

为了更深入地探究这个话题，我们需要用牛顿力学来处理这个问题，并利用每个行星的引力来计算 SSB 相对于太阳的位置。然而，我们不想把这个特定的主题推得太紧。下一篇教程将于 2020 年 5 月 2 日出版，我们将继续研究一个完全不同的空间主题的相位角。

在科学中创造情节通常是具有挑战性和耗时的。有没有其他方法来可视化数据和分析？当然！以某种方式可视化数据只是一种可能的解决方案。如果你想分享你的想法或解决方案，请随意。在这里或 GitHub 上。

同时，保持健康，享受夜空，

托马斯