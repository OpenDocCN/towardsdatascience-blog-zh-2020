# 查阅字典的正确方法

> 原文：<https://towardsdatascience.com/the-right-way-to-access-a-dictionary-2270e936443e?source=collection_archive---------22----------------------->

## PYTHON 词典指南

## 小心点！你可能做错了

![](img/0ce2b9c9683f1eef7327d505dc82d47c.png)

图片由 [Syd Wachs](https://unsplash.com/@videmusart) 在 [Unsplash](https://unsplash.com/) 上拍摄

在用 Python 编程时，字典是现成可用的数据结构之一。

# 在我们开始之前，什么是字典？

Dictionary 是一个无序和无序的 Python 集合，它将惟一的键映射到一些值。在 Python 中，字典是用花括号`{}`写的。键与键之间用冒号`:`隔开，每个键-值对用逗号`,`隔开。下面是用 Python 声明字典的方法。

```
#A dictionary containing basketball players with their heights in m
playersHeight = {"Lebron James": 2.06,
                 "Kevin Durant": 2.08, 
                 "Luka Doncic": 2.01,
                 "James Harden": 1.96}
```

我们已经创建了字典，但是，如果我们不能再次检索数据，这对我们有什么好处呢？这是很多人做错的地方。我应该承认，不久前我也是其中之一。当我意识到优势后，我再也不会回头了。这就是为什么我有动力与你们分享它。

# 错误的方式

众所周知的，或者我应该说是传统的在字典中访问一个值的方法是通过引用它在方括号中的键名。

```
print(playersHeight["Lebron James"]) #print 2.06
print(playersHeight["Kevin Durant"]) #print 2.08
print(playersHeight["Luka Doncic"]) #print 2.01
print(playersHeight["James Harden"]) #print 1.96
```

一切都很好，对吗？没那么快！如果你输入一个字典里没有的篮球运动员的名字，你认为会发生什么？仔细看

```
playersHeight["Kyrie Irving"] #KeyError 'Kyrie Irving'
```

请注意，当您想要访问字典中不存在的键值时，将会导致 KeyError。这可能会很快升级为一个大问题，尤其是当你正在构建一个大项目的时候。不要烦恼！当然有一两种方法可以解决这个问题。

> *使用 If*

```
if "Kyrie Irving" is in playersHeight:
    print(playersHeight["Kyrie Irving"])
```

> *使用 Try-Except*

```
try:
    print("Kyrie Irving")
except KeyError as message:
    print(message) #'Kyrie Irving'
```

这两个代码片段运行起来都没有问题。现在，看起来还可以，我们可以容忍写更多的行来处理可能的 KeyError。然而，当你写的代码是错误的时候，它会变得很烦人。

幸运的是，有更好的方法来做到这一点。不是一个，而是两个更好的方法！系好安全带，准备好！

# 正确的方式

> *使用 get()方法*

使用 get 方法是处理字典时最好的选择之一。这个方法有两个参数，第一个是必需的，第二个是可选的。然而，为了发挥`get()`方法的全部潜力，我建议您填充这两个参数。

*   First:要检索其值的键的名称
*   第二:如果我们要搜索的键在

```
#A dictionary containing basketball players with their heights in m
playersHeight = {"Lebron James": 2.06,
                 "Kevin Durant": 2.08,
                 "Luka Doncic": 2.01,
                 "James Harden": 1.96}#If the key exists
print(playersHeight.get("Lebron James", 0)) #print 2.06
print(playersHeight.get("Kevin Durant", 0)) #print 2.08#If the key does not exist
print(playersHeight.get("Kyrie Irving", 0)) #print 0
print(playersHeight.get("Trae Young", 0)) #print 0
```

当键存在时，`get()`方法的工作方式与引用方括号中的键的名称完全相同。但是，当键不存在时，使用`get()`方法将打印我们输入的默认值作为第二个参数。

如果不指定第二个值，将返回一个`None`值。

您还应该注意，使用`get()`方法不会修改原始字典。我们将在本文后面进一步讨论它。

> *使用 setdefault()方法*

什么？还有别的办法吗？是的，当然！

当您不仅想跳过 try-except 步骤，还想覆盖原来的字典时，您可以使用`setdefault()`方法。

```
#A dictionary containing basketball players with their heights in m
playersHeight = {"Lebron James": 2.06,
                 "Kevin Durant": 2.08,
                 "Luka Doncic": 2.01,
                 "James Harden": 1.96}#If the key exists
print(playersHeight.setdefault("Lebron James", 0)) #print 2.06
print(playersHeight.setdefault("Kevin Durant", 0)) #print 2.08#If the key does not exist
print(playersHeight.setdefault("Kyrie Irving", 0)) #print 0
print(playersHeight.setdefault("Trae Young", 0)) #print 0
```

我说的改写是这个意思，当你再次看到原词典的时候，你会看到这个。

```
print(playersHeight)
"""
print
{"Lebron James": 2.06,
 "Kevin Durant": 2.08,
 "Luka Doncic": 2.01,
 "James Harden": 1.96,
 "Kyrie Irving": 0,
 "Trae Young": 0}
```

除此之外，`setdefault()`方法与`get()`方法完全相似。

# 最后的想法

`get()`和`setdefault()`都是你们都必须熟悉的高级技术。实现它并不困难和简单。你现在唯一的障碍是打破那些旧习惯。

但是，我相信随着你使用它，你会立即体验到不同之处。过一段时间，你就不再犹豫改变，开始用`get()`和`setdefault()`的方法。

记住，当你不想覆盖原来的字典时，用`get()`方法。

当你想对原来的字典进行修改时，`setdefault()`会是你更好的选择。

问候，

**弧度克里斯诺**