# 初学者改进 R 中 plotly 图表的 3 个简单技巧

> 原文：<https://towardsdatascience.com/3-easy-tricks-for-beginners-to-improve-your-plotly-charts-in-r-86d65772a701?source=collection_archive---------28----------------------->

## 简单的改进和定制让您的图表脱颖而出

![](img/99db09af71005c86deec988ab268fc61.png)

卢克·切瑟在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我们都经历过数据科学项目的尾声:我们清理了所有数据，探索了数据，获得了有价值的见解，并通过网络工具将结果提供给企业。我们甚至已经用 plotly 制作了令人敬畏的交互式可视化！

唯一的问题是，这些图表的现成格式很一般。同事对分析洞察力印象深刻，但你会听到他们说“我希望图表更好，这样我们就可以展示给客户看了！”或者“为什么光标看起来这么奇怪？”您还在想如何为未来的项目改进图表。

如果这就是你(或者如果你想更多地定制你的图表)，这里有 3 个简单的技巧来改进你的 R 绘图图表(这些技巧也很容易移植到其他语言，如 Python)。

在我们开始之前，我们需要一个可以改进的样本图表。下面是一个简单的条形图，显示了三种不同产品在一年中的销售情况。

![](img/5debd54f19fa9897c6e0e1cde587af8e.png)

条形图示例(图片由作者提供)

# 技巧 1:删除/定制模式栏

***模式栏*** 是位于右上角的小菜单，鼠标悬停在图表上时会显示出来。

![](img/40f10a7643b8d368ffbdfaecc5f49404.png)

标准模式栏(图片由作者提供)

如果您不想让它出现，或者您只是不想向用户提供某些功能，可以很容易地禁用该功能。

```
fig %>% config(displayModeBar = FALSE)
```

或者，用户也可以自定义模式栏，只显示某些选项，例如下载绘图为图像按钮。

```
fig %>% config(modeBarButtons = list(list("toImage")), displaylogo = FALSE, toImageButtonOptions = list(filename = "plotOutput.png"))
```

欲了解更多定制可能性，请点击此处查看 plotly-r 文档。

# 技巧 2:定制光标

悬停在绘图图表上时显示的标准光标是十字准线。有些人可能喜欢这样，但我个人更喜欢默认的箭头光标。

要实现这一点，最简单的方法是使用 htmlwidgets 包中的 onRender 函数和一点 JavaScript。

```
fig %>% onRender("function(el, x) {Plotly.d3.select('.cursor-crosshair').style('cursor', 'default')}")
```

# 技巧 3:自定义悬停信息

正如我们在基本条形图中看到的，显示的标准悬停信息是 *(x 值，y 值)*，旁边是系列名称(在我们的案例中是笔记本电脑或电视)。要添加更多的信息和格式，我们可以创建一个单独的变量并将其添加到悬停中，例如，如果我们想显示每月总销售额的百分比，我们可以创建一个包含百分比值的新列，并在 plot_ly 函数调用中使用该列作为文本参数。

```
#add percentages to our data
data <- data %>%
  group_by(variable) %>%
  mutate(pct = value / sum(value), pct = scales::percent(pct))#updated plot_ly function call
plot_ly(x = ~dates, y = ~value, type = 'bar', text = ~pct, name = ~variable, color = ~variable)
```

![](img/99de56630396145df86894d5f3e86105.png)

带首次悬停定制的条形图(图片由作者提供)

如果我们想更进一步，显示一些自定义文本，就没有必要向数据中添加另一列，它的唯一用途是显示一些文本。我们可以用 plot_ly 的 hovertemplate 参数来代替！

通过使用%{var}语法可以访问数据集的各个部分，例如 x 和 y 值、文本参数甚至数据集本身(通过使用 fullData.var，请参见附加部分)。

```
#updated plot_ly function call
plot_ly(data, x = ~dates, y = ~value, type = 'bar', name = ~variable, color = ~variable, text = ~pct, hovertemplate = "Units sold: %{y} <br> Percentage: %{text}")
```

![](img/80c993a13f21516b1773350e34b822f1.png)

具有高级悬停自定义功能的条形图(图片由作者提供)

## 奖金:

虽然这个 hoverinfo 看起来已经好多了，但是如果将“售出的数量”替换为“售出的电视”，而不是在盒子外面显示系列名称，不是更好吗？轻松点。使用${fullData.name}访问系列名称，并将其添加到 hovertemplate。要删除框旁边多余的文本，添加空的 html 标签<extra>，不要忘记关闭它！</extra>

```
#updated plot_ly function call
plot_ly(data, x = ~dates, y = ~value, type = 'bar', name = ~variable, color = ~variable, text = ~pct, hovertemplate = "%{fullData.name} sold: %{y}<br>Percentage: %{text} <extra></extra>")
```

![](img/d55307c55b557e23b741fd4c20438e70.png)

带有完整悬停定制的条形图(图片由作者提供)

# 结论

在这篇文章中，我展示了 3 个快速简单的技巧，您可以在开始使用 plotly in R 时立即使用。这些方法允许您增强和定制图表的某些方面，并使您和您的用户能够从数据中获得更多的洞察力。所有这一切只需几行简单的代码，没有任何复杂的格式！