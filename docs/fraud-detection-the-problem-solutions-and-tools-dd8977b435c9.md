# 欺诈检测:问题、解决方案和工具

> 原文：<https://towardsdatascience.com/fraud-detection-the-problem-solutions-and-tools-dd8977b435c9?source=collection_archive---------5----------------------->

## 机器学习如何改变欺诈检测格局

![](img/dfd476c4a4f431f2ef7ebdb57b3d5ea2.png)

瑞安·波恩在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 1.1 问题的性质

> [“欺诈是一个价值十亿美元的行业](https://en.wikipedia.org/wiki/Data_analysis_techniques_for_fraud_detection)
> 
> [而且每年都在增加](https://en.wikipedia.org/wiki/Data_analysis_techniques_for_fraud_detection)

什么是欺诈？有许多正式的定义，但本质上欺诈是一种“艺术”，是在金融交易中欺骗和诈骗人们的犯罪。欺诈在整个人类历史中一直存在，但在这个数字技术时代，金融欺诈的策略、范围和规模变得越来越广泛，从信用卡交易到健康福利再到保险索赔。欺诈者也变得越来越有创造力。谁从来没有收到过一个尼日利亚皇室寡妇的电子邮件，说她正在寻找一个值得信任的人来移交她的大笔遗产？

难怪欺诈是一件大事。由于欺诈性交易，商业组织的估计损失飙升至其收入的 4-5%。5%的欺诈听起来可能不算多，但从货币角度来看，这不是小事，远远超过不采取任何行动的成本。普华永道的一项调查发现，他们调查的 7200 家公司中有 50%是某种欺诈的受害者。FICO 最近的一项研究发现，接受调查的五分之四的银行经历了欺诈活动的增加，预计这种情况在未来还会增加。

尽管许多组织采取措施打击欺诈，但欺诈永远无法根除。我们的目标是将它的影响降到最低。这种筛选的好处必须与成本进行权衡，例如，欺诈检测技术的投资和由于“假阳性”警报而可能失去的客户。

因此，本文的目的是强调欺诈检测领域的一些工具、技术和最佳实践。最后，我将使用公开可用的数据集提供一个 python 实现。

# 1.2 个使用案例

欺诈无处不在——无论交易涉及到哪里——但信用卡欺诈可能是最广为人知的案例。它可以是简单的偷窃或使用偷来的卡，也可以是侵略性的形式，如账户接管、伪造[和更多](https://www.experian.com/blogs/ask-experian/credit-education/preventing-fraud/credit-card-fraud-what-to-do-if-you-are-a-victim/)。信用卡欺诈一直存在，但由于每天通过信用卡进行的在线交易越来越多，其规模只是在最近有所增长。根据[尼尔森报告](https://nilsonreport.com/upload/content_promo/The_Nilson_Report_10-17-2016.pdf)2010 年，全球欺诈金额为 76 亿美元，预计 2020 年将突破 310 亿美元。仅在英国，2018 年欺诈交易损失估计超过[10 亿美元。](https://en.wikipedia.org/wiki/Credit_card_fraud)

保险行业还在发生其他类型的大欺诈案件。一些估计表明，美国多达 10%的健康保险索赔可归因于欺诈，这是一个不小的数字，每年高达 1100 亿美元。

> 保险欺诈如此普遍，以至于有一个完整的组织叫做[反保险欺诈联盟](http://www.insurancefraud.org/)和一个杂志叫做[保险欺诈杂志](https://www.insurancefraud.org/jifa/jun-2016)来科学地研究保险业务中的欺诈。

# 2.1 数据科学解决方案

在过去(即机器学习成为趋势之前)，标准做法是使用所谓的“基于规则的方法”。由于每个规则都有例外，这种技术只能部分缓解问题。随着越来越多的在线交易和大量客户数据的产生，机器学习越来越被视为检测和反欺诈的有效工具。然而，没有一个特定的工具，银弹，适用于每个行业的各种欺诈检测问题。在每个案例和每个行业中，问题的性质是不同的。因此，每个解决方案都是在每个行业的领域内精心定制的。

在机器学习中，用语欺诈检测通常被视为一个[监督分类](/supervised-learning-basics-of-classification-and-main-algorithms-c16b06806cd3)问题，其中根据观察结果中的特征将观察结果分类为“欺诈”或“非欺诈”。由于不平衡的数据，这也是 ML 研究中一个有趣的问题，即在极其大量的交易中很少有欺诈案例。如何处理不平衡的阶级本身是另一个讨论的主题。

欺诈也可以通过几种异常检测技术来隔离。离群点检测工具有自己解决问题的方式，如[时间序列分析](https://www.datasciencecentral.com/profiles/blogs/outlier-detection-with-time-series-data-mining)，聚类分析，实时事务监控等。

# 2.2 技术

**统计技术:**平均值、分位数、概率分布、关联规则

**监督 ML 算法:**逻辑回归、神经网络、时间序列分析

**无监督的 ML 算法:**聚类分析、贝叶斯网络、对等组分析、断点分析、本福特定律(异常数定律)

# 3.一个简单的 Python 实现

## 3.1 数据准备

对于这个简单的演示，我使用了一个流行的 [Kaggle 数据集](https://www.kaggle.com/mlg-ulb/creditcardfraud)。

```
# import libraries
import pandas as pd
import numpy as np# import data
df = pd.read_csv("..\creditcard.csv")# view the column names
df.columns
```

数据集有 31 列。第一列“时间”是交易时间戳，倒数第二列“金额”是交易金额，最后一列“类别”指定交易是欺诈还是非欺诈(欺诈= 1，非欺诈= 0)。其余的列，“V1”到“V28”是未知变量，在公开数据之前进行了转换。

```
# number of fraud and non-fraud observations in the dataset
frauds = len(df[df.Class == 1])
nonfrauds = len(df[df.Class == 0])print("Frauds", frauds); print("Non-frauds", nonfrauds)## scaling the "Amount" and "Time" columns similar to the others variablesfrom sklearn.preprocessing import RobustScaler
rob_scaler = RobustScaler()df['scaled_amount'] = rob_scaler.fit_transform(df['Amount'].values.reshape(-1,1))
df['scaled_time'] = rob_scaler.fit_transform(df['Time'].values.reshape(-1,1))# now drop the original columns
df.drop(['Time','Amount'], axis=1, inplace=True)# define X and y variables
X = df.loc[:, df.columns != 'Class']
y = df.loc[:, df.columns == 'Class']
```

## 3.2 制作子样本

这是一个极度不平衡的数据集，所以我们需要通过所谓的欠采样进行子采样。

```
# number of fraud cases
frauds = len(df[df.Class == 1])# selecting the indices of the non-fraud classes
fraud_indices = df[df.Class == 1].index
nonfraud_indices = df[df.Class == 0].index# From all non-fraud observations, randomly select observations equal to number of fraud observations
random_nonfraud_indices = np.random.choice(nonfraud_indices, frauds, replace = False)
random_nonfraud_indices = np.array(random_nonfraud_indices)# Appending the 2 indices
under_sample_indices = np.concatenate([fraud_indices,random_nonfraud_indices])# Under sample dataset
under_sample_data = df.iloc[under_sample_indices,:]# Now split X, y variables from the under sample data
X_undersample = under_sample_data.loc[:, under_sample_data.columns != 'Class']
y_undersample = under_sample_data.loc[:, under_sample_data.columns == 'Class']
```

## 3.3 建模

```
## split data into training and testing set
from sklearn.model_selection import train_test_split# # The complete dataset
# X_train, X_test, y_train, y_test = train_test_split(X,y,test_size = 0.3, random_state = 0)# Split dataset
X_train_undersample, X_test_undersample, y_train_undersample, y_test_undersample = train_test_split(X_undersample,y_undersample                                                                                                                                                                                                                                                                                             ,random_state = 0)## modeling with logistic regression#import model
from sklearn.linear_model import LogisticRegression
# instantiate model
model = LogisticRegression()
# fit 
model.fit(X_train_undersample, y_train_undersample)
# predict
y_pred = model.predict(X_test_undersample)
```

## 3.4.模型验证

> 注意:不要使用准确性分数作为衡量标准。在一个具有 99.9%非欺诈观察的数据集中，您可能会在 99%的时间内做出正确的预测。混淆矩阵和精确度/召回分数是更好的度量。

```
# import classification report and confusion matrix
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrixclassification_report = classification_report(y_test_undersample, y_pred)
confusion_matrix = confusion_matrix(y_test_undersample, y_pred)print("CLASSIFICATION REPORT")
print(classification_report)
print("CONFUSION MATRIX") 
print(confusion_matrix)
```

## *结束注意:*

*感谢您通读这篇文章。一个 Jupyter 笔记本连同 python 演示可以在*[*GitHub repo*](https://github.com/mabalam/FraudDetection)*中找到。可以通过*[*Twitter*](https://twitter.com/DataEnthus)*或*[*LinkedIn*](https://www.linkedin.com/in/mab-alam/)*联系到我。*