# Python 字符串 101

> 原文：<https://towardsdatascience.com/5-quick-tips-to-manage-your-strings-in-python-4e7e48f72560?source=collection_archive---------21----------------------->

## 掌握基础知识

![](img/72edcdd608f5bd20021c2442974cce30.png)

[图片来自 Pixabay 中的 stock snap](https://pixabay.com/photos/school-books-desk-chalkboard-chalk-926213/)

这篇文章非常实用，为你提供了在 Python 中使用字符串的简单快捷的技巧。

# 字符串乘法

Python 不仅允许数字相乘，还允许字符串相乘！重复一个字符串，如下所示:

```
>>> "a"*3
'aaa'
>>>
```

您也可以将字符串乘以一个布尔值。例如，如果一个数字小于 10，下面的代码片段会用前导零填充该数字:

```
>>> sn = lambda n: '{}{}'.format('0' * (n < 10), n)
>>> sn(9)
'09'
>>> sn(10)
'10'
>>>
```

# 标题

如今，写标题很常见(即使是中号)，每个单词的首字母都要大写。嗯，用 Python 可以很容易地做到这一点:

```
>>> "the curious frog went to denmark".title()
'The Curious Frog Went To Denmark'
>>>
```

# 连锁的

您可以使用+运算符连接两个字符串，但这不是最好的方法。Python 字符串是不可变的，使得这个操作符从这两个字符串中创建一个新的字符串，而不是像我们所想的那样，简单地将第二个字符串添加到第一个字符串中。更好的方法([和更快的](/3-techniques-to-make-your-python-code-faster-193ffab5eb36))是使用 [join()](https://docs.python.org/3/library/stdtypes.html#str.join) 函数将它们连接起来:

```
>>> "".join(["abc", "def"])
'abcdef'
>>>
```

# 获取子字符串

使用 Python 的切片获得子串。您只需要指定起始索引(包括)和结束索引(不包括):

```
>>> "Such a good day"[7:11]
'good'
>>>
```

# 反转一根绳子

字符串切片允许您选择性地定义第三个参数:步长。如果您使用完整的字符串并应用步长-1，您将得到相反的字符串:

```
>>> "Such a good day"[::-1]
'yad doog a hcuS'
>>>
```

就是这样！访问 Python 的[字符串文档](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)了解更多信息。