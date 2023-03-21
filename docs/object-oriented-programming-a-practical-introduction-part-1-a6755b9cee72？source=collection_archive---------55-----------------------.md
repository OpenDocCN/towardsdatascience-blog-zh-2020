# 面向对象编程:实用介绍(第一部分)

> 原文：<https://towardsdatascience.com/object-oriented-programming-a-practical-introduction-part-1-a6755b9cee72?source=collection_archive---------55----------------------->

![](img/61e60afef51ee5df90b258ba6cdddce7.png)

凯利·麦克林托克在 Unsplash

# 面向对象的力量

如果你已经编程至少有一段时间了，你可能会遇到(也许使用过)面向对象编程(OOP)的概念和语言特性。自 90 年代中期以来，这种编程范式一直是软件工程中的核心概念，可以为程序员提供一些非常强大的功能——尤其是在小心使用时。

然而，对于许多程序员来说，围绕着 OOP 这样的概念打转很多年并不罕见——也许在这里或那里获得了一点点洞察力——但是没有将这种理解整合成一组清晰的想法。对于初学者来说，OOP 的概念可能有点令人困惑，一些指南利用特定语言的 OOP 实现来说明思想，许多指南使用微妙不同的重载语言，所有这些反过来有时会在更一般的意义上混淆 OOP 概念。

这篇文章旨在对 OOP 的核心概念进行一次简短而实用的浏览。它会给你一些例子和用 Python 写的比较，比较你可能已经在写的代码类型和 OOP 简化的代码类型。它的目标是那些熟悉编程，但对 OOP 的正式理解有限，并且希望加深对这一领域的理解的人。它不会深入探讨 OOP 的具体方面和应用，也不会深入探讨你在不同语言实现中可能遇到的 OOP 的各种“风格”——它会尽可能保持一般性。如果这听起来不错，请继续阅读！

# 初学者的方法

让我们开始吧。假设您正在编写一个程序来计算一组形状的总面积。也许你正在开发一个绘图工具，或者一个边界框类型的计算机视觉问题。你知道你需要计算一组矩形和圆形的总面积。很简单，对吧？你设置并编写两个函数:

```
import math def area_of_rectangle(length, width): 
    return length * width def area_of_circle(radius): 
    return math.pi * radius ** 2
```

太好了。一个美好的全新开始。现在，要计算一组特定形状的面积，您可以写:

```
area_of_rectangle(10, 5) + area_of_circle(5) + area_of_circle(10)
```

如果你只需要计算这三个特定的形状，那么恭喜你，你已经完成了！但是如果你需要计算任意的圆和矩形集合呢？也许你会这样做:

```
circles = [5, 10, 25] # each element is a radius of a given circle. rectangles = [(10, 5), (6, 8)] # each element is (length, width) pair for a given rectangle. # calculate area of rectangles 
area_rectangles = 0 
for (l, w) in rectangles: 
    area_rectangles += area_of_rectangle(l, w) # calculate area of circles
area_circles = 0 
for r in circles: 
    area_circles += area_of_circles(r) # get total area total_area = area_circles + area_rectangles
```

同样，简单明了，和以前一样:也许你需要做的就是。但是您可能已经能够看到一些问题悄悄进入这段代码:每个形状都需要自己的`for`循环，并且每个循环在结构上非常相似，这导致了一定程度的代码重复。然而，如果你完全确定你只会被要求计算一组圆和矩形的总面积，那么这是很棒的。

也就是说，如果你在混乱的商业软件世界中工作，你也许能够预测到将要发生什么。你的“上游”有人发布了一个新功能，将*三角形*引入到形状的混合中，他们也需要你计算这些三角形的面积。现在，在上面的代码中，您可以添加如下内容:

```
def area_of_triangle(base, height): 
    return (base * height) / 2.0
```

然后，您可以添加*另一个* `for`循环，*另一个`triangles`的*列表，并更新`total_area`以反映您的需求。很简单，但是你的代码可能开始看起来重复和有点冗长。也许这还可以忍受，在这种情况下，很公平。

*然而*，你现在得到一个请求，让你的程序计算每个形状的面积*和周长*。你是做什么的？您可以添加以下内容:

```
def perimeter_of_rectangle(length, width): 
    return 2*length + 2*width
```

对于每个新的形状，你必须复制(甚至复制粘贴！😱)更多的代码行来支持每个新的形状或操作。

此外，计算面积(和周长)的核心问题由于周围的逻辑变得有点混乱。更糟糕的是，随着代码的增长，出现错误变得越来越容易，调试这类问题变得越来越困难，新人也更难学会和改变。虽然这个具体的例子有点做作，但总的来说，这种方法不太具有可扩展性，相当冗长，并且有很多代码重复。当事情开始变得越来越复杂时，所有这些都会使您的代码变得有点可怕——在专业环境中，这是不可避免的。

# 走向面向对象

这就是(谨慎应用)OOP 的好处所在。又到了从头开始计算形状面积的时候了。一眼看去，很明显你上面的小程序有很多结构上的相似性——你有一组形状，每个形状都可以说有一个面积。*改变*的是定义每个形状所需的信息:圆形的`radius`、矩形的`length`和`width`，以及这些信息如何用于计算面积。如何以编程方式捕捉这种观察？这就是`classes`能派上用场的地方。这里有一个新的`Shape`类:

```
class Shape: 
    def area(self) -> float: ...
```

这告诉你什么？它说有一个名为`Shape`的结构，它附带了一个*方法* `area`。换句话说:所有的`Shape`都有一个与之相关的区域。您可能认为`area`看起来很像一个标准的 Python 函数，您是对的。作为类的*成员*的函数通常称为*方法*。

重要的是，这个特定的片段可以说是定义了一个*抽象*类:你对`Shape`有一个完全抽象的“概念”。它很抽象，因为它没有定义*如何计算* `area`，只是说所有的`Shape`都有一个你可以访问的区域。为什么*这个*有帮助？现在你可以定义*的子类* -抽象`Shape`的特定变体，为*的特定*形状实现这些信息。下面是一个`Rectangle`和`Circle`可能使用的样子:

```
class Rectangle(Shape): def __init__(self, length: float, width: float) -> None:
        self.length = length 
        self.width = width def area(self) -> float: 
        return self.length * self.width 
class Circle(Shape): 
    def __init__(self, radius: float) -> None: 
        self.radius = radius     def area(self) -> float: 
        return math.pi * self.radius ** 2
```

让我们打开包装。您会看到每个子类都是按照`class Circle(Shape)`的思路定义的。你可以这样理解“a `Circle`是`Shape`的一种”。从技术上讲，这反过来意味着`Circle`将立即从`Shape`继承*的`area`方法。逻辑上:所有的`Circle`也都有一个`area`。这里你可以看到，对于每个形状，现在已经针对所讨论的形状实施了`area`方法。*

你还会看到现在这些类上也有一个奇怪的方法`__init__`。在 Python 中，这在技术上被称为“初始化器”。然而，它在功能上非常类似于更广为人知的*构造函数方法*，出于一般性考虑，本文将这样称呼它(两者之间的区别将在*稍后*讨论)。

一个*构造器*是一个特殊的方法，它提供了如何构造一个给定类的新*实例*的指令。在上面的例子中，你可以看到两种形状之间的“可见”差异在构造函数中被捕捉到:a `Rectangle`需要`length`和`width`，而`Circle`需要`radius`。这意味着所有其他的区别(例如`area`是如何计算的)对代码的其余部分是隐藏的。看看这对您的初始示例意味着什么:

```
shapes = [Circle(5), Rectangle(10, 5), Circle(10), Rectangle(6, 8), Circle(25)] area = 0 
for shape in shapes: 
    area += shape.area()
```

如您所见，核心代码本身并不冗长，而且可以说更容易理解:给定一组形状，迭代这些形状并求和它们的面积。您甚至可以选择使用一个*生成器表达式*来代替:

```
area = sum(shape.area() for shape in shapes)
```

更加简洁的*和明确的*。显然，`area`的实现对于这个逻辑来说是不重要的(并且是不可见的)。

# 扩展您的代码

现在，回到在代码中引入新特性的问题。在最初的例子中，您需要添加对计算三角形面积的支持。使用你所看到的，你可以创建一个新的形状类型`Triangle`，如下所示:

```
class Triangle(Shape): 
    def __init__(self, base: float, height: float) -> None:
        self.base = base 
        self.height = height     def area(self) -> float: 
        return (self.base * self.height) / 2.0
```

同样，`area`的实现是特定于类的，但是使用类的实例对代码*隐藏。因此，代码的核心逻辑(在某种意义上是“有趣的部分”)保持不变。你所需要做的就是在你的`Shape`列表中添加一个`Triangle`，这个想法更进一步:*一个类的任何*实例，只要是一个‘有效的’`Shape`就可以用在任何一个可以使用`Shape`的地方*，而不需要其他的改变*。您可以将*抽象* `Shape`类视为定义了一个*契约*，所有使用`Shape`的代码都可以依赖它来提供所需的功能(在本例中，是形状的`area`)。具体来说，您的`shapes`列表现在应该是这样的:*

```
shapes = [Circle(5), Rectangle(10, 5), Circle(10), Rectangle(6, 8), Circle(25), Triangle(10, 5)]
```

您可能会看到，随着一些基本 OOP 概念的引入，代码变得更具可扩展性。您可以添加任意数量的形状变体，并且不需要做任何工作来改变您的“核心”逻辑。您可能会注意到，这也将您的“业务逻辑”(一组形状的总面积)与实现细节(`Shape`本身)隔离开来。

但是等等——还有一个要求:然后要求你也计算每个形状的*周长*。你是怎么做到的？

```
class Shape: 
    def area(self) -> float: ... 
    def perimeter(self) -> float: ... class Rectangle(Shape): 
    ... 
    def perimeter(self) -> float: 
        return (self.length + self.width) * 2.0
```

在这种情况下，您可以看到一个新的*方法* `perimeter`被添加到了`Shape`类中。如你所料，这通知程序所有有效的`Shape`类都应该实现`perimeter`方法。回到你的`Rectangle`类，你现在可以实现如代码片段所示的`perimeter`方法。你们也可以为彼此做同样的事情`Shape`。注意，为了简洁起见，这里的省略号被用作上面定义的`__init__`和`area`方法的简写。

然而，你可能在这里先占另一个问题:通过`base`和`height`单独计算三角形的`perimeter`不同于三角形的*类型*，但是`area`保持不变。在这种情况下，使*成为`Triangle` : `RightTriangle`和`EquilateralTriangle`的子类*可能是有意义的:

```
class RightTriangle(Triangle):    
    def perimeter(self) -> float:        
    	return self.base + self.height + math.sqrt(self.base**2 + self.height ** 2)

class EquilateralTriangle(Triangle):    
    def __init__(self, base: float) -> None:        
    	super().__init__(base, base)

    def area(self):
    	return (math.sqrt(3) * self.base ** 2) / 2.0

    def perimeter(self) -> float:        
    	return 3.0 * self.base
```

*这个*是什么意思？您会注意到，在这两种情况下，现在都有了一个特定类型的`perimeter`方法实现。您可能还注意到`RightTriangle`没有使用`Triangle`上定义的构造函数方法。这是因为这种类型的`Triangle`的构造函数与父类相同，并且通过不覆盖*子类`RightTriangle`中的这个方法将告诉该类使用默认的构造函数。*

相比之下，`EquilateralTriangle` *不会覆盖`__init__`方法。显然，对于一个等边三角形，你只需要一条边的长度就可以完全确定它的形状。这里，构造函数被修改为只需要`base`来反映这一点。您将会看到实现执行了第`super().__init__(base, base)`行。这一行调用*父类* `Triangle`的构造函数，参数位置参数`base`和`length`被映射到`base`。*

把这些放在一起，你会有:

```
shapes = [Circle(5), Rectangle(10, 5), Circle(10), Rectangle(6, 8), Circle(25), RightTriangle(10, 5), EquilateralTriangle(5)] area = 0 
for shape in shapes: 
    area += shape.perimeter()
```

希望您可以看到对“核心”逻辑的更改是最小的，这个小程序现在比最初的例子更具可扩展性。您已经创建了一个由您的类层次结构捕获的*本体*。这允许你通过*扩展*你的类层次*或者*更新你的核心业务逻辑来给你的代码添加新的特性。您可能会看到，这(在很大程度上)分离了这些任务，使得新用户更容易专注于一个或另一个。你会问，这在实践中有什么用？

让我们暂时撇开形状不谈。想象一下，你正在为一家类似于 [Stripe](https://stripe.com/gb) 的公司工作(一家数字支付公司)。您有一些代码来处理平台上发生的每个事务。您已经定义了一个简单的`Transaction`类来捕获关于事务的信息。然后，您的业务逻辑管道获取该类的实例，检查欺诈迹象，更新您的内部交易记录，向您的客户发送推送通知，然后将交易元数据归档到某个地方。

这个流程可能非常复杂，显然性能和可靠性对您的服务至关重要。您可能会有一组测试套件围绕这个管道，也许还有一个特别健壮的代码审查过程。基本上:你不想对你的代码做虚假的修改。然而，商业世界*喜欢虚假的变化。在本例中，可能法规发生了变化，或者您需要在一个新的区域运营，这需要您处理额外的数据。实际上，你仍然想要一个设计良好的渠道——你不想为每一个小的业务变化或每一个新的领域更新整个渠道。*

通过这个例子，你可能会明白如何应用本文中的概念。通过使用 OOP 技术的组合将变化的东西(即事务信息)从管道实现中隔离出来，您可以减少您遇到的每个新的特定于业务的用例需要更改的代码量，同时保持您的业务关键代码坚如磐石。酷吧。

# 直到下次

第 1 部分到此为止。您已经看到了 OOP 在玩具问题上的一些实际能力，以及它们如何改变您对设计和构建软件的看法。您还看到了一个简单的例子，说明了 OOP 对于更“真实”的应用程序的好处。希望你已经发现它有用。

然而，到目前为止，您已经避开了对支撑 OOP 的语言和概念的更技术性的研究。此外，你可能也看到了对 OOP 应用过于乐观的看法:在使用 OOP 时有相当多的理由要谨慎，在将它应用到你的项目之前理解这些理由是很重要的。

幸运的是，这是您将从第 2 部分中得到的！下次请继续收听！

如果您有任何问题或反馈，[通过 Twitter](https://twitter.com/MarklDouthwaite) 与我联系。

*原载于 2020 年 9 月 14 日*[*https://mark . douthwaite . io*](https://mark.douthwaite.io/object-oriented-programming-a-whistlestop-tour/)*。*