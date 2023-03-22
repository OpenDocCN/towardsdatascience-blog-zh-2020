# 每天 10 分钟学习 Python # 22

> 原文：<https://towardsdatascience.com/learning-python-10-minutes-a-day-22-a1bc96b529a1?source=collection_archive---------56----------------------->

![](img/861a215f083567059b0616591c415d2f.png)

[杰瑞米·拉帕克](https://unsplash.com/@jeremy_justin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的原始照片。

## [每天 10 分钟 Python 速成班](https://towardsdatascience.com/tagged/10minutespython)

## 在 Python 中，我们修饰函数以使它们更好

这是一个[系列](https://python-10-minutes-a-day.rocks)10 分钟的简短 Python 文章，帮助您提高 Python 知识。我试着每天发一篇文章(没有承诺)，从最基础的开始，到更复杂的习惯用法。如果您对 Python 的特定主题有任何问题或要求，请随时通过 LinkedIn 联系我。

装饰者是另一个令人困惑的话题，他们的使用肯定会让人感觉有些不可思议。虽然修饰函数这个术语非常恰当，但它也模糊了正在发生的事情，即:将一个函数包装在另一个函数中。首先，我们将创建一个示例函数:

![](img/ac9b699358b8a891e739d450d22e4653.png)

imgflp.com 上产生的模因

这个函数非常简单，我想每个人都很清楚。我们的工作包括编写许多函数，有时我们需要对函数计时，例如，如果我们想检查一个新的实现是否更快。一种方法是简单地使用时间函数:

希望明确这不是真的[干](/learning-python-10-minutes-a-day-9-60ecdf101cb5)。当然，我们可以使用 iPython 的神奇语句%timeit，但是现在让我们忽略这种可能性。当检查这些步骤时，我们看到我们在实际函数之前添加了几个步骤，在函数完成之后添加了几个步骤。解决这个问题的一个方法是将计时放在 square_value 函数中。但是如果你还想为其他功能计时呢？因为很大一部分代码是活的，所以我们可以很容易地将函数包装在函数周围。让我们进行第一次尝试:

这里我们创建了一个新函数，它将函数及其参数作为参数。还记得让[捕捉所有未命名参数](/learning-python-10-minutes-a-day-7-5390dd024178)的 **args* 方法吗？我们将这些传递给被调用的函数。与我们之前创建的非常湿的代码相比，这已经是一个很大的改进了。然而，为每个调用包装一个函数仍然令人感觉乏味。难道我们不能定义它让它一直存在吗？你可能猜到我们可以。我们利用了这样一个事实，即我们可以在任何地方定义一个函数，并返回新的定义。这听起来可能很奇怪，但在下一个例子中会很清楚:

timer 函数现在是一个包装函数，它围绕我们的 square_values 创建一个新函数，并返回包装后的函数。这种方法的最大优点是，我们现在可以通过简单地将任何函数封装在我们的 timer()中来为它计时。在这里，我们通过包装状态覆盖了对原始函数的引用。这一步称为修饰函数。因为这一步是非常常见的操作，Python 创建了一个特殊的语法来使用@符号修饰函数:

所以@符号只是语法，没有什么真正神奇的。这是一种用包装函数覆盖原始函数引用的简写符号。

对于初学者来说，装饰者的用例不是那么明显。在我使用 Python 的最初几年，我从未使用过它们。现在，我偶尔使用它们。除了计时功能，日志记录也是一个很好的用例。使用装饰器，您可以在函数级监控您的代码。

有相当多的软件包在它们的语法中使用了装饰器。例如，塞巴斯蒂安·拉米雷斯的 [FastApi](https://fastapi.tiangolo.com/) 使用 decorators 来创建与 RestFull api 端点的链接。Django 和 [Flask](https://palletsprojects.com/p/flask/) ，这两个伟大的 web 框架都使用装饰器来做各种事情。例如，确保某个功能仅在用户登录时使用。另一个近乎神奇的用法是与 Numba 一起使用。当用 Numba 装饰函数时，它使用实时(JIT)编译来加速函数，使它们在速度上与编译语言相当。还有很多其他用法，[这里的](https://github.com/lord63/awesome-python-decorator)是一个精选列表。

# 今天的练习:

今天我们将练习装饰者的另一个用例，即改变现有的功能。有时，您会遇到一个您导入的函数，它几乎可以完成您想要的功能。差不多了，但是需要稍微调整一下。为此，您可以编写另一个函数来完成它，或者您可以使用 decorators。如果您需要更改许多以相同格式返回值的函数，则特别方便。让我们看看今天作业的功能。

## 任务:

编写一个 decorator 函数并修饰 current_temperature()，以便它返回以摄氏度为单位的温度。

提示:

1.  你可以复制/粘贴我们之前的定时器装饰函数的结构。
2.  摄氏度=(华氏 32 度)* 5 / 9

我的 Github 上发布了一个解决方案。

如果您有任何问题，请随时通过 [LinkedIn](https://www.linkedin.com/in/dennisbakhuis/) 联系我。