# 我能比 LendingClub 更好地对贷款进行评级吗？

> 原文：<https://towardsdatascience.com/loan-grading-showdown-ba64110c99f2?source=collection_archive---------39----------------------->

## 将我的神经网络与企业基准对比

![](img/27e0ff8b90f4f651938a46f4e7072357.png)

纽约公共图书馆在 [Unsplash](https://unsplash.com/s/photos/loans?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

1.  [**简介**](#4041)
2.  [**基本规则**](#2305)
3.  [**测试指标**](#60f3)
4.  [**轮到 lending club**](#1a0d)
5.  [**轮到我了**](#55bc)
6.  [**胜利！**](#d2ae)
7.  [**延伸阅读**](#644e)

# 介绍

如果你错过了，我[建立了一个神经网络来预测贷款风险](/loan-risk-neural-network-30c8f65f052e)使用来自 [LendingClub](https://www.lendingclub.com/) 的[公共数据集](https://www.kaggle.com/wordsforthewise/lending-club)。然后我构建了一个[公共 API](https://tywmick.pythonanywhere.com/) 来服务模型的预测。那很好，但是…我的模型有多好？

今天我将对它进行测试，将它与发放这些贷款的机构的风险模型进行对比。没错，LendingClub 在数据集中包括了他们自己计算的贷款等级(和子等级)，因此本世纪(或至少本周)最激动人心的风险建模对决的所有部分都已就绪。愿最好的算法胜出！

```
(1110171, 70) 
```

```
5 rows × 70 columns
```

顺便说一下，这篇文章改编自一个 Jupyter 笔记本，所以如果你想在你自己的笔记本上跟随，继续前进，在 Kaggle 上叉我的[！](https://www.kaggle.com/tywmick/can-i-grade-loans-better-than-lendingclub)

# 基本规则

这将是一场公平的战斗——我的模型不会使用 LendingClub 在计算贷款等级时无法访问的任何数据(包括等级本身)。

我将按时间顺序对数据集进行排序(使用`issue_d`列，即发放贷款的月份和年份),并将它分成两部分。前 80%我将用于训练我的竞争模型，我将比较后 20%的表现。

```
The test set contains 222,035 loans.
```

在测试集的早期，我的模型可能有一点信息优势，因为我接受了一些贷款的培训，这些贷款在 LendingClub 对这些贷款进行评级时可能还没有结束。另一方面，LendingClub 可能在测试集的后期具有轻微的信息优势，因为他们在测试集的早期就已经知道一些贷款的结果。

顺便说一句，我不得不称赞迈克尔·伍伦(Michael Wurm)提出的[将我的模型的表现与 LendingClub 的贷款等级进行比较的想法](/intelligent-loan-selection-for-peer-to-peer-lending-575dfa2573cb#fac8)，但我的方法相当不同。我不是在试图模拟一个投资组合的表现；我只是在评估我对简单风险的预测有多好。

# 测试度量

测试:谁能挑选出最好的 A 级贷款，根据我上一个笔记本中的独立变量来判断，即预期借款人将偿还的预期贷款回报的一部分(我将其设计为`fraction_recovered`)。

LendingClub 先拿盘子。我会从测试集中收集他们所有的 A 级贷款，统计一下，算出他们的平均值`fraction_recovered`。这个平均值将是我的模型必须超越的指标。

然后，我将使用我在上一个笔记本中选定的相同的[管道和参数](/loan-risk-neural-network-30c8f65f052e#b158)在训练集上训练我的模型。一旦对它进行了训练，我将使用它在测试集上进行预测，然后收集与 LendingClub 的 A 级贷款数量相等的顶级预测数量。最后，我将计算这个子集上`fraction_recovered`的平均值，这样我们就有了赢家！

# 轮到 LendingClub 了

```
LendingClub gave 38,779 loans in the test set an A grade.Average `fraction_recovered` on LendingClub's grade A loans:
0.96021
```

这个比例相当高。我有点紧张。

# 轮到我了

首先，我将从我以前的笔记本中复制我的`run_pipeline`函数:

现在是关键时刻了:

```
Epoch 1/100
6939/6939 - 11s - loss: 0.0245
Epoch 2/100
6939/6939 - 11s - loss: 0.0204
Epoch 3/100
6939/6939 - 11s - loss: 0.0203
Epoch 4/100
6939/6939 - 12s - loss: 0.0202
Epoch 5/100
6939/6939 - 11s - loss: 0.0202
Epoch 6/100
6939/6939 - 11s - loss: 0.0202
Epoch 7/100
6939/6939 - 11s - loss: 0.0201
Epoch 8/100
6939/6939 - 11s - loss: 0.0201
Epoch 9/100
6939/6939 - 13s - loss: 0.0201
Epoch 10/100
6939/6939 - 11s - loss: 0.0201
Epoch 11/100
6939/6939 - 11s - loss: 0.0201
Epoch 12/100
6939/6939 - 11s - loss: 0.0201
Epoch 13/100
6939/6939 - 11s - loss: 0.0201
Epoch 14/100
6939/6939 - 11s - loss: 0.0201
Epoch 15/100
6939/6939 - 11s - loss: 0.0201
Epoch 16/100
6939/6939 - 11s - loss: 0.0201
Epoch 17/100
6939/6939 - 11s - loss: 0.0201
Epoch 18/100
6939/6939 - 11s - loss: 0.0201
Epoch 19/100
6939/6939 - 11s - loss: 0.0201
Epoch 20/100
6939/6939 - 11s - loss: 0.0201
Epoch 21/100
6939/6939 - 11s - loss: 0.0201
Epoch 22/100
6939/6939 - 11s - loss: 0.0201
Epoch 23/100
6939/6939 - 11s - loss: 0.0200
Epoch 24/100
6939/6939 - 11s - loss: 0.0200
Epoch 25/100
6939/6939 - 11s - loss: 0.0200
Epoch 26/100
6939/6939 - 11s - loss: 0.0200
Epoch 27/100
6939/6939 - 11s - loss: 0.0200
Epoch 28/100
6939/6939 - 11s - loss: 0.0200
Epoch 29/100
6939/6939 - 11s - loss: 0.0200
Epoch 30/100
6939/6939 - 11s - loss: 0.0200
Epoch 31/100
6939/6939 - 12s - loss: 0.0200
Epoch 32/100
6939/6939 - 11s - loss: 0.0200
Epoch 33/100
6939/6939 - 11s - loss: 0.0200
Epoch 34/100
6939/6939 - 11s - loss: 0.0200
Epoch 35/100
6939/6939 - 11s - loss: 0.0200
Epoch 36/100
6939/6939 - 12s - loss: 0.0200
Epoch 37/100
6939/6939 - 13s - loss: 0.0200
Epoch 38/100
6939/6939 - 12s - loss: 0.0200
Epoch 39/100
6939/6939 - 11s - loss: 0.0200
Epoch 40/100
6939/6939 - 11s - loss: 0.0200
Epoch 41/100
6939/6939 - 11s - loss: 0.0200
Epoch 42/100
6939/6939 - 11s - loss: 0.0200
Epoch 43/100
6939/6939 - 11s - loss: 0.0200
Epoch 44/100
6939/6939 - 11s - loss: 0.0200
Epoch 45/100
6939/6939 - 11s - loss: 0.0200
Epoch 46/100
6939/6939 - 11s - loss: 0.0200
Epoch 47/100
6939/6939 - 12s - loss: 0.0200
Epoch 48/100
6939/6939 - 11s - loss: 0.0200
Epoch 49/100
6939/6939 - 11s - loss: 0.0200
Epoch 50/100
6939/6939 - 11s - loss: 0.0200
Epoch 51/100
6939/6939 - 11s - loss: 0.0200
Epoch 52/100
6939/6939 - 11s - loss: 0.0200
Epoch 53/100
6939/6939 - 11s - loss: 0.0200
Epoch 54/100
6939/6939 - 11s - loss: 0.0200
Epoch 55/100
6939/6939 - 11s - loss: 0.0200
Epoch 56/100
6939/6939 - 11s - loss: 0.0200
Epoch 57/100
6939/6939 - 11s - loss: 0.0200
Epoch 58/100
6939/6939 - 12s - loss: 0.0200
Epoch 59/100
6939/6939 - 11s - loss: 0.0200
Epoch 60/100
6939/6939 - 11s - loss: 0.0200
Epoch 61/100
6939/6939 - 11s - loss: 0.0200
Epoch 62/100
6939/6939 - 11s - loss: 0.0200
Epoch 63/100
6939/6939 - 11s - loss: 0.0200
Epoch 64/100
6939/6939 - 11s - loss: 0.0200
Epoch 65/100
6939/6939 - 13s - loss: 0.0200
Epoch 66/100
6939/6939 - 13s - loss: 0.0200
Epoch 67/100
6939/6939 - 11s - loss: 0.0200
Epoch 68/100
6939/6939 - 11s - loss: 0.0200
Epoch 69/100
6939/6939 - 12s - loss: 0.0200
Epoch 70/100
6939/6939 - 11s - loss: 0.0200
Epoch 71/100
6939/6939 - 11s - loss: 0.0200
Epoch 72/100
6939/6939 - 11s - loss: 0.0200
Epoch 73/100
6939/6939 - 11s - loss: 0.0200
Epoch 74/100
6939/6939 - 12s - loss: 0.0200
Epoch 75/100
6939/6939 - 11s - loss: 0.0200
Epoch 76/100
6939/6939 - 11s - loss: 0.0200
Epoch 77/100
6939/6939 - 11s - loss: 0.0200
Epoch 78/100
6939/6939 - 12s - loss: 0.0200
Epoch 79/100
6939/6939 - 12s - loss: 0.0200
Epoch 80/100
6939/6939 - 11s - loss: 0.0200
Epoch 81/100
6939/6939 - 11s - loss: 0.0200
Epoch 82/100
6939/6939 - 11s - loss: 0.0200
Epoch 83/100
6939/6939 - 11s - loss: 0.0200
Epoch 84/100
6939/6939 - 11s - loss: 0.0200
Epoch 85/100
6939/6939 - 13s - loss: 0.0200
Epoch 86/100
6939/6939 - 11s - loss: 0.0200
Epoch 87/100
6939/6939 - 11s - loss: 0.0200
Epoch 88/100
6939/6939 - 11s - loss: 0.0200
Epoch 89/100
6939/6939 - 11s - loss: 0.0200
Epoch 90/100
6939/6939 - 12s - loss: 0.0200
Epoch 91/100
6939/6939 - 11s - loss: 0.0200
Epoch 92/100
6939/6939 - 11s - loss: 0.0200
Epoch 93/100
6939/6939 - 13s - loss: 0.0200
Epoch 94/100
6939/6939 - 11s - loss: 0.0200
Epoch 95/100
6939/6939 - 11s - loss: 0.0200
Epoch 96/100
6939/6939 - 11s - loss: 0.0200
Epoch 97/100
6939/6939 - 11s - loss: 0.0200
Epoch 98/100
6939/6939 - 11s - loss: 0.0200
Epoch 99/100
6939/6939 - 11s - loss: 0.0200
Epoch 100/100
6939/6939 - 12s - loss: 0.0200Average `fraction_recovered` on Ty's grade A loans:
0.96108
```

# 胜利！

唷，好险！我的胜利可能太小，没有统计学意义，但嘿，看到我能跟上 LendingClub 最优秀和最聪明的人，这很酷。

# 进一步阅读

*   [贷款风险的自然语言处理](/loan-risk-nlp-d98021613ff3)

我现在真正想知道的是每个贷款俱乐部等级和子等级对应的估计风险的量化范围，但看起来[是专有的](https://www.lendingclub.com/foliofn/rateDetail.action)。有没有人知道贷款等级一般是不是和学术类的字母等级一样对应一定的百分比范围？如果没有，有没有更好的基准来评估我的模型的性能？请留下您的回复，加入我们的讨论吧！