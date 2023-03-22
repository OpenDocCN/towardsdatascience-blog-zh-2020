# Python 理解及其实现

> 原文：<https://towardsdatascience.com/python-comprehensions-with-implementation-852e17811bac?source=collection_archive---------53----------------------->

## 列表|词典|集合理解

![](img/e28968d2d52d778913a80b8693abc178.png)

图片作者:[Unsplash.com](https://unsplash.com/@hudsoncrafted)|[王思然](https://unsplash.com/)

**简介:**

Python 是一种流行的语言，允许程序员编写优雅、易于编写和阅读的代码，就像普通英语一样。Python 的独特之处在于不同类型的理解。

在 Python 中，有三种理解类型，即。列表、字典和集合。

在这篇博客结束时，你将理解 Python comprehensions 的全部功能，以及如何轻松地使用它的功能。

1.  **列表理解**

**List:** List 是由方括号包围的数据集合，每个元素由逗号分隔。

**列表理解:**也被方括号包围，但是它包含类似 for 循环&的表达式，或者后跟 if 子句。

**示例:**

a.创建一个 1 到 100 之间的数字平方列表。

```
# Without List comprehension
SquaresWithoutComprehension = []
for i in range(1,101):
    SquaresWithoutComprehension.append(i**2)## List Comprehension
SquaresWithComprehension = [i**2 for i in range(1,101)] 
```

b.列出对条件的理解。

```
# Suppose we have List consist movie names with released year.MoviesYear = [('Star Wars',2019),('Glass',2020),('The Upside',2018), ('The LEGO Movie 2',2020),('Cold Pursuit',2017),
         ('Hotel Mumbai',2020)]## Problem: Create List of movies released in 2020?Movies_20 = [title for (title, year) in MoviesYear if year == 2020]
```

c.列表理解-数学应用。

```
# Suppose we have List of numbers 1 to 10.Numbers = [1,2,3,4,5,6,7,8,9,10]## Problem 1: Perform scalar multiplication i.e multiply each number with 2 and store result into the List.ScalarMultiplication  = [4*X for X in Numbers]### Problem 2: Perform cartesian multiplication between List A and B.A = [1,2,3,4]
B = [10,11,12,13]CartesianProduct = [(a,b) for a in A for b in B]##Output:[(1, 10), (1, 11), (1, 12), (1, 13), (2, 10), (2, 11), (2, 12), (2, 13), (3, 10), (3, 11), (3, 12), (3, 13), (4, 10), (4, 11), (4, 12), (4, 13)]
```

**说明:**

上面会产生相同的结果，但是当我们使用列表理解时，代码行减少了，同样的操作只用一行代码就完成了。

2.**字典理解**

**字典:**字典是一个**无序、可变、索引**的集合。用花括号写的 Python 字典，它们有键和值对。

dictionary example = { " IDE ":" JupyterNotebook "，" Language": "Python "，" Version": 3}

**词典理解:**词典理解也是一个无序的、可变的、有索引的集合，其中的键值对借助表达式生成。

**词典理解示例:**

a.创建字典，其中键为字母，值为字母在句子中出现的次数(字典定义)。

```
# SentenceDictDefination = "A dictionary is a collection which is unordered, changeable and indexed. In Python written with curly brackets, and they have keys and values."## Dictionary comprehensionAlphabetDictionary = {
        key: DictDefination.count(key) for key in DictDefination
                     }
```

b.词典理解—数学应用

```
# Problem: Create dictionary where Key as number and Value as cube of key i.e. number.## Dictionary comprehensionNumSquareDictComprehension = {key:key**3 for key in range(1,10)}ORNumSquareDictComprehension = {f"The square of {key} is":key**3 for key in range(1,10)} 
```

3.**设定理解**

**集合:Python 集合中的**是**唯一的、无序的、可变的**元素集合。设置用花括号括起来的元素和用逗号分隔的元素。

SetExample = {'Python '，' Java '，' R'}

**集合理解:**类似于列表理解，但是由一组**唯一的、无序的、可变的**元素组成，其中的字典元素包含一个表达式。

**集合理解的例子:**

a.创建由 1 到 10 之间的数字组成的方块集。

```
SquareSet = {num**2 for num in range (1,11)}
```

b.创建列表中可用的唯一元素集。

```
# Suppose list having duplicate elementsCarBrands = ['BMW','Chevrolet','Bentley','BMW']

## Set comprehension only give unique elements from the above ListCarBrandDict = {Brand for Brand in CarBrands} 
```

**理解的优势:**

*   容易实现。
*   减少代码行数。
*   执行速度更快，使用的资源更少。

**理解的弊端:**

*   程序中使用更多的理解会增加代码的复杂性。
*   理解表达工作很复杂。

**结论:**

如上所述，Python 中有不同类型的理解。上面实现的是基本的例子，理解中使用的表达式可以用来调用函数。最常见的是，列表和字典理解用来简化代码。

希望，这个博客能帮助你了解一些有效且易于使用的 Python 技术。

感谢您的阅读！！