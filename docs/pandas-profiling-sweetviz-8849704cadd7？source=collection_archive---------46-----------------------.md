# 熊猫简介& Sweetviz

> 原文：<https://towardsdatascience.com/pandas-profiling-sweetviz-8849704cadd7?source=collection_archive---------46----------------------->

## Python EDA 工具有助于 EDA

![](img/9da06c9329b0f6393610e2a1f45f91d9.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

D 数据清理和探索性数据分析齐头并进——通过更好地理解数据，可以更好地发现错误或异常值以进行缓解。

我们大多数人通过 pandas 函数做 EDA，再加上使用 matplotlib 到 seaborn 的可视化。有时，我们定义函数来执行 1)自动化和 2)定制的数据集 EDA(例如，在合并多个大型数据集之前对它们执行 EDA)。在为一个 pet 项目处理相关矩阵时，我偶然发现了 Sweetviz & Pandas 分析，并将它们纳入了我的 EDA 工作流。以下是一些观察结果:

# 熊猫简介

兼容性和安装:厌倦了潜在的兼容性问题，一个快速的 google/ StackOverflow 搜索浮出了 ***pandas*** 和***pandas profiling***版本之间的问题。对我有用的版本有 ***熊猫(1.0.1)*** 和 ***熊猫简介(2.4.0)*** 。如果 pandas profiling and upgrade 的缺省安装无法通过 anaconda navigator 正常工作，请尝试:

```
conda install -c conda-forge/label/cf202003 pandas-profiling
```

输出:查看输出有两种方法— 1 .jupyter 笔记本本身或 2 中的交互式小部件。HTML 格式(允许在笔记本外部查看)。HTML 格式在内容和布局上本质上是一样的。

Pandas profiling 从数据集的概述开始，然后是变量的统计信息(按字母顺序)。相关性矩阵位于相关性选项卡下。缺失值(如果有)在图中用空白标记——类似于 ***缺失号*** 包。

![](img/3d8caa523fa0455eb2ecc58db543833a.png)

熊猫简介概述(图片由作者提供)

# Sweetviz

兼容性和安装:安装可通过 pip install 获得，非常简单。它支持 Python 3.6+和 Pandas 0.25.3+，尽管对 Google Colab 的支持还不可用。

输出:Sweetviz 默认生成一个 HTML 报告。这意味着预测变量到目标变量的快速可视化。Sweetviz 输出可视为左右面板显示器。顶部的左侧面板显示顶级数据集特征(例如，数据集中的观测值数量、特征(变量)数量、副本(如果有))，后面是各个变量的深入汇总统计数据。

右侧面板显示了对变量间关系(皮尔逊相关)和值分布的更深入的分析。在此空间下可以找到按数字和分类类型对变量进行的分组(不确定 sweetviz 是否可以自动识别分类特征)。

要在变量之间导航，单击变量，会出现一个红色的边界框，将右侧面板显示“锁定”在适当的位置。再次取消选择以继续其他变量。

sweetviz 的一个有趣特性是它的汇总关联图，其中区分了分类变量和数值变量。在我的个人项目中，如果变量被正确编码，它可以作为一个快速的完整性检查。对于较大的数据集，这可能是筛选变量的快速直观帮助。

![](img/7521cd9e6eb20687bf19e19ff0c67485.png)

Sweetviz 关联图(图片由作者提供)

# 哪个对 EDA 更好？

这取决于读者的需求和个人喜好。由于这是我第二次涉足这两个库，我花了大部分时间将我自己对 EDA 的见解与这两个库的输出进行比较。就我个人而言，我仍然更喜欢通过自定义函数来完成自己的 EDA，然后使用其中一个库进行快速复查。pandas profiling 和 sweetviz 都在一定程度上自动化和加速了 EDA 过程(减少了初始安装故障排除)，并提供了对数据集特征的快速洞察。

对于初学者来说，在尝试 pandas profiling 和/或 sweetviz 之前，最好先了解一下用于 EDA 的 pandas 函数。依我看，更重要的是具备基本的统计知识来很好地解释输出，以便充分利用这些图书馆的潜力。

我希望这篇文章是一篇信息丰富的文章。下次再见，干杯。