# 在 Python 3.9 中合并字典

> 原文：<https://towardsdatascience.com/merging-dictionaries-in-python-3-9-3b0409aa91a8?source=collection_archive---------2----------------------->

## Python 初学者

## 语法简洁的 Python 字典

![](img/f030172680f6caf740dff9ff06ce4fa0.png)

丹尼尔·科内斯基在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Python 3.9 中有一个*改变游戏规则的*新特性，允许你在处理 Python **字典**的同时，编写更多**可读**和**紧凑**代码。

## Python 版本

你现在用的是哪个版本的 Python？3.7?3.5?还是还是 2.7？上次我们介绍了 Python 3.8 中的 6 个新特性，即使是初学者也应该注意。如果你还没有读过这篇文章，可以去看看。

[](/6-new-features-in-python-3-8-for-python-newbies-dc2e7b804acc) [## Python 3.8 中针对 Python 新手的 6 项新特性

### 请做好准备，因为 Python 2 不再受支持

towardsdatascience.com](/6-new-features-in-python-3-8-for-python-newbies-dc2e7b804acc) 

Python 3.9 现在正处于开发的 alpha 阶段，许多新特性已经被提出并被接受！它将在 2020 年 5 月进入测试阶段，届时将不会提出任何新功能。

第一个稳定版本将于 2020 年 10 月发布。你可以在这里找到发布时间表。

有一个新功能涉及字典。

## Python 词典

Dictionary 是 Python 中一个非常独特的数据结构。一个字典**包含多个元素**，每个元素是一个**键-值对**。例如，让我们用两个元素初始化一个字典`d1`。关键字`'name'`的值为`'Tom'`，而关键字`'age'`的值为 20。

```
d1 = {'name': 'Tom', 'age': 20}
```

我们现在有一本存储 20 岁的汤姆的信息的字典。

![](img/199dcfac1b070075e46e58ff6b704ed0.png)

[Roman Bürki](https://unsplash.com/@romanleandro?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

假设出于某种原因，你收集了更多关于汤姆的信息，比如他的平均绩点和婚姻状况。你现在有了另一本叫做`d2`的字典。

```
d2 = {'gpa': 4.0, 'is_single': True}
```

您希望将这两本词典合并在一起，因为它们都包含关于同一个人 Tom 的不同信息。

问题是— **如何用 Python 合并两个字典？**

## 1.笨拙的方式

您可以使用带有语法`dict_name[key]= value`的赋值操作符`=`在现有字典中插入一个新元素。

```
d1 = {'name': 'Tom', 'age': 20}
d1['sex'] = 'Male'# d1 == {'name': 'Tom', 'age': 20, 'sex': 'Male'}
```

因此，在不使用任何特定于字典的方法的情况下，想到的第一个方法是编写一个 **for-loop** ，使用 **iterable** `.items()`遍历每个键值对，然后将键值对插入新字典`dnew`。

```
d1 = {'name': 'Tom', 'age': 20}
d2 = {'gpa': 4.0, 'is_single': True}
dnew = dict()for key, value in d1.items():
    dnew[key] = value
for key, value in d2.items():
    dnew[key] = value# dnew == {'name': 'Tom', 'age': 20, 'gpa': 4.0, 'is_single': True}
```

尽管如此，合并字典应该是非常简单和直接的事情，并且应该在一行代码中完成。

![](img/6dce1a4468d357ff906b17d9b60324a4.png)

照片由[沙哈达特·拉赫曼](https://unsplash.com/@hishahadat?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我们可以做得更好。好多了。

## 2.默认方式

实际上有一个**内置的**方法，可以用另一个词典`d2`来‘更新’一个词典`d1`。

```
dnew = d1.copy()
dnew.update(d2)
```

一个缺点是`.update()`就地修改了字典**。您需要首先通过复制`d1`来创建一个新的字典`dnew`。这种“内置”方法违背了使用方便的内置方法合并词典的目的。**

**我们能在一行代码中完成合并吗？是啊！**

## **3.“整洁”的方式**

**Python 从 3.5+版本开始支持字典解包`**`。您可以通过解包两个字典的元素来创建一个新的“合并”字典。**

```
dnew = {**d1, **d2}
```

**这种解包方法成为从 Python 3.5+合并字典的事实方法。然而，这种语法对你们中的一些人来说可能很难看，对我们大多数人来说肯定不直观。第一次看到它时，你能猜出它的意思吗？**

**还有另一种简洁的方法可以在一行中实现字典合并。它看起来也不直观。**

```
dnew = dict(d1, **d2)
```

**![](img/2e8f780a691e9fdde28382947d444563.png)**

**照片由[普贡达兰干清真寺](https://unsplash.com/@masjidmpd?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄**

# **Python 3.9 中的简洁方式**

**Python 3.9 引入了新的**和干净的**(!)使用**联合运算符** `|`合并字典的方法。相当整洁。**

```
dnew = d1 | d2# dnew == {'name': 'Tom', 'age': 20, 'gpa': 4.0, 'is_single': True}
```

**这个 union 操作符实际上在 Python 中并不新鲜。它可用于“合并”两个集合。一个**集合**是一个**无序且未索引的集合**，也是用花括号写的。**

```
a = {1, 2, 3}
b = {3, 4, 5}
print( a | b )
# {1, 2, 3, 4, 5}
```

## **扩充赋值**

**对于两个列表或两个值`a`和`b` , `a += b`是`a = a+b`的简称。**

**这种**增强的赋值行为**也适用于字典联合操作符。这意味着`d1 |= d2`相当于`d1 = d1 | d2`。**

## **警告**

**集合是无序的。字典是**插入有序的**(来自 Python 3.6)。换句话说，词典 ***记住了*** 插入条目的顺序。**

**这意味着字典联合是不可交换的 T21。这意味着`d1 | d2`和`d2 | d1`将会产生具有不同项目**顺序**的合并字典。**

**感谢您的阅读！Python 3.9 中还有其他新特性，包括对**变量注释**的更新。不知道那是什么？查看以下关于**静态类型**和注释的文章。**

**[](/static-typing-in-python-55aa6dfe61b4) [## Python 中的静态类型

### 轻松进行类型检查

towardsdatascience.com](/static-typing-in-python-55aa6dfe61b4) 

如果您想收到我的新文章的更新，您可以使用下面的滑块订阅我的时事通讯:

如果您对 Python 感兴趣，以下文章可能会有用:

[](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4) [## 我希望我能早点知道的 5 个 Python 特性

### 超越 lambda、map 和 filter 的 Python 技巧

towardsdatascience.com](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4) [](/4-common-mistakes-python-beginners-should-avoid-89bcebd2c628) [## Python 初学者应该避免的 4 个常见错误

### 我很艰难地学会了，但你不需要

towardsdatascience.com](/4-common-mistakes-python-beginners-should-avoid-89bcebd2c628) 

*快乐编码！***