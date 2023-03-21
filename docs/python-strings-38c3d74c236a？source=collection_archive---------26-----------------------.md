# Python 字符串

> 原文：<https://towardsdatascience.com/python-strings-38c3d74c236a?source=collection_archive---------26----------------------->

## 他们比你想象的要多得多

![](img/77f5d6977e3c7780839bdc66ae49a64f.png)

照片由 [Aditya Wardhana](https://unsplash.com/@wardhanaaditya?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在 Python 中，字符串似乎是简单的数据类型。但是你知道`str.format_map`是做什么的吗？还是`str.expandtabs`？没有吗？好的，`str.isidentifier`怎么样？

直到最近，我才听说过他们中的任何一个——如果你听说过，公平地说，我印象深刻。

Python 的字符串方法多种多样，有些非常有用，有些则不那么有用。但是它们都有自己独特的用例。我们将看看一些不太常见但仍然有用的特性。

所以，让我们来看看 Python 的字符串方法。

## **案件卷宗**

更激进的版本`lower`。鉴于`lower`只适用于 ASCII 字符`[A-Z] -> [a-z]`，`casefold`试图标准化非标准字符，例如拉丁语或希腊语。

这真的很好用——希腊神话中的奥林匹斯山神赫尔墨斯，在希腊语中被写成ἑρμῆς，在小写中，这是ἑρμῆς.

```
"Ἑρμῆς".casefold() == "ἑρμῆς".casefold()
```

`**[Out]:** True`

相比之下，`lower`在这点上就失败了。

```
"Ἑρμῆς".lower() == "ἑρμῆς".lower()
```

`**[Out]:** False`

## 中心

这里，我们在字符串的两边添加填充，使其居中对齐，如下所示:

```
"hello there".center(20, ".")
```

`[Out]: "....hello there....."`

第二个参数是可选的，默认为`" "`。

## ljust 和 rjust

与`center`类似，这些函数向字符串添加填充，并分别对其进行左对齐或右对齐。同样，默认参数是`" "`。

```
"hello there".ljust(20, ".")
"hello there".rjust(20, ".")[Out]: "hello there........."
       ".........hello there"
```

## 数数

这让我们可以计算一个模式在一个句子中重复的次数。

```
"Lets count how many spaces are in this string".count(" ")
```

`**[Out]:** 8`

```
"You cannot end a sentence with because because because is a conjunction.".count("because")
```

`**[Out]:** 3`

## 编码

允许我们指定或改变字符串的编码类型，默认为`utf-8`。如果字符串包含编码集之外的字符，我们将收到一个`UnicodeEncodeError`。

```
"ἑρμῆσ".encode('ascii')
```

`**[Out]:** UnicodeEncodeError`

我们可以调整错误处理方案的行为——默认为`'strict'`。`'ignore'`会忽略错误，`'replace'`会替换它们。

```
"ἑρμῆσ".encode('ascii', 'ignore')
```

`**[Out]:** b''`

```
"ἑρμῆσ".encode('ascii', 'replace')
```

`**[Out]:** b'??????'`

## 扩展表

这将所有制表符替换为空格，默认为八个空格。

```
print("\tthis is tabbed".replace(" ", "-"))  # without expandtabs
print("\tthis is too"**.expandtabs()**.replace(" ", "-"))  # with**[Out]:** "        this-is-tabbed"
       "--------this-is-too"
```

## 格式 _ 地图

类似于`format`方法，但是允许我们使用字典。例如:

```
mappings = {'x': 'mapped', 'y': 'values', 'z': 'dictionary'}
print("We have {x} the {y} from the {z}".format_map(mappings))
```

`**[Out]:** "We have mapped the values from the dictionary"`

## 分区和 rpartition

`partition`类似于`split`，但是在给定模式的第一个实例上拆分字符串，并且也返回给定模式。`rpartition`做同样的事情，但是从字符串的右边开始。

```
"lets split this".split(" ")
"and this too".partition(" ")
"to the right".rpartition(" ")**[Out]:** ['lets', 'split', 'this']
       ('and', ' ', 'this too')
       ('to the', ' ', 'right')
```

## rfind

其中`find`返回满足给定模式的字符串中第一个字符的索引，`rfind`做同样的事情，但是从字符串的右边开始。

```
"l**e**ts search this string".find("e")
"lets s**e**arch this string".rfind("e")**[Out]:** 1
       6
```

如果没有找到模式，`find`和`rfind`返回`-1`。

```
"this string does not contain the pattern".rfind("ABC")**[Out]:** -1
```

## rindex

`index`和`rindex`几乎总是以与`find`和`rfind`相同的方式工作:

```
"search me".rfind("e")
"search me".rindex("e")**[Out]:** 8
       8
```

除了模式是**而不是**的地方。`index`和`rindex`将返回一个`ValueError`:

```
"another string".rfind("X")
"another string".rindex("X")**[Out]:** -1
       *ValueError: substring not found*
```

## rsplit

与`split`相同，但从右侧开始。这仅在指定了最大分割数时才有所不同，如下所示:

```
"there are six spaces in this string".split(" ", 3)
"there are six spaces in this string".rsplit(" ", 3)**[Out]:** ['there', 'are', 'six', 'spaces in this string']
       ['there are six spaces', 'in', 'this', 'string']
```

## 分割线

也与`split`相同，但由换行符`\n`分开。

```
"""This is a
multi-line\nstring""".splitlines()
```

`**[Out]:** ['This is a', 'multi-line', 'string']`

## 交换情况

交换字符的大小写，也适用于非英语字母。

```
"CamelCase".swapcase()
"Ἑρμῆς".swapcase()[Out]: 'cAMELcASE'
       'ἑΡΜΗΣ'
```

## 翻译

允许我们使用用`maketrans`方法构建的翻译字典来交换模式。

```
translate = "".maketrans("e", "3")
"lets translate this string".translate(translate)**[Out]:** 'l3ts translat3 this string'
```

## 零填充

用零填充字符串的开头，直到达到给定的长度。如果给定的长度小于字符串的长度，则不进行填充。

```
"test one".zfill(5)
"two".zfill(5)**[Out]:** 'test one'
       '00two'
```

# 格式检查

我们也有许多格式检查方法，它们返回`True`或`False`。大多数都是不言自明的，所以我们将保持简短。

## 伊萨勒姆

字符串是字母数字的，只包含字母和数字的混合字符。

```
"a1phanum3r1c".isalnum()  # contains no space character
```

`**[Out]:** True`

```
"a1pha num3r1c".isalnum()  # contains a space character
```

`**[Out]:** False`

## 伊萨法

字符串是按字母顺序排列的，只包含字母吗？

```
"ABC".isalpha()
```

`**[Out]:** True`

```
"A/B/C".isalpha()
```

`**[Out]:** False`

## 伊萨西

字符串是否只包含 128 字符 ASCII 编码标准中的字符？

```
"Pretty much anything on your English language keyboard! Punctuation included ~#!$%^&*()|\<>,.?/:;@'{}[]-=_+ and numbers too 1234567890".isascii()
```

`**[Out]:** True`

```
"We are not allowed Ἑρμῆς, nor £, €, and ¬".isascii()
```

`**[Out]:** False`

## isdecimal

这一个在行为上更有趣，也更出乎意料。它识别只包含 Unicode 十进制数字字符的字符串。这并不意味着像`3.142`这样的十进制数。

在许多不同的脚本中，Unicode 十进制数字字符涵盖了数字本身。例如，它涵盖了`0-9`，它还涵盖了孟加拉数字三৩，以及藏语数字五༥.

```
"123".isdecimal()
```

`[Out]: True`

```
"3.142".isdecimal()
```

`[Out]: False`

```
"༥৩".isdecimal()
```

`[Out]: True`

[Unicode 十进制数字的完整列表可在此处找到。](https://www.fileformat.info/info/unicode/category/Nd/list.htm)

## 标识符

这告诉我们这个字符串是否是一个有效的 Python 标识符，这意味着我们可以将它指定为变量、函数、类等的名称。它包括字母、数字和下划线。但是，它不能以数字开头。

```
"valid_identifier".isidentifier()
"9_is_not_valid".isidentifier()
"CamelCaseIsOkay".isidentifier()
"no spaces".isidentifier()**[Out]:** True
       False
       True
       False
```

## 可打印

检查我们的字符串中是否有没有直接打印到控制台的字符，比如换行符`\n`或制表符`\t`。

```
"line 1".isprintable()
```

`**[Out]:** True`

```
"line 1\nline 2".isprintable()
```

`**[Out]:** False`

## isspace

检查字符串是否只包含空格字符(包括换行符`\n`和制表符`\t`)。

```
" \n \t".isspace()
```

`**[Out]:** True`

## ist title

这个检查每个新单词是否以大写字母开头。它忽略数字和标点符号:

```
"A Typical Title Of Something".istitle()
"123 Still A Title".istitle()
"IT DOES NOT WORK IF WE SHOUT".istitle()
"But not if we don't capitalize everything".istitle()**[Out]:** True
       True
       False
       False
```

仅此而已！Python 中字符串方法的数量(至少对我来说)非常惊人。虽然我很难看到`zfill`的用例(如果你用过，请告诉我)，但其余的看起来确实有用。

我希望这篇文章至少向您介绍了一些 Python 的字符串方法。

如果您有任何问题或建议，请随时通过 [Twitter](https://twitter.com/jamescalam) 或在下面的评论中联系我们。

感谢阅读！

# 其他方法

我已经排除了更多的字符串方法，因为它们要么是不言自明的，要么是常识。这些是:

```
capitalize
endswith
find
format
isdigit
islower
isnumeric
isupper
lower
lstrip
replace
rstrip
strip
startswith
```

如果您喜欢这篇文章，您可能会喜欢我写的另一篇文章，它讲述了 Python 的四个不太为人所知但却非常有用和有趣的特性:

[](/4-super-useful-python-features-993ae484fbb8) [## 4 个非常有用的 Python 特性

### 四个不太知名但非常有用的 Python 功能

towardsdatascience.com](/4-super-useful-python-features-993ae484fbb8)