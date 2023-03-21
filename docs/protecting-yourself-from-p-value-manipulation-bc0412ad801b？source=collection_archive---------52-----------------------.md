# 保护自己免受 P 值操纵

> 原文：<https://towardsdatascience.com/protecting-yourself-from-p-value-manipulation-bc0412ad801b?source=collection_archive---------52----------------------->

## 为什么调查研究失去了可信度，你如何通过问这些简单的问题来建立你的可信度

![](img/60f362c29bd97e4841c3fd02af6f1e2d.png)

由 [Unsplash](https://unsplash.com/s/photos/lying-businessman?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的 [Taras Chernus](https://unsplash.com/@chernus_tr?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

> 如果你认为你看到了有趣的事情，你需要证据来证明这不是一次性事件。

我有一手幸运牌，最近，我赢了很多。我赌的每一个数字都是轮盘赌上的中奖号码。我发现了什么有趣的事情吗？假设我和以前一样是个失败者，那么我最近的所有胜利有多少可能是随机幸运的呢？

在统计学中， [p 值](https://en.wikipedia.org/wiki/P-value)是最后一个问题的答案。好了，现在疼了！我如何证明我的获胜直觉不仅仅是纯粹的运气？当然，我开始尝试和赌博。我赌得越多，赢得越多，我就建立了对我有利的证据。“p 值”越低，证据越有力。

我真的可以操纵 p 值让自己在朋友面前好看，而不伪造结果吗？如果你发现我所有的赢钱赌注都放在同一个赌场，你会怎么想？你有我在其他赌场下注的信息吗？我在那里赢了同样多的钱吗？

不幸的是，p 值很容易被操纵。此外，这在现实生活中经常发生，包括科学研究和商业研究。例如， [p-hacking](https://bitesizebio.com/31497/guilty-p-hacking/#:~:text=The%20term%20p%2Dhacking%2C%20coined,decisions%20made%20by%20the%20investigator.) 指控导致[研究报告](https://www.theatlantic.com/science/archive/2015/08/psychology-studies-reliability-reproducability-nosek/402466/)发表后遭到进一步质疑，因为它们的结果不可复制。随着越来越多如此高调的例子发生，p-hacking 甚至找到了进入流行文化的途径。

> “科学是扯淡吗？不，但是现在有很多伪装成科学的胡说八道”——约翰·奥利弗

无论你以哪种方式操纵 p 值，都会导致糟糕的决策和昂贵的结果，更不要忘了研究者可信度的损失。如果我们正在测试一个假设，高于某个[统计显著性](https://en.wikipedia.org/wiki/Statistical_significance)水平的 p 值表明没有足够的证据来拒绝对假设的反驳。这并不意味着我们自己的假设是错误的。然而，这确实在解释 p 值时创造了一个巨大的灰色区域，这在研究和商业中被广泛操纵。

# 我们如何躲避 p 值子弹？

我们需要明白，作为数据科学家和研究人员，我们的工作是提出问题和提出建议，而不是冒险证明给定的假设是正确的。在你进一步阅读之前，我强烈推荐你浏览一下关于测试和实验的[基础知识](https://medium.com/swlh/the-modern-approach-to-uncertainty-2836945bba52)。

> 没有足够的证据来验证一个假设并不意味着这个假设是错误的。然而，这一事实被扭曲了很多，以发布来自学术界和商界的误导性研究结果。

在从我们自己的研究或其他工作中得出结论之前，我们应该经常问自己这些问题。

## 我们接受一个结果仅仅是因为它符合我们自己的偏见吗？

p-hacking 发生的一个常见原因，不管是有意还是无意，是利用了[研究人员的自由度](https://en.wikipedia.org/wiki/Researcher_degrees_of_freedom#:~:text=Researcher%20degrees%20of%20freedom%20is,and%20in%20analyzing%20its%20results.&text=Furthermore%2C%20studies%20with%20smaller%20sample,of%20researcher%20degrees%20of%20freedom.)。这个术语指的是研究人员在设计统计实验时使用的灵活性，以便他们想要发表的结果具有统计学意义。通常，这些设计决策可归结为一些因素，如实验中包含的变量、决定何时停止收集数据、数据采样方法等。

> 作为数据科学家，我们的工作是客观地验证我们的假设，而不是为它们辩护。

假设我们拥有一家连锁超市，注意到在我们的一家店中，[啤酒和尿布](https://tdwi.org/articles/2016/11/15/beer-and-diapers-impossible-correlation.aspx)总是一起购买。这是否意味着啤酒和尿布销售之间的相关性是真实的？相关性在统计上有意义吗？如果我们操纵实验来强迫一个有统计学意义的结果，不管是有意的还是无意的。举个例子，

*   我们只选择了那些包含购买婴儿用品的交易。
*   我们只在一个社区的少数几家商店里进行了研究，这些商店碰巧有很多刚生完孩子的年轻夫妇。

相反，作为优秀的数据科学家，我们实际上希望采取进一步的措施来确保我们正在进行一个可靠的实验。除了啤酒和尿布的销售，还有更多变数在起作用。举个例子，

*   我们应该在其他市场进行实验，并验证结果是否可重复。
*   为实验选择数据时，要随机抽样。这是黄金标准。

根据我们认为可靠的虚假结果得出结论可能会被证明是极其昂贵的。我们当然不希望这些美元白白浪费掉。

## 我们愿意在什么样的显著性阈值上接受结果？

换句话说，我们愿意去维加斯的几率有多大？我们的假设是啤酒和尿布相伴而行，相反的观点是它们并不相伴。这个反驳论点就是[零假设](https://www.investopedia.com/terms/n/null_hypothesis.asp)。拒绝零假设需要什么？p-value 意在成为诚实直率、不带偏见的导师。我们如何解读它的建议来做出理性的决定？我们制定一个计划。我们在沙地上画了一条线，如果 p 值越过这条线，我们就不能拒绝零假设。沙子上的这条线被称为[显著性水平](https://blog.minitab.com/blog/adventures-in-statistics-2/understanding-hypothesis-tests-significance-levels-alpha-and-p-values-in-statistics#:~:text=The%20significance%20level%2C%20also%20denoted,there%20is%20no%20actual%20difference.)，或 alpha。

> 保守的显著性阈值不一定使测试可靠。它只表明我们在做决定前愿意接受的风险水平。

那么，更低、更保守的显著性阈值是否意味着我们的测试有可靠的结果？不，这只是我们愿意承担风险的一个指标。它不是我们研究质量的指标。根据领域的不同，显著性级别可以更保守或更宽松。

> 拒绝[无效假设](https://www.investopedia.com/terms/n/null_hypothesis.asp)的阈值可以从一个域到另一个域变化，并且可以是保守的或宽松的，这取决于情况的优点。这不会改变研究的质量，也不会改变观察到的 p 值本身的可靠性。

例如，与新药的临床试验相比，市场研究的显著性水平可以更宽松。然而，收紧或放松这个阈值不会改变研究的价值和 p 值统计本身的可靠性。我们的判断应该基于研究的质量，改变显著性水平以使结果看起来更好不会改变它实际上有多好。

> 有些东西看起来很好，并不意味着它就是好的，或者一定是正确的。

当然，在建立一个实验时，需要考虑很多事情。我们需要做好一些基本的事情，比如弄清楚我们想要测量什么指标，测试的规模是多少，或者我们需要进行多长时间的实验。米尔恰·达维德斯库博士在这篇[的精彩文章](/in-depth-on-testing-who-1c583efd6163)中详细解释了其中的一些。然而，一旦我们弄清楚了基础，作为数据科学家，我们的工作就是客观地对待结果，而不是操纵它们。

毕竟，我们不欠任何人任何实验的结果。我们观察我们所看到的，并推荐前进的方法。