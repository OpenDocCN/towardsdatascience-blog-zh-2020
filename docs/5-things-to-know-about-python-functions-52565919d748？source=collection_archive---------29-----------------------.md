# 关于 Python 函数要知道的 5 件事

> 原文：<https://towardsdatascience.com/5-things-to-know-about-python-functions-52565919d748?source=collection_archive---------29----------------------->

## 是时候编写结构良好的程序了

![](img/108ef688563c341973eb2394796610b7.png)

由 [David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

函数是重用程序代码的有效方法。使用函数的好处是节省空间，我们选择的名字使我们的程序易于阅读。函数可以在没有输入的情况下定义，并且不一定需要产生输出。

python 中有许多内置函数[但是我们可以创建自己的函数。](https://docs.python.org/3/library/functions.html\)

在 python 中，函数是使用`def`关键字定义的。

```
>>> def myfunc():
      print("Python functions are easy to learn.")>>> myfunc() #calling function
'Python functions are easy to learn.'
```

函数主要帮助你构建你的程序。最好将长程序分解成函数，每个函数都有自己的用途，使其结构透明，如下所示:

```
data = extract_data(source)
results = analyze(data)
present(results)
```

恰当地使用函数使程序更具可读性和可维护性。此外，在不关心程序其余部分的情况下，对一个函数进行修改更容易。

在本文中，我们将讨论五个主题，我认为这些主题对于了解函数非常重要。这些主题将帮助您更好地理解函数。

# 1.变量作用域

函数定义为变量创建新的局部范围。当一个新变量在函数体内赋值时，它只在函数内部定义。变量在函数外部不可见。所以我们在定义变量的时候可以选择任何名字，而不用关心函数之外的变量。

例如

```
>>> x = 10
>>> def myfunc():
        x = 5
```

在上面的函数中，变量`x`在函数外赋给`10`，在函数内赋给`5`。现在，当我们在函数外打印 x 时:

```
>>> x
10
```

变量 x 在函数外不会改变，因为函数内定义的`x`的范围仅限于函数本身。

# 2.参数类型

自变量也称为参数，用于将信息传递给函数。有两种类型的论点:

*   ***关键字参数:*** 在函数调用中以标识符(`key1 = value1`)开头的参数，或者在以`**`开头的字典中作为值传递的参数。

```
ratio(numerator=10, denominator=4)
ratio(**{'numerator':10,'denominator':4})
```

*   ***位置论点:*** 不是关键字论点的论点。该值被直接传递到一个函数中，该函数的位置表示函数中的一个变量。它也可以作为前面带`*`的 iterable 的元素来传递。

```
ratio(10,4)
ratio(*(10,4))
```

# 3.作为参数的函数

有趣的是，我们也可以将函数作为参数传递。例如，我们可以将内置函数`len()`或用户定义的函数`vowel_present()`作为参数传递给另一个函数:

```
>>> sent = ['Python', 'functions', 'are', 'amazing']
>>> def execute_func(prop):
        return [prop(word) for word in sent]

>>> execute_func(len)
[6, 9, 3, 7]
>>> def vowel_present(word):
...     for x in word:
            if x in 'aeiou':
                return True
        return False>>> execute_func(vowel_present)
[True, True, True, True]
```

# 4.参数类型检查

Python 函数不要求我们指定输入参数的类型。所以在执行函数之前，有必要检查输入变量的类型。

```
def is_vowel(char): #Returns True if char is vowel, False otherwise.
    if char in 'aeiou':
        return True
    else:
        return False>>> is_vowel(5)
False
```

例如，在上面的函数中，没有指定`number`的数据类型，但是我们希望变量的类型为长度为 1 的`str`。然而，如果我们传递一个整数变量，函数就会执行并返回`False`。

为了避免执行并引发错误，我们可以像这样使用`assert`函数。

```
def is_vowel(char):
    assert(isinstance(char, str)) #raises error if not satisfied
    assert(len(char)==1) #raises error if string not of length 1
    if char in 'aeiou':
        return True
    else:
        return False
```

如果`assert`语句失败，它会产生一个错误，函数会暂停执行。

# 5.记录功能

很好的做法是记录描述其用途的函数，并在函数定义的顶部使用 docstring 提供它，如下所示:

```
def is_vowel(char):
    '''Takes a string input of length one and returns True if vowel, False otherwise''' assert(isinstance(char, str))
    assert(len(char)==1) 
    if char in 'aeiou':
        return True
    else:
        return False
```

运行`help`函数可以找到任何函数的 Docstrings。也可以使用属性`__doc__`来检索它。

```
>>> help(is_vowel)
Help on function is_vowel in module __main__:is_vowel(char)
    Takes a string input of length one and returns True if vowel, False otherwise>>> is_vowel.__doc__
'Takes a string input of length one and returns True if vowel, False otherwise'
```

尝试运行`help(sklearn)`来获取 sklearn 库的 docstring。

在本文中，我们主要讨论了函数的应用以及它们如何帮助构建结构良好的程序。在下一篇文章中，我们将讨论 python 模块以及它们如何帮助高效编程。