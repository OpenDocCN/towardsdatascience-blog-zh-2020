# R 中的 Fisher 精确检验:小样本的独立性检验

> 原文：<https://towardsdatascience.com/fishers-exact-test-in-r-independence-test-for-a-small-sample-56965db48e87?source=collection_archive---------5----------------------->

![](img/770c3659a387fa3ef94fdd6efdcd5598.png)

莱昂·麦克布莱德的照片

# 介绍

A 在介绍了手工独立的[卡方检验](https://www.statsandr.com/blog/chi-square-test-of-independence-by-hand/)和 R 中的[之后，本文重点介绍费雪精确检验。](https://www.statsandr.com/blog/chi-square-test-of-independence-in-r/)

独立性检验用于确定两个分类变量之间是否存在显著关系。存在两种不同类型的独立性测试:

*   卡方检验(最常见的)
*   费希尔精确检验

一方面，当样本足够大时使用卡方检验(在这种情况下，p 值是一个近似值，当样本变得无限大时变得精确，这是许多统计检验的情况)。另一方面，当样本很小时，使用费希尔精确检验(在这种情况下，p 值是精确的，而不是近似值)。

文献表明，决定χ2 近似值是否足够好的通常规则是，当列联表的一个单元格中的**预期值**小于 5 时，卡方检验不合适，在这种情况下，费希尔精确检验是首选(MC crum-Gardner 2008；Bower 2003)。

# 假设

费希尔精确检验的假设与卡方检验的假设相同，即:

*   H0:变量是独立的，两个分类变量之间没有关系。知道一个变量的值无助于预测另一个变量的值
*   H1:变量是相关的，两个分类变量之间有关系。知道一个变量的值有助于预测另一个变量的值

# 例子

# 数据

以我们的例子为例，我们想确定吸烟和成为职业运动员之间是否有统计学上的显著联系。吸烟只能是“是”或“不是”，做职业运动员只能是“是”或“不是”。两个感兴趣的变量是定性变量，我们收集了 14 个人的数据。 [1](https://www.statsandr.com/blog/fisher-s-exact-test-in-r-independence-test-for-a-small-sample/#fn1)

# 观察到的频率

下表总结了我们的数据，报告了每个亚组的人数:

```
dat <- data.frame(
  "smoke_no" = c(7, 0),
  "smoke_yes" = c(2, 5),
  row.names = c("Athlete", "Non-athlete"),
  stringsAsFactors = FALSE
)
colnames(dat) <- c("Non-smoker", "Smoker")dat##             Non-smoker Smoker
## Athlete              7      2
## Non-athlete          0      5
```

# 预期频率

请记住，当期望频率低于 5 的列联表中至少有一个单元格时，使用费雪精确检验。要检索预期频率，将`chisq.test()`功能与`$expected`一起使用:

```
chisq.test(dat)$expected## Warning in chisq.test(dat): Chi-squared approximation may be incorrect##             Non-smoker Smoker
## Athlete            4.5    4.5
## Non-athlete        2.5    2.5
```

上面的列联表证实了我们应该使用费希尔精确检验而不是卡方检验，因为至少有一个单元格低于 5。

*提示*:虽然在决定卡方检验和费希尔检验之前检查预期频率**是一个好的做法，但是如果您忘记了，这并不是一个大问题。从上面可以看到，在 R 中做卡方检验时(用`chisq.test()`，会出现“卡方近似可能不正确”这样的警告。该警告意味着最小预期频率低于 5。因此，如果您忘记在对数据进行适当的测试之前检查预期频率，请不要担心，R 会警告您应该使用费雪精确测试，而不是卡方测试。**

# R 中的费希尔精确检验

要在 R 中执行 Fisher 精确检验，请像卡方检验一样使用`fisher.test()`函数:

```
test <- fisher.test(dat)
test## 
##  Fisher's Exact Test for Count Data
## 
## data:  dat
## p-value = 0.02098
## alternative hypothesis: true odds ratio is not equal to 1
## 95 percent confidence interval:
##  1.449481      Inf
## sample estimates:
## odds ratio 
##        Inf
```

输出中最重要的是 p 值。您还可以使用以下方法检索 p 值:

```
test$p.value## [1] 0.02097902
```

请注意，如果您的数据尚未显示为列联表，您可以简单地使用以下代码:

```
fisher.test(table(dat$variable1, dat$variable2))
```

其中`dat`是数据集的名称，`var1`和`var2`对应于两个感兴趣的变量的名称。

# 结论和解释

从输出和`test$p.value`中，我们看到 p 值小于 5%的显著性水平。像任何其他统计检验一样，如果 p 值小于显著性水平，我们可以拒绝零假设。

在我们的上下文中，拒绝费希尔独立性精确检验的零假设意味着两个分类变量(吸烟习惯和是否是运动员)之间存在显著关系。因此，知道一个变量的值有助于预测另一个变量的值。

感谢阅读。我希望这篇文章有助于您执行 Fisher 关于 R 中独立性的精确检验，并解释其结果。手动或 R 中的[了解更多关于独立性卡方检验的信息。](https://www.statsandr.com/blog/chi-square-test-of-independence-in-r/)

和往常一样，如果您有与本文主题相关的问题或建议，请将其添加为评论，以便其他读者可以从讨论中受益。

**相关文章:**

*   [安装和加载 R 包的有效方法](https://www.statsandr.com/blog/an-efficient-way-to-install-and-load-r-packages/)
*   [我的数据服从正态分布吗？关于最广泛使用的分布以及如何检验 R 中的正态性的注释](https://www.statsandr.com/blog/do-my-data-follow-a-normal-distribution-a-note-on-the-most-widely-used-distribution-and-how-to-test-for-normality-in-r/)
*   [R 中独立性的卡方检验](https://www.statsandr.com/blog/chi-square-test-of-independence-in-r/)
*   [如何在简历中创建时间线](https://www.statsandr.com/blog/how-to-create-a-timeline-of-your-cv-in-r/)

# 参考

基思·m·鲍尔，2003 年。"何时使用费雪精确检验."在*美国质量协会，六适马论坛杂志*，2:35–37。4.

埃维·麦克拉姆·加德纳律师事务所。2008."使用哪种统计检验是正确的？"*英国口腔颌面外科杂志* 46 (1)。爱思唯尔:38-41 岁。

1.  这些数据与涵盖手动[卡方检验的文章](https://statsandr.com/blog/chi-square-test-of-independence-by-hand/)中的数据相同，只是为了减少样本量，删除了一些观察数据。 [↩︎](https://statsandr.com/blog/fisher-s-exact-test-in-r-independence-test-for-a-small-sample/#fnref1)

*原载于 2020 年 1 月 28 日 https://statsandr.com**T21*[。](https://statsandr.com/blog/fisher-s-exact-test-in-r-independence-test-for-a-small-sample/)