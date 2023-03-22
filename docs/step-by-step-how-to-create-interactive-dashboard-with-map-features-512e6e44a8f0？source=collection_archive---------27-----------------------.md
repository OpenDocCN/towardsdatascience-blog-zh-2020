# 循序渐进:如何创建具有地图功能的交互式仪表盘

> 原文：<https://towardsdatascience.com/step-by-step-how-to-create-interactive-dashboard-with-map-features-512e6e44a8f0?source=collection_archive---------27----------------------->

![](img/1a6eb13840bf84a4913a5f475b6c0f76.png)

图片由 [janjf93](https://pixabay.com/users/janjf93-3084263/) 来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2755908)

演示是许多数据科学项目中最关键的部分。当你准备一个演示文稿时，也许你不喜欢花时间清理数据。但是演示对你的同事/客户/顾客的影响非常大。简单明了很有必要，尤其是在向一个非技术人员解释结果的时候。

# 空间数据的重要性？

空间数据是需求最大的数据类型之一。因为空间数据可以帮助我们；

*   更好的理解**哪里的答案？**
*   确定关系
*   识别模式
*   作出预测

地图当然是显示空间数据的好方法。如今，你不需要任何 GIS 软件来创建和发布地图。BI 工具现在能够创建专题地图。

有许多**商业智能工具**可以帮助你创建强大而有效的图表、仪表盘、可视化工具，如 Tableau、Qlik Sence、Power BI、Looker、Microstrategy 等。

在这篇文章中，我将一步一步地解释如何用 Tableau Public 创建一个交互式仪表盘。您可以在我的 [**Github 资源库中找到我将在本教程中使用的数据。**](https://github.com/yalinyener/LondonBoroughSpatialData)

*   **伦敦行政区**:从 Airbnb 内部获取的该城市街区的 GeoJSON 文件
*   **LondonBoroughProfile:** 从大伦敦当局获取的伦敦自治市档案的 CSV 文件

# **步骤 1:安装 Tableau Public 并创建概要文件**

Tableau Public 是一款免费软件，允许任何人连接到电子表格或文件，并为 web 创建交互式数据可视化。你可以使用 [**这个链接下载 Tableau Public for windows and Mac。**](https://public.tableau.com/en-us/s/download)

然而，你需要创建一个个人资料来展示你在互联网上的形象。

# 步骤 2:将你的文件连接到 Tableau Public

打开 Tableau 公共应用，可以看到左上角的“**连接”**区域。

![](img/0d1d3d6c998652aa238c6590a2f7c238.png)

作者照片

来连接你的空间文件 **(LondonBorough。选择空间文件，浏览你的文件并打开。当您打开您的空间文件连接屏幕出现如下所示。您的文件必须包括多重多边形类型的几何列。**

![](img/fd4c0999e0ed0410f71a2693f67215ef.png)

作者照片

要添加您的表格数据(**londonboroughprofile . CSV)**点击连接区域的添加按钮，然后选择“**文本文件”。**

![](img/085a7924b0e8c76b95ec992e7106865a.png)

作者照片

浏览您的 **LondonBoroughProfile.csv 文件**并点击打开。现在你有 2 个连接和 2 个文件，一个是空间文件，另一个是**“文本文件”**。

![](img/e337d626d92d46bfba21789e227c2d21.png)

作者照片

如果您想预览您的表格数据，您可以点击“**查看数据”**按钮并预览您的数据。

![](img/9824793cae167eea992cef612047bdba.png)

作者照片

![](img/bd11ab959905b6b9dd0dc94703939806.png)

作者照片

要创建两个文件的关系，选择 LondonBoroughProfile.csv 文件，拖放到关系区域。

![](img/a5fb2a9939255faeb6c535f6a1315783.png)

作者照片

现在，您必须选择两个文件的公共列(区名)。LondonBoroughs.geojson 的专栏名称为“**街区**”，LondonBoroughProfile.csv 的专栏名称为“**区名**”

![](img/92e0de8a0db6f0074dbb69985ad109cb.png)

作者照片

创建文件之间的关系后，您可以看到如下所示的连接。

![](img/2620803f2590b8465d8bf1c0207eb7ae.png)

作者照片

# 步骤 3:创建地图图幅

要创建地图可视化，点击左下方的**第 1 页**。

![](img/4782136716b150c4bfba83d11f84df1e.png)

作者照片

现在你有了两个表格和几十个属性，根据它们的类型(字符串、数字、日期、地理角色等)显示不同的图标。)

![](img/5a616dfed334c6383fd3715c9e06b50d.png)

作者照片

要创建伦敦区地图，请选择放置在 LondonBoroughs.geojson 下的几何列，拖放 Sheet1 area。

![](img/f6733b2970a0687bb06b7007a1b34bdf.png)

作者照片

现在你有了伦敦行政区的基本地图。你需要用一些外观特征来配置你的地图，比如标签、颜色、工具提示等等。

**创建标签:**选择邻居，拖拽至标签

![](img/13ca58568e736f82d581121f1b8835b4.png)

作者照片

![](img/ad184fcb7e6afa3f7822343389e526fd.png)

作者照片

**创建专题地图:**选择一个你想作为专题地图查看的特征(在 LondonBoroughProfile.csv 下)，拖放到**彩色**区域。在本教程中，我选择了**“2014/2015 年每千人口犯罪率”**

![](img/974cf63861f237caf972feb73b9dcbc2.png)

作者照片

现在，您有了一个基于犯罪率的专题地图。您可以通过改变颜色、标签等来配置外观特征。您可以添加过滤器，图例，标题等，以获得一个更容易理解的地图。

![](img/ea88395491fe49ef08e0c91ad4b7599c.png)

作者照片

**添加新特性作为工具提示:**工具提示为我们提供了关于属性的附加信息。因此，您可以添加更多的属性，当光标在地图上移动时，用户可以进行交互。选择一个或多个属性，拖放到工具提示区域。在本教程中我选择了**“2015 年就业率(%)”**“2017 年平均年龄”**“汽车数量，(2011 年人口普查)”**属性并重命名如下图所示；

*   就业率
*   平均年龄
*   汽车数量

![](img/b8807f62f85efcfab5f18b9638bb2ec5.png)

作者照片

# 步骤 4:创建图表

要创建新图表，请单击左下角的**新工作表**。

![](img/9b3864500929267b75b537e170701433.png)

作者照片

创建新工作表后，将出现一个空工作表。选择**区名称，**拖拽到列区。选择**犯罪率，**拖拽到行区域。

![](img/2b754784c4964698a52aca519e85752d.png)

作者照片

要着色并添加标签，选择**犯罪率**，拖放以着色，再次拖放以标记犯罪率

![](img/98013f4be338a0ff64e11c9945f21e18.png)

作者照片

“伦敦金融城”区没有数据，因此您需要从图表中移除该记录。单击行政区名称的箭头并选择过滤器。然后取消选中伦敦城

![](img/16c927a0ed6c8f0ad6233e79a95c9044.png)

作者照片

![](img/383acaf5f11eadca6d5f982e41888b23.png)

作者照片

# 步骤 5:创建仪表板

您可以创建更多包含不同图表的工作表。最后，您可以将这些工作表组合成一个仪表板。要创建新的仪表板，请单击“新建仪表板”按钮，此时将显示空仪表板。

![](img/7ff0454fddf09833f884f5f58a7ae222.png)

作者照片

将尺寸更改为“自动”

![](img/2e9b508b7bdbbe1bfd34f97c8651ce80.png)

作者照片

将 Sheet1(专题地图)和 Sheet2(条形图)拖放到仪表板区域。

如下图所示，我放置了这两张纸。

![](img/cdd6a81b6e27d1cd36cb6ce256e431a6.png)

作者照片

如果你想看到所有可视化的交互，你需要设置“用作过滤器”按钮。

![](img/1c8af184a1925156fc345f6294c14c1c.png)

作者照片

因此，如果您从地图或图形中选择一个区，您会动态地看到其他图形的结果

![](img/4dc4628b599043ab5bc50cca25199b87.png)

作者照片

# 第六步:发布

最后，您准备了一个包含地图和图表的仪表板。现在，您需要将此仪表板发布到您的 Tableau 公共个人资料。点击文件并选择**另存为 Tableau Public As**

![](img/5b8f8bed9efc220a17241893f91d6bd3.png)

作者照片

![](img/da6b5469818d08efd487328d63b7ffa4.png)

作者照片

保存仪表板后，您的 Tableau 公共个人资料会自动打开。您可以使用全屏按钮来最大化您的仪表板。

![](img/d137dcb6070f9fdad5c6dbf1fc3bf01e.png)

作者照片

你可以在我的 [**Tableau 公众号**](https://public.tableau.com/profile/yalin.yener#!/vizhome/LondonBoroughsCrime/Dashboard1) **找到我的仪表盘。**

感谢你阅读我的帖子，希望你喜欢。如果您有任何问题或想要分享您的意见，请随时联系我。