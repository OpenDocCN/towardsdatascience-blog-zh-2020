# 编写更好的 Python 代码的终极指南

> 原文：<https://towardsdatascience.com/the-ultimate-guide-to-writing-better-python-code-1362a1209e5a?source=collection_archive---------1----------------------->

## 让你的软件更快、更易读和更易维护并不需要这么难

![](img/418b969d9ac3e07c8f582ff0462969d9.png)

让您的 Python 代码更上一层楼。戴维·克洛德在 [Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

![D](img/e7b97ba5c7306f218d23a013412aef1c.png)  D 尽管有[的缺点](/why-python-is-not-the-programming-language-of-the-future-30ddc5339b66)，Python 仍然是当今编程世界的王者。它的多功能性、初学者友好性和巨大的社区只是促成它在过去几年里巨大增长的几个因素。尽管像 [Rust](/thought-you-loved-python-wait-until-you-meet-rust-64a06d976ce) 、 [Go](/one-in-two-pythonistas-should-learn-golang-now-ba8dacaf06e8) 和 [Julia](/bye-bye-python-hello-julia-9230bff0df62) 这样的新语言越来越受欢迎，Python 的统治可能会持续几年。

无论您是初学者还是经验丰富的工程师，您都可能经常与 Python 打交道。在这种情况下，稍微升级一下你的游戏是很有意义的。

开发人员不会编写一个东西，然后让它在未来几年保持不变。因此，好的代码不仅仅是关于速度和效率。这也是关于可读性和可维护性。

就像生活中的所有事情一样，没有什么灵丹妙药可以让你的代码达到一流的性能。有一些快速解决办法，比如高效的功能和方便的自动化。然而，要真正让你的代码发光，你还需要养成长期的习惯，比如让你的代码尽可能简单，并与你的风格保持一致。

[](/12-python-tips-and-tricks-for-writing-better-code-b57e7eea580b) [## 编写更好代码的 12 个 Python 技巧和诀窍

### 通过掌握最新的 Python 特性、技术、技巧和诀窍来提高代码的质量…

towardsdatascience.com](/12-python-tips-and-tricks-for-writing-better-code-b57e7eea580b) 

# 使用更高效的功能和结构

## 用 f 弦避免弦乐打嗝

从处理输入到在屏幕上打印消息，字符串是任何编程语言的重要组成部分。在 Python 中，有三种格式化字符串的方法:原始的`%s`-方法、`str.format()`，以及从 Python 3.6 开始的 f-字符串。

当你处理简短的表达时，古老的`%s`方法很好。你需要做的就是用`%s`标记出你想要插入字符串的地方，然后在语句后引用这些字符串。像这样:

```
>>> name = "Monty"
>>> day = "Tuesday"
>>> "Happy %s, %s. Welcome to Python!" % (day, name)'Happy Tuesday, Monty. Welcome to Python!'
```

这一切都很好，但是当您同时处理大量字符串时，就会变得混乱。这就是为什么 Python 从 2.6 版本开始引入了`str.format()`。

实际上，`%s`符号被花括号取代了。像这样:

```
>>> "Happy {}, {}. Welcome to Python!".format(day, name)
```

`str.format()`的真正好处是你现在能够定义对象，然后在你的格式化语句中引用它们:

```
>>> greeting = {'name': 'Monty', 'day': 'Tuesday'}
>>> "Happy {day}, {name}. Welcome to Python!".format(day=greeting['day'], name=greeting['name']) 
```

这是一个巨大的进步，但是 f 弦更简单。考虑一下这个:

```
>>> name = "Monty"
>>> day = "Tuesday"
>>> f"Happy {day}, {name}. Welcome to Python!"
```

这就更简单了！不过，f 字符串更好的一点是，您可以在其中传递函数:

```
>>> f"Happy {day}, {name.upper()}. Welcome to Python!"'Happy Tuesday, MONTY. Welcome to Python!'
```

f 弦的最后一个优点是它们比其他两种方法更快。因为它们非常简单，所以从现在开始使用它们是很有意义的。

![](img/c5a43b23e3dcc86567e2e2a362a99274.png)

让你的代码更有效率。由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/programmer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 使用列表理解

列表理解是一种在保持可读性的同时使代码更加简洁的好方法。考虑这段代码，我们试图将前几个平方数存储在一个列表中:

```
>>> square_numbers = [0, 1, 2, 3, 4]
>>> for x in range(5):
...     square_numbers.append(x**2)
...
>>> square_numbers[0, 1, 2, 3, 4, 0, 1, 4, 9, 16]
```

这很笨拙，我们必须再次从列表中删除前几项。此外，我们仍然有变量`x`浮动并占用内存。有了列表理解，这就变得简洁多了:

```
>>> square_numbers = [x**2 for x in range(5)]
>>> square_numbers[0, 1, 4, 9, 16]
```

[语法](/12-python-tips-and-tricks-for-writing-better-code-b57e7eea580b)相当简单:

```
new_list = [**expression** for **item** in **iterable** (if **conditional**)]
```

所以下次你准备用一个`for`循环或者一个`if`语句做一个列表的时候，请三思。每次都可以节省几行代码。

## 快速创建长度为 N 的列表

在 Python 中初始化列表很容易。但是你可以通过使用`*`操作符来升级你的游戏，例如像这样的:

```
>>> four_nones = **[**None**]** * 4
>>> four_nones[None, None, None, None]
```

如果要创建一个列表列表，可以使用列表串联:

```
>>> four_lists = [[] for __ in range(4)]
>>> four_lists[[], [], [], []]
```

当你试图初始化长列表或者列表的列表时，这很方便。不仅更容易阅读和调试，而且也不太可能在列表的单个元素中出现拼写错误。

## 删除列表元素时要小心

考虑一个列表`a`，我们想要删除所有等于`bar`的条目。我们可以使用 for 循环来完成这项工作:

```
>>> a = ['foo', 'foo', 'bar', 'bar']
>>> for i in a:
...     if i == 'bar':
...             a.remove(i)
...
>>> a['foo', 'foo', 'bar']
```

但是还剩下一只`bar`！发生了什么事？

当我处于位置 0 和 1 时，它正确地认识到这些元素不需要被删除。在`i = 2`，它删除了`bar`。现在第二个`bar`移动到位置`2`。但是现在循环已经结束，因为在`i = 3`处不再有元素，因此第二个`bar`仍然存在。

简而言之，当你从列表中删除项目时，不要使用`for`循环。虽然你可以通过一些修正来避免这种错误，但是它们很容易出错。相反，列表理解再次派上了用场:

```
>>> foos = [value for value in a if value != 'bar']
>>> foos['foo', 'foo']
```

![](img/1087ec9d23d46fe004cf78143387e602.png)

给你的代码增加一些优雅。照片由[妮可·沃尔夫](https://unsplash.com/@joeel56?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/developer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 优雅地访问字典元素

使用`dict.get()`来访问字典的元素既笨拙又容易出错。考虑这个[例子](https://docs.python-guide.org/writing/style/#access-a-dictionary-element):

```
>>> d = **{**'hello': 'world'}
>>> if d.has_key('hello'):
...    print d['hello']    *# prints 'world'*
>>> else:
...    print 'default_value'
```

如果您使用`dict.get()`，会简洁得多:

```
>>> d.get('hello', 'default_value') *# prints 'world'*
>>> d.get('thingy', 'default_value') *# prints 'default_value'*
```

或者，如果你想更短，你可以写:

```
>>> if 'hello' in d:
...    d['hello']
```

## 用*、**和 _

当你有一个列表，但你不需要所有的参数，你会怎么做？为此，您可以使用`_`:

```
>>> numbers = [1, 2, 3]
>>> a, b, _ = numbers
>>> a
1
```

下划线通常用来告诉程序员这个变量不值得记住。虽然 Python 确实记住了变量，但是使用这种约定增加了代码的可读性。

如果您有一个很长的列表，但您只对前几个或后几个变量感兴趣，您可以这样做:

```
>>> long_list = [x for x in range(100)]
>>> a, b, *c, d, e, f = long_list
>>> e
98
```

以`*`开头的变量可以包含任意数量的元素。在本例中，我们创建了一个从 0 到 99 的列表，并对前两个和后三个元素进行了解压缩。具体来说，倒数第二个元素`e`是 98，这是我们所期望的。

您还可以使用`*`操作符向函数传递任意数量的参数:

```
>>> def printfunction(*args):
...    print(args)
>>> printfunction(1,2,3,4,5,6)(1, 2, 3, 4, 5, 6)
```

最后，您可以使用`**`操作符将整个[字典](/12-python-tips-and-tricks-for-writing-better-code-b57e7eea580b)传递给一个函数:

```
>>> def myfriendsfunction(name, age, profession):
...     print("Name: ", name)
...     print("Age: ", age)
...     print("Profession: ", profession)
>>> friendanne = {"name": "Anne", "age": 26, "profession": "Senior Developer"}
>>> myfriendsfunction(**friendanne)Name:  Anne
Age:  26
Profession:  Senior Developer
```

这些是访问变量的快速简单的方法，没有太多的麻烦。把它们记在心里！

## 自动打开和关闭文件

打开文件的传统方式意味着您需要再次关闭它们:

```
>>> newfile = open("file.txt")
>>> data = newfile.read()
>>>     print(data)
>>> newfile.close()
```

通过使用`with open`，您可以节省一行代码和引入 bug 的可能性:

```
>>> with open('file.txt') as f:
>>>     for line in f:
>>>         print line
```

这样，即使在`with`块中出现异常，文件[也会再次关闭](https://docs.python-guide.org/writing/style/#read-from-a-file)。

![](img/4b2e348e9eb848084d6e0b50b15173e1.png)

使用这些快速提示加快编码速度。安妮·斯普拉特在 [Unsplash](https://unsplash.com/s/photos/developer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 快速获取信息

## 在转到 StackOverflow 之前，请使用 help()

如果你想看看一个函数是做什么的，只需使用`help()`。如果你原则上知道你在做什么，但是不太确定函数的细节，这是很有用的。

您还可以通过在自己的函数中添加帮助信息来简化您的工作。这很容易做到:

```
>>> def mynewfunction(arg1, arg2):
...     """
...     This function does something really cool.
...     Hope that helps!
...     """
>>> help(mynewfunction)This function does something really cool.
Hope that helps!
```

这个帮助消息是一个 docstring，作为代码的文档也很有用。在这个例子中，您显然需要向函数添加一个主体，并使消息更加具体。但是你明白了。

## 使用 dir()了解属性和方法

如果你不太清楚一个对象是如何工作的，`dir()`可以帮助你。它返回您希望了解的任何函数、模块、列表、字典或其他对象的属性和方法。

从 Python 2.7 开始，可以使用`dir()`函数。所以对于大多数 Python 用户来说，使用这个可能是一个不错的选择。

## 使用 getsizeof()优化您的内存

如果你的程序运行缓慢，你认为这与内存问题有关，你可以用`getsizeof()`检查你的对象，以了解问题的症结所在。这相当容易:

```
>>> import sys
>>> x = [1, 2, 3, 4, 5]
>>> sys.getsizeof(x)112
```

因为在我的机器上,`x`是一个由五个 24 位整数组成的向量，所以它占用 112 位内存是完全合理的。注意，根据您的机器、架构和 Python 版本，这个数字会有所不同。

# 使文档更容易

## 选择您的 docstring 风格并坚持下去

如上所述，将 docstrings 添加到所有的模块、函数、类或方法定义中是一个非常好的主意。

为了使你的代码尽可能的可维护和可读，你应该选择一种 docstring [风格](http://daouzli.com/blog/docstring.html)并坚持下去。如果你正在合作一个项目，确保你从一开始就决定了一种风格。这使得以后阅读和添加到文档中变得更加容易。

## 您代码版本

大多数软件项目都有一个三位数的版本号，例如“Python 3.8.4”。这个系统很简单——如果你做了一些小的改动或错误修正，你可以修改最后一个数字，如果要做更大的改动，你可以修改中间的数字，如果要做更基本的改动，你可以修改第一个数字。

即使您的项目很小，保留所有的旧副本并对它们进行编号也有一些好处。如果你试图修复一个 bug，但是你最终破坏了一些东西，你总是可以回到最新的版本。如果你的项目不是向后兼容的，保留旧的拷贝是至关重要的。此外，这种编号方案创建了一个整洁的更改日志，您可以在以后参考。

![](img/aaedf29a341203f9a0571dca8b01c0c0.png)

在协作时，适当的文档记录是至关重要的。安妮·斯普拉特在 [Unsplash](https://unsplash.com/s/photos/developer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 定期领取

如果你正在合作或从事一个开源项目，这是必须的:把你的代码放在 Github、Gitlab 或任何你希望你的代码存在的地方。这样，任何人都可以在任何时间访问它。

此外，确保你经常得到它。这不仅让每个人都了解最新情况；这也是你的代码被维护并且仍然相关的标志。

## 保留变更日志

即使你使用了文档字符串并对你的版本进行了一致的编号，那也不能捕捉到你所做的所有改变。为了保证细节的安全，最好有一个单独的文档来详细解释所有的更改。

即使这几乎与最终用户无关，您的变更日志对您自己和您的同事来说都是一份有价值的文档。人类的大脑忘记得很快，现在对你来说完全显而易见的事情在两周内往往变得相当模糊。

## 使用长的描述性变量名

你注意到上面的例子了吗？函数和变量的名字几乎总是很长。这不是巧合。

对于一小段代码来说，使用如此多的字符似乎是多余的，但是对于阅读代码来说，这要好得多。此外，编写代码不会花太多时间——大多数编辑器都有自动完成功能。

## 使用 PEP 8

也许你是那种代码可以运行但格式不规范的开发人员。虽然这听起来可能没那么悲惨，但它会带来一大堆问题。更不用说那些试图阅读你的代码的同事了…

所以帮你自己一个忙，用官方 Python 风格指南 [PEP 8](https://www.python.org/dev/peps/pep-0008/) 格式化你的代码。别担心，你不需要学习每一个细节，也不需要重构你的每一段代码。您可以自动检查您的代码是否符合`pycodestyle`。在您的 shell 中，安装软件包，然后在您的 Python 文件上运行它:

```
$ pip install pycodestyle
$ pycodestyle my_file.py
```

这表明您需要在哪里进行调整，以便您的文件符合 PEP 8。如果您想让您的文件自动符合，您可以使用`autopep8`:

```
$ pip install autopep8
$ autopep8 --in-place my_file.py
```

## 评论很好

关于评论是好是坏有一个持续的争论。一些程序员认为好的代码可读性很强，你不需要任何注释。其他人希望你评论每一行。

除非你是一个纯粹主义者，否则你至少会时不时地发表评论。无论如何，确保你的评论简明扼要。我喜欢思考，如果有人从来没有看过我的代码，但现在因为他们想改变什么而看了一下，他们会喜欢读什么。这种方法效果很好。

# 养成好习惯

## 使线路延续更加稳定

原则上，您可以在每行的末尾用`\`、[分割多行表达式，就像这样](https://docs.python-guide.org/writing/style/#line-continuations):

```
>>> big_string = """This is the beginning of a really long story. \
... It's full of magicians, dragons and fabulous creatures. \
... Needless to say, it's quite scary, too."""
```

然而，这很容易出错，因为如果在`\`后面加上一个字符，即使只是一个空格，它也会停止工作。最好这样分:

```
>>> big_string = (
... "This is the beginning of a really long story. "
... "It's full of magicians, dragons and fabulous creatures. "
... "Needless to say, it's quite scary, too."
... )
```

然而，大多数时候，多行表达式是你把事情过于复杂的标志。

## 不要写意大利面条代码

这应该是显而易见的，但令人震惊的是，许多初级开发人员仍然编写单片代码。但是没有人阅读一千行代码…

因此，请确保您定义了函数，并在必要时调用它们。这将使你的代码可读性提高一千倍，你的同事也会因此而感谢你。

![](img/931ebb4f533890b9cfbd286a74555396.png)

从长远来看，保持一些好习惯并改进你的代码。照片由[阿里夫·里扬托](https://unsplash.com/@arifriyanto?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/developer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 每行只使用一条语句

这是一个类似的方向。不要做这样的事情:

```
print("one"); print("two")
```

它可能看起来很复杂，但它不是。当您在某一行中遇到错误时，您会希望确切地知道该行做了什么，以便可以快速地调试它。所以分了吧；各行各业。

## 使用类型提示

Python 是一种动态类型语言。虽然这对快速生成代码很有好处，但也会使调试变得更加困难。

从 Python 3.5 开始，可以通过添加类型提示来为调试做准备。这不会影响代码，因为解释器会忽略它。但是这对你很有帮助，因为你可以立即看到你是否传递了错误的东西给一个函数。

类型提示的工作方式如下例所示:

```
>>> def sayhello(day:str, name:str) -> str:
...     return f"Happy {day}, {name}. Welcome to Python!"
```

`:str`表示函数的参数应该是字符串，`-> str`部分表示函数应该返回一个字符串。

# 底线是:Python 很棒。更好的 Python 很神奇

不要退而求其次。Python 仍然是通用编程语言中无可争议的王者，这一事实不能成为编写平庸代码的借口。

通过遵循风格惯例、恰当地记录变更并保持代码简洁，您将使您的代码脱颖而出。在某些情况下，这甚至会提高其性能。无论如何，这将使你和你的同事更容易阅读和维护代码。

虽然这个列表包含了大量的提示和技巧，但它肯定不是详尽无遗的。如果我错过了什么，请在评论中告诉我。同时，祝您编码愉快！

*编辑:正如* [*眨眼萨维尔*](https://medium.com/u/6c6a760e66da?source=post_page-----1362a1209e5a--------------------------------) *指出的，* `*getsizeof()*` *的结果各不相同，取决于机器、架构和 Python 版本。现在包括在内了。还有，* [*奥利弗·法伦*](https://medium.com/u/87c94c990627?source=post_page-----1362a1209e5a--------------------------------) *正确的表述了下划线* `*_*` *确实是一个占用内存的变量。文本现在包括使用下划线，这是一种增加可读性的约定。而正如* [*迈克尔·弗雷德森*](https://twitter.com/mfrederickson) *在推特上指出的，* `*dir()*` *功能从 Python 2.7 开始就可用了。这个故事错误地声明它只能从 Python 3 中获得。*