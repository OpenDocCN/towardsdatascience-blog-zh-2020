# Python 3.9 中的字典联合运算符和新的字符串方法

> 原文：<https://towardsdatascience.com/dictionary-union-operators-and-new-string-methods-in-python-3-9-4688b261a417?source=collection_archive---------49----------------------->

## 可以简化您的数据处理代码的新功能—它们是什么？它们改进了什么？

![](img/584dcfff7238f9a0a5e7a3294acba5c5.png)

卡南·哈斯马多夫在 [Unsplash](https://unsplash.com/s/photos/9?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

Python 3.9 已经积累了一份长长的改进列表，其中有一些非常重要的变化，比如一种新型的解析器。与 LL(1)解析器相比，新的 [PEG](https://medium.com/@gvanrossum_83706/peg-parsers-7ed72462f97c) 解析器占用了更多的内存，但是速度更快，应该能够更好地处理某些情况。虽然，根据发布文档，我们可能从 Python 3.10 开始才能看到真正的效果。Python 3.9 还扩展了一些内置和标准库集合类型的使用，作为通用类型，而不必求助于`typing`模块。如果您使用类型检查器、linters 或 ide，这尤其有用。

也许，在处理文本数据的日常工作中最方便的两个特性是添加到`dict`中的联合操作符和新的`string`方法。

## `dict`的联合运算符

`dict`引入了[合并](https://www.python.org/dev/peps/pep-0584/) `|`和[更新](https://www.python.org/dev/peps/pep-0584/) `|=`两个联合运算符。

因此，如果您想基于您已经拥有的两个字典创建一个新的`dict`，您可以执行以下操作:

```
>>> d1 = {'a': 1}
>>> d2 = {'b': 2}
>>> d3 = d1 | d2
>>> d3
{'a': 1, 'b': 2}
```

如果您想用这两个字典中的一个来扩充另一个，您可以这样做:

```
>>> d1 = {'a': 1}
>>> d2 = {'b': 2}
>>> d1 |= d2
>>> d1
{'a': 1, 'b': 2}
```

在 Python 中已经有几种合并字典的方法。这可以使用`[dict.update()](https://docs.python.org/3/library/stdtypes.html#dict.update)`来完成:

```
>>> d1 = {'a': 1}
>>> d2 = {'b': 2}
>>> d1.update(d2)
>>> d1
{'a': 1, 'b': 2}
```

然而，`dict.update()`修改了您正在就地更新的`dict`。如果你想有一个新的字典，你必须先复制一个现有的字典:

```
>>> d1 = {'a': 1}
>>> d2 = {'b': 2}
>>> d3 = d1.copy()
>>> d3
{'a': 1}
>>> d3.update(d2)
>>> d3
{'a': 1, 'b': 2}
```

除此之外，在 Python 3.9 之前，还有 3 种方法可以实现这一点。合并两个字典也可以通过`[{**d1, **d2}](https://www.python.org/dev/peps/pep-0448/)`构造来实现:

```
>>> d1 = {'a': 1}
>>> d2  = {'b': 2}
>>> {**d1, **d2}
{'a': 1, 'b': 2}
```

但是，如果您使用的是`dict`的子类，例如`defaultdict`，这个构造将把新字典的类型解析为`dict`:

```
>>> d1
defaultdict(None, {0: 'a'})
>>> d2
defaultdict(None, {1: 'b'})
>>> {**d1, **d2}
{0: 'a', 1: 'b'}
```

工会运营商不会出现这种情况:

```
>>> d1
defaultdict(None, {0: 'a'})
>>> d2
defaultdict(None, {1: 'b'})
>>> d1 | d2
defaultdict(None, {0: 'a', 1: 'b'})
```

另一种将两个字典合并成一个新字典的方法是 `dict(d1, **d2)`:

```
>>> d1 = {'a': 1}
>>> d2  = {'b': 2}
>>> dict(d1, **d2)
{'a': 1, 'b': 2}
```

然而，这只适用于所有键都是类型`string`的字典:

```
>>> d1 = {'a': 1}
>>> d2 = {2: 'b'}
>>> dict(d1, **d2)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: keyword arguments must be strings
```

第三种选择是使用 `[collections.ChainMap](https://www.python.org/dev/peps/pep-0584/#id15)`。这可能比前两种方法更不直接，而且不幸的是，如果您更新链图对象，会修改底层字典:

```
>>> d1 = {'a': 1, 'b': 2}
>>> d2 = {'b': 3}
>>> from collections import ChainMap
>>> d3 = ChainMap(d1, d2)
>>> d3
ChainMap({'a': 1, 'b': 2}, {'b': 3})
>>> d3['b'] = 4
>>> d3
ChainMap({'a': 1, 'b': 4}, {'b': 3})
>>> d1
{'a': 1, 'b': 4}
>>> d2
{'b': 3}
```

用于`dict`的新的合并和更新 union 操作符解决了这些问题，看起来没有那么麻烦。但是，它们可能会与按位“或”和其他一些运算符混淆。

## 移除前缀和后缀的字符串方法

正如 [PEP-616](https://www.python.org/dev/peps/pep-0616/) 文档所建议的，几乎任何时候你使用`str.startswith()`和`str.endswith()`方法，你都可以简化代码如下。

以前，如果您正在规范化一个列表，例如，电影标题，并从无序列表中删除任何额外的标记，如破折号，您可以这样做:

```
>>> normalized_titles = []
>>> my_prefixed_string = '- Shark Bait'
>>> if my_prefixed_string.startswith('- '):
...     normalized_titles.append(my_prefixed_string.replace('- ', ''))
>>> print(normalized_titles)
['Shark Bait']
```

然而，使用`str.replace()`可能会产生不必要的副作用，例如，如果破折号是电影标题的一部分，而您实际上更愿意保留它，如下例所示。在 Python 3.9 中，您可以安全地这样做:

```
>>> normalized_titles = []
>>> my_prefixed_string = '- Niko - The Flight Before Christmas'
>>> if my_prefixed_string.startswith('- '):
...     normalized_titles.append(my_prefixed_string.removeprefix('- '))
>>> print(normalized_titles)
['Niko - The Flight Before Christmas']
```

PEP 文档指出，用户报告期望来自另一对内置方法`[str.lstrip](https://docs.python.org/3/library/stdtypes.html#str.lstrip)`和`[str.rstrip](https://docs.python.org/3/library/stdtypes.html#str.rstrip)`的这种行为，并且通常必须实现他们自己的方法来实现这种行为。这些新方法应该以更健壮和有效的方式提供该功能。用户将不必关心空字符串的情况或使用`str.replace()`。

另一个典型的用例如下:

```
>>> my_affixed_string = '"Some title"'
>>> if my_affixed_string.startswith('"'):
...     my_affixed_string = my_affixed_string[1:]
...
>>> if my_affixed_string.endswith('"'):
...     my_affixed_string = my_affixed_string[:-1]
...
>>> my_affixed_string
'Some title'
```

在 Python 3.9 中，这可以简化为:

```
>>> my_affixed_string = '"Some title"'
>>> my_affixed_string.removeprefix('"').removesuffix('"')
'Some title'
```

`bytes`、`bytearray`和`collections.UserString`也宣布了这些方法。

Python 3.9 还有一系列有趣的模块改进和优化——一定要看看这些改进和优化，找出对您的应用程序最有用的。享受新功能！

如果您对 Python 感兴趣，请随意查看我关于[并发性和并行性](https://annaeastori.medium.com/concurrency-and-parallelism-in-python-bbd7af8c6625)的文章。