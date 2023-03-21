# Python 生成器

> 原文：<https://towardsdatascience.com/python-generators-393455aa48a3?source=collection_archive---------27----------------------->

## 使用 yield 关键字开发 python 生成器函数的教程

![](img/bd23c2a4cd8a9991be0fe158d0617a1f.png)

图片来自[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4550711)

简单地说，Python 生成器促进了维护持久状态的功能。这使得增量计算和迭代成为可能。此外，生成器可以用来代替数组以节省内存。这是因为生成器不存储值，而是存储带有函数状态的计算逻辑，类似于一个准备启动的未赋值函数实例。

# 生成器表达式

生成器表达式可以用来代替数组创建操作。与数组不同，生成器将在运行时生成数字。

```
>>> import sys
>>> a = [x for x in range(1000000)]
>>> b = (x for x in range(1000000))
>>> sys.getsizeof(a)
8697472
>>> sys.getsizeof(b)
128
>>> a
[0, 1, ... 999999]
>>> b
<generator object <genexpr> at 0x1020de6d0>
```

我们可以看到，在上面的场景中，通过用生成器代替数组，我们节省了大量内存。

# 用收益代替回报的函数

让我们考虑一个简单的例子，你想产生任意数量的素数。下面是检查一个数是否是质数的函数，以及将产生无穷多个质数的生成器。

```
def isPrime(n):
    if n < 2 or n % 1 > 0:
        return False
    elif n == 2 or n == 3:
        return True
    for x in range(2, int(n**0.5) + 1):
        if n % x == 0:
            return False
    return Truedef getPrimes():
    value = 0
    while True:
        if isPrime(value):
            yield value
        value += 1
```

正如你在第二个函数中看到的，我们在一个 while 循环中迭代并产生质数。让我们看看如何使用上面的生成器。

```
primes = getPrimes()>>> next(primes)
2
>>> next(primes)
3
>>> next(primes)
5
```

首先，我们调用函数并获取生成器实例。虽然这可以模拟一个无限数组，但是还没有找到任何元素。如果你调用`list(primes)`，你的程序可能会因为内存错误而崩溃。然而，对于质数，它不会到达那里，因为质数空间对于在有限时间内达到内存限制的计算是稀疏的。但是，对于发电机，你不会事先知道长度。如果您调用`len(primes)`,您将得到下面的错误，原因与数字只在运行时生成的原因完全相同。

```
----------------------------------------------------------------
TypeError                      Traceback (most recent call last)
<ipython-input-33-a6773446b45c> in <module>
----> 1 len(primes)

TypeError: object of type 'generator' has no len()
```

# 迭代次数有限的生成器

虽然我们的质数例子有一个无限的迭代空间，但在大多数日常场景中，我们面对的是有限的计算。因此，让我们来看一个例子，我们可以用它来读取一个包含文本数据和下一行句子的语义分数的文件。

## 为什么我们需要使用收益率？

假设文件是 1TB，词的语料库是 500000。它不适合存储。一个简单的解决方案是一次读取两行，计算每行的单词字典，并在下一行返回语义得分。该文件如下所示。

```
The product is well packed
5
Edges of the packaging was damaged and print was faded.
3
Avoid this product. Never going to buy anything from ShopX.
1
Shipping took a very long time
2
```

很明显，我们不需要马上打开文件。此外，这些线条必须矢量化，并可能保存到另一个可以直接解析的文件中，以训练机器学习模型。因此，给我们一个干净代码的选项是使用一个生成器，它可以一次读取两行，并以元组的形式给我们数据和语义得分。

## 实现文件解析生成器

假设我们在一个名为`test.txt`的文件中有上述文本文档。我们将使用下面的生成器函数来读取文件。

```
def readData(path):
    with open(path) as f:
        sentiment = ""
        line = ""
        for n, d in enumerate(f):
            if n % 2 == 0:
                line = d.strip()
            else:
                sentiment = int(d.strip())
                yield line, sentiment
```

我们可以在一个`for`循环中使用上述函数，如下所示。

```
>>> data = readData("test.txt")
>>> for l, s in data: print(l, s)
The product is well packed 5
Edges of the packaging was damaged and print was faded. 3
Avoid this product. Never going to buy anything from ShopX. 1
Shipping took a very long time 2
```

## 发电机如何退出？

在一个普通的 for 循环中，当生成器不再生成时，迭代停止。然而，这可以通过我们在生成器实例上手动调用`next()`来观察到。超出迭代限制调用`next()`将引发以下异常。

```
----------------------------------------------------------------
StopIteration                  Traceback (most recent call last)
<ipython-input-41-cddec6aa1599> in <module>
---> 28 print(next(data))StopIteration:
```

# 使用发送、抛出和关闭

## 发送功能

让我们回忆一下质数的例子。假设我们想将我们的生成函数的值重置为 100，如果它们是质数，就开始产生大于 100 的值。我们可以在生成器实例上使用`send()`方法将一个值推入生成器，如下所示。

```
>>> primes = getPrimes()
>>> next(primes)
2
>>> primes.send(10)
11
>>> primes.send(100)
101
```

注意，在调用`send()`之前，我们必须至少调用`next()`一次。让我们看看我们必须如何修改我们的函数来适应这个目的。因为该函数应该知道如何分配接收到的值。

```
def getPrimes():
    value = 0
    while True:
        if isPrime(value):
            i = yield value
            if i is not None:
                value = i
        value += 1
```

我们将产生的值存储在变量`i`中。如果那不是`None`类型，我们把它赋给`value`变量。`None`检查至关重要，因为第一个`next()`在`value`变量中没有要产出的值。

## 投掷功能

假设您想要在大于 10 的值处结束迭代，以避免溢出或超时(假设)。`throw()`功能可用于提示发电机暂停引发异常。

```
primes = getPrimes()for x in primes:
    if x > 10:
        primes.throw(ValueError, "Too large")
    print(x)
```

这种技术对于验证输入很有用。逻辑取决于发生器的用户。这将产生以下输出。

```
2
3
5
7----------------------------------------------------------------
ValueError                     Traceback (most recent call last)
<ipython-input-113-37adca265503> in <module>
 **12** for x in primes:
 **13**     if x > 10:
---> 14         primes.throw(ValueError, "Too large")
 **15**     print(x)

<ipython-input-113-37adca265503> in getPrimes()
 **3**     while True:
 **4**         if isPrime(value):
----> 5             i = yield value
 **6**             if i is not None:
 **7**                 value = i

ValueError: Too large
```

## 关闭功能

处理闭包时没有异常通常是优雅的。在这种情况下，`close()`函数可以用来有效地关闭迭代器。

```
primes = getPrimes()for x in primes:
    if x > 10:
        primes.close()
    print(x)
```

这将为我们提供以下输出。

```
2
3
5
7
11
```

请注意，我们的值是 11，这是最后一个大于 11 的计算值。这模拟了 C/C++中 **do while** 循环的行为。

我相信这篇文章将有助于你将来开发更好的软件和研究程序。感谢阅读。

干杯😃