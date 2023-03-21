# 使用消息传递接口(mpi4py)的 Python 并行编程

> 原文：<https://towardsdatascience.com/parallel-programming-in-python-with-message-passing-interface-mpi4py-551e3f198053?source=collection_archive---------8----------------------->

## 为超级计算机准备好您的代码。没那么难。

![](img/56159355fe8ed7b4292be689436e6599.png)

照片由 [Carol Jeng](https://unsplash.com/@carolran?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

您知道您可以编写并行 Python 代码，在您的笔记本电脑和超级计算机上运行吗？你可以，而且没有你想象的那么难。如果您已经为[异步并行化](/asynchronous-parallel-programming-in-python-with-multiprocessing-a3fc882b4023)编写了代码，那么您甚至不需要进行太多的重构。

高性能计算(HPC)将任务分配到数千个 CPU 上(与笔记本电脑上的 4–8 个 CPU 形成对比),以实现显著的性能提升。CPU 使用消息传递接口(MPI)进行通信和传递数据。当您编写代码将任务分配给多个内核同时运行时，在您的笔记本电脑上也使用了相同的原则。本文将演示如何使用 MPI 和 Python 来编写可以在您的笔记本电脑或超级计算机上并行运行的代码。

## 安装 MPI

您需要为您的操作系统安装一个 MPI 应用程序。对于 Windows 用户，我建议直接从微软安装 MPI。对于 Mac 和 Linux 用户，我建议安装 [OpenMPI](https://www.open-mpi.org/software/ompi/v4.0/) 。Windows 用户必须将 MPI 安装目录添加到[路径变量](https://www.java.com/en/download/help/path.xml)。

要测试您的安装，请在终端窗口中键入`mpiexec` (Windows)或`mpirun` (Mac/Linux，但请检查安装文档)，然后按“Enter”键。如果您已正确安装，这将生成一条包含使用信息的消息。当你在终端时，也输入`python`并按下‘回车’。这应该会启动一个交互式 Python 会话。如果没有，您需要安装或配置 Python。

## 安装 mpi4py

`mpi4py`是一个 Python 模块，允许您与您的 MPI 应用程序进行交互(`mpiexec`或`mpirun`)。和任何 Python 模块(`pip install mpi4py`等)一样安装。).

一旦你安装了 MPI 和`mpi4py`，你就可以开始了！

## 一个基本例子

使用 MPI 运行 Python 脚本与您可能习惯的略有不同。使用`mpiexec`和`mpirun`，每一行代码将由每个处理器运行，除非另有说明。让我们制作一个“hello world”示例来演示 MPI 基础知识。

创建一个新的 python 脚本(`.py`文件)。导入`mpi4py`并使用`MPI.COMM_WORLD`来获取关于所有可用于运行您的脚本的处理器的信息(当调用脚本时，这个数字被传递给 MPI 应用程序)。`COMM_WORLD`访问可用于分配工作的进程数量(等级/处理器),以及关于每个处理器的信息。给出了为运行我们的脚本而分配的队列或处理器的总数。`rank`给出当前执行代码的处理器的标识符。下面的`print`语句将为作业中使用的每个处理器打印一次。

通过打开终端，导航到包含该脚本的目录，并执行以下命令来执行该脚本:

```
mpiexec -n 4 python mpi_hello_world.py
```

`n -4`指定要使用的处理器数量。在这个实例中，我使用了 4 个处理器，这意味着 print 语句将执行 4 次。注意，等级不是按数字顺序打印出来的，所以您需要确保您的代码可以异步运行。换句话说，不可能知道哪个处理器将首先启动或完成，因此您的代码需要以这样一种方式进行组织，即结果不依赖于可能在不同处理器上计算的值。

```
Hello world from rank 2 of 4
Hello world from rank 3 of 4
Hello world from rank 0 of 4
Hello world from rank 1 of 4
```

现在，更新脚本，以便它为不同的等级输出不同的消息。这是使用逻辑语句(`if`、`elif`、`else`)完成的。

我们现在得到 0 级和 1 级的不同消息。

```
First rank
Hello world from rank 0 of 4
Not first or second rank
Hello world from rank 2 of 4
Not first or second rank
Hello world from rank 3 of 4
Second rank
Hello world from rank 1 of 4
```

## 发送和接收数组

`send`和`recv`功能分别将数据从一个处理器发送到另一个处理器，并从一个处理器接收数据。许多数据类型可以通过这些函数传递。这个例子将特别关注发送和接收`numpy`数组。`Send`和`Recv`函数(注意大写字母“S”和“R”)是专用于`numpy`数组的。有关基本`send`和`recv`的示例，请参见`[mpi4py](https://mpi4py.readthedocs.io/en/stable/tutorial.html)` [文档](https://mpi4py.readthedocs.io/en/stable/tutorial.html)。

在上一篇文章中，我用`multiprocessing`模块演示了并行处理。我们将在这个例子中使用相同的函数。

[](/asynchronous-parallel-programming-in-python-with-multiprocessing-a3fc882b4023) [## 多处理 Python 中的异步并行编程

### 一种在个人计算机上加速代码的灵活方法

towardsdatascience.com](/asynchronous-parallel-programming-in-python-with-multiprocessing-a3fc882b4023) 

在同一个目录中创建两个新的 Python 脚本。说出一个`my_function.py`和另一个`mpi_my_function.py`的名字。在`my_function.py`中，实现上面链接的文章中的功能。你的脚本应该是这样的。这是一个简单的函数，用暂停来模拟长时间的运行。

这些段落解释了`my_function`的并行化程序。代码在下面的要点中给出(带注释)。在`mpi_my_function.py`中导入`my_function`、`mpi4py`和`numpy`。然后从`MPI.COMM_WORLD`得到大小和等级。使用`numpy`为`my_function`创建随机参数值。所有处理器上都有`params`变量。现在划分参数列表，将数组的一部分分配给每个进程(或等级)。我特意将`params` (15)中的行数奇怪地被处理器(4)的数量整除，因此我们必须做一些额外的数学运算来分解`params`。现在每个处理器都有一个变量来索引它在`params`数组中的块的`start`和`stop`位置。

我们希望最终结果是一个数组，其中包含每个参数集的参数值和函数结果。创建一个空数组，`local_results`,其行数与参数数组相同，并增加一列来存储结果。然后对每个参数集运行`my_function`，并将结果保存在结果数组中(`local_results`)。现在每个处理器都有了它的`params`数组块的结果。

必须收集结果以创建最终数组，其中包含每个原始参数组合的结果。将每个等级的`local_results`数组发送到等级‘0’，在那里它们被组合成一个数组。当使用`Send`时，指定要发送到的秩，`dest`，并指定一个`tag`(唯一的整数)，以便接收秩知道要检索哪个值(如果您最终执行多个`Send`，这一点很重要)。

对于接收秩(0)，遍历所有其他秩，创建一个大小为要接收的数组的空数组，并使用`Recv`从每个秩中检索发送的值，指定要接收的秩和`tag`。检索到数组后，将其添加到现有值中。打印出最终的数组，确保它看起来是正确的。我们完事了。

使用以下命令运行上面的脚本:

```
mpiexec -n 4 python mpi_my_function.py
```

结果应该类似于:

```
results
[[7.58886620e+00 5.62618310e+01 9.09064771e+01 3.33107541e+03]
 [2.76707037e+01 4.03218572e+01 2.20310537e+01 3.08951805e+04]
 [7.82729169e+01 9.40939134e+01 7.24046134e+01 5.76552834e+05]
 [9.88496826e+01 6.91320832e+00 1.59490375e+01 6.75667032e+04]
 [8.94286742e+01 8.88605014e+01 5.31814181e+01 7.10713954e+05]
 [3.83757552e+01 4.64666288e+01 3.72791712e+01 6.84686177e+04]
 [9.33796247e+01 1.71058163e+01 2.94036272e+00 1.49161456e+05]
 [1.49763382e+01 6.77803268e+01 7.62249839e+01 1.52787224e+04]
 [7.42368720e+01 8.45623531e+01 6.27481273e+01 4.66095445e+05]
 [6.76429554e+01 5.95075836e+01 9.82287031e+00 2.72290902e+05]
 [4.94157194e+00 7.38840592e+01 3.70077813e+00 1.80788546e+03]
 [2.71179540e+01 2.94973140e+00 2.86632603e+01 2.19784685e+03]
 [2.92793532e+01 9.90621647e+01 9.45343344e+01 8.50185987e+04]
 [1.20975353e+01 8.89643839e+01 7.13313160e+01 1.30913009e+04]
 [8.45193908e+01 4.89884544e+01 5.67737042e+01 3.50007141e+05]]
```

## 结论

您可能已经注意到，并行运行一个 4 行函数需要 43 行代码。似乎有点矫枉过正。我们并行化了一个简单的函数，但这实际上是一个更复杂的例子，因为需要进行操作来获得所需的输出格式。这个例子作为更复杂函数的并行化模板。一般来说，我将编写一个 Python 模块来运行我的模型/分析，然后在并行化脚本(上面的 43 行要点)中设置并调用模块中的一个函数(完成所有工作)。您可以运行非常复杂的分析，而无需向我们在此创建的脚本添加太多代码。一旦你让这个代码在你的个人机器上运行，它就可以在大多数超级计算机上运行，而不需要太多额外的工作。

并行快乐！