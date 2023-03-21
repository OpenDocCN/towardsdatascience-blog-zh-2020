# 使用 PostgreSQL/PostGIS 的空间数据科学

> 原文：<https://towardsdatascience.com/spatial-data-science-with-postgresql-postgis-2f941c8c367a?source=collection_archive---------13----------------------->

## 波斯特吉斯

## PostgreSQL 和 Python 空间数据入门实用指南。

![](img/483e3effaf0836e1f1ced030452d62fe.png)

丹尼斯·库默在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

空间性不特别！如果您曾经尝试过在普通 SQL 中使用空间数据，那么处理几何可能并不简单。然而，使用空间 SQL，感觉就像处理表中的另一列。

在本指南中，我们将介绍如何设置和开始使用空间 SQL。我们将安装 g PostgreSQL，激活 PostGIS 扩展并执行一些简单的空间 SQL 操作。在本文的后面，我们将了解如何将空间 SQL 整合到 Python 工作流中。

## 空间 SQL (PostGIS)

![](img/a0601f2757d73aac4867c43a7605ce7c.png)

空间数据科学被邮政地理信息系统托起

空间 SQL 处理、利用和执行空间数据的空间操作。空间数据库针对存储、操作和查询具有已定义几何图形的地理数据进行了优化。

虽然有许多空间数据库提供商，但我通常更喜欢 PostgreSQL/PostGIS。PostGIS 在地理空间数据库领域也得到广泛应用——例如，Big Querry 就采用了它。

如果您是数据库领域的新手，您可能会问 PostgreSQL 和 PostGIS 之间的区别。我们可以这样想:PostgreSQL 是数据库 Database，而 PostGIS 是在 Postgresql 之上提供空间功能的扩展。

## 安装 PostgreSQL + PostGIS

安装 PostgreSQL 很简单。您需要为您的发行版选择二进制包:Windows、macOS 或 Linux。

[](https://www.postgresql.org/download/) [## 下载

### PostgreSQL 对象关系数据库管理系统的核心有几个源代码和二进制…

www.postgresql.org](https://www.postgresql.org/download/) 

一旦你下载了 EDB 的交互式安装程序，运行。或者按照发行版中给出的说明进行操作。我已经在这里包括(GIF 指令)如何添加 PostGIS 扩展到你的 PostgreSQL。我们还安装了 PgAdmin4 作为 PostgreSQL 的 GUI 界面。

![](img/67ef52124ef3eb8f6888b73e91d4b70d.png)

确保在遵循交互式安装指南的同时检查 PgAdmin4 和 Stackbuilder。此步骤将允许我们稍后使用 PG Admin 4 用户界面。请务必检查 PostGIS 扩展，它使我们能够执行空间操作。

如果您想要操作系统的逐步指南，有一个交互式安装程序指南。

 [## 简介— PostgreSQL 安装指南 12.0 文档

### EnterpriseDB 创建的 PostgreSQL 安装程序旨在使在…上安装 PostgreSQL 变得快速而简单

www.enterprisedb.com](https://www.enterprisedb.com/edb-docs/d/postgresql/installation-getting-started/installation-guide-installers/12/index.html) 

## PgAdmin 4 GUI 界面

让我们熟悉一下我们将在本教程中使用的 PgAdmin GUI 界面。下面的 GIF 展示了界面。我们还创建了一个 PostGIS 扩展，它提供了我们稍后将使用的空间功能。

![](img/66c6a6ad1a98908c1184c07c37fa7076.png)

我们可以简单地创建一个 PostGIS 扩展，在 PgAdmin 查询工具中实现以下命令。

```
CREATE EXTENSION postgis;
```

运行该命令后，我们有大量新的空间函数可供使用。

让我们也使用 PgAdmin4 创建一个数据库。下面的 GIF 说明向您展示了如何创建一个新的数据库。

## 加载数据

加载地理空间数据不同于将 CSV 文件加载到 PostgreSQL 中。我们需要保留几何列。但是，有一种使用 Shapefile Loader 上传空间数据的简单方法。

以下 GIF 指令显示了如何将 shapefiles 加载到 PostgreSQL 数据库中。

![](img/3bc883a1584f87045c3eabbc2740b183.png)

在上传数据之前，您需要了解数据的坐标参考系统(CRS)是什么。在这种情况下，我已经在 QGIS 的帮助下查看了数据，并且我知道投影代码。但是如果你不确定，你可以输入 4326，这是常用的 WGS 84 码。

加载数据后，我们就可以使用 PostGIS 执行空间数据分析了。我也上传一些其他的文件到数据库:街道，边界和伦敦地铁站。我们将把它们作为

## 基本空间操作

空间 SQL 可以简化地理空间数据的处理和分析，还可以加快特定过程的速度。有数百种空间功能随时可供您使用。

让我们从一些简单的例子开始。在以后的文章中，我将更深入地研究空间 SQL 函数和过程。我已经上传了。

**例 1:计算建筑物面积**

```
SELECT objectid, 
       shapestare, 
       ROUND(ST_Area(geom)::numeric,2) AS AREA
FROM public.buildings
```

这个简单的 SQL 代码返回所有带有新面积列的建筑物，精度为两位小数。总的查询运行时间:659 毫秒。有 176023 行受到影响。

我们在这里使用的是`ST_Area()` PostGIS 函数。

**例 2:计算每种街道类型的长度**

```
SELECT streettype, Sum(ST_Length(geom)) AS length
FROM public.streets
GROUP BY 1
ORDER BY 2 DESC;
```

上述 SQL 代码对每种街道类型进行分组，并计算每种类型的长度。我们在这里使用的是`ST_Length()` PostGIS 函数。

## 用 Python 访问 PostGIS

好的一面是，您可以用 Python 轻松连接您的 PostGIS 数据库。Geopandas 是最常用的地理空间 python 库，可以从 PostGIS 中获取和读取 SQL 语句。以下是如何用 Python 连接数据库和运行空间 SQL 语句的方法。

```
import geopandas as gpd
import sqlalchemy as db# Create a database connection
engine = db.create_engine(‘postgresql+psycopg2://postgres:postgres@localhost:5432/postgis_database’)
con = engine.connect()# Read PostGIS database with Geopandas.
sql = “SELECT * FROM public.streets;”
data = gpd.read_postgis(sql=sql, con=con)
```

将您的 PostGIS 数据库引入 Python，为探索数据和可视化数据提供了其他可能性。让我们使用 Geopandas 和上下文底图来可视化我们所读取的街道数据集。

```
import geopandas as gpd
import matplotlib.pyplot as plt
import contextily as ctxfig, ax = plt.subplots(figsize=(18, 16))
data.to_crs(epsg=3857).plot(ax = ax,
 figsize=(20,18),
 color=”gray”,
 alpha=0.5,
 );
ctx.add_basemap(ax, url=ctx.providers.OpenStreetMap.Mapnik);
```

![](img/fb1c1393ea916a7cc42b9a13df06c05d.png)

街道地图

## 结论

在本指南中，我们已经安装了 PostgreSQL 并激活了 PostGIS 扩展。我们已经了解了如何使用 PgAdmin 创建数据库，以及如何将空间数据上传到数据库 database 中。我们还看到了一些简单的空间 SQL 示例，最后，我们将空间数据库数据库集成到 Python 中。

我计划下一篇用空间 SQL 写空间数据处理的文章。敬请关注。