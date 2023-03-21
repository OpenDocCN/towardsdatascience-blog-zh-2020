# Python 中的属性:数据科学家的基础

> 原文：<https://towardsdatascience.com/properties-in-python-fundamentals-for-data-scientists-43071e6be580?source=collection_archive---------23----------------------->

## 用一个具体的例子来理解基础！

![](img/0bc70a6d6758b76e1b26f17f9eab3fd7.png)

戈兰·艾沃斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在许多面向对象的编程语言中，通过使用特定的关键字，如 *private* 或 *protected* ，一个对象或一个类结构中的数据可以被显式地隐藏起来，不被外部访问。这通过不允许从类结构外部访问数据来防止误用。

相比之下，Python 类中的所有属性(存储在类或实例中的数据)都是公开的。因此，您没有对数据访问的显式控制，但是您有两个主要的隐式选项来限制数据访问& Python 类中的操作；

*   使用命名约定，用一个下划线作为受保护属性的前缀，用两个下划线作为私有属性的前缀。因为它只是一个显示意图的命名约定，所以即使正确应用了命名约定，用户仍然可以从外部访问属性。
*   使用 Python **property** 函数或 **@property** decorator 来定义 setter 和 getter 方法。这些特殊的方法规定了其他人应该如何以可控的方式设置和获取属性。

这篇文章将向你介绍 Python 中属性的基础知识。

让我们编写一个 Python3 代码，其中包含实现 **@property** decorator 的简单示例:

```
import pandas as pd*class* **CSVGetInfo**(*object*):
 **""" This class displays the summary of the tabular data contained 
 in a CSV file """***def* **__init__**(*self*, *path*, *file_name*):
  self._path = path
  self._file_name = file_name**@*property*** *def* **path(*self*):**
  """ The docstring for the path property """
  print("Getting value of path")
  return self._path**@path.setter**
 *def* **path(*self*,*value*):**
  if '/' in value:
   self._path = value
   print("Setting value of path to {}".format(value))
  else:
   print("Error: {} is not a valid path string".format(value))**@path.deleter**
 *def* **path(*self*):**
  print('Deleting path attribute')
  del self._path**@*property*** *def* **file_name(*self*):**
  """ The docstring for the file_name property """
  print("Getting value of file_name")
  return self._file_name**@file_name.setter**
 *def* **file_name(*self*,*value*):**
  if '.' in value:
   self._file_name = value
   print("Setting value of file_name to {}".format(value))
  else:
   print("Error: {} is not a valid file name".format(value))**@file_name.deleter**
 *def* **file_name(*self*):**
  print('Deleting file_name attribute')
  del self._file_name*def* **display_summary(*self*):**
  data = pd.read_csv(self._path + self._file_name)
  print(self._file_name)
  print(data.info())if __name__ == '__main__':data_by_genres = CSVGetInfo("/Users/erdemisbilen/Lessons/", 
 "data_by_genres.csv") ***(1)***print(data_by_genres.path)
 ***(2)***print(data_by_genres.file_name) ***(3)***data_by_genres.path="lessons"
 ***(4)***print(data_by_genres.path)
 ***(5)***data_by_genres.path="/Users/"
 ***(6)***print(data_by_genres.path)
 ***(7)***del data_by_genres.path ***(8)***data_by_genres.file_name="datacsv"
 ***(9)***print(data_by_genres.file_name)
 ***(10)***data_by_genres.file_name="data.csv"
 ***(11)***print(data_by_genres.file_name)
 ***(12)***del data_by_genres.file_name**Output:**
air-MacBook-Air: **python PropertyExample.py** ***(1)***Getting value of path
 ***(1)***/Users/erdemisbilen/Lessons/
 ***(2)***Getting value of file_name
 ***(2)***data_by_genres.csv ***(3)***Error: lessons is not a valid path string
 ***(4)***Getting value of path
 ***(4)***/Users/erdemisbilen/Lessons/
 ***(5)***Setting value of path to /Users/
 ***(6)***Getting value of path
 ***(6)***/Users/
 ***(7)***Deleting path attribute ***(8)***Error: datacsv is not a valid file name
 ***(9)***Getting value of file_name
 ***(9)***data_by_genres.csv
 ***(10)***Setting value of file_name to data.csv
 ***(11)***Getting value of file_name
 ***(11)***data.csv
 ***(12)***Deleting file_name attribute
```

![](img/df1beb0f8678761a09f2028ea0714228.png)

照片由 [Waranya Mooldee](https://unsplash.com/@anyadiary?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# @property Decorator 来定义 Setter 和 Getter 方法

我们可以使用 **property()** 函数或者 **@property** decorator 来定义属性。他们做同样的工作，但后者被认为更直观，更容易阅读。

```
*class* **CSVGetInfo**(*object*):*def* **__init__**(*self*, *path*, *file_name*):
  self._path = path
  self._file_name = file_name
```

在我们的 **CSVGetInfo** 类中有 **_path** 和 **_file_name** 私有属性。为了控制其他人如何从外部获取和设置这些属性，让我们使用**@*property***decorator 来定义属性的 getter 和 setter 函数。

```
**@*property*** *def* **path(*self*):**
  """ The docstring for the path property """
  print("Getting value of path")
  return self._path
```

**@*属性*** 装饰器被添加到**路径( *self* )** 方法的开始处。这表明 **path(self)** 方法被用作 getter 函数。

```
**@file_name.setter**
 *def* **file_name(*self*,*value*):**
  if '.' in value:
   self._file_name = value
   print("Setting value of file_name to {}".format(value))
  else:
   print("Error: {} is not a valid file name".format(value))
```

同理**，@ < property_name >。setter** decorator 帮助定义 setter 方法。在我们的例子中， **@file_name.setter** 被添加到了 **file_name( *self* ， *value* )** 方法之前，以将其定义为 setter 方法。注意，在我们的 setter 方法中，我们在将值赋给 **_file_name** 属性之前对其进行了验证。

```
**@path.deleter**
 *def* **path(*self*):**
  print('Deleting path attribute')
  del self._path
```

除了 setter 和 getter 方法，我们可以用@ **< property_name >定义 deleter 方法。删除器**装饰器。

我们不必为每个属性定义所有三个方法，因为有时属性只能在创建实例时设置。对于这种情况，我们可以通过包含一个 getter 方法来定义只读属性。

```
 data_by_genres = CSVGetInfo("/Users/erdemisbilen/Lessons/", 
 "data_by_genres.csv") print(data_by_genres.path)
 print(data_by_genres.file_name) data_by_genres.path="lessons"
 print(data_by_genres.path)
 data_by_genres.path="/Users/"
 print(data_by_genres.path)
 del data_by_genres.path
```

既然我们已经定义了我们的属性，现在我们可以使用它们来设置和获取包含在我们的**data _ by _ genders**实例中的数据。

请注意，我们可以随意使用 setter 和 getter 方法并修改它们，而不会影响代码结构中的实现。

![](img/7d4f10b5e7555d941f89205710156197.png)

[T S](https://unsplash.com/@anna99t?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 关键要点

*   **@property** decorator 被认为是定义 getters、setters 和 deleters 方法的首选方式。
*   通过定义属性，我们在类中创建了一个中间层，通过不影响已开发的代码结构来控制其他人如何访问数据。

# 结论

在这篇文章中，我解释了 Python 中属性的基础知识。

这篇文章中的代码可以在[我的 GitHub 库中找到。](https://github.com/eisbilen/PropertiesExample)

我希望这篇文章对你有用。

感谢您的阅读！