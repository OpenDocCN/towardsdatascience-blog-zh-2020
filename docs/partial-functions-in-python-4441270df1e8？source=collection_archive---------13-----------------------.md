# Python 中的部分函数

> 原文：<https://towardsdatascience.com/partial-functions-in-python-4441270df1e8?source=collection_archive---------13----------------------->

## 降低代码的复杂性。

![](img/bbf2c2ef40250c8e6751cc1a3267ad8e.png)

[来源](https://pixabay.com/images/id-5507459/)

在这篇文章中，我将谈论部分函数，以及我们如何使用它们来使我们的代码更整洁。

# 部分函数能为我们做什么？

分部函数允许我们调用第二个函数，在某些参数中使用固定值。例如，我们可能有一个计算指数的函数。然后，我们可能需要创建一个新的函数，为底数或指数分配一个固定值。我们可以通过使用 [functool 的部分函数](https://docs.python.org/3/library/functools.html#functools.partial)轻松做到这一点，而不必复制原始函数的主体。让我们通过上面例子的实现来更好的理解。

# 实现部分取幂函数

首先，我们需要分部函数所基于的函数。如前所述，我们将实现一个求幂函数:

```
def power(base, exponent):
  return base ** exponent
```

现在，我们可能想要实现一个函数来计算一个数的平方值。如果 Python 中没有可用的部分函数，我们就必须复制(和修改)我们的代码:

```
def squared(base):
  return base ** 2
```

在这种情况下，这没什么大不了的，因为我们只需要复制一行代码。然而，在大多数情况下，您将不得不处理更大的函数。让我们看看如何通过部分函数重用“power()”:

```
from functools import partialdef power(base, exponent):
  return base ** exponentsquared = partial(power, exponent=2)
```

最后，我们可以简单地调用“squared()”如下:

```
squared(3)
squared(base=7)
```

就是这样！我希望分部函数成为您的有用工具，帮助您降低代码的复杂性！