# 可行干预的因果效应估计导论

> 原文：<https://towardsdatascience.com/an-introduction-to-estimating-the-causal-effects-of-feasible-interventions-e07ed45136ff?source=collection_archive---------63----------------------->

## 一种灵活的因果推理方法

![](img/5b1d0625f643704743fc3cfb4e7af12b.png)

照片由[德鲁·比姆](https://unsplash.com/@drew_beamer) r 在 [Unsplash](https://unsplash.com/) 上拍摄

想象一下你正在申请法学院。你参加了法学院入学考试，但没有得到你想要的高分。你开始想知道，提高你的 LSAT 分数对你的录取结果(定义为你申请的学校中你被录取的比例)会有什么影响？

乍一看，这似乎很容易。你找到前几年申请人的一些数据，回归 LSAT 分数的录取结果。太好了，你在 LSAT 分数和你期望被录取的学校比例之间有了一个衡量标准。

但是…这真的回答了你的问题吗？你的估计是无偏的吗？你回想一下线性回归的假设，意识到你能被录取的学校比例有界在 0 和 1 之间，所以不可能是线性的。你还记得一个回归系数的固定解释，“X 每增加一个单位，Y 增加或减少β”；如果有人刷爆了他们的 LSAT 分数，这种解释如何成立？

考虑到这一点，你开始缩小你的研究问题:如果 LSAT 分数提高了一个现实的数量，那么提高 LSAT 分数对法学院入学结果会有什么影响？我们可以认为这是一种可行的干预措施，并正式将其定义为一种修改后的治疗政策。

> 修改后的处理政策(MTP)被定义为可以依赖于暴露的自然价值的干预。这不同于其他因果效应，如平均治疗效应(ATE)，在平均治疗效应中，暴露量将确定性地增加(或减少)，这通常是不可能的。

对于我们的问题，考虑 LSAT 分数所在的 MTP:

*   如果个人的*观察到的* LSAT 分数在 120-150 之间，则增加 10 分
*   如果在 151-160 之间，则增加 5 分
*   如果在 161-165 之间，则增加 2 分
*   如果他们的分数高于 165，则保持不变。

这种干预考虑到观察到的 LSAT 分数越高，就越难提高。我们认为这种干预是可行的，因为我们可以想象一种假设的干预可以实现它(例如，参加 LSAT 预备课程)。相比之下，确定性干预，也是不可能的干预，将是每个人的 LSAT 分数增加相同数量的干预。

定义了感兴趣的 MTP 后，您会很快意识到标准参数方法无法处理这个问题。进入 R 包`[lmtp](https://github.com/nt-williams/lmtp)`。`[lmtp](https://github.com/nt-williams/lmtp)`为基于点治疗和纵向修正治疗策略的可行干预措施的非参数偶然效应提供了一个估计框架。它支持两种主要的估计量，一种交叉验证的基于目标最小损失的估计量(CV-TMLE)和一种序列双稳健估计量(SDR)。这两种估计量都具有双重稳健性。这一点很重要，有几个原因，但也许最引人注目的是，它允许我们使用机器学习进行评估。因此，`[lmtp](https://github.com/nt-williams/lmtp)`使用`[sl3](https://github.com/tlverse/sl3)`包进行整体机器学习(也称为超级学习或堆叠泛化)来进行估计。

[1]超级学习对多个单独的机器学习算法的结果进行加权，以创建模型的最佳组合。

# 应用

让我们用`[lmtp](https://github.com/nt-williams/lmtp)`来回答我们的问题。首先，我们安装并加载必要的包，并模拟我们的数据。为了简单起见，我们假设录取只受 GPA 和 LSAT 分数的影响，GPA 混淆了 LSAT 和录取之间的关系。我们还将假设法学院不设置招生上限。

```
# install and load lmtp, sl3, future, and progressr
remotes::install_github("nt-williams/lmtp")
remotes::install_github("tlverse/sl3")
install.packages(c("future", "progressr"))library(lmtp)
library(sl3)
library(future)
library(progressr)# set seed and sample size
set.seed(6232)
n <- 1000# assume you must have a minimum gpa of 3.0 to 
# graduate from college and the maximum gpa is a 4.0
gpa <- round(rnorm(n, 3.5, .25), 2)
gpa <- ifelse(gpa < 3, 3, gpa)
gpa <- ifelse(gpa > 4, 4, gpa)# LSAT scores are bounded between 120 and 180 so we truncate
lsat <- round(rnorm(n, 149, 10) + gpa)
lsat <- ifelse(lsat < 120, 120, lsat)
lsat <- ifelse(lsat > 180, 180, lsat)# admissions results
admit <- (lsat / 300) + 0.05*gpa + 0.01*lsat*gpa
admit <- (admit - min(admit)) / (max(admit) - min(admit))# combine the data
admissions <- data.frame(gpa, lsat, admit)
```

我提到过`[lmtp](https://github.com/nt-williams/lmtp)`可以使用一个集合学习器进行估计，所以让我们来设置一下。我们将使用仅拦截模型、GLM 和随机森林的集合。

```
lrnrs <- make_learner_stack(Lrnr_mean, 
                            Lrnr_glm, 
                            Lrnr_ranger)
```

我们还需要一种方式将我们感兴趣的干预传达给`[lmtp](https://github.com/nt-williams/lmtp)`。我们可以使用移位函数来实现这一点。移位函数只是将一个向量作为输入，并返回一个长度和类型相同但根据我们的干预进行了修改的向量。

```
mtp <- function(x) {
  (x <= 150)*(x + 10) + 
    (x >= 151 & x <= 160)*(x + 5) + 
    (x >= 161 & x <= 165)*(x + 2) + 
    (x > 165)*x
}head(lsat)
#> [1] 145 156 161 139 148 148head(mtp(lsat)) # testing our shift function
#> [1] 155 161 163 149 158 158
```

剩下的就是估计干预的效果了。我们将使用 CV-TML 估计量`lmtp_tmle()`。

```
plan(multiprocess) # using parallel processingwith_progress({
  psi <- lmtp_tmle(admissions, 
                   trt = "lsat", 
                   outcome = "admit", 
                   baseline = "gpa", 
                   shift = mtp, 
                   outcome_type = "continuous", 
                   learners_outcome = lrnrs,
                   learners_trt = lrnrs)
})psi
#> LMTP Estimator: TMLE
#>    Trt. Policy: (mtp)
#> 
#> Population intervention effect
#>       Estimate: 0.4945
#>     Std. error: 0.0044
#>         95% CI: (0.4858, 0.5032)
```

根据我们的 LSAT 处理政策，我们估计法学院在人口中的平均比例为 0.495，95%的置信区间为 0.49 到 0.5。我们可以正式地将这与观察到的法学院在 LSAT 自然分数 0.42 以下被录取的平均比例进行比较。

```
ref <- mean(admit)
lmtp_contrast(psi, ref = ref)
#> Non-estimated reference value, defaulting type = 'additive'.
#> 
#> LMTP Contrast: additive
#> Null hypothesis: theta == 0
#> 
#>    theta shift   ref std.error conf.low conf.high p.value
#> 1 0.0701 0.494 0.424   0.00443   0.0614    0.0788  <0.001
```

我们估计，我们的治疗政策将使法学院在人口中的平均比例增加 0.07，95%的置信区间为 0.06 至 0.08。看来提高分数对你最有利。

# 最后的想法

前面的例子应该已经帮助你对什么是改良的治疗策略以及它们为什么有用建立了一些直觉。此外，这是一个玩具的例子，不应该被视为任何更多。

也就是说，这里有几个 MTP 实际应用的例子:

*   老年人体力活动对心血管疾病发病率的影响
*   手术时间对术后结果的影响
*   水质、卫生、洗手和营养干预对疾病转归的影响

修改后的治疗策略并不局限于持续暴露，也可用于二元和分类治疗。此外，`[lmtp](https://github.com/nt-williams/lmtp)`适用于处理点治疗和纵向设置的二元、连续和存活结果(结果中均有遗漏),并可估计确定性治疗效果，如 ate。关于使用`[lmtp](https://github.com/nt-williams/lmtp),`的深入介绍，我推荐你通读软件包[简介](https://htmlpreview.github.io/?https://gist.githubusercontent.com/nt-williams/ddd44c48390b8d976fad71750e48d8bf/raw/c56a7b0bbdf24ec18d08f839e73fa06a42ca9265/intro-lmtp.html)。

如果您想了解更多关于因果推断、修改后的治疗策略以及它们为何如此有用、基于目标最小损失的估计或超级学习者的信息，我推荐以下资源:

*   Pearl、Glymour M .和 Jewel N.P .，[统计学中的因果推断，初级读本](http://bayes.cs.ucla.edu/PRIMER/) (2016) Wiley
*   díaz muoz，I .和 van der Laan，m .，[基于随机干预的人口干预因果效应](https://pubmed.ncbi.nlm.nih.gov/21977966/) (2012)，生物统计学
*   Díaz，I .、Williams，n .、Hoffman，K. L .和 Schenck，E. J .，[基于纵向修正治疗政策的非参数因果关系](https://arxiv.org/abs/2006.01366v2) (2020)，arXiv
*   Haneuse，s .和 Rotnitzky，a .，[对修改已接受治疗的干预措施效果的评估](https://onlinelibrary.wiley.com/doi/abs/10.1002/sim.5907?casa_token=Mc7QwrKckREAAAAA%3ADJfUP5oz0R9BP4YtRYz2g65EYgZsX1TsTceFl-VgIWuhA5DX1KWznh1KyzSLn8H9DuA5RHur7zi4ib80) (2013)，医学统计
*   范德莱恩，M. J .和罗斯，S. [统计学中的目标学习](https://www.springer.com/gp/book/9781441997814) (2011)斯普林格系列
*   Naimi，A. I .和 Balzer，L. B. [堆叠概括:超级学习简介](https://link.springer.com/article/10.1007/s10654-018-0390-z) (2018)，《欧洲流行病学杂志》