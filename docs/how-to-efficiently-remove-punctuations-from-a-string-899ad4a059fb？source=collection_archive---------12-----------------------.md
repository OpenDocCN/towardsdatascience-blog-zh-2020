# 如何有效地从字符串中删除标点符号

> 原文：<https://towardsdatascience.com/how-to-efficiently-remove-punctuations-from-a-string-899ad4a059fb?source=collection_archive---------12----------------------->

## Python 中清理字符串的 8 种不同方法

![](img/8bd2268d433099036dd83af32ff6f3f8.png)

由 [Murat Onder](https://unsplash.com/@muratodr?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

最近，我发现自己花了很多时间试图弄懂杂乱的文本数据，并决定回顾一些相关的预处理。有许多不同的方法来实现简单的清洁步骤。今天，我将回顾几种不同的方法来删除字符串中的标点符号，并比较它们的性能。

# 使用翻译

string **translate** 方法是一次性将多个字符更改为不同值的便捷方法。 **Translate** 需要一个表作为字典来映射字符串。maketrans 为您完成这项工作。

**maketrans** 语法的工作方式类似于`str.maketrans('abcd', '0123', 'xyz')`。它将创建一个表格，告诉 **translate** 将所有 *a* 更改为 0， *b* 更改为 1， *c* 更改为 2，以此类推。，并移除 *x、*y、 *z、*

使用**翻译**删除标点符号和数字的完整语法如下。

```
# importing a string of punctuation and digits to remove
**import** string
exclist = **string**.punctuation + **string**.digits# remove punctuations and digits from oldtext
table_ = **str.maketrans**('', '', exclist)
newtext = oldtext.translate(table_)
```

这种方法将完全删除字符串**中的任何字符。标点符号**和字符串 **.digits.** 包括*！" #$% & \'()*+，-。/:;< = >？@[\\]^_`{|}~'* 和所有数字。

# 使用翻译+连接

但有时，我们可能希望添加一个空格来代替这些特殊字符，而不是完全删除它们。我们可以通过告诉一个表将特殊字符改为空格而不是排除它们来做到这一点。

```
table_ = **str.maketrans**(exclist, ' '*len(exclist))
```

此外，我们可以简单地用**分割**和**连接**来确保这个操作不会导致单词之间出现多个空格。

```
newtext = ' '.**join**(oldtext.**translate**(table_).**split**())
```

# 使用连接+字符串

我们也可以只使用 **join** 而不是 **translate，**从我们上面制作的字符串包中取出相同的排除列表。

```
# using exclist from above
newtext = ''.**join**(x **for** x **in** oldtext **if** x **not in** exclist)
```

# 使用 Join + isalpha

我们可以放弃排除列表，只使用字符串方法来调用字母。

```
newtext = ''.**join**(x **for** x **in** oldtext **if** x.**isalpha**())
```

这种方法只会保留字母表。因此，它还会消除单词之间的空格。

# 使用联接+过滤器

代替列表理解，我们可以使用**过滤器**做同样的事情。这比使用列表理解稍微更有效，但是以相同的方式输出新的文本。

```
newtext = ''.**join**(**filter**(str.**isalpha**, oldtext))
```

# 使用替换

另一种删除标点符号(或任何选定字符)的方法是遍历每个特殊字符，一次删除一个。我们可以通过使用**替换**的方法来做到这一点。

```
# using exclist from above
**for** s **in** exclist:
     text = text.**replace**(s, '')
```

# 使用正则表达式

根据确切的目标，使用 regex 有许多方法可以完成类似的事情。一种方法是用空格替换不是字母的字符。

```
**import** re
newtext = re.**sub**(r'[^A-Za-z]+', ' ', oldtext)
```

**【^a-za-z]+】**选择符合方括号( **[]** )内规则的任意字符，即而非( **^** )在大写字母( **A-Z** )或小写字母( **a-z** )中至少有一个字母( **+** )。然后 regex **sub** 用空格替换旧文本中的这些字符。

另一种方法是使用元字符 **\W.** 选择所有非单词。该元字符不包括下划线(-)和数字。

```
newtext = re.**sub**(r'\W+', ' ', oldtext)
```

![](img/c78fa8eb4d74ee827742d3bf9c5b1218.png)

照片由[凯勒·琼斯](https://unsplash.com/@gcalebjones?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 表演

我们回顾了一些方法，但是哪一个是最好的呢？我使用了 [timeit](https://docs.python.org/2/library/timeit.html) 模块来测量每种方法处理大约 1kb 的字符串数据 10000 次需要多长时间。

测试表明，与其他方法相比，使用翻译花费的时间要少得多！另一方面，在列表理解中使用**连接**似乎是清理选择字符最低效的方式。 **Translate** 是当今所有评测中最通用、最快速的选项。

如果你有任何其他方法，请留下评论，我会将测试结果添加到帖子中！