# 每天 10 分钟学习 Python 10

> 原文：<https://towardsdatascience.com/learning-python-10-minutes-a-day-16-c8b83919a13e?source=collection_archive---------71----------------------->

![](img/861a215f083567059b0616591c415d2f.png)

[杰瑞米·拉帕克](https://unsplash.com/@jeremy_justin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的原始照片。

## [每天 10 分钟 Python 速成班](https://towardsdatascience.com/tagged/10minutespython)

## 导入、模块、包、库和框架

这是一个[系列](https://python-10-minutes-a-day.rocks)10 分钟的简短 Python 文章，帮助您提高 Python 知识。我试着每天发一篇文章(没有承诺)，从最基础的开始，到更复杂的习惯用法。如果您对 Python 的特定主题有任何问题或要求，请随时通过 LinkedIn 联系我。

我们已经引入了一些东西，但没有真正解释发生了什么。今天我们将学习 Python 中的导入实际上非常简单。使用导入，我们可以向 Python 添加功能。因为 Python 是一个如此活跃和开放的社区，所以有很多东西需要导入。甚至 Python 本身也附带了丰富的[标准特性库](https://docs.python.org/3/library/)(包括电池)。例如， *pathlib* 库给了你文件系统的超能力。但是也有无数的第三方包和库扩展了 Python 或者使得某种功能非常容易实现。例如，Flask 是创建动态网站的一种非常简单的方法。熊猫增加了一个新的数据帧结构，这对于数据科学来说是惊人的。几乎对于任何用例，都有某种可用的包。

![](img/9bc70e969bc79aec85a1bf2c42e35025.png)

在[imgflp.com](https://imgflip.com/i/48fq5h)创造的迷因。

在我们开始之前，我们需要澄清一些术语。包含 Python 代码的文件被称为模块，具有*。py* 分机。模块可以被导入，通常由一个或多个函数或类组成。如果您导入一个模块(另一个。py 文件)到当前脚本中，这些函数和/或类在当前脚本中变得可用。这是一个很好的方式来编码干燥和共享代码。要为多个模块提供结构，您可以将它们组合成一个包。Python 中的包只不过是一个目录，其中包含所有模块和一个名为 __init__.py 的特殊文件。如果该文件位于目录中，则可以使用目录名导入包。当包变得越来越大时，您可以将它们组织到更多的文件夹中，并创建一个库。在幕后，库在概念上等同于包。框架是库的集合。例如，Flask 有许多用于授权和数据库交互的子包。通常，您导入的所有内容都可以追溯到 Python 脚本。

另一个重要的概念是名称空间。一个名称空间定义了一个名称在哪里是*已知的*(作用域)。Python 有一个全局内置名称空间，这使得像 print 这样的函数随处可见。在这个名称空间中，我们有模块名称空间。我们创建的 Python 程序也称为模块。我们定义的每个变量在这个模块名称空间中都是已知的。这意味着如果我们在脚本顶部定义 banana=3，从现在开始，这个变量在整个模块中都是已知的。每次创建函数时，它都有自己的名称空间。这意味着每个变量的定义只有函数内部知道，不能从函数外部访问。然而，来自模块命名空间的变量在函数内部是可用的。

对于下一个例子，我们首先需要创建一个模块。为此，我们将使用 IPython 中一个名为%%writefile 的神奇命令。顾名思义，它会将单元格的内容写入一个文件。该文件包含一个包含变量和函数的简短 Python 脚本:

有两种方法可以导入我们的新模块。第一个是常规导入，它将整个文件导入到其词干文件名命名空间(不带扩展名的名称)下。因为模块名可能很长(我们懒得键入)，我们可以使用*作为*关键字将名称空间重命名为较短的名称。很多热门包都有众所周知的缩写:import pandas 为 pd， *import matplotlib.pyplot 为 plt* 等。另一种导入方法是使用 *from … import* …构造。通过这种方法，你可以将模块的一部分导入到当前的名称空间中。导入的项目就像在当前模块中定义的一样可用。

当模块变得太大时，明智的做法是将它们分成更小的部分。通常它们以某种方式组合在一起。为此，创建了包。如前所述，包只是一个目录中模块的集合。当存在名为 __init__ 的文件时，目录被检测为包。init 两边的双下划线)。然后使用<directory_name>访问每个模块。<module_name>。</module_name></directory_name>

这是软件包和模块的全部内容。更大的类型(库和框架)是包的集合，组合在一个目录中，访问方式与模块相同。因此，当您使用 pip 安装包时，会创建一个包含 Python 文件的文件夹。这个文件夹位于您的 Python 路径中，这个路径是一个字符串，包含在哪里查找模块和包的目录。当我们导入一个模块时，Python 将首先在当前工作目录中寻找该模块。接下来，它将检查 Python 标准库。如果仍然找不到模块，它将遍历 Python path 环境变量中定义的路径。在 Python 中，这个路径很容易通过 *sys* 包来访问。使用 sys.path，您可以检查所有路径，甚至可以添加另一个路径，以便可以找到该目录中的包。

当编辑模块时，您会遇到一个有趣的情况:当导入已经导入的模块时，它会一直引用以前导入的模块。这意味着，如果您更改了一个模块并再次导入它，而不重新启动内核，您将仍然使用该模块的旧版本。我不得不艰难地意识到这一点，这花了我几个小时才明白发生了什么。为了解决这个问题，Python 有一个名为 *importlib* 的附加包，使用 *importlib.reload()* 函数我们可以强制重新加载一个模块。

我认为现在，我们已经对导入系统以及模块和包的工作原理有了一些了解。当然，还有[更多微妙之处需要学习](https://docs.python.org/3/reference/import.html)但是现在，这可能就足够了。需要知道的最重要的事情是，您可以使用包来扩展 Python。这个社区非常活跃，几乎任何你想做的事情，都可能有一个包可以帮助你。几乎所有的包都在 Python 包索引( [PyPi](https://pypi.org/) )中，所以这是一个开始寻找的好地方。如果你偶然发现一些不存在的东西，一般来说创建起来并不困难。如果你想获得不朽的地位，你可以把你的包上传到 PyPi，让其他人使用。分享就是关爱！

# 今天的练习:

今天的任务是创建你自己的包，它包含两个模块。第一个模块包含字符串函数，第二个模块包含数字函数(如下所述)。测试您的软件包是否正常工作。

```
# String functions (in a separate module)
def split_string(sentence):
    return sentence.split(' ')def is_long(sentence):
    if len(sentence) > 10:
        return True
    return False# Numeric functions (in a separate module)
def square_root(number):
    return number**0.5def square(number):
    return number * number
```

如有任何问题，欢迎通过 [LinkedIn](https://www.linkedin.com/in/dennisbakhuis/) 联系我。