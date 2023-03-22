# 已经会 Python 了怎么学 Julia

> 原文：<https://towardsdatascience.com/how-to-learn-julia-when-you-already-know-python-641ed02b3fa7?source=collection_archive---------7----------------------->

## 跳到好的方面

![](img/32c30b5894417e9cbd75c36132a1ff37.png)

鲍里斯·斯莫克罗维奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 朱莉娅是新来的吗？

Julia 是一种较新的获奖编程语言，学起来像 Python 一样简单，但执行起来像 c 一样快，不信？确实是真的。(点击[此处](https://github.com/astrojhgu/adaptrapezoid_benchmark)进行多语言速度对比。)

Julia 提供的不仅仅是语法和速度。为了解释他们开发这种语言的原因，朱莉娅的创造者说:

“我们想要 C 的速度和 Ruby 的活力。我们想要一种同形异义的语言，既有像 Lisp 那样的真正的宏，又有像 Matlab 那样明显、熟悉的数学符号。我们想要像 Python 一样可用于一般编程，像 R 一样易于统计，像 Perl 一样自然用于字符串处理，像 Matlab 一样强大用于线性代数，像 shell 一样善于将程序粘合在一起。这种东西学习起来非常简单，却能让最严肃的黑客感到高兴。[1]"

陪审团还没有决定，但感觉他们已经做出了决定。当 Julia 1.0 发布时，一种有潜力实现大部分(如果不是全部)目标的语言的框架就诞生了。

同时，Julia 要达到主流编程语言的成熟还有很长的路要走。Julia 的软件包需要改进，它的文档和学习资源也需要改进。幸运的是，一个活跃的(甚至是热心的)开发者社区正在解决这些问题。

尽管这种语言正在发展，但有很多理由学习 Julia ，尤其是如果你对机器学习、数据科学或科学计算感兴趣的话。

# 懂 Python 想学 Julia？

![](img/fe53c35125795afdf5c3e126388c82f4.png)

照片由 [Arif Riyanto](https://unsplash.com/@arifriyanto?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Python 用户通常能够很快学会 Julia 语法。语法类似于 Python，有许多 Python 用户熟悉的约定。

然而，用 Julia 编程与用 Python 编程有着根本的不同。很有可能，Python 用户编写的第一个 Julia 代码在外观和行为上都很像 Python。虽然这种方法没有什么大问题，但看起来像 Python 的 Julia 可能会效率低下，并且会错过该语言的重要方面。

Julia 在不同的范例下运行——通用函数、巧妙的调度和深思熟虑的类型(仅举几个例子),这些想法中有许多根本没有出现在 Python 中。因此，本文的目标是教授三个简单而重要的 Julia 概念:类型层次**、**多分派**和**用户定义类型**。选择这些概念是为了帮助加速 Pythonic Julia 程序，说明 Julia 与 Python 的不同之处，并向 Python 用户介绍新的编程思想。**

因此，因为我侧重于概念，所以我不会在这里介绍安装 Julia 和学习基本语法。对于安装和语法，我推荐以下资源:

*   Changhyun Kwon 的《Julia 运筹学编程》的第一章包含了一个优秀的安装和设置指南。
*   由 J. Fernandez-Villaverde 编写的安装和语法综合指南。
*   通过例子向 [Julia 学习语法。](https://juliabyexample.helpmanual.io/)
*   从德里克·巴纳斯的[视频](https://www.youtube.com/watch?v=sE67bP2PnOo)中学习语法。

# 三个重要的朱莉娅概念

## **1。类型层次**

Julia 的许多速度、多功能性和可组合性优势部分归功于打字系统。在 Python 中，类型可能是事后才想到的，所以通过类型思考可能会显得乏味。然而，Julia 保持简单，并以速度提升奖励仔细思考。

**具体/原始类型**

Julia 使用两种不同的类型:具体类型和抽象类型。每一个都有不同的用途。具体类型包括典型的`String`、`Bool`、`Int64`等。并用于标准计算。类型`Float64`是一个具体类型，意味着`Float64`可以被实例化并用于计算。

**抽象类型**

类型`Union{}`、`AbstractFloat`、`Real`和`Any`都是抽象类型。抽象类型不能实例化。相反，抽象类型是将相似类型的数据组合在一起的容器。它们通常用于向编译器表明可以在抽象类型的任何子类型上调用函数。

```
""" This function accepts Float16, Float32, Float64 
    because they are all subtypes of AbstractFloat
"""
function g(a::AbstractFloat)
    return floor(Int, a) 
end
```

类型`Any`和`Union{}`是特殊的。`Union{}`被预定义为所有类型的子类型。它是类型层次结构的底部。类似地，每个类型都是`Any`的子类型，使其成为类型层次结构的顶层。

**为什么使用抽象类型？**

抽象类型很有用，因为定义为作用于抽象类型的函数能够作用于抽象类型的所有子类型。

举个例子，假设一个开发人员需要一个类似数组的数据结构。在 Julia 中，他们可以定义自己的应用程序特定结构，并确保它满足`AbstractArray`类型的要求。然后，Julia 生态系统中定义为对`AbstractArray`数据进行操作的所有函数将在开发人员的类似数组的数据结构上工作。由于这个特性，Julia 的许多包可以顺利地一起工作，即使它们不是一起设计的。

与 Python 包形成对比。几乎每一个使用数组的包都是为使用`numpy`数组而设计的。这就造成了对`numpy`的巨大依赖。如果一个程序员想创建自己的数组并在上面调用`numpy`函数，这可能会引发错误。很少有 Python 库能处理自定义对象。相比之下，Julia 中的抽象类型给了开发人员更多的灵活性，并有助于使包更具可组合性。

**操作员**

二元运算符`::`用于断言变量是某种类型。更具体地说，运算符可以将变量初始化为特定类型，表明函数参数必须是特定类型，或者断言预定义变量是特定类型。下面演示了其中的每一种用法。

```
# Initialize a Float64
x::Float64 = 100# Argument z must be an Int64
**function** f(z::Int64)
   **return** z^2
**end**# Assert x is a Float64 
x::Float64 # (Does nothing)# Assert that x is an Int64 
x::Int64 # (Raises error)
```

值得一提的是，我们可以在没有类型断言的情况下声明变量或定义函数，例如`x = 100`。(在这种情况下，变量`x`将是一个`Int64`。)

子类型运算符`<:`确定一个类型是否是另一个类型的子类型。如果我们想比较两个变量`x`和`y`的类型，如果变量`x`的类型是变量`y’s`类型的子类型，那么计算表达式`typeof(x) <: typeof(y)`将返回`true`。作为另一个例子，考虑以下表达式:

```
Union{} <: Float64 <: AbstractFloat <: Real <: Any
```

这评估为`true`,表示我们已经在类型层次结构中对类型进行了正确排序。(`<:`操作符可以比较这些对象，因为它们是类型而不是变量。)

**更多关于类型的阅读:**

*   关于类型的文档
*   一个很棒的关于类型的[教程](https://scls.gitbooks.io/ljthw/content/_chapters/06-ex3.html)来自《艰难地学习茱莉亚》
*   Julia 的创造者之一，(Stephan Karpinski) [讲解栈溢出上的类型系统](https://stackoverflow.com/questions/28078089/is-julia-dynamically-typed)。

## 2.多重调度

这个概念可能是了解朱莉娅最重要的概念。从开发的角度来看，Julia 提供的许多优势都源于多重调度。

多重分派是指函数根据其参数的类型表现不同。它类似于函数重载，但[不完全相同](https://discourse.julialang.org/t/is-multiple-dispatch-the-same-as-function-overloading/4145/8)。

当程序员将类型注释添加到函数定义中时，会发生多重分派。考虑下面的例子:

我们需要一个函数`f`来平方它的输入，然后计算它的值 mod 4。在 Julia 中，有 3 种等价的方式来定义`f`:

```
# Verbose definition
**function** f(x)
    **return** x^2 % 4
**end**# Mathematical notation
f(x) = x^2 % 4 # Like a Python lambda 
f = x -> x^2 % 4
```

假设我们总是需要`f`输出一个整数，但是它的输入`x`可以是一个`String`、`Float64`或者`Int64`，直到运行时我们才会知道`x`的类型。在 Python 中，这是通过以下方式解决的:

```
**def** f_py(x):
    **if** type(x) == string:
        x = float(x)
    **if** type(x) == float:
        x = ceil(x)
    **return** x**2 % 4
```

在 Julia 中，我们*可以*编写一个类似上面 Python 函数的函数:

```
**function** f_py(x)
    **if** isa(x, String)
        x = parse(Float64, x)
    **end**
    **if** isa(x, Float64)
        x = ceil(Int64,x)
    **end**
    x^2 % 4
**end**
```

然而，我们最好这样写:

```
f(x::Int64) = x^2 % 4
f(x::Float64) = f(ceil(Int64, x))
f(x::String) = f(parse(Float64, x))
```

这个定义集合与 Python 函数`f_py`做同样的事情。但是`f`对`x`的作用取决于`x`的类型。三个定义中的每一个都指定了`f`将对特定类型做什么。

1.  如果`f`被传递了一个`Int64`，它将把它的平方和 mod 减四。
2.  如果向`f`传递了一个`Float64`，它将计算高于 float 的整数上限，并对该整数调用`f`。这将调用 1 中描述的整数版本的`f`。
3.  如果`f`被传递了一个`String`，它将把它转换成一个`Float64`，然后在 float 上调用`f`，这将调用`f`的 float 版本，如 2 中所述。正如我们已经看到的，`Float64`版本转换为`Int64`，并调用`f`的`Int64`版本。

当这些函数在一个有 300 万个混合类型元素的数组上广播时，被调度的函数在 0.039 秒内完成。Python 版的`f_py`比`f`慢 [50 倍](https://github.com/djpasseyjr/SideProjects/tree/master/Medium%20Articles/LearnJuliaWhenYouKnowPython/DispatchSpeed)。此外，调度函数`f`的速度是 pythonic Julia 的两倍。

一方面，Julia 基本上比 Python 快，但是我们也看到多分派比 pythonic Julia 快。这是因为在 Julia 中，`f`的正确版本是在运行时通过查找表确定的，这避免了多次`if`语句求值。

正如您所看到的，多重分派速度很快，可以有效地解决各种编程挑战，使其成为 Julia 语言最有用的工具之一。

**更上多分派**

*   [Julia 如何使用多重调度击败 Python](https://medium.com/swlh/how-julia-uses-multiple-dispatch-to-beat-python-8fab888bb4d8) 。来自 DJ Passey 的多个调度的更多例子。
*   [多次派遣的不合理效力](https://www.youtube.com/watch?v=kc9HwsxE1OY)。斯蒂芬·卡尔平斯基的演讲。
*   关于 Erik Schnetter 的博客文章[泛型编程](https://eschnett.github.io/julia/2016/06/23/some-thoughts-on-generic-programming-in-julia)的一些想法。

## 3.复合类型(结构)

这可能令人震惊，但事实证明，Julia 不是面向对象的。没有类，也没有具有成员函数的对象。然而，通过利用多重分派和类型系统，Julia 获得了面向对象编程语言的优点和额外的灵活性。

Julia 不使用对象，而是使用用户定义的复合类型结构。结构没有内部函数。它们只是命名类型的集合。在下面的例子中，我们定义了一个 NBA 球员结构:

```
**struct** NBAPlayer
    name::String
    height::Int    
    points::Float64
    rebounds::Float64
    assists::Float64
**end**
```

类型`NBAPlayer`有一个默认的构造函数:

```
doncic = NBAPlayer("Luka Doncic", 79, 24.4, 8.5, 7.1)
```

每个字段都可以用熟悉的点符号访问:`doncic.name`、`doncic.height`、`doncic.points`、`doncic.rebounds`和`doncic.assists`。

您可以定义额外的构造函数，只要它们接受不同于默认的类型组合。这是多重调度在起作用:

```
# Constructor with no arguments**function** NBAPlayer()
    # Make an empty player
    **return** NBAPlayer("", 0.0, 0.0, 0.0)
**end**
```

有了定义的结构，我们可以给 Julia 库中的函数新的定义:

```
**function** Base.show(io::IO, player::NBAPlayer)
    print(io, player.name)
    print(io, ": ")
    print(io, (player.points, player.rebounds, player.assists))
**end**
```

这定义了结构`NBAPlayer`在打印时的显示方式。这类似于在 Python 中为一个类定义一个`__repr__()`函数。然而，与我们在 Python 中定义内部函数不同，在 Julia 中，我们为外部函数如何作用于结构提供了新的定义。

Python 允许开发者用[魔法方法](https://rszalski.github.io/magicmethods/)来决定某些操作符应该如何作用于一个类。程序员可以为`+, -, +=`和其他人应该如何作用于一个类编写他们自己的定义。但是，这从根本上受限于具有魔法方法的操作符列表。在 Julia 中，任何函数都可以被赋予任意类型或结构组合的定义。

## **结论**

尽管 Julia 很容易上手，但掌握起来却很棘手。学习这些概念使开发人员走上了掌握 Julia 的道路。通过实践和尝试这些想法，您可以开发出编写高质量 Julia 程序所必需的技能。

## 参考

[1] J. Bezanson，S. Karpinski V. Shah，A. Edelman，[为什么我们创造了 Julia](https://julialang.org/blog/2012/02/why-we-created-julia/) (2012)，JuliaLang.org

[](https://medium.com/swlh/how-julia-uses-multiple-dispatch-to-beat-python-8fab888bb4d8) [## Julia 如何利用多重调度击败 Python

### 亲自看

medium.com](https://medium.com/swlh/how-julia-uses-multiple-dispatch-to-beat-python-8fab888bb4d8) [](/a-scenic-look-at-the-julia-language-e8ba53dea5bd) [## 朱莉娅语言的风景照

### 体验朱莉娅，而不必做任何困难的事情

towardsdatascience.com](/a-scenic-look-at-the-julia-language-e8ba53dea5bd) [](/deep-learning-side-by-side-julia-v-s-python-5ac0645587f6) [## 深度学习并驾齐驱:Julia v.s. Python

### 你能说出哪一种是未来的语言吗？

towardsdatascience.com](/deep-learning-side-by-side-julia-v-s-python-5ac0645587f6)