# 对象、类型和值

> 原文：<https://towardsdatascience.com/python-data-model-part-1-objects-types-and-values-cb9316f57e8?source=collection_archive---------13----------------------->

## Python 数据模型

## Python 数据模型—第 1 部分

![](img/c57de6c8a08113329cb90e3a87273927.png)

克里斯里德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

> **Python 最好的品质之一就是它的一致性。在使用 Python 一段时间后，您能够开始对新特性做出明智、正确的猜测**。****
> 
> **“Python 数据模型，它描述了 API，您可以使用该 API 使您自己的对象与最惯用的语言特性配合良好。**—流畅的 Python，卢西亚诺·拉马尔霍****

*****Python 数据模型*可以定义为*“Python 框架”*或*“Python 设计哲学”*。这种语言的开发者已经开发了相当大的文档，乍一看可能有点模糊、吓人或者含糊不清。****

****然而，简单地说，术语*数据模型*是不言自明的——它处理 Python 如何在内部组织一切以处理和处理数据，以及其核心设计和哲学的一些高级概念。它讨论了语言本身的基本构造块、构造块的设计/结构以及与它们相关的基本代码块。****

****在本章中，我们将讨论**对象**、**类型和值******

> ****在 Python 中——一切都是对象。每个对象都有一个身份、类型和值****

****在 Python 中——一切都是对象。每个对象都有一个**标识**，一个**类型**，一个**值**。那么，**身份**、**类型、**和**值有什么大惊小怪的？我们将编写一些简单的代码—******

```
**>>> player_1 = "Maradona"
>>> id(player_1)
139780790567600
>>> age = 53
>>> id(age)
9753824**
```

> ****我们可以认为身份是一个对象在内存中的地址****

****在这里， **id()** 给出了我们正在谈论的**标识**，它是一个惟一的整数，在一个对象的生命周期中不会改变。我们可以认为身份是一个对象在内存中的地址。我们可以用**操作符**检查两个对象是否相同。****

```
**>>> player_2 = "Pele"
>>> player_1 is player_2     #They are not referring the same object
False >>> player_copy = player_1
>>> player_1 is player_copy   #They are referring the same object
True>>> id(player_2)
139780790567984
>>> id(player_copy)
139780790567600**
```

****现在，如果我们想检查这些对象的**类型**，我们可以使用 **type()** 。****

```
**>>> type(age)
<class ‘int’>
>>> type(player_1)
<class 'str'>**
```

> ******物体的类型不能改变******

****对象的**类型**不能改变****

*****在某些情况下，在某些受控条件下，可以改变对象的类型。但这通常不是一个好主意，因为如果处理不当，它会导致一些非常奇怪的行为。*****

******类型**指定了两件事:
–允许哪些操作
–对象可以保存的一组值****

> ******如果对象是*可变的*对象的值可以改变，如果对象是*不可变的*对象的值不能改变。******

****并且，一个对象的**值**是该对象在其中保存的内容。这里`53`是`age`的值，`Maradona`是`player_1`的值。如果对象是 ***可变*** 对象的**值**可以改变，如果对象是 ***不可变*** 对象的值不能改变。****

> ****`**age = 53**` **是一个值为 53 的整型对象，id 为 9753824******

****这意味着`age = 53`是一个整数**类型的**对象，值为**53，id 为**9753824，如果我们现在将`age`改为`54`，它将引用一个不同的对象********

```
**>>> age = 54 # here, age is referring to a different object
>>> id(age)  # showing different **identity** than the previous one 
9753856**
```

****因此，如果我们想改变`player_1`的值，那么对象`player_copy`和`player_1`将不会显示相同的**标识******

```
**>>> player_1 = "Messi" # assigning new value to 
>>> player_copy
'Maradona'
>>> player_1 is player_copy
False**
```

****因此，`player_1`又名*字符串*和`age`又名*整数*都是**不可变**类型。还有其他不可变类型。如— *整数、浮点数、字符串、元组*。另一方面，**可变**类型是*列表*、*字典*和*集合。*现在，看看**可变**类型****

```
**>>> players = ["Messi", "Maradona", "Pele"]
>>> players_copy = players     # Coping the reference to the list
                                object,not the list object itself.>>> players is players_copy
True>>> players[0]="Ronaldo"          # Changing the first element
>>> players
['Ronaldo', 'Maradona', 'Pele']
>>> players_copy
['Ronaldo', 'Maradona', 'Pele']   # They refer to same list object.>>> players.append("Pirlo")       # Appending a new element
>>> players
['Ronaldo', 'Maradona', 'Pele', 'Pirlo']
>>> players_copy
['Ronaldo', 'Maradona', 'Pele', 'Pirlo']>>> players_copy.append("Cruyf")
>>> players_copy
['Ronaldo', 'Maradona', 'Pele', 'Pirlo', 'Cruyf']
>>> players
['Ronaldo', 'Maradona', 'Pele', 'Pirlo', 'Cruyf']**
```

> ******由于** `**players**` **和** `**players_copy**` **引用同一个*列表*对象，一个*列表*的变化会影响另一个变量。******

****在上面的例子中，我们通过改变第一个值并使用`append()`方法来变异`players` *列表*。由于`players`和`players_copy`引用同一个*列表*对象，一个*列表*的变化会影响另一个变量。****

```
**>>> id(players)
139780790567040
>>> id(players_copy)
139780790567040**
```

****注意，如果我们改变一个变量所引用的*列表*，它将创建一个新的*列表*对象。我们不应该混淆突变和改变变量。****

```
**>>> players = ["Messi", "Maradona", "Pele"]
>>> id(players)
139780790567040>>> players = ["Messi", "Maradona", "Pele", 'Pirlo', 'Cruyf']
>>> id(players) 
139780790568064**
```

****在这里，这一行`players = [“Messi”, “Maradona”, “Pele”, ‘Pirlo’, ‘Cruyf’]`创建了一个新的列表并将`players`变量引用到其中。我们可以通过检查两个身份证来判断。****

****为了更清楚地理解**可变性**和**不变性，**让我们把*元组*带到射击区。我们前面说过，*元组*是**不可变的，**突变*元组* **类型**应该不可能。****

```
**>>> players_tuple = ("Kaka", "Buffon")
>>> players_tuple
('Kaka', 'Buffon')
>>> type(players_tuple)
<class 'tuple'>
>>> id(players_tuple)
139780790517248>>> players_tuple[0]
'Kaka'
>>> players_tuple[0]="De Bruyne"     # can't mutate in tuples
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment>>> players_tuple= players_tuple +("De Bruyne",) # adding a new data
>>> players_tuple
('Kaka', 'Buffon', 'De Bruyne')
>>> id(players_tuple)                # it's creating a new tuple
139780766363584**
```

****在这一部分，我们试图清楚地理解一个对象如何在 pythonic 世界中保持其本质，以及**可变性**和**不变性**之间的区别。在后面的章节中，我们将更深入地研究 python 数据模型。****

****参考文献:
1。[https://docs.python.org/3/reference/datamodel.html](https://docs.python.org/3/reference/datamodel.html)2。卢西亚诺·拉马尔霍的流畅 Python。这是每一个 python 爱好者都应该读的书，以提高他们的 python 技能。****