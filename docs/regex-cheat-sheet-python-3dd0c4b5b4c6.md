# 正则表达式备忘单— Python

> 原文：<https://towardsdatascience.com/regex-cheat-sheet-python-3dd0c4b5b4c6?source=collection_archive---------17----------------------->

## 终极指南

## 用正则表达式升级你的搜索方法！

你有没有发现自己感到困惑，试图从一串字符中提取一些有价值的信息？我应该承认这和“大海捞针”差不多，除非你懂 regex！

![](img/d94a3a474bb65b36d01fc068b77de63d.png)

布莱克·康纳利在 [Unsplash](https://unsplash.com/) 上拍摄的图片

假设您想从数据库 *log.txt* 中提取公司客户的电话号码，如下所示:

```
1\. Ema Dough (+1-202-555-0189) - 915 Ridge Street Corpus, TX 78418
2\. Tom Hitt (+33-93-751-3845) - 9190 Berkshire Ave. Wayne, NJ 07470
3\. Maya Raine (+49-30-833-931-313) - 18 SW. Sage Ave. Ride, CA 95993
```

遍历所有的句子并定位括号之间的数字需要做大量的工作。这确实是真的，直到我们的救世主正则表达式(regex)的到来！

# 什么是正则表达式？

正则表达式是一种高级字符串搜索方法，允许用户在文本中搜索某些内容。这是通过创建一个匹配我们想要检索的信息的模式来实现的。正则表达式具有如此强大的功能，以至于它已经被包含在许多编程语言中，如 Python、Pearl、JavaScript、PHP 和 Java。

所以，让我们回到我们的问题上来！

我们可以通过首先创建一个模式来获取用户的电话号码。请注意，电话号码位于括号“()”之间。这是一个有用的信息，我们可以利用它。为了使用 Python 中的正则表达式，我们需要**导入**re 库**。**

```
import re phone_numbers = [] 
pattern = r"\(([\d\-+]+)\)"with open("log.txt", "r") as file: 
    for line in file: 
        result = re.search(pattern, line)
        phone_numbers.append(result.group(1))print(phone_numbers)
```

**输出:**

```
['+1-202-555-0189', '+33-93-751-3845', '+49-30-833-931-313']
```

**我将一个接一个地检查这段代码:**

1.  **导入重新导入 Python 中的正则表达式库。**
2.  **phone_numbers = [] —准备存储电话号码的列表。**
3.  **pattern = r"\(([\d\-+]+)\)" —我们用来定位电话号码的模式，我们将在本文后面讨论每个符号的作用！**
4.  **用 open("log.txt "，" r ")作为文件:—打开我们要处理的文件。**
5.  **对于文件中的行:—迭代(遍历)log.txt 中的每一行**
6.  **result = re.search(pattern，line)-在该行中搜索电话号码**
7.  **phone _ numbers . append(result . group(1))-将客户的电话号码添加到电话号码列表中**
8.  **print(phone_numbers) —打印电话号码列表。**

**re 库中有许多有用的函数和字符，然而学习所有的东西可能会让人不知所措。因此，我选择了最有用的函数和字符，它们将帮助您开始在 Python 脚本中实现 RegEx。**

**让我们在 re 图书馆开始潜水吧！**

# **正则表达式原始字符串**

**在我们的例子中，我们在 log . txt:r“\(([\ d \-]+)\)”中使用这种模式**

**你想知道为什么我们要在字符串前输入“r”吗？这里的“r”代表原始字符串。Python 正则表达式使用反斜杠(\)来表示特殊序列或作为转义字符。这与 Python 在 string lateral 中出于相同目的使用反斜杠(\)相冲突。因此，这里的原始字符串用于避免两者之间的混淆。除此之外，它还帮助我们使我们的模式更短。如果不键入“r”，可能需要键入“\\\”来表示反斜杠(\)。**

**所以，别忘了你的“r”！**

# ****正则表达式特殊序列****

**我们将从正则表达式中最简单的语法开始，即特殊序列。RegEx 中的特殊序列以反斜杠(\)开头。因此，如果您在正则表达式模式中遇到反斜杠，那么它很可能是特殊序列的语法。**

```
**\d**               matches a single digit character [0-9]**\w**               matches any alphabet, digit, or underscore**\s**               matches a white space character (space, tab, enter)
```

**这些序列的否定也可以用大写字母来表示。例如，\D 是\d 的否定。**

```
**\D**               matches a single non-digit character
```

# ****正则表达式元字符****

**然后我们将通过元字符，这将有助于我们达到我们的目标。这些字符中的每一个都有其特殊的含义。**

```
**.**                matches any character(except for newline character)^                the string starts with a character$                the string ends with a character*                zero or more occurrences +                one or more occurrences?                one or no occurrence {}               exactly the specified number of occurrences|                either or
```

**示例:**

```
"c.t"            will match anything like "cat", "c*t", "c1t", etc"^a"             will match "a" from "a cat" but not "eat a cake""cat$"           will match "a cat" but not "cat party""a*b"            will match "b", "ab", "aab", "aaab", ..."a+b"            will match "ab", "aab", "aaab", ..."a?b"            will match "b" or "ab""a{1}b"          will match "ab""a{1,3}b"        will match "ab", "aab", or "aaab""cat|dog"        will match "cat" or "dog"
```

# **正则表达式集**

**集合可以用来匹配方括号内的一个字符。**

```
**[abcd]**           matches either a, b, c or d
```

**您也可以使用我们之前讨论过的特殊序列。除此之外，还有一个破折号，我们很快就会讲到。**

```
**[a-z0-9]**         matches one of the characters from a-z or 0-9**[\w]**             matches an alphabet, digit, or underscore
```

**脱字符号 character(^)代表除了。**

```
**[^\d]**            matches a character that is not a digit [0-9]
```

**要在集合中包含具有特殊含义的字符，如反斜杠(\)和破折号(-)，您需要在前面添加一个反斜杠(\\)和(\-)-第一个反斜杠代表转义字符。转义字符使得具有特殊含义的字符可以按字面意思理解。**

**然而，没有任何特殊含义的字符，如？_+*.|()${}可以直接使用。**

# **正则表达式函数**

**最后，我们将通过使用可用的函数来完成 RegEx 所能做的事情！**

```
**findall()** Returns a list that contains all matches**search()** Returns a 'match object' if there is a match in          the string**split()** Returns a list of string that has been split at each match**sub()** Replaces the matches with a string
```

**在所有这些函数中，自变量都是相同的，分别是<pattern>和<string>。</string></pattern>**

**示例:**

1.  **findall()**

```
import repattern = r".at"
line = "The big fat cat sat on a cat"
result = re.findall(pattern, line)print(result)
```

**输出:**

```
['fat', 'cat', 'sat', 'cat']
```

**2.搜索()**

```
import repattern = r".* .*"
line = "Ada Lovelace"
result = re.search(pattern, line)print(result)
print(result.group())
print(result.group(0))
```

**输出:**

```
<_sre.SRE_Match object; span=(0, 12), match='Ada Lovelace'>
Ada Lovelace
Ada Lovelace
```

**3.拆分()**

```
import repattern = r"cat"
line = "The big fat cat sat on a cat"
result = re.split(pattern, line)print(result)
```

**输出:**

```
['The big fat ', ' sat on a ', '']
```

**4.sub()**

```
import repattern = r"Ada"
line = "Ada Lovelace"
result = re.sub(pattern, r"Tom", line)print(result)
```

**输出:**

```
Tom Lovelace
```

# **正则表达式捕获组**

**当我们想要从匹配中提取信息时，捕获组非常有用，就像我们的例子 log.txt 一样。**

**我们在 log.txt 中使用这种模式:r“\(([\ d \-]+)\)”**

**这里，我们使用捕获组只是为了提取电话号码，不包括括号字符。**

**我们想要提取的电话号码可以通过 result.group(1)来访问。**

**通过例子理解捕获组会更容易。**

**示例:**

1.  **搜索()**

```
import repattern = r"(.*) (.*)"
line = "Ada Lovelace"
result = re.search(pattern, line)print(result)
print(result.groups())
print(result.group(0))
print(result.group(1))
print(result.group(2))
```

**输出:**

```
<_sre.SRE_Match object; span=(0, 12), match='Ada Lovelace'>
('Ada', 'Lovelace')
Ada Lovelace
Ada
Lovelace
```

**2.拆分()**

```
import repattern = r"(cat)"
line = "The big fat cat sat on a cat"
result = re.split(pattern, line)print(result)
```

**输出:**

```
['The big fat ', 'cat', ' sat on a ', 'cat', '']
```

**3.sub()**

```
import repattern = r"(.*) (.*)"
line = "Ada Lovelace"
result1 = re.sub(pattern, r"\2 \1", line)
result2 = re.sub(pattern, r"Tom", line)print(result1)
print(result2)
```

**“\1”和“\2”分别代表第一个和第二个捕获组。**

**输出:**

```
Lovelace Ada
Tom
```

# **最后一个想法…**

**正则表达式将是你编程工具箱中的一个强大工具，尤其是在 Python 中。现在您可能已经意识到 RegEx 的潜力是无穷的。绝对值得花时间和精力去学习 RegEx。回报会比你想象的多。请记住:**

> **努力永远不会让你失望。—匿名**

**如果这些概念没有在一开始就被接受，不要担心，我们都经历过，你也可以做到！经过几次练习，你会掌握它的。如果你想了解更多关于 RegEx 的知识，我推荐你去看看这个数据营课程。**

**[](https://www.datacamp.com/courses/regular-expressions-in-python) [## Python 中的正则表达式

### 作为一名数据科学家，您将会遇到许多需要从大量数据中提取关键信息的情况

www.datacamp.com](https://www.datacamp.com/courses/regular-expressions-in-python) 

不要浪费任何时间。准备好，让我们征服正则表达式库！

问候，

**弧度克里斯诺****