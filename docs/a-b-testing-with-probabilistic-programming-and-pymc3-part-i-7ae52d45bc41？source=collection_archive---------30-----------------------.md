# 用概率编程和 PyMC3 进行 A/B 测试(上)

> 原文：<https://towardsdatascience.com/a-b-testing-with-probabilistic-programming-and-pymc3-part-i-7ae52d45bc41?source=collection_archive---------30----------------------->

## 离散变量的 A/B 测试

声明:这篇文章深受《黑客的概率编程和贝叶斯方法》一书的影响。我们的主要贡献可能是说明性的风格——我们希望这将对概率编程或 A/B 测试或者两者都有好处。我们也将思考我们在 [Kibo Commerce](https://kibocommerce.com/) 实习时处理这些话题的一些经验。

这是关于概率编程应用的系列文章中的第二篇，特别是 PyMC3。在本文中，我将描述如何使用 PyMC3 对离散变量进行 A/B 测试。由于篇幅已经很长，我将在后续文章中讨论使用连续变量的 A/B 测试。目前的文章结构如下。

1.什么是 A/B 测试，为什么它对商业成功很重要？

2.经典 A/B 测试的一些缺点，对更多信息的 A/B 测试的需求，以及 PyMC3 如何帮助。

3.用离散变量进行 A/B 测试的一个例子。

让我们深入细节。

**第一部分:什么是 A/B 测试，为什么它对商业成功很重要？**

首先，让我们通过一个具体的例子来简单解释一下什么是 A/B 测试。我们注意到，在实现和部署 A/B 测试时会有各种变化，所以尽管我们的例子相当典型，但它并不具有唯一的代表性。

假设，一个活动组织怀疑，如果主题行更长，会吸引更多人阅读电子邮件，然后捐款。当然，竞选组织者没有办法事先核实这一点。换句话说，它们只是形成了一个需要用数据来检验的假设；否则，如果他们的假设被证明是错误的，这场运动可能会失去捐助者。简单地说，活动组织者计划向 50，000 名潜在捐赠者发送活动电子邮件。一个简单过程如下。

1.将潜在的捐助者随机分成两组:测试组和维持组。

2.顾名思义，我们将使用测试集来测试我们的假设。这一部分可以这样做:我们将测试集随机分为两组:控制组和实验组。然后，我们将把旧的运动策略应用于控制组，把新的策略应用于实验组。我们将等到从这两组中收集到足够的数据来比较这两种策略。

3.然后，我们可以将第二步验证的更好的策略应用于维持集。

这个简单的过程很容易实现，事实上，它被证明是成功的:请看这里的一些真实世界的例子。

**第 2 部分:经典 A/B 测试的一些缺点，对更多信息的 A/B 测试的需求，以及 PyMC3 如何能有所帮助。**

本文中我们主要关注的是第二步。通常，为了比较这两种策略，我们经常使用 [t 检验](/the-art-of-a-b-testing-5a10c9bb70a4)。虽然这种测试相对容易实现，但也有一些缺点。其中一些是:

㈠它没有提供关于这两种方法之间差异的全部信息。特别是，它并没有真正为我们的最终决定提供一定程度的信心。

(二)它要求我们事先确定一个[样本量](https://www.medcalc.org/manual/sampling_comparison_of_two_means.php)。

(iii)传统的 t 检验使用 p 值来接受/拒绝零假设。从我和我在 Kibo Commerce 的导师的谈话中， [Austin Rochford，](https://austinrochford.com/)我了解到，营销人员经常将 p 值误解为实验组比对照组表现更好的概率。这种误解可能会导致糟糕的商业决策。

(iv)不容易使用 t 检验来回答更复杂的问题，例如差异的量化。例如，有时我们不仅对实验组是否比控制组表现更好感兴趣，而且对好多少感兴趣。这一点很重要，因为实施一项新战略通常需要花费时间和金钱等资源；因此，我们必须从战略上决定这样做是否值得。

这些原因和其他原因是发现更多信息测试的动机。我们在本文中采用的方法是贝叶斯 A/B 测试。粗略地说，在贝叶斯/概率编程框架中进行 A/B 测试的过程如下。

*第一步:*我们形成了对数据真实性的先验信念。

*第二步*:从证据/观察到的数据中，我们更新信念；换句话说，我们(数值地)计算后验分布。

*步骤 3* :我们从后验分布中抽取样本，并使用这些样本进行统计推断。

正如我们在[第一篇文章](/introduction-to-pymc3-a-python-package-for-probabilistic-programming-5299278b428)中解释的，计算后验分布的精确形式通常具有挑战性，尤其是在共轭先验不可用的情况下。幸运的是，我们有 PyMC3 来神奇地帮助我们。

**3。使用离散变量进行 A/B 测试的示例**

第一部分中讨论的 A/B 测试问题的一个重要指标是转换率:即潜在捐赠者向活动捐赠的概率。

我们将使用模拟数据，如第一部分中的图片所示。更准确地说，假设我们进行实验，结果由下式给出:

```
donors_A=1300
donors_B=1500
conversions_from_A=273
conversions_from_B=570
```

请注意，与图片不同，A 组和 B 组的样本量不同。这种情况在实践中并不少见:严格遵循上述 A/B 测试程序通常具有挑战性(例如，我观察到 Kibo 的数据经常出现这种情况。)然而，正如我们所看到的，样本大小几乎相等。此外，在贝叶斯框架中，不相等的样本大小不是一个大问题。

我们可以通过下面几行代码在数字上近似这两组转换率的后验分布。

```
#group A
with pm.Model() as model_A:
    #set the prior distribution of $p_A$.
    p_A=pm.Uniform('p_A', lower=0, upper=1)  

    #fit the observed data to our model 
    obs=pm.Binomial("obs", n=donors_A, 
                    p=p_A, observed=conversions_from_A) #MCMC algorithm 
    step=pm.Metropolis() #sampling from the posterior distriubtion. 
    trace_A=pm.sample(30000, step=step)
    burned_trace_A=trace_A[1000:]#group B
with pm.Model() as model_B:
    #set the prior distribution of $p_B$. p_B=pm.Uniform('p_B', lower=0, upper=1)  

    #fit the observed data to our model 
    obs=pm.Binomial("obs", n=donors_B, 
                    p=p_B, observed=conversions_from_B)

    #MCMC algorithm 
    step=pm.Metropolis() #sampling from the posterior distriubtion. 
    trace_B=pm.sample(30000, step=step)
    burned_trace_B=trace_B[1000:]
```

关于代码的一些话。由于我们观察到的数据是转换的数量，我们使用二项分布来描述我们的观察。如果我们有逐点数据(即每个潜在捐献者的转化率)，我们可以使用伯努利分布，就像我们在[的上一篇文章中描述的那样。](/introduction-to-pymc3-a-python-package-for-probabilistic-programming-5299278b428)当然，不管我们使用什么分布，后验分布都是相同的。从数学上来说，总转化率是[对我们问题的充分统计](https://en.wikipedia.org/wiki/Sufficient_statistic)。

我们还注意到这不是计算后验分布的唯一方法。在这种特殊情况下，我们可以显式地计算它们，详见我们的[上一篇文章](/introduction-to-pymc3-a-python-package-for-probabilistic-programming-5299278b428)。更准确地说，我们可以证明 p_A 的后验分布是β(274，1028)，p_B 的后验分布是β(571，931)。

我们绘制了 A 组样本的直方图及其精确后验分布β(274，1028)。我们看到 PyMC3 在近似 pA 的后验分布方面表现得相当好。

```
#histogram for samples from the posterior distribution of  $p_A$figsize(12.5, 4)
alpha_prior=beta_prior=1
a=alpha_prior+conversions_from_A
b=beta_prior+donors_A-conversions_from_A
coef=beta(a,b)
plt.title(r"Posterior distribution of $p_A$")
plt.hist(burned_trace_A["p_A"], bins=50, histtype='stepfilled', density=True, color='#348ABD')
x=np.arange(0,1.04,0.001)
y= beta.pdf(x, a, b) 
plt.plot(x, y, color='black', label='True posterior distribution of $p_A$')
plt.xlim(0.15, 0.3)
plt.legend()
plt.show()
```

![](img/5ba2077f2df7676f2df72c68b957547a.png)

我们可以通过取样本平均值来估计 A 组的转化率。

```
#the sample mean-an estimate for conversion rate of group A. 
samples_posterior_A=burned_trace_A['p_A']
samples_posterior_A.mean()0.21050251102125828
```

我们看到这个估计非常接近频率主义者的估计。

同样，我们可以对 p_B 做同样的事情。

```
#histogram for samples from the posterior distribution of  $p_A$figsize(12.5, 4)
alpha_prior=beta_prior=1
a=alpha_prior+conversions_from_B
b=beta_prior+donors_B-conversions_from_B
plt.title(r"Posterior distribution of $p_B$")
plt.hist(burned_trace_B["p_B"], bins=50, histtype='stepfilled', density=True, color='#348ABD')
x=np.arange(0,1.04,0.001)
y= beta.pdf(x, a, b) 
plt.plot(x, y, color='black', label='True posterior distribution of $p_A$')
plt.xlim(0.3, 0.45)
plt.show()
```

![](img/74c7b535d19f09da422a1c83e9c17f88.png)

现在，由于我们从 pA 和 pB 的后验分布中获得了足够的样本，我们可以开始进行统计推断。

我们可以一起绘制两个直方图。

```
figsize(12.5, 4)
plt.hist(samples_posterior_A, bins=40, label='posterior of p_A', density=True)
plt.hist(samples_posterior_B, bins=40, label='posterior of p_B', density=True)
plt.xlabel('Value')
plt.ylabel('Density')
plt.title("Posterior distributions of the conversion rates of group $A$ and group $B$")
plt.legend()
plt.show()
```

![](img/0a4e20a9494c02b2a4fb6d379dbe1365.png)

我们清楚地看到，B 组的表现优于 a 组。

但是好多少呢？我们可以用两个指标来回答这个问题:

(一)两种手段的绝对区别。

(二)两种手段的相对区别。

这些是我们之前讨论过的量化问题的例子。这两个问题都可以在贝叶斯框架下解决。首先，我们来回答第一个问题。为此，我们可以绘制 A 组和 b 组样本之间的差异。

```
difference=samples_posterior_B-samples_posterior_A
figsize(12.5, 4)
plt.hist(difference, bins=40, density=True)
plt.vlines(0.14, 0, 25, linestyle='--', color='red')
plt.title('Posterior distribution of the difference of the two means')
plt.show()
```

![](img/ffdc575db742950632dab58ec6e9970b.png)

我们可以清楚地看到，这个分布远远不是 0。多远？比如我们有信心说 B 组的转化率比来自 A 组的转化率高 14%吗？我们可以用下面几行代码来近似这个概率。

```
#probability of 14% increase by group B100*len(difference[difference>0.14])*1.0/len(difference)96.23620689655172
```

这非常接近 100；所以我们真的对上面的说法很有信心。B 组的转化率比 A 组的转化率高 16%呢？

```
#probability of 16% increase by group B
100*len(difference[difference>0.16])*1.0/len(difference)71.44913793103449
```

现在，我们对第二种说法不太有信心。

接下来，我们回答第二个问题。如上所述，我们可以绘制两组转化率之间的相对差异。

```
rel_difference=100*(samples_posterior_B-samples_posterior_A)/samples_posterior_A
figsize(12.5, 4)
plt.hist(rel_difference, bins=40, density=True)
plt.vlines(60, 0, 0.04, linestyle='--', color='red')
plt.title('Posterior distribution of the relative difference of the two means')
plt.xlabel("percentage")
plt.show()
```

![](img/6bffc52a3f835c57c42db25a22765515.png)

让我们计算一下 B 组提高转化率至少 60%的概率。

```
100*len(rel_difference[rel_difference>60])*1.0/len(rel_difference)97.6301724137931
```

嗯，我们有信心 B 组确实提高了 60%的转化率。80%呢？

```
100*len(rel_difference[rel_difference>80])*1.0/len(rel_difference)52.0051724137931
```

正如我们所看到的，这个概率与通过投掷公平硬币获得正面的概率没有太大区别，换句话说，它大多是随机的，我们不应该相信 80%的增长。

**结论**

如上所述，贝叶斯框架能够克服经典 t 检验的许多缺点。此外，PyMC3 使得在离散变量的情况下实现贝叶斯 A/B 测试变得非常简单。

**参考文献**

[1] Cameron Davidson-Pilon，[黑客的概率编程和贝叶斯方法](https://github.com/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers)

[2]安德鲁·盖尔曼、约翰·卡林、哈尔·斯特恩、大卫·邓森、阿基·维赫塔里和唐纳德·鲁宾，[贝叶斯数据分析。](http://www.stat.columbia.edu/~gelman/book/)