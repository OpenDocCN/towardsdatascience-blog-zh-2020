# 每天 10 分钟学习 Python # 4

> 原文：<https://towardsdatascience.com/learning-python-10-minutes-a-day-4-df06c3c2e6e8?source=collection_archive---------56----------------------->

![](img/03dc2863429db24ccb060a0d15008706.png)

[杰瑞米·拉帕克](https://unsplash.com/@jeremy_justin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的原始照片。

## [每天 10 分钟 Python 速成班](https://towardsdatascience.com/tagged/10minutespython)

## 数字变量以及如何在 Python 中给它们赋值

这是一个[系列](https://python-10-minutes-a-day.rocks)的 10 分钟 Python 短文，帮助你开始学习 Python。我试着每天发一篇文章(没有承诺)，从最基础的开始，到更复杂的习惯用法。如果您想了解关于 Python 特定主题的问题或请求，请随时通过 LinkedIn[联系我。](https://www.linkedin.com/in/dennisbakhuis/)

在这节课中，我们将讨论变量赋值和数字。正如您在之前的几次会议中所记得的，Python 是动态类型化的。这意味着您不必将类型赋给变量，而是在赋值时自动分配类型。如果我们指定一个没有点的数字，它将被指定为类型 *int* (整数)，如果它有点或小数，它将被指定为类型 *float* (浮点):

动态类型的一个好处是，一般来说，你不必担心类型转换。如果需要，该值将被转换为类型 *float* 。与 Python 中的几乎所有类型一样，类型 *int* 和 *float* 是不可变的，并且在创建后不能更改。当进行基本算术运算时，输入值用于创建新的实例，即新的 int 或 float 值。如果这替换了对前一个结果的引用(变量),垃圾收集将删除该对象。

当然，显式地改变类型是可能的。例如，要将 float 转换为 int，可以使用 int()对它们进行强制转换。但是，此操作将截断所有小数。如果该值为 4.999，int(4.999)将返回 4。如果要四舍五入，可以使用 builtin round()函数。对于向上舍入( *ceil* )和向下舍入( *floor* )，您可以从标准库中导入它们(在后面的会话中将详细介绍导入)。使用截断或舍入时要小心。1982 年，范库弗峰证券交易所意外地使用了[截断而不是](https://en.wikipedia.org/wiki/Vancouver_Stock_Exchange)舍入，因此，神秘地降低了他们的指数(钱正在消失)。这种类型转换也用于将文本转换为值，但是要小心，因为字符串应该是有效的数字。

我们刚刚看到了内置方法 round，还有几个，即整数除法、绝对值、模数和复数。这些不常用，但值得一提。

使用变量时，需要先对变量赋值，然后才能使用它们。这可能不足为奇。令人惊讶的是你分配变量的方式。您可以将多个赋值链接起来，也可以使用一种叫做元组解包的技术。当我们讨论元组时，后一个术语将更有意义。下面的例子展示了元组解包，这可能不难理解。

Python 中的 int 值是不受限制的，也就是说，你可以在一个 int 变量中存储任意大的值，它只受系统内存大小的限制。这与许多其他语言不同，在这些语言中，如果不使用额外的包，您只能使用 64 位。float 类似于 double 的 C 实现，它是一个 64 位浮点变量。限制相对较大，但如果您需要更大的数字或更高的精度，您可以使用标准库中的[十进制](https://docs.python.org/3/library/decimal.html#decimal.Decimal)包。float 类型也有特例，它们本身就是有效的 float。比如可以将 *infinity* 或者 *-infinity* 定义为 float(*a = float(' infinity ')*)。另一种特殊情况是 nan，它代表“不是一个数”，当一个数没有被定义时(如果没有出现异常)，它被赋值。NaNs 也常用来表示缺失的数字。将来，我们会花一些时间处理 nan，因为有时会很棘手。这些特殊情况是通过在内置的 float 类中放入一个字符串(不区分大小写)来调用的。

## 今天的练习:

1.  在新的笔记本中，分配各种 int 和 float 值。
2.  使用新变量进行一些计算。
3.  测试所有讨论过的舍入策略。
4.  算“1.2 - 1”。解释为什么这不是一个错误。
5.  你觉得 float('nan') == float('nan ')等于 False 很奇怪吗？

如有任何问题，欢迎通过 [LinkedIn](https://www.linkedin.com/in/dennisbakhuis/) 联系我。