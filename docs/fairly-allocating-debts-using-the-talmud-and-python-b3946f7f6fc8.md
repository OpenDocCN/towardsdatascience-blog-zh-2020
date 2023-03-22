# 使用犹太法典和 Python 公平分配债务

> 原文：<https://towardsdatascience.com/fairly-allocating-debts-using-the-talmud-and-python-b3946f7f6fc8?source=collection_archive---------38----------------------->

## 用博弈论分割遗产的古老方法

![](img/beab1c4c136f81b914b24752d8dac95d.png)

坦纳·马迪斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

根据 Experian 的一项研究，2016 年 10 月至 12 月间死亡的 73%的美国人都有一些未偿还的债务[【1】](https://www.credit.com/blog/americans-are-dying-with-an-average-of-62k-of-debt-168045/)。不幸的是，负债而死对许多人来说是一种新常态，我预计这一比例在未来还会增加。尽管逃了税，但当你死后，债务不会简单地消失。未偿还的债务通常会通过你的遗产来收回，除非你使用退休账户或人寿保险来保护自己免受债权人的伤害。如果你的遗产不能偿还所有债务怎么办？你的债务如何公平分配？

## 债务分配方法

*   平均分配

按照债权人的数量平均分配遗产

```
Estate = 100
Creditors = [100, 200, 300]## Equal DivisionPayments = {'0_100': 33.33, '1_200': 33.33, '2_300': 33.33}
```

在这种情况下，每个债权人得到 33 1/3 美元。

*   比例除法

比例分割将更多的财产判给最高的债权人

```
Estate = 300
Creditors = [100, 200, 300]## Equal DivisionPayments = {'0_100': 50, '1_200': 100, '2_300': 150}
```

在这种情况下，每个债权人将获得其要求金额的一半，最大的债权人将获得最大的一笔金额。

*   犹太法典部

直到 1985 年，Robmann Aumann 和 Michael Maschler 编写了一种算法来解决下面的奇怪除法，这种方法仍然是一个数学之谜:

```
Estate = 200
Creditors = [100, 200, 300]## Equal DivisionPayments = {'0_100': 50, '1_200': 75, '2_300': 75}
```

拆分既不是等额拆分，也不是比例拆分。分割实际上是基于竞争金额的**等分。**

## 该算法

下面的五个步骤详细说明了由 Aumann 和 Maschler 提出的有争议的和算法的等分。

1.  按从低到高的顺序排列债权人
2.  在所有债权人之间平均分配财产，直到最低的债权人得到一半的债权
3.  删除最低的债权人，重复步骤 2，直到所有债权人拥有一半的索赔权或产业是空的
4.  反向操作:从遗产中给予最高债权人，直到损失(分配和索赔之间的差额)等于次高债权人的损失。
5.  重复第 4 步，直到所有的钱都分配完

给定算法后，我想创建一个函数来实现一个有争议的和的等分。我写了带有债务函数的 talmud _ debts 包来完成这个任务。还有一个带有 jupyter 笔记本实现的 [GitHub](https://github.com/Datadolittle/Talmud_Debts) 。

```
pip install talmud_debts
```

这将安装带有债务功能的软件包。要运行该函数，请导入 talmud _ debts 并在 python 中调用 debits 函数。预期的参数是遗产大小(整数)和要支付的债权人数组。

```
from talmud_debts import debtsEstate = 200
Creditors = [100,200, 300]debts(Estate, Creditors)
```

输出是一个有序的字典，债权人由一个索引定义，然后是债务金额。0_100 表示最低债权人的预期支出。

## Python 中算法的伪代码

**步骤 1–3**

第一步是对数组进行排序，使债务从索引 0 处的最低值到索引 n 处的最高值。我生成了一些帮助器函数来为字典值添加一个或一个浮点数。我使用了 for 和 while 循环 O(N**2)来分配所有债权人，直到至少一半的债务被给予最低的债权人。我跳出了循环，用值更新了第二个字典，然后删除了最低的债权人。

**步骤 4–5**

另一个 while 循环，for 循环组合用于获得债权人的最新损失。如果破产财产少于最高债权人的损失与次高债权人的损失之间的差额，则将剩余部分分配给损失最大的债权人。否则，在具有最大损失的债权人匹配下一个最大损失之后，循环重复。

总的来说，这个算法实现既有趣又有挑战性。实现这个算法的想法来自 Presh Talwalkar 的书 [*《博弈论的快乐》*](https://www.amazon.com/Joy-Game-Theory-Introduction-Strategic/dp/1500497444) 。该算法适用于任何数量的债权人和任何规模的财产。如果遗产规模大于债权人的总和，该函数将打印一个声明，说明所有债务都可以支付。我可以在 LinkedIn 上找到我，我希望我们能像兰尼斯特家一样，永远偿还我们的债务。

![](img/45f3bfea3a8882cdffb0a44a9e646025.png)

[1][https://www . credit . com/blog/Americans-are-dead-in-a-average-62k-of-debt-168045/](https://www.credit.com/blog/americans-are-dying-with-an-average-of-62k-of-debt-168045/)

[2][https://www . MSN . com/en-us/money/personal finance/on-average-Americans-die-with-dollar 61000-in-debt-who-pays/ar-BBMBgqM](https://www.msn.com/en-us/money/personalfinance/on-average-americans-die-with-dollar61000-in-debt-who-pays/ar-BBMBgqM)

[3]http://www.cs.cmu.edu/~arielpro/15896s15/docs/paper8.pdf