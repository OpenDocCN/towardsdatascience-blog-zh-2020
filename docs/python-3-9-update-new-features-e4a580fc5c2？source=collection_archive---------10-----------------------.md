# Python 3.9 更新新功能

> 原文：<https://towardsdatascience.com/python-3-9-update-new-features-e4a580fc5c2?source=collection_archive---------10----------------------->

## Python 备忘单

## 少写代码，多成就！

当你阅读这篇文章时，你可能正在使用 Python 3.8。准备好！几个月后，我们将看到 Python 的新版本(计划在 2020 年 10 月)。由于我们目前是测试版，让我们看看这个版本有什么新的。这次更新有 4 个主要的新特性，它们是字典联合操作符、字符串方法、类型提示和 Python 解析器。但是，本文将只讨论前三个更新，因为它们是您最有可能遇到/使用的。

![](img/b55a0160fe8865158a51ee92358169cc.png)

图片由[法比奥](https://unsplash.com/@fabioha)在 [Unsplash](https://unsplash.com/) 上拍摄

# Python 字典联合方法

1.  更新方法

*   老办法

直到 Python 3.8，人们仍然普遍使用`update()`方法更新字典。它看起来是这样的:

```
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
dict1.update(dict2)print(dict1) #return {"a": 1, "b": 2, "c": 3, "d": 4}
```

真的能变好吗？是的，它可以！3.9 的更新绝对符合 Python 的精神，编写更少的代码！

*   新的方式

要在 Python 3.9 中更新字典，可以使用这个新的更新操作符`|=`。与之前的`update()`方法相比，这将使你的代码看起来更加光滑。

```
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
**dict1 |= dict2**print(dict1) #return {"a": 1, "b": 2, "c": 3, "d": 4}
```

2.合并方法

*   老办法

到目前为止，合并 2 个字典的最好方法是使用双星号操作符`**`。我们是这样做的:

```
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
dict3 = {**dict1, **dict2}print(dict3) #return {"a": 1, "b": 2, "c": 3, "d": 4}
```

上面的片段看起来不错。它短小精悍，更重要的是，做工作。然而，对包括我在内的大多数开发人员来说，它看起来有点丑，也没什么吸引力。终于！有一个更好的方法来解决这个问题。

*   新的方式

```
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
dict3 = **dict1 | dict2**print(dict3) #return {"a": 1, "b": 2, "c": 3, "d": 4}
```

你们应该记住的唯一一件事是当两个字典有相似的关键字时。合并后，公共键的值将从第二个字典中获取

```
dict1 = {"a": 1, "b": 2, **"same" : "old"**}
dict2 = {"c": 3, "d": 4, **"same": "new"**}
dict3 = **dict1 | dict2**print(dict3) #return {"a": 1, "b": 2, "c": 3, "d": 4, **"same": "new"**}
```

# Python 字符串方法

在这些新的更新中，字符串方法增加了两项内容。他们增加了`removeprefix()`和`removesuffix()`功能。这个特性并不能改变生活，但绝对值得一提。

*   removeprefix()

```
string = "New Python".**removeprefix("Ne")**print(string) #return ***"w Python"***
```

*   removesuffix()

```
string = "New Python".**removesuffix("on")**print(string) #return **"New Pyth"**
```

# Python 类型提示

Python 在最好的时候不需要类型声明。然而，在某些情况下，它可能会导致混乱。下面是事情可能变得混乱的例子:

假设您想通过使用自己创建的奖金函数来确定办公室员工的年度奖金。

*   没有类型提示:

```
def Bonus(base):
    print("Your Bonus is ${}".format(base*3))
Bonus(4000) #print "Your Bonus is **$12000**"
```

在上面的代码片段中看起来很好，但是如果您要求用户输入呢？

```
base = input("Enter the employee's base salary")
#for example you typed 4000Bonus(base) #print "Your Bonus is **$400040004000"**
```

和你想象的不太一样，对吗？这是因为 Python 输入函数将返回字符串类型。因为`*`操作符也可用于字符串，所以不会发生错误。`*`操作符，当应用于一个字符串时，将在指定的次数内连接该字符串。

```
string = "Hello"
print(**string*2**) #print "HelloHello"
```

在`*`操作符之后，我们输入值 2。解释器将“Hello”字符串连接了两次，并输出“HelloHello”作为结果。

别担心，您可以通过使用新的 Python 类型提示特性来解决这个问题。

*   使用类型提示

```
def Bonus(base: int):
    print("Your Bonus is ${}".format(base*3))base = input("Enter the employee's base salary")
#for example you typed 4000Bonus(base) #return "Expected type 'int', got 'str' instead"
```

类型提示实际上是从 Python 3.5 开始引入的。由于 Python 3.9 中的新更新，现在看起来干净多了！

# 最后的想法

Python 3.9 的新更新并没有那么重要。然而，总的来说，可以肯定地说，这将是一个不错的更新。Python 总是做对的一件事是为用户提供良好的可读性。

记住伙计们！将于 10 月上市。希望本文能帮助您过渡到新的 Python 3.9。

问候，

**弧度克里斯诺**