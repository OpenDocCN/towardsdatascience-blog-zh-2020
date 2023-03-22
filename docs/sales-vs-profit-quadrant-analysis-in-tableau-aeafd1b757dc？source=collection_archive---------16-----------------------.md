# Tableau 中的销售与利润(象限)分析

> 原文：<https://towardsdatascience.com/sales-vs-profit-quadrant-analysis-in-tableau-aeafd1b757dc?source=collection_archive---------16----------------------->

## 使用 Tableau 中的象限分析快速识别任何销售数据集中表现最佳的类别。

![](img/f15b616cf6a39db46c54d8e725492b57.png)

Tableau 中的销售与利润象限分析(图片由作者提供)

使用 Tableau 中的象限分析快速识别任何销售数据集中表现最佳的类别。

详细信息:象限图只不过是一个由四个相等部分组成的散点图。每个象限支持具有相似特征的数据点。以下是如何使用[超市](https://community.tableau.com/s/question/0D54T00000CWeX8SAL/sample-superstore-sales-excelxls)销售数据集在 Tableau 中绘制象限分析图的分步方法，以确定在销售和利润方面表现最佳的品类:

**步骤 1:** 打开一个新的 Tableau 文件，将“超市”公共数据集导入工作簿。如果你以前没有用过 Tableau，请从以下网址下载“Tableau Public”:[https://public.tableau.com/en-us/s/download](https://public.tableau.com/en-us/s/download)

![](img/f8ba2b37189d13878e2d69fc2bbdab04.png)

Tableau 公开下载(图片由作者提供)

**第 2 步:**我们的目标是根据销售和利润指标评估各个子类别的表现。因此，将“订单”表导入到工作区中；将汇总的“销售”和“利润”指标拖放到行&列中。

![](img/9b930914236a36a5548cd3288bf891c1.png)

导入订单表(作者图片)

![](img/e4e1e6ff51481e9968c2696411819993.png)

总销售额和利润(作者图片)

**第 3 步:**通过将“子类别”字段拖放到“标签”和“分析”窗格中，引入子类别。

![](img/e358a66e6921f9ec9d2d8aeb669852f6.png)

导入子类别(按作者分类的图片)

**步骤 4:** 为参考行创建计算字段:

```
Reference Line for Profit = WINDOW_AVG(SUM([Profit]))Reference Line for Sales = WINDOW_AVG(SUM([Sales]))
```

将参考线计算拖至“详细信息”和“分析”窗格。编辑两个参考行计算，一个用于销售，另一个用于利润。

![](img/4b6291f91631563d0096babeb7191b3a.png)

创建计算字段(作者图片)

![](img/03269ecc6e19cf655c912ffd827dc870.png)

利润参考线(图片由作者提供)

![](img/0fbd8d8c331be4466645dde3bb742964.png)

销售参考线(图片由作者提供)

![](img/d0477a71297816d501363fee1a3dff57.png)

统一参考线部件(图片由作者提供)

![](img/22990c2c3a12535a9e3a69070d807c91.png)

带圆圈的参考线(图片由作者提供)

**步骤 5:** 为“象限(颜色指示器)”创建计算字段:

```
Quadrant (Colour Indicator)=IF [Reference Line for Profit]>= WINDOW_AVG(SUM([Profit]))
AND [Reference Line for Sales]>= WINDOW_AVG(SUM([Sales]))
THEN 'UPPER RIGHT'ELSEIF[Reference Line for Profit]< WINDOW_AVG(SUM([Profit]))
AND [Reference Line for Sales]>= WINDOW_AVG(SUM([Sales]))
THEN 'LOWER RIGHT'ELSEIF[Reference Line for Profit]> WINDOW_AVG(SUM([Profit]))
AND [Reference Line for Sales]< WINDOW_AVG(SUM([Sales]))
THEN 'UPPER LEFT'ELSE 'LOWER LEFT'END
```

拖动“象限(颜色指示器)”来着色和编辑表格计算；选择“子类别”作为特定维度。

![](img/b6467ca22d1eacf1ac6f5618ea103b9f.png)

象限颜色指示器(图片由作者提供)

![](img/fecd2cc1596a53effff99a493df36e13.png)

选择子类别(按作者分类的图片)

**步骤 6:** 此外，为等级创建一个计算字段，并将它拖到工具提示中:

```
Sales Rank = RANK(SUM([Sales]))Profit Rank = RANK(SUM([Profit]))Count of Sub-Category = WINDOW_COUNT(COUNTD([Sub-Category]))
```

*对于每个计算字段:编辑表格计算&选择“子类别”作为特定维度。*

![](img/5f4c7fdbad759ac4c8263d36656950ef.png)

销售排名(作者图片)

![](img/ad31d2bc6119a560c87803fd6066e6f0.png)

利润排名(作者图片)

![](img/803229e46bb67cba6039bc475df40ace.png)

子类别计数(按作者分类的图片)

**第七步:**给标题，格式化坐标轴，添加参考线。

![](img/b5610aa346c1740d63b71a7ec5d001bf.png)

Y 轴格式(图片由作者提供)

![](img/8aa97b89f264890f2d164a101dc20453.png)

X 轴格式(图片由作者提供)

![](img/82c5f2d30fb210e55d6e381c8a2121da.png)

在两个轴上添加参考线(图片由作者提供)

![](img/25bd7410dd9c6f432f653e6b8c7dee2e.png)

y 轴参考线配置。(图片由作者提供)

![](img/4200a77d118575fa3a7f13aaeaec5815.png)

x 轴参考线(图片由作者提供)

**第 8 步:**固定 BI 小工具的大小，设置工具提示。

![](img/effd7f481dc860b2b719f125e93bc7a7.png)

BI 部件大小(图片由作者提供)

**工具提示:**

```
<Sub-Category>Sales:<SUM(Sales)>Sales Rank:<AGG(Sales Rank)>/<AGG(Count of Sub-Category)>Profit:<SUM(Profit)>Profit Rank:<AGG(Profit Rank)>/<AGG(Count of Sub-Category)>
```

![](img/a367050c6b44641ef21545d7eccbd4b6.png)

工具提示配置。(图片由作者提供)

**第九步:产生洞察力:**

![](img/d5f2fc75e2b2cb668b2a526df6c6c6b0.png)

销售与利润象限分析(图片由作者提供)

(1)我们可以从象限图中清楚地看到，通过为超市创造最大的销售额和利润，手机、储物件和活页夹销量最大。因此，让销售团队知道这些产品对我们的年收入有多重要。

(2)另一方面，我们可以看到销售了大量的桌子和机器，但它们的利润似乎出人意料地低于同行。我们在抛售吗？我们应该提高产品的价格吗？

(3)超市出售的电器不多，而它们的利润率似乎真的很高。因此，让销售团队知道，如果他们能想出更多创新措施来推动这些设备的销售，那就太好了。

(4)最后，重新审视我们的销售和营销策略，以确保有足够数量的家具、书架和艺术品以修订后的价格售出。

**结论:**

因此，在本文中，我们简要地研究了 Tableau 中交互式象限分析的基本原理，并从相似和不相似的数据点中获得见解。从这里，我们可以继续用 R 或 Python 进行探索性数据分析，以识别数据集中真正推动销售的其他重要组件。

**GitHub 库**

我已经从 Github 的许多人那里学到了(并且还在继续学到)。因此在一个公共的 [**GitHub 库**](https://github.com/srees1988/quadrant-analysis-tableau) 中分享我的 Tableau 文件，以防它对任何在线搜索者有益。此外，如果您在理解 Tableau 中数据可视化的基础知识方面需要任何帮助，请随时联系我。乐于分享我所知道的:)希望这有所帮助！

**关于作者:**

[](https://srees.org/about) [## Sreejith Sreedharan - Sree

### 数据爱好者。不多不少！你好！我是 Sreejith Sreedharan，又名 Sree 一个永远好奇的数据驱动的…

srees.org](https://srees.org/about)