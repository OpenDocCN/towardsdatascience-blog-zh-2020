# 数据可视化的前 3 篇文章

> 原文：<https://towardsdatascience.com/top-3-articles-for-data-visualization-956a08a54b04?source=collection_archive---------39----------------------->

## 如果您想更好地构建数据可视化，这些文章很有帮助。

![](img/d1212aa1365670b6be079484b920e970.png)

艾萨克·史密斯在 [Unsplash](https://unsplash.com/) 上拍摄的照片

对数据科学家来说，可视化数据是一项很有价值的技能。学起来容易，掌握起来难。数据可视化是您的数据、聚合和模型结果的图形表示，总结为最终用户可访问的可视元素。这些可视化允许用户快速看到数据中的趋势、异常值和存在的模式。当你开发可视化时，你需要记住用户将如何与你的作品交互。您的可视化应该有助于用户分析数据。如果操作正确，可视化和表格可以让数据更容易被他人访问和理解。

# 绘制您的仪表板

我想与你分享的第一篇文章是由[约翰·麦卡利斯特](https://medium.com/u/4353bb6a3013?source=post_page-----956a08a54b04--------------------------------)写的，他谈到了首先绘制你的仪表盘，然后实现它们。在研究使用 Tableau 创建仪表板的示例之前，John 详细介绍了数据可视化。在他的文章中，John 提出了一个很好的观点，即为什么你应该首先用手绘制你的可视化效果。

> 花时间手绘一个可视化图需要你思考你实际上在做什么…手绘图表让常识占上风。手绘视觉效果的每一个细节都倾注了心血。

在绘图时，花些时间退后一步思考可视化的设置，可以让您思考如何最好地可视化数据。我们经常发现自己在确定制作的视觉效果是正确的之前，就在快速制作仪表盘和视觉效果。问问你自己，为什么你使用一种特定类型的可视化，比如条形图和折线图。当你处理视觉效果时，想想你希望用户如何与这些数据进行交互。使用不同的视觉效果和表格的过程对其他人来说是直观的吗，还是会让他们感到困惑？

如果你想更深入地探讨为什么这种方法有效，可以看看 John 的文章！

[](/my-tableau-dashboards-sucked-until-i-started-drawing-them-ec4c9d4a4fe8) [## 我的画面仪表盘糟透了——直到我开始画它们

### 为什么手绘仪表盘能培养更强的数据可视化技能

towardsdatascience.com](/my-tableau-dashboards-sucked-until-i-started-drawing-them-ec4c9d4a4fe8) 

# 理解科学绘图

为科学出版物、白皮书和会议开发情节可能非常耗时，以确保您以简洁的格式获得正确的信息，尤其是如果您受限于页数或海报大小。在一篇关于使用 Matplotlib 进行科学绘图的文章中，作者 [Rizky Maulana Nurhidayat](https://medium.com/u/4614e8d3f678?source=post_page-----956a08a54b04--------------------------------) 讨论了如何定制 Matplotlib 参数并创建色盲友好的可视化。

这篇文章是我希望能早点看到的，因为它详细介绍了如何为你的论文或海报设计漂亮的视觉效果。我特别喜欢这篇论文，因为他指出了通过 Matplotlib 可以获得的许多不同的绘图风格以及如何使用它们。为了更深入地了解这篇文章，Nurhidayat 讨论了 rcParams，它允许您使用 LaTeX 字体、自定义字体大小等等。他提出了每一点，并详细介绍了如何使用它们，这有助于理解如何使用每一个特性并在图中实现它。我发现这篇文章很有帮助，因为他在一个例子中遍历了 rcParams，向您展示了如何创建一个看起来相似的图。

他的文章的第二部分讨论了色盲友好的情节实现，以及如何为色盲的个人开发调色板。创建这些调色板后，您可以在 Matplolib 图和其他设计元素中使用它们。他论文的这一半给出了调色板的优秀例子，以及如何在你的可视化中使用它们。

如果你想改善你的科学情节，或者想就开发色盲友好的调色板进行一次很好的讨论，我建议你看看他的文章。

[](/matplotlib-styles-for-scientific-plotting-d023f74515b4) [## 用于科学绘图的 Matplotlib 样式

### 为您的科学数据可视化定制 Matplotlib

towardsdatascience.com](/matplotlib-styles-for-scientific-plotting-d023f74515b4) 

# 定制你的支线剧情

继续与 Matplotlib 合作制作科学图的主题， [Rizky Maulana Nurhidayat](https://medium.com/u/4614e8d3f678?source=post_page-----956a08a54b04--------------------------------) 发表了另一篇有用的文章。在本文中，Nurhidayat 讨论了如何使用多个子情节来创建更复杂的可视化效果。当试图将不同的数据联系在一起时，支线剧情会派上用场。学会在引人注目的情节中将这些数据联系起来可以帮助你通过你的视觉引导你的用户。

当需要在同一轴上将不同的数据联系在一起时，我经常创建情节和支线情节。我主要处理在确定的时间段内共享一个轴的时间序列数据，例如用三个时间序列数据集创建从当前日期开始的 90 天回顾。理解如何获取这些数据，并用一个共享轴将它们组合成不同的支线剧情是很有价值的。

像他的上一篇文章一样，Nurhidayat 很好地分解了他的过程，从一个情节开始，以不同的方式添加额外的次要情节。这篇文章分享了简单和复杂的多重支线剧情设计，以及如何在最终的可视化中设置不同大小的剧情。他使用不同的方法来添加支线剧情，这让你可以学习多种添加支线剧情的方法。

这篇文章是理解如何使用 Matplotlib 开发支线剧情的很好资源。

[](/customizing-multiple-subplots-in-matplotlib-a3e1c2e099bc) [## 在 Matplotlib 中自定义多个子情节

### 使用 subplot、add_subplot 和 GridSpec 在 Matplotlib 中创建复杂 subplot 的指南

towardsdatascience.com](/customizing-multiple-subplots-in-matplotlib-a3e1c2e099bc) 

# 最后的想法

数据可视化是数据科学的重要组成部分。这是我们通过仪表板、科学论文/海报等与他人分享我们见解的一种方式！在这三篇文章中，你将学到一些重要的经验:

*   后退一步，在做之前把你的想象画出来。这个动作会让你思考你为什么要制作视觉效果，以及用户会如何与他们互动。
*   您将在期刊、白皮书、海报等中使用的科学可视化。使用 Matplotlib 创建您的可视化风格并开发您的可视化。
*   要明白，如果你的可视化用户是色盲，他们可能很难与你的可视化用户互动。看看创建色盲友好的调色板用于您的可视化。
*   当试图将不同的数据联系在一起时，支线剧情会派上用场。学会在引人注目的情节中将这些数据联系起来可以帮助你通过你的视觉引导你的用户。

如果你想阅读更多，看看我下面的其他文章吧！

[](/top-8-skills-for-every-data-scientist-79e6b1faf3e1) [## 每位数据科学家的 8 大技能

### 当我参加大学讲座时，最常被问到的问题是“我需要具备什么技能？”

towardsdatascience.com](/top-8-skills-for-every-data-scientist-79e6b1faf3e1) [](/stop-wasting-your-time-and-consult-a-subject-matter-expert-f6ee9bffd0fe) [## 停止浪费你的时间，咨询一个主题专家

### 在从事数据科学项目时，请一位主题专家来审查您的工作可能会有所帮助。

towardsdatascience.com](/stop-wasting-your-time-and-consult-a-subject-matter-expert-f6ee9bffd0fe) [](/keys-to-success-when-adopting-a-pre-existing-data-science-project-9f1225fb0275) [## 采用现有数据科学项目的成功关键

### 代码本来可能不是你的，但现在是你的了。那么接下来呢？

towardsdatascience.com](/keys-to-success-when-adopting-a-pre-existing-data-science-project-9f1225fb0275) [](https://medium.com/the-innovation/leveraging-your-1-1-meetings-c0de2d3c8704) [## 利用您的一对一会议

### 您如何从一对一会议中获得更多价值，从而更好地了解您自己和您的工作？

medium.com](https://medium.com/the-innovation/leveraging-your-1-1-meetings-c0de2d3c8704) [](/dont-be-too-proud-to-ask-for-help-76f21d16f318) [## 不要太骄傲而不愿寻求帮助

### 如果你被一个 bug 卡住了或者感到不知所措，你可以寻求你需要的帮助。

towardsdatascience.com](/dont-be-too-proud-to-ask-for-help-76f21d16f318)