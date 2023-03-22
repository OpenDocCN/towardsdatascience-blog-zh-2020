# Python 中的交互式 Choropleth 地图

> 原文：<https://towardsdatascience.com/a-step-by-step-guide-to-interactive-choropleth-map-in-python-681f6bd853ce?source=collection_archive---------21----------------------->

## 学习使用 Python 的叶库轻松开发 Choropleth 地图

Choropleth 地图是最有趣和最有用的可视化工具之一。它们很重要，因为它们可以提供地理位置信息，它们看起来很漂亮，在演示中能吸引注意力。几个不同的库可以用来做这件事。在本教程中，我将使用叶。

## 什么是 choropleth 地图？

这是来自维基百科的定义:

> Choropleth 图提供了一种简单的方法来可视化一个地理区域内的测量值如何变化，或显示一个区域内的可变性水平。一张[热图](https://en.wikipedia.org/wiki/Heat_map)或[是一张](https://en.wikipedia.org/wiki/Isarithmic_map)类似但不使用*先验*地理区域。它们是最常见的专题地图类型，因为发布的统计数据(来自政府或其他来源)通常被聚合到众所周知的地理单元中，如国家、州、省和县，因此使用 [GIS](https://en.wikipedia.org/wiki/Geographic_information_system) 、[电子表格](https://en.wikipedia.org/wiki/Spreadsheet)或其他软件工具创建它们相对容易。

用简单易懂的话来说，choropleth 地图是通过在地图上使用颜色来显示地理位置信息的地图。看下面的一些图片，获得更多的理解。

## 数据准备

数据准备是所有数据科学家的一项重要而普遍的任务。我在这里使用的数据集相当漂亮和干净。但是对于这个可视化，我仍然需要做一些工作。让我们导入必要的库和数据集。

```
import pandas as pd
import numpy as npdf = pd.read_excel('[https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/DV0101EN/labs/Data_Files/Canada.xlsx'](https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/DV0101EN/labs/Data_Files/Canada.xlsx'),
                     sheet_name='Canada by Citizenship',
                     skiprows=range(20),
                     skipfooter=2)
```

我不能在这里显示数据集的截图，因为它太大了。我鼓励你自己运行代码。这是唯一的学习方法。

该数据集包含从 1980 年到 2013 年有多少来自世界不同国家的移民来到加拿大。让我们看看数据集的列名，以了解该数据集包含的内容:

```
df.columns#Output:
Index(['Type', 'Coverage', 'OdName', 'AREA', 'AreaName', 'REG', 'RegName', 'DEV', 'DevName', 1980, 1981, 1982, 1983, 1984, 1985, 1986, 1987, 1988, 1989, 1990, 1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998, 1999, 2000, 2001, 2002, 2003, 2004, 2005, 2006, 2007, 2008, 2009, 2010, 2011, 2012, 2013],
dtype='object')
```

**我们要绘制每个国家从 1980 年到 2013 年的移民总数。**

我们需要国名和年份。从数据集中删除一些不必要的列。

```
df.drop(['AREA', 'REG', 'DEV', 'Type', 'Coverage', 'AreaName', 'RegName', 'DevName'], axis=1, inplace=True)
```

“OdName”列是国家的名称。为了便于理解，将其重命名为“国家”。

```
df.rename(columns={'OdName':'Country'}, inplace=True)
```

现在，做一个“总数”栏，这是每个国家所有年份的移民总数。

```
df['Total'] = df.sum(axis=1)
```

![](img/932cd2ba681c94b6c4a590eef440b87d.png)

看，我们在最后有“总计”栏。它给出了每个国家的移民总数。

记住将这个**轴**设置为 1 是很重要的。它说求和操作应该是跨列的。否则，它将跨行求和，我们将得到每年的移民总数，而不是每个国家的移民总数。

## 基本 Choropleth 图

我将在这里展示，如何一步一步地绘制出一张 choropleth 地图。进口叶。如果您没有 lyum，请在 anaconda 提示符下运行以下命令进行安装:

```
conda install -c conda-forge folium
```

现在导入叶子，生成世界地图。

```
import folium
world = folium.Map(location=[0,0], zoom_start=2)
```

![](img/f5d6b0422231b31ffd64f3ef34dc6a0c.png)

现在在这个世界地图中，我们将设置我们的数据。但它也需要包含每个国家坐标的地理数据。**从此链接下载地理数据**[](https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/DV0101EN/labs/Data_Files/world_countries.json)****。我已经下载并把它放在了我在本教程中使用的笔记本所在的文件夹中。我现在只需要看那份文件。****

```
wc = r'world-countries.json'
```

**对于这张 choropleth 地图，你需要传递**

1.  **我们在上面保存为“wc”的地理数据，**
2.  **数据集，**
3.  **我们需要从数据集中使用的列，**
4.  **来自地理数据的“钥匙开启”参数。“key_on”参数的值始终以“feature”开头。然后，我们需要添加我们保存为“wc”的 geo_data 中的键。那个 JSON 文件太大了。因此，我展示了其中的一部分来解释 key_on 参数:**

```
{"type":"Feature","properties":{"name":"Afghanistan"},"geometry":{"type":"Polygon","coordinates":[[[61.210817,35.650072],[62.230651,35.270664],[62.984662,35.404041],[63.193538,35.857166],[63.982896,36.007957],[64.546479,36.312073],[64.746105,37.111818],[65.588948,37.305217],[65.745631,37.661164],[66.217385,37.39379],[66.518607,37.362784],[67.075782,37.356144],[67.83,37.144994],[68.135562,37.023115],[68.859446,37.344336],[69.196273,37.151144],[69.518785,37.608997],[70.116578,37.588223],[70.270574,37.735165],[70.376304,38.138396],[70.806821,38.486282],[71.348131,38.258905],[71.239404,37.953265],[71.541918,37.905774],[71.448693,37.065645],[71.844638,36.738171],[72.193041,36.948288],[72.63689,37.047558],[73.260056,37.495257],[73.948696,37.421566],[74.980002,37.41999],[75.158028,37.133031],[74.575893,37.020841],[74.067552,36.836176],[72.920025,36.720007],[71.846292,36.509942],[71.262348,36.074388],[71.498768,35.650563],[71.613076,35.153203],[71.115019,34.733126],[71.156773,34.348911],[70.881803,33.988856],[69.930543,34.02012],[70.323594,33.358533],[69.687147,33.105499],[69.262522,32.501944],[69.317764,31.901412],[68.926677,31.620189],[68.556932,31.71331],[67.792689,31.58293],[67.683394,31.303154],[66.938891,31.304911],[66.381458,30.738899],[66.346473,29.887943],[65.046862,29.472181],[64.350419,29.560031],[64.148002,29.340819],[63.550261,29.468331],[62.549857,29.318572],[60.874248,29.829239],[61.781222,30.73585],[61.699314,31.379506],[60.941945,31.548075],[60.863655,32.18292],[60.536078,32.981269],[60.9637,33.528832],[60.52843,33.676446],[60.803193,34.404102],[61.210817,35.650072]]]},"id":"AFG"}
```

**在 properties 键中，我们有国家的名称。这就是我们需要传递的。因此，key_on 参数的值将是“feature.properties.name”。**

**5.我还将使用一些样式参数:fill_color、fill_opacity、line_opacity 和 legend_name。我觉得这些都是不言自明的。**

**这是我们第一张 choropleth 地图的代码:**

```
world.choropleth(geo_data=wc,
                data=df,
                columns=['Country', 'Total'],
                key_on='feature.properties.name',
                fill_color='YlOrRd',
                fill_opacity=0.8,
                line_opacity=0.2,
                legend_name='Immigration to Canada'
                )
world
```

**![](img/dea519b037439808b8d5dcefe0a51c87.png)**

**这张地图是互动的！你可以用鼠标导航。而且，它会随着强度改变颜色。颜色越深，越多的移民从那个国家来到加拿大。但是黑色意味着没有可用的数据或者没有移民。**

## **添加图块**

**这张地图可能看起来有点平面。我们可以用瓷砖让它看起来更有趣:**

```
world_map = folium.Map(location=[0, 0], zoom_start=2, tiles='stamenwatercolor')
world_map.choropleth(geo_data=wc,
                     data=df,
                     columns=['Country', 'Total'],
                     threshold_scale=threshold_scale,
                     key_on='feature.properties.name',
                     fill_color='YlOrRd',
                     fill_opacity=0.7,
                     line_opacity=0.2,
                     legend_name='Immigration to Canada'
                    )
```

**![](img/64756bec395c75b6db9f7f669b9a4f3b.png)**

**是不是更好看！我们可以通过使用一些瓷砖来使它变得更有趣，这将为我们提供根据需求更改瓷砖的选项。我们将使用 follow 的 TileLayer 方法在地图上添加不同的平铺层。最后，我们还将包含 LayerControl 方法，以获得更改图层的选项。**

```
world = folium.Map(location=[0, 0], zoom_start=2, tiles='cartodbpositron')
tiles = ['stamenwatercolor', 'cartodbpositron', 'openstreetmap', 'stamenterrain']
for tile in tiles:
    folium.TileLayer(tile).add_to(world)

world.choropleth(
    geo_data=wc,
    data=df,
    columns=['Country', 'Total'],
    threshold_scale=threshold_scale,
    key_on='feature.properties.name',
    fill_color='YlOrRd', 
    fill_opacity=0.7, 
    line_opacity=0.2,
    legend_name='Immigration to Canada',
    smooth_factor=0
)folium.LayerControl().add_to(world)
world
```

**![](img/adb3facf8f4cc37a5c305229339c45a5.png)**

**看，在传说的右上角下面，有一堆瓷砖。如果你点击它，你会得到一个列表。您可以在那里更改瓷砖样式。我觉得这个选项很酷！**

## **添加信息标签**

**最后，我想向您展示另一个有用且有趣的选项。那就是使用一个信息标签。我们不能指望每个人通过看地图就知道国家的名字。地图上有国家的标签会很有用。我们会让它变得有趣。Folium 有一个名为“GeoJsonTooltip”的功能可以做到这一点。首先，我们需要像往常一样制作世界地图。将所有参数添加到其中并保存在一个变量中。然后使用带有 add_child 方法的“GeoJsonTooltip”添加此附加功能。这是完整的代码。**

```
world = folium.Map(location=[0,0], zoom_start=2, tiles='cartodbpositron')
choropleth = folium.Choropleth(geo_data=wc,
    data=df,
    columns=['Country', 'Total'],
    threshold_scale=threshold_scale,
    key_on='feature.properties.name',
    fill_color='YlOrRd', 
    fill_opacity=0.7, 
    line_opacity=0.2,
    legend_name='Immigration to Canada',
).add_to(world)choropleth.geojson.add_child(
    folium.features.GeoJsonTooltip(['name'], labels=False))
world
```

**![](img/21221dcfbdd23f3ba79ab2f95f7a63b5.png)**

**注意，我把光标放在法国，它显示法国这个名字。同样的方法，你可以把光标放在地图上的任何地方，得到这个地方的名字。**

## **结论**

**我想展示如何开发一个交互式 choropleth 地图，设计它的样式，并向它添加信息标签。我希望它有帮助。**

## **阅读推荐**

**[](/interactive-geospatial-data-visualization-in-python-490fb41acc00) [## Python 中的交互式地理空间数据可视化

### 绘制世界特定地区的地图，在地图上展示活动，并四处导航

towardsdatascience.com](/interactive-geospatial-data-visualization-in-python-490fb41acc00) [](/waffle-charts-using-pythons-matplotlib-94252689a701) [## 使用 Python 的 Matplotlib 的华夫饼图表

### 如何使用 Matplotlib 库在 Python 中绘制华夫饼图表

towardsdatascience.com](/waffle-charts-using-pythons-matplotlib-94252689a701) [](/bubble-plots-in-matplotlib-3f0b3927d8f9) [## Matplotlib 中的气泡图

### 通过使用 Python 的 Matplotlib 库的例子学习绘制气泡图

towardsdatascience.com](/bubble-plots-in-matplotlib-3f0b3927d8f9) [](/exploratory-data-analysis-intro-for-data-modeling-8ff019362371) [## 用于数据建模的探索性数据分析

### 如何了解数据集，定义因变量和自变量，计算相关系数…

towardsdatascience.com](/exploratory-data-analysis-intro-for-data-modeling-8ff019362371) [](/a-complete-guide-to-confidence-interval-and-examples-in-python-ff417c5cb593) [## 置信区间的完整指南，以及 Python 中的示例

### 对统计学中一个非常流行的参数——置信区间及其计算的深入理解

towardsdatascience.com](/a-complete-guide-to-confidence-interval-and-examples-in-python-ff417c5cb593) [](/generate-word-clouds-of-any-shape-in-python-e87f265f6352) [## 在 Python 中生成任意形状的单词云

### 学习生成一个单词云，设计它的样式并使用自定义形状

towardsdatascience.com](/generate-word-clouds-of-any-shape-in-python-e87f265f6352)**