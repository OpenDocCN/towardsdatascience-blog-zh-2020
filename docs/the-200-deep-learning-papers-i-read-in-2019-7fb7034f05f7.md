# 2019 年我看的 200 篇深度学习论文

> 原文：<https://towardsdatascience.com/the-200-deep-learning-papers-i-read-in-2019-7fb7034f05f7?source=collection_archive---------27----------------------->

半年前，我在 Medium 上写了我的第一篇文章,讲述了我的日常纸质阅读之旅，作为实现我新年决心的一部分。当时，我得出结论，这种每天阅读论文的活动对于保持我的思维活跃和了解深度学习领域的最新进展至关重要。现在，在 2020 年新年前夕，我可以自豪地说，我出色地执行了我的 2019 年新年决心“每周至少阅读一篇新论文”。下半年看了 [116 篇](https://github.com/patrick-llgc/Learning-Deep-Learning#2019-12-12)，做了有条理的笔记。如果你感兴趣，这些论文的结构笔记列在下面的 Github repo 中。

[](https://github.com/patrick-llgc/Learning-Deep-Learning) [## 帕特里克-llgc/学习-深度学习

### 这个知识库包含了我关于深度学习和机器学习的论文阅读笔记。它的灵感来自丹尼·布里兹…

github.com](https://github.com/patrick-llgc/Learning-Deep-Learning) 

# 2019 年我读了什么？

![](img/fe4a9d5f736c56a58f816f0f6216b138.png)

[从大约 200 篇论文的标题中生成的文字云](https://www.wordclouds.com/)

加上上半年的 76 篇论文，我在 2019 年阅读的论文总数是 192 篇(包括我在写笔记时浏览过的论文这个数字应该是 200 多一点)。这非常接近于[每个工作日*一份文件*](https://medium.com/@patrickllgc/summary-of-paper-reading-in-2019h1-2dca535aace0) 的延伸目标。当然，我有时会偷懒，或者没有遵守早起完成一篇论文的纪律，但我会在周末努力弥补。为了更好地了解我的阅读习惯，我做了一个快速可视化，灵感来自 [ICLR OpenReview 可视化](https://github.com/shaohua0116/ICLR2019-OpenReviewData)。生成图表的代码，包括论文的标题，这里是[这里是](https://github.com/patrick-llgc/Learning-Deep-Learning/blob/master/ipynb/visualization_paper_reading_2019.ipynb)。

![](img/86633273203c5d245028c4ebaf07c2a3.png)

200 篇论文中约有 140 篇是同行评审的出版物，其余是 ArXiv 预印本。按发表年份来看，约一半的同行评议论文来自 2019 年这一年内，最早的一篇可追溯到 2012 年。(如果你想知道，2012 年唯一的出版物是 2012 年 CVPR 的原始 KITTI 数据集论文。)

![](img/222cd9cdceaea616005519a0098ba676.png)

截至发稿地点，他们中的三分之一来自 CVPR，加上 ICCV，他们占了我阅读的论文的一半左右。我发现 CVPR 的论文更注重应用，而 NIPS 和 ICLR 的论文对我的口味和我关注的自动驾驶领域来说有点太理论化了。有趣的是，我经常在 NeurIPS/ICLR 的研讨会论文中发现许多隐藏的宝石。

![](img/ba7823319d97ac294a7e455f9855c923.png)

在 2019 年下半年，除了 12 月份，我一直在严格遵守我的延伸目标。因此，每个工作日阅读一篇论文的任务是可行的，但可能有一个警告(见下文)。

# 对我有用的东西

*   坚持早起，早上看报纸。这是一个很好的唤醒练习，让我为一天做更好的准备。
*   按题目看论文。这有助于避免过于频繁的上下文切换，并在阅读相关领域的论文时保持良好的连续性。此外，这也促使我就这些话题写迷你评论博客。2019 年下半年，我已经在 Medium 上写了屈指可数的评论博客。
*   坚持写博客，从小处着手，专注于一个小领域，然后扩展到更广泛的话题。当我意识到我已经在 9 月份之前阅读了相当多关于**单目 3D 物体检测**的论文时，我决定写一篇关于它的评论，至少作为我的记录。然而，这是一项艰巨的任务，因为最近在 2019 年发表了如此多的论文(实际上关于这一主题的大多数论文都是回顾性的)。所以我从小处着手写了一篇关于用于回归物体方向的多面元损失方法的文章( [**多模态目标回归**](/anchors-and-multi-bin-loss-for-multi-modal-target-regression-647ea1974617?source=friends_link&sk=cc3d7f0691bed14293857efe01a54bc9) )。然后，我意识到文献中的偏航方向术语非常混乱，所以我开始了另一个帖子，这可能是最不专业的，但主要是概念性的( [**单目 3D 对象检测中的方向估计**](/orientation-estimation-in-monocular-3d-object-detection-f850ace91411?source=friends_link&sk=b873cbe0f2d601cac0e2e3ff8c1796fc) )。在这两篇帖子之后，我觉得更适合将我下一篇帖子的主题扩展到在后处理中将 2D 提升到 3D 的研究的特定方法(在自动驾驶中 [**将 2D 物体检测提升到 3D**](/geometric-reasoning-based-cuboid-generation-in-monocular-3d-object-detection-5ee2996270d1?source=friends_link&sk=ebead4b51a3f75476d308997dd88dd75))。最后，当我觉得自己已经充分掌握了这一领域的趋势时，我在 11 月底写了一篇综述，总结了解决看似不适定的单目 3D 物体检测问题的不同方法( [**自动驾驶中的单目 3D 物体检测——综述**](/monocular-3d-object-detection-in-autonomous-driving-2476a3c7f57e?source=friends_link&sk=160d236be1881b6ee1b431a943666fdb) )。

# 需要改进的地方

*   Read, but also skim. I still tend to get bogged down in details. It is my habit to dive directly into papers in a linear manner with the same amount of attention to detail. Professor Srinivasan Keshav from Cambridge University has written [a short article](https://blizzard.cs.uwaterloo.ca/keshav/home/Papers/data/07/paper-reading.pdf) on how to read a scientific paper in three passes (Summary in Chinese can be found on [知乎](https://zhuanlan.zhihu.com/p/100068913)). MIT also has [a guideline on reading paper under time constraints](https://be.mit.edu/sites/default/files/documents/HowToReadAScientificPaper.pdf). I should definitely start practicing these more disciplined methods to guide me to decide when to skim through and when to dive deep. Reading one paper a day in full detail is not sustainable for me as a software engineer — although this may be doable and even make sense to do so for full-time researchers.

![](img/39e774d2efaac2c61f4fdebfee98c4dd.png)

麻省理工学院如何在时间限制下阅读论文的图解指南

*   质量重于数量。看高质量的论文，多花时间在高质量的论文上。低质量的论文经常有相互矛盾或不确定的结果(可能是由于过早和不充分的实验)。这尤其难以培养对特定主题的系统和连贯的理解。如何辨认这些文件？首先，我应该**有一份已知能产生高质量论文的作者和研究小组的观察名单**。其次，我应该浏览主要会议的高分或**最佳论文提名**。第三，阅读稍微老一点但有开创性的论文(一两年前的)，有**很多引用**。第四，找到发布了源代码的**论文，读取代码。**
*   [***说话便宜，给我看看代码。***](https://en.wikiquote.org/wiki/Linus_Torvalds#2000-04) 多花点时间在 Github 上看源代码。我多次意识到，代码并不总是反映论文所宣称的内容。我的 2020 新年决心应该是，1)每个月阅读和玩**一个高质量的 GitHub repo** 并写关于读码的复习笔记；2)每月三遍深入阅读 **10 篇论文**(每篇需要几个小时)。这大约是我在深度学习纸堆中犁过的速度的一半，这应该给我更多的时间来消化。

# 展望 2020 年

回顾 2019 年这一年，例行论文阅读的习惯真的重塑了我生活的很多方面。很多个早晨，我都迫不及待地从床上爬起来，一头扎进我一直渴望阅读的报纸——这种感觉就像你在网飞疯狂观看一些精彩节目时的感觉。这些论文为我的日常项目提供了许多新的想法，并证实了我自己的许多想法，从而使我对它们更有信心。

最后但同样重要的是，我感谢机器学习和计算机视觉社区让这个领域成为人们分享思想的开放平台。ArXiv 和 Github 可能是促成这一波人工智能革命的两个最大的催化剂。我也感到真正的幸运，能够在这个领域工作，将许多最新的研究成果转化为产品。在 2020 年的新的一年，我期待深度学习及其在自动驾驶中的应用取得更多令人兴奋的进展。