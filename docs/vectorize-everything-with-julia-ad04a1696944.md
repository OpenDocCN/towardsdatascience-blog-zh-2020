# 向量化朱莉娅的一切

> 原文：<https://towardsdatascience.com/vectorize-everything-with-julia-ad04a1696944?source=collection_archive---------21----------------------->

## 告别 for loops，广播所有的东西

你有没有过这样的感觉:for loops 正在接管你的生活，你无法逃离它们？你是否觉得被这些循环困住了？别害怕！有出路！我将向你展示如何在没有任何 for 循环的情况下完成 FizzBuzz 挑战**。**

向量化所有的东西！— [来源](https://giphy.com/gifs/dancing-despicable-me-J2vqNbXJkGrx6)

> FizzBuzz 的任务是打印 100 以内的每一个数字，但是要把能被 3 整除的数字换成“Fizz”，能被 5 整除的数字换成“Buzz”，能被 3 和 5 都整除的数字要换成“FizzBuzz”。

用 for 循环解决 [FizzBuzz 很容易](/learning-julia-the-simplest-of-beginnings-464f590e5665)，你甚至可以在 BigQuery 中做这件事[。在这里，我将向您展示另一种实现方式——没有任何 for 循环。解决方案是**矢量化函数**。](/loops-in-bigquery-db137e128d2d)

如果你已经有了一些 R 和 Python 的经验，你可能已经在标准 R 中或者通过 Python 的`numpy`库遇到了矢量化函数。让我们看看如何在 Julia 中类似地使用它们。

> 矢量化函数非常有用，因为它们减少了与 for 循环相关的混乱。

# 矢量化函数的第一步

在我们开始解决 FizzBuzz 之前，让我们看看如何在 Julia 中用矢量化替代方法替换非常简单的 for 循环。

让我们从一个琐碎的任务开始:*给定一个向量* `*a*` *给它的每个元素加 1。*

## 对于循环版本:

```
julia> print(a)[2, 3, 4]
```

## 矢量化版本:

上面的代码完成了工作，但是它占用了 3 行代码和很多多余的字符。如果`a`是 Python 中的一个`numpy`数组🐍，你只需要做`a + 1`，工作就完成了。但是首先，您必须将您的普通旧数组转换成一个`numpy`数组。

朱莉娅有一个聪明的解决办法。您可以使用广播操作符`.`对一个对象的所有元素应用一个操作——在本例中是加法。这就是它的作用:

这与上面的 for 循环给出了相同的答案。并且不需要转换你的数组。

[来源](https://giphy.com/gifs/LxPsfUhFxwRRC)

更棒的是，**你可以播放任何你喜欢的功能**，甚至是你自己的。在这里，我们计算一个圆的面积，然后在我们的阵列中传播它:

是的，pi 是 Julia 中的内置常量！

```
julia> area_of_circle.(a)3-element Array{Float64,1}:
  3.141592653589793
 12.566370614359172
 28.274333882308138
```

# 循环再见

![](img/cc309bb3008a116e473c716598442473.png)

回环拜拜！— [来源](https://www.pxfuel.com/en/free-photo-jalfs)

现在我们知道了基本知识，让我们做 FizzBuzz 吧！但是记住，不允许 for 循环。

我们将稍微重新表述一下我们的问题。代替打印数字、嘶嘶声和嗡嗡声，我们将把它们作为一个向量一起返回。我将以与 for 循环文章[链接]中相同的方式分解解决方案，因此如果您还没有看到以前的帖子，现在将是查看它的好时机！

首先，让**返回数字**直到`n`作为一个向量:

在这里，collect 只是接受我们的 range 操作符，并对它进行数组求值。

```
julia> fizzbuzz(5)5-element Array{Int64,1}:
 1
 2
 3
 4
 5
```

## 添加气泡

这个管用。让我们看看能否打印出每个能被 3 整除的数字的嘶嘶声。我们可以用 Fizz 字符串替换所有能被 3 整除的数字。

```
julia> fizzbuzz(7)7-element Array{String,1}:
 "1"
 "2"
 "Fizz"
 "4"
 "5"
 "Fizz"
 "7"
```

让我们一步一步地分解它:

*   为什么我们把所有东西都换成了`string`？这个数组就是一个数组。我们不希望数字和字符串混杂在一个对象中。
*   我们广播`rem.(numbers, 3`来寻找所有数字的余数**。**
*   然后我们将这个余数数组**与 0** ( `.== 0`)进行元素比较。
*   最后，我们**用布尔掩码**索引我们的字符串数组，并将“Fizz”赋给掩码为`true`的每个元素。

> 请随意分解这些步骤，在你自己的朱莉娅·REPL 身上试试吧！

我知道使用`.=`将单个元素分配给多个元素可能有点争议，但我实际上很喜欢它。通过**显式地指定赋值**的广播，你强迫自己去思考这些对象的区别，然后每个阅读你的代码的人**将会看到一个是向量，另一个是标量**。

## 添加蜂鸣器

添加蜂鸣器的方法完全相同:

对此不予置评。😃

## 将这一切结合在一起

我们所缺少的是能被 3 和 5 整除的数字的嘶嘶声元素。

我们使用`.*`来“乘”两个布尔数组。在这种情况下，您可以将*视为布尔值`and`。所以这个**给了我们一个真值，其中的数字可以被 3 和 5 整除。**

让我们看看这一切是否行得通:

```
julia> fizzbuzz(16)16-element Array{String,1}:
 "1"
 "2"
 "Fizz"
 "4"
 "Buzz"
 "Fizz"
 "7"
 "8"
 "Fizz"
 "Buzz"
 "11"
 "Fizz"
 "13"
 "14"
 "FizzBuzz"
 "16"
```

# 该打扫了

![](img/bd56f493135d71da4a1b27fb8ec89c83.png)

稍微无关，但请永远清理你的狗了！—照片由[哈里·格劳特](https://unsplash.com/@harryjamesgrout?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/tidy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

以上当然是我们想要的，但它并不是最漂亮的。是时候整理和**让我们的代码变得漂亮和像样了**。

这与上面的实现完全一样，但是它更容易阅读。我们找到想要替换的索引，然后在需要的地方替换数组元素。我们还使用了`%`而不是`rem`函数，因为**如果你的函数有一个操作符，那么你应该使用它！**

# 结论

读完这篇文章后，你现在知道如何使用`.`在 Julia 中传播任何函数，并通过删除一些不必要的 for 循环使你的代码更具可读性。

> 要获得所有媒体文章的完整访问权限，包括我的文章，请考虑在此订阅。

[](/learning-julia-the-simplest-of-beginnings-464f590e5665) [## 学习朱莉娅——最简单的开始

### 如何使用 FizzBuzz 执行循环和条件函数

towardsdatascience.com](/learning-julia-the-simplest-of-beginnings-464f590e5665) [](/loops-in-bigquery-db137e128d2d) [## BigQuery 中的循环

### 了解如何使用 BigQuery 脚本来计算斐波那契数

towardsdatascience.com](/loops-in-bigquery-db137e128d2d)