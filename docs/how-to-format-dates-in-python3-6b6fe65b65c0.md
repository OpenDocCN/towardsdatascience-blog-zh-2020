# 如何在 Python3 中格式化日期

> 原文：<https://towardsdatascience.com/how-to-format-dates-in-python3-6b6fe65b65c0?source=collection_archive---------50----------------------->

## strftime、format 和 f 字符串的比较

![](img/c713229635f92e8d7ef67eaf391daee7.png)

照片由 [**Surene Palvie**](https://www.pexels.com/@surene-palvie-1075224?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 发自 [**Pexels**](https://www.pexels.com/photo/four-green-yarns-on-chopping-board-2062061/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

您可能以前使用过 Python 的格式化语法。但是你如何格式化日期呢？使用 datetime 对象，可以用`%Y`、`%m`、`%d`分别以数字形式显示对象的年、月、日(即三月会被数字“03”代替)。

Python 有几种格式化字符串的语法。这样每条线路都会产生相同的输出。

输出:

```
2020 年 08 月 04 日
```

These lines produce the standard for Japanese dates. 年 means year, 月 means month, 日 means day.

让我们看看每个语法。

# 使用`strftime`

`strftime`是一个可以追溯到 python 早期的函数。它的名字相当神秘，但尽管如此，它最常用于我们所期望的日期格式化:一种工作就是格式化事物的方法。

它做得很好。但是语法可能有些笨拙。`strftime`不是最容易打字的单词。此外，如果我们需要在同一个字符串中格式化几个日期呢？

不过注意`.strptime`的存在，它是`.strftime`的逆变换。在英语中，这些名称可以翻译成“字符串解析时间”和“字符串格式化时间”。

# 使用`.format`方法

`.format`是`str`类的一个方法，用来格式化一个包含格式化语法的字符串。我们称之为*的替代领域*以荣誉`{}`为标志。它用于标记传递给该方法的参数将被插入的位置。奖项内的数字`{1}`用于选择参数。注意它从 0 开始，所以第一个传递的参数将被插入`{0}`。这里有一个例子来总结:

```
"The {0} parameter. The {2} parameter. The {1} parameter.".format("first", "second", "third")
```

请注意，您可以根据需要订购或复制替换字段`{}`的使用。您也可以插入命名参数

```
"Cut the {first}. Wash the {second}. Prepare the {third}.".format(first="onions", second="carrots", third="salad")
```

对于日期时间对象，我们有额外的格式。例如,`{0:%Y}`将在第一个位置参数中插入日期的年份，这应该是一个日期时间对象。因此，您可以看到为什么这两行会产生相同的输出。

```
"{:%Y 年%m 月%d 日}".format(datetime.today())
"{0:%Y}{1}{0:%m}{2}{0:%d}{3}".format(datetime.today(), "年", "月", "日")
```

第一个将所有内容放在替换字段`{}`中，而第二个将它分成几个插入的参数。看情况选择哪个会更有可读性。

# f 弦`f""`

由于最近在 python3.6 中引入，f-String 或 String interpolation 是一种强大而简洁的字符串格式化方法，这种方法鲜为人知。使用`f""`语法，您可以直接将变量名与格式语法放在一起。比如说。

```
age = 25
print(f"So you are {age} years-old?")
```

我们甚至可以在里面写函数调用！现在，您应该明白我们是如何获得与之前相同的输出的:

```
f"{datetime.today():%Y 年%m 月%d 日}"
```

# 结论

尽管`.strftime`仍然被广泛使用，但在我看来，它的可读性较差，因为其他语法更接近我们所期望的 pythonic 代码。这背后的主要原因是格式化操作是在`str`对象上有效执行的，而不是在 datetime 对象上。所以我会推荐你更喜欢`.format`和`f""`，它们更简洁。f 字符串也可以用于调试。例如,`print(f"{a=}")`将打印出`a=`,后跟`a`的实际值。[查看文档了解更多](https://docs.python.org/3/reference/lexical_analysis.html#f-strings)。

*原载于 2020 年 8 月 4 日*[*https://adamoudad . github . io*](https://adamoudad.github.io/posts/format-dates-python3/)*。*