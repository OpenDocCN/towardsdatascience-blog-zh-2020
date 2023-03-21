# 让你的 Python 代码流畅

> 原文：<https://towardsdatascience.com/make-your-python-code-fluent-7ee2dd7c9ae3?source=collection_archive---------32----------------------->

## 函数和运算符重载

![](img/4972d94be57c463c7d2ed9b40faebd6f.png)

诺兰·马尔克蒂在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

**Python 中的重载**允许我们定义函数和操作符，它们根据所使用的参数或操作数以不同的方式运行。

# 运算符重载

作为一个例子，我们可以使用 **"+"操作符**对**数值**进行算术计算，而当使用**字符串**操作数时，同一个 **"+"操作符**连接两个字符串。这被称为**操作符重载**，它允许我们在不同的对象类型上使用相同的操作符来执行类似的任务。

如下所示，我们可以重载 **"+"操作符**来使用我们定制的对象类型。

```
**# No overloading, task is performed by 'add' method**
cost1 = Cost(10)
cost2 = Cost(24)
cost_total = cost1.add(cost2)**# '+' operator is overloaded to work with 'Cost' type of objects**
cost1 = Cost(10)
cost2 = Cost(24)
cost_total = cost1 + cost2
```

与第一个代码块相比，上面的第二个代码块更容易阅读和理解。这就是重载如何使我们的代码流畅和干净。

# 函数重载

虽然 Python 默认情况下不支持函数重载，但是有一些技巧可以实现它。

假设我们想创建一个函数来计算三角形的面积。用户可以提供；

*   三角形的底和高，
*   三角形三条边的长度

如果不考虑重载，我们需要定义两个不同的函数来处理这个任务。

我们可以只编写一个函数并重载它来增加代码的一致性和可读性，而不是为同一个任务定义不同的函数。

```
**#---------------------
# No overloading, task is performed by two similar functions
#---------------------** def **triangle_area_base_height(base, height)**:
 ....
def **triangle_area_three_sides(a_side, b_side, c_side):**
 ....**area1 = triangle_area_base_height (10,14)
area2 = triangle_area_three_sides (10, 12,8)****#---------------------
# Function overloading
#---------------------** **from** multipledispatch **import** dispatch@dispatch(int,int)
def **triangle_area****(base, heigth):**
 .....@dispatch(int,int,int)
def **triangle_area****(****a_side, b_side, c_side****):**
 .....**area1 = triangle_area (10,14)
area2 = triangle_area (10,12,8)**
```

# 重载运算符

当我们使用一个操作符时，会调用一个与该操作符相关的特殊函数。举个例子，当我们使用 **+操作符**时，会调用 **__add__** 的特殊方法。要重载+ operator，我们需要在一个类结构中扩展 **__add__** 方法的功能。

```
**# Addition of 2D point coordinates with
# + operator overloading****class** Point2D:
 **def** __init__(self, x, y):
  self.x **=** x
  self.y **=** y# adding two points
 **def****__add__(self, other):**
  **return** self.x **+** other.x, self.y **+** other.y**def** __str__(self):
  **return** self.x, self.ypoint1 **=** Point2D(5, 4)
point2 **=** Point2D(6, 1)
point3 **=** point1 **+** point2**print**(point3)**Output:** (11, 5)
```

我们可以重载 **+ operator** 来拥有更加流畅和易读的代码，而不是定义一个额外的函数来添加 **Point2d objects** 。

通过修改类结构中相关的特殊方法，可以重载所有操作符，包括赋值、比较和二元操作符。

# 重载内置函数

我们还可以重载内置的 Python 函数来修改它们的默认动作。考虑 **len()** 内置函数，它返回序列或集合中对象的数量。为了将它与我们定制的对象类型一起使用，我们需要实现重载。

为了重载 **len()** ，我们需要在一个类结构中扩展 **__len__** 方法的功能。

下面让我们看看如何用 Python 来实现:

```
class **Names**:def **__init__**(self, name, country):
  self.name = list(name)
  self.country = countrydef **__len__**(self):
  return len(self.name)obj1 = Names(['Amy', 'Harper', 'James'], 'UK')
print(len(obj1))**Output:** 3
```

同样，您可以重载所有内置函数。

![](img/8b1b3ed00427dbabf2da9899af7b9ee1.png)

照片由[春迪·坦茨](https://unsplash.com/@chundy_tanz?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 重载用户定义的函数

Python 默认不支持函数重载。但是我们可以使用多个分派库来处理重载。

导入 **multipledispatch，**我们需要做的就是用 **@dispatch()** decorator 来修饰我们的函数。

```
**import math
from multipledispatch import dispatch**@dispatch(int,int)
def **triangle_area(base, height)**:
 return (base*height)/2@dispatch(int,int,int)
def **triangle_area(a_side, b_side, c_side)**:
 s = (a_side + b_side + c_side) / 2
 return math.sqrt(s * (s-a_side) * (s-b_side) * (s-c_side))area1 = triangle_area (10,14)
area2 = triangle_area (5,5,5)print("Area1: {}".format(area1))
print("Area2: {}".format(area2))**Output:**
Area1: 70.0 
Area2: 10.825317547305483
```

# 关键要点

*   **Python 中的重载**允许我们根据所使用的参数或操作数来定义行为不同的函数和操作符。
*   **操作符重载**允许我们在不同的对象类型上使用相同的操作符来执行相似的任务。
*   我们可以只编写一个函数并重载它来增加代码的一致性和可读性，而不是为同一个任务定义不同的函数。

# 结论

在这篇文章中，我解释了 Python 中重载的基础知识。

这篇文章中的代码可以在[我的 GitHub 库中找到。](https://github.com/eisbilen/Overloading)

我希望这篇文章对你有用。

感谢您的阅读！