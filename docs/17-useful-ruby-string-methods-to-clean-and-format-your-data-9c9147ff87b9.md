# 清理和格式化数据的 17 个有用的 Ruby 字符串方法

> 原文：<https://towardsdatascience.com/17-useful-ruby-string-methods-to-clean-and-format-your-data-9c9147ff87b9?source=collection_archive---------8----------------------->

## 你不写的每一行代码都是你不需要维护的一行代码

![](img/1e61ae1c710a26b0ad16fb818431a034.png)

凯文·Ku 摄于 [Pexels](https://www.pexels.com/photo/coding-computer-data-depth-of-field-577585/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

大约两年前，当我开始使用 Ruby 时，我第一次知道了这个方法，自从我发现了它，我每天都用它来比较两个字符串。从那以后，我遇到了其他几种方法。我想在这里编译我最喜欢的——这些是最适合简化代码的。

字符串操作是程序员每天都要做的工作。从清理到格式化数据，再到分析字符串，各不相同。根据您的需要，您最终可能会采用以下一种、多种或所有方法。

在这篇文章中，我将讨论由 [Ruby String 类](https://ruby-doc.org/core-2.7.1/String.html)提供的 17 种有效的字符串方法，它们将通过节省宝贵的时间使你的生活变得更加容易。

## 1.迭代字符串中的每个字符

我们经常需要遍历字符串来处理字符。例如，您可能想要打印所有元音。

```
str = "abcdeU"
temp = ""
str.each_char do |char|
 puts char if ['a','e','i','o','u'].include? char.downcase
end
# a
# e
# U
```

我们可以加上`with_index`得到字符的位置。

```
str = "abcdeU"
temp = ""
str.each_char.with_index do |char, i|
 puts "#{i} #{char}" if ['a','e','i','o','u'].include? char.downcase
end
# 0 a
# 4 e
# 5 U
```

默认情况下，索引从零开始，但是您可以根据需要定义它。

```
str = "abcdeU"
temp = ""
str.each_char.with_index(100) do |char, i|
 puts "#{i} #{char}" if ['a','e','i','o','u'].include? char.downcase
end
# 100 a
# 104 e
# 105 U
```

另一个有用的方法是`str.each_byte`来获取单个字节。当你处理 ASCII 码的时候，这很有帮助。

```
str = "abcdeABCDE"
str.each_byte do |char|
 puts char
end
# 97
# 98
# 99
# 100
# 101
# 65
# 66
# 67
# 68
# 69
```

## 2.将字符串转换为字符数组

要将字符串转换成数组，我们可以使用`str.chars`，它是`str.each_char.to_a`的简写。

```
char_array = "abcdeABCDE".chars
# ["a", "b", "c", "d", "e", "A", "B", "C", "D", "E"]
```

现在我们有了一个数组，我们可以使用任何[数组](https://ruby-doc.org/core-2.7.1/Array.html)方法！多酷啊！例如，`join`方法将数组中的每个元素转换成一个字符串，由给定的分隔符分隔。通过公开数组方法，我们有更多的选项来操作字符串。

```
char_array.map { |c| c.downcase }.join(', ')
# "a, b, c, d, e, a, b, c, d, e"
```

## 3.获取字符串的长度

我认为这是有史以来最常用的方法。当您想在将字符串插入数据库表之前检查字符串长度时，这非常有用。我们也可以用`size`，是同义词。根据你的喜好，你可以选择其中任何一个👍。我更喜欢用`length`的方法，因为它更容易理解。

```
"HELLO World".length 
# 11
"HELLO World".size
# 11
```

## 4.获取字符串的字符数

`str.count`将一组或多组字符作为参数。之后，我们取这些集合的交集来得到最终的字符集。最后，这套用来统计`str`中的人物。

```
"look up!".count("o")
# 2
"look up!".count("m")
# 0
"abcdef".count("a-c", "c-f")
# 1
```

我们可以用它来统计多个字符，所以我们来统计一下元音的个数。

```
"abcdeUUU".downcase.count("aeiou")
# 5
```

辅音怎么样？露比掩护你。`^`符号是用来否定人物的。

```
"abcdeUUU".downcase.count("^aeiou")
# 3
```

以上两个例子并没有涵盖每一种情况，比如含有数字或特殊字符的字符串。

```
"^-12#abcdeUUU".downcase.count("^aeiou")
# 8
```

如果我们想更清楚地了解允许的字符集，我们可以使用下面的例子。这里的`a-z`表示它们之间的所有字符。之后，我们排除元音。如果我们取第一个和第二个字符集的交集，我们就有了有效常量集。

```
"^-12#abcdeUUU".downcase.count("a-z", "^aeiou")
# 3
```

如果我们想在我们的字符集中包含`^`或`-`符号，我们需要使用反斜杠字符对它们进行转义。在这里，我们通过用反斜杠字符对符号进行转义来包含符号`^`或`-`。因此，最终的字符集将有`^`、`-`和`0 to 9`。

```
"^-1234#".downcase.count("\\^\\-0-9")
# 6
```

## 5.反转一根绳子

反转字符串会很方便，例如，当你想检查一个字符串是否是回文时。

```
str = "Anna"
str.reverse 
# "annA"

puts "palindrome" if str.downcase == str.downcase.reverse
# palindrome

# eql? is a synonym for ==
puts "palindrome" if str.downcase.eql?(str.downcase.reverse)
# palindrome
```

## 6 搜索字符串中的一个或多个字符

如果字符串或字符存在，则`str.include?`返回 true，否则返回 false。

```
"hEllo wOrlD".include?("w") 
# true
"hEllo wOrlD".include?("1") 
# false
```

## 7.替换字符串中的字符

**替换字符串中的一个或多个字符是清理或格式化数据的好方法。`str.gsub`或全局替换用提供的字符串替换所有出现的内容。这里第一个参数表示我们想要替换的字符集，第二个参数是替换字符集。**

```
"Red, Red and Blue".gsub("Red", "Orange") 
"Orange, Orange and Blue"
```

**如果您想替换第一次出现的，使用`str.sub`。**

```
"Red, Red and Blue".sub("Red", "Orange") 
"Orange, Red and Blue"
```

**`str.gsub`也接受散列或块。**

```
"organization".gsub("z", 'z' => 's') 
# "organisation"
```

**这里我们寻找数字，并在开头添加一个`$`符号。**

```
"Price of the phone is 1000 AUD".gsub(/\d+/) { |s| '$'+s } 
# "Price of the phone is $1000 AUD"
```

## **8.拆开一根绳子**

**根据分隔符(默认为空格)或模式分割字符串。**

```
sentence = "There Is No Spoon"
words = sentence.split
# ["There", "Is", "No", "Spoon"]
sentence = "There_Is_No_Spoon"
words = sentence.split("_")
# ["There", "Is", "No", "Spoon"]
```

**`?=`用于正向预测以查找大写字母。**

```
sentence = "ThereIsNoSpoon"
words = sentence.split(/(?=[A-Z])/)
# ["There", "Is", "No", "Spoon"]
sentence = "a111b222c333"
words = sentence.split(/(?=[a-z])/)
# ["a111", "b222", "c333"]
```

**您可以通过提供第二个参数来限制拆分的数量。**

```
sentence = "June 27,June 26,June 25"
words = sentence.split(/,/, 2)
# ["June 27", "June 26,June 25"]
```

## **9.修剪绳子**

**`str.trim`将删除以下任何前导和尾随字符:`null("\x00")`、横线`tab("\t")`、`line feed(\n)`、`vertical tab("\v")`、`form feed(f)`、`carriage return(\r)`、`space(" ")`。**

```
" hEllo WOrlD \n\t\v\r ".strip 
# "hEllo WOrlD"
```

## **10.修剪字符串的最后一个字符**

**当给定记录分隔符或回车符(`\n`、`\r`和`\r\n`)时，`str.chomp`删除尾随字符。**

```
"...hello...world...".chomp(".")
# "...hello...world.."
"...hello...world".chomp(".")
"...hello...world"
"...hello...world...\n".chomp(".")
# "...hello...world...\n"
"...hello...world...\n".chomp
# "...hello...world..."
"...hello...world...\r".chomp
# "...hello...world..."
"...hello...world...\r\n".chomp
# "...hello...world..."
"...hello...world...\n\r".chomp
"...hello...world...\n"
```

## **11.在另一个字符串之前添加一个字符串**

**将一个或多个字符追加到字符串的开头。**

```
a = "world" 
a.prepend("hello ") 
# "hello world"
```

## **12.插入字符串**

**向字符串的特定位置添加一个或多个字符。**

```
a = "hello" 
a.insert(a.length, " world") 
# "hello world"
```

## **13.更改字符串大小写的方法**

**`str.downcase`会将字符串中的每个字符转换成小写。**

```
"HELLO World".downcase 
# "hello world"
```

**`str.upcase`会将字符串中的每个字符转换成大写。**

```
"hello worlD".upcase 
# "HELLO WORLD"
```

**`str.capitalize`将字符串的第一个字符转换成大写，其余的转换成小写。**

```
"hEllo wOrlD".capitalize 
# "Hello world"
```

**`str.swapcase`会将字符串中的大写字符转换成小写字符，并将小写字符转换成大写字符。**

```
"hEllo WOrlD".swapcase 
# "HeLLO woRLd"
```

## ****14。添加字符串****

**一种常见的字符串操作是串联。为此，我们可以使用`str.concat`或`<<`。**

```
str1 = "hello"
str2 = "world"
str1.concat(" ").concat(str2)
puts "#{str1}"
# "hello world"

# << is same as concat
str1 = "hello"
str2 = "world"
str1 << " " << str2
puts "#{str1}"
# "hello world"
```

## **15.获取子字符串**

**当您想要字符串的特定部分时,`str.slice`方法是完美的；它返回一个子字符串，其中第一个索引是包含性的，第二个索引是排他性的。**

```
str = "hello world"
puts "#{str.slice(0, 5)}"
# hello
```

## **16.查找带有给定前缀和后缀的字符串**

**我们可以检查一个字符串是以一个字符串开始还是结束。**

```
str = "Mr. Leonardo"
str.start_with?("Mr.")
# true
str = "The quick brown fox jumps over the lazy dog."
str.end_with?(".")
# true
```

## **17.空字符串检查**

**大概另一个最常用的方法是`str.empty`，可以用于数据验证。**

```
output = ""
output.empty?
# true
output = " "
output.empty?
# false
```

# **包裹**

**像其他编程语言一样，Ruby 有自己的标准库方法，这些方法针对性能进行了优化。使用这 17 种方法，你可以直接解决问题，而不是花时间实现大量的帮助方法。永远记住，没有写的一行代码是你不必维护的代码。如果你有兴趣学习更多关于 Ruby 的知识，请阅读下面的初学者指南。编码快乐！**

**[](https://medium.com/swlh/a-beginners-guide-to-ruby-810f230c53da) [## Ruby 初学者指南

### 离写下你的下一件大事又近了一步

medium.com](https://medium.com/swlh/a-beginners-guide-to-ruby-810f230c53da)**