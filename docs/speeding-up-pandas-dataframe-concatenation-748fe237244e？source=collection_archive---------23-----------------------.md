# 加速熊猫数据帧的拼接

> 原文：<https://towardsdatascience.com/speeding-up-pandas-dataframe-concatenation-748fe237244e?source=collection_archive---------23----------------------->

![](img/424b3aea36ba50f7a702670d0a011596.png)

熊猫[https://pandas.pydata.org/](https://pandas.pydata.org/)

## 简单的方法。

数据帧连接是一项开销很大的操作，尤其是在处理时间方面。假设有 12 个不同大小的熊猫数据帧，您想在列轴上连接它们，如下图所示。

```
df1 Shape:  (24588, 31201) 
df2 Shape:  (24588, 1673) 
df3 Shape:  (24588, 5)
df4 Shape:  (24588, 1)
df5 Shape:  (24588, 148) 
df6 Shape:  (24588, 1) 
df7 Shape:  (24588, 6) 
df8 Shape:  (24588, 1) 
df9 Shape:  (24588, 1) 
df10 Shape: (24588, 1) 
df11 Shape: (24588, 1) 
df12 Shape: (24588, 19)
```

为了提高 pd.concate()的速度，需要记住两件事。

1.  对于每个数据帧，总是***df = df . reset _ index(drop = true)。*** 记住串联命令使用索引，没有合适的索引你会得到错位的数据帧。
2.  始终尝试连接一系列数据帧。串联一个列表比串联单独的数据帧要快，即***df _ concat = PD . concat([df1，df2，…)。]，axis = 1)***

```
df_concat = pd.concat([df1, df2, df3, df4, df5, df6, df7, df8, df9, df10, df11, df12], axis=1)
```

你只需要知道这些:)

Ori Cohen 博士拥有计算机科学博士学位，主要研究机器学习。他是 TLV 新遗迹公司的首席数据科学家，从事 AIOps 领域的机器和深度学习研究。