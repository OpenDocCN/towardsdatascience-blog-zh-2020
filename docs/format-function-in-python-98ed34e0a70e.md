# Python 中的格式函数

> 原文：<https://towardsdatascience.com/format-function-in-python-98ed34e0a70e?source=collection_archive---------5----------------------->

Python 的 string 类别的 **str.format()** 技术允许您尝试进行变量替换和数据格式化。这使您能够通过点数据格式以所需的间隔连接字符串的各个部分。

本文可以引导您了解 Python 中格式化程序的一些常见用法，这可能有助于您的代码和程序对用户友好。

**1)单一格式化程序:**

格式化程序的工作方式是将一个或多个替换字段或占位符(由一对方括号 **"{}"** )固定成一个字符串，并调用 str.format()技术。您将把希望与字符串连接的值传递给 format()方法。运行程序后，该值将打印在占位符{}所在的位置。单格式器可以定义为只有一个占位符的格式器。在下面的例子中，你可以看到 print 语句中 format 的实现。

```
print("{} is a good option for beginners in python".format("Research Papers"))**OUTPUT:** Research Papers is a good option for beginners in python
```

除了在 print 语句中直接使用它之外，我们还可以对变量使用 format()

```
my_string = "{} is a good option for beginners in python"print(my_string.format("Research Papers"))**OUTPUT:** Research Papers is a good option for beginners in python
```

**2)多重格式化程序:**

比方说，如果一个句子中需要另一个变量替换，可以通过在需要替换的地方添加第二个花括号并将第二个值传递给 format()来实现。然后 Python 会用输入中传递的值替换占位符。

```
my_string = "{} is a good option for beginners in {}"
print(my_string.format("Research Papers","Machine Learning"))**OUTPUT:** Research Papers is a good option for beginners in Machine Learning
```

我们可以在给定变量中添加任意数量的占位符或花括号，并为格式()添加相同数量的输入。

```
my_string = "{} is an {} option for {} in {}"
print(my_string.format("Research Papers","excellent","experienced","Machine Learning"))**OUTPUT:** Research Papers is an excellent option for experienced in Machine Learning
```

**3)使用位置和关键字参数的格式化程序:**

当占位符为空{}时，Python 解释器将通过 str.format()按顺序替换这些值。

str.format()方法中存在的值主要是 tuple(**“tuple 是一系列不可变的 Python 对象”**)数据类型，tuple 中包含的每个单独的项通常由其索引号引用，索引号从零开始。然后，这些索引号被传递到原始字符串中的花括号中。

您可以使用花括号中的位置参数或索引号，以便将特定值从格式()中获取到变量中:

```
my_string = "{0} is a good option for beginners in {1}"
print(my_string.format("Research Papers","Machine Learning"))**OUTPUT:**
Research Papers is a good option for beginners in Machine Learningmy_string = "{1} is a good option for beginners in {0}"
print(my_string.format("Research Papers","Machine Learning"))**OUTPUT:**
Machine Learning is a good option for beginners in Research Papers
```

关键字参数通过调用花括号内的变量名来帮助调用 format()中的变量:

```
my_string = "{0} is a good option for beginners in {domain}"
print(my_string.format("Research Papers",domain = "Machine Learning"))**OUTPUT:** Research Papers is a good option for beginners in Machine Learning
```

我们可以同时使用关键字和位置参数:

```
my_string = "{domain} is a good option for beginners in {0}"
print(my_string.format("Research Papers",domain = "Artificial Intelligence"))**OUTPUT:**Artificial Intelligence is a good option for beginners in Research Papers
```

**4)型号规格:**

通过使用格式代码语法，可以在语法的花括号中包含更多的参数。在此语法中，无论 field_name 在哪里，它都会指定参数或关键字对 str.format()技术的指示符，而 conversion 是指数据类型的转换代码。一些转换类型包括:

s-字符串

d —十进制整数(以 10 为基数)

f——浮动

c-字符

b —二进制

o-八进制

x—16 进制，9 后面有小写字母

e —指数符号

```
my_string = "The Temperature in {0} today is {1:d} degrees outside!"
print(my_string.format("Research Papers",22))**OUTPUT:** The Temperature in Vizag today is 22 degrees outside!
```

确保使用正确的转换。如果您使用不同的转换代码，将会出现以下错误:

```
my_string = "The Temperature in {0} today is {1:d} degrees outside!"
print(my_string.format("Vizag",22.025))--------------------------------------------------------------------ValueError                                Traceback (most recent call last)  in () **      1** my_string = "The Temperature in {0} today is {1:d} degrees outside!" ----> 2 print(my_string.format("Vizag",22.025))  ValueError: Unknown format code 'd' for object of type 'float'
```

您甚至可以限制浮点整数中的小数位数:

```
my_string = "The Temperature in {0} today is {1:f} degrees outside!"
print(my_string.format("Vizag",22.025))**OUTPUT:**
The Temperature in Vizag today is 22.025000 degrees outside!my_string = "The Temperature in {0:20} today is {1:.2f} degrees outside!"
print(my_string.format("Vizag",22))**OUTPUT:** The Temperature in Vizag today is 22.02 degrees outside!
```

**5)使用格式化程序的间距和对齐:**

我们可以使用 format()将空格或对齐方式应用到占位符的右侧、左侧或两侧。对齐代码是:

< : left-align text

^ : center text

>:右对齐

```
my_string = "The Temperature in {0:20} today is {1:d} degrees outside!"
print(my_string.format("Vizag",22))**OUTPUT:** The Temperature in Vizag                today is 22 degrees outside!my_string = "The Temperature in {0} today is {1:20} degrees outside!"
print(my_string.format("Vizag",22))**OUTPUT:** The Temperature in Vizag today is                   22 degrees outside!
```

我们可以看到字符串是左对齐的，数字是右对齐的。通过使用 format()，我们可以对它们进行如下修改:

```
my_string = "The Temperature in {0:>20} today is {1:d} degrees outside!"
print(my_string.format("Vizag",22))**OUTPUT:** The Temperature in                Vizag today is 22 degrees outside!my_string = "The Temperature in {0:<20} today is {1:d} degrees outside!"
print(my_string.format("Vizag",22))**OUTPUT:** The Temperature in Vizag                today is 22 degrees outside!my_string = "The Temperature in {0:^20} today is {1:d} degrees outside!"
print(my_string.format("Vizag",22))**OUTPUT:** The Temperature in        Vizag         today is 22 degrees outside!
```

**6)整理资料:**

我们倾向于在 Excel 表中组织数据，我们可以用各种方法调整列的大小，但是我们如何在程序中应用同样的东西，一列中的值以指数方式递增，一列中的项目进入另一列，或者最终用户可能会发现很难理解哪个值属于哪个列。

```
for i in range(4,15):
   print(i,i*i,i*i*i)**OUTPUT:** 4 16 64
 5 25 125
 6 36 216
 7 49 343
 8 64 512
 9 81 729
 10 100 1000
 11 121 1331
 12 144 1728
 13 169 2197
 14 196 2744
```

在这里，我们可以使用 format()来定义每一列之间的间距，以便最终用户可以轻松地区分不同列的值。

```
for i in range(4,15):
    print("{:6d} {:6d} {:6d}".format(i,i*i,i*i*i))**OUTPUT:** 4     16     64
      5     25    125
      6     36    216
      7     49    343
      8     64    512
      9     81    729
     10    100   1000
     11    121   1331
     12    144   1728
     13    169   2197
     14    196   2744
```

**总结:**

从上面的使用中，我们可以说，用于变量替换的格式化程序是连接字符串、转换值、组织值和数据的有效方法。格式化程序代表了一种简单但非描述性的方式，用于将变量替换传递到字符串中，并有助于创建某些可理解且用户友好的输出。

**谢谢**