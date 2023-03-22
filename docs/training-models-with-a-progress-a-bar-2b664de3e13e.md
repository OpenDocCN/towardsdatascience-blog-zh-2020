# 带进度条的培训模型

> 原文：<https://towardsdatascience.com/training-models-with-a-progress-a-bar-2b664de3e13e?source=collection_archive---------7----------------------->

## 如何跟踪您的 ML 实验的进展

![](img/843dd9e57a1526a04a3de4ace6b29bb9.png)

照片由 [**迈克**](https://www.pexels.com/@mike-468229?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 发自 [**派克斯**](https://www.pexels.com/photo/brown-hourglass-on-brown-wooden-table-1178684/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

`tqdm`是一个用于添加进度条的 Python 库。它允许您配置并显示一个进度条，其中包含您想要跟踪的指标。其易用性和多功能性使其成为跟踪机器学习实验的完美选择。

我将本教程分为两部分。我将首先介绍`tqdm,`，然后展示一个机器学习的例子。对于本文中的每个代码片段，我们将从 Python 的`time`库中导入`sleep`函数，因为它将让我们减慢程序速度以查看进度条更新。

```
from time import sleep
```

# Tqdm

可以用`pip install tqdm`安装 tqdm。这个库附带了各种迭代器，每个迭代器都有我将要介绍的特定用途。

`tqdm`是默认的迭代器。它接受一个迭代器对象作为参数，并在遍历该对象时显示一个进度条。

输出是

```
100%|█████████████████████████████████| 5/5 [00:00<00:00, 9.90it/s]
```

您可以看到带有`9.90it/s`的漂亮输出，意味着每秒 9.90 次迭代的平均速度。迭代的“it”可以被配置成其他的东西，这就是我们将在下一个例子中看到的。

`trange`遵循与 Python 中的`range`相同的模板。例如，给`trange`迭代次数。

```
Proving P=NP: 100%|████████████| 20/20 [00:02<00:00, 9.91carrots/s]
```

在这个例子中，你可以看到我们添加了一个(笑话)描述，描述了我们正在做的事情以及每次迭代的单元。

# 在执行过程中更新进度栏

`tqdm`有两个方法可以更新进度条中显示的内容。

要使用这些方法，我们需要将`tqdm`迭代器实例赋给一个变量。这可以通过 Python 中的`=`操作符或`with`关键字来完成。

例如，我们可以用数字`i`的除数列表来更新后缀。让我们用这个函数来得到除数的列表

这是我们带有进度条的代码。

如果你觉得自己是一个杰出的 Python 程序员，你可以像这样使用`with`关键字

使用`with`会自动调用块末尾的`pbar.close()`。

这里是显示在`i=6`的状态。

```
Testing even number 6: 70%|██████████████▋ | 7/10 [00:03<00:01, 1.76carrots/s, divisors=[1, 2, 3]]
```

# 跟踪损失和准确性

在本节中，我们使用 PyTorch 编写的神经网络，并使用`tqdm`对其进行训练，以显示损失和准确性。这是模型

这是一个简单的感知器模型，我们可以用它来处理和分类 MNIST 数据集中的数字图像。以下加载 MNIST 数据集的代码灵感来自于 [PyTorch 示例](https://github.com/pytorch/examples/blob/master/mnist/main.py)。

我们刚刚加载了数据，定义了模型和设置，现在可以运行训练实验了。

我再次使用`sleep`功能暂停程序，这样我们就可以看到进度条的更新。正如你所看到的，我们只是应用了我们之前在这里学到的东西，特别是用`tepoch.set_postfix`和`tepoch.set_description`让你更新进度条显示的信息。下面是程序运行时的输出截图

```
Epoch 1: 15%|▉ | 142/937 [00:16<01:32, 8.56batch/s, accuracy=89.1, loss=0.341]
```

这给了我们如何在实际中使用`tqdm`的想法。

# 结论

你可以用`tqdm`实现更多，比如让它适应 Jupyter 笔记本，精细配置进度条更新或嵌套进度条，所以我推荐你阅读文档了解更多:[https://github.com/tqdm/tqdm](https://github.com/tqdm/tqdm)

感谢您的阅读！

*原载于 2020 年 10 月 12 日*[*https://adamoudad . github . io*](https://adamoudad.github.io/posts/progress_bar_with_tqdm/)*。*