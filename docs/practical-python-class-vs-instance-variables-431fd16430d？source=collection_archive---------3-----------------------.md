# 实用 Python:类与实例变量

> 原文：<https://towardsdatascience.com/practical-python-class-vs-instance-variables-431fd16430d?source=collection_archive---------3----------------------->

## 如何定义它们并与之互动

![](img/439e2978b72125f1ba82b4575b462291.png)

(图片由作者提供)

类是 Python 最基础的部分，因为它是面向对象编程的本质。

Python 中的一切都是对象比如整数、列表、字典、函数等等。每个对象都有一个类型，对象类型是使用类创建的。

实例是属于一个类的对象。例如，list 是 Python 中的一个类。当我们创建一个列表时，我们有一个 list 类的实例。

在本文中，我将重点关注类和实例变量。我假设您对 Python 中的类有基本的了解。如果没有，您仍然可以理解什么是类和实例变量，但是语法可能看起来有点让人不知所措。

如果你想重温基础知识，我还写了一篇关于 Python 类的介绍性文章。

让我们从类和实例变量开始。

类变量是在类内部而不是在任何函数外部声明的。实例变量是在构造函数 _ _ init _ _ 方法中声明的。

考虑下面的类定义:

```
class Book():
   fontsize = 9
   page_width = 15 def __init__(self, name, writer, length):
      self.name = name
      self.writer = writer
      self.length = length
```

假设 Book 类被一家出版公司使用。fontsize 和 page_width 是类变量。创建的每个 book 实例的 fontsize 为 9，page_width 为 15 cm。

![](img/f3ffbad42d47c258e344789499b723c1.png)

[NMG 网](https://unsplash.com/@nmgnetwork?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/book-publishing?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

name、writer 和 length 变量是在 __init__ 方法中声明的，因此它们是实例变量。这些变量的值在创建实例时确定。

这是有意义的，因为 fontsize 和 page_width 通常是标准的。但是，名称、作者和长度是特定于每本书的(即实例)。

将 fontsize 和 page_width 声明为类变量节省了我们大量的时间和精力。假设出版公司决定改变书籍中使用的字体大小。如果它是一个实例变量，我们将需要为每个实例(如 book)更改它。然而，由于它是一个类变量，一个赋值就可以完成每本书的工作。

让我们创建 Book 类的一个实例:

```
book1 = Book("Intro to Python", "John Doe", 50000)print(book1.writer)
"John Doe"print(book1.page_width)
15
```

尽管我们没有显式地将 page_width 变量分配给我们的实例，但它在默认情况下具有这个属性。

我们可以为任何实例更改类变量的值(即覆盖它们)。假设我们想让 book1 宽一点，那么我们将改变这个实例的 page_width。

```
book1.page_width = 17print(book1.page_width)
17print(Book.page_width)
15
```

如您所见，我们只更改了 book1 实例的值。类变量的值保持不变。

我们定义的方法可以访问类变量，但是我们需要小心。我将在下面的例子中说明原因。

我们为 Book 类定义了一个方法，该方法根据长度(字数)、字体大小和 page_width 计算页数:

```
def number_of_pages(self):
   pages = (self.length * fontsize) / (page_width * 100)
   return pages
```

此方法将引发一个 NameError，表明未定义“fontsize”。“页面宽度”也是如此。虽然这些是类变量，但我们需要告诉方法从哪里获取它们。

我们可以从实例本身或从类中获取它们。

```
#Using the instance
def number_of_pages(self):
   pages = (self.length * self.fontsize) / (self.page_width * 100)
   return pages#Using the class
def number_of_pages(self):
   pages = (self.length * Book.fontsize) / (Book.page_width * 100)
   return pages
```

这两种方法都行得通。我们来做一个例子:

```
book1 = Book("Intro to Python", "John Doe", 50000)book1_pages = book1.number_of_pages()print(book1_pages)
300
```

更新类变量将影响该类的所有实例。这是一个节省我们时间和精力的好功能。然而，我们也需要小心变化的程度。

这里有一个例子。

```
book1 = Book("Intro to Python", "John Doe", 50000)
book2 = Book("Intro to Pandas", "Jane Doe", 40000)print(book1.fontsize, book2.fontsize)
(9, 9)Book.fontsize = 11print(book1.fontsize, book2.fontsize)
(11, 11)
```

实例通过它们所属的类来访问类变量。让我们做一个例子来说明我的意思。

__dict__ 方法可用于查看实例或类的属性。

```
book1 = Book("Intro to Python", "John Doe", 50000)
print(book1.__dict__)
{'name': 'Intro to Python', 'writer': 'John Doe', 'length': 50000}print(book1.fontsize)
9
```

当我们打印出 book1 的数据属性时，我们看不到 fontsize 或 page_width。但是，我们可以通过打印出来看到 book1 有 fontsize 属性。

实例(book1)通过类访问类变量。让我们也在 Book 类上应用 __dict__ 方法。

```
print(Book.__dict__)
```

__module__': '__main__ '， **'fontsize': 9，' page _ width ':15【T1]，' __init__': <功能本。__init__ at 0x7f6cc5f54620 >，' number_of_pages': <函数 Book . number _ of _ at 0x 7 F6 cc 5 f 548 c8>，' __dict__': <属性' __dict__' of 'Book '对象>，' __weakref__': <属性' _ _ weak ref _ _ '' Book '对象>，' __doc__': None}**

类变量以及方法和其他一些东西都打印出来了。

## 结论

类变量影响整个类。因此，当一个类变量被更新时，所有的实例也被更新。这非常方便，但我们也需要小心。如果使用不慎，可能会出现不良后果。

实例变量为每个实例取唯一的值。我们在创建实例时为它们赋值。

所有的实例一开始都为类变量取相同的值，但是我们可以在以后为一个实例更新它们。我们对一个实例所做的更改不会影响其他实例的类变量的值。

感谢您的阅读。如果您有任何反馈，请告诉我。