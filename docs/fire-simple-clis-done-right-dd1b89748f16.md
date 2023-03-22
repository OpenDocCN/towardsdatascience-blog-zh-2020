# 火:简单的气候做对了

> 原文：<https://towardsdatascience.com/fire-simple-clis-done-right-dd1b89748f16?source=collection_archive---------46----------------------->

## 创建 CLI 有助于提高 ML 管道的可访问性和重用性，但是设置它们可能会很麻烦。进入火场。

![](img/5a35ef706befe3ff73b112569cc1a375.png)

照片由来自 Unsplash 的 Cullan Smith 拍摄

# 什么是火？

几个月前，我大发雷霆，我[表达了我对](https://mark.douthwaite.io/monster-jobs-with-tqdm/)创建进度条的一个叫做`TQDM`的小软件包的赞赏。这篇文章也是同样的思路，但这次是关于`Fire`:一个[伟大的包](https://github.com/google/python-fire)，它可以让命令行界面(CLI)在几秒钟内(字面上)建立并运行起来轻而易举。

那么，为什么要写 CLI 呢？实际上，一个简单的 CLI 可以使配置脚本变得像更改几个命令行参数一样简单。假设你有一个设置在编排服务上的脚本(可能类似于 Jenkins ),它定期重新训练你最新最棒的 Tweet 情感分类器。假设是一个 [Scikit-Learn](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html) [随机森林](https://en.wikipedia.org/wiki/Random_forest#:~:text=Random%20forests%20or%20random%20decision,prediction%20(regression)%20of%20the%20individual)。您可以像这样运行作业:

```
python tweet-classifier/train.py
```

您可以选择直接在代码中调整模型的参数。一个更好的方法是将模型封装在一个函数中，并将模型的参数绑定到一个函数签名(例如`train(n_estimators: int = 100)`)上，而不是运行类似如下的代码:

```
python tweet-classifier/train.py --n_estimators 100
```

这将允许您通过 CLI 配置模型的超参数。

这个*防止你为了改变脚本的配置而需要改变代码*本身，这反过来可以帮助其他用户轻松地获取和运行你的代码。对于“生产”代码来说，这通常也是一个更好的想法，对源代码的更改应该被仔细跟踪，并传播到该代码的所有实例(您可能正在运行几十个不同的 Tweet 分类器，更改其中一个代码可能会以意想不到的方式改变它们)。几十个陈旧的 Git 分支对此也是一个糟糕的解决方案。对已部署代码的特别更改会造成混乱，最终会让您(或您的雇主)损失一大笔钱。CLIs 可以成为你工具箱里的一个工具，帮助你避免这种命运。

虽然使用 Python 的标准库来设置 CLI 是可能的(并且仍然非常简单),但是它很快就会变得冗长。下面是一个简单的例子，说明如何为一个脚本创建一个 CLI 来打印到`n`的斐波那契数列:

```
# fib.py
import argparsedef fibonacci(n, a=0, b=1):
    """Print the Fibonacci series up to n."""
    while a <= n:
        print(a)
        a, b = b, a + bif __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("-n", type=int)
    args = parser.parse_args()
    fibonacci(args.n)
```

现在，要执行它，您可以运行:

```
python fib.py -n 13
```

您应该看到 Fibonacci 序列打印到控制台。很简单。现在让我们假设你想设置你的初始条件`a`和`b`。您可以通过以下方式实现:

```
if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("-n", type=int)
    parser.add_argument("-a", type=int)
    parser.add_argument("-b", type=int)
    args = parser.parse_args()
    fibonacci(args.n, a=args.a, b=args.b)
```

仍然很简单，但是你的代码很快变得有点臃肿。您可能会想象，随着函数签名中参数数量的增加(或添加新函数)，我们的`ArgumentParser`块将会快速增长，跟踪所有这些函数及其参数可能会变得非常复杂，最终可能会构成几十行代码。此外，对于简单的脚本来说，这可能是一个足够大的障碍，添加 CLI 可能看起来有点痛苦——也许您可能*甚至*想直接在脚本中更改一些参数。

# 输入`Fire`

这就是`Fire`的用武之地:它*自动*从函数/方法签名生成 CLI。让我们看看最初的例子，但是这次使用`Fire`:

```
# fib.py
import firedef fibonacci(n, a=0, b=1):
    """Print the Fibonacci series up to n."""
    while a <= n:
        print(a)
        a, b = b, a + bif __name__ == "__main__":
    fire.Fire(fibonacci)
```

首先，你可以用`pip install fire`把`Fire`安装成一个普通的 Python 包。在`Fire`的项目报告上有更多的安装细节。您现在可以运行:

```
python fib.py -n 13
```

您应该会看到相同的输出。您也可以运行:

```
python fib.py -n 13 -a 0 -b 1
```

如您所见，`Fire`自动将函数签名中的*参数(即函数参数)映射到相应的 CLI 参数。只需一行代码，您就可以启动并运行 CLI。但这只是一个简单的案例。让我们假设您想要为一个脚本创建一个具有许多不同功能的 CLI(可能一个用于训练模型，一个用于运行推理等等。).下面是前面例子的修改版本，引入了一个新的函数和一个新的 tuple 对象`struct`:*

```
# cli.py
import firedef fibonacci(n, a=0, b=1):
    """Print the Fibonacci series up to n."""
    while a <= n:
        print(a)
        a, b = b, a + bdef say_hello(name):
    print(f"Hello, {name}!")struct = tuple(["a", "b"])if __name__ == "__main__":
    fire.Fire()
```

这做了一些非常有趣的事情。如果将`Fire`函数调用的第一个参数留空，那么`Fire`会检查当前模块中的 Python 对象，并通过 CLI 公开它们。例如，您可以运行:

```
python cli.py fibonacci -n 13
```

这将像以前一样运行`fibonacci`功能。但是，您也可以运行:

```
python cli.py say_hello Jane
```

这将产生预期的输出`Hello, Jane!`。也许最有趣的是，你也可以运行:

```
python cli.py struct 0
```

您应该会看到输出`a`。同样，您可以运行:

```
python cli.py struct 1
```

且看`b`。在这种情况下，`Fire`让您直接从 CLI 与本地 Python 对象直接交互。您也可以将这个想法扩展到您自己的定制数据结构中。

# 结束语

`Fire`是一个很棒的工具，可以在几秒钟内让干净、高质量的 CLIs 启动并运行，它非常简单，你几乎不需要学习任何新概念就可以在工作中使用它。将一个新模型管道快速包装在一个闪亮的新 CLI 中以备部署是理想的。还有一堆[更高级的特性](https://github.com/google/python-fire/blob/master/docs/guide.md#grouping-commands)你也可以用来构建更复杂的 CLI。

然而，如果你正在构建一个全功能的 CLI 并想要细粒度的控制，你可能想看看古老而超级灵活的`[click](https://github.com/pallets/click)` [库](https://github.com/pallets/click)。这将为您提供尽可能多的控制(至少在 Python 中)。