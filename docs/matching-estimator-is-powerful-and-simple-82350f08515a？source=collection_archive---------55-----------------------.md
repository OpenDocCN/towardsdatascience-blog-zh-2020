# 匹配估计器功能强大且简单

> 原文：<https://towardsdatascience.com/matching-estimator-is-powerful-and-simple-82350f08515a?source=collection_archive---------55----------------------->

## 您可以使用 13 行 R 代码手动进行匹配

![](img/48a44ee37d9b35384cec9e267cc7072a.png)

upslash.com

# 背景

为获得[因果关系](https://medium.com/@bowenchen_9406/the-you-know-who-in-economics-a9ef41b8e82e)的许多观察研究面临的挑战是自我选择——在许多情况下，人们出于某些原因选择接受治疗，因此，接受治疗的人不能直接与未接受治疗的人进行比较。例如，来自富裕家庭的学生可能更有可能选择上私立大学。如果我们想知道上私立大学对收入的因果影响，我们应该排除(或控制)家庭背景的影响。

匹配估计量已广泛应用于统计学、经济学、社会学、政治学等学科，用于估计因果关系。这是一种准实验方法，目的是在许多未处理单元中寻找与处理单元相当的反事实单元。

匹配很简单，从技术上讲，很大程度上是数据处理。然而，我已经看到许多资料，使简单的方法难以理解。这太令人沮丧了！所以我决定通过这个方法向您展示，您可以用少于 15 行的 R 代码实现这个方法。我保证！

# 运行倾向评分匹配的 r 代码

我在这里重点介绍倾向得分匹配，因为它是一种流行的匹配方法。其他匹配方法和这个差不多。

首先，我们用下面的 R 代码模拟一些数据。这些都是不必要的。复制粘贴即可。在这些代码中，我指定了两个协变量:W1 和 W2。a 是二元处理，Y 是观察结果。Y.0 和 Y.1 是潜在的结果，所以真实的治疗效果只是 Y.0 和 Y.1 的差别。

```
# Make the libraries ready
library(dplyr)
library(Matching)generateData<- function(n) {
 # Generate baseline covariates:
 W1 <- runif(n, min = 0.02, max = 0.7) 
 W2 <- rnorm(n, mean = (0.2 + 0.125*W1), sd = 1) 
 # Generate binary treatment indicator
 A <- rbinom(n, 1, plogis(-.7 + 1.8*W1–1.4*W2))
 # Generate the potential outcomes
 Y.0 <- rnorm(n, mean = (-.5 + 2*poly(W1,2) — W2), sd=1)
 Y.1 <- rnorm(n, mean = (-.5 + 2*poly(W1,2) — W2 + 1.5 + 2*poly(W1,2) — W2), sd=1)
 # Generate the observed outcome
 Y <- ifelse(A == 1, Y.1, Y.0)
 return(data.frame(W1,W2,A,Y,Y.1,Y.0))
}
set.seed(101)
data <- generateData(n = 1000) # Generate 1000 data.
```

用于匹配的 r 代码:

第一步。估计 logit 模型以获得倾向得分:

```
logitmodel <- glm(A ~ W1 + W2, data = data, family=’binomial’) # L1
```

第二步。在与已处理单元的估计倾向得分差异最小的未处理单元中进行搜索。发现的单元然后被用作反事实单元，用于估算处理单元的潜在结果。

```
# Add the estimated ps to the data. 
data <- mutate(data, pscore = logitmodel$fitted.values) #L2
data1 <- filter(data, A == 1) # Treated units data #L3
data0 <- filter(data, A == 0) # Untreated units data #L4match.data <- data1 #Make a copy of treated units data #L5
for(i in 1:nrow(match.data)){#Now find counterfactual units #L6
 temp <- data0 %>% # Search among untreated units #L7
 mutate(pscorei = data1$pscore[i], # PS of the treated unit i #L8
      dif.score = abs(pscorei — pscore)) %>% # Score difference
      arrange(dif.score) %>% # Arrange the data  #L10
      slice(1) %>% # Choose the top one with lowest score dif
      dplyr::select(!!colnames(match.data)) # Keep needed cols# L11
 match.data[i,] <- temp[1,] # Replace with the found unit #L12
}
```

第三步。计算观察到的 Y 和匹配的 Y 之间的平均差异。该平均差异是被治疗者的平均治疗效果(ATT)。

```
mean(data1$Y — match.data$Y) #L13
```

# 结束

我刚刚展示了如何使用 13 行 R 代码进行匹配。还有其他方法可以加速编码，计算标准误差，估计治疗对未治疗者的影响，等等。但这些都不是本文的目标。敬请关注我的下一篇文章！

万一你不相信我，你可以运行下面的代码从*匹配*包得到相同的 ATT 值。

```
m.out <- Match(Y = data$Y, Tr = data$A, X = data$pscore, distance.tolerance = 0)
summary(m.out)mean(data1$Y.1 - data1$Y.0) # True ATT
```