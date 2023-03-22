# 鲜为人知的 Python 特性

> 原文：<https://towardsdatascience.com/lesser-known-python-features-f87af511887?source=collection_archive---------4----------------------->

## Python 中经常不为人知和被低估的功能示例

![](img/939daab2ae43e88e7133ed477bbd5ec8.png)

由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

时不时地，当我了解到 Python 中的一个新特性，或者我注意到其他一些人不知道这个特性时，我会记下来。

在过去的几周里，有一些有趣的特性是我自己最近了解到的，或者意识到了其他一些特性——例如，在堆栈溢出方面，我以前并不知道。

下面是对其中一些特性的快速浏览，以及每个特性的概要。

# divmod

非常有用的功能。`divmod()`函数对两个数执行模数除法`%`，然后返回商和余数。例如:

```
divmod(5, 2)
```

`**[Out]:** (2, 1)`

这只是简单地找出我们可以将`2`放入`5`的次数，而不用拆分这个数，这给了我们`2`，即商。在此之后，我们还有`1`剩余，这是我们的余数。

这对于返回进程运行所用的时间(以小时、分钟和秒为单位)特别有用。像这样:

```
start = datetime.datetime.now() ...  # process code goes hereend = datetime.datetime.now()# we get the total runtime in seconds
runtime = (end - start).seconds  # we will assume 30000# how many hours are in these secs, what are the remaining secs?
hours, remainder = divmod(runtime, 3600)# now how many minutes and seconds are in our remainder?
mins, secs = divmod(remainder, 60)print("{:02d}:{:02d}:{:02d}".format(hours, mins, secs))
```

`**[Out]:** "08:00:08"`

# *args，**kwargs

有时，您可能会注意到函数定义包含这两个参数，比如`def func(x, y, *args, **kwargs)`。

它们实际上都非常简单。两者都允许我们向一个函数传递多个值，然后将这些值打包到一个生成器中。

如果我们将一个列表/生成器传递给一个标准参数，结果是相似的，就像这样:

```
def func(values):
    for x in values:
        print(x, end=" ")func(**[1, 2, 3]**)
```

`**[Out]:** '1 2 3 '`

现在，让我们使用`*args`——这将允许我们将每个值作为一个新的参数传递，而不是将它们都包含在一个列表中。

```
def func(*values):
    for x in values:
        print(x, end=" ")func(**1, 2, 3**)
```

`**[Out]:** 1 2 3`

注意，我们不需要输入`*args`，相反，我们输入了`*values`。我们使用的变量名无关紧要。由于只有一个星号`*`，它被定义为`*args`。

`*args`简单地从我们传递给函数的参数中创建一个元组。

`**kwargs`另一方面，创造了字典。因此得名，**k**ey-**w**ord**arg**ument**s**。我们像这样使用它:

```
def func(**values):
    for x in values:
        print(f"{x}: {values[x]}")func(x=1, y=2, z=3)**[Out]:** x: 1
       y: 2
       z: 3
```

同样，我们可以随意调用变量，在本例中我们使用了`**values`。使用双星号`**`将其定义为`**kwargs`。

# （听力或阅读）理解测试

这绝对是 Python 最有用的特性之一。理解表达是不可错过的。最常见的是**列表理解**，我相信你们中的绝大多数人都见过这些:

```
vals = [1, 2, 3, 4, 5][i**2 for i in vals]
```

`**[Out]:** [1, 4, 9, 16, 25]`

但是我们不限于这些方括号。我们可以用几乎完全相同的语法定义一个**生成器表达式**:

```
(i**2 for i in vals)
```

`**[Out]:** <generator object <genexpr> at 0x7f0281730fc0>`

当然，生成器中的每个元素只有在被调用时才会被输出，我们可以用`list()`来实现:

```
list((i**2 for i in vals))
```

`**[Out]:** [1, 4, 9, 16, 25]`

语法上的另一个小变化是，我们甚至可以使用**字典理解**来构建字典:

```
{i: i**2 for i in vals}**[Out]:** {1: 1,
        2: 4,
        3: 9,
        4: 16,
        5: 25}
```

# 文件夹

一个特别有意思的字符串方法。其功能与`lower`相似。然而，`casefold`试图更积极地标准化更广泛的字符。

在大多数情况下，`lower`和`casefold`的行为是相同的，但偶尔也会有不同:

```
"ς".casefold()  # both ς and σ are the Greek letter sigma**[Out]:** "σ"
```

相比之下，使用`lower`:

```
"ς".lower()  # however, lower recognizes them as different**[Out]:** "ς"
```

`**[Out]:** False`

这里，两个 sigmas 都已经是小写了。这取决于用例，可以如预期的那样运行。

然而，如果我们打算比较两个等价的希腊单词，一个使用σ，另一个使用ς.尽管相同，但只有`casefold`能让我们准确地比较它们:

```
"ἑρμῆσ" == "ἑρμῆς"**[Out]:** False"ἑρμῆσ".lower() == "ἑρμῆς".lower()**[Out]:** False"ἑρμῆσ".casefold() == "ἑρμῆς".casefold()**[Out]:** True
```

希望你从这篇文章中有所收获。特别是，`divmod`和`casefold`都是非常有趣的功能，我个人直到最近才使用过。

如果你有任何独特的、有趣的或者相对不为人知的 Python 特性想要分享，请告诉我，我很乐意看到它们！欢迎通过 [Twitter](https://twitter.com/jamescalam) 或在下面的评论中联系我们。

感谢阅读！

如果您喜欢这篇文章，您可能会对我之前关于另外四个鲜为人知的 Python 特性的文章感兴趣:

[](/4-super-useful-python-features-993ae484fbb8) [## 4 个非常有用的 Python 特性

### 四个不太知名但非常有用的 Python 功能

towardsdatascience.com](/4-super-useful-python-features-993ae484fbb8)