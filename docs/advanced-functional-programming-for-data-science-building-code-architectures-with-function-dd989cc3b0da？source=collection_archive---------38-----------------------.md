# 数据科学高级函数编程:用函数运算符构建代码架构

> 原文：<https://towardsdatascience.com/advanced-functional-programming-for-data-science-building-code-architectures-with-function-dd989cc3b0da?source=collection_archive---------38----------------------->

## 使用函数运算符对 Pandas 中的`read_csv`函数进行矢量化

*你也可以阅读 Github 上的文章*[](https://github.com/PaulHiemstra/function_operator_article)**包括完整的可复制代码**

# *介绍*

*在[的几种编程范式中](http://www.eecs.ucf.edu/~leavens/ComS541Fall97/hw-pages/paradigms/major.html)、[、*函数式编程*、*、* (FP)非常适合数据科学。函数式编程的核心概念是一个*函数*，因此得名函数式编程。每个函数都将数据作为输入，并返回该数据的修改版本。例如，mean 函数获取一系列数字并返回这些数字的平均值。这种情况下的核心是函数没有副作用，即函数的结果不会改变函数外部的状态，也不会受外部状态的影响。这使得 FP 函数非常容易预测:给定一定的输入，输出总是相同的。](https://en.wikipedia.org/wiki/Functional_programming)*

*因此，乍一看，我们的笔记本电脑有两个组成部分:数据和对数据进行操作的功能。在大多数情况下，这两个将足以写你的笔记本。然而，当编写更复杂的代码时，比如像‘sk learn’这样的库，许多其他的 FP 概念将会派上用场。本文中我们将重点介绍的一个概念是所谓的 [*函数运算符*](http://adv-r.had.co.nz/Function-operators.html) ，它是一个 [*高阶函数*](https://en.wikipedia.org/wiki/Higher-order_function) 。函数运算符将一个或多个函数作为输入，并返回一个新函数作为结果。进度条函数操作符就是一个很好的例子，它可以给任何数据处理函数添加进度条。这些函数运算符扩展了我们的选择，使我们能够创建灵活的、可重用的 FP 代码。*

*这篇文章的重点是构建一个函数运算符*矢量化*，它可以[矢量化](https://en.wikipedia.org/wiki/Array_programming)任何现有的非矢量化函数。您将了解以下主题:*

*   *如何使用闭包在 Python 中构建函数运算符*
*   *如何使用`*args '和` **kwargs '将任何输入参数从一个函数传递到另一个函数*
*   *如何构建向量函数运算符*
*   *如何使用函数操作符创建一个清晰的函数和函数操作符的层次结构，类似于面向对象编程中的类层次结构*
*   *如何使用函数运算符让您在笔记本中编写干净的代码*

*在本文中，我们首先构建一个简单的函数运算符，并将其扩展为矢量化函数运算符。最后，我将总结一些关于如何使用函数运算符来构建代码架构的最终想法。*

*![](img/c4e5b57fb1bc6e8c121c31af4658c078.png)*

*由[罗马魔术师](https://unsplash.com/@roman_lazygeek?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/math?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片*

# *构建我们的第一个函数运算符*

*为了简化函数操作符，我们将首先构建一个非常简单的操作符。此函数运算符向输入函数添加一个计数器，用于跟踪函数被调用的频率:*

*这里的核心技巧是用`internal_function`包装`input_function`。`internal_function`的返回仅仅是调用输入函数，使得新函数的输出等于`input_function`。行为上的变化就在之前的几行代码中:调用的次数被打印到屏幕上，并且计数器增加 1。请注意，`number_of_times_called`变量是在`internal_function`的[作用域](https://matthew-brett.github.io/teaching/global_scope.html)之外定义的，我们使用`[nonlocal](https://www.programiz.com/python-programming/global-local-nonlocal-variables)`来访问该变量，尽管它超出了作用域。`number_of_times_called`在函数被调用的所有时间内保持持久的关键是`internal_function`记住了它被创建的原始作用域，即`count_times_called`的作用域。这种函数式编程技术被称为[闭包](https://www.programiz.com/python-programming/closure)。*

*因此，现在我们有两类函数:执行操作的函数(`do_nothing`)和改变其行为的函数操作符(`count_times_called)`)。给定一组操作函数和函数操作符，我们可以构建一个非常复杂和灵活的函数层次结构，我们可以混合和匹配来编写我们的笔记本。潜在函数运算符的好例子有:*

*   *进度条。接受一个函数，以及完成整个操作需要调用该函数的次数。例如，您知道需要读取 25 个文件，并且您希望看到一个进度条，显示这 25 个文件中有多少已经被读取。顺便说一下， [tqdm](https://github.com/tqdm/tqdm) 包已经实现了这样一个函数操作符。*
*   *减速函数运算符。虽然降低代码速度感觉不太直观，但这样的函数对于修改调用 API 的函数非常有用，因为 API 对每分钟调用的次数有限制。*
*   *缓存函数运算符。该函数操作符存储输入和输出的组合，当输出已经存在于缓存中时，返回给定输入的缓存版本。这个过程叫做[记忆](/memoization-in-python-57c0a738179a)。*
*   *通过交叉验证函数运算符进行超参数优化。该函数运算符包装拟合函数，并搜索给定参数的最佳值。这基本上就是 sklearn 中的 [GridSearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html) 已经在做的。*

# *构建矢量化函数算子*

*矢量化意味着您可以将向量传递给函数的输入参数，函数将执行适当的操作。例如，如果您将文件列表传递给一个读取 csv 文件的函数，它将读取所有文件。可悲的是，`pandas.read_csv`并不支持这种行为。在本节中，我们将构建一个矢量化函数操作符，它将允许我们升级`pandas.read_csv`，并允许您传递输入文件的矢量。*

*首先，我们将构建我们的矢量化函数操作符的基础版本，它将受到 R 函数[矢量化](https://stat.ethz.ch/R-manual/R-devel/library/base/html/Vectorize.html)的极大启发:*

*这里的关键代码是在`internal_function`末尾的列表理解，它实际上通过迭代作为输入给出的列表来对`input_function`进行矢量化。注意，我们需要知道哪些输入变量应该被矢量化，以便准确地编写列表理解。因此，我们首先从输入字典(`kwargs`)中获取合适的输入参数，然后使用`del`将其从列表中删除。这允许我们使用列表理解准确地构造对`input_function`的调用。*

*将其应用于`read_csv`:*

*这段代码为我们提供了一个“pandas.read_csv”版本，您可以向它传递一个文件列表，该函数返回一个包含六个 csv 文件内容的 pandas 数据帧列表。请注意，与对应的 R 相比，函数运算符有以下限制:*

*   *输出函数只支持命名参数，这是必需的，因为我使用参数名称来选择适当的输入参数进行矢量化。*
*   *该函数不能同时对多个参数进行矢量化处理*
*   *对输出不进行任何聚合。例如，这可以是将熊猫数据帧的列表自动连接成一个大的数据帧*

*下面的代码添加了最后一个特性:*

*现在，我们的函数运算符将最终结果的矢量化和聚合添加到一个大的数据框架中。在我看来，这使得`pandas.read_csv`更具表现力:用更少的代码传达同样的意图。这种表达能力使得代码可读性更强，并且在您的笔记本上更切中要点。*

*请注意，上面的代码比严格要求的要冗长一些，我可以直接使用‘PD . concat’而不用‘try _ to _ simplify’函数。然而，在这种当前形式下，代码可以根据函数返回的内容以多种方式支持简化。*

# *使用函数运算符构建代码架构*

*使用数据、对数据进行操作的函数以及对这些函数进行操作的函数运算符，我们可以构建非常复杂的代码架构。例如，我们有读取数据的`read_csv`函数，以及允许我们读取多个文件的`vectorize`函数操作符。我们还可以将进度条函数操作符应用于矢量化的`read_csv` 函数，以便在读取大量 csv 文件时跟踪进度:*

*这里，`read_data`是组合了函数的所有性质和两个函数运算符的复合函数。从大量的函数和函数运算符中，我们可以构造复杂的函数。这允许我们创建简单、易于理解的代码组件，这些组件可以组合起来创建更复杂的代码。这种基于函数运算符的代码架构在数据科学环境中非常强大。*

**你也可以阅读 Github 上的文章*[](https://github.com/PaulHiemstra/function_operator_article)**包括完整的可复制代码***