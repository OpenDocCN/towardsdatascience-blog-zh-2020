# 跟踪数据的 Python 技巧

> 原文：<https://towardsdatascience.com/python-tricks-for-keeping-track-of-your-data-aef3dc817a4e?source=collection_archive---------11----------------------->

## 如何用列表、字典计数器和命名元组来跟踪信息

# 动机

在您的数据科学项目中，有时您希望跟踪数据中的信息，或者灵活地快速轻松地更新新输入的数据。了解如何使用 Python 的一些数据对象将使您在数据科学职业生涯中处理大量数据时保持条理并避免错误。

在本文中，您将学习如何:

*   循环时跟踪索引
*   更新新词典项目
*   用可重复使用的对象记录新信息

我将从这些问题开始，然后详细介绍如何用 Python 工具解决它们。我希望这种方法能帮助你想象这些工具在你的代码中的应用。

![](img/7827bbdc9e2d23237a1a239dbb3683a3.png)

照片由[埃德加·恰帕罗](https://unsplash.com/@echaparro?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 循环时保持跟踪

假设你有一个朋友的名单。您希望在跟踪计数的同时遍历列表。你怎么能这样做？这可以通过`enumerate`轻松完成

```
>>> friends = ['Ben', 'Kate', 'Thinh']
>>> for i, item in enumerate(friends):
>>>     print(f'{i}: {item}')
0: Ben
1: Kate
2: Thinh
```

或者简单地用字典理解

```
>>> {i: friends[i] for i in range(len(friends))}
{0: 'Ben', 1: 'Kate', 2: 'Thinh'}
```

# 更新新词典项目:

你在用字典记录第一句话中的单词及其数量。

```
sent1 = {'love': 1, 'hate': 3}
```

但是当你读到第二个句子时，你想用新的句子更新你的旧字典。

```
sent2 = {'love': 2, 'flower': 1}
```

所以你新更新的单词包会变成这样:

```
{'love': 3, 'hate': 3, 'flower': 1}
```

这怎么可能呢？如果有一些工具可以让你轻松做到这一点，那不是很好吗？如果这就是你想要的，`collections.Counter`会支持你。`collections.Counter`该类允许集合中的元素**出现不止一次**

```
from collections import Counter
bag_words = Counter()sent1 = {'love': 1, 'hate': 3}bag_words.update(sent1)sent2 = {'love': 2, 'flower': 1}bag_words.update(sent2)bag_words
```

结果:

```
Counter({'love': 3, 'hate': 3, 'flower': 1})
```

不错！现在，随着你从其他句子中收集更多的信息，你可以很容易地更新你的单词包。要找出句子中有多少个独特的单词，你可以使用`len`

```
>>> len(bag_words)
3
```

或者想知道句子中的单词总数，你可以用`sum`

```
>>> sum(bag_words.values())
7
```

# 用命名元组定义可重用对象

你想记录朋友的信息列表，为他们的生日做准备。因为您现在还没有可用的信息，所以您想先创建一个占位符，以便以后输入信息。如果你想记录凯特的生日，喜欢的食物，颜色，以及她是否内向，你可以这样做:

```
>>> Kate = Friend('Feb', 'cake', 'pink', True)
```

当你不记得她的生日时，你可以打电话

```
>>> Kate.birthday
'Feb'
```

Python 中的 class 对象将使您能够实例化 Kate，但是您发现创建一个`Friend`类来保存简单信息需要时间。如果是这样的话，`namedtuple`就是你的 to-go 函数。`nametuples`允许您**为您的记录定义一个可重用的对象**，以确保使用正确的文件名

```
from collections import namedtupleFriend = namedtuple('Friend' , 'birthday food color introvert')Kate = Friend('Feb', 'cake', 'pink', True)Ben = Friend('Jan', 'fish', 'red', False)
```

显示关于 Kate 的信息:

```
>>> Kate
Friend(birthday='Feb', food='cake', color='pink', introvert=True)
```

如果你想知道本是内向还是外向，你可以打电话

```
>>> Ben.introvert
False
```

使用`nametuples`，您可以轻松地重用同一个对象来实例化新信息。

# 结论

厉害！你已经学会了如何使用`enumerate`、集合理解、`Counter`和`namedtuple`来跟踪信息。我希望本教程能够为您提供额外的有用知识，将它们添加到您的数据科学工具包中。

在[这个 Github repo](https://github.com/khuyentran1401/Data-science/blob/master/python/keep_track.ipynb) 中，您可以随意派生和使用本文的代码。

我喜欢写一些基本的数据科学概念，并尝试不同的算法和数据科学工具。你可以在 [LinkedIn](https://www.linkedin.com/in/khuyen-tran-1401/) 和 [Twitter](https://twitter.com/KhuyenTran16) 上联系我。

如果你想查看我写的所有文章的代码，请点击这里。在 Medium 上关注我，了解我的最新数据科学文章，例如:

[](/timing-the-performance-to-choose-the-right-python-object-for-your-data-science-project-670db6f11b8e) [## 高效 Python 代码的计时

### 如何比较列表、集合和其他方法的性能

towardsdatascience.com](/timing-the-performance-to-choose-the-right-python-object-for-your-data-science-project-670db6f11b8e) [](/how-to-use-lambda-for-efficient-python-code-ff950dc8d259) [## 如何使用 Lambda 获得高效的 Python 代码

### lambda 对元组进行排序并为您创造性的数据科学想法创建复杂函数的技巧

towardsdatascience.com](/how-to-use-lambda-for-efficient-python-code-ff950dc8d259) [](/maximize-your-productivity-with-python-6110004b45f7) [## 使用 Python 最大化您的生产力

### 你创建了一个待办事项清单来提高效率，但最终却把时间浪费在了不重要的任务上。如果你能创造…

towardsdatascience.com](/maximize-your-productivity-with-python-6110004b45f7) [](/cython-a-speed-up-tool-for-your-python-function-9bab64364bfd) [## cy thon——Python 函数的加速工具

### 当调整你的算法得到小的改进时，你可能想用 Cython 获得额外的速度，一个…

towardsdatascience.com](/cython-a-speed-up-tool-for-your-python-function-9bab64364bfd) [](/how-to-build-a-matrix-module-from-scratch-a4f35ec28b56) [## 如何从头开始构建矩阵模块

### 如果您一直在为矩阵运算导入 Numpy，但不知道该模块是如何构建的，本文将展示…

towardsdatascience.com](/how-to-build-a-matrix-module-from-scratch-a4f35ec28b56)