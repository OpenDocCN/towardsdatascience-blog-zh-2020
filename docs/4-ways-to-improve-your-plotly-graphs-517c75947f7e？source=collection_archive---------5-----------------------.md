# 改善你的绘图的 4 种方法

> 原文：<https://towardsdatascience.com/4-ways-to-improve-your-plotly-graphs-517c75947f7e?source=collection_archive---------5----------------------->

## 使用 Plotly 的 Python 库提升您的数据可视化技能

![](img/26d108e58134faf79179f0be4b0c8314.png)

*照片由* [*艾萨克*](https://unsplash.com/@isaacmsmith?utm_source=medium&utm_medium=referral) *上* [*下*](https://unsplash.com?utm_source=medium&utm_medium=referral)

*注:发布后，我注意到 Plotly 的 embeds in Medium 存在一些问题。因此，您需要点击链接来查看图表的外观。为了更好的阅读体验，请看我的网站* *上的* [*原版。*](https://dylancastillo.co/4-ways-to-improve-your-plotly-graphs/)

最近几周，我一直在使用 Dash 和 Plotly 开发一个应用程序。如果你想快速得到一些东西，这些工具是很棒的。但是，像往常一样，没有神奇的`make_beautiful_graphs`参数可以默认设置为`True`。

如果你想在你的应用程序中有漂亮的和定制的可视化效果，你需要花一些时间在 Plotly 的图形属性的广泛列表上玩。我想拥有比默认外观更好的东西，所以我查阅了 Plotly 的文档、我拥有的旧代码片段和堆栈溢出问题。

这不是我第一次发现自己这样做。然而，这一次我决定跟踪我在用 Plotly 制作图表时经常做的事情。这样，下次我就不需要阅读相同的文档或浏览相同的堆栈溢出问题了。

在本文中，我列出了我在使用 Plotly 构建数据可视化时经常做的事情。放心，[我在数据相关岗位工作过一段时间](https://dylancastillo.co/about/)，所以你不会发现*如何制作 3D 饼状图*之类离谱的东西。虽然这些改进是基于一个例子，但我经常看到其他人应用类似的想法。

我将重点放在适用于大多数基本图表的实用而简单的改进上:散点图、折线图、条形图和一些统计图。在这里你会发现像*删除网格线*这样的事情，而不是像*为你的 4D 等高线图选择最佳颜色*这样的事情。

首先，我将简要介绍如何使用 Plotly 构建图形。接下来，我将提供一个改进列表及其背后的原因。最后，我将给出我发现在使用 Plotly 和其他绘图库时有用的其他建议。

# 如何使用 Plotly 制作图形

关于 Plotly 的内部运作，你需要知道三件事:

首先，在 Plotly 中制作图表本质上是填充一个 Python 字典。这本字典通常被称为*图*。

二、*图*字典中有两个键:*布局*和*数据。*在*布局中，*你定义了图形的外观，如排版、标题和轴。在 *data* 键中，您设置将要绘制的轨迹的值和信息。这可能类似于 X 的[1，2，3]，Y 的[5，3，9]和条形图类型。

最后，一旦您填充了*图*字典，它就会被序列化(转换)成一个 JSON 结构。然后，Plotly JavaScript 库使用这个结果数据结构来绘制图表。

就是这样。

那么，如何做出一个*图*？

做这件事有多种方法。最底层的方法是使用 Python 字典，最高层的方法是使用 *Plotly Express* 接口*。*我倾向于使用一个叫做*图形构造器*的中级接口。比使用 Python 字典更容易调试，比 *Plotly Express* 更灵活。

使用*图形构造器*制作图形的代码如下:

```
import plotly.graph_objects as go
import numpy as np
np.random.seed(42)

# Simulate data
returns = np.random.normal(0.01, 0.2, 100)
price = 100 * np.exp(returns.cumsum())
time = np.arange(100)

# Generate graph using Figure Constructor
layout = go.Layout(
    title="Historic Prices",
    xaxis_title="time",
    yaxis_title="price"
)

fig = go.Figure(
    data=go.Scatter(x=time, y=price),
    layout=layout
)   
fig.show()
```

这是结果图:

 [## 历史价格|由 Dylanjcastillo 制作的散点图| plotly

### 编辑描述

chart-studio.plotly.com](https://chart-studio.plotly.com/~dylanjcastillo/627/#/) 

# 改进列表

以下是我通常用来改善 Plotly 图形的一系列事情:

*   #1:去除网格线和背景色
*   #2:保持图表颜色一致
*   #3:使用峰值线来比较数据点
*   #4:移除浮动菜单，禁用缩放并调整点击行为

# #1:去除网格线和背景色

网格线是穿过图表以显示坐标轴刻度的线。它们帮助查看者快速可视化未标记数据点所代表的值。然而，在处理交互式图形时，网格线不是很有用。您可以将鼠标悬停在数据点上并查看其值。因此，在使用 Plotly 时，我通常会删除网格线。‌
‌

你可以这样做:

```
import plotly.graph_objects as go
import numpy as np
np.random.seed(42)

# Simulate data
returns = np.random.normal(0.01, 0.2, 100)
price = 100 * np.exp(returns.cumsum())
time = np.arange(100)

layout = go.Layout(
    title="Historic Prices",
    plot_bgcolor="#FFF",  # Sets background color to white
    xaxis=dict(
        title="time",
        linecolor="#BCCCDC",  # Sets color of X-axis line
        showgrid=False  # Removes X-axis grid lines
    ),
    yaxis=dict(
        title="price",  
        linecolor="#BCCCDC",  # Sets color of Y-axis line
        showgrid=False,  # Removes Y-axis grid lines    
    )
)

fig = go.Figure(
    data=go.Scatter(x=time, y=price),
    layout=layout
)   
fig.show()
```

这就是 looks:‌

 [## 历史价格|由 Dylanjcastillo 制作的散点图| plotly

### 编辑描述

plotly.com](https://plotly.com/~dylanjcastillo/627/) 

使用类别时，人们通常喜欢做两件事。首先，他们想给每个组分配一些特定的颜色。例如，如果您正在分析美国的选举结果，您可能希望使用特定的蓝色和红色变体来识别民主党和共和党。

# #2:在 graphs‌保持一致的颜色

第二，您希望这种颜色在所有图表中保持一致。例如，如果您正在分析一些现实世界中的公司，您可能希望使用它们与众不同的颜色来绘制它们的价格，但在分析它们的回报时也是如此。

以下是如何使用 Plotly 实现这一点:

```
import plotly.graph_objects as go
import numpy as np
np.random.seed(42)

# Simulate data
returns_A = np.random.normal(0.01, 0.2, 100)
returns_B = np.random.normal(0.01, 0.2, 100)
returns = np.append(returns_A, returns_B)

prices_A = 100 * np.exp(returns_A.cumsum())
prices_B = 100 * np.exp(returns_B.cumsum())
prices = np.append(prices_A, prices_B)

companies = ["A"] * 100 + ["B"] * 100
time = np.append(np.arange(100), np.arange(100))

df = pd.DataFrame({
    "company": companies,
    "time": time,
    "price": prices,
    "returns": returns
})

# Build graph
COLORS_MAPPER = {
    "A": "#38BEC9",
    "B": "#D64545"
}

layout = go.Layout(
    title="Performance of A vs. B",    
    plot_bgcolor="#FFFFFF",
    barmode="stack",
    xaxis=dict(
        domain=[0, 0.5],
        title="time",
        linecolor="#BCCCDC",
    ),
    yaxis=dict(
        title="price",
        linecolor="#BCCCDC"
    ),
    xaxis2=dict(
        domain=[0.6, 1],
        title="returns",
        linecolor="#BCCCDC",
    ),
    yaxis2=dict(
        anchor="x2",
        linecolor="#BCCCDC"
    )
)

data = []
for company,col in COLORS_MAPPER.items():
    time = df.loc[df.company == company, "time"]
    price = df.loc[df.company == company, "price"]
    returns = df.loc[df.company == company, "returns"]
    line_chart = go.Scatter(
        x=time,
        y=price,
        marker_color=col,  # Defines specific color for a trace
        legendgroup=company,  # Groups traces belonging to the same group in the legend 
        name=company
    )
    histogram = go.Histogram(
        x=returns,
        marker_color=col,  # Defines specific color for a trace
        legendgroup=company,  # Groups traces belonging to the same group in the legend 
        xaxis="x2", 
        yaxis="y2",
        showlegend=False
    )
    data.append(line_chart)
    data.append(histogram)

fig = go.Figure(data=data, layout=layout)
fig.show()
```

上面的代码片段允许您在处理共享相同类别的多个图表时保持一致的颜色。关键部分是`COLOR_MAPPER`字典及其在添加新轨迹时的使用。这本字典是您将在图表中使用的类别和颜色的映射。

每当你添加一个轨迹到一个图中，你可以通过从`COLOR_MAPPER`字典中获得正确的颜色来分配给`marker_color`属性。

结果图看起来像 follows:‌

 [## Dylanjcastillo 制作的 A 与 B |散点图的性能| plotly

### 编辑描述

plotly.com](https://plotly.com/~dylanjcastillo/671/) 

# #3:使用峰值线来比较数据点

尖线是悬停在数据上时出现的垂直线或水平线。这对于比较折线图和散点图中的值很有用。这就是你如何使用 Plotly 添加这些:

```
import plotly.graph_objects as go
import numpy as np
np.random.seed(42)

# Simulate data
returns_A = np.random.normal(0.01, 0.2, 100)
returns_B = np.random.normal(0.01, 0.2, 100)
returns = np.append(returns_A, returns_B)

prices_A = 100 * np.exp(returns_A.cumsum())
prices_B = 100 * np.exp(returns_B.cumsum())
prices = np.append(prices_A, prices_B)

companies = ["A"] * 100 + ["B"] * 100
time = np.append(np.arange(100), np.arange(100))

df = pd.DataFrame({
    "company": companies,
    "time": time,
    "price": prices,
    "returns": returns
})

# Build graph
layout = go.Layout(
    title="Performance of A vs. B",    
    plot_bgcolor="#FFFFFF",
    hovermode="x",
    hoverdistance=100, # Distance to show hover label of data point
    spikedistance=1000, # Distance to show spike
    xaxis=dict(
        title="time",
        linecolor="#BCCCDC",
        showspikes=True, # Show spike line for X-axis
        # Format spike
        spikethickness=2,
        spikedash="dot",
        spikecolor="#999999",
        spikemode="across",
    ),
    yaxis=dict(
        title="price",
        linecolor="#BCCCDC"
    )
)

data = []
for company in ["A", "B"]:
    time = df.loc[df.company == company, "time"]
    price = df.loc[df.company == company, "price"]
    returns = df.loc[df.company == company, "returns"]
    line_chart = go.Scatter(
        x=time,
        y=price,
        name=company
    )
    data.append(line_chart)

fig = go.Figure(data=data, layout=layout)
fig.show()
```

这是结果图:

 [## Dylanjcastillo 制作的 A 与 B |散点图的性能| plotly

### 编辑描述

plotly.com](https://plotly.com/~dylanjcastillo/700/) 

# #4:移除浮动菜单，禁用缩放并调整点击行为

我不太喜欢默认情况下自动添加到图表中的浮动菜单。它让图形看起来*酷*，但是我很少看到人们使用它。它有如此多的选项，以至于对于第一次看图表的人来说很困惑。通常，我会移除它。

另外，我喜欢重新定义另外两个用户交互参数。我更喜欢限制用户放大和更改单击图例中的轨迹的行为的能力。在 Plotly 中，默认情况下，如果您想单独检查一个轨迹，您必须双击该轨迹，而不是只单击它。这不是很直观，所以我倾向于颠倒这种行为。

这是您应用这些更改的方式:

```
import plotly.graph_objects as go
import numpy as np
np.random.seed(42)

# Simulate data
returns_A = np.random.normal(0.01, 0.2, 100)
returns_B = np.random.normal(0.01, 0.2, 100)
returns = np.append(returns_A, returns_B)

prices_A = 100 * np.exp(returns_A.cumsum())
prices_B = 100 * np.exp(returns_B.cumsum())
prices = np.append(prices_A, prices_B)

companies = ["A"] * 100 + ["B"] * 100
time = np.append(np.arange(100), np.arange(100))

df = pd.DataFrame({
    "company": companies,
    "time": time,
    "price": prices,
    "returns": returns
})

# Build graph
layout = go.Layout(
    title="Performance of A vs. B",    
    plot_bgcolor="#FFFFFF",
    legend=dict(
        # Adjust click behavior
        itemclick="toggleothers",
        itemdoubleclick="toggle",
    ),
    xaxis=dict(
        title="time",
        linecolor="#BCCCDC",
    ),
    yaxis=dict(
        title="price",
        linecolor="#BCCCDC"
    )
)

data = []
for company in ["A", "B"]:
    time = df.loc[df.company == company, "time"]
    price = df.loc[df.company == company, "price"]
    returns = df.loc[df.company == company, "returns"]
    line_chart = go.Scatter(
        x=time,
        y=price,
        name=company
    )
    data.append(line_chart)

fig = go.Figure(data=data, layout=layout)
fig.show(config={"displayModeBar": False, "showTips": False}) # Remove floating menu and unnecesary dialog box
```

这是结果图:

 [## Dylanjcastillo 制作的 A 与 B |散点图的性能| plotly

### 编辑描述

plotly.com](https://plotly.com/~dylanjcastillo/716/) 

# 其他建议

我发现有三件事对学习如何更好地实现数据可视化很有用:

1.  从你的听众那里获得反馈:这并不总是可能的。但是如果可以的话，总是优先考虑从那些将使用你的数据可视化的人那里获得输入。如果你在一个仪表板上工作，你应该做的第一件事是理解你的仪表板解决什么问题。然后看到用户与之交互。对你的时间有最高的投资回报率。
2.  查看 Cole Knaflic 的[用数据讲故事](http://www.storytellingwithdata.com/):如果你想提高你的数据可视化设计技能，这是一本很棒的书。它提供了许多实用的建议和引人注目的用例。
3.  [plotty 的图参考](https://plotly.com/python/reference/):习惯 plotty 的图参考和 plotty 的文档。你会经常用到它。总的来说，Plotly 的文档非常优秀。

# 结束语

我希望这些想法对你有用。可能会有一些事情和你没有共鸣，或者其他你觉得缺失的事情。如果是这样的话，请在下面的评论中告诉我。我很乐意更新这个并添加其他有价值的建议。

你可以在这里找到我写的其他文章[，或者如果你想了解我正在做的事情，你可以在](https://dylancastillo.co) [Twitter](https://twitter.com/_dylancastillo) 或 [LinkedIn](https://www.linkedin.com/in/dylanjcastillo/) 上关注我。

*本文原载于* [*我的博客*](https://dylancastillo.co/build-a-site-quickly-using-google-sheets-python-and-aws/) *。*