# 你应该知道的比例的五个置信区间

> 原文：<https://towardsdatascience.com/five-confidence-intervals-for-proportions-that-you-should-know-about-7ff5484c024f?source=collection_archive---------2----------------------->

置信区间是统计推断的重要指标。如今，置信区间正受到越来越多的关注(理应如此！)这一点过去常常被忽视，尤其是因为对 p 值的痴迷。在这里，我详细介绍了比例的置信区间和五种不同的统计方法来推导比例的置信区间，特别是如果你在医疗数据科学领域，你应该知道。我还使用现有的 base R 和其他具有完全可复制代码的函数，将这些区间的实现合并到 R 中。

***注:*** *这篇文章是写给那些对置信区间和样本总体推断统计概念至少有一点概念的人的。初学者可能会发现很难坚持完成这篇文章。那些对置信概念非常熟悉的人可以跳过开始部分，直接跳到以 Wald 区间开始的置信区间列表。我也推荐阅读这篇* [*关于置信区间估计*的综述文章](https://projecteuclid.org/euclid.ss/1009213286)

## ***比例和置信区间***

*在我详述[二项式分布](/demystifying-the-binomial-distribution-580475b2bb2a)的早期文章中，我谈到了二项式分布，即在固定数量的独立试验中成功数量的分布，是如何与比例内在相关的。从临床/流行病学研究的背景来看，在任何研究中几乎总是会遇到比例问题。发病率(特定时期人口中新病例的数量)、患病率(特定时期患病人口的比例)都是比例。通过估计疾病的真实发病率和流行率来估计疾病负担可能是最常进行的流行病学研究。在我之前关于[二项分布](/demystifying-the-binomial-distribution-580475b2bb2a)的文章中，我试图通过引用一项假设的新冠肺炎血清阳性率研究来说明二项分布如何与疾病的患病率内在相关。*

*为了研究任何事件在任何人群中的比例，从整个人群中获取数据是不切实际的。但是我们能做的是随机选取一个实际可行的更小的人口子集，并计算感兴趣的事件在样本中的比例。现在，我们怎么知道我们从样本中得到的这个比例可以和真实的比例，人口中的比例联系起来呢？这就是置信区间发挥作用的地方。下图说明了从样本数据估计真实比例的推理统计过程。*

*![](img/ee22881f76312fe4a3cf10f6bf55f626.png)*

*根据从样本数据中得到的点估计值来估计真实比例的过程*

*从样本数据得到的点估计值构造置信区间通常是通过假设点估计值遵循特定的概率分布来完成的。在我之前关于[二项式分布](/demystifying-the-binomial-distribution-580475b2bb2a)的文章中，我谈到了二项式分布如何类似于正态分布。这意味着我们对从样本概念中得到的比例点估计的概率分布有所了解。这反过来意味着我们可以对真实比例进行一些相当合理的估计。下图说明了二项式分布正态近似背后的直觉。*

*![](img/c9d61506a1d78becb8a247f968d3d4c0.png)*

*当成功概率(p)为 0.1 时，不同样本量(n)的二项式分布。您可以看到，随着样本量的增加，分布变得越来越正常。当 p 接近 0.5 时，即使样本量较小，也可以假定分布是正态分布。在这里，我只是想说明一个相当极端的情况，当 p 处于极值时(这里是 0.1)，因为实际上这些极值比流行病学研究中接近 0.5 的值更常见*

*好了，现在我们知道，由于二项式分布的正态近似现象，来自样本数据的比例的点估计可以被假定为遵循正态分布，我们可以使用点估计来构造置信区间。但是这个置信区间到底是什么呢？现在让我们假设一个 95%的置信区间意味着我们 95%确信真实的比例在这个区间内。请注意，这个定义在统计学上是不正确的，纯粹主义者会觉得难以接受。然而，出于实用的目的，我觉得这个定义是好的开始。*

*在平均值为 0、标准差为 1 的正态分布(也称为标准正态分布)中，95%的值将围绕平均值对称分布，如下图所示。因此，在这个例子中，范围从-1.96 到+1.96 的 X 轴值是 95%的置信区间。*

*![](img/3b25d9ce49c42f81f7fefaf2a9a7b575.png)*

*标准正态分布。X 轴代表数值，Y 轴代表概率。阴影区域表示构成平均值周围所有值的 95%的值。左侧和右侧的无阴影区域代表相当极端的值，它们不构成 95%的置信区间*

*上图中 z = 1.96 是一个神奇的数字。这是因为置信区间通常报告为 95%的水平。由于正态分布是对称的，这意味着我们必须排除上图中左侧 2.5%和右侧 2.5%的值。这反过来意味着我们需要找到切割这两个点的阈值，对于 95%的置信区间，该值结果为 1.96。我们可以说，95%的分布值位于这些值(左右)的标准偏差的 1.96 倍之内。在均值为 0，标准差为 1 的标准正态分布的情况下，这个区间恰好是(-1.96，+1.96)。*

*同样，如果我们假设‘p’是你的比例点估计值，而‘n’是样本量，那么‘p’的置信区间为:*

*![](img/688d98ebf9b54fdb000a5fb753c5b719.png)*

*这里，p 上面的帽子符号只是用来表示这是样本的点估计值，而不是真实的比例。对于 95%的置信区间，z 为 1.96。这个置信区间通常也称为 Wald 区间。*

*在 95%置信区间的情况下，如上所述，上述等式中的“z”值仅为 1.96。对于 99%的置信区间,“z”的值将是 2.58。这抓住了一个直觉，如果你想把你的信心从 95%增加到 99%，那么你的区间范围必须增加，这样你才能更有信心。所以直觉上，如果你的置信区间需要从 95%的水平变为 99%的水平，那么在后一种情况下‘z’的值必须更大。类似地，对于 90%的置信区间,“z”的值将小于 1.96，因此您将得到一个更窄的区间。90%的 z 恰好是 1.64。*

*既然已经详细介绍了置信区间的基础知识，让我们详细讨论一下用于构建比例置信区间的五种不同方法。*

# *1.沃尔德间隔*

*Wald 区间是比例最基本的置信区间。Wald 区间在很大程度上依赖于二项式分布的正态近似假设，并且没有应用任何修改或修正。这是可以从这个正态近似中构造的最直接的置信区间。*

*![](img/688d98ebf9b54fdb000a5fb753c5b719.png)*

*沃尔德间隔*

*然而，它在实际场景中的表现非常差。这种“低性能”的含义是，在许多情况下，95% Wald 区间的覆盖率小于 95%！这可不好。当我们构建置信区间时，我们必须有一个合理的“覆盖范围”。例如，我们预计 95%的置信区间将“覆盖”95%的真实比例，或者至少接近 95%的真实比例。但是如果少了很多，那我们就有麻烦了。Wald interval 因实际场景中的低覆盖率而臭名昭著。这是因为在许多实际情况下,‘p’的值在极端侧(接近 0 或 1)和/或样本大小(n)不是那么大。*

*我们可以使用 R 对不同的 p 值探索 Wald 区间的覆盖范围。必须注意的是，基础 R 包似乎没有为比例返回 Wald 区间。这可能是因为 Wald 间隔通常被认为不是一个好的间隔，因为它在覆盖范围方面表现很差。所以，我定义了一个简单的函数 R，它以 x 和 n 为参数。x 是 n 次伯努利试验的成功次数。因此，样本比例就是 x 与 n 之比。根据上述公式，使用 Wald 方法返回置信区间的上下界非常简单。*

```
*waldInterval <- function(x, n, conf.level = 0.95){
 p <- x/n
 sd <- sqrt(p*((1-p)/n))
 z <- qnorm(c( (1 — conf.level)/2, 1 — (1-conf.level)/2)) #returns the value of thresholds at which conf.level has to be cut at. for 95% CI, this is -1.96 and +1.96
 ci <- p + z*sd
 return(ci)
 }#example
waldInterval(x = 20, n =40) #this will return 0.345 and 0.655*
```

*好了，现在我们有了一个函数，它将返回 95% Wald 区间的上下界。下一步是模拟随机抽样，估计每个随机样本的置信区间，看看这些样本构建的置信区间是否真正“覆盖”(包括)了真实比例。为此，我们将预先定义一组不同的真实人口比例。最后，对于这些预定义的概率中的每一个，我们看到覆盖率是多少%。理想情况下，对于 95%的置信区间，这个覆盖率应该总是大约在 95%左右。让我们看看沃尔德区间是否成立。所有这些步骤都在下面显示的 R 代码中实现。*

```
*numSamples <- 10000 #number of samples to be drawn from population
numTrials <- 100 #this is the sample size (size of each sample)
probs <- seq(0.001, 0.999, 0.01) #true proportions in prevalence. #for each value in this array, we will construct 95% confidence #intervals
coverage <- as.numeric() #initializing an empty vector to store coverage for each of the probs defined above
for (i in 1:length(probs)) {
 x <- rbinom(n = numSamples, size=numTrials, prob = probs[i]) #taken #n random samples and get the number of successes in each of the n #samples. thus x here will have a length equal to n
 isCovered <- as.numeric() #a boolean vector to denote if the true #population proportion (probs[i]) is covered within the constructed ci
 #since we have n different x here, we will have n different ci for #each of them. 
 for (j in 1:numSamples) {
 ci <- waldInterval(x = x[j], n = numTrials)
 isCovered[j] <- (ci[1] < probs[i]) & (probs[i] < ci[2]) #if the #true proportion (probs[i]) is covered within the constructed CI, #then it returns 1, else 0
 }
 coverage[i] <- mean(isCovered)*100 #captures the coverage for each #of the true proportions. ideally, for a 95% ci, this should be more #or else 95%
}plot(probs, coverage, type=”l”, ylim = c(75,100), col=”blue”, lwd=2, frame.plot = FALSE, yaxt=’n’, main = “Coverage of Wald Interval”,
 xlab = “True Proportion (Population Proportion) “, ylab = “Coverage (%) for 95% CI”)
abline(h = 95, lty=3, col=”maroon”, lwd=2)
axis(side = 2, at=seq(75,100, 5))*
```

*下面是 Wald 区间的覆盖图*

*![](img/843fa24b7eb70fd387e4fa7934fc1d9a.png)*

*不同人口比例的 Wald 区间覆盖率*

*上面的图证明了 Wald 区间表现很差的事实。事实上，只有大约 0.5 的比例才能达到 95%的覆盖率。对于 p 的极值，覆盖率非常低。*

## *2.clopper-Pearson 区间(精确区间)*

*Clopper-Pearson 区间(也称为精确区间)的目标是使所有 p 和 n 值的覆盖率至少达到 95%。正如“精确”区间的别名所表明的，该区间基于精确的二项式分布，而不是像 Wald 区间那样基于大样本中 p 正态近似。对于那些对数学和原始文章感兴趣的人，请参考 Clopper 和 Pearson 在 1934 年发表的原始文章。这有时被认为是太保守了(在大多数情况下，这个覆盖率可以达到 99%！).在 R 中，流行的“binom.test”返回 Clopper-Pearson 置信区间。这也被称为精确二项式检验。类似于我们对 Wald 区间所做的，我们也可以探索 Clopper-Pearson 区间的覆盖。下面给出了完全可再现的 R 代码。*

```
*numSamples <- 10000 #number of samples to be drawn from population
numTrials <- 100 #this is the sample size (size of each sample)
probs <- seq(0.001, 0.999, 0.01) #true proportions in prevalence. #for each value in this array, we will construct 95% confidence #intervals
coverage <- as.numeric() #initializing an empty vector to store coverage for each of the probs defined above
for (i in 1:length(probs)) {
 x <- rbinom(n = numSamples, size=numTrials, prob = probs[i]) #taken #n random samples and get the number of successes in each of the n #samples. thus x here will have a length equal to n
 isCovered <- as.numeric() #a boolean vector to denote if the true #population proportion (probs[i]) is covered within the constructed ci
 #since we have n different x here, we will have n different ci for #each of them. 
 for (j in 1:numSamples) {
 ci <- binom.test(x = x[j], n = numTrials)$conf
 isCovered[j] <- (ci[1] < probs[i]) & (probs[i] < ci[2]) #if the #true proportion (probs[i]) is covered within the constructed CI, #then it returns 1, else 0
 }
 coverage[i] <- mean(isCovered)*100 #captures the coverage for each #of the true proportions. ideally, for a 95% ci, this should be more #or else 95%
}plot(probs, coverage, type=”l”, ylim = c(75,100), col=”blue”, lwd=2, frame.plot = FALSE, yaxt=’n’, main = “Coverage of Wald Interval”,
 xlab = “True Proportion (Population Proportion) “, ylab = “Coverage (%) for 95% CI”)
abline(h = 95, lty=3, col=”maroon”, lwd=2)
axis(side = 2, at=seq(75,100, 5))*
```

*这是克洛普-皮尔逊区间的覆盖图*

*![](img/6c506e824bf77db314e52ba6d1a277f4.png)*

*针对不同人口比例的 Clopper Pearson(精确)区间覆盖率*

*哇，这看起来像是沃尔德区间报道的完全相反！事实上，在许多场景中，覆盖率甚至达到了几乎 100%,并且覆盖率从未低于 95%。这看起来很有希望，这是正确的。但这也太保守了，因为置信区间可能会更宽。这是克洛普-皮尔逊间隔的一个缺点。*

## *3.威尔逊区间(得分区间)*

*Wilson 评分区间是正常近似值的扩展，以适应 Wald 区间典型的覆盖范围损失。因此，通过对标准近似公式进行一些变换，可以认为它是对 Wald 区间的直接改进。对数学感兴趣的人可以参考威尔逊的原文。*

*在 R 中，测试比例的常用“prop.test”函数默认返回 Wilson 分数区间。需要注意的是，可以用两种不同的方式来校正 Wilson 分数区间。一个没有连续性校正，一个有连续性校正。后者被称为 Yate 的连续性校正，并且“prop.test”中的参数“correct”可以被指定为 TRUE 或 FALSE，以分别应用或不应用该校正。如果样本量很小或者 p 值处于极端值(接近 0 或 1)，建议使用 Yate 的连续性校正。Yate 的连续性修正被认为有点保守，虽然没有 Clopper-Pearson 区间保守。*

*下面的 R 代码是完全可再现的代码，用于在有和没有 Yate 的连续性校正的情况下生成 Wilson 分数区间的覆盖图。*

```
*#let's first define a custom function that will make our jobs easiergetCoverages <- function(numSamples = 10000,numTrials = 100, method, correct = FALSE){
 probs <- seq(0.001, 0.999, 0.01) 
 coverage <- as.numeric() 
 for (i in 1:length(probs)) {
 x <- rbinom(n = numSamples, size=numTrials, prob = probs[i]) 
 isCovered <- as.numeric() 
 for (j in 1:numSamples) {
 if (method ==”wilson”){
 if (correct){
 ci <- prop.test(x = x[j], n = numTrials, correct = TRUE)$conf
 }else {
 ci <- prop.test(x = x[j], n = numTrials, correct = FALSE)$conf
 }
 }else if (method==”clopperpearson”){
 ci <- binom.test(x = x[j], n = numTrials)$conf
 }else if(method==”wald”){
 ci <- waldInterval(x = x[j], n = numTrials)
 }else if(method ==”agresticoull”){
 ci <- waldInterval(x = x[j]+2, n = numTrials + 4)
 }
 isCovered[j] <- (ci[1] < probs[i]) & (probs[i] < ci[2])
 }
 coverage[i] <- mean(isCovered)*100 #captures the coverage for each #of the true proportions. ideally, for a 95% ci, this should be more #or else 95%
 }
 return(list(“coverage”= coverage, “probs” = probs))
}*
```

*下面的代码使用上面定义的函数来生成 Wilson 分数覆盖率和相应的两个图，如下所示*

```
*out <- getCoverages(method=”wilson”)out2 <- getCoverages(method=”wilson”, correct = TRUE)plot(out$probs, out$coverage, type=”l”, ylim = c(80,100), col=”blue”, lwd=2, frame.plot = FALSE, yaxt=’n’, 
 main = “Coverage of Wilson Score Interval without continuity correction”,
 xlab = “True Proportion (Population Proportion) “, ylab = “Coverage (%) for 95% CI”)
abline(h = 95, lty=3, col=”maroon”, lwd=2)
axis(side = 2, at=seq(80,100, 5))plot(out2$probs, out2$coverage, type=”l”, ylim = c(80,100), col=”blue”, lwd=2, frame.plot = FALSE, yaxt=’n’, 
 main = “Coverage of Wilson Score interval with continuity correction”,
 xlab = “True Proportion (Population Proportion) “, ylab = “Coverage (%) for 95% CI”)
abline(h = 95, lty=3, col=”maroon”, lwd=2)
axis(side = 2, at=seq(80,100, 5))*
```

*![](img/9faa35757df0e515e16d2cc361d84dda.png)*

*具有和不具有连续性校正的 Wilson 分数区间覆盖。Yate 连续性校正的覆盖率(右图)与 Clopper-Pearson 相似，具有非常好的覆盖率，但在极端情况下可能有点过于保守*

## *4.阿格莱斯蒂-库尔区间*

*Agresti & Coull 一个简单的 solution⁴来提高 Wald 区间的覆盖率。这种简单的解决方案也被认为比 Clopper-Pearson(精确)区间执行得更好，因为该 Agresti-Coull 区间不太保守，同时具有良好的覆盖。解决方案可能看起来非常简单，因为这只是在原始观察结果上增加了两个成功和两个失败！是的，没错。这里，对于 95%的置信区间，x(成功次数)变成 x+2，n(样本大小)变成 n+4。仅此而已。但是这个非常简单的解决方案在实际场景中似乎非常有效。这就是它的美妙之处。通过添加这些伪观测值，p 的分布被拉向 0.5，因此当 p 处于极值时，p 的分布的偏斜被拉向 0.5。因此，在某种程度上，你可以说这也是某种连续性的修正。另一个令人惊讶的事实是，原始论文发表于 1998 年，而不是第二次世界大战前克洛普-皮尔逊和威尔逊的论文。因此，这是一种相对较新的方法。*

*Agresti-Coull 区间的覆盖范围如下图所示。这*

*![](img/66ab3ffef126f4c034175cdfd6812114.png)*

*Agresti-Coull 区间的覆盖率*

*Agresti-Coull 区间是一种非常简单的解决方案，可以缓解 Wald 区间的较差性能，但如上所示，这种非常简单的解决方案极大地提高了覆盖范围。下面给出了为 Agresti-Coull 区间生成此覆盖图的 R 代码。请注意，它使用了之前定义的自定义函数“getCoverages”。*

```
*ac <- getCoverages(method =”agresticoull”)plot(ac$probs, ac$coverage, type=”l”, ylim = c(80,100), col=”blue”, lwd=2, frame.plot = FALSE, yaxt=’n’, 
 main = “Coverage of Agresti-Coull Interval without continuity correction”,
 xlab = “True Proportion (Population Proportion) “, ylab = “Coverage (%) for 95% CI”)
abline(h = 95, lty=3, col=”maroon”, lwd=2)
axis(side = 2, at=seq(80,100, 5))*
```

## *5.贝叶斯 HPD 区间*

*贝叶斯 HPD 区间是这个列表中的最后一个，它源于一个完全不同的概念，被称为贝叶斯统计推断。*

*我们上面讨论的所有四个置信区间都是基于频率统计的概念。频数统计是一个统计领域，通过关注数据的频率，基于样本数据进行人口统计的推断或人口统计的估计。这里的假设是，一个假设是正确的，数据的概率分布假设遵循一些已知的分布，我们从该分布中收集样本。*

*贝叶斯统计推断是一个完全不同的统计推断学派。这里，参数的推断需要假设数据和观察(采样)数据的先验分布，在给定数据的情况下，使用似然性来创建参数的分布。后验分布是我们真正感兴趣的，也是我们想要估计的。我们从数据中知道可能性，并且通过假设分布我们知道先验分布。使用可能性，我们可以从先验到后验更新我们的结论——也就是说，数据提供了一些信息，使我们能够更新我们现有的(假设的)先验知识。*

*贝叶斯统计推断在 20 世纪以前非常流行，然后频率主义统计学统治了统计推断世界。贝叶斯推理不受欢迎的原因之一是，很明显，要产生稳健的贝叶斯推理，需要大量的计算能力。然而，在过去的一二十年里，世界已经见证了计算能力的巨大提高，因此贝叶斯统计推断再次获得了广泛的流行。这就是为什么流行的“**贝叶斯与频率主义者**”的辩论出现在统计文献和社交媒体中。*

> *p 值，置信区间——这些都是频率统计。所以贝叶斯 HPD(最高后验密度)区间实际上根本不是置信区间！**这个区间被称为可信区间。***

*如上所述，我们可以将贝叶斯推理总结为*

> *后验~似然*先验*

*对于比例， [beta 分布](https://en.wikipedia.org/wiki/Beta_distribution)通常被认为是先验分布的选择。β分布取决于两个参数α和β。**当α=β= 0.5 时，这就是所谓的杰弗里先验。据说 Jeffrey 的先验有一些理论上的好处，这是估计比例可信区间最常用的先验分布。***

*![](img/7a76747229c76e8beb26c7381168e6fe.png)*

*杰弗里的先验*

*R 中的“binom”包有一个“binom.bayes”函数，用于估计比例的贝叶斯可信区间。最佳可信区间与后验线相交，这些区间被称为最高后验密度(HPD)区间。*

*让我们看看贝叶斯 HPD 可信区间的覆盖范围*

*![](img/b6cbfb5b961f92dd254607536c5a710a.png)*

*贝耶的 HPD 可信区间覆盖与杰弗里的先验*

*Baye 的 HPD 可信区间的覆盖范围似乎比 Wald 的好，但并不比其他三个频率主义者的置信区间好。不过，使用可信区间的一个优势在于区间的解释。*

*95%置信区间的频率主义者定义:*

> *如果我们从总体中抽取几个独立的随机样本，并从每个样本数据中构建置信区间，那么 100 个置信区间中有 95 个将包含真实平均值(真实比例，在比例的上下文中)*

*哎呀，与我们最初对置信区间的想法相比，上面的定义似乎非常复杂，甚至令人困惑。就像我之前说的，我们仍然可以有 95%的把握认为真实比例在置信区间内。*

*但是说到贝叶斯可信区间，实际的统计定义本身就很直观。*

***95%可信区间的贝叶斯定义:***

> *真实比例位于 95%可信区间内的概率为 0.95*

*哇，上面的定义似乎比频率主义者的定义更“可爱”。因此，这是贝叶斯统计推断的一个明显优势，因为从实用的角度来看，这些定义更加直观，而频率参数(如 p 值、置信区间)的*实际*定义对于人类思维来说是复杂的。*

## ***把所有这些放在一起***

*让我们总结一下我们列出的所有五种不同类型的置信区间。下图将所有的覆盖范围放在一起。*

*![](img/223d1600ae6647eae3d313d1ca1e404b.png)*

## *简而言之…*

*   *Clopper-Pearson 区间是迄今为止覆盖最广的置信区间，但它过于保守，尤其是在 p 的极值处*
*   *Wald 间隔表现非常差，并且在极端情况下，它无论如何都不能提供可接受的覆盖*
*   *贝叶斯 HPD 可信区间在大多数情况下都具有可接受的覆盖范围，但在 Jeffrey 的先验下，它不能在 p 的极值处提供良好的覆盖范围。然而，这可能取决于所使用的先验分布，并且可能随着不同的先验而改变。与其他置信区间不同，可信区间的一个优点是直观的统计定义*
*   *Agresti-Coull 通过对 Wald 公式进行非常简单的修改，提供了良好的覆盖范围。*
*   *Wilson 的有和没有校正的得分区间也具有非常好的覆盖范围，尽管应用了校正，它往往有点太保守。*

*下表总结了五个不同置信区间的一些要点*

*![](img/e1baf07cf730aff72be721c65857ca7e.png)*

> ***Brown、Cai 和 Dasgupta 建议，当样本量小于 40 时，使用带连续性校正的 Wilson 评分，对于更大的样本，建议使用 Agresti-Coull 区间。***

***参考文献***

1.  *劳伦斯·布朗；蔡，托尼；达斯古普塔，阿尼班。二项比例的区间估计。统计学家。Sci。第 16 期(2001 年)，第 2 期，第 101-133 页。doi:10.1214/ss/1009213286。[https://projecteuclid.org/euclid.ss/1009213286](https://projecteuclid.org/euclid.ss/1009213286)*
2.  *Clopper，C.J .和 Pearson，E.S.(1934)，“二项式情况下说明的置信度或置信限的使用”，《生物计量学》26，404–413。*
3.  *E.B .威尔逊(1927)。或然推断、继承法和统计推断。美国统计协会杂志，22，209–212。doi: 10.2307/2276774。*
4.  *对于二项式比例的区间估计，Agresti A .，Coull B.A .近似比“精确”更好。我是。统计。1998;52:119–126.doi: 10.2307/2685469*