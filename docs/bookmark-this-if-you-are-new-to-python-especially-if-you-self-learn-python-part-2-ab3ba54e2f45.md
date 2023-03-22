# (第 2 部分)如果您是 Python 的新手(尤其是自学 Python 的话)，请将此加入书签

> 原文：<https://towardsdatascience.com/bookmark-this-if-you-are-new-to-python-especially-if-you-self-learn-python-part-2-ab3ba54e2f45?source=collection_archive---------42----------------------->

## 提示和技巧的第二部分在这里。现在就学习这些来平滑你的编码吧！

首先，感谢大家支持我之前关于 Python 技巧和提示的文章。我不敢相信我的文章能吸引超过 12 万的浏览量。我希望你们都能从这篇文章中受益。如果你还没有读过第一本书，最好现在就读。

[](/bookmark-this-if-you-are-new-to-python-especially-if-you-self-learn-python-54c6e7b5dad8) [## 如果你是 Python 的新手(尤其是自学 Python 的话),请将此加入书签

### Python 中简单但有用的技巧和提示列表

towardsdatascience.com](/bookmark-this-if-you-are-new-to-python-especially-if-you-self-learn-python-54c6e7b5dad8) ![](img/faf2aed22ddb208cf77f845ad36c9a3e.png)

在 [Unsplash](https://unsplash.com/s/photos/python3?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

为了奖励大家，这里是 Python 技巧和提示的第 2 部分。除了展示技巧和提示，我还会包括解释和链接，这样你就可以了解更多。

让我们继续我们的 Python 之旅。

# 目录

[同时更新多个变量](#f0ab)

[从列表中返回一个长字符串](#6786)

[字符串格式化](#56a4)

[下划线](#123d)

[关键字、关键字值、关键字项](#2586)

[两组对比](#f8a8)

[收藏。计数器](#c0e5)

## 同时更新多个变量

代替

```
>>> a = 5
>>> b = 8
>>> temp = a
>>> a = b
>>> b = temp + a
>>> a
8
>>> b
13
```

您可以在一行中同时计算这两个变量。在这种情况下，您不需要创建临时变量。

```
>>> a = 5
>>> b = 8
>>> a,b = b, a+b
>>> a
8
>>> b
13
```

(PS:这是什么？斐波那契数列！)

更多信息:[评估订单](https://docs.python.org/3/reference/expressions.html#evaluation-order)

## 从列表中返回一个长字符串

```
>>> str_list = ['This', 'is', 'WYFok']
>>> ' '.join(str_list)
'This is WYFok'
```

您可以使用 join 函数将所有元素(需要是字符串格式)组合成一个字符串。您需要决定每个元素之间的分隔符是什么

进一步应用

```
>>> ans_list = [3,6,9]
>>> 'The answer is '+','.join(map(str,ans_list))
'The answer is 3,6,9'
```

使用 map 函数，你可以把整数类型的元素转换成字符串，结合 join 函数，你可以得到一个新的字符串变量。

进一步信息: [str.join](https://docs.python.org/3/library/stdtypes.html#str.join) ，[地图](https://docs.python.org/3/library/functions.html#map)

## 字符串格式

```
>>> pi = 3.14159
# Before Python3.6>>> print('The value of pi is {:.2f}'.format(pi))
The value of pi is 3.14# Python 3.6 and after 
>>> print(f'The value of pi is {pi:.2f}')
The value of pi is 3.14>>> pi
3.14159
```

字符串格式的一个优点是，您可以打印带有舍入的数值，而不会影响原始值的准确性。

更多信息: [PyFormat](https://pyformat.info/)

## 强调

通常，如果想重复一个步骤，可以使用如下的 for 循环:

```
>>> for i in range(3):
...     print('Hello')
...
Hello
Hello
Hello
```

但是，您可以看到变量“I”没有任何用法。在这种情况下，您可以使用下划线“_”来替换“I”。

```
>>> for _ in range(3):
...     print('Hello')
...
Hello
Hello
Hello
```

在这里，“_”是一个一次性变量。Python 不会在这个 for 循环中创建新变量。读者知道索引在 for 循环中是不必要的。

## 关键字，关键字值，关键字项

```
>>> teacher_subject = {'Ben':'English','Maria':'Math','Steve':'Science'}
>>> teacher_subject.keys()
dict_keys(['Ben', 'Maria', 'Steve'])
>>> teacher_subject.values()
dict_values(['English', 'Math', 'Science'])
>>> teacher_subject.items()
dict_items([('Ben', 'English'), ('Maria', 'Math'), ('Steve', 'Science')])
```

对于字典，可以使用键和值函数分别进行检索。对于 items 函数，可以同时检索键和值。当您需要切换键和值时，这很有用。

```
>>> subject_teacher = {y:x for x,y in teacher_subject.items()}
>>> subject_teacher
{'English': 'Ben', 'Math': 'Maria', 'Science': 'Steve'}
```

特别要注意的一点是，在切换过程中要小心重复的值。这将导致切换后元素丢失。

(额外:使用 zip，您可以从两个列表组成一个字典)

```
>>> subject = ['English','Math','Scienc']
>>> teacher = ['Ben','Maria','Steve']
>>> subject_teacher = {f:v for f,v in zip(subject,teacher)}
>>> subject_teacher
{'English': 'Ben', 'Math': 'Maria', 'Scienc': 'Steve'}
```

更多信息:[映射类型—字典](https://docs.python.org/3/library/stdtypes.html?#mapping-types-dict)

## 两组的比较

```
>>> a = {1,2,3}
>>> b = {1,2,3,4,5}# Is a a subset of b?
>>> a<=b
True# Is a a superset of b?
>>> a>=b
False# Union of a and b 
>>> a.union(b)
{1, 2, 3, 4, 5}# Intersection of a and b 
>>> a.intersection(b)
{1, 2, 3}# Difference # Return elements in a but not in b 
>>> a.difference(b)
set()# Return elements in b but not in a
>>> b.difference(a)
{4, 5}
```

进一步信息:[设置](https://docs.python.org/3/library/stdtypes.html#set)

## 收藏。计数器

当你想计算一个列表中所有元素的数量时，这很有用。您将收到一个类对象，显示列表中所有独特的元素及其各自的编号。

```
>>> import collections>>> arr_list = [1,1,1,1,2,2,2,3,3,5,5,5,7]
>>> c = collections.Counter(arr_list)
>>> c
Counter({1: 4, 2: 3, 5: 3, 3: 2, 7: 1})
>>> type(c)
<class 'collections.Counter'>
>>> c[1]
4
>>> c[6]
0# Convert back to a dictionary
>>> dict(c)
{1: 4, 2: 3, 3: 2, 5: 3, 7: 1}
```

更多信息:[收藏。计数器](https://docs.python.org/3/library/collections.html#collections.Counter)

我希望在阅读完这篇文章后，你能进一步了解 Python 的简单性。享受学习，享受编码。下次见。

我的其他文章

[Web Scrape Twitter by Python Selenium(第 1 部分)](/web-scrape-twitter-by-python-selenium-part-1-b3e2db29051d)

[Web Scrape Twitter by Python Selenium(第二部分)](/web-scrape-twitter-by-python-selenium-part-2-c22ae3e78e03)

[运载数据分析演示(纽约市 Airbnb 开放数据)](/a-demonstration-of-carrying-data-analysis-new-york-city-airbnb-open-data-a02d0ce8292d)

[如何用熊猫分析数值型数据？](/how-to-use-pandas-to-analyze-numeric-data-9739e0809b02)