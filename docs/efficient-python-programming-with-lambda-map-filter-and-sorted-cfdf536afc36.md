# 使用 Lambda、Map、Filter 和 Sorted 进行高效的 Python 编程

> 原文：<https://towardsdatascience.com/efficient-python-programming-with-lambda-map-filter-and-sorted-cfdf536afc36?source=collection_archive---------25----------------------->

![](img/ce106b1b949e077f4d1d6982c9a11247.png)

照片由[普里斯·安莉](https://unsplash.com/@prissenri?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

## 使用数字、字符串和字典列表的快乐编程

一些内置函数，比如 lambda 函数，可以以更简洁的方式修改列表。我喜欢使用 lambdas，因为我可以用更少的代码编写程序，并且仍然清晰易懂。另外，它不会使你的程序变慢。

下面是语法:

```
lambda x,…. : expression
```

我在 x 后面加了几个点来象征更多的争论。

> Lambdas 可以接受任意数量的参数，但只能接受一个表达式。

Lambda 用于匿名函数，即没有名称的函数。我们使用“def”来定义 python 函数。但是当我们使用 lambda 时，我们可以不使用' def '来创建函数。如果要保存，一个变量就够了。

让我们看一些例子。这个函数有一个参数 x，返回 x 的平方。

```
def squared(x):
     return x*x
```

现在用 lambda 写同样的函数。

```
squared = lambda x: x*x
```

两个平方函数的工作方式完全相同。

现在来看一个简单的加法函数，它接受两个参数 x 和 y，并返回它们的和:

```
add = lambda x, y: x+y
```

使用 lambda 不是更简洁吗？它节省了额外的一行。

调用函数“add”并向其传递两个整数。

```
add(4, 5)
```

输出:

```
9
```

在处理列表时，它看起来会更有效率！

# 地图

Python 中的 Map 函数接受列表和函数，并返回由函数修改的列表。让我们来看一些例子。这里有三个列表 a，b，c。

```
a = [3,4,5,2,7,8]
b = [7,9,2,4,5,1]
c = [5,7,3,4,5,9]
```

在列表 a 和 b 上调用 add 函数:

```
add(a,b)
```

输出:

```
[3, 4, 5, 2, 7, 8, 7, 9, 2, 4, 5, 1]
```

这没有给出元素方面的增加。当你简单地添加到列表中时，它们只是连接在一起。它只是创建了一个更大的列表，包含了两个列表中的所有元素

通常，我们对两个相同大小的列表进行元素相加的方法是使用“for 循环”,如下所示:

```
res = []
for i in range(0, len(a)):
    res.append(a[i]+ b[i])res
```

输出:

```
[10, 13, 7, 6, 12, 9]
```

我不得不写三行代码来添加两个列表。现在，让我们使用“map”用一行代码来完成它。

使用“map ”,您可以调用我们之前在列表 a 和 b 上定义的 add 函数来获得元素相加。它的工作方式完全类似于“for 循环”,并给出相同的输出。

```
list(map(add, a, b))
```

输出:

```
[10, 13, 7, 6, 12, 9]
```

是不是简洁优雅了很多？

让我们总结一下我们所做的。我们首先定义了一个名为“add”的函数，然后使用一个“map”函数来获得两个列表的元素范围的相加。我们能做得更好吗？

是啊！我们可以在同一行代码中调用' add '函数。

```
list(map(lambda x, y: x+y, a, b))
```

输出:

```
[10, 13, 7, 6, 12, 9]
```

我们在这里做的是，

1.  我们用 lambda 定义了一个函数，它有两个参数 x 和 y。
2.  向函数传递了两个列表 a 和 b。
3.  使用地图，因为 a 和 b 是列表。我们想要 a 和 b 的元素相加。

事实上，您可以添加所有三个列表或任意数量的参数:

```
list(map(lambda x, y, z: x+y+z, a, b, c))
```

输出:

```
[15, 20, 10, 10, 17, 18]
```

如果你有一个包含三个参数的公式，而不是简单的加法:

```
list(map(lambda x, y, z: 2*x + 2.5*y + z, a, b, c))
```

输出:

```
[28.5, 37.5, 18.0, 18.0, 31.5, 27.5]
```

# 过滤器

过滤器将列表作为参数，将函数作为表达式。它在通过函数过滤掉元素后返回修改后的列表。这里有一些例子。

只返回列表中大于 5 的数字。在此之前，制作一个新列表 c:

```
c = [4,2,9,6,10,4,1]
list(filter(lambda x: x > 5, c))
```

输出:

```
[9, 6, 10]
```

在这里，我定义了返回大于 5 的数字的函数，并将列表 c 作为参数。看，它返回了所有大于 5 的 c 元素。

让我们来看另一个例子。只返回列表中的偶数。这一次，我将传递列表 a。

```
a = [3,4,5,2,7,8]
list(filter(lambda x: x % 2==0, a))
```

输出:

```
[4, 2, 8]
```

我们也可以在字符串上使用 lambda 和 filter。

上面所有的例子都有数字。接下来的几个例子是关于字符串的。这是一份名单:

```
names = [‘Abram’, ‘Arib’, ‘Bob’, ‘Shawn’, ‘Aria’, ‘Cicilia’, ‘John’, ‘Reema’, ‘Alice’, ‘Craig’, ‘Aaron’, ‘Simi’]
```

返回所有以 will 'A '开头的名字。

```
list(filter(lambda x: x[0]==’A’, names))
```

输出:

```
['Abram', 'Arib', 'Aria', 'Alice', 'Aaron']
```

# 分类的

sorted 函数是一种非常简单、容易的按字母排序数字或字符串的方法。以下是一些例子:

按名字的第一个字母对上面列表中的名字进行排序。

```
sorted(names, key=lambda x: x[0])
```

输出:

```
[‘Abram’, ‘Arib’, ‘Aria’, ‘Alice’, ‘Aaron’, ‘Bob’, ‘Cicilia’, ‘Craig’, ‘John’, ‘Reema’, ‘Shawn’, ‘Simi’]
```

太棒了！但是一个更简单的选择是:

```
sorted(names)
```

输出:

```
[‘Aaron’, ‘Abram’, ‘Alice’, ‘Aria’, ‘Arib’, ‘Bob’, ‘Cicilia’, ‘Craig’, ‘John’, ‘Reema’, ‘Shawn’, ‘Simi’]
```

这样，名字就按字母顺序排列了。同样的事情也适用于列表。

```
sorted(c)
```

输出:

```
[1, 2, 4, 4, 6, 9, 10]
```

# λ、映射、过滤和用字典排序

使用 lambda、map、filter 和 sorted 来处理字典要简单和高效得多。

这是一个有四本字典的列表。每本词典都包括一个人的名字和他或她的年龄。

```
dict_a = [{'name': 'John', 'age': 12},{'name': 'Sonia', 'age': 10},{'name: 'Steven', 'age': 13},{'name': 'Natasha', 'age': 9}]
```

仅返回上面列表中的姓名列表:

```
list(map(lambda x: x['name'], dict_a))
```

输出:

```
['John', 'Sonia', 'Steven', 'Natasha']
```

同样，您可以只从 dict_a 中输出年龄:

```
list(map(lambda x: x['age'], dict_a))
```

输出:

```
[12, 10, 13, 9]
```

如果您希望姓名按字母排序或“年龄”列表按字母排序，只需在前面加一个 sorted:

```
sorted(list(map(lambda x: x['age'], dict_a)))
```

输出:

```
[9, 10, 12, 13]
```

你可能会想，把整本字典按年龄分类会更有用。在这种情况下，我们只需要使用 lambda 的密钥:

```
sorted(dict_a, key=lambda x: x['age'])
```

输出:

```
[{'name': 'Natasha', 'age': 9},{'name': 'Sonia', 'age': 10},{'name': 'John', 'age': 12},{'name': 'Steven', 'age': 13}]
```

输出最小的孩子的信息:

```
sorted(dict_a, key=lambda x: x['age'])[0]
```

输出:

```
{'name': 'Natasha', 'age': 9}
```

输出最大的孩子的信息:

```
sorted(dict_a, key=lambda x: x['age'])[-1]
```

输出:

```
{‘name’: ‘Steven’, ‘age’: 13}
```

如果我们需要列表以降序排列，而不是升序排列:

```
sorted(dict_a, key=lambda x: x['age'], reverse =True)
```

输出:

```
[{'name': 'Steven', 'age': 13},{'name': 'John', 'age': 12},{'name': 'Sonia', 'age': 10},{'name': 'Natasha', 'age': 9}]
```

或者，如果您想按字母顺序对姓名进行排序:

```
sorted(dict_a, key=lambda x: x['name'])
```

输出 10 岁以上儿童的信息:

```
list(filter(lambda x: x['age'] > 10, dict_a))
```

输出:

```
[{'name': 'John', 'age': 12},{'name': 'Steven', 'age': 13}]
```

三年后，你又回到了孩子们的年龄。所以，每个年龄加 3 就行了:

```
list(map(lambda x: x['age']+3, dict_a))Output: [15, 13, 16, 12]
```

## 结论

我希望本教程有助于提高您的 python 编程。

以下是 YouTube 上的视频教程:

欢迎在推特上关注我，喜欢我的 T2 脸书页面。

## 更多阅读:

[](/sort-and-segment-your-data-into-bins-to-get-sorted-ranges-pandas-cut-and-qcut-7785931bbfde) [## 数据宁滨与熊猫削减或 Qcut 方法

### 当你在寻找一个范围而不是一个确切的数值，一个等级而不是一个分数

towardsdatascience.com](/sort-and-segment-your-data-into-bins-to-get-sorted-ranges-pandas-cut-and-qcut-7785931bbfde) [](/a-complete-guide-to-numpy-fb9235fb3e9d) [## Numpy 完全指南

### 日常工作中需要的所有数字方法

towardsdatascience.com](/a-complete-guide-to-numpy-fb9235fb3e9d) [](/your-everyday-cheatsheet-for-pythons-matplotlib-c03345ca390d) [## Python Matplotlib 的日常备忘单

### 完整的可视化课程

towardsdatascience.com](/your-everyday-cheatsheet-for-pythons-matplotlib-c03345ca390d) [](/basic-linear-regression-algorithm-in-python-for-beginners-c519a808b5f8) [## Python 中的线性回归算法:一步一步

### 学习线性回归的概念，并使用 python 从头开始开发一个完整的线性回归算法

towardsdatascience.com](/basic-linear-regression-algorithm-in-python-for-beginners-c519a808b5f8) [](/master-pandas-groupby-for-efficient-data-summarizing-and-analysis-c6808e37c1cb) [## Pandas 的 Groupby 功能详细，可进行高效的数据汇总和分析

### 学习对数据进行分组和汇总，以使用聚合函数、数据转换、过滤、映射、应用函数…

towardsdatascience.com](/master-pandas-groupby-for-efficient-data-summarizing-and-analysis-c6808e37c1cb) [](/clear-understanding-of-a-knn-classifier-with-a-project-for-the-beginners-865f56aaf58f) [## 学习使用 Python 的 Scikit_learn 库通过项目开发 KNN 分类器

### 适合机器学习新手

towardsdatascience.com](/clear-understanding-of-a-knn-classifier-with-a-project-for-the-beginners-865f56aaf58f)