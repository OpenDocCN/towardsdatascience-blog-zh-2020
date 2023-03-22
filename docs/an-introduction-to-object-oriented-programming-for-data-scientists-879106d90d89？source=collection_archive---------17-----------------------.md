# 面向数据科学家的面向对象编程介绍

> 原文：<https://towardsdatascience.com/an-introduction-to-object-oriented-programming-for-data-scientists-879106d90d89?source=collection_archive---------17----------------------->

## [入门](https://towardsdatascience.com/tagged/getting-started)

## 面向对象的基础知识，适合那些以前可能没有接触过这个概念或者想知道更多的人

![](img/f58af33f68fad1b68bef3187f8ce224c.png)

照片由[埃米尔·佩伦](https://unsplash.com/@emilep?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

首先，什么是面向对象编程？这就是所谓的编程范式，本质上意味着它是做某事或构建代码的一种特定方式。OOP 的意思是你围绕对象构建你的代码，这有利于构建框架和工具，或者使代码更加可用和可伸缩。这些对象本质上是将数据和方法存储在一个结构中，可以反复使用该结构来创建该结构的新实例，这样您就不必重复了。这样做的好处是，您可以让不同的对象和实例相互交互，同时在单个结构中存储属性和行为。

这与数据科学中传统使用的过程化编程形成对比。在这里，代码遵循一系列步骤，使用特定的代码块来完成任务。这可以利用函数，这些函数可以在整个脚本中多次循环使用，但通常遵循给定的使用顺序，这在 Jupyter 笔记本中很常见。

两者的区别在于，过程化编程可以被认为是关注于需要做什么，而 OOP 关注于构成整个系统的结构[1]。

OOP 范例通过类来创建对象，这些类被用作创建新对象的蓝图。该类描述了对象的总体情况，但与对象本身是分开的。这里的介绍主要是向您介绍面向对象编程中使用的结构，这样当您在代码中遇到它们时，您就可以理解对象的基本结构。

**定义一个类**

因此，要做的第一件事是定义一个新的类，这个类是使用关键字`class`创建的，后面是包含方法的缩进代码块(方法是对象的一部分的函数)。

对于我们的例子，我们可以使用一个需要存储雇员信息的公司。这方面的一个例子如下:

```
class Employee: pass
```

这将创建 Employee 类(该名称的标题是遵循 [CamelCase](https://en.wikipedia.org/wiki/Camel_case#:~:text=Camel%20case%20(stylized%20as%20camelCase,word%20starting%20with%20either%20case.) 逻辑)，它目前只使用`pass`参数，因此不做任何事情。我们可以如下创建一个雇员的实例，方法是调用该类并将其赋给一个名为 Steve 的变量。

```
Steve = Employee()
```

然后，我们可以使用下面这段代码来检查 Steve 是否是雇员:

```
Steve.__class.__name__# out: 'Employee'
```

在这里，`.__class__`检查类的类型，而`.__name__` 用它只打印类名。从这里可以看出，我们可以看出 Steve 是一名员工。

**添加属性**

创建类的下一步是开始向我们的 Employee 类添加属性。这是通过使用`__init__()`方法来完成的，该方法在创建类的实例时被调用。本质上，这允许您将属性附加到任何新创建的对象上。

出于我们的目的，我们可以为员工指定工资、级别和为该公司工作的年数:

```
class Employee:

    def __init__(self, wage, grade, years_worked):
        self.wage = wage
        self.grade = grade
        self.exp = years_worked

Steve = Employee(25000, 3, 2)
```

这里值得注意的是，所有方法都必须以`self`属性开始，虽然在创建实例时没有显式调用，但它用于表示类的实例。因此，我们指定`self.wage`能够存储实例的工资。

为了访问创建的每个属性，我们可以使用点符号，这意味着我们将`.`和属性放在实例名称的后面。在这种情况下，访问 Steve 的属性如下:

```
print("Steve's wage is:", Steve.wage)
print("Steve's grade is:", Steve.grade)
print("Steve has worked for", Steve.exp, "years")# out: Steve's wage is: 25000
Steve's grade is: 3
Steve has worked for 2 years
```

使用`__init__()`方法创建的属性是实例属性，因为它们的值特定于类的特定实例。在这里，虽然所有雇员都有工资、级别和工作年限，但它们将特定于所创建的类的实例。

我们也可以通过在`__init__()`方法之前赋值来为所有类实例设置具有相同值的类属性。在这里，我们可以指定员工工作的公司，假设这些都是同一家公司的，我们可以像访问属性一样访问这些信息:

```
class Employee:

    #class attribute
    company = "Data Sci"

    #instance attributes
    def __init__(self, wage = 20_000, grade=1, years_worked=0):
        self.wage = wage
        self.grade = grade
        self.exp = years_worked#create an instance of Julie as an employee
Julie = Employee(40000, 3, 5)#print out the company name
print("Julie works for:", Julie.company)#out: Julie works for: Data Sci
```

**添加方法**

既然我们已经添加了属性，那么我们可以开始向我们的对象添加方法或行为。

`__init__()`是我们已经介绍过的一种用于分配属性的方法，但是我们可以开始定义自己的方法来执行某些动作或与某些行为相关联。这可能包括更改它们的属性或执行某些操作。

对于我们的员工，我们可以创建一种方法，通过这种方法我们可以给他们升职，工资增加 5000 英镑，级别增加 1 级。这可以定义如下:

```
class Employee:

    def __init__(self, wage = 20_000, grade=1, years_worked=0):
        self.wage = wage
        self.grade = grade
        self.exp = years_worked

    def promotion(self):
        self.wage += 5000
        self.grade += 1
```

通过生成一个名为 Sarah 的新员工并给她升职，可以看到这样做的结果。我们可以通过查看她晋升前后的工资来检查差异:

```
Sarah = Employee(50000, 5, 12)#Checking the original objects attributes
print("Sarah's wage is:", Sarah.wage)
print("Sarah's grade is:", Sarah.grade)
print("Sarah has worked for", Sarah.exp, "years\n")#Giving Sarah an promotion
print("Sarah has got a promotion\n")
Sarah.promotion()#Checking to see that the grade and wage have changed
print("Sarah's wage is now:", Sarah.wage)
print("Sarah's grade is now:", Sarah.grade) # out: Sarah's wage is: 50000
Sarah's grade is: 5
Sarah has worked for 12 years

Sarah has got a promotion

Sarah's wage is now: 55000
Sarah's grade is now: 6
```

为此可以使用其他方法来改变特性或执行某些行为。例如，可以使用一种方法来定义一个周年纪念日，从而为他们的经历添加一年，或者我们可以添加一种方法来比较两个对象的资历。你可以自己测试一下！

**进一步 OOP**

定义属性和将方法附加到对象上仅仅是 OOP 的开始，还有更多的东西需要探索。这包括类继承，由此可以定义原始类的子类，例如基于 Employee 类的基础创建经理、数据科学家、分析师和其他雇员类型。这些子类本质上继承了初始父类的属性和行为，但是可以添加更多的功能。还有对象比较，通过它可以比较对象，因为您可以设置哪些属性用于比较操作，例如==，！=，≥或≤。有一种字符串表示法，可以用一个字符串来表示一个对象，这样就可以很容易地创建该实例的副本。还有更多…

这篇文章只是给出了一个进入 OOP 世界的基本入口，并不包括定义属性或创建方法。然而，希望这能让你更详细地理解 OOP、类和对象，这样当你遇到它们的时候，你就能理解一些基础知识和它们的结构。

[1][https://isaacuterscience . org/concepts/Prog _ OOP _ paradigm？topic =面向对象编程](https://isaaccomputerscience.org/concepts/prog_oop_paradigm?topic=object_oriented_programming)

[](https://philip-wilkinson.medium.com/membership) [## 通过我的推荐链接加入媒体-菲利普·威尔金森

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

philip-wilkinson.medium.com](https://philip-wilkinson.medium.com/membership) [](/introduction-to-decision-tree-classifiers-from-scikit-learn-32cd5d23f4d) [## scikit-learn 决策树分类器简介

towardsdatascience.com](/introduction-to-decision-tree-classifiers-from-scikit-learn-32cd5d23f4d) [](/ucl-data-science-society-python-fundamentals-3fb30ec020fa) [## UCL 数据科学协会:Python 基础

### 研讨会 1: Jupyter 笔记本，变量，数据类型和操作

towardsdatascience.com](/ucl-data-science-society-python-fundamentals-3fb30ec020fa) [](/introduction-to-random-forest-classifiers-9a3b8d8d3fa7) [## 随机森林分类器简介

### 预测 NBA 球员的位置——我们正在看到一个真正的“无位置”联盟吗？

towardsdatascience.com](/introduction-to-random-forest-classifiers-9a3b8d8d3fa7)