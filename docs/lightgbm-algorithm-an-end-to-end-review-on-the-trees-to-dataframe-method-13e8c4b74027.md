# LightGBM 算法:树到数据帧方法的端到端评述

> 原文：<https://towardsdatascience.com/lightgbm-algorithm-an-end-to-end-review-on-the-trees-to-dataframe-method-13e8c4b74027?source=collection_archive---------11----------------------->

![](img/6889ce19590b8d786737d3ce6d90f4e5.png)

[由 upklyak / Freepik 设计](http://www.freepik.com)

LightGBM 最近获得了一种称为“trees_to_dataframe”的新方法，它允许您将构成 lightGBM 模型的多个树估计器转换为可读的 pandas 数据框架。该方法增加了模型的可解释性，并允许您在粒度级别上理解影响任何数据点预测的划分。在本文中，我将构建一个简单的模型，并向您展示“trees_to_dataframe”的输出，以帮助您理解如何解释日常使用的数据帧。

# **什么是 LightGBM:**

对于本文，我将使用以下学术论文作为参考:[https://papers . nips . cc/paper/6907-light GBM-a-highly-efficient-gradient-boosting-decision-tree](https://papers.nips.cc/paper/6907-lightgbm-a-highly-efficient-gradient-boosting-decision-tree)。

Light GBM 是一种梯度增强集成方法，其特征在于:

1.  **单侧采样(GOSS)** :仅使用具有较大梯度的数据点来估计信息增益(使用较少的观测值获得信息增益的精确估计。这使得它更轻——因此它的名字是:LightGBM。**术语预警！我们将梯度定义为我们试图最小化的损失函数的负导数。**
2.  **排他性特征捆绑(EFB):** 将很少出现非零值的特征捆绑在一起，用于相同的观察(例如:独热编码)。
3.  **基于直方图的分割:**使用更有效的基于直方图的宁滨策略来寻找数据中的分割点。这也使得它比常规的梯度推进算法更快。

# 练习:

为了深入研究 LightGBM 使用树来数据化框架的可解释性，我将使用移动价格分类 Kaggle 数据集。我们的模型将被训练来预测手机的价格范围。

原始数据集的目标变量用数字编码成从零(0)到三(3)的 4 个类别。因为这个练习的目的不是预测，而是理解如何解释 LightGBM 的“trees_to_dataframe”方法，所以我将简化我们的练习，把目标变量分成二进制类别，而不是多类。从价格范围零(0)到价格范围一(1)，我将编码为 0，从价格范围二(2)到三(3)，我将编码为 1。我也不会把数据分成训练和测试。

该数据集包含以下功能:“电池电量”、“蓝色”、“时钟速度”、“双 sim”、“fc”、“四 g”、“int_memory”、“m_dep”、“mobile_wt”、“n_cores”、“pc”、“px_height”、“px_width”、“ram”、“sc_h”、“sc_w”、“talk_time”、“three_g”、“touch_screen”和“wifi”。

为了充分理解树到数据帧的方法，我将放弃使用测试数据。我将训练一个特意简单的模型，只包含三个估计器，最大深度为 3。在 Kaggle 笔记本内核中，我开始安装最新的 LightGBM 版本，因为“trees_to_dataframe”方法最近被集成到算法的源代码中。

使用下面的代码，我创建了一个训练数据集，我将使用它来生成我将要分析的 lightGBM 模型。

这里有木星的代码

从 basic_model.trees_to_dataframe()中，我们获得了一个包含以下各列的 pandas 数据帧:' tree_index '，' node_depth '，' node_index '，' left_child '，' right_child '，' parent_index '，' split_feature '，' split_gain '，' threshold '，
' decision _ type '，' missing_direction '，' missing_type '，' value '，' weight '，
' count '。我们将探究这些分别代表什么。

# **图 1:获得的数据帧的第一行。**

# **图 2:算法的第一棵树(估计器)。**

# 树索引:

树索引是指我们正在查看的树。在这种情况下，我们看到的是索引为 0 的树。第一个估计量。

# 节点深度:

指示分区发生的级别。例如，第一行引用节点深度 1，这是第一个分区(节点索引 0-S0)。

# 节点索引，左子节点和右子节点，父节点:

指示节点的索引及其“子节点”的索引。换句话说，当前节点分区到的要创建的节点。父节点是指当前节点所在的节点。因为 0-S0 是第一个分区，所以它没有父节点。

# 分割特征:

该功能对节点进行分区以创建子节点或叶。

# 分割增益:

通过以下方式测量分割质量。

# 阈值:

用于决定观察是前进到节点的左侧子节点还是右侧子节点的特征值。

# 缺少方向:

指示缺少的值转到哪个子节点(基于决策类型)。

# 价值:

这些是粗略的预测。详情请参见附录示例 A.1。

# 重量

用于分割增益计算。

# 计数:

计数是落在节点内的观察值的数量。

# 附录 A.1:

让我们遵循下面的观察:

我们的 LightGBM 算法有三个估计器:

在估计值 0 中，我们的观察结果落在叶子 1: ram 为 2549，battery_power 为 842。第一个叶的叶值是 0.496250。

在估计器 1 中，观察值落在叶子 7 中:ram 为 2549，battery_power 为 842。估计量 1 中叶 7 的值是 0.044437。

在估计器 2 中，观察值落在叶 1 中:ram 为 2549，battery_poer 为 842。叶 1 的值是-0.001562。

我们将数据点所在的不同叶的值相加:

0.49625 + 0.044437 — 0.001562 = .539125

总之，新推出的 lightGBM“trees _ to _ data frame”方法通过将 light GBM 模型转换为 pandas 数据框架，成为一个可解释的工具。通过允许您在较低的级别读取多个分区标准，以及每个数据点所属的叶值，该方法增加了透明度，并有助于更好地理解影响任何数据点预测的决策。

[](https://jovian.ai/anaprec07/notebooke133a3e568-1) [## ana prec 07/notebook 133 a3 e 568-1-Jovian

### 在 anaprec07/notebook 133 a3 e 568-1 笔记本上与 ana prec 07 协作。

jovian.ai](https://jovian.ai/anaprec07/notebooke133a3e568-1) 

# 关于我:

[](https://www.linkedin.com/in/anapreciado/) [## Ana Preciado -数据科学家-COVIDPTY.com | LinkedIn

### 联系人:anamargaritapreciado@gmail.com |+(507)61305543。自从我做了电子商务的本科研究后，我…

www.linkedin.com](https://www.linkedin.com/in/anapreciado/)