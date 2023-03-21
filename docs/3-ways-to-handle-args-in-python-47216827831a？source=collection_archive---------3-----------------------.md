# 在 Python 中处理参数的 3 种方法

> 原文：<https://towardsdatascience.com/3-ways-to-handle-args-in-python-47216827831a?source=collection_archive---------3----------------------->

## 直奔主题的指南

![](img/9eaf4377b50f813e83d5065ed6042c90.png)

照片由 [Maja Petric](https://unsplash.com/@majapetric?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## #1 系统模块

Python 中的`sys`模块具有`argv`功能。当通过终端触发执行时，该功能返回提供给`main.py`的所有命令行参数的列表。除了其他参数，返回列表中的第一个元素是`main.py`的路径。

考虑下面这个`main.py`的例子

```
import syslist_of_arguments = sys.argvprint(list_of_args[0]) 
print(list_of_args[1]) 
print(list_of_args[2]) 
print(list_of_args[3])
```

触发`main.py`为:

```
python main.py first_arg "[second_arg]" "{\"arg\": 3}"
```

会返回:

```
test.py
first_arg
[second_arg]
{"arg": 3}
```

## #2 带有一个大参数的 sys 模块

这是为 Python 代码提供参数的一种简单而强大的方法。您提供了一个单一的“大”参数，而不是提供许多按空格分开的参数。这个大参数是一个字符串字典，其中 dict-keys 表示参数名，dict-values 表示它们对应的值。

因为这个字典参数在 Python 中被表示为一个字符串，所以您应该将其转换为字典。这可以通过使用`ast.literal_eval`或`json.loads`功能来完成。需要相应地导入`ast`或`json`模块。

考虑下面这个`main.py`的例子:

```
import sys
import ast

raw_arguments = sys.argv[1]

print(raw_arguments)
arguments = ast.literal_eval(raw_arguments)

print(arguments['name']) # John
print(arguments['surname']) # Doe
print(arguments['age']) # 22
```

触发`main.py`为:

```
python main.py "{\"name\": \"John\", \"surname\": \"Doe\", \"age\": 22}"
```

会返回:

```
{"name": "John", "surname": "Doe", "age": 22}
John
Doe
22
```

## # 3 arg parse 模块

如果您想为您的应用程序提供一个合适的命令行界面，那么`argparse`就是您需要的模块。这是一个完整的模块，它提供了开箱即用的参数解析、帮助消息以及在参数被误用时自动抛出错误。这个模块预装在 Python 中。

为了充分利用 **argparse 提供的功能，**需要一些时间来掌握。作为开始，考虑以下`main.py`的例子:

```
import argparse

parser = argparse.ArgumentParser(description='Personal information')parser.add_argument('--name', dest='name', type=str, help='Name of the candidate')parser.add_argument('--surname', dest='surname', type=str, help='Surname of the candidate')parser.add_argument('--age', dest='age', type=int, help='Age of the candidate')

args = parser.parse_args()print(args.name)
print(args.surname)
print(args.age)
```

在初始化一个`ArgumentParses`的对象后，我们使用`add_argument`函数添加所有期望的参数。该函数接收多个参数，其中包括*参数名称*(例如`--name`)*目标变量*、*预期数据类型、*、要显示的*帮助消息*等。

触发`main.py`为:

```
python main.py --name John --surname Doe --age 22
```

会回来

```
John
Doe
22
```

要找到关于这个模块的更多信息，请看一下 [argparse 文档](https://docs.python.org/2/library/argparse.html)。

# 结论

很多时候，您需要向 Python 脚本传递参数。Python 通过`sys`模块提供对这些参数的访问。您可以直接访问`argv`功能并处理您自己的参数解析，或者您可以使用其他模块作为`argparse`来为您完成这项工作。

在我的日常编码生活中，我使用小脚本中的 *sys 和*arg parse 时，我是它们的唯一用户/维护者。

编码快乐！

[](/150-concepts-heard-in-data-engineering-a2e3a99212ed) [## 数据工程中听到的 150 多个概念

### 面向数据工程师的综合词汇表

towardsdatascience.com](/150-concepts-heard-in-data-engineering-a2e3a99212ed) [](/how-to-generate-ms-word-tables-with-python-6ca584df350e) [## 如何用 Python 生成 MS Word 表格

### 30 秒内自动生成报告

towardsdatascience.com](/how-to-generate-ms-word-tables-with-python-6ca584df350e) [](/5-scenarios-where-beginners-usually-misuse-python-98bac34e6978) [## 新手通常误用 Python 的 5 种场景

### 更好地利用 Python

towardsdatascience.com](/5-scenarios-where-beginners-usually-misuse-python-98bac34e6978)