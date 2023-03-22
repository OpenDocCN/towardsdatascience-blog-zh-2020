# 定制 PySpark 累加器

> 原文：<https://towardsdatascience.com/custom-pyspark-accumulators-310f63ca3c8c?source=collection_archive---------30----------------------->

![](img/c03f854a08c44d5cc6125c0cd58b11ff.png)

约书亚·索蒂诺在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## pyspark 蓄能器的字典、列表和设置类型

默认情况下，Spark 提供支持交换和关联操作的 int/float 累加器。尽管 spark 也提供了一个类 *AccumulatorParam* 来继承，以支持不同类型的累加器。只需要实现两个方法 *zero* 和 *addInPlace。zero* 定义累加器类型的零值，addInPlace 定义累加器类型的两个值如何相加。

在上一篇文章中，我讨论了管理 pyspark 累加器，这也提供了一个很好的累加器概述:

[](/broadcasting-pyspark-accumulators-343104c18c44) [## 广播 PySpark 累加器

### 以及如何管理它们

towardsdatascience.com](/broadcasting-pyspark-accumulators-343104c18c44) 

在这篇文章中，我将讨论三种不同类型的自定义累加器:dict、list 和 set。

# 口述积累器

字典累加器的目标是累加字典。现在，当我们积累字典时，有多种考虑因素:

1.  现有键:如果键已经存在于累积字典中，我们该怎么办？
2.  Key 的值类型:如何处理 list 和 set 等键的不同类型的值？

## 现有密钥

我们至少有三种策略:

*   用新值替换该项的值
*   保持旧值
*   添加到现有值

注意，前两个操作是不可交换的，因为它取决于要累加的字典的顺序。如果我们想为一个给定的键保留所有的或者唯一的值，那么我们可以分别用一个列表或者集合来表示值。

## 键的值类型

如上所述，值的类型对于跟踪给定键的所有或所有唯一值可能是有用的。在这种情况下，我们需要一个列表或集合。可能有其他场景需要其他类型，但这超出了本文的范围。

所以现在我们已经讨论了五种不同风格的录音机。我们没有为每个实现一个类，而是实现了*枚举*来跟踪组合方法。

## 白藓积累法

*   REPLACE:如果键存在，则用新值替换当前值，否则将键值添加到字典中
*   KEEP:如果键存在，则保留旧值，否则将键值添加到字典中
*   ADD:如果键存在，则向现有值添加新值，否则向字典添加键值
*   LIST:如果键存在，将新列表添加到现有列表中，否则将键和列表作为值添加到字典中
*   SET:如果关键字存在，则将新集合(从新列表转换)与现有集合值(从现有集合转换为值)联合，否则将关键字和列表值添加到字典中

## __init__ 拯救世界

现在我们知道了不同风格的 DictAccumulator，那么如何使用一个单独的类实现来创建呢？很少宣传的事实是用于 *AccumulatorParam，*的 *__init__* 方法，但是它不需要被指定，首先，因为它是 python 中的类的构造函数。

用 DictAccumulatorMethod 初始化 DictAccumulator 解决了给它调味的问题。除非指定，否则将使用 REPLACE。在 *addInPlace* 方法中，我们只是用需要合并到 first ( *v1* )中的 seconds ( *v2* )字典中的键和值来调用方法。

## 单元测试指令累积器

首先只是设置单元测试。将累加器管理器设置为[广播累加器](/broadcasting-pyspark-accumulators-343104c18c44)，通过测试设置中的 *get_spark* 方法和 *input_data* 、 *rdd* 产生火花。

**更换**

在这个单元测试中，我们首先为作为字典的键添加双倍的键值，然后添加相同的键值。相同的密钥值会替换双倍的密钥值。

**保持**

首先在下面的测试中，我们将字典添加到累加器中，其中值是键的两倍。然后将字典添加到值与键相同的累加器中。在第二遍之后，键值应该是键值的两倍，因为我们将累加器初始化为 keep。

**增加**

在下面的测试中，我们只是将所有的值添加到定义为“ *count* ”的键中。*count*键的值与 input_data 的 *sum* 相同。

**列表**

在此测试中，我们将所有奇数和偶数数据组合到其各自的密钥中，该密钥是从数字除以 2 的余数中导出的。每个值都被附加到相应键的列表中。

**设置**

在下面的测试中，残差被添加到键中。因为它的设置只有值 0 和 1 在列表中(键值)。如果是列表，我们会看到多个 0 和 1。

# 列表累加器

ListAccumulator 的实现相当简单。在这个场景中，我们只添加两个列表，并在 *addInPlace* 方法中返回它。

## **单元测试列表累加器**

在单元测试中，我只是在加倍后将一个数字列表添加到 ListAccumulator 中。我在驱动程序上创建了相同的数据并进行比较，结果是匹配的。

# 设定累加器

在 SetAccumulator 实现中，它只是两个集合的并集，作为 *addInPlace* 方法的返回。

## **单元测试设置累加器**

测试方法与测试过的带 set 类型的录音机相似。多次添加剩余的列表，最后，我们只得到两个剩余的 0 和 1，而不是 0 和 1。

# 结论

在这篇文章中，讨论了基于列表和集合的 pyspark 累加器，并围绕实现进行了推理。替换和保留字典的累加器是不可交换的，所以使用它们时要小心。下面是 GitHub 上的实现。

[](https://github.com/SalilJain/PySparkCustomAccumulators) [## salil Jain/PySparkCustomAccumulators

### 在 GitHub 上创建一个帐户，为 salil Jain/PySparkCustomAccumulators 的开发做出贡献。

github.com](https://github.com/SalilJain/PySparkCustomAccumulators)