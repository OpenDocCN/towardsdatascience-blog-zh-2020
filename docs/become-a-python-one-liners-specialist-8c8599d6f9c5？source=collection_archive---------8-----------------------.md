# 成为 Python“一行程序”专家

> 原文：<https://towardsdatascience.com/become-a-python-one-liners-specialist-8c8599d6f9c5?source=collection_archive---------8----------------------->

## 终极指南

## 少写代码，多成就！

你只需要一句台词！

![](img/fdc7ee562dd360e1ae001edbaa8d9be3.png)

图片由 [Hitesh Choudary](https://unsplash.com/@hiteshchoudhary) 在 [Unsplash](https://unsplash.com/) 上拍摄

由于其简单的语法，Python 是一种初学者友好的语言。与其他需要特定使用花括号和分号的语言不同，Python 只关心缩进。到这一点，已经显得 Python 关心用户了，不是吗？然而它并没有就此停止，Python 通过其编写单行代码片段的能力给了我们更多的便利。虽然很短，但这些单行代码可以像用 Python 或任何其他语言编写的冗长乏味的程序一样强大。

# 你为什么关心这个？

通过使用 Python 一行程序，您将能够编写更少的代码并实现更多，这直接转化为生产力的提高。最重要的是，一行代码读起来更有趣，并且可以展示你对 Python 语言的熟练程度。

# 我们开始吧

1.交换两个变量的值

你们大多数人可能会这样做。

```
a = 1
b = 2
temp = a
a = b
b = temp
```

这样就可以了，a 的值现在是 2，b 的值是 1。但是，考虑一下这个！

```
a, b = b, a
```

它甚至适用于任何数量的变量！

```
a, b, c, d = b, c, d, a
```

2.列表理解

列表理解可以用来代替制作列表所需的 for 循环。

```
ls = []
for i in range(5):
    ls.append(i)
```

你可以这样做，而不是使用 for 循环。

```
ls = [i for i in range(5)]
```

您还可以添加 use list comprehension 来迭代现有的列表，并提供元素需要满足的一些标准。

```
ls = [-1, 2, -3, 6, -5]ls = [i for i in ls if i > 0] #ls = [2, 6], positive integers only
```

3.地图方法

Python 中的 map 方法为列表中的每个元素执行特定的函数。

```
ls = list(map(int, ["1", "2", "3"])) #ls = [1, 2, 3]
```

这里，我们将列表中的元素类型转换为整数类型。您不再需要使用循环函数遍历每个元素并逐个转换，map 会为您完成这些工作。

4.过滤方法

Python 中的 filter 方法检查 iterables，例如，列表的元素，并删除返回 False(不符合标准)的元素。在这个例子中，我们将利用 filter 方法来查找两个列表中的公共元素。

```
ls1 = [1, 3, 5, 7, 9]
ls2 = [0, 1, 2, 3, 4]
common = list(filter(lambda x: x in ls1, ls2)) #common = [1, 3]
```

与 map 方法一样，您不再需要遍历每个元素。

5.还原方法

Python 中的 Reduce 方法用于对每个元素应用一个函数，直到只剩下一个元素。顾名思义，该方法将数组简化为单个元素。

Reduce 函数是在“functools”模块中定义的，所以不要忘记预先导入它！

```
from functools import reduceprint(reduce(lambda x, y: x*y, [1, 2, 3])) #print 6
```

6.λ函数

```
def fullName(first, last):
    return f"{first} {last}"name = fullName("Tom", "Walker") #name = Tom Walker
```

Lambda function 是一个小型匿名(单行)函数，它能够替换用 def 关键字声明的常规函数。上面的片段和这个类似。

```
fullName = lambda first, last: f"{first} {last}"name = fullName("Tom", "Walker") #name = Tom Walker
```

当我们把 lambda 作为高阶函数使用时，它的威力会更加明显。高阶函数是将其他函数作为自变量或返回其他函数的函数。

```
highOrder = lambda x, func: x + func(x)result1 = highOrder(5, lambda x: x*x) #result1 = 30
result2 = highOrder(3, lambda x : x*2) #result2 = 9
```

注意，通过使用 lambda 函数，我们可以很容易地使用不同的 lambda 函数作为 func(x)的参数。它不是给了我们很大的自由吗？

7.使用 reverse 方法反转列表

也许你们都非常熟悉我下面介绍的列表切片方法:

```
ls = [1, 2, 3]
ls = ls[::-1] #ls = [3, 2, 1]
```

但是，这种方法会使用更多的内存，所以可以考虑使用相反的方法。

```
ls = [1, 2, 3]
ls.reverse() #ls = [3, 2, 1]
```

8.使用*多次打印同一元素

而不是像这样使用 for 循环

```
for i in range(5):
    print("x", end = "") #print xxxxx
```

你可以这样做

```
print("x"*5) #print xxxxx
```

# 例子

既然我们已经研究了用 Python 中的一行代码能做些什么，现在是实现它的时候了。这里有一些例子，你可以应用到目前为止你所学到的东西。

1.  求一个数的阶乘

一个数的阶乘，用 n 表示！是所有小于或等于 n 的正整数的乘积。也许你们大多数人会考虑使用这样的递归函数。

```
def fact(n):
    if n == 1:
        return 1
    else:
        return n*fact(n-1)
```

相反，我们可以这样做:

```
from functools import reducereduce(lambda x, y: x*y, range(1, n+1)) #n factorial
```

2.寻找斐波那契数

斐波那契数列是一个数字序列，从 0 开始作为第 0 个数字，1 作为第 1 个数字。序列从一个数字开始，该数字是前面两个数字的和。斐波那契数列的前 8 项是 0，1，1，2，3，5，8，13。直觉上，你可以像这样使用递归。

```
def fib(n):
    if n == 0 or n == 1:
        return n
    else:
        return fib(n-1) + fib(n-2)print(fib(n)) #prints the nth Fibonacci number
```

或者，您可以这样做:

```
fib = lambda x: x if x<=1 else fib(x-1) + fib(x-2)print(fib(n)) #prints the nth Fibonacci number
```

3.检查一个字符串是否是回文

回文是一个单词、短语或序列，前后读起来是一样的。这段代码没有错:

```
def is_palindrome(string):
    if string == string[::-1]:
        return True
    else:
        return Falseprint(is_palindrome("abcba")) #prints True
```

然而，这样做不是更优雅吗？

```
string = "abcba" #a palindromeprint(string == string[::-1]) #prints True
```

4.展平二维数组

我们希望给定的二维数组是一维数组。这可以通过用 for 循环遍历每个元素来实现。

```
ls = [[1, 2], [3, 4], [5, 6]]
ls2 = []for i in ls:
    for j in i:
        ls2.append(j)print(ls2) #prints [1, 2, 3, 4, 5, 6]
```

然而，我们可以用这个一行程序实现同样的事情:

```
ls = [[1, 2], [3, 4], [5, 6]]print([i for j in ls for i in j]) #prints [1, 2, 3, 4, 5, 6]
```

# 结束语

学习这种一行程序 Python 技巧不会让你成为编程奇才。然而，这将是一件有趣的事情，它将帮助你变得更有效率(更少的代码行→更少的时间需要)。除此之外，在你的代码中加入这些技巧会很酷。嘘…你也许可以用这个打动某人。

我希望你能在你的代码中直接应用这些很酷的技巧。也许这些技术中的一些在你第一次使用时不会下沉。放心吧！随着你使用它的次数越来越多，它会变得越来越自然。

问候，

**弧度克里斯诺**