# 数据科学编码练习项目报告

> 原文：<https://towardsdatascience.com/project-report-for-data-science-coding-exercise-9a9c76a09be8?source=collection_archive---------13----------------------->

![](img/2a13cd121419ee5b1fa80e3a935756e6.png)

Benjamin O. Tayo 的图片

## 数据科学带回家的编码挑战问题的示例项目报告

# 一.导言

带回家的挑战问题或编码练习是数据科学家面试流程中最重要的一步。这通常是一个数据科学问题，例如机器学习模型、线性回归、分类问题、时间序列分析等。一般来说，面试团队会给你提供项目方向和数据集。

一些编码挑战问题会指定一个正式的项目报告与一个 Jupyter 笔记本或 R 脚本文件一起提交。这篇文章将提供一些关于如何为带回家的编码挑战问题写一份正式的项目报告的指南。

本文组织如下。在第二部分中，我们描述了项目陈述和项目目标。在**第三章**中，我们描述了一个问题的示例解决方案，包括数据集、代码和输出。在**第四部分**，我们提交了一份关于带回家挑战问题的项目报告样本。一个简短的总结结束了这篇文章。

# 二。项目说明

在这个问题中，你将预测贷款组合的结果。每笔贷款计划在 3 年内偿还，结构如下:

*   *首先，借款人收到资金。这个事件被称为起源。*
*   *然后，借款人定期还款，直到发生以下情况之一:*

*(i)借款人在 3 年期限结束前停止付款，通常是由于财务困难。这一事件被称为销账，然后贷款被称为已销账。*

*(ii)借款人继续还款，直到发放日之后 3 年。至此，债务已全部还清。*

*在附加的 CSV 中，每行对应一笔贷款，列的定义如下:*

*   *标题为“自发起以来的天数”的列表示发起和数据收集日期之间经过的天数。*
*   *对于在收集数据之前已经销账的贷款，标题为“从发起到销账的天数”的列表示发起和销账之间经过的天数。对于所有其他贷款，此栏为空白。*

***目标*** *:我们希望您估计这些贷款在全部 3 年期限结束时将会被冲销的比例。请包括你如何得到你的答案的一个严格的解释，并且包括你使用的任何代码。你可以做出简化的假设，但是请明确地陈述这些假设。* ***请随意以您喜欢的任何格式提交您的答案；特别是 PDF 和 Jupyter 笔记本都不错*** *。此外，我们希望这个项目不会占用您超过 3-6 个小时的时间。*

# 三。项目数据集、代码和示例输出

这个问题的数据集和建议的解决方案(包括代码和输出)可以从下面的链接下载:

[贷款状况的蒙特卡洛模拟](https://github.com/bot13956/Monte_Carlo_Simulation_Loan_Status)

**注** : *以上给出的建议解决方案是我对该问题的解决方案版本。请记住，数据科学或机器学习项目的解决方案不是唯一的。欢迎你自己尝试这个问题，提出你自己的预测模型。*

# 四。项目报告

完成项目的编码部分后，是时候整理一份正式的项目报告了。下面是该项目的样本报告。

## **利用蒙特卡罗模拟预测贷款状况**

**摘要:**使用所提供的数据集，我们建立了一个简单的模型，使用蒙特卡罗(MC)模拟来预测贷款在 3 年期限后违约的比例。对于 N = 1000 个数据集复制副本的蒙特卡洛模拟，我们的模型显示了 14.8% +- 0.2%的 95%置信区间。

**关键词**:贷款状态、贷款发放、贷款核销、蒙特卡洛模拟、预测分析

**引言:**预测贷款的状况是风险评估中的一个重要问题。银行或金融机构在向客户发放贷款之前，必须能够估计所涉及的风险。数据科学和预测分析技术可以用来预测贷款违约的概率。在本项目中，我们获得了包含 50000 个数据点的[***loan _ timing . CSV***](https://github.com/bot13956/Monte_Carlo_Simulation_Loan_Status)数据集。每个数据点代表一笔贷款，提供两个特征如下:

*   标题为“**自发起以来的天数**”的列表示从发起到收集数据的日期之间经过的天数。
*   对于在收集数据之前销账的贷款，标题为“从发起到销账的**天”的列表示发起和销账之间经过的天数。对于所有其他贷款，此栏为空白。**

**项目目标:**这个项目的目标是使用数据科学的技术来估计这些贷款在所有三年期限结束时已经注销的部分。

**探索性数据分析:**数据集在 R 中很重要，使用 R 进行计算。我们绘制了下图:

![](img/73ae604cd960383468af4320992af7e8.png)

**图 1** :当前贷款自发放以来的天数直方图。

![](img/4b510cd3f176960f1d995919e49fb290.png)

**图 2** :违约贷款销账天数柱状图。

![](img/4c511336c1b95c7f6f23a9d1ab0ef5fb.png)

**图 3** :违约贷款发放以来天数柱状图。

**图 1** 显示了当前(活跃)贷款的直方图，该直方图具有很好的近似性，自发放以来在几天内均匀分布。

在**图 2** 中，我们看到从发放到核销的贷款比例随着天数的增加而减少。这说明越年轻的贷款违约概率越大。它还显示，100%的贷款在自发放日起的 2 年内(730 天)违约。

**图 3** 显示了从发放贷款到收集贷款状态数据期间的违约贷款分布情况。违约贷款中有很大一部分(71%)是一年或一年以上的贷款。

我们进行了蒙特卡洛模拟，以研究违约贷款的核销天数和发放后天数之间的关系，并将结果与原始样本数据进行比较，如图**图 4** 和 **5 所示。**由于贷款核销存在随机性(随机过程)，我们看到蒙特卡罗模拟为违约贷款的分布提供了合理的近似。

![](img/64b4f2b94afcc2705819057260257f71.png)

**图 4** :违约贷款的核销天数与发放后天数的关系图。

![](img/a8067d42622dd5a868264f897a49d722.png)

**图 5** :违约贷款的核销天数与发放后天数的蒙特卡洛模拟。

**模型选择:**我们的数据集只有 2 个特征或预测因子，并且避开了流行性问题:93%的贷款具有活跃(当前)状态，而 7%具有违约状态。使用线性回归来预测 3 年贷款期限后将被注销的贷款部分，将产生一个偏向于活跃贷款的模型。

**图 4** 和 **5** 表明，可以使用蒙特卡罗方法模拟违约贷款的核销天数和发放后天数之间的关系。因此，我们选择蒙特卡洛模拟作为我们预测贷款违约比例的模型。

**预测:**由于我们已经证明，在最初 2 年(即 0 至 730 天)中，可以使用蒙特卡罗模拟来近似计算待核销天数和自发放以来的天数之间的关系，因此我们可以使用相同的方法来预测在所有 3 年期限结束时将被核销的贷款比例。

我们数据集中冲销贷款的总数是 3，305。这意味着目前有 46，695 笔贷款处于活跃状态。在这些活跃的贷款中，一定比例的贷款将在 3 年内违约。为了估计违约贷款的总比例，我们模拟了涵盖整个贷款期限(即 0 至 1095 天)的冲销和自发放以来的天数的违约贷款，然后通过适当的缩放，我们计算了将在 3 年期限(即 1095 天)后冲销的贷款比例。

通过创建 1000 个随机试验，我们获得了 3 年贷款期限内违约贷款比例的以下分布(见**图 6** ):

![](img/6368214fc6c1879b0139dc2776a95242.png)

**图 6** :使用 N = 1000 个样本的 3 年期后冲销贷款比例直方图。

根据我们的计算，3 年贷款期限后将被冲销的贷款部分的 95%置信区间相应地为 14.8% +- 0.2%。

**结论:**我们提出了一个基于蒙特卡罗模拟的简单模型，用于预测在 3 年贷款期限结束时将违约的贷款比例。可以使用不同的模型，例如逻辑回归、决策树等。这将是一个好主意，尝试这些不同的方法，看看是否结果是可比的蒙特卡洛模拟结果。

[**附录:用于执行数据分析的 R 代码**](https://github.com/bot13956/Monte_Carlo_Simulation_Loan_Status/blob/master/loan_timing.R)

```
***# R CODE FOR PREDICTING LOAN STATUS*** *#author: Benjamin O. Tayo
#Date: 11/22/2018****# IMPORT NECESSARY LIBRARIES*** library(readr)
library(tidyverse)
library(broom)
library(caret)***# IMPORTATION OF DATASET*** df<-read_csv("loan_timing.csv",na="NA")
names(df)=c("origination","chargeoff")***#partition dataset into two: default (charged off ) and current*** index<-which(!(df$chargeoff=="NA"))
default<-df%>%slice(index)
current<-df%>%slice(-index)***# EXPLORATORY DATA ANALYSIS*****# Figure 1: Histogram of days since origination for current loans**current%>%ggplot(aes(origination))+geom_histogram(color="white",fill="skyblue")+ xlab('days since origination')+ylab('count')+ ggtitle("Histogram of days since origination for current loans")+ theme(plot.title = element_text(color="black", size=12, hjust=0.5, face="bold"),axis.title.x = element_text(color="black", size=12, face="bold"),axis.title.y = element_text(color="black", size=12, face="bold"),legend.title = element_blank())***# Figure 2: Histogram of days to charge-off for defaulted loans***default%>%ggplot(aes(chargeoff))+geom_histogram(color="white",fill="skyblue")+ xlab('days to charge-off')+ylab('count')+ ggtitle("Histogram of days to charge-off for defaulted loans")+ theme(plot.title = element_text(color="black", size=12, hjust=0.5, face="bold"), axis.title.x = element_text(color="black", size=12, face="bold"), axis.title.y = element_text(color="black", size=12, face="bold"), legend.title = element_blank())***# Figure 3: Histogram of days since origination for defaulted loans***default%>%ggplot(aes(origination))+geom_histogram(color="white",fill="skyblue")+ xlab('days since origination')+ylab('count')+ ggtitle("Histogram of days since origination for defaulted loans")+ theme(plot.title = element_text(color="black", size=12, hjust=0.5, face="bold"),axis.title.x = element_text(color="black", size=12, face="bold"),axis.title.y = element_text(color="black", size=12, face="bold"), legend.title = element_blank())***# Figure 4: Plot of days to charge-off vs. days since origination for defaulted loans***default%>%ggplot(aes(origination,chargeoff))+geom_point()+ xlab('days since origination')+ylab('days to charge-off')+ ggtitle("days to charge-off vs. days since origination")+ 
theme( plot.title = element_text(color="black", size=12, hjust=0.5, face="bold"), axis.title.x = element_text(color="black", size=12, face="bold"), axis.title.y = element_text(color="black", size=12, face="bold"),legend.title = element_blank())***# Figure 5: Monte Carlo Simulation of Defaulted Loans***set.seed(2)
N <- 3*365 ***# loan duration in days***
df_MC<-data.frame(u=round(runif(15500,0,N)),v=round(runif(15500,0,N)))
df_MC<-df_MC%>%filter(v<=u)
df_MC<-df_MC%>%filter(u<=730 & v<=730) ***#select loans within first 2 years***df_MC[1:nrow(default),]%>%ggplot(aes(u,v))+geom_point()+ xlab('days since origination')+ylab('days to charge-off')+ ggtitle("MC simulation of days to charge-off vs. days since origination")+ theme(plot.title = element_text(color="black", size=12, hjust=0.5, face="bold"),axis.title.x = element_text(color="black", size=12, face="bold"),axis.title.y = element_text(color="black", size=12, face="bold"),legend.title = element_blank())***# Predicting fraction of these loans will have charged off by the time all of their 3-year terms are finished***set.seed(2)
B<-1000
fraction<-replicate(B, {
df2<-data.frame(u=round(runif(50000,0,N)),v=round(runif(50000,0,N))) df2<-df2%>%filter(v<=u) 
b2<-(df2%>%filter(u<=730 & v<=730))
total<-(nrow(df2)/nrow(b2))*nrow(default)
100.0*(total/50000.0)})***# Figure 6: Histogram for fraction of charged off loans after 3-year term using N = 1000 samples***fdf<-data.frame(fraction=fraction)
fdf%>%ggplot(aes(fraction))+geom_histogram(color="white",fill="skyblue")+ xlab('fraction of charged off loans after 3-year term')+ylab('count')+ ggtitle("Histogram of total fraction of charged off loans")+ 
theme( plot.title = element_text(color="black", size=12, hjust=0.5, face="bold"),axis.title.x = element_text(color="black", size=12, face="bold"),axis.title.y = element_text(color="black", size=12, face="bold"),legend.title = element_blank())***# Calculate Confidence Interval for Percentage of Defaulted Loans After 3-year Term***mean<-mean(fraction)
sd<-sd(fraction)
confidence_interval<-c(mean-2*sd, mean+2*sd)
```

# 动词 （verb 的缩写）摘要

总之，我们已经描述了如何撰写数据科学带回家挑战的项目报告。一些数据科学家的工作面试会要求申请人提交一份正式的项目报告以及一个 Jupyter 笔记本或 R 脚本文件。这里提供的指导方针可以用来为带回家的编码练习准备正式的项目报告。

# 参考

1.  [贷款状况的蒙特卡洛模拟](https://github.com/bot13956/Monte_Carlo_Simulation_Loan_Status)。
2.  [数据科学家编码练习](https://medium.com/towards-artificial-intelligence/data-scientist-coding-exercise-e62f4de7df9e)。
3.  [数据科学家面试流程——个人经历。](https://medium.com/towards-artificial-intelligence/data-scientist-interview-process-a-personal-experience-33295495b4a0)