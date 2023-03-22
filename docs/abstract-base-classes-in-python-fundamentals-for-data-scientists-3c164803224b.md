# Python 中的抽象基类:数据科学家的基础

> 原文：<https://towardsdatascience.com/abstract-base-classes-in-python-fundamentals-for-data-scientists-3c164803224b?source=collection_archive---------2----------------------->

## 用一个具体的例子来理解基础！

![](img/523a4dd3f78912033ebb7543c223a58d.png)

约书亚·阿拉贡在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在 Python 中，**抽象基类**为具体类提供了蓝图。它们不包含实现。相反，它们提供一个接口，并确保派生的具体类得到正确实现。

*   抽象基类不能被实例化。相反，它们被具体的子类继承和扩展。
*   从特定抽象基类派生的子类必须实现该抽象基类中提供的方法和属性。否则，在对象实例化期间会引发错误。

让我们编写一个 Python3 代码，其中包含实现抽象基类的简单示例:

```
from **abc** import **ABCMeta, abstractmethod***class* **AbstactClassCSV**(*metaclass* = *ABCMeta*):

 *def* **__init__**(*self*, *path*, *file_name*):
  self._path = path
  self._file_name = file_name **@*property* @abstractmethod**
 *def* path(*self*):
  pass **@path.setter
 @abstractmethod**
 *def* path(*self*,*value*):
  pass **@*property* @abstractmethod**
 *def* file_name(*self*):
  pass **@file_name.setter
 @abstractmethod**
 *def* file_name(*self*,*value*):
  pass **@abstractmethod**
 *def* display_summary(*self*):
  pass
```

![](img/b58c824002f886727759244e2a582bc9.png)

约翰·邓肯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 定义抽象基类

Python 为抽象基类定义提供了一个 **abc** 模块。我们需要导入 **ABCMeta** 元类，并将其分配给我们想要定义的抽象类。

```
from **abc** import **ABCMeta, abstractmethod***class* **AbstactClassCSV**(*metaclass* = *ABCMeta*):
 .....
 .....
```

在抽象基类内部，我们使用**@ abstract method**decorator 强制子类实现 **path** 和 **file_name** 属性。请注意，这些属性没有实现并且为空，因为抽象类仅用于定义接口。

```
 **@*property* @abstractmethod**
 *def* path(*self*):
  pass **@path.setter
 @abstractmethod**
 *def* path(*self*,*value*):
  pass **@*property* @abstractmethod**
 *def* file_name(*self*):
  pass **@file_name.setter
 @abstractmethod**
 *def* file_name(*self*,*value*):
  pass
```

同样，使用 **@abstractmethod** 装饰器，我们可以定义一个抽象方法，并强制子类实现**display _ summary(*self*)**方法。

```
 **@abstractmethod**
 *def* display_summary(*self*):
  pass
```

此时，如果我们试图实例化一个抽象类 **AbstactClassCSV** 的对象，我们会得到一个错误。

```
**abc_instantiation** = **AbstactClassCSV**("/Users/erdemisbilen/Lessons/", "data_by_genres.csv")**Output:**Traceback (most recent call last):
  File "ABCExample.py", line 119, in <module>
    abc_instantiation = AbstactClassCSV("/Users/erdemisbilen/Lessons/", "data_by_genres.csv")
**TypeError: Can't instantiate abstract class AbstactClassCSV with abstract methods display_summary, file_name, path**
```

![](img/d79a94859b816edd50e4f75e4a1a3e92.png)

Benoit Gauzere 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 创建从抽象基类派生的具体子类

现在我们已经定义了抽象基类( **AbstactClassCSV)** ，我们可以通过继承来创建子类。

下面，我通过继承 ***AbstactClassCSV*** 抽象类，创建了 **CSVGetInfo** 具体类。因此，我必须严格遵循抽象类提供的接口，并在我的具体子类中正确实现所有规定的方法和属性。

```
*class* **CSVGetInfo**(***AbstactClassCSV***):**""" This class displays the summary of the tabular data contained in a CSV file """** **@*property*** *def* **path(*self*)**:
  """ The docstring for the path property """
  print("Getting value of path")
  return self.**_path** **@path.setter**
 *def* **path(*self*,*value*):**
  if '/' in value:
   self.**_path** = value
   print("Setting value of path to {}".format(value))
  else:
   print("Error: {} is not a valid path string".format(value))**data_by_genres** = CSVGetInfo("/Users/erdemisbilen/Lessons/", "data_by_genres.csv")**Output:**Traceback (most recent call last):
  File "ABCExample.py", line 103, in <module>
    **data_by_genres** = CSVGetInfo("/Users/erdemisbilen/Lessons/", "data_by_genres.csv")
TypeError: Can't instantiate abstract class **CSVGetInfo** with abstract methods **display_summary, file_name**
```

为了演示这个案例，我只定义了**路径**属性，没有实现**文件名**属性和**显示摘要**方法。

如果我试图在这种状态**下实例化 **CSVGetInfo** 的对象，就会出现上面的**错误。

这就是抽象类如何防止不正确的子类定义。

```
*class* **CSVGetInfo(*AbstactClassCSV)*:**
 **""" This class displays the summary of the tabular data contained 
 in a CSV file """** **@*property*** *def* **path(*self*):**
  """ The docstring for the path property """
  print("Getting value of path")
  return self._path **@path.setter**
 *def* **path(*self*,*value*):**
  if '/' in value:
   self._path = value
   print("Setting value of path to {}".format(value))
  else:
   print("Error: {} is not a valid path string".format(value)) **@*property*** *def* **file_name(*self*):**
  """ The docstring for the file_name property """
  print("Getting value of file_name")
  return self._file_name **@file_name.setter**
 *def* **file_name(*self*,*value*):**
  if '.' in value:
   self._file_name = value
   print("Setting value of file_name to {}".format(value))
  else:
   print("Error: {} is not a valid file name".format(value))*def* **display_summary(*self*):**
  data = pd.read_csv(self._path + self._file_name)
  print(self._file_name)
  print(data.info())
```

由于我们已经定义了所有必需的属性和上面抽象类指定的方法，现在我们可以实例化并使用派生子类 **CSVGetInfo** 的对象。

![](img/8dabc3f037aeb481020bde2c9466c81a.png)

文森特·范·扎林盖在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 关键要点

*   抽象基类将接口与实现分开。
*   它们确保派生类实现抽象基类中指定的方法和属性。
*   抽象基类将接口与实现分开。它们定义了必须在子类中使用的泛型方法和属性。实现由具体的子类来处理，我们可以在其中创建可以处理任务的对象。
*   它们有助于避免错误，并通过提供创建子类的严格方法，使类层次结构更容易维护。

# 结论

在这篇文章中，我解释了 Python 中抽象基类的基础知识。

这篇文章中的代码可以在我的 GitHub 库中找到。

我希望这篇文章对你有用。

感谢您的阅读！