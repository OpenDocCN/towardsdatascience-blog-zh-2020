# Python 采用了 R (Markdown)方式

> 原文：<https://towardsdatascience.com/python-done-the-r-markdown-way-d03bec4b96b?source=collection_archive---------25----------------------->

![](img/73d0cacbabd320f1198120daab3cd850.png)

照片由[维特·加布雷](https://www.data-must-flow.com/projects/photos/photos.html)拍摄

# 介绍

开始使用一种新工具可能并不容易，尤其是当那个*“工具”*意味着一种新的编程语言的时候。与此同时，可能会有机会在已知的基础上再接再厉，让过渡更平稳，痛苦更少。

对我来说，是从 **R** 到 **Python** 的过渡。不幸的是，我以前的同事 pythonians 没有分享我对 R 的真正喜爱。此外，不管 R 爱好者喜欢与否， **Python** 是数据分析/工程/科学等领域广泛使用的工具。因此，我得出结论，至少学习一些 Python 是一件合理的事情。

对我来说，最初的步骤可能是最困难的。在舒适的 RStudio 中，像 Pycharm 或 Atom 这样的 ide 并不熟悉。这一经历促使我们决定从众所周知的环境开始，并在使用 Python 时测试它的极限。

说实话，我最终并没有将 RStudio 作为在一般环境下使用 Python 的首选武器。希望下面的文字能传达为什么的信息。然而，我确信对于一些用例，比如以特别分析 R Markdown 的方式集成 R 和 Python， *RStudio* 仍然是一条可行的道路。

更重要的是，对于在 R 有**初级背景的人来说，这可能是一个方便的**起跑线**。**

那么，我发现了什么？

# 分析

# 包装和环境

首先，让我们设置环境并加载所需的包。

*   **全球环境:**

```
*# Globally round numbers at decimals*
**options**(digits=2)*# Force R to use regular numbers instead of using the e+10-like notation*
**options**(scipen = 999)
```

*   **R 库:**

```
*# Load the required packages.* 
*# If these are not available, install them first.*
ipak <- **function**(pkg){
  new.pkg <- pkg[!(pkg %in% **installed.packages**()[, "Package"])]
  **if** (**length**(new.pkg)) 
    **install.packages**(new.pkg, 
                     dependencies = TRUE)
  **sapply**(pkg, 
         require, 
         character.only = TRUE)
}packages <- **c**("tidyverse", *# Data wrangling*
              "gapminder", *# Data source*
              "knitr", *# R Markdown styling*
              "htmltools") *# .html files manipulation***ipak**(packages)tidyverse gapminder     knitr htmltools 
     TRUE      TRUE      TRUE      TRUE
```

在这个分析中，我将使用 RStudio 开发的包`reticulate` [库](https://rstudio.github.io/reticulate/)。但是，可以随意寻找[的替代品](https://www.datacamp.com/community/tutorials/using-both-python-r)。

此外，为了更清晰的流程，我将单独导入`reticulate`包。

*   **Python 激活:**

```
*# The "weapon of choice"*
**library**(reticulate)*# Specifying which version of python to use.*
**use_python**("/home/vg/anaconda3/bin/python3.7", 
           required = T) *# Locate and run Python*
```

*   **Python 库:**

```
import pandas as pd *# data wrangling*
import numpy as np *# arrays, matrices, math*
import statistics as st *# functions for calculating statistics*
import plotly.express as px *# plotting package*
```

在 **R** *(Studio)* 中使用 **Python** 的一个限制是，在某些情况下，默认情况下你不会收到 Python 回溯。这是一个问题，因为，如果我把事情过于简化，追溯可以帮助你确定问题出在哪里。意思是它能帮你解决问题。所以，把它当作一个**错误信息**(或者没有它)。

*   例如，当调用一个您没有安装的库时，R Markdown 中的 Python 块给了您绿灯(所以一切看起来都在运行)，但这并不意味着代码以您期望的方式运行(例如，它导入了一个库)。

为了解决这个问题，我建议确保你已经在`Terminal`中安装了你的库。如果没有，您可以安装它们，然后再导入。

例如，我将首先导入已经安装在我的机器上的`json`包。我将在 *RStudio* 中使用**端子**来完成此操作。另外，让我试着导入`TensorFlow` [包](https://www.tensorflow.org/)。

从下图可以看出没有包`TensorFlow`。所以，让我切换回 **bash** 并安装这个包:

然后转到新安装的包`TensorFlow`所在的目录，切换到 **Python** ，再次导入包:

好吧。有关更多信息，请查看[安装 Python 模块](https://docs.python.org/3.4/installing/index.html)页面。

# 数据

现在我们已经有了 **R** 和 **Python** 集合，让我们导入一些数据来玩玩。我将使用来自 Gapminder 的样本，这是一个关于世界人口社会人口学的有趣项目。更具体地说，我将使用 [GapMinder](https://cran.r-project.org/web/packages/gapminder/README.html) 库中最新的可用数据。

*   **R 中的数据导入:**

```
*# Let us begin with the most recent set of data*
gapminder_latest <- gapminder %>%
          **filter**(year == **max**(year))
```

所以，现在我们在 **R** 中加载了一个数据。不幸的是，无法通过 **Python** 直接访问 **R 对象**(例如*向量*od*tibles*)。所以，我们需要先转换 R 对象。

*   **Python 中的数据导入:**

```
*# Convert R Data Frame (tibble) into the pandas Data Frame*
gapminder_latest_py = r['gapminder_latest']
```

当使用 *Python 对象*(例如*数组*或`pandas` *数据帧*)时，要意识到的一件重要事情是，它们是**而不是** [显式地**存储在环境**](https://community.rstudio.com/t/rstudio-ide-global-environment-variable-when-running-python/17999)中的，就像 *R 对象*一样。

*   换句话说，如果我们想知道工作区中存储了什么，我们必须调用像`dir()`、`globals()`或`locals()`这样的函数:

```
['R', '__annotations__', '__builtins__', '__doc__', '__loader__', '__name__', '__package__', '__spec__', 'contact_window_days', 'contact_window_days_style', 'fig', 'gapminder_latest_count_py', 'gapminder_latest_max_lifeExp_py', 'gapminder_latest_mean_lifeExp_py', 'gapminder_latest_median_lifeExp_py', 'gapminder_latest_min_lifeExp_py', 'gapminder_latest_py', 'gapminder_latest_shape', 'gapminder_latest_stdev_lifeExp_py', 'lifeExpHist', 'np', 'pd', 'px', 'r', 'st', 'sys', 'variable', 'variable_grouping', 'variable_name']
```

太好了，在现在的对象中，我们可以清楚地看到数据(`gapminder_latest_py`)或库(如`px`)。

那么，让我们来探究一下这些数据吧！

# 预期寿命

出于演示的目的，我将把重点放在预期寿命(T21)或一个人预期的平均寿命上。

# 描述统计学

让我们从使用 **Python** 计算一些**描述性统计数据**开始，如*表示*、*中位数*或*数据中的行数*:

```
*# Descriptive statistics for the inline code in Python**## Data Frame Overview**### Number of rows*
gapminder_latest_shape = gapminder_latest_py.shape[0] 
*### Number of distinct values within the life expectancy variable*
gapminder_latest_count_py = gapminder_latest_py['lifeExp'].nunique()*## Life Expectancy**### Median (Life Expectancy)*
gapminder_latest_median_lifeExp_py = st.median(gapminder_latest_py['lifeExp']) 
*### Mean*
gapminder_latest_mean_lifeExp_py = st.mean(gapminder_latest_py['lifeExp'])
*### Minimum*
gapminder_latest_min_lifeExp_py = min(gapminder_latest_py['lifeExp']) 
*### Maximum*
gapminder_latest_max_lifeExp_py = max(gapminder_latest_py['lifeExp'])
*### Standard deviation*
gapminder_latest_stdev_lifeExp_py = st.stdev(gapminder_latest_py['lifeExp'])
```

很好。不幸的是，我们无法使用 **Python 对象**进行**内联编码**，这是[](https://en.wikipedia.org/wiki/Literate_programming#:~:text=Literate%20programming%20is%20a%20programming,source%20code%20can%20be%20generated.)**中 [*R Markdown*](https://rmarkdown.rstudio.com/lesson-4.html) 的关键特性之一。因此，如果我们想要将结果用于内联代码，我们需要将 **Python 对象**转换回 **R** :**

```
*# Descriptive statistics for the inline code in Python - transformed to R**## Data Frame Overview**## Number of rows*
gapminder_latest_nrow_r = py$gapminder_latest_shape
*### Number of distinct values within the life expectancy variable*
gapminder_latest_count_r = py$gapminder_latest_count_py*## Life Expectancy**### Median (Life Expectancy)*
gapminder_latest_median_lifeExp_r = py$gapminder_latest_median_lifeExp_py
*### Mean*
gapminder_latest_mean_lifeExp_r = py$gapminder_latest_mean_lifeExp_py
*### Minimum*
gapminder_latest_min_lifeExp_r = py$gapminder_latest_min_lifeExp_py
*### Maximum*
gapminder_latest_max_lifeExp_r = py$gapminder_latest_max_lifeExp_py
*### Standard deviation*
gapminder_latest_stdev_lifeExp_r = py$gapminder_latest_stdev_lifeExp_py
```

**那么，关于 **2007** 中的**寿命**，我们能说些什么呢？**

**首先，有 **142 个国家**上榜。预期寿命的**最小值**为 **39.61** 岁，最大值 **82.6** 岁。**

**平均预期寿命值为 67.01 岁，平均预期寿命中位数为 71.94 岁或更长。最后，标准差是 **12.07** 年。**

# **图表(使用 Plotly)**

**好吧，我们换个话题，比如图表。**

**例如，我们可以使用`Plotly`来看看*全球的预期寿命是如何分布的*:**

```
fig = px.histogram(gapminder_latest_py, *# package.function; Data Frame*
                   x="lifeExp", *# Variable on the X axis*
                   range_x=(gapminder_latest_min_lifeExp_py, 
                            gapminder_latest_max_lifeExp_py), *# Minimum and maximum values for the X axis*
                   labels={'lifeExp':'Life expectancy - in years'}, *# Naming of the interactive part*
                   color_discrete_sequence=['#005C4E']) *# Colour of fill* lifeExpHist = fig.update_layout(
  title="Figure 1\. Life Expectancy in 2007 Across the Globe - in Years", *# The name of the graph*
  xaxis_title="Years", *# X-axis title*
  yaxis_title="Count", *# Y-axis title*
  font=dict( *# "css"*
    family="Roboto",
    size=12,
    color="#252A31"
  ))lifeExpHist.write_html("lifeExpHist.html") *# Save the graph as a .html object*
```

**遗憾的是，无法通过 **Python** 在 **R Markdown** 中打印交互式`Plotly`图形。或者，更准确地说，你将通过打印(如`print(lifeExpHist)`)得到一个`Figure object`:**

```
Figure({
    'data': [{'alignmentgroup': 'True',
              'bingroup': 'x',
              'hovertemplate': 'Life expectancy - in years=%{x}<br>count=%{y}<extra></extra>',
              'legendgroup': '',
              'marker': {'color': '#005C4E'},
              'name': '',
              'offsetgroup': '',
              'orientation': 'v',
              'showlegend': False,
              'type': 'histogram',
              'x': array([43, 76, 72, 42, 75, 81, 79, 75, 64, 79, 56, 65, 74, 50, 72, 73, 52, 49,
                          59, 50, 80, 44, 50, 78, 72, 72, 65, 46, 55, 78, 48, 75, 78, 76, 78, 54,
                          72, 74, 71, 71, 51, 58, 52, 79, 80, 56, 59, 79, 60, 79, 70, 56, 46, 60,
                          70, 82, 73, 81, 64, 70, 70, 59, 78, 80, 80, 72, 82, 72, 54, 67, 78, 77,
                          71, 42, 45, 73, 59, 48, 74, 54, 64, 72, 76, 66, 74, 71, 42, 62, 52, 63,
                          79, 80, 72, 56, 46, 80, 75, 65, 75, 71, 71, 71, 75, 78, 78, 76, 72, 46,
                          65, 72, 63, 74, 42, 79, 74, 77, 48, 49, 80, 72, 58, 39, 80, 81, 74, 78,
                          52, 70, 58, 69, 73, 71, 51, 79, 78, 76, 73, 74, 73, 62, 42, 43]),
              'xaxis': 'x',
              'yaxis': 'y'}],
    'layout': {'barmode': 'relative',
               'font': {'color': '#252A31', 'family': 'Roboto', 'size': 12},
               'legend': {'tracegroupgap': 0},
               'margin': {'t': 60},
               'template': '...',
               'title': {'text': 'Figure 1\. Life Expectancy in 2007 Across the Globe - in Years'},
               'xaxis': {'anchor': 'y', 'domain': [0.0, 1.0], 'range': [39.613, 82.603], 'title': {'text': 'Years'}},
               'yaxis': {'anchor': 'x', 'domain': [0.0, 1.0], 'title': {'text': 'Count'}}}
})
```

**所以，我们**导入**先前创建的*。html* 文件(例如使用`htmltools`包中的`includeHTML`功能):**

```
htmltools::**includeHTML**("lifeExpHist.html") *# Render the graph*
```

**所以，这里有一个基本的，然而是用 Python 版本的`Plotly`制作的[交互式直方图](https://www.data-must-flow.com/posts/python-in-r/lifeexphist)。**

**然而，在 **RStudio** 中生成这个图表需要一种变通方法。同时，以这种方式产生的图形的大小可以很容易地达到几十兆字节。**

*   **因此，包含这些图表的. html 报告需要**大量数据来下载**给读者，并且需要**更多时间来呈现页面**。**

# **汇总表(使用熊猫)**

**`pandas`的一个常见用例是提供数据描述。各自的代码运行良好。但是，由于您在`pandas`中使用了样式，因此无法对输出进行样式化。**

*   **说到`pandas`，我们可以很容易地为常用的统计数据创建一个汇总表，如**个大洲**的**预期寿命**的平均值或标准差:**

```
*# Create a pandas Data Frame object containing the relevant variable,*
*# conduct formatting.*

  gapminder_latest_py['continent'] = gapminder_latest_py['continent'].astype(str)
  variable = 'continent'
  variable_name = 'Continents'

  gapminder_latest_py['lifeExp'] = gapminder_latest_py['lifeExp'].astype(int)
  variable_grouping = 'lifeExp'

  contact_window_days = gapminder_latest_py.groupby([
                          pd.Grouper(key=variable)])\
                          [variable_grouping]\
                          .agg(['count',
                                'min',
                                'mean',
                                'median',
                                'std',
                                'max'])\
                          .reset_index()

  contact_window_days_style = contact_window_days\
                          .rename({'count': 'Count',
                                   'median': 'Median',
                                   'std': 'Standard Deviation',
                                   'min': 'Minimum', 
                                   'max': 'Maximum',
                                   'mean': 'Mean',}, axis='columns')
```

**输出:**

```
continent  Count  Minimum       Mean  Median  Standard Deviation  Maximum
  0    Africa     52       39  54.326923    52.0            9.644100       76
  1  Americas     25       60  73.040000    72.0            4.495183       80
  2      Asia     33       43  70.151515    72.0            7.984834       82
  3    Europe     30       71  77.100000    78.0            2.916658       81
  4   Oceania      2       80  80.500000    80.5            0.707107       81
```

**另外，请注意 **Python 环境设置覆盖了 R** 中的设置。例如，看看均值或标准差的位数。有六个数字，而不是开头的两个数字。**

# **结束语**

**好了，现在够了。如果你渴望更高级的东西，比如使用 **R** 中的`scikit-learn` [包](https://scikit-learn.org/stable/)进行无监督学习，看看[的](https://www.r-bloggers.com/r-and-python-using-reticulate-to-get-the-best-of-both-worlds/)。**

**然而，在结束这篇文章之前，让我说一下，如果你考虑切换到 **Python** 并经常使用它，考虑一下 IDE 替代 *RStudio* 。**

**许多分析师相信 [**Jupyter 笔记本**](https://jupyter.org/) 的交互性，集成`markdown`或选择运行各种语言的代码，如`R`、`Julia`或`JavaScript`。 [**JupyterHub**](https://jupyter.org/hub) 是基于 Jupyter 笔记本的平台，增加了版本控制。通常，用户在集装箱环境中运行分析)。另一个关于交互性和协作的例子可能是 [**Colab**](https://colab.research.google.com/notebooks/intro.ipynb#recent=true) ，基本上就是运行在谷歌云上的 Jupyter 笔记本。**

**最后，有一个伟大的软件叫做 [**Visual Studio 代码**](https://code.visualstudio.com/) 。它不仅允许你用多种语言创建和运行代码，或者在纯 Python 代码和交互式 Jupyter 笔记本之间无缝流动。也许更重要的是，它为您提供了非常有效的版本控制管理(比如 Git 集成和扩展)。如果选择这个 IDE，可以像 RStudio 一样设置 [VS 代码进行 Python 开发。](https://stevenmortimer.com/setting-up-vs-code-for-python-development-like-rstudio/#settings-json-file)**

**但是不管你选择什么样的 Python 之路，不要忘记它是一个适合某些情况的工具，而对其他人可能就不那么适合了。就像 r 一样，在了解不同工具的优点的同时，尽量利用它的优点。**