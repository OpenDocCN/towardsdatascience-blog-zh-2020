# Python 中的方法:数据科学家的基础

> 原文：<https://towardsdatascience.com/methods-in-python-fundamentals-for-data-scientists-6a9393b2c2e7?source=collection_archive---------47----------------------->

## 用一个具体的例子来理解基础！

![](img/7e9091513cf5aa5509bab18118ff9ca3.png)

制造者在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [NESA 的照片](https://unsplash.com/@nesabymakers?utm_source=medium&utm_medium=referral)

Python 类可以保存以下函数:

*   一个类的一部分**(类方法)**
*   实例的一部分**(实例方法)**
*   既不属于类也不属于实例**(静态方法)**

每种方法都有不同的用例。如果你理解了基本原理，你就可以非常清晰地编写面向对象的 Python 代码。

这篇文章将向你介绍这些方法的基础知识和它们的用例。

让我们用 Python3 编写一个包含所有三种方法类型的简单示例的类；

```
import pandas as pd
import random*class* CSVGetInfo:
**""" This class displays the summary of the tabular data contained in a CSV file """****# Initializer / Instance Attributes**
*def* __init__(*self*, *path*, *file_name*):
 CSVGetInfo.increase_instance_count()
 self.path = path
 self.file_name = file_name**# Instance Methods**
*def* display_summary(*self*):
 data = pd.read_csv(self.path + self.file_name)
 print(self.file_name)
 print(data.head(self.generate_random_number(10)))
 print(data.info())**# Class Methods** @*classmethod
def* increase_instance_count(*cls*):
 cls.instance_count += 1
 print(cls.instance_count)@*classmethod
def* read_file_1(*cls*):
 return cls("/Users/erdemisbilen/Lessons/", "data_by_artists.csv")@*classmethod
def* read_file_2(*cls*):
 return cls("/Users/erdemisbilen/Lessons/", "data_by_genres.csv")**# Static Methods**
@*staticmethod
def* generate_random_number(*limit*):
 return random.randint(1, limit)if __name__ == '__main__':
 data_by_artists = CSVGetInfo.read_file_1()
 data_by_genres = CSVGetInfo.read_file_2() data_by_artists.display_summary()
 data_by_genres.display_summary()
```

![](img/b7f41dd4a4a02bf835fca7d2cbfffed6.png)

照片由 [Skylar Sahakian](https://unsplash.com/@skylarfaithfilm?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 实例方法

实例方法是类结构中最常用的方法。任何在类结构中定义的函数都是一个实例方法，除非用 decorators 声明。所以，你不需要装饰器来定义实例方法。

```
**# Instance Method***def* display_summary(*self*):
 data = pd.read_csv(self.path + self.file_name)
 print(self.file_name)
 print(data.head(self.generate_random_number(10)))
 print(data.info())
```

它们采用一个隐式参数， ***self*** *，*表示方法被调用时实例本身。

借助***self****参数，实例方法可以访问实例变量(属性)和同一对象中的其他实例方法。*

# *何时使用实例方法*

*实例方法是类结构的核心，它们定义了类的行为。*

*我们可以使用特定于实例的数据来执行实例方法中定义的任务。他们可以在 ***self*** 参数的帮助下访问实例中包含的唯一数据。*

*在我们的示例中，我们有两个 **CSVGetInfo** 类的实例，它们分别存储不同的文件名值“data_by_artists.csv”和“data _ by _ genres.csv”。***display _ summary(self)***的实例方法通过访问每个实例特有的值来执行任务。*

*![](img/eae8a63abeed3f8aa1f2241c9a258cb3.png)*

*由 [Travis Gergen](https://unsplash.com/@travisgergen?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片*

# *类方法*

*代替实例方法接受的 ***self*** 参数，类方法接受一个 ***cls*** 隐式参数。 ***cls*** 代表类本身，而不是类(对象)的实例。*

```
***# Class Methods**@*classmethod
def* increase_instance_count(*cls*):
 cls.instance_count += 1
 print(cls.instance_count)@*classmethod
def* read_file_1(*cls*):
 return cls("/Users/erdemisbilen/Lessons/", "data_by_artists.csv")@*classmethod
def* read_file_2(*cls*):
 return cls("/Users/erdemisbilen/Lessons/", "data_by_genres.csv")*
```

*因此，类方法不能修改对象的状态，因为我们需要 ***self*** 参数来这样做。相反，他们可以修改对该类的所有实例都有效的类状态。*

*类方法的编写类似于任何其他方法，但用 **'@classmethod'** 修饰，并采用 ***cls*** 参数。*

# *何时使用类方法*

*您不必为了使用类方法而创建实例，因为类方法直接绑定到类，而不是绑定到特定的实例/对象。*

*因此它们用于管理类级操作，如下所列；*

*   ***实例创建:**类方法可用作对象的工厂，以简化设置和实例化工作。*
*   ***实例管理:**我们可以限制一个类可以创建的实例数量。*
*   *班级水平查询:它们可以用来提供有关班级的有用信息，例如；创建的实例数。*
*   ***测试***
*   ***例子:**类方法可以提供类用法的例子，这样其他人可以很容易地理解如何使用类。*

# *静态方法*

*静态方法不带任何隐式参数。由于不能带 ***self*** 和 ***cls*** 参数，所以既不能修改对象状态，也不能修改类状态。*

*它们就像带有 **'@staticmethod'** 修饰的类的名称空间内的独立方法。*

```
***# Static Methods**@*staticmethod
def* generate_random_number(*limit*):
 return random.randint(1, limit)*
```

*![](img/bbc6bb45b929ecb2b53be3ab0bd565a7.png)*

*[潘晓珍](https://unsplash.com/@zhenhappy?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照*

# *何时使用静态方法*

*由于静态方法是完全独立的代码，它们不能访问类中的任何东西。它们主要使用传递给它们的参数来处理实用程序任务。因此，它们被用作助手或实用函数。*

# *关键外卖*

*使用 decorators 并正确地标记方法可以让其他人更好地理解我们的意图和类结构。随着其他人更好地理解我们的类架构和正确分配的装饰器，其他开发人员不正确使用我们的类的可能性就更小了。它提供了维护的好处，并有助于防止错误和失误。*

# ***结论***

*在这篇文章中，我解释了 Python 中方法类型的基础。*

*这篇文章中的代码和使用的 CSV 文件可以在[我的 GitHub 库中找到。](https://github.com/eisbilen/MethodsExampleClass)*

*我希望这篇文章对你有用。*

*感谢您的阅读！*