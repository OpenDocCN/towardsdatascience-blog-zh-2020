# Python 变量赋值

> 原文：<https://towardsdatascience.com/python-variable-assignment-9f43aed91bff?source=collection_archive---------5----------------------->

## 解释 Python 编程语言最基本的概念之一

本文旨在解释 Python 变量赋值是如何工作的。

![](img/98cb46d05666efce70ab35260ff5db99.png)

阿里安·达尔维什在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 基础知识:变量—对象类型和范围

*   变量存储可以在程序中使用和/或更改的信息。该信息可以是整数、文本、集合等。
*   变量用于保存用户输入、程序的本地状态等。
*   变量有一个**名**，这样它们就可以在代码中被引用。
*   要理解的基本概念是，在 Python 中，一切都是对象。

**Python 支持数字、字符串、集合、列表、元组和字典。这些是标准的数据类型。我将详细解释它们中的每一个。**

## 声明变量并赋值

赋值给变量设置一个值。

要给变量赋值，请使用等号(=)

```
myFirstVariable = 1
mySecondVariable = 2
myFirstVariable = "Hello You"
```

*   在 Python 中，赋值被称为**绑定**。在上面的例子中，我们将值 2 赋给了 mySecondVariable。

*注意我是如何将一个整数值 1 和一个字符串值“Hello You”赋给同一个 myFirstVariable 变量的。* ***这是可能的，因为数据类型在 python 中是动态类型化的。***

这就是 Python 被称为动态类型编程语言的原因。

如果您想将相同的值赋给多个变量，那么您可以使用链式赋值:

```
myFirstVariable = mySecondVariable = 1
```

## 数字的

*   支持整数、小数、浮点数。

```
value = 1 #integer
value = 1.2 #float with a floating point
```

*   也支持长整型。它们的后缀是 L，例如 999999999999L

## 用线串

*   文本信息。字符串是一系列字母。
*   字符串是字符的数组
*   字符串值用引号括起来:单引号、双引号或三引号。

```
name = 'farhad'
name = "farhad"
name = """farhad"""
```

*   字符串是不可变的。一旦创建，就不能更改，例如

```
a = 'me'Updating it will fail:
a[1]='y'It will throw a Type Error
```

*   当变量被赋予一个新值时，Python 会在内部创建一个新对象来存储这个值。

因此，创建了对对象的引用/指针。然后将这个指针赋给这个变量，这样，这个变量就可以在程序中使用了。

我们也可以将一个变量赋给另一个变量。它所做的只是创建一个指向同一对象的新指针:

```
a = 1 #new object is created and 1 is stored there, new pointer is created, the pointer connects a to 1
b = a #new object is not created, new pointer is created only that connects b to 1
```

## 变量可以有局部或全局范围。

## 局部范围

*   例如，在函数中声明的变量只能存在于块中。
*   一旦块存在，变量也变得不可访问。

```
def some_funcion():
  TestMode = Falseprint(TestMode) <- Breaks as the variable doesn't exist outside
```

在 Python 中，if-else 和 for/while 循环块不创建任何局部范围。

```
for i in range(1, 11):
    test_scope = "variable inside for loop"print(test_scope)
```

输出:

```
variable inside for loop
```

使用 if-else 块

```
is_python_awesome = Trueif is_python_awesome:
    test_scope = "Python is awesome"print(test_scope)
```

输出:

```
Python is awesome
```

## 全球范围

*   可以从任何函数中访问的变量都具有全局范围。它们存在于 __main__ 框架中。
*   您可以在函数外部声明一个全局变量。需要注意的是，要给一个全局变量赋一个新值，你必须使用" *global* "关键字:

```
TestMode = True
def some_function():
  global TestMode
  TestMode = Falsesome_function()
print(TestMode) <--Returns False
```

*删除“全局测试模式”这一行只会将 some_function()函数中的变量设置为 False。*

**注意:虽然我稍后会写更多关于模块的概念，但是如果你想在多个模块间共享一个全局变量，你可以创建一个共享的模块文件，比如 configuration.py，并在那里定位你的变量。最后，在您的消费者模块中导入共享模块。**

## 查找变量类型

*   如果要查找变量的类型，可以实现:

```
type('farhad')
--> Returns <type 'str'>
```

## 整数变量中的逗号

*   逗号被视为一系列变量，例如

```
9,8,7 are three numeric variables
```

# 3.操作

*   允许我们对变量进行计算

## 数字运算

*   Python 支持基本的 ***、/、+、-**
*   Python 也支持楼层划分

```
1//3  #returns 0
1/3 #returns 0.333
```

*   此外，python 支持通过**运算符求幂:

```
2**3 = 2 * 2 * 2 = 8
```

*   Python 也支持模数(余数)运算符:

```
7%2 = 1
```

还有一个 **divmod** 内置方法。它返回除法器和模数:

```
print(divmod(10,3)) #it will print 3 and 1 as 3*3 = 9 +1 = 10
```

## 字符串操作

**串联字符串:**

```
'A' + 'B' = 'AB'
```

记住字符串是不可变的数据类型，因此，连接字符串会创建一个新的字符串对象。

**重复字符串:**

```
‘A’*3 will repeat A three times:  AAA
```

**切片:**

```
y = 'Abc'
y[:2] = ab
y[1:] = bc
y[:-2] = a
y[-2:] = bc
```

**反转:**

```
x = 'abc'
x = x[::-1]
```

**负指数:**

如果你想从最后一个字符开始，那么使用负索引。

```
y = 'abc'
print(y[:-1]) # will return ab
```

它还用于移除任何新的线路托架/空间。

数组中的每个元素都有两个索引:

*   从左到右，索引从 0 开始，增量为 1
*   从右到左，索引从-1 开始，递减 1
*   因此，如果我们做 y[0]和 y[-len(y)]那么两者都将返回相同的值:“a”

```
y = 'abc'
print(y[0])
print(y[-len(y)])
```

**寻找指数**

```
name = 'farhad'
index = name.find('r')#returns 2name = 'farhad'
index = name.find('a', 2) # finds index of second a#returns 4
```

**对于正则表达式，使用:**

*   split():通过正则表达式将一个字符串拆分成一个列表
*   sub():通过正则表达式替换匹配的字符串
*   subn():通过正则表达式替换匹配的字符串，并返回替换次数

## 铸造

*   str(x):到字符串
*   int(x):到整数
*   浮动(x):到浮动
*   元组(列表):To 元组:print(元组([1，2，3]))
*   list(tuple(1，2，3)): To list: print(list((1，2，3)))

记住列表是可变的(可以更新),元组是不可变的(只读)

## 集合操作

*   该集合是没有任何重复的无序数据集合。我们可以将集合变量定义为:

```
a = {1,2,3}
```

**相交集**

*   得到两组的共同点

```
a = {1,2,3}
b = {3,4,5}
c = a.intersection(b)
```

**器械包差异**

*   要检索两个集合之间的差异:

```
a = {1,2,3}
b = {3,4,5}
c = a.difference(b)
```

**集合的联合**

*   以获得两个集合的不同组合集合

```
a = {1,2,3}
b = {3,4,5}
c = a.union(b)
```

## 三元运算符

*   用于在单行中编写条件语句。

**语法:**

***【If True】If【表情】Else【If False】***

例如:

```
Received = True if x == 'Yes' else False
```

# 对象身份

我现在将尝试解释客体同一性的重要主题。

每当我们在 Python 中创建一个对象，比如变量、函数等，底层的 Python 解释器都会创建一个唯一标识该对象的编号。一些对象是预先创建的。

当一个对象在代码中不再被引用时，它就会被删除，它的标识号可以被其他变量使用。

*   Python 代码被加载到位于堆栈中的帧中。
*   函数连同它们的参数和变量一起被加载到一个框架中。
*   随后，帧以正确的执行顺序加载到堆栈中。
*   堆栈概述了函数的执行。在函数外部声明的变量存储在 __main__ 中
*   Stacks 首先执行最后一帧。

如果遇到错误，您可以使用**回溯**来查找函数列表。

**dir()和 help()**

*   dir()-显示定义的符号
*   help()-显示文档

## 我们来详细了解一下:

考虑下面的代码:

```
var_one = 123
def func_one(var_one):
    var_one = 234
    var_three = 'abc'var_two = 456
print(dir())
```

var_one 和 var_two 是上面代码中定义的两个变量。与变量一起，还定义了一个名为 func_one 的函数。需要记住的重要一点是，在 Python 中，一切都是对象，包括函数。

在该函数中，我们将值 234 赋给了 var_one，并创建了一个名为 var_three 的新变量，并将值“abc”赋给了它。

**现在，让我们借助 dir()和 id()** 来理解代码

上述代码及其变量和函数将被加载到全局框架中。全局框架将包含其他框架需要的所有对象。例如，Python 中加载了许多内置方法，可用于所有框架。这些是功能框架。

运行上面的代码将会打印:

```
*[‘__annotations__’, ‘__builtins__’, ‘__cached__’, ‘__doc__’, ‘__file__’, ‘__loader__’, ‘__name__’, ‘__package__’, ‘__spec__’,* ***‘func_one’, ‘var_one’, ‘var_two’****]*
```

以 __ 为前缀的变量称为特殊变量。

请注意，var_three 还不可用。让我们执行 func_one(var_one)然后评估 dir():

```
var_one = 123
def func_one(var_one):
    var_one = 234
    var_three = 'abc'var_two = 456
func_one(var_one)
print(dir())
```

我们将再次看到相同的列表:

```
['__annotations__', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', 'func_one', 'var_one', 'var_two']
```

这意味着 func_one 内的变量只在 func_one 内。执行 func_one 时，会创建一个帧。Python 是自顶向下的，因此它总是从上到下执行代码行。

功能框架可以引用全局框架中的变量，但任何其他功能框架都不能引用在自身内部创建的相同变量。举个例子，如果我创建一个新的函数 func_two，试图打印 var_three，那么它将会失败:

```
var_one = 123
def func_one(var_one):
    var_one = 234
    var_three = 'abc'var_two = 456 def func_two():
    print(var_three)func_one(var_one)
func_two()
print(dir())
```

我们得到一个错误，即**名称错误:名称“var_three”未定义**

**如果我们在 func_two()内部创建一个新变量，然后打印 dir()会怎么样？**

```
var_one = 123
def func_one(var_one):
    var_one = 234
    var_three = 'abc'var_two = 456 def func_two():
    var_four = 123
    print(dir())func_two()
```

这将打印 var_four，因为它是 func_two 的本地变量。

## Python 中赋值是如何工作的？

这是 Python 中最重要的概念之一。Python 有一个 id()函数。

当一个对象(函数、变量等。)时，CPython 会在内存中为它分配一个地址。id()函数返回一个对象的“身份”。它本质上是一个唯一的整数。

例如，让我们创建四个变量并为它们赋值:

```
variable1 = 1
variable2 = "abc"
variable3 = (1,2)
variable4 = ['a',1]#Print their Ids
print('Variable1: ', id(variable1))
print('Variable2: ', id(variable2))
print('Variable3: ', id(variable3))
print('Variable4: ', id(variable4))
```

id 将按如下方式打印:

变量 1: 1747938368
变量 2: 152386423976
变量 3: 152382712136
变量 4: 152382633160

每个变量都被赋予一个新的整数值。

第一个假设是，每当我们使用赋值“=”时，Python 就会创建一个新的内存地址来存储变量。是不是百分百真实，不是真的！

我将创建两个变量，并将它们赋给现有的变量。

```
variable5 = variable1
variable6 = variable4print('Variable1: ', id(variable1))
print('Variable4: ', id(variable4))
print('Variable5: ', id(variable5))
print('Variable6: ', id(variable6))
```

蟒蛇皮印花:

变量 1:**1747938368**变量 4: 819035469000
变量 5:**1747938368**变量 6: 819035469000

*注意 Python 没有为这两个变量创建新的内存地址。这一次，它将两个变量指向同一个内存位置。*

让我们为变量 1 设置一个新值。记住 2 是整数，整数是不可变的数据类型。

```
print('Variable1: ', id(variable1))
variable1 = 2
print('Variable1: ', id(variable1))
```

这将打印:

*变量 1: 1747938368
变量 1: 1747938400*

**这意味着每当我们使用=并给一个不是变量引用的变量赋值时，就会在内部创建一个新的内存地址来存储该变量。让我们看看它能不能坚持住！**

当变量是可变数据类型时会发生什么？

变量 6 是一个列表。让我们给它添加一个条目并打印它的内存地址:

```
print('Variable6: ', id(variable6))
variable6.append('new')
print('Variable6: ', id(variable6))
```

注意，变量的内存地址保持不变，因为它是可变数据类型，我们只是更新了它的元素。

变量 6: 678181106888
变量 6: 678181106888

让我们创建一个函数，并向它传递一个变量。如果我们在函数内部设置变量的值，它在内部会做什么？让我们评估一下

```
def update_variable(variable_to_update):
 print(id(variable_to_update)) update_variable(variable6) print(’Variable6: ', id(variable6))
```

我们得到:

678181106888
变量 6: 678181106888

注意 variable_to_update 的 id 指向变量 6 的 id。

这意味着如果我们在一个函数中更新变量 _to_update，并且如果变量 _to_update 是可变数据类型，那么我们将更新变量 6。

```
variable6 = [’new’]
print(’Variable6: ', variable6def update_variable(variable_to_update):    variable_to_update.append(’inside’)update_variable(variable6)
print('Variable6: ', variable6)
```

这是打印的:

变量 6: ['新']
变量 6: ['新'，'内部']

它向我们展示了同一个对象在函数中按照预期被更新，因为它们有相同的 ID。

如果我们给一个变量赋一个新值，不管它的数据类型是不可变的还是可变的，一旦我们从函数中出来，这个变化就会丢失:

```
print('Variable6: ', variable6)def update_variable(variable_to_update):
    print(id(variable_to_update))
    variable_to_update = ['inside']update_variable(variable6)
print('Variable6: ', variable6)
```

变量 6: ['新']
344115201992
变量 6: ['新']

现在有一个有趣的场景:Python 并不总是为所有新变量创建一个新的内存地址。让我解释一下。

最后，如果我们给两个不同的变量赋值一个字符串值，比如' a '会怎么样。会不会产生两个内存地址？

```
variable_nine = "a"
variable_ten = "a"
print('Variable9: ', id(variable_nine))
print('Variable10: ', id(variable_ten))
```

注意，这两个变量有相同的内存位置:

变量 9: 792473698064

如果我们创建两个不同的变量并给它们分配一个长字符串值会怎么样:

```
variable_nine = "a"*21
variable_ten = "a"*21
print('Variable9: ', id(variable_nine))
print('Variable10: ', id(variable_ten))
```

这次 Python 为两个变量创建了两个内存位置:

变量 9: 541949933872
变量 10: 541949933944

这是因为 Python 在启动时会创建一个值的内部缓存。这样做是为了提供更快的结果。它为-5 到 256 之间的小整数和更小的字符串值创建了一些内存地址。这就是为什么我们例子中的两个变量有相同的 ID。

本文解释了变量是如何赋值的。

如果您想详细了解 Python，请阅读本文:

[](https://medium.com/fintechexplained/everything-about-python-from-beginner-to-advance-level-227d52ef32d2) [## 关于 Python 的一切——从初级到高级

### 在一篇文章中你需要知道的一切

medium.com](https://medium.com/fintechexplained/everything-about-python-from-beginner-to-advance-level-227d52ef32d2)