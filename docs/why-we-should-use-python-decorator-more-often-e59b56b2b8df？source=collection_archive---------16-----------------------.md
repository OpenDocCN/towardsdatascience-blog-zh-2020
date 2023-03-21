# 为什么我们应该更频繁地使用 Python Decorator

> 原文：<https://towardsdatascience.com/why-we-should-use-python-decorator-more-often-e59b56b2b8df?source=collection_archive---------16----------------------->

## Python 装饰者

## Python Decorator 是一个强大的工具，它可以帮助我们操作一个函数的功能，一个只需添加@decorator_name 的测试方法

![](img/faafca4ee9491258894eeb0b39a8df51.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com/s/photos/decorate?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

在 Python 中，一切都是对象，这意味着每个实体都有一些元数据(称为*属性*)和相关的功能(称为*方法*)。这些属性和方法是通过点语法访问的。我们定义的名字只是绑定到这些对象的标识符。函数也不例外，它们也是对象(带有属性)。同一个函数对象可以绑定各种不同的名称。

例如:

这里我们将函数赋给一个变量。我们会注意到，变量赋值后，就变成了函数。下面是 test_decorator 的输出:

```
test_decorators.py::test_decorator PASSED                                [100%]8
3
```

Python 中的函数也可以将其他函数作为输入，然后返回其他函数。这种技术被称为高阶函数。

这里我们传递函数(double 或 triple)作为函数 operate 的输入。当运行 test_decorator 时，我们将在控制台中看到:

```
test_decorators.py::test_decorator PASSED                                [100%]6
9
```

函数和方法被称为**可调用的**，因为它们可以被调用。

事实上，任何实现特殊的`__call__()`方法的对象都被称为 callable。所以，在最基本的意义上，装饰器是一个返回可调用对象的可调用对象。

基本上，装饰者接受一个函数，添加一些功能并返回它。

运行 test_decorator 会显示

```
test_decorators.py::test_decorator PASSED                                [100%]I got decorated
I am ordinary
None
```

通常，我们会看到使用@decorator_name 以语法糖的方式编写 decorator

运行 test_decorator 会显示相同的

```
test_decorators.py::test_decorator PASSED                                [100%]I got decorated
I am ordinary
None
```

# 可以一起玩的简单装饰

这是一个简单的装饰器，为使用该装饰器的方法打印开始时间和结束时间。

在控制台中，您将看到

```
test_decorators.py::test_decorator_simple PASSED                         [100%]Start time is : 2020-07-21 14:41:44.064251
Time in test : 2020-07-21 14:41:44.064337
End time is : 2020-07-21 14:41:44.064366
```

# 有争论的装饰者

运行测试代码将向控制台显示这一点

```
test_decorators.py::test_decorator_with_arguments PASSED                 [100%]Inside wrapped_f()
Decorator arguments: Donald Le 1990
Testing decorator with arguments
After f(*args)
```

# 用 Python 链接装饰器

在 Python 中，多个装饰者可以链接在一起。

也就是说，一个函数可以用不同的(或相同的)装饰者进行多次装饰。我们简单地将装饰器放在期望的功能之上。

运行这个将显示

```
test_decorators.py::test_decorator PASSED                                [100%]..............................
Start time is 2020-07-22 10:35:17.462552
Test Decorator
End time is 2020-07-22 10:35:17.462579
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
```

# Decorators 与 pytest 等其他框架集成

然后我们可以在测试方法中使用它

很好奇装饰器的值是如何使用的，它实际上是在收集 testrail id 的 pytest 挂钩中使用的。

这是我们如何从 pytest 获得装饰值的:

```
testrail_ids = item.get_closest_marker('testrail').kwargs.get('ids')
```

就是这样。

感谢您阅读我的帖子。

# 参考

[https://www.geeksforgeeks.org/decorators-in-python/](https://www.geeksforgeeks.org/decorators-in-python/)

[](https://python-3-patterns-idioms-test.readthedocs.io/en/latest/PythonDecorators.html) [## 装饰者——Python 3 模式、配方和习惯用法

### 请注意，本章是一项正在进行的工作；在我完成之前，你最好不要开始改变…

python-3-模式-习语-test.readthedocs.io](https://python-3-patterns-idioms-test.readthedocs.io/en/latest/PythonDecorators.html)  [## Python 装饰者

### Python 有一个有趣的特性，叫做 decorators，用于向现有代码添加功能。这也叫…

www.programiz.com](https://www.programiz.com/python-programming/decorator) 

**注**:如果你喜欢这个故事，想看类似这样的故事，而你还没有订阅媒体，请通过这个链接【https://ledinhcuong99.medium.com/membership】的[订阅媒体](https://ledinhcuong99.medium.com/membership)。这可以支持我这样写内容。谢谢大家！