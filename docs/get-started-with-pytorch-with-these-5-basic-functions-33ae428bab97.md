# 从这 5 个基本功能开始使用 PyTorch。

> 原文：<https://towardsdatascience.com/get-started-with-pytorch-with-these-5-basic-functions-33ae428bab97?source=collection_archive---------18----------------------->

## PyTorch 基础

## 对深度学习的需求不断增长。更多的开发人员和数据科学家正在加入深度学习的行列，提供量身定制的库。

# 功能 1 —火炬.设备()

PyTorch 是一个由脸书开发的开源库，在数据科学家中很受欢迎。其崛起背后的一个主要原因是 [GPU](https://en.wikipedia.org/wiki/Graphics_processing_unit) 对开发者的内置支持。

*torch.device* 使您能够指定负责将张量加载到内存中的设备类型。该函数需要一个指定设备类型的字符串参数。

您甚至可以传递一个序号，如设备索引。或者不指定 PyTorch 使用当前可用的设备。

示例 1.1

示例 1.2

对于 [*例 1.1*](https://jovian.ml/aman-a-agarwal/01-tensor-operations/v/8#C3) ，我们选择运行时要存储张量的设备类型。注意，我们已经将我们的设备类型指定为 **cuda** ，并在相同的字符串中附加了序号，用“:”分隔。

[*例 1.2*](https://jovian.ml/aman-a-agarwal/01-tensor-operations/v/8#C5) 通过为设备类型和索引传入单独的参数，获得了相同的结果。

创建张量时定义设备类型

> torch.device()中预期的设备类型是 **cpu、cuda、mkldnn、opengl、opencl、ideep、hip、msnpu** 。为了正确使用此方法，设备类型应该存在于预期设备列表中。

让我们看看当我们尝试将 GPU 指定为设备类型时会发生什么。

示例 1.3 (a) —创建“gpu”类型设备实例时出现运行时错误

运行笔记本的计算机上应该有指定的设备类型。否则将导致错误，如*示例 1.3 (b)所述。*

在 *1.3 (b)* 中，我们使用需要 NVIDIA GPU 的 **cuda** 设备类型定义了一个张量。由于我们的机器没有任何可用的 GPU，内核抛出了一个运行时错误。

示例 1.3 (b) —指定不存在的设备类型时出现运行时错误

# 功能 2 — torch.view()

torch.view()方法将张量(无论是向量、矩阵还是标量)的视图更改为所需的形状。转换后的张量修改了表示维度，但保留了相同的数据类型。

让我们来看一个例子——定义一个样本张量 x，并转换它的维度以存储在另一个张量 z 中。

示例 2.1

在 [*例 2.1*](https://jovian.ml/aman-a-agarwal/01-tensor-operations/v/8#C18) 中，我们对一个表示为单行向量的 4 x 5 矩阵的维数进行了变换。默认情况下，行优先顺序连续打印行的元素。变换视图中的每个行元素都以与原始张量中相同的顺序出现。

此外，新张量的形状必须支持原始张量中相同数量的元素。您不能在 5 x 3 视图中存储 4 x 5 形状的张量。

对于 [*例 2.2*](https://jovian.ml/aman-a-agarwal/01-tensor-operations/v/8#C20) ，我们将用-1 来表示一个维度。PyTorch 自动从其他维度解释未知维度。变换张量在大小和步幅上与原始张量兼容。

示例 2.3 —不正确的形状

> 请注意，一次只能推断一个维度。对多个维度使用-1 只会引入歧义和运行时错误！

PyTorch 只允许-1 作为最多 1 维的可接受值。如果不止一个维度作为-1 传递，[它将抛出一个运行时错误](https://jovian.ml/aman-a-agarwal/01-tensor-operations/v/8#C24)。

# 函数 3 — torch.set_printoptions()

很多时候，你想在执行某些任务之前打印出张量的内容。为此，在笔记本中打印张量时，您可能需要更改显示模式。

> 使用 set_printoptions，您可以调整精度级别、线宽、结果阈值等属性。

在我们的例子中，我们将采用一个代表 20 x 30 矩阵的张量。通常不需要在笔记本中表示如此庞大的矩阵。打印张量变量背后的一个常见用例是查看前几行和最后几行。

示例 3.1(a)-使用默认打印选项打印

> 我们将根据自己的喜好，利用阈值、边项和线宽属性来改变张量的视觉表示。我们还可以使用 precision 属性改变小数点后显示的位数。

示例 3.1 (b) —更新了打印选项以显示更少的行

> 在该方法中，我们有三种可用的配置文件:默认、微小和完整。将配置文件名与其他属性一起传递是不正确的用法。在这种情况下，该函数会忽略 profile 属性。

示例 3.2

# 函数 4 — Tensor.backward()

张量用于简化机器学习管道中所需的常见任务。为了执行梯度下降(一种流行的损失最小化技术),我们需要计算损失函数的梯度(召回导数)。

> PyTorch 使用 backward()方法简化了这一过程，它在每次方法调用时存储渐变。注意:PyTorch 仅在其 require_grad 属性设置为 True 时计算张量的梯度。

示例 4.1

我们会用线性方程 *y = mx + c* 来求 y w.r.t 对方程中每个变量的偏导数。

示例 4.1 (a)

在调用 y.backward()方法并打印出计算出的梯度后，我们可以访问。张量 x 的梯度性质。

示例 4.1 (b)

因为我们没有将 require_grad 选项设置为 True，所以我们在调用时不会得到结果。m 的 grad 属性。

再次调用 y.backward()将导致 y . w . r . t 张量的二阶微分。注意 PyTorch 累积存储梯度。

# 函数 5 — torch.linspace()

linspace()方法返回一个包含一系列数字的一维张量。与随机生成数字的 rand()函数不同，返回的数字是 linspace()中等差数列的成员。

> 每个成员的差异由 steps 属性和范围(end-start)指定。

示例 5.1

输出张量包含 50 个 1-10 范围内的等距数字。 ***dtype*** 属性是 ***int*** 所以不存储小数位。

例 5.2—*out*属性也可用于指定张量来存储方法的结果。

请注意，在使用 linspace()方法创建张量时， ***dtype*** 值必须与输出张量定义的 ***dtype*** 一致。

示例 5.3-dtype 属性不匹配

## 结束语

本文介绍了 PyTorch API 中的一些基本方法，帮助您开始使用它。由于大部分实现都是从 NumPy 库中借用来的，以 Python 开发人员现有的理解和经验为基础，因此该 API 很容易上手。

通读这些功能后的下一步是浏览[官方文档](https://pytorch.org/docs/stable/tensors.html)。由于 PyTorch 是一个深度学习库，所以强烈建议您在开始使用这些函数之前先学习 ML 基础知识。

> 就这样，你坚持到了我 PyTorch 系列的第一篇博客的结尾！

## 参考

PyTorch 官方文件[https://pytorch.org/docs/stable/torch.html](https://pytorch.org/docs/stable/torch.html)

如果你喜欢这篇文章，并希望在未来阅读我的更多内容，你可以在这里以及在 [LinkedIn](http://www.linkedin.com/in/aman-agarwal-0512) 和 [Twitter](https://twitter.com/amanagarwal2) 上关注我。此外，请在评论中提出你的建议，告诉我在这一页上还能覆盖哪些功能。

![](img/a5fc029e985cc8ab381e21b8bb8ffec5.png)

照片由[特伦特·欧文](https://unsplash.com/@tjerwin?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

## 在 LinkedIn[和 Twitter](http://www.linkedin.com/in/aman-agarwal-0512)[上关注我，获取关于数据科学、机器学习和数据结构的内容。](https://twitter.com/amanagarwal2)