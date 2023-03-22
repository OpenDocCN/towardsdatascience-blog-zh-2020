# Python 中的继承:数据科学家的基础

> 原文：<https://towardsdatascience.com/inheritance-in-python-fundamentals-for-data-scientists-99455f176f76?source=collection_archive---------71----------------------->

## 用一个具体的例子来理解基础！

![](img/bb291446428ff6ce70cbdf0b7e69474d.png)

克里斯托夫·高尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

类继承是面向对象编程中的一个重要概念。它使我们能够扩展现有类的能力。为了创建一个新的类，我们使用一个可用的类作为基础，并根据我们的需要扩展它的功能。因为我们通过使用基类中可用的方法和数据定义来构建新类，所以开发和维护时间减少了，代码可重用性增加了。

这篇文章将向你介绍 Python 中类继承的基础知识。

让我们编写一个 Python3 代码，其中包含一个简单的类继承示例；

```
import pandas as pd
import random**# BASE CLASS
*class* CSVGetInfo:**
 **""" This class displays the summary of the tabular data contained  
 in a CSV file """** instance_count = 0 **# Initializer / Instance Attributes** *def* __init__(*self*, *path*, *file_name*):
  CSVGetInfo.increase_instance_count()
  self.path = path
  self.file_name = file_name
  print("CSVGetInfo class object has been instantiated")  **# Instance Method**
 *def* display_summary(*self*):
  data = pd.read_csv(self.path + self.file_name)
  print(self.file_name)
  print(data.head(self.generate_random_number(10)))
  print(data.info())
  return data **# Class Methods
 @*classmethod*** *def* increase_instance_count(*cls*):
  cls.instance_count += 1
  print(cls.instance_count)  **@*classmethod*** *def* read_file_1(*cls*):
  return cls("/Users/erdemisbilen/Lessons/", "data_by_artists.csv") **@*classmethod*** *def* read_file_2(*cls*):
  return cls("/Users/erdemisbilen/Lessons/", "data_by_genres.csv") **# Static Methods
 @*staticmethod*** *def* generate_random_number(*limit*):
  return random.randint(1, limit)**# SUB CLASS *class* CSVGetColumnDetails(*CSVGetInfo*):**
 **""" This class displays the summary of a column in a tabular data 
 contained in a CSV file """**  **# Initializer / Instance Attributes** *def* __init__(*self*, *path*, *file_name*, *column_name*):
  CSVGetInfo.__init__(self, path, file_name)
  self.column_name = column_name
  print("CSVGetDetail class object has been instantiated")  **# Instance Method**
 *def* display_column_summary(*self*):
  data = self.display_summary()
  print(data[self.column_name].describe())  **@*classmethod*** *def* read_file_1(*cls*, *column_name*):
  return cls("/Users/erdemisbilen/Lessons/", "data_by_artists.csv", 
  column_name)if __name__ == '__main__':
 data = CSVGetColumnDetails.read_file_1("danceability")
 data.display_column_summary()
```

![](img/f6e89f82cc17d2c0620ec0912f356258.png)

照片由[萨布里·图兹库](https://unsplash.com/@sabrituzcu?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# Python 中的继承

在上面的 Python 代码中，我们有 **CSVGetInfo** 基类，它包含几个方法和数据定义。

***display _ summary(self)***实例方法使用 ***path*** 和 ***file_name 中提供的值打印 CVS 文件中包含的表格数据的摘要。***

还有几个类方法，****read _ file _ 2(cls)*******increase _ instance _ count(cls)***，缓解基类的对象实例化。**

**假设我们想要创建一个新类**csvgetcolumndedetails**，以获取表格数据中特定列的汇总信息。我们可以从头开始编写一个新的类，但是更好的方法是继承和扩展 **CSVGetInfo** 类中已经可用的一些方法和数据定义。**

```
****# BASE CLASS
*class* CSVGetInfo:
 ....
 ....****# SUB CLASS *class* CSVGetColumnDetails(*CSVGetInfo*):
 ....
 ....****
```

*****【class name】***是 Python 用来创建子类的继承语法。在我们的例子中，我们的基类是***CSVGetInfo****，我们的扩展子类是****csvgetcolumndedetails。*****

# **子类中的属性初始化**

**我们调用我们基类的 ***__init__*** 方法来实例化 ***路径*** 和 ***文件名*** 属性。我们将这些属性从基类派生到子类。**

**仅在我们的子类级别可用的 ***column_name*** 属性用表示 **CSVGetColumnDetails 的**实例**的 ***self*** 符号进行实例化。****

```
****# Initializer / Instance Attributes
** *def* __init__(*self*, *path*, *file_name*, *column_name*):
  CSVGetInfo.__init__(self, path, file_name)
  self.column_name = column_name
  print("CSVGetDetail class object has been instantiated")**
```

**![](img/3b9535aa4721b495816a0923b618dc4a.png)**

**Raul Varzar 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片**

# **扩展基类中可用的方法**

**然后我们在基类派生的***display _ summary()***方法的帮助下，在我们的子类中创建新方法。我们在子类中使用并扩展了***display _ summary()***的功能来定义一个新方法。**

```
****# Instance Method**
 *def* display_column_summary(*self*):
  data = self.display_summary()
  print(data[self.column_name].describe())**
```

**这个新方法在我们的子类中，***【display _ column _ summary】(self)，*** 显示特定列的摘要，通过使用数据 ***【显示 _ 摘要】*****方法返回。这个子类方法还有一个子类属性 ***column_name。*******

# **重写基类中可用的方法**

**请注意，我们有 ***read_file_1*** 类方法，它们都在我们的基类和子类中定义了不同的实现。**

**基类中的***read _ file _ 1***class 方法只是传递了 ***path*** 和 ***file_name*** 值来实例化基类对象，而子类中的方法用一个附加参数 ***column_name 来实例化子类对象。*****

**这意味着子类中的 ***read_file_1*** 方法覆盖了基类中可用的相同方法。当这些方法被调用时，它们执行它们独特的实现。**

```
****# BASE CLASS
*class* CSVGetInfo:
 ....
 ....
** **@*classmethod*** *def* read_file_1(*cls*):
  return cls("/Users/erdemisbilen/Lessons/", "data_by_artists.csv")**# SUB CLASS *class* CSVGetColumnDetails(*CSVGetInfo*):
 ....
 ....** **@*classmethod*** *def* read_file_1(*cls*, *column_name*):
  return cls("/Users/erdemisbilen/Lessons/", "data_by_artists.csv", 
  column_name)**
```

**![](img/20db23555ad133e1ea087bcf1a5b8927.png)**

**乔希·考奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片**

# **类继承中的命名约定**

**基类中可能有一些方法或属性(实例变量)只在基类中使用。它们被称为**私有方法**，并以双下划线约定***_ _ private method name***命名。这些方法不应该在基类之外调用，包括子类。**

**基类中还可能有一些其他的方法或属性，它们只在基类或子类定义中使用。它们被称为**受保护的方法**，并以单条下划线约定***_ protected method name***命名。这些方法应该只在基类和子类结构中调用。**

# **关键要点**

*   **继承增加了代码的可重用性，减少了开发和维护时间。**
*   **继承允许我们定义一个类，它接受并扩展基类中所有可用的方法和数据定义。**

# **结论**

**在这篇文章中，我解释了 Python 中类继承的基础。**

**这篇文章中的代码和使用的 CSV 文件可以在[我的 GitHub 资源库中找到。](https://github.com/eisbilen/ClassInheritanceExample)**

**我希望这篇文章对你有用。**

**感谢您的阅读！**