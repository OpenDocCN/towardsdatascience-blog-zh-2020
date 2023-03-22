# 如何使用 R Shiny 进行 EDA 和预测

> 原文：<https://towardsdatascience.com/how-to-use-r-shiny-for-eda-and-prediction-72e6ef842240?source=collection_archive---------6----------------------->

![](img/dc8d468a028c488c048d55b8b5226367.png)

奥比·奥尼耶德在 [Unsplash](https://unsplash.com/s/photos/bike-share?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## R Shiny 分析、探索和预测自行车共享注册的简单指南

本文的目的是提供一个简单的指南，介绍如何开发一个出色的应用程序来分析、探索和预测数据集中的变量。

文章的第一部分涵盖了 R 闪亮的基础知识，比如对其功能的解释。此外，我将开发一个交互式图表形式的自行车共享数据的探索性数据分析。然后，我将创建一个预测模型，通过考虑天气条件和一年中的特定日子来帮助应用程序的用户预测系统中自行车注册的总数。

此外，我将在数据集包含的信息的上下文中描述要获取的数据。为了使 web 应用程序有一个目的，在数据理解部分，我将创建几个业务问题，我将通过这些问题来构建 R Shiny。

然后，通过使用 R，我将按照正确的格式排列数据，以构建机器学习模型和闪亮的应用程序。最后，我将展示代码并解释如何创建一个 R Shiny 应用程序的步骤。

# 内容

1.  什么是 R 闪亮？
2.  数据理解
3.  数据准备
4.  建模
5.  埃达在 R 闪亮
6.  R 闪亮中的预测

# 什么是 R 闪亮？

R Shiny 是一个 R 包，它能够直接从 R 构建交互式 web 页面应用程序，而无需使用任何 web 应用程序语言，如 HTML、CSS 或 JavaScript 知识。

Shiny 的一个重要特性是，这些应用程序在某种程度上是“活的”,因为网页的输出会随着用户修改输入而改变，而无需重新加载浏览器。

Shiny 包含两个基本参数，UI 和服务器。用户界面(UI)包含描述页面布局的所有文本代码、任何附加文本、图像和我们想要包含的其他 HTML 元素，以便用户可以进行交互并理解如何使用网页。另一方面，服务器是闪亮应用程序的后端。该参数创建一个 web 服务器，专门用于在受控环境中托管闪亮的应用程序。

# 数据理解

用于开发 R Shiny 应用程序的数据集名为 ***自行车共享数据集*** (具体来说就是“hour.csv”文件)，取自 [UCI 机器学习资源库](https://archive.ics.uci.edu/ml/datasets/bike+sharing+dataset#)。

我收集的数据来自名为“ [Capital Bikeshare](https://www.capitalbikeshare.com/about) ”的自行车共享系统。该系统由位于 DC 华盛顿州六个不同辖区的一组自行车组成。自行车被锁在停靠站的整个网络中，供用户进行日常运输。用户可以通过应用程序解锁自行车，并在完成骑行后，将它们归还到任何其他可用的坞站。

该数据集包含 2011 年至 2012 年间每小时的自行车注册数，考虑了天气条件和季节信息。它包含 16 个属性和 17，379 个观察值，其中每一行数据代表从 2011 年 1 月 1 日开始的一天中的一个特定小时；直到 2012 年 12 月 31 日。

共有 9 个分类变量，如下所示:

1.  日期:日期
2.  季节:季节(1 =冬天，2 =春天，3 =夏天，4 =秋天)
3.  年:年(0 = 2011 年，1 = 2012 年)
4.  月:月(1 至 12)
5.  小时:小时(0 到 23)
6.  假日:天气日是否为假日(0 =否，1 =是)
7.  工作日:一周中的某一天
8.  工作日:天气日是否为工作日(如果该日既不是周末也不是节假日，则为 1，否则为 0)
9.  天气状况(1 =晴朗，少云，部分多云，部分多云；2 =薄雾+多云，薄雾+碎云，薄雾+少云，薄雾；3 =小雪、小雨+雷雨+散云、小雨+散云；4 =暴雨+冰托盘+雷暴+薄雾、雪+雾)

此外，数据有 7 个数值变量:

1.  temp:以摄氏度为单位的标准化温度(这些值是通过(t-t_min)/(t_max-t_min)，t_min=-8，t_max=+39 得出的)
2.  atemp:以摄氏度为单位的标准化感觉温度(该值通过(t-t_min)/(t_max-t_min)得出，t_min=-16，t_max=+50)
3.  嗡嗡声:标准化湿度(数值分为 100(最大))
4.  风速:标准化风速(数值分为 67(最大值)
5.  临时:特定小时内使用自行车共享系统的临时用户数
6.  已注册:特定小时内注册使用自行车共享系统的新用户数
7.  计数:特定小时内租赁自行车总数(包括临时自行车和注册自行车)

## 商业问题

一旦我理解了这些数据，我就开发了一些我将在本文中回答的业务问题，以展示如何在现实世界中使用 R Shiny 应用程序。

1.  数据集中新注册和临时注册的数量是否有显著差异？
2.  在这两年中，骑自行车时最主要的天气条件是什么？
3.  2012 年，自行车共享系统的新用户注册数量最多的月份是哪个月？
4.  在 2012 年的“良好”天气条件下，哪个季节的临时用户和新用户注册数量最多？
5.  该公司希望预测 2020 年 5 月 14 日(工作日)下午 3 点的自行车注册数量；提供以下天气信息:

*   将会有 35%的湿度。
*   温度 17 摄氏度，感觉温度 15 摄氏度。
*   每小时 10 英里的风速。
*   和“晴朗”天气类型。

# 数据准备

为了将数据以正确的格式输入到 R Shiny 应用程序中，首先，我准备了数据。为此，我将数据集加载到 RStudio 中，并检查每个属性的数据类型。

```
## Bike-sharing initial analysis
#Import the dataset
data <- read.csv('hour.csv')[,-1]
str(data)
```

> 有必要注意数据集的第一列是行索引，它不包含任何预测值。因此，我决定加载数据，排除原始数据集的第一列。

![](img/c83cd46d399443d2b428030c90cb7828.png)

来自 **str()** 函数的结果

现在，一旦加载了变量，检查它们拥有正确的数据类型是绝对重要的。可视化结果，我们可以看到九个分类变量中的八个具有不正确的数据类型。为此，我将数据类型从 *int* (整数)改为 *factor* 。此外，我将编号值更改为它们各自的类别名称，以使它们的操作更容易理解。

有必要提及的是，我遵循了一个不同的标准来标记天气状况。通过分析天气的每一个输出，我决定把它们标上从“好”到“非常坏”的等级；其中 1 代表好天气，4 代表非常糟糕的天气。

```
#Data preparation
  #Arranging values and changing the data type
data$yr <- as.factor(ifelse(data$yr == 0, '2011', '2012'))data$mnth <- as.factor(months(as.Date(data$dteday), 
                              abbreviate = TRUE))data$hr <- factor(data$hr)data$weekday <- as.factor(weekdays(as.Date(data$dteday)))data$season <- as.factor(ifelse(data$season == 1, 'Spring',
                                ifelse(data$season == 2, 'Summer',
                                       ifelse(data$season == 3, 
                                              'Fall', 'Winter'))))data$weathersit <- as.factor(ifelse(data$weathersit == 1, 'Good',
                                    ifelse(data$weathersit == 2, 
                                           'Fair',
                                           ifelse(data$weathersit == 
                                                    3, 'Bad', 
                                                  'Very Bad'))))data$holiday<-as.factor(ifelse(data$holiday == 0, 'No', 'Yes'))data$workingday<-as.factor(ifelse(data$workingday == 0, 'No', 
                                  'Yes'))
```

此外，我继续将“注册”和“cnt”列分别更改为“新”和“总计”。

> 这种变化不是必需的，但它将允许我区分新的和注册总数。

```
 #Changing columns names
names(data)[names(data) == "registered"] <- "new"
names(data)[names(data) == "cnt"] <- "total"
```

最后，对于数据准备的最后一步，我将变量“temp”、“atemp”、“hum”和“windspeed”的值反规格化；以便以后我可以在探索性数据分析和预测模型中分析真实的观察结果。

```
 #Denormalizing the values
    #Temperature
for (i in 1:nrow(data)){
  tn = data[i, 10]
  t = (tn * (39 - (-8))) + (-8)
  data[i, 10] <- t
} #Feeling temperature
for (i in 1:nrow(data)){
  tn = data[i, 11]
  t = (tn * (50 - (-16))) + (-16)
  data[i, 11] <- t
} #Humidity
data$hum <- data$hum * 100 #Wind speed
data$windspeed <- data$windspeed * 67
```

对于最终的安排，我删除了数据集的第一列，因为它对于研究来说并不重要。一旦我有了正确类型的所有变量和所需格式的所有值，我就用当前排列的数据编写一个新文件。

> 此外，我将使用这个新文件来构建 R Shiny 应用程序的图形。

```
#Write the new file
data <- data[-1]
write.csv(data, "bike_sharing.csv", row.names = FALSE)
```

# 建模

对于该模型，我想预测在考虑时间框架(月、小时和星期几)和天气条件的情况下，一天中可能发生的注册总数。对于这个问题，第一步是删除不属于模型的列。

“季节”和“工作日”列被删除，因为变量“月份”和“工作日”可以提供相同的信息。通过指示月份，我们知道一年中的哪个季节相对应。此外，通过提供关于一周中某一天的信息，我们可以确定这一天是否是工作日。

由于预测模型承认天气状况和某一天的特定信息，可变年份不提供模型所需的任何基本信息。对于这个问题，我从数据框架中删除了变量“yr”。同样，模型中不包括列“casual”和“new ”,因为模型的目的是预测注册总数。

```
#Modeling
  #Dropping columns
data <- data[c(-1,-2,-7,-13,-14)]
```

## 拆分数据

既然数据已经为建模做好了准备，我继续将数据分成训练集和测试集。此外，为了将数据用于 R Shiny 应用程序，我将这些数据集保存到一个新文件中。我使用了 0.8 的分割比率，这意味着训练数据将包含总观察值的 80%，而测试将包含剩余的 20%。

```
 #Splitting data
library(caTools)set.seed(123)
split = sample.split(data$total, SplitRatio = 0.8)
train_set = subset(data, split == TRUE)
test_set = subset(data, split == FALSE) #Write new files for the train and test sets
write.csv(train_set, "bike_train.csv", row.names = FALSE)
write.csv(test_set, "bike_test.csv", row.names = FALSE)
```

## 模型

此外，我继续选择模型进行预测。由于因变量是数字(总注册数)，这意味着我们正在进行回归任务。为此，我预先选择了多重线性回归、决策树和随机森林模型来预测因变量的结果。

下一步，我用训练数据创建了回归模型。然后，我通过计算平均绝对误差(MAE)和均方根误差(RMSE)来评估它的性能。

```
 #Multilinear regression
multi = lm(formula = total ~ ., data = train_set) #Predicting the test values
y_pred_m = predict(multi, newdata = test_set) #Performance metrics
library(Metrics)mae_m = mae(test_set[[10]], y_pred_m)
rmse_m = rmse(test_set[[10]], y_pred_m)
mae_m
rmse_m
```

![](img/a4b4c9d22947cfbcbc09411e25036d03.png)

多重线性回归梅伊和 RMSE 结果

```
 #Decision tree
library(rpart)dt = rpart(formula = total ~ ., data = train_set,
           control = rpart.control(minsplit = 3)) #Predicting the test values
y_pred_dt = predict(dt, newdata = test_set) #Performance metrics
mae_dt = mae(test_set[[10]], y_pred_dt)
rmse_dt = rmse(test_set[[10]], y_pred_dt)
mae_dt
rmse_dt
```

![](img/e83276fbe78043d2aa7313ec42fd626e.png)

决策树回归梅和 RMSE 结果

```
 #Random forest
library(randomForest)set.seed(123)
rf = randomForest(formula = total ~ ., data = train_set,
                  ntree = 100) #Predicting the test values
y_pred_rf = predict(rf, newdata = test_set) #Performance metrics
mae_rf = mae(test_set[[10]], y_pred_rf)
rmse_rf = rmse(test_set[[10]], y_pred_rf)
mae_rf
rmse_rf
```

![](img/34519be226453667aa251012561797f8.png)

随机森林回归梅和 RMSE 结果

一旦我有了所有模型的性能指标，我就继续分析它们，以最终选择最佳模型。众所周知，平均绝对误差(MAE)和均方根误差(RMSE)是衡量回归模型准确性的两个最常见的指标。

由于 MAE 提供预测误差的平均值，因此最好选择 MAE 值较小的模型。换句话说，在 MAE 较低的情况下，预测中的误差幅度很小，使得预测更接近真实值。根据这些标准，我选择了随机森林模型，它的 MAE 等于 47.11。

此外，我继续分析 RMSE。该指标还通过计算预测值和实际观测值之间的平均平方差的平方根来测量误差的平均大小，这意味着它会对较大的误差给予较高的权重。换句话说，使用较小的 RMSE，我们可以在模型中捕获较少的错误。也就是说，随机森林是正确的选择。此外，随机森林的 MAE 和 RMSE 之间的差异很小，这意味着个体误差的方差较低。

现在我知道了随机森林模型是最好的模型，需要将该模型保存在一个文件中，以便在 Shiny 应用程序中使用。

```
#Saving the model
saveRDS(rf, file = "./rf.rda")
```

# 埃达在 R 闪亮

在项目的这一步，我构建了一个 R Shiny 应用程序，可以在其中对数字和分类自变量进行单变量分析。此外，我开发了一个交互式仪表板来回答本文开头提出的业务问题。

在一个新的 R 文档中，我开始创建 R 闪亮的应用程序。首先，我加载了在整个闪亮应用程序和数据集中使用的必要库。

```
#Load libraries
library(shiny)
library(shinydashboard)
library(ggplot2)
library(dplyr)#Importing datasets
bike <- read.csv('bike_sharing.csv')
bike$yr <- as.factor(bike$yr)
bike$mnth <- factor(bike$mnth, levels = c('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
bike$weekday <- factor(bike$weekday, levels = c('Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'))
bike$season <- factor(bike$season, levels = c('Spring', 'Summer', 'Fall', 'Winter'))
```

## r 闪亮的 UI

首先，我创建了 web 页面(UI ),它将显示探索性数据分析的所有信息和可视化。

UI 将是一个变量，包含关于元素的信息，如标题、侧栏和仪表板的主体。要用所有必要的组件构建一个新的仪表板页面，请遵循下面的代码。

```
ui <- dashboardPage(dashboardHeader(), 
                    dashboardSidebar(),
                    dashboardBody())
```

此外，我继续设计标题，创建侧边栏选项卡，并指明正文信息。

> 为了更好地理解代码，我将把它分成几个独立的部分。但是，有必要知道，为了让应用程序正常工作，我们必须一起运行所有的部分。

对于仪表板标题，我指定了闪亮应用程序的标题名称，并安排了标题显示的空间宽度。

```
#R Shiny ui
ui <- dashboardPage(

  #Dashboard title
  dashboardHeader(title = 'BIKE SHARING EXPLORER', 
                  titleWidth = 290),
```

然后，我定义了侧边栏布局，设置了侧边栏的宽度。然后，用 *sidebarMenu()* 函数，我指定了侧边栏菜单的标签数量和标签名称。为了设置选项卡，我使用了 *menuItem()* 函数，其中我提供了选项卡的实际名称(应用程序中显示的名称)、引用选项卡名称(用于在整个代码中调用选项卡的别名)，并定义了我想要附加到实际名称的图标或图形。

对于 EDA，我创建了两个名为 Plots 和 Dashboard 的选项卡。在第一个选项卡中，我显示了所有变量的单变量分析。另一个选项卡显示特定变量的双变量分析，以回答业务问题。

```
 #Sidebar layout
  dashboardSidebar(width = 290,
                   sidebarMenu(menuItem("Plots", tabName = "plots", icon = icon('poll')),
                               menuItem("Dashboard", tabName = "dash", icon = icon('tachometer-alt')))),
```

此外，在仪表板正文中，我描述了创建的每个选项卡的内容和布局。

首先，我使用了一个 CSS 组件，它是一种可以在 HTML 文档中使用的样式语言，将标题更改为粗体。此外，在 *tabItems()，*中，我设置了上面创建的每个选项卡的内容。使用 *tabItem()* 函数，我定义了引用标签名，然后描述了它将包含的不同元素。

一旦我确定了引用标签名(' plots ')，我就通过使用 *box()* 函数找到四个不同的空格来显示信息。使用这个函数，我定义了框的样式(状态)、标题、添加可选的页脚、控件和绘图输出。

在这段代码中，首先，我定义了四个盒子。其中两个框包含一个小部件，另外两个框显示稍后在服务器中创建的图。使用的控件小部件是 *selectInput()，*，这是一个下拉菜单，有不同的选项可供选择。在同一个函数中，我描述了小部件的引用名、实际名称以及将出现在菜单上的选项列表。另一方面，我用了最后两个方框来揭示剧情的输出。为此，我定义了函数 *plotOutput()* 并指明了它的引用名。

```
#Tabs layout
  dashboardBody(tags$head(tags$style(HTML('.main-header .logo {font-weight: bold;}'))),
                tabItems(
                         #Plots tab content
                         tabItem('plots', 
                                 #Histogram filter
                                 box(status = 'primary', title = 'Filter for the histogram plot', 
                                     selectInput('num', "Numerical variables:", c('Temperature', 'Feeling temperature', 'Humidity', 'Wind speed', 'Casual', 'New', 'Total')),
                                     footer = 'Histogram plot for numerical variables'),
                                 #Frecuency plot filter
                                 box(status = 'primary', title = 'Filter for the frequency plot',
                                     selectInput('cat', 'Categorical variables:', c('Season', 'Year', 'Month', 'Hour', 'Holiday', 'Weekday', 'Working day', 'Weather')),
                                     footer = 'Frequency plot for categorical variables'),
                                 #Boxes to display the plots
                                 box(plotOutput('histPlot')),
                                 box(plotOutput('freqPlot'))),
```

对于下一个选项卡项目(“破折号”)，我使用了与上面代码类似的设计。选项卡的布局包含三个框。第一个框拥有过滤器，其余的框拥有绘图空间。

在过滤框中，我使用了 *splitLayout()* 函数将框的布局分成三个不同的列。对于这些过滤器，我使用了一个名为 *radioButtons()，*的控件小部件，其中我指出了引用名、实际名称以及我们将在应用程序中使用的选项列表。

然后，在仪表板的其他框中，我指出了它的内容，比如图表，并声明了引用名称。同样，对于最后一个框，我输入了函数 *column()* ，在这里我指出了它的宽度。此外，我创建了两个不同的列，通过使用 *helpText()* 函数，我在其中描述了每个天气条件过滤器的含义。

```
 #Dashboard tab content
                         tabItem('dash',
                                 #Dashboard filters
                                 box(title = 'Filters', status = 'primary', width = 12,
                                     splitLayout(cellWidths = c('4%', '42%', '40%'),
                                                 div(),
                                                 radioButtons( 'year', 'Year:', c('2011 and 2012', '2011', '2012')),
                                                 radioButtons( 'regis', 'Registrations:', c('Total', 'New', 'Casual')),
                                                 radioButtons( 'weather', 'Weather choice:', c('All', 'Good', 'Fair', 'Bad', 'Very Bad')))),
                                 #Boxes to display the plots
                                 box(plotOutput('linePlot')),
                                 box(plotOutput('barPlot'), 
                                     height = 550, 
                                     h4('Weather interpretation:'),
                                     column(6, 
                                            helpText('- Good: clear, few clouds, partly cloudy.'),
                                            helpText('- Fair: mist, cloudy, broken clouds.')),
                                     helpText('- Bad: light snow, light rain, thunderstorm, scattered clouds.'),
                                     helpText('- Very Bad: heavy rain, ice pallets, thunderstorm, mist, snow, fog.')))))
)
```

最后，我创建了一个空服务器来运行代码，并可视化这个闪亮应用程序的进度。

```
# R Shiny server
server <- shinyServer(function(input, output) {})shinyApp(ui, server)
```

![](img/9fbc9f3f959501a2a198d76468cc8744.png)

绘图选项卡

![](img/dcd33490ce1d87328aa3c90115b8efa2.png)

仪表板选项卡

由于我在服务器上没有输出，应用程序将显示空白空间，稍后我将在那里放置绘图。

## r 闪亮服务器

此外，在服务器变量中，我继续创建为分析而构建的图，以回答业务问题。

我创建的第一个图是直方图。通过使用*输出$histPlot* 向量，我访问了 UI 中指示的直方图绘图框，我继续将它分配给 *renderPlot({})* 反应函数。这个函数将从 UI 中获取数据，并将其实现到服务器中。

而且，我创建了一个名为“num_val”的新变量。此变量将存储数据集中引用数值变量筛选器的列的名称。现在，有了这个新变量，我开始构建直方图。

```
# R Shiny server
server <- shinyServer(function(input, output) {

  #Univariate analysis
  output$histPlot <- renderPlot({ #Column name variable
    num_val = ifelse(input$num == 'Temperature', 'temp',
                     ifelse(input$num == 'Feeling temperature', 'atemp',
                            ifelse(input$num == 'Humidity', 'hum',
                                   ifelse(input$num == 'Wind speed', 'windspeed',
                                          ifelse(input$num == 'Casual', 'casual',
                                                 ifelse(input$num == 'New', 'new', 'total'))))))

    #Histogram plot
    ggplot(data = bike, aes(x = bike[[num_val]]))+ 
      geom_histogram(stat = "bin", fill = 'steelblue3', 
                     color = 'lightgrey')+
      theme(axis.text = element_text(size = 12),
            axis.title = element_text(size = 14),
            plot.title = element_text(size = 16, face = 'bold'))+
      labs(title = sprintf('Histogram plot of the variable %s', num_val),
           x = sprintf('%s', input$num),y = 'Frequency')+
      stat_bin(geom = 'text', 
               aes(label = ifelse(..count.. == max(..count..), as.character(max(..count..)), '')),
      vjust = -0.6)
  })
```

接下来，我绘制了分类变量的频率图。执行与前面相同的步骤，我调用了 *output$freqPlot* 向量来使用 *renderPlot({})* 反应函数绘制图形。同样，我生成了一个新变量来存储与过滤器上选择的值相关的数据集的列名。然后，使用新的变量，我建立了频率图。

```
output$freqPlot <- renderPlot({#Column name variable
    cat_val = ifelse(input$cat == 'Season', 'season',
                     ifelse(input$cat == 'Year', 'yr',
                            ifelse(input$cat == 'Month', 'mnth',
                                   ifelse(input$cat == 'Hour', 'hr',
                                          ifelse(input$cat == 'Holiday', 'holiday',
                                                 ifelse(input$cat == 'Weekday', 'weekday',
                                                    ifelse(input$cat == 'Working day', 'workingday', 'weathersit')))))))

    #Frecuency plot
    ggplot(data = bike, aes(x = bike[[cat_val]]))+
      geom_bar(stat = 'count', fill = 'mediumseagreen', 
               width = 0.5)+
      stat_count(geom = 'text', size = 4,
                 aes(label = ..count..),
                 position = position_stack(vjust = 1.03))+
      theme(axis.text.y = element_blank(),
            axis.ticks.y = element_blank(),
            axis.text = element_text(size = 12),
            axis.title = element_text(size = 14),
            plot.title = element_text(size = 16, face="bold"))+
      labs(title = sprintf('Frecuency plot of the variable %s', cat_val),
           x = sprintf('%s', input$cat), y = 'Count')

  })
```

现在，我继续为 dashboard 选项卡构建图表。为了回答第三个业务问题，我创建了一个线形图来显示每月的注册数量。此外，我将能够按年份和注册类型过滤图表。

首先，我指出了输出向量和反应函数。然后，我开发了一个表，通过遵循特定的条件，该表将包含绘图中使用的必要列。

后来，考虑到用户在过滤器上选择的注册类型，我生成了一个新变量(“regis_val”)来存储数据集的列名。最后，我建立了线条图。

```
#Dashboard analysis
  output$linePlot <- renderPlot({

    if(input$year != '2011 and 2012'){

      #Creating a table filter by year for the line plot
      counts <- bike %>% group_by(mnth) %>% filter(yr == input$year) %>% summarise(new = sum(new), casual = sum(casual), total = sum(total))} else{

      #Creating a table for the line plot
      counts <- bike %>% group_by(mnth) %>% summarise(new = sum(new), casual = sum(casual), total = sum(total))}#Column name variable
    regis_val = ifelse(input$regis == 'Total', 'total',
                       ifelse(input$regis == 'New', 'new','casual'))

    #Line plot
    ggplot(counts, aes(x = mnth, y = counts[[regis_val]],
                       group = 1))+
      geom_line(size = 1.25)+
      geom_point(size = 2.25,
                 color = ifelse(counts[[regis_val]] == max(counts[[regis_val]]), 'red','black'))+
      labs(title = sprintf('%s bike sharing registrations by month', input$regis),
           subtitle = sprintf('Throughout the year %s \nMaximum value for %s registrations: %s \nTotal amount of %s registrations: %s', input$year, regis_val, max(counts[[regis_val]]), regis_val, sum(counts[[regis_val]])),
           x = 'Month', 
           y = sprintf('Count of %s registrations', regis_val))+
      theme(axis.text = element_text(size = 12),
            axis.title = element_text(size = 14),
            plot.title = element_text(size = 16, face = 'bold'),
            plot.subtitle = element_text(size = 14))+
      ylim(NA, max(counts[[regis_val]])+7000)+
      geom_text(aes(label = ifelse(counts[[regis_val]] == max(counts[[regis_val]]), as.character(counts[[regis_val]]),'')),
                col ='red',hjust = 0.5, vjust = -0.7) })
```

此外，我继续开发条形图来回答业务问题 4。我遵循了与上面代码相似的步骤。首先，我创建了一些限制来创建用于不同过滤器的表，该表将考虑年份和天气条件。然后，我创建了一个变量，它存储了在过滤器上选择的注册类型的列名。最后，构建条形频率图，按年份、注册类型和天气条件显示注册计数。

```
 output$barPlot <- renderPlot({

    if(input$year != '2011 and 2012'){

      if(input$weather != 'All'){

        #Creating a table filter by year and weathersit for the bar plot
        weather <- bike %>% group_by(season, weathersit) %>% filter(yr == input$year) %>%  summarise(new = sum(new), casual = sum(casual), total = sum(total))

        weather <- weather %>% filter(weathersit == input$weather)} else{

        #Creating a table filter by year for the bar plot
        weather <- bike %>% group_by(season, weathersit) %>% filter(yr == input$year) %>%  summarise(new = sum(new), casual = sum(casual), total = sum(total))

      }

    } else{

      if(input$weather != 'All'){

        #Creating a table filter by weathersit for the bar plot
        weather <- bike %>% group_by(season, weathersit) %>% filter(weathersit == input$weather) %>%  summarise(new = sum(new), casual = sum(casual), total = sum(total))

      } else{

        #Creating a table for the bar plot
        weather <- bike %>% group_by(season, weathersit) %>%  summarise(new = sum(new), casual = sum(casual), total = sum(total))

      }
    }

    #Column name variable
    regis_val = ifelse(input$regis == 'Total', 'total', 
                       ifelse(input$regis == 'New', 'new','casual'))

    #Bar plot
    ggplot(weather, aes(x = season, y = weather[[regis_val]], 
                        fill = weathersit))+
      geom_bar(stat = 'identity', position=position_dodge())+
      geom_text(aes(label = weather[[regis_val]]),
                vjust = -0.3, position = position_dodge(0.9), 
                size = 4)+
      theme(axis.text.y = element_blank(),
            axis.ticks.y = element_blank(),
            axis.text = element_text(size = 12),
            axis.title = element_text(size = 14),
            plot.title = element_text(size = 16, face = 'bold'),
            plot.subtitle = element_text(size = 14),
            legend.text = element_text(size = 12))+
      labs(title = sprintf('%s bike sharing registrations by season and weather', input$regis),
           subtitle = sprintf('Throughout the year %s', input$year),
           x = 'Season', 
           y = sprintf(Count of %s registrations', regis_val))+
      scale_fill_manual(values = c('Bad' = 'salmon2', 'Fair' = 'steelblue3', 'Good' = 'mediumseagreen', 'Very Bad' = 'tomato4'),
                        name = 'Weather')

  })
```

现在我已经在服务器中创建了图表，我继续运行代码来可视化这个闪亮的应用程序。

![](img/ab9576eb9dbfae88b0eaef57de18f10a.png)

带有图的图选项卡

![](img/794c14f0fe9f155c62e745f42cf9362a.png)

带有绘图的仪表板选项卡

## 结论

最后，我继续通过分析展示业务问题的答案。

*   ***数据集中新注册和临时注册的数量是否有显著差异？***

我继续展示图表来回答第一个问题。

![](img/8a1105ffa8cf1ac892a03714dc4f0447.png)

新登记类型变量的直方图

![](img/6478f55ffeaebe19ac6db00187c848f9.png)

可变临时登记类型的直方图

在分析“新”变量时，我指出，在整个数据集中，最少有 0 个新注册，最多大约有 875 个。另一方面，有一个最低的休闲注册 0 和粗略的最高 350。

正如我们所看到的，两个图都向右倾斜，这意味着每天记录的大多数注册分别低于新注册和临时注册类型的 250 和 100。

最后，为了回答第一个商业问题，我得出结论，新注册和临时注册的数量之间存在显著差异。该数据集显示，在 1500 多次观察中，用户每天进行的新注册不到 125 次。然而，在 1000 多次情况下，该系统每天捕获的临时登记不到 50 次。

换句话说，该系统在 2011 年和 2012 年，比使用该系统的临时用户有更多的新用户注册到首都自行车共享。造成这种现象的原因之一是系统的启动。自 2010 年启动该业务以来，在随后的几年里，新用户仍在注册。

*   ***在这两年中，骑自行车时最主要的天气条件是什么？***

为了回答这个问题，我展示了所需的情节。

![](img/a5d9e01c76da3561d718885dc4b2f6f1.png)

变量 weathersit 的频率图

通过观察图表，我得出结论，2011 年和 2012 年最常见的天气状况属于“良好”类型。该图显示，数据集中的 11，413 个观测值捕捉到了“好”天气，这意味着这两年的天气大多是晴朗的，很少有云，或部分多云。

*   ***2012 年，自行车共享系统的新用户注册数量最多的月份是哪个月？***

为了回答这个问题，我继续点击 dashboard 选项卡并显示下面的图。

![](img/e491a7964c4e7e5d6d8e540f4edb9a81.png)

2012 年新注册数量的时间序列图

一旦我按年份和注册类型过滤了图，我就获得了上面显示的结果。我们可以想象，新用户注册量最高的月份是在 9 月。新增用户约为 170，000 的原因之一是，9 月是秋季的开始，气温正在下降。换句话说，热量更少，天气更适合人们骑自行车上下班。

*   ***2012 年，在“良好”天气条件下，哪个季节的休闲和新注册人数最多？***

对于下面的问题，我继续从仪表板选项卡显示两个不同的图。

![](img/0ab8ad8883d5363f58c3e0bc926c2d47.png)

根据年份和天气条件过滤的绘图季节与临时注册数

![](img/9c6995238a597f786567225a49dea55a.png)

根据年份和天气条件筛选的绘图季节与新注册数

正如我们从之前的分析中所知，自行车共享系统中的新注册量远高于临时注册量。对于这个问题，新的注册地块将显示更高的注册用户数，而不管季节。

此外，我指出，在这两种类型的注册中，最高数量的注册发生在秋季，其次是夏季。正如我之前提到的，秋季是人们骑自行车上下班的理想天气。此外，夏季也呈现出相当高的注册数量，因为这是人们度假和使用自行车系统共享进行观光和旅游的季节。

# R 闪亮中的预测

为了回答最后一个问题，我开始部署模型。在这一步中，我在 R Shiny 应用程序中创建了一个新的选项卡。在该选项卡中，用户将能够输入每个变量的单个值，并获得注册总数和预测结果的值范围。

> **为了更好地理解代码，我将类型字符设置为“bold ”,以表示添加到 R Shiny 中的新文本。**

在闪亮的应用程序文档中，我继续添加新的库并导入预测所需的数据集。训练和测试数据都被引入到应用程序中以安排分类级别。此外，我加载了随机森林模型来收集平均绝对误差值。

```
#Load libraries
library(shiny)
library(shinydashboard)
library(ggplot2)
library(dplyr)
**library(randomForest)
library(Metrics)**#Importing datasets
bike <- read.csv('bike_sharing.csv')
bike$yr <- as.factor(bike$yr)
bike$mnth <- factor(bike$mnth, levels = c('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
bike$weekday <- factor(bike$weekday, levels = c('Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'))
bike$season <- factor(bike$season, levels = c('Spring', 'Summer', 'Fall', 'Winter'))**train_set <- read.csv('bike_train.csv')
train_set$hr <- as.factor(train_set$hr)****test_set <- read.csv('bike_test.csv')
test_set$hr <- as.factor(test_set$hr)****levels(test_set$mnth) <- levels(train_set$mnth)
levels(test_set$hr) <- levels(train_set$hr)
levels(test_set$holiday) <- levels(train_set$holiday)
levels(test_set$weekday) <- levels(train_set$weekday)
levels(test_set$weathersit) <- levels(train_set$weathersit)****#Importing model
model_rf <- readRDS(file = './rf.rda')
y_pred = predict(model_rf, newdata = test_set)
mae_rf = mae(test_set[[10]], y_pred)
rmse_rf = rmse(test_set[[10]], y_pred)**
```

## r 闪亮的 UI

现在，我在 UI 中加入了新的文本代码。我在 *dashboardSidebar()* 和 *dashboardBody()* 函数的末尾添加了新的代码行。

对于侧边栏，我添加了一个名为“pred”的新菜单项，表示闪亮应用程序中的新标签。

```
#R Shiny ui
ui <- dashboardPage(

  #Dashboard title
  dashboardHeader(title = 'BIKE SHARING EXPLORER', titleWidth = 290),

  #Sidebar layout
  dashboardSidebar(width = 290,
                   sidebarMenu(menuItem("Plots", tabName = "plots", icon = icon('poll')),
                               menuItem("Dashboard", tabName = "dash", icon = icon('tachometer-alt')),
                               **menuItem("Prediction", tabName = "pred", icon = icon('search'))**)),
```

然后，在仪表板正文中，我继续描述上面创建的新选项卡的内容。该选项卡有四个框。前两个框将包含控件小部件，用户可以在其中输入模型中每个变量的值。第一个框将包含所有的分类变量，其中我实现了四个*选择输入()*和一个*单选按钮()*小部件。

第二个框包含数字变量。为了输入湿度值，我使用了 *sliderInput()* 小部件从 0%到 100%的范围内选择湿度的百分比。这个小部件接受引用名称、实际名称、最小值、最大值和默认值作为参数。此外，对于剩余的变量，我使用了 *numericInput()* ，用户可以在这里输入变量的值。这个小部件的参数是引用名称、实际名称和默认值。

此外，我构建了下面的框来显示单个预测的结果。为了显示预测的结果和预测值的范围，我使用了一个名为 *verbatimTextOutput()* 的控件小部件，在这里我定义了引用名并将其标记为值的占位符。此外，我添加了一个带有 *actionButton()* 小部件的按钮，以便在用户输入所有值后计算结果。对于这个控件小部件，我将引用名称、实际名称和图标标识为参数。

最后，最后一个框的功能是传递信息，解释我选择的模型。

```
 #Tabs layout
  dashboardBody(tags$head(tags$style(HTML('.main-header .logo {font-weight: bold;}'))),
                        #Plots tab content
                tabItems(tabItem('plots', 
                                 #Histogram filter
                                 box(status = 'primary', title = 'Filter for the histogram plot',
                                     selectInput('num', "Numerical variables:", c('Temperature', 'Feeling temperature', 'Humidity', 'Wind speed', 'Casual', 'New', 'Total')),
                                     footer = 'Histogram plot for numerical variables'),
                                 #Frecuency plot filter
                                 box(status = 'primary', title = 'Filter for the frequency plot',
                                     selectInput('cat', 'Categorical variables:', c('Season', 'Year', 'Month', 'Hour', 'Holiday', 'Weekday', 'Working day', 'Weather')),
                                     footer = 'Frequency plot for categorical variables'),
                                 #Boxes to display the plots
                                 box(plotOutput('histPlot')),
                                 box(plotOutput('freqPlot'))), #Dashboard tab content
                         tabItem('dash',
                                 #Dashboard filters
                                 box(title = 'Filters', 
                                     status = 'primary', width = 12,
                                     splitLayout(cellWidths = c('4%', '42%', '40%'),
                                                 div(),
                                                 radioButtons( 'year', 'Year:', c('2011 and 2012', '2011', '2012')),
                                                 radioButtons( 'regis', 'Registrations:', c('Total', 'New', 'Casual')),
                                                 radioButtons( 'weather', 'Weather choice:', c('All', 'Good', 'Fair', 'Bad', 'Very Bad')))),
                                 #Boxes to display the plots
                                 box(plotOutput('linePlot')),
                                 box(plotOutput('barPlot'), 
                                     height = 550, 
                                     h4('Weather interpretation:'),
                                     column(6, 
                                            helpText('- Good: clear, few clouds, partly cloudy.'),
                                            helpText('- Fair: mist, cloudy, broken clouds.')),
                                     helpText('- Bad: light snow, light rain, thunderstorm, scattered clouds.'),
                                     helpText('- Very Bad: heavy rain, ice pallets, thunderstorm, mist, snow, fog.'))),**#Prediction tab content
                         tabItem('pred',
                                 #Filters for categorical variables
                                 box(title = 'Categorical variables', 
                                     status = 'primary', width = 12, 
                                     splitLayout(
                                  tags$head(tags$style(HTML(".shiny-split-layout > div {overflow: visible;}"))),
                                                 cellWidths = c('0%', '19%', '4%', '19%', '4%', '19%', '4%', '19%', '4%', '8%'),
                                                 selectInput( 'p_mnth', 'Month', c("Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul","Aug", "Sep", "Oct", "Nov", "Dec")),
                                                 div(),
                                                 selectInput('p_hr', 'Hour', c('0', '1', '2', '3', '4', '5', '6', '7', '8','9', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23')),
                                                 div(),
                                                 selectInput( 'p_weekd', 'Weekday', c('Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday')),
                                                 div(),
                                                 selectInput( 'p_weather', 'Weather', c('Good', 'Fair', 'Bad', 'Very Bad')),
                                                 div(),
                                                 radioButtons( 'p_holid', 'Holiday', c('Yes', 'No')))),
                                 #Filters for numeric variables
                                 box(title = 'Numerical variables',
                                     status = 'primary', width = 12,
                                     splitLayout(cellWidths = c('22%', '4%','21%', '4%', '21%', '4%', '21%'),
                                                 sliderInput( 'p_hum', 'Humidity (%)', min = 0, max = 100, value = 0),
                                                 div(),
                                                 numericInput( 'p_temp', 'Temperature (Celsius)', 0),
                                                 div(),
                                                 numericInput( 'p_ftemp', 'Feeling temperature (Celsius)', 0),
                                                 div(),
                                                 numericInput( 'p_wind', 'Wind speed (mph)', 0))),
                                 #Box to display the prediction results
                                 box(title = 'Prediction result',
                                     status = 'success', 
                                     solidHeader = TRUE, 
                                     width = 4, height = 260,
                                     div(h5('Total number of registrations:')),
                                     verbatimTextOutput("value", placeholder = TRUE),
                                     div(h5('Range of number of registrations:')),
                                     verbatimTextOutput("range", placeholder = TRUE),
                                     actionButton('cal','Calculate', icon = icon('calculator'))),
                                 #Box to display information about the model
                                 box(title = 'Model explanation',
                                     status = 'success', 
                                     width = 8, height = 260,
                                     helpText('The following model will predict the total number of bikes rented on a specific day of the week, hour, and weather conditions.'),
                                     helpText('The name of the dataset used to train the model is "Bike Sharing Dataset Data Set", taken from the UCI Machine Learning Repository website. The data contains 17,379 observations and 16 attributes related to time and weather conditions.'),
                                     helpText(sprintf('The prediction is based on a random forest supervised machine learning model. Furthermore, the models deliver a mean absolute error (MAE) of %s total number of registrations, and a root mean squared error (RMSE) of %s total number of registrations.', round(mae_rf, digits = 0), round(rmse_rf, digits = 0)))))**
                         )
                )
  )
```

添加 UI 代码后，我运行应用程序来可视化布局。

![](img/66ffed07343ce2f7f41c77237d261d1b.png)

带有默认值和模型说明的预测选项卡

## r 闪亮服务器

接下来，我继续添加服务器文本代码，它将使用模型计算单个预测。

首先，对于单个预测结果，我定义了一个 *reactiveValues()* 函数，其中我设置了一个默认值 NULL。当用户按下“计算”按钮时，该值将做出反应。

接下来，我使用了 *observeEvent()* 函数来执行预测的计算。这个函数将在闪亮的应用程序后面执行计算，并在用户调用函数开头提到的按钮时显示结果(*输入$cal* )。因为在结果和上面创建的反应值之间有一个链接，所以除非用户点击“计算”按钮，否则不会显示结果。现在，我继续调用*输出$value* ，它是单个预测结果的占位符，通过使用函数 *renderText({})* 在应用程序中显示结果。有必要提到的是，这个函数只提供字符串类型的结果。

最后，通过调用占位符显示范围结果(*输出$range* )使用函数 *renderText({})，*并调用函数中的活动按钮(*输入$ cal*)；闪亮的应用程序将在指定位置显示范围结果。

> 此外，由于“单变量分析”和“仪表板分析”部分的代码有许多行文本，为了更好地理解，我决定用符号“…”代替它。

```
# R Shiny server
server <- shinyServer(function(input, output) {

  #Univariate analysis
  output$histPlot <- renderPlot({...})
  output$freqPlot <- renderPlot({...}) #Dashboard analysis
  output$linePlot <- renderPlot({...})
  output$barPlot <- renderPlot({...})**#Prediction model
    #React value when using the action button
  a <- reactiveValues(result = NULL)

  observeEvent(input$cal, {
    #Copy of the test data without the dependent variable
    test_pred <- test_set[-10]

    #Dataframe for the single prediction
    values = data.frame(mnth = input$p_mnth, 
                        hr = input$p_hr,
                        weekday = input$p_weekd,
                        weathersit = input$p_weather,
                        holiday = input$p_holid,
                        hum = as.integer(input$p_hum),
                        temp = input$p_temp, 
                        atemp = input$p_ftemp, 
                        windspeed = input$p_wind)

    #Include the values into the new data
    test_pred <- rbind(test_pred,values)

    #Single preiction using the randomforest model
    a$result <-  round(predict(model_rf, 
                               newdata = test_pred[nrow(test_pred),]), 
                       digits = 0)
  })

  output$value <- renderText({
    #Display the prediction value
    paste(a$result)
  })

  output$range <- renderText({
    #Display the range of prediction value using the MAE value
    input$cal
    isolate(sprintf('(%s) - (%s)', 
                    round(a$result - mae_rf, digits = 0),
                    round(a$result + mae_rf, digits = 0)))
  })**

})

shinyApp(ui, server)
```

## 结论

此外，我继续使用应用程序来回答最后一个业务问题。

*   ***该公司希望预测某一天和天气条件下的自行车注册数量。***

参数:

1.  日期:2020 年 5 月 14 日星期四(是工作日)，下午 3 点。
2.  天气条件:35%的湿度，17 摄氏度的温度，15 摄氏度的感觉温度，10 英里/小时的风速，以及“晴朗”的天气类型。

最后，为了回答最后一个业务问题，我继续输入问题中显示的日期和天气值，然后单击“计算”按钮获得结果。

![](img/0d83297f3b3da4f7a3ce5fdf52b27919.png)

单一预测结果

从预测中获得的结果是该特定日期和天气条件下的注册总数为 228。此外，预测还揭示了预测值的变化范围。结果显示，该公司预计注册总数在 181 到 275 之间。

# 旁注

要找到所有的 R 代码和数据集，请访问[我的 GitHub 库](https://github.com/ClaudiaCartaya/Bike-sharing)。另外，在[我的 ShinnyApp](https://claudiacartaya.shinyapps.io/bike_sharing/?_ga=2.213120020.1031504369.1614209302-493917967.1614209302) 中可以找到 R shiny 应用。