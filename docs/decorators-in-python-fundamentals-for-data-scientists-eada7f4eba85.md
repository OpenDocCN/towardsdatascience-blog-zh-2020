# Python 中的装饰者:数据科学家的基础

> 原文：<https://towardsdatascience.com/decorators-in-python-fundamentals-for-data-scientists-eada7f4eba85?source=collection_archive---------33----------------------->

## 用一个具体的例子来理解基础！

![](img/bf37cfc7f628a2f019977a8145ec2e5b.png)

戈兰·艾沃斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Python 中的 Decorators 用于扩展可调用对象的功能，而无需修改其结构。基本上，装饰函数包装另一个函数来增强或修改它的行为。

这篇文章将向你介绍 Python 中装饰者的基础知识。

让我们编写一个包含装饰器实现示例的 Python3 代码:

# **装饰定义**

```
def **decorator_func_logger**(**target_func**):
 def **wrapper_func**():
   print("Before calling", **target_func**.__name__)
   target_func()
   print("After calling", **target_func**.__name__)

 return **wrapper_func** def **target()**:
 print('Python is in the decorated target function')**dec_func** = **decorator_func_logger**(**target**)
**dec_func()*****Output:***air-MacBook-Air:$ **python DecoratorsExample.py**
('Before calling', 'target')
Python is in the decorated target function
('After calling', 'target')
```

上面的装饰结构帮助我们在调用目标函数之前和之后在控制台上显示一些注释。

下面是定义装饰器的简单步骤；

*   首先，我们应该定义一个可调用的对象，比如一个**装饰函数**，其中也包含一个包装函数。
*   装饰函数应该将一个**目标函数**作为参数。
*   它应该返回**包装函数**，该函数扩展了作为参数传递的目标函数。
*   **包装函数**应包含一个**目标函数调用**以及扩展目标函数行为的代码。

![](img/fac3774ae728ccf78e566c940a1ea93f.png)

多鲁克·耶梅尼西在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

```
def **decorator_func_logger**(**target_func**):
 def **wrapper_func**():
   print("Before calling", **target_func**.__name__)
   target_func()
   print("After calling", **target_func**.__name__)

 return **wrapper_func**@**decorator_func_logger**
def **target()**:
 print('Python is in the decorated target function')**target()*****Output:***air-MacBook-Air:$ **python DecoratorsExample.py**
('Before calling', 'target')
Python is in the decorated target function
('After calling', 'target')
```

在 Python 提供的语法糖的帮助下，我们可以简化 decorator 定义，如上所示。

注意@**decorator _ func _ logger**被添加在我们想要修饰的 **target** 函数之前。然后我们可以直接调用目标函数。不需要像我们在第一个例子中所做的那样，显式地分配装饰器。

![](img/94426fa03a5acb026b75cdf857cc3284.png)

多鲁克·耶梅尼奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# **用参数定义多个装饰器和装饰函数**

```
**import time***def* **decorator_func_logger(*target_func*):**
 *def* **wrapper_func(*****args, **kwargs****):**
  print("Before calling", target_func.__name__)
  **target_func(*****args, **kwargs****)**
  print("After calling", target_func.__name__)
 return **wrapper_func***def* **decorator_func_timeit(*target_func*):**
 *def* **wrapper_func(*****args, **kwargs****):**
  ts = time.time()
  **target_func(*****args, **kwargs****)**
  te = time.time()
  print (target_func.__name__, (te - ts) * 1000)
 return **wrapper_func****@decorator_func_logger
@decorator_func_timeit**
*def* **target(*loop*):**
 count = 0
 print('Python is in the decorated target function')
 for number in range(loop):
  count += number**target(100)
target(3000)*****Output:***air-MacBook-Air:$ **python DecoratorsExample.py**('Before calling', 'wrapper_func')
Python is in the decorated target function
('target', 0.015974044799804688)
('After calling', 'wrapper_func')('Before calling', 'wrapper_func')
Python is in the decorated target function
('target', 0.47397613525390625)
('After calling', 'wrapper_func')
```

通过使用' @ '语法在目标函数之前添加几个装饰器，可以很容易地用多个装饰器来装饰目标函数。装饰器的执行顺序将与它们在目标函数之前的排列顺序相同。

注意，在我们的目标函数中有一个参数 **loop** 。只要包装函数使用相同的参数，这就没有问题。为了确保装饰器可以灵活地接受任意数量的参数，包装器函数使用了 **(*args，**kwargs)** 参数。

# 关键要点

*   Decorators 定义了可重用的代码块，您可以将这些代码块应用到可调用的对象(函数、方法、类、对象)来修改或扩展其行为，而无需修改对象本身。
*   考虑到你的脚本中有许多函数执行许多不同的任务，你需要给所有的函数添加特定的行为。在这种情况下，将相同的代码块复制到函数中以获得所需的功能并不是一个好的解决方案。你可以简单地修饰你的函数。

# 结论

在这篇文章中，我解释了 Python 中 decorators 的基础知识。

这篇文章中的代码可以在[我的 GitHub 库中找到。](https://github.com/eisbilen/DecoratorsExample)

我希望这篇文章对你有用。

感谢您的阅读！