# 销售仪表板告诉你人们最喜欢为 2019 年圣诞节买什么

> 原文：<https://towardsdatascience.com/a-sales-dashboard-tells-you-what-people-like-most-to-buy-for-christmas-2019-5412987ab928?source=collection_archive---------40----------------------->

圣诞节刚刚过去。今年圣诞节你收到了什么礼物，送了什么礼物？你是否好奇 2019 年圣诞节最畅销的商品是什么，人们最想要的礼物是什么？这些问题的答案都在我下面做的圣诞销售仪表盘里。让我们来看看。

这个圣诞仪表板是我用互联网上的数据制作的在线销售仪表板。

我们先来看左边的两张图表。下面的柱状图展示的是每一类产品的销售额，上面的折线图展示的是某一类产品的年销售额。注意，左边的两个图表是联动的，即如果你在下面的柱状图中点击不同的产品类别，上面的折线图中就会显示出对应产品类别的年销量。

![](img/cdd761f64be0f7458610489694b4a13c.png)

中间酷炫的 3D 地球地图显示了圣诞节订购商品的物流路线，流线的粗细对应着商品的销量。我们还可以定制流线的样式。

仪表盘右上角是一个字云图，显示了人们最想要的圣诞礼物类别。字体越大，这类礼物越受欢迎。

最后，让我们看看右下角的实时销售图表。该图表可以连接到数据库系统，以更新实时销售数据。并且通过上面的水球图，可以实时观察产品的仓库库存。当存储容量低于警戒线时，会显示红色警告，并弹出一个框通知您:缺货。

现在，我相信你们都知道什么是最受欢迎的圣诞产品类别。这个圣诞销售仪表板上的数据可能不准确，但您仍然可以从数据可视化的实现和销售仪表板的设计中学习。如果你有兴趣，我很乐意和你分享制作这个圣诞仪表盘的方法和技巧。以下是具体制作过程。

# 步骤 1:选择工具并准备数据

首先，我们需要选择一个合适的数据可视化工具或 BI 工具来设计很酷的仪表盘。如果你有很强的技术功底，也可以用 Python 或者 r 之类的编程语言，我做这个圣诞仪表盘用的工具是 [**FineReport**](https://www.finereport.com/en/?utm_source=medium&utm_medium=media&utm_campaign=blog&utm_term=A%20Sales%20Dashboard%20Tells%20You%20What%20People%20Like%20Most%20to%20Buy%20for%20Christmas%202019) 。可以从官网下载其个人版免费使用。

> N **OTE:** [FineReport](https://www.finereport.com/en/?utm_source=medium&utm_medium=media&utm_campaign=blog&utm_term=A%20Sales%20Dashboard%20Tells%20You%20What%20People%20Like%20Most%20to%20Buy%20for%20Christmas%202019) 是我工作的数据研究所开发的一款 BI 报告工具。个人使用完全免费。你可以下载它来练习制作图表、报告和仪表板。

圣诞节仪表盘中的数据来自[统计公司](https://www.statista.com/)。并将数据存储在 Excel 文件中。接下来，我们需要将数据导入 FineReport。

# 步骤 2:导入数据

下载 FineReport 后，将 Excel 文件放在计算机上的“Reportlets”文件夹中。或者您可以使用自己的数据并将 FineReport 直接连接到您的数据库。

![](img/2755a5a9a755cacc40c8671b1a0266c5.png)![](img/a216b13c639c3ea486d5c93869b1e042.png)![](img/0364a65a0eeeef30106db70e16db34e7.png)

# 第三步:图表链接

要实现数据的联动，可以选择一个数据库来连接数据。

![](img/69b35d6b35a40d1f591f9943b58463c8.png)![](img/e83b2173857f85b3adbc1cf3f13eb4aa.png)![](img/cdfbc2045bf61d8508b15034b7e89869.png)

而要实现图表联动，使用简单的 SQL 语句作为条件。

![](img/04cdc43cb324644025a4e1a34180c4a2.png)

# 第四步:设计布局

准备好数据后，开始布局。我选择绝对布局的类型。点击“body”，可以设置其属性。

![](img/7a4c0a83771bd599bccf400c8a013032.png)![](img/b02127984171cf21fbc4d2f98910eaf5.png)

# 步骤 5:插入图表

我们只需要拖放来插入图表。对于图表的选择，可以参考本文 [*数据可视化中前 16 种图表类型*](/top-16-types-of-chart-in-data-visualization-196a76b54b62) 。

![](img/2285dad38bd791a00e8104ab4bc08cc3.png)

总体布局如下。

![](img/379f5e0acfe8cd6399ed10f82fd4951b.png)

然后为图表设置数据。依次选择图表所需的数据。

![](img/966430260e0ef85e90c1a41e8f25d9d3.png)

对于链接效果，我们之前已经设置了简单的 SQL 语句。现在我们为需要链接的图表设置参数。

![](img/9f8f996c97c6ed382600293f6c50f0c6.png)

# 第六步:实时显示数据

右下方的实时销售图表可以实时更新销售数据。这种效果是 FineReport 的免费插件直接实现的。

![](img/a091403d97f3a9790cb3bca53d449a70.png)

对了，雪花的效果是用 JS 实现的。你可以自己做。

![](img/1a47c7ea10bd7c5e66c04035e93c4e26.png)

# **终于**

嗯，这些是制作圣诞仪表盘的主要步骤。如果你对操作还有疑问，还有两个更详细的指南供你参考: [*制作销售仪表盘的分步指南*](/a-step-by-step-guide-to-making-sales-dashboards-34c999cfc28b) 和 [*新手如何打造一个很棒的仪表盘*](/how-can-beginners-create-a-great-dashboard-cf48c0f68cd5) 。

# 您可能也会对…感兴趣

[*让你的数据报告脱颖而出的指南*](/a-guide-to-making-your-data-reporting-stand-out-cccd3b99e293)

[*动态图表:让你的数据动起来*](/dynamic-charts-make-your-data-move-19e540a06bd3)

[*新手如何设计酷炫的数据可视化？*](/how-can-beginners-design-cool-data-visualizations-d413ee288671)

[*2019 年你不能错过的 9 款数据可视化工具*](/9-data-visualization-tools-that-you-cannot-miss-in-2019-3ff23222a927)