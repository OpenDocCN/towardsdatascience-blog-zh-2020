# 下一次面试前要解决的 5 组 Python 算法

> 原文：<https://towardsdatascience.com/5-pythons-sets-problems-to-solve-before-your-coding-interview-41bb1d14ac25?source=collection_archive---------18----------------------->

## 集合是许多 Python 访谈中反复出现的话题，并且经常提供高效求解算法的捷径

![](img/d3ce97d6e6410726a736c7657c17ac80.png)

[JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

*更新:你们很多人联系我要有价值的资源* ***钉 Python 编码访谈*** *。下面我分享 4 个我强烈推荐在练习完本帖的算法后坚持锻炼的课程/平台:*

*   [***leet code In Python:50 算法编码面试问题***](https://click.linksynergy.com/deeplink?id=533LxfDBSaM&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fleetcode-in-python-50-algorithms-coding-interview-questions%2F)**→最适合涉及算法的编码回合！**
*   *[***StrataScratch:pt hon&SQL 面试编码问题***](https://stratascratch.com/?via=antonello)***→****最佳平台我找到了额外的 Python & SQL 编码演练！**
*   *[**用 Python 练习编码面试题(60+题)**](https://datacamp.pxf.io/DV3mMd) **→** *列表、数组、集合、字典、map()、filter()、reduce()、iterable 对象**
*   *[**Python 数据结构&算法(纳米级)**](https://imp.i115008.net/kjjdnv) **→** *优质课程如果你有更多的时间。**

*这篇文章包括代销商链接，如果你购买的话，我可以在不增加你额外费用的情况下赚取一小笔佣金。*

*在学校学习数学的时候，几乎没有人真正喜欢它们，因为它们看起来很无聊，但是如果你正在准备 Python 编码屏幕或白板面试，并且你希望搞定它，你现在真的必须学习集合运算符和方法。集合是你需要在你的武器库中拥有的一种武器，因为常见的用途包括**删除重复**、**计算集合上的数学运算**(像并集、交集、差和对称差)和**成员测试**。*

***集合也不是我在 Python 之旅中学到的第一件事。如果你像我一样是一个动手型的人，你可能从一些真正的项目开始，这意味着最初，你几乎肯定会使用高级包而不是内置模块。然而，当我开始解决算法以面试大型科技公司时，我意识到集合在许多不同难度的挑战中反复出现，并经常提供解决它们的捷径。***

> ***“我意识到集合在许多不同难度的挑战中反复出现，并经常提供解决它们的捷径”***

***在最近的一篇文章中，**我展示并分享了我在**真实采访中遇到的一些 Python 算法**的解决方案** ( *看看吧！*):***

***[](/10-algorithms-to-solve-before-your-python-coding-interview-feb74fb9bc27) [## Python 编码面试前要解决的 10 个算法

### 在这篇文章中，我介绍并分享了 FAANG 中经常出现的一些基本算法的解决方案

towardsdatascience.com](/10-algorithms-to-solve-before-your-python-coding-interview-feb74fb9bc27) 

在这篇文章中，我将采用类似的方法，将理论与算法挑战结合起来。大家一起来解决吧！

## 1)删除重复项

在 Python 中，可以通过两种方式创建集合:

*   使用内置的`set()`功能。例如，如果一个字符串`s`被传递给`set()`，那么`set(s)`在该字符串中生成一个字符列表:

```
set(‘algorithm’)
Output: {'a', 'g', 'h', 'i', 'l', 'm', 'o', 'r', 't'} #<- unordered
```

*   使用花括号`{}`。使用这种方法，即使是可迭代的对象也可以完整地放入集合中:

```
set1 = {‘algorithm’}
print(set1)
Output: {'algorithm'}set2 = {'algorithm', 'interview'}
print(set2)
Output: {'algorithm', 'interview'}
```

更详细地说，内置的`**set()**`数据类型具有以下特点:

*   就是 ***无序、*** ***可迭代*** 和 ***可变***；
*   它的元素可以是不同类型的对象(*整数*、*浮点*、*元组*、*字符串*等)。)但是这些元素必须是不可变的类型(列表和字典被排除在外)；
*   它只能包含**特有的元素。这意味着在使用集合时会自动删除重复的元素。**

**最后一个特征是关键的，因为它意味着我们可以使用`set()`从(例如)数组中删除重复项。**

## ****问题#1:包含重复→简单****

```
**Output:
True**
```

**多亏了 sets，这个问题可以通过一行程序得到解决。事实上，当一个数组被传递给`set()`函数时，所有重复的元素都被删除，这意味着集合的长度将等于原始数组的长度，前提是数组首先包含不同的元素。**

## ****2)计算集合上的数学运算****

**因为集合是**无序集合**(它们不记录*元素位置*或*插入顺序*，许多可以在其他 Python 数据类型上执行的操作(如*索引*和*切片*)都不被集合支持。**

**尽管如此，Python 为我们提供了大量的**操作符**和**方法**来复制为数学集合定义的操作。例如，假设我们有两个集合(`s1 and s2`)，都包含字符串，我们想要合并它们。在这种情况下，我们可以使用 union 操作符(`|`)或 union 方法(`s1.union(s2)`)来执行相同的操作:**

```
**#Create 2 sets including strings:
s1 = {‘python’, ‘interview’, ‘practice’}
s2 = {‘algorithm’, ‘coding’, ‘theory’}#Using the operator:
print(s1|s2)# Using the method:
print(s1.union(s2))Output:
{'python', 'algorithm', 'interview', 'practice', 'coding', 'theory'}
{'python', 'algorithm', 'interview', 'practice', 'coding', 'theory'}**
```

**正如你所看到的，看起来*集合操作符和方法的行为与* *是一样的，它们可以互换使用*，但是它们之间有一点点不同。当使用运算符时，两个操作数都必须是 set，而方法将接受任何参数，先将其转换为 set，然后执行运算。此外，集合上的每一个数学运算都有一个方法，但对于运算符来说就不一样了。**

> **使用运算符时，两个操作数都必须是集合，而方法接受任何参数，先将其转换为集合，然后执行运算**

**让我们通过解决以下三个问题来发现一些更常见的集合运算符和方法:**

## **问题#2:两个数组的交集→简单**

```
**Output:
[5, 7]
[5, 7]**
```

**如上所示，要返回两个数组共有的元素，您可以将`nums1`和`nums2`都转换为集合，然后用`&`运算符找到它们的交集，或者只将`nums1`转换为集合，并使用`nums2`作为`.intersection()`方法的参数(*这将在执行交集*之前自动将 `nums2` *从数组转换为集合)。第二种方法( *solution_two* )需要更多的输入，但它更安全、更快，需要的内存也更少，因此在实际面试中解决算法时应该优先考虑。有几次，在我的解决方案中，我选择了一个运算符，然后面试官要求我谈谈等效的方法，并相应地重写解决方案…***

## *问题#3:键盘行中的单词→简单*

```
*Output:
['Type', 'Router', 'Dash', 'Top', 'Rower']*
```

*以上问题的解决方案既简洁又高效。实际上，为了检查`words`数组中的每个单词是否由美式键盘上单行的字母组成，每个单词最初都使用`set()`函数转换成一个集合。如前所述，当我们这样做时，单词被拆分，这样每个字母都成为集合中的一个元素。例如，如果我们将单词 *"Type"* 传递给*`set()`函数(并转换为小写)，我们将获得:**

```
**set1 = set(‘Type’.lower())
print(set1)
Output: {'t', 'e', 'p', 'y'}**
```

**现在我们可以利用`.difference()`方法来检查`{'t','e','p','y'}` 和三组键盘字母之间的差是否返回空集。如果返回一个空集(`if not`，这意味着这个单词是由属于一个特定行的字母组成的，然后它应该被附加到`result`。**

## **问题#4:两句话中的生僻字→简单**

```
**#Output:
['enjoy', 'love'] #<-- note how "you" and "we" are also excluded**
```

**在实际的面试中，你经常会得到两个整数数组(或者像本例中的两个句子),你被要求找出属于第一个或第二个项目组的所有元素，而不是两个都属于。这种问题可以通过使用`.symmetric_difference()`方法或等效的`^`操作符来解决。**

**然而，如果一方面这种算法通过提供只包含空格分隔/小写单词的句子使您的生活更容易，另一方面，它要求每个单词也是唯一的，这意味着您需要找到一种方法来计算单词(`collections.Counter()`)，并验证它们“*在每个句子中只出现一次*。**

## **3)执行成员测试**

**通常，为了解决一个算法，你需要*确定两个或多个数组是否有任何共同的元素*或者*一个数组是否是另一个数组的子集*。幸运的是，Python 的集合提供了许多运算符来执行成员资格测试，但是我在实际采访中使用最多的是:**

*   **`s1.isdisjoint(s2)`:如果`s1`和`s2`没有共同的元素，这个操作返回`True`。没有对应于此方法的运算符。**
*   **`s1.issubset(s2)`或`s1<=s2`:如果`s1`是`s2`的子集，则该操作返回`True`。注意，因为集合`s1`被认为是自身的集合，所以要检查一个集合是否是另一个集合的真子集，最好使用`s1<s2`。这样可以验证`s1`的每个元素都在`s2`中，但是两个集合不相等。**

**在本文的最后一个问题中，您将能够应用上述方法之一。请记住，下面的算法比其他四个算法(级别:*中等)*)更具挑战性，因此，请花些时间找到合适的策略来解决它。**

## **问题 5:最喜欢的公司列表→中等**

```
**Output:
[0, 1, 4]**
```

**在上面的解决方案中，`enumerate()`使用了两次:第一次循环返回`favcomp`中包含的每个子列表，而第二次循环检查第一次循环获得的列表是否是`favcomp`中任何其他列表的子集。当 set 方法返回`True`时，`flag`(最初设置为`True`)切换到`False`，从而最终只有与`flag`保存有`True`值的列表相匹配的索引被分配给`result`。这是一个 BF 解决方案，所以请随意找到一个更有效的方法，并在评论中分享。**

## ****结论****

**在本文中，我提出了 5 个 Python 编码挑战，并使用集合操作符和方法解决了它们。当算法需要删除重复项、执行常见的数学运算或验证成员资格时，Set 方法是非常强大的工具。请放心，随着您不断练习编码，循环模式将变得更加明显，您将学会动态应用集合运算！**

**注意*这篇文章中的练习(以及它们的解决方案)是对 Leetcode 上的问题的重新诠释。我远非该领域的专家，因此我提出的解决方案只是指示性的。***

**如果你喜欢这篇文章，请留下评论。祝你面试准备顺利！**

## **资源:**

**[套*在 Python 教程中由真正的 Python* 套](https://realpython.com/python-sets/)**

**[Python 集合编程教程](https://www.programiz.com/python-programming/set)**

**[Leetcode 算法列表](https://leetcode.com/problemset/all/)**

## ****您可能还喜欢:****

**[](/8-popular-sql-window-functions-replicated-in-python-e17e6b34d5d7) [## Python 中复制的 8 个流行的 SQL 窗口函数

### 关于如何利用业务分析中的 Pandas 高效复制最常用的 SQL 窗口的教程…

towardsdatascience.com](/8-popular-sql-window-functions-replicated-in-python-e17e6b34d5d7) [](https://levelup.gitconnected.com/15-git-commands-you-should-learn-before-your-very-first-project-f8eebb8dc6e9) [## 在你开始第一个项目之前，要掌握 15 个 Git 命令

### 您需要掌握的最后一个 Git 教程是命令行版本控制。

levelup.gitconnected.com](https://levelup.gitconnected.com/15-git-commands-you-should-learn-before-your-very-first-project-f8eebb8dc6e9)*****