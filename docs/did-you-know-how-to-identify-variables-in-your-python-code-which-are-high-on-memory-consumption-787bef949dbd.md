# 识别内存消耗高的变量

> 原文：<https://towardsdatascience.com/did-you-know-how-to-identify-variables-in-your-python-code-which-are-high-on-memory-consumption-787bef949dbd?source=collection_archive---------52----------------------->

## 优化 Python 代码

![](img/536d544916c8e1e7892e8c86678616f7.png)

[附身摄影](https://unsplash.com/@possessedphotography?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

有时候，在执行 Python 脚本时，我们会遇到**内存错误**。这些错误主要是由于一些变量**内存消耗高**。

在本教程中，我们将重点关注剖析 Python 代码以**优化内存消耗**。**内存分析**是一个过程，使用它我们可以剖析我们的代码并识别导致内存错误的变量。

## **1。)内存分析器**

*   **内存分析器**是一个 python 包，它评估函数中编写的每一行 Python 代码，并相应地检查内部内存的使用情况。
*   我们可以使用 ***pip 或者 conda 包管理器*** 来安装这个包。如果使用 ***Anaconda*** 发行版，我会推荐使用 ***conda install*** ，因为它会自动解决依赖和环境问题。

```
**#### Install Packages** conda install memory_profiler
```

*   安装完成后，可以使用以下代码行在 ***iPython*** 环境中加载评测器:

```
**#### Loading Memory Profiler Package** %load_ext memory_profiler
```

*   加载后，使用以下语法对任何预定义的函数进行内存分析:

```
**#### Memory Profiling a Function** %mprun -f function_name_only Call_to_function_with_arguments
```

**注**:要使用内存评测器评测一个函数，将其作为一个模块导入，这意味着将其写入一个单独的 ***。py*** 文件，然后像其他包一样导入到主程序中。

## 2.)工作示例

对于本教程，我使用了与[内存分析器的 PyPi 网页](https://pypi.org/project/memory-profiler/)相同的功能。它**创建两个有大量元素的列表变量**，然后删除其中一个(参考下面代码中的函数定义)。然后我们保存并导入这个函数到主程序中进行分析。

```
**#### Function Definition (Saved as example.py file)**
def my_func(): 
    a = [1] * (10 ** 6) 
    b = [2] * (2 * 10 ** 7) 
    del b 
    return a**#### Loading the line profiler within ipython/jupyter environment** %load_ext memory_profiler
from example import my_func**#### Profiling the function using line_profiler**
%mprun -f my_func my_func()**#### Memory Profile Output**
Line #    Mem usage    Increment   Line Contents
================================================
     1     50.8 MiB     50.8 MiB   def my_func():
     2     58.4 MiB      7.6 MiB    a = [1] * (10 ** 6)
     3    211.0 MiB    152.6 MiB    b = [2] * (2 * 10 ** 7)
     4     58.4 MiB      0.0 MiB    del b
     5     58.4 MiB      0.0 MiB    return a
```

# 说明

从内存分析器的输出中，可以看到输出表有四列:

*   **第 1 列(行号)** —代码行号
*   **第 2 列(内存使用)** —函数使用的总内存，直到行号
*   **第 3 列(增量)** —程序特定行使用的内存
*   **第 4 列(内容)** —实际代码内容

**注意**:从程序中去掉 ***变量“b”***后内存消耗明显下降。

从表中可以清楚地看出， ***变量“b”***的创建导致了内存使用量的突然激增。在实时场景中，一旦我们意识到这样的发现，我们总是可以寻找其他方法来编写代码，使内存消耗保持在可接受的范围内。

# 结束语

在这个简短的教程中，我们学习了 Python 代码的内存分析。要了解 Python 代码的时间分析，请参考[本](/did-you-know-you-can-measure-the-execution-time-of-python-codes-14c3b422d438)教程。

我希望这个**你知道吗**系列的教程信息丰富，足智多谋。

我们将在未来的教程中尝试引入更多有趣的话题。在此之前:

快乐学习！！！！