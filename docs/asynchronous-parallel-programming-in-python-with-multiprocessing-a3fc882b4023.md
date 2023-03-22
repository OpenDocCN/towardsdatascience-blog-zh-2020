# 多处理 Python 中的异步并行编程

> 原文：<https://towardsdatascience.com/asynchronous-parallel-programming-in-python-with-multiprocessing-a3fc882b4023?source=collection_archive---------12----------------------->

## 一种在个人计算机上加速代码的灵活方法

![](img/edd5a7fe34fa50080daa86b75cca763d.png)

托马斯·索贝克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

你希望你的 Python 脚本能运行得更快吗？也许他们可以。而且你不会(可能)不得不买一台新电脑，或者使用一台超级电脑。大多数现代计算机都包含多个处理核心，但默认情况下，python 脚本只使用一个核心。编写可以在多个处理器上运行的代码确实可以减少您的处理时间。本文将演示如何使用`multiprocessing`模块来编写并行代码，这些代码使用所有机器的处理器并提升脚本的性能。

## 同步与异步模型

异步模型在新资源可用时立即启动任务，而不等待之前运行的任务完成。相比之下，同步模型在开始任务 2 之前等待任务 1 完成。有关示例的更详细解释，请查看启动中的[这篇文章](https://medium.com/swlh/understanding-sync-async-concurrency-and-parallelism-166686008fa4)。如果您能够以适当的方式构建代码，异步模型通常会为性能改进提供最大的机会。也就是说，任务可以彼此独立运行。

为了简洁起见，本文将只关注异步并行化，因为这是最有可能提升性能的方法。此外，如果您在笔记本电脑上为异步并行化构建代码，那么升级到超级计算机会容易得多。

## 装置

由于 Python 2.6 `multiprocessing`已经作为基础模块包含，所以不需要安装。简单来说就是`import multiprocessing`。因为“多重处理”需要一点时间来打字，所以我更喜欢`import multiprocessing as mp`。

## 问题是

我们有一组参数值，希望在灵敏度分析中使用。我们运行分析的函数计算量很大。我们可以通过同时并行运行多个参数来减少处理时间。

## 设置

导入`multiprocessing`、`numpy`和`time`。然后定义一个函数，将行号、`i`和三个参数作为输入。行号是必需的，以便以后可以将结果链接到输入参数。记住，异步模型不保持顺序。

```
import multiprocessing as mp
import numpy as np
import timedef my_function(i, param1, param2, param3):
    result = param1 ** 2 * param2 + param3
    time.sleep(2)
    return (i, result)
```

出于演示的目的，这是一个计算开销不大的简单函数。我添加了一行代码来暂停函数 2 秒钟，模拟长时间运行。函数输出对`param1`最敏感，对`param3`最不敏感。实际上，您可以用任何函数来替换它。

我们需要一个函数，可以将`my_function`的结果添加到一个结果列表中，这个函数被创造性地命名为`results`。

```
def get_result(result):
    global results
    results.append(result)
```

让我们以串行(非并行)方式运行这段代码，看看需要多长时间。用 0 到 100 之间的 3 列随机数建立一个数组。这些是将被传递给`my_function`的参数。然后创建空的`results`列表。最后，遍历`params`中的所有行，并将`my_function`到`results`的结果相加。计时，看看需要多长时间(大约 20 秒)，并打印出`results`列表。

```
if __name__ == '__main__':
    params = np.random.random((10, 3)) * 100.0
    results = []
    ts = time.time()
    for i in range(0, params.shape[0]):
        get_result(my_function(i, params[i, 0], params[i, 1],\
         params[i, 2]))
    print('Time in serial:', time.time() - ts)
    print(results)
```

正如所料，这段代码运行了大约 20 秒。另外，请注意结果是如何按顺序返回的。

```
Time in serial: 20.006245374679565
[(0, 452994.2250955602), (1, 12318.873058254741), (2, 310577.72144939064), (3, 210071.48540466625), (4, 100467.02727256044), (5, 46553.87276610058), (6, 11396.808561138329), (7, 543909.2528728382), (8, 79957.52205218966), (9, 47914.9078853125)]
```

## 并行运行

现在使用`multiprocessing`并行运行相同的代码。只需将下面的代码直接添加到串行代码的下面进行比较。为清晰起见，本文末尾包含了完整 Python 脚本的要点。

重置`results`列表，使其为空，并重置开始时间。我们需要指定我们想要使用多少个 CPU 进程。`multiprocessing.cpu_count()`返回机器可用的全部进程。然后循环遍历`params`的每一行，使用`multiprocessing.Pool.apply_async` 调用`my_function`并保存结果。使用`apply_async`的`args`参数传递`my_function`的参数，回调函数是发送`my_function`结果的地方。这将在一个新进程可用时立即开始一个新进程，并继续这样做，直到循环完成。然后关闭进程池。等待执行任何后续代码，直到所有进程都运行完毕。现在打印这段代码运行的时间和结果。

```
results = []
ts = time.time()
pool = mp.Pool(mp.cpu_count())
for i in range(0, params.shape[0]):
    pool.apply_async(my_function, args=(i, params[i, 0], params[i,\
     1], params[i, 2]), callback=get_result)
pool.close()
pool.join()
print('Time in parallel:', time.time() - ts)
print(results)
```

注意，使用`apply_async`将运行时间从 20 秒减少到 5 秒以下。另外，请注意，结果没有按顺序返回。这就是传递并返回行索引的原因。

```
Time in parallel: 4.749683141708374
[(0, 452994.2250955602), (2, 310577.72144939064), (1, 12318.873058254741), (3, 210071.48540466625), (4, 100467.02727256044), (5, 46553.87276610058), (6, 11396.808561138329), (7, 543909.2528728382), (9, 47914.9078853125), (8, 79957.52205218966)]
```

## 结论

对代码实现异步并行化可以大大减少运行时间。`multiprocessing`模块是在个人计算机上进行并行化的一个很好的选择。正如您在本文中看到的，根据您的机器规格，您可以获得显著的速度提升。如果你最终想升级到超级计算机，请注意`multiprocessing`有局限性。如果您的目标是超级计算，那么您会希望使用与消息传递接口(MPI)兼容的并行化模型。