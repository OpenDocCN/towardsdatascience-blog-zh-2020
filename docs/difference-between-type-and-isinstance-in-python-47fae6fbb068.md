# Python 中 type()和 isinstance()的区别

> 原文：<https://towardsdatascience.com/difference-between-type-and-isinstance-in-python-47fae6fbb068?source=collection_archive---------22----------------------->

## Python 初学者

## Python 中的实例是什么？

![](img/22259ec40af1d35c27a559364cd4c435.png)

克里斯汀娜·戈塔迪在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

上次我们谈到了 Python 中的 4 个内省工具，每个初学者都应该使用。如果你还没有读过这篇文章，可以看看下面这篇文章。最先引入的两个函数是`type()`和`isinstance()`。

[](/4-easy-to-use-introspection-functions-in-python-49bd5ee3a2e8) [## Python 中 4 个易于使用的自省函数

### 如何在运行时检查对象

towardsdatascience.com](/4-easy-to-use-introspection-functions-in-python-49bd5ee3a2e8) 

这里简单回顾一下。

Python 中的每个对象都有一个数据类型，一个内置的或者定制的。可以是整数`int`，字符串`str`，T21【NumPy】数组等..也可以是自定义类的对象。

给定某个对象`obj`，`type(obj)`返回该对象的数据类型。如果对象是`dtype`的实例，则`isintance(obj, dtype)`返回`True`，否则返回`False`。

那么 ***到底是某个类的一个实例*** 呢？

# 子类和继承

重要的事情先来。我们需要理解子类和继承。以下面的代码为例:

我们将类`Rectangle(Shape)`定义为`Shape`的子类，将`Square(Rectangle)`定义为`Rectangle`的子类。子类 ***继承其超类的*** 方法、属性和其他功能。

我们以一种方式定义层次结构，使得类`Square`的对象继承`Rectangle`的所有属性和方法。毕竟，正方形是矩形的特例，所有边的长度都相同。

![](img/332de68d13b0abafa5a3c187afe4814b.png)

[Edvard Alexander lvaag](https://unsplash.com/@edvardr?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果我们用`length=5`创建一个`Square`对象`a`，我们可以调用`get_area()`方法来计算它的面积。注意，这个方法没有在`Square`类中显式定义，而是在它的超类中指定，例如`Rectangle`。

```
a = Square(5)
a.get_area() # 25
```

因此我们说`Square(Rectangle)` ***从它的超类`Rectangle`继承了*** 方法`get_area()`。

# 情况

现在你应该对子类和继承有了基本的了解。但是和`type()`和`isinstance()`有什么关系呢？考虑以下关于方形物体`a`的陈述。这两个语句会返回什么？两个`True`？两个`False`？

```
type(a) == Rectangle
isinstance(a, Rectangle)
```

首先，我们肯定地知道`type(a)`返回`Square`。并且类别`Square`不等于类别`Rectangle`。所以第一条语句返回`False`。

```
Square == Rectangle # returns False
```

但是，第二条语句会返回`True`！因为`Square`是`Rectangle`的**子类**，所以对象`a`是`Rectangle`的**实例**。

***子类的实例也是基类的实例。***

![](img/01f2b2041f585fa5bdd4d4eac6208a09.png)

[梁杰森](https://unsplash.com/@ninjason?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 外卖

综上所述，`isinstance()`考虑了**继承**，但是使用 **equality** ( `==`)语句检查对象的`type()`却没有。

***一个子类不等于它的基类。***

感谢阅读！你可以[注册我的时事通讯](http://edenau.mailchimpsites.com/)来接收我的新文章的更新。如果您对提高 Python 技能感兴趣，以下文章可能会有所帮助:

[](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4) [## 我希望我能早点知道的 5 个 Python 特性

### 超越 lambda、map 和 filter 的 Python 技巧

towardsdatascience.com](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4) [](/6-new-features-in-python-3-8-for-python-newbies-dc2e7b804acc) [## Python 3.8 中针对 Python 新手的 6 项新特性

### 请做好准备，因为 Python 2 不再受支持

towardsdatascience.com](/6-new-features-in-python-3-8-for-python-newbies-dc2e7b804acc)