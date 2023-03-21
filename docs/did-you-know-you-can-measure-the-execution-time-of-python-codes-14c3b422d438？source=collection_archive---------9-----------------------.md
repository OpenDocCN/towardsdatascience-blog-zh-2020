# 您知道吗——Python 技巧

> 原文：<https://towardsdatascience.com/did-you-know-you-can-measure-the-execution-time-of-python-codes-14c3b422d438?source=collection_archive---------9----------------------->

## 测量 Python 代码的执行时间

![](img/aade13314da56d77537b8c7f71b12a6f.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) 拍摄的照片

当我们开始精通一门编程语言时，我们渴望不仅交付最终目标，而且使我们的程序更高效。

在**你知道吗**系列的这个教程中，我们将学习一些 **Ipython 的**魔法命令，它们可以帮助我们对 python 代码进行时间分析。

注意，出于本教程的目的，建议使用 Anaconda 发行版。

# 1.)分析一行代码

要检查一行 python 代码的执行时间，可以使用 **%timeit** 。下面是一个简单的例子来说明它是如何工作的:

```
#### Simple usage of magics command %timeit
**%timeit [num for num in range(20)]**#### Output
**1.08 µs ± 43 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)**
```

关键注意事项:

*   在要评测的代码行之前使用 **%timeit**
*   返回每次运行内循环**r**次数或运行和**n**次数计算的代码运行时间的**平均值和标准差**。在上面的例子中，列表理解被评估了 7 次，每次运行有一百万次循环(默认行为)。这平均花费了 1.08 微秒，标准偏差为 43 纳秒。
*   你可以在调用魔术命令时自定义运行和循环的次数。下面的例子供参考:

```
#### Customizing number of runs and loops in %timeit magic command
**%timeit -r5 -n100 [num for num in range(20)]**1.01 µs ± 5.75 ns per loop (mean ± std. dev. of 5 runs, 100 loops each)
```

使用命令选项 **-r 和-n，**分别表示运行次数和循环次数，**我们将时间曲线操作定制为** 5 次运行和每次运行中的 100 次循环。

# 2.)剖析多行代码

本节向前迈进了一步，解释了如何分析一个完整的代码块。通过对**%下面有一个示例演示供参考:**

```
#### Time profiling the code block using %%timeit
**%%timeit -r5 -n1000
for i in range(10):
    n = i**2
    m = i**3
    o = abs(i)**#### Output
**10.5 µs ± 226 ns per loop (mean ± std. dev. of 5 runs, 1000 loops each)**
```

可以观察到循环的**的**平均执行时间**为 10.5 微秒。注意，相同的命令选项 **-r 和-n** 分别用于控制**运行计数和每次运行**中的循环数。**

# 3.2)对代码块中的每一行代码进行时间剖析

到目前为止，在分析单行代码或完整代码块时，我们只查看了汇总统计数据。如果我们想评估代码块中每一行代码的性能，该怎么办？**线路侧写师**包来救援！！！

**Line_profiler** 包可用于对任何**功能进行逐行剖析。**要使用 line_profiler 软件包，请执行以下步骤:

*   **安装包**—**line profiler**包可以通过简单调用 pip 或 conda install 来安装。如果使用 Python 的 anaconda 发行版，建议使用 **conda install**

```
#### Installing line_profiler package
**conda install line_profiler**
```

*   **加载扩展** —安装后，您可以使用 IPython 加载 **line_profiler IPython 扩展**，该扩展作为该软件包的一部分提供:

```
#### Loading line_profiler's Ipython extension
**%load_ext line_profiler**
```

*   **对函数**进行时间分析——一旦加载，使用以下语法对任何预定义的**函数**进行时间分析

```
%lprun -f **function_name_only** **function_call_with_arguments**
```

**语法细节**:

*   对 line_profiler 的调用以关键字 **%lprun** 开始，后跟**命令选项-f**
*   命令选项后面是**函数名**，然后是**函数调用**

在本练习中，我们将定义一个函数，接受身高(单位为米)和体重(单位为磅)列表作为输入，并将它们分别转换为厘米和千克。

```
**#### Defining the function**
def conversion(ht_mtrs, wt_lbs ):
    ht_cms = [ht*100 for ht in ht_mtrs]
    wt_kgs = [wt*.4535 for wt in wt_lbs]**#### Defining the height and weight lists:**
ht = [5,5,4,7,6]
wt = [108, 120, 110, 98]**#### Profiling the function using line_profiler** %lprun -f conversion conversion(ht,wt)---------------------------------------------------------------
**#### Output
Total time: 1.46e-05 s****File: <ipython-input-13-41e195af43a9>****Function: conversion at line 2****Line #      Hits         Time  Per Hit   % Time  Line Contents
==============================================================
     2       1        105.0    105.0     71.9      ht_cms = [ht*100 for ht in ht_mtrs]
     3       1         41.0     41.0     28.1      wt_kgs = [wt*.4535 for wt in wt_lbs]**
```

输出详细信息:

*   完整的时间曲线以 **14.6 微秒**为单位完成(参考输出的第一行)

生成的表格有 6 列:

*   第 1 列(行号)-代码的行号(**注意，第 1 行被故意从输出中省略，因为它只是函数定义语句**)
*   第 2 列(命中数)—线路被调用的次数
*   第 3 列(时间)—花费在代码行上的**时间单位数(每个时间单位为 14.6 微秒)**
*   第 4 列(每次命中)-第 3 列除以第 2 列
*   第 5 列(%Time) —在花费的总时间中，**在特定代码行上花费的时间百分比是多少**
*   第 6 列(内容)-代码行的内容

人们可以清楚地注意到，从米到厘米的高度转换几乎花费了总时间的 72%。

# 结束语

有了我们可以支配的每行代码的执行时间，我们就可以部署策略来提高代码的效率。在接下来的 3 个教程中，我们将分享一些**最佳实践**来帮助你提高代码**的效率。**

我希望这个【T42 你知道吗】系列的教程是有益的，你学到了一些新的东西。

会在以后的教程中尝试并带来更多有趣的话题。在此之前:

快乐学习！！！！