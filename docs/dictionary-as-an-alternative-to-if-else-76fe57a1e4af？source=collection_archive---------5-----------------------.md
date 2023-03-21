# 字典作为 If-Else 的替代

> 原文：<https://towardsdatascience.com/dictionary-as-an-alternative-to-if-else-76fe57a1e4af?source=collection_archive---------5----------------------->

## 使用字典创建一个更清晰的 If-Else 函数代码

# 动机

您可能经常使用 Python 的字典。但是，您是否已经释放了字典的全部容量来创建更高效的代码呢？如果你不知道如何创建一个有序的字典，将多个字典组合成一个映射，创建一个只读字典，你可以在这里找到更多的。

本文将关注如何使用 Python 的字典作为 if-else 语句的替代。

![](img/3ffd8d34969d9b513761e83027f6f2ba.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4255486) 的 [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4255486)

# 削减 If-Else 语句，增加默认值

假设我们有杂货店商品的价格表:

```
price_list = {
'fish': 8,
'beef': 7,
'broccoli': 3,
}
```

我们想打印商品的价格，但是预期不是每个商品都在`price_list.`中，所以我们决定创建一个函数:

```
def find_price(item):
    if item in price_list:
        return 'The price for {} is {}'.format(item, price_list[item])
    else:
        return 'The price for {} is not available'.format(item)>>> find_price('fish')
'The price for fish is 8'>>> find_price('cauliflower')
'The price for cauliflower is not available'
```

聪明。if-else 语句做了我们希望它做的事情:当项目不可用时返回另一个值。但是我们查询字典两次，并使用两条语句返回几乎相同的内容。我们能做得更好吗？如果条目不在列表中，有没有办法返回一个默认值？幸运的是，Python 的字典方法有一种方法可以做到这一点，叫做`get()`

```
def find_price(item):
    return 'The price for {} is {}'.format(item, price_list.get(
        item, 'not available'))
```

`.get()`查找一个键，用不存在的键返回默认值。代码看起来确实更短了，但是它的表现是否像我们想要的那样呢？

```
>>> find_price('fish')
'The price for fish is 8'>>> find_price('cauliflower')
'The price for cauliflower is not available'
```

整洁！

# 但是我可以用 Dict 来实现不涉及字典的功能吗？

好问题。让我们来处理一个完全不涉及字典的例子。

假设您想创建一个函数，返回两个数之间的运算值。这就是你得出的结论:

```
def operations(operator, x, y):
    if operator == 'add':
        return x + y
    elif operator == 'sub':
        return x - y
    elif operator == 'mul':
        return x * y
    elif operator == 'div':
        return x / y>>> operations('mul', 2, 8)
16
```

你可以利用字典和`get()`方法得到一个更优雅的代码:

```
def operations(operator, x, y):
    return {
        'add': lambda: x+y,
        'sub': lambda: x-y,
        'mul': lambda: x*y,
        'div': lambda: x/y,
    }.get(operator, lambda: 'Not a valid operation')()
```

操作符的名字变成了键，`lambda`有效地将函数压缩成字典的值。`get()`找不到键时返回默认值。让我们检查一下我们的功能:

```
>>> operations('mul', 2, 8)
16
>>> operations('unknown', 2, 8)
'Not a valid operation'
```

太好了！它像我们想要的那样工作。

# 那么我应该把 If-Else 语句切换到字典吗？

替代方案使代码看起来更干净。但是如果我们想使用字典，我们应该考虑这个开关的缺点。每次我们调用`operations()`，它都会创建一个临时字典和 lambdas，让操作码循环。这将降低代码的性能。

因此，如果我们想使用字典，我们应该考虑在函数调用之前创建一次字典。这个行为防止代码在我们每次查找时重新创建字典。这种技术当然不会在所有情况下都适用，但是在您的工具箱中有另一种技术可供选择将是有益的。

在 [this Github repo](https://github.com/khuyentran1401/Data-science/blob/master/python/dictionary_ifelse.ipynb) 中，您可以随意使用本文的代码。

我喜欢写一些基本的数据科学概念，并尝试不同的算法和数据科学工具。你可以通过 [LinkedIn](https://www.linkedin.com/in/khuyen-tran-1401/) 和 [Twitter](https://twitter.com/KhuyenTran16) 与我联系。

如果你想查看我写的所有文章的代码，请点击这里。在 Medium 上关注我，了解我的最新数据科学文章，例如:

[](/timing-the-performance-to-choose-the-right-python-object-for-your-data-science-project-670db6f11b8e) [## 高效 Python 代码的计时

### 如何比较列表、集合和其他方法的性能

towardsdatascience.com](/timing-the-performance-to-choose-the-right-python-object-for-your-data-science-project-670db6f11b8e) [](/maximize-your-productivity-with-python-6110004b45f7) [## 使用 Python 最大化您的生产力

### 你创建了一个待办事项清单来提高效率，但最终却把时间浪费在了不重要的任务上。如果你能创造…

towardsdatascience.com](/maximize-your-productivity-with-python-6110004b45f7) [](/how-to-use-lambda-for-efficient-python-code-ff950dc8d259) [## 如何使用 Lambda 获得高效的 Python 代码

### lambda 对元组进行排序并为您创造性的数据科学想法创建复杂函数的技巧

towardsdatascience.com](/how-to-use-lambda-for-efficient-python-code-ff950dc8d259) [](/cython-a-speed-up-tool-for-your-python-function-9bab64364bfd) [## cy thon——Python 函数的加速工具

### 当调整你的算法得到小的改进时，你可能想用 Cython 获得额外的速度，一个…

towardsdatascience.com](/cython-a-speed-up-tool-for-your-python-function-9bab64364bfd) [](https://medium.com/better-programming/boost-your-efficiency-with-specialized-dictionary-implementations-7799ec97d14f) [## 通过专门的字典实现提高您的效率

### 创建包含有序和只读条目的字典，为不存在的键返回默认值，等等

medium.com](https://medium.com/better-programming/boost-your-efficiency-with-specialized-dictionary-implementations-7799ec97d14f) 

# 参考

巴德丹。“Python 的把戏。”2017