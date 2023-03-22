# 想成为“真实世界”的数据科学家？

> 原文：<https://towardsdatascience.com/want-to-be-a-real-world-data-scientist-make-these-changes-to-your-portfolio-projects-e61d1139c018?source=collection_archive---------47----------------------->

## 对您的投资组合项目进行这些更改。

![](img/10c9dc45ac14071f8da7309641e36382.png)

**注:**

我注意到这个库的所有代码最初都在付费墙后面。不幸的是，我换了公司，失去了所有这些代码。

幸运的是，我重新编写了它——现在它可能更好了(尽管可能与原始代码略有不同)。储存库[可以在这里](https://github.com/jhouzz/sudoku_solver)找到。

下面的任何断开的链接应该导致这个回购代替。干杯！

**简介**

无论你想成为软件工程师、数据科学家、机器学习工程师，还是人工智能工程师；构建一个解决数独的应用程序是一个很好的项目组合。

这是一个如此伟大的项目，以至于 Peter Norvig 在这里写了一篇关于解决这个问题的惊人帖子。然后 [Aaron Fredrick 写了另一篇关于如何解决这个问题的好文章](https://medium.com/datadriveninvestor/solving-sudoku-in-seconds-or-less-with-python-1c21f10117d6)。事实上，如果你想要一个不同语言的数独解决方案，你可以从以下每个人那里找到相应语言的解决方案:

*   保罗·费尔南德斯的 C++
*   由 Richard Birkby 编写的 C#和 LINQ 版本
*   贾斯汀·克莱默的 Clojure 版本
*   Andreas Pauley 的 Erlang 版本
*   伊曼纽尔·德拉博德的哈斯克尔版本
*   约翰尼斯·布罗德沃尔的 Java 版本
*   布兰登·艾希的 Javascript 版本
*   Pankaj Kumar 的 Javascript 版本
*   卢德派极客的红宝石版本
*   [红宝石版](http://rubyforge.org/projects/sudokusolver/)作者马丁-路易·布莱特

鉴于这个问题已经被许多不同的人用许多不同的语言解决了很多次，你如何让你的解决方案脱颖而出呢？

让你脱颖而出的一个方法是扩展现有的解决方案。例如，[亚伦的解决方案](https://medium.com/datadriveninvestor/solving-sudoku-in-seconds-or-less-with-python-1c21f10117d6)包括建立一个卷积神经网络，以便在解谜之前处理数独图像。这增加了另一层复杂性，它显示了他对深度学习解决方案的舒适性。

另一种脱颖而出的方法是展示你的 ***软件工程技能*** 作为你解决方案的一部分。具体来说，雇主寻找的是那些已经了解

*   测试
*   林挺
*   证明文件
*   记录

除此之外，对于任何新用户来说，编写易于使用和维护的代码也很重要。在这篇文章中，我们将关注*测试、林挺*和*文档。*

假设我们从 Github 中所示的[初始解决方案开始。我们如何着手改变这个解决方案来展示我们的技能？](https://github.com/aaronfrederick/SudokuSolver)

> 测试

在你开始写你的解决方案之前，你可能应该写*测试。你的脑海中可能会闪现一幅如下图所示的画面，但这并不完全是我想要的…*

![](img/be397a81356161a1f23bd0ca87a198b2.png)

我[法师最初发现这里](https://susanfitzell.com/five-strategies-making-multiple-choice-tests-easier/)

python 中的测试是一种确保您打算编写的代码按照您期望的方式执行并提供您期望的输出的方法。

在这个例子中，编写测试可能意味着知道一个特定的起始难题的解决方案，然后将我们代码的结果与已知的解决方案进行比较。

一个健壮的测试工具包将有一系列不同的例子来测试。下面你可以看到一个简单的例子，其中容器和解决方案是由函数提供的。你可以想象有多个这样的例子，其中你将`container`定义为起始数独，`container_sol`是相应的解。

你也可能有一些边缘情况。我想到的三个例子是

*   如果我有一个空的板子，它有多种可能的解决方案呢？
*   如果我的起始板违反了有效的解决方案，该怎么办？例如，如果同一行中有两个 1，或者同一列中有两个 2，该怎么办？
*   如果一块起始板的值大于 9 或小于 1，该怎么办？

您可以在这里找到一个文件，其中包含我的每个测试用例[的函数。下面你可以看到我们如何引入`SudokuSolver`，并将求解器的解(`code_sol`)与我们的测试解(`sol`)进行比较。](https://git.generalassemb.ly/jbernhard-nw/sudoku_solver/blob/master/sudoku_tests.py#L1)

我们可以为我们的每个测试用例设置如上所示的案例。好消息是，两个“正常”的测试用例——在函数`test1`和`sudoku_tests.py`的`test2`中找到——可以用现有代码轻松处理。

不幸的是，边缘案例留给我们的是一个永远不会结束的程序。因此，我们需要编写代码来处理这些边缘情况。这非常简单(尽管有些乏味)，因为我们提前编写了测试，并且我们知道每种情况应该如何处理。

下面你可以看到一个函数，它将根据上面提到的三种情况来检查以确保任何提供的数独`puzzle`都是有效的。

从这个函数来看，如果返回值是`False`，那么我们的谜题就是有效的。如果返回其中一个字符串，那么我们的谜题由于该字符串指定的原因而无效。

> 林挺

现在我们所有的测试用例都准备好了，我们将开始编写我们的解决方案。众所周知，第一个解决方案可能会完成任务。也就是说，它解决了问题，但是代码可能没有被很好地文档化，不容易阅读，或者没有使用最佳的编码实践。这样就可以了！

重要的是找到解决问题的节奏，不要对编码最佳实践太过焦虑，否则你根本写不出任何东西！然而，当你有了一个可行的解决方案，你应该回去重构你的代码，提高它的可读性，看看它是否可以被优化。

我通过将现有代码改为 python `class`开始了我的重构。我编写测试的方式要求是这样的，而且很简单，把现有的函数放在一个`class`体中，在必要的地方放上一堆`self`。重构一个类的结果在这里可用[。](https://git.generalassemb.ly/jbernhard-nw/sudoku_solver/blob/master/sudoku.py)

此时，可以从您的终端运行该文件来查看两个示例，使用:

`$ python sudoku.py`

以上将产生两个起始谜题，以及每个谜题对您的主机的解决方案。

如果这个文件与您的`sudoku_tests.py`在同一个目录中，您可以运行:

`$ python sudoku_tests.py`

上面的命令不会产生任何结果，因为您的所有测试现在都应该通过了！

![](img/05fe0dcd542aa291c8b79f30db820294.png)

Tumblr 上找到的图片

从这里，我们应该可以看到我们的代码是如何遵守 Python 标准的，也就是所谓的 [PEP8](https://www.python.org/dev/peps/pep-0008/) 。这些不是严格的规则，而是指导方针。

通过运行以下命令，您可以了解您的代码符合这些准则的程度:

`$ pylint sudoku.py`

你会得到一个问题列表，以及一个分数:`Your code has been rated at -0.60/10`。而且，问题的清单很长。您可能会直接开始手动修复每个项目，但是有一些库可以帮助您加快这个过程。

![](img/e81c7567e84633f3850ff784f4434b86.png)

[最初在这里找到的图像](https://www.perforce.com/blog/qac/what-lint-code-and-why-linting-important)

有一个 [autoPEP8 库](https://github.com/hhatto/autopep8)可以用来快速清理代码。要安装，请运行以下命令:

`$ pip install —-upgrade autopep8`

然后使用新的库运行:

`$ autopep8 --in-place --aggressive --aggressive sudoku.py`

让我们看看这在多大程度上改进了我们的解决方案。

```
$ pylint sudoku.py
Your code has been rated at 7.95/10 (previous run: -0.60/10, +8.56)
```

看起来这是一个显著的进步。虽然仍然有一个相当大的变化列表，主要属于

*   间隔
*   未使用的变量
*   变量命名
*   文档字符串

虽然使用林挺的输出，每个修复都是不言自明的，但要完成完整的列表可能需要一些时间。同样，这些更多的是作为指导方针，而不是严格的规则，所以这取决于你有多在乎做出特定的改变。

我创建了`refactor-branch`，它包含了与`master`相同的代码，但是`sudoku.py`和`sudoku_tests.py`文件分别针对`8.76/10`和`9.32/10`进行了重构。你可以在这里查看[的修改](https://git.generalassemb.ly/jbernhard-nw/sudoku_solver/tree/refactor-branch)。考虑到这些分别从`-0.60/10`和`-46.60/10`开始，这已经很不错了。

> 证明文件

正如你已经从林挺过程中看到的，记录你的工作是非常重要的！针对这种特殊情况的文档意味着提供信息性的`README.md`；每个脚本的文档；以及每个脚本中每个函数、类和方法的文档字符串。

我们在上一节中使用的`pylint`可以通过简单地在必要的地方提供空字符串来欺骗。然而，[Google Style Python Docstrings](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html)提供了一个很好的指南。

![](img/b88fcb13c8ffd65ed7e4164e02a5afdc.png)

就写一篇好的自述而言，Akash 的[帖子很好地解释了一篇好自述的来龙去脉。对于这个特定的项目，我们不需要所有这些组件，但是这给了你一个好主意，当你创建一个自述文件时应该考虑什么。](https://medium.com/@meakaakka/a-beginners-guide-to-writing-a-kickass-readme-7ac01da88ab3)

一般来说，你的自述文件对于让个人与你的代码互动是非常重要的。如果你想让人们看到你的作品，附上一份好的自述。我经常在项目的最后写我的自述，因为我想变得非常有用并且与代码相关。通常我不知道如何向读者解释一些事情，直到我写完所有的代码！

你可以在下面看到我的模块字符串的例子。请注意，它包括如何运行脚本的示例，以及关于脚本用途的信息。

然后类和方法字符串应该遵循 Google 的协议，这意味着类文档包含`Args`和`Attributes`。方法字符串包含`Args`和`Returns`。如果有你认为对用户有帮助的附加信息，你也可以把它包括进去。

下面是一个例子，您可以用它来创建自己的函数字符串:

我们也可以按照相同的格式向`sudoku_tests.py`文件添加文档字符串。由于测试并不真正供其他人使用，所以愿意在这个文档上花费更少的时间似乎是合理的，但是您可以在上面链接的存储库中再次看到结果。

在完成文档的最后，我通过运行以下命令重新运行`pylint`来查看更新的分数:

```
$ pylint sudoku.py
```

和

```
$ pylint sudoku_tests.py
```

`sudoku.py`和`sudoku_tests.py`的最终得分分别为`9.66/10`和`10.00/10`。

最后，让我们把自述文件放在一起！整理自述文件没有完全正确的答案。为了展示数据科学项目，一个好的自述文件的大纲可能与创建一个生产库(这是上面 Akash 的自述文件的作用)有很大不同。对于我的项目，我通常尝试在我的自述文件中包含以下部分，但是**还是没有正确的答案**:

*   一个**项目的标题**。
*   项目的简短**描述**。
*   项目的**动机**。
*   一些关于用户如何与你的代码交互或使用你的代码的指示。
*   对你在项目中使用资源和获得灵感的地方给予肯定。
*   一个**许可证**如果对其他人如何使用你的代码有任何限制的话。

你可以在我的[库的`documentation-branch`上看到`sudoku.py`的完整文档。](https://git.generalassemb.ly/jbernhard-nw/sudoku_solver/tree/documentation-branch)

这样，您的项目看起来前所未有的专业，您已经准备好与全世界分享它了！