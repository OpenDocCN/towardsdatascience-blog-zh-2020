# 电力商业智能预测

> 原文：<https://towardsdatascience.com/forecasting-in-power-bi-4cc0b3568443?source=collection_archive---------1----------------------->

## 数据科学/电力 BI 可视化

## 使用 Power BI 进行预测的直观分步指南。

![](img/bed8d322449d2aa2b2aa366619c58aed.png)

照片由[卢马·皮门特尔](https://unsplash.com/@lumapimentel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/s/photos/birth?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

*在本帖中，我们将介绍在 Power BI 中创建预测的过程。*

# 获取数据

你可以在这里下载我用的数据集。它包含了 1959 年加利福尼亚每天的女性出生人数。关于其他时间序列数据集的列表，请查看 Jason Brownlee 的文章。

[](https://machinelearningmastery.com/time-series-datasets-for-machine-learning/) [## 机器学习的 7 个时间序列数据集-机器学习掌握

### 机器学习可以应用于时间序列数据集。在这些问题中，数值或分类值必须…

machinelearningmastery.com](https://machinelearningmastery.com/time-series-datasets-for-machine-learning/) 

让我们将数据加载到 Power BI 中。打开 Power BI 并点击欢迎屏幕上的“获取数据”,如下所示。

![](img/675cbb4718b6dadcd5f2a275ee2c702c.png)

[作者](https://medium.com/@ednalyn.dedios)截图

接下来，您将看到另一个面板，询问我们想要获取什么类型的数据。选择如下所示的“文本/CSV”并点击“连接”

![](img/10215a92f9d95e4bd240a8689f29b4a9.png)

[作者截图](https://medium.com/@ednalyn.dedios)

当“文件打开”窗口出现时，导航到我们保存数据集的位置，然后单击右下角的“打开”按钮。

![](img/d1b533705c74249e41e44478ed594a2f.png)

[作者截图](https://medium.com/@ednalyn.dedios)

当预览出现时，只需点击“加载”

![](img/6a8a799b7c578914dc99dc611189f0f3.png)

[作者截图](https://medium.com/@ednalyn.dedios)

我们现在将看到 Power BI 的主要工作区域。前往“可视化”面板，寻找“折线图”

![](img/29deb5c09650d3af2769c0b484a42ed8.png)

。[作者截图](https://medium.com/@ednalyn.dedios)

这是折线图图标的外观:

![](img/bf0f6d12b27b0ae3ba904da09be9a9d4.png)

[作者截图](https://medium.com/@ednalyn.dedios)

接下来，将出现一个可视占位符。拖动占位符右下角的热角标记，并将其对角向下拖动到主工作区的右上角。

![](img/e1e58a1074bdf27bd5398eed64844beb.png)

截图由[作者](https://medium.com/@ednalyn.dedios)

![](img/c610ebf509b45c8b2b49eee731740fc6.png)

[作者截图](https://medium.com/@ednalyn.dedios)

接下来，打开“字段”面板。

![](img/8d205d4b59a2565c7a7a4377628e1058.png)

[作者截图](https://medium.com/@ednalyn.dedios)

在折线图占位符仍处于选中状态的情况下，找到“日期”字段，然后单击方形框以选中它。

![](img/e2cf1b20db21852191da65135fc20478.png)

[作者截图](https://medium.com/@ednalyn.dedios)

我们现在将看到轴下的“日期”字段。点击“日期”右侧的向下箭头，如下所示。

![](img/6678034c1e37eb016dcc859c68c40cf8.png)

截图由[作者](https://medium.com/@ednalyn.dedios)

选择“日期”而不是默认的“日期层次结构”

![](img/5c3282699af526c976442ddc3cae6c69.png)

截图由[作者](https://medium.com/@ednalyn.dedios)

然后，让我们在“出生”字段打上勾号。

![](img/bd2458eb05a13cd5f509cbe8915943eb.png)

[作者截图](https://medium.com/@ednalyn.dedios)

我们现在将看到一个类似下图的线形图。在可视化面板和图标列表下，找到如下所示的分析图标。

![](img/16871d121656aa52922e0a35056c4cd2.png)

[作者](https://medium.com/@ednalyn.dedios)截图

向下滚动面板，找到“预测”部分。如有必要，单击向下箭头将其展开。

![](img/8e2743e815f2406ccac730803684d873.png)

[作者截图](https://medium.com/@ednalyn.dedios)

接下来，单击“+Add”在当前可视化中添加预测。

![](img/a58dad892dde3ae03946cfbcee713ebc.png)

[作者截图](https://medium.com/@ednalyn.dedios)

我们现在将看到一个实心的灰色填充区域和一个位于可视化右侧的线图，如下图所示。

![](img/6ee1e337a80dc3d08bbff7c19b95d639.png)

[作者截图](https://medium.com/@ednalyn.dedios)

让我们把预测长度改为 31 点。在这种情况下，一个数据点相当于一天，所以 31 大致相当于一个月的预测值。单击预测组右下角的“应用”以应用更改。

![](img/9a1036d80411d1c2b2c839579cb3871a.png)

[作者截图](https://medium.com/@ednalyn.dedios)

让我们将度量单位改为“月”,而不是点，如下所示。

![](img/f3f98df074f043f36861b69395e0321b.png)

[作者截图](https://medium.com/@ednalyn.dedios)

单击“应用”后，我们将看到可视化效果的变化。下图包含 3 个月的预测。

![](img/7efc5465358261e7b857602fbedfb6a4.png)

截图由[作者](https://medium.com/@ednalyn.dedios)

如果我们想比较预测与实际数据的对比情况会怎样？我们可以通过“忽略最后一个”设置来做到这一点。

对于这个例子，让我们忽略过去 3 个月的数据。Power Bi 将使用数据集预测 3 个月的数据，但忽略过去 3 个月的数据。这样，我们可以将电力 BI 的预测结果与数据集最近 3 个月的实际数据进行比较。

如下所示，完成设置更改后，我们单击“应用”。

![](img/b2b12906fe6b56fef87ffcb24bbb6a75.png)

[作者截图](https://medium.com/@ednalyn.dedios)

下面，我们可以看到功率 BI 预测与实际数据的对比。黑色实线代表预测，而蓝色线代表实际数据。

![](img/54d5da183296a6b74a3210fb15853576.png)

[作者截图](https://medium.com/@ednalyn.dedios)

预测上的实心灰色填充表示置信区间。其值越高，面积就越大。让我们将置信区间降低到 75%,如下所示，看看它对图表的影响。

![](img/123467922f76bda2f8b0e50c9498db6e.png)

截图由[作者](https://medium.com/@ednalyn.dedios)

实心灰色填充变小，如下所示。

![](img/8b9ad167af5cf06b22eb664d039403dc.png)

截图由[作者](https://medium.com/@ednalyn.dedios)

接下来，让我们考虑季节性。下面，我们把它设定为 90 分相当于 3 个月左右。输入这个值将告诉 Power BI 在 3 个月的周期内寻找季节性。用根据数据有意义的东西来玩这个值。

![](img/ce054988e59aa65d3d4ed57bc9b0039e.png)

[作者截图](https://medium.com/@ednalyn.dedios)

结果如下所示。

![](img/9421791ae697b8387ca1cb40f56dba83.png)

[作者](https://medium.com/@ednalyn.dedios)截图

让我们将置信区间返回到默认值 95%,并向下滚动该组以查看格式选项。

![](img/42715fb3746ec7e476f13e8c06911dff.png)

[作者截图](https://medium.com/@ednalyn.dedios)

让我们将预测线更改为橙色，并通过将格式更改为“无”来使灰色填充消失

![](img/e4e1881be692368919a8b5ee9582547f.png)

[作者截图](https://medium.com/@ednalyn.dedios)

就是这样！只需点击几下鼠标，我们就能从数据集中得到一个预测。

*感谢您的阅读。如果你想了解更多关于我从懒鬼到数据科学家的旅程，请查看下面的文章:*

[](/from-slacker-to-data-scientist-b4f34aa10ea1) [## 从懒鬼到数据科学家

### 我的无学位数据科学之旅。

towardsdatascience.com](/from-slacker-to-data-scientist-b4f34aa10ea1) 

*如果你正在考虑改变方向，进入数据科学领域，现在就开始考虑重塑品牌:*

[](/the-slackers-guide-to-rebranding-yourself-as-a-data-scientist-b34424d45540) [## 懒鬼将自己重塑为数据科学家指南

### 给我们其他人的固执己见的建议。热爱数学，选修。

towardsdatascience.com](/the-slackers-guide-to-rebranding-yourself-as-a-data-scientist-b34424d45540) 

*敬请期待！*

你可以通过推特或 LinkedIn 联系我。

[1]机器学习掌握。(2020 年 6 月 21 日)。*机器学习的 7 个时间序列数据集。*[https://machine learning mastery . com/time-series-datasets-for-machine-learning/](https://machinelearningmastery.com/time-series-datasets-for-machine-learning/)