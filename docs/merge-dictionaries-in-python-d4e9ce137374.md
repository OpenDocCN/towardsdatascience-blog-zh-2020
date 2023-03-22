# 用 Python 合并字典的 5 种方法

> 原文：<https://towardsdatascience.com/merge-dictionaries-in-python-d4e9ce137374?source=collection_archive---------0----------------------->

## 编程；编排

## Python 3.9 中合并 Python 字典和新运算符的不同方式

![](img/1731921ba1a94bf423b79ea31443c5ce.png)

图片来自 [Clker-Free-Vector-Images](https://pixabay.com/users/Clker-Free-Vector-Images-3736/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=39400) 来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=39400)

在 Python 中，字典是一种数据结构，它包含键-值对形式的元素，其中键用于访问字典的值。Python 字典是无序和可变的，即字典的元素可以改变。

在本文中，我们将探索合并两个或更多字典的五种不同方法，以及一种粗略的方法。

对于本文，让我们创建两个字典`d1`和`d2`，我们希望将它们连接成一个字典:

```
d1 = {'India': 'Delhi',
      'Canada': 'Ottawa',
      'United States': 'Washington D. C.'}

d2 = {'France': 'Paris',
      'Malaysia': 'Kuala Lumpur'}
```

## 粗暴的方式

通过用第一个字典迭代第二个字典的键值对，可以合并两个字典。

```
d3 = d1.copy()
for key, value in d2.items():
    d3[key] = value

print(d3)
```

```
Output:

{'India': 'Delhi',
 'Canada': 'Ottawa',
 'United States': 'Washington D. C.',
 'France': 'Paris',
 'Malaysia': 'Kuala Lumpur'}
```

现在，让我们看看合并字典的更干净、更好的方法:

## 方法 1:使用更新方法

Dictionary 有一个方法 [update()](https://docs.python.org/3/library/stdtypes.html#dict.update) ，该方法将 dictionary 与其他 dictionary 中的条目合并，并覆盖现有的键。

```
d4 = d1.copy()
d4.update(d2)

print(d4)
```

```
Output:
{'India': 'Delhi',
 'Canada': 'Ottawa',
 'United States': 'Washington D. C.',
 'France': 'Paris',
 'Malaysia': 'Kuala Lumpur'}
```

update 方法修改当前字典。因此，在对字典进行操作之前，您可能需要创建字典的副本。

## 方法 2:使用解包操作符

我们可以通过简单地使用解包操作符(**)将字典合并在一行中。

```
d5 = {**d1, **d2}

print(d5)
```

```
Output:
{'India': 'Delhi',
 'Canada': 'Ottawa',
 'United States': 'Washington D. C.',
 'France': 'Paris',
 'Malaysia': 'Kuala Lumpur'}
```

我们也可以用这种方法合并多个字典。

```
{**dict1, **dict2, **dict3}
```

## 方法 3:使用集合。链式地图

这可能是最不为人知的合并字典的方法。集合模块中的
[ChainMap](https://docs.python.org/3/library/collections.html#collections.ChainMap) 类将多个字典组合在一个视图中。

```
from collections import ChainMap
d6 = ChainMap(d1, d2)

print(d6)
```

```
Output:
ChainMap({'Canada': 'Ottawa',
          'India': 'Delhi',
          'United States': 'Washington D. C.'},
         {'France': 'Paris',
          'Malaysia': 'Kuala Lumpur'})
```

这个方法返回一个 ChainMap 类的对象。我们仍然可以像使用任何其他词典一样使用这个对象。例如`d6[’India’]`将返回`'Delhi’`。

然而，在两个字典中有相同键的情况下，这个方法将返回第一个字典的值，这与从第二个字典返回值的其他方法不同。

```
x = {'A': 1, 'B': 2}
y = {'B': 10, 'C': 20}

z = ChainMap(x, y)
z['B']

# outputs 2
```

## 方法 4:打开第二本字典

我们可以通过打开第二本字典来合并字典。

```
d7 = dict(d1, **d2)

print(d7)
```

```
Output:
{'India': 'Delhi',
 'Canada': 'Ottawa',
 'United States': 'Washington D. C.',
 'France': 'Paris',
 'Malaysia': 'Kuala Lumpur'}
```

但是，只有当第二个字典的键是字符串时，这种方法才有效。

```
x = {1: 'A', 2: 'B'}
y = {3: 'C', 4: 'D'}

z = dict(x, **y)
```

```
Output:
TypeError: keyword arguments must be strings
```

## 方法 5:使用合并运算符

Python 3.9 在`dict`类中引入了[合并](https://docs.python.org/3.9/whatsnew/3.9.html#dictionary-merge-update-operators)操作符(|)。使用 merge 操作符，我们可以在一行代码中组合字典。

```
d8 = d1 | d2

print(d8)
```

```
Output:
{'India': 'Delhi',
 'Canada': 'Ottawa',
 'United States': 'Washington D. C.',
 'France': 'Paris',
 'Malaysia': 'Kuala Lumpur'}
```

我们还可以通过使用更新操作符(|=)来合并字典。

```
d1 |= d2
```

## 资源

本文中使用的代码片段可以在我的 [GitHub 页面](https://jimit105.github.io/medium-articles/5%20Ways%20to%20Merge%20Dictionaries%20in%20Python.html)上找到。

## 让我们连接

领英:[https://www.linkedin.com/in/jimit105/](https://www.linkedin.com/in/jimit105/)
GitHub:[https://github.com/jimit105](https://github.com/jimit105)
推特:[https://twitter.com/jimit105](https://twitter.com/jimit105)