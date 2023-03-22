# 谷歌大查询地理信息系统初学者指南

> 原文：<https://towardsdatascience.com/a-beginners-guide-to-google-s-bigquery-gis-46a1193499ef?source=collection_archive---------21----------------------->

## 通过这个循序渐进的教程，免费开始使用 Google Big Query GIS

![](img/b62b2e9cbcf2ad74aa3c0ef28fdc7350.png)

约书亚·索蒂诺在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

这个大数据时代的云计算无处不在。然而，许多云服务不提供位置组件来分析和可视化地理空间数据。Big Query 具有接收、处理和分析地理空间数据的内置功能。

在本教程中，我将指导您免费设置 BigQuery 沙盒，使用熟悉的 PostGIS/Spatial SQL 接口处理空间数据，并在云中可视化。

## 设置免费的 BigQuery 沙箱

谷歌慷慨地提供了一个免费的沙盒供你实验。 [BigQuery 沙箱](https://cloud.google.com/bigquery/docs/sandbox)让你可以自由尝试 BigQuery 的功能，并带有一些[限制](http://The BigQuery sandbox gives you free access to the power of BigQuery subject to the sandbox's limits. The sandbox lets you use the web UI in the Cloud Console without providing a credit card. You can use the sandbox without creating a billing account or enabling billing for your project.  The web UI is the graphical interface that you can use to create and manage BigQuery resources and to run SQL queries. See the BigQuery web UI quickstart for a working introduction to the web UI.)。有了沙盒，您可以使用 BigQuery，而无需为您的项目创建计费帐户或启用计费。

他们还提供 3 个月 300 美元的免费试用，你可能有资格。

要使用 BigQuery 沙盒，只需转到此 URL。

```
[https://console.cloud.google.com/bigquery](https://console.cloud.google.com/bigquery)
```

用谷歌账户登录(最好使用匿名模式)。请注意下图左上角的沙盒标志，它表示您现在处于一个免费的沙盒环境中(每月 10 GB 的活动存储和 1 TB 的已处理查询数据)

![](img/2284819a87d58d4ce3b8e3bf861e92ce.png)

BigQuery 界面—作者图片

您可以在“资源”面板中添加数据源(左侧以蓝色突出显示)。在中间的面板中，有查询编辑器，您可以在其中编写 SQL 语法(突出显示为绿色)。一旦准备好 SQL，就可以使用 run 按钮运行查询。

## BigQuery 公共数据集

谷歌有一个庞大的公共数据集存储库。在撰写本文时，可用的公共数据集数量为 195 个。在本教程中，我们将使用开放的公共数据集之一:芝加哥犯罪。请随意从列表中确定一个感兴趣的数据集并进行实验，但本文中的所有示例都将使用芝加哥的犯罪数据。

要添加公共数据集，您需要点击`+ sign ADD DATA`，然后点击`Explore public data set`。选择一个令人兴奋的数据集，点击它查看数据集。在本例中，我们使用芝加哥犯罪数据集。

![](img/8a2195859d7b8695341c7c0d56c5a43f.png)![](img/1f066dd3e95ac5e4a01eb4f2768ee2de.png)

搜索数据集(左)。正确可用的 big query-big query 界面中的公共数据-作者图片

现在我们已经添加了公共查询数据集，我们可以查询它们了。让我们在下一节看到这一点。

## 使用 BigQuery 运行 GIS 查询

现在，您可以运行标准的 SQL 查询来浏览公共数据集。但是，由于这些数据集通常很大，您可以运行限制行数的 select 语句来预览数据集的前几行或查看表的架构。但是，您可以预览模式和数据集的几行，而无需运行任何 SQL 代码，从而节省运行查询的成本。

查看数据集的架构。单击数据集和下方的按钮，您将看到表格的`Schema`(如下所示)。

![](img/c31ee9143c7a6f1306f58800b0271508.png)

Bigquery 接口数据集架构-按作者分类的图像

如果您想预览前几行，那么您可以点击`Preview`，然后您应该会看到数据集的一些行，如下所示。

![](img/5f284bb87e4699160fdbb1ecaaca4bb1.png)

Bigquery 接口数据集预览-按作者分类的图像

在下一节中，我们将介绍 BigQuery 的地理特性。

## 创建和处理地理要素

我们的表有经度和纬度列；因此，我们可以使用 SQL 地理函数将这些值转换为地理要素。如果您已经熟悉 PostGIS，BigQuery GIS SQL 语法也应该是典型的。

我们可以使用`ST_GeogPoint`函数创建一个地理列。让我们看一个例子。

![](img/7f98829c2d17fe6b7981c3015ac104e6.png)

BigQuery 中的 ST_GeogPoint 查询-按作者分类的影像

让我们先来看一下 SQL 语法。我们选择犯罪的主要类型，使用`ST_GeogPoint`创建一个地理点，并传递纬度和经度列。我们还删除了经度列中的所有空值，因为我们不能用坐标创建地理点。

![](img/a524ce687fcf780276b6686fd6fe89a8.png)

ST_GeogPoint 查询 SQL 代码-作者图片

如果您查看上面的图像，您可以保存结果，但更重要的是，BigQuery 具有 GIS 可视化功能，您可以在其中交互式地绘制地图。要可视化地理空间数据，点击`Explore with GeoViz`，会弹出一个新窗口。点击`Authorize`并使用您的帐户登录。现在，您应该可以将之前运行的 SQL 语法复制到 BigQuery Geo Viz 工具中。点击`Run`。现在，您应该会看到一个带有 SQL 结果的底图，在本例中是犯罪点。

![](img/f84281e7b751bd6f6835e8af81a8ca2e.png)

Geoviz 工具-图片由作者提供

如果数据已经是地理空间格式，运行一个简单的 SQL 查询就足够了。比方说，我们想得到芝加哥的邮政编码。Big Query 在 geo_us_boundaries 上有一个公共数据集，我们可以运行一个包含 geom 列的 select 语句。

![](img/ea6e279d381db4f22dfdfb0f881d8265.png)

多边形几何 SQL 代码-作者图片

现在，我们可以使用 BigQuery Geo Viz 工具可视化结果。

![](img/0058e14680e2629f181a23a38f631656.png)

使用 GeoViz 可视化多边形-作者提供的图像

现在，我们可以在 BigQuery 中查询地理数据集，让我们转到一个使用空间连接函数的更高级的示例。

## 空间连接

空间连接是应用最广泛的空间过程之一。当我们想要按位置连接数据时，我们使用空间连接。假设我们想要将每个犯罪点与其邮政编码关联起来。我们可以使用`ST_Within`函数来检查该点是在邮政编码以内还是以外。

![](img/f63c451072a263a2f04b73a92ac9edbf.png)

简单空间连接 SQL 代码-按作者分类的图像

上面的 SQL 查询将芝加哥犯罪点与邮政编码关联起来。我们正在使用`ST_Within`功能，并通过犯罪和邮政编码几何地理点。结果如下表所示，其中每个点都与其邮政编码相匹配。

如果我们想统计每个邮政编码中的犯罪数量，我们可以添加一个 group by 语句。以下 SQL 查询返回每个邮政编码的犯罪数量。

![](img/b170d2e4da0d64bc3d30bb6ad2e4bdef.png)

具有空间连接的分组依据 SQL 代码-按作者排序的图像

结果如下所示。我们统计了每个邮编的所有犯罪。

![](img/c140d04a4afd79a7f6d83bad679ebde8.png)

按结果分组-按作者分组的图像

你知道我们没有任何地理特征，因为我们不能按地理特征分组。但是，我们可以运行 with-statement 查询来获取邮政编码几何以及每个邮政编码中所有犯罪的计数。

让我们尝试一下(参见下面的 SQL 查询)。这里没有什么新的东西，我们首先通过一个带有空间连接的语句运行通常的 group，然后连接到实际的邮政编码几何。

![](img/4cff87ba461349845e058c60f2e5d863.png)

带语句的空间连接 SQL 代码-按作者分类的图像

查询结果如下所示。您现在可以看到，我们已经在结果中包含了几何列。

![](img/5bb43a1f5f60f0cc2c65a2657ba22697.png)

几何空间连接-作者提供的图像

因为我们有一个几何列，所以我们也可以使用 Goeviz 工具来可视化结果。我需要设计地图的样式，因此我使用了计数栏来绘制一个 choropleth 地图(见下图)。

![](img/27bf00baf2735583700beb8f8f50e502.png)

作者绘制的带 GeoViz- Image 的 Choropleth 地图

## 结论

在本文中，我们已经使用免费的沙箱介绍了 BigQuery 的一些基本地理功能。在熟悉了 BigQuery 接口之后，我们已经使用 BigQuery GIS 的地理功能创建并运行了简单的查询。最后，我们实现了一个按位置连接数据的例子。

BigQuery GIS 中有大量的地理功能。你可以在这里找到它们。

[](https://cloud.google.com/bigquery/docs/reference/standard-sql/geography_functions) [## 标准 SQL | BigQuery | Google Cloud 中的地理功能

### 如果任何输入参数为 NULL，所有 BigQuery 地理函数都将返回 NULL。地理功能分为…

cloud.google.com](https://cloud.google.com/bigquery/docs/reference/standard-sql/geography_functions)