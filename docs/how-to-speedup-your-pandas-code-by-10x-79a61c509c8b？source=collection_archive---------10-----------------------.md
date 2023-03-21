# 如何将你的熊猫代码加速 10 倍

> 原文：<https://towardsdatascience.com/how-to-speedup-your-pandas-code-by-10x-79a61c509c8b?source=collection_archive---------10----------------------->

## 骗了你！诀窍是离开熊猫

![](img/2cfcfac076751dc62b17aad0b36ac3c1.png)

图片来自 JESHOOTS at[https://www . pexels . com/photo/fuzzy-motion-of-illuminated-railway-station-in-city-253647/](https://www.pexels.com/photo/blurred-motion-of-illuminated-railroad-station-in-city-253647/)

众所周知，熊猫是一个神奇的数据科学工具。它为我们提供了我们需要的数据框架结构、强大的计算能力，并且以非常用户友好的格式做到了这一点。它甚至有高质量的文档和一个庞大的支持网络，使它易于学习。太棒了。

但并不总是特别快。

当涉及很多很多计算时，这可能会成为一个问题。如果处理方法很慢，则运行程序需要更长的时间。如果需要数百万次计算，并且总计算时间不断延长，那就非常麻烦了。

这是我工作中常见的问题。我工作的一大重点是开发代表建筑中常见设备的模拟模型。这意味着我在一台设备中创建模拟热传递过程和控制逻辑决策的功能，然后将描述建筑条件和居住者行为选择的数据传递到这些模型中。然后，模型预测设备将做什么，它将如何满足居住者的需求，以及它将消耗多少能量。

为了做到这一点，模型需要基于时间。它需要能够计算模拟中某一点发生了什么，然后才能进行下一组计算。这是因为一次的输出是下一次的输入。例如，想象一下预测你的烤箱在任何时间点的温度。目前正在加热吗？自上次讨论以来，温度上升了多少？当时的温度是多少？

这种对上一次的依赖导致了一个问题。我们不能使用向量计算。我们必须使用可怕的 for 循环。For 循环速度很慢。

## 我们能做些什么让事情进展得更快一点？

无论矢量化计算是否可行，一个解决方案是将计算转换为 numpy。根据 [Sofia Heisler 在 Upside Engineering 博客](https://engineering.upside.com/a-beginners-guide-to-optimizing-pandas-code-for-speed-c09ef2c6a4d6)上的说法，numpy 使用预编译的 C 代码执行大量的后台信息。这种预编译的 C 代码通过跳过编译步骤和包含预编程的速度优化，使它比 pandas 快得多。此外，numpy 删除了许多关于熊猫的信息。Pandas 跟踪数据类型、索引，并执行错误检查。所有这些都非常有用，但在这个时候没有必要，而且会减慢计算速度。Numpy 不会这样做，并且可以更快地执行相同的计算。

有多种方法可以将 panads 数据转换为 numpy。

可以使用。价值方法。这将在 numpy 中创建相同的系列。对于一个简单的示例，请参见下面的代码:

```
import pandas as pd
Series_Pandas = pd.Series(data=[1, 2, 3, 4, 5, 6])
Series_Numpy = Series_Pandas.values
```

数据帧可以使用。to_numpy()函数。这将在 numpy 中创建一个具有相同值的 int64 对象。注意，这不会保留列名，您需要创建一个字典，将 pandas 列名转换为 numpy 列号。这可以通过下面的代码来实现:

```
import pandas as pd
import numpy as npDataframe_Pandas = pd.DataFrame(data=[[0,1], [2,3], [4,5]], columns = ['First Column', 'Second Column'])
Dataframe_Numpy = Dataframe_Pandas.to_numpy()
Column_Index_Dictionary = dict(zip(Dataframe_Pandas.columns, list(range(0,len(Dataframe_Pandas.columns)))))
```

该代码将 dataframe 转换为 numpy int64 对象，并提供了遍历每一行所需的所有工具，以用户友好的方式编辑特定列中的值。每个单元都可以用类似熊猫的方式来调用。具有 numpy 索引的 loc 函数，遵循结构 *int64object[row，Dictionary['Pandas 列名']]* 。例如，如果要将“第二列”的第一行中的值设置为“9”，可以使用以下代码:

```
Dataframe_Numpy[0, Column_Index_Dictionary['Second Column']] = 9
```

## 这会让我的代码快多少？

当然，这将因情况而异。通过切换到 numpy，一些脚本会比其他脚本有更多的改进。这取决于脚本中使用的计算类型以及转换为 numpy 的所有计算的百分比。但是结果可能是激烈的。

例如，我最近用它将我的一个模拟模型从熊猫基地转换为 numpy 基地。最初，基于熊猫的模型需要 362 秒来进行年度模拟。超过 6 分钟了。如果你用模型运行一个模拟，这并不可怕，但是如果你运行一千个呢？在将模型的核心转换为 numpy 之后，同样的年度模拟需要 32 秒来计算。

做同样的事情要多花 9%的时间。将我的代码从 pandas 转换到 numpy 的速度比用户预先存在的函数快了 10 倍以上。

## 包装它

Numpy 拥有熊猫的所有计算能力，但执行它们时没有携带太多的开销信息，并且使用预编译、优化的方法。因此，它可以明显快于熊猫。

将数据帧从 pandas 转换为 numpy 相对简单。您可以使用数据框。to_numpy()函数来自动转换它，然后创建一个列名字典，以便像访问熊猫一样访问每个单元格。锁定功能。

这个简单的改变可以产生显著的效果。使用 numpy 的脚本执行时间大约是使用 pandas 所需时间的 10%。