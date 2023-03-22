# 如何在 Tableau 中制作桑基图

> 原文：<https://towardsdatascience.com/how-to-make-sankey-diagram-in-tableau-f5f8730e5962?source=collection_archive---------0----------------------->

![](img/5c94853eb1be03a240887825e94093b9.png)

桑基图是一种图表，我们可以用它来可视化一个度量在多个维度上的流动。桑基图是由爱尔兰船长桑基发明的，用来描述蒸汽机中的能量流动。

![](img/3d9918f2440cde9e484d77da14583128.png)

M. H. Sankey 在[维基百科](https://upload.wikimedia.org/wikipedia/commons/1/10/JIE_Sankey_V5_Fig1.png)上的第一张 Sankey 图(CC-PD)

有几个用例适合用 Sankey 图来可视化。一些例子是:

*   网站访问者行为
*   用户转化
*   交通模式
*   产品分配

在这篇文章中，我将一步一步地介绍如何使用 Tableau Pubilc 创建一个 Sankey 图。对于没有 Tableau 的你，你可以使用这个[链接](https://public.tableau.com/en-us/s/download)免费下载 Tableau Public。

## 步骤 1 导入数据集

我们使用的数据源是 Superstore Dataset 和 tableau。我们可以通过连接 Microsoft Excel 并选择一个超级商店文件来导入它。

![](img/183c2450f8de4b2df2a880694f005fd8.png)

导入数据集

## 步骤 2 创建参数、尺寸和测量

在这一步中，我们将创建两个参数和三个计算字段。这些参数和计算字段将用于为用户提供选择 Sankey 两侧尺寸的能力。

首先，为“选择尺寸 1”和“选择尺寸 2”创建参数，设置如下。为此，我们可以创建选择维度 1，然后复制并粘贴它，并更改选择维度 2 的名称。

![](img/51c5f1dabad8518111ddfdb173466d15.png)

选择维度 1 的配置

其次，我们需要创建计算字段作为所选维度的占位符。我们需要为选择维度 1 和选择维度 2 创建它。

![](img/0437708e522400e8bb92b16ab2734c47.png)

维度 1 计算字段

然后，我们将为我们选择的度量创建一个计算字段，并为此选择 Sales。

![](img/1562a3e357f0c694734f59125536105a.png)

选择的度量计算字段

## 步骤 3:创建数据加密框架

在这一步中，我们将我们的测量值与测量值的固定最小值进行比较。通过这样做，我们可以确保我们帧之间的两个数据点。

![](img/3c71d4fd45e0389ada63713f7b4d399c.png)

路径框架计算字段

接下来，我们将划分 bin 大小等于 1 的路径帧，并为该路径创建索引。

![](img/d5d585ddb448e75c68cf77f0b18c1404.png)

为路径框架创建箱

![](img/0224643358915c792af0f195b83323f6.png)

路径框架箱设置

![](img/2a5f0d24bba24761b227028c756fb164.png)

路径索引计算字段

## 步骤 4 设置图表曲线

我们需要制作两个计算字段，以便在我们的 Sankey 中制作一条曲线。我们将采用 sigmoid 公式来完成这项任务。执行此任务的计算字段如下所示。

![](img/91921777491d86900aefdff1a36ae5e5.png)![](img/058e5b34546a51ac53f36167e1995a1f.png)

## 步骤 5 桑基臂尺寸

此计算字段将每个 Sankey arm 作为完整数据集的百分比。

![](img/7f0a905a4083034fc5761ab2982cf0a6.png)

## 第 6 步顶线和底线计算

此时，我们需要创建八个计算字段。这些字段用于创建每个 Sankey 的顶部和底部。在这些计算中，位置 1 是指左侧的尺寸 1，位置 2 是指右侧的尺寸 2。

以下是顶部和底部的计算字段:

**最大位置 1:**

RUNNING_SUM([Sankey 臂长])

**最大位置 1 包**:

WINDOW _ SUM([最大位置 1])

**最大位置 2:**

RUNNING_SUM([Sankey 臂长])

**最大位置 2 缠绕:**

WINDOW _ SUM([最大位置 2])

***最小位置 1 的最大值:***

RUNNING_SUM([Sankey 臂长])

***最小位置 1:***

RUNNING _ SUM([最小位置 1 的最大值]-[Sankey 臂尺寸]

***最小位置 1 换行:***

WINDOW _ SUM([最小位置 1])

***最大为最小位置 2:***

RUNNING_SUM([Sankey 臂长])

***最小位置 2:***

RUNNING _ SUM([最小位置 2 的最大值]-[Sankey 臂尺寸]

***最小位置 2 换行:***

WINDOW _ SUM([最小位置 2])

WINDOW _ SUM([最大位置 2])

## 步骤 7 三键多边形计算

该计算结合了上述所有计算，并为 Sankey 生成多边形。

![](img/a6156042d8a4bcdcc724441f72e8eef5.png)

## 步骤 8 创建桑基表。

此时，我们已经创建了三个新的维度、16 个度量和两个参数。在这里，我们将使用所有这些来制作桑基。

![](img/207ef7f956cc4e4371d81709a88b2ffd.png)

首先，拖动路径框架(bin)，尺寸 1，尺寸 2，作为标记中的细节。接下来，将 T 放入列中，并使用路径框架(bin)计算它，同时将 Sankey 多边形拖动到行中。然后将标记更改为多边形，并将路径索引添加到路径中，并与路径框架(bin)一起计算。最后，单击右下角的 96 nulls 显示所有 null 值。

![](img/4b63cc038f78364e4330c40053714f24.png)

现在我们需要编辑桑基多边形的表格计算。我们需要一个一个仔细考虑。

![](img/d84d0f3b5c51eb6caf0a76f5717dcfe0.png)![](img/9ed592760650ecfd3c57dd66581b3855.png)![](img/4ec11bcd2af8ad8ee540de40e5b666a7.png)![](img/741c400ab6c50f3722077b9c3eebd032.png)

总共有 12 个嵌套计算。编辑完计算表后，我们将把这张表准备好。

![](img/9358ade0c5551bfe189aea89adae295a.png)

## 制作桑基手臂

我们继续通过创建两个新的表并如下设置，在 Sankey 的左侧和右侧创建 Sankey 手臂。

![](img/ac23593110462b56eaec1fe31d23b372.png)

左侧桑基臂

![](img/fdbeb59917053fa80bc1ee987806c905.png)

右侧桑基臂

通过将选择的度量拖到行中，并将表计算改为占总数的百分比。然后添加颜色和文本的维度。另外，为 Sankey 右侧的颜色标记添加 INDEX()。

## 步骤 10 创建仪表板

最后一步，我们需要将所有的表和过滤器排列在一起，以得到我们的 Sankey 图。

![](img/27899130cc483af4425e483e4e4ae039.png)

## 结论

最后，我们得到了第一个 Sankey 图，可以用来显示超市数据集中的流量。您可以查看此 Sankey，并通过此[链接](https://public.tableau.com/views/SankeyDiagram_15885756725040/Dashboard1?:display_count=y&:origin=viz_share_link)下载工作簿。

此外，您可以更改数据源并按照步骤构建自己的 Sankey 图并共享您的作品。

## 参考

*   [https://www . theinformationlab . co . uk/2018/03/09/build-sankey-diagram-tableau-without-data-prep-previous/](https://www.theinformationlab.co.uk/2018/03/09/build-sankey-diagram-tableau-without-data-prep-beforehand/)