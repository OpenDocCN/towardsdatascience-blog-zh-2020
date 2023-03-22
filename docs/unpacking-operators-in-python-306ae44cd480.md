# Python 中的解包运算符

> 原文：<https://towardsdatascience.com/unpacking-operators-in-python-306ae44cd480?source=collection_archive---------5----------------------->

## 在 Python 中使用*和**解包运算符

![](img/4a43996cddfb1754d8598dd6624752d4.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 介绍

在本教程中，我们将学习如何使用星号(*)操作符解包可迭代对象，以及如何使用两个星号(**)解包字典。此外，我们将讨论如何使用同一个运算符将几个值打包到一个变量中。最后，我们将讨论什么是*args 和**kwargs 以及何时可以使用它们。

## *操作员

假设我们有一个列表:

```
num_list = [1,2,3,4,5]
```

我们定义了一个函数，它接受 5 个参数并返回它们的和:

```
def num_sum(num1,num2,num3,num4,num5):
    return num1 + num2 + num3 + num4 + num5
```

我们想找出 **num_list** 中所有元素的总和。嗯，我们可以通过将 **num_list** 的所有元素传递给函数 **num_sum** 来实现这一点。由于 **num_list** 中有五个元素，因此 **num_sum** 函数包含五个参数，每个参数对应于 **num_list** 中的一个元素。

一种方法是通过使用元素的索引来传递元素，如下所示:

```
num_sum(num_list[0], num_list[1], num_list[2], num_list[3], num_list[4])# 15
```

然而，有一种更简单的方法，那就是使用*操作符。*操作符是一个解包操作符，它将解包来自任何可迭代对象的值，例如列表、元组、字符串等…

例如，如果我们想解包 **num_list** 并传入 5 个元素作为 **num_sum** 函数的独立参数，我们可以这样做:

```
num_sum(*num_list)# 15
```

就是这样！星号*或解包操作符解包 **num_list** ，并将 **num_list** 的值或元素作为单独的参数传递给 **num_sum** 函数。

> 注意:为了实现这一点， **num_list** 中的元素数量必须与 **num_sum** 函数中的参数数量相匹配。如果它们不匹配，我们会得到一个类型错误。

## ***带内置函数的运算符:**

我们还可以在 python 的内置函数中使用星号，*或解包操作符，比如 print:

```
print(*num_list)# 1 2 3 4 5
```

## **解包多个列表:**

假设我们有另一个列表:

```
num_list_2 = [6,7,8,9,10]
```

我们希望打印出 **num_list** 和 **num_list_2** 中的所有元素。我们可以使用解包操作符*来实现这一点，如下所示:

```
print(*num_list, *num_list_2)# 1 2 3 4 5 6 7 8 9 10
```

*两个* ***num_list*** *和****num _ list _ 2****都被解包。然后，所有的元素都作为单独的参数传递给 print。*

## **合并多个列表:**

我们还可以创建一个新的列表，包含来自 **num_list** 和 **num_list_2** 的所有元素:

```
new_list = [*num_list, *num_list_2]# [1,2,3,4,5,6,7,8,9,10]
```

***num _ list****和****num _ list _ 2****被解包，导致它们的元素构成新制作的 list 的元素，****new _ list****。*

> 注意:我们可以简单地添加 **num_list** 和 **num_list_2** 来创建 **new_list** 。然而，这只是为了描述拆包操作员的功能。

[](/the-walrus-operator-in-python-a315e4f84583) [## Python 中的海象算子

### 了解什么是 walrus 操作符以及如何在 Python 中使用它

towardsdatascience.com](/the-walrus-operator-in-python-a315e4f84583) 

## *运算符的其他用途:

假设我们有一个字符串分配给变量 **name** :

```
name = ‘Michael’
```

我们想把这个名字分成三部分，第一个字母分配给一个变量，最后一个字母分配给另一个变量，中间的所有字母分配给第三个变量。我们可以这样做:

```
first, *middle, last = name
```

就是这样！由于 name 是一个字符串，而字符串是可迭代的对象，所以我们可以对它们进行解包。赋值操作符右边的值将根据它们在 iterable 对象中的相对位置被赋给左边的变量。因此,“Michael”的第一个字母被分配给变量 **first** ，在本例中为“M”。最后一个字母‘l’被分配给变量 **last** 。而变量**中间**会以列表的形式包含‘M’和‘l’之间的所有字母:[‘I’，‘c’，‘h’，‘a’，‘e’]。

> 注意:上面的**第一个**和**最后一个**变量被称为强制变量，因为它们必须被赋予具体的值。由于使用了*或解包操作符，中间的**变量可以有任意数量的值，包括零。如果没有足够的值来解包强制变量，我们将得到一个 ValueError。**

例如，如果我们使用下面的赋值语句:

```
first, *middle, last = ‘ma'
```

然后，变量**第一个**将被赋值为“m”，变量**最后一个**将被赋值为“a”，而变量**中间的**将只是一个空列表，因为没有其他值要赋值给它。

[](/ternary-operators-in-python-49c685183c50) [## Python 中的三元运算符

### 用三元运算符改进您的 Python 代码！

towardsdatascience.com](/ternary-operators-in-python-49c685183c50) 

## **用*运算符打包:**

我们还可以使用*运算符将多个值打包到一个变量中。例如:

```
*names, = ‘Michael’, ‘John’, ‘Nancy’# names 
['Michael', 'John', 'Nancy']
```

在* **名称**后使用尾随逗号的原因是因为赋值的左边必须是元组或列表。因此， **names** 变量现在以列表的形式包含了右侧的所有名字。

> 注意:这就是我们在定义可以接收不同数量参数的函数时所做的事情！那就是*args 和**kwargs 的概念！

## ***参数:**

例如，假设我们有一个函数， **names_tuple** ，它接受名字作为参数并返回它们。然而，我们传递给这个函数的名字的数量是可变的。我们不能只选择这个函数的一些参数，因为位置参数的数量会随着函数的每次调用而变化。我们可以使用*运算符将传入的参数打包成一个元组，如下所示:

```
def names_tuple(*args):
    return argsnames_tuple('Michael', 'John', 'Nancy')
# ('Michael', 'John', 'Nancy')names_tuple('Jennifer', 'Nancy')
# ('Jennifer', 'Nancy')
```

无论我们在调用 **names_tuple** 函数时传入多少个位置参数，* **args** 参数都会将位置参数打包到一个元组中，类似于上面的* **names** 赋值。

## *** *夸格斯**

为了传递不同数量的关键字或命名参数，我们在定义函数时使用了**操作符。**解包操作符将把我们传入的不同数量的命名参数打包到一个字典中。

```
def names_dict(**kwargs):
    return kwargsnames_dict(Jane = 'Doe')
# {'Jane': 'Doe'}names_dict(Jane = 'Doe', John = 'Smith')
# {'Jane': 'Doe', 'John': 'Smith'}
```

> 注意:当使用*运算符创建一个在定义函数时接收不同数量的位置参数的参数时，通常使用参数名 args(和 kwargs 来接收不同数量的关键字或命名参数)。但是，可以为这些参数选择任何名称。

[](/iterables-and-iterators-in-python-849b1556ce27) [## Python 中的迭代器和迭代器

### Python 中的可迭代对象、迭代器和迭代

towardsdatascience.com](/iterables-and-iterators-in-python-849b1556ce27) 

## 字典

当我们尝试在字典中使用*操作符时会发生什么？

```
num_dict = {‘a’: 1, ‘b’: 2, ‘c’: 3}print(*num_dict)# a b c
```

注意它是如何打印字典的键而不是值的？要解包一个字典，我们需要使用**解包操作符。但是，由于每个值都与一个特定的键相关联，所以我们将这些参数传递给的函数必须具有与被解包的字典的键同名的参数。例如:

```
def dict_sum(a,b,c):
    return a+b+c
```

这个 **dict_sum** 函数有三个参数: **a** 、 **b** 和 **c** 。这三个参数的命名与 **num_dict** 的键相同。因此，一旦我们使用**操作符传入解包的字典，它将根据相应的参数名分配键值:

```
dict_sum(**num_dict)# 6
```

*因此，对于* ***a*** *、* ***b*** *和******中的参数 dict_sum*** *将分别为 1、2 和 3。这三个值之和是 6。***

## ****合并字典:****

**就像列表一样，**操作符可以用来合并两个或更多的字典:**

```
**num_dict = {‘a’: 1, ‘b’: 2, ‘c’: 3}num_dict_2 = {‘d’: 4, ‘e’: 5, ‘f’: 6}new_dict = {**num_dict, **num_dict_2}# {‘a’: 1, ‘b’: 2, ‘c’: 3, ‘d’: 4, ‘e’: 5, ‘f’: 6}**
```

**如果你喜欢阅读这样的故事，并想支持我成为一名作家，考虑注册成为一名媒体成员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你用我的 [*链接*](https://lmatalka90.medium.com/membership) *注册，我会赚一小笔佣金。***

**[](https://lmatalka90.medium.com/membership) [## 通过我的推荐链接加入媒体——卢艾·马塔尔卡

### 阅读卢艾·马塔尔卡的每一个故事(以及媒体上成千上万的其他作家)。您的会员费直接支持…

lmatalka90.medium.com](https://lmatalka90.medium.com/membership)** 

## **结论**

**在本教程中，我们学习了如何使用*运算符解包可迭代对象，以及如何使用**运算符解包字典。我们还学习了利用这些操作符完成许多不同任务的许多方法。此外，在定义接收不同数量的位置或命名参数的函数时，我们通过使用*和**操作符简要讨论了用*args 和**kwargs 打包的概念。**