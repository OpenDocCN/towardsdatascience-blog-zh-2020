# 数据探索，简化。

> 原文：<https://towardsdatascience.com/data-exploration-simplified-2c045a495fe4?source=collection_archive---------49----------------------->

## 厌倦了在互联网上查找数百个方法和函数来探索您的数据集吗？Xplore 将探索性数据分析过程的速度提高了 10 倍，所有这一切都只需要一行代码。

![](img/b239d5996ab943409a74c7bb440eba34.png)

[**xplore**](https://pypi.org/project/xplore/) logo 设计由**[**Divine alor vor**](https://www.linkedin.com/in/divine-kofi-alorvor-86775117b/)**

# **什么是数据探索？**

**根据[维基百科](https://en.wikipedia.org/wiki/Data_exploration)的定义，数据探索是一种类似于初始数据分析的方法，数据分析师通过视觉探索来了解数据集中的内容和数据的特征，而不是通过传统的数据管理系统。**

**正是在数据分析过程的这个阶段，数据科学家或分析师、人工智能或人工智能工程师试图真正理解他们正在处理的数据。在这一点上，他们试图熟悉他们正在处理的数据，以便能够知道它在解决手头的问题时有多有效，以及他们还需要做多少处理。在这个阶段，数据分析师或科学家、人工智能或 ML 工程师使用如此多的工具和库来有效地探索他们的数据集。这使得数据探索的过程很难完全完成，特别是如果你不知道许多必要的工具，库和方法来很好地探索你的数据集。有一个好消息，想象一下，只需一行代码就能高效快速地浏览数据集！**

# **探险**

**考虑到与有效探索您的数据相关的这么多问题，我的队友、[**Benjamin acqua ah**](https://www.linkedin.com/in/benjamin-acquaah-9294aa14b/)、 [**Adam Labaran**](https://www.linkedin.com/in/adam-labaran-111358181/) 和我自己编写了一些自动化脚本，可以极大地自动化和简化数据探索过程。我们使用 Pandas 开源库编写了这个脚本，利用了该库附带的许多方法和函数。在修复了几个错误、优化了代码并运行了一系列测试之后，我们最终构建并打包了 [**xplore**](https://github.com/buabaj/xplore) **🎉。****

**[**xplore**](https://github.com/buabaj/xplore) 是为数据科学家或分析师、AI 或 ML 工程师构建的 python 包，用于在单行代码中探索数据集的特征，以便在数据争论和特征提取之前进行快速分析。 [**xplore**](https://github.com/buabaj/xplore) 还利用了 pandas-profiling 的全部功能，如果用户需要，可以生成非常高级和详细的数据探索报告。**

**[**xplore**](https://github.com/buabaj/xplore) 完全开源，感兴趣的贡献者或代码爱好者可以在**[**Github**](https://github.com/buabaj/xplore)上找到完整的源代码和测试文件。该项目也已经发布在 PyPi 上，因此任何感兴趣的人都可以轻松地将它安装并导入到他们的本地项目中。****

****![](img/18092f170c04389bf1cab5047c5e9667.png)****

****[**xplore**](https://pypi.org/project/xplore/) logo 由**[**Divine alor vor**](https://www.linkedin.com/in/divine-kofi-alorvor-86775117b/)设计******

# ******如何使用 xplore 浏览您的数据******

******在本节中，我将通过一个演练教程来指导您如何在您的本地项目中安装、导入和使用 [**xplore**](https://github.com/buabaj/xplore) 包。******

## ******安装 xplore 包******

******使用 xplore 包成功浏览数据的最低要求是在计算机上安装 Python。一个额外的好处是可以在您的计算机上安装 Anaconda 并添加到 PATH 中。完成这些工作后，您可以轻松地导航到您的命令提示符(Windows)、终端(Linux/macOS)或 anaconda 提示符并运行命令:******

```
****pip install xplore****
```

******通过运行这个命令，最新版本的 [**xplore**](https://github.com/buabaj/xplore) 及其依赖项将会完全存储在您的本地计算机上，这样您就可以方便地在以后的 python 文件中导入和使用它。******

## ******将 xplore 模块导入到代码中******

******在任何想要使用 [**xplore**](https://github.com/buabaj/xplore) 包探索数据的 python 文件中，您都必须将 xplore 模块直接导入到代码文件中，这样您就可以轻松地访问和使用该模块附带的内置方法。这可以通过在代码中进行必要的导入时添加以下行来实现:******

```
****from xplore.data import xplore****
```

## ******为探索准备数据******

******在这篇文章发表的时候， [**xplore**](https://github.com/buabaj/xplore) 包已经过优化，只能处理带标签的数据。使用 [**xplore**](https://github.com/buabaj/xplore) 包准备您的数据，就像将您读入代码的数据赋给任何变量名一样简单。******

```
****import pandas as pddata = pd.read_csv('name_of_data_file.csv')****
```

## ******使用 xplore 方法******

******实际探索数据的过程被简化为一行代码。其他要做的事情将由我们编写的自动化脚本自动完成。您可以使用以下命令浏览您的数据:******

```
****xplore(data)****
```

******通过运行该程序，您将看到从数据探索中得出的分析输出。几乎所有您需要的重要分析都将被打印出来，但是对于那些希望从他们的数据分析中看到非常详细和高级的报告的人，当提示询问您是否希望看到关于您的数据探索的详细报告时，请键入“y”。否则，如果您对打印出来的输出感到满意，当您看到该提示时，您可以轻松地键入“n”。******

## ******运行 xplore 方法的示例输出******

******在运行了使用 xplore 方法浏览数据的代码之后，您的输出应该是这样的:******

```
****------------------------------------
The first 5 entries of your dataset are:

   rank country_full country_abrv  total_points  ...  three_year_ago_avg  three_year_ago_weighted  confederation   rank_date
0     1      Germany          GER           0.0  ...                 0.0                      0.0           UEFA  1993-08-08
1     2        Italy          ITA           0.0  ...                 0.0                      0.0           UEFA  1993-08-08
2     3  Switzerland          SUI           0.0  ...                 0.0                      0.0           UEFA  1993-08-08
3     4       Sweden          SWE           0.0  ...                 0.0                      0.0           UEFA  1993-08-08
4     5    Argentina          ARG           0.0  ...                 0.0                      0.0       CONMEBOL  1993-08-08

[5 rows x 16 columns]

------------------------------------
The last 5 entries of your dataset are:

       rank country_full country_abrv  total_points  ...  three_year_ago_avg  three_year_ago_weighted  confederation   rank_date
57788   206     Anguilla          AIA           0.0  ...                 0.0                      0.0       CONCACAF  2018-06-07
57789   206      Bahamas          BAH           0.0  ...                 0.0                      0.0       CONCACAF  2018-06-07
57790   206      Eritrea          ERI           0.0  ...                 0.0                      0.0            CAF  2018-06-07
57791   206      Somalia          SOM           0.0  ...                 0.0                      0.0            CAF  2018-06-07
57792   206        Tonga          TGA           0.0  ...                 0.0                      0.0            OFC  2018-06-07

[5 rows x 16 columns]

------------------------------------
Stats on your dataset:

<bound method NDFrame.describe of        rank country_full country_abrv  total_points  ...  three_year_ago_avg  three_year_ago_weighted  confederation   rank_date
0         1      Germany          GER           0.0  ...                 0.0                      0.0           UEFA  1993-08-08
1         2        Italy          ITA           0.0  ...                 0.0                      0.0           UEFA  1993-08-08
2         3  Switzerland          SUI           0.0  ...                 0.0                      0.0           UEFA  1993-08-08
3         4       Sweden          SWE           0.0  ...                 0.0                      0.0           UEFA  1993-08-08
4         5    Argentina          ARG           0.0  ...                 0.0                      0.0       CONMEBOL  1993-08-08
...     ...          ...          ...           ...  ...                 ...                      ...            ...         ...
57788   206     Anguilla          AIA           0.0  ...                 0.0                      0.0       CONCACAF  2018-06-07
57789   206      Bahamas          BAH           0.0  ...                 0.0                      0.0       CONCACAF  2018-06-07
57790   206      Eritrea          ERI           0.0  ...                 0.0                      0.0            CAF  2018-06-07
57791   206      Somalia          SOM           0.0  ...                 0.0                      0.0            CAF  2018-06-07
57792   206        Tonga          TGA           0.0  ...                 0.0                      0.0            OFC  2018-06-07

[57793 rows x 16 columns]>

------------------------------------
The Value types of each column are:

rank                         int64
country_full                object
country_abrv                object
total_points               float64
previous_points              int64
rank_change                  int64
cur_year_avg               float64
cur_year_avg_weighted      float64
last_year_avg              float64
last_year_avg_weighted     float64
two_year_ago_avg           float64
two_year_ago_weighted      float64
three_year_ago_avg         float64
three_year_ago_weighted    float64
confederation               object
rank_date                   object
dtype: object

------------------------------------
Info on your Dataset:

<bound method DataFrame.info of        rank country_full country_abrv  total_points  ...  three_year_ago_avg  three_year_ago_weighted  confederation   rank_date
0         1      Germany          GER           0.0  ...                 0.0                      0.0           UEFA  1993-08-08
1         2        Italy          ITA           0.0  ...                 0.0                      0.0           UEFA  1993-08-08
2         3  Switzerland          SUI           0.0  ...                 0.0                      0.0           UEFA  1993-08-08
3         4       Sweden          SWE           0.0  ...                 0.0                      0.0           UEFA  1993-08-08
4         5    Argentina          ARG           0.0  ...                 0.0                      0.0       CONMEBOL  1993-08-08
...     ...          ...          ...           ...  ...                 ...                      ...            ...         ...
57788   206     Anguilla          AIA           0.0  ...                 0.0                      0.0       CONCACAF  2018-06-07
57789   206      Bahamas          BAH           0.0  ...                 0.0                      0.0       CONCACAF  2018-06-07
57790   206      Eritrea          ERI           0.0  ...                 0.0                      0.0            CAF  2018-06-07
57791   206      Somalia          SOM           0.0  ...                 0.0                      0.0            CAF  2018-06-07
57792   206        Tonga          TGA           0.0  ...                 0.0                      0.0            OFC  2018-06-07

[57793 rows x 16 columns]>

------------------------------------
The shape of your dataset in the order of rows and columns is:

(57793, 16)

------------------------------------
The features of your dataset are:

Index(['rank', 'country_full', 'country_abrv', 'total_points',
       'previous_points', 'rank_change', 'cur_year_avg',
       'cur_year_avg_weighted', 'last_year_avg', 'last_year_avg_weighted',
       'two_year_ago_avg', 'two_year_ago_weighted', 'three_year_ago_avg',
       'three_year_ago_weighted', 'confederation', 'rank_date'],
      dtype='object')

------------------------------------
The total number of null values from individual columns of your dataset are:

rank                       0
country_full               0
country_abrv               0
total_points               0
previous_points            0
rank_change                0
cur_year_avg               0
cur_year_avg_weighted      0
last_year_avg              0
last_year_avg_weighted     0
two_year_ago_avg           0
two_year_ago_weighted      0
three_year_ago_avg         0
three_year_ago_weighted    0
confederation              0
rank_date                  0
dtype: int64

------------------------------------
The number of rows in your dataset are:

57793

------------------------------------
The values in your dataset are:

[[1 'Germany' 'GER' ... 0.0 'UEFA' '1993-08-08']
 [2 'Italy' 'ITA' ... 0.0 'UEFA' '1993-08-08']
 [3 'Switzerland' 'SUI' ... 0.0 'UEFA' '1993-08-08']
 ...
 [206 'Eritrea' 'ERI' ... 0.0 'CAF' '2018-06-07']
 [206 'Somalia' 'SOM' ... 0.0 'CAF' '2018-06-07']
 [206 'Tonga' 'TGA' ... 0.0 'OFC' '2018-06-07']]

------------------------------------

Do you want to generate a detailed report on the exploration of your dataset?
[y/n]: y
Generating report...

Summarize dataset: 100%|████████████████████████████████████████████████████████████████████████████| 30/30 [03:34<00:00,  7.14s/it, Completed] 
Generate report structure: 100%|█████████████████████████████████████████████████████████████████████████████████| 1/1 [00:31<00:00, 31.42s/it] 
Render HTML: 100%|███████████████████████████████████████████████████████████████████████████████████████████████| 1/1 [00:12<00:00, 12.07s/it] 
Export report to file: 100%|█████████████████████████████████████████████████████████████████████████████████████| 1/1 [00:00<00:00,  8.00it/s] 
Your Report has been generated and saved as 'output.html'****
```

> ******Python 是你能读懂的最强大的语言。
> —保罗·杜布瓦******

******作为一个狂热的学习者，我非常热衷于为下一代学习者提供更简单的东西，自动化是实现这一点的最简单的方法之一。为自己省去在数据探索上浪费大量时间的压力。当你下一次浏览你的数据时，一定要 ***浏览*** 你的数据😉******

******如果你喜欢我的队友和我在这个项目中所做的，请花几分钟时间给明星⭐️on 留下 [GitHub repo](https://github.com/buabaj/xplore) ，并通过点击这个[链接](https://ctt.ac/ptbm8)在 twitter 上告诉你的朋友关于 **xplore** 。******

******感谢您抽出几分钟时间阅读本文。我希望它是有教育意义和有帮助的😊如果你想在推特和 T2 的 LinkedIn 上私下聊天，我随时奉陪。编程快乐！******

*******我衷心感谢* [*安娜·阿伊库*](mailto:ayikuanna44@gmail.com) *校对并纠正了我写这篇文章时犯的许多错误。*******