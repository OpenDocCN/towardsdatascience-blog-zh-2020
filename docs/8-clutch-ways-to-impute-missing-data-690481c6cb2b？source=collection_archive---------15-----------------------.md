# 8 种填补缺失数据的方法

> 原文：<https://towardsdatascience.com/8-clutch-ways-to-impute-missing-data-690481c6cb2b?source=collection_archive---------15----------------------->

![](img/9ac6a38e5e56e1cd390ba352d9de5a72.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Ehimetalor Unuabona](https://unsplash.com/@theeastlondonphotographer?utm_source=medium&utm_medium=referral) 拍摄的照片

## 如何处理任何数据集中缺失数据的教程

缺失数据可能是任何数据相关项目的瓶颈。虽然拥有高质量的数据总是值得赞赏的，但情况并非总是如此。有时你需要处理你拿到的任何一块半生不熟的蛋糕。因此，处理缺失数据对任何数据科学家/分析师都非常重要，因为这将使他们能够更准确地得出结果。

![](img/cc48693a5c5febf2d52f5c176c58d53f.png)

由[弗兰基·查马基](https://unsplash.com/@franki?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

“估算”一词指的是对我们丢失的任何数据进行统计估计。不同的数据类型有不同的插补技术。例如，您可能有**数字**数据，并且将应用以下插补技术:

*   使用数据的平均值/中值估算
*   使用任意值估算
*   使用尾部结束法估算。

如果您有**分类**数据，那么您可以执行以下操作:

*   使用模式估算数据
*   为缺失数据添加类别

如果您有混合的**数据，包含数值和分类值，那么您可以执行以下操作:**

*   完整的案例分析
*   为缺失值添加指示器
*   使用随机抽样方法估算数据

# 数字数据插补

![](img/1c33c726060f5cd34834eeef76d5abe1.png)

照片由[米卡·鲍梅斯特](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 平均值/中值

使用平均值/中值进行估算是最直观的方法之一，在某些情况下，它也可能是最有效的方法。我们基本上取数据的平均值，或者取数据的中位数，用那个值代替所有缺失的值。

*   假设:数据随机缺失；缺失的观察值看起来像大多数的观察值
*   优点:实现简单快捷；保留数据的丢失
*   缺点:协变和方差可能发生变化；丢失的数据越多，失真就越高

## 任意值方法

![](img/484b5c6a7d5e2f680855509905eb7743.png)

照片由[戴维斯科](https://unsplash.com/@codytdavis?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

这里，目的是标记数据集中缺失的值。您可以用一个固定的任意值(随机值)来估算缺失的数据。

它主要用于分类变量，但也可用于具有任意值的数值变量，如 0，999 或其他类似的数字组合。

*   假设:数据不是随机缺失的
*   优点:快速且易于实现；揭示缺失价值的潜在重要性
*   缺点:改变共方差/方差；可能会产生异常值

## 尾部结束法

![](img/0699f0ae343ecaa63da1253bddf11eea.png)

照片由[克里斯蒂安·凯恩德尔](https://unsplash.com/@christiankaindl?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

这种方法类似于任意值方法，但是，这里的任意值是在变量的基本分布的尾端选择的。

如果正态分布，我们使用平均值+/- 3 倍标准差。

如果分布是偏斜的，使用 IQR 邻近规则。

*   假设:数据不是随机缺失的；数据在尾端是倾斜的
*   优点:能带出缺失值的重要性；
*   缺点:改变共方差/方差；可能会产生有偏差的数据。

# 分类数据插补

![](img/b3a16cca66881e834a3c0e5704fd14f4.png)

由 [CJ Dayrit](https://unsplash.com/@cjred?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 方式

顾名思义，您用最常出现的值来估算缺失数据。这种方法最适合分类数据，因为缺失值最有可能是出现频率最高的值。

*   假设:数据随机缺失；缺失值看起来像多数
*   优点:快速且易于实现；适用于分类数据
*   缺点:可能创建有偏差的数据集，偏向最频繁的值

## 添加缺失数据的类别

下一个方法非常简单，只适用于分类数据。您可以为缺失值创建一个单独的标签——“缺失”或者任何相关的内容。这个想法是标记缺失的价值观，并理解缺失的重要性。

*   假设:无假设
*   优点:快速且易于实现；帮助理解缺失数据的重要性
*   缺点:可能被误解的数据；缺失数据的数量应该足够大

# 混合数据插补

![](img/0babd3c2619a07aeec46e1de3bc9d007.png)

布鲁克·拉克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

## 完整的案例分析

在这种方法中，我们基本上放弃了缺少值的选项。我们只考虑所有值都存在的数据行。顾名思义，是只对完整案例的评估。这对混合数据集很有用。

*   假设:数据在随机点缺失；没有模式的证据
*   优点:没有数据操纵；保持数据的分布
*   缺点:数据丢失；如果数据不是随机丢失的，可能会创建有偏差的数据集

## 为缺失值添加指示器

![](img/c18d3152e4667519cf0dac289689dfd1.png)

斯文·舍尔梅尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在这种方法中，您基本上会创建一个新列来指示数据丢失的位置。这将是一个二进制值(1 或 0 ),表示丢失或未丢失。

您可以将此方法与其他插补方法结合使用，以便更好地理解缺失数据。

*   假设:数据不是随机缺失的；数据是可预测的
*   优点:易于实现；抓住缺失数据的本质
*   缺点:创建另一个列/特征；仍然需要估算

## 随机抽样方法

该方法包括从变量的数据池中随机抽取样本，并使用随机抽取的值来估算缺失值。

*   假设:数据随机缺失；数据呈正态分布
*   优点:快速且易于实现；保留方差
*   缺点:可能产生随机数据；当丢失数据占数据的百分比很高时不起作用；

![](img/03f820c048a2f936aa27b1511d4957b9.png)

照片由 [magnezis magnestic](https://unsplash.com/@agneska?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

> 感谢阅读。希望你学到了新东西。更多类似内容关注我。