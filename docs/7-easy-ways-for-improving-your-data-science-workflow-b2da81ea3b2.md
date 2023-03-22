# 改进数据科学工作流程的 7 种简单方法

> 原文：<https://towardsdatascience.com/7-easy-ways-for-improving-your-data-science-workflow-b2da81ea3b2?source=collection_archive---------24----------------------->

## 作为一名数据科学家，我学到了一些技巧

![](img/12e30210447186d5c76b4dafd241ad11.png)

[斯科特·格雷厄姆](https://unsplash.com/@sctgrhm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

赢得数据科学比赛并不容易，但我们肯定有办法改进我们实践数据科学的方式。在这篇博客中，我们将讨论 7 种可以快速改善数据科学工作流程的简单方法。这些是我离开大学后艰难地学到的技巧。希望这可以帮助您组织您的环境，更好地编码，并为您的数据科学项目找到更合适的堆栈！

## 1.组织您的项目目录

没有什么比拥有一个`Untitled.ipynb`到`Untitled9999.ipynb`的乱七八糟的文件夹，文件夹里到处散落的数据 csv，还有一堆`.ipynb_checkpoints`一样的缓存更糟糕的了。建立项目文件夹的第一步会让你的日常工作流程更加顺畅。一些有助于建立有序文件夹的常用工具有:

*   [Cookiecutter](https://cookiecutter.readthedocs.io/en/1.7.2/) :帮助你建立标准文件夹目录的 Python 包。查看[这里](/cookiecutter-plugin-for-jupyter-easily-organise-your-data-science-environment-a56f83140f72)的一个受 cookiecutter 启发的插件，它可以直接从你的 Jupyter 开发环境中访问。
*   迷你巨蟒:巨蟒的一个准系统版本。如果您不需要 Anaconda 启动程序附带的 GUI 或其他应用程序，Miniconda 是一个不错的选择。
*   requirements.txt 或 environment.yml:这有助于立即重新创建开发环境
*   。gitignore file:这有助于通过忽略不需要推送到 repo 的文件来保持 git 存储库的整洁。例如临时文件、笔记本检查点、venv 文件夹等。

关于这个话题的更多信息，请点击这里:

[](/cookiecutter-plugin-for-jupyter-easily-organise-your-data-science-environment-a56f83140f72) [## 为数据科学建立必要的 Jupyter 扩展

### 自定义 Jupyter 扩展，帮助组织您的项目文件夹

towardsdatascience.com](/cookiecutter-plugin-for-jupyter-easily-organise-your-data-science-environment-a56f83140f72) 

## 2.清理您的 Python 环境

大多数 data science medium 博客帖子会告诉你，为你的项目创建一个虚拟环境，但很少告诉你保持环境整洁的重要性。通常，当您创建开发环境时，会安装一堆生产环境不需要的 IPython 相关包。要使您的工作流程更加顺畅，您可以:

*   Multiple requirements.txt 或 enviornment.yml:对 dev 和 prod 进行单独的配置可能是有用的，但是当您添加新的包时，您需要更新这两者
*   [pipreqs](https://github.com/bndr/pipreqs) :一个 Python 包，通过只保留您的代码通过运行`pipreqs /path/to/your/project/folder`导入的包来清理您的 production requirements.txt。
*   [pipdeptree](https://github.com/naiquevin/pipdeptree) :一个 Python 包，通过运行`pipdeptree`帮助您理解 Python 环境的依赖性。

关于这个话题的更多信息，请点击这里:

[](https://medium.com/python-pandemonium/better-python-dependency-and-package-management-b5d8ea29dff1) [## 打包项目时更好的 Python 依赖性

### 这个博客话题想法我煮了很久了。我做了大量的搜索，阅读和尝试，同时致力于…

medium.com](https://medium.com/python-pandemonium/better-python-dependency-and-package-management-b5d8ea29dff1) 

## 3.更好地利用 Python 的标准库

不要重新发明轮子！很多时候，当您觉得有一些笨拙的代码需要编写时，您会重新创建 Python 提供的一些内置函数。

示例 1: `collection.Counter`用于统计出现次数

```
# instead of this
counter = dict()
for item in my_list:
    counter[item] = counter.get(item, 0) + 1# do this
from collections import Counter
counter = Counter(my_list)
```

例 2: `itertools.chain.from_iterable`用于连接列表列表

```
# instead of this
joined_list = []
for sub_list in my_list_of_list:
    joined_list.extend(sub_list)# do this
from itertools import chain
joined_list = list(chain.from_iterable(my_list_of_list))
```

例 3: `collections.namedtuple`作为一个不可变类的替换

```
# instead of this
class Color:
    def __init__(self, r, g, b):
        self.r, self.g, self.b = r, g, b# or this
RED, GREEN, BLUE = 0, 1, 2
c = (0, 255, 120) # lime green
red_value = c[RED]# do this
from collections import namedtuple
Color = namedtuple('Color', ['r', 'g', 'b'])
c = Color(0, g=255, b=120) # lime green
red_value = c.r
```

示例 4: `bisect.insort`用于维护排序列表

```
# just do this
from bisect import insort
my_sorted_list = []
for i in (1, 324, 52, 568, 24, 12, 8):
    insort(my_sorted_list, i)
```

另一个常见的陷阱是使用过度的数据结构。例如，如果一旦定义了一个列表，你就不改变它的内容，那么一个元组会节省你很多内存；或者，如果您只使用列表的第一个或最后一个元素，那么请使用 deque。

关于此主题的更多信息:

 [## Python 标准库- Python 3.9.0 文档

### 虽然 Python 语言参考描述了 Python 语言的确切语法和语义，但是这个库…

docs.python.org](https://docs.python.org/3/library/) 

## 4.包装笔记本电脑时简化您的代码

我们自己都知道，就可读性、可伸缩性和可维护性而言，开发笔记本是世界上最糟糕的脚本之一。在将笔记本打包到 Python 包中时，通常有一些经验法则:

*   **识别重复的代码片段，并将它们分组到一个函数中**:例如，我不用每次都编写一个正则表达式来进行关键字匹配，而是使用下面的代码片段:

*   **去掉多余的单元格、线和变量**:对，我们说的是`df_temp = df.copy()`、`gc.collect()`、`temp = df.some_column.unique()`
*   **将所有的函数和导入移到笔记本的开头:**我在大学的时候，C 给我上了一堂很好的课，那就是预先定义你需要的所有东西，你永远不需要担心去哪里找它们
*   **按执行顺序解开并排序单元格:**你最不想要的就是记住你是什么时候从笔记本的最后一个单元格跳到第一个单元格的

## 5.尽可能继承现有的抽象类

当涉及到组织代码时，面向对象编程和函数式编程将永远是一场争论。但是底线是，使用任何一个都比什么都不用要好。说到数据科学，有许多以 OOP 方式组织的包，例如 sci-kit learn、nltk、spacy 等等。如果你设法将你的脚本打包成扩展抽象类的类，这将有助于更好地组织你的代码，并允许与你的数据科学管道更好地集成。

例如，根据我以前的 regex 表达式构造片段，我想构造一个标记器，只返回函数捕获的标记。通过继承 nltk 的`TokenizerI`，我的`RegexpEntityTokenizer`现在与 say nltk 的`PunktSentenceTokenizer`具有相同的结构。这意味着我现在可以将我的自定义标记器传递给 sklearn `Pipeline`来实现无缝工作流。

关于这个话题的更多信息，请点击这里:

[](/object-oriented-programming-for-data-scientists-build-your-ml-estimator-7da416751f64) [## 面向数据科学家的面向对象编程:构建您的 ML 估计器

### 通过构建自己的 Scikit-learn-like，在机器学习环境中实现一些核心的 OOP 原则…

towardsdatascience.com](/object-oriented-programming-for-data-scientists-build-your-ml-estimator-7da416751f64) 

## 6.停止腌制一切

是的，我知道。腌制数据框等物品方便；你基本上可以将你的 python 脚本中的所有东西序列化到一个`.pkl`文件中。然而，pickle 的便利性伴随着几个薄弱环节:

*   **不是为速度而设计的** : Pickle 是为任何对象而设计的，这使得它比其他更专业的序列化要慢
*   **不安全**:解包一个 pickle 文件可以执行一些已经隐藏在 pickle 文件中的任意代码。想象一下，如果你的前任为你准备了一些生日恶作剧。
*   **不可移植**:不同版本的 Python 之间并不是所有的 pickled 数据都兼容。

所以记住，pickle 不是保存数据和进度的唯一方法。以下是一些备选方案:

1.  **cPickle** :底线，看看 cPickle，这是用 C 实现的 Pickle，比 Pickle 快 1000 多倍。
2.  **JSON** : JSON 序列化你的数据比 pickle 快很多。最重要的是，JSON 使得在 Python 之外的文本编辑器中存储和编辑字典(或者配置模型的 kwargs)成为可能。
3.  **NumPy** :如果你正在序列化一个定义明确的结构，你可以把它放进`numpy.ndarray`，NumPy 的`np.save`和`np.memmap`是一些更快的选择
4.  **Joblib** :对于一直在开发机器学习模型的人来说，这应该并不陌生。如果你的对象包含大的`np.ndarray`，那么 joblib 可能适合你。它的界面与 pickle 基本相同，但更简单一些，所以你使用它应该没有问题。但是，请注意 joblib 与 pickle 有相同的安全问题。
5.  **h5py** :对于之前玩过 say tensorflow 或者 keras 的人来说并不陌生。HDF5 阵列可以存储大量压缩数字数据。的确，拥有一个包含压缩数据的层次数据结构意味着一点学习曲线，也意味着查询数据集有点困难；但另一方面，您将从中获得非常高效的数据 I/O。

## 7.考虑使用数据库

最后但同样重要的是，这将是一个巨大的飞跃，并有可能改变你的游戏计划。但是相信我，在自己经历了转换之后，是值得的。查看您是否经历过以下任何情况:

*   在你巨大的熊猫数据帧上不能再加速你的 python 脚本了
*   保存和加载数据集的时间越来越长
*   您的所有数据都位于同一位置，无论是原始数据、外部数据还是经过处理的数据
*   新数据不断出现，并保存为 CSV 格式

这些都是数据库将使你的生活变得容易得多的迹象。一旦有了数据库，SQL 会帮你做很多繁重的工作。很多时候，I/O 是我的 Python 脚本的瓶颈。通过将处理向数据靠近一步，读写速度得到了不同数量级的提高。最棒的是，无论你的需求是什么，你总能找到合适的数据库:MySQL、PostgreSQL、MongoDB、CouchDB 等。

关于这个话题的更多信息，请点击这里:

[](/databases-101-introduction-to-databases-for-data-scientists-ee18c9f0785d) [## 数据库 101:数据科学家数据库简介

### 如何开始接触数据库世界？

towardsdatascience.com](/databases-101-introduction-to-databases-for-data-scientists-ee18c9f0785d) 

## **出发前**

这些是我的一些关于数据科学和 python 的中型博客，您可能想看看:

[](/efficient-implementation-of-conditional-logic-on-pandas-dataframes-4afa61eb7fce) [## 熊猫数据帧上的高效条件逻辑

### 是时候停止过于依赖。iterrows()和。应用()

towardsdatascience.com](/efficient-implementation-of-conditional-logic-on-pandas-dataframes-4afa61eb7fce) [](/cookiecutter-plugin-for-jupyter-easily-organise-your-data-science-environment-a56f83140f72) [## 为数据科学建立必要的 Jupyter 扩展

### 自定义 Jupyter 扩展，帮助组织您的项目文件夹

towardsdatascience.com](/cookiecutter-plugin-for-jupyter-easily-organise-your-data-science-environment-a56f83140f72) [](/mastering-root-searching-algorithms-in-python-7120c335a2a8) [## Python 中高效的根搜索算法

### 在 Python 中实现高效的寻根和优化搜索算法

towardsdatascience.com](/mastering-root-searching-algorithms-in-python-7120c335a2a8) 

## 好了

希望您已经发现这些提示对完善您的数据科学工作很有用。让我知道如果评论你如何找到这些技巧。

再见！

 [## Louis Chan-FTI Consulting | LinkedIn 数据科学总监

### 雄心勃勃的，好奇的和有创造力的个人，对分支知识和知识之间的相互联系有强烈的信念

www.linkedin.com](https://www.linkedin.com/in/louis-chan-b55b9287/)