# @ Python 中的 Decorators🍃(高级)

> 原文：<https://towardsdatascience.com/decorators-in-python-advanced-8e6d3e509ffe?source=collection_archive---------15----------------------->

提升你的 Python 编程水平(**奖励** : `*args` `**kwargs` )…..🔥

![](img/e3dba94e35dbe57d5e0e6dd9737ac485.png)

由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

> B 自己比赛吧，它确实包含了很多代码。✌

这些为什么难以理解，不用我解释，就是你在这里的原因**(不要指望理论只有代码)**。但是你真的需要写装饰者吗？

> 编写 Python 代码的乐趣应该在于看到短小、简洁、易读的类，这些类用少量清晰的代码表达了大量的动作——而不是让读者厌烦得要死的大量琐碎代码。古迪奥·范·罗森。(python 的创造者)

目录:(可点击)

1.  [什么是装修工？](#9a40)
2.  操纵 Python 函数。
    **1** 。[给多个变量分配一个函数。](#d84a)
    2。[在函数](#75aa)中创建函数。
    **3** 。[将一个函数作为参数传递给其他函数。](#67be)
    4。[函数可以返回其他函数。](#73dd)
3.  如何表示和使用装饰者。
4.  装饰者的真实例子。
    1。[](http://6f42)**=>*获取某个功能的执行时间。*2
    。[**Deco**](#643e)**=>*并行运行 python 函数。*
    3。[**ShowMe**](#c584)=>*获取 CPU 时间文件等一个 python 函数。*****
5.  ****Python 内置的类装饰器。
    1。[**@ class method**](#3539)
    2。 [@staticmethod](#c788)
    3。[**@属性**](#5fb0)****

******加成:**学习如何使用`*args` & `**kwargs`****

> ******decorator 只不过是在不改变现有方法的情况下向现有方法添加新功能的函数。******

****要理解 Decorators，你需要理解 Python 中的函数，函数是一级对象，也就是说，我们可以给同一个函数分配多个变量，或者我们甚至可以将它们作为参数发送给其他函数。****

*****让我们看看用 python 函数可以做的一些很酷的事情。*****

## ****1.将一个函数赋给多个变量。****

```
**>>> **def** func(x):
...     return x.upper()
>>> func("roar")
'ROAR'>>> new_func = func  ***# Assign func to a variable.***>>> new_func("meow")
'MEOW'>>> **del** func
# We can call the new_func even after deleting the func
>>> new_func("meow "*2)
'MEOW MEOW '**
```

## ****2.在函数中创建函数。(对`C/C++`程序员来说是新的。)****

```
****def** factorial(n):
    *""" 
    Calculates the factorial of n, 
    n => integer and n >= 0.
    """*
    **if** type(n) == int **and** n >= 0:
        **if** n == 0:
            **return** 1
        **else**:
            **return** n * factorial(n-1) ***#*** ***Recursive Call***
    **else**:
        **raise** **TypeError**("n should be an integer and n >= 0")**
```

*******上面的代码有什么缺陷？*******

****`factorial(10)`递归检查 10，9，8，7…的类型，这是不必要的。我们可以通过使用内部函数很好地解决这个问题。****

```
****def** factorial(n):
    *""" 
    Calculates the factorial of n, 
    n => integer and n >= 0.
    """*
    **def** inner_factorial(n):
        **if** n == 0:
            **return** 1
        **else**:
            **return** n * inner_factorial(n-1)
    **if** type(n) == int **and** n >=0:
        **return** inner_factorial(n)
    **else**:
        **raise** **TypeError**("n should be an integer and n >= 0")**
```

****嘿，你可以用一个室内设计师。好的，让我们等等。****

## ****3.将函数作为参数传递给其他函数。****

```
****import math
def** sin_cos(func, var):
    print("Call this" + **func.__name__** +"function")
    print(func(var))sin_cos(**math.sin**, 60) # -0.3048106211022167
sin_cos(**math.cos**, 45) # 0.5253219888177297**
```

## ****4.函数可以返回其他函数。****

```
****def** sound(range):    
    """ 
    Args: range (Type of sound). (<class 'str'>)
    Return: function object of the sound (<class 'function'>)
    """ 
    **def** loud(x):
        print(x.upper() + '🐯')
    **def** low(x):
        print(x.lower() + '🐱')
    **if** range == 'loud':
        **return** loud
    **else**:
        **return** lowtiger = sound("loud") ***# you can use this as a functions.***
tiger("roar..") **# ROAR..🐯**cat = sound("low")
cat("MEOW..") **# meow..🐱****
```

****当我第一次看到这个的时候，我也有点困惑。对于每个范围，`sound`返回其各自的函数。一个更有用的例子是创建一个 n 次多项式。****

```
***Ref:* [https://www.python-course.eu/python3_decorators.php#Functions-returning-Functions](https://www.python-course.eu/python3_decorators.php#Functions-returning-Functions)**def** polynomial_creator(a, b, c):
    """
    **Creates 2nd degree polynomial functions**
    """
    **def** polynomial(x):
        **return** a * x**2 + b * x + c
    **return** polynomial

p1 = polynomial_creator(2, 3, -1)
p2 = polynomial_creator(-1, 2, 1)x = -2
print(x, p1(x), p2(x)) # -2 1 -7**
```

*******简短练习:*** 试写一个以次数和参数为自变量，返回一个 n 次多项式的函数。****

****![](img/a9cd6cc50e21839d42e44cefa3d7f515.png)****

****查尔斯·德鲁维奥在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片****

## ****完成了基础工作，接下来就很棘手了。（👀)****

****让我们从一个简单的装饰器开始，它在函数运行之前打印出它的名字。****

****🦋用你自己的例子来试试吧🦋****

****🔥 **Pro 提示:**通过使用`*args`你可以给一个函数发送不同长度的变量。`*args`将其作为元组接受。[了解更多](https://realpython.com/python-kwargs-and-args/) `[*args](https://realpython.com/python-kwargs-and-args/)` [和](https://realpython.com/python-kwargs-and-args/) `[*kwargs](https://realpython.com/python-kwargs-and-args/)`****

****decorators 的语法与我们在上面的例子中表达的不同。通常用`**@**` ( *美化你的代码*)来表示。****

****🦋用你自己的例子来试试吧🦋****

****🔥 **ProTip:** 通过使用 [Code-Medium](https://github.com/Maluen/code-medium) 你可以直接从你的 Medium 文章中 ***创建*** & ***编辑*** GitHub Gists。(如上图)。****

> ****注意:使用 decorator 包装函数时，原始函数的属性，如 __doc__ (docstring)、__name__(函数名)、__module__(定义函数的模块)将会丢失。****

****尽管可以在 decorator 函数中覆盖它们，但 python 有一个内置的 decorator @ wraps 来做这件事。****

****如果你仔细看，我会打印每个函数的输出，如果你想返回一个输出的话。您需要在包装函数中添加一个额外的 return。****

****使用 decorators 时**返回**的用法。****

****![](img/99dfdbab57f034adf0ef554fd3442002.png)****

****[丹](https://unsplash.com/@danmakesgames?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照****

## ****为了更好地理解，让我们看一些装饰者的真实例子。****

1.  ******定时器:** *跟踪一个函数运行所用的时间。*****

****🏄‍♂️理解*args 和*kwargs 复制代码并通过传递不同的变量来运行它🏄‍♂️****

****🔥 **Pro 提示:**您可以通过使用`original_looper = looper.__wrapped__` ( *假设您已经使用了* `*@wraps*`)解包来访问原始函数****

****2. **Deco:** *一个自动并行化 python 程序的库*[***Link***](https://github.com/alex-sherman/deco)***。*******

****[Alex Sherman](https://github.com/alex-sherman) 创建了这个简单的库，名为 deco，它允许 python 函数并行运行，只需添加一个装饰器*很酷吧..！。*****

*****你要并行运行的函数用* @concurrent
*修饰，调用并行函数的函数用* @synchronized 修饰****

```
****@concurrent** # We add this for the concurrent function
**def** process_lat_lon(lat, lon, data):
  #Does some work which takes a while
  **return** result

# And we add this for the function which calls the concurrent function
**@synchronized
def** process_data_set(data):
  results = **defaultdict**(dict)
  **for** lat **in** range(...):
    **for** lon **in** range(...):
      results[lat][lon] = process_lat_lon(lat, lon, data)
  **return** results**
```

******亲提示🔥:**更快运行 python 代码的类似但先进的科学计算库是 [**numba**](https://numba.pydata.org/) **。******

****3. **Showme** : *ShowMe 是一组简单的非常有用的 Python 函数装饰器。它允许您查看跟踪信息、执行时间、cputime 和函数文档。* [***链接***](https://github.com/navdeep-G/showme) ***。*******

******打印传入的参数和函数调用。******

```
****ref:** [https://github.com/navdeep-G/showme](https://github.com/navdeep-G/showme)@showme.trace
def complex_function(a, b, c, **kwargs):
    ...

>>> complex_function('alpha', 'beta', False, debug=True)
calling haystack.submodule.complex_function with
   args: ({'a': 'alpha', 'b': 'beta', 'c': False},)
   kwargs: {'debug': True}**
```

******打印功能执行时间。******

```
**@showme.time
def some_function(a):
    ...

>>> some_function()
Execution speed of __main__.some_function:
0.000688076019287 seconds**
```

******打印功能 CPU-执行时间。******

```
**@showme.cputime
 def complex_function(a, b, c):
     ...

 >>> complex_function()
 CPU time for __main__.complex_function:
      3 function calls in 0.013 CPU seconds

ncalls  tottime  percall  cumtime  percall filename:lineno(function)
     1    0.013    0.013    0.013    0.013 test_time.py:6(test)
     1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
     1    0.000    0.000    0.000    0.000 {range}**
```

****参考资料部分提供了更多示例的链接。****

****到目前为止，您已经了解了装饰人员是如何工作的，并且创建了自己的装饰人员。让我们看看一些内置的 Python 装饰器。****

****![](img/124b3205be844285a2695ed8b6845309.png)****

****[岳翎](https://unsplash.com/@leen_and_pretty_thoughts?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片****

## ****流行的内置 Python 装饰器****

****`**@classmethod:**` *创建类的一个实例。(通* `*cls*` *非* `*self*` *)*****

*****示例:*假设你从不同的数据类型中获取数据，而不是创建多个类，我们可以使用@classmethod 来处理它。****

****Python 中@classmethod 的用法。****

****`**@staticmethod:**` *你可以在不创建类实例的情况下使用这个装饰器的方法。(无需通过* `*self*` *)*****

*******举例*** *:* 假设你想为一个包含一些逻辑但却是该类的 not 属性的类创建一些 helper 函数。****

****`**@Property**`:OOPs 中使用`get` & `set`命令的一种方式。****

******每次你调用`set`或`get`到盒子的`maxlimit`值，它会通过`@property`和`@maxlimit.setter.`******

*******参考文献:
-* [*高级 Python*](https://www.python-course.eu/advanced_python.php) *作者 Brend Klein。
-如果你喜欢 python 你会爱上* [*真正的 Python*](https://realpython.com/) *。
-详细介绍*[*@ property*](https://www.programiz.com/python-programming/property)*如何工作。
——牛逼巨蟒* [*装修工*](https://github.com/lord63/awesome-python-decorator) *。*******

> ******我知道这很长，但是如果你到了这里👏敬你。不断学习，不断成长。******

******在 [LinkedIn](https://www.linkedin.com/in/prudhvi-vajja-22079610b/) 或 [Github](https://github.com/PrudhviVajja) 上与我联系。******