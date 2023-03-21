# 每天 10 分钟学习 Python # 8

> 原文：<https://towardsdatascience.com/learning-python-10-minutes-a-day-8-5e835880f284?source=collection_archive---------58----------------------->

![](img/861a215f083567059b0616591c415d2f.png)

[杰瑞米·拉帕克](https://unsplash.com/@jeremy_justin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的原始照片。

## [每天 10 分钟 Python 速成班](https://towardsdatascience.com/tagged/10minutespython)

## Python 中的循环:暂时

这是一个[系列](https://python-10-minutes-a-day.rocks)10 分钟的简短 Python 文章，帮助您提高 Python 知识。我试着每天发一篇文章(没有承诺)，从最基础的开始，到更复杂的习惯用法。如果您对 Python 的特定主题有任何问题或要求，请随时通过 LinkedIn 联系我。

今天我们将讨论循环。有两种不同类型的循环，即 for 循环和 while 循环。两者在几乎任何编程语言中都被大量使用，当然这是有原因的。代码中经常出现重复。例如，您有一个值列表，需要过滤低于某个阈值的值。解决这个问题的一种方法是使用 for 循环遍历所有值，并测试该值是否低于设置的阈值。当然，还有其他方法来解决这个问题。

for 循环和 while 循环都有不同的用例。和前面的例子一样，for 循环用于迭代某些东西。这可以是一个简单的计数器(一个值列表)，一个字符串列表，实际上是放入 iterable 的任何对象。因此 for-loops 将遍历一个固定的列表。while 循环有不同的目的，因为它不遍历列表，但有一个在每次迭代前测试的条件。如果条件为真，它将执行代码块，当条件为假时，while 循环结束。while 循环用于继续迭代，直到例如用户按下按键或者最小化过程已经达到某个精度。

在许多其他语言中，创建 for 循环来拥有一个计数器是很常见的，然后用它来遍历一个数据结构。在 Python 中，你可以直接遍历一个可迭代对象，比如一个列表。让我们来看一些例子:

for 循环以关键字的*开始，后跟一个变量名。这个变量名将被用来引用列表的当前项。因为 Python 是动态类型化的，所以您不必为分配类型而烦恼。这将由 Python 解释器动态设置。变量名后面是关键字*中的*，后面是 iterable 和分号。与 if 语句类似，循环中的代码通过缩进来标识。缩进加上清晰的语义，使得这些循环可读性极强。*

Python 已经打包了一些方便的工具来操作列表。例如，要反转一个列表，可以使用内置的 reversed()方法。这些类型的方法接受一个 iterable(比如一个 list ),并通过应用的操作返回一个新的 iterable。其中一些方法比如 reversed()也内置在列表类本身中( *my_list.reverse()* )。不同之处在于这些方法改变了列表本身(因为列表是可变的)，并且不返回新的列表。

有时候，迭代的时候需要有一个计数器或者索引。对于这种特殊情况，Python 提供了 enumerate，它用连续增加的数字“压缩”你的列表。Zip 可以将多个列表组合成一个单独的 iterable，这个 iterable 可以在一个循环中解包。当然，所有列表的长度必须相同，否则会出现错误。

range()方法用于创建一个数字列表。它接受一个或两个边界值和一个步长。Range()返回一个特殊类型的 iterable，它是一个 range 对象([它不是一个生成器](https://treyhunner.com/2018/02/python-range-is-not-an-iterator/))。在前面的例子中，我们为 for 循环提供了一个列表。这个列表完全定义在内存中的某个地方，等待 for 循环引用。如果我们要对大范围的数字做同样的事情，我们首先需要创建这个列表。这可能会占用大量内存，尤其是当我们迭代数百万个值时( *range(1e12)* )。对于这种情况，Python 有生成器，这是一种“懒惰执行”的形式。仅当需要该值时，即当 for 循环请求该值时，才会生成该值。虽然 range 与 generators 有一些微妙的区别，但其思想有些类似:只在需要时获取您需要的值，而无需先创建完整的数字列表。

与 if 语句类似，for 循环也可以嵌套。当然，您需要确保两个 for 循环的变量是唯一的。第二个 for 循环从下一个缩进级别开始，并且随着每个 for 循环的增加而增加。虽然 Python 可以接受添加大量嵌套的 for 循环，但一般来说，三个是最大值。如果你最终使用了更多，是时候考虑减少循环次数的策略了。循环很慢，如果你需要大量的循环，矢量化可能是你想要的。

for 循环和 while 循环都有 *break* 和 *continue* 关键字来对循环流进行额外的控制。发出*中断*时，当前循环停止，即如果处于 for 循环，循环停止。如果您在嵌套的 for 循环中，则 for 循环的当前级别将停止。使用 *continue* 可以进入下一次迭代，忽略 *continue* 关键字之后的任何语句。这些关键字为流程提供了额外的控制，但是针对特定的用例。

我从未使用过 for-else 或 while-else 结构，但它可能有一些用例。然而，这很容易理解。如果 for 循环或 while 循环成功，即没有中断或错误，则执行 else 之后定义的代码块。

While-loops 是 Python 中创建循环的另一种构造。while 循环测试条件，而不是循环遍历 iterable。while 循环以关键字*开始，而*后面是条件和分号。

在基于 Python 的 restFul API 服务器中常见的一个构造是一个永无止境的 while 循环。这些循环会一直运行下去(或者直到用户中止程序(或者出现错误))，因为所提供的条件是常量 True。真，永远不会变成假，因为它是真中最真的，而只有通过 *break* 关键字或神的干预才能停止。while 循环中的条件类似于 if 语句中的条件。因此，您可以使用逻辑运算符( *and、not 或*)来组合多个。此外，如果您想稍后对循环进行编码，您可以使用 *pass* 占位符。

# 今天的练习:

**a.** 在一个新的笔记本中，创建一个 for-loop，对 1 到 100(包括 100)之间的所有值进行循环，并对所有值求和。(应该是 5050)

**b.** 循环下面的列表:

```
my_list = ['Alfred', 'Dennis', 'Rob', 'Coen', 'Coen', 'Alfred', 'Jeroen', 'Hendri', 'Alfred', 'Coen', 'Rob', 'Dennis', 'Rob', 'Dennis', 'Rob', 'Coen', 'Rob', 'Alfred', 'Jeroen', 'Hendri', 'Alfred', 'stop', 'Coen', 'Rob', 'Dennis', 'Dennis', 'Rob', 'Jeroen', 'Jeroen', 'Alfred', 'Jeroen', 'Hendri', 'Alfred', 'Coen', 'Rob', 'Dennis']
```

a)统计所有名字
b)统计所有名字，但使用*继续*关键字
跳过“Dennis ”; c)统计所有名字，但使用*中断*关键字在“stop”处停止

**c.** 使用 while 循环创建所有值的和，即 1 + 2 + 3 + … + n，其中 n 每一步增加 1。继续，直到总和大于 10000，并返回当前步骤 n。在 while 循环结束后，使用 *else* 构造打印该值。(应该是 142)

如果您有任何问题，欢迎通过 [LinkedIn](https://www.linkedin.com/in/dennisbakhuis/) 联系我。