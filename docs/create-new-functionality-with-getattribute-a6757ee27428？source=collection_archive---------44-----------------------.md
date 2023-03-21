# 使用 __getattribute__ 创建新功能

> 原文：<https://towardsdatascience.com/create-new-functionality-with-getattribute-a6757ee27428?source=collection_archive---------44----------------------->

## 继承、包装器、类属性等等！

![](img/67dd0c5e478d95b67e36882be8ff91c9.png)

图片由 [josealbafotos](https://pixabay.com/users/josealbafotos-1624766/) 提供

作为一个班级的孩子，很难从父母手中夺回控制权。你可能认为一旦你父母给了你一些功能，你就不能改变它了，但事实并非如此！

我们将考虑一个例子，我们想使用父方法给我们的功能，但是我们想在调用父方法之前或之后做一些事情。如果你从一个定义在不属于你的模块中的类继承，这将特别有用。

## 背景

为了理解如何实现这一点，我们需要理解 Python 如何调用类方法。

考虑下面的类:

```
class Person:
    def set_name(self,name):
        self.name = nameclass Worker(Person):
    def set_occupation(self,job):
        self.occupation = jobxander = Worker()
```

所以`xander`是`Worker`的一个实例。当我们运行`xander.set_name("Xander")`时，会发生两件事。

1.  班级在`set_name`上呼叫`__getattribute__`
2.  然后用参数`"Xander"`在`xander.set_name`上调用`__call__`

我们想改变父类中一个函数的行为，但是我们没有访问它的权限(可能是因为它属于一个不同的模块)。

我们对`__call__`没有任何控制权，因为该函数归父函数所有。然而，我们确实可以控制我们自己的`__getattribute__`,所以让我们改为修改它。

## 功能包装

因此，要添加新功能，我们需要采用原始方法，在之前或之后添加新功能，并将其包装在新函数中。然后，我们中断`__getattribute__`的正常行为，注入新的包装器。

让我们看一个基本的例子，我们想在函数运行之前和之后添加`print`语句。

```
def print_wrapper(func):
    def new_func(*args,**kwargs):
        print(f"Running: {func.__name__}")
        out = func(*args,**kwargs)
        print(f"Finished: {func.__name__}")
        return out return new_func
```

为了获得更多令人兴奋的例子，也为了更全面地理解装饰者，我在这里写下了它们:

[](/level-up-your-code-with-python-decorators-c1966d78607) [## 用 Python decorators 提升你的代码

### 日志记录、类型检查、异常处理等等！

towardsdatascience.com](/level-up-your-code-with-python-decorators-c1966d78607) 

现在我们需要编写新的`__getattribute__`函数:

```
def __getattribute__(self, attr):
    attribute = super(Parent, self).__getattribute__(attr)
    if callable(attribute):
        return print_wrapper(attribute)
    else:
        return attribute
```

检查属性是否为`callable`就是检查被选择的属性是否可以用参数调用，因此应用包装器是有意义的。如果属性只是一个属性，而不是一个方法，那么行为将不会改变。

综上所述，我们上面的基本示例应该是这样的:

```
class Person:
    def set_name(self,name):
        self._name = namedef print_wrapper(func):
    def new_func(*args,**kwargs):
        print(f"Running: {func.__name__}")
        out = func(*args,**kwargs)
        print(f"Finished: {func.__name__}")
        return out
    return new_funcclass Worker(Person):

    def __getattribute__(self, attr):
        attribute = Person.__getattribute__(self,attr)
        if callable(attribute):
            return print_wrapper(attribute)
        else:
            return attribute

    def set_occupation(self,job):
        self._job = job
```

当我们尝试使用它时，我们会得到:

```
>>> xander = Worker()
>>> xander.set_name("Xander")
Running: set_name
Finished: set_name
>>> xander.set_occupation("Butcher")
Running: set_occupation
Finished: set_occupation
>>> print(xander.__dict__)
{'_name': 'Xander', '_job': 'Butcher'}
```

## 警告的话

使用这些类型的函数，弄乱`__getattribute__`应该小心，每个属性都会调用它，所以添加计算密集型功能会大大降低程序的速度。摆弄像`__getattribute__`这样的函数也很容易导致递归错误和内核崩溃，因为类会一次又一次地调用自己。

这种递归的一个例子是，如果你使用`__getattribute__`调用`__dict__`，因为`__dict__`本身调用`__getattribute__`，所以循环继续…

## 我们学到了什么？

使用 Python 对象的双下划线方法可以让您节省大量时间和精力。实现同样目标的另一种方法是重新实现每个函数和装饰器。对于上面的例子来说，这可能不会太糟糕，但是想象一下，如果你的父类是像 Pandas 数据框或 NumPy 数组这样的东西，那么这将成为一项非常艰巨的任务。