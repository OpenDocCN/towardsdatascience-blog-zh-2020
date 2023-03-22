# 关于 Pythonic 类的所有内容:生命周期

> 原文：<https://towardsdatascience.com/all-about-pythonic-class-the-life-cycle-5d2deae1ad8d?source=collection_archive---------30----------------------->

## Python 数据模型

## Python 数据模型—第 2 部分[b]

![](img/d49863ec01a8fba63b9ca64b4b1bce50.png)

图片:[来源](https://www.pxfuel.com/en/free-photo-ernyb)

这是“关于 Pythonic 类的一切”的第二部分。现在，我们的“Python 数据模型”系列—

1.  [对象、类型和值；Python 数据模型—第 1 部分](/python-data-model-part-1-objects-types-and-values-cb9316f57e8)
2.  [关于 Pythonic 类的一切:诞生和风格；Python 数据模型—第 2 部分[a]](https://farhadnaeem.medium.com/all-about-pythonic-class-the-birth-and-style-36002e28d035)
3.  关于 Pythonic 类的所有内容:生命周期；Python 数据模型—第 2 部分[b]

在前一章中，我们已经展示了如何编写一个基本类，类和对象之间的关系，以及新式类和旧式类之间的区别。我们将从上一章离开的地方开始这一章

# 4.当我们定义一个类时，实际上会发生什么

那么当我们声明一个类时会发生什么呢？Python 解释器是怎么解释的？

当我们调用一个类来形成一个对象时，有两个特殊的方法被调用。

*   首先，解释器调用 __ **new__** 方法，这是一个类的“真正”构造函数。然后，这个特殊的方法在内存中未命名的位置创建一个具有正确类型的对象。
*   然后调用 __ **init__** 用 __ **new__** 创建的前一个对象初始化类对象。大多数时候，如果没有明显的原因，我们不需要声明一个 __ **new__** 的特殊方法。它主要发生在后台。用 __ **init__** 实例化一个对象是最常见和标准的做法，足以满足我们的大多数目的。

这是一个复杂的过程，在幕后创建一个类。它还需要深入了解 Python 解释器的设计以及该语言的核心实现。为了简单起见，我们将从高层次的角度描述该过程，并编写后端过程的假设或伪代码。

当我们调用一个类来形成一个对象时，解释器调用`type`。它需要三个参数-
`type(classname, superclass, attribs)`或
`type("", (), {})`

其中类名是声明类的字符串表示，超类是类的元组表示，我们的类将从该元组表示中继承属性，属性是`class.__dict__`的字典表示。让我们正常地声明一个类

```
class Footballer():
    game = "Football"

    def __init__(self, name, club):
        self.name = name
        self.club = club

    def a_method(self):
        return None

print(Footballer.__dict__)    

# Output
'''
{'__module__': '__main__', 'game': 'Football', '__init__': <function Footballer.__init__ at 0x7f10c4563f28>, 'a_method': <function Footballer.a_method at 0x7f10c45777b8>, '__dict__': <attribute '__dict__' of 'Footballer' objects>, '__weakref__': <attribute '__weakref__' of 'Footballer' objects>, '__doc__': None}
'''
```

现在我们将使用`type`元类编写相同类型的类

```
def outer_init(self, name, club):
    self.name = name
    self.club = club

Footballer1 = type("Footballer1", (), {"game":"Soccer",  "__init__": outer_init, "b_method": lambda self: None})print(Footballer1.__dict__)  
print(Footballer1.game)

# Output
'''
{'game': 'Soccer', '__init__': <function outer_init at 0x7f10c4510488>, 'b_method': <function <lambda> at 0x7f10c4510f28>, '__module__': '__main__', '__dict__': <attribute '__dict__' of 'Footballer1' objects>, '__weakref__': <attribute '__weakref__' of 'Footballer1' objects>, '__doc__': None}Soccer
'''
```

啊哈！我们刚刚使用类型元类创建了一个常规的用户定义类！！这到底是怎么回事？
当类型被调用时，它的`__call__`方法被执行，运行另外两个特殊的方法-

1.  `type.__new__(typeclass, classname, superclasses, attributedict)`建造师
2.  `type.__init__(cls, classname, superclasses, attributedict)`发起者/初始者

`__new__(cls, [...)`特殊方法`__new__`是创建对象时调用的第一个方法。它的第一个参数是类本身，后面是构成它所需的其他参数。这个函数很少使用，默认情况下在对象创建期间由解释器调用。
`__init__(self, [...)`初始化器特殊方法初始化一个类，并给出对象的默认属性。此方法将一个对象作为第一个参数，后面跟它的其他参数。这是 Python 世界中最常用的特殊方法。

下面的`pseudo-code`展示了 Python 解释器如何从一个类创建一个对象。请记住，为了有一个基本的理解，我们已经省略了解释器所做的底层工作。看一看不言自明的代码说明

```
class Bar():
    def __init__(self, obVar):
        self.obVar = obVar

>>> obj = Bar("a string") ----------------[1]# putting  object parameter into a temporary variable
>>> tmpVar = Bar.__new__(Bar, "another string") 
>>> type(tmpVar)
>>> __main__.Bar>>> tmpVar.obVar -------------------------[2]# the class is not initialised yet 
# It will throw errors>>> AttributeError Traceback (most recent call last)
....
AttributeError: 'Bar' object has no attribute 'obVar'
------------------------------------------------------# initialise a temporary variable 
>>> tmpVar.__init__("a string") ---------------[3]
>>> tmpVar.obVar
>>> 'another string'>>> obVar = tmpVar 
>>> obVar.obVar
>>> 'another string' 
```

`Obj`的表现和我们预期的一样`[1]`。但是，当我们通过调用`Bar.__new__`并试图访问`obVar,`来创建`tmpVar`时，它抛出了一个错误`[2]`。因此，我们需要用`__init__`初始化，之后，它会像前面的变量`[3]`一样很好地工作。

附:名字两边加双下划线的方法被称为特殊方法或魔法方法。特殊方法是 CPython 的“实现细节”或低级细节。为什么我们称之为魔法方法？因为这些方法给我们的课增加了“魔力”。对于魔法方法，我们将有两个单独的章节。

# 5.类的生命周期

到目前为止，我们已经讨论了 Python 类的几乎所有基本概念。我们在这一节将要讨论的内容可以算作是上一节的重复，我们将讨论一个类的更微妙的细节。

当我们运行 Python 程序时，它会在运行时从内置或用户定义的类中生成对象。每个对象在创建后都需要内存。Python 在完成分配给它的任务时不需要对象。它变成了一条不需要的信息或垃圾。

另一方面，Python 解释器需要定期释放内存用于进一步的计算、新对象的空间、程序效率和内存安全性。当一块“垃圾对象”被处置时，它就不再存在于内存中。问题是 Python 类存在多久，什么时候不再存在？最简单的答案是一个对象或类存在，只要它

1.  保存对其属性或派生的引用，并且
2.  它被另一个对象引用。

当两个标准都不满足时，一个对象或类就不存在了。答案是最简单的，但是很多事情都是在幕后进行的。我们将揭示其中的一些。

下面的类是设计来连接到一个 web 服务器并打印状态代码，服务器信息，然后我们将关闭连接。我们将属性从另一个名称空间“打包”到`Response`类中，该名称空间将是类型的元类；不出所料。

```
import requests

# Another name space
var = "a class variable."

def __init__(self, url):
    self.__response = requests.get(url)
    self.code = self.__response.status_code

def server_info(self):
    for key, val in self.__response.headers.items():
        print(key, val)

def __del__(self): ---------------------- [1]
    self.__response.close()
    print("[!] closing connection done...")# Injecting meta from another namespace
Response = type('Response', (), {"classVar":var,"__init__": __init__, "server":server_info, "__del__":__del__})  

# Let's connect to our home server on my father's desk 
resp = Response("http://192.168.0.19")print(type(Response))
print(resp.code)
resp.server()del resp  # It will call the DE-CONSTRUCTOR# Output
'''
<class 'type'>
401
Date Fri, 25 Sep 2020 06:13:50 GMT
Server Apache/2.4.38 (Raspbian)
WWW-Authenticate Basic realm="Restricted Content"
Content-Length 461Keep-Alive timeout=5, max=100
Connection Keep-Alive
Content-Type text/html; charset=iso-8859-1
[!] closing connection done... # resp object destroyed
'''
```

`__del__(self)`特殊方法`__del__`可以称为终结器或析构器，负责终止一个对象或类。`__del__`是在对象的垃圾收集过程中调用的终结器。当对一个对象的所有引用都被删除时，这个过程就会发生。

在上面这段代码中，为了有一个基本的理解，我们省略了解释器所做的底层事情。当我们删除响应对象`del resp`时，会执行`__del__`特殊方法或反构造函数来清除 RAM 中的对象。

该方法在删除时需要额外清理的特殊情况下很有用，例如套接字和文件对象。然而，请记住这样一个事实，即使解释器退出时目标对象仍然存在，也不能保证`__del__`是否会被执行。因此，手动或使用包装器关闭任何文件或连接是一个很好的做法。

# 6.碎片帐集

一个类产生一个对象。当一个对象不再存在于一个名称空间中时，假设一个类也不需要存在于该空间中。类也可以像普通对象一样被删除，因为类本身就是一个对象。为了内存安全和高效的代码，垃圾收集过程会处理不必要的对象。

Python 在 CPython*中采用了两种垃圾收集机制

1.  引用计数
2.  分代垃圾收集

在这两者之间，第一个是最常用的，因为在用户空间中销毁对象的自由如果不小心实现可能会带来灾难，例如，任何对被销毁对象的强引用都可能成为孤儿/悬空对象并泄漏内存。引用计数很容易实现，但是它不能处理引用周期，这使得它更容易发生内存泄漏。

默认情况下，一个类在创建时会得到一个*类型和引用*计数。每当我们通过调用一个对象来创建它时，解释器就增加它的引用计数。对象或类的引用计数在

1.  作为函数参数传递
2.  赋给另一个对象或变量
3.  附在名单上
4.  作为一个类的属性添加等等。

当该计数达到*零*时，该类不再存在于存储器中，这可以被视为“类的死亡”。换句话说，当一个对象在任何地方被引用时，它的引用计数增加，而当它被取消引用时，它的引用计数减少。我们编写的高级代码不会影响这种低级实现，尽管可以使用分代垃圾收集或`gc`模块手工处理垃圾收集。请遵守以下代码

```
import sys

class Bar():
    pass

print(sys.getrefcount(Bar)) # default reference count is 5

a = Bar()
b = Bar()
c = Bar()

print(sys.getrefcount(Bar)) # reference count increased by 3 

del a , b , c  

print(sys.getrefcount(Bar)) # reference count decreased by 3 

# Output
'''
5
8
5

'''
```

当一个或多个 Python 对象相互引用时，就会出现引用循环。因此，在这些情况下，仅仅使用`del`删除一个对象只会减少它的引用计数，并使它不可被程序访问。它实际上保留在记忆中。
例如

```
class Foo():
    pass

obj.var = obj
```

在上面的例子中，我们通过将对象`obj`本身赋予其属性`obj.var`来创建一个引用循环。仅仅减少`obj`的引用计数不会从内存中破坏它，而是需要分代垃圾收集来永久清除。这种实现可以由高级编码空间中的`gc`模块来处理。
这是‘杀班’的另一种方式。

# 最终想法:

从这两部分，我们试图展示关于 Pythonic 类的 6 个重要事实。预计现在我们至少对 Python 类如何工作以及如何从后台内存中清理有了一些基本的概念。

为了掩盖 python 数据模型，我们将有单独的更详细的文章，讨论魔术方法、装饰器、元编程、python 继承等。对该过程的良好理解使开发人员能够充分利用 Python 的潜在能力，并在事情变得更加复杂时调试复杂的问题。

# 参考资料:

1.  [https://docs.python.org/3/reference/datamodel.html](https://docs.python.org/3/reference/datamodel.html)
2.  卢西亚诺·拉马尔霍的流畅 Python。这是每一个 python 爱好者都应该读的书，以提高他们的 python 技能。
3.  [https://stackify.com/python-garbage-collection/](https://stackify.com/python-garbage-collection/)