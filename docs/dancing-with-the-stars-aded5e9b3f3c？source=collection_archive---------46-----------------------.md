# 与星共舞

> 原文：<https://towardsdatascience.com/dancing-with-the-stars-aded5e9b3f3c?source=collection_archive---------46----------------------->

## 从简单的计算规则中探索涌现行为

![](img/45af48b6f9bd870a206f3da1fad69e98.png)

照片由 [Unsplash](https://unsplash.com/s/photos/dance-star?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的 [Allef Vinicius](https://unsplash.com/@seteph?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

对人类来说，星星是生活中最可靠的事实之一。我们的主星，太阳，在我们出生的时候就在那里，在我们死去的时候也会在那里。对于我们在夜空中可以观察到的几乎所有其他恒星来说，情况也是如此。是的，偶尔一颗恒星可能会“噗”的一声变成超新星，但总的来说，在我们的一生中，天空中的恒星不会有太大的变化。你可以用它们来设置你的时钟。

从更长的时间尺度来看，恒星会四处走动。每颗恒星都不断地被它周围的所有恒星轻微地推搡着。距离较近的恒星有很大的影响，但是距离较远的大量恒星也有影响。甚至神秘的“暗物质”也会影响恒星之间的相对运动。

几年前，Wolfram|Alpha 的天体物理学家兼研究程序员 Jeffrey Bryant 写了一篇[博文](https://blog.wolfram.com/2013/01/08/volumetric-rendering-of-colliding-galaxies/)，讲述了一个非常有趣的星系碰撞模拟，每个星系包含数十亿颗恒星。这篇文章包括完整的 Wolfram 语言代码，以重现这里显示的完整模拟:

碰撞星系是一种涌现行为的有趣例子，其中微小的个体物体对典型的微小外部影响做出反应。这种行为的另一个很好的例子是一群鸟，其中每只鸟单独对周围鸟的飞行方向的变化做出反应。

![](img/7ba3c2bbd1abc6279c66c863552ea093.png)

照片由[周](https://unsplash.com/@cszys888?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/flock-of-birds?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

用 Wolfram 语言编写非常简单的代码就可以模拟这些突发行为。我的 Wolfram 语言示例大致基于 Wolfram 社区西蒙·伍兹的帖子。它考虑了平面上的许多物体，姑且称之为粒子，它们相互作用。每个粒子都有一个“朋友”,无论它在平面上的什么地方，它总是想朝这个朋友的方向运动。每个粒子也有一个“敌人”，它总是想远离这个敌人。然后，每隔一段时间，一个粒子可能会随机得到一个新朋友和一个新敌人。这有助于保持突发行为的趣味性和动态性。最后，每个粒子都想慢慢漂移到平面中心。这个规则使粒子慢慢远离中心。

这有很多规则，但是一旦分解，用 Wolfram 语言编程就很简单。让我们从挑选粒子数开始，从 1000 开始:

```
n = 1000
```

接下来，我们在坐标范围为(-1，1) x ( -1，1)的平面上生成 **n** 个随机粒子:

```
x = RandomReal[{-1, 1}, {n, 2}]
```

每个粒子都有一个朋友和一个敌人。我们用下面一行代码一次性生成它们。列表 **p** 包含每个粒子的朋友列表，列表 **q** 包含敌人列表:

```
{p, q} = RandomInteger[{1, n}, {2, n}]
```

比如 **p** 可能以 **{621，47，874，…}** 开头表示 **x** 中的第一个粒子与 **x** 中的第 621 个粒子是好友等等。

吸引力或排斥力的大小取决于粒子和它的朋友或敌人之间的距离。以下函数 **f** 使用非常简洁的 Wolfram 语言符号一次性计算所有粒子的力:

```
f = Function[{x, i}, (#/(.01 + Sqrt[#.#])) & /@ (x[[i]] - x)]
```

为了更新一个时间步长的每个粒子，我们使用下面的代码。第一项 **0.995 x** 代表粒子向平面中心的漂移。第二项代表吸引力，参数 **0.02** 指定整体吸引力有多强。最后一项代表排斥力。它的作用与吸引力相同，但方向相反

```
x = 0.995 x + 0.02 f[x, p] - 0.01 f[x, q]
```

最后，每个时间步都运行下面的条件语句。大约有 10%的时间是正确的，当这种情况发生时，一个单独的粒子被指派一个新的朋友和敌人:

```
If[ RandomReal[] < 0.5,
 With[{r1 = RandomInteger[{1, n}]}, 
  p[[r1]] = RandomInteger[{1, n}];
  q[[r1]] = RandomInteger[{1, n}]
]]
```

为了将粒子可视化，我们使用一个基本的**图形**对象将粒子显示为白点。**动态**表达式检测其范围内是否有任何变量发生变化，如果有，它会重新评估。它本质上是一个智能的 while-true 循环，可以放在呈现的排版代码中的任何位置:

```
Graphics[{ White, PointSize[0.007],
 Dynamic[
  If[RandomReal[] < 0.1, 
   With[{r1 = RandomInteger[{1, n}]}, p[[r1]] = r; q[[r1]] = r]
  ];
  Point[x = 0.995 x + 0.02 f[x, p] - 0.01 f[x, q]]
 ]}, 
 PlotRange -> {{-1, 1}, {-1, 1}}, 
 Background -> Black,
 ImageSize -> 600
]
```

最终结果可以在 Wolfram 笔记本上直接看到(它会一直播放，直到你选择停止它)，或者在下面我为 YouTube 制作的一分钟视频中看到:

很少的代码就能产生如此复杂的行为，这总是令人惊讶的。有很多有趣的方法来创造变化，我希望在未来的帖子中探索。例如，每个粒子可以有多个朋友和敌人，而不是用简单的白点来渲染，人们可以使用任意图形(圆形，多边形)来使事情看起来更奇怪。