# 数据科学中的实验设计

> 原文：<https://towardsdatascience.com/designing-experiments-in-data-science-23360d2ddf84?source=collection_archive---------11----------------------->

## 约翰·霍普斯金 DS 专业化系列

## 数据科学中的实验需要适当的规划和设计来最好地回答它的问题

![](img/7c6152d7d2aa7ec6cb7f3048876dc59a.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Greg Rakozy](https://unsplash.com/@grakozy?utm_source=medium&utm_medium=referral) 拍摄的照片

```
[Full series](https://towardsdatascience.com/tagged/ds-toolbox)[**Part 1**](/the-data-scientists-toolbox-part-1-c214adcc859f) - What is Data Science, Big data and the Data Science process[**Part 2**](/how-to-learn-r-for-data-science-3a7c8326f969) - The origin of R, why use R, R vs Python and resources to learn[**Part 3**](/a-crash-course-on-version-control-and-git-github-5d04e7933070) - Version Control, Git & GitHub and best practices for sharing code.[**Part 4**](/the-six-types-of-data-analysis-75517ba7ea61) - The 6 types of Data Analysis[**Part 5**](/designing-experiments-in-data-science-23360d2ddf84) - The ability to design experiments to answer your Ds questions[**Part 6**](/what-is-a-p-value-2cd0b1898e6f) - P-value & P-hacking [**Part 7**](/big-data-its-benefits-challenges-and-future-6fddd69ab927) - Big Data, it's benefits, challenges, and future
```

*本系列基于约翰·霍普斯金大学在 Coursera 上提供的* [*数据科学专业*](https://www.coursera.org/specializations/jhu-data-science) *。本系列中的文章是基于课程的笔记，以及出于我自己学习目的的额外研究和主题。第一门课，* [*数据科学家工具箱*](https://www.coursera.org/learn/data-scientists-tools) *，笔记会分成 7 个部分。关于这个系列的注释还可以在这里找到*[](http://sux13.github.io/DataScienceSpCourseNotes/)**。**

# *介绍*

*在解决问题之前问正确的问题是一个很好的开始，它在复杂中为你指明了正确的方向。但是要真正有效地解决你的问题，你还需要适当的规划和良好的实验设计。类似于用科学方法进行实验，这需要遵循一系列简洁的步骤来进行安全实验，即设备的名称和某些东西的测量，您还必须设计您的数据科学实验。只有这样，才能铺出一条清晰的道路，用正确的方法一部分一部分地、一丝不苟地解决你的问题。以下是更多关于实验设计的内容。*

# *什么是实验设计？*

*![](img/3b5e776b6c354f5bad98ad0885c1bf85.png)*

*照片由[艾萨克·史密斯](https://unsplash.com/@isaacmsmith?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄*

*这通常意味着**组织一个实验**以便你有**正确的数据**到**有效地回答**你的 DS 问题。*

*在开始解决 DS 问题之前，可能会出现一些问题。*

*   *回答这个问题的最好方法是什么？*
*   *你在衡量什么，如何衡量？*
*   *什么是正确的数据？*
*   *我如何收集这些数据？*
*   *我使用什么工具和库？*
*   *等等…*

*所有这些问题都是 DS 实验的关键部分，必须从一开始就回答。如果没有适当的规划和设计，您将在整个工作流程中面临许多这样的问题，从而使工作效率低下。*

*那么在一个实验中什么是正确的流程呢？*

## *实验设计流程*

> *1.提出问题*
> 
> *2.设计实验*
> 
> *3.识别问题和错误来源*
> 
> *4.收集数据*

*这个过程开始于在任何数据收集之前明确地**制定你的问题**，然后**设计尽可能好的设置**来收集数据以回答你的问题，识别**的问题**或你设计中的错误来源，然后**收集数据**。*

## *重要*

> *坏数据→错误分析→错误结论*

*   *错误的结论可能会产生涓滴效应(论文中的引用最终会应用于真实的医学案例)*
*   *对于高风险的研究，如确定癌症患者的治疗计划，实验设计至关重要。*
*   *数据不好、结论错误的论文会被撤稿，名声不好。*

# *实验设计原则*

*![](img/d1003fed87a463eb5de2bc5c91eda155.png)*

*照片由[摩根·豪斯](https://unsplash.com/@morganhousel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄*

## *1.独立变量(x 轴)*

*   *被操纵的变量，不依赖于其它变量*

## *2.因变量(y 轴)*

*   *预期因自变量变化而变化的变量*

## *3.假设*

*   *对变量和实验结果之间关系的有根据的猜测*

## *例子*

*这里有一个实验设计的例子，用来测试阅读书籍和读写能力之间的关系。*

*   ***假设**:随着书籍阅读量的增加，读写能力也会提高*
*   ***X 轴**:阅读的书籍*
*   ***Y 轴**:识字*
*   *实验设置:我假设文化水平取决于阅读的书籍*
*   ***实验设计—** 测量 100 个人的书籍阅读和读写能力*
*   ***样本量(n) —** 实验中包含的实验对象数量*

*在收集数据来测试你的假设之前，你首先要考虑可能导致你的结果出错的问题，其中之一就是混杂因素。*

## *什么是混杂因素？*

*   *可能影响因变量和自变量之间关系的外来变量*

*对于这个例子，混杂因素可以是年龄变量，因为它既可以影响阅读的书籍数量，也可以影响识字率。读的书和识字的任何关系都可能是年龄造成的。*

***假设**:读书→识字*

***混杂因素**:读过的书←年龄→识字*

*记住混杂因素，设计控制这些混杂因素的实验是至关重要的，这样它就不会影响你的结果，并且你正在正确地设计你的实验。*

*有三种方法可以处理混杂因素:*

1.  ***控制***
2.  ***随机化***
3.  ***复制***

# *1.控制*

*回到读书识字实验，为了**控制**年龄对结果的影响，我们可以**测量每个人的年龄**以考虑年龄对识字的影响。*

*这将在您的研究中创建两个组:*

*(1) **对照组** & (2) **治疗组**，其中:*

*   *对照组→固定年龄的参与者*
*   *治疗组→各年龄段的参与者。*
*   *比较结果，了解固定年龄组与不同年龄组的识字能力是否不同。*

*另一个常见的例子是药物测试，为了测试药物对患者的效果，对照组不接受药物，而治疗组接受药物，并比较效果。*

## *安慰剂效应*

*这是一种偏见，一个人在心理上认为某种治疗对他们有积极影响，即使根本没有治疗。这在医学上很常见，安慰剂药物可以替代真正药物的作用。*

*为了解决这种偏见，受试者将被蒙住双眼，这意味着他们不知道自己被分到哪一组:*

*   *对照组与模拟治疗(糖丸，但告诉药物)*
*   *治疗组采用实际治疗*
*   *如果存在安慰剂，两组患者的感受是一样的*

# *2.随机选择*

*随机化基本上是将个体随机分配到不同的组，这是很好的，有两个原因，(1)你不知道混杂变量，和(2)它减少了偏向一个组以丰富混杂变量的风险*

*以药物测试为例*

*   *对许多受试者取样，进行随机化*
*   *将参与者分配到两个主要组，一个**混杂**组，另一个**随机**，每个组都有自己的对照组和治疗组*
*   *从随机分组中，你可以看出是否存在偏见。*

# *3.分身术*

*复制基本上是重复你的实验，但这次是用不同的对象，这很重要，因为它显示了你的实验的可重复性。*

*复制也是必要的，主要是因为只进行一个实验可能是偶然的，这是许多因素的结果，例如:*

*   *混杂因素分布不均匀*
*   *数据收集中的系统误差*
*   *极端值*

*如果复制完成了(用一组新的数据)并且产生了相同的结论，这表明该实验是强有力的，并且具有良好的设计。*

*此外，复制的核心是数据的可变性，这与 P 值有关，P 值有一个黄金法则( **P** ≤ **0.05** )，这是许多人在统计假设检验中努力追求的。在下一篇文章中会有更多的介绍*

# *摘要*

*![](img/babd80b741b53b3d767c47cb9bb55026.png)*

*照片由[兵浩](https://unsplash.com/@bingh?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄*

*建设一个像纽约这样的城市需要蓝图和非常精确的规划，这创造了你今天所看到的。在数据科学中设计实验也应该如此。*

*这是实验设计的基础，从根本上讲是关于精确的计划和设计，以确保您的分析或研究有适当的数据和设计，从而防止错误的结论。*

*流程的一个很酷的助记符是**QDPC**-**Q**u 问题**D**ATA**P**LAN**C**are fully*

> *制定问题→设计实验→识别问题和错误→收集数据。*

*错误的结论会产生涓滴效应，因为它们在现实生活中被引用和使用，特别是在医学上。*

## ***原理:***

*   ***独立变量** (x 轴)——被操纵且不受其他变量影响*
*   ***因变量** (y 轴)—x 的变化导致 y 的变化*
*   ***假设** —关于 X 和 Y 之间关系的有根据的猜测*

## ***实验设计***

*   ***混杂因素**——影响自变量和因变量之间关系的外来变量*
*   ***控制**——在实验中固定混杂因素或考虑和测量*
*   ***对照组**——一组测量了独立变量但未接受治疗的受试者。作为实验的对照，比较变化和效果*
*   ***盲法受试者** —将受试者盲法分组，以测试安慰剂效应*
*   ***安慰剂效应** —一个人对安慰剂治疗产生的效应的信念*
*   ***随机化** — 随机分配参与者，以呈现偏倚和混杂效应*
*   ***复制—** 重复实验到强化结果的结论*

# *参考*

*   *[五三十八](https://projects.fivethirtyeight.com/p-hacking/)*
*   *[xkcd 漫画](https://imgs.xkcd.com/comics/significant.png)*
*   *[耶鲁统计实验设计](http://www.stat.yale.edu/Courses/1997-98/101/expdes.htm)*

## *如果您对学习数据科学感兴趣，请查看“超学习”数据科学系列！*

*[](https://medium.com/better-programming/how-to-ultralearn-data-science-part-1-92e143b7257b) [## 如何“超级学习”数据科学—第 1 部分

### 这是一个简短的指南，基于《超学习》一书，应用于数据科学

medium.com](https://medium.com/better-programming/how-to-ultralearn-data-science-part-1-92e143b7257b) 

## 查看这些关于数据科学资源的文章。

[](/top-20-youtube-channels-for-data-science-in-2020-2ef4fb0d3d5) [## 2020 年你应该订阅的 25 大数据科学 YouTube 频道

### 以下是你应该关注的学习编程、机器学习和人工智能、数学和数据的最佳 YouTubers

towardsdatascience.com](/top-20-youtube-channels-for-data-science-in-2020-2ef4fb0d3d5) [](/top-20-free-data-science-ml-and-ai-moocs-on-the-internet-4036bd0aac12) [## 互联网上 20 大免费数据科学、ML 和 AI MOOCs

### 以下是关于数据科学、机器学习、深度学习和人工智能的最佳在线课程列表

towardsdatascience.com](/top-20-free-data-science-ml-and-ai-moocs-on-the-internet-4036bd0aac12) [](https://medium.com/swlh/top-20-websites-for-machine-learning-and-data-science-d0b113130068) [## 机器学习和数据科学的 20 大网站

### 这里是我列出的最好的 ML 和数据科学网站，可以提供有价值的资源和新闻。

medium.com](https://medium.com/swlh/top-20-websites-for-machine-learning-and-data-science-d0b113130068) [](/the-best-book-to-start-your-data-science-journey-f457b0994160) [## 开始数据科学之旅的最佳书籍

### 这是你从头开始学习数据科学应该读的书。

towardsdatascience.com](/the-best-book-to-start-your-data-science-journey-f457b0994160) 

# 联系人

如果你想了解我的最新文章[，请通过媒体](https://medium.com/@benthecoder07)关注我。

也关注我的其他社交资料！

*   [领英](https://www.linkedin.com/in/benthecoder/)
*   [推特](https://twitter.com/benthecoder1)
*   [GitHub](https://github.com/benthecoder)
*   [Reddit](https://www.reddit.com/user/benthecoderX)

请关注我的下一篇文章，记得**注意安全**！*