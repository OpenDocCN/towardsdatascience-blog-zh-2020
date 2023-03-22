# Python 类继承

> 原文：<https://towardsdatascience.com/python-class-inheritance-62fdb33ede47?source=collection_archive---------53----------------------->

## Python 继承简介

![](img/319ec2e77748f0eed5325e83d8b0827e.png)

[来源](https://www.pexels.com/photo/family-of-four-walking-at-the-street-2253879/)

类和对象构成了 python 编程语言的核心功能。类提供了一种组织属性(数据)和方法(作用于数据的函数)的便捷方式。面向对象编程中的一个重要概念是类继承。继承允许我们定义一个从父类获取所有功能的类，同时添加额外的数据和/或方法。在这篇文章中，我们将定义一个父类和子类，并演示继承的用法。

我们开始吧！

首先，让我们定义一个名为“Spotify_User”的类。该类将包含一个“__init__()”方法，该方法允许我们初始化用户对象值，如名称、电子邮件和高级用户状态:

```
class Spotify_User:
    def __init__(self, name, email, premium):
        self.name = name
        self.email = email
        self.premium = premium 
```

接下来，让我们定义一个允许我们检查用户是否是高级用户的方法:

```
def isPremium(self):
        if self.premium:
            print("{} is a Premium User".format(self.name))
        else:
            print("{} is not a Premium User".format(self.name))
```

现在，让我们定义两个“Spotify_User”对象:

```
user_1 = Spotify_User('Sarah Phillips', 'sphillips@gmail.com', True)
user_2 = Spotify_User('Todd Grant', 'tgrant@gmail.com', False)
```

我们可以使用“isPremium”方法检查用户是否为“高级用户”:

```
user_1.isPremium()
user_2.isPremium()
```

![](img/03291c7673c3ed517c6f98072d4b0e32.png)

现在我们已经定义了父类，让我们定义一个名为‘Premium _ User’的子类。类名遵循“ChildClass(ParentClass)”结构，其中父类是子类的一个参数。让我们假设 Spotify 有不同的订阅层级。子类构造函数将初始化属性“subscription_tier”:

```
class Premium_User(Spotify_User):
    def __init__(self, subscription_tier):
        self.subscription_tier = subscription_tier
```

接下来，我们将使用“super()”方法，该方法允许子类调用父类构造函数:

```
class Premium_User(Spotify_User):
    def __init__(self, subscription_tier, name, email, premium):
        self.subscription_tier = subscription_tier
        super(Premium_User, self).__init__(name, email, premium)
```

现在，让我们向‘Premium _ User’类添加一个额外的类方法。此方法将检查实例的“subscription_tier”值，并打印该层可用的内容:

```
class Premium_User(Spotify_User):
    ...
    def premium_content():
        if self.subscription_tier == 'Tier 1':
            print("Music streaming with no advertisements")
                if self.subscription_tier == 'Tier 2':
            print("Tier 1 content + Live Concert Streaming")
```

现在，让我们定义一个拥有“第 1 层”订阅的新用户:

```
user3 = Premium_User('Tier 1', 'Megan Harris', '[mharris@gmail.com](mailto:mharris@gmail.com)', True)
```

让我们使用父类方法“isPremium”来检查我们的新用户是否是高级用户:

```
user3.isPremium()
```

![](img/92ae98c3063b92a7677964b3d0a66a9c.png)

接下来，我们可以使用子类“premium_content”中的方法来查看“第 1 层”订阅者可以获得哪些内容:

```
user3.premium_content()
```

![](img/ef9aeb3b4dc4e91cd94696f52e0d10ef.png)

我们还可以定义“2 级”订户:

```
user4 = Premium_User('Tier 2', 'Bill Rogers', '[brogers@gmail.com](mailto:mharris@gmail.com)', True)
```

并检查可用的内容:

```
user4.premium_content()
```

![](img/4aa9f1852bcee5b0ec37dc9e1aabf1c0.png)

我将在这里停下来，但是您可以随意摆弄代码。

# 结论

总之，在这篇文章中，我们演示了 python 中父类和子类继承的用法。在我们的例子中，我们能够看到，通过继承，一个子类如何可以获得父类的所有数据和方法，同时向子类添加额外的数据和方法。我希望你觉得这篇文章有用/有趣。这篇文章的代码可以在 [GitHub](https://github.com/spierre91/medium_code/blob/master/classes_in_python/class_inheritance_python.py) 上找到。感谢您的阅读！