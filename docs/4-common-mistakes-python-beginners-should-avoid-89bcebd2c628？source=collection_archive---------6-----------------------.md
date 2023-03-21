# Python 初学者应该避免的 4 个常见错误

> 原文：<https://towardsdatascience.com/4-common-mistakes-python-beginners-should-avoid-89bcebd2c628?source=collection_archive---------6----------------------->

## Python 初学者

## 我很艰难地学会了，但你不需要

![](img/4fb4317b389bd798668f2001dd8ce4e6.png)

由[拍摄的杰米街](https://unsplash.com/@jamie452?utm_source=medium&utm_medium=referral)上[的 Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> 让我们面对现实吧。学习编程很难。

许多人会同意，但有些人不同意。我不相信。

这是因为我总能在不同的编程语言中发现微妙的方法来做我想做的事情。我以为我已经**掌握了**它们。但我错了。你可以在你的代码中做任何事情，但是你不应该做任何你想做的事情。

我很快意识到我尝试的那些“微妙”的方法是不好的做法。但是一段有效的代码怎么会是坏的呢？我习惯于采用这些不好的(和微妙的)做法，它回来困扰着我。我是吃了苦头才知道的。

在分享每个 Python 新手都应该知道的 4 个常见错误之前，确保你熟悉下面这篇文章中的一些 Python 内置特性。

[](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4) [## 我希望我能早点知道的 5 个 Python 特性

### 超越 lambda、map 和 filter 的 Python 技巧

towardsdatascience.com](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4) 

# 1.不使用迭代器

每个 Python 新手都这么做，不管他们对其他编程语言的熟练程度如何。无处可逃。

给定一个列表`list_`，如何使用 for 循环逐个访问列表中的元素？我们知道 Python 中的列表是由**索引的**，因此我们可以通过`list_[i]`访问第 I 个元素。然后我们可以为 for 循环创建一个范围从`0`到`len(list_)`的整数迭代器，如下所示:

```
**for** i **in** **range**(**len**(list_)):
    foo(list_[i])
```

它工作了。代码没有问题。这也是在其他语言如 *C* 中构造 for 循环的标准方式。但是实际上我们可以用 Python 做得更好。

*如何？*

你知道 Python 中的列表是**可迭代的**吗？通过利用它的可迭代特性，我们可以生成可读性更好的代码，如下所示:

```
**for** element **in** list_:
    foo(element)
```

![](img/18f3160bfbdd11f7917022f5330eaba9.png)

由[在](https://unsplash.com/@creativeexchange?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上创意交流拍摄的照片

**在 for 循环中并行遍历多个列表**可以通过`zip`函数实现，而`enumerate`可以帮助你在迭代一个 iterable 对象时获取索引号(即计数器)。在 [5 Python 特性中介绍和解释了它们，我希望我能早点知道](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4)。

# 2.使用全局变量

**全局**变量是在主脚本中声明的具有全局作用域的变量，而**局部**变量是在具有局部作用域的函数中声明的变量。在 Python 中使用`global`关键字允许你在一个函数中局部访问和修改全局变量。这里有一个例子:

许多初学者喜欢它，因为使用`global`似乎可以避免传递函数所需的所有参数。*但这其实不是真的。它只是隐藏了动作。*

使用`global` s 也不利于**调试**目的。功能应被视为**块盒**，并且应该是**可重用的**。修改全局变量的函数可能会给主脚本带来**副作用**，这很难发现，而且很可能会导致复杂的代码，调试起来更加困难。

在局部函数中修改全局变量是一种糟糕的编程实践。您应该将变量作为参数传入，对其进行修改，并在函数结束时将其返回。

![](img/2bf8dc3ee1226e45939e4c968daef586.png)

照片由[弗拉季斯拉夫·克拉平](https://unsplash.com/@lemonvlad?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

*不要混淆全局变量和全局常量，因为在大多数情况下使用后者是完全可以的。

# 3.不理解可变对象

对于新的 Python 学习者来说，这可能是最常见的惊喜，因为这个特性在这种语言中是非常独特的。

Python 中有两种对象。可变对象可以在运行时改变它们的状态或内容**，而不可变对象则不能。很多内置对象类型是不可变的，包括 **int、float、string、bool、**和 **tuple** 。**

```
st **=** 'A string' 
st[0] **=** 'B' # You cannot do this in Python
```

另一方面，像 **list** 、 **set** 和 **dict** 这样的数据类型是可变的。因此，您可以更改列表中元素的内容，例如`list_[0] = 'new'`。

当函数中的**默认参数**可变时，会发生意想不到的事情。让我们以下面的函数为例，其中一个*可变* **空列表**是参数`list_`的默认值。

```
**def** foo(element, list_=[]):
    list_.**append**(element)
    **return** list_
```

让我们调用函数**两次**，而不为`list_`输入参数，这样它就取默认值。理想情况下，如果没有提供第二个参数，那么每次调用函数时都会创建一个新的空列表。

```
a = foo(1) # returns [1]
b = foo(2) # returns [1,2], not [2]! WHY?
```

*什么？*

原来 Python 中的默认参数是在定义函数时计算一次的**。这意味着调用函数不会刷新默认参数。**

![](img/e48b68c992f8c67e5374d083c7c26d31.png)

[Ravi Roshan](https://unsplash.com/@ravi_roshan_inc?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

因此，如果默认参数是可变的，并且每次调用函数时都会发生变化。变异后的默认参数将**用于所有未来的函数调用。“标准”**修复**是使用(不可变)`None`作为默认值，如下所示。**

```
**def** foo(element, list_=**None**):
    if list_ is **None**:
        list_ = []
    list_.**append**(element)
    **return** list_
```

# 4.不复制

对于学习者来说，复制的概念可能是外国的，甚至是违反直觉的 T42。假设你有一个列表`a = [[0,1],[2,3]]`，然后你在`b = a`前声明一个新列表。您现在有两个具有相同元素的列表。通过改变列表`b`中的一些元素，应该不会对列表`a`有什么(副作用)影响吧？

*错了。*

当您使用**赋值语句**即`b = a`来“复制”一个列表时，对一个列表的元素所做的任何修改在两个列表中都是可见的。赋值操作符只在目标和对象之间创建**绑定**，因此示例中的`a`和`b`列表共享同一个**引用**，即 Python 中的`id()`。

如何复制对象？

如果您想要“复制”对象，并且只修改新(或旧)对象中的值而不进行绑定，有两种方法可以创建副本:**浅层复制**和**深层复制**。两个对象将有不同的引用。

![](img/389020cd858a4ab4cfa82938df3020e9.png)

由[路易·汉瑟](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

使用我们之前的例子，您可以通过`b = copy.copy(a)`创建一个`a`的浅层副本。浅层拷贝创建了一个新对象，它存储了原始元素的**引用**。这听起来可能很复杂，但让我们看看下面的例子:

在创建了一个**嵌套列表** `a`的浅层副本之后，我们称之为`b`，两个列表有不同的引用`id(a) != id(b)`，用符号`!=`表示‘不相等’。然而，它们的元素具有相同的引用，因此`id(a[0]) == id(b[0])`。

这意味着改变`b`中的元素不会影响列表`a`，但是修改`b[1]`中的元素会影响`a[1]`，因此这个副本是浅层的。

简而言之，如果 `**b**` **是** `**a**`的浅层拷贝，那么**对** `**b**` **中嵌套对象内元素的任何更改都会出现在** `**a**` **中。**

如果你想复制一个嵌套的对象，而在它们的元素之间没有任何绑定，你需要一个由`b = copy.deepcopy(a)`生成的`a`的深层副本。深度复制创建一个新对象，而**递归地**在原始元素中创建嵌套对象的**副本。**

简而言之，**深度拷贝无任何绑定复制一切**。

![](img/65f0c99b3b4352a36afdd9676ed883de.png)

由[德鲁·科夫曼](https://unsplash.com/@drewcoffman?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 外卖

这就是 Python 初学者应该避免的 4 个常见错误。我很艰难地学会了，但你不需要。你可以[注册我的时事通讯](http://edenau.mailchimpsites.com/)来接收我的新文章的更新。如果您对 Python 感兴趣，您可能会发现以下文章很有用:

[](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4) [## 我希望我能早点知道的 5 个 Python 特性

### 超越 lambda、map 和 filter 的 Python 技巧

towardsdatascience.com](/5-python-features-i-wish-i-had-known-earlier-bc16e4a13bf4) [](/6-new-features-in-python-3-8-for-python-newbies-dc2e7b804acc) [## Python 3.8 中针对 Python 新手的 6 项新特性

### 请做好准备，因为 Python 2 不再受支持

towardsdatascience.com](/6-new-features-in-python-3-8-for-python-newbies-dc2e7b804acc) [](/4-numpy-tricks-every-python-beginner-should-learn-bdb41febc2f2) [## 每个 Python 初学者都应该学习的 4 个 NumPy 技巧

### 编写可读代码的技巧

towardsdatascience.com](/4-numpy-tricks-every-python-beginner-should-learn-bdb41febc2f2) 

*编码快乐！*