# Python Heapq 模块简介

> 原文：<https://towardsdatascience.com/introduction-to-python-heapq-module-53534feda625?source=collection_archive---------19----------------------->

## 关于如何使用 Python 的 heapq 模块的简单介绍

一个**堆**是二叉树的一个特例，其中父节点与它们的子节点的值进行比较，并相应地进行排列。如果你看过我以前的一篇名为 [8 常见数据结构的文章，每个程序员都必须知道](/8-common-data-structures-every-programmer-must-know-171acf6a1a42)有两种类型的堆；最小堆和最大堆。

[](/8-common-data-structures-every-programmer-must-know-171acf6a1a42) [## 每个程序员都必须知道的 8 种常见数据结构

### 数据结构是一种在计算机中组织和存储数据的专门方法，以这种方式我们可以执行…

towardsdatascience.com](/8-common-data-structures-every-programmer-must-know-171acf6a1a42) 

python 的 **heapq** 模块实现了堆队列算法。它使用**最小堆**，其中父堆的键小于或等于其子堆的键。在本文中，我将介绍 python heapq 模块，并通过一些示例向您展示如何将 heapq 用于原始数据类型和具有复杂数据的对象。

![](img/f1c8285b1046a9aa5504bcd18400a1be.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2819215) 的[凯伦·阿诺德](https://pixabay.com/users/Kaz-19203/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2819215)

# Heapq 函数

假设你知道堆数据结构是如何工作的，我们来看看 Python 的 heapq 模型提供了哪些功能。

*   **heappush(heap，item)** —将值 *item* 推入*堆*
*   **heappop(heap)** —弹出并返回*堆中的最小值*
*   **heappushpop(heap，item)** —将值 *item* 推入*堆*并返回*堆*中的最小值
*   **堆(x)** —将列表 *x* 转换成堆
*   **heap preplace(heap，item)** —弹出并返回*堆*中的最小值，然后将值 *item* 推入*堆*

# 使用原始数据类型的 Heapq 示例演练

让我们看一些使用不同 heapq 函数的例子。首先你要导入 heapq 模块。

```
import heapq
```

考虑下面给出的示例列表`a`。

```
>>> a = [52, 94, 13, 77, 41]
```

如果我们把这个列表放大，结果将如下。请注意，堆积是就地完成的。

```
>>> heapq.heapify(a)
>>> print(a)
[13, 41, 52, 77, 94]
```

请注意，第 0 个索引包含所有值中的最小值，即 13。

让我们把值 10 放到我们的堆里。

```
>>> heapq.heappush(a,10)
>>> print(a)
[10, 41, 13, 77, 94, 52]
```

请注意，添加了 10，因为 10 是可用值中的最小值，所以它在第 0 个索引中。

现在让我们从我们的堆里跳出来。

```
>>> print(heapq.heappop(a))
10
>>> print(a)
[13, 41, 52, 77, 94]
```

当我们从堆中弹出时，它将从堆中移除最小值并返回它。现在值 10 已经不在我们的堆中了。

让我们看看 heappushpop()函数是如何工作的。让我们把值 28。

```
>>> print(heapq.heappushpop(a,28))
13
>>> print(a)
[28, 41, 52, 77, 94]
```

您可以看到 28 被推送到堆中，最小值 13 从堆中弹出。

现在让我们试试 heapreplace()函数。让我们将值 3 放入堆中。

```
>>> print(heapq.heapreplace(a,3))
28
>>> print(a)
[3, 41, 52, 77, 94]
```

您可以看到，最初最小的值 28 被弹出，然后我们的新值 3 被推入。新堆值 3 中的第 0 个索引。请注意 heappushpop()和 heapreplace()函数中 push 和 pop 操作顺序的不同。

# 如何在 Heapq 中使用对象？

在前面的例子中，我已经解释了如何将 heapq 函数用于整数等原始数据类型。类似地，我们可以使用带有 heapq 函数的对象来对复杂数据(如元组甚至字符串)进行排序。为此，根据我们的场景，我们需要一个包装类。考虑这样一种情况，您希望存储字符串，并根据字符串的长度对它们进行排序，从最短到最长。我们的包装类将如下所示。

```
class DataWrap:

    def __init__(self, data):
        self.data = data  

    def __lt__(self, other):
        return len(self.data) < len(other.data)
```

`__lt__`函数(它是比较运算符的运算符重载函数；>、≥、==、<和≤)将包含比较字符串长度的逻辑。现在让我们试着用一些字符串做一个堆。

```
# Create list of strings
my_strings = ["write", "go", "run", "come"]# Initialising
sorted_strings = []# Wrap strings and push to heap
for s in my_strings:
    heapObj = DataWrap(s)
    heapq.heappush(sorted_strings, heapObj)# Print the heap
for myObj in sorted_strings:
    print(myObj.data, end="\t")
```

打印堆中项目的输出如下。

```
go	come	run	write
```

注意最短的单词`go`在堆的前面。现在，您可以通过包装字符串来尝试其他 heapq 函数。

# 使用 Heapq 实现最大堆

我们可以使用 heapq 轻松实现最大堆。你所要做的就是改变`__lt__`函数中的比较运算符，将最大值排在前面。让我们用字符串和它们的长度来试试前面的例子。

```
class DataWrap:

    def __init__(self, data):
        self.data = data  

    def __lt__(self, other):
        return len(self.data) > len(other.data)# Create list of strings
my_strings = ["write", "go", "run", "come"]# Initialising
sorted_strings = []# Wrap strings and push to heap
for s in my_strings:
    heapObj = DataWrap(s)
    heapq.heappush(sorted_strings, heapObj)# Print the heap
for myObj in sorted_strings:
    print(myObj.data, end="\t")
```

注意长度比较在`DataWrap`类的`__lt__`函数中是如何变化的。打印这个堆中的项目的输出如下。

```
write	come	run	go
```

注意，现在最长的单词`write`在堆的前面。

# 最后的想法

我希望在使用 heapq 模块实现的过程中，这篇文章对您有所帮助。请随意使用提供的代码。

感谢您的阅读！

干杯！

# 参考

[1] heapq —堆队列算法— Python 3.8.3rc1 文档([https://docs.python.org/3/library/heapq.html](https://docs.python.org/3/library/heapq.html))