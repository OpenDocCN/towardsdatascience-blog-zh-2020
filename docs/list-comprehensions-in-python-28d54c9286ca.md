# Python 中的列表理解

> 原文：<https://towardsdatascience.com/list-comprehensions-in-python-28d54c9286ca?source=collection_archive---------23----------------------->

## 用 Python 创建列表的更优雅、更简洁的方式

![](img/41f41135d5c4e032d83f3ada45af32ad.png)

凯文·Ku 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 创建列表

假设我们想用 python 从另一个可迭代对象或序列创建一个列表，比如另一个列表。例如，我们有一个数字列表，我们希望创建一个新列表，其中包含第一个列表中数字的立方。有多种方法可以完成这项任务。也许最基本的方法是使用 for 循环，如下所示:

*在上面的代码中，我们使用了一个 for 循环来遍历 num_list，并将每个数字的立方添加到 cube_list 中。*

事实证明，完成同样的任务有一个更简单的方法，那就是使用列表理解。

# 列出理解

列表理解允许我们以一种非常简洁的方式从其他序列中创建列表。我们通常使用列表理解来循环遍历另一个序列，比如一个列表，或者添加满足特定条件的元素，或者添加应用于序列中每个元素的操作的结果。

[](/looping-in-python-5289a99a116e) [## Python 中的循环

### 如何在 python 中使用 enumerate()函数

towardsdatascience.com](/looping-in-python-5289a99a116e) 

## 写清单理解

列表理解由括号组成，括号包含一个表达式，后跟一个 for 循环，以及零个或多个 for 或 if 子句。然后，这个列表理解创建一个列表，其中包含在其后的 for 和 if 子句的上下文中计算的表达式。

让我们从只包含一个表达式和一个 for 循环的基本列表理解开始:

> 【<expression>用于<iterable or="" sequence="">中的<name></name></iterable></expression>

例如，让我们看看如何使用列表理解来创建上面的 cube_list，或包含另一个列表的多维数据集的列表:

**那么这行代码是怎么回事呢？**

```
cube_list = [num**3 for num in num_list]
```

首先，我们注意到我们有包含表达式的括号， **num**3** ，后面是 for 循环，**表示 num_list** 中的 num。在 for 循环中， **num** 是我们给在 **num_list** 中循环的元素的参数名，就像上面的原始 for 循环一样。我们基本上是说，从我们的 **num_list** 中取出 **num** (或当前元素)，对其进行立方，然后将该操作的结果添加到我们的列表中，类似于 cube _ list . append(**num * * 3**)。因此，我们将表达式 **num**3** 的输出添加到我们正在创建的列表中，因为它在 for 循环中迭代。

> 注意:列表理解的行为与 python 中内置的 [map](/using-map-and-filter-in-python-ffdfa8b97520) 函数非常相似。

**关于表达式的注释:**

> 列表理解中的表达式也可以包括函数。例如，如果我们想要创建一个包含字符串列表的相应长度的列表，我们可以通过下面的列表理解来实现:

```
list_of_strings = [‘hello’, ‘my’, ‘name’, ‘a’]len_of_strings = [len(word) for word in list_of_strings]print(len_of_strings) # output is [5,2,4,1]
```

注意我们是如何使用内置的 len 函数作为 list comprehension 中表达式的一部分的。

[](/four-things-you-should-know-in-python-62a0519ca20e) [## 在 Python 中你应该知道的四件事

### 学习如何在 Python 中分割序列、循环序列、压缩/解压缩可重复项，等等

towardsdatascience.com](/four-things-you-should-know-in-python-62a0519ca20e) 

## 用条件列出理解

我们还可以在 for 循环后使用 if 语句将条件添加到我们的列表理解中:

> 【<expression>为<iterable or="" sequence="">中的<name>如果</name></iterable></expression>

例如，假设我们想从 num_list 创建一个新的列表，它只包含奇数的立方。好吧，我们通过下面的列表理解来实现:

```
cubes_of_odds = [num**3 for num in num_list if num%2 != 0]print(cubes_of_odds) # output is [1,27,125]
```

注意我们是如何在末尾添加 if 语句的。所以***num * * 3****只会添加到我们的****cubes _ of _ odds****列表中，如果当前元素或者****num****是奇数的话使用模运算符。当 num 除以 2 时，模运算符返回余数，如果****num****是偶数，则余数等于零。*

**想要多个条件怎么办？**

我们不仅可以添加 if 语句，还可以使用以下格式添加 else 语句:

> 【<expression>如果<condition>否则<expression>为<iterable or="" sequence="">中的<name></name></iterable></expression></condition></expression>

例如，假设我们想遍历 num_list，用奇数的立方和偶数的平方创建一个新的列表。我们可以用下面的代码做到这一点:

```
cubes_and_squares = [num**3 if num%2!=0 else num**2 for num in num_list]print(cubes_and_squares) # output is [1,4,27,16,125]
```

*所以当我们循环通过* ***num_list*** *，if* ***num%2！对于特定的****num****或元素，****num * * 3****为真。如果* ***num%2！=0*** *不成立，则将使用****num * * 2****表达式来代替元素。***

## 嵌套列表理解

一个列表理解中的表达式也可以是另一个列表理解。例如，假设我们想要制作以下矩阵(或二维数组):

```
matrix = [[1, 2, 3, 4],
          [1, 2, 3, 4],
          [1, 2, 3, 4],
          [1, 2, 3, 4]]
```

请注意，它由四行组成，每行包含数字 1 到 4。换句话说，它是一个包含四个相同列表的列表，每个列表都是[1，2，3，4]。

除了外部 for 循环之外，我们还可以使用列表理解来创建这个矩阵，如下所示:

```
matrix = []for y in range(1,5):
    matrix.append([x for x in range(1,5)])
```

for 循环有四次迭代，在每次迭代中用 list comprehension 创建一个 list:**【x for x in range(1，5)】**。这个列表理解创建了一个[1，2，3，4]的列表。因此，我们正在创建一个包含四个列表[1，2，3，4]的列表。

上面的代码等价于下面的嵌套列表理解:

```
matrix = [[x for x in range(1,5)] for y in range(1,5)]
```

因此，我们正在执行初始表达式，这是一个列表理解， **[x for x in range(1，5)]，**创建了一个[1，2，3，4]列表。该表达式在第二个 for 循环的每次迭代中执行，每次创建一个[1，2，3，4]列表并将其添加到外部列表中。

[](/using-reduce-in-python-a9c2f0dede54) [## 在 Python 中使用 Reduce

### 如何使用 python 中的 reduce 函数

towardsdatascience.com](/using-reduce-in-python-a9c2f0dede54) 

## **列表理解与映射和过滤功能**

列表理解用于从其他序列中创建列表，或者通过对元素应用某种操作，或者通过过滤元素，或者两者的某种组合。换句话说，列表理解可以具有与内置映射和过滤功能相同的功能。应用于每个元素的操作类似于 map 函数，如果您添加一个条件，将元素添加到您的列表理解中的列表，这类似于 filter 函数。此外，添加到列表理解开头的表达式类似于可以在 map 和 filter 函数中使用的 lambda 表达式。

例如，这个列表理解等同于随后的映射和过滤器(带有 lambda 表达式)示例:

```
[x**2 for x in range(10) if x%2==0]
```

*此列表理解为从 0 到 9 的元素的平方相加，仅当元素是偶数时。*

```
list(map(lambda x:x**2, filter(lambda x:x%2==0, range(10))))
```

*传递给 map 函数的函数是一个 lambda 表达式，它接受输入 x 并返回它的平方。传递给 map 函数的列表是一个经过过滤的列表，包含从 0 到 9 的偶数元素。*

> ***都创建如下列表:[0，4，16，36，64]***

*关于地图和过滤功能的更多信息:*

[](/using-map-and-filter-in-python-ffdfa8b97520) [## 在 Python 中使用地图和过滤器

### 如何使用 python 中内置的地图和过滤功能

towardsdatascience.com](/using-map-and-filter-in-python-ffdfa8b97520) 

*关于 lambda 表达式的更多信息:*

[](/writing-lambda-expressions-in-python-745288614a39) [## 用 Python 编写 lambda 表达式

### 如何用 python 写 lambda 表达式！

towardsdatascience.com](/writing-lambda-expressions-in-python-745288614a39) 

如果你喜欢阅读这样的故事，并想支持我成为一名作家，考虑注册成为一名媒体成员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你用我的 [*链接*](https://lmatalka90.medium.com/membership) *注册，我会赚一小笔佣金。*

[](https://lmatalka90.medium.com/membership) [## 通过我的推荐链接加入媒体——卢艾·马塔尔卡

### 阅读卢艾·马塔尔卡的每一个故事(以及媒体上成千上万的其他作家)。您的会员费直接支持…

lmatalka90.medium.com](https://lmatalka90.medium.com/membership) 

## 结论

在本教程中，我们学习了如何使用列表理解从其他列表或序列中生成列表。我们看到它们可以包含零个或多个条件。然后我们看到了如何使用嵌套列表理解来创建一个二维列表。最后，我们将列表理解与 python 中的 map 和 filter 函数进行了比较。