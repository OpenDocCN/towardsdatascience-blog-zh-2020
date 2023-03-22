# Python 中的字典

> 原文：<https://towardsdatascience.com/dictionaries-in-python-84b05bb95629?source=collection_archive---------8----------------------->

![](img/a4a9c2ca4369e763568b323c53b18c31.png)

由[玛蒂尔德·德库尔塞尔](https://unsplash.com/@mathilde_34?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## Python 中最重要的数据结构之一

在这篇文章中，我将谈论**字典**。这是“Python 中的数据结构”系列的第二篇文章。这个系列的第一部分是关于列表的。

字典是 Python 中使用**键**进行索引的重要数据结构。它们是一个无序的项目序列(键值对)，这意味着顺序没有被保留。密钥是不可变的。就像列表一样，字典的值可以保存**异构数据**即整数、浮点、字符串、NaN、布尔、列表、数组，甚至嵌套字典。

这篇文章将为您提供清晰的理解，并使您能够熟练地使用 **Python 字典**。

本文涵盖了以下主题:

*   **创建字典并添加元素**
*   **访问字典元素**
*   **删除字典元素**
*   **添加/插入新元素**
*   **合并/连接字典**
*   **修改字典**
*   **整理字典**
*   **字典理解**
*   **创建词典的替代方法**
*   **抄字典**
*   **重命名现有密钥**
*   **嵌套字典**
*   **检查字典中是否存在关键字**

# 1)创建字典并添加元素

像列表用方括号([ ])初始化，字典用花括号({ })初始化。当然，空字典的长度为零。

```
dic_a = {} # An empty dictionarytype(dic_a)
>>> dictlen(dic_a)
>>> 0
```

一个字典有两个典型特征:**键**和**值**。**每个键都有相应的值。**键和值都可以是字符串、浮点、整数、NaN 等类型。向字典中添加元素意味着添加一个键值对。**字典由一个或多个键值对组成。**

让我们给我们的空字典添加一些元素。下面是这样做的一种方法。这里，`'A'`是键，`'Apple'`是它的值。您可以添加任意多的元素。

```
# Adding the first element
dic_a['A'] = 'Apple'print (dic_a)
>>> {'A': 'Apple'}# Adding the second element
dic_a['B'] = 'Ball'print (dic_a)
>>> {'A': 'Apple', 'B': 'Ball'}
```

**注意:** Python 区分大小写，`'A'`和`'a'`充当两个不同的键。

```
dic_a['a'] = 'apple'print (dic_a)
>>> {'A': 'Apple', 'B': 'Ball', 'a': 'apple'}
```

## 一次初始化字典

如果您发现上述逐个添加元素的方法令人厌烦，您还可以通过指定所有的键-值对来立即初始化字典。

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}
```

## 异质词典

到目前为止，您的字典将字符串作为键和值。字典也可以存储混合类型的数据。以下是有效的 Python 字典。

```
dic_b = {1: 'Ace', 'B': 123, np.nan: 99.9, 'D': np.nan, 'E': np.inf}
```

不过，您应该为关键字使用有意义的名称，因为它们表示字典的索引。特别要避免使用 floats 和 **np.nan** 作为键。

# 2)访问字典元素

创建了字典之后，让我们看看如何访问它们的元素。

## 访问键和值

您可以分别使用功能`dict.keys()`和`dict.values()`访问键和值。您还可以使用`items()`函数以元组的形式访问键和值。

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}dic_a.keys()
>>> dict_keys(['A', 'B', 'C'])dic_a.values()
>>> dict_values(['Apple', 'Ball', 'Cat'])dic_a.items()
>>> dict_items([('A', 'Apple'), ('B', 'Ball'), ('C', 'Cat')])
```

或者，您也可以使用“for”循环一次访问/打印一个。

```
# Printing keys
for key in dic_a.keys():
    print (key, end=' ')
>>> A B C############################## Printing values
for key in dic_a.values():
    print (key, end=' ')
>>> Apple Ball Cat
```

您可以避免两个“for”循环，并使用`items()`访问键和值。“for”循环将遍历由`items()`返回的键值对。这里，`key`和`value`是任意的变量名。

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}for key, value in dic_a.items():
    print (key, value)
>>> A Apple
    B Ball
    C Cat
```

## 访问单个元素

无法使用列表式索引来访问词典条目。

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}dic_a[0]
>>> **---->** 1 dic_a[**0**] **KeyError**: 0
```

您需要使用键从字典中访问相应的值。

```
# Accessing the value "Apple"
dic_a['A']
>>> 'Apple'# Accessing the value "Cat"
dic_a['C']
>>> 'Cat'
```

如果字典中不存在该键，您将得到一个错误。

```
dic_a['Z']**>>> KeyError** Traceback (most recent call last)
**----> 1** dic_a**['Z']****KeyError**: 'Z'
```

如果您想在不存在键的情况下避免这样的键错误，您可以使用`get()`功能。当键不存在时，返回`None`。您也可以使用自定义消息来返回。

```
print (dic_a.get('Z'))
>>> None# Custom return message
print (dic_a.get('Z', 'Key does not exist'))
>>> Key does not exist
```

## 像在列表中一样访问字典元素

如果您想使用索引来访问字典元素(键或值)，您需要首先将它们转换成列表。

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}list(dic_a.keys())[0]
>>> 'A'list(dic_a.keys())[-1]
>>> 'C'list(dic_a.values())[0]
>>> 'Apple'list(dic_a.values())[-1]
>>> 'Cat'
```

# 3)删除字典元素

从字典中删除元素意味着一起删除一个键值对。

## 使用 del

您可以使用`del`关键字和您想要删除其值的键来删除字典元素。**删除是原位的**，这意味着您不需要在删除后重新分配字典的值。

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}# Deleting the key-value pair of 'A': 'Apple'
del dic_a['A']print (dic_a)
>>> {'B': 'Ball', 'C': 'Cat'}# Deleting the key-value pair of 'C': 'Cat'
del dic_a['C']print (dic_a)
>>> {'B': 'Ball'}
```

## 使用 pop()

您还可以使用“pop()”函数来删除元素。它返回被弹出(删除)的值，字典被就地**修改**。

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}dic_a.pop('A')
>>> 'Apple' print (dic_a)
# {'B': 'Ball', 'C': 'Cat'}
```

在上述两种方法中，如果要删除的键在字典中不存在，您将得到一个 KeyError。在“pop()”的情况下，如果键不存在，您可以指定要显示的错误消息。

```
key_to_delete = 'E'
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}dic_a.pop(key_to_delete, f'Key {key_to_delete} does not exist.')
>>> 'Key E does not exist.'
```

## 删除多个元素

没有直接的方法，但是你可以使用如下所示的“for”循环。

```
to_delete = ['A', 'C']
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}for key in to_delete:
    del dic_a[key]

print (dic_a)
>>> {'B': 'Ball'}
```

# 4)添加/插入新元素

您可以一次添加一个元素到一个已经存在的字典中，如下所示。

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}dic_a['D'] = 'Dog'print (dic_a)
>>> {'A': 'Apple', 'B': 'Ball', 'C': 'Cat', 'D': 'Dog'}dic_a['E'] = 'Egg'print (dic_a)
>>> {'A': 'Apple', 'B': 'Ball', 'C': 'Cat', 'D': 'Dog', 'E': 'Egg'}
```

如果您添加的密钥已经存在，现有值将被覆盖**。**

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}dic_a['A'] = 'Adam' # key 'A' already exists with the value 'Apple'print (dic_a)
>>> {'A': 'Adam', 'B': 'Ball', 'C': 'Cat'}
```

## 使用更新( )

您还可以使用`update()`函数添加一个新的键-值对，方法是将该对作为参数传递。

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}
dic_a.update({'D': 'Dog'})print (dic_a)
>>> {'A': 'Apple', 'B': 'Ball', 'C': 'Cat', 'D': 'Dog'}
```

`update()`函数还允许您同时向现有字典添加多个键值对。

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}
dic_b = {'D':'Dog', 'E':'Egg'}dic_a.update(dic_b)print(dic_a)
>>> {'A': 'Apple', 'B': 'Ball', 'C': 'Cat', 'D': 'Dog', 'E': 'Egg'}
```

# 5)合并/连接字典

从 Python 3.5 开始，可以使用解包操作符(`**`)合并两个或多个字典。

```
dic_a = {'A': 'Apple', 'B': 'Ball'}
dic_b = {'C': 'Cat', 'D': 'Dog'}dic_merged = {**dic_a, **dic_b}print (dic_merged)
>>> {'A': 'Apple', 'B': 'Ball', 'C': 'Cat', 'D': 'Dog'}
```

如果您不想创建一个新的字典，而只想将`dic_b`添加到现有的`dic_a`中，您可以简单地更新第一个字典，如前面所示。

```
dic_a = {'A': 'Apple', 'B': 'Ball'}
dic_b = {'C': 'Cat', 'D': 'Dog'}dic_a.update(dic_b)print (dic_a)
>>> {'A': 'Apple', 'B': 'Ball', 'C': 'Cat', 'D': 'Dog'}
```

## 串联时如何处理重复键？

Python 字典的一个特点是**它们不能有重复的键**，也就是说，一个键不能出现两次。那么，如果你把两个或更多有一个或多个公共关键字的字典连接起来，会发生什么呢？

答案是最后合并的字典中的键值对(按合并的顺序)会存活下来。在下面的例子中，键`'A'`存在于所有三个字典中，因此，最终的字典采用最后一个合并的字典的值(`dic_c`)。

```
dic_a = {'A': 'Apple', 'B': 'Ball'}
dic_b = {'C': 'Cat', 'A': 'Apricot'}
dic_c = {'A': 'Adam', 'E': 'Egg'}dic_merged = {**dic_a, **dic_b, **dic_c}print (dic_merged)
>>> {'A': 'Adam', 'B': 'Ball', 'C': 'Cat', 'E': 'Egg'}
```

## 一句警告

我刚刚说过字典不能有重复的键。严格地说，您可以定义一个具有重复键的字典，但是，当您打印它时，将只打印最后一个重复键。如下所示，只返回唯一的键，对于重复的键(此处为“A”)，只返回最后一个值。

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'A': 'Apricot', 'A': 'Assault'}print (dic_a)
>>> {'A': 'Assault', 'B': 'Ball'}
```

## Python 3.9+中更简单的方法

从 Python 3.9 开始，可以使用`|`操作符连接两个或更多的字典。

```
dic_a = {'A': 'Apple', 'B': 'Ball'}
dic_b = {'C': 'Cat', 'D': 'Dog'}dic_c = dic_a | dic_b
>>> {'A': 'Apple', 'B': 'Ball', 'C': 'Cat', 'D': 'Dog'}# Concatenating more than 2 dictionaries
dic_d = dic_a | dic_b | dic_c
```

# 6)修改字典

如果想把`'A'`的值从`'Apple'`改成`'Apricot'`，可以用一个简单的赋值。

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}dic_a['A'] = 'Apricot'print (dic_a)
>>> {'A': 'Apricot', 'B': 'Ball', 'C': 'Cat', 'D': 'Dog'}
```

# 7)整理字典

字典中不维护顺序。使用`sorted()`功能，您可以使用关键字或值对字典进行排序。

## 使用关键字排序

如果键是字符串(字母)，它们将按字母顺序排序。在字典中，我们有两个主要元素:键和值。因此，在根据键进行排序时，我们使用第一个元素，即键，因此，lambda 函数中使用的索引是“[0]”。你可以阅读[这篇文章](https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions)了解更多关于 lambda 函数的信息。

```
dic_a = {'B': 100, 'C': 10, 'D': 90, 'A': 40}sorted(dic_a.items(), key=lambda x: x[0])
>>> [('A', 40), ('B', 100), ('C', 10), ('D', 90)]
```

分类不到位。如下所示，如果您现在打印字典，它仍然是无序的，和最初初始化时一样。排序后，您必须重新分配它。

```
# The dictionary remains unordered if you print it
print (dic_a)
>>> {'B': 100, 'C': 10, 'D': 90, 'A': 40}
```

如果您想以相反的顺序排序，请指定关键字`reverse=True`。

```
sorted(dic_a.items(), key=lambda x: x[0], reverse=True)
>>> [('D', 90), ('C', 10), ('B', 100), ('A', 40)]
```

## 使用值排序

要根据字典的值对其进行排序，需要在 lambda 函数中使用索引“[1]”。

```
dic_a = {'B': 100, 'C': 10, 'D': 90, 'A': 40}sorted(dic_a.items(), key=lambda x: x[1])
>>> [('C', 10), ('A', 40), ('D', 90), ('B', 100)]
```

# 8)词典理解

这是动态创建字典**的一个非常有用的方法**。假设您想要创建一个字典，其中键是一个整数，值是它的平方。字典的理解应该是这样的。

```
dic_c = {i: i**2 for i in range(5)}print (dic_c)
>>> {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

如果你希望你的键是字符串，你可以使用“f-strings”。

```
dic_c = {f'{i}': i**2 for i in range(5)}print (dic_c)
>>> {'0': 0, '1': 1, '2': 4, '3': 9, '4': 16}
```

# 9)创建词典的替代方法

## 从列表创建字典

假设你有两个列表，你想用它们创建一个字典。最简单的方法是使用`dict()`构造函数。

```
names = ['Sam', 'Adam', 'Tom', 'Harry']
marks = [90, 85, 55, 70]dic_grades = dict(zip(names, marks))print (dic_grades)
>>> {'Sam': 90, 'Adam': 85, 'Tom': 55, 'Harry': 70}
```

您还可以将这两个列表压缩在一起，并使用前面所示的“字典理解”创建字典。

```
dic_grades = {k:v for k, v in zip(names, marks)}print (dic_grades)
>>> {'Sam': 90, 'Adam': 85, 'Tom': 55, 'Harry': 70}
```

## 传递键值对

您还可以将逗号分隔的键值对列表传递给`dict()`构造，它将返回一个字典。

```
dic_a = dict([('A', 'Apple'), ('B', 'Ball'), ('C', 'Cat')])print (dic_a)
>>> {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}
```

如果您的键是字符串，您甚至可以使用更简单的初始化，只使用变量作为键。

```
dic_a = dict(A='Apple', B='Ball', C='Cat')print (dic_a)
>>> {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}
```

# 10)复制字典

我将用一个简单的例子来解释这一点。字典的复制机制涉及到更多的微妙之处，我建议读者参考[这篇](https://stackoverflow.com/q/3975376/4932316)堆栈溢出帖子，了解详细的解释。

## 参考分配

当您简单地将一个现有字典(父字典)重新分配给一个新字典时，两者都指向同一个对象(**“引用分配】**)。

考虑下面的例子，您将`dic_a`重新分配给`dic_b`。

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}dic_b = dic_a # Simple reassignment **(Reference assignment)**
```

现在，如果您修改`dic_b`(例如添加一个新元素)，您会注意到这一变化也会反映在`dic_a`中。

```
dic_b['D'] = 'Dog'print (dic_b)
>>> {'A': 'Apple', 'B': 'Ball', 'C': 'Cat', 'D': 'Dog'}print (dic_a)
>>> {'A': 'Apple', 'B': 'Ball', 'C': 'Cat', 'D': 'Dog'}
```

## 浅拷贝

使用`copy()`函数创建一个浅层副本。在浅层拷贝中，两个字典作为两个独立的对象，它们的内容仍然共享相同的引用。如果在新字典(浅层拷贝)中添加一个新的键值对，它将不会出现在父字典中。

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}
dic_b = dic_a.copy()dic_b['D'] = 'Dog'# New, shallow copy, has the new key-value pair
print (dic_b)
>>> {'A': 'Apple', 'B': 'Ball', 'C': 'Cat', 'D': 'Dog'}# The parent dictionary does not have the new key-value pair
print (dic_a)
>>> {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}
```

现在，父字典(" dic_a ")中的内容是否会改变取决于值的类型。例如，在下面的例子中，内容是简单的不可变的字符串。因此，改变“dic_b”中给定键的值(本例中为`'A'`)不会改变“dic_a”中键`'A'`的值。

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}
dic_b = dic_a.copy()# Replace an existing key with a new value in the shallow copy
dic_b['A'] = 'Adam'print (dic_b)
>>> {'A': 'Adam', 'B': 'Ball', 'C': 'Cat'}# Strings are immutable so 'Apple' doesn't change to 'Adam' in dic_a
print (dic_a)
>>> {'A': 'Apple', 'B': 'Ball', 'C': 'Cat'}
```

但是，如果“dic_a”中关键字`'A'`的值是一个列表，那么在“dic_b”中改变它的值将反映“dic_a”(父字典)中的变化，因为列表是可变的。

```
dic_a = {'A': ['Apple'], 'B': 'Ball', 'C': 'Cat'}# Make a shallow copy
dic_b = dic_a.copy()dic_b['A'][0] = 'Adam'print (dic_b)
>>> {'A': ['Adam'], 'B': 'Ball', 'C': 'Coal'}# Lists are mutable so the changes get reflected in dic_a too
print (dic_a)
>>> {'A': ['Adam'], 'B': 'Ball', 'C': 'Cat'}
```

# 11)重命名现有密钥

假设您想将键“Adam”替换为“Alex”。您可以使用`pop`函数，因为它删除传递的键(这里是“亚当”)并返回删除的值(这里是 85)。所以你一箭双雕。使用返回的(已删除的)值将该值分配给新键(此处为“Alex”)。可能有更复杂的情况，其中键是一个元组。这种情况超出了本文的范围。

```
dic_a = {'Sam': 90, 'Adam': 85, 'Tom': 55, 'Harry': 70}dic_a['Alex'] = dic_a.pop('Adam')print (dic_a)
>>> {'Sam': 90, 'Tom': 55, 'Harry': 70, 'Alex': 85}
```

# 12)嵌套字典

嵌套字典在一个字典中有一个或多个字典。下面是具有两层嵌套的嵌套字典的最简单的例子。这里，外部字典(第 1 层)只有一个键值对。然而，该值现在是字典本身。

```
dic_a = {'A': {'B': 'Ball'}}dic_a['A']
>>> {'B': 'Ball'}type(dic_a['A'])
>>> dict
```

如果想要进一步访问内部字典(第 2 层)的键-值对，现在需要使用`dic_a['A']`作为字典。

```
dic_a['A']['B']
>>> 'Ball'
```

# 三层字典

让我们添加一个额外的嵌套字典层。现在，`dic_a['A']`本身就是一个嵌套字典，不像上面最简单的嵌套字典。

```
dic_a = {'A': {'B': {'C': 'Cat'}}}# Layer 1
dic_a['A']
>>> {'B': {'C': 'Cat'}}# Layer 2
dic_a['A']['B']
>>> {'C': 'Cat'}# Layer 3
dic_a['A']['B']['C']
>>> 'Cat'
```

# 13)检查字典中是否存在关键字

您可以使用`in`操作符来查找特定的键是否存在于字典中。

```
dic_a = {'A': 'Apple', 'B': 'Ball', 'C': 'Cat', 'D': 'Dog'}'A' in dic_a
# True'E' in dic_a
# False
```

在上面的代码中，不需要使用“in dic_a.keys()”，因为“in dic_a”已经在键中查找了。

这让我想到了这篇文章的结尾。您可以在这里访问本系列的第一部分“Python 中的数据结构”[。](/data-structures-in-python-da813beb2a0d)