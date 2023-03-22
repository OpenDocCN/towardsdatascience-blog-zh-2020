# Python 中的循环

> 原文：<https://towardsdatascience.com/looping-in-python-5289a99a116e?source=collection_archive---------7----------------------->

## 如何在 Python 中使用 enumerate()函数

![](img/09b699b35bf7a67586becca9c45d64bc.png)

弗洛里安·奥利佛在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 介绍

假设我们有一个列表。我们想要遍历这个列表，打印出索引，后面跟着列表元素或索引处的值。让我们使用 for 循环来实现这一点:

```
num_list= [42, 56, 39, 59, 99]for i in range(len(num_list)): 
    print(i, num_list[i])# output: 
0 42
1 56
2 39
3 59
4 99
```

*range()* 是 python 中的内置函数，它允许我们遍历一系列数字。如上所述，我们使用 for 循环来循环遍历一个 range 对象(这是一种 iterable 类型)，直到我们的列表的长度。换句话说，我们从 I 值 0 开始，一直到(但不包括)num_list 的长度，也就是 5。然后我们使用方括号在第 I 个*索引处访问 *num_list* 的元素。*

然而，重要的是要理解我们实际上并没有迭代 *num_list* 。换句话说， *i* 充当索引的代理，我们可以用它来访问来自 *num_list* 的元素。

[](/a-skill-to-master-in-python-d6054394e073) [## 如何在 Python 中分割序列

### 了解如何在 Python 中分割列表和字符串

towardsdatascience.com](/a-skill-to-master-in-python-d6054394e073) 

## 使用 enumerate()函数

不使用 range()函数，我们可以改用 python 中内置的 [*enumerate()*](https://docs.python.org/3/library/functions.html#enumerate) 函数。 *enumerate()* 允许我们遍历一个序列，但是它同时跟踪索引和元素。

> 枚举(iterable，start=0)

enumerate()函数接受一个 iterable 作为参数，比如列表、字符串、元组或字典。此外，它还可以接受一个可选参数， *start* ，它指定了我们希望计数开始的数字(默认为 0)。

使用 *enumerate()* 函数，我们可以将 for 循环重写如下:

```
num_list= [42, 56, 39, 59, 99]for index, element in enumerate(num_list):
    print(index, element)# output: 
0 42
1 56
2 39
3 59
4 99
```

就是这样！我们不需要使用 *range()* 函数。代码看起来更干净，更 pythonic 化。

[](/dictionary-comprehensions-in-python-912a453da512) [## Python 中的词典理解

### 如何使用字典理解在 python 中创建字典

towardsdatascience.com](/dictionary-comprehensions-in-python-912a453da512) 

## enumerate()如何工作

*enumerate()* 函数返回一个枚举对象，它是一个迭代器。当从这个枚举对象访问每个元素时，返回一个元组，其中包含索引和该索引处的元素:(index，element)。因此，在上面的 for 循环中，在每次迭代中，它都将这个返回元组的元素分配给 index 和 element 变量。换句话说，返回的元组在 for 语句中被解包:

```
for **index, element** in enumerate(num_list):# similar to:
index, element = (index, element)
```

通过下面的例子可以更容易地看出这些元组:

```
name_list = ['Jane', 'John', 'Mike']list(enumerate(name_list))# [(0, 'Jane'), (1, 'John'), (2, 'Mike')]
```

一旦我们在枚举对象(迭代器)上调用 list()函数，它将返回一个元组列表，每个元组都包括索引及其对应的元素或值。

如果你喜欢阅读这样的故事，并想支持我成为一名作家，考虑注册成为一名媒体会员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你用我的 [*链接*](https://lmatalka90.medium.com/membership) *注册，我会赚一小笔佣金。*

[](https://lmatalka90.medium.com/membership) [## 通过我的推荐链接加入媒体——卢艾·马塔尔卡

### 阅读卢艾·马塔尔卡的每一个故事(以及媒体上成千上万的其他作家)。您的会员费直接支持…

lmatalka90.medium.com](https://lmatalka90.medium.com/membership) 

## 结论

在本教程中，我们学习了如何使用 enumerate()函数对 iterable 进行计数。