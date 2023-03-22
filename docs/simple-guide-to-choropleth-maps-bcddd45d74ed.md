# COVID 19 Choropleth 图

> 原文：<https://towardsdatascience.com/simple-guide-to-choropleth-maps-bcddd45d74ed?source=collection_archive---------62----------------------->

## Choropleth 地图使用 Plotly 追踪 COVID 19 例。

![](img/fa248168bfd7b56e686324956a8b3484.png)

Ashkan Forouzani 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

这里有一个使用 Python 中的 Plotly 图形库绘制 Choropleth 地图的简单教程。

我从 [**世卫组织仪表盘**](https://covid19.who.int/) 得到灵感，想自己实现一个。

> 事实证明这很容易做到，虽然这篇文章不是我通常的随机数据集系列[的一部分](https://medium.com/@shraddha.anala)，但像这样学习数据可视化技能，对于数据科学家交流见解非常重要。

让我们直接进入教程。

## 1)下载并清理数据集-

你可以从 [**世卫组织站点**](https://covid19.who.int/info) 下载数据集，但是要注意有很多特殊字符要删除。

此外，一些国家，如库拉索岛、圣巴泰勒米岛、留尼汪岛等。，格式不正确。因此，在将 CSV 文件导入熊猫数据框架之前，有大量的清理工作要做。

你可以跳过上面的麻烦，下载我已经清理过的数据集 的 [**，但是这可能意味着在将来你决定为自己实现本教程的任何时候，数据都不是最新的。**](https://github.com/shraddha-an/covid-choro-map/blob/master/WHO-COVID-19-global-data.csv)

## 2)导入清理后的数据集-

既然您已经自己清理了数据集，或者已经下载了预处理过的数据集，下一步就是导入这个数据集。

我们将删除一些不需要绘图的列。

## 3) Choropleth 地图

使用 Plotly 图形库绘制 Choropleth 地图非常容易。

我们必须创建一个数据对象，其中包含每个国家及其感染/死亡人数的实际信息，并创建一个布局对象来查看图表。

有 2 个 choropleth 图；一个追踪全世界的感染人数，另一个显示 COVID 死亡人数。

COVID 19 感染计数。作者 GIF

> 在中型文章中直接嵌入交互式 Plotly 可视化不再有效，所以我捕捉了一张我在不同国家上空盘旋的 gif 图，向你展示地图的样子。您可以通过点击下面的链接来查看交互式可视化。

 [## COVID 19:全球感染计数| choropleth 由 Shrad09 | plotly 制作

### 编辑描述

plotly.com](https://plotly.com/~shrad09/1/) 

这是你绘制这张地图的方法。

```
Once you execute the line, plot(fig_cases), the Choropleth Map will open up in your browser as an interactive map.
```

用同样的方法绘制世界各地的死亡率。

COVID 19 伤亡人数。作者 GIF。

> 再一次，点击下面的链接，你可以随意地玩这个交互式可视化游戏。

 [## COVID 19:全球死亡人数| choropleth 由 Shrad09 | plotly 制作

### 编辑描述

plotly.com](https://plotly.com/~shrad09/3/) 

这就对了。以下是如何用一种简单的方式绘制 Choropleths 地图。要记住的重要一点是以易于绘制的方式设置数据集。

如果你自己已经实现了一个，并且想弄清楚如何在媒体上嵌入你的交互式可视化，你可以看看这篇文章[](/how-to-create-a-plotly-visualization-and-embed-it-on-websites-517c1a78568b)**。**

**希望你觉得这个教程有用和有趣。可以看看我的其他 [**文章**](https://medium.com/@shraddha.anala) 有趣的机器学习教程和 [**我的 GitHub repo。**](https://github.com/shraddha-an)**

**非常感谢您的阅读，我们很快会再见的！**