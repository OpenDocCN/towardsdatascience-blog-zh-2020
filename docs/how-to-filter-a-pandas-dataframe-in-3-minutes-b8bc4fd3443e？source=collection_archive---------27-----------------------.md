# 如何在 3 分钟内过滤一个熊猫数据帧

> 原文：<https://towardsdatascience.com/how-to-filter-a-pandas-dataframe-in-3-minutes-b8bc4fd3443e?source=collection_archive---------27----------------------->

## 如何过滤熊猫数据框有很多选择，我将向你展示最常见的功能

![](img/e7274e0e13a0ff5a291d621069881146.png)

> 在 Pandas 中，有许多方法可以过滤数据帧。我将在辛普森一家的帮助下向你介绍最重要的选择。

## 布尔索引

布尔索引需要找到每一行的真值。如果你寻找`df['column'] == 'XY'` **，**，一个真/假序列被创建。

我想象你刚刚在《辛普森一家》的片场。他们是明星，每个人都可以为下一季订购一些东西，由制作人支付费用。你的工作是接受主角的命令并转发相关数据:

```
df = pd.DataFrame({'Items': 'Car Saxophone Curler Car Slingshot Duff'.split(),
 'Customer': 'Homer Lisa Marge Lisa Bart Homer'.split(),
 'Amount': np.arange(6), 'Costs': np.arange(6) * 2})print(df)
       Items Customer  Amount  Costs
0        Car    Homer       0      0
1  Saxophone     Lisa       1      2
2     Curler    Marge       2      4
3        Car     Lisa       3      6
4  Slingshot     Bart       4      8
5       Duff    Homer       5     10
```

## 示例 1-选择具有特定值的行

让我们找出 Bart 的所有条目，以便转发给他的经理:

```
df.loc[df['Customer'] == 'Bart'] Items         Customer   Amount Costs
4  Slingshot     Bart       4      8
```

## 示例 2 —从列表中选择行

片场有传言说，我们是数据专家，广告合作伙伴想知道节目中的孩子们出于营销原因订购了什么:

```
kids = ['Lisa','Bart']df.loc[df['Customer'].isin(kids)] Items     Customer   Amount Costs
1  Saxophone     Lisa       1      2
3        Car     Lisa       3      6
4  Slingshot     Bart       4      8
```

我们会在电视广告上看到萨克斯管、汽车和弹弓…

## 示例 3 —组合多个条件

辛普森一家也必须省钱。新规则是:1)禁止汽车，2)每人最多可订购 3 件商品:

```
df.loc[(df['Items'] != 'Car') & (df['Amount'] <= 3)] Items Customer  Amount  Costs
1  Saxophone     Lisa       1      2
2     Curler    Marge       2      4
```

我希望霍默和巴特能和我们在一起，不要怒气冲冲地离开节目…

## 示例 4 —选择未出现在列表中的所有行

不幸的是，该系列的明星们对这些削减一点也不热情，所以第一批赞助商站出来帮忙:

```
happy_stars = ['Lisa','Marge']df.loc[~df['Customer'].isin(happy_stars)]Items     Customer   Amount Costs
1  Saxophone     Lisa       1      2
3        Car     Lisa       3      6
4  Slingshot     Bart       4      8
```

## 位置索引

有时，您不想根据特定条件进行过滤，而是根据数据帧的位置选择特定行。在这种情况下，我们使用切片来获得想要的行。

## 示例 1 —选择数据帧的前几行

你部门的新实习生不应该直接处理整个数据集，他只需要前三个条目:

```
df.iloc[0:3]

       Items Customer  Amount  Costs
0        Car    Homer       0      0
1  Saxophone     Lisa       1      2
2     Curler    Marge       2      4
```

## 示例 2 —选择数据帧的最后几行

你有很多工作要做，你会得到另一个实习生。为了让两个学员独立完成他们的任务，您现在保存记录的最后三行:

```
df.iloc[-3:]

       Items Customer  Amount  Costs
3        Car     Lisa       3      6
4  Slingshot     Bart       4      8
5       Duff    Homer       5     10
```

## 结论

我们能够根据其值或位置从我们的数据集中选择任何特定的行。Pandas 提供了在制定条件时考虑多个值的简单方法。列表和条件可以很容易地链接在一起。

[如果您喜欢中级和数据科学，并且还没有注册，请随时使用我的推荐链接加入社区。](https://medium.com/@droste.benedikt/membership)