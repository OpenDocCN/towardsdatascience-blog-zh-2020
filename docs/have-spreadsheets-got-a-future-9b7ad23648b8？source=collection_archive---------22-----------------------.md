# 电子表格的时代结束了吗？

> 原文：<https://towardsdatascience.com/have-spreadsheets-got-a-future-9b7ad23648b8?source=collection_archive---------22----------------------->

## [现实世界中的数据科学](https://towardsdatascience.com/data-science-in-the-real-world/home)

## 为什么现代数据分析工具仍然难以竞争。

![](img/adb8dd5561a1a6ec8638b42e9ea35be5.png)

[图片来源](http://www.pngfind.com)

人们说我们应该抛弃像微软 Excel 和[这样的电子表格应用程序，使用更先进的数据分析工具](/jupyter-is-the-new-excel-a7a22f2fc13a)。像 Python 这样的开源编程语言对于日常用户来说是容易理解的，并且比以前的语言更容易学习。像 Jupyter Notebook 这样的工具正在彻底改变我们编写和使用代码的方式。微软的用户网站上甚至有一个帖子，要求对“ [Python 作为 Excel 脚本语言](https://excel.uservoice.com/forums/304921-excel-for-windows-desktop-application/suggestions/10549005-python-as-an-excel-scripting-language?tracking_code=83722f91dbb6e5829f30e05e925c197d)”的想法发表评论(它获得了 6300 张投票和许多评论)。

在本文中，我试图阐明使电子表格作为通用数据分析工具对如此多的用户如此流行和有效的基本特性。对我来说，它可以归结为三个属性:

1.  电子表格以数据为中心
2.  电子表格在视觉上很直观，就像一张纸
3.  电子表格为您管理计算图表

为了说明这些想法，我比较了电子表格方法和使用编程语言来完成一个简单的任务。

最后，我概述了当前电子表格应用程序和现代编程工具(如 Jupyter 笔记本)的缺点，并推测什么可能会弥合这两种范式之间的差距。

我每天都用 Python 为客户做数据分析。在那之前，我用了 20 多年的 Excel。现在我已经精通 Python 和它的[数据科学库](https://scipy.org)，我发现我比以前更有效率，能够更自信地实现更强大的分析技术。我真的只用 Excel 和客户分享数据和最终结果。

所以我发现自己在想“电子表格死了吗？”如果没有，“为什么不呢？”

我的第一反应是将电子表格扔进计算历史的垃圾桶——我花了太多时间纠结于复杂得可笑的公式，追踪损坏的单元格引用或试图恢复损坏的文件。然而，一旦我开始更深入地思考电子表格的本质，我开始意识到它具有编程语言所缺乏的固有特性。

# 电子表格是以数据为中心的

电子表格向我们展示的是数据，而不是数据背后的计算。

我们举一个很简单的例子。这是我们每个人都需要做的事情。想象一下，我们刚从公路旅行回来，我们需要弄清楚谁欠谁多少钱。

![](img/18942db195559b2c7122665f1c9cc8b3.png)

Microsoft Excel 电子表格

这个例子非常直观，不需要解释。当我们既是数据的发起者，也是最终消费者时，我们已经理解了分析的目的，并且通常我们可以通过查看数据和结果来猜测计算是如何进行的。在这种情况下，你可以说“数据不言自明”

我喜欢 Excel 的另一个特性是，你可以用鼠标选择多个单元格，并查看常见的统计数据，如总和和平均值。

然而没有公式是可见的——我们必须点击单元格才能看到它们背后的计算。没有声明变量名。我们只是将每个数据点放在不同的单元格中，按照对我们有意义的方式排列，然后用计算和单元格引用将单元格连接起来。从视图中完全看不到计算。我认为这是电子表格如此有用和如此受欢迎的一个原因；我们知道我们的数据，我们知道它代表什么，我们喜欢看到它。

相比之下，我们大多数人都不喜欢盯着电脑代码看。编程语言是基于文本和线性的。下面是 Python 中的等价旅行费用计算。

```
import pandas as pd# Trip expenses
data = {
    'Food': [38.15, 0, 109.75], 
    'Car': [139, 0, 0], 
    'Fuel': [25.08, 0, 0], 
    'Tickets': [0, 134, 0], 
    'Other': [95, 0, 250]
}
index = ['Diane', 'Kelly', 'John']
df = pd.DataFrame(data, index=index)# Calculate amount owing
df['Total Paid'] = df.sum(axis=1)
average_cost = df['Total Paid'].mean()
df['Owing'] = average_cost - df['Total Paid']
print(df)
```

这会产生:

```
 Food  Car   Fuel  Tickets  Other  Total Paid   Owing
Diane   38.15  139  25.08        0     95      297.23  -33.57
Kelly    0.00    0   0.00      134      0      134.00  129.66
John   109.75    0   0.00        0    250      359.75  -96.09
```

该代码使计算过程和计算方法显而易见，但从数据的角度来看，它更不透明。我们使用标签来引用数据，但是数据只有在打印出来后才真正可见。

中间变量包含哪些值？看到你需要一个交互式编程环境。例如，Python 有一个[解释器](https://docs.python.org/3/tutorial/interpreter.html)，它允许你检查变量并一次执行一小段代码。

如果您刚刚运行了上面的脚本，您可以使用解释器对结果进行一些分析。

```
>>> df['Total Paid'].idxmax()
'John'
>>> df.loc['John']
Food          109.75
Car             0.00
Fuel            0.00
Tickets         0.00
Other         250.00
Total Paid    359.75
Owing         -96.09
Name: John, dtype: float64
>>>
```

现代编程语言通过提供交互式环境使数据更容易访问，您可以在交互式环境中探索和可视化数据。

# 编程语言是面向对象的

如果你对编程有所了解，你可以很容易地破译代码做什么。每个数据对象都被赋予一个名称，并有一个定义的类型或结构。编程语言有大量的对象类型和方法，并允许导入外部包来提供额外的功能。

在上面的示例代码中，我使用了一个名为 [dataframe](https://pandas.pydata.org/pandas-docs/stable/getting_started/dsintro.html#dataframe) 的对象，它是专门为处理二维数据计算而设计的。

![](img/b773bdda3a915947b9942e6858f2fd5a.png)

数据帧结构

各种“适合用途”的对象结构使得计算机代码健壮、强大且计算效率高。

为了克服早期只有行和列的电子表格的一些限制，微软在 Excel 中添加了[表](https://support.office.com/en-us/article/overview-of-excel-tables-7ab0bb7d-3a9e-4b56-a3c9-6c94334e492c)和其他数据结构，以便更容易进行更复杂的数据分析——类似于计算机语言的数据结构。

这是上面的同一个电子表格，但是使用了 Excel 中内置的表格格式和[结构化引用](https://support.office.com/en-us/article/using-structured-references-with-excel-tables-f5ed2452-2337-4f71-bed3-c8ae6d2b276e)。

![](img/4b9d935efb9bfe41922f57acf71416e1.png)

table 对象具有引用其元素的特殊语法。代替

```
=SUM(C4:G4)
```

我们现在可以写:

```
=SUM(Table1[@[Food]:[Other]])
```

这更直观，如果我们对表进行更改，也不太可能破坏(符号“@”表示“来自当前行”)。

Python 中的对等词是:

```
df.loc['Diane', 'Food':'Other'].sum()
```

# 电子表格就像纸张

我认为我们喜欢电子表格的一个原因是它类似于好的旧纸张。写在纸上给了我们控制力和可预测性。东西总是在你离开的地方。我的一个老朋友半开玩笑地称纸质笔记为他的“持久、灵活的存储系统”。

![](img/fe8c92ba5f362d9ca7ce182e27014cd3.png)

图片来源:[迈克·格雷斯利 CEA 投资组合](https://sites.google.com/site/mikegresleyceaportfolio/)

尽管现在我们很少用纸做数字工作，但它有一种直觉上的吸引力。我认为这是因为我们在二维空间看世界(在将图像转换为三维空间之前)。在你面前的平面上，很容易看到所有东西的位置。即使区域太大而不能立刻查看，我们也记得我们把东西放在哪里，因为我们直观地建立了一个所有东西都在哪里的二维地图。(MS 工作簿上的选项卡提供了另一个维度，但它实际上只是一组带标签的电子表格。)

开发用户应用程序的程序员谈论“GUI”或图形用户界面。所有设计给人类使用的东西都需要一个用户界面。电子表格应用也不例外。事实上，电子表格实际上是一个 GUI。具有标准化功能的统一网格既是一个 GUI，同时也是一个构建系统，就像[乐高](https://www.lego.com)一样。

当您第一次开始处理电子表格时，它看起来相当简单，就像一张白纸。但是，一旦你学会了如何添加数据和公式，引用其他单元格并格式化它们，你就可以使用这个不可思议的多功能构建系统来创建一个难以想象的大量不同的应用程序。最终产品，一个为特定目的设计的电子表格，也可能是最终用户的图形用户界面。当然，这其中潜藏着危险，但我怀疑在绝大多数情况下，电子表格是由创建它们的同一批人使用的。开发者就是用户。所以开发环境就是用户界面是有意义的。

电子表格可以释放你的创造力，让你自由地按照我们自己的设想来构建东西。这是制作电子表格如此令人愉快的原因之一。它既是一个计算平台，也是一个用户界面，无论它是一个简单的数据输入表单，一个格式良好的报告，还是一个漂亮的图表。

相比之下，计算机代码看起来很单调——你必须阅读变量名并浏览基于文本的代码。与电子表格不同，代码脚本是一维的，即线性布局。您可以指定每个命令的精确执行顺序。语法限制了可视化个性化代码的程度。

# 数据流编程

虽然您可能没有这样想过，但 Excel 在“幕后”构建了一个计算图，以便它能够以正确的顺序执行计算(我不太确定如何执行，但我假设它使用了某种[定向图](https://en.wikipedia.org/wiki/Directed_graph))。该图和创建它的过程是自动处理的，对用户来说是模糊的。您的工作只是定义数据以及它们之间的依赖关系。

什么是计算图？[计算图](https://www.youtube.com/watch?v=hCP1vGoCdYU)是描述输入数据和输出之间的函数关系的网络图。它们被用于机器学习应用中，许多复杂的计算被串联在一起。以下是旅行费用计算的计算图表。每个方框是一个数据项，箭头代表计算。

![](img/09e60d77dde73e9aecdaa6a695bea739.png)

差旅费计算的计算图表

当你看一个电子表格时，计算图并不总是很明显，但是如果你逻辑地展示你的数据并且很好地标记它，它应该是容易想象的。Excel 中的“公式”菜单有一些很好的工具，可以通过绘制箭头来显示单元格之间的依赖关系，从而帮助使图形更加可见，但我认为它对更复杂的图形没有多大用处。

![](img/08c5892de4af0b757b9495681b4c1e99.png)

在 Excel 中可视化依赖关系

[数据流编程](https://en.wikipedia.org/wiki/Dataflow_programming)完全不同于更常见的顺序或过程编程范例。作为数据分析师，我认为我们发现将计算视为图形或数据流模型比考虑一长串计算步骤更容易。这才是 excel 真正做 Excel 的地方！

当我们构建一个电子表格时，我们可能在头脑中有一些关于我们想要的计算图的想法，但是 Excel 让我们以一种非常灵活和流畅的方式来完成它。我们不需要从头开始，稍后我们可以轻松地断开和连接图形的不同部分(当然，直到您创建一个[循环引用](https://support.office.com/en-us/article/remove-or-allow-a-circular-reference-8540bd0f-6e97-4483-bcf7-1b49cd50d123)，我确信我们都做过一两次)。

也许令人惊讶的是，当我们编写计算机代码时，没有内置的工具来自动处理计算图。这取决于你知道计算需要发生的顺序，如果你做错了，不会有任何警告。

例如，假设我们弄错了一项费用，我们需要更改它。我们可以这么做。

```
>>> df.loc['John', 'Other'] = 25  # Correct value
>>> df
         Food  Car   Fuel  Tickets  Other  Total Paid   Owing
Diane   38.15  139  25.08        0     95      297.23  -33.57
Kelly    0.00    0   0.00      134      0      134.00  129.66
John   109.75    0   0.00        0     25      359.75  -96.09
```

对一个数据项进行了更正，但没有重新计算数据框中的其余值(因为它们会自动出现在电子表格中)。

应用程序开发人员当然不会犯这种错误——他们小心翼翼地按照正确的顺序编写代码，并且[手工编写特殊检查](https://github.com/billtubbs/pythonflow-demo/blob/master/pythonflow-demo.ipynb),以确保对输入数据的任何更改都会自动触发依赖于它的所有变量的重新计算。

根据我的经验，这意味着当你为数据分析编写代码时，你需要从一开始就非常仔细地考虑计算图，并相应地计划和组织你的代码。当你写一个程序时，你实际上是根据语句的编写顺序来决定计算图的。除非你非常擅长面向对象编程，否则如果你弄错了，以后要改变对象和关系会很麻烦。我们并不都是职业程序员，为什么计算机不能为我们做呢？有一些工具会有所帮助，比如 [d6tflow](https://github.com/d6t/d6tflow) 和这个[有趣的 Python 包](https://github.com/spotify/pythonflow/)，但是这些都是专门的工具，不经常使用。

# 电子表格不可伸缩

在 Excel 中很难动态改变数据集的大小和维度。您必须手动插入或删除行和列。编程方法的最大优势在于，一旦您自动化了您的分析，就很容易对其进行扩展，并使其可配置用于各种不同规模的任务。

尽管调试可能需要更多的精力和时间，但是一旦您有了一个正常工作的计算机程序，它往往会更加健壮(不容易出现人为错误)，可能会更加高效，并且可以扩展到 Excel 无法处理的更大的数据集。

此外，还有一系列工具和基于网络的服务可供使用，允许您借用和共享代码，协作和管理更改和版本更新。

# 两全其美

最好的两个世界将是一个互动的环境，我们可以创建工作流，并看到我们的数据在一个有吸引力的和直观的视觉格式。同时，我们希望强大的数据结构通过健壮的计算图中的计算操作绑定在一起。据我所知，我们还没有到那一步，尽管有一些有趣的发展，事情进展很快。

首先，像 Jupyter Notebook 这样的开源工具确实让我们这些非专业软件开发人员的编程变得更加友好。代码编写和执行的过程不再像过去那样不连贯，所以现在我们可以在同一个视图中单步执行代码、检查变量值和试验(调试)。下面的截屏显示了如何在 Jupyter 笔记本中编辑代码时查看代码旁边的数据。

Jupyter 笔记本中的代码和数据

但是计算图呢？遗憾的是，Jupyter notebook 没有提供一种方法来可视化数据对象之间的相互联系或控制它们的依赖性。事实上，一旦你开始按照 Jupyter 笔记本允许的非线性方式执行代码，Jupyter 笔记本就有[相当多的问题。](https://docs.google.com/presentation/d/1n2RlMdmv1p25Xy5thJUhkKGvjtV-dkAIsUXP-AL4ffI/edit#slide=id.g362da58057_0_1)

也许最有希望的方向是最近开发的商业智能(BI)应用程序，它提供了一种可视化的点击式数据处理方法，具有强大的数据工作流以及强大的数据分析和可视化功能。

![](img/b441bf1d41c65d1719ee7dac7b48bec0.png)

图片来源:来自 [Tableau](/www.tableau.com) 的宣传视频

不胜枚举，我对其中几个(如 Tableau、TIBCO Spotfire、Microsoft Power BI)的经验有限，但它们都有一套全面的功能，涵盖从采集到报告和协作的整个数据流管道。许多还内置了强大的机器学习能力。

![](img/37c5cef9ba38677055a8eb5ff2f1f098.png)

图片来源: [Tableau](https://www.tableau.com/about/blog/2018/8/new-tableau-prep-use-command-line-schedule-flows-plus-localized-user-experience)

这些工具中的一些现在已经将高级编程语言集成到他们的产品中。例如， [TIBCO Spotfire 自带了一个内置的 Python 环境](https://www.tibco.com/blog/2020/01/16/how-to-get-the-most-out-of-python-in-spotfire/)，允许用户添加自己定制的 Python 代码。Tableau，有自己独特的[计算语言](https://help.tableau.com/current/pro/desktop/en-us/calculations_calculatedfields_formulas.htm)，类似 Excel 公式。

![](img/72fa600f1b4100063921503cc428cd4a.png)

图片来源: [Knime Analytics](https://www.knime.com/knime-software/knime-analytics-platform)

另一个受欢迎的平台 Looker 为他们决定[创造一种叫做 LookML](https://looker.com/blog/7-reasons-looker-built-a-new-language-for-data) 的新语言辩护，称现有的语言还没有准备好，并声称他们的语言更容易学习。我认为这是一个错误。学习任何新的编程语言都是一项巨大的投资，只有当这种语言被广泛使用并经得起时间的考验时，它才会有回报。Python、R 和 Java 等语言将在可预见的未来出现，它们可能是目前高级数据分析和机器学习的最佳语言(我认为 MATLAB 是亚军，它不是开源的，因此不太容易获得)。

我觉得这些强大的 BI 应用程序缺乏的是电子表格的简单、直观的感觉和它提供的创作自由。在使用复杂的内置功能之前，您需要与您的数据建立良好的连接，并拥有一个清晰、透明的工作环境，让您感觉可以控制，而不是不知所措。

# 结论

总的来说，我会说电子表格非常适合做标准数据分析任务的人和那些没有时间学习如何编码的人。它们让您可以快速浏览您的数据，并立即产生一些结果，而不必考虑太多。

对照表

像 Python 这样的编程语言功能强大，可以比电子表格更可靠地处理大量数据。如果你需要实现一个健壮、高效的数据处理管道，这个管道将被多次使用，并且必须被其他人所依赖，那么 Python(或者其他一些[高级编程语言](https://en.wikipedia.org/wiki/High-level_programming_language)，比如 C#、R、Java 或者 MATLAB)可能是一个不错的选择。但是如果你以前没有编程的话，会有一个很大的学习曲线。

最后，商业 BI 工具(假设你可以使用)可能是弥合电子表格和手工编码的计算机程序之间差距的解决方案。但是我还没有见过既有电子表格的直观吸引力，又能使用像 Python 这样强大的高级编程语言的。

我认为我们仍然需要弄清楚什么是完美的工作环境。我们希望解决方案能够实现以下目标:

1.  像在纸上写字一样直观
2.  为数据提供透明的接口
3.  使数据结构可见并管理计算图
4.  自动扩展以适应不同大小的数据集
5.  允许使用行业标准数据分析工具和编程语言添加定制功能。

在我们有这个之前，我不认为电子表格的时代已经结束了。

# 入门指南

如果本文中的一些概念对您来说是新的，请查看这些教程和学习资源。

*   关于在 Excel 中使用结构化表格引用的 3 分钟视频教程[https://Excel jet . net/lessons/introduction-to-structured-references](https://exceljet.net/lessons/introduction-to-structured-references)
*   解释什么是计算图形的 3 分钟视频【https://www.youtube.com/watch?v=hCP1vGoCdYU T2
*   面向完全初学者的免费 Python 编程入门书【https://greenteapress.com/wp/think-python-2e/ 
*   熊猫 Python 库的 10 分钟介绍[https://Pandas . pydata . org/Pandas-docs/stable/getting _ started/10min . html](https://pandas.pydata.org/pandas-docs/stable/getting_started/10min.html)
*   关于 Python 数据分析的综合书籍可在线获得[https://jakevdp.github.io/PythonDataScienceHandbook/](https://jakevdp.github.io/PythonDataScienceHandbook/)