# 初学者应该知道的 4 个隐藏的 Python 特性

> 原文：<https://towardsdatascience.com/4-hidden-python-features-that-beginners-should-know-ec9de65ff9f8?source=collection_archive---------4----------------------->

## Python 初学者

## 如何轻松增强您的 Python 代码

![](img/1ec2d98569705c2e99e24361c0e2567f.png)

本·怀特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

学习是永无止境的。你将永远学不完如何用 Python 编程。你可能会**不断发现**Python 中有用的新特性，你可能会**被需要学习的内容**淹没。

不存在一本书或一个网站包含你需要了解的关于 Python 的一切。甚至官方文件也没有。Python 的可能性基本上是**无限**。

我用 Python 编码已经很多年了，并且发现了一些有趣的 Python 特性，这些特性非常独特。你在其他编程语言中找不到的东西。这里有 4 个**隐藏的** Python 特性，Python 初学者可能会觉得有用。

# 1.For-else 循环

你熟悉 Python 中的**条件语句**吗？

`while`循环，*打勾*。
`for`循环往复，*轻松*。
`if else`从句，*你把它钉在了*上。

`for else`条款呢？什么？

只有当 for 循环完成**而没有遇到** a `break`语句时，`else`块中的代码才会运行。如果你不知道什么是`break`，那么看看下面的文章了解更多信息。

[](https://medium.com/better-programming/how-to-use-pass-break-and-continue-in-python-6e0201fc032a) [## 如何在 Python 中使用传递、中断和继续

### Python 中退出子句的三种方法

medium.com](https://medium.com/better-programming/how-to-use-pass-break-and-continue-in-python-6e0201fc032a) 

那么这个 for-else 循环**怎么有用呢**？也许用户在`for _ in range(3):`循环中有 3 次尝试输入正确的**密码**，只有正确的密码激活`break`语句。`else`块包含连续 3 次输入错误密码的后果，例如锁定用户使用系统。

***令人惊讶的巨蟒。***

![](img/62e360d4fee1456952a60a3a8bf8d44e.png)

照片由 [Ali Gooya](https://unsplash.com/@aligooya?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 2.比较运算符链接

您希望检查`a`中的值是否在 0 到 100 的范围内，因此您将进行两次比较，并使用`and`将它们组合起来。`0 <= a`和`a <= 100`。这很简单。

但是你可以**链接**这些比较，并以一种更容易被人理解和更直观的方式`0 <= a <= 100`写出来。解释器会将它们分开，并读取类似`0 <= a and a <= 100`的比较操作。

```
a = 25
0 <= a <= 100       # True
0 <= a and a <= 100 # True
# The two expressions above are **equivalent** in Python
```

也可以把链条**做得更长**，比如`0 < a < 10 < b < 100`。

***直觉蟒。***

# 3.扩展切片

我们可以使用语法`a[start:stop:step]`和可选的第三个**步骤参数**来**分割**列表`a`。它读取**切片序列**的一部分，从`start`开始，到`stop`结束，步长为`step`。步长必须是整数，并且可以是负的。

```
a = list(range(10))
print(a[::2])   # [0, 2, 4, 6, 8]
print(a[3::-1]) # [3, 2, 1, 0]
```

我们可以通过将步骤参数设置为`-1`简单地通过`a[::-1]`来**反转列表**。如果没有指定，该步骤默认为`1`，这意味着没有元素**跳过**或列表**反转**。

***令人印象深刻的蟒蛇。***

![](img/6b27c99e96013dd9de2042b6d45c0f5f.png)

西蒙·马辛格在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 4.价值交换

Python 中如何**交换两个对象**的值？轻松点。你只需创建一个**临时**对象`temp`，就像你在其他语言中做的那样。

```
# Standard way to swap values of two objects in other languages
temp = a
a = b
b = temp
```

但这不是很**可读性**也远非**优雅**。但是您实际上可以使用 Python 中的一行简单代码轻松地交换这些值。

```
# Standard Python way to swap values
b, a = a, b
```

为什么有效？解释器首先评估右侧，并在**内存**中创建一个**元组** `(a,b)`。然后对左侧进行评估，其中元组中的两个元素分别被**解包**和**赋值**给`b`和`a`。它实质上交换了分配给`a`和`b`的对象。

***极小的蟒蛇。***

![](img/8f7aee5ea694953a80cc395d34b369a0.png)

[Jakub Dziubak](https://unsplash.com/@jckbck?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 相关文章

感谢您的阅读！你觉得那些功能有趣有用吗？下面留言评论！你也可以[注册我的时事通讯](http://edenau.mailchimpsites.com/)来接收我的新文章的更新。如果您对提高 Python 技能感兴趣，以下文章可能会有所帮助:

[](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4) [## 我希望我能早点知道的 5 个 Python 特性

### 超越 lambda、map 和 filter 的 Python 技巧

towardsdatascience.com](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4) [](/6-new-features-in-python-3-8-for-python-newbies-dc2e7b804acc) [## Python 3.8 中针对 Python 新手的 6 项新特性

### 请做好准备，因为 Python 2 不再受支持

towardsdatascience.com](/6-new-features-in-python-3-8-for-python-newbies-dc2e7b804acc) [](/4-common-mistakes-python-beginners-should-avoid-89bcebd2c628) [## Python 初学者应该避免的 4 个常见错误

### 我很艰难地学会了，但你不需要

towardsdatascience.com](/4-common-mistakes-python-beginners-should-avoid-89bcebd2c628) 

*原载于*[*edenau . github . io*](https://edenau.github.io/)*。*