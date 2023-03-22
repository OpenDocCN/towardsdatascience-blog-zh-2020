# Python 中的 5 种高级字符串方法

> 原文：<https://towardsdatascience.com/5-advanced-string-methods-in-python-5b7f6ef382e1?source=collection_archive---------34----------------------->

![](img/222056282371960e2560df9d3dbddd83.png)

[摄影记者](https://www.shutterstock.com/home)

## 在 python3.8 中使用 f-string 符号格式化漂亮的字符串

## *F 弦符号介绍*

格式化字符串是编程的一个基本但重要的部分。[F-Strings(python 3.6 中引入)](https://docs.python.org/3/reference/lexical_analysis.html#f-strings)提高格式化丰富字符串的简单性和直观性。

**F-字符串符号** *(格式化字符串)*要求在字符串的开头有一个`f`(例如。`f”hello”`)。有许多高级方法可以用来格式化漂亮的字符串，下面将讨论 5 种这样的方法。

# 1.字符串中的变量

在适当的位置插入/替换变量。

```
name = "Alexander Hamilton"
print(f"hello {name}")
# hello Alexander Hamilton
```

# 2.字符串中的变量格式

就地格式化变量，如整数。

## 2.1 打印一个带逗号的大整数:

```
x = 2*10**5
print(f"You have ${x:,} in your bank account")
# You have $200,000 in your bank account
```

## 2.2 打印指定位数的浮点数:

```
pi = 3.1415926535897932384 
print(f"The first 5 digits of pi are {pi:.5f}")
# The first 5 digits of pi are 3.14159
```

# 3.原地布尔逻辑:

也许你想根据几个条件格式化一个字符串。`F-string`符号允许使用布尔参数，正如你对 lambdas 的期望。

```
new_user = False
user_name = "Alexander Hamilton"
print(f"{'Congrats on making your account' if new_user else 'Welcome back'} {user_name}!")
# Welcome back Alexander Hamilton!
```

# 4.打印变量名和值

从 [python 3.8 开始，f 字符串符号](https://docs.python.org/3/whatsnew/3.8.html#f-strings-support-for-self-documenting-expressions-and-debugging)允许打印变量名及其值——这是一个特别有用的调试工具:

```
max_price = 20000
min_price = 4000print(f"{max_price=:,}  |  {min_price=:,})
# max_price=20,000  |  min_price=4,000
```

# 5.格式化预填充字符串中的变量(邮件合并)

字符串格式允许替换预先填充的字符串中的变量。

> 这对于允许外部用户格式化程序将填写的电子邮件信息特别有用。

## 5.1 字符串。格式化方法(首选)

```
my_message = "Welcome to our platform {FNAME}! Your favorite ice cream flavor is {FLAVOR}."FNAME = "Alexander"
FLAVOR = "Mint Chocolate Chip"my_message_formatted = my_message.format(**{"FNAME":FNAME, "FLAVOR":FLAVOR})print(my_message_formatted)
# Welcome to our platform Alexander! Your favorite ice cream flavor is Mint Chocolate Chip.
```

## 5.2 F 字符串方法(带评估)

这里有一个 f 字符串符号不符合*的例子(仅仅因为它打算在代码中使用…)*

```
my_message = "Welcome to our platform {FNAME}! Your favorite ice cream flavor is {FLAVOR}."FNAME = "Alexander"
FLAVOR = "Mint Chocolate Chip"my_message_formatted = eval(f”f’{my_message}’”)print(my_message_formatted)
# Welcome to our platform Alexander! Your favorite ice cream flavor is Mint Chocolate Chip.
```

# 结论

F-string 符号是一种格式化漂亮字符串的简单方法。上面演示了 python3.8 中格式化字符串的 5 种高级方法。

> 考虑在 python 代码中实现这些方法。
> 
> 如果你有其他值得注意的字符串方法，请在评论中分享！