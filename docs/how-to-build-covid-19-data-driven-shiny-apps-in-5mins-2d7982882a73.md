# 如何在 5 分钟内构建新冠肺炎数据驱动的闪亮应用

> 原文：<https://towardsdatascience.com/how-to-build-covid-19-data-driven-shiny-apps-in-5mins-2d7982882a73?source=collection_archive---------60----------------------->

## 利用新冠肺炎数据中心构建闪亮的应用程序。一个 20 行代码的全功能示例。

***编者按:*** [*走向数据科学*](http://towardsdatascience.com/) *是一份以数据科学和机器学习研究为主的中型刊物。我们不是健康专家或流行病学家，本文的观点不应被解释为专业建议。想了解更多关于疫情冠状病毒的信息，可以点击* [*这里*](https://www.who.int/emergencies/diseases/novel-coronavirus-2019/situation-reports) *。*

![](img/7f8fb4bc4c5af49f72f618ca2d04df8e.png)

徽标由[加里·桑多兹](http://www.garysandoz.ch/index.html)和[跟我说话](https://www.talk-to-me.ch/)提供。

与新冠肺炎系统相关的数据库很多，但目前没有一个虚拟平台整合了这些来源的很大一部分。这样就很难进行全面的分析，也很难将这些医学信息与外部因素，尤其是社会政治因素联系起来。考虑到这一点，[新冠肺炎数据中心](https://covid19datahub.io)旨在开发一个统一的数据集，有助于更好地了解新冠肺炎。

在本教程中，我们将使用到新冠肺炎数据中心的 [R 包 COVID19](https://cran.r-project.org/package=COVID19) : R 接口构建一个简单而完整的闪亮应用程序。

假设对[闪亮的](https://shiny.rstudio.com/tutorial/)(网络应用)和[情节性的](https://plotly.com/r/)(互动情节)有基本的了解，但是可以简单地通过复制/粘贴来构建一个全功能的应用。加载以下软件包以开始使用:

```
library(shiny)
library(plotly)
library(COVID19)
```

## 新冠肺炎（新型冠状病毒肺炎）

COVID19 R 包通过`covid19()`功能提供了与[新冠肺炎数据中心](https://covid19datahub.io)的无缝集成。键入`?covid19`获取参数的完整列表。这里我们将使用:

*   `country`:国名或 ISO 代码的向量。
*   `level`:粒度级别；数据按(1)国家，(2)地区，(3)城市。
*   `start`:计息期的开始日期。
*   `end`:计息期的结束日期。

## 定义用户界面

定义以下输入…

*   `country`:国家名称。请注意，选项是使用`covid19()`功能自动填充的。
*   `type`:要使用的度量。其中一个是`c("confirmed", "tests", "recovered", "deaths")`，但还有许多其他的可供选择。完整列表见[此处](https://covid19datahub.io/articles/doc/data.html)。
*   `level`:粒度级别(国家-地区-城市)。
*   `date`:开始和结束日期。

…以及输出:

*   `covid19plot` : plotly 输出，将渲染一个交互的 plot。

把一切都包装成一个`fluidPage`:

```
# Define UI for application
ui <- fluidPage(

 selectInput(
  "country", 
  label    = "Country", 
  multiple = TRUE, 
  choices  = unique(covid19()$administrative_area_level_1), 
  selected = "Italy"
 ), selectInput(
  "type", 
  label    = "type", 
  choices  = c("confirmed", "tests", "recovered", "deaths")
 ), selectInput(
  "level", 
  label    = "Granularity", 
  choices  = c("Country" = 1, "Region" = 2, "City" = 3), 
  selected = 2
 ), dateRangeInput(
  "date", 
  label    = "Date", 
  start    = "2020-01-01"
 ),   

 plotlyOutput("covid19plot")

)
```

## 服务器逻辑

在 UI 中定义了反馈输入之后，我们将这些输入连接到`covid19()`函数来获取数据。以下代码片段显示了如何呈现交互式绘图(ly ),当任何输入发生变化时，该绘图会自动更新。请注意，`covid19()`功能使用内部**内存缓存**系统，因此数据不会被下载两次。多次调用该函数是非常高效和用户友好的。

```
# Define server logic
server <- function(input, output) { output$covid19plot <- renderPlotly({
  if(!is.null(input$country)){

   x <- covid19(
    country = input$country, 
    level   = input$level, 
    start   = input$date[1], 
    end     = input$date[2]
   )

   color <- paste0("administrative_area_level_", input$level)
   plot_ly(x = x[["date"]], y = x[[input$type]], color = x[[color]]) }
 })

}
```

## 运行应用程序

函数`shinyApp`从上面实现的`ui`和`server`参数构建一个应用程序。

```
# Run the application 
shinyApp(ui = ui, server = server)
```

在[https://guidotti.shinyapps.io/h83h5/](https://guidotti.shinyapps.io/h83h5/)有售

## 结束语

我们构建了一个与 R 包 COVID19 接口的简单应用程序，它代表了一个可重用的通用架构。示例应用程序可以用作更高级的新冠肺炎数据驱动应用程序的构建块。特别是，通过`covid19()`功能获得的数据集包括关于新冠肺炎案例、政策措施、地理信息和外部关键字的额外指标，这些指标允许使用[世界银行公开数据](https://data.worldbank.org/)、[谷歌移动报告](https://www.google.com/covid19/mobility/)、[苹果移动报告](https://www.apple.com/covid19/mobility)和当地政府数据轻松扩展数据集。参见[完整数据集文档](https://covid19datahub.io/articles/doc/data.html)和 [COVID19 代码片段](https://covid19datahub.io/articles/api/r.html)。

新冠肺炎数据中心是免费软件，没有任何担保。请确保同意[使用条款](https://covid19datahub.io/LICENSE.html)。

[1]吉多蒂，即阿尔迪亚，d .(2020)。[新冠肺炎数据中心](https://doi.org/10.21105/joss.02376)，《开源软件杂志》，5(51):2376