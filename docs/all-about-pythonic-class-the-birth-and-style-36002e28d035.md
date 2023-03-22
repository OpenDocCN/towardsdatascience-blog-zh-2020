# 关于 Pythonic 类的一切:诞生和风格

> 原文：<https://towardsdatascience.com/all-about-pythonic-class-the-birth-and-style-36002e28d035?source=collection_archive---------38----------------------->

## Python 数据模型

## Python 数据模型—第 2 部分[a]

![](img/d49863ec01a8fba63b9ca64b4b1bce50.png)

图片:[来源](https://www.pxfuel.com/en/free-photo-ernyb)

如果您错过了本系列的第一部分，您可以在这里阅读[对象、类型和值；Python 数据模型—第 1 部分](/python-data-model-part-1-objects-types-and-values-cb9316f57e8)。

我们将这个“关于 Pythonic 类的一切”的标题分成两部分。
—关于 python 类的一切:诞生和风格
— [关于 python 类的一切:生命周期](https://farhadnaeem.medium.com/all-about-pythonic-class-the-life-cycle-5d2deae1ad8d)

这些章节的标题最终可能不会明确地反映读者刚刚描绘到她/他脑海中的想法。我们将试图用一个简单的叙述来涵盖 Python 类如何形成和析构的基本概念。Python 类(更具体地说是 Python 3.x 类)的创建和销毁不可避免地会涉及许多看似不相关的主题。

更重要的是，读者至少需要适度了解 Python 数据模型、特殊方法、元编程以及解释器如何在幕后工作。此外，有些重复、循环讨论和过度简化是不可避免的——例如，我们必须坚持用对象来解释一个类的行为。

# **1。如何写一个基础类**

让我们为 Foo 小姐阁下*写一个基础类，她是我们当地一位著名的后摇滚吉他练习者，她要求我们写一个程序来跟踪她的学生的 ***姓名******金钱*** ，他们将支付给她并分配一个带有她的域名的 ***电子邮件*** 地址。她自己是一名开发人员，但是每天的日程安排很紧，她向像我们这样懒惰的程序员寻求帮助。然而，她将把这个蓝图或类用到自己的脚本中。现实生活中代码重用的例子，哈！让我们帮助她。*

```
*>>> class Student():     # ...[1]
       pass>>> student_1 = Student() # ... [2]
>>> student_2 = Student() # ... [3]>>> student_1, student_2 # ... [4]
(<__main__.Student object at 0x7fec1b6387b8>, <__main__.Student object at 0x7fec1b638780>)>>> print(id(student_1), id(student_2)) # ... [5]
140652048517048 140652048516992>>> type(student_1)  # .......... [6]
<class '__main__.Student'>*
```

*我们刚刚编写了一个框架类`Student`并给解释器传递了一个`pass`语句，告诉她在我们给它赋值之前保持它的完整性。我们还在语句`[2]`和`[3]`中创建了两个`Student`类实体。这里需要注意的最重要的两点是，尽管来自同一个类`Student` , `student_1`和`student_2`在 RAM 语句`[5]`和`[4]`中具有不同的身份和位置。*

*一个星期天的早上，安德鲁·汤普森被她的班级录取了。 ***傅小姐*** 每个月会收他 ***五千*** 块钱。让我们加上他的名字*

```
*>>> student_1.firstname = "Andrew"
>>> student_1.lastname = "Thompson"
>>> student_1.pays = 5000
>>> student_1.mail = student_1.firstname.lower()+'_'+student_1.lastname.lower()+"@msfooguitar.edu"
>>> student_1.mail
'andrew_thompson@msfooguitar.edu'*
```

*一个星期五的早上，安德鲁的朋友马克·伯德报名参加了这个班。因为他是个初学者，所以傅小姐需要对他付出更多的努力和关注。马克每月将支付 ***6000*** 美元。让我们把他的名字登记在登记簿上。*

```
*>>> student_2.firstname = "Marc"
>>> student_2.lastname = "Byrd"
>>> student_2.pays = 6000
>>> student_2.mail = student_2.firstname.lower()+'_'+student_2.lastname.lower()+"@msfooguitar.edu"
>>> student_2.mail
'marc_byrd@msfooguitar.edu'*
```

*请注意，我们在上述示例中采用的方法符合我们的目的，但使进一步的工作变得复杂。*

*   *我们重复地给每个对象分配实例。当类将产生更多的对象时，这将进一步增加复杂性。*
*   *我们将需要在需要的时候定义类。当前的例子对我们没有帮助。*

*我们可以通过避免重复和确保代码的可重用性来使情况变得更加灵活，具体方法是编写一个蓝图，描述`Student`类及其对象将获得什么属性。*

```
*class Student():
   def __init__(self, fname, lname, pays): # ... [1] an initialiser.
      self.fname = fname
      self.lname = lname
      self.pays = pays
      self.mail = f"{self.fname.lower()}@msfooguitar.edu"

student_1 = Student("Andrew", "Thompson", 5000)
student_2 = Student("Marc", "Byrd", 6000)*
```

*现在我们所拥有的是，*

*   *类`Student`现在的行为就像一个表单，有三个必填字段——名、姓、支付的金额以及一个额外的电子邮件地址——由每个新学生或对象在注册课程时填写。这样编码器可以避免重复。*
*   *`__init__`魔法方法(将在几分钟内讨论)初始化每个新的`Student`对象/实例将获得的变量、字段或属性。在创建类的每个新对象时调用此方法。记住`__init__`不是*构造器*。在 Python 中，`__new__`是构造函数*
*   *你有没有注意到初始值设定项中传递了`self`参数？当从一个类中创建一个对象时，解释器将首先把对象本身传递给`__init__`方法，这个方法通常用`self`参数表示。我们可以使用除了`self`之外的任何参数名。*

```
*class Student():
  def __init__(self, fname, lname, pays): # ... [1] an initialiser.
    self.fname = fname
    self.lname = lname
    self.pays = pays
    self.mail = f"{self.fname.lower()}@msfooguitar.edu"

  def details(self):
    print(f"name: {self.fname} {self.lname} pays: {self.pays} mail:\
     {self.mail}")

student_1 = Student("Andrew", "Thompson", 5000)# ... [2]
student_2 = Student("Marc", "Byrd", 6000)print(student_1.details(), student_2.details()) # Output
'''
name: Andrew Thompson pays: 5000 mail: andrew_thompson@msfooguitar.edu
name: Marc Byrd pays: 6000 mail: marc_byrd@msfooguitar.edu
'''*
```

*语句`[2]`可以重写为 Class.objectMethod(object)。在我们这里相当于`Student.details(student_1)`。*

*有趣的是，我们可以像动态变量一样覆盖实例方法。比如，我们可以把`details()`这个可调用的函数改成一个新的字符串。*

```
*student_1.details = "A new string."
print(student_1.__dict__)

# Output
'''
{'fname': 'Andrew',
'lname': 'Thompson',
'pays': 5000,
'mail': 'andrew_thompson@msfooguitar.edu',
'details': 'A new string.'}
'''*
```

*注意`details`现在已经变成了一个不可调用的字符串对象。__dict__ 是一个字典，包含了对象拥有的变量。*

*注意`details`现在已经变成了一个不可调用的字符串对象。*

```
*print(student_1.details())
# Output
'''
Traceback (most recent call last)
...
---> 19 print(student_1.details())
TypeError: 'str' object is not callable
'''*
```

# *2.**类和对象:硬币的两面***

*继续本系列的第一部分，关于 Python 数据模型的讨论，我们会经常遇到“陈词滥调”和重复的概念 ***“在 Python 中，一切都是对象。”****

> *它们或者是原型或者是*类型*的*元类*或者是一些其他类的对象*

*这可能与我们刚刚开始我们的 ****nix*** 之旅的时候相比，我们被告知至少数百次，从文本文件到键盘 ***的一切都是文件。***
在 Python 3.x 中，类也是对象。也就是说，我们可以把一个类当作一个对象。我们可以像前面提到的例子一样动态地操作和修改一个类:*

```
*>>> class ObjClass(): pass
...
>>> ObjClass.sentence = "A class is an Object" # ...[1]
>>> ObjClass.sentence 
'A class is an Object'>>> type(ObjClass) # ... [2]
<class 'type'>>>> print(ObjClass)
<class '__main__.ObjClass'>
>>> ObjClass = "I'm a string now" # ... [3]>>> type(ObjClass)
<class 'str'>*
```

*通过把类本身当作一个对象来对待，**我们可以改变它的类型** `[3]`为什么这是可能的？
准确地说，我们可以运行一个循环来查看我们的常规数据类型，甚至`type`本身也是从`type` *元类*中派生出来的。我们将有一章深入讨论*元类。目前，*元类*从其他类获取属性，生成一个具有附加功能的新类。**

```
*class MyClass():
    pass

myObj = MyClass()

for x in str, int, float, type, True, bool, myObj, MyClass:
    print(f'{x} : {type(x)}') # Output
'''
<class 'str'> : <class 'type'>
<class 'int'> : <class 'type'>
<class 'float'> : <class 'type'>
<class 'type'> : <class 'type'>
True : <class 'bool'> # ...
<class 'bool'> : <class 'type'> # ... [2]
<__main__.MyClass object at 0x7f70d8380550> : <class '__main__.MyClass'>
<class '__main__.MyClass'> : <class 'type'>
'''*
```

*所以，从上面的例子可以清楚地看出，Python 中的类和对象在本质上是一样的。它们或者是原型或者是*类型*的*元类*或者是一些其他类的对象*

# *3.旧式班级和新式班级的区别*

*一个旧样式的类曾经被声明为`class A()`，而一个新样式是`class A(object)`，它“从*对象*继承，或者从另一个新样式的类继承。”根据[文档](https://docs.python.org/2/reference/datamodel.html#new-style-and-classic-classes):*

> *“在 Python 2.1 之前，旧式类是用户可以使用的唯一风格。(旧式)类的概念与类型的概念无关:如果 x 是旧式类的实例，那么`x.__class__`指定 x 的类，但是`type(x)`总是`<type 'instance'>`。
> ...这反映了这样一个事实:所有旧式的实例，不管它们的类是什么，都是用一个内置类型实现的，这个内置类型叫做 instance。
> ...引入新型类的主要动机是提供一个具有完整元模型的统一对象模型。*

*让我们来看一个例子*

1.  *旧式班级:*

```
*#python 2.1
class Student(): _names_cache = {}
  def __init__(self, fname):
    self.fname = fname

  def __new__(cls,fname):
    return cls._names_cache
    .setdefault(name,object.__new__(cls,fname)) student_1 = Student("Andrew")
student_2 = Student("Andrew")
print student_1 is student_2
print student_1
print student_2

>>> False
<__main__.Student instance at 0xb74acf8c>
<__main__.Student instance at 0xb74ac6cc>
>>>*
```

*2.新型类:*

```
*#python 2.7
class Student(): _names_cache = {}
  def __init__(self, fname):
    self.fname = fname def __new__(cls,fname):
    return cls._names_cache
    .setdefault(fname,object.__new__(cls,fname))student_1 = Student("Andrew")
student_2 = Student("Andrew")
print student_1 is student_2
print student_1
print student_2

>>> True
<__main__.Student instance at 0xb74acf8c>
<__main__.Student instance at 0xb74ac6cc>
>>>*
```

*Python 3 只有一个新样式的类。即使你写的是‘旧式类’，也是从`object`隐式派生的。新式职业有一些老式职业所缺乏的高级特征，比如`super`，新的 [C3 mro](http://en.wikipedia.org/wiki/C3_linearization) ，一些神奇的方法等等。请记住，我们在整篇文章中都使用 Python 3.x。*

*[Guido](https://gvanrossum.github.io/) 已经写了 [*关于新型类的内幕*](http://python-history.blogspot.com/2010/06/inside-story-on-new-style-classes.html) ，这是一篇关于 Python 中新型和旧式类的非常棒的文章。*

*在[的下一部分](/all-about-pythonic-class-the-life-cycle-5d2deae1ad8d)，我们将讨论类的生命周期*

# *参考资料:*

1.  *[https://docs.python.org/3/reference/datamodel.html](https://docs.python.org/3/reference/datamodel.html)*
2.  *卢西亚诺·拉马尔霍的流畅 Python。这是每一个 python 爱好者都应该读的书，以提高他们的 python 技能。*