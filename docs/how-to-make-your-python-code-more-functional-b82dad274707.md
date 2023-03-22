# 如何让您的 Python 代码更具功能性

> 原文：<https://towardsdatascience.com/how-to-make-your-python-code-more-functional-b82dad274707?source=collection_archive---------13----------------------->

## 以及为什么这会使您的代码更加健壮、可测试和可维护

![](img/b5ea29eb957fa0db9548c33e2415f1b1.png)

Python 很棒。以下是让它变得更好的方法。[大卫·克洛德](https://unsplash.com/@davidclode?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

近年来，函数式编程越来越受欢迎。它不仅非常适合像数据分析和机器学习这样的任务。这也是一种使代码更容易测试和维护的强大方法。

趋势显而易见:尽管它们仍处于小众位置，但像 [Elm](https://elm-lang.org) 和 [Haskell](https://www.haskell.org) 这样的纯函数式语言正在获得关注。有些功能性的语言，如 [Scala](https://www.scala-lang.org) 和 [Rust](https://www.rust-lang.org) ，正在起飞。像 C++、Python 这样的流行语言也在它们的清单中加入越来越多的函数式编程。

如果你已经习惯了面向对象的代码，那么写函数式程序一开始看起来会很吓人。好消息是，您可以很好地混合使用函数式代码和面向对象代码。函数式编程的一些调整通常足以获得一些好处。所以让我们开始吧！

[](/why-developers-are-falling-in-love-with-functional-programming-13514df4048e) [## 为什么开发人员会爱上函数式编程

### 从 Python 到 Haskell，这种趋势不会很快消失

towardsdatascience.com](/why-developers-are-falling-in-love-with-functional-programming-13514df4048e) 

# 纯函数

非函数式编程的讨厌之处在于函数可能会有副作用。也就是说，它们利用了不一定出现在函数声明中的变量。

考虑这个简单的例子，我们将两个数字相加:

```
b = 3
def add_ab(a):
    return a + b
add_ab(5)
```

全局变量`b`没有出现在`add_ab`的声明中，所以如果你想调试它，你必须检查一下`b`是否被使用了。听起来很简单，但是对于更大的程序来说可能会很乏味。我们可以很容易地解决这个问题，只要诚实地说出我们在函数中放入了什么:

```
def add_ab_functional(a, b):
    return a + b
add_ab_functional(5, 3)
```

这只是一个愚蠢的小例子。但是对于更大的程序，你会注意到当你不需要担心副作用时，理解和调试代码会容易得多。

# 高阶函数

在函数式编程中，可以嵌套函数:要么设计一个以另一个函数作为参数的函数，要么编写一个返回另一个函数的函数。

作为一个采用另一个函数的函数的例子，假设您有一个数字数组，并且您想要计算该数组的正弦、余弦和指数。理论上，你可以这样写(`numpy`是一个数学的 Python 包):

```
import numpy as np*# make a list of numbers as input values for functions*
numbers_list = np.arange(0, 2*np.pi, np.pi/10).tolist()*# calculate sine* def do_sin(numbers):
    return np.sin(numbers)
sin_list = do_sin(numbers_list)*# calculate cosine* def do_cos(numbers):
    return np.cos(numbers)
cos_list = do_cos(numbers_list)*# calculate exponential* def do_exp(numbers):
    return np.exp(numbers)
exp_list = do_exp(numbers_list)
```

这很好也很简单，但是用完全相同的结构写三个不同的函数有点烦人。相反，我们可以编写一个函数，像这样使用其他函数:

```
import numpy as np*# make a list of numbers as input values for functions*
numbers_list = np.arange(0, 2*np.pi, np.pi/10).tolist()*# calculate with some function* def do_calc(numbers, function):
    return function(numbers)*# calculate sin, cos, and exp*
new_sin_list = do_calc(numbers_list, np.sin)
new_cos_list = do_calc(numbers_list, np.cos)
new_exp_list = do_calc(numbers_list, np.exp)
```

这不仅更加简洁易读。它也更容易扩展，因为您只需要为一个新的`function`添加一行，而不是上面例子中的三行。

![](img/1dfbf3f2ecd412ecb147975d84dd029f.png)

函数式编程的一个关键概念是将函数嵌套在一起。照片由[this engineering RAEng](https://unsplash.com/@thisisengineering?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/mathematics?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

还可以把函数中函数的概念倒过来:不仅可以让一个函数把另一个函数作为自变量；你也可以让它返回一个参数。

假设您有一个数字数组，您需要[将数组的每个元素](https://stackabuse.com/functional-programming-in-python/)递增 2:

```
def add2(numbers):     
    incremented_nums = []     
    for n in numbers:         
        incremented_nums.append(n + 2)     
        return incremented_numsprint(add2([23, 88])) *# returns [25, 90]*
```

如果你想增加一个数组的元素，当然，你可以复制粘贴这个函数并用这个增量替换`2`。但是有一个更优雅的解决方案:我们可以编写一个函数，它接受任何增量，并返回另一个函数来执行`add2`所做的事情，但是针对任何增量。

```
def add_num(increment):
    def add_inc(numbers):
        incremented_nums = []
        for n in numbers:
            incremented_nums.append(n + increment)
        return incremented_nums
    return add_incadd5_25 = add_num(5.25)
print(add5_25([23.37,88.93])) *# returns [28.62, 94.18]*
```

使用这个例程，每个新函数只需要一行来定义，而不是五行。像接受函数的函数一样，返回函数的函数更容易调试:如果你在`add_num`中有一个错误，你只需要修复它。你不需要回去修改`add2`和其他任何你用同样方式定义的函数。项目越大，回报就越多。

注意，尽管`add_num`是以函数式风格编写的，但它并不纯粹是函数式的。它有一个副作用，`numbers`，这使它成为一个不纯的函数。但是没关系:你不需要成为一种编程范式的奴隶；相反，你可以充分利用这两者来最大化你的生产力。

![](img/aa2bbd39591682c33f367b393361e6a8.png)

装饰者可以让你的代码更加优雅。费尔南多·埃尔南德斯在 [Unsplash](https://unsplash.com/s/photos/developer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 装修工

当然，你可以将上面的两种方法结合起来，编写一个函数，它不仅接受一个函数作为参数，还返回一个函数。考虑这段代码，我们从上面扩展了`add_num`函数:

```
def add_num(message):
    def add_inc(increment, numbers):
        message()
        incremented_nums = []
        for n in numbers:
            incremented_nums.append(n + increment)
        return incremented_nums
    return add_incdef message1():
    print("Doing something...")**message1 = add_num(message1)** print(message1(5, [28,93]))*# Doing something...
# [33, 98]*
```

与上面示例的一个不同之处在于，您可以定制屏幕上输出的消息。例如，在一个更大的程序中，你可以扩展它来考虑不同的错误信息。

线条`message1 = add_num(message1)`是魔法发生的地方:名字`message1`现在指向`add_num`的内层，即`add_inc`。这叫装饰。

另一个区别是参数`increment`被向下推了；这只是让下一步更容易处理。

我们可以用`@`语法让装饰更加漂亮(其中的`def add_num`部分保持不变):

```
**@add_num** def message1():
    print("Doing something...")print(message1(5, [28,93]))
```

实际上，这只是编写装饰的一种更加简洁的方式。注意，使用 decorator[并不意味着](https://realpython.com/primer-on-python-decorators/)你的代码是有效的。更确切地说，装饰者受到函数式编程的启发，就像嵌套函数一样。上面的例子并不纯粹是函数式的，因为它包含了两个副作用，但是它仍然受到了函数式编程的启发。

[](/elements-of-functional-programming-in-python-1b295ea5bbe0) [## Python 中函数式编程的要素

### 了解如何使用 Python 中的 lambda、map、filter 和 reduce 函数来转换数据结构。

towardsdatascience.com](/elements-of-functional-programming-in-python-1b295ea5bbe0) 

# 生成器表达式和列表理解

列表理解和生成器表达式是 Python 从纯函数式编程语言 Haskell 复制的概念。考虑下面的例子，我们试图计算几个平方数:

```
numbers = [0, 1, 2, 3, 4]
square_numbers = []for x in range(5):
    square_numbers.append(x**2)square_numbers *# [0, 1, 4, 9, 16]*
```

这很笨拙，因为我们需要定义两个数组并编写一个`for`循环。一个更简洁优雅的方法是用列表理解来做这件事:

```
square_numbers = [x**2 for x in range(5)]
square_numbers *# [0, 1, 4, 9, 16]*
```

通过添加一个`if`条件，您可以只选择特定的元素。例如，假设我们只想要偶数的平方数:

```
even_square_numbers = [x**2 for x in range(5)
    if x%2 == 0]
even_square_numbers *# [0, 4, 16]*
```

列表理解将列表的所有值存储在内存中。这对小对象来说很好，但是如果你处理大的列表，它们会让你的程序变得很慢。这就是生成器表达式发挥作用的地方:

```
lots_of_square_numbers = (x**2 for x in range(10000))
lots_of_square_numbers *# <generator object <genexpr> at 0x1027c5c50>*
```

生成器表达式不会立即计算对象。这就是为什么如果你试图调用它们，你只会看到一个有趣的表达式(输出的确切形式取决于你的操作系统)。但是，它们使它们以后可以访问。您可以像这样调用生成器表达式的元素:

```
next(lots_of_square_numbers) # 0
next(lots_of_square_numbers) # 1
next(lots_of_square_numbers) # 4
...
```

或者，您可以创建生成器表达式中前几个元素的列表，如下所示:

```
[next(lots_of_square_numbers) for x in range(10)]
*# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]*
```

与其他技巧一样，这不会自动使您的代码变得纯粹实用。这只是从函数式编程中借用的一个概念，在许多情况下都很有用。

![](img/6614afe1902a95c3ccde0bbf650b1a8d.png)

Lambda 表达式可能是常规函数定义的一个很好的替代品。图片由 [NESA 制作](https://unsplash.com/@nesabymakers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 小函数和 lambda 表达式

如果想写一个[小函数](https://docs.python.org/3/howto/functional.html#small-functions-and-the-lambda-expression)，这样写也没什么不好:

```
def add_ab(a, b):
    return a + b
```

然而，你也可以用一个`lambda`式的表达:

```
add_ab2 = lambda a, b: a + b
```

它的长度几乎是一样的，一旦你习惯了它的语法，就很容易读懂。用不用真的看个人口味了。但是正如我们将在下面看到的，它在某些情况下非常方便。

如上所述，使用`lambda`-表达式不一定会使你的代码功能化，即使它们是函数式编程的一个关键思想。

# 内置 Python 函数

## 地图()

函数`map()`主要返回生成器表达式。这是一个简单的例子:

```
numbers = [0, 1, 2, 3, 4]
squared_numbers_map = list(map(lambda x: x**2, numbers))print(squared_numbers_map)
*# [0, 1, 4, 9, 16]*
```

正如你前面看到的，你可以用列表理解做同样的事情。然而，当您使用`map()`函数时，有时您的代码会更具可读性。

## 过滤器()

这类似于使用`if`子句的列表理解，例如:

```
squared_numbers_map_even = list(filter(lambda x: x%2 == 0, squared_numbers_map))print(squared_numbers_map_even)
*# [0, 4, 16]*
```

你也可以像这样嵌套`map()`和`filter()`:

```
squared_numbers_map_even_new = list(filter(lambda x: x%2 == 0, list(map(lambda x: x**2, numbers))))print(squared_numbers_map_even_new)
*# [0, 4, 16]*
```

## 枚举()

如果你在遍历一个列表，并且需要跟踪索引，`enumerate()`是一个不错的选择:

```
for num in enumerate(squared_numbers_map_even):
    print(num)
*# (0, 0)
# (1, 4)
# (2, 16)*
```

## zip()

如果需要从两个列表中创建元组，可以使用`zip()`:

```
list(zip(['a', 'b', 'c'], (1, 2, 3)))
*# [('a', 1), ('b', 2), ('c', 3)]*
```

因为`zip()`像生成器表达式一样只返回可迭代的对象，所以`list()`包含在这个表达式中。

# functools 模块

有时候你会有一个函数，它有几个参数，但是你需要修改几个参数。考虑这个简单的例子:

```
import functoolsdef add_lots_of_numbers(a, b, c, d):
    return a + b + c + dadd_a_and_b_27 = functools.partial(add_lots_of_numbers, c=18, d=9)
add_a_and_b_27(1,2) *# 30*
```

在这个模块中，除了`functools.partial()`之外，还有[几个](https://docs.python.org/3/howto/functional.html#the-functools-module)功能，但这是迄今为止最重要的一个。和以前一样，`partial()`并不总是导致函数式代码，但是它是从函数式编程中借用来的一个简洁的概念。

[](/the-ultimate-guide-to-writing-better-python-code-1362a1209e5a) [## 编写更好的 Python 代码的终极指南

### 让你的软件更快、更易读和更易维护并不需要这么难

towardsdatascience.com](/the-ultimate-guide-to-writing-better-python-code-1362a1209e5a) 

# 几个简单的小技巧就能帮上大忙

当你开始编码的时候，你可能听说过很多关于面向对象编程的东西，而不是关于函数式编程的。这确实有道理，因为面向对象编程非常有用。

但是最近几年，我们遇到了越来越多的问题，当你掌握一些函数式编程的技巧时，这些问题就更容易解决了。

你不需要马上学习像 Elm 或者 Haskell 这样的函数式编程语言。相反，您可以提取它们最有用的方面，并直接在 Python 代码中使用它们。

一旦你知道了窍门，你会发现到处都有机会使用它们。编码快乐！