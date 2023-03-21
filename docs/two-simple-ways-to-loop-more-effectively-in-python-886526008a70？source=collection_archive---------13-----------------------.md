# 在 Python 中更有效地循环的两种简单方法

> 原文：<https://towardsdatascience.com/two-simple-ways-to-loop-more-effectively-in-python-886526008a70?source=collection_archive---------13----------------------->

## 使用枚举和压缩编写更好的 Python 循环

![](img/b7e2db585b48a670728d2107364ad0c1.png)

许多循环。由 [David Streit](https://unsplash.com/@daviidstreit?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/loops?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

Python `range`函数非常强大，但它通常可以被其他内置函数所替代，使您的循环更容易编写和阅读。在本文中，我将向您展示何时可以用`enumerate`或`zip`来替换`range`。

# 使用 enumerate(object)而不是 range(len(object))

**问题 1** :你经常有像列表这样的对象需要迭代，同时还要跟踪每次迭代的索引。给定下面的列表，您将如何使用 for 循环来生成所需的输出？

```
my_list = ['apple', 'orange', 'cat', 'dog']# desired output
Item 0: apple
Item 1: orange
Item 2: cat
Item 3: dog
```

**方案一**:使用`for i in range(len(my_list))`

```
for i in range(len(my_list)):
    print(f"Item {i}: {my_list[i]}")Item 0: apple
Item 1: orange
Item 2: cat
Item 3: dog
```

**更好的解决方案**:使用`for i, value in enumerate(my_list)`

```
for i, value in enumerate(my_list):
    print(f"Item {i}: {value}")Item 0: apple
Item 1: orange
Item 2: cat
Item 3: dog
```

**解释** : `enumerate`在迭代器`my_list`上循环，并在迭代对象时将条目及其索引作为索引条目元组返回(参见下面的代码和输出以查看元组输出)。当我们将循环构造为`for i, value in enumerate(my_list)`时，我们解包索引项元组。

```
for i in enumerate(my_list):
    print(i)(0, 'apple')  # tuple, which can be unpacked (see code chunk above)
(1, 'orange')
(2, 'cat')
(3, 'dog')
```

**问题 2** :给定与上面相同的列表，编写一个循环来生成所需的输出(确保第一个索引从 101 开始，而不是从 0 开始)。

```
my_list = ['apple', 'orange', 'cat', 'dog']# desired output
Item 101: apple
Item 102: orange
Item 103: cat
Item 104: dog
```

**解决方案二**:使用`for i, value in enumerate(my_list, 101)`

函数`enumerate(iterable, start=0)`让您从任何想要的数字开始计数索引(默认为 0)。

```
for i, value in enumerate(my_list, 101):
    print(f"Item {i}: {value}")Item 101: apple
Item 102: orange
Item 103: cat
Item 104: dog
```

**外卖**

*   `enumerate`内置函数在迭代器上循环，并从迭代器中返回索引和条目作为索引条目元组
*   使用`enumerate(object)`而不是`range(len(object))`来获得更简洁易读的代码
*   提供第二个参数来指示开始计数的数字(默认值为 0)

# 使用 zip 并行迭代多个对象

**问题 3:** 你有多个想要并行迭代的列表或者对象。给定下面的三个列表，你将如何产生期望的输出？

```
my_list = ['apple', 'orange', 'cat', 'dog']
my_list_n = [11, 12, 25, 26]
my_list_idx = [1, 2, 3, 4]# desired output
1\. apple: 11
2\. orange: 12
3\. cat: 25
4\. dog: 26
```

**解决方案 3** :使用`range(len(my_list))`获取索引

```
for i in range(len(my_list)):
    print(f"{my_list_idx[i]}. {my_list[i]}: {my_list_n[i]}")1\. apple: 11
2\. orange: 12
3\. cat: 25
4\. dog: 26
```

**更好的解决方案**:使用`zip(my_list_idx, my_list, my_list_n)`

```
for i, obj, count in zip(my_list_idx, my_list, my_list_n):
    print(f"{i}. {obj}: {count}")1\. apple: 11
2\. orange: 12
3\. cat: 25
4\. dog: 26
```

**说明**:可以使用`zip`同时迭代多个对象。`zip`返回可以在循环中解包的元组。参见下面的例子来理解这个函数是如何工作的。

`zip`返回元组:

```
for i in zip(my_list_idx, my_list, my_list_n):
    print(i)(1, 'apple', 11)  # 3-item tuple
(2, 'orange', 12)
(3, 'cat', 25)
(4, 'dog', 26)
```

元组中的每个元素都可以手动提取:

```
for i in zip(my_list_idx, my_list, my_list_n):
    print(f"{i[0]}. {i[1]}: {i[2]}")  # i is a 3-item tuple1\. apple: 11
2\. orange: 12
3\. cat: 25
4\. dog: 26
```

**外卖**

*   `zip`内置函数可以同时迭代多个迭代器。
*   `zip`创建一个惰性生成器来生成元组

# 结论

使用内置的 Python 函数`enumerate`和`zip`可以帮助您编写更好的 Python 代码，可读性更强，更简洁。

如果您对提高数据科学技能感兴趣，以下文章可能会有所帮助:

[](/reshaping-numpy-arrays-in-python-a-step-by-step-pictorial-tutorial-aed5f471cf0b) [## 在 Python 中重塑 numpy 数组—一步一步的图形教程

### 本教程和备忘单提供了可视化效果，帮助您理解 numpy 如何重塑数组。

towardsdatascience.com](/reshaping-numpy-arrays-in-python-a-step-by-step-pictorial-tutorial-aed5f471cf0b) [](https://medium.com/better-programming/code-and-develop-more-productively-with-terminal-multiplexer-tmux-eeac8763d273) [## 使用终端多路复用器 tmux 提高编码和开发效率

### 简单的 tmux 命令来提高您的生产力

medium.com](https://medium.com/better-programming/code-and-develop-more-productively-with-terminal-multiplexer-tmux-eeac8763d273) [](/real-or-spurious-correlations-attractive-people-you-date-are-nastier-fa44a30a9452) [## 真实或虚假的关联:你约会的有魅力的人更令人讨厌

### 使用 Python 模拟数据、测试直觉并提高数据科学技能

towardsdatascience.com](/real-or-spurious-correlations-attractive-people-you-date-are-nastier-fa44a30a9452) [](/free-online-data-science-courses-during-covid-19-crisis-764720084a2) [## 新冠肺炎危机期间的免费在线数据科学课程

### 像 Udacity、Codecademy 和 Dataquest 这样的平台现在免费提供课程

towardsdatascience.com](/free-online-data-science-courses-during-covid-19-crisis-764720084a2) 

*更多帖子，* [*订阅我的邮件列表*](https://hauselin.ck.page/587b46fb05) *。*