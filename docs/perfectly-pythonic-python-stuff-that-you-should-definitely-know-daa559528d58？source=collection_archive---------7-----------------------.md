# 完美的 Pythonic Python 你绝对应该知道的东西

> 原文：<https://towardsdatascience.com/perfectly-pythonic-python-stuff-that-you-should-definitely-know-daa559528d58?source=collection_archive---------7----------------------->

## 像 Python 禅师一样编写简单而优雅的代码

![](img/9f4c914b9d6606885389057754cf1777.png)

图片来自[壁纸 flare](https://www.wallpaperflare.com/closeup-photo-of-eyeglasses-screen-computer-electronics-monitor-wallpaper-ezhbo/download/1920x1080)

M 我们大多数人在编码时都有一些定位功能。一旦我们习惯了以某种方式编码，我们倾向于一遍又一遍地使用相同的函数，即使可能有更好的方式。而且，按照 Python 的禅理，*应该只有一种——最好只有一种——显而易见的方法来做这件事！*

比我愿意承认的更多的时候，我也掉进了这个陷阱。例如，太习惯于循环，太频繁地使用打印而不是日志，等等。这绝对不是蟒蛇的方式。您可以随时更新关于 Pythonic 的知识，即编写清晰、简洁且易于维护的代码，更多内容请点击此处:

[](https://docs.python-guide.org/writing/style/) [## 代码风格 Python 的搭便车指南

### 如果你问 Python 程序员他们最喜欢 Python 的什么，他们往往会引用它的高可读性。的确，一个…

docs.python-guide.org](https://docs.python-guide.org/writing/style/) 

经过一段时间，以下 7 个功能成为我最喜欢使用的功能，因为它们的简单和内在的优雅。如果你到现在还没有，你应该尽可能地使用它们。

**1。f 弦**

Python 3.6 提出了一种格式化字符串的绝妙方法，称为 string(格式化字符串文字)。在 Python 3 中的 f-strings 之前，我们可以通过以下两种方式中的任何一种来实现:

```
# Method_1 Using %s>> name = "Leonardo"
>> surname = "Da Vinci"
>> Occupation_1 = "Artist"
>> Occupation_2 = "Inventor"
>> Occupation_3 = "Scientist"
>> Occupation_4 = "Mathematician"
>> Occupation_5 = "Philosopher">> print ("%s %s was an %s, %s, %s, %s and %s." % (name, surname, Occupation_1, Occupation_2, Occupation_3, Occupation_4, Occupation_5))'Leonardo Da Vinci was an Artist, Inventor, Scientist, Mathematician and Philosopher.'
```

正如我们所看到的，代码的可读性完全被破坏了。下面的 str.format()就是这种情况。

```
# Method_2 Using str.format()>> print (("{name} {surname} was an {Occupation_1}, {Occupation_1}, {Occupation_1}, {Occupation_1} and {Occupation_1}.").format(name = name, surname = surname, Occupation_1 = Occupation_1, Occupation_2 = Occupation_2, Occupation_3 = Occupation_3, Occupation_4 = Occupation_4, Occupation_5 = Occupation_5))'Leonardo Da Vinci was an Artist, Inventor, Scientist, Mathematician and Philosopher.'
```

这一点也不比第一个好。幸运的是，我们有 Python 3 f-strings 来拯救我们。他们是这样工作的:

```
>> f'{name} {surname} was an {Occupation_1}, {Occupation_2}, {Occupation_3}, {Occupation_4} and {Occupation_5}.''Leonardo Da Vinci was an Artist, Inventor, Scientist, Mathematician and Philosopher.'
```

就这么简单！好家伙，我们有可读性。最重要的是，因为 f 字符串是在运行时计算的，所以你可以做很多事情，包括其他函数和方法。请看这里:

```
>> f'{2*3}'
>> f'{name.lower()}''6'
'leonardo'
```

我知道有些保守的 python 专家会引用 Python 的禅，说 f 字符串是不符合 Python 的，因为应该只有一种显而易见的方法来做事。在这种情况下，有两个。但是实用性胜过纯粹性和可读性。我现在发现了一个显而易见的方法，那就是 f 弦！

**2。清单和字典(打包、拆包)**

列表和字典是 python 中最常用的两个对象，尤其是当您对数据争论和分析感兴趣的时候。它们也很难使用。以精益的方式使用它们是极其重要的。在这种情况下，我们释放*和**的力量，作为与创建函数的列表和字典相关的重要参数。

```
>> def some_function (a,b,c,d):
       print (a,b,c,d)>> some_list = [1,2,3,4]
>> some_dictionary = {'a':1, 'b':2, 'c':13, 'd':14}
```

现在，我们来看看上面的解包是如何工作的:

```
# Unpacking list>> some_function(some_list)TypeError: some_function() missing 3 required positional arguments: 'b', 'c', and 'd'
```

如您所见，它自然地将列表视为一个位置参数。为了正确地做到这一点，您可以在列表前使用*号，瞧:

```
# Unpacking list>> some_function(*some_list)1 2 3 4
```

同样，对于字典来说，是**。

```
# Unpacking dictionary>> some_function(**some_dictionary)1 2 13 14
```

必须注意的是，字典的键是与函数相匹配的，如果键不同，你会得到一个错误。

```
# Unpacking dictionary>> some_dictionary_2 = {'x':1, 'y':2, 'z':13, 'w':14}
>> some_function(**some_dictionary_2)TypeError: some_function() got an unexpected keyword argument 'x'
```

让我们看看包装是如何工作的。对于列表，我们可以使用带*的包参数。同样，对于字典，我们使用**来打包关键字参数(kwargs)。

```
# List Packing>> def some_list_packing(*****args):
       args **=** list(args)

       args[0] **=** 'I am about to'
       args[1] **=** 'pack lists' some_function(*args) # use the previous function unpacking>> some_list_packing('I am packing','','','')I am about to pack lists# Dictionary Packing>>> def some_dictionary_packing(**kwargs):>>> for key in kwargs: print(f'{key} = {kwargs[key]}')
            # See how I used the f-string there?>>> some_dictionary_packing(a= "I", b= "am", c= "about", d= "to write..")

a = I
b = am
c = about
d = to write..
```

正如你所注意到的，列表和字典的打包和解包非常容易，可以大大简化你的代码！

**3。动态新型**

我知道这听起来像是最新的流行时尚，但在 Python 中它更酷。您知道 Python 允许您随时使用“类型”创建类吗？这是如何做到的，它比你想象的更棒:

```
>>> NewClass = type("NewClass", (object,), {"intro": "This is an awesome new class"})>>> n = NewClass()
>>> n.intro"This is an awesome new class"
```

这与以下内容完全相同:

```
>>> class NewClass(object):
        intro = "This is an awesome new class"
>>> n = NewClass()
>>> n.intro"This is an awesome new class"
```

当然，为了让您更容易理解动态类的强大功能，我简化了这里的示例。使用这个功能，您可以轻松地创建高级类。

**4。试试除了**

我无法数清有多少次感谢 python 之神创造了这一切。它测试一个代码块，并允许我们以简单有效的方式处理错误！

```
# use the unpacking lists function again for the example>> def some_function (a,b,c,d):
       print (a,b,c,d)a_list = [1,2,3,4]>> try:
       some_function (a_list)
   except:
       print(“Put * before the input list in the function”) Put * before the input list in the function
```

当您的代码变得更大时，您应该更频繁地使用它和日志功能。在任何情况下，切勿执行以下操作:

```
>> try:
       some_function (a_list)
   except:
       pass
```

蟒蛇神永远不会原谅你的！

**5。可变默认值**

这是一个漂亮的小技巧，它的好处比看起来要多，如果没有正确遵循，出错的可能性也比你最初想象的要大。下面是一个代码示例，可以让您明白我的意思:

```
>>> def something(x=[]):
        x.append(1)
        print (x)

>>> something()
[1]>>> something()
[1, 1]>>> something()
[1, 1, 1]
```

你看，每次使用这个函数时，列表都会被追加。相反，你应该使用一个缺省值来表示“未指明”,并替换为你希望作为缺省值的可变变量:

```
>>> def something(x= None):
        if x is None:
            x= []
        x.append(1)
        print (x)>>> something()
[1]
>>> something()
[1]
```

相信我，事先知道如何使用可变的默认参数将为您节省大量调试时间！

**6。价值交换**

我依稀记得当我学习 Java 时，我们被介绍交换值的基础是创建第三个临时变量。

a = 1，b =2。想交换价值观吗？创建一个临时变量 c .然后 c = a，a = b，b = c .多烦啊！下面是它在 Java 中的样子:

```
void swap(int a, int b)
{
    int temp = a;
    a = b;
    b = temp;
    // a and b are copies of the original values.
}
```

好吧，我们有 python 元组来拯救。当我们可以将它们互换位置时，为什么要创建第三个变量呢！

```
>> a = 10
>> b = 5
>> a, b = b, a >> print(a)
5>> print(b)
10
```

这没什么大不了的，但有时，从长远来看，了解简单的基础知识会对我们有很大帮助。

**7。用大括号替换空格？**

是的，伙计们，你们没听错！我们都属于 Java 阵营，我们知道你讨厌空格分隔块，喜欢那些花括号。最后，还有一个办法:

`from __future__ import braces`

一旦运行了上面的代码，您会得到

`SyntaxError: not a chance`

**逮到你了！**那永远不会发生。这么长的吸盘！

![](img/e490fdde76f9ec8352cecfc922bf7e1a.png)

图片来自 [Pixabay](https://pixabay.com/vectors/snake-python-serpent-green-reptile-312561/)

请原谅我用复活节彩蛋来描绘和开始这一小节，我控制不住自己。

但是 __ 未来 _ _ 模块比你想象的更有用。这次是真的！它有助于兼容最新版本的 python。以此为例:

如果您使用的是 Python 2.7

```
a = 1000print a>> 1000
```

现在，您可以从最新的 Python 版本中导入打印功能。

```
>> from __future__ import print_function>> print aSyntaxError: Missing parentheses in call to ‘print’. Did you mean print(a)?>> print (a)1000
```

未来模块包括一系列功能。你可以在这里查看:

 [## 未来- pysheeet

### 未来语句告诉解释器编译一些语义作为将来可用的语义…

www.pythonsheets.com](https://www.pythonsheets.com/notes/python-future.html#:~:text=__future__%20is%20a,import%20to%20current%20Python%20interpreter.&text=Future%20statements%20not%20only%20change,_Feature%20into%20the%20current%20program.) 

就是这样，伙计们！上面的清单只包含了 Python 中许多优秀功能中的一小部分。我希望你发现它们是有用的，如果你应用了它们，你的编码同行一定会发现你的代码更容易审查。

编码快乐！