# 帮助您起步的数据项目

> 原文：<https://towardsdatascience.com/data-projects-to-get-you-started-fe923b047f6b?source=collection_archive---------83----------------------->

## 如果您足够仔细地观察，就会发现有大量的项目需要复制，还有大量未使用的数据源需要分析。

![](img/48d4009d5bae69c8761b419b0414c029.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Carlos Muza](https://unsplash.com/@kmuza?utm_source=medium&utm_medium=referral) 拍摄的照片

在一个城市中，有数不清的数据项目要做。项目选择可能非常困难，最初的挑战几乎总是找到合适的数据来使用。尽管来自全国各地的其他首席数据官总是愿意提供帮助并分享他们自己的工作示例(来自[数据智能城市解决方案](https://datasmart.ash.harvard.edu/)、[宾夕法尼亚大学穆萨项目](https://pennmusa.github.io/MUSA_801.io/)和[社会公益数据科学](https://www.dssgfellowship.org/projects/)的优秀示例)，但通常使用的特定数据集与您所在城市收集的数据并不完全相同，或者数据集无法公开共享以供参考。

在这篇文章中，我将详细介绍几个不同的地方，以找到几乎可以在该县任何其他城市复制的数据(或者以您所在的州为例)。这些数据源是健壮的，并且通常非常干净和易于使用。

**人口普查**

新推出的[data.census.gov](https://data.census.gov/cedsci/)是一个优秀且易于使用的数据下载资源。人口普查局还构建了 API，使得以编程方式下载数据变得容易。数据在全国范围内以完全相同的方式格式化，并且在许多情况下可以精确到您所在城市的几个街区。

几年前，我们撰写了一份名为《线下的 T14》的报告，分析了锡拉丘兹与贫困相关的人口普查数据和指标。收集和清理数据很简单，我们能够将锡拉丘兹与纽约北部的其他城市进行比较。任何人都可以为他们的城市找到相同的精确数据表，并将他们的数据与锡拉丘兹的数据进行比较，以扩展报告。人口普查的数据非常重要，经常用于城市分析。一个地方的有趣分析可以很容易地复制到另一个地方，只需简单地转换一下县、市或人口普查区的号码。

**财产评估资料**

我们与约翰·霍普金斯大学的[卓越政府中心合作，这是我们与](https://govex.jhu.edu/)[有效城市](https://whatworkscities.bloomberg.org/)合作的一部分，以[分析财产评估数据](https://github.com/CityofSyracuse/syracuseny_analytics/blob/master/syracuseny_analytics/GovEx_AdvancedAnalytics_FinalReport_SyracuseNY.pdf)，并试图更好地了解相对于可能的财产销售价值，财产评估哪里出了问题。我们从伊利诺伊州的库克县评估办公室获得了这个项目的灵感。了不起的是，库克县的评估数据与锡拉丘兹的评估数据非常相似。

更好的是，在纽约州，几乎每个自治市都使用一个名为房地产系统(或 RPS)的数据库，该数据库为全州的房地产维护一个一致的数据模式。这意味着使用我们的代码，纽约的另一个市政当局可以很容易地复制我们在锡拉丘兹的工作，在他们各自的地区尝试相同的项目。

**物联网**

也许你真的想进入“智能城市”领域，开始使用来自传感器的数据。这可能是昂贵的，难以实施，有时价值是值得怀疑的。相反，为了测试分析实时传来的传感器数据，像美国地质调查局这样的联邦级机构在许多不同的地方放置传感器，尤其是水道。因此，您可以创建一个查看湖泊或河流水位的项目，然后只需在 API 调用中更改特定的水体即可轻松复制该项目。

同样，国家气象局在全国各地放置天气传感器，通过他们提供的 API 很容易获取数据。[像 ESRI 这样的公司使得使用这些数据变得更加容易](https://www.esri.com/arcgis-blog/products/public-safety/public-safety/new-live-feeds-weather-data-goes-live/)，而且你很有可能在工作中拥有 ArcGIS 许可。

**开放数据门户**

美国联邦政府的开放数据门户[data.gov](http://data.gov)，是一个可用数据的宝库。美国许多州也有开放的数据门户(纽约州的是 data.ny.gov 的)。这些开放的数据门户包含关于你的城市的数据。你可能会发现所有拥有[酒类许可证](https://data.ny.gov/Economic-Development/Liquor-Authority-Daily-List-of-Active-Licenses-API/wg8y-fzsj)的企业或者你所在城市的[质量桥梁](https://data.ny.gov/Transportation/Bridge-Conditions-NYS-Department-of-Transportation/wpyb-cjy8)在哪里。同样，即使您所在的城市可能没有收集这些数据，但这些数据是有用的，可以通过非常简单的特定位置过滤，与州或国家的其他城市或地区进行比较。

**结论**

即使在应对让市政当局收集的数据更有用的挑战之前，也有大量资源可以从州政府或联邦政府那里找到数据。很多时候，这些数据得到了更好的维护，并且可以很容易地与该国的其他地区进行比较，或者有人可能已经使用您想要复制的相同数据集完成了一个项目。您还可以将这些数据集用作更大分析中的附加数据点，将您所在城市的数据与其他级别政府的数据相结合。

您在哪里使用这些来源的数据复制分析？