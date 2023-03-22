# Python 装饰器:从简单装饰器到嵌套多重

> 原文：<https://towardsdatascience.com/python-decorators-from-simple-decorators-to-nesting-multiple-33bbab8c5a45?source=collection_archive---------22----------------------->

![](img/518df37ec89d7a91abf6f59851f99011.png)

米尔蒂亚迪斯·弗拉基迪斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 如何用多个参数化的 decorators 修改一个函数？

装饰器对象是修改一个*装饰的*对象的功能的一种方式。用 python 实现装饰器有多种方法。在这篇文章中，我们将讨论一些以及如何将多个 decorators 链接在一起以真正增强一个对象的功能。在 python 中，函数/方法只是对象，所以在这篇文章中，我们将同时研究实现 decorators 的类和方法。

# 简单装饰

在我们开始嵌套之前，让我们看一个简单的装饰器。我将首先从一个简单的 *add* 方法开始。

上面的方法只是取了两个参数 *a* 和 *b* 。上述方法的功能是将这两个输入参数相加，并返回相加的结果。

现在我要装饰这个方法，这样方法 *add* 的结果就会乘以 2。

为了实现这一点，我创建了一个方法*乘 2。multiply_by_two* 采用另一种方法作为输入。它创建了另一个方法 *_multiply_by_two* ，该方法接受两个参数，这两个参数随后被传递给输入法，其结果乘以 2。*乘二*返回这个构造的方法*_ 乘二*。

我所做的是创建了一个装饰器 *multiply_by_two* ，它基本上是通过将输入法乘以 2 来装饰输入法的功能(顾名思义)。现在让我们来看看它的作用。

上面的代码，修饰了方法*加*乘*乘 2。*这样做的构造是将@符号与正在修饰的方法一起使用(*乘 2)*在正在修饰的方法之前(*加*)。

在上面的代码中，相同的 decorator 被应用于 subtract 方法。在这种情况下，输出是-8。所以很容易看出装修工是多才多艺的。它们可以相当普遍地应用。

# 参数化装饰器

在上面的例子中，我演示了乘 2，但是如果我们想给开发者一个选择乘任意数的机会呢？在这种情况下，我们希望将装饰器参数化。

这可能看起来让人不知所措，让我们来分解一下。方法*乘 _ 乘*取一个输入数。如果你仔细观察，方法 *_multiply* 与 *multiply_by_two* 非常相似。唯一的区别是现在 num 被用来乘，而不是硬编码 2。

在 decorator 中使用参数的方法是在使用@ symbol 和 decorator 方法的名称之后发送它们。在这种情况下，它是 *@multiply_by* ，我们传递 3。这种情况下的输出是预期的 18。

# 嵌套参数化装饰器

现在让我们来看看装饰器的嵌套，一个接一个的装饰器可以被应用到一个方法上。与算术主题保持一致，我现在将创建另一个方法 *divide_by* ，它与 *multiply_by* 相同，只是它使用输入 num 来划分修饰方法的结果。

要嵌套 decorators，需要在实际的被修饰方法之前使用与 symbol @之前使用的相同机制一次指定一个。

在上面的代码中，我装饰了方法 add by *multiply_by* ，然后 *divide_by* 。因为乘法和除法都发生在同一个数字 3 上，所以该方法的结果是 6。

我在这里所做的只是首先用参数 3 指定 decorator *divide_by* ，然后用相同的参数 3 指定 *multiply_by* 。这产生了嵌套，因为首先**乘以然后**再**除以装饰器被应用。应用程序的顺序与它们在代码中出现的顺序相反，注意这一点很重要。**

# 奖金

如上面的代码所示，装饰器也可以应用在方法内部。我会让你决定以上的结果。也可以放在评论/回复里。

# 结论

在这篇文章中，我解释了如何在 python 中使用 decorator，方法是引入一个简单的 decorator，然后将其参数化，最后将多个 decorator 嵌套在一起。我希望你喜欢它！

在 LinkedIn 上与我联系或在 Medium 上关注我。如果你喜欢这个故事，你可能会喜欢我关于 python decorators 的其他故事:

[](/python-decorators-with-data-science-random-sampling-177962cae80c) [## Python 装饰者与数据科学:随机抽样

### 使用 python decorator 进行随机采样

towardsdatascience.com](/python-decorators-with-data-science-random-sampling-177962cae80c) [](https://medium.com/better-programming/decorator-pattern-and-python-decorators-b0b573f4c1ce) [## 装饰模式和 Python 装饰器

### 为什么它们不一样？

medium.com](https://medium.com/better-programming/decorator-pattern-and-python-decorators-b0b573f4c1ce)