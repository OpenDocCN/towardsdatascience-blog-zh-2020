# 53 Python 面试问答

> 原文：<https://towardsdatascience.com/53-python-interview-questions-and-answers-91fa311eec3f?source=collection_archive---------0----------------------->

## 面向数据科学家和软件工程师的 Python 问题

![](img/dbbff25e5d35ebe60d61cf30c9719f5b.png)

由[布鲁克·卡吉尔](https://unsplash.com/@brookecagle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

不久前，我开始担任“数据科学家”的新角色，实际上是“Python 工程师”。

如果我提前温习 Python 的线程生命周期而不是推荐系统，我会准备得更充分。

本着这种精神，以下是我的 python 面试/工作准备问答。大多数数据科学家都写了很多代码，所以这对科学家和工程师都适用。

无论你是在面试候选人，准备申请工作，还是只是在温习 Python，我认为这个列表都是非常宝贵的。

问题无序。我们开始吧。

## 1.列表和元组有什么区别？

在我参加的每一次 python /数据科学面试中，我都会被问到这个问题。对答案了如指掌。

*   列表是可变的。它们可以在创建后进行修改。
*   元组是不可变的。元组一旦创建，就不能更改
*   列表是有顺序的。它们是有序的序列，通常属于同一类型的对象。Ie:按创建日期排序的所有用户名，`["Seth", "Ema", "Eli"]`
*   元组有结构。每个索引可能存在不同的数据类型。Ie:内存中的一个数据库记录，`(2, "Ema", "2020–04–16") # id, name, created_at`

## 2.字符串插值是如何执行的？

在不导入`Template`类的情况下，有 3 种方法可以插入字符串。

```
name = 'Chris'# 1\. f strings
print(f'Hello {name}')# 2\. % operator
print('Hey %s %s' % (name, name))# 3\. format
print(
 "My name is {}".format((name))
)
```

## 3.“是”和“==”有什么区别？

在我 python 职业生涯的早期，我认为这些都是一样的…你好，臭虫。因此，根据记录，`is`检查身份，`==`检查平等。

我们将浏览一个示例。创建一些列表，并将它们分配给名称。注意`b`与下面的`a`指向同一个对象。

```
a = [1,2,3]
b = a
c = [1,2,3]
```

检查等式并注意它们都是相等的。

```
print(a == b)
print(a == c)
#=> True
#=> True
```

但是他们有相同的身份吗？没有。

```
print(a is b)
print(a is c)
#=> True
#=> False
```

我们可以通过打印他们的对象 id 来验证这一点。

```
print(id(a))
print(id(b))
print(id(c))
#=> 4369567560
#=> 4369567560
#=> 4369567624
```

`c`的`id`与`a`和`b`不同。

## 4.什么是室内设计师？

我在每次面试中都会被问到的另一个问题。这本身就值得一贴，但是如果你能写下自己的例子，你就已经准备好了。

装饰器允许通过将现有函数传递给装饰器来为现有函数添加功能，装饰器执行现有函数和附加代码。

我们将编写一个装饰器，在调用另一个函数时记录日志。

**编写装饰函数。**这需要一个函数`func`作为参数。它还定义了一个函数`log_function_called`，这个函数调用`func()`并执行一些代码`print(f'{func} called.')`。然后它返回它定义的函数

```
def logging(func):
  def log_function_called():
    print(f'{func} called.')
    func()
  return log_function_called
```

让我们编写最终将添加装饰器的其他函数(但现在还没有)。

```
def my_name():
  print('chris')def friends_name():
  print('naruto')my_name()
friends_name()
#=> chris
#=> naruto
```

现在将装饰器添加到两者中。

```
[@logging](http://twitter.com/logging)
def my_name():
 print('chris')[@logging](http://twitter.com/logging)
def friends_name():
 print('naruto')my_name()
friends_name()
#=> <function my_name at 0x10fca5a60> called.
#=> chris
#=> <function friends_name at 0x10fca5f28> called.
#=> naruto
```

看看我们现在如何通过在上面添加`@logging`来轻松地将日志添加到我们编写的任何函数中。

## 5.解释范围函数

Range 生成一个整数列表，有 3 种方法可以使用它。

该函数有 1 到 3 个参数。注意，我将每种用法都包装在列表理解中，这样我们就可以看到生成的值。

`**range(stop)**` **:** 生成从 0 到“停止”整数的整数。

```
[i for i in range(10)]
#=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

`**range(start, stop)**` **:** 生成从“开始”到“停止”的整数。

```
[i for i in range(2,10)]
#=> [2, 3, 4, 5, 6, 7, 8, 9]
```

`**range(start, stop, step)**` **:** 以“步”为间隔，从“开始”到“停止”生成整数。

```
[i for i in range(2,10,2)]
#=> [2, 4, 6, 8]
```

感谢塞尔格·博瑞姆丘克建议了一种更 pythonic 化的方式来做这件事！

```
list(range(2,10,2))
#=> [2, 4, 6, 8]
```

## 6.定义一个名为 car 的类，它有两个属性，“颜色”和“速度”。然后创建一个实例，返回速度。

```
class Car :
    def __init__(self, color, speed):
        self.color = color
        self.speed = speedcar = Car('red','100mph')
car.speed
#=> '100mph'
```

## 7.python 中的实例、静态和类方法有什么区别？

**实例方法:**接受`self`参数，并关联到类的一个特定实例。

**静态方法:**使用`@staticmethod`装饰器，与特定实例无关，并且是独立的(不要修改类或实例属性)

**类方法:**接受`cls`参数并可以修改类本身

我们将围绕一个虚构的`CoffeeShop`类来说明这种差异。

```
class CoffeeShop:
    specialty = 'espresso'

    def __init__(self, coffee_price):
        self.coffee_price = coffee_price

    # instance method
    def make_coffee(self):
        print(f'Making {self.specialty} for ${self.coffee_price}')

    # static method    
    [@staticmethod](http://twitter.com/staticmethod)
    def check_weather():
        print('Its sunny') # class method
    [@classmethod](http://twitter.com/classmethod)
    def change_specialty(cls, specialty):
        cls.specialty = specialty
        print(f'Specialty changed to {specialty}')
```

`CoffeeShop`类有一个属性`specialty`，默认设置为`'espresso'`。`CoffeeShop`的每个实例都用属性`coffee_price`初始化。它也有 3 个方法，一个实例方法，一个静态方法和一个类方法。

让我们用`5`的`coffee_price`初始化咖啡店的一个实例。然后调用实例方法`make_coffee`。

```
coffee_shop = CoffeeShop('5')
coffee_shop.make_coffee()
#=> Making espresso for $5
```

现在调用静态方法。静态方法不能修改类或实例状态，所以它们通常用于实用函数，例如，将两个数相加。我们用我们的来查天气。`Its sunny`。太好了！

```
coffee_shop.check_weather()
#=> Its sunny
```

现在我们用类的方法修改咖啡店的特产然后`make_coffee`。

```
coffee_shop.change_specialty('drip coffee')
#=> Specialty changed to drip coffeecoffee_shop.make_coffee()
#=> Making drip coffee for $5
```

注意`make_coffee`以前是怎么做`espresso`的，现在是怎么做`drip coffee`的！

## 8.「func」和「func()」有什么区别？

这个问题的目的是看你是否明白 python 中所有的函数也是对象。

```
def func():
    print('Im a function')

func
#=> function __main__.func>func()    
#=> Im a function
```

`func`是代表函数的对象，该函数可被赋给一个变量或传递给另一个函数。`func()`用括号调用函数并返回它输出的内容。

## 9.解释地图功能的工作原理

`map`返回一个映射对象(一个迭代器),它可以对序列中的每个元素应用一个函数而返回的值进行迭代。如果需要，地图对象也可以转换为列表。

```
def add_three(x):
    return x + 3li = [1,2,3][i for i in map(add_three, li)]
#=> [4, 5, 6]
```

上面，我给列表中的每个元素加了 3。

一位读者建议了一个更 pythonic 化的实现。感谢[克里斯扬·伍斯特](https://medium.com/u/b3236beb43a6?source=post_page-----91fa311eec3f--------------------------------)！

```
def add_three(x):
    return x + 3li = [1,2,3]
list(map(add_three, li))
#=> [4, 5, 6]
```

此外，感谢[迈克尔格雷姆肖特](https://medium.com/u/a582f1392208?source=post_page-----91fa311eec3f--------------------------------)的更正！

## 10.解释 reduce 函数的工作原理

这可能会很棘手，直到你使用它几次。

`reduce`接受一个函数和一个序列，并对该序列进行迭代。在每次迭代中，当前元素和前一个元素的输出都被传递给函数。最后，返回一个值。

```
from functools import reducedef add_three(x,y):
    return x + yli = [1,2,3,5]reduce(add_three, li)
#=> 11
```

返回`11`，它是`1+2+3+5`的和。

## 11.解释过滤函数的工作原理

Filter 确实如其名。它过滤序列中的元素。

每个元素被传递给一个函数，如果该函数返回`True`，则该函数以输出序列返回，如果该函数返回`False`，则该元素被丢弃。

```
def add_three(x):
    if x % 2 == 0:
        return True        
    else:
        return Falseli = [1,2,3,4,5,6,7,8][i for i in filter(add_three, li)]
#=> [2, 4, 6, 8]
```

请注意所有不能被 2 整除的元素是如何被删除的。

## 12.python 是按引用调用还是按值调用？

如果你在谷歌上搜索这个问题并阅读了上面的几页，就要做好陷入语义陷阱的准备。

简而言之，所有的名字都是通过引用来调用的，但是一些内存位置保存对象，而另一些保存指向其他内存位置的指针。

```
name = 'object'
```

让我们看看这是如何处理字符串的。我们将实例化一个名称和对象，将其他名称指向它。然后删除名字。

```
x = 'some text'
y = x
x is y #=> Truedel x # this deletes the 'a' name but does nothing to the object in memoryz = y
y is z #=> True
```

我们看到的是，所有这些名字都指向内存中的同一个对象，它不受`del x`的影响。

这是另一个有趣的函数例子。

```
name = 'text'def add_chars(str1):
    print( id(str1) ) #=> 4353702856
    print( id(name) ) #=> 4353702856

    # new name, same object
    str2 = str1

    # creates a new name (with same name as the first) AND object
    str1 += 's' 
    print( id(str1) ) #=> 4387143328

    # still the original object
    print( id(str2) ) #=> 4353702856

add_chars(name)
print(name) #=>text
```

注意在函数内部的字符串中添加一个`s`是如何创建一个新名称和一个新对象的。即使新名称与现有名称具有相同的“名称”。

感谢[迈克尔·p·雷利](https://medium.com/u/e0fac81c6192?source=post_page-----91fa311eec3f--------------------------------)的指正！

## 13.如何反转一个列表？

请注意`reverse()`是如何在列表上被调用并对其进行变异的。它不会返回变异后的列表本身。

```
li = ['a','b','c']print(li)
li.reverse()
print(li)
#=> ['a', 'b', 'c']
#=> ['c', 'b', 'a']
```

## 14.字符串乘法是如何工作的？

我们来看看字符串`‘cat’`乘以 3 的结果。

```
'cat' * 3
#=> 'catcatcat'
```

该字符串自身连接了三次。

## 15.列表乘法是如何工作的？

让我们看看一个列表`[1,2,3]`乘以 2 的结果。

```
[1,2,3] * 2
#=> [1, 2, 3, 1, 2, 3]
```

输出包含重复两次的[1，2，3]的内容的列表。

## 16.一个班里的“自己”指的是什么？

Self 指的是类本身的实例。这就是我们如何给予方法访问和更新它们所属对象的能力。

下面，将 self 传递给`__init__()`使我们能够在初始化时设置实例的`color`。

```
class Shirt:
    def __init__(self, color):
        self.color = color

s = Shirt('yellow')
s.color
#=> 'yellow'
```

## 17.如何在 python 中连接列表？

将两个列表相加会将它们连接起来。请注意，数组的工作方式不同。

```
a = [1,2]
b = [3,4,5]a + b
#=> [1, 2, 3, 4, 5]
```

## 18.浅拷贝和深拷贝有什么区别？

我们将在一个可变对象(列表)的上下文中讨论这个问题。对于不可变的对象，浅和深并不相关。

我们将经历 3 个场景。

**i)引用原始对象。**这个新名字`li2`指向内存中`li1`指向的相同位置。所以我们对`li1`所做的任何改变也会发生在`li2`上。

```
li1 = [['a'],['b'],['c']]
li2 = li1li1.append(['d'])
print(li2)
#=> [['a'], ['b'], ['c'], ['d']]
```

**ii)创建原始文件的浅拷贝。**我们可以用`list()`构造函数，或者更 pythonic 化的`mylist.copy()`(感谢 [Chrisjan Wust](https://medium.com/u/b3236beb43a6?source=post_page-----91fa311eec3f--------------------------------) ！).

浅表副本创建一个新对象，但用对原始对象的引用填充它。所以向原始集合添加一个新对象`li3`，不会传播到`li4`，但是修改`li3`中的一个对象会传播到`li4`。

```
li3 = [['a'],['b'],['c']]
li4 = list(li3)li3.append([4])
print(li4)
#=> [['a'], ['b'], ['c']]li3[0][0] = ['X']
print(li4)
#=> [[['X']], ['b'], ['c']]
```

**iii)创建深层副本。**这是通过`copy.deepcopy()`完成的。这两个对象现在完全独立，对其中一个对象的更改不会影响另一个对象。

```
import copyli5 = [['a'],['b'],['c']]
li6 = copy.deepcopy(li5)li5.append([4])
li5[0][0] = ['X']
print(li6)
#=> [['a'], ['b'], ['c']]
```

## 19.列表和数组有什么区别？

*注意:Python 的标准库有一个数组对象，但这里我特指常用的 Numpy 数组。*

*   列表存在于 python 的标准库中。数组由 Numpy 定义。
*   列表可以在每个索引处填充不同类型的数据。数组需要同质元素。
*   列表算术在列表中添加或删除元素。每个线性代数上的数组函数的算术。
*   阵列还使用更少的内存，并提供更多的功能。

我写了另一篇关于数组的综合文章。

## 20.如何连接两个数组？

记住，数组不是列表。数组来自 Numpy 和算术函数，如线性代数。

我们需要使用 Numpy 的 concatenate 函数来实现。

```
import numpy as npa = np.array([1,2,3])
b = np.array([4,5,6])np.concatenate((a,b))
#=> array([1, 2, 3, 4, 5, 6])
```

## 21.你喜欢 Python 的什么？

*请注意，这是一个非常主观的问题，你需要根据角色的需求来修改你的回答。*

Python 可读性很强，几乎所有事情都有 python 式的方法，这意味着这是一种清晰简洁的首选方法。

我将此与 Ruby 进行对比，在 Ruby 中，通常有许多方法来做某件事，但没有哪种方法更好的指导原则。

## 22.你最喜欢的 Python 库是什么？

*亦主观，见问题 21。*

当处理大量数据时，没有什么比 pandas 更有用了，它使处理和可视化数据变得轻而易举。

## 23.命名可变和不可变对象

不可变意味着状态在创建后不能修改。例如:int、float、bool、string 和 tuple。

可变意味着状态在创建后可以修改。例子有列表、字典和集合。

## 24.你如何将一个数字四舍五入到小数点后三位？

使用`round(value, decimal_places)`功能。

```
a = 5.12345
round(a,3)
#=> 5.123
```

## 25.你如何分割一个列表？

切片表示法有 3 个参数，`list[start:stop:step]`，其中 step 是返回元素的间隔。

```
a = [0,1,2,3,4,5,6,7,8,9]print(a[:2])
#=> [0, 1]print(a[8:])
#=> [8, 9]print(a[2:8])
#=> [2, 3, 4, 5, 6, 7]print(a[2:8:2])
#=> [2, 4, 6]
```

## 26.什么是腌制？

Pickling 是 Python 中序列化和反序列化对象的常用方法。

在下面的例子中，我们序列化和反序列化一个字典列表。

```
import pickleobj = [
    {'id':1, 'name':'Stuffy'},
    {'id':2, 'name': 'Fluffy'}
]with open('file.p', 'wb') as f:
    pickle.dump(obj, f)with open('file.p', 'rb') as f:
    loaded_obj = pickle.load(f)print(loaded_obj)
#=> [{'id': 1, 'name': 'Stuffy'}, {'id': 2, 'name': 'Fluffy'}]
```

## 27.字典和 JSON 有什么区别？

Dict 是 python 数据类型，是一个索引但无序的键和值的集合。

JSON 只是一个遵循特定格式的字符串，用于传输数据。

## 28.你在 Python 中用过哪些 ORM？

ORMs(对象关系映射)将数据模型(通常在应用程序中)映射到数据库表，并简化数据库事务。

SQLAlchemy 通常用在 Flask 的上下文中，Django 有自己的 ORM。

## 29.any()和 all()是如何工作的？

**Any** 接受一个序列，如果序列中的任何元素为真，则返回真。

**All** 仅当序列中的所有元素都为真时返回真。

```
a = [False, False, False]
b = [True, False, False]
c = [True, True, True]print( any(a) )
print( any(b) )
print( any(c) )
#=> False
#=> True
#=> Trueprint( all(a) )
print( all(b) )
print( all(c) )
#=> False
#=> False
#=> True
```

## 30.字典或列表查找更快吗？

在列表中查找一个值需要 O(n)时间，因为需要遍历整个列表直到找到该值。

在字典中查找一个键需要 O(1)时间，因为它是一个散列表。

如果有很多值，这会造成很大的时间差，因此为了加快速度，通常建议使用字典。但是它们也有其他的限制，比如需要唯一的键。

## 31.模块和包的区别是什么？

模块是可以一起导入的文件(或文件集合)。

```
import sklearn
```

包是模块的目录。

```
from sklearn import cross_validation
```

所以包是模块，但不是所有的模块都是包。

## 32.Python 中如何对一个整数进行加减运算？

可以用`+-`和`-=`来增加和减少。

```
value = 5value += 1
print(value)
#=> 6value -= 1
value -= 1
print(value)
#=> 4
```

## 33.如何返回整数的二进制？

使用`bin()`功能。

```
bin(5)
#=> '0b101'
```

## 34.如何从列表中删除重复的元素？

这可以通过将列表转换成集合，然后再转换回列表来实现。

```
a = [1,1,1,2,3]
a = list(set(a))
print(a)
#=> [1, 2, 3]
```

*注意，集合不一定保持列表的顺序。*

## 35.如何检查列表中是否有值？

使用`in`。

```
'a' in ['a','b','c']
#=> True'a' in [1,2,3]
#=> False
```

## 36.追加和扩展有什么区别？

`append`将一个值添加到一个列表，而`extend`将另一个列表中的值添加到一个列表。

```
a = [1,2,3]
b = [1,2,3]a.append(6)
print(a)
#=> [1, 2, 3, 6]b.extend([4,5])
print(b)
#=> [1, 2, 3, 4, 5]
```

## 37.如何取整数的绝对值？

这可以通过`abs()`功能完成。

```
abs(2)
#=> 2abs(-2)
#=> 2
```

## 38.如何将两个列表合并成一个元组列表？

您可以使用`zip`函数将列表组合成一个元组列表。这并不局限于只使用两个列表。也可以用 3 个或更多。

```
a = ['a','b','c']
b = [1,2,3][(k,v) for k,v in zip(a,b)]
#=> [('a', 1), ('b', 2), ('c', 3)]
```

## 39.你如何按字母顺序按关键字给字典排序？

您不能“排序”字典，因为字典没有顺序，但是您可以返回一个排序的元组列表，其中包含字典中的键和值。

```
d = {'c':3, 'd':4, 'b':2, 'a':1}sorted(d.items())
#=> [('a', 1), ('b', 2), ('c', 3), ('d', 4)]
```

## 40.Python 中一个类如何继承另一个类？

在下面的例子中，`Audi`继承自`Car`。随着这种继承而来的是父类的实例方法。

```
class Car():
    def drive(self):
        print('vroom')class Audi(Car):
    passaudi = Audi()
audi.drive()
```

## 41.如何从字符串中删除所有空格？

最简单的方法是用空格分割字符串，然后不用空格重新连接。

```
s = 'A string with     white space'''.join(s.split())
#=> 'Astringwithwhitespace'
```

2 读者推荐了一种更 Python 化的方式来处理这个问题，这种方式遵循了`Explicit is better than Implicit`的 Python 精神。它也更快，因为 python 不创建新的列表对象。感谢евгенийкрамаров和 [Chrisjan Wust](https://medium.com/u/b3236beb43a6?source=post_page-----91fa311eec3f--------------------------------) ！

```
s = 'A string with     white space'
s.replace(' ', '')
#=> 'Astringwithwhitespace'
```

## 42.为什么在对序列进行迭代时要使用 enumerate()。

`enumerate()`迭代序列时允许跟踪索引。这比定义和递增一个表示索引的整数更有技巧。

```
li = ['a','b','c','d','e']for idx,val in enumerate(li):
    print(idx, val)
#=> 0 a
#=> 1 b
#=> 2 c
#=> 3 d
#=> 4 e
```

## 43.传球、继续、突破有什么区别？

`pass`指无所事事。我们通常使用它，因为 Python 不允许在没有代码的情况下创建类、函数或 if 语句。

在下面的例子中，如果在`i > 3`中没有代码，将会抛出一个错误，所以我们使用`pass`。

```
a = [1,2,3,4,5]for i in a:
    if i > 3:
        pass
    print(i)
#=> 1
#=> 2
#=> 3
#=> 4
#=> 5
```

`continue`继续执行下一个元素，并停止执行当前元素。所以对于`i < 3`处的值，永远不会达到`print(i)`。

```
for i in a:
    if i < 3:
        continue
    print(i)
#=> 3
#=> 4
#=> 5
```

`break`中断循环，序列不再迭代。因此不打印从 3 开始的元素。

```
for i in a:
    if i == 3:
        break
    print(i)    
#=> 1
#=> 2
```

## 44.将下面的 for 循环转换成列表理解。

这个`for`循环。

```
a = [1,2,3,4,5]

a2 = []
for i in a:
     a2.append(i + 1)print(a2)
#=> [2, 3, 4, 5, 6]
```

变成了。

```
a3 = [i+1 for i in a]print(a3)
#=> [2, 3, 4, 5, 6]
```

列表理解通常被认为是更 pythonic 化的，因为它仍然是可读的。

## 45.举一个三元运算符的例子。

三元运算符是一行 if/else 语句。

语法看起来像`a if condition else b`。

```
x = 5
y = 10'greater' if x > 6 else 'less'
#=> 'less''greater' if y > 6 else 'less'
#=> 'greater'
```

## 46.检查字符串是否只包含数字。

可以用`isnumeric()`。

```
'123a'.isnumeric()
#=> False'123'.isnumeric()
#=> True
```

## 47.检查字符串是否只包含字母。

可以用`isalpha()`。

```
'123a'.isalpha()
#=> False'a'.isalpha()
#=> True
```

## 48.检查字符串是否只包含数字和字母。

可以用`isalnum()`。

```
'123abc...'.isalnum()
#=> False'123abc'.isalnum()
#=> True
```

## 49.从字典中返回一个键列表。

这可以通过将字典传递给 python 的`list()`构造函数`list()`来完成。

```
d = {'id':7, 'name':'Shiba', 'color':'brown', 'speed':'very slow'}list(d)
#=> ['id', 'name', 'color', 'speed']
```

## 50.如何区分字符串的大小写？

您可以使用`upper()`和`lower()`字符串方法。

```
small_word = 'potatocake'
big_word = 'FISHCAKE'small_word.upper()
#=> 'POTATOCAKE'big_word.lower()
#=> 'fishcake'
```

## 51.remove，del，pop 有什么区别？

`remove()`删除第一个匹配值。

```
li = ['a','b','c','d']li.remove('b')
li
#=> ['a', 'c', 'd']
```

`del`按索引删除元素。

```
li = ['a','b','c','d']del li[0]
li
#=> ['b', 'c', 'd']
```

`pop()`通过索引移除元素并返回该元素。

```
li = ['a','b','c','d']li.pop(2)
#=> 'c'li
#=> ['a', 'b', 'd']
```

## 52.举一个字典理解的例子。

下面我们将创建一个字典，以字母表中的字母作为关键字，以字母表中的值作为索引。

```
# creating a list of letters
import string
list(string.ascii_lowercase)
alphabet = list(string.ascii_lowercase)# list comprehension
d = {val:idx for idx,val in enumerate(alphabet)} d
#=> {'a': 0,
#=>  'b': 1,
#=>  'c': 2,
#=> ...
#=>  'x': 23,
#=>  'y': 24,
#=>  'z': 25}
```

## 53.Python 中的异常处理是如何执行的？

Python 提供了 3 个词来处理异常，`try`、`except`和`finally`。

语法看起来像这样。

```
try:
    # try to do this
except:
    # if try block fails then do this
finally:
    # always do this
```

在下面这个简单的例子中，`try`块失败了，因为我们不能将整数和字符串相加。`except`块设定`val = 10`，然后`finally`块打印`complete`。

```
try:
    val = 1 + 'A'
except:
    val = 10
finally:
    print('complete')

print(val)
#=> complete
#=> 10
```

# 结论

你永远不知道面试中会出现什么问题，最好的准备方式是拥有丰富的代码编写经验。

也就是说，这个列表应该涵盖了数据科学家或初级/中级 python 开发人员角色在 python 方面被问到的几乎所有问题。

我希望这对你和我一样有帮助。

有没有我错过的很棒的问题？