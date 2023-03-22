# 使用 Python 在几分钟内构建一个 web 数据仪表板

> 原文：<https://towardsdatascience.com/build-a-web-data-dashboard-in-just-minutes-with-python-d722076aee2b?source=collection_archive---------0----------------------->

## 通过 Plotly Dash 将您的数据可视化转换为基于 web 的仪表板，以指数方式提高功能和可访问性。

![](img/b442aee1ba7740865dc2651bd09355ec.png)

只用几行 Python 代码就能构建一个 web 数据仪表板

我不知道你是怎么想的，但我偶尔会发现不得不编写代码有点令人生畏。当我在构建类似于 *web 开发*的东西，而不是做一些本地数据分析和可视化时，更是如此。我是一个有能力的 Python 程序员，但是我根本不会称自己为 web 开发人员，即使我已经涉猎了 Django 和 Flask。

尽管如此，将您的数据输出转换为 web 应用程序会为您的项目带来一些不小的改进。

在 web 应用程序中嵌入真正的、强大的交互性要容易得多。这也意味着您可以准确控制数据的呈现方式，因为 web 应用程序可以成为事实上的报告以及数据的访问点。最后，也是最重要的，您可以指数级扩展对输出的可访问性；让它们随时随地可用。用户手边总有一个网络浏览器。

因此，我咬紧牙关，最近开始在我的一些数据项目中这样做，速度和效率惊人地快。我只用了几个小时就把这篇文章的一个输出转换成了一个 web 应用。

![](img/6c1783ff86aeffb2edbf28570b38b0f6.png)

我的 NBA 分析网络应用([链接](https://dash-nba-shot-dists.herokuapp.com))

我认为这很酷，并想分享这是如何在短短几行代码中实现的。

和往常一样，我包括了你复制我的步骤所需要的一切(数据和代码)，这篇文章并不是真的关于篮球。因此，如果您对它不熟悉，请不要担心，让我们开始吧。

# 在开始之前

## 数据

我把代码和数据放在我的 [GitLab repo 这里](https://gitlab.com/jphwang/online_articles) ( `dash_simple_nba` 目录)。所以请随意使用它/改进它。

## 包装

我假设您熟悉 python。即使你相对较新，这个教程也不应该太难。

你需要`pandas`、`plotly`和`dash`。用一个简单的`pip install [PACKAGE_NAME]`安装每一个(在你的虚拟环境中)。

# 前情提要。

在本教程中，我将简单地跳过创建我们观想的本地版本的大部分步骤。如果你对正在发生的事情感兴趣，看看这篇文章:

[](/create-effective-data-visualizations-of-proportions-94b69ad34410) [## 创建有效的比例数据可视化

### 在各种数据集规模下，查看个体对整体的贡献以及随时间变化的最佳方式—(包括…

towardsdatascience.com](/create-effective-data-visualizations-of-proportions-94b69ad34410) 

不过，我们将有一个回顾会议，因此您可以看到使用`Plotly`在本地绘制图表之间发生了什么，以及如何使用`Plotly Dash`将其移植到 web 应用程序。

## 加载数据

我对数据进行了预处理，并将其保存为 CSV 文件。这是当前 NBA 赛季(截至 2020 年 2 月 26 日)的球员数据集合，其中显示:

*   他们在自己球队的投篮命中率是多少，以及
*   他们做这件事的效率/效果如何。

**对于这部分，请在我的回购中打开** `local_plot.py` **。**

用以下内容加载数据:

```
all_teams_df = pd.read_csv(‘srcdata/shot_dist_compiled_data_2019_20.csv’)
```

用`all_teams_df.head()`检查数据，你应该看到:

每个球员的数据都是针对比赛的每一分钟(不包括加时赛)进行汇编的，统计数据`pl_acc`和`pl_pps`是唯一的例外，因为它们是针对比赛的每一个季度(每 12 分钟)进行汇编的。

这个数据框架包含了所有的 NBA 球员，所以让我们通过过滤一个球队来把它分解成一个可管理的大小。例如，新奥尔良鹈鹕队的球员可以选择:

```
all_teams_df[all_teams_df.group == 'NOP']
```

然后，我们的数据可以可视化，如下图所示:

```
import plotly.express as px
fig = px.scatter(all_teams_df[all_teams_df.group == 'NOP'], x='min_mid', y='player', size='shots_freq', color='pl_pps')
fig.show()
```

![](img/1db19dded765831e4d16fe8b3c719051.png)

新奥尔良鹈鹕队的可视化球员数据

冒着这样做的风险:

![](img/99f17c00f5d27356a8fc882b1a017970.png)

如何画一匹马— Van Oktop ( [Tweet](https://twitter.com/ossia/status/588389121053200385?lang=en) )

我确实在我的图表中添加了一些小细节，以产生相同图表的这个版本。

![](img/c3f2dd06b8265dd11e36e8686995ace8.png)

相同的图表，添加了一些“小细节”(&不同的团队)。

这是我用来做这件事的代码。

现在，虽然有很多格式化代码，但我认为向您展示我是如何做的是有用的，因为我们将在代码的 Dash 版本中重用这些函数。

现在，让我们进入主题——如何从这些情节中创建一个 web 应用程序。

# 进入万维网

你可以在这里阅读更多关于 Plotly Dash [的内容，但现在你只需要知道它是一个开源软件包，开发它是为了消除将你的可视化放到网络上的困难。](https://dash.plot.ly/introduction)

它与`Flask`一起工作，你可以愉快地重用你在`plotly.py`中用来开发情节的大部分代码。

这是我整理的简单版本:

试试吧！它应该会在您的浏览器上打开这个图。

![](img/aca4f29f2a5763133204b161db05ceb1.png)

我们的第一款 Dash 应用！

有什么大不了的？首先，它是一个活的网络应用，不到 25 行代码。注意左上方的下拉菜单？试着改变上面的值，看着图形神奇地改变。

去吧，我等着。

好吗？完成了。

让我们简单地浏览一下代码。

总体而言，我现在要做的是:

*   初始化 Dash 应用程序；
*   获取一个可用团队名称的列表，并将其提供给一个带有默认值或“TOR”的下拉菜单(带有 DOM id `group-select`);
*   实例化一个图形对象作为 Dash 中的`shot-dist-graph`标识符；和
*   创建一个回调函数，如果任何值发生变化，它将调用`update_graph`函数并将返回的对象传递给`Output`。

如果你看一下代码，那么许多对 web 开发人员来说可能微不足道但对我来说很烦人的东西都被抽象掉了。

dcc。Graph 将 plotly.py 中的图形对象包装到我的 web 应用程序中，并且可以使用 html 方便地调用和设置像 div 这样的 HTML 组件。Div 对象。

对我个人来说，最令人满意的是输入对象和来自这些输入的回调是以声明方式设置的，我可以避免处理 HTML 表单或 JavaScript 之类的东西。

由此产生的应用程序仍然运行良好。使用下拉菜单选择另一个值时，图表会更新。

我们只用了不到 25 行代码就完成了所有这些工作。

## 为什么是 Dash？

在这一点上，你可能会问——为什么是 Dash？我们可以用 JS 框架前端、Flask 或无数其他组合中的任何一个来完成所有这些。

对于像我这样喜欢 Python 的舒适性而不是原生处理 HTML 和 CSS 的人来说，使用 Dash 抽象掉了许多不会给最终产品增加很多价值的东西。

举个例子，这个应用程序的一个版本包含了更多的格式和给用户的注释:

(在 git 回购中是`simple_dash_w_format.py`

大多数更改都是修饰性的，但是我要注意，在这里，我只是在 Markdown 中编写了主体文本，并简单地从 Plotly 中继承了我的格式化函数，以用于 Dash 中的图形格式化。

这为我节省了从数据分析和可视化到部署到客户视图之间的大量时间。

总而言之，从我最初的图表开始，我认为把它部署到 Heroku 大概用了不到一个小时。这真是太神奇了。

我将深入 Dash 的更多高级特性，并实际上用它做一些很酷的功能性的事情，但我对这个结果在易用性和速度方面非常满意。

你自己试试吧——我想你会印象深刻的。下一次，我计划写一些你可以用 Dash 做的非常酷的事情，以及构建真正交互式的仪表板。

**编辑**:我正在为**的所有事物数据和可视化**开始一个**子堆栈**——这将是我直接与你接触的一种方式。我希望你能加入我。

[](https://visualnoise.substack.com/p/coming-soon) [## 在噪音中引入视觉:所有的数据和视觉化

### 数据无处不在，并且随着媒体对“大数据”的大量报道而呈指数级增长

visualnoise.substack.com](https://visualnoise.substack.com/p/coming-soon) 

我也写了一点关于为什么我要开始一个子栈。

如果你喜欢这个，比如说👋/在 [twitter](https://twitter.com/_jphwang) 上关注，或关注更新。这是数据 viz 所基于的文章:

[](/create-effective-data-visualizations-of-proportions-94b69ad34410) [## 创建有效的比例数据可视化

### 在各种数据集规模下，查看个体对整体的贡献以及随时间变化的最佳方式—(包括…

towardsdatascience.com](/create-effective-data-visualizations-of-proportions-94b69ad34410) 

此外，这是我写的另一篇关于通过更好地可视化时间序列数据来改进基于数据的故事的文章:

[](/effectively-visualize-data-across-time-to-tell-better-stories-2a2c276e031e) [## 随着时间的推移有效地可视化数据，以讲述更好的故事

### 使用 Python 和 Plotly 构建清晰易读的时序数据可视化，以支持您的叙述。

towardsdatascience.com](/effectively-visualize-data-across-time-to-tell-better-stories-2a2c276e031e)