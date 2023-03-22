# 为 PyTorch 生成批处理数据

> 原文：<https://towardsdatascience.com/generating-batch-data-for-pytorch-7435b1a02e21?source=collection_archive---------12----------------------->

![](img/847a70d9c49063f6611e38e5695b69a9.png)

由[肯尼斯·贝里奥斯·阿尔瓦雷斯](https://unsplash.com/@chezzus38?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 实践中的深度学习

## 为 PyTorch 创建定制数据加载器——变得简单！

我正在创建一个定制的 PyTorch 培训模块，这个模块过于复杂，尤其是在生成培训批次并确保这些批次在培训期间不会重复的时候。“这是一个已解决的问题”，我在实验室的深处疯狂地编码时心想。

从数据集中选择项目时，不希望只增加索引是有原因的。1)这无法扩展到多个员工。2)你需要随机化你的序列以最大化训练效果。

这就是 Torch 的数据工具(`torch.utils.data`)派上用场的地方。您不应该从头开始创建批处理生成器。

您可以采取两种方法。1)在创建数据集之前移动所有预处理，并仅使用数据集生成项目，或者 2)在数据集的初始化步骤中执行所有预处理(缩放、移位、整形等)。如果你只使用火炬，方法#2 是有意义的。我使用多个后端，所以我使用方法 1。

# 步伐

1.  创建自定义数据集类。你重写了`__len__()`和`__getitem__()`方法。
2.  创建一个使用`torch.utils.data.dataloader`的迭代器
3.  在训练循环中使用这个迭代器。

简单。瞧啊。

关于如何使用的示例，请参见附加的代码。

# 摘要

在本文中，我们回顾了向 PyTorch 训练循环提供数据的最佳方法。这打开了许多感兴趣的数据访问模式，有助于更轻松、更快速的培训，例如:

1.  使用多个进程读取数据
2.  更加标准化的数据预处理
3.  在多台机器上使用数据加载器进行分布式培训

感谢阅读！

如果你喜欢这个，你可能会喜欢:

[](/using-lstm-autoencoders-on-multidimensional-time-series-data-f5a7a51b29a1) [## 对多维时间序列数据使用 LSTM 自动编码器

### 演示如何使用 LSTM 自动编码器分析多维时间序列

towardsdatascience.com](/using-lstm-autoencoders-on-multidimensional-time-series-data-f5a7a51b29a1)