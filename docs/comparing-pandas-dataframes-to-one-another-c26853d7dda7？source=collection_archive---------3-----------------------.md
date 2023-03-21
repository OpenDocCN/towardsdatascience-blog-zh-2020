# 比较熊猫的数据结构

> 原文：<https://towardsdatascience.com/comparing-pandas-dataframes-to-one-another-c26853d7dda7?source=collection_archive---------3----------------------->

![](img/0de435ce252e8a3a165c4eb2a4bea11b.png)

伊洛娜·弗罗利希在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 我将演示几种方法，并解释每种方法的优缺点

数据框架是数据科学的主力。虽然它们不是该领域最迷人的方面，但如果你让我挑选一个数据科学家掌握的最重要的东西，那就是熟练构建和操作数据框架的能力。

在过去的一篇文章中，我将数据帧描述为部分 Excel 电子表格和部分 SQL 表，但是具有 Python 的所有通用性和分析能力。老实说，我还不是一个专业的熊猫用户，但我的目标是成为一个。这就是为什么每当我学到一些新的有用的东西，我都会试着把它记录在这里。

今天的主题是比较平等(或不平等)的数据框架。通常，当处理存储在数据帧中的数据时，我们需要知道它们是否相同。如果不是，则突出显示差异。使用案例包括:

*   根据主拷贝快速检查您的数据帧。
*   如果您下载了现有数据集的更新版本，您可能希望标识任何新行或更新的单元格。

# 方法 1:使用。eq 方法

熊猫数据框自带便利**。eq** 法。它可以让您快速比较两个数据帧，并突出显示任何不同的单元格。例如，假设我们有一些 NBA 球员的数据和他们赢得的冠军数(戒指)。

现在我们假设一个朋友正在做一个关于篮球的研究项目，并要求我们检查他的数据(他比我们更少是一个 NBA 球迷)。我们的数据在 array_1 和 df_1，朋友让我们查的数据在 array_2 和 df_2:

```
*# Our data*
array_1 = np.array([['LeBron',3],
                    ['Kobe',5],
                    ['Michael',6,],
                    ['Larry',3],
                    ['Magic',5],
                    ['Tim',4]])
df_1 = pd.DataFrame(array_1, 
                    columns=['Player','Rings'])*# Data from friend*
array_2 = np.array([['LeBron',3],
                    ['Kobe',3],
                    ['Michael',6,],
                    ['Larry',5],
                    ['Magic',5],
                    ['Tim',4]])
df_2 = pd.DataFrame(array_2, 
                    columns=['Player','Rings'])
```

我们可以使用。快速比较数据帧的 eq 方法。的产量。eq 列出了每个单元格位置，并告诉我们该单元格位置的值在两个数据帧之间是否相等(注意第 1 行和第 3 行包含错误)。

```
**In:**
df_1.eq(df_2)**Out:
**   Player  Rings
0    True   True
1    True  False
2    True   True
3    True  False
4    True   True
5    True   True
```

我们可以使用**布尔索引**和**。all** 方法打印出有错误的行。布尔索引使用一组条件来决定打印哪些行(布尔索引等于 True 的行被打印)。

```
**In:** *# .all returns True for a row if all values are True*df_1.eq(df_2).all(axis=1)**Out:** 0     True
1    False
2     True
3    False
4     True
5     True*# Above the rows marked True are the ones where all values equal.
# We actually want the rows marked False***In:** *# Note that we specify the axis to let Pandas know that we care* # *about equality across all the columns in a row*df_2[df_1.eq(df_2).all(axis=1)==False]**Out:
**  Player Rings
1   Kobe     3
3  Larry     5
```

我们发现我们朋友关于科比和拉里的数据都是错的(科比其实就是 5 环的那个)。因为我们正在寻找值不相等的单元格，所以我们实际上可以使用**更简洁地完成这项工作。ne** 方法(。ne 代表不相等)和**。任何**:

```
**In:** # *.any returns true for row if any of the row's values are True*df_2[df_1.ne(df_2).any(axis=1)]**Out:
**  Player Rings
1   Kobe     3
3  Larry     5
```

突出的一点是。情商和。当我们比较相同维数的数据帧时，效果更好。但是如果他们不是呢？比如说我们朋友的 dataframe 行数比我们多(加了 KG 和 Charles)。

```
*# Data from friend*
array_3 = np.array([['LeBron',3],
                    ['Kobe',3],
                    ['Michael',6,],
                    ['Larry',5],
                    ['Magic',5],
                    ['Tim',4],
                    ['KG',1],
                    ['Charles',0]])
df_3 = pd.DataFrame(array_3, 
                    columns=['Player','Rings'])
```

现在，让我们使用。需要找出不同之处。的。ne 方法已确定第 1、3、6 和 7 行中的差异。

```
**In:**
df_1.ne(df_3)**Out:**
   Player  Rings
0   False  False
1   False   True
2   False  False
3   False   True
4   False  False
5   False  False
6    True   True
7    True   True
```

如果我们试图在 df_1 上使用布尔索引来打印差异，Python 会给我们一个警告(因为我们的布尔索引比 df_1 长)，并且只打印 Kobe 和 Larry 的行(我们最初识别的差异)。它没有打印出 KG 和 Karl，因为它们不在 df_1 中。为了打印所有的文件，我们需要在 df_3 上使用布尔索引:

```
**In:**
df_3[df_1.ne(df_3).any(axis=1)]**Out:**
    Player Rings
1     Kobe     3
3    Larry     5
6       KG     1
7  Charles     0
```

## 如果指数不同呢？

所以当数据帧长度不同时，它也能工作。但是如果指数不同呢？索引是数据帧的关键部分，它基本上是一行的名称，以及我们需要获取数据时如何引用该行。**当两个数据帧之间的索引不同时(即使单元格中的实际内容相同)，则。eq 方法将它们视为不同的实体。**让我们用字母索引代替数字索引来创建一个新的数据框架:

```
**In:**
# Array with alphabetical index
df_4 = pd.DataFrame(array_3,
                    index=['a','b','c','d','e','f','g','h'],
                    columns=['Player','Rings'])
print(df_4)**Out:
**    Player Rings
a   LeBron     3
b     Kobe     3
c  Michael     6
d    Larry     5
e    Magic     5
f      Tim     4
g       KG     1
h  Charles     0
```

现在让我们试试。eq。输出是一个长而无用的数据帧。它的行数与我们的两个数据帧 df_1 (6 行)和 df_2 (8 行)的总和一样多。这样做是因为即使单元格内容相同，索引也不相同。等式假设两个数据帧之间没有什么是相同的。

```
**In:**
df_1.eq(df_4)**Out:**
   Player  Rings
0   False  False
1   False  False
2   False  False
3   False  False
4   False  False
5   False  False
a   False  False
b   False  False
c   False  False
d   False  False
e   False  False
f   False  False
g   False  False
h   False  False
```

让我们看看如何解决这个问题。

# 比较具有不同索引的数据帧

最简单的方法就是**重置**索引。在这种情况下，我们只需要重置 df_3 的索引(它将从字母回到从 0 开始的数字)。不要忘记删除索引，这样在重置后就不会出现额外的列。

正如你所看到的，由于重置了索引，输出回到了错误的条目(科比和拉里)以及新的条目(KG 和查尔斯)。

```
**In:**
*# Reset index first and drop the original index*
df_3_reset = df_3.reset_index(drop=True)*# Use boolean indexing and .ne method on reset index*
df_3_reset[df_1.ne(df_3_reset).any(axis=1)]**Out:**
    Player Rings
1     Kobe     3
3    Larry     5
6       KG     1
7  Charles     0
```

不算太糟吧？这种方法的一个问题是，它要求第二个数据帧的索引(post reset)与第一个数据帧的对齐。换句话说，df_1 和 df_3 中重叠的球员一定是相同的(而且顺序相同)，他们是——勒布朗、科比、迈克尔、拉里、魔术师、蒂姆。

但如果他们不是呢？相反，假设我们下载了一些新数据，我们希望将它们合并到我们的数据集中。不幸的是，新的数据与我们现有的数据有一些冗余，即勒布朗，迈克尔和魔术师。

```
*# New data to add to our dataset*
array_new = np.array([['LeBron',3],
                      ['Michael',6,],
                      ['Magic',5],
                      ['KG',1],
                      ['Charles',0],
                      ['Stephen',3],
                      ['Patrick',0]])
df_new = pd.DataFrame(array_new, 
                      columns=['Player','Rings'])
```

新数据如下所示。注意迈克尔的指数在 df_1 里是 2 但是在这里是 1，魔术师的指数在 df_1 里是 4 但是在这里是 2。

```
 Player Rings
0   LeBron     3
1  Michael     6
2    Magic     5
3       KG     1
4  Charles     0
5  Stephen     3
6  Patrick     0
```

让我们使用之前的方法来比较数据帧。eq 和. ne .首先让我们用。eq(和。all)来查看新数据和现有数据之间的相同之处:

```
**In:**
df_new[df_1.eq(df_new).all(axis=1)]**Out:
**   Player Rings
0  LeBron     3
```

它说只有勒布朗的条目是相同的，即使迈克尔和魔术师的数据也是一样的。正如我们已经知道的，问题在于不同的指数。如果我们使用。ne(和。any)来标识新的或不同的行，我们会得到一个很长的列表，其中包括我们不想放在那里的行—我们不想错误地将多余的 Michael 和 Magic 条目插入到我们的数据中。

```
**In:**
df_new[df_1.ne(df_new).any(axis=1)]**Out:
**    Player Rings
1  Michael     6
2    Magic     5
3       KG     1
4  Charles     0
5  Stephen     3
6  Patrick     0
```

## 使用合并

在这种情况下，最好的方法(据我所知)是使用**。合并**方法([我在这里写了一篇关于合并的文章，所以如果你需要背景知识，可以看看这篇文章。合并](/pandas-join-vs-merge-c365fd4fbf49)。这有点绕弯，但是**通过使用 merge，我们可以只比较每个条目的值，而不考虑索引。**

使用 Player 和 Rings 列进行合并允许我们匹配现有数据帧和新数据帧中具有相同值的行(同时忽略数据帧索引中的差异)。我们需要重命名新数据帧中的 Rings 列，使它作为两个独立的列输出(旧的 Rings 和新的 Rings_new)。一旦我们检查了输出，您就会明白为什么。

```
# Need to rename Rings since we are merging on it but we want
# it to show as different columns post-merge
temp = df_new.rename({'Rings': 'Rings_new'}, axis=1)merged = temp.merge(df_1, how='left', 
                    left_on=['Player','Rings_new'],
                    right_on=['Player','Rings'])
```

我已经打印了名为“合并”的数据框的内容。NaN 就是我们要找的东西(这也是为什么我们需要两栏都显示)。因为我们进行了左连接，所以输出包括 df_new 中的每一行——我们在 df_1 中已经有数据的球员在 Rings 列中有数值。**新玩家在“环”列中用 NaN 值表示，这是我们唯一想添加到数据集的玩家。**

```
 Player Rings_new Rings
0   LeBron         3     3
1  Michael         6     6
2    Magic         5     5
3       KG         1   NaN
4  Charles         0   NaN
5  Stephen         3   NaN
6  Patrick         0   NaN
```

我们可以通过对数据帧进行切片来分离出新的条目，只对环中具有 NaN 值的行进行切片:

```
**In:**
df_new[merged['Rings'].isna()]**Out:**
    Player Rings
3       KG     1
4  Charles     0
5  Stephen     3
6  Patrick     0
```

并像这样连接到我们的数据帧:

```
final_df = pd.concat([df_1,
                      df_new[merged['Rings'].isna()]],
                     axis=0)
```

最后，我们得到了我们想要的。final_df 的内容如下所示:

```
 Player Rings
0   LeBron     3
1     Kobe     5
2  Michael     6
3    Larry     3
4    Magic     5
5      Tim     4
3       KG     1
4  Charles     0
5  Stephen     3
6  Patrick     0
```

请注意，如果在新数据中有一个现有球员(如[LeBron，0])的错误条目，它也会被插入，因为我们合并了球员和戒指(因此，为了匹配条目，姓名和戒指数需要相同)。如果我们不想要这种行为，那么我们可以只在玩家列合并。这也可能引起问题，因为可能会有另一个同名但戒指数不同的球员(比如迈克尔·库帕有 5 枚戒指，而迈克尔·乔丹只有 6 枚)。当然，对于一个真实的数据集，我们将包括足够多的字段，以便我们可以唯一地识别每个球员(例如，名字，姓氏，出生日期)。

希望这是有见地的，祝你的数据框架好运！干杯！

如果你总体上喜欢这篇文章和我的写作，请考虑通过我的推荐链接注册 Medium 来支持我的写作。谢谢！