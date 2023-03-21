# Python Walrus 运算符

> 原文：<https://towardsdatascience.com/the-walrus-operator-7971cd339d7d?source=collection_archive---------12----------------------->

## Python 3.8 特性

## 你现在肯定用过了！

Walrus operator 是 Python 3.8 更新中添加的最酷的特性，这是你们大多数人现在可能正在使用的 Python 版本。操作符`:=`被称为海象操作符，因为它描述了海象的样子。把冒号看作眼睛，等号看作长牙。

![](img/996b70fad6528e06507cc8e31b52af0b.png)

图片由[杰伊·鲁泽斯基](https://unsplash.com/@wolsenburg)在 [Unsplash](https://unsplash.com/) 上拍摄

walrus 运算符用作赋值表达式。它允许你给一个变量赋值，同时也返回值。

# 那么我们如何利用这一点呢？

如果你想知道这个操作符是否有用，你应该看看这个！

假设您有一个程序提示用户说出他们最喜欢的颜色，并且您希望该程序向用户发回一条包含答案的消息。通常，您会这样做:

```
start = input("Do you want to start(y/n)?")
print(start == "y")
```

但是有了 walrus operator，我们可以让它更紧凑。

```
print((start := input("Do you want to start(y/n)?")) == "y")
```

这两个代码片段将打印相同的结果，如果输入是 y，则打印 True，否则打印 False。你能认出我在第二段代码中使用了双括号吗？在这个代码片段中，即使没有双括号，仍然可以获得理想的结果 True/False。但是，应该注意的是，鼓励在 walrus 操作符中使用括号。它可以节省你很多时间。

# 为什么重要？

比较这两个片段:

1.  带括号

```
if (sum := 10 + 5) > 10:
    print(sum) #return 15 
```

2.不带括号

```
if sum := 10 + 5 > 10:
    print(sum) #return True
```

注意，在第二个示例中，值 True 被赋给了 sum 变量。这是因为零件`> 10`包含在 sum 变量的评估中。

# 最后的想法

Python 3.8 中的 Walrus 操作符是您现在应该已经在使用的一个特性。该操作符允许的赋值表达式的可能性将是对 Python 特性的一个很好的补充。我担心不是每个人都会同意，因为对一些人来说，这会牺牲可读性。尽管如此，我个人认为，通过正确的实现，walrus 操作符将是对您的代码的一个很好的补充。

使用 walrus 操作符时，只需记住使用括号以避免歧义。

问候，

**弧度克里斯诺**