# 如何在 Python 中分析您的代码

> 原文：<https://towardsdatascience.com/how-to-profile-your-code-in-python-e70c834fad89?source=collection_archive---------2----------------------->

## 使用 cProfile 查找瓶颈并优化性能

![](img/4988a7c1b57cf832aa29bdf51be8813f.png)

来自 [Pexels](https://www.pexels.com/photo/speed-bump-speed-limit-traffic-sign-vintage-1502365/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Anthony Maggio](https://www.pexels.com/@anthony-maggio-657507?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 摄影

如果你曾经写过一行代码(或者甚至几万行)，你一定想知道“为什么我的代码需要这么长时间才能运行？”回答这个问题并不总是简单的，但如果你以正确的方式寻找答案，它会变得更容易。

也许你相信你手头问题的知识，利用你的专业知识先检查某些片段。也许你对几个不同的模块/类/函数计时，看看大部分执行时间花在哪里。更好的是，您可以分析您的代码，以获得更多关于不同函数和子函数所花费的相对时间的信息。不管你的过程是什么，这个博客可能会教你一些更快找到答案的方法。

在这篇文章中，我将首先向您展示一个基本的分析方法。我将为它添加越来越多的特性和味道，以一个好的、可靠的剖析装饰器结束。对于那些赶时间的人(或者想回头参考这些资料的人)，可以去这个 [GitHub 仓库](https://github.com/ekhoda/profile_decorator)，在那里你可以找到概要文件装饰器和一个例子。

# 计时！

要分析你的代码，你需要知道如何计时。为此，您可以使用如下简单的方法:

```
**from** time **import** timestart = time()
*# your script here* end = time()
print(**f'It took {**end - start**} seconds!'**)
```

为了方便地为几个函数/代码片段计时(或者如果您只是喜欢使用更简洁、更 pythonic 化的方法)，您可以将上面的内容转换成一个*计时器装饰器*(这里用示例[讨论](/bite-sized-python-recipes-52cde45f1489))。

在任何函数上使用定时器都可以单独显示该部分的运行时间。为了使这种方法有助于找到瓶颈，我们需要更多的信息。为了有效地分析代码，下列两个条件中至少有一个应该为真:

1.  **我们应该知道程序的总运行时间，以便更好地了解我们期望的功能/部分的相对运行时间。**例如，如果一段代码需要 5 分钟来执行，那是总运行时间的 10%、40%还是 90%？
2.  我们应该对手头的问题或程序其他部分的运行时间有足够的了解，从而有把握地将一段给定的代码标记为瓶颈。即使一个功能需要 10 分钟才能运行(假设 10 分钟相对来说很长)，如果我们确信没有其他部分需要更长的时间，我们就应该担心它的低效率。正如唐纳德·克努特的名言:

> 过早优化是万恶之源。

像`[cProfile](https://docs.python.org/3.8/library/profile.html)`这样的分析器包通过满足这两个条件来帮助我们找到代码中的瓶颈。

# 如何使用 cProfile

## 基本用法

用`cProfile`进行概要分析的最基本方式是使用`run()`函数。您需要做的就是将您想要分析的内容作为字符串语句传递给`run()`。这是报告的一个示例(为简洁起见，进行了删节):

```
>>> **import** cProfile
>>> **import** pandas **as** pd>>> cProfile.run(**"pd.Series(list('ABCDEFG'))"**)258 function calls (256 primitive calls) in 0.001 secondsOrdered by: standard namencalls  tottime  percall  cumtime  percall filename:lineno(function)
     4    0.000    0.000    0.000    0.000 <frozen importlib._bootstrap>:997(_handle_fromlist)
     1    0.000    0.000    0.000    0.000 <string>:1(<module>)
     1    0.000    0.000    0.000    0.000 _dtype.py:319(_name_get)
  ....
  11/9    0.000    0.000    0.000    0.000 {built-in method builtins.len}
     1    0.000    0.000    0.000    0.000 {built-in method numpy.array}
     1    0.000    0.000    0.000    0.000 {built-in method numpy.empty}
  ....
```

第一行表示监控了 258 个调用，其中 256 个是原始调用(原始调用不是通过递归引起的)。下一行`Ordered by: standard name`表示报告是基于标准名称排序的，标准名称是`filename:lineno(function)`列中的文本。其后的一行是列标题:

`ncalls`:呼叫次数。当有两个数字时(如上面的 11/9)，该函数循环出现。第一个值是调用的总数，第二个值是原始或非递归调用的数量。

`tottime`:给定函数花费的总时间(不包括调用子函数的时间)。

`percall`:是`tottime`除以`ncalls`的商。

`cumtime`:该功能及所有子功能的累计时间。这个数字对于递归函数来说是精确的*甚至*。

`percall`:是`cumtime`的商除以原语调用。

`filename:lineno(function)`:提供各功能各自的数据。

`run()`函数可以再接受两个参数:一个`filename`将结果写入文件而不是 stdout，另一个`sort`参数指定输出应该如何排序。您可以查看[文档](https://docs.python.org/3.8/library/profile.html#pstats.Stats.sort_stats)以了解更多关于有效排序值的信息。常见的有`'cumulative'`(累计时间)`'time'`(总时间)`'calls'`(通话次数)。

如果您传递一个文件名并保存结果，您可能会注意到输出不是人类可读的。在这种情况下，您需要使用`pstats.Stats`类来格式化结果，我们将在接下来讨论。

## 使用`Profile`和`pstats.Stats`进行更多控制

虽然在大多数情况下使用`cProfile.run()`就足够了，但是如果您需要对分析进行更多的控制，您应该使用`cProfile`的`Profile`类。下面的片段摘自`Profile`级[文档](https://docs.python.org/3.8/library/profile.html#profile.Profile)。

```
**import** cProfile, pstats, io
**from** pstats **import** SortKey

pr = cProfile.Profile()
pr.enable()
*# ... do something ...* pr.disable()
s = io.StringIO()
sortby = SortKey.CUMULATIVE
ps = pstats.Stats(pr, stream=s).sort_stats(sortby)
ps.print_stats()
print(s.getvalue())
```

让我们一行一行地过一遍:

首先，我们创建一个`Profile`类(`pr`)的实例，并通过调用`enable`收集概要分析数据。当我们想要停止收集分析数据时，我们调用`disable`。接下来是对收集的统计数据进行格式化。然后，我们可以使用`[pstats.Stats](https://docs.python.org/3.8/library/profile.html#pstats.Stats)`类构造函数来创建 statistics 对象的实例(`ps`)。

`Stats`类可以从 profile 对象(`pr`)创建一个 statistics 对象，并将输出打印到传递给它的流中。`Stats`类还有一个`sort_stats`方法，根据提供的标准对结果进行排序。在这种情况下，标准是`SortKey.CUMULATIVE`,它代表在一个函数中花费的累计时间。如`[sort_stats](https://docs.python.org/3.8/library/profile.html#pstats.Stats.sort_stats)`文档所述，排序标准可以是`SortKey`枚举(在 Python 3.7 中添加)或字符串的形式(即使用`'cumulative'`代替`SortKey.CUMULATIVE`也是有效的)。最后，创建结果并打印到标准输出。

这是构建装饰器的好样板(要了解更多关于装饰器的知识，我推荐这本伟大而全面的[初级读本](https://realpython.com/primer-on-python-decorators/)):

```
**import** cProfile
**import** io
**import** pstats**def** profile(func):
    **def** wrapper(*args, **kwargs):
        pr = cProfile.Profile()
        pr.enable()
        retval = func(*args, **kwargs)
        pr.disable()
        s = io.StringIO()
        sortby = SortKey.CUMULATIVE  # **'cumulative'** ps = pstats.Stats(pr, stream=s).sort_stats(sortby)
        ps.print_stats()
        print(s.getvalue())
        **return** retval

    **return** wrapper*# Profile foo* @profile
**def** foo():
    print(**'Profile me!'**)
```

虽然这是一个好的开始，但是上面的装饰器中还有几个部分可以做得更好。特别是，更全面的职能可以:

1.  允许用户指定排序键(单个键或一组排序键)
2.  仅显示最大时间消费者，而不是所有线路
3.  通过删除文件名中的所有前导路径信息来整理报告，减小打印输出的大小并使报告更易于阅读
4.  将输出保存到文件，而不是打印到标准输出

以上所有情况都可以使用`pstats.Stats`中的方法进行处理。让我们按顺序复习一下:

1.  将所需的排序键传递给`sort_stats()`方法(尽管我们仍然需要检查它是单个值还是一个元组)
2.  将最大所需打印行数传递给`print_stats()`
3.  使用`strip_dirs()`
4.  默认情况下，输出被打印到`[sys.stdout](https://docs.python.org/3/library/sys.html#sys.stdout)`,因为`pstats.Stats()`使用它作为默认的流参数，但是要将结果写入文件，我们可以将文件作为流传递

将上述更改应用到`profile`装饰器将产生以下函数(在 docstring 中提供了参数的更多解释):

# 总结示例

为了了解`profile`装饰器如何帮助我们检测瓶颈，让我们创建一个具有多个函数的例子。假设您有一份过去一年销售的产品列表，并且您想通过计算售出的数量来了解每种产品的受欢迎程度。(我们想要的可以通过使用`[collections.Counter](https://docs.python.org/3/library/collections.html#collections.Counter)`很容易地完成，或者如果你是一个`pandas`用户，通过`[pandas.Series.value_counts](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.value_counts.html)`，但是为了这个练习，让我们忘记那些。)

```
**import** random
random.seed(20)**def** create_products(num):
    *"""Create a list of random products with 3-letter alphanumeric name."""* **return** [**''**.join(random.choices(**'ABCDEFG123'**, k=3)) **for** _ **in** range(num)]

*# version1* @profile(sort_by=**'cumulative'**, lines_to_print=10, strip_dirs=**True**)
**def** product_counter_v1(products):
    *"""Get count of products in descending order."""* counter_dict = create_counter(products)
    sorted_p = sort_counter(counter_dict)
    **return** sorted_p

**def** create_counter(products):
    counter_dict = {}
    **for** p **in** products:
        **if** p **not in** counter_dict:
            counter_dict[p] = 0
        counter_dict[p] += 1
    **return** counter_dict

**def** sort_counter(counter_dict):
    **return** {k: v **for** k, v **in** sorted(counter_dict.items(),
                                    key=**lambda** x: x[1],
                                    reverse=**True**)}

*# ===========
# Analysis starts here
# ===========* num = 1_000_000  *# assume we have sold 1,000,000 products* products = create_products(num)
*# Let's add profile decorator to product_counter_v1 function* counter_dict = product_counter_v1(products)
```

结果将保存在您当前目录下的`product_counter_v1.prof`中，它应该是这样的:

```
1007 function calls in 0.228 seconds

Ordered by: cumulative time

ncalls  tottime  percall  cumtime  percall filename:lineno(function)
     1    0.000    0.000    0.228    0.228 scratch_6.py:69(product_counter_v1)
     1    0.215    0.215    0.215    0.215 scratch_6.py:86(create_counter)
     1    0.000    0.000    0.013    0.013 scratch_6.py:105(sort_counter)
     1    0.013    0.013    0.013    0.013 {built-in method builtins.sorted}
     1    0.000    0.000    0.000    0.000 scratch_6.py:106(<dictcomp>)
  1000    0.000    0.000    0.000    0.000 scratch_6.py:107(<lambda>)
     1    0.000    0.000    0.000    0.000 {method 'items' of 'dict' objects}
     1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
```

检查这个文件，我们可以看到绝大部分的执行时间(0.228 秒中的 0.215 秒)都花在了`create_counter`函数上。所以，让我们制作一个新的`create_counter`函数，并检查它的效果。

```
*# version2* @profile(sort_by=**'cumulative'**, lines_to_print=10, strip_dirs=**True**)
**def** product_counter_v2(products):
    *"""Get count of products in descending order."""* counter_dict = create_counter_v2(products)
    sorted_p = sort_counter(counter_dict)
    **return** sorted_p

**def** create_counter_v2(products):
    counter_dict = {}
    **for** p **in** products:
        **try**:
            counter_dict[p] += 1
        **except** KeyError:
            counter_dict[p] = 1
    **return** counter_dict
```

剖析结果`product_counter_v2`如下所示:

```
1007 function calls in 0.169 seconds

Ordered by: cumulative time

ncalls  tottime  percall  cumtime  percall filename:lineno(function)
     1    0.000    0.000    0.169    0.169 scratch_6.py:78(product_counter_v2)
     1    0.158    0.158    0.158    0.158 scratch_6.py:95(create_counter_v2)
     1    0.000    0.000    0.010    0.010 scratch_6.py:105(sort_counter)
     1    0.010    0.010    0.010    0.010 {built-in method builtins.sorted}
     1    0.000    0.000    0.000    0.000 scratch_6.py:106(<dictcomp>)
  1000    0.000    0.000    0.000    0.000 scratch_6.py:107(<lambda>)
     1    0.000    0.000    0.000    0.000 {method 'items' of 'dict' objects}
     1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
```

运行时间从 0.228 秒减少到 0.169 秒，减少了约 26%。如果这仍然不令人满意，您可以尝试使用`[collections.defaultdict](https://docs.python.org/3/library/collections.html#collections.defaultdict)`来创建`counter_dict`。对于最后一个实现，我们将使用`collections.Counter`:

```
**import** collections

*# version3* @profile(sort_by=**'cumulative'**, lines_to_print=10, strip_dirs=**True**)
**def** product_counter_v3(products):
    *"""Get count of products in descending order."""* **return** collections.Counter(products)
```

性能分析`product_counter_v3`显示它比`product_counter_v1`提高了 62%。

```
11 function calls in 0.086 seconds

Ordered by: cumulative time

ncalls  tottime  percall  cumtime  percall filename:lineno(function)
     1    0.000    0.000    0.086    0.086 scratch_6.py:118(product_counter_v3)
     1    0.000    0.000    0.086    0.086 __init__.py:517(__init__)
     1    0.000    0.000    0.086    0.086 __init__.py:586(update)
     1    0.086    0.086    0.086    0.086 {built-in method _collections._count_elements}
     1    0.000    0.000    0.000    0.000 {built-in method builtins.isinstance}
     1    0.000    0.000    0.000    0.000 abc.py:180(__instancecheck__)
     2    0.000    0.000    0.000    0.000 _weakrefset.py:70(__contains__)
     2    0.000    0.000    0.000    0.000 {built-in method builtins.len}
     1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
```

试验一下`profile`装饰器的不同参数，看看输出会如何变化。你可以在这个 [GitHub 库](https://github.com/ekhoda/profile_decorator)中找到完整的例子。

快乐剖析！

我希望这篇博客对你有用。我可以在[*Twitter*](https://twitter.com/EhsanKhoda)*和*[*LinkedIn*](https://www.linkedin.com/in/ehsankhodabandeh)*上联系，我欢迎任何反馈。*