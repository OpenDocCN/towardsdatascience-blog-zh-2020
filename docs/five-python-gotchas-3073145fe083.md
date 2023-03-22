# 五个 Python 陷阱！

> 原文：<https://towardsdatascience.com/five-python-gotchas-3073145fe083?source=collection_archive---------54----------------------->

## 当你最不期待的时候会期待什么！

![](img/e7874e9c5922bbbe1acf1f9299b77674.png)

照片由[颜](https://www.pexels.com/@yankrukov?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[像素](https://www.pexels.com/photo/photo-of-woman-showing-frustrations-on-her-face-4458420/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

许多贴子都列出了 Python 和/或其最流行的包的“陷阱”。这篇博客是该系列的又一篇文章，但有一点不同:我自己实际上也确实犯过这些错误(其中一些错误的发生频率令人尴尬)。然而，只要知道每个经典 Python 对象的定义和类型，就可以帮助您在工作中避免大多数(如果不是全部)这样的错误！

事不宜迟，我们开始吧。

# 1.Truthy 还是 Falsy: NumPy.nan 和 Pandas.nan

您可能知道，要检查一个对象的值是真还是假，您可以像下面这样做:

```
lst = [1, 2, 3]
a = None# rather than this ...
if len(lst) > 0 or a is not None: print('success')# you can simply do this ...
if lst or not a: print('success')
```

这是因为空列表(以及所有其他空序列/集合)、`False`、`None`、0(任何数值类型)的计算结果都为 False。这组对象和值因此被称为“ [falsy](https://docs.python.org/3/library/stdtypes.html#truth-value-testing) ”。

考虑下面的例子:您有一个项目及其成本的字典，您用它来构建一些分析的数据框架。

```
import pandas as pdd1 = {'item': ['foo', 'bar', 'baz'], 'cost': [100, None, 20]}
df = pd.DataFrame(d1)
# lots of analysis here ...# if an item has a cost, print the item and its cost
for i, r in df.iterrows():
    if r['cost']:
        print(f"item = {r['item']} and cost = {r['cost']}")
```

您期望:

```
item = foo, cost = 100.0
item = baz, cost = 20.0
```

但是你会得到:

```
item = foo, cost = 100.0
item = bar, cost = nan
item = baz, cost = 20.0
```

原因是[熊猫](https://pandas.pydata.org/pandas-docs/dev/user_guide/missing_data.html#values-considered-missing)认为`None`失踪或不在，因此用`nan`代表。由于`nan`不是虚假的，它流过。

乍一看，这似乎违反直觉，但是`nan` s 是丢失的值，如果丢失了什么，您并不真正*知道它是什么。例如，数字列中给定的`nan`可以代表 100(真)或 0(假)吗？如果是字符串，是' hello' (truthy)还是空字符串(你猜对了，是 falsy)？熊猫不确定，所以它不会假设是假的。*

小心避免考虑`nan` falsy。

# 2.是 NaN == NaN？

考虑下面的例子:

```
>>> s1 = {True, 1, 1.0}
>>> s1
{True}
```

这是意料之中的，因为我们知道`1==1.0==True`的计算结果为真。

现在来看看这个案例:

```
>>> s2 = {float('nan'), float('nan'), float('nan')}
>>> s2
{nan, nan, nan}
```

考虑前面例子中的逻辑:因为`nan`是一个缺失值，所以不可能知道三个缺失值是否相同。因此，`nan == nan`的计算结果始终为 False。(如果您想了解更多相关信息，请务必查看 [PEP 754 — IEEE 754 浮点特殊值](https://www.python.org/dev/peps/pep-0754/)。)

# 3.所有和任何

我认为`any`和`all`的工作方式如下:如果你有一个可迭代的，如果可迭代的任何元素为真，那么`any`将返回真。如果 iterable 的所有元素都为真，那么`all`将返回真。

所以，让我们来看看。第一，`any`:

```
>>> any([0, None, False])  # expected False
False
>>> any([0, None, False, 'False'])  # expected True
True
>>> any([float('nan'), 0])  # expected True
True
>>> any([])  # expected False
False
```

到目前为止，一切顺利！让我们检查一下`all`:

```
>>> all((1, 0, 0))  # expected False
False
>>> all([float('nan'), 1])  # expected True
True
>>> all([])  # expected False
True
```

我认为所有的元素都应该是真的，空的列表显然是假的。那么为什么`all([])`评估为真呢？

答案在 [Python 文档](https://docs.python.org/3.8/library/functions.html#all)(我的强调是附加的):

> `**all**` *(* 可迭代 *)*
> 
> 如果 *iterable* 的所有元素都为真(**或者 iterable 为空**)，则返回`True`。

![](img/618c8a306007c0c3fd98f09ff5736a99.png)

但这是为什么呢？嗯，长话短说，是因为[空洞的真相](https://en.wikipedia.org/wiki/Vacuous_truth)。“如果我有七英尺高，我也是万亿超级英雄”对我来说永远是真的。我还差大约 6 英寸(忽略我同事的任何相反的证词)，所以不管逗号后面是什么，该语句都是真的。这是一个永远不会错的说法**因为我没有七英尺高，因此不可能评价我的万亿富翁超级英雄身份。**

**除了更仔细地阅读文档之外，记住`all()`这种行为的最好方法是不要认为它是“如果 iterable 的所有元素都为真”，而是“如果 iterable 中没有 false 元素”当 iterable 为空时，其中不能有 false 元素，这意味着`all([])`的计算结果为 True。**

# **4.可变默认参数**

**我认为这是目前为止最常见的 Python“gotcha”。我们开始吧。**

**考虑以下函数:**

```
def foo(a, a_list=[]):
    a_list.append(a)
    return a_list
```

**让我们使用这个函数`foo`来创建两个单独的列表:**

```
>>> my_list = foo(10)
>>> print(my_list)  # expected [10]
[10]>>> another_list = foo(20)
>>> print(another_list)  # expected [20][10, 20]
```

**您可能希望每个函数调用都创建一个新的列表，但是在第一次调用`foo`时创建的列表会在每个后续调用中使用。**

**发生这种情况是因为，在 Python 中，只有当函数被定义时，默认参数才会被计算**，而不是每次函数被调用时(你可以在 [Python 的文档](https://docs.python.org/3/tutorial/controlflow.html#default-argument-values)中了解更多)。如果我们使用一个可变的默认参数(比如`foo`中的`a_list=[]`)并在函数中改变它，那么每当调用函数时，这个对象就会发生变化。****

**避免这种混乱的最好方法是在函数中使用不可变的默认参数。下面是`foo`的相应更新版本:**

```
def foo_v2(a, a_list=None):
    if a_list is None:
        a_list = []
    a_list.append(a)
    return a_list
```

**这个`foo`的定义假设每次调用都有一个新的列表是可取的。但是，在某些情况下，您可能希望有意传递一个可变对象。一种这样的情况是在编写递归函数时，这需要从一个调用到下一个调用保存对象的状态。[深度优先搜索(DFS)](https://en.wikipedia.org/wiki/Depth-first_search) 算法的以下实现是这种情况的一个例子:**

```
def dfs(graph, node):
    """dfs from a given node"""
    return _dfs(graph, node, [])def _dfs(graph, node, path)
    """interior utility function"""
    path += [node]
    for neighbor in graph[node]:
        if neighbor not in path:
            path = _dfs(graph, neighbor, path)
    return path>>> graph = {'A': ['B', 'C'], 'B': ['D', 'E'], 'C': ['G'], 
         'D': ['F'], 'E': [], 'F': [], 'G': []}
>>> print(dfs(graph, 'A'))  # give the path starting from node 'A'
['A', 'B', 'D', 'F', 'E', 'C', 'G']
```

# **5.迭代时修改列表**

**我已经在我的[小食谱](/bite-sized-python-recipes-52cde45f1489)帖子中讨论了这最后一个“陷阱”,但是因为我亲眼看到一些人落入这个陷阱，所以我也在这里提一下。**

**假设您想从列表中删除所有小于 5 的数字。**

***错误实现:*迭代时移除元素！**

```
nums = [1, 2, 3, 5, 6, 7, 0, 1]
for ind, n in enumerate(nums):
    if n < 5:
        del(nums[ind])# expected: nums = [5, 6, 7]
>>> nums
[2, 5, 6, 7, 1]
```

***正确实施:***

**使用列表理解创建一个新列表，只包含您想要的元素:**

```
>>> id(nums)  # before modification 
2090656472968
>>> nums = [n for n in nums if n >= 5]
>>> nums
[5, 6, 7]
>>> id(nums)  # after modification
2090656444296
```

**上面可以看到`[id](https://docs.python.org/3/library/functions.html#id)(nums)`是前后勾选的，说明其实两个列表是不一样的。因此，如果在其他地方使用该列表，并且改变现有列表很重要，而不是创建一个同名的新列表，则将它分配给切片:**

```
>>> nums = [1, 2, 3, 5, 6, 7, 0, 1]
>>> id(nums)  # before modification 
2090656472008
>>> nums[:] = [n for n in nums if n >= 5]
>>> id(nums)  # after modification
2090656472008
```

**我希望这篇博客对你有用。我可以在[*Twitter*](https://twitter.com/EhsanKhoda)*和*[*LinkedIn*](https://www.linkedin.com/in/ehsankhodabandeh)*上联系到我，我欢迎任何反馈。***