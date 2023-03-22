# 使用无监督学习技术的分割——聚类

> 原文：<https://towardsdatascience.com/segmentation-using-unsupervised-learning-technique-clustering-bb4089c6392e?source=collection_archive---------28----------------------->

## 基于案例研究的使用分类技术确定高价值细分市场的实践指南

![](img/b613eeea37c265a2faf1361efced8db0.png)

梅尔·普尔在 [Unsplash](https://unsplash.com/@melipoole?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

“注意—细分练习在医疗保健和以客户为中心的行业中非常常见。细分分析有助于企业将精力和投资集中在高价值、忠诚或未开发的客户上。这对推动增长和收入非常重要。为了帮助您更全面地了解行业中通常是如何进行细分的，作者编写了一个问题陈述和一个虚拟数据来模拟这些分析的实时进行方式。

# **以下是我们将要讲述的五件事**

*   问题陈述
*   方法
*   聚类技术
*   眼前的步骤
*   让您的结果具有商业意义

# **问题陈述**

Andias Pharmaceutical 是一家总部位于美国的零售药店，在美国拥有约 3500 家门店。20%的商店每周 7 天、每天 24 小时营业，然而，其余的商店在正常营业时间营业。Andias Pharmaceuticals 在 2019 年第四季度的平均销售额增长了 3%，但客户群几乎保持不变。为了获得更多的病人，它正计划扩大业务，底特律(密歇根州的一个城市)是重点关注的地区。作为该计划的一部分，位于国会街西段的 Guardian 大楼附近的一家社区药房将于 2020 年 5 月开业。这家药店将是底特律第一家此类药店，将全天候运营。它将拥有训练有素的专业人员和药剂师，他们将帮助患者了解他们的保险细节，补充后续措施，药物摄入，甚至记录药丸可能产生的任何副作用。

为了让更多人了解该计划，运营总监 Sheldon Mall 先生希望联系药房内外的医生(**也称为内科医生/处方医生**)。由于财务部门没有为此计划分配更高的预算，他们计划将活动分为两部分。销售代表(Rep)瞄准高机会医生，直接邮寄给低机会医生。您需要确定 3 到 4 组这样的医生，以便开展活动。

# **进场**

## **为什么我们需要分类技术？**

问题陈述表明，该活动将使用两种不同的媒体展开:

**销售代表瞄准** —药店的销售代表将亲自前往与处方医生交谈。由于锁定销售代表的成本远高于数字或直接邮件营销活动，我们需要确定能够带来更多业务的**高价值处方商**。

如何定义**高值处方**？

*   现有业务良好的处方医生
*   具有高市场机会的处方者
*   有经验的处方医生(多年实践)
*   附属(执业)于多家医院的处方医师
*   处方量较高的处方者
*   患者人数较多的处方医生
*   具有已知专业的处方医生，如肿瘤学家、心脏病专家、家庭医生等。

**直邮** —包含商店及其服务信息的小册子将直接邮寄到处方者的地址。

*   我们可以看到，问题陈述要求我们根据医生/开处方者的执业特征来创建他们的组。活动的类型和活动的内容可以基于组所显示的特征来定制

![](img/8b01e3bc925e2c553f616f1e8feaa3ed.png)

处方者特质对信息传递的影响

# **聚类技术**

## **什么是聚类？**

基于某些标准(也称为变量)对**相似的**数据实体进行分组的技术。

*   例如，我们可以根据标准/变量(如收入、患者数量、总处方量、经验年限等)对处方者进行分组

***无监督学习*** 的一种形式——因为算法没有关于不同数据实体如何分组的预定义规则。

*   例如，数据集没有处方者组的预定义标签。该算法需要研究数据并将每个个体处方者分配到一个组中

所以，这是一种 ***数据调查*** 的方法——一种研究数据中重要模式或结构的方法。

*   例如，算法将研究所用变量的模式，并对数据进行分组。现有收入在 100 美元至 150 美元之间且有 200 至 300 名患者的处方者可以被分组为一个组

![](img/b6a4b10b7a61e1542cae96a5f6ffc509.png)

使用一维数据的聚类示例

*   我们取一个一维轴，代表一个英超赛季的进球数。这条线上显示的每个点代表一名足球运动员的总进球数
*   使用传统的统计数据，我们可以知道一个职业联赛赛季的平均进球数是 23 个，但是，如果我们必须根据进球数揭示数据中的一些模式，如球员之间的关系，我们会看到有两到三个更广泛的球员群体
*   K 均值聚类可以发现这样的组或模式，而不需要我们事先告诉它

## **什么是 K-Means 聚类？**

**K-Means** 算法是一种将数据集划分为多个聚类(比如 K)的方法，这样每个聚类都是不同的，即一个聚类不会与另一个聚类重叠，并且每个数据点只属于一个特定的聚类。它试图以这样一种方式构建聚类，使得一个聚类内的所有点(聚类内点)非常相似，但是来自不同聚类的点(聚类间点)是不同的。

它将数据点分配给一个聚类，使得数据点和聚类质心(属于该聚类的所有数据点的算术平均值)之间的平方距离之和最小。聚类中的差异越小，同一聚类中的数据点就越相似。

**优势**

*   当使用的数据集包含连续变量时效果最佳，例如，在问题陈述中，我们需要考虑收入、处方、经验年限、患者人数等指标，这些指标可能包含特定范围内的任何可能值。收入范围从 0 到 1000 美元不等，因此被称为持续收入
*   当观察的数量很大时，K 均值比其他聚类技术工作得更快，例如，在底特律开业的开处方者的数量是 5000。K-Means 在这种情况下会工作得更快
*   当聚类变量彼此不同或分离良好时，给出更有利的结果，例如，如果处方者实际上彼此非常不同，那么 K-Means 将产生良好的结果

**缺点**

*   无法处理分类变量，即颜色、类型、品牌名称、专业等变量。
*   如果数据高度重叠，则很难确定 k 个聚类。对于所有数据变量的特征都非常相似的情况，K-Means 通常不能产生不相交的聚类，例如，如果处方者实际上彼此没有很大的不同，那么 K-Means 将产生很差的结果
*   欧几里德距离可以不平等地加权潜在因素，即在使用 K-Means 形成聚类时，不可能对单个或一组变量进行加权。例如，企业希望包括所有可能的变量，但在聚类过程中更重视收入和患者数量。这在 K-Means 中是不可能的

## **K-意味着如何工作？**

*   从数据集中随机选择 k 形心，使得每个数据点只能分配给一个形心。通常，大多数算法*选择 K 个没有任何缺失值*的随机点作为它们的初始质心，然而，对于一些统计包，使用**替换=部分或全部**的概念来确定初始质心
*   选择质心后，计算每个数据点和所选质心之间的欧几里德距离。注意，聚类遵循最小平方估计的概念，即对于属于聚类的点，质心和数据点之间的欧几里德距离应该最小
*   对于数据集中的每个观测值，将计算它们跨这些聚类质心的距离，然后将观测值分配给欧氏距离最短的那个聚类
*   将观察值分配给一个聚类后，会重新计算质心。聚类质心是分配给每个聚类的观察值的平均值。这个过程持续到满足局部收敛为止，即在当前迭代中形成的聚类类似于在先前迭代中形成的聚类

![](img/6aeff996a8884e0db3edc26280fec030.png)

聚类—聚类工作方式的图形表示

# **直接步骤**

## **数据准备**

**第一步:** **离群点处理:**任何与数据集中其余数据点有很大不同的数据点都被认为是离群点。例如，在开处方者数据集中，大多数收入指标介于 100 美元到 300 美元之间，但是，我们也观察到 1200 美元的值。在这种情况下，1200 美元是一个异常值。然而，一般来说，可以使用下面的方法来识别异常值，这种方法通常被称为 **winsorization。**

*   任何超出平均值正负 2/3 标准偏差的值都被视为异常值(两个标准偏差为 5 和 95 个百分点值，而三个标准偏差为 0.3 和 99.7 个百分点值)
*   或 Q1 之外的任何值-1.5 * IQR 和 Q3+1.5*IQR，其中 Q1 和 Q3 是第一和第三四分位数，IQR 是四分位数之间的范围，被视为异常值
*   移除异常值取决于业务需求和业务意识，但是建议在执行聚类练习之前移除任何异常值或限制它们
*   查看数据集时，您可以看到收入数字的巨大差异。总会有开处方者的收入远远高于其他群体。他们是现实中的异类吗？答案是否定的。但是将它们保留在数据集中会使聚类结果产生偏差，因此我们在分析中删除了这些数据

**第二步:** **缺失值处理**。任何缺失的值都应在聚类之前删除或处理。缺失值通常用数据集的中心趋势替换，即所考虑变量的平均值、中值或众数。通常使用像 K-最近邻这样的增强技术来代替集中趋势来处理这些缺失值。

*   例如，在我们的问题陈述中，当我们处理收入时，我们可以用人口的平均收入替换所有缺失的收入数字，但是在替换多年经验时，我们可以使用人口的模式来替换这些数字。从商业角度看，像底特律这样的地区可能有更多经验丰富的处方医生，因此最好看看人口模式

**第三步:** **检查聚类变量是否高度相关。**如果变量高度相关，那么可以去掉其中一个变量。这是因为相关变量往往会否定非相关变量的影响和重要性。

**第四步:** **在运行 K 均值模型之前，标准化**所有分析/聚类变量。由于 K-Means 使用欧几里得距离将值分配给它们的聚类，因此在将所有变量用于建模练习之前，确保它们属于相似的范围是很重要的。

![](img/11a3ca73d4a1667ce4ce8da026ef8a00.png)

为什么扩展/标准化在集群中很重要

在上面的示例中，我们看到聚类变量具有不同的标度，因此，与多年的经验相比，收入、患者总数和处方数等更大的变量对距离计算的影响更大。通常，最小-最大缩放、z 分数标准化和对数缩放是行业中使用的一些流行的标准化技术。

[](/are-your-coding-skills-good-enough-for-a-data-science-job-49af101457aa) [## 对于数据科学的工作，你的编码技能够好吗？

### 5 编码嗅探如果你在数据科学行业工作，你必须知道

towardsdatascience.com](/are-your-coding-skills-good-enough-for-a-data-science-job-49af101457aa) 

# **代码片段**

```
#--------------------------------Project: Identifying Physician Groups for Community Pharmacy Campaign-------------------
 #--------------------------------Author: Angel Das--------------------------------
 #--------------------------------Create Date: Mar 2, 2020--------------------------------
 #--------------------------------Last Modified: Mar 2, 2020--------------------------------
 #--------------------------------Description: Extract and transform prescriber level information and create clusters using K-Means algorithm--------------------------------#--------------------------------Note: Dummy Data is used for this analysis, not a lot of data is required to complete the analysis#-------------------------------Getting Required Libraries in R--------------------------------------
 install.packages('gdata') #------contains function to read Excel files
 install.packages("corrplot")#------drawing a correlogram
 install.packages("purrr")
 install.packages("factoextra")
 install.packages("dplyr")library(gdata)
 library(corrplot)
 library(purrr)
 library(factoextra)
 library(dplyr)
 #------------------------------Storing Folder address from where inputs will be extracted and outputs will be stored-------------------
 #folder_address="C:/Users/91905/Desktop/Clustering"#--------------------------------Extracting Prescriber Information---------------------------------------
 #--------------------------------QC: 99 observations and 12 variables are read for the analysisprescriber_data=read.csv("C:/Users/91905/Desktop/Clustering/Physician_Data.csv",header=TRUE)#--------------------------------Variable Definition---------------------------------------------------------
 #--1\. HCP: Unique Identifier to of a prescriber
 #--2\. Scripts: Total prescriptions that came to Andias Pharmaceutical in the last 3 months
 #--3\. Market_Scripts: Total prescriptions written in the last 3 months
 #--4\. Revenue: Total Revenue of Andias Pharmaceutical in the last 3 months
 #--5\. Market_Revenue: Revenue generated in the market in the last 3 months
 #--6\. scripts_Share: Scripts/Market_Scripts (share of business)
 #--7\. Revenue_Share: Revenue/Market_Revenue
 #--8\. Pat_count: Patients visiting a prescriber in the last 3 months
 #--9\. Affiliation: Number of Hospitals where an HCP Practices
 #--2\. Experience: Number of Years since the Prescriber has started practicing
 #--2\. Specialty: Specialty of the Prescriber#------------------------------Exploratory Data Analysis in R-------------------------------------------
 #------------------------------Analyzing variables to understand missing observation, mean, median, percentiles etc.prescriber_data_upd<-subset(prescriber_data, select=-c(HCP,Specialty))
 variable_summary=summary(prescriber_data_upd)#------------------------------Display Variable Summary--------------------------------------------------
 variable_summary#--------------------------------Findings: Variables Scripts, Market_Revenue, scripts_share, Affiliation, Experience & Conference_Attended have missing values"
 Once the data summary is obtained, it is important to perform an outlier treatment before missing values are treated.
 As discussed we often replace missing values using Mean, hence mean calculated directly without an outlier treatment will skew the figures.
 "#--------------------------------Box plots to check Outliers-------------------------------------------boxplot(prescriber_data_upd$Scripts, prescriber_data_upd$Market_Scripts, prescriber_data_upd$Revenue, prescriber_data_upd$Market_Revenue,
         main = "Multiple boxplots for comparision",
         at = c(1,2,4,5),
         names = c("Scripts", "Market Scripts", "Revenue", "Market_Revenue"),
         las = 2,
         col = c("orange","red"),
         border = "brown",
         horizontal = TRUE,
         notch = FALSE
 )#--------------------------------Missing Value Treatment------------------------------------------------
 "
 1\. There are two ways to treat missing values. Omit all records which have at least one missing value using na.omit()
 2\. Replace observations with missing value using population mean, median and mode. This is preferable for any business problem
 as you don't want to lose out an entity just because one field or two fields are missing. Comes in handy especially where the number of observation
 is less."#--------------------------------Removing records with missing value---------------------------
 #--------------------------------QC: 80 observations
 prescriber_data_mst<-na.omit(prescriber_data_upd)#---------------------------------Correlation Check. Pearson Correlation coefficient--------------------correlation_data<-round(cor(prescriber_data_mst,method="pearson"),2)#--------------------------------Use corrplot() to create a correlogram--------corrplot(correlation_data, type = "upper", order = "hclust", 
          tl.col = "black", tl.srt = 45)
 wss <- function(k) {
   kmeans(prescriber_data_scale, k, nstart = 10)$tot.withinss
 }# Compute and plot wss for k = 1 to k = 15
 k.values <- 1:10# extract wss for 2-15 clusters
 wss_values <- map_dbl(k.values, wss)"
 Revenue, Scripts Share, and Pat Counts are highly correlated. This is because higher the scripts, higher the revenue and patient count"#---------------------------------Retaining non-correlated variables: Scripts, Market Revenue, Revenue Share, Affiliation, conference attended and experience---------------------------wss_valuesprescriber_data_req<-subset(prescriber_data_mst, select=c(Scripts,Market_Revenue,Affiliation,Conference_Attended,Revenue_Share,Experience))#--------------------------------Standardizing Variables: Make sure to remove any categorical variable during scaling-----------------------------------
 prescriber_data_scale<-scale(prescriber_data_req)#--------------------------------Clustering: Elbow Plot
 set.seed(123)# function to compute total within-cluster sum of squareplot(k.values, wss_values,
      type="b", pch = 19, frame = FALSE, 
      xlab="Number of clusters K",
      ylab="Total within-clusters sum of squares")
```

## 决定 K-均值中的 K

在运行 K 均值算法时，决定聚类的数量是很重要的。由于 K-Means 聚类是一种无监督学习形式，因此事先并不知道最适合数据集的聚类数。你可以按照下面的步骤得出 k 的最佳值。

**步骤 1** :对于 i=1 到 n(其中 n 表示所需的最大集群数)，运行以下步骤:

*   **步骤 1.1** :设置 K=i，使用任意聚类函数进行聚类(基本上会创建 K 个聚类)。对于 i=1，将创建一个集群，对于 i=2，将创建两个集群，依此类推
*   **步骤 1.2** :找出创建的 K 个聚类的 SSE(误差平方和)。 ***SSE 是一个点到聚类质心的距离的平方之和***
*   **步骤 1.3** :重复步骤 1.1 & 1.2，直到 i=n，即循环遍历所需的最大聚类数

**第二步**:使用折线图绘制上证综指穿过 k 线

**第三步:**用折线图绘制上证综指在 k 线上的变化。这将有助于随着集群数量的增加，可视化 SSE 的变化

**第四步:**计算每个聚类值的输出，如总内数、中间数、方差分析、大小以及中间数与内数之比

![](img/ab53adb0d05bae6cdd1a2013e37f8406.png)

弯头绘图图

![](img/0fbd22db0687631b2f4cefe0fdb05603.png)

集群 EDA

```
cluster_results=kmeans(prescriber_data_scale,8, nstart=13)
 #cluster_resultsfviz_cluster(cluster_results, data = prescriber_data_scale)cluster_numbers<-as.data.frame(cluster_results$cluster)
 names(cluster_numbers)[1] <- "cluster"final_result=cbind(prescriber_data_req,cluster_numbers)write.csv(final_result,"C:/Users/91905/Desktop/Clustering/Results.csv")final_result %>%
   group_by(cluster) %>%
   summarise_all("mean")
```

# **让您的结果具有商业意义**

一旦结果出来，对你的结果进行商业意义上的分析是很重要的。以下是我遵循的一些最佳实践:

*   计算每个聚类组的描述性统计数据，即参与聚类的所有变量的平均值、中值、最小值和最大值
*   描述性统计将帮助我们理解和识别每个聚类的特征。在上面的例子中，具有较低收入和较高市场脚本的聚类表示具有机会或潜力的处方者群体，即他们具有良好的市场存在，但是为 Andias 药房编写的脚本很少
*   为每个集群指定业务名称，而不是集群编号。利益相关者更容易理解每个集群的含义，而不仅仅是查看图表
*   记住，任何业务都需要理解不同的集群意味着什么。这给了他们对细分的信心，以及某个特定的计划是否合适

最后，如果时间允许，进行一个快速的前后分析，以检查这些分类的描述性统计数据是否随时间而变化。

[](/introduction-to-hive-859ba31a5769) [## Hive 简介

### Hive & SQL 编码初学者指南

towardsdatascience.com](/introduction-to-hive-859ba31a5769) 

*关于作者:高级分析专家和管理顾问，帮助公司通过对组织数据的商业、技术和数学的组合找到各种问题的解决方案。一个数据科学爱好者，在这里分享、学习、贡献；可以和我在* [*上联系*](https://www.linkedin.com/in/angel-das-9532bb12a/) *和* [*推特*](https://twitter.com/dasangel07_andy)*；*