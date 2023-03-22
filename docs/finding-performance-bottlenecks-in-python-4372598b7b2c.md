# 在 Python 中寻找性能瓶颈

> 原文：<https://towardsdatascience.com/finding-performance-bottlenecks-in-python-4372598b7b2c?source=collection_archive---------5----------------------->

## 是什么让你的代码如此缓慢？

![](img/446e5c504b0daabe93191fe0f38cba5b.png)

[来源](https://pixabay.com/images/id-382992/)

在这篇文章中，我将解释如何分析一段代码，并可能发现性能瓶颈。

# 让我们首先创建瓶颈

假设我们需要创建一个在循环上迭代的脚本，每次循环执行都会产生一个依赖于当前迭代的值和一个独立值。独立值是初始设置的一部分，这可能是一个相对繁重的功能:

```
import timedef initial_setup():
  a = 7
  time.sleep(1)
  return a
```

我们的“initial_setup”函数只是返回一个固定值，但是我添加了一个 1 秒钟的 [time.sleep()](https://docs.python.org/3/library/time.html#time.sleep) 调用来模拟一个更重的函数。让我们继续构建脚本的主函数，我们姑且称之为“slow_function”。

```
def slow_function():
  x = range(5)
  for i in x:
    a = initial_setup()
    b = a + i
    print(b)
```

我们的新函数只是简单地迭代[0，4]并将当前数字添加到初始设置中返回的固定数字。您可能已经注意到，我在循环块中错误地调用了“initial_setup()”。即使结果是正确的，这仍然是一个性能错误，因为“initial_setup()”不依赖于循环迭代，而且，它是一个很重的函数。

无论如何，让我们假装没有注意到，并测试我们的解决方案。您应该会得到以下结果:

```
7
8
9
10
11
```

很管用，对吧？虽然它比我预期的要长…我不知道是什么让它这么慢，所以也许是时候使用 Python 的分析器了…

# cProfile 入门

我们需要剖析我们的功能，并找出是什么阻碍了它的性能！幸运的是，Python 提供了一个可以帮助我们完成这项任务的模块: [cProfile](https://docs.python.org/3/library/profile.html#module-cProfile) 。我们将从创建 Profile 类的一个实例开始:

```
import cProfileprofile = cProfile.Profile()
```

我们现在准备好分析我们的函数了！我们将开始使用一种简单的方法，使用 runcall()方法，它接收要分析的函数(及其参数，如果有的话)。在我们的例子中，我们想要分析“slow_function()”，所以我们将添加以下指令:

```
import cProfileprofile = cProfile.Profile()
profile.runcall(slow_function)
```

现在，如果您运行上面的代码片段，您将会看到，显然，什么都没有发生。至少，在 stdout 中没有输出任何分析信息。这是因为我们缺少了拼图的最后一块: [pstats](https://docs.python.org/3/library/profile.html#pstats.Stats) 。让我们继续将它添加到我们的代码片段中:

```
import cProfile
import pstatsprofile = cProfile.Profile()
profile.runcall(slow_function)
ps = pstats.Stats(profile)
ps.print_stats()
```

如您所见，我们将“profile”实例传递给了构造函数 [Stats](https://docs.python.org/3/library/profile.html#pstats.Stats) 来创建这个类的新实例“ps”。这个实例最终允许我们打印分析结果。

好了，说够了(或写够了)，让我们运行我们的代码片段，看看我们得到了什么！

```
7
8
9
10
11
         17 function calls in 5.011 seconds Random listing order was used ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        5    0.001    0.000    0.001    0.000 {built-in method builtins.print}
        5    5.010    1.002    5.010    1.002 {built-in method time.sleep}
        5    0.000    0.000    5.010    1.002 python-performance-bottlenecks.py:5(initial_setup)
        1    0.000    0.000    5.011    5.011 python-performance-bottlenecks.py:10(slow_function)
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
```

# 如何解读 print_stats()的结果？

很好！我们现在可以看到一系列信息，这些信息应该可以指出性能瓶颈。为了找到它，我们应该首先理解每一列的含义。

*   ncalls:被分析的函数被调用的次数
*   tottime:在分析的函数中花费的总执行时间(不包括子函数的执行时间)
*   percall:总时间除以 ncalls
*   累计时间:所分析的函数花费的总执行时间(包括子函数的执行时间)
*   percall:累计时间除以 ncalls
*   文件名:行号(函数):文件、行号和分析的函数

既然我们理解了每一列的含义，我们可以寻找阻碍我们表现的原因。嗯…哦！“initial_setup()”被调用了 5 次。实际上累计执行时间在 5 秒以上！这是有意义的，因为我在 for 循环中错误地调用了“initial_setup()”!好吧，我在循环外调用它，因为它不依赖于它。

我们已经解决了第一个性能问题！现在，我们将如何解决未来的瓶颈？我们应该一直关注 ncalls 吗？嗯，有时候。但有时，我们会面临另一种问题，ncalls 不会告诉我们任何事情。例如，我们可能不得不处理一个只被调用一次，但是非常慢的函数。作为一般策略，我倾向于首先查看具有较高累积时间(cumtime)的函数，然后检查是否有任何函数在 ncalls 中表现突出。然而，我要说的是，选择一个策略是一个相当个人化的旅程，是我们根据自己的经验建立起来的。

# 更灵活的分析方法

有时，您可能不想分析整个函数，而只想分析它的一部分。这也是可能的，而且很容易，方法如下:

```
def slow_function():
  profile = cProfile.Profile()
  profile.enable()
  x = range(5)
  for i in x:
    a = initial_setup()
    b = a + i
    print(b)
  profile.disable()
  ps = pstats.Stats(profile)
  ps.print_stats()
```

我们必须将我们想要分析的代码片段放在“profile.enable()”和“profile.disable()”之间。请记住，在这种情况下，我们正在分析一个玩具示例，但在实际情况下，您可能必须分析一个更大的函数，因此，如果您怀疑瓶颈可能在哪里，限制被分析的代码片段的范围总是有用的。

此外，[从 Python 3.8](https://docs.python.org/3/whatsnew/3.8.html#cprofile) 开始，您还可以选择将其用作上下文管理器，将“profile”实例的范围限制在封闭的块中:

```
with cProfile.Profile() as profile:
  x = range(5)
  for i in x:
    a = initial_setup()
    b = a + i
    print(b)
  ps = pstats.Stats(profile)
  ps.print_stats()
```

# 充分利用 pstats

一旦您开始在真实世界的代码库中进行分析，您将会看到打印的统计数据会大得多，因为您可能会分析更大的代码片段。嗯，在某些时候，有这么多的信息，它变得有点混乱。幸运的是， [pstats](https://docs.python.org/3/library/profile.html#pstats.Stats) 可以帮助您使这些信息更易于管理。

首先，您可以使用 [sort_stats()](https://docs.python.org/3/library/profile.html#pstats.Stats.sort_stats) 来确定打印函数的顺序(您也可以在链接中找到可用的排序键)。例如，根据呼叫次数或累计时间对其进行分类可能会有所帮助。您可以定义多个排序键，按照键的顺序定义它们的优先级。另一个方便的选择是设置打印功能数量的限制。你可以通过传递一个整数给 [print_stats()](https://docs.python.org/3/library/profile.html#pstats.Stats.print_stats) 来实现。让我们修改我们的示例，按照 ncalls 和 cumtime 的顺序对输出进行排序，并限制为 3 个函数:

```
def slow_function():
  profile = cProfile.Profile()
  profile.enable()
  x = range(5)
  for i in x:
    a = initial_setup()
    b = a + i
    print(b)
  profile.disable()
  ps = pstats.Stats(profile)
  ps.sort_stats('calls', 'cumtime') 
  ps.print_stats(3)
```

您的输出应该如下所示:

```
7
8
9
10
11
         17 function calls in 5.017 seconds Ordered by: call count, cumulative time
   List reduced from 5 to 3 due to restriction <3> ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        5    0.000    0.000    5.016    1.003 python-performance-bottlenecks.py:5(initial_setup)
        5    5.016    1.003    5.016    1.003 {built-in method time.sleep}
        5    0.001    0.000    0.001    0.000 {built-in method builtins.print}
```

如您所见，已经应用了定义的顺序和限制。还要注意，这些限制是在输出中声明的。

好了，就这样，你现在已经准备好让你的代码更快了！