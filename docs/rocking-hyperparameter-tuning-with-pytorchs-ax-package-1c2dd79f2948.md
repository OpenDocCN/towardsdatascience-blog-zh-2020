# 使用 PyTorch 的 Ax 包进行摇摆超参数调谐

> 原文：<https://towardsdatascience.com/rocking-hyperparameter-tuning-with-pytorchs-ax-package-1c2dd79f2948?source=collection_archive---------13----------------------->

## 使用贝叶斯和土匪优化较短的调谐，同时享受难以置信的简单设置。它非常有效。

![](img/e5ebdd7d4e30ad1ca1cf0c479cb031dc.png)

雅罗斯瓦夫·米奥

对于许多机器学习任务，超参数调整是必须的。我们通常努力为我们的问题选择正确的算法和架构，然后严格训练以获得一个好的模型。在这两次之后进行超参数调整(HPT)可能看起来没有必要，但事实上，这是至关重要的。HPT 应该定期进行，并可能帮助我们以较小的努力实现性能的巨大改进。

我在这里建议的是用新发布的 python 包实现 HPT 的一种简单方法。它将完美地工作，并且花费不超过半个小时来设置你在你自己的计算机上训练的任何东西。对于其他模型，尤其是那些需要部署培训或在生产中运行的模型，这将通过一些小的调整来实现，我们将在本文的 B 部分进一步讨论。

## Ax 是什么？

Ax 是 PyTorch 的一个开源包，它可以帮助您在您定义的参数范围内找到任何函数的最小值。机器学习中最常见的用例是找到训练模型的最佳超参数，这些参数将使您的总体损失最小化。该软件包通过运行多轮训练来实现这一点，每轮训练使用一组不同的参数，并返回损失最小的参数。诀窍在于，它不需要对这些参数进行网格搜索或随机搜索，而是使用更复杂的算法，因此节省了大量训练和运行时间。

Ax 可以找到连续参数(比如学习率)和离散参数(比如隐藏层的大小)的最小值。它对前者使用贝叶斯优化，对后者使用 bandit 优化。即使这个包来自 pytorch，它也可以用于任何函数，只要它返回一个您想要最小化的值。

在我们开始之前，安装说明可以在[这里](https://ax.dev/docs/installation.html)找到。

## 第一部分——“你好，世界！”

Ax 有几种操作模式，但我们将从最基本的一种开始，用一个小例子来说明。如前所述，我们通常会使用 Ax 来帮助我们找到最佳的超参数，但在其核心，这个包可以帮助我们找到一个函数关于某些参数的最小值。这就是为什么对于这个例子，我们将运行 Ax 来寻找一个复杂的二次函数的最小值。为此，我们将定义一个名为 booth 的函数，该函数在字典 *p* 中接收其参数 *{x1，x2}* :

```
# Sample Quadratic Function
def booth(p): 
 # p = dictionary of parameters 
 return (p[“x1”] + 2*p[“x2”] — 7)**2 + (2*p[“x1”] + p[“x2”] — 5)**2print(booth({“x1”:6, “x2”: 7}))
```

这将返回:365。

为了找到“booth”的最小值，我们将让 ax 运行该函数几次。它为每次运行选择的参数取决于以前的运行——它运行的参数以及对这些参数的函数的结果是什么(在机器学习中，“结果”==“损失”)。运行下一个代码位执行 20 次连续的函数运行，具有 20 组不同的参数 *{x1，x2}* 。然后打印出*【最佳参数】*。作为比较，如果我们想用网格搜索来运行这个，对于 *x1* 和 *x2，*的跳跃为 0.5，我们将需要 1600 次运行，而不是 20 次！

```
from ax import optimize
best_parameters, best_values, _, _ = optimize(
 parameters=[
 {“name”: “x1”,
  “type”: “range”,
  “bounds”: [-10.0, 10.0],},
 {“name”: “x2”,
  “type”: “range”,
  “bounds”: [-10.0, 10.0],},],
 evaluation_function=booth,
 minimize=True,)print(best_parameters)
```

这会打印出来(真实最小值是(1，3)):

```
{'x1': 1.0358775112792173, 'x2': 2.8776698220783423}
```

相当接近！在笔记本电脑上，这可能不到 10 秒钟。您可能会注意到，返回的“最佳参数”并不完全是真正的最小值，但是它们使函数非常接近实际的最小值。您对结果中的边距有多宽容是一个您稍后可以使用的参数。

这里我们需要做的唯一一件事是确保我们有一个以字典作为输入返回单个值的函数。这是运行 ax 时大多数情况下需要的。

## B 部分—使用部署运行

A 部分非常简单，可以在你的电脑上运行的任何东西上很好地工作，但是它有几个缺点:

1.  它一次只运行一个“训练”,如果一个训练会话超过几分钟就不好了。
2.  它只在本地运行，如果您的常规模型运行在云/部署等上，这是很糟糕的。

为此，我们需要一些更复杂的东西:最好是某种神奇的方法，只需要几组参数就可以运行，这样我们就可以部署培训，然后耐心等待，直到他们得到结果。当第一批训练完成时，我们可以让 ax 知道损失是什么，获得下一组参数并开始下一批。用 ax 的行话来说，这种运行 ax 的方式就是所谓的“ax 服务”。短语和运行都很简单，几乎和我们在 a 部分看到的一样简单。

为了适应“ax 范式”,我们仍然需要某种运行训练并返回损失的函数，可能类似于下面这样:

```
def training_wrapper(parameters_dictionary): 1\. run training with parameters_dictionary (this writes the loss value to somewhere in the cloud)2\. read loss from somewhere in the cloud3\. return loss
```

在定义了适当的包装器之后，我们可以运行 ax 服务 HPT 代码。因为我希望您能够在您的计算机上运行这个演示代码，所以我将坚持使用我们在 A 部分中介绍的二次函数——“booth”。 *{x1，x2}* 的参数范围将保持不变。对于运行部署，我可以用我的“training_wrapper”版本替换下一个示例代码中的 booth。示例代码如下所示:

```
from ax.service.ax_client import AxClientax = AxClient(enforce_sequential_optimization=False)ax.create_experiment(
 name=”booth_experiment”,
 parameters=[
 {“name”: “x1”,
 “type”: “range”,
 “bounds”: [-10.0, 10.0],},
 {“name”: “x2”,
 “type”: “range”,
 “bounds”: [-10.0, 10.0],},],
 objective_name=”booth”,
 minimize=True,
)for _ in range(15):
 next_parameters, trial_index = ax.get_next_trial()
 ax.complete_trial(trial_index=trial_index, raw_data=booth(next_parameters))best_parameters, metrics = ax.get_best_parameters()
```

这与 a 部分有一点不同，我们不再仅仅使用“优化”功能。相反:

1.  我们初始化一个 ax 客户端。
2.  我们建立了一个“实验”，并选择了一系列我们想要检查的超参数。
3.  我们用 *get_next_trial()得到下一组我们想要运行函数的参数。*
4.  等待函数完成，用 *complete_trial()* 运行

也就是说，我们把获取下次运行的参数和实际运行分开了。如果您想并发运行，您可以一次获得 N 个 get_next_trial()，并异步运行它们。如果您想这样做，请确保不要忘记将*“enforce _ sequential _ optimization”*标志设置为 false。如果您想知道您可以同时运行多少次，您可以使用*get _ recommended _ max _ parallelism*(在这里阅读这个函数[的输出)。](https://ax.dev/api/service.html)

差不多就是这样。这个包是非常可定制的，并且可以处理您想要为您的特定环境所做的任何改变。[文档](https://ax.dev/docs/why-ax.html)可读性很强，尤其是[教程](https://ax.dev/tutorials/)。它还有各种各样的可视化效果[来帮助你弄清楚发生了什么。](https://ax.dev/tutorials/visualizations.html)

## 摘要

您必须手动设置多次运行或使用网格搜索的日子已经一去不复返了。的确，在一个非常大的超参数集上，ax 可能导致类似随机搜索的结果，但是即使它没有减少您最终运行的训练数量，它仍然为您节省了编码和多次运行的处理。设置非常简单——我强烈建议您亲自尝试一下！

链接:

1.  通用文档:[https://ax.dev/docs/why-ax.html](https://ax.dev/docs/why-ax.html)
2.  教程:[https://ax.dev/tutorials/](https://ax.dev/tutorials/)
3.  可视化:[https://ax.dev/tutorials/visualizations.html](https://ax.dev/tutorials/visualizations.html)