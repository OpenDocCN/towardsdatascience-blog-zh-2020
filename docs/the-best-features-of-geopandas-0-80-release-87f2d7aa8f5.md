# Geopandas 0.8.0 版本的最佳特性

> 原文：<https://towardsdatascience.com/the-best-features-of-geopandas-0-80-release-87f2d7aa8f5?source=collection_archive---------28----------------------->

## 使用 PyGeo 加速地理空间操作，使用示例代码改进 IO

![](img/96817933391de80709729f3d32d512a3.png)

来自画布的基本图像(用画布创建)

我喜欢在我的地理空间数据科学项目中使用 [Geopandas](https://geopandas.org/) 。多亏了它的开发者，Geopandas 在每个版本中都变得更好。如果您曾经处理过地理空间矢量数据，那么您很可能使用过 Geopandas。

在最新的 0.8.0 版本中，有很多改进和错误修复。特别是，我对 PyGEOS 集成加速地理空间操作感到兴奋。虽然这还处于实验阶段，但我已经看到了使用这个选项对 Geopandas 的一些改进。

此版本中还有许多 IO 增强功能，包括 PostGIS 改进以及大量新的 IO 功能，包括 GeoFeather 和 Parquet 数据类型。

在本文中，我将通过一些代码示例介绍这两个特性，PyGEOS 集成和 IO 增强。

# PyGEOS 选项

Geopandas 作为空间操作的默认后端仍然是 Shapely，但是您可以选择使用 PyGeos 来加速地理空间过程。

> PyGEOS 是一个带有矢量几何函数的 C/Python 库。几何操作在开源几何库 GEOS 中完成。PyGEOS 将这些操作包装在 NumPy ufuncs 中，在操作几何数组时提供了性能改进。— [PyGE](https://pygeos.readthedocs.io/en/latest/) OS

根据官方发布，PyGeos 集成是试验性的，仅支持 Geopandas 中可用的一些空间操作。然而，它已经改善并加速了我经常使用的一些空间处理。我用空间连接操作对它进行了测试，这是空间数据的基本空间操作之一。

我将使用著名的纽约市出租车数据的子集，其中包含 160 万行和出租车区域多边形，以找出每个点在哪个多边形内。结果是使用 PyGEOS 和不使用 py geos 在运行时间上略有改进。

## 没有 PyGEOS 的空间连接

```
import geopandas as gpd
import time# Set PyGEOS to False 
gpd.options.use_pygeos = False
print(gpd.options.use_pygeos)# Read the data
gdf = gpd.read_file(“data/taxidata.shp”)
zones = gpd.read_file(“data/taxizones.shp”)# Spatial Join
start_time = time.time() 
sjoined = gpd.sjoin(gdf, zones, op=”within”)
print(“It takes %s seconds” % (time.time() — start_time))
```

如果没有 PyGEOS 选项，完成该过程需要 **1.8 秒**。同样的代码在 Geopandas 0.7.0 中运行的时间要长得多。同样的数据和代码需要两分钟以上。因此，即使不显式使用 PyGeos，总体上也有所改进。向开发商致敬。

## 使用 PyGEOS 的空间连接

让我们也使用 PyGEOS 进行测试，并使用 PyGEOS 中的矢量化几何操作运行相同的代码。要使用可选的 PyGEOS，只需将 use_pygeos 设置为 True。请注意，您需要在读取数据之前执行此操作。

```
# Set PyGEOS to True
gpd.options.use_pygeos = True
print(gpd.options.use_pygeos)# Read the data
gdf = gpd.read_file(“data/taxidata.shp”)
zones = gpd.read_file(“data/taxizones.shp”)# Spatial Join
start_time = time.time() 
sjoined = gpd.sjoin(gdf, zones, op=”within”)
print(“It takes %s seconds” % (time.time() — start_time))
```

使用 PyGEOS 在运行时间上略有改进。当 use_pygeos 设置为 True 时，该过程只需 0.84 秒。虽然这是一个显著的改进，并且当应用于更大的数据集时会成倍增加，但我对从 Geopandas 0.7.0 到 Geopandas 0.8.0 的整体发展印象深刻。

# IO 增强

此版本中还有许多输入/输出增强功能，这使得实时地理空间数据科学家可以轻松地在不同的数据格式之间转换和工作。

## 波斯特吉斯

在早期版本的 Geopadnas 中，可以读取 PostGIS 数据；但是，无法将数据写入 PostGIS。有了这个版本。可以将地理数据框写入 PostGIS 数据库。

新的`GeoDataFrame.to_post()`方法是一项出色的功能，可促进地理数据框架和 PostGIS 之间的完全可移植性。要使用此功能，您需要安装 SQLAlchemy 和 GeoAlchemy2，以及一个 PostgreSQL Python 驱动程序(例如 psycopg2)。

例如，如果我们想要编写上面使用的 Taxi 地理数据框架，我们只需要使用 alchemy 创建一个引擎。

```
import geopandas as gpd
from sqlalchemy import create_engineengine = create_engine(f'postgresql://{user}:{password}@localhost:5432/database_name?gssencmode=disable')gdf.sample(1000).to_postgis("taxi", engine)
```

数据顺利地转移到邮政地理信息系统。我也对这个选择感到非常兴奋。它为使用 PostGIS 的强大功能以及 Pandas 和 Geopandas 处理 Python 中的数据提供了新的可能性。

## 地质羽毛

我已经在另一篇文章中介绍了 GeoFeather 及其读写地理数据的速度。

[](/accelerate-geospatial-data-science-with-these-tricks-752d8b4e5381) [## 利用这些技巧加速地理空间数据科学

### 关于如何用代码加速地理空间数据处理的提示和技巧

towardsdatascience.com](/accelerate-geospatial-data-science-with-these-tricks-752d8b4e5381) 

在 Geopandas 0.8.0 版本中，您无需安装 GeoFeather Python 库即可使用它。它与 Geopandas 集成在一起，您无需安装其他库就可以使用它。

您可以使用`GeoDataFrame.to_feather()`将地理数据框架写入 GeoFeather 格式。也可以用`gpd.from_feather()`读取 GeoFeather 格式数据。

## 其他人

*   在此版本中，Geopandas 中也提供了拼花支持。您可以使用`gpd.read_parquet()`读写拼花数据，也可以使用`GeoDataFrame.to_parquet()`将地理数据框本地存储为拼花。
*   在`geopandas.read_postgis`中还有一个新的`chunksize`关键字，可以成块地读取查询。这一增加将有助于高效地读取大型数据集。

## 结论

在本文中，我们介绍了 Geopandas 0.8.0 版本中的新特性和改进。我们已经看到的特性包括 PyGEOS 集成、PostGIS 编写以及 Feather 和 Parquet IO 添加。

我希望你和我一样对这个版本感到兴奋。它带来了许多功能，使您作为地理空间分析师的生活变得更好。在本文中，我只提到了我感兴趣的几个特性，但是还有一些您可能感兴趣的特性，我在本文中没有提到。请随意访问这里的[发布说明](https://github.com/geopandas/geopandas/releases)。