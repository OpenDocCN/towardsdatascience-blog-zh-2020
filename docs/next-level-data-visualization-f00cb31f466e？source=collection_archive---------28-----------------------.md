# 下一级数据可视化

> 原文：<https://towardsdatascience.com/next-level-data-visualization-f00cb31f466e?source=collection_archive---------28----------------------->

## 完整的 Plotly 手册

## 制作能激发灵感的图表/第 1 部分:制作定制图表

在古印度文献中，对哲学概念的解释通常以否定这个概念是关于什么而不是关于什么开始。利用一个反复出现的短语 [*neti neti*](https://en.wikipedia.org/wiki/Neti_neti) (意思是*既不是这个也不是那个*)，这个想法是，告诉什么东西不是，至少和解释那个概念/想法的实际意义一样重要。跟随这些古代哲学家的脚步，让我首先列举出本文不涉及的内容:

*   这篇文章不是关于如何在 plotly 中快速制作图表，通常只有一行代码，就像 plotly express 一样。如果这是你感兴趣的，请跟随威尔·科尔森的这篇[惊人的媒体文章](/the-next-level-of-data-visualization-in-python-dd6e99039d5e)。
*   本文也不是要列出所有可用于数据可视化的不同图表类型。如果这是你正在寻找的，看看这篇[非常翔实的文章](https://visme.co/blog/types-of-graphs/)萨曼莎·李乐。事实上，本文只讨论了两种不同的图表类型:折线图和散点图。

# 介绍

任何数据分析项目都有两个基本目标。首先，以易于理解的形式整理数据，揭示隐藏的模式，并确定关键趋势。第二，也许更重要的是，通过深思熟虑的数据可视化将这些发现有效地传达给读者。这是一篇介绍性文章，讲述了如何开始考虑定制可视化，以方便地将关键数据特性传播给查看者。我们通过超越使 plotly 在数据分析师中如此受欢迎的单线图，并专注于个性化的图表布局和美学**来实现这一目标。**

本文中使用的所有代码都可以在 [Github](https://github.com/Aseem139/Plotly/blob/main/PLOTLY_Medium_Part_01.ipynb) 上获得。这里展示的所有图表都是交互式的，并且是使用 [jovian](https://www.jovian.ai) 渲染的，这是一个用于共享和管理 jupyter 笔记本的不可思议的工具。Usha Rengaraju 的这篇文章包含了如何使用这个工具的所有细节。

![](img/dd75df43ca2a5d616c9ad42e491a9462.png)

[来源](https://unsplash.com/s/photos/graph-drawing)

## Plotly

Plotly 是数据可视化的自然选择库，因为它易于使用，文档记录良好，并允许定制图表。在接下来的章节中，我们先简要总结一下 plotly 架构，然后再进行可视化。

虽然大多数人更喜欢使用高级的`plotly.express`模块，但在本文中，我们将关注使用`plotly.graph_objects.Figure`类来呈现图表。虽然 plotly 网站上有大量的[文档](https://plotly.com/python/),但这些材料对于那些不熟悉可视化的人来说可能有点难以接受。因此，我努力提供一个清晰和简洁的语法解释。

我们将使用的 plotly `graph_objects`由以下三个高级属性组成，绘制一个图表主要涉及指定这些属性:

*   `data`属性包括从超过 40 种不同类型的轨迹中选择图表类型，如`[scatter](https://plotly.com/python/line-and-scatter/)`、`[bar](https://plotly.com/python/bar-charts/)`、`[pie](https://plotly.com/python/pie-charts/)`、`[surface](https://plotly.com/python/3d-surface-plots/)`、`[choropleth](https://plotly.com/python/choropleth-maps/)`等，并将数据传递给这些函数。
*   `layout`属性控制图表的所有非数据相关方面，如文本字体、背景颜色、轴&标记、边距、标题、图例等。在处理大型数据集时，我们将花费相当多的时间操纵这个属性来进行更改，如添加一个额外的 y 轴或在一个图形中绘制多个图表。
*   `frames`用于指定制作动画图表时帧的顺序。本系列的后续文章将广泛利用这一属性。

对于本文中制作的大多数图表，下面三个是我们将使用的标准库:

对于 python 的新手，可以看看我之前的一篇关于使用 pandas 进行数据争论的文章。其中一些工具将用于提取和转换可视化数据。

[](/data-wrangling-in-pandas-a-downloadable-cheatsheet-84326d255a7b) [## 使用熊猫进行数据争论的备忘单

### 将原始数据转换成函数形式

towardsdatascience.com](/data-wrangling-in-pandas-a-downloadable-cheatsheet-84326d255a7b) 

本文的其余部分分为以下几个部分:

1.  **折线图**

*   基本折线图
*   定制的折线图
*   何时不使用折线图

**2。散点图**

*   基本散点图
*   带有下拉菜单的图表
*   散点图矩阵

# 折线图

## 基本折线图

还有什么比折线图更常规的呢？当考虑数据可视化时，这是首先想到的事情之一。我们首先利用 gapminder 数据集来呈现一个包含许多数据点的折线图。对于那些不熟悉这种数据可视化经典的人，请查看他们的[网站](https://www.gapminder.org/data/)和[这种基于 gapminder 数据的动画](https://www.gapminder.org/videos/200-years-that-changed-the-world/)，它在不到 5 分钟的时间内捕捉了近 200 年的世界历史。

首先，我们使用一行`plotly.express`代码制作折线图，其中绘图主要涉及指定`x`和`y`变量，绘图`title`和用于`color`编码数据的变量。

现在让我们使用 plotly `graph_objects`制作同样的图表。

我们首先使用`fig1 = go.Figure()`初始化一个空的 go figure 对象。然后，我们通过使用来自`plotly.graph_objects`的`go.Scatter`类为每个国家添加一个轨迹`fig1.add_trace`来绘制一个折线图。这个类可以通过改变`mode`参数来制作折线图和散点图，该参数可以采用由`+`连接的`"lines"`、`"markers"`、&、`"text"`的任意组合。`add_trace`用于向`go.Figure()`对象提供**数据参数**。此外，我们还可以为后续图表提供**布局参数**和**框架参数**，我们将在后续文章中介绍。

这是基本的折线图。我们使用只有一行代码的`plotly.express`和使用基本上做同样事情的`graph_objects`的稍长代码来绘制它。

这张图表看起来不可怕吗？只有我有这种感觉吗？到处看到这样的图表，你不烦吗？

我们使用 plotly `graph_objects`类制作这个图表的原因是，在决定定制图表的**数据**和**布局** ( & **框架**)属性时，它提供了很大的灵活性。

## 定制的折线图

就数据可视化美学而言，我一直很欣赏 Nathan Yau 在[流动数据](https://flowingdata.com)中的作品。例如，检查这张[图表](https://flowingdata.com/2020/06/22/age-generation-populations/)，然后将其与我们刚刚制作的图表进行比较。从默认设置的世界走向这种定制的图表是我在这篇文章和本系列后续文章中的最终目标。

让我们开始定制那个可怕的图表。

**配色方案。**所有数据要么是离散的，要么是连续的。离散数据集是指各个数据点相互独立的数据集。在我们刚刚可视化的 GDP 数据集中，每个国家的 GDP 独立于其他国家的 GDP，因此我们必须使用来自`px.colors.qualitative`模块的颜色来可视化它。查看 plotly [文档](https://plotly.com/python/discrete-color/)以获得该模块中所有可用颜色集的完整列表。这种颜色集中的所有颜色具有不同的[色调](https://en.wikipedia.org/wiki/Hue)但是具有相似的[饱和度和值](http://learn.leighcotnoir.com/artspeak/elements-color/hue-value-saturation/)。

连续数据集的值在一个范围内变化，就像这张失业率图一样。我们使用顺序配色方案来可视化这种数据，其中数据的大小映射到颜色的强度。本系列的后续文章将更多地关注颜色以及如何有效地将颜色用于数据可视化。

回到定制我们的折线图。我们使用`cycle`工具通过`palette = cycle(px.colors.qualitative.Pastel)`交互`**Pastel**`调色板中的不同颜色。每次我们绘制数据点时，我们使用`marker_color=next(palette).`选择其中一种颜色

**轴。**让我们开始格式化轴，去掉矩形网格，在长度为 10 的轴外添加记号(`showgrid=False, ticks="outside", tickson="boundaries", ticklen=10`)。我们也去掉了 y 轴，有了粗黑的 x 轴(`showline=True, linewidth=2.5, linecolor='black'`)。所有这些参数都被传递给`update_xaxes`和`update_yaxes.`

**布局。**最后，我们指定几个参数来定义图表的整体布局，即:

*   文本字体样式、大小和颜色。`family="Courier New, monospace", size=18, color="black"`
*   总体图表尺寸。`width=1000, height=500`
*   图表背景和纸张颜色为白色。`plot_bgcolor='#ffffff', paper_bgcolor = '#ffffff'`。看看这个在线工具[可以将任何颜色转换成十六进制格式，以便在 python 中使用。`'#ffffff'`是白色的十六进制代码](https://htmlcolorcodes.com/color-picker/)
*   标题及其水平位置。 `title='GDP per capita of European Countries <br> 1952-2007', title_x=0.4.`
*   x 和 y 轴标签。`xaxis=dict(title='Year'), yaxis=dict(title='GDP per capita in USD').`

将所有这些放在一起，这就是我们得到的结果:

这张图表可能与 Nathan Yau 的一些作品不一样，但它仍然比默认的 plotly 图表好得多。这个图表代码也代表了我们将在本文中绘制的所有其他图表的基本框架。上图中的几个关键变量可以很容易地改变，以改变剧情的整体美感。这些包括绘图和纸张背景颜色(`plot_bgcolor='#ffffff'` & `paper_bgcolor = '#ffffff')`)、渲染数据的颜色集(`px.colors.qualitative.Pastel)`和轴布局(`update_xaxes` & `update_yaxes)`)。一定要花时间玩这些。

## 何时不使用折线图

现在让我们制作一个类似于上图的图表，但是是针对不同的数据集。

随着变量数量的增加，折线图变得难以解释，因为不同变量对应的颜色可能难以区分，或者至少需要一些努力。一种选择是使用堆积面积图。只需将`stackgroup=’one’`添加到上面代码中的 add_trace 函数中，就可以获得相同数据集的堆积面积图。

为了比较不同的数据点，面积图优于线图。在上面的图表中，我们不仅可以看到一个国家排放了多少二氧化碳，还可以比较它与其他国家的表现。中国排放的二氧化碳略多于美国和 EU-28 国的总和，这是我们可以从堆积面积图中立即得出的结论，而同样的结论很难从简单的线图中得到解释。

# 散点图

## 基本散点图

虽然折线图呈现了一个变量随时间变化的过程，但散点图也可用于绘制两个或多个相关或不相关变量之间的变化。我们从绘制一个公开可用的数据集开始，该数据集包含两个指数( [GCAG](https://www.ncdc.noaa.gov/cag/global/time-series) 和 [GISTEMP](https://data.giss.nasa.gov/gistemp/) )，测量自 1880 年以来的平均地表温度变化。这次我们使用`plotly.graph_objects`的`go.Scatter`类和`mode='markers'` 。代码的整体主干与以前基本相同。

现在让我们添加另一个变量，在(大致)相同的持续时间内的平均海平面数据。结果是，当两个温度指数从-0.5 到 1.5 变化时，海平面从 0 到 10 变化。如果我们在同一条轴上绘制这两条曲线，温度曲线基本上会变平。所以我们用`fig = make_subplots(specs=[[{"secondary_y": True}]])`初始化一个有两个不同 y 轴的图形。现在，当使用`fig.add_trace`将每个轨迹添加到该图时，我们必须通过提供一个额外的参数`secondary_y.`来指定绘制它的 y 轴

## 带有下拉菜单的图表

如果我们想在同一张图表上绘制更多的变量呢？我们可以利用下拉菜单来选择单个变量，一次显示一个。为此，我们需要做到以下几点:

1.  向`layout`添加一个新的参数，它包括所有要添加的按钮的列表`buttons= list_updatemenus`和这个下拉菜单的位置`x=1.2,y=0.8`。`updatemenus=list([dict(buttons= list_updatemenus, x=1.2,y=0.8)])`
2.  使用`list_updatemenus.` 指定下拉菜单的选项列表，包括:

*   `label`:出现在下拉菜单中的变量名称
*   `method`:决定当从下拉菜单中选择特定选项时如何修改图表。它可以是下列之一:`restyle`(修改数据)、`relayout`(修改布局)、`update`(修改两个数据的&布局)和`animate`(开始或结束动画)。
*   `args`:这包括(1) `visible`一个列表，指定哪个数据集将以布尔列表的形式绘制。该列表的大小与添加到该图中的迹线数量相同。`'visible': [True, False]`表示将显示两个`go.Scatter`图中的第一个。(2) `'title'`绘制变量时显示在顶部的标题。

将所有这些放在一起，我们得到以下结果:

尝试使用下拉菜单选择单个变量。

## 散布矩阵

使用下拉菜单可能并不总是一个好的选择，因为我们经常希望同时可视化几个变量，以揭示它们之间可能的关系。我们可以使用散点图来做到这一点，散点图是探索性数据分析的一部分。

我们将使用的天气数据集记录了几个变量，如最高和最低温度、风向和风速、阳光量和相对湿度。对于每个数据点，我们也知道结果，即是否下雨以及降雨量。现在的目标是可视化所有这些变量是如何影响降雨结果和降雨量的。我们首先给结果变量分配标签(0 表示无雨，1 表示下雨)。然后，我们使用`go.Splom`模块在矩阵上绘制所有这些变量。同样，基本的代码主干保持不变。

注意，我们没有指定调色板，而是使用了 colorscale 来代替`colorscale='temps'`。查看 [plotly 文档](http://colorscale='temps')获得所有可用的`colorscales.`列表`showupperhalf`参数设置为 true 时将产生一个完整的矩阵，其中每个变量都绘制两次，一次在对角线下方，一次在对角线上方。

# 结论

在本文中，我们讨论了如何绘制基本的折线图，以及何时用面积图替换它。然后，我们讨论了如何绘制散点图以及何时用散点图替换散点图。在此过程中，我们讨论了如何定制 plotly graph_objects 来生成符合我们要求的图表。我们通过对基本的代码主干做一些小的改动来实现这种定制。

本系列的后续文章将重点讨论绘制地图，深入使用颜色和动画来实现高级数据可视化。感谢阅读。请分享您的反馈。