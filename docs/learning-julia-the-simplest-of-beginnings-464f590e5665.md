# 学习朱莉娅——最简单的开始

> 原文：<https://towardsdatascience.com/learning-julia-the-simplest-of-beginnings-464f590e5665?source=collection_archive---------51----------------------->

![](img/0fb8908bc395df1454a48b04dc7d05e9.png)

亚历山大·杜默在 [Unsplash](https://unsplash.com/s/photos/first-steps?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 如何使用 FizzBuzz 执行循环和条件函数

在本帖中，我们将创建一个函数来解决过于流行的 FizzBuzz 编程挑战。

看完这篇文章，你会知道:

*   如何在 Julia 中创建函数
*   如何执行 for 循环
*   如何创建 if-else 块
*   1:5 意味着什么
*   如何计算一个数被除后的余数

> 这篇文章是为初级程序员或者那些以前从未听说过 Julia 的人写的。看完这篇文章后，不要指望做[惊天动地的大规模并行科学工作负载](https://www.nextplatform.com/2017/11/28/julia-language-delivers-petascale-hpc-performance/)。就当这是你在茱莉亚这个令人敬畏的世界里卑微的开始。

# FizzBuzz 是什么？

如果你从未听说过它， **FizzBuzz 是一个简单的计数游戏**，旨在教孩子们**乘法表**。参与者围成一圈，有人从 1 开始计数。下一个人继续 2 等，绕圈。诀窍在于，如果这个数能被 3 整除，那么玩家应该说 *Fizz* 而不是说这个数。如果这个数字能被 5 整除，那么他们应该说 *Buzz* 。如果这个数字能被 3 和 5 整除，那么他们应该说 *FizzBuzz* 。

我猜你可以用任何其他 2 个数字玩这个游戏，但是 3 和 5 是最受欢迎的选择。

这个简单的游戏已经被翻译成一个折磨初学者的编程挑战。这也给了我们一个在 Julia 中练习循环和 if 语句的好机会。让我们看看如何创建一个函数来打印前 n 个数的解。

> 我之前已经在 BigQuery 中解决了 FizzBuzz，所以如果你有兴趣知道如何使用 PB 级数据仓库解决这个问题，请在这里查看我的[文章](/fizzbuzz-in-bigquery-e0c4fbc1d195)。

# 一步一步来

![](img/8951ae5a3d60e82aaa5a6548c38eebe9.png)

照片由[大卫·布鲁克·马丁](https://unsplash.com/@dbmartin00?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/baby-steps?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

让我们一步一步来，看看如何解决这个问题，而不是简单地讨论这个问题。

## 我们在做什么？

我发现对这个功能应该实现什么有一个清晰的概念是非常重要的。在我们的例子中，我们希望打印第一个`n`数字，但是用 Fizz 替换能被 3 整除的数字，用 Buzz 替换能被 5 整除的数字，用 FizzBuzz 替换能被 3 和 5 整除的数字(或者用 Fizz Buzz 替换能被 3 和 5 整除的数字)。

## 例子？

有一个关于我们想要达到的目标的想法是很好的，但是有时候通过例子来理解一个任务更容易。对于`n=3`或`n=6`，我们的函数会输出什么？写下例子将有助于我们找到解决方案，还能让我们检验我们的结果。

因此对于`n=3`,我们期望得到:

```
1
2
Fizz
```

对于 n=6，我们也希望出现“嗡嗡声”:

```
1
2
Fizz
4
Buzz
Fizz
```

同样，当我们达到 15 岁时，我们应该说“嘶嘶”。我们也不要忘记这一点！

# 你的第一个朱莉娅函数

让我们现在开始编码。既然我们正在一个接一个地打印数字，让我们**做一个 for 循环**。首先，让我们只打印前 n 个数字，然后我们可以稍后将它们更改为嘶嘶声和嗡嗡声。

这是你的第一个 Julia 函数。让我们把它分解一下，看看它有什么作用:

*   **功能**以`function`关键字开始，以`end`结束。中间的一切都是你的功能块。该函数有一个输入`n`并且不返回任何内容，因为我们只打印数字。
*   说到打印，我们用的是`println`，内置的 Julia 函数，代表 print line。这个**打印给定的值**并开始新的一行，所以我们所有的数字将整齐地打印在彼此下面。
*   我们这里还有一个 for 循环。这以关键字`for`开始，以`end`结束。我们正在循环的数字是`i`，所以在每次迭代中，`i`的值将在 for 循环中改变。
*   `i`会有怎样的变化？这由`1:n`位给出，即**定义了从 1 到 n 的范围**，每一步递增 1。要了解更多信息，请在此处搜索*使用范围*对象[创建数组。](https://en.wikibooks.org/wiki/Introducing_Julia/Arrays_and_tuples)

显然，这个函数不会给出我们需要的东西，但至少我们有一些基本的函数。让我们来测试一下:

```
julia> fizzbuzz(5)1
2
3
4
5
```

# 分步解决

![](img/791b29c5f407cf0017299f1b63973c74.png)

迈克尔·卡鲁斯在 [Unsplash](https://unsplash.com/s/photos/divide?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

让我们修改上面的内容，这样我们就可以为所有能被 3 整除的数字打印出 *Fizz* 。

我们如何做到这一点？我们可以检查，当除以 3 时，余数是否为 0。这叫做一个数的模。有一个函数可以做到这一点— `rem()`(或者您可以使用`%`运算符)！

以下是一些关于`rem`如何运作的例子:

```
rem(1,3) -> remainder of 1, when devided by 3 -> 1
rem(3,3) -> remainder of 3, when devided by 3 -> 0
rem(8,3) -> remainder of 8, when devided by 3 -> 2
```

修改我们的函数给我们:

更好看！所以现在，每当`i`除以 3 的余数为 0 时，我们打印“Fizz”而不是 I。

> Julia 中 if 块的条件部分必须始终计算为布尔值—真或假。

*如果你明白上面是怎么回事，你就知道 Julia 里的 if-else 块了！👏*

# 继续下一个

同样的逻辑也适用于能被 5 整除的数字。我们只需要把`rem`的第二个参数改成 5。

[来源](https://tenor.com/view/simples-jay-cartwright-the-inbetweeners-gif-9259576)

再测试一次，看看我们是否还可以:

```
julia> fizzbuzz(5)1
2
Fizz
4
Buzz
```

差不多了，现在我们有嘶嘶声和嗡嗡声了！最后一步是添加当数既可被 3 又可被 5 整除时的情况，即被 15 整除:

```
julia> fizzbuzz(15)1
2
Fizz
4
Buzz
Fizz
7
8
Fizz
Buzz
11
Fizz
13
14
Fizz
```

不对不对。15 岁还有“嘶嘶”声！这是因为 15 能被 3 整除，所以我们的`if-else`块只到达第一位，检查这个数是否能被 3 整除，然后愉快地终止，只打印“Fizz”。

我们必须确保先检查 FizzBuzz 案件，然后再做其他事情:

如果你不想被 15 整除，你也可以说:(rem(i，3) == 0) & (rem(i，5) == 0

```
julia> fizzbuzz(15)1
2
Fizz
4
Buzz
Fizz
7
8
Fizz
Buzz
11
Fizz
13
14
FizzBuzz
```

# 成功！🎉

通过阅读这篇关于 Julia 的介绍性文章，您了解到:

*   功能
*   If-else 块
*   对于循环
*   余数函数

你还解决了 FizzBuzz 挑战👏！

我真的很喜欢茱莉亚，所以希望将来能从我这里看到更多关于茱莉亚的内容。如果你对学习这种令人敬畏的语言感兴趣，一定要关注我！