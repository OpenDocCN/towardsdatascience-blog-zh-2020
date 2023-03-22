# 用数学表达式解析器实现松散耦合

> 原文：<https://towardsdatascience.com/achieving-loosely-coupling-with-a-math-expression-parser-4abed38ff00e?source=collection_archive---------65----------------------->

## 使用 mxParser 将数据计算逻辑定义与其执行位置分离开来

![](img/8d863e1a6e742051bce620221cc8889d.png)

杰斯温·托马斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

通过与数据科学家合作，我了解到一个数据处理系统需要改进多少次才能达到最佳效果。这就是为什么**你不应该在代码**中硬编码数学公式、表达式和常数。如果计算数据逻辑不是由数据科学家编写的，则尤其如此。

在我从事上一个项目时，我一直在寻找一种方法，允许我团队中的数据科学家改变数据计算逻辑，而不必强迫我更新 Kotlin 后端的代码。解决方案是**允许他们在数据库**(或配置文件)**中定义复杂的数学公式、表达式和常数，我将从中读取并应用它们**。首先，这可能吗？又是怎么做到的？

可能有很多方法可以达到这样的目标，但是为什么不使用一个**数学表达式解析器呢？**

在网上搜索一个好的库，我碰到了 [mXparser](http://mathparser.org/) 。

> **mXparser** 是一个**超级简单**、**丰富**、**快速**和**高度灵活的数学表达式解析器**库(解析器和**评估器**提供的数学表达式/公式为**纯文本/字符串**)。软件为 JAVA、Android 和 C#提供易于使用的 API。NET/MONO(符合公共语言规范:F#、Visual Basic、C++/CLI)。— [mxParser](http://mathparser.org/)

有了这个库，如果数据科学家编写遵循特殊 mxParser 符号的数学表达式、函数、常数，我可以加载并运行它们。

```
// few examples of string equivalent to the math formula notation:"1+2""1+2/3+(4*5-10)""2*x+sin(y)""int( sqrt(1-x^2), x, -1, 1)""sum(n, 1, 100, 1/n^2)""C(n,k)""fib(n) = iff( n>1, fib(n-1)+fib(n-2); n=1, 1; n=0, 0)"
```

多亏了这个强大的工具，我能够**将数据计算逻辑定义的地方与它实际执行的地方**分开，使得架构**松散耦合**。这似乎是一个微不足道的成就，但它使更新数据计算逻辑的过程更加高效和可靠。事实上，我团队中的数据科学家现在可以自由地编写数学表达式，**不再需要相信我能正确地实现它们**。

可以使用 mxParser 定义简单的[表达式](http://mathparser.org/mxparser-tutorial/simple-expressions/)、[函数](http://mathparser.org/mxparser-tutorial/user-defined-functions/) s(甚至[递归](http://mathparser.org/mxparser-tutorial/user-defined-recursion-not-limited/))。它还提供了几个[特性](http://mathparser.org/)，以及大量的[内置](http://mathparser.org/mxparser-math-collection/) [运算符](http://mathparser.org/mxparser-math-collection/operators/)、[常量](http://mathparser.org/mxparser-math-collection/constants/)和数学函数。

访问[**math parser . org-MX parser**](https://github.com/mariuszgromada/MathParser.org-mXparser)**GitHub 账号进行进一步阅读:**

**[](https://github.com/mariuszgromada/MathParser.org-mXparser) [## mariuszgromada/math parser . org-MX parser

### 数学解析器 Java Android C#。NET/MONO(。NET 框架，。网芯，。NET 标准，。净 PCL，Xamarin。安卓系统…

github.com](https://github.com/mariuszgromada/MathParser.org-mXparser)** 

**感谢阅读！我希望这篇文章对你有所帮助。如有任何问题、意见或建议，请随时联系我。**