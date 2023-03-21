# 地理空间冒险。第二步:熊猫大战地质熊猫

> 原文：<https://towardsdatascience.com/geospatial-adventures-step-2-pandas-vs-geopandas-16e842d0e3a7?source=collection_archive---------35----------------------->

![](img/71cfef7b3932f78433456cc4cf657929.png)

使用 KeplerGL 生成

## 探索熊猫和 GeoPandas 之间的差异，并介绍一些真实世界的地理空间数据集，我们将在本系列的稍后部分进行分析。

*在我的* [*第一篇帖子*](https://medium.com/@datingpolygons/geospatial-adventures-step-1-shapely-e911e4f86361) *中介绍了 shapely 之后，是时候看看一些有趣的地理数据集了，为了做到这一点，我们不可能没有熊猫，更具体地说是地理熊猫。我假设你以前遇到过熊猫，会试着强调两者之间的一些区别，更重要的是，你如何将一个转换成另一个。*

如果您以前从未使用过 GeoPandas，我们将从头开始——继续安装它，特别是如果您想自己按照这篇文章中的步骤操作数据。你只需要一个值得信任的老皮普。

```
!pip install geopandas
```

如果你愿意(你绝对应该这样)，你可以在这里阅读这些文件。

让我们从导入熊猫和 GeoPandas 开始。

```
import pandas as pd
import geopandas as gpd
```

现在，在我们对这两个进行任何操作之前，我们需要一个数据集。在本文的最后有一些有趣的地理特定数据集的链接(一定要查看它们)，但让我们从相对简短但足够现实的东西开始:英国地方当局边界。我喜欢这个数据集的原因是，一方面，它只有 380 条记录，另一方面，它使用的边界和形状一点也不简单，所以它很好地说明了您可能会在现实生活中使用的数据集。我还认为这是对英国数据进行任何地理空间分析的一个很好的起点，因为它允许你将所有东西分成易于管理的小块。相信我——有时候真的很有帮助。

## 浏览数据集。英国地方当局界限。

所以，事不宜迟，数据集在这里[存放](http://geoportal.statistics.gov.uk/datasets/ae90afc385c04d869bc8cf8890bd1bcd_1)。点击你右边的下载按钮。您将获得一个压缩文件，文件名简短而甜美，大约 35mb(在撰写本文时，他们偶尔会更新)。继续解压缩，您将得到一个如下所示的文件夹:

![](img/aed2f46a9e64f557c86216f992273fc1.png)

我们要的是形状文件，带*的那个。shp 扩展。

据我所知，普通的熊猫不能处理这种格式，所以这是熊猫和地理熊猫的第一个区别，当然后者可以。让我们打开它:

```
la = gpd.read_file(
'Downloads/Local_Authority_Districts__December_2017__Boundaries_in_Great_Britain-shp/Local_Authority_Districts__December_2017__Boundaries_in_Great_Britain.shp'
)
```

确保您使用的是与您的系统相关的路径。如果你不确定你的笔记本默认使用的是哪个文件夹，只需在其中一个单元格中运行即可——你将获得当前的目录内容，因此你应该能够从这里找到下一步。如果你需要进入一个文件夹，使用<../>。

让我们来看看数据是什么样子的。所有标准的 Pandas 命令都有效，所以我们可以这样做:

```
la.head()
```

得到

![](img/398292e1084a9cdbad582acbc0971873.png)

不错！好吧，这到底是什么意思？

让我们一栏一栏地来看:
**objectid** 相当简单，坦率地说，对我们来说不是很有用。
**lad17cd** 为当地权限代码。这实际上非常方便，因为它与 ONS 的超级输出区域等一起使用，所以这给了我们一个很好的参考系统，可以在不同的地理分区和相应的数据集之间来回移动。非英国人——我很抱歉在这里表现得如此党派化，事实是，大多数国家看待事物的方式都有些相似，所以即使你发现这不是直接的兴趣所在，这仍然是可以转移和有用的。
**lad17nm:** 地方机关名称。其中有些很诡异很奇妙，绝对值得一探究竟。没有吗？那就只有我…
**lad17nmw，**里面好像都是看起来友好的<没有> s(抱歉，忍不住)。这一列也有名称，而“ *w* ”有点泄露了这一点——这是威尔士的地方当局名称，因此前五个记录中没有值，因为这些都是英格兰的地方当局。如果你想知道的话，我们要到 317 号才能离开英国。
**bng_e** 和**bng _ n**——邪恶的双胞胎。说真的，这些是东/北——我稍后会再多谈一点。基本上，坐标显示这个地区在地图上的位置。
**长**和**横**——这次多了一些坐标——经纬度。

**st_lengths** —边界长度(也以米为单位)。
最后，这是我们真正需要的！请敲鼓。
**几何图形**-这是实际的多边形，或者在某些情况下，是多重多边形。不要让我开始谈论苏格兰群岛…Geometry 是 GeoPandas 表的关键属性，许多使用 geo pandas 表的应用程序基本上都假设这个列在那里，并且它被准确地称为“geometry”。所以，不管出于什么原因，如果你想给它取一个更友好的名字，请三思。

在我们进一步深入之前，因为我们要比较 Pandas 和 GeoPandas，我们可以用常规的 Pandas 格式创建这个表的副本。

```
la_pd = pd.DataFrame(la)
la_pd.head()
```

你会注意到这张新桌子看起来完全一样。不仅如此，几何表中的对象仍然非常完整:

![](img/e71a39071841405fbc11ca66f9b8aed4.png)![](img/79ae43d38a43cf1c64e921e5e2a4f5ca.png)

到目前为止一切顺利。我们还可以像在 Pandas 中一样查看数据帧的所有统计数据，这两个表看起来是一样的，所以我将只在这里显示 GeoPandas 的结果(您必须相信我，不是吗？好的，在您自己的笔记本上运行它，以便检查)。

![](img/71c658279b03c4204fa21429cf930ffb.png)

注意，这个函数只给出数字数据列的统计信息。

在[9]中:

```
la.info()
```

Out[9]:

```
<class 'geopandas.geodataframe.GeoDataFrame'>
RangeIndex: 380 entries, 0 to 379
Data columns (total 11 columns):
objectid      380 non-null int64
lad17cd       380 non-null object
lad17nm       380 non-null object
lad17nmw      22 non-null object
bng_e         380 non-null int64
bng_n         380 non-null int64
long          380 non-null float64
lat           380 non-null float64
st_areasha    380 non-null float64
st_lengths    380 non-null float64
geometry      380 non-null geometry
dtypes: float64(4), geometry(1), int64(3), object(3)
memory usage: 32.7+ KB
```

最后:

在[10]中:

```
la.memory_usage()
```

Out[10]:

```
Index           80
objectid      3040
lad17cd       3040
lad17nm       3040
lad17nmw      3040
bng_e         3040
bng_n         3040
long          3040
lat           3040
st_areasha    3040
st_lengths    3040
geometry      3040
dtype: int64
```

## 深部热疗

到目前为止，一切顺利。是时候发现另一个不同之处了。此命令不适用于您的熊猫数据帧:

在[11]中:

```
la.crs
```

Out[11]:

```
<Projected CRS: EPSG:27700>
Name: OSGB 1936 / British National Grid
Axis Info [cartesian]:
- E[east]: Easting (metre)
- N[north]: Northing (metre)
Area of Use:
- name: UK - Britain and UKCS 49°46'N to 61°01'N, 7°33'W to 3°33'E
- bounds: (-9.2, 49.75, 2.88, 61.14)
Coordinate Operation:
- name: British National Grid
- method: Transverse Mercator
Datum: OSGB 1936
- Ellipsoid: Airy 1830
- Prime Meridian: Greenwich
```

哇哦。！！那是什么意思？！还记得我答应过要扩展北/东吗？是时候看看坐标参考系统了。你可以在这里阅读一个更专业的 GeoPandas 作为具体的解释[。简而言之，定义坐标系并不像听起来那么简单。这里的主要问题是地球有不平坦的大胆。不仅如此——它甚至不是球形的，所以在表面上定义一个精确的点可能很棘手(如果你不相信我——试着用一张方面纸包裹橄榄球)。更有趣的是，因为它太大了——有时假装它是平的是有道理的，因为在规模上甚至大到足以覆盖整个国家，如英国，这不会引入足够的误差来真正担心。因此，您可能会遇到两个主要的参考系统——经度和纬度(伪 3D)和北距/东距(2D)。我不得不承认，在大多数情况下，我更喜欢和后者一起工作。原因是——这很容易，而且我很懒。不过说真的，北距和东距都是用米来表示的，并且测量的是直接的距离。所以，如果你想知道事物之间的距离，你所要做的就是得到 Xs(东距)和 Ys(北距)之间的差异，应用毕达哥拉斯，就大功告成了。请注意，我们在之前的帖子中谈到的 Shapely 库也内置了 CRS 的概念，但是，我们并不真的需要使用它，因为 GeoPandas 为我们提供了所有可能需要的高精度弹药。](https://geopandas.org/projections.html)

## CRS 转换

GeoPandas 中有许多 CRS 系统。上表中使用的这个特别的是“epsg:27700”——如果你计划使用英国数据，这是你会经常使用的东西，所以你要记住它，要小心。在英国，使用 lat/long 的另一种方法是“epsg:4326”。GeoPandas 为我们提供了一种简单的转换方法。我们要做的就是跑:

```
la_4326 = la.to_crs("epsg:4326")
la_4326.head()
```

获得:

![](img/1ea8c9ab64b8aa174b01f882c45b6c4a.png)

眼尖的人会发现，这个表中唯一的变化是我们的多边形/多多边形对象列，现在每个对象都由引用纬度和经度的点组成。您仍然可以看到与我们之前看到的相同的图形表示，它看起来应该是相同的。我会让你来验证的。

## 保存和读取非地理特定格式(csv)。几何对象转换。

在我们在这些之上进行一些计算之前，让我们看一下保存到 csv 和从 Pandas 转换到 GeoPandas。在 Pandas 的较新版本中，您应该能够将数据帧直接保存到 csv 文件中，尽管最后一列中有几何对象，即

```
la.to_csv('Downloads/la.csv', compression='gzip')
```

会成功的。更重要的是，结果文件只有 39.9mb，而 shapefile 只有 63.7mb(当然，我们已经在这里应用了压缩)。然而，这是有代价的。据我所知，您不能直接从 GeoPandas 读取 csv 文件，所以您必须将其作为普通的数据帧加载回来。只需运行:

```
la_new = pd.read_csv('Downloads/test.csv', compression='gzip')
```

乍一看，没什么变化:

```
la_new.head()
```

![](img/91d6b16971e38454cbff1a3ed5010113.png)

本质上，我们有一个单独的额外列，通过运行

```
la_new = la_new[la_new.columns[1:]]
```

然而，这还不是全部，如果我们像以前一样尝试查看我们的一个几何对象，我们会得到非常不同的结果:

在[17]

```
la_new['geometry'].iloc[0]
```

Out[17]:

```
'MULTIPOLYGON (((447213.8995000003 537036.1042999998, 447228.7982999999 537033.3949999996, 447233.6958999997 537035.1045999993, 447243.2024999997 537047.6009999998, 447246.0965 537052.5995000005, 447255.9988000002 537102.1953999996, 447259.0988999996 537108.8035000004, 447263.6007000003 537113.8019999992, 447266.1979 537115.6015000008, 447273.1979999999 537118.6007000003, 447280.7010000004 537120.1001999993, 447289.3005999997 537119.6004000008, 447332.2986000003 537111.5026999991, 447359.5980000002 537102.3953000009, 447378.0998999998 537095.0974000003, 447391.0033999998 537082.9009000007, 447434.6032999996 537034.5046999995, 447438.7011000002 537030.9956999999, 447443.7965000002 537027.6966999993...
```

好吧，我在这里作弊，在现实中，输出比这长得多。实际上，我们所有的几何对象都被转换成了字符串。然而，这一切并没有失去，因为 shapely 为我们提供了一种将它们转换回来的方法。除了在这里加载 shapely，我还需要更快的库。通过使用直接应用方法，您可以不使用它，但除此之外，swifter 为您提供了一个很好的进度条和时间估计(它也可以提高性能，但这超出了本文的范围)。

```
import swifter
from shapely import wkt
la_new['geometry1'] = la_new['geometry'].swifter.apply(lambda x: wkt.loads(x))
```

这里发生的事情是，我将转换函数应用于几何列的每个元素，并将输出存储在新列中。在我的笔记本电脑上，在我们的 380 记录长的数据集上，这需要 12 秒。我在非常非常大的数据集上做过，公平地说，还不算太差。

然后，我们可以通过运行以下命令来抽查新元素是否确实是几何图形对象

```
la_new['geometry1'].iloc[0]
```

但是我们还没有完成。事实上，如果我们运行 la_new.info()，我们会得到以下结果:

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 380 entries, 0 to 379
Data columns (total 12 columns):
 #   Column      Non-Null Count  Dtype  
---  ------      --------------  -----  
 0   objectid    380 non-null    int64  
 1   lad17cd     380 non-null    object 
 2   lad17nm     380 non-null    object 
 3   lad17nmw    22 non-null     object 
 4   bng_e       380 non-null    int64  
 5   bng_n       380 non-null    int64  
 6   long        380 non-null    float64
 7   lat         380 non-null    float64
 8   st_areasha  380 non-null    float64
 9   st_lengths  380 non-null    float64
 10  geometry    380 non-null    object 
 11  geometry1   380 non-null    object 
dtypes: float64(4), int64(3), object(5)
memory usage: 35.8+ KB
```

新列显示为对象类型，而不是几何图形类型。但是，这不会阻止我们将其转换为地理数据框架。我们所要做的就是去掉我们不再需要的额外的 geometry 列，并将 geometry1 重命名为 geometry(记住—必须这样命名),然后(！)这一点很重要，在转换为地理数据框架后，我们必须为其设置 crs，

```
la_new_geo = la_new.drop(columns=['geometry']).rename(columns={'geometry1': 'geometry'})
la_new_geo = gpd.GeoDataFrame(la_new_geo)
la_new_geo.crs = 'epsg:27700'
```

瞧啊。la_new_geo.info()为我们提供了:

```
<class 'geopandas.geodataframe.GeoDataFrame'>
RangeIndex: 380 entries, 0 to 379
Data columns (total 11 columns):
 #   Column      Non-Null Count  Dtype   
---  ------      --------------  -----   
 0   objectid    380 non-null    int64   
 1   lad17cd     380 non-null    object  
 2   lad17nm     380 non-null    object  
 3   lad17nmw    22 non-null     object  
 4   bng_e       380 non-null    int64   
 5   bng_n       380 non-null    int64   
 6   long        380 non-null    float64 
 7   lat         380 non-null    float64 
 8   st_areasha  380 non-null    float64 
 9   st_lengths  380 non-null    float64 
 10  geometry    380 non-null    geometry
dtypes: float64(4), geometry(1), int64(3), object(3)
memory usage: 32.8+ KB
```

## 基本计算示例

最后，对我们新的闪亮地理数据框架进行一些快速计算:

验证长度和面积:

```
la_new_geo['area']=la_new_geo['geometry'].swifter.apply(lambda x: x.area)
la_new_geo['length'] = la_new_geo['geometry'].swifter.apply(lambda x: x.length)
la_new_geo[['lad17cd', 'lad17nm', 'st_areasha', 'st_lengths', 'area', 'length']].head()
```

![](img/a8bead91a7a020cf9c27eddc5b495581.png)

或者，如果我们想要验证整个列，我们可以这样做:

```
(la_new_geo.st_lengths == la_new_geo.length).all()
```

您会注意到，由于舍入误差，这实际上会返回 False。例如，在第一条记录中，st_lengths 的值是 71，707.455227013，length 的值是 7 . 1676767771

因此，要进行检查，我们可以以设定的精度运行它:

```
(la_new_geo.st_lengths.round(4)==la_new_geo.length.round(4)).all()
```

在任何情况下，当处理地理对象时，考虑到现有数据的质量，移动超过 25 厘米的精度可能是不合理的，对于大多数应用程序，您可能可以将其限制在最近的米。

让我们也看看哪个区域具有最复杂的多边形/多多边形。我们将通过描述它所需的点数来判断。本质上，这意味着计算外部和内部边界的点:

```
la_new_geo['point_count'] = la_new_geo['geometry'].swifter.apply(
    lambda x: np.sum([len(np.array(a.exterior)) + np.sum([len(np.array(b)) for b in a.interiors]) for a in x]) if x.type == 'MultiPolygon' 
    else len(np.array(x.exterior)) + np.sum([len(np.array(a)) for a in x.interiors])
)
```

好了，这个有点复杂，我们来看看是怎么回事。首先，我们正在寻找多多边形对象，如果我们有一个，我们必须迭代通过每个多边形，将其转换为外部边界，然后 numpy 数组获得一个坐标数组。我们取数组的长度。我们还获得了内部边界的列表，因此我们还必须遍历它们，将它们转换为数组并取长度。类似地，如果我们处理一个简单的多边形，我们减少一级迭代，所以只有一个外部边界，但是我们仍然需要迭代潜在的多个洞。

然后我们对这些值进行排序，瞧，我们得到了前十名:

```
la_new_geo[
    ['lad17cd', 'lad17nm', 'point_count']
].sort_values('point_count', ascending=False).head(10)
```

![](img/847eb04e5748f3aabbb0f29213b1062a.png)

这就是为什么每个人都如此热爱苏格兰。

没有吗？那就只有我了…

下一个帖子是关于多边形相互匹配，人类最酷的发明——R 树(Erm..算是)。嗯……好吧，也许还有一两张照片。

最后是承诺的…

## 数据集

世界各地的 OSM 数据集:[http://download.geofabrik.de/](http://download.geofabrik.de/)

英国的 OSM 数据集:[http://download.geofabrik.de/europe/great-britain.html](http://download.geofabrik.de/europe/great-britain.html)

ONS 开放数据:[https://www . ordnance survey . co . uk/opendata download/products . html](https://www.ordnancesurvey.co.uk/opendatadownload/products.html)

NOMIS(劳动力市场/人口普查数据):
-这些通常包含特定区域的数据，然后您可以通过使用它们的边界形状文件(通过 ID 匹配它们)将这些数据与地图相关联

我相信还有很多很多，请在评论中随意添加任何有趣的地理空间数据集(最好是免费使用的)。

回头见…

**还在这个系列:**

[*地理空间冒险。第一步:匀称。*](https://medium.com/@datingpolygons/geospatial-adventures-step-1-shapely-e911e4f86361)

[*地理空间历险记。第三步。多边形长在 R 树上*](https://medium.com/analytics-vidhya/geospatial-adventures-step-3-polygons-grow-on-r-trees-2f15e2712537)

[*地理空间历险记。第四步。魔法的颜色或者如果我没有看到它——它就不存在*](https://medium.com/@datingpolygons/geospatial-adventures-step-4-the-colour-of-magic-or-if-i-dont-see-it-it-doesn-t-exist-56cf7fb33ba9)

[*地理空间历险记。第五步。离开平地或飞越多边形的海洋*](https://medium.com/@datingpolygons/geospatial-adventures-step-5-leaving-the-flatlands-or-flying-over-the-sea-of-polygons-846e45c7487e)