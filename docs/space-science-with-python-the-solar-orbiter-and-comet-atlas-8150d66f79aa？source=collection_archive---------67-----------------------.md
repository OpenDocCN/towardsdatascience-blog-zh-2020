# Python 的空间科学——太阳轨道器和彗星图谱

> 原文：<https://towardsdatascience.com/space-science-with-python-the-solar-orbiter-and-comet-atlas-8150d66f79aa?source=collection_archive---------67----------------------->

## [用 Python 进行空间科学](https://towardsdatascience.com/tagged/space-science-with-python)

## [系列教程的第 15 部分](https://towardsdatascience.com/tagged/space-science-with-python)描述并分析了最近发生的一件事:欧空局的太阳轨道飞行器穿越了阿特拉斯彗星的尾部

![](img/1b0abd576ccb408a0b4709d7ede4295e.png)

可能是夜空中一颗漂亮的彗星:彗星 C/2019 Y4 (ATLAS)在彗星核心分裂后，其残余物散落在周围。美国宇航局/欧空局哈勃太空望远镜拍摄的图像。鸣谢: [NASA，ESA，D. Jewitt(加州大学洛杉矶分校)，Q. Ye(马里兰大学)](https://www.esa.int/ESA_Multimedia/Images/2020/05/Hubble_observation_of_Comet_ATLAS_on_23_April_2020)；许可证: [CC 乘 4.0](https://creativecommons.org/licenses/by/4.0/legalcode)

# 前言

*这是我的 Python 教程系列“用 Python 进行空间科学”的第 15 部分。这里显示的所有代码都上传到了*[*GitHub*](https://github.com/ThomasAlbin/SpaceScienceTutorial)*上。尽情享受吧！*

# 太阳观察者遇到…

几个月前，2020 年 2 月，欧洲航天局(ESA)启动了一项探索太阳及其暴力环境的雄心勃勃的任务:太阳轨道飞行器(Solar Orbiter)，简称: *SolO* 。这个 1.8 吨重的航天器配备了 10 个仪器来研究太阳的各种属性，如日冕或太阳风。大约 0.3 AU 的近日点会将航天器的隔热罩加热几百摄氏度。然而，这一工程杰作将产生的数据可能会为我们提供重要的太阳科学目标的科学见解，如:

*   太阳风是如何产生的，在哪里产生的？
*   太阳能发电机是如何工作的？

照相机、磁力计和带电粒子探测器将提供“数据拼图”，使我们能够创建离我们最近的恒星的复杂图片。

[](https://www.esa.int/Science_Exploration/Space_Science/Solar_Orbiter) [## 太阳轨道飞行器

### 太阳轨道器将解决太阳系科学中的大问题，帮助我们了解我们的恒星是如何产生和…

www.esa.int](https://www.esa.int/Science_Exploration/Space_Science/Solar_Orbiter) 

# …太阳访客

发射前两个月发现了一颗新彗星:C/2019 Y4(图集)。ATLAS 是一颗长周期彗星([彗星的起源](/space-science-with-python-the-origin-of-comets-3b2aa57470e7))，远日点约 650 AU，近日点约 0.25 AU。近日点通道是在五月的最后一天。

几周前，哈勃太空望远镜的观测显示这颗彗星分裂成了几块。一颗新的*大彗星*的希望破灭了…

[](https://hubblesite.org/contents/news-releases/2020/news-2020-28) [## 哈勃观察着阿特拉斯彗星分裂成 20 多块碎片

### 发布 ID: 2020-28 彗星是最富传奇色彩的外太空生物之一。它们的长尾巴是如此的…

hubblesite.org](https://hubblesite.org/contents/news-releases/2020/news-2020-28) 

… *然而*科学家确定彗星的尘埃和离子尾穿过了太阳轨道器的轨迹[1]！凭借其仪器，航天器能够通过飞行穿过彗星的残余物来做*额外的科学*。

[](https://sci.esa.int/web/solar-orbiter/-/solar-orbiter-to-pass-through-the-tails-of-comet-atlas) [## 欧空局科学技术-太阳轨道器将穿过彗星阿特拉斯的尾巴

### 在接下来的几天里，欧空局的太阳轨道飞行器将穿过阿特拉斯彗星的尾部。虽然最近…

sci.esa.int](https://sci.esa.int/web/solar-orbiter/-/solar-orbiter-to-pass-through-the-tails-of-comet-atlas) 

*但是科学家如何确定如此近距离的相遇呢？需要研究什么样的参数来确定太空中计划外的重大科学机会？*

# 紧密交会

彗星有两条尾巴:

*   **尘埃尾:**由于距离太阳很近，彗星的内核变热，气体喷流正在发展。这些喷射流从地核中输送并喷射出较小的尘埃颗粒。由于核心直径只有几公里，几米/秒的喷射速度就足以离开彗星的引力范围。尘埃粒子沿着彗星的轨迹扩散，但较小的粒子也会受到太阳辐射的影响，导致尾部“弯曲”，如下图所示。
*   **离子尾:**带电粒子，离子，也离开彗星。这些粒子与磁场，尤其是太阳风发生强烈的相互作用。太阳风也由氦核或质子等离子组成，移动速度高达 1000 公里/秒。在第一级近似下，这些粒子远离太阳径向移动。太阳风和辐射压力将离子带离彗星:几乎是以直线的方式。

看看下面的草图。你可以看到太阳位于我们太阳系的中心，由彗星阿特拉斯(白色)和太阳轨道器(红色)环绕。尘埃尾显示为灰色，离子尾径向远离太阳(蓝色)。为了在离子尾内成功进行测量，必须满足以下限制条件(将在编程部分进行分析):

1.  彗星及其离子尾必定出现在太阳和宇宙飞船之间，因为离子尾指向远离太阳的方向。
2.  离子尾应该指向太阳轨道器的方向/轨迹几度之内。
3.  时间:让我们考虑 100 公里/秒的平均离子尾速度[2](当然，它可能会有很大的变化)。一段时间后(取决于距离)，离子到达航天器的轨道。离子和宇宙飞船必须满足时间限制，否则，它们会互相错过。

![](img/cf663fffd581a4ad73210d0c2db6c68d.png)

彗星尾巴的草图。彗星(此处:ATLAS)和宇宙飞船(此处:太阳轨道器)围绕太阳旋转。与太阳的亲密接触导致了两条彗尾:一条尘埃尾和一条离子尾。离子尾几乎径向远离太阳，应该满足一定的几何和时间要求，才能被航天器探测到。贷方:T. Albin

让我们深入研究 Python 代码。今天，我们将再次需要 [*spiceypy*](https://github.com/AndrewAnnex/SpiceyPy) 和我们在上一次会议中创建的彗星数据库([彗星——来自远方的访客](/comets-visitors-from-afar-4d432cf0f3b))。由于它很小，数据库也存储在我的 [GitHub 库](https://github.com/ThomasAlbin/SpaceScienceTutorial)中。

第 1/12 部分

ATLAS 彗星的运动是通过使用存储在数据库中的轨道要素来确定的。对于太阳轨道飞行器，我们需要相应的 spk 内核来计算航天器的轨迹。ESA 提供了一个包含所有必需内核的 FTP 库:

*FTP://SPI FTP . esac . esa . int/data/SPICE/SOLAR-ORBITER/kernels/spk/*

我们下载内核…

*solo _ ANC _ SOC-orbit _ 2020 01 120–2030 11 20 _ L015 _ V1 _ 00024 _ v01 . bsp*

…也上传到 GitHub 存储库，设置内核元文件的相对路径并加载它:

第 2/12 部分

对于彗星的轨道元素，我们需要太阳的 G*M 值(引力常数乘以质量)。我们使用函数 [*bodvcd*](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/bodvcd_c.html) 提取该信息，并在第 3 行中指定一个常数。

第 3/12 部分

现在，我们需要从数据库中提取轨道元素。使用 [*熊猫*](https://pandas.pydata.org/) 函数 [*read_sql*](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_sql.html) 执行第 5 行到第 10 行的 SQL 查询需要设置连接(第 2 行)。提取所有需要的元素，稍后 SPICE 函数 [*圆锥曲线*](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/conics_c.html) 需要这些元素，将这些信息转换成状态向量。彗星的名字是 *C/2019 Y4(图集)*。

空间和角度信息分别以 AU 和度存储。这些值需要转换成 km(第 13 到 16 行)和弧度(第 21 到 23 行)。使用 [*convrt*](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/convrt_c.html) 我们可以轻松地将 km 值转换为 AU。

在第 21 到 23 行中，应用了一个 for 循环，遍历包含子字符串 *DEG* (第 21 行*DEG*的缩写)的所有列名。子字符串被替换为 *RAD* (弧度*的缩写*，第 22 行)，转换是通过 [*numpy*](https://numpy.org/) 函数 [*弧度*](https://numpy.org/doc/stable/reference/generated/numpy.radians.html) 完成的。最后，我们加上太阳的 G*M 值(第 26 行)。

第 4/12 部分

pandas 数据帧现在被转换为一个列表，该列表包含 SPICE 功能所需顺序的轨道元素:

第 5/12 部分

根据欧空局的说法，尾部穿越是在 2020 年 5 月底。因此，我们设置一个日期时间数组，从 2020 年 5 月 20 日(第 2 行)到 2020 年 6 月 6 日(第 3 行)，以 1 小时为步长(第 6 行到第 7 行)。

第 6/12 部分

基于日期时间数组*时间数组*我们现在可以计算出 ATLAS 和太阳轨道飞行器的相应状态向量。两个空向量将存储结果(第 3 行和第 7 行)。第 10 到 19 行和第 22 到 32 行对这两个对象进行计算。人们可以很容易地合并这两个 for 循环，但是，如果想添加额外的 comet 或航天器特定代码，我将它放在单独的一个中。for 循环遍历日期时间数组，并在第 13 / 25 行将其转换为星历时间。然后，分别使用 [*圆锥曲线*](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/conics_c.html) 和 [*spkgeo*](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/spkgeo_c.html) 在第 16 / 28 到 29 行中计算状态向量。最后(第 19 行和第 32 行),位置组件被附加到列表中，稍后在第 35 行和第 36 行被转换为 *numpy* 数组。

第 7/12 部分

让我们检查一下第一个要求，彗星是否比太阳轨道器更靠近太阳。我们计算所有 ATLAS 和太阳轨道飞行器向量的范数，并提取最小值(第 2 行和第 7 行)。我们将 km 值转换为 AU(为了更好的可读性)并打印出来。

第 8/12 部分

在我们的时间窗内，ATLAS 肯定离太阳更近。实际上，我们应该分析所有时间步长的距离，但是我们的计算只涵盖了几天，而且由于 ESA 的文章，我们已经知道彗星现在离太阳更近了。

```
Minimum distance ATLAS - Sun in AU: 0.25281227838079623
Minimum distance Solar Orbiter - Sun in AU: 0.5208229568917663
```

第二个几何考虑:彗星轨道与航天器轨道之间的最小距离。这种计算可以从理论的角度进行，实现一些优化算法…这里，我们使用一种快速简单的方法，通过比较 ATLAS 和太阳轨道器之间的所有位置矢量距离。为此，我们需要模块[](https://www.scipy.org/)*。函数[*spatial . distance . cdist*](https://docs.scipy.org/doc/scipy/reference/generated/scipy.spatial.distance.cdist.html)需要两个向量列表，并计算包含这两个列表之间所有距离的矩阵(第 5 行)。我们在第 8 到 9 行打印最小值。*

*第 9/12 部分*

*两个轨道之间的最小距离约为 40，000，000 公里，约为地球和太阳之间距离的 25 %。*

```
*Minimum distance between ATLAS and Solar Orbiter in km: 40330530.0*
```

*现在我们需要考虑时机！*阿特拉斯什么时候到达两个轨道距离最小的点？是在太阳轨道器到达它的点之前吗？记住:阿特拉斯必须首先到达他的点，因为离子尾需要发展并向太阳轨道器的轨道移动。在这段代码中，我们提取了计算出的距离矩阵的索引，这些索引对应于定义最小距离的 ATLAS 和 Solar Orbiter 向量(第 6 行和第 7 行)。让我们打印两个对象的索引。**

*第 10/12 部分*

*地图集的索引较小；太好了！现在我们来看看相应的时间。【ATLAS 和太阳轨道器什么时候到达两个轨道最接近的轨道位置(第 3 至 5 行)？*

```
*ATLAS Index of close approach: 292
Solar Orbiter Index of close approach: 503*
```

*第 11/12 部分*

*阿特拉斯在 2020 年 6 月 1 日到达他的点，太阳轨道飞行器在大约 8.5 天后跟随。让我们假设离子尾部速度为 100 千米/秒。离开彗星附近，离子每天移动 8，640，000 千米。4.6 天后，离子移动了 4000 万公里。4 天后，太阳轨道飞行器到达尾部。*还来得及吗？*谢天谢地没有。100 km/s 的速度是一个非常简单的方法，可能与实际速度分布有很大差异。*

```
*ATLAS closest approach date-time: 2020-06-01 04:00:00
Solar Orbiter closest approach date-time: 2020-06-09 23:00:00*
```

*因此，距离是 4000 万公里(在天文尺度上相当小)，时间似乎是可行的。最后一个问题是:离子尾 ***真的*** 指向太阳轨道器的方向吗？在我们的计算中，我们只考虑两个轨道最接近的点。考虑到轨道的倾斜，最小的角距离也可以在不同的时间。*这可能是一项需要你去完成的任务！**

**但是离子尾的指向是什么？*好吧，既然离子尾径向指向远离反太阳的方向，我们可以简单的用彗星本身的位置矢量！我们得到 ATLAS(第 7 行)和太阳轨道器(第 8 行)的位置向量，并计算它们相应的范数(第 11 行和第 12 行，使用 [*vnorm*](https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/vnorm_c.html) )。计算点积(第 15 行)以最终确定 ATLAS 的位置矢量和航天器之间的角度(第 18 和 19 行)。在第 23 到 25 行，我们打印了角距离。*

*第 12/12 部分*

```
*Minimum angular distance between a possible ion tail and the Solar Orbiter's trajectory in degrees: 7.74*
```

*最小角距离为 7.7 度。这是否意味着我们无法用太阳轨道飞行器的仪器探测到任何东西？同样，这要视情况而定。离子尾相当窄，但是它的分布和运动受到辐射和太阳风的严重影响。考虑恩克彗星的离子尾，如下图所示。太阳在右上角，你可以很容易地看到尾巴指向左下角。尾巴的形状、宽度和运动一直在变化。这些快速的变化也适用于彗星阿特拉斯。问题是，太阳轨道飞行器足够幸运吗？*

*![](img/a72e9e48e7779b48fde8865afd66ac2a.png)*

*恩克彗星的离子尾正在向反太阳方向移动。正如你所看到的，尾巴并不总是一条直线，并且受到太阳风的影响。太阳在右上角(在这个记录之外)，然而，你可以清楚地看到太阳风，因为它在右上角的密度很高。演职员表: [NASA/SOHO](https://www.nasa.gov/feature/goddard/comet-encke-a-solar-windsock-observed-by-nasa-s-stereo)*

# *结论*

*凭借我们的 Python 和 SPICE 技能，我们能够重建一个非常近期的事件！当然，我们简化了一些几何方面的考虑，然而，我们学会了如何快速创建一种方法来为完全不同的任务设计的航天器任务找到*额外的科学机会*。去吧，使用 comet 数据库，从其他航天器任务中下载更多的 spk 内核，也许你会发现一个作为公民科学家的未知机会！*

*托马斯*

# *参考*

*[1] Geraint H. Jones，Qasim Afghan 和 Oliver Price。2020.*太阳轨道器对彗星 C/2019 Y4 图集的原位探测前景*。AAS 研究笔记，第 4 卷，第 5 号，【https://iopscience.iop.org/article/10.3847/2515-5172/ab8fa6 *

*[2] E .贝哈尔、h .尼尔森、p .亨利、l .贝里奇、g .尼古拉乌、g .斯滕贝格·威瑟、m .威瑟、b .塔伯恩、m .萨伦费斯、c .戈茨。2018.*彗星尾巴的根部:在彗星 67P/丘留莫夫-格拉西缅科*进行的罗塞塔离子观测。一辆&一辆 615 A21。[https://www . aanda . org/articles/aa/full _ html/2018/08/aa 32842-18/aa 32842-18 . html](https://www.aanda.org/articles/aa/full_html/2018/08/aa32842-18/aa32842-18.html)*