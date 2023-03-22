# Python 中的多态性:数据科学家的基础

> 原文：<https://towardsdatascience.com/polymorphism-in-python-fundamentals-for-data-scientists-9dc19071da55?source=collection_archive---------34----------------------->

## 用一个具体的例子来理解基础！

![](img/c4680a05bea0903704cfe21b4523b930.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的 [ThisisEngineering RAEng](https://unsplash.com/@thisisengineering?utm_source=medium&utm_medium=referral)

多态是面向对象编程中的另一个重要概念。

当这些类包含具有不同实现但名称相同的方法时，它们就是多态的。在这种情况下，我们可以使用这些多态类的对象，而不用考虑这些类之间的差异。它允许我们有一个界面来以许多不同的方式执行类似的任务。

多态性通过增加灵活性使代码易于更改、维护和扩展。

这篇文章将向你介绍在 Python 中实现多态性的基础知识。

让我们编写一个 Python3 代码，其中包含简单的多态例子；

# 函数和对象的多态性

```
*class* **CSVColumnOne:** *def* **col_count**(*self*):
  print(“Column One — count function is called.”)*def* **col_mean**(*self*):
  print(“Column One — mean function is called.”)*def* **col_std**(*self*):
  print(“Column One — std function is called.”)*class* **CSVColumnTwo:** *def* **col_count**(*self*):
  print(“Column Two — count function is called.”)*def* **col_mean**(*self*):
  print(“Column Two — mean function is called.”)*def* **col_std**(*self*):
  print(“Column Two — std function is called.”)
```

上面你可以看到我们有两个不同的类， ***CSVColumnOne*** 和 ***CSVColumnTwo。*** 而它们之间没有任何联系，它们有三个同名的方法。虽然它们的名字相同，但每个类中的方法执行类似任务的方式不同。

```
*def* **func**(*obj*):
 obj.col_count()
 obj.col_mean()
 obj.col_std()**obj_col_one** = CSVColumnOne()
**obj_col_two** = CSVColumnTwo()func(**obj_col_one**)
func(**obj_col_two**)**Output:**
Column One - count function is called.
Column One - mean function is called.
Column One - std function is called.
Column Two - count function is called.
Column Two - mean function is called.
Column Two - std function is called.
```

由于 Python 中的多态性，我们可以通过将每个类的对象传递给函数来创建调用方法的函数，而不用考虑不同类的对象如何不同地执行任务。

![](img/73cbf1e63555a7f6b4696ea4ad5d45ef.png)

照片由 [Louan García](https://unsplash.com/@louangm?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# **Python 内置的多态函数**

Python 中有很多内置的多态函数。举个例子，就拿 ***len()*** 函数来说吧；

```
**# len() being used for a string**
print(len("Data Science"))**# len() being used for a list**
print(len([1, 2, 3, 4]))**Output:** 12
4
```

从上面可以看出，我们可以对不同类型的对象使用 ***len()*** 函数，而无需考虑该函数将如何处理基于对象类型差异的任务。这使得代码直观易读。

# 具有类方法和继承的多态性

继承允许我们从其他类中派生出函数和数据定义，以增加代码的可重用性。下面可以看到 ***CSVColumnSub*** 类是 ***CSVColumn*** 的子类。

***CSVColumnSub*** 类通过重写 ***col_mean()*** 方法来继承基类的 ***col_count()*** 和 ***col_mean()*** 方法，以具有特定于子类的不同实现。

```
*class* **CSVColumn**:
 *def* **col_count**(*self*):
 print("Count function is called for all columns.") *def* **col_mean**(*self*):
  print("Mean function is called for all columns.")*class* **CSVColumnSub**(*CSVColumn*):
 *def* **col_mean**(*self*):
  print("Mean function is called for only sub-columns.")
```

由于多态，我们可以调用基类和子类中同名的方法，而不用考虑每个类中方法的不同实现。

```
**obj_col_all** = CSVColumn()
**obj_col_sub** = CSVColumnSub()**obj_col_all.col_count()
obj_col_all.col_mean()****obj_col_sub.col_count()
obj_col_sub.col_mean()****Output:**
Count function is called for all columns.
Mean function is called for all columns.
Count function is called for all columns.
Mean function is called for only sub-columns.
```

![](img/2766ac9b87a820e7c484de7360a3cd40.png)

照片由[诺亚·博耶](https://unsplash.com/@emerald_?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 关键要点

*   多态性允许我们用一个接口以多种不同的方式执行相似的任务。
*   多态性通过增加灵活性使代码易于更改、维护和扩展。

# 结论

在这篇文章中，我解释了 Python 中多态性的基础。

这篇文章中的代码可以在我的 GitHub 库中找到。

我希望这篇文章对你有用。

感谢您的阅读！