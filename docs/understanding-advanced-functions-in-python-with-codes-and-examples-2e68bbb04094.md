# 用代码和例子理解 Python 中的高级函数！

> 原文：<https://towardsdatascience.com/understanding-advanced-functions-in-python-with-codes-and-examples-2e68bbb04094?source=collection_archive---------11----------------------->

## 通过代码和示例详细了解 python 中的匿名函数和高级函数及其实际实现。

![](img/bdf3ff53b821a6e71992d1d27651a77a.png)

萨法尔·萨法罗夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Python 是一种优雅的编程语言，它为用户提供了许多简化代码和缩短代码长度的工具。它是一种面向对象的高级编程语言，早在 1991 年就发布了，具有高度的可解释性和高效性。

我最初是从 C、C++和 Java 这样的语言开始的。当我终于遇到 python 的时候，我发现它相当优雅，简单易学，易于使用。对于任何人来说，Python 都是开始机器学习的最佳方式，甚至对于以前没有编程或编码语言经验的人也是如此。

由于其数据结构、条件循环和函数的简单性，它提供了广泛的通用性来处理各种问题。匿名函数和高级函数是我们今天要讨论的主题，但基本问题是——什么是函数？

**函数**是写在程序中的一段代码，这样它们可以被多次调用。函数的主要用途是在同一个程序中可以多次重复调用它，而不需要一遍又一遍地编写相同的代码。然而，你也可以用它来为你的程序提供更多的结构和更好的整体外观。

使用关键字 **'def，**定义函数，可以使用已定义或未定义的参数调用这些函数。当您调用特定的函数时，python 编译器会解释将要返回的任何值。

如果您的代码采用这个由几行代码组成的块，并且只用一两行代码就执行了它，会怎么样呢？

这个任务可以通过 python 中的**匿名函数**来执行。让我们在下一节更深入地理解这个概念。

![](img/fde583f288178bbd9b06d72f93a69675.png)

阿诺德·弗朗西斯卡在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 匿名函数:

在 Python 中，匿名函数是定义时没有名字的函数。在 Python 中，普通函数是使用 def 关键字定义的，而匿名函数是使用 lambda 关键字定义的。因此，匿名函数有时也被称为 lambda 函数，它们通常可以互换使用。

该函数的语法如下:

```
lambda arguments: expression
```

使用 lambda 函数的主要优点是执行一个 lambda 函数，该函数计算其表达式，然后自动返回其结果。所以总有一个隐式的 return 语句。这就是为什么有些人把 lambdas 称为单表达式函数。在大多数情况下，它对于简化代码非常有用，并且是编程语言不可或缺的一部分。

大多数情况下，lambda 函数是好的，但是如果使用这些函数会使单行代码比预期的长，并且用户很难阅读，那么就应该考虑不使用它们。基本上，当代码的可读性降低时，您应该考虑不使用 lambda 函数。

让我们了解如何准确地使用 lambda 函数中的代码来为我们造福。为了有一个清晰的概念理解，我将列出一些问题陈述，以及它们的带有普通函数和 lambda 函数的代码块解决方案。

## 问题陈述:

> 使用函数打印数字的平方。

## 具有定义功能的代码:

## 输出:

```
25
```

上面的代码块和输出代表了如何通过使用一个普通的 def 函数来计算问题语句来编写下面的代码。

## 带有 lambda 函数的代码:

## 输出:

```
25
```

在 lambda 函数的帮助下，同样的问题语句可以在两行代码内解决。这就是 lambda 函数简化问题陈述的方式。

Lambda 函数通常与其他函数一起使用，在几行代码而不是一段代码中解决复杂的任务。这些功能即过滤、映射和减少。让我们用单独的问题陈述来分别分析每一个问题。

![](img/b1677b0b13cd17e79c2cd35a4214b169.png)

照片由 [Cookie 在](https://unsplash.com/@cookiethepom?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 Pom 拍摄

## 1.过滤器:

顾名思义，filter()函数将创建一个新的列表，只存储满足特定条件的元素，并过滤掉剩余的值。

filter()操作接受一个函数对象和一个 iterable，并创建一个新列表。只要满足条件并返回 true，就会执行成功的计算。

## 问题陈述:

> 只打印元素列表的偶数。

## 具有定义功能的代码:

## 输出:

```
[2, 4]
```

在上面的代码块中，函数接受一个数字列表的输入。然后我们遍历列表，使用条件语句只存储给定列表中的偶数。该函数最终返回包含所有偶数的列表。

## 带有 lambda 函数的代码:

## 输出:

```
[2, 4]
```

在一行代码中，我们可以借助上面的代码从 iterable 列表中筛选出偶数。返回的输出与前面冗长的函数完全相同。

## 2.地图:

map()函数遍历给定 iterable 中的所有项，并对每个项执行我们作为参数传递的函数。这类似于 filter()函数。

它可以用来获得条件列表上的布尔值 true 和 false，或者只是将每个元素映射到其各自的输出。让我们用一些代码更详细地理解这一点。

## 问题陈述:

> 打印列表中每个元素的方块。

## 具有定义功能的代码:

## 输出:

```
[1, 4, 9, 16, 25]
```

在上面的代码块中，该函数接受一个类似于前一个问题的数字列表的输入。然后我们遍历列表，计算存储在列表中的每个元素的平方。该函数最后返回包含每个元素的所有平方数的列表。

## 带有 lambda 函数的代码:

## 输出:

```
[1, 4, 9, 16, 25]
```

map 函数在一行代码中计算并解决问题陈述。列表中的每个元素都映射有各自的方块。

## 3.减少:

与前两个函数(即 filter()和 map())不同，reduce 函数的工作方式略有不同。它遍历可迭代数字列表，并继续返回一个值。

这个函数在用简单代码而不是较长的代码块执行列表的加法或乘法等计算时非常有用。reduce()函数一次计算列表中的两个元素，直到完成整个 iterable。

## 问题陈述:

> 打印列表中所有元素的乘积。

## 具有定义功能的代码:

## 输出:

```
120
```

在上面的代码块中，该函数接受一个类似于前两个问题的数字列表输入。然后我们遍历列表，计算给定列表中所有数字的乘积。该函数最终返回评估列表后获得的最终产品的输出。

## 带有 lambda 函数的代码:

## 输出:

```
120
```

reduce()函数需要从 python 3 中的 functools 模块导入。上面的代码块一次执行两个元素的顺序乘法，直到完成整个列表的计算。

![](img/9dc54f69c57c201e5e7cb2075aa12927.png)

照片由[奎诺·阿尔](https://unsplash.com/@quinoal?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论:

在本文中，我们简要介绍了 python，并对函数在 Python 编程语言中的作用有了基本的了解。

然后，我们进一步讨论了 python 中的匿名函数，并通过一些代码和示例了解了其中可用的每个高级函数选项。

如果你对今天的话题有任何疑问，请在下面的评论中告诉我。我会尽快回复你。如果我错过了什么，请随时告诉我。您希望我在以后的文章中介绍这一点。

看看我的其他一些文章，你可能会喜欢读！

[](/do-you-really-need-a-gpu-for-deep-learning-d37c05023226) [## 深度学习真的需要 GPU 吗？

### 获得一个 GPU 是深度学习的必备条件吗？了解 GPU 及其优势，并探索…

towardsdatascience.com](/do-you-really-need-a-gpu-for-deep-learning-d37c05023226) [](/10-step-ultimate-guide-for-machine-learning-and-data-science-projects-ed61ae9aa301) [## 机器学习和数据科学项目的 10 步终极指南！

### 详细讨论构建您的机器学习和数据科学项目的最佳方法…

towardsdatascience.com](/10-step-ultimate-guide-for-machine-learning-and-data-science-projects-ed61ae9aa301) [](/step-by-step-guide-proportional-sampling-for-data-science-with-python-8b2871159ae6) [## 分步指南:使用 Python 进行数据科学的比例采样！

### 了解使用 python 进行数据科学所需的比例采样的概念和实现…

towardsdatascience.com](/step-by-step-guide-proportional-sampling-for-data-science-with-python-8b2871159ae6) [](/10-most-popular-programming-languages-for-2020-and-beyond-67c512eeea73) [## 2020 年及以后最受欢迎的 10 种编程语言

### 讨论当今 10 种最流行的编程语言的范围、优缺点

towardsdatascience.com](/10-most-popular-programming-languages-for-2020-and-beyond-67c512eeea73) [](/opencv-complete-beginners-guide-to-master-the-basics-of-computer-vision-with-code-4a1cd0c687f9) [## OpenCV:用代码掌握计算机视觉基础的完全初学者指南！

### 包含代码的教程，用于掌握计算机视觉的所有重要概念，以及如何使用 OpenCV 实现它们

towardsdatascience.com](/opencv-complete-beginners-guide-to-master-the-basics-of-computer-vision-with-code-4a1cd0c687f9) 

谢谢你们坚持到最后。我希望你们都喜欢这篇文章。祝大家有美好的一天！