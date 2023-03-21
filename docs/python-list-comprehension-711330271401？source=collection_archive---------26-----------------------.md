# Python 列表理解

> 原文：<https://towardsdatascience.com/python-list-comprehension-711330271401?source=collection_archive---------26----------------------->

## Python 列表终极指南！

## 成为列表理解专家

列表理解是处理 Python 列表时最强大的特性之一。这个功能已经触手可及。不需要导入任何库或模块，因为列表理解是 Python 的一个内置特性。

![](img/69f021af7681f57f833d034a659c9034.png)

图片由[约书亚·索蒂诺](https://unsplash.com/@sortino)在 [Unsplash](https://unsplash.com/) 上拍摄

# 如何写列表理解

List Comprehension 可用于创建仅在一行中包含限制(用数学语句编写)的列表。除此之外，列表理解可以用来从另一个列表中创建一个新列表。

列表理解语法很容易理解，别担心，它很简单。做列表理解，需要先做一个方括号`[]`。在括号内，您需要使用 for 关键字，后跟一个或多个您选择的子句。这里的表达可以是任何东西，你可以倾注你的创造力。

# 基本语法

```
[**expression** for **element** in **list** if **conditional**]
```

以上列表理解等同于:

```
for **element** in **list**:
    if **conditional**:
        **expression** #appends the resulting element to the list
```

# 例子

当然，我会给你提供一些例子！否则这篇文章就不完整。我将从最简单的开始，继续讨论更高级的应用程序。

# 生成新列表

1.制作一个由前 5 个自然数组成的新列表。

```
ls = [i for i in range(1, 6)] #ls = [1, 2, 3, 4, 5]
```

2.制作前 5 个偶数自然数的新列表。

```
ls = [i for i in range(2, 11, 2)] #ls = [2, 4, 6, 8, 10] 
```

或者使用 if 来说明如何使用条件来创建新列表。

```
ls = [i for i in range(2, 11) if i % 2 == 0] #ls = [2, 4, 6, 8, 10]
```

3.列出前 5 个平方数

```
ls = [i**2 for i in range(1, 6)] #ls = [1, 4, 9, 16, 25]
```

# 使用我们声明的函数

列表理解的条件不需要明确地写在方括号内。你可以预先声明一个函数，然后在列表理解中调用它。在下一个例子中，我们将这样做。

4.列出 1 到 30 之间的质数

```
import mathdef isPrime(n):
    if n == 1:
        return False
    elif n == 2:
        return True
    elif n % 2 == 0:
        return False
    else:
        for i in range(3, int(math.sqrt(n)) + 1, 2):
            if n % i == 0:
                 return False
        return Truels = [i for i in range(1, 31) if isPrime(i)]
#ls = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
```

# 从另一个列表创建新列表

列表理解的另一个有用的用法是从另一个列表中创建一个新列表。你可以这样想，你正在从原始列表中选择满足你需求的元素。然后，将这些项目分组到一个新列表中。以下是一些例子:

5.只取原始列表中的偶数

```
ls = [15, 6, 1, 3, 10]result = [i for i in ls if i % 2 == 0] #result = [6, 10]
```

6.查找一个列表中也存在于另一个列表中的元素

```
ls = [15, 6, 1, 3, 10]
ls2 = [1, 2, 3, 4, 5, 6]result = [i for i in ls if i in ls2] #result = [6, 1, 3]
```

# 最后的话

Python 列表理解是您在编程工具箱中绝对需要的一项技术。它功能强大，易于在任何地方实现。过一段时间，相信我，你会习惯的。在这一点上，你可以随时轻松地使用它。如果您觉得这篇文章很有用，并且想了解更多关于 Python 的其他技巧，您可以阅读这篇文章:[成为 Python“一行程序”专家](/become-a-python-one-liners-specialist-8c8599d6f9c5)。

最诚挚的问候，

**弧度克里斯诺**