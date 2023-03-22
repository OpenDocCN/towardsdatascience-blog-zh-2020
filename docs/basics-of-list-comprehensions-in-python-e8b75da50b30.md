# Python 中列表理解的基础

> 原文：<https://towardsdatascience.com/basics-of-list-comprehensions-in-python-e8b75da50b30?source=collection_archive---------40----------------------->

## 技术的

## 理解如何在 python 编程语言中比循环标准更快地创建序列类型列表

![](img/29e38ebb9ed3867db261ccc102fba581.png)

照片由[克莱门特·H](https://unsplash.com/@clemhlrdt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 介绍

本文介绍了列表理解的基础 Python 语言中一种常见的编程语法风格。

尽管列表理解在 python 中很常见，但是来自松散类型语言(如 JavaScript)的程序员可能不熟悉列表理解。

列表理解背后的直觉很容易理解，它们的实现方法也是如此。

**所以本文旨在做以下几点:**

*   呈现列表理解的直觉
*   用示例代码比较创建列表的标准方法和使用列表理解的方法
*   利用列表理解的好处
*   围绕列表理解主题的常见关键术语的定义。

# 列表

列表是 Python 编程语言中常见的序列类型之一。还有其他常见的序列类型，如元组、集合或字节数组。

```
example_list = ['deeplearning', 'machinelearning', 'python', 'datascience']
print(example_list)
print(type(example_list))>> ['deeplearning', 'machinelearning', 'python', 'datascience']
>> <class 'list'>
```

> 给来自 JavaScript 等语言的人的提示。列表和数组可能看起来相似，但它们在 Python 中并不相同。Python 中经常使用列表。下面就[简要解释一下](https://www.pythoncentral.io/the-difference-between-a-list-and-an-array/)它们的区别

python 编程语言中的列表被归类为序列类型。序列类型通常能够以结构化的方式存储一个或多个值。

列表是序列类型，可以分为容器序列和可变序列。

*   **容器序列**中存储了对象的引用，而不是对象本身。
*   **可变序列**允许修改包含在其中的对象。

# 列出理解

我们对列表有了一个了解，现在可以深入到列表理解这个话题了。

与使用 for 循环的传统方式相比，列表理解是在 python 中构造列表的一种有效方式。

# 密码

下面的代码片段描述了构建列表的标准方式

下面的代码没有多大意义，写出来只是为了说明。

```
name = 'James'
name_and_number = []for i in range(5):
   name_and_number.append(name + ' ' + str(i))
print(name_and_number)>> ['James 0', 'James 1', 'James 2', 'James 3', 'James 4']
```

上面的代码简单地将数字添加到名称中，并修改在变量' *name_and_number* 中初始化的空列表。

现在让我们看看如何用列表理解生成相同的列表。

```
name = 'James'
name_and_number = [name + ' ' + str(i) for i in range(5)]
print(name_and_number)
>>['James 0', 'James 1', 'James 2', 'James 3', 'James 4']
```

仅此而已，三行代码实现了相同的结果，如果不包括 print 语句，则需要两行。

列表理解的好处是显而易见的，我将在下面的部分总结它们。

在我们继续之前，读者应该注意到,“ *for* ”循环允许对列表中的每个元素执行更多的操作，而列表理解只是为构造列表而设计的。

您也可以在列表理解中放置条件语句。

```
name = 'James'
name_and_number = [name + ' ' + str(i) for i in range(5) if i%2==0 ]
print(name_and_number)
>> ['James 0', 'James 2', 'James 4']
```

# 利益

*   **简洁**:当使用普通的 for 循环时，列表可以在一行中生成，而不是几行
*   **可读性**:列表理解有一个易于阅读的语法，可以很容易地传达实现的目的。

列表理解是一个有用且容易理解和学习的概念，请随意使用你的例子。

## 如果你喜欢这篇文章，这里有一些我写的。

[](/algorithm-bias-in-artificial-intelligence-needs-to-be-discussed-and-addressed-8d369d675a70) [## 人工智能中的算法偏差需要讨论(和解决)

### 你在这件事上有责任…

towardsdatascience.com](/algorithm-bias-in-artificial-intelligence-needs-to-be-discussed-and-addressed-8d369d675a70) [](/why-machine-learning-engineers-or-data-scientists-are-not-the-stars-of-the-show-d91ec9c5256b) [## 为什么机器学习工程师(或数据科学家)不是这场秀的主角

### 但是我们仍然是成功的员工队伍中不可或缺的一部分

towardsdatascience.com](/why-machine-learning-engineers-or-data-scientists-are-not-the-stars-of-the-show-d91ec9c5256b)