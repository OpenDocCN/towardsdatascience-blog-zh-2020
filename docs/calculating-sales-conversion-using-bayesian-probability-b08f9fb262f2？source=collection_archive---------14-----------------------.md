# 使用贝叶斯概率计算销售转换

> 原文：<https://towardsdatascience.com/calculating-sales-conversion-using-bayesian-probability-b08f9fb262f2?source=collection_archive---------14----------------------->

## 计算销售转换率的直观方法，无需历史数据进行比较

![](img/a82f5b7a3b91e2df92d9ac59ce0fb185.png)

[图像来源](https://media.istockphoto.com/vectors/businessman-is-thinking-vector-id901587568?k=6&m=901587568&s=612x612&w=0&h=GKn9Bt7zjL7Dh0b1LApHIilWy26IUuIHcXUNDcuJjEE=)

下面发表的这篇文章从[的拉斯穆斯贝斯](http://www.sumsar.net/about.html)贝叶斯统计教程中获得了灵感和参考。下面，我试图解释贝叶斯统计如何应用于回答任何公司分析部门的人可能面临的问题。

**背景**

一个成功的企业通常希望通过各种营销策略获得新客户，从而扩大其客户基础。根据公司的商业模式，他们可能会选择各种营销策略方法。因此，当务之急是了解哪种策略最能产生有证据支持的成功，而不是直觉。

**渗透新市场**

*Seeder* 是一家在加州销售电动滑板车的公司，是该国最成功的电动滑板车生产商之一。他们试图通过在德克萨斯州销售电动滑板车来开发德克萨斯州地区。

播种机*的营销团队*利用印刷媒体并设计宣传册来吸引新客户。他们将小册子分发给 23 名德州人，最终卖出了 9 本。

**问题**

管理层想知道的是，印刷媒体在推广新型电动滑板车方面有多好？如果公司继续生产大量的小册子，比如说几十万册，他们期望看到的转化率是多少？

这个问题的答案初看起来很简单。使用营销团队收集的数据，通过将销售数量除以分发的手册总数来计算成功的概率。

```
brochures_distributed = 23
new_sales = 9
conversion_rate = new_sales/brochures_distributed
conversion_rate
```

![](img/0c7c3590dea5d63f18ec54b37ed63457.png)

汇率

我们可以看到，达成销售的概率约为 39%。这意味着，每分发 100 份宣传册， *Seeder* 应该能卖出 39 辆电动滑板车。

考虑到我们现有的数据，这似乎是一个不错的估计，但它有很大的不确定性。我们不知道哪些人看到了宣传册，他们决定购买电动滑板车的动机是什么，也不知道可能会严重影响他们购买电动滑板车决定的过多信息。小样本量也对转换估算的准确性提出了警告。

**使用贝叶斯概率来量化不确定性**

贝叶斯概率允许我们使用概率分布来量化未知信息。为了能够量化我们研究中的*不确定性*，我们需要三样东西。

1.  数据
2.  生成模型
3.  传道者

**数据**

我们案例中的数据将来自我们进行的试点研究。我们知道，在 23 个看过宣传册的德州人中，有 9 个已经转变为客户。

**生成模型**

生成模型可以被定义为我们传递给一些参数的一组指令，比如潜在的转换百分比，以便模型基于参数模拟数据。例如，我们可以假设有 40%的潜在转化率，我们向随机选择的 50 名德州人展示宣传册。基于我们假设的转换率，生成模型将估计要进行 20 次销售。

基于上面假设的生成模型，我们有关于我们可能进行的销售数量的信息，但是我们感兴趣的是可能进行的销售的百分比。我们的生成模型是模拟数据，但我们已经知道我们的数据是什么。我们需要逆向求解生成模型，使其输出转化率。为了实现这一点，我们需要的是第三项*先验*。

**前科**

先验是模型在看到任何数据之前所拥有的信息。在贝叶斯术语中，先验是一种概率分布，用于表示模型中的不确定性。为了给我们的模型创建先验，我们将使用均匀概率分布。通过使用从 0 到 1 的均匀概率分布，我们声明在看到任何数据之前，模型假设 0 和 1 之间的任何转换率 *(0%到 100%)* 都是同等可能的。

**拟合模型**

下面是模型工作的基本流程。

*   根据我们的先验分布，我们为参数值抽取一个随机样本。在我们的例子中，参数值是转换率。

```
# number of samples to draw from the prior distribution
n_size <- 100000# drawing sample from the prior distribution - which in our case is uniform distribution
prior_dist <- runif(n_size, 0, 1)# peeking at the histogram to verify the random sample was generated correctly.
hist(prior_dist, main = "Histogram of Prior Distribution", xlab = "Prior on the Conversion Rate", ylab = "Frequency")
```

![](img/7643f7209729e8a0f8b2a8e62df85260.png)

我们先前均匀分布的直方图

*   我们从步骤 1 中获取参数值，将其插入到我们的生成模型中，以便模型模拟一些数据。我们使用 for 循环多次重复这个过程。

```
# defining the generative model - a model or set of rules that we feed to parameters so that it simulates data based on those set of rules
generative_model <- function(rate) {
  sales <- rbinom(1, size = 23, prob = rate)
  sales
}# simulating the data through our generative model
sales <- rep(NA, n_size)
for(i in 1:n_size) {
  sales[i] <- generative_model(prior_dist[i])
}
```

*   筛选出与我们的数据一致的所有抽样，即转化率为 0.3913 或总销售额等于 9。

```
# filtering out values from the model that do not match our observed results
post_rate <- prior_dist[sales == 9]
```

我们在这里做的是多次重复采样过程，并在每次迭代后生成一个随机转换率。过滤掉样本图的原因是我们希望保留我们在现实中观察到的数据。也就是说，当营销团队做这个过程时，他们通过进行 **9** 销售产生了 **0.3913** 的转化率。

**真实转化率是多少？**

这个问题的答案不是一个数字。它是可能转化率的概率分布。这些可能的转换率分布如下。

```
#plotting the posterior distribution
post_rate_hist = hist(post_rate, xlim = c(0,1), main = "Histogram of Posterior Distribution")
post_rate_hist
```

![](img/a59641aceea010456efce775c27b6f26.png)

转化率的概率分布

从观察到的可能转化率的分布可以看出，最有可能的转化率应该存在于 **35** & **45** 百分比之间。我们也可以看到，转化率超过 **60%** 或低于 **20%** 的可能性极小。

我们可以通过计算每根棒线的频率并除以总抽取次数来计算可能的转换率的概率。

为了计算 40%和 45%之间的转换率的概率，我们做如下数学计算

```
# sum of frequency of draws between 0.40 and 0.45 divided by total draws
post_rate_hist$counts[8]/length(post_rate)
```

![](img/ffef9380d62eccada1fde43e30c99d0b.png)

转换概率在 40%-45%之间

在这里，我们有 20%的可能性，转化率会落在 40-45%之间。

**回答更多问题**

**哪种策略比较好？**

我们可以使用我们创建的模型来回答关于营销策略的比较问题。例如，我们可以比较两种营销策略的转换率。

比方说，营销团队告诉我们，当他们部署电子邮件营销时，转化率为 20%。管理层现在想知道印刷媒体营销优于电子邮件营销的可能性。

从我们的概率分布中，我们可以计算出转化率大于 20%的频率，除以总吸引次数，得到平面媒体营销实现转化率大于 20%的概率。

```
sum(post_rate > 0.20) / length(post_rate)
```

![](img/42829c16e853a3b9cfaa4bfa457c050d.png)

转化概率大于 20%

在这里，我们可以说印刷媒体有 98%的可能性实现超过 **20%** 的转化率

**使用置信区间**

我们可以使用置信区间来计算覆盖总体分布 95%的转换率。

```
quantile(post_rate, c(0.025, 0.975))
```

![](img/5f3a4fccf5b2c4425af280835ee7a040.png)

95%置信区间

在这里，它意味着我们有 95%的信心，真实的转化率落在 **22%** 和 **60%** 之间。

**结论**

贝叶斯概率允许我们通过在运行测试之前考虑我们所拥有的所有信息来区分真实和噪音。这正是我们在这里所做的。我们考虑了这样一个事实，即从 0 到 1 的转换发生的概率是相等的。然后我们进行了一次模拟，结果与我们分发宣传册时观察到的数据一致。最终结果不是一个单一的数字，而是概率的分布。

这种分布允许涉众输入他们的领域知识，并尝试估计在不确定性的影响下他们的问题的正确答案是什么。