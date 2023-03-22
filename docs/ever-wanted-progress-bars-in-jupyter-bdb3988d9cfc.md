# 想要 Jupyter 的进度条吗？

> 原文：<https://towardsdatascience.com/ever-wanted-progress-bars-in-jupyter-bdb3988d9cfc?source=collection_archive---------3----------------------->

## 对你的长时间循环进行健全性检查(和一点视觉风格)

![](img/9e9f3cdd1fb7ea374e4e5823054f674b.png)

照片由[盖尔·马塞尔](https://unsplash.com/@gaellemarcel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

> 如何使用循环可以告诉你很多编程技巧。

## 介绍

循环可能非常有效，但有时运行时间太长。特别是，随着您更多地使用大数据，您将不得不处理大量数据集，并使用循环来执行每个观察的计算。

当你不得不花几分钟甚至几个小时在笔记本上寻找星号时，循环一点也不好玩。最糟糕的是，当循环有数千次迭代时，您不知道循环是否正常工作，或者只是因为数据中的一些愚蠢错误而停留在某次迭代上。

好消息是你可以用`tqdm`库为 Jupyter 笔记本创建进度条。这个库非常容易使用，而且它也给难看的黑白笔记本带来了某种风格。此外，在进行过程中，您还可以让条形图显示循环正在进行的迭代。不错吧。在这里，观看它的行动:

[](https://ibexorigin.medium.com/membership) [## 通过我的推荐链接加入 Medium-BEXGBoost

### 获得独家访问我的所有⚡premium⚡内容和所有媒体没有限制。支持我的工作，给我买一个…

ibexorigin.medium.com](https://ibexorigin.medium.com/membership) 

获得由强大的 AI-Alpha 信号选择和总结的最佳和最新的 ML 和 AI 论文:

[](https://alphasignal.ai/?referrer=Bex) [## 阿尔法信号|机器学习的极品。艾总结的。

### 留在循环中，不用花无数时间浏览下一个突破；我们的算法识别…

alphasignal.ai](https://alphasignal.ai/?referrer=Bex) 

> 如果你想知道`tqdm`到底是什么意思，这个词是一个类似的阿拉伯词的缩写，意思是**进步**。这就对了，上下文关系。

## 可点击的目录(仅限网络)

∘ [简介](#fe9d)
∘ [安装](#ae7a)
∘ [自动进度条](#d8e4)
∘ [嵌套进度条](#0819)
∘ [手动控制进度条](#b920)
∘ [总结](#6210)

## 装置

安装`tqdm`对于脚本来说非常简单。对于 Jupyter 笔记本或 Jupyter 实验室，还需要一些额外的步骤(感谢用户， [Sam Wilkinson](https://towardsdatascience.com/@sammycdubs) 展示了这些方法):

1.  安装`tqdm`(这一步对于脚本来说足够了):

```
pip install tqdm                    # pip
conda install -c conda-forge tqdm   # conda
```

2.Jupyter 笔记本(经典款)的后续产品:

```
pip install ipywidgets
jupyter nbextension enable --py widgetsnbextension
```

3.继续 Jupyter 实验室(加上所有上述步骤):

```
jupyter labextension install @jupyter-widgets/jupyterlab-manager
```

> 如果你想知道更多关于经典 Jupyter 和 JupyterLab 的区别，请阅读[这个](https://stackoverflow.com/questions/50982686/what-is-the-difference-between-jupyter-notebook-and-jupyterlab#:~:text=The%20current%20release%20of%20JupyterLab%20is%20suitable%20for%20general%20daily%20use.&text=JupyterLab%20will%20eventually%20replace%20the,has%20a%20extensible%20modular%20architecture.) StackOverflow 线程。

## 自动进度条

安装后，库的基本用法非常简单。对于笔记本电脑，您可以像这样导入主模块:

`tqdm`子模块提供模块的所有灵活性和大部分功能。`trange`是一个使用`range()`功能创建进度条的快捷功能。让我们从这个开始:

`trange`只提供单一功能。对于除了`range objects`之外的其他类型的迭代，我们将使用`tqdm`。让我们使用`tqdm`来看看同样的进度条:

一般的语法就是这样。在您的循环中，只需将 iterable 包装在`tqdm()`中。

> Iterables 基本上是任何可以循环的对象，比如列表、生成器对象等。

## 带有进度条的嵌套 for 循环

进度条的另一个很好的用例是在循环中使用嵌套的*。所有循环级别的语法都是相同的。为了区分进度条，我们可以使用`tqdm()`的另一个参数叫做`desc`。它将允许我们命名酒吧:*

输出中进度条的数量取决于外部的 iterable 级别。所以当你对外层有一个很长的描述时，要注意这一点。内部水平条的数量与可迭代的长度一样多。

## 手动控制进度条

有时，你会处理一个很长的循环，并且很难观察到循环在哪个成员上工作。`tqdm`为此提供了一个非常有用的替代方案:

这比使用挤出输出单元格的`print`语句要好得多。在上面的代码中，我们首先在循环之外创建了这个条。接下来，我们使用`in`后的 bar，编写循环体。为了指示循环的成员，我们使用进度条的`set_description()`方法并传递所需的字符串。

## 包裹

在这篇文章中，我只展示了简单循环中进度条的例子。当您处理更复杂的问题时，如对数千个文件执行操作或迭代百万行长的数据帧时，您会看到它们的好处。还有一些东西我没有包括在这篇文章中，所以一定要查看库的[文档](https://github.com/tqdm/tqdm)来了解更多！

# 如果你喜欢这篇文章，请分享并留下反馈。作为一名作家，你的支持对我来说意味着一切！

阅读更多文章:

[](/from-kagglers-best-project-setup-for-ds-and-ml-ffb253485f98) [## 来自 Kagglers:DS 和 ML 的最佳项目设置

### 来自顶级 Kagglers 的项目成功最佳实践的集合

towardsdatascience.com](/from-kagglers-best-project-setup-for-ds-and-ml-ffb253485f98) [](/ultimate-guide-to-merging-joining-data-in-pandas-c99e482a73b9) [## 熊猫合并/连接数据终极指南

### 从半连接/反连接到验证数据合并

towardsdatascience.com](/ultimate-guide-to-merging-joining-data-in-pandas-c99e482a73b9) [](/mastering-catplot-in-seaborn-categorical-data-visualization-guide-abab7b2067af) [## 掌握 Seaborn 中的 catplot():分类数据可视化指南。

### 如果你能在锡伯恩做到，那就在锡伯恩做吧，#2

towardsdatascience.com](/mastering-catplot-in-seaborn-categorical-data-visualization-guide-abab7b2067af) [](/deep-guide-into-styling-plots-delivering-effective-visuals-12e40107b380) [## 我的情节糟透了。以下是我如何修复它们。

### 你的，在某种意义上，可能也是。

towardsdatascience.com](/deep-guide-into-styling-plots-delivering-effective-visuals-12e40107b380) [](/finally-learn-to-annotate-place-text-in-any-part-of-the-plot-d9bcc93c153f) [## 掌握` plt.annotate()`让您的情节更上一层楼

### 上帝保佑所有阅读 Matplotlib 文档的人

towardsdatascience.com](/finally-learn-to-annotate-place-text-in-any-part-of-the-plot-d9bcc93c153f)