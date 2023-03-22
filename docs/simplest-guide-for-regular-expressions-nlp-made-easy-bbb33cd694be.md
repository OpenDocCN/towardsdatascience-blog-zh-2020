# 正则表达式简单指南

> 原文：<https://towardsdatascience.com/simplest-guide-for-regular-expressions-nlp-made-easy-bbb33cd694be?source=collection_archive---------41----------------------->

## NLP 变得简单

![](img/3bb29e2693b2a441791bf64a7b8c92b1.png)

由[弗兰基·查马基](https://unsplash.com/@franki?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

如果你不知道正则表达式，它会被认为是很难和高级的东西，但是如果你知道，恭喜你，你知道很难和高级的东西。在这篇文章中，我们将揭示正则表达式的基础，在文章的最后，我附上了一个指向我的笔记本的链接，在那里你可以看到除了我们将在这里讨论的功能之外的三倍多的功能。

所以让我们从定义正则表达式开始:

> 正则表达式是定义搜索模式的字符序列。

## 它们被用在哪里？

正则表达式有广泛的用途:

*   文本预处理
*   根据某种模式提取文本，查找并替换某些符合模式匹配的单词，例如，查找所有以“a”开头的单词，并用单词 cat 替换它们
*   密码模式匹配
*   数据有效性

正则表达式在您的日常编码任务中起着至关重要的作用，此外，当涉及到数据清理、数据挖掘和其他过于庞大而无法硬编码的操作时，它还是一个超级强大的工具。我们创建一个正则表达式模式，在整个文本中滑动，匹配部分的位置或值返回给我们。

正则表达式模式匹配有两个部分:

1.正确的模式:它是关于找出我们真正想要匹配的东西

2.右 **re** 功能:与职位有关。不管我们是想匹配一开始的模式还是任何地方的模式。万一我们想要分裂和替换，我们也必须改变我们的 re 函数。

## **问题:给你两个文本，你必须找出它们是否以单词 corona 开头。**

所以现在我们必须弄清楚两件事:使用什么模式，使用什么函数。

1.  我们必须匹配单词 corona，所以 corona 是我们的模式字符串。
2.  对于函数，我们将选择`re.match`,因为我们需要找到字符串**是否以图案电晕开始**。
3.  `re.match`接受 3 个输入- >模式匹配、需要搜索的文本和标志。我们稍后会谈到旗帜。

**输出**:

```
<re.Match object; span=(0, 6), match='corona'>
None
```

如果模式与字符串匹配，则返回一个匹配对象，否则不返回任何对象。

在匹配的情况下，如在`result_text1`中，返回具有三个属性的匹配对象:

*   `.span()`:返回包含匹配开始和结束位置的元组。
*   `.string`:返回传递给函数的字符串
*   `.group()`:返回匹配的字符串部分

**输出**:

```
Span of result_text1:  (0, 6)
String passed to result_text1:  corona epidemic has taken world by storm.
Groups in result_text1:  corona
```

# 与`re.match`的问题:

*   这是一个非常好的函数，只在开始的时候匹配字符串。
*   如果我们想要匹配字符串中的任何地方，我们使用`re.search`。它的工作原理和`re.match`一样，但是允许我们匹配字符串中的任何位置。返回一个**匹配对象**，如果没有找到匹配，则返回**无**。

**输出**:

```
<re.Match object; span=(0, 6), match='corona'>
None
<re.Match object; span=(29, 35), match='corona'>
```

# `re.match`和`re.search`的限制

`re.match`只能在开始时匹配，而`re.search`可以在任何地方匹配，但只返回第一个匹配。原因是`re.search`是为了查找字符串中是否存在模式而设计的。如果我们想要返回所有的匹配，我们使用`re.findall`。事实上，`re.findall`默认用于模式匹配，因为通过各种标识符和条件，它可以表现为`re.match` & `re.search`。

```
<re.Match object; span=(0, 6), match='corona'>
['corona', 'corona']
```

## 如果我们想替换模式匹配我们选择的字符串的地方，该怎么办？

假设我们想用新冠肺炎病毒取代所有出现的冠状病毒。为此，我们使用`re.sub` `re.sub(pattern,replacement,text,count)`:模式是我们试图匹配的内容，替换是用来替换模式的内容，文本是我们搜索模式的内容。设定**计数**以替代有限的发生次数。默认情况下，它会替换所有引用。

**输出**:

```
coronavirus is causing international shutdowns. Neil Ferguson's report stated that coronavirus matches SARS.
COVID-19 is causing international shutdowns. Neil Ferguson's report stated that COVID-19 matches SARS.ONLY ONE OCCURENCE IS SUBSTITUTED IF WE MENTION COUNT = 1coronavirus is causing international shutdowns. Neil Ferguson's report stated that coronavirus matches SARS.
COVID-19 is causing international shutdowns. Neil Ferguson's report stated that coronavirus matches SARS.
```

# 在模式匹配的地方拆分文本

`re.split(pattern,string,maxsplit,flags)` 模式和字符串是常用参数。maxsplit 表示我们最大希望进行多少次分割，默认情况下表示全部。我们稍后会谈到旗帜

**输出**:

```
['COVID-19 is ', 'virus, as of 24 March ', 'virus has causes more than 400,000 cases.']
```

> 字符串在匹配模式的地方被分割。由于上述字符串有两个电晕事件，它只是分裂它在两个地方，因此导致三个部分。将返回拆分列表。

正则表达式也是优秀的武器文本清理、文本挖掘和在一行程序中做复杂的事情。

其余功能请查看我的 Github 笔记本。正则表达式的其他高级特性将在此讨论:

[](https://github.com/iamchiragsharma/Regular-Expressions-NLP-Text) [## iamchiragshama/正则表达式-NLP-Text

### 自然语言处理和机器学习的正则表达式。贡献给 iamchiragshama/正则表达式-NLP-Text…

github.com](https://github.com/iamchiragsharma/Regular-Expressions-NLP-Text) 

在 twitter 上关注我，了解关于自然语言处理、深度学习和文本分析的更多更新:

[](https://twitter.com/csblacknet) [## 奇拉格·夏尔马

### Chirag Sharma 的最新推文(@csblacknet)。联合创始人 BlackNet |我创造可持续和实用的人工智能。印度

twitter.com](https://twitter.com/csblacknet)