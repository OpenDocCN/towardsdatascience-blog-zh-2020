# 一百万美元的问题

> 原文：<https://towardsdatascience.com/the-1000-000-question-c89f2daa9b34?source=collection_archive---------54----------------------->

## 一个看似微不足道的问题失控了。但是你知道答案的几率有多大？这是矛盾的

![](img/dc56656d20f462fb746d40c491fff53d.png)

介绍问题，图片作者。城市景观背景由 [Pawel Nolbert](https://unsplash.com/photos/4u2U8EO9OzY)

这个简单的小问题在社交媒体上被问及并被广泛分享。起初，看起来我们都知道答案，但当我们仔细观察时，我们意识到这不是那么微不足道的。

> 在阅读文章的其余部分之前，请在评论中告诉我们你的想法。

# 我们去兔子洞吧

我们都知道，如果我们有四个选择，有一个是正确的，那么正确的概率是 1/4=25%，这是正确的。因为在 A 和 D 处有两个 25%的可能性，我们有 1/2 给我们 50%作为正确答案。假设现在问题 C 是对的，随机选择它的几率还是 1/4=25%。

考虑到这个自我参照的美丽圈，许多用户选择了选项 B，为 0%。然而，假设这确实是正确答案，我们将有 1/4=25%的机会随机抽取。

再来一圈？让我们假设一个答案必须是正确的，这导致排除答案 b。这将导致我们相信我们有 1/3 的机会是正确的 33%，这不在黑板上，留给我们 0%的正确答案…你的脑袋转圈比任何 f 1 车手都快吗？

不用担心，正如我们所看到的，这只是一个不适定的问题，的确很多答案可以被视为正确的。通过问这个问题的方式，我们不能确定我们遇到的是什么随机方法。可以排除一个答案吗？这整个事情可以被看作是一个所谓的数学悖论。

# 悖论

有时也被称为二律背反。悖论一般指逻辑上自相矛盾的陈述。这种悖论的两个非常著名的例子是“**说谎者悖论”**和“罗素的**悖论”**

> [骗子的悖论](https://en.wikipedia.org/wiki/Liar_paradox):“这个说法是假的”
> 
> 罗素悖论:不是自身元素的所有集合的集合不能存在

**“说谎者的悖论”，**举例来说，不能被赋予一个真值，因为当它为假时它就是真的，反之亦然。

**“罗素悖论”，**是一个理解起来稍微复杂一点的例子，过去制造了不少麻烦。

同一个问题的一个稍微容易理解的版本可以通过查看

> 理发师悖论:理发师给所有不刮胡子的人剃毛

理发师现在应该给自己刮胡子吗？

真实悖论的主要问题在于，当集合不包含自身时，它会包含自身。这个看似简短的声明让当时最聪明的人怀疑自己和他们选择的研究领域。

这个悖论以伯特兰·罗素的名字命名，并于 1901 年由他发表，它造成了相当多的问题，因为它可以从他们的轴中导出。

# 为什么会有问题？

首先，我们必须明白数学是建立在公理之上的。从这些公理出发，其他一切都被证明了。公理是被认为是正确的陈述。它们是所有基于它们的推理的基础。最早的例子之一来自古希腊。

> 当从相等的数中取出相等的数时，结果是相等的数。

从这个公理出发，我们可以证明一些简单的事情，比如:

```
if a=b -> a-42=b-42 (not entirely formal ;)
```

我们需要了解的另一件小事是爆炸的 p [原理。爆炸原理指出，从一个错误的陈述，任何事情都可以被证明。比如说我们可以。](https://en.wikipedia.org/wiki/Principle_of_explosion)

```
P1: You follow me and you don't follow me
P2: You follow me *(1, simplification since both are true)*
P3: You dont' follow me *(1, simplification)*
P4: You follow me **or** You will clapp 100 times for this article
*(Since we can add anything to a already true* ***OR*** *statement and it will still be true, also called addition)* P5: You will clapp 100 times for this article
*(Since P3 is true and P4 is true, we know that the second part of P4 must be true, aka* disjunctive syllogism*)*
```

悖论和爆炸原理的结合现在基本上打破了数学、科学和一般的推理。它给我们留下了一个非常模糊的真假定义。当时数学家的成果是创造了一种新的一致的集合论。

# 结论

干得好，你今天用一些最棘手的问题挑战了自己。这些例子中的每一个都让你更加怀疑我们可以用什么和怎样来表达一个问题！

自从人类提出奇怪的问题以来，悖论就一直存在。他们的存在给我们上了宝贵的一课；有时候，不是答案不对。这是个问题！

全世界都在研究悖论，这是有原因的，它们对发展批判性思维至关重要。我希望你现在可以停止担心它，并接受一些问题确实不需要回答，假设你想保持理智。

如果你喜欢这篇文章，我会很高兴在 [Twitter](https://twitter.com/san_sluck) 或 [LinkedIn](https://www.linkedin.com/in/sandro-luck-b9293a181/) 上联系你。

一定要看看我的 [YouTube](https://www.youtube.com/channel/UCHD5o0P16usdF00-ZQVcFog?view_as=subscriber) 频道，我每周都会在那里发布新视频。