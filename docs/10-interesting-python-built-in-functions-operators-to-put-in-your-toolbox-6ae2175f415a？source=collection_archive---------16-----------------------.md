# 将 10 个有趣的 Python 内置函数和运算符放入您的工具箱

> 原文：<https://towardsdatascience.com/10-interesting-python-built-in-functions-operators-to-put-in-your-toolbox-6ae2175f415a?source=collection_archive---------16----------------------->

![](img/f1e8454ed14d2eaca0d41e7d648c21bd.png)

来源: [Unsplash](https://unsplash.com/photos/D9Zow2REm8U)

我们都至少有过这样的例子，当我们花了相当多的时间试图写一个函数，后来，我们意识到有一个内置函数或操作符。

Python 充满了有趣的内置函数，可以帮助任何应用程序节省时间——在本文中，我们将深入研究其中的 10 个**。**

# r

为了表示一个新行，转义序列是`\n`。打印时，它显示为一个新行。对于`\t`等也是一样，只要放入一个字符串就执行。

`r`号操作员可以帮忙！

```
print('1'+'\t'+'hello')
print('2'+r'\t'+'hello')
```

输出:

```
1     hello
2\thello
```

# 格式映射(字典或对象)

与使用字符串的`format`类似，`format_map`允许引用字典中的条目。

举个例子，

```
dinner = {"Food": "Salad", "Drink": "Orange juice"}
print("Lunch: {Food}, {Drink}".format_map(dinner))
```

输出结果是

```
Lunch: Salad, Orange juice
```

当需要打印几个对象时，这非常方便——它保持了数据的结构化，并允许对要打印的变量进行无限制的存储。

# %

您可能已经了解了`%`的字符串格式化功能:

```
a = "Hi %s" % ("there")
print(a)
```

…输出…

```
Hi there
```

但是您知道您可以对许多其他数据类型使用`%`操作符吗？

*   `%s`用于字符串转换
*   `%c`对于性格
*   `%d`为带符号的十进制整数
*   `%e`对于带小写 e 的指数符号
*   `%E`对于带大写 e 的指数符号
*   `%f`浮点实数

# 标题()

如果希望所有单词的第一个字符大写，而不是只大写第一个单词，可以用`title()`代替`capitalize()`。

```
print('1',capitalize('hello there'))
print('2',title('hello there'))
```

输出将是

```
Hello there
Hello There
```

# 居中(宽度，填充字符)

`center`返回在字符串中央格式化的字符串。可以用`fillchar`指定填充。

```
a = "hey" 
b = a.center(12, "-")
print(b)
```

这输出

```
----hey-----
```

因为`“hey”`是三个字母，即使指定的 with 是 12，最大字符`center()`可以容纳的是 11 (3 代表`“hey”`，每边四个破折号)。

# 复制()

说你有一个单子`l1 = [1,2,3,4,5]`。我们想要创建一个列表的副本，`l2`，所以你分配`l2` = `l1`。

但是当我们对`l2`做任何事情的时候，比如说`l2.pop(2)`，`l1`也会受到影响，甚至在无意改变`l1`的时候。

```
l1 = [1,2,3,4,5]
l2 = l1
l2.pop(2)
print('l2',l2)
print('l1',l1)
```

输出:

```
l2 [1, 2, 4, 5] 
l1 [1, 2, 4, 5]
```

若要复制列表而不影响前一个列表，请使用 copy()。这将创建一个相同的副本，可以在不影响其父列表的情况下进行更改。

```
l1 = [1,2,3,4,5]
l2 = l1.copy()
l2.pop(2)
print('l2',l2)
print('l1',l1)
```

输出:

```
l2 [1, 2, 4, 5] 
l1 [1, 2, 3, 4, 5]
```

# encode(编码='utf8 '，错误='strict ')

这个函数以 bytes 对象的形式返回字符串的编码版本。默认编码是`utf-8`。这允许编码之间的快速转换，当某些函数只能接受某些编码时，这很有帮助。

`errors`可以给定设置不同的错误处理方案。`errors`的可能值是:

*   `strict`(编码错误引发一个`UnicodeError`)
*   `ignore`
*   `replace`
*   `xmlcharrefreplace`
*   `backslashreplace`
*   通过`codecs.register_error()`注册的任何其他名称

# expandtabs(tabsize=8)

该函数返回一个字符串，其中所有的制表符被一个或多个空格替换，这取决于当前列和制表符的大小。

```
a = "1\t2\t3"
print(a)
print(a.expandtabs())
print(a.expandtabs(tabsize=12))
print(a.expandtabs(tabsize=2))
```

输出

```
1 2 3 
1       2       3 
1           2           3 
1 2 3
```

# 分区(分割器)

如果`.split(splitter)`忽略了分割器，只返回分割器两边的字符串，那就试试`partition(splitter)`。

```
print('abcdefg.hijklmnop'.partition('.'))
```

输出

```
['abcdefg','.','hijklmnop']
```

# 清除()

`clear()`函数是一种清空列表内容的干净方式。

```
a = ['a','b','c','d']
print(a)
a.clear()
print(a)
```

输出将是

```
['a','b','c','d']
[]
```

在本文中，我们讨论了各种方便的内置方法，从字符串处理(`r`、`expandtabs`、`title`等等)。)到列表(`copy`、`partition`、`clear`)，等等。有了这些函数，您将能够清理和增强您的代码。

感谢阅读！