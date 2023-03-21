# Python 中 4 个易于使用的自省函数

> 原文：<https://towardsdatascience.com/4-easy-to-use-introspection-functions-in-python-49bd5ee3a2e8?source=collection_archive---------16----------------------->

## Python 初学者

## 如何在运行时检查对象

![](img/98ff9a90652e7d4277dddcdbf35c21c4.png)

照片由[埃迪·利贝丁斯基](https://unsplash.com/@supernov?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

Python 是近年来最流行的编程语言。很多人都知道。但如果你问 Python 有哪些与众不同的优势？不是每个人都能回答这个问题。可能有很多答案。

对我来说，肯定是'**对象内省**'或者'类型内省'。虽然您可以在许多其他语言中找到类似的特性，但 Python 无疑为您提供了最适合初学者的内置工具。

但首先，什么是对象内省？

它能够在运行时检查对象**的类型或属性，并在代码中利用它们的属性。下面是初学者应该学习的 4 个简单易用的自省功能。**

# 1.类型()

**数据类型**是对象的分类，决定了可以对其执行何种操作。例如，加号运算符`+`可以应用于整数，也可以应用于字符串，但是它们的执行方式不同。

```
a = 1 + 1 # 2
b = 'Hello' + 'World' # 'HelloWorld'
```

前者是求和运算，而后者是串联运算。如果你试着把`1 + 'Hello'`加在一起，你会得到一个错误，因为这个操作是未定义的。

因此，数据类型非常重要，因为它影响操作的执行方式。然而，Python 中的数据类型是一个噩梦。这是因为不同的库有许多不同的数据类型，由不同的人群用不同的标准和用法编写。

例如，在 Python 中有许多不同的方法来“包含”或“保存”数字 1，下面是一些例子:

```
a = 1
b = 1.0
c = [1]
d = (1,)
e = np.int64(1)
f = pd.DataFrame([1])
g = 'one' # well, why not?
```

![](img/9a0092aedb551729de393fd44e0e337a.png)

照片由[伊丽莎白·罗威尔·马特森](https://unsplash.com/@vintageimages?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在同一个脚本中使用不同的数据类型是很常见的。为诊断目的检索对象数据类型的最简单方法是使用`type()`。

```
print( [type(x) for x in [a,b,c,d,e,f,g]] )
# [<class 'int'>, 
   <class 'float'>, 
   <class 'list'>, 
   <class 'tuple'>, 
   <class 'numpy.int64'>, 
   <class 'pandas.core.frame.DataFrame'>, 
   <class 'str'>]
```

# 2.isinstance()

要检查一个对象是否是一个类的**实例**，标准的方法是`isinstance(object, class)`。它有两个参数，一个对象和一个类。如果`object`是`class`的实例，函数返回`True`，否则返回`False`。

```
a = 1
if isinstance(a, int):
    print('a is an integer.')
else:
    print('a is not an integer.')# 'a is an integer.'
```

因为它返回一个布尔对象，所以通常用在条件子句中。如果`class`参数是一个**元组**，如果对象是元组中的类型之一，它将返回`True`。

```
my_fav_numbers = [-1, 1, 5]
print(isinstance(my_fav_numbers, (dict, list))) # True
```

假设你定义了一个函数，它应该只接受整数作为参数，否则，就会出现意想不到的后果，比如一个永不结束的递归函数。您可能想要检查参数是否是`int`的实例。

```
assert isinstance(n, int), 'n should be an integer!'
```

本文中的更多信息:

[](https://medium.com/better-programming/how-to-write-recursion-in-3-steps-9d512189a94e) [## 如何用 3 个步骤编写递归

### Python 中的分步示例

medium.com](https://medium.com/better-programming/how-to-write-recursion-in-3-steps-9d512189a94e) ![](img/89f1cc3786176293c2e026a431ee8661.png)

[蒂莫西·戴克斯](https://unsplash.com/@timothycdykes?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 3.目录()

这可以说是 Python 中最强大的内省功能。您可以看到对象的所有有效属性和方法。

```
a = [1,2,3]
print(dir(a))
# ['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
```

这里有一个可以应用在对象`a`上的方法列表(以字符串的形式),比如`sort`对列表进行排序，`pop`获取和移除列表的最后一个元素等等..因此，如果您不确定是否存在方法`sort`，只需检查`'sort' in dir(a)`，如果存在，它将返回`True`。

另一方面，当你在没有任何参数的情况下调用`dir()`时，你会期待什么？一个错误？不要！

它返回当前范围内的名称列表！记得我之前定义过`my_fav_numbers`吗？或者我有吗？让我们检查一下:

```
'my_fav_numbers' in dir() # True
```

# 4.id()

`id()`返回对象的**内存地址**。Python 没有指针的概念，但是您仍然可以使用`id()`来模拟指针操作。这可能太高级了，但是仍然有一些简单的事情可以通过这个函数来完成。

```
a = [1,2,3]
b = aprint(id(a), id(b))
# 140301827626760 140301827626760print(id(a[0]))
# 140302068451232
```

通过检查对象`a`和`b`的 id，我们知道它们有相同的内存地址。

![](img/df3ce0ad0a932c656f5385625c16155a.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [MARK S.](https://unsplash.com/@blocks?utm_source=medium&utm_medium=referral) 拍摄

另一方面，如果您想更多地了解可变对象，检查它们的内存地址会有所帮助。

```
a = [1,2,3]
print(id(a)) # 140298456699464a = [4,5]
print(id(a)) # 140298456729672a[0] = -1
print(id(a)) # 140298456729672
```

你能解释一下上面片段中发生的事情吗？如果您不知道什么是可变对象，请查看下面的文章:

[](/4-common-mistakes-python-beginners-should-avoid-89bcebd2c628) [## Python 初学者应该避免的 4 个常见错误

### 我很艰难地学会了，但你不需要

towardsdatascience.com](/4-common-mistakes-python-beginners-should-avoid-89bcebd2c628) 

# 外卖

我们仅仅触及了 Python 中内省函数的皮毛。你觉得那些功能有趣有用吗？下面留言评论！你也可以[注册我的时事通讯](http://edenau.mailchimpsites.com/)来接收我的新文章的更新。如果您对提高 Python 技能感兴趣，以下文章可能会有所帮助:

[](/4-hidden-python-features-that-beginners-should-know-ec9de65ff9f8) [## 初学者应该知道的 4 个隐藏的 Python 特性

### 如何轻松增强您的 Python 代码

towardsdatascience.com](/4-hidden-python-features-that-beginners-should-know-ec9de65ff9f8) [](/4-common-mistakes-python-beginners-should-avoid-89bcebd2c628) [## Python 初学者应该避免的 4 个常见错误

### 我很艰难地学会了，但你不需要

towardsdatascience.com](/4-common-mistakes-python-beginners-should-avoid-89bcebd2c628)