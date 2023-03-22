# 抽象基类以及如何在您的数据科学项目中使用它们

> 原文：<https://towardsdatascience.com/abstract-base-classes-and-how-to-use-them-in-your-data-science-project-2503c13704f4?source=collection_archive---------22----------------------->

![](img/256e8a2fa57a492b164bc63b5520a164.png)

公共许可证

## 通过使用这种面向对象的编程构建块，编写更干净、更安全的 python 代码

您是否曾经遇到过这样的代码，并且想知道这些类和方法有什么如此抽象？好吧，我要让你看看！

如果你能坚持足够长的时间，我还会分享一个小技巧，你可以用它在你的类中**自动化单元测试**。

# 遗产

面向对象编程(OOP)的一个关键概念是继承。继承意味着我们将一个(子)类建立在另一个(父)类的基础上。因此，子类从其父类继承某些属性，同时实现新的属性。

让我们看一个例子。假设你想写一个关于动物世界的软件库。你可能会从定义一个类*动物*开始:

```
class Animal: def __init__(self, height, weight):
        self.height = height
        self.weight = weight
```

动物是一个非常笼统的术语，这就是为什么您想要创建更具体的类:

```
class Dog(Animal):

    def make_sound(self):
        print('woof!')class Cat(Animal):

    def make_sound(self):
        print('meow!')

    def pounce(self):
        print('Pouncing at prey!')
```

在括号中写*动物*表示*狗*和*猫* **从*动物*类继承**。这意味着，不用写出来，两个动物都会自动实现 *__init__* 方法。此外，两个子类都实现了自己的方法:“make_sound”和“猛扑”。根据经验，如果你的两个类符合“A **是 a** B】:“狗**是一种**动物”这句话，那么继承是有意义的。

假设您想要实现一个 lion 类。狮子是猫，所以我们希望这个类继承自猫。这样一来，*猫*从*动物*那里继承的方法也同样适用于*狮*

```
class Lion(Cat):

    def make_sound(self):
        print('roar!')
```

*狮子*这个类现在有三个方法: *__init__* 、*猛扑*和 *make_sound* 。通过在 *Lion* 类中重新定义 *make_sound* ，我们已经覆盖了从 *Cat* 继承的版本。

# 抽象基类

抽象基类(ABC)提供了一种方法来管理这种继承，同时构造您的代码。一个 ABC 可以被看作是**脚手架**或者模板(不要和实际的模板混淆，想想:C++！)用于从它继承的所有类。

在 Python 中，可以通过继承 abc 来定义 ABC。ABC:

```
import abc 
class Animal(abc.ABC):
```

一个 ABC**并不意味着被实例化**，也就是说，一个人不应该(在某些情况下不能)创建一个 ABC 的对象。

```
my_animal = Animal(height=100, weight=80) #Don't do this!
```

那么，为什么还要使用抽象基类呢？因为，就像我之前说的，他们把**结构和安全**加到你的项目里。ABC“Animal”告诉未来开发您的代码的开发人员(或未来的您)从 Animal 继承的任何类应该如何操作。要做到这一点，ABC 有一个工具可供使用:

> **抽象方法**
> 
> 抽象方法是**必须由任何从 ABC 继承的类实现**的方法。

例如，我们可以决定代码中的每种动物都必须能够发出某种声音。然后我们可以定义:

```
from abc import ABC, **abstractmethod**class Animal(ABC): def __init__(self, height, weight):
        self.height = height
        self.weight = weight **@abstractmethod**
    def make_sound(self):
        passclass Bird(Animal): def fly(self):
        print('I am flying')
```

如果我们试图实例化任何没有实现抽象方法的子类，我们会得到以下错误:

```
tweetie = Bird(height=5, weight=1)>>> TypeError: Can't instantiate abstract class Bird with abstract methods make_sound
```

> 在一个新项目的开始，开发人员应该暂停一下，考虑哪些方法对于一个类的正常运行是绝对必要的。这些方法应该被声明为**抽象方法**，以避免将来出现意外的运行时错误。

在我看来，这在 Python 这样的非编译语言中尤其重要，这种语言在这些错误面前非常脆弱。

# **现实生活中的例子**

许多流行的数据科学和机器学习库使用抽象基类。其中一个库是 scikit-learn。本文开头的代码片段包含 scikit-learn 对一个“内核”基类的实现。核用于支持向量机、核脊和高斯过程方法等。它们可以被视为两个向量之间的距离度量。您可能熟悉径向基函数(RBF)或平方指数核

![](img/40b3f94ac60dff88ebd88bc4d90ba677.png)

但是还有许多其他的存在并在 scikit-learn 中实现。

所有这些内核的共同点是(在 scikit-learn 中)它们继承自*内核* ABC:

通过使用 *__call__* 和 *is_stationary* 抽象方法，scikit-learn 确保每个新内核都将实现这些方法。至少在 *__call__(self，X，Y)* 的情况下，这是非常合理的，作为在两个输入矩阵上评估内核的函数是绝对必要的。没有它，内核就没有多大用处。

# 注册和单元测试

为内核类的每个新的子类自动生成单元测试不是很好吗？

谢天谢地，有！

## 注册

通过创建一个所谓的**注册表**，我们只需要实现一个单元测试一次，就可以在我们的类中应用它(人们可以用注册表做其他有趣的事情，但是我不会在这里详细讨论)。

注册表只是记录你创建的所有类。为了用 Python 实现注册中心，我们需要使用**元类**。

什么是元类？

看待它的一种方式如下:

> 类告诉 Python 类的实例如何行为，而元类定义了类本身的行为。[1]

因此，通过使用元类，我们可以告诉 Python:“每当实现一个新类时，在注册表中创建一个新条目”

如果这一切听起来非常混乱，您也可以将以下代码复制粘贴到您的项目中:

*(注意，原则上注册中心不必继承 ABCMeta，但是将两者结合起来通常是有意义的。)*

让我们回到我们的内核例子。我们现在写的不是从 ABC 继承的

```
class Kernel(**metaclass=ABCRegistry**):   
    **_registry_name = 'kernel'**
    ...class RBF(Kernel):
    **_registry_name = 'rbf'**
    ...class DotProduct(Kernel):
    **_registry_name = 'dotproduct'**
    ...
```

通过创建 attribute _registry_name，我们告诉我们的注册表如何调用这些类。访问注册表将产生以下输出:

```
Kernel.get_registry()
>>> {'kernel': Kernel, 'rbf': RBF, 'dot': Dot}
```

## 单元测试

假设我们想要测试我们实现的内核是否是对称的:

> **K(X，Y) = K(Y，X)**

每个有效的内核都必须遵守这个条件，所以测试它是很有意义的。

使用 **pytest** 及其*参数化*功能，我们现在可以自动将测试应用到从“内核”继承的每个类:

当然，人们总是可以显式地循环所有相关的类。使用注册表的好处是，添加新类的贡献者也不需要担心向适当的测试添加条目。

*如果你喜欢这篇文章，请随时关注我，在*[*Twitter*](https://twitter.com/semodi92)*或在*[*LinkedIn*](https://www.linkedin.com/in/sebastianmdick/)*上联系。*

**参考文献**

[1]大致基于托马斯·伍特斯对斯塔克伟福的解释。