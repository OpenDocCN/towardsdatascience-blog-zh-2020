# Python 中的词典理解

> 原文：<https://towardsdatascience.com/dictionary-comprehensions-in-python-912a453da512?source=collection_archive---------21----------------------->

## 如何使用字典理解在 Python 中创建字典

![](img/0cda26f7eb6ab8bf1494fe507289ceb3.png)

[天一马](https://unsplash.com/@tma?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

## 创建字典

假设我们想用 python 从另一个可迭代对象或序列(比如一个列表)创建一个字典。例如，我们有一个数字列表，我们想创建一个字典来计算每个元素在列表中出现的次数。因此，我们可以让字典的键成为列表中的不同元素(或数字)，它们对应的值等于特定元素(或数字)在列表中出现的次数。

我们可以使用 for 循环来创建这个字典，如下所示:

```
num_list = [1,1,7,3,5,3,2,9,5,1,3,2,2,2,2,2,9]count_dict = {}for num in num_list:
    count_dict[num] = num_list.count(num)print(count_dict)
# {1: 3, 7: 1, 3: 3, 5: 2, 2: 6, 9: 2}
```

注意，我们必须先创建一个空字典， *count_dict* 。然后，当我们使用 for 循环遍历 *num_list* 时，我们在 *count_dict* 中创建了 key:value 对。在遍历 *num_list* 时，该键将等于我们当前所在的数字，其对应的值等于 *num_list* 中该数字的计数。

count()是一个列表方法，它返回我们传入的值在列表中出现的次数。

## 词典释义

就像我们在 python 中有列表理解一样，我们也有字典理解。我们可以回忆一下，列表理解允许我们以一种非常简洁的方式从其他序列中创建列表。

这里有一个完整的列表理解教程

[](/list-comprehensions-in-python-28d54c9286ca) [## Python 中的列表理解

### 用 python 创建列表的更优雅、更简洁的方式

towardsdatascience.com](/list-comprehensions-in-python-28d54c9286ca) 

嗯，我们也可以使用 python 中的字典理解来以非常简洁的方式创建字典。

> { key:key 的值，iterable 或 sequence 中的值}

例如，我们可以使用字典理解来创建上面的字典，如下所示:

```
num_list = [1,1,7,3,5,3,2,9,5,1,3,2,2,2,2,2,9]count_dict = {num:num_list.count(num) for num in num_list}print(count_dict)
# {1: 3, 7: 1, 3: 3, 5: 2, 2: 6, 9: 2}
```

就是这样！首先，我们使用花括号来确定我们想要创建一个字典(与使用括号的列表相反)。然后，当我们在 *num_list* 上循环时，我们创建如下的键:值对:*num:num _ list . count(num)*，其中键是数字(或 num)，它的值是那个数字在 *num_list* 中的计数。

## 有条件的词典理解

就像在 list comprehensions 中一样，我们可以在 for 循环后使用 if 语句将条件添加到字典理解中。

> { key:key 的值，iterable 或 sequence if <condition>中的值}</condition>

例如，如果我们只想在字典中查找偶数，我们可以使用下面的代码:

```
num_list = [1,1,7,3,5,3,2,9,5,1,3,2,2,2,2,2,9]count_dict = {num:num_list.count(num) for num in num_list if num_list.count(num)%2==0}print(count_dict)
# {5: 2, 2: 6, 9: 2}
```

正如我们所看到的，我们在字典理解中的 for 循环之后添加了一个 if 语句。如果当前数字的计数是偶数，我们将相应的键:值对添加到字典中。否则，我们不会将键:值对添加到字典中。

[](/three-concepts-to-become-a-better-python-programmer-b5808b7abedc) [## 成为更好的 Python 程序员的三个概念

### 了解 Python 中的*和**运算符，*args 和**kwargs 以及更多内容

towardsdatascience.com](/three-concepts-to-become-a-better-python-programmer-b5808b7abedc) 

## 额外收获——对字典进行排序

我们还可以使用 sorted()函数根据计数对字典进行排序，如下所示:

```
num_list = [1,1,7,3,5,3,2,9,5,1,3,2,2,2,2,2,9]count_dict = {num:num_list.count(num) for num in num_list}sorted_dict = dict(sorted(count_dict.items(), key=lambda x:x[1]))print(sorted_dict)
# {7: 1, 5: 2, 9: 2, 1: 3, 3: 3, 2: 6}
```

有关上述代码的完整解释，请查看 python 中的对象排序教程:

[](https://levelup.gitconnected.com/the-ultimate-guide-to-sorting-in-python-d07349fb96d5) [## Python 排序的终极指南

### 如何在 python 中对列表、元组、字符串和字典进行排序

levelup.gitconnected.com](https://levelup.gitconnected.com/the-ultimate-guide-to-sorting-in-python-d07349fb96d5) 

如果你喜欢阅读这样的故事，并想支持我成为一名作家，考虑注册成为一名媒体会员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你用我的 [*链接*](https://lmatalka90.medium.com/membership) *注册，我会赚一小笔佣金。*

[](https://lmatalka90.medium.com/membership) [## 通过我的推荐链接加入媒体——卢艾·马塔尔卡

### 阅读卢艾·马塔尔卡的每一个故事(以及媒体上成千上万的其他作家)。您的会员费直接支持…

lmatalka90.medium.com](https://lmatalka90.medium.com/membership) 

## 结论

在这个简短的教程中，我们首先回顾了如何使用一个 iterable 对象(比如一个 list)创建一个带有 for 循环的字典，其中我们必须先创建一个空字典。然后，我们学习了如何使用字典理解来以更简洁和 pythonic 化的方式创建同一个字典，而不必先创建一个空字典。然后，我们看到了如何使用 if 语句将条件添加到字典理解中。最后，我们使用 sorted()函数根据列表中数字的计数值对字典进行了排序。