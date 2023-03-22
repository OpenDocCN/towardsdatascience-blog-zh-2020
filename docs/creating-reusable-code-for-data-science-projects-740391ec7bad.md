# 为数据科学项目创建可重用的代码

> 原文：<https://towardsdatascience.com/creating-reusable-code-for-data-science-projects-740391ec7bad?source=collection_archive---------26----------------------->

## 编写自己的库的路线图

作为有抱负的数据科学家，我们花费大量时间编写代码，但是从更大的角度来看，数据科学的核心不是编写代码，而是理解我们的数据并从中提取价值。编码部分只是完成这个目标的手段。显然，我们无法避免编写代码，这样做可能对过程有害，但是我们可以减少我们花在编写代码上的时间，这使我们能够将大部分时间花在制定策略以实现我们的主要目标上。

对我们大多数人来说，这个过程如下。我们倾向于编写小代码块，然后根据需要进行复制、粘贴和修改。这对于小任务来说是没问题的，但是对于大块的代码多次这样做可能会使你的笔记本杂乱无章，很难找到东西，并且更多的时候，在尝试不同的策略时会破坏笔记本。在多个不同的地方修改代码也是非常烦人的，再次浪费你的时间。

我们中的一些人定义了一些函数来解决这个问题，但是问题是每当我们创建新的函数时，我们必须不断地将这些函数复制到不同的笔记本中。此外，当我们在以后的项目中工作时，可能很难检索我们在早期项目中使用的函数，这使我们花费时间试图在我们所有的项目中找到它，然后只是放弃并再次写出一个自定义函数。

![](img/093e3d6157b1cd9a0d40009f905efc35.png)

图片来自:[https://imgur.com/gallery/0BpqqmW](https://imgur.com/gallery/0BpqqmW)

# 级别 1 —输入。py 文件

这个问题的最佳解决方案是将您的代码定义在外部 py 文件的函数中。这样，您可以在任何需要的时候在任何笔记本中重用您的代码。你所需要做的就是导入那些函数，就像导入你最喜欢的库并使用它们一样。这使您的代码保持有组织，并专注于任务。不会有大块大块的代码分散你对主要任务的注意力，在你的笔记本上找东西会变得容易得多。当你移动到一个不同的项目时，你所需要做的就是将这些 py 文件复制到项目文件夹中，这样你就万事俱备了。(在文章的最后，我讨论了一些方法来帮助你避免这样做。)

现在，我们如何创建这些文件？让我们用一个简单的例子一步步来。

让我们创建一个名为 custom_mean()的函数，它接收一个列表，将所有值加 2，再乘以 2，然后返回平均值。当然，这对于任何真正有用的函数来说都太简单了，但是您可以根据自己的需要将它变成任何样子。

现在，打开一个文本编辑器，将函数复制到其中。在页面顶部，添加您已经用于自己的函数的任何导入函数。在我的例子中，我使用了 Numpy 的 mean()，所以我的文件看起来像这样。

```
import numpy as npdef my_mean(list):
   list = np.array(list)
   return np.mean((list + 2) * 2)
```

接下来，只需用您希望用一个*调用您的导入的名称来保存您的文件。py* 分机。我就把我的*叫做 custom_means.py* 。

现在把它导入你的笔记本，你可以这样做。

```
from custom_means import my_mean
```

现在让我们试着利用它。

```
list = [34, 54, 46, 57, 86, 34]
print('My Mean:', my_mean(list))# Output
# My Mean: 107.66666666666667
```

这很容易。现在你如何把这带到下一个层次？

# 第 2 级—目录

随着项目的进展，您可以在文件中添加更多的函数。您还可以为 Scikit-Learn 之类的库创建定制类。你可以在这里学习如何做那个。

最终，当你写了更多的函数和类时，你会了解到，根据它们完成的任务的种类，在不同的文件中组织它们是很重要的，就像我们使用的流行的库一样。例如，您可能希望您的科学计算功能是 numpy 的一部分，而您的绘图实用程序是 matplotlib 的一部分。所以，对你的函数做同样的事情是明智的。

随着文件数量的增加，将所有文件放在一个或多个文件夹中会变得更加容易。现在，如果一个文件不在你正在工作的同一个文件夹中，你如何从这个文件中导入函数呢？答案是你一直在做却没有意识到的事情。我可以用上一个例子中的同一个文件来举例说明。让我们将它添加到一个名为 *utilities* 的文件夹中。

在我们的工作目录中，我们现在有了包含 *custom_means.py* 文件的 *utilities* 文件夹。为了导入相同的函数，我们现在可以输入。

```
from utilities.custom_means import my_mean
```

看起来很熟悉，不是吗？例如，当我们从 Scikit-learn 中导入 RandomForestRegressor 时，

```
from sklearn.tree import RandomForestRegressor
```

我们实际做的是访问 sklearn 安装中的树目录，然后从 *_classes.py* 文件中导入类。你可以在这里看一看。

请注意，这与我们上面的例子略有不同，但想法是一样的。这些差异是由于他们使用额外的方法来加速使用 Cython 的代码，这需要额外的文件。由于项目的规模，他们有一种更复杂但有效的方法来管理他们的文件。 *__init__。py* 文件自动告诉 python 每个类的代码应该查看哪个文件，而不需要我们明确地告诉它

```
from sklearn.tree._classes import RandomForestRegressor#Note: This produces an error, because of the way the code is #organized
```

你现在不必担心，但是把它作为将来某一天要达到的目标是很好的。

在文件夹中组织我们所有的自定义实用程序会非常有帮助。现在我们只需将文件夹复制到任何新项目的工作目录中，我们就完成了。随着时间的推移，您的工具在调整后变得更加完善，能够调用这些实用程序而不必每次都复制您的实用程序文件夹最终可能会很有用。

# 级别 3-Python 路径

现在让我们进入下一个阶段。除非你有一套不需要经常编辑的有用工具，否则不值得这么做。如果您的函数和类还没有完成，您可能应该继续将文件夹复制到新的项目目录中。但是现在让我们假设我们有一些完善的工具。

为了理解这一点，我们需要理解当您导入一个包时会发生什么。Python 首先检查你的工作目录，然后检查‘path’变量，看看你要导入的函数是否在那里。path 变量实际上是计算机中安装各种软件包的位置列表。您可以通过运行以下命令来查看您的路径变量

```
import sys
sys.path
```

我电脑上的输出如下。我使用的是 Linux 系统，它会根据你使用的操作系统而有所不同，但是不管你用的是什么电脑，这个过程都是一样的。

```
['/home/anupjsebastian/anaconda3/envs/my-env/lib/python37.zip',
 '/home/anupjsebastian/anaconda3/envs/my-env/lib/python3.7',
 '/home/anupjsebastian/anaconda3/envs/my-env/lib/python3.7/lib-dynload',
 '',
 '/home/anupjsebastian/anaconda3/envs/my-env/lib/python3.7/site-packages',
 '/home/anupjsebastian/anaconda3/envs/my-env/lib/python3.7/site-packages/IPython/extensions',
 '/home/anupjsebastian/.ipython']
```

如果您将 utilities 文件夹放在这些目录中的一个目录中，您将能够像访问您使用的所有其他包一样访问您的函数和类。我个人更希望是在

```
/home/anupjsebastian/anaconda3/envs/my-env/lib/python3.7
```

因为的其余部分主要是这个目录的子目录。

还有一种方法是将其放置在自定义位置。对于 Linux 和 Mac，在终端中键入以下内容

```
# For Linux
nano ~/.bashrc# For Mac
nano ~/.bash_profile
```

并将下面一行添加到文件的末尾。

```
# For Linux
export PYTHONPATH=/Your/path/here# For Mac
export PYTHONPATH="/Your/path/here"
```

对于 Windows，你可以在这里找到[。更多关于 Python 路径的细节可以在](https://www.architectryan.com/2018/08/31/how-to-change-environment-variables-on-windows-10/)[这里](https://bic-berkeley.github.io/psych-214-fall-2016/using_pythonpath.html)找到。

## 奖金水平

以简单优雅的方式做同样的事情的一个酷方法是使用 GitHub。你可以把你的代码放在 GitHub 仓库里，然后把它安装到你的电脑上。这确保了软件包会自动与 pip 安装的其他库一起放置在正确的位置。pip 安装 GitHub 存储库的代码如下。

```
pip install git+(weblink here)# For example pip install git+[https://github.com/scikit-learn/scikit-learn](https://github.com/scikit-learn/scikit-learn)
```

我在上面展示了一个关于如何从其 [GitHub](https://github.com/scikit-learn/scikit-learn) 库安装 scikit learn 的例子，但是我不建议你为 Scikit-learn 或任何专业软件包这样做，除非你知道你在做什么。但是，您可以为自己的包做这件事。

这就是全部内容——这是一个完整的路线图，说明如何通过编写可重用的代码和自动化您最终为每个新项目所做的大量繁琐过程，来逐渐减少您花费在编码上的时间并专注于手头的任务。

总之，首先在代码中编写更多的函数来完成重复的任务，然后将它们转移到单独的 py 文件中，这样它们就可以被很好地组织起来，并且很容易被利用。然后使用目录来管理您的 pip 文件。继续调整和开发您的可重用代码，在您认为它们已经可以在黄金时间使用之后，将它们放在一个您可以从任何地方轻松访问它们的位置。或许，你可以在 GitHub 上分享你的代码，并回馈给开源社区。

这篇文章包含了大量的信息，一次可能要吸收很多。我希望无论你处于学习的哪个阶段，这篇文章都对你有用。即使你不打算做此时此刻提到的每一件事，在你需要的时候回到这里也是有用的。

祝你好运！