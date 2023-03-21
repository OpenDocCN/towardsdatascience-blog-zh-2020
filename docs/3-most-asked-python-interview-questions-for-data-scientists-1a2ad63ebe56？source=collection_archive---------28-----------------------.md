# 数据科学家最常被问到的 3 个 Python 面试问题

> 原文：<https://towardsdatascience.com/3-most-asked-python-interview-questions-for-data-scientists-1a2ad63ebe56?source=collection_archive---------28----------------------->

## 面向数据科学家的顶级 python 面试问题

![](img/e37f30d730064dce5e57577a957ed412.png)

照片由[法比奥](https://unsplash.com/@fabioha?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/data-science?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

许多博客和网站都有一些关于数据科学面试的问题和建议。然而，大多数数据科学面试也想测试候选人的软件工程技能。在这里，我们列出了在数据科学访谈中最常被问到的关于 Python 编程的 3 个问题。

# **链表和元组有什么区别？**

这个问题其实是关于理解 Python 中的数据结构。什么时候使用列表，什么时候使用元组？这与一个没有进入前三名的问题密切相关，这个问题是关于什么是不可变的和易变的对象。我们将直接进入它。

元组是不可变的对象，而列表不是。简单地说，可变对象可以被改变，而不可变对象则不能。因此，一个列表有一个可变的大小，即，可以扩展，我们可以替换它的项目，而一个元组是不可变的，因此我们不能。

这种解释在大多数情况下足以通过面试问题，但在本帖中，我们会给你更多的细节，让你更好地理解它。

元组和列表都是用于存储项目集合的数据结构，这些项目可以是任何数据类型。元组和列表都使用索引来访问这些项目。这就是相似之处的终结。

元组的语法不同于列表，列表中的元组是用`‘()’` 和列表`‘[]’`创建的。如上所述，元组是不可变的，这意味着如果你有一个这样的元组: `my_tuple=('A','B','C')`，并且你试图覆盖索引“0”处的元素，例如`my_tuple[0]='D'`，这将抛出一个类型错误:`TypeError: ‘tuple’ object does not support item assignment`。如果你在修改一个列表，这种情况不会发生。

就内存效率而言，由于元组是不可变的，因此可以为它们分配更大的内存块，而列表需要更小的内存块来适应这种可变性。这里不要混淆。更大的内存块实际上意味着更少的内存占用，因为开销很低。因此，元组的内存效率更高，但如果预计它们会发生变化，那么使用元组并不是一个好主意。

# **什么是列表理解？**

任何用 python 编程的人都可能遇到过这种符号，在这里你可以像这样做 for 循环。意志导致['E '，' x '，' a '，' m '，' p '，' l '，' e']。

列表理解*是一种创建和定义列表的优雅方式，无需编写太多代码*。此外，您可以像这样向列表理解添加条件:`[ x for x in range(20) if x % 2 == 0]`它只返回能被 2 整除的数字(包括 0)。

这里的一个关键点是，每个列表理解都可以使用普通的 for 循环重写。然而，并不是每个 for 循环都可以用列表理解格式重写。然而，通常情况下，列表理解更有效。如需更多分析，请访问此[链接](/python-basics-list-comprehensions-631278f22c40)。

有时，列表理解被比作 lambda 函数(不要与 AWS Lambda 混淆),这是另一个在面试中经常出现的问题。Lambda function 实际上是做一个函数操作，就像你通常做的那样，但它是在一行中完成的。

举个例子，

```
def my_func(x):
    print(x+1)
```

可以转到这个 lambda 函数`my_func=lambda x: print(x+1)`，这里可以像普通函数一样调用这个函数，例如`my_func(2)`

# **什么是装修工？**

Python 中的装饰器是任何可调用的 Python 对象，用于修改函数或类，而不改变其底层结构。使用装饰器很容易，对于一个没有经验的程序员来说，编写装饰器可能很复杂。

让我们看看如何创建一个。

假设我们想要创建一个装饰器来将字符串转换成小写。

```
def lowercase_decorator(function): def wrapper():
        func = function()
        make_lowercase = func.lower()
        return make_lowercase return wrapper
```

我们定义了一个函数，它将另一个函数作为参数，并对传递的函数执行小写操作。

```
def say_hello():
    return ‘HELLO WORLD’decorate = lowercase_decorator(say_hello) 
decorate()
```

输出将是“hello world”。

然而，在实践中，我们使用' @ '符号作为我们上面所做的语法糖，以减少改变函数行为所需的代码。

在定义了我们的装饰器之后，我们可以得到如下结果:

```
@lowercase_decorator
def say_hello():
    return ‘HELLO WORLD’say_hello()
```

我们希望输出是相同的，即，“你好世界”。