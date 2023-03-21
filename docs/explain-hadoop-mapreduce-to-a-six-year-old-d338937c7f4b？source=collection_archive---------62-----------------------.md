# 向六岁小孩解释 Hadoop MapReduce

> 原文：<https://towardsdatascience.com/explain-hadoop-mapreduce-to-a-six-year-old-d338937c7f4b?source=collection_archive---------62----------------------->

## 好奇的头脑永远不知道自己的极限

![](img/28fe1a559ed2fdf9b7ae67d84dd0f79b.png)

[张家瑜](https://unsplash.com/@danielkcheung?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

在向我六岁的孩子解释大数据时，我履行了一个父亲的职责。尽管如此，我还是感觉到了对答案明显的不满。

我不确定原因。要么是我让我的孩子在拆他最喜欢的乐高玩具时心烦意乱，要么是我的解释不够有教育意义。无论如何，我必须想出另一个计划。

[](/explain-big-data-to-a-six-year-old-71e341e5da45) [## 向一个六岁的孩子解释大数据

### 爸爸，什么是大数据？

towardsdatascience.com](/explain-big-data-to-a-six-year-old-71e341e5da45) 

毫无疑问，我的孩子已经掌握了一两件关于大数据的事情，但大数据不仅仅是这些。我回忆了我成为大数据工程师的历程。我想到了我最初是如何理解它的每个核心概念的。MapReduce 这个词突然出现在我的脑海里。

在我职业道路的开端，Hadoop MapReduce 是我遇到的第一个关键想法。它帮助我获得了如何利用常见操作在大数据上执行大规模计算的概念。

我想这对于我六岁的孩子来说是另一个合适的概念。相比之下，我变得更加沮丧。

我不知道如何向我的孩子解释 MapReduce。

它没有任何关于计算机如何工作的线索。天真的头脑怎么可能理解 Hadoop MapReduce 的原理呢？

当我们进行这种对话时，一切都变了

*   爸爸，你最喜欢的食物是什么？
*   嗯，我喜欢披萨，你呢？
*   我喜欢冰淇淋。我太爱他们了，我长大后想娶一个冰淇淋。

就在那一刻，我的内心有了火花。我的孩子对这种喜爱的食物的热情可能是我的 MapReduce 去神秘化的关键。

那是一个炎热的夏日。我和我的宝宝站在一辆冰淇淋车前。卖冰淇淋的人友好地问我们想要什么样的冰淇淋。我不喜欢任何一个，所以我让我的孩子决定。

*   “爸爸，我想吃三色冰淇淋——它在我耳边轻声说道
*   那就两筒三色冰淇淋吧——我向忙碌的冰淇淋小贩下了订单

卖冰淇淋的人做了三勺巧克力、香草和草莓口味的冰淇淋。然后，他把这些放在一个褐色的华夫饼干筒上。

![](img/f590c66938108c661c1a5da8b8d68219.png)

照片由[弗洛伦西亚·维亚达纳](https://unsplash.com/@florenciaviadana?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

我的宝宝对它的新鲜冰淇淋很满意，毫不犹豫地开始享用。突然，一个 MapReduce 的图像闪过我的脑海。我都想明白了。我让我的宝宝停下来吃第一口。我说:“我的宝贝，我用你的三色冰淇淋给你解释 Hadoop MapReduce”。

你看到那个带着巨大冰柜的卖冰淇淋的人了吗？那是成吨的冰淇淋。每天他都要处理顾客的冰淇淋需求。想象一下，每个人都和你一样喜欢三色冰淇淋。卖冰淇淋的人必须提供那种独特的冰淇淋。

但是制作合适的三色冰淇淋需要时间。卖冰淇淋的人必须先从冰淇淋容器里拿勺子。在那里，他必须把它们放在华夫饼干筒的正确位置上，然后把它们送到顾客手中。爸爸打赌他可以这样做一整天。

想象一下那些冰淇淋容器变成了一大堆冰淇淋。成堆的冰淇淋可以让卖冰淇淋的人站在它们的树荫下。冰淇淋车现在每小时吸引成千上万的顾客。不管卖冰淇淋的人有多熟练，他永远也赶不上节奏。

作为一名大数据工程师，爸爸伸出手来帮助卖冰淇淋的人。爸爸不知道如何制作一个漂亮的冰淇淋勺，但他知道如何快速制作。像往常一样，爸爸有一些朋友帮助他。这一次他将他们分成两组:**映射器**和**减速器**。

![](img/8dee5e34ce409e8a4007bb6e1407cd9c.png)

照片由[莎拉·瓜尔蒂埃里](https://unsplash.com/@sarahjgualtieri?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

映射器是做什么的？他变了。一位制图师牺牲自己的生命来制作完美的冰淇淋勺。卖冰激淋的人的琐碎任务成了地图绘制者的生活目标。每一个都有三种口味的巧克力、香草和草莓。出色的制图者不能在组内竞争。每个制图者在不同的时间完成他的任务。它们都有助于还原剂使用的公共池。

手上拿着华夫饼干的减速器，正在等待制图者完成他们的任务。地图绘制者已经准备好了冰淇淋勺。减速器只需将它们从公共池中取出。每个减速器每分钟能够输送数千个三色冰淇淋。这种效率使得卖冰淇淋的人能够取悦他的顾客。

卖冰淇淋的人很满足，因为他想招待多少顾客就可以招待多少顾客。那些绘制者和缩减者总是关注他们唯一的可能性。他们从未让他失望。“我的孩子，你现在可以继续享用你最喜欢的三色冰淇淋了。请记住，一项简单的工作可能会因为大量的投入而变得不可能。在测绘员和减速器的帮助下，我们可以克服障碍”。

Hadoop MapReduce 的强大之处在于其组件定义明确的职责。它使我们能够将大量的工作分成单个工人就能完成的小块。

理解 Hadoop MapReduce 是每个大数据工程师的职责。它为其他大数据计算算法奠定了基础。这完全是利用平庸的计算机来获得计算能力。它激发了使用大数据的想法。

我叫 Nam Nguyen，我写关于大数据的文章。享受你的阅读？请在 Medium 和 Twitter 上关注我，了解更多更新。

[](/how-to-build-a-scalable-big-data-analytics-pipeline-7de18f8be848) [## 如何构建可扩展的大数据分析管道

### 大规模建立端到端系统

towardsdatascience.com](/how-to-build-a-scalable-big-data-analytics-pipeline-7de18f8be848)