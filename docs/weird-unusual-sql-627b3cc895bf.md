# 奇怪而不寻常的 SQL

> 原文：<https://towardsdatascience.com/weird-unusual-sql-627b3cc895bf?source=collection_archive---------21----------------------->

![](img/e00200f6dc4ea32bcb33726d85b54fe8.png)

照片由[Meagan car science](https://unsplash.com/@mcarsience_photography?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/weird?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 数据工程

## 使用异常的 SQL 查询探索数据库行为

答虽然阅读数据库理论和数据库文档是最重要的，但是[通过尝试来习惯一个系统也是非常重要的](https://linktr.ee/kovid)。为了更好地掌握 SQL，从实验开始，无止境地编写愚蠢的查询，使用查询解决数学难题，使用查询创建一个随机的国际象棋游戏等等。当然，你可能没有机会在工作中做这些事情，所以抽出时间，尤其是如果你是一名数据人员的职业生涯的早期阶段。很明显，在你自己的机器上做。

了解 where 子句如何求值、子查询如何工作、排序如何工作以及空值在数据库中的行为，如果您进行实验，将会对这些内容有更深的理解。这样做也能为面试中被问到的任何棘手问题做好准备。然而，现在在面试中问一些刁钻的问题已经不太流行了。

[](/easy-fixes-for-sql-queries-ff9d8867a617) [## SQL 查询的简单修复

### 查询任何传统关系数据库的经验法则

towardsdatascience.com](/easy-fixes-for-sql-queries-ff9d8867a617) 

首先，看看这个例子——所有的 SQL 都归结于对`WHERE`子句的有效使用。理解它的行为是很重要的。

SQL 中整数是如何求值的！

当您在`WHERE`子句中使用整数时，它表示整数，但是当您在`GROUP BY`和`ORDER BY`子句中使用整数时，整数表示列在结果集中的位置号(不在表中)。这是另一个让查询写得更快的懒惰伎俩。但是，建议您不要使用`GROUP BY 1, 3 ORDER BY 1 DESC`,因为它为将来其他人编辑查询时导致错误留下了空间，特别是如果它是一个复杂的查询。一些零怎么样？

空值的行为方式！

对于大多数数据库，您会发现一篇专门的文档讨论如何处理空值。这里有一段来自 MySQL 的官方文档，讲述了使用空值的[，还有一段是关于空值](https://dev.mysql.com/doc/refman/8.0/en/working-with-null.html)的[问题的信息。阅读这些内容，您会发现不能对空值使用等式或比较运算符。](https://dev.mysql.com/doc/refman/8.0/en/problems-with-null.html)

[](/how-to-avoid-writing-sloppy-sql-43647a160025) [## 如何避免编写草率的 SQL

### 让您的 SQL 代码更易读、更容易理解

towardsdatascience.com](/how-to-avoid-writing-sloppy-sql-43647a160025) 

继续前进。理解逻辑运算符对于编写 SQL 查询非常重要。大多数 SQL 引擎支持所有主要的局部运算符，如 AND、or、NOT、XOR。尽管我们在 where 子句中没有看到它们的替代项，但我们绝对可以编写这样的查询——

逻辑运算符&波浪号的使用

我在这里有了一些有趣的发现。对整数使用波浪号将从数据库中可能的最大整数中减去该数字，并返回结果。但是如果单独选择`~`，会给你一个错误。关于 and 和& &的可互换性，建议您在整个代码库中使用 AND 而不是& &或反之亦然，以便具有良好的可读性。否则会很混乱。还有，大家最喜欢的工具搜索&替换不会正常工作。

关于逻辑运算符的另一点是，它们支持所有附带的逻辑数学。例如，`SELECT NOT NOT 1`会给我们 1。当我们做`SELECT NOT NOT 'Nice'`时会发生什么。这里有几个例子来演示。

以串行方式使用多个逻辑运算符

当与数学运算符一起使用时，逻辑运算符应该用括号分隔，否则数据库引擎将无法识别逻辑运算符，并将其视为字符串，从而引发错误。

结合使用逻辑和算术运算符

我发现这种探索数据库行为的方法非常迷人。大多数已编写的查询都使用了内置于数据库引擎中的上述原则，大多数数据库都处理空值，但是我们了解数据库实际上是如何读取和解释我们的查询的吗？我认为，为了更深入地理解数据库，人们必须经历这样的随机查询。它们就像数学难题，但显然非常简单。

几乎所有支持 SQL 的数据库都是如此。我已经在这里写下了—

[](/the-many-flavours-of-sql-7b7da5d56c1e) [## SQL 的多种风格

### 2020 年的 SQL 前景会是怎样的？它的未来会是怎样的？

towardsdatascience.com](/the-many-flavours-of-sql-7b7da5d56c1e)