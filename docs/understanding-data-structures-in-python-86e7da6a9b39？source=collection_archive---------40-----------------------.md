# 理解 Python 中的数据结构

> 原文：<https://towardsdatascience.com/understanding-data-structures-in-python-86e7da6a9b39?source=collection_archive---------40----------------------->

## 最有用的内置数据结构概述。

![](img/e6620bee603bb6d0d0b71b0e6287a730.png)

照片由 [Riho Kroll](https://unsplash.com/@rihok) 在 [Unsplash](https://unsplash.com/photos/RgLaH00kZOk) 上拍摄

ython 有很大的可能性来处理我们的数据。让我们仔细看看列表、元组/命名元组、集合和字典的功能。我不打算深入研究，例如，如何在列表中添加或删除元素。我的目标是用简单直接的方式展示每个集合的操作，举例说明它们的陈述。在每个解释的最后，将展示每个系列的主要特征。这些例子将用 Python 语言编写，但是由于重点是概念而不是应用程序，所以您可以将这些应用到您的编程语言中，这可能会以非常相似的方式工作。

# 实验数字电视系统

**2020–06–18**

*   从 Python 3.7 开始，字典保证保持插入顺序。坦克[让-弗朗索瓦·科比特](https://medium.com/u/88e14d0476bd?source=post_page-----86e7da6a9b39--------------------------------)

# 元组

元组是可以按顺序存储特定数量的其他对象的对象。一旦元素被放入 tuple 中，它们就变得不可变，这意味着您将无法在运行时添加、删除或移动这些元素。反过来，元组在大多数情况下用于存储数据，因此它的主要功能是在一个地方存储不同类型的数据。元组有两种声明方式，不管是否使用括号(，它们的项之间用逗号分隔。

带括号和不带括号的元组示例。

```
items = ('Name', 75, 80, 90)
items = 'Name', 75, 80, 90
```

此外，我们可以在元组中添加函数，例如:

`items = calculate((“Name”, 75, 80, 90), datetime.date(2020,01,01))`

在这个例子中，我们需要括号，所以函数的第一个参数是我们的元组，第二个是数据，最后，我们关闭函数的括号，这样我们的 Python 解释器将知道函数的开始和结束位置。

## 主要特征:

*   无序的
*   不变的
*   比列表更快

# 命名元组

上面我们看到了元组的使用，但在某些情况下，我们可能希望单独访问我们的一些键，让我们分析下面的例子，其中我希望使用元组来表示 RGB 中的颜色。

`rgb_color = (55, 155, 255)`

我们可以同意，对这个作业的解读并不完全错误。读到这里，我可以确切地理解，当访问 rgb_color [0]键时，它反过来将返回值 55。通过对变量的描述，我知道值 55 代表 RGD 红，但在大多数情况下，这可能不是显而易见的，要么是由于使用的上下文，要么是由于开发人员的经验。举例说明了使用命名元组处理这种情况的更好方法。

```
from collections import namedtuple

Color = namedtuple("Color", "red blue green")
color = Color(red=55, blue=155, green=255)
```

命名元组的声明类似于传统元组的声明，在我们的 named tuple 中，作为第一个参数，我们必须通知我们的标识符，第二个参数字符串用空格或逗号分隔。结果是一个与传递的标识符完全相同的对象。这样，我们可以通过访问它的一个属性来解包您的值，例如:`cor.blue`

## 主要特征:

*   类似于元组
*   让您的元组自文档化。
*   不需要使用索引来访问元素

# 目录

列表是几个项目序列的概念，其中，根据需要，您需要将它们分组或按顺序放置。列表的一个特性是，如果有必要，我们可以单独访问列表的每个位置，在 Python 中，它是在两个方括号[]之间声明的，它的内部项由逗号(，)分隔。

`fruits = [‘apple’, ‘banana’, ‘orange’]`

除了已经举例说明的数据之外，您还可以用其他类型的数据填充您的列表，比如对象、整数、元组甚至列表的列表。根据定义，我们应该总是优先在我们的列表中存储相同类型的数据，如果你觉得有必要做这种类型的操作，也许你应该看看字典提供了什么。

## 主要特征:

*   整齐的
*   易变的
*   接受重复元素
*   按索引访问元素
*   可以嵌套
*   动态增长

# 设置

您可以将该集合视为列表的扩展，在您想要存储唯一值的情况下应该使用它，而列表本身并没有提供这些值。它的主要特性是存储唯一的和无序的值。在 Python 中，在某些时候，你必须意识到所有的东西都是对象，默认情况下，我们的集合只存储每个对象的相同副本。集合在初始声明时用括号()声明。

在下面的示例中，我们有一个列表，其中包含汽车型号和品牌的元组:

```
brand_model = [("Fiesta", " Ford"), ("Gol", "VW"), ("KA", "Ford"), ("Uno", Fiat)]
unique_brands = set()

for model, brand in brand_model:
    unique_brands.add(brand)

>>> {'Fiat', 'Ford', 'VW'}
```

这个结果显示了我们独特的汽车品牌，一个重要的细节是，我们的 set 并不按照插入的顺序将结果返回给我们，就像字典一样，Set 并不保证其元素的顺序。我们可以利用集合的一个很好的用途是能够看到集合中有什么或没有什么。

## 主要特征:

*   无序的
*   不允许重复项目
*   与列表相比，访问速度更快
*   支持数学运算(并、交、差)

# 字典

根据我作为开发人员的经验，字典是使用 Python 操作数据的最简单和最有效的方式，因为您的问题和数据都可以使用字典。基本上，它在键和值存储中是有意识的，所以 redis 的主要目标是存储来自键-值对的信息，例如:color = blue，其中 color 是我的键，blue 是我的值。非常类似哈希内容，其他语言的 hashmap。在 Python 中，它是在两个大括号{}之间声明的，其内部项用逗号(，)分隔。

```
car = {'model': 'Ka', 'brand': 'Ford', 'Year': 2012}
```

## 主要特征:

*   无序的
*   存储不同类型数据的能力
*   可以嵌套
*   动态增长

# 结论

展示的每个系列都有自己的特色，每个系列都必须用于特定的场合。它们的结合使用为我们处理数据提供了极大的灵活性，并使我们解决实际问题的主要目标变得更加容易。

# 进一步阅读。

如果你想更深入地理解或者看到不同的方法，我在 Medium 上挑选了一些深入研究这个主题的文章。好好学习。

[](/pythons-collections-module-high-performance-container-data-types-cb4187afb5fc) [## Python 的集合模块——高性能容器数据类型。

### Python 集合模块的快速概述。

towardsdatascience.com](/pythons-collections-module-high-performance-container-data-types-cb4187afb5fc) [](https://blog.usejournal.com/python-basics-data-structures-d378d854df1b) [## Python 基础-数据结构

### Python 内置集合简介

blog.usejournal.com](https://blog.usejournal.com/python-basics-data-structures-d378d854df1b) [](https://medium.com/swlh/python-collections-you-should-always-be-using-b579b9e59e4) [## 您应该始终使用的 Python 集合

### Python collections 是一个被低估的库，它可以将您的编码提升到下一个级别。

medium.com](https://medium.com/swlh/python-collections-you-should-always-be-using-b579b9e59e4)