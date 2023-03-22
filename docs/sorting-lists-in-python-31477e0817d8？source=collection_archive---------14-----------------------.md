# Python 中的排序列表

> 原文：<https://towardsdatascience.com/sorting-lists-in-python-31477e0817d8?source=collection_archive---------14----------------------->

## 如何在 Python 中使用 sort()和 sorted()对列表进行排序

![](img/86dfeaf4193fca478fd4d6069a5d525b.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在本教程中，我们将看看如何使用 sort()和 sorted()基于 python 中的不同标准对列表进行排序。

# 对列表进行排序

有两种方法对列表进行排序。我们可以使用 sort()方法或 sorted()函数。sort()方法是一个列表方法，因此只能用于列表。sorted()函数适用于任何 iterable。

## sort()方法

sort()方法是一个列表方法，它就地修改列表并返回 None。换句话说，sort()方法修改或更改它所调用的列表，而不创建新列表。

sort()方法有两个可选参数:key 参数和 reverse 参数。key 参数接受一个函数，该函数接受一个参数并返回一个用于排序的键。默认情况下，sort()方法将按数值对一系列数字进行排序，并按字母顺序对一系列字符串进行排序。reverse 参数接受布尔值 True 或 False。reverse 的默认值为 False，这意味着它按升序排序。要按降序排序，我们应该设置 reverse=True。当我们看下面的一些例子时，这些参数会更有意义。

## 对数字列表进行排序

假设我们有一个数字列表，我们想按升序排序。

```
num_list = [1,-5,3,-9,25,10]num_list.sort()print(num_list)
# [-9,-5,1,3,10,25]
```

所以我们有一个数字列表，或者说 *num_list。*我们在这个列表上调用 sort()方法。请注意，我们没有为 key 参数传入值。因此，它只是按照实际值对这个数字列表进行排序。由于我们没有设置 reverse = True，所以它按升序排序。sort()方法修改了我们的 *num_list* 。

如果我们想根据数字的绝对值对列表进行排序呢？这就是我们需要使用关键参数的时候。key 参数接受一个函数，该函数接受一个参数并返回一个用于排序的键。

```
num_list = [1,-5,3,-9,25,10]def absolute_value(num):
    return abs(num)num_list.sort(key = absolute_value)print(num_list) 
# [1,3,-5,-9,10,25]
```

我们定义了一个函数， *absolute_value* ，它接受一个数字并返回其绝对值。然后，我们将这个函数作为 sort()方法的关键参数传入。因此，在进行比较之前，它通过绝对值函数运行 *num_list* 的每个元素或数字。因此，数字的绝对值用于按升序对列表进行排序(因为 reverse 默认设置为 False)。

## 使用 lambda 表达式

我们可以为 key 参数传入一个 lambda 表达式，如下所示:

```
num_list.sort(key = lambda num: abs(num))
```

*有关 lambda 函数的更多信息:*

[](/lambda-expressions-in-python-9ad476c75438) [## Python 中的 Lambda 表达式

### 如何用 python 写匿名函数

towardsdatascience.com](/lambda-expressions-in-python-9ad476c75438) 

记住 sort()方法返回 None。因此，如果我们将 sort()方法的输出或返回值设置为一个新变量，我们将得不到任何值，如下所示:

```
new_list = num_list.sort(key = absolute_value)print(new_list)
# None
```

## 使用内置函数

我们可以不像上面那样编写自己的 absolute_value 函数，而是直接为关键参数传入 python 内置的 abs()函数，如下所示:

```
num_list.sort(key = abs)
```

[](/unpacking-operators-in-python-306ae44cd480) [## Python 中的解包运算符

### 在 python 中使用*和**解包运算符

towardsdatascience.com](/unpacking-operators-in-python-306ae44cd480) 

## sorted()函数

sorted()函数可以接受三个参数:iterable、key 和 reverse。换句话说，sort()方法只对列表有效，但是 sorted()函数可以对任何可迭代对象有效，比如列表、元组、字典等等。然而，与返回 None 并修改原始列表的 sort()方法不同，sorted()函数返回一个新列表，同时保持原始对象不变。

让我们再次使用绝对值对 *num_list* 进行排序，但是使用 sorted()函数:

```
num_list = [1,-5,3,-9,25,10]new_list = sorted(num_list, key = abs)print(new_list) 
# [1,3,-5,-9,10,25]print(num_list)
# [1,-5,3,-9,25,10]
```

我们将 iterable、 *num_list* 传递给 sorted()函数，并将内置的 *abs* 函数传递给关键参数。我们将 sorted()函数的输出设置为一个新变量， *new_list* 。请注意 *num_list* 是如何保持不变的，因为 sorted()函数没有修改它所作用的 iterable。

> 注意:无论向 sorted()函数传递什么 iterable，它总是返回一个列表。

## 对元组列表进行排序

假设我们有一个元组列表。列表中的每个元素都是一个包含三个元素的元组:姓名、年龄和薪水。

```
list_of_tuples = [
    ('john', 27, 45000),
    ('jane', 25, 65000),
    ('beth', 31, 70000)
]
```

我们可以按字母顺序、年龄或薪水对这个列表进行排序。我们可以指定我们想要使用的密钥参数。

要按年龄排序，我们可以使用以下代码:

```
sorted_age_list = sorted(list_of_tuples, key = lambda person: person[1])print(sorted_age_list) 
# [('jane', 25, 65000), ('john', 27, 45000), ('beth', 31, 70000)]
```

*list_of_tuples* 的每个元素都作为 *person 参数*传递给 lambda 函数。返回每个元组的索引 1 处的元素。这是用于对列表进行排序的值，即年龄。

要按字母顺序对名称进行排序，我们可以不传递任何键，因为默认情况下，每个元组的第一个元素就是要比较的内容(记住，默认情况下，字符串是按字母顺序排序的):

```
sorted_name_list = sorted(list_of_tuples)print(sorted_name_list) 
# [('beth', 31, 70000), ('jane', 25, 65000), ('john', 27, 45000)]
```

但是，我们可以指定要按每个元组的第一个元素进行排序，如下所示:

```
sorted_name_list = sorted(list_of_tuples, key = lambda person: person[0])print(sorted_name_list) 
# [('beth', 31, 70000), ('jane', 25, 65000), ('john', 27, 45000)]
```

记住，我们可以将 lambda 表达式赋给变量(类似于使用 def 关键字定义函数)。因此，我们可以根据 lambda 表达式用来对列表进行排序的标准来组织它们:

```
name = lambda person: person[0]
age = lambda person: person[1]
salary = lambda person: person[2]# sort by name
sorted(list_of_tuples, key = name)# sort by age
sorted(list_of_tuples, key = age)# sort by salary
sorted(list_of_tuples, key = salary)
```

[](/sorting-a-dictionary-in-python-4280451e1637) [## 用 Python 对字典进行排序

### 如何用 python 对字典进行排序

towardsdatascience.com](/sorting-a-dictionary-in-python-4280451e1637) 

如果你喜欢阅读这样的故事，并想支持我成为一名作家，考虑注册成为一名媒体会员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你用我的 [***链接***](https://lmatalka90.medium.com/membership) *注册，我会赚一小笔佣金。*

[](https://lmatalka90.medium.com/membership) [## 通过我的推荐链接加入媒体——卢艾·马塔尔卡

### 阅读卢艾·马塔尔卡的每一个故事(以及媒体上成千上万的其他作家)。您的会员费直接支持…

lmatalka90.medium.com](https://lmatalka90.medium.com/membership) 

## 结论

在本教程中，我们比较了 sort()方法和 sorted()函数在基于不同标准对列表进行排序时的情况。我们学习了 sort()方法如何修改原始列表，sorted()函数返回一个新列表。