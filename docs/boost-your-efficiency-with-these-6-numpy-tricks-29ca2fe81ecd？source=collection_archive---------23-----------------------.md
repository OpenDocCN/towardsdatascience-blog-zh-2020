# 用这 6 个小窍门提高你的效率

> 原文：<https://towardsdatascience.com/boost-your-efficiency-with-these-6-numpy-tricks-29ca2fe81ecd?source=collection_archive---------23----------------------->

## 并控制您的阵列

# 动机

有时，当我向我的朋友展示我使用的一些数字方法时，他们会发现它们很有帮助。所以我决定分享这些数字方法(或技巧),希望你也会发现它们很有帮助。

![](img/81636a5c773e60713e230b352e6dc3e1.png)

照片由 [Pop &斑马](https://unsplash.com/@popnzebra?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

从导入 Numpy 开始

```
import numpy as np
```

# 面具

假设我们有一个数组，我们想在这个数组中选择一个特定的元素:0，2，并删除其他元素。有没有一种快速的方法可以选择这两个元素？口罩就可以了。

```
>>> A = np.array([0, 1, 2, 3, 4, 5])>>> mask = np.array([True, False, True, False, False, False])>>> A[mask]array([0, 2])
```

我们可以看到，标记为`False`的元素被删除，而标记为`True`的元素被保留。

# 随意

创建快速随机数组是测试代码的一种有用且快速的方法。但是有不同的`np.random`方法，你可能想要选择一种而不是另一种来用于特定的用途。

用 0 到 1 范围内的给定形状创建随机值

```
>>> np.random.rand(10,2)array([[0.38850622, 0.31431385],        
      [0.00815046, 0.13334727],        
      [0.47482606, 0.92837947],        
      [0.89809998, 0.38608183],        
      [0.25831955, 0.56582022],        
      [0.36625782, 0.52753452],        
      [0.88125428, 0.71624809],        
      [0.83642275, 0.79315897],        
      [0.27361664, 0.8250761 ],        
      [0.89894784, 0.95994016]])
```

或者创建特定范围内具有特定大小的随机整数数组

```
>>> np.random.randint(0,10,(3,3))array([[0, 9, 6],        
      [1, 7, 3],        
      [6, 4, 7]])
```

# 类形状阵列

你有没有发现自己创建了第二个数组，它的尺寸与第一个数组的尺寸相同，但元素不同？通常的做法是使用`shape`来捕捉第一个数组的维度

```
>>> A = np.random.randint(0,10,(3,3))>>> A.shape(3, 3)>>> B = np.zeros(A.shape)array([[0., 0., 0.],        
       [0., 0., 0.],        
       [0., 0., 0.]])
```

这种方法没有错。但是有一个更快的方法来创建一个类似的数组，用 0 或 1 作为元素，用`zeros_like`或`ones_like`

```
>>> A = np.random.randint(0,10,(3,3))>>> B = np.zeros_like(A)>>> Barray([[0., 0., 0.],        
       [0., 0., 0.],        
       [0., 0., 0.]])>>> B = np.ones_like(A)>>> Barray([[1, 1, 1],        
       [1, 1, 1],        
       [1, 1, 1]])
```

正如我们所见，数组 B 与数组 A 的维数相同，但元素不同。

# 使再成形

如果我们想改变一个数组的维数，`reshape`将是我们要使用的方法

```
>>> A = np.arange(9)>>> Aarray([0, 1, 2, 3, 4, 5, 6, 7, 8])#Reshape to 3x3 matrix>>> A.reshape((3,3))>>> Aarray([[0, 1, 2],        
       [3, 4, 5],        
       [6, 7, 8]])
```

或者展平

```
>>> A.reshape(-1)array([0, 1, 2, 3, 4, 5, 6, 7, 8])>>> A.flatten()array([0, 1, 2, 3, 4, 5, 6, 7, 8])
```

`reshape(-1)`或`flatten()`将完成展平阵列的工作。

# 变换超出范围数组

假设我们有一个范围从-3 到 6 的矩阵 B

```
>>> B = np.arange(-3,9).reshape((3,4))>>> Barray([[-3, -2, -1,  0],        
       [ 1,  2,  3,  4],        
       [ 5,  6,  7,  8]])
```

但是我们只想保持元素的范围从 0 到 5，并将超出范围的元素转换为范围内的元素。我们如何做到这一点？这时我们需要`np.clip`

```
>>> np.clip(B,0, 5)array([[0, 0, 0, 0],        
       [1, 2, 3, 4],        
       [5, 5, 5, 5]])
```

从变换后的矩阵可以看出，0 以下的元素变换为 0，5 以上的元素变换为 5，而范围内的元素保持不变。

# 洗牌

有时，我们希望使用随机数组，但我们的数组可能不是随机的。想想一副牌，当我们怀疑牌可能以特殊方式排序时，我们会怎么做？我们洗牌！

```
>>> A = np.arange(9).reshape((3,3))>>> Aarray([[0, 1, 2],        
       [3, 4, 5],        
       [6, 7, 8]])>>> np.random.shuffle(A)>>> Aarray([[3, 4, 5],        
       [0, 1, 2],        
       [6, 7, 8]])
```

正如我们所看到的，数组 A 中的元素与原始元素的位置不同。

# 结论

我希望这篇文章有助于为您的数据科学和编程实践添加一些新的 Numpy 技巧。拥有几个有用的方法来操作数组将极大地提高您的工作流程，并为您提供转换数组以满足特定需求的灵活性。

在 [this Github repo](https://github.com/khuyentran1401/Data-science/blob/master/python/Numpy_tricks.ipynb) 中，您可以随意使用本文的代码。

我喜欢写一些基本的数据科学概念，并尝试不同的算法和数据科学工具。你可以通过 [LinkedIn](https://www.linkedin.com/in/khuyen-tran-1401/) 和 [Twitter](https://twitter.com/KhuyenTran16) 与我联系。

如果你想查看我写的所有文章的代码，请点击这里。在 Medium 上关注我，了解我的最新数据科学文章，例如:

[](/dictionary-as-an-alternative-to-if-else-76fe57a1e4af) [## 字典作为 If-Else 的替代

### 使用字典创建一个更清晰的 If-Else 函数代码

towardsdatascience.com](/dictionary-as-an-alternative-to-if-else-76fe57a1e4af) [](/how-to-create-fake-data-with-faker-a835e5b7a9d9) [## 如何用 Faker 创建假数据

### 您可以收集数据或创建自己的数据

towardsdatascience.com](/how-to-create-fake-data-with-faker-a835e5b7a9d9) [](/python-tricks-for-keeping-track-of-your-data-aef3dc817a4e) [## 跟踪数据的 Python 技巧

### 如何用列表、字典计数器和命名元组来跟踪信息

towardsdatascience.com](/python-tricks-for-keeping-track-of-your-data-aef3dc817a4e) [](/timing-the-performance-to-choose-the-right-python-object-for-your-data-science-project-670db6f11b8e) [## 高效 Python 代码的计时

### 如何比较列表、集合和其他方法的性能

towardsdatascience.com](/timing-the-performance-to-choose-the-right-python-object-for-your-data-science-project-670db6f11b8e) [](/cython-a-speed-up-tool-for-your-python-function-9bab64364bfd) [## cy thon——Python 函数的加速工具

### 当调整你的算法得到小的改进时，你可能想用 Cython 获得额外的速度，一个…

towardsdatascience.com](/cython-a-speed-up-tool-for-your-python-function-9bab64364bfd)