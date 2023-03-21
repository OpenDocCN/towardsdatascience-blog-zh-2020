# 4 个非常有用的 Python 特性

> 原文：<https://towardsdatascience.com/4-super-useful-python-features-993ae484fbb8?source=collection_archive---------6----------------------->

## 四个不太知名但非常有用的 Python 功能

![](img/4c682dc41c7e86f786e44a033d8bd4ad.png)

克里斯·里德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

ython 是一种非常易读的语言，越来越成为许多程序员的首选。然而，这种易用性和大规模采用导致一些真正有用的功能消失在噪音中。

这里有四个超级有用的功能，在过去的几个月里，我越来越多地使用它们。

*(* [*意大利文*](https://medium.com/code-italia/4-super-funzionalit%C3%A0-di-python-fcdb377ea76b) *)*

# 列表方法:追加和扩展

**。append()** 将参数作为单个元素添加到给定列表的末尾。例如:

```
x = [1, 2, 3]
y = [4, 5]
x.append(y)
print(x)
```

`**[Out]:** [1, 2, 3, [4, 5]]`

**。extend()** 遍历参数并将每个元素添加到列表中。例如:

```
x = [1, 2, 3]
y = [4, 5]
x.extend(y)
print(x)
```

`**[Out]:** [1, 2, 3, 4, 5]`

使用加法运算符 **+** 和**扩展**也有区别。`x + y`给出了一个**新的**列表，而`x.extend(y)` **对原来的**列表进行了变异。

# 收益与回报

yield 语句向调用者发回一个值，但保持函数的状态。这意味着下一次调用这个函数时，它将从它结束的地方继续。

我们在生成器函数中使用这个功能——例如，让我们使用`yield`构建一个生成器，使用`return`构建一个等价的函数:

```
def gen_yield():
    yield "hel"
    yield "lo "
    yield "wor"
    yield "ld!"def func_return():
    return "hel"
    return "lo "
    return "wor"
    return "ld!"
```

如果我们打印这些生成器给出的值，首先使用`**yield**`:

```
for value in generator_yield():
    print(value, end="")
```

`**[Out]:** "hello world!"`

现在用`**return**`:

```
for value in generator_return():
    print(value, end="")
```

`**[Out]:** "hel"`

# 打印(开始，结束)

另一个，我刚刚在上面用过。`print`函数实际上由要打印的字符串对象和一个`end`组成！

我在快速代码中经常使用这种方法，通常是当我想迭代地打印出变量而不为每个变量换行时，例如:

```
generator = (x for x in [1, 2, 3])
for _ in generator:
    print(_, end=" ")
```

`**[Out]:** "1 2 3 "`

与普通打印相比:

```
for _ in generator:
    print(_)**[Out]:** "1"
       "2"
       "3"
```

# map 和 lambda

的。map()函数允许我们将一个 iterable 映射到一个函数。例如:

```
def add2(x):
    return x + 2list(**map(add2, [1, 2, 3])**)
```

`**[Out]:** [3, 4, 5]`

我最喜欢的是我们也可以使用 lambda 函数，就像这样:

```
results = [(0.1, 0.2), (0.3, 0.1), (0.2, 0.4)]list(**map(lambda x: x[0], results)**)
```

`**[Out]:** [0.1, 0.3, 0.2]`

在过去的几个月里，我一直在使用这四种方法——不久前，我要么没有意识到，要么根本就没有使用其中的任何一种。

如果你最近有什么不常用但有用的功能，请告诉我，我很感兴趣！欢迎通过[推特](https://twitter.com/jamescalam)或在下面的评论中联系我们。如果你想要更多这样的内容，我也会在 YouTube 上发布。

感谢阅读！