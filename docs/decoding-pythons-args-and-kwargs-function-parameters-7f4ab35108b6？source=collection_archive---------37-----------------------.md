# 解码 Python 的*args 和**kwargs 函数参数

> 原文：<https://towardsdatascience.com/decoding-pythons-args-and-kwargs-function-parameters-7f4ab35108b6?source=collection_archive---------37----------------------->

## Python 中可选的函数和方法参数是如何工作的？

![](img/1b39a39bddd92e15720f9196754f5f63.png)

在 Python 函数中使用*args 和**kwargs

在使用 Python 函数时，您一定碰到过*args (args)或**kwargs (kwargs)参数。在初学者定义中，这些函数允许你用 Python 写一个变量或“n”个参数的函数。

虽然这是最外行的定义，但这些功能分别有特定的用途。实际的语法是*前导 args，而**前导 kwargs。

***参数**

如果您将*args 作为函数的一个参数传递，函数现在可以接受比定义的参数数量更多的参数。

```
def myFun(arg1, arg2, arg3, arg4) :
    print("arg1:", arg1)
    print("arg2:", arg2)
    print("arg3:", arg3)
    print("arg4:", arg4) 
args = ("My", "name", "is","Vishal")
myFun(*args)
```

位置参数作为元组收集。函数定义中的这些参数(arg1、arg2、arg3、arg4)以连续的方式从*args 中获取值。

```
def myFun(name, *args):
    print(name)
    if args:
        print(args)myFun("Vishal") #name parameter passed
myFun("Vishal",1,2,3) #name and args parameter passed
```

当你在第一个函数调用中传递你的名字时，它将返回你的名字——在这个例子中是“Vishal”。

当第二次调用该函数时，它传递 name 参数和三个数字。定义函数时，这三个数字存储在*args 中。

因此，输出将看起来像这样

```
Vishal # For first function callVishal # For second function call
(1,2,3)
```

args 的其他功能可能以这种方式派上用场:

```
def fun(*args):
    args=args + (4,)
    return args

print(fun(1,2,3))
```

在 args 元组中传递(1，2，3)之后，可以使用下面这段代码增加元组的大小。

```
args=args + (4,)
```

因此，您将看到该函数的输出如下所示:

```
(1,2,3,4)
```

*** *克沃格**

使用**kwargs 与使用*args 几乎相同，但是在**kwargs 中，您可以传入关键字参数。

```
def myFun(**kwargs) :
    for key, value in kwargs.items() :
        print("%s == %s" % (key, value))

myFun(first='Foo', second='Baz', third='Bar')
```

运行该函数后，该函数将返回 kwargs 的键和值。

```
first==Foosecond==Bazthird==Bar
```

以*args 解释中定义的例子为例:

```
def myFun(name, **kwargs):
    print(name)
    if kwargs:
        print(kwargs)myFun("Vishal") #name parameter passed
myFun("Vishal",one=1,two=2,three=3) #name and kwargs parameter passed
```

第一个函数调用只会将“Vishal”发送到定义中的名字，“Vishal”会返回。

在第二次调用中，关键字参数将与名称一起作为字典存储在 kwargs 中。

```
Vishal # For first function callVishal  # For second function
{‘one’:1,’two’:2,’three’:3}
```

您也可以使用这些参数，例如:

```
def fun(**kwargs):
    kwargs['name']='Vishal'
    return kwargsfun(roll=12, subject='Data Science')
```

现在，这个函数将向 kwargs 字典中再添加一个键值对。输出看起来会像这样。

```
{'roll': 12, 'subject': 'Data Science', 'name': 'Vishal'}
```

Python 以其独特的功能令人惊讶。而且，使用*args 和**kwargs 是每个初学者都必须知道的事情，因为它们经常会派上用场，尤其是当你想让你的函数在参数方面更加灵活的时候。

如需反馈和讨论，请通过我的 [Linkedin](https://www.linkedin.com/in/vishal-sharma-239965140/) 找到我！