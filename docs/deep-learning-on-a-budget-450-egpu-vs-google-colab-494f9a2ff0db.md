# 深度学习预算:450 美元 eGPU vs 谷歌 Colab

> 原文：<https://towardsdatascience.com/deep-learning-on-a-budget-450-egpu-vs-google-colab-494f9a2ff0db?source=collection_archive---------5----------------------->

## Colab 对于开始深度学习来说是非凡的，但它如何与 eGPU +超极本相抗衡？

![](img/63ad7c2ce0f00192e07bda30bd9c005b.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由[娜娜杜瓦](https://unsplash.com/@nanadua11?utm_source=medium&utm_medium=referral)拍摄的照片

深度学习很贵。即使是最简单的任务，GPU 也是必不可少的。对于希望获得最佳按需处理能力的人来说，一台新电脑的价格将超过 1500 美元，而借用云计算服务的处理能力，在大量使用的情况下，每个月的费用很容易超过 100 美元。这对企业来说绝对没问题，但对普通个人来说，这意味着更多。

正因为如此，2 个月前，我进行了第一次重大采购，以给自己合理的计算能力。我已经有了一台配有小 GPU 的旧 XPS 15(GTX 960m，只是不合适)，所以我决定买一台 Razer core + NVIDIA GTX 1080，我立即拥有了大约 4 倍于我以前的处理能力，价格不到 450 美元(我买的都是二手的)。这是一个如此巨大的转变，以至于我写了一篇中型文章详细描述了整个过程和结果([你可以在这里看到](/how-i-turned-my-older-laptop-into-a-machine-learning-superstar-with-an-egpu-66679aa27a7c))。

这篇文章很受欢迎，但是我的许多读者和同事问我一个问题，“这和 colab 相比如何？”对于所有这些，我会回答说，我听说有人喜欢它，但我从来没有考虑过。毕竟，一个免费的 GPU 能有多好？

在我进入结果之前，我将给出一个关于 [Colab](https://research.google.com/colaboratory/faq.html#usage-limits) 本身的小背景。Colab 是谷歌研究院开发的一款产品，让任何人都能执行 Python 代码。它完全在浏览器中运行，并使用 google drive 作为其主要文件存储。这使得它很容易被共享，并且完全在云中。(你可以点击查看 colab [的精彩介绍)](https://neptune.ai/blog/google-colab-dealing-with-files)

问题是计算资源没有保证，这意味着你基本上得到了谷歌目前没有使用的东西。Colab 表示，大多数时候，这意味着分配的 GPU 将从 Nvidia K80s、T4s、P4s 和 P100s 中选择。另一个问题是，如果您暂时不使用 Colab，它会将这些资源分配给其他人。在我的测试中，这不是很宽容。甚至在我带我的狗去洗手间的时候，我也经历了超时。如果您让某个东西运行，做其他事情，并且在完成时没有保存结果，这可能会很烦人。最后一个缺点是运行时间有 12 小时的限制。大多数人不应该担心这一点，但是对于大型项目，调优可能需要一段时间，这可能会很麻烦。

现在我已经解释了什么是 Colab，我可以解释我是如何测试它的。与我在上一篇文章中用来测试我的 eGPU 的类似，我使用了 [AI 基准](http://ai-benchmark.com/ranking_deeplearning.html)，这是一个非常简单但功能强大的 python 库，它利用 Tensorflow 在 19 个不同的部分上运行 [42 个测试，为 GPU 提供了很好的通用 AI 分数。接下来，我初始化了一个运行时 15 次，每次都运行 AI benchmark，记录结果以及分配的 GPU——我得到了 K80 10 次，P100 4 次，T4 1 次(我从未得到 P4，但它的性能应该比 T4 略差)——下面是速度结果！](http://ai-benchmark.com/alpha)

![](img/c6216a83402bde1086816b111ecfda0e.png)

Colab 结果

正如你所看到的，结果有很大的差异，顶级 GPU 比他们可以指定的最慢的 GPU 快近 4 倍，但对于一个免费的 GPU 来说，它们都是惊人的。我很惊讶这些都是免费的。它们都提供 12g 以上的内存，对于大多数项目来说足够了，当 P100 被分配时，它的速度接近全新的 RTX 2070。即使是最慢、最常见的 K80，其性能也比 GTX 1050 Ti 略差，后者仍然是一款 150 美元的 GPU。

这与我的 eGPU 相比如何？老实说，它堆叠得非常好。下面是我的 1080 设置绘制在旁边时的相同图形！

![](img/1cd86ba38c60d9c1fbd34c1960953dcc.png)

科 lab vs GTX 1080 eGPU

在中等情况下，Colab 将为用户分配一个 K80，GTX 1080 的速度大约是它的两倍，这对 Colab 来说不是特别好。然而，有时，当 P100 被分配时，P100 是绝对的杀手级 GPU(再次为**免费**)。因为 P100 是如此之大，当比较一般情况时，您可以看到两个结果没有太大的不同，我的 eGPU 平均只提高了 15%左右。

![](img/1044d93639371c3412013630078762b9.png)

eGPU 与 Colab 平均值

因此，如果你想进入机器学习领域(或者想升级你目前的设置)，你应该怎么做？我个人的建议是，如果你只是想涉足机器学习(至少是开始)，参加一个初级的机器学习课程，或者并不总是需要计算资源，Colab 绝对是一个不用动脑筋的人。它免费提供计算机是巨大的。

然而，如果您觉得受到 Colab 产品的限制，可能需要更快的速度，随时了解您的资源，需要长时间的训练，想要使用非 python 语言，或者计划使用 GPU 来完成机器学习任务以外的任务，eGPU(或专用 GPU)可能会更适合您！

我也不想在结束这篇文章时不提到 Colab Pro。每月支付 10 美元，您就可以获得更高的使用限制，并优先使用速度更快的处理器。如果你喜欢 Colab，但担心它的速度和使用限制，它是云计算的一个很好的替代品，价格也很实惠！

如果你喜欢这篇文章，请随时[关注我](https://jerdibattista.medium.com/)，阅读我写的更多内容，或者请将我[作为推荐人](https://jerdibattista.medium.com/membership)，这样我就可以继续制作我喜欢的内容。

[](/how-i-turned-my-older-laptop-into-a-machine-learning-superstar-with-an-egpu-66679aa27a7c) [## 我是如何用 eGPU 将我的旧笔记本电脑变成机器学习超级明星的

### 以新电脑的零头成本改造超极本的惊人简单之旅

towardsdatascience.com](/how-i-turned-my-older-laptop-into-a-machine-learning-superstar-with-an-egpu-66679aa27a7c) [](/why-is-thunderbolt-3-such-a-huge-deal-and-why-apple-loves-them-614542d32dc2) [## 为什么雷电 3 如此重要？(以及苹果为什么喜欢它们)

### 无论您是将它用于 eGPU 和机器学习、游戏，还是用于外部显示器…没有它的生活可能…

towardsdatascience.com](/why-is-thunderbolt-3-such-a-huge-deal-and-why-apple-loves-them-614542d32dc2) [](/the-most-efficient-way-to-merge-join-pandas-dataframes-7576e8b6c5c) [## 合并/连接熊猫数据帧的最有效方法

### 为什么几乎每个人都写得很低效

towardsdatascience.com](/the-most-efficient-way-to-merge-join-pandas-dataframes-7576e8b6c5c) 

编辑:2011 年 11 月 25 日——在[的这篇文章](https://jerdibattista.medium.com/why-is-thunderbolt-3-such-a-huge-deal-and-why-apple-loves-them-614542d32dc2)中，我使用一台连接速度更快的新笔记本电脑测试了我的 eGPU，看到了更好的 AI 成绩，使它与 Colab 相比更具竞争力。