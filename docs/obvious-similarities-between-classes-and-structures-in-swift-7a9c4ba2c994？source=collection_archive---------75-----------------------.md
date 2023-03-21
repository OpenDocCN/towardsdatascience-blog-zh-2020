# Swift 编程语言中类和结构之间的(明显)相似性

> 原文：<https://towardsdatascience.com/obvious-similarities-between-classes-and-structures-in-swift-7a9c4ba2c994?source=collection_archive---------75----------------------->

## 技术的

## 在 Swift 编程语言中，结构和类是不同的对象类型，但它们确实有一些相似之处。本文探讨了其中的一些相似之处。

![](img/917e9a2c5696d5ab7a2c78626255d91c.png)

克里斯托夫·高尔在 [Unsplash](https://unsplash.com/s/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 介绍

本文旨在展示 Swift 编程语言中两种不同的对象类型——类和结构之间的相似性。

您不需要具备丰富的 Swift 使用经验就能理解本文中的示例和相似之处。

本文中的内容可用作 Swift 中的类和结构对象的复习或介绍。

*但是为什么还要麻烦斯威夫特呢？*

Swift 是一种通用的编程语言，用于 iOS 应用程序开发和机器学习等任务。它可能是一种需要掌握的自然语言，尤其是对于那些有 C 或 Python 等语言经验的开发人员来说。

下面是一些相似之处的快速总结，然后是深入的探索。

# 1.创造

**在 Swift 中创建结构或类别的过程几乎完全相同。**

对于结构的定义，您只需键入'*struct '*(Swift 中识别的关键字语法)，然后是所需的结构名称，最后是相应的左花括号和右花括号。

```
struct NewCustomStructure {
  // Properties, methods...
}
```

类遵循与结构相似的定义模式。只需输入可识别的关键字' *class'* ，然后输入所需的类名，最后输入花括号。

```
class NewCustomClass {
  // Properties, methods...
}
```

结构和类命名通常遵循相同的命名模式，即[upper case](https://whatis.techtarget.com/definition/UpperCamelCase)模式(每个新单词的第一个字母大写)。

# 2.例子

熟悉面向对象编程语言(如 Java、Python 或 JavaScript)的有经验的程序员将熟悉创建类实例的概念。

对于那些不是这样的人来说，创建类的实例可以被描述为将类视为蓝图，而类的实例是从蓝图(类)构建的对象。

**在 Swift 中，您可以创建一个类实例，也可以创建一个结构实例。**

类/结构实例的创建遵循类似的模式。我们将创建先前创建的结构' *NewCustomStructure'* 和类' NewCustomClass' *的新实例。*

将创建结构实例，如下所示:

```
let structureInstance = NewCustomStructure()
```

和类实例的创建如下所示:

```
let classInstance = NewCustomClass()
```

# 3.创建、定义和访问类属性

类和结构都可以定义属性。

属性可以被认为是可区分的数据容器/占位符，它们与创建它们的类或结构直接相关。变量和常量是类和结构的属性。

当一个类或结构的实例被创建时，我们可以在它们的属性中定义数据。

属性在类和结构中以相同的方式创建。

为了说明我们如何在类和结构中创建、定义和访问属性，我们将使用结构来定义房子，使用类来定义人。

```
struct House {
  var noOfDoors: Int
  var noOfWindows: Int
  let noOfRooms: Int
  let noOfFloors = 2
}
```

上面是一个结构定义，它代表了一个具有一些在实际住宅中可以找到的明显属性的房子。

下面的代码片段显示了 house 结构的一个实例，该实例是用该实例的预期属性值创建和初始化的。

```
let smallHouse = House(noOfDoors:2, noOfWindows:3, noOfRooms: 6)
print(smallHouse)>> House(noOfDoors: 2, noOfWindows: 3, noOfRooms: 6, noOfFloors: 2)
```

可以使用点标记法访问 struct 实例的已定义属性。

```
print(smallHouse.noOfDoors)
print(smallHouse.noOfWindows)
print(smallHouse.noOfRooms)
```

为了说明类中的属性是以类似于结构的方法来访问的，我们将创建一个类并使用点符号来访问它的属性。

```
class Person {
    var firstName: String
    var lastName: String
    var age: Intinit(firstName:String, lastName:String, age:Int) {
        self.firstName = firstName
        self.lastName = lastName
        self.age = age
    }
}let kindPerson = Person(firstName: "James", lastName: "John", age:23)print(kindPerson.firstName)
print(kindPerson.lastName)
print(kindPerson.age)
```

这篇短文介绍了这两种对象类型、类和结构之间的主要相似之处。

我还介绍了这两种对象类型之间的区别。有关主要差异的信息可通过下面的链接获得。

[](https://medium.com/swlh/differences-between-classes-and-structures-in-swift-ab2e27956665) [## Swift 中类别和结构之间的差异

### 虽然类和结构有一些明显的相似之处，但本文列出了结构之间的差异…

medium.com。](https://medium.com/swlh/differences-between-classes-and-structures-in-swift-ab2e27956665)