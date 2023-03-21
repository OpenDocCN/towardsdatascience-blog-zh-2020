# 新冠肺炎开源仪表板

> 原文：<https://towardsdatascience.com/covid-19-open-source-dashboard-fa1d2b4cd985?source=collection_archive---------0----------------------->

## 作为数据科学家，我们必须尽最大努力从数据角度接近当前的新冠肺炎疫情。因此，我使用开源工具创建了一个[仪表板](https://chschoenenberger.shinyapps.io/covid19_dashboard/)来跟踪和可视化新冠肺炎的传播。

几周来，新冠肺炎疫情一直是一个不可回避的话题。媒体用新感染名人的突发新闻淹没了我们，几个国家的病例和死亡人数呈指数增长。如今，人们在社交媒体上无法谈论其他任何事情，卫生纸囤积者的视频在网上疯传。

公司没有错过这一趋势。在过去的几天里，我的 Twitter 和 LinkedIn 上充斥着各种公司，展示他们的技术有多棒，以及他们如何无私地利用它来支持政府机构、医疗保健机构等。不要误解我，我认为公司愿意提供帮助是很棒的；在当前的危机中，向任何需要帮助的公共机构提供援助的每一家公司都值得称赞！然而，如果这些贡献中的每一个都必须在营销方面得到充分利用，它们会留下一种陈腐的味道。

此外，[最受欢迎的仪表盘](https://coronavirus.jhu.edu/map.html)使用了鼓励制造恐慌的黑红配色方案，就像各种新闻页面都有令人震惊的标题一样。可视化的确很重要，作为数据科学家，我们应该很好地意识到这一点，正如这篇[文章](https://medium.com/nightingale/ten-considerations-before-you-create-another-chart-about-covid-19-27d3bd691be8)所概述的。我知道使用开源技术，如 [R Shiny](https://shiny.rstudio.com/) 或 [Python Dash](https://plot.ly/dash/) ，可以创建一个不那么危言耸听的仪表板，与最流行的新冠肺炎仪表板不相上下。我就是这么做的！你可以在这里找到我创建的 R 闪亮仪表盘。

# 数据

用于所有可视化的数据由约翰·霍普金斯大学系统科学与工程中心(JHU·CSSE)提供，他们在 Github 公共页面上分享了他们的数据。约翰霍普金斯 CSSE 汇总了主要来源的数据，如世界卫生组织、国家和地区公共卫生机构。这些数据免费提供，并且每天更新。

仪表板中使用的人口数据来源于 [**世界银行公开数据**](https://data.worldbank.org/) 。这些数据只需要做一些小的修改就可以适应约翰霍普金斯大学 CSSE 新冠肺炎分校的数据。数据集中没有的“国家”人口(大部分是岛屿实体)是用来自[维基百科](https://en.wikipedia.org/wiki/Main_Page)的数据手动添加的。世界银行有一个庞大的数据库，涵盖从性别统计到投资流动的各种主题。这个存储库是高度可访问的，要么直接使用他们的 API，要么使用大多数通用编程语言中可用的各种第三方 API 之一。这为我们提供了在仪表板中集成各种有趣的数据集的机会，这些数据集目前不包括在“主流”选项中。你可以在这里找到更多关于开发者的信息。

# 新冠肺炎仪表板——开源版

仪表板分为几个部分，可在浏览器窗口的左上角选择。

[仪表板的总览部分](https://chschoenenberger.shinyapps.io/covid19_dashboard/)显示了新冠肺炎疫情最重要的关键数字和可视化效果，例如世界地图和国家列表，以及它们各自的确诊、治愈、死亡和活跃病例数。包括一个带有简单滑块的延时功能，以了解疫情的发展。

![](img/4185373f900ff08c0997ed6e1e4623fb.png)

新冠肺炎仪表板-概览部分

“绘图”部分包含可视化新冠肺炎·疫情级数的几个有趣方面的绘图。这些额外信息包括一些问题，如案件的全球演变；新案件；以及两个允许在不同国家之间进行比较的图表。用户能够手动选择她或他感兴趣的国家。大多数图都有复选框，y 轴可以切换到对数刻度，特定国家的图可以按人口进行标准化。默认情况下，在特定国家图中选择确诊病例最多的五个国家。

![](img/4ab6fea3282f794c52ddd4ca2d91d498.png)

新冠肺炎仪表板—绘图部分(用红色标记:国家选择)

仪表板上的“关于”部分描述了动机、数据来源以及关于发展和贡献的其他信息。

## 考虑

请注意，这个数据不能想当然。关于新冠肺炎案件的数字有很多不确定性。我避免计算死亡率和类似的数字，因为它们相对模糊，如几个来源所述[1][2]。各国的检测制度差异很大，因此很难在各国之间进行直接比较。

# 技术

仪表板是使用 [**R shiny**](https://shiny.rstudio.com/) 开发的，它让你只用 R 代码就能构建和托管交互式 web 应用。对于 IDE，我使用的是 [IntelliJ 社区版](https://www.jetbrains.com/idea/)，其中有一个 R 插件，可以让你像在最流行的 R IDE 中一样有效地编写 R 代码；r 工作室。我使用的其他库有:

*   [**Shinydashboard**](https://rstudio.github.io/shinydashboard/) :提供了很多预先配置好的仪表盘元素，可以相对容易的集成。
*   [**Tidyverse**](https://www.tidyverse.org/) :为数据争论和处理而设计的 R 包的集合，它们共享底层的设计哲学、语法和数据结构。
*   [**传单**](https://rstudio.github.io/leaflet/) :围绕最流行的用于交互式地图的开源 JavaScript 库的包装器库。
*   [**Plotly**](https://plot.ly/r/) :一个绘图库，允许你创建多种颜色和形状的交互式绘图。
*   [**WBstats**](https://cran.r-project.org/web/packages/wbstats/vignettes/Using_the_wbstats_package.html) :一个包，只需要一行代码就可以访问和下载世界银行的数据。

仪表板目前托管在 [shinyapps.io](https://www.shinyapps.io/) 上，它有一个免费使用计划，允许您访问 shinyapps.io 云上的一些有限资源。由于资源非常有限，我需要评估更换另一家服务器/云提供商是否有意义。尽管如此，shinyapps.io 是一种发布 R 应用程序的快捷方式。

## 为什么要开源？

那么为什么创建一个**开源**版本的仪表板很重要呢？

1.  开源是免费的，每个人都可以使用。
2.  如果你创造了一些开源的东西，并向其他人展示你的代码，错误将会很快被发现并修复。
3.  使用群体智能来改进你的产品，或者获得解决某些问题的其他想法。
4.  迅速从各方面获得反馈，这样你就可以提高技能，学习新事物。

Github 的创始人 Tom Preston-Werner 大约十年前写了一篇关于为什么我们应该(几乎)开源所有东西的伟大文章,他的观点今天仍然适用！

# 捐助

这个项目的想法是创建一个使用免费工具的开源仪表板。我希望这个仪表板有助于为全球疫情提供更多的见解。如果你对如何改进仪表板有任何想法，或者对新的情节或这方面的任何事情，**请贡献**。你可以在 [Github](https://github.com/chschoenenberger/covid19_dashboard) 上找到所有代码。如果你有问题，需要帮助或类似的事情，请给我留言。我很乐意帮忙！

你可以在这里找到仪表盘[，在这里](https://chschoenenberger.shinyapps.io/covid19_dashboard/)找到代码[。](https://github.com/chschoenenberger/covid19_dashboard)

***编者按:*** [*走向数据科学*](http://towardsdatascience.com/) *是一份以数据科学和机器学习研究为主的中型刊物。我们不是健康专家或流行病学家，本文的观点不应被解释为专业建议。想了解更多关于疫情冠状病毒的信息，可以点击* [*这里*](https://www.who.int/emergencies/diseases/novel-coronavirus-2019/situation-reports) *。*

# 更新:

上周末，我创建了一个专门针对瑞士的仪表盘。如果您想为您所在的国家制作一个特定的仪表板，请派生存储库并添加您所在国家的数据。如果您有任何问题或需要一些输入，请随时给我留言！

# 来源

1.  [https://www . medical news today . com/articles/why-are-新冠肺炎-死亡率-如此难以计算-专家权衡](https://www.medicalnewstoday.com/articles/why-are-covid-19-death-rates-so-hard-to-calculate-experts-weigh-in)
2.  [https://time.com/5798168/coronavirus-mortality-rate/](https://time.com/5798168/coronavirus-mortality-rate/)

## 数据

*   [约翰·霍普金斯 Cases 新冠肺炎病例](https://github.com/CSSEGISandData/COVID-19)
*   [世界银行人口数据](https://data.worldbank.org/indicator/SP.POP.TOTL)

感谢[迷离情骇、](https://medium.com/u/6dbb32ff7eb6?source=post_page-----fa1d2b4cd985--------------------------------)艾弗和托马斯的校对！

[](https://www.linkedin.com/in/cschonenberger/) [## Christoph schnenberger-数据科学家-苏黎世集团| LinkedIn

### 在我的童年，我小心翼翼地将乐高积木分类、组织并组装成新的东西。今天…

www.linkedin.com](https://www.linkedin.com/in/cschonenberger/)