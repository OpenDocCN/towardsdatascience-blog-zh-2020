# 循环，中断，继续与朱莉娅在一起

> 原文：<https://towardsdatascience.com/control-flow-basics-with-julia-c4c10abf4dc2?source=collection_archive---------80----------------------->

## Julia 的控制流基础知识

![](img/e3ce2bc9db022932f597560d4c882739.png)

学会随波逐流-图片由[迈克·刘易斯智慧媒体](https://unsplash.com/@mikeanywhere?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/flow?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上发布

让我们继续探索朱莉娅的基础知识。之前我谈到了循环的[和矢量化的](/learning-julia-the-simplest-of-beginnings-464f590e5665)和[。在这里，我们将讨论如何在 Julia 内部使用控制流操作符。](/vectorize-everything-with-julia-ad04a1696944)

# 什么是控制流操作符？

顾名思义，控制流操作符帮助我们塑造程序的流程。你可以**从一个函数返回**，你可以**从一个循环中中断**，你可以通过**继续**跳过一个循环的迭代。

# 简单的任务

为了理解这些概念，我们将尝试**解决一个问题**。没有比亲身体验更好的了，对吧？我们的挑战如下:

> 给定 2 个整数(a，b ),打印 a 和 b 之间最小的(最多)5 个整数，这样我们就不会打印能被 3 整除的数字。

例如如果`a=5`和`b=23`我们应该打印以下数字:

```
5
7
8
10
11
```

如果`a`和`b`彼此更接近，我们打印到`b`的所有内容。下面是另一个关于`a=2`和`b=4`的例子:

```
2
4
```

如果你完全是编程新手，你可能想看看我的 FizzBuzz 文章[链接],在那里我解释了 for 循环和 modulo 函数。

# 首先，我们打印

本着循序渐进的精神，让我们构建我们的函数:

这只是打印从`a`到`b`之间的所有数字。

# 然后，我们继续

[来源](https://giphy.com/gifs/snl-saturday-night-live-season-45-QYM0d2M7GTOApm99Zw)

我们的问题是，这也打印可被 3 整除的数字，这不是我们想要的。为了**在 for 循环**中跳过这些数字，我们可以使用`continue`。

所以无论何时`rem(i,3) == 0`计算为`true`，我们都将跳回循环的顶端，继续下一个`i`。这就是`continue`的作用。

让我们来测试一下:

```
julia> fancy_printer(3,10)4
5
7
8
10
```

厉害了，再也没有能被 3 整除的数字了！👏

# 最后，我们休息

![](img/ee23d4dd994d694b9e6f04ed3e01f4f0.png)

有时候，我们需要从循环中休息一下——张肯尼在 [Unsplash](https://unsplash.com/s/photos/break?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

以上照顾到了我们的一个问题，但是在`a`和`b`之间有超过 5 个不能被 3 整除的数的情况下，我们还是要处理。例如:

```
julia> fancy_printer(3,13)4
5
7
8
10
11
13
```

这意味着我们要小心的不是我们打印的内容，而是我们总共打印了多少次。为了处理这个问题，我们将引入另一个名为 printed 的变量，我们可以用它来计算我们打印的次数。如果这个值达到 5，我们就可以**用 break** 结束 for 循环，结束它。

让我们看看它是否有效🤞：

```
julia> fancy_printer(3,25)4
5
7
8
10
```

确实如此。但是现在所有这些 if 语句看起来有点难看…

# 越来越花哨

现在我们有了一个`fancy_printer`函数，看起来没那么花哨。**我来给大家介绍一下** [**短路操作工**](https://docs.julialang.org/en/v1/base/base/#?:) **。**短路运算符`&&`和`||`可以将中频模块压缩成一个单元。他们将一个**布尔表达式带到左侧**并将一个**动作带到右侧**。`&&`仅在布尔值为`true`时评估动作，而`||`仅在布尔值为`false`时动作。

> 使用短路操作符`&&`和`||`使你的代码更加简洁。

下面是这个更简洁的版本的样子:

%是余数运算符。i % 3 与 rem(i，3)相同

简洁紧凑的代码的确很酷！— [来源](https://giphy.com/gifs/thegoodplace-season-2-nbc-l3mZfxgPWhmuXa8Cc)

# 结论

现在你知道了:

*   如果您想在任何时候停止循环，请使用`break`。
*   为了跳过你的循环迭代，`continue`是你的朋友。
*   三元运算符`&&`和`||`可以让你的代码更小，可读性更好。

*这里还有一些关于茱莉亚的文章:*

[](/learning-julia-the-simplest-of-beginnings-464f590e5665) [## 学习朱莉娅——最简单的开始

### 如何使用 FizzBuzz 执行循环和条件函数

towardsdatascience.com](/learning-julia-the-simplest-of-beginnings-464f590e5665) [](/vectorize-everything-with-julia-ad04a1696944) [## 向量化朱莉娅的一切

### 告别 for loops，广播所有的东西

towardsdatascience.com](/vectorize-everything-with-julia-ad04a1696944)