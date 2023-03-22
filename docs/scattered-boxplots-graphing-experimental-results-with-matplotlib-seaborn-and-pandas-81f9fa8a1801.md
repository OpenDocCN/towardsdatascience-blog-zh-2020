# 散点图:用 matplotlib、seaborn 和 pandas 绘制实验结果

> 原文：<https://towardsdatascience.com/scattered-boxplots-graphing-experimental-results-with-matplotlib-seaborn-and-pandas-81f9fa8a1801?source=collection_archive---------9----------------------->

## 利用 python 可视化库绘制散点箱线图

![](img/c99bb6420c8a2c5548de3d777992ff85.png)

Ciaran 库尼使用 Matplotlib 绘制。

Matplotlib、Seaborn 和 Pandas 的联合力量为数据科学家和工程师提供了丰富的资源，用于数据可视化和结果呈现。然而，对于初学者来说，将可用的工具操作成他们想象的美丽图形并不总是容易的。我在自己的工作(脑机接口博士学位)中发现，我获取的数据并不总是以直接适用于使用某些功能的方式进行组织，定制可能很困难且耗时。此外，选择最佳类型的图表来显示您的结果可能需要深思熟虑，并且经常需要反复试验。

散点图是一种非常有效的交流结果的方式，既能吸引观众的眼球，又能给观众提供信息。箱线图显示了结果的分布，显示了中间值、四分位距和其他与数据的偏斜度和对称性相关的因素。你可以在这里找到了解 boxplots 的有用教程:[https://towards data science . com/understanding-box plots-5e 2 df 7 bcbd 51](/understanding-boxplots-5e2df7bcbd51)。

使用散点图将特定的数据点叠加在箱线图上，是以一种吸引人的方式描绘结果的一种极好的方式，这种方式可以使读者很容易理解所呈现的内容。在这篇文章的剩余部分，我将逐步完成创建散点图的过程，提出一些定制的想法，并展示如何同时绘制多个图形。

# Scatted box plots

在导入所需的 Python 包之后，我创建了一个小型数据集，表示可能从包含结果(例如，使用不同算法的分类结果)的 excel 文件中读入的数据类型。这里，我有四列数据，每一列包含从 65 到 95 的二十个值(类似于分类精度)。每列还有一个基本标题。

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as snsdataset = np.random.default_rng().uniform(60,95,(20,4))
df = pd.DataFrame(dataset, columns=['data1','data2','data3','data4'])
df.head()
```

![](img/4e6ae4966abfd2bca9b68c59501b18d0.png)

excel 格式的随机样本数据。

加载数据后，我通常喜欢做的一件事是将 dataframe 列重命名为更具描述性的名称，以便在绘制数据时使用。在这里，我修改了标题以表示实验编号。

```
for n in range(1,df.columns.shape[0]+1):
    df.rename(columns={f"data{n}": f"Experiment {n}"}, inplace=True)
df.head()
```

![](img/50f8f474dac196ed8870751fe19410bc.png)

带有编辑过的列名的示例数据。

编辑完列名后，生成一个初始的散点图就非常容易了。首先，我为结果(val)、要绘制的数据名称(names)和要添加到散点图数据点的抖动(xs)创建列表变量。**注意:抖动被添加到数值中，以提供将覆盖在箱线图顶部的数据点的分隔。然后，我使用 for 循环来遍历数据帧中不同数据列的范围，以组织绘图所需的数据。**

```
vals, names, xs = [],[],[]for i, col in enumerate(df.columns):
    vals.append(df[col].values)
    names.append(col)
    xs.append(np.random.normal(i + 1, 0.04, df[col].values.shape[0]))  # adds jitter to the data points - can be adjusted
```

使用这些数据容器来绘制初始的散点图是非常简单的。matplotlib *boxplot* 函数只需要上面收集的 val 和 names 数据。散点图稍微复杂一点，但是只需要一个带有 python **zip** 关键字的 for 循环来遍历抖动值、数据点和调色板。

```
plt.boxplot(vals, labels=names)palette = ['r', 'g', 'b', 'y']for x, val, c in zip(xs, vals, palette):
    plt.scatter(x, val, alpha=0.4, color=c)plt.show()
```

只需几行简短的 python 代码，就可以生成一个描述结果分布的散点图:

![](img/ba37c99ee2f0ee697d39cdc0f5fff02a.png)

最简风格的箱线图(来源:恰兰·库尼使用 Matplotlib)。

然而，这并不是一个非常漂亮的图形，所以我通常会尝试做一些事情来定制它，使它更有吸引力，希望更具描述性。我相信你们很多人都知道，seaborn 提供了一些主题，可以用来概括你的情节风格。目前，我更喜欢“白色网格”——但这种情况经常改变。此外，boxplot 函数接受多个可自定义的属性参数，以帮助您完善演示文稿。我在下面插入了一些选项，包括调整框的颜色、线条的粗细和中值标记的样式。

我要提醒你的是，分散的箱线图可能会做得过火，所以不要让图表过多，也不要使用太多冲突的颜色。

```
##### Set style options here #####
sns.set_style("whitegrid")  # "white","dark","darkgrid","ticks"boxprops = dict(linestyle='-', linewidth=1.5, color='#00145A')
flierprops = dict(marker='o', markersize=1,
                  linestyle='none')
whiskerprops = dict(color='#00145A')
capprops = dict(color='#00145A')
medianprops = dict(linewidth=1.5, linestyle='-', color='#01FBEE')
```

用吸引人的配色方案定制您的图是以引人入胜的方式呈现结果的一个非常重要的方面。你可以在上面的代码片段中看到，我使用十六进制颜色代码来定制属性。虽然 Matplotlib 提供了颜色选项，但我最近开始使用网站[https://htmlcolorcodes.com/](https://htmlcolorcodes.com/)来高精度地选择我想要的颜色。在这里，我选择了 4 个十六进制颜色代码用于散点图点。

```
palette = ['#FF2709', '#09FF10', '#0030D7', '#FA70B5']plt.boxplot(vals, labels=names, notch=False, boxprops=boxprops, whiskerprops=whiskerprops,capprops=capprops, flierprops=flierprops, medianprops=medianprops,showmeans=False) 
```

![](img/561ae5701016f125a1e581e92365280a.png)

带有附加样式的箱线图(来源:恰兰·库尼使用 Matplotlib)。

现在我可以给剧情添加一些可选的特性(**注:**这只是一个演示。在实践中，尽量避免过度填充图表，因为这会降低可读性)。让我们添加一条对应于 y 轴上某点的水平线。这种东西可以用来表示某个阈值或者某个模型的概率分类精度。

```
plt.xlabel("Categorical", fontweight='normal', fontsize=14)
plt.ylabel("Numerical", fontweight='normal', fontsize=14)sns.despine(bottom=True) # removes right and top axis lines
plt.axhline(y=65, color='#ff3300', linestyle='--', linewidth=1, label='Threshold Value')
plt.legend(bbox_to_anchor=(0.31, 1.06), loc=2, borderaxespad=0., framealpha=1, facecolor ='white', frameon=True)
```

![](img/4f531de389e2585de04373df6375a30d.png)

增加了 x/y 标签、消旋和图例(来源:Ciaran 库尼，使用 Matplotlib)。

现在你有了它，一个快速简单的方法来产生一个分散的箱线图，它可以帮助你的结果向观众展示。下面我将这个功能扩展到同时绘制多个箱线图。

# 正在策划

有些情况下，单个箱线图不足以传达你想要呈现的结果。也许一个实验有多个条件，或者在几个不同的数据集上评估了几个独立的机器学习分类器。在这种情况下，我们可以使用 matplotlib 的子绘图函数来生成理想的图形。

事实上，实现这一点所需的大部分代码只是我们上面实现的代码的简单复制，但是完成这些步骤仍然是有用的。第一件事是创建第二个数据集。这里，我有效地使用了和以前一样的代码，但是我调整了数据点的范围，这样我们可以在两个图中看到这个效果。

```
dataset = np.random.default_rng().uniform(50,86,(20,4))
df_1 = pd.DataFrame(dataset, columns=['data1','data2','data3','data4'])
df_1.head()
```

然后，只需像以前一样完成这些步骤，将数据处理成我们需要的格式。唯一的区别是，现在我们是针对两个数据集进行的。

```
for n in range(1,df.columns.shape[0]+1):
    df.rename(columns={f"data{n}": f"Experiment {n}"}, inplace=True)
    df_1.rename(columns={f"data{n}": f"Experiment {n}"}, inplace=True)namesA, valsA, xsA = [], [], []
namesB, valsB, xsB = [], [], []for i, col in enumerate(df.columns):
    valsA.append(df[col].values)
    namesA.append(col)
    xsA.append(np.random.normal(i + 1, 0.04, df[col].values.shape[0]))for i, col in enumerate(df_1.columns):
    valsB.append(df_1[col].values)
    namesB.append(col)
    xsB.append(np.random.normal(i + 1, 0.04, df_1[col].values.shape[0]))
```

有了形状中的数据，我们就可以用两个轴对象创建一个 matplotlib 子图对象。接下来，我们分别在 **ax1** 和 **ax2** 上绘制来自两个数据集的方框和数据点。

```
fig, (ax1, ax2) = plt.subplots(nrows=2, ncols=1, figsize=(5, 5))bplot1 = ax1.boxplot(valsA, labels=namesA, notch=False,     showmeans=False)bplot2 = ax2.boxplot(valsB, labels=namesB, notch=False, 
            showmeans=False)palette = ['#33FF3B', '#3379FF', '#FFD633', '#33FFF1']for xA, xB, valA, valB, c in zip(xsA, xsB, valsA, valsB, palette):
    ax1.scatter(xA, valA, alpha=0.4, color=c)
    ax2.scatter(xB, valB, alpha=0.4, color=c)
```

![](img/e3d47ac39e1302e75f827184e1d3a3bc.png)

多个分散的箱线图同时绘制(来源:Ciaran 库尼使用 Matplotlib)。

你可以看到数据已经被正确地绘制出来，稍加努力，你甚至可以看出两者之间的差异。当然，如果我们试图描述两个图之间数据的一些重要方面，添加一点颜色来区分这两个图会很有帮助。下面，我修改了箱线图属性，加入了独特的蓝色和红色方案，以帮助区分数据。

```
boxpropsA = dict(linestyle='-', linewidth=1, color='#33B3FF')
flierpropsA = dict(marker='o', markersize=10,
                  linestyle='none', markeredgecolor='g')
whiskerpropsA = dict(color='#33B3FF')
cappropsA = dict(color='#33B3FF')
medianpropsA = dict(linewidth=1, linestyle='-', color='#33B3FF')  # colors median lineboxpropsB = dict(linestyle='-', linewidth=1, color='#FF4533')
flierpropsB = dict(marker='o', markersize=10, linestyle='none', markeredgecolor='g')
whiskerpropsB = dict(color='#FF4533')
cappropsB = dict(color='#FF4533')
medianpropsB = dict(linewidth=1, linestyle='-', color='#FF4533')  # colors median line
```

![](img/73db043ffccd70ae4d6ecb5ad0f85150.png)

用颜色区分你的数据(来源:Ciaran 库尼使用 Matplotlib)。

最后，让我们看一两个小的补充，使图表更可读，更有吸引力。和以前一样，我将样式设置为 seaborn whitegrid。然后我使用 for 循环来设置两个支线剧情的某些属性。其中之一是移除内部 x 标签，只留下外部标签。我还设置了一个常见的 y 轴范围和一条指示某个阈值的水平线以及一个 y 轴标签。

```
sns.set_style("whitegrid")
for ax in fig.get_axes():
    ax.label_outer()
    sns.despine(ax=ax)
    ax.set_ylim(50, 100)
    ax.axhline(y=65, color='#ff3300', linestyle='--', linewidth=1,      label='Threshold')fig.text(0.04, 0.5, 'Classification accuracy (%)', ha='center', va='center', rotation='vertical', fontsize=12)
```

![](img/f48868f0fd51e45858c2f6f04e7d8337.png)

双图散点图的最终版本(来源:希兰·库尼使用 Matplotlib)。

你可以看到这些增加的效果。特别是，y 轴界限和水平线的结合使绘制的数据分布存在显著差异变得非常明显。如果你将它与第一个双盒图进行比较，你会发现现在已经做出了这些改变，推断信息要容易得多。

用于编写这篇文章的代码可在此处获得:[https://github . com/cfcooney/medium _ posts/blob/master/scattered _ box plots . ipynb](https://github.com/cfcooney/medium_posts/blob/master/scattered_boxplots.ipynb)

我希望它能帮助你们中的一些人更好地展示你们的成果。