# 数据科学家，开始使用分析器

> 原文：<https://towardsdatascience.com/data-scientists-start-using-profilers-4d2e08e7aec0?source=collection_archive---------32----------------------->

## 找到算法中真正让你慢下来的部分

![](img/5790f4209f638c9b17d5bdea6c4f62f9.png)

照片由 [Jantine Doornbos](https://unsplash.com/@jantined?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

数据科学家通常需要编写大量复杂、缓慢、CPU 和 I/O 繁重的代码，无论您是处理大型矩阵、数百万行数据、读取数据文件还是浏览网页。

当对另一部分进行一些简单的修改就能使代码速度提高 10 倍时，难道你不想浪费时间重构代码的一部分，试图榨干最后一丝性能吗？

如果您正在寻找一种加快代码速度的方法，一个分析器可以准确地显示哪些部分花费的时间最多，让您看到哪些部分将从优化中受益最多。

分析器测量程序的时间或空间复杂度。对算法的大 O 复杂度进行理论化肯定是有价值的，但是检验算法的真实复杂度也同样有价值。

*你的代码最大的减速在哪里？你的代码* [*是 I/O 绑定还是 CPU 绑定*](https://stackoverflow.com/questions/868568/what-do-the-terms-cpu-bound-and-i-o-bound-mean) *？哪些具体线路导致了速度变慢？*

一旦你回答了这些问题，你将 A)对你的代码有更好的理解，B)知道你的优化工作的目标是什么，以便用最少的努力获得最大的收益。

让我们深入一些使用 Python 的快速示例。

# 基础知识

您可能已经熟悉了一些为代码计时的方法。您可以检查一行执行前后的时间，如下所示:

```
In [1]: start_time = time.time()
   ...: a_function() # Function you want to measure
   ...: end_time = time.time()
   ...: time_to_complete = end_time - start_time
   ...: time_to_complete
Out[1]: 1.0110783576965332
```

或者，如果您在 Jupyter 笔记本中，您可以使用神奇的`%time`命令来计时语句的执行，如下所示:

```
In [2]: %time a_function()
CPU times: user 14.2 ms, sys: 41 µs, total: 14.2 ms
Wall time: 1.01 s
```

或者，您可以使用 *other* 魔法命令`%timeit`，它通过多次运行该命令获得更精确的测量，如下所示:

```
In [3]: %timeit a_function()
1.01 s ± 1.45 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

或者，如果您想为整个脚本计时，您可以使用 bash 命令`time`，就像这样…

```
$ time python my_script.py

real    0m1.041s
user    0m0.040s
sys     0m0.000s
```

如果您想快速了解一个脚本或一段代码运行需要多长时间，这些技术非常有用，但是当您想要更全面的了解时，这些技术就没那么有用了。如果你不得不在`time.time()`支票中换行，那将是一场噩梦。在下一节中，我们将看看如何使用 Python 的内置分析器。

# 使用 cProfile 深入探索

当你试图更好地理解你的代码是如何运行的，首先要从 Python 的内置分析器 [cProfile](https://docs.python.org/3/library/profile.html#module-cProfile) 开始。cProfile 将记录你的程序的各个部分被执行的频率和时间。

请记住，cProfile 不应该用于测试您的代码。它是用 C 写的，这使得它很快，但它仍然引入了一些开销，可能会影响您的时间。

有多种方法可以使用 cProfile，但是一种简单的方法是从命令行使用。

在演示 cProfile 之前，让我们先来看一个基本的示例程序，它将下载一些文本文件，计算每个文件中的单词数，然后将每个文件中的前 10 个单词保存到一个文件中。话虽如此，代码做什么并不太重要，只是我们将使用它来展示分析器是如何工作的。

测试我们的分析器的演示代码

现在，使用下面的命令，我们将分析我们的脚本。

```
$ python -m cProfile -o profile.stat script.py
```

`-o`标志为 cProfile 指定一个输出文件，以保存分析统计数据。

接下来，我们可以使用 [pstats](https://docs.python.org/3/library/profile.html#module-pstats) 模块(也是标准库的一部分)启动 python 来检查结果。

```
In [1]: import pstats
   ...: p = pstats.Stats("profile.stat")
   ...: p.sort_stats(
   ...:   "cumulative"   # sort by cumulative time spent
   ...: ).print_stats(
   ...:   "script.py"    # only show fn calls in script.py
   ...: )Fri Aug 07 08:12:06 2020    profile.stat46338 function calls (45576 primitive calls) in 6.548 secondsOrdered by: cumulative time
List reduced from 793 to 6 due to restriction <'script.py'>ncalls tottime percall cumtime percall filename:lineno(function)
     1   0.008   0.008   5.521   5.521 script.py:1(<module>)
     1   0.012   0.012   5.468   5.468 script.py:19(read_books)
     5   0.000   0.000   4.848   0.970 script.py:5(get_book)
     5   0.000   0.000   0.460   0.092 script.py:11(split_words)
     5   0.000   0.000   0.112   0.022 script.py:15(count_words)
     1   0.000   0.000   0.000   0.000 script.py:32(save_results)
```

哇！看看那些有用的信息！

对于每个被调用的函数，我们看到以下信息:

*   `ncalls`:函数被调用的次数
*   `tottime`:给定函数花费的总时间(不包括调用子函数)
*   `percall` : `tottime`除以`ncalls`
*   `cumtime`:该功能和所有子功能花费的总时间
*   `percall`:(再次)`cumtime`除以`ncalls`
*   `filename:lineo(function)`:文件名、行号、函数名

当读取这个输出时，请注意我们隐藏了大量数据——事实上，我们只看到了 793 行中的 6 行。那些隐藏的行都是从像`urllib.request.urlopen`或`re.split`这样的函数中调用的子函数。另外，注意`<module>`行对应于`script.py`中不在函数内部的代码。

现在让我们回头看看结果，按累计持续时间排序。

```
ncalls tottime percall cumtime percall filename:lineno(function)
     1   0.008   0.008   5.521   5.521 script.py:1(<module>)
     1   0.012   0.012   5.468   5.468 script.py:19(read_books)
     5   0.000   0.000   4.848   0.970 script.py:5(get_book)
     5   0.000   0.000   0.460   0.092 script.py:11(split_words)
     5   0.000   0.000   0.112   0.022 script.py:15(count_words)
     1   0.000   0.000   0.000   0.000 script.py:32(save_results)
```

记住函数调用的层次结构。顶层`<module>`调用`read_books`，`save_results.`调用`get_book`，`split_words`和`count_words`。通过比较累积时间，我们看到`<module>`的大部分时间花在了`read_books`上，而`read_books`的大部分时间花在了`get_book`上，在这里我们发出 HTTP 请求，使得这个脚本(*不出所料*)受到 I/O 的限制。

接下来，让我们看看如何通过逐行剖析我们的代码来更加细化。

# 逐行剖析

一旦我们使用 cProfile 了解了哪些函数调用花费了最多的时间，我们就可以逐行检查这些函数，从而更清楚地了解我们的时间都花在了哪里。

为此，我们需要用以下命令安装`line-profiler`库:

```
$ pip install line-profiler
```

一旦安装完毕，我们只需要将`@profile`装饰器添加到我们想要分析的函数中。以下是我们脚本的更新片段:

注意，我们不需要导入`profile`装饰函数——它将由`line-profiler`注入。

现在，为了分析我们的函数，我们可以运行以下代码:

```
$ kernprof -l -v script-prof.py
```

`kernprof`随`line-profiler`一起安装。`-l`标志告诉`line-profiler`逐行进行，而`-v`标志告诉它将结果打印到终端，而不是保存到文件中。

我们脚本的结果看起来会像这样:

这里重点关注的关键栏目是`% Time`。正如你所看到的，我们解析每本书的 89.5%的时间都花在了`get_book`函数上——发出 HTTP 请求——进一步验证了我们的程序是 I/O 受限的，而不是 CPU 受限的。

现在，有了这个新的信息，如果我们想加速我们的代码，我们就不会浪费时间试图让我们的单词计数器更有效。与 HTTP 请求相比，它只需要很少的时间。相反，我们会专注于加速我们的请求——可能是通过异步方式。

在这里，结果并不令人惊讶，但是在一个更大更复杂的程序中，`line-profiler`是我们编程工具带中一个无价的工具，它允许我们窥视我们程序的内部并找到计算瓶颈。

# 轮廓记忆

除了分析我们程序的时间复杂度，我们还可以分析它的内存复杂度。

为了进行逐行内存分析，我们需要安装`memory-profiler`库，它也使用相同的`@profile`装饰器来确定要分析哪个函数。

```
$ pip install memory-profiler$ python -m memory_profiler script.py
```

在同一个脚本上运行`memory-profiler`的结果应该如下所示:

目前有一些关于“增量”准确性的[问题](https://github.com/pythonprofilers/memory_profiler/issues/236)，所以现在只关注“内存使用”栏。

当我们把书分成单词时，我们的脚本在第 28 行内存使用达到峰值。

# 结论

希望现在您的编程工具箱中有一些新工具来帮助您编写更高效的代码，并快速确定如何最好地利用您的优化时间。

你可以在这里阅读更多关于 cProfile [的内容，在这里](https://docs.python.org/3/library/profile.html)阅读线分析器[的内容，在这里](https://github.com/pyutils/line_profiler)阅读内存分析器[的内容。我也强烈推荐 Micha Gorelick 和 Ian Ozsvald [1]写的书*高性能 Python* 。](https://github.com/pythonprofilers/memory_profiler)

*感谢阅读！我很想听听你对分析器、数据科学或其他任何东西的看法。下面评论或者伸手上*[*LinkedIn*](https://www.linkedin.com/in/austinpoor)*或者*[*Twitter*](https://twitter.com/austin_poor)*！*

# 参考

[1] M. Gorelick 和 I. Ozsvald，[高性能 Python 第二版](https://www.oreilly.com/library/view/high-performance-python/9781492055013/) (2020)，奥赖利媒体公司。