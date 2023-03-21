# Python 3.8 中针对 Python 新手的 6 项新特性

> 原文：<https://towardsdatascience.com/6-new-features-in-python-3-8-for-python-newbies-dc2e7b804acc?source=collection_archive---------2----------------------->

## Python 初学者

## 请做好准备，因为 Python 2 不再受支持

![](img/b75bd9e648f78dc53d167df0dd2076bd.png)

照片由[普里西拉·杜·普里兹](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

语言会变。语言会适应。21 世纪 20 年代不再支持 Python 2。

鉴于数据科学的兴起，Python 是 2019 年最受欢迎的编程语言。然而，对于要学的东西太多，感到有点不知所措是正常的。**语法**不断变化。每次更新都会添加许多新形式的**表达式**。很难跟踪 Python 中的变化。有些特征我希望我能早点知道。

[](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4) [## 我希望我能早点知道的 5 个 Python 特性

### 超越 lambda、map 和 filter 的 Python 技巧

towardsdatascience.com](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4) 

如果你也有同样的感觉，那么对你来说是个坏消息。Python 3.8 最近发布了。不要害怕。我总结了 Python 3.8 的 6 个新特性，每个 Python 初学者都应该学习。

# 1.赋值表达式—可读性

也被称为 **walrus 操作符**，它是一个新的操作符，语法`:=`允许你**给变量** **赋值，作为一个更大的表达式**的一部分。这可以说是 Python 3.8 中讨论最多的新特性。这里来一个例子。

赋值表达式`b := a**2`将变量`b`赋值为`a`的平方，在本例中为 36，然后检查`b`的值是否大于 0。

赋值表达式有时可以让你的代码更加紧凑，并且**可读性更好。注意不要滥用它，因为在某些情况下，它可能会使您的代码比必要的更难理解。**

```
# **DON'T DO THIS!**
a = 5
d = [b := a+1, a := b-1, a := a*2]
```

![](img/3e5456abf1488bff1bc0aec8373c5de2.png)

照片由[杰伊·鲁泽斯基](https://unsplash.com/@wolsenburg?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

这个操作符已经存在于其他(更老的)编程语言中，我希望许多转到 Python 的程序员会(ab)使用这个新特性。在它无处不在之前熟悉它。这是一只海象。

# 2.参数类型—鲁棒性

Python 函数中的参数可以接受两种类型的参数。

*   **按位置传递的位置**参数
*   **关键字**由关键字提供的参数

在下面的例子中，参数`a`和`b`的值可以由位置或关键字参数提供。灵活。

```
def my_func(a, b=1):
    return a+bmy_func(5,2)     # both positional arguments
my_func(a=5,b=2) # both keyword arguments
```

Python 的新版本提供了一种方法，通过使用语法`/`和`*`进行**分隔**，来指定只能接受位置参数或关键字参数的参数。
*后一种语法`*`在 Python 3.8 中并不新鲜

在下面的例子中，前两个参数`a`和`b`是仅位置的，中间两个`c`和`d`可以是位置的或关键字的，最后两个`e`和`f`是仅关键字的。

![](img/f0c2c0d5338d55a1821b497735489ab2.png)

由 [Austin Distel](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

你为什么决定牺牲灵活性？当参数名无用或任意时，应该排除关键字参数。如果**函数中的参数名在将来****被更改，也可以避免破坏您的代码。它有助于更健壮的代码。**

# **3.f-string 2.0 —调试**

**Python **f-string** 是游戏改变者。这是一个可读的优雅的**字符串格式化语法**，它在字符串中嵌入了表达式。这是通过语法`f'{expr}'`完成的，其中表达式被 f 字符串中的花括号括起来，引号前有一个`f`。**

**新的更新支持使用**等号** `=`作为语法为`f'{expr=}'`的 f 字符串内表达式的**格式说明符**。输出字符串将包括变量名及其值，在`=`之间有一个等号，如下所示。**

**出于文档或调试的目的，我们经常想要打印出变量值。这使得用**最小的**努力**调试**变得容易。**

## **4.可逆字典—顺序**

**字典现在可以在**中重复，使用`[reversed()](https://docs.python.org/3/library/functions.html#reversed)`反转**插入顺序。**

## **5.新模块—元数据**

**有一个新的`importlib.metadata`模块允许你**从第三方包**中读取元数据。您可以在您的脚本中提取包的**版本号**。**

## **6.继续——终于**

**由于实现的问题，在`finally`子句中使用`continue`语句曾经是**非法的**。不再是了。**

**![](img/406f2a872815745e817946ed5ddbe890.png)**

**汉娜·雅各布森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片**

# **外卖**

**注意，我没有提到一些高级特性，这些特性与普通程序员如何为小项目编写代码没有什么关系。包括*多处理共享内存*、*新 Pickle 协议*等。对感兴趣的人来说。**

**这就是 6 个新的 Python 特性，即使是 Python 初学者也能从中受益。在进入 Python 3.8 之前，请确保您熟悉一些基本的 Python 特性。你可以[注册我的时事通讯](http://edenau.mailchimpsites.com/)来接收我的新文章的更新。如果您对 Python 感兴趣，以下文章可能会有用:**

**[](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4) [## 我希望我能早点知道的 5 个 Python 特性

### 超越 lambda、map 和 filter 的 Python 技巧

towardsdatascience.com](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4) [](/4-numpy-tricks-every-python-beginner-should-learn-bdb41febc2f2) [## 每个 Python 初学者都应该学习的 4 个 NumPy 技巧

### 编写可读代码的技巧

towardsdatascience.com](/4-numpy-tricks-every-python-beginner-should-learn-bdb41febc2f2) 

*今天就开始用 Python 3.8 编码吧！***