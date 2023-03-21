# 如何创建自定义损失函数| Keras

> 原文：<https://towardsdatascience.com/how-to-create-a-custom-loss-function-keras-3a89156ec69b?source=collection_archive---------2----------------------->

## 对于神经网络

![](img/bc93d959eeeef8b09ea95559448622a0.png)

照片由[艾萨克·史密斯](https://unsplash.com/@isaacmsmith?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/graph?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

**损失**功能**是神经网络的重要组成部分之一。**损失**无非是神经网络的一个预测误差。计算损失的方法称为损失函数。**

损耗用于计算神经网络的梯度。并且梯度被用于更新权重。这就是神经网络的训练方式。

Keras 有许多内置的损失函数，我在之前的一篇 [**博客**](/understanding-different-loss-functions-for-neural-networks-dd1ed0274718) 中提到过。这些损失函数对于分类和回归等许多典型的机器学习任务来说已经足够了。

但是可能有一些任务我们需要实现一个自定义的损失函数，我将在这篇博客中介绍。

开始之前，让我们快速回顾一下如何在 Keras 中使用内置损失函数。

# 使用内置损失函数

下面是一个使用内置损失函数的简单例子。

克拉斯损失函数

这里我们使用内置的**分类 _ 交叉熵**损失函数，它主要用于分类任务。我们在`model.compile()`方法中传递损失函数的名称。

# 创建自定义损失函数

我们可以简单地如下创建一个定制的损失函数。

定制损失函数

构建自定义损失函数时，您必须遵循以下规则。

*   损失函数应该只有两个参数，即目标值`(y_true)`和预测值`(y_pred)`。因为为了测量预测中的误差(损失),我们需要这两个值。这些参数是在拟合数据时从模型本身传递过来的。
*   损失函数在计算损失时必须使用`y_pred`值，如果您不这样做，那么梯度表达式将不会被定义，您将得到一个错误。
*   然后你可以简单地将这个函数插入到`model.compile()`方法中来使用它。

## 小心数据维度

*   参数`y_true`和`y_pred`的第一维总是与批处理大小相同。**例如-** 如果您正在拟合批量为 32 的数据，并且您的神经网络有 5 个输出节点，那么`y_pred`的形状将是`(32, 5)`。因为会有 32 个输出，每个输出有 5 个值。
*   损失函数应该总是返回一个长度为`batch_size`的向量。因为你必须返回每个数据点的损失。**例如-** 如果您要拟合批量为 32 的数据，那么您需要从损失函数中返回一个长度为 32 的向量。

为了更好地理解，我们来看一个例子。

# 示例|自定义损失函数

比方说，你为某个回归任务设计了一个神经网络，它输出一个长度为 2 的向量`[x1, x2]`。

假设值`x2`比`x1`更重要，你希望它真正接近目标值。对于这种情况，您可以创建一个定制的 **MSE** (均方差)损失函数，该函数对`x2`的预测误差比对`x1`的预测误差更不利。

MSE 损失函数

这里我用权重 0.3 和 0.7 乘以损失值，给第二个值，也就是`x2`，更多的惩罚。您可以根据自己的需要决定重量。

我还写下了代码片段中变量的形状。您可以看到最终损失的形状是`(batch_size,)`，这意味着每个数据点有一个损失值。

现在你可以简单地将这个损失函数插入到你的模型中。

```
model.compile(loss=custom_mse, optimizer='adam')
```

## 注意

*   我建议您使用 Keras 后端函数，而不是 Numpy 函数，以避免任何意外。Keras 后端函数的工作方式几乎类似于 Numpy 函数。

# 奖金

现在，您已经知道如何构建自定义损失函数。您也可以以完全相似的方式创建定制的评估指标。这里，我创建了一个平均绝对误差(MAE)指标，并将其插入到`model.compile()`方法中。

MAE 度量

如果你想理解尺寸的概念和形状的概念。你可以阅读下面这篇文章，我会在其中详细解释这些概念。

[](/understanding-axes-and-dimensions-numpy-pandas-606407a5f950) [## 了解轴和维度| Numpy |熊猫

### 知道如何沿数据的不同轴应用函数。

towardsdatascience.com](/understanding-axes-and-dimensions-numpy-pandas-606407a5f950)