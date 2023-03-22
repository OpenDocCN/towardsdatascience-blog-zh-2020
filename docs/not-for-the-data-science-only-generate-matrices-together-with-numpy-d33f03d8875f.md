# 与 NumPy 一起生成矩阵

> 原文：<https://towardsdatascience.com/not-for-the-data-science-only-generate-matrices-together-with-numpy-d33f03d8875f?source=collection_archive---------18----------------------->

## Python 技巧

## 创建 2d 阵列的 NumPy 函数的综合列表

![](img/f8fd7f3c6aedaae7e4c761c155097acf.png)

Anton Vietrov 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

NumPy 位于最初与数据科学相关联的 Python 库中。但是，很明显，它是一种处理高维数组、矩阵和表的通用工具。虽然应用程序的数量太大，无法在一篇文章中描述，但我想关注一个非常有用的特性:2d 数组生成。你可能会想，我们为什么要为此烦恼呢？因此，我会提醒我们在每个项目中产生的数百个微小的操作，而没有注意到它们:默认变量的值生成、单元测试的数据创建、中间结果的准备、转换矩阵、“哑”值，等等..准备这些值需要一些时间，在项目开发结束之前，你会意识到它们的必要性。

# 1.顺序

你可能会感到惊讶，因为我已经把二维数组作为本文的主题。虽然数组到矩阵的转换非常普遍，所以这是有意义的。让我们来探索一对 NumPy 原生的把戏。

例如，对于序列:

重塑它:

增加维度:

由于我们有将数组转换为矩阵的工具，我们可以将其应用于传统的`np.arange()`序列发生器，例如:

```
np.arange(1, 12, 2).reshape(2, 3)
```

# 2.随机数

当然，我们不能忽略随机序列，因为它们在测试中被过度使用。Numpy [基于不同的公式提供了大量不同的随机分布](https://docs.scipy.org/doc/numpy-1.15.0/reference/routines.random.html)。虽然我们将回顾创建随机矩阵的最常见的方法，但所有其他方法也可以类似地使用:

`np.random.random([3,3])` —样本来自均匀`[0,1)`分布的 3x3 矩阵

`np.random.randn(3, 3)` —3×3 矩阵，样本来自均值为 0、方差为 1 的单变量“正态”(高斯)分布

`np.random.randint(0, 2, size=(3, 3))` —3×3 矩阵，随机整数来自`[0,2]`区间。

# 3.分布

我们再次使用数组，但是我们不能错过这样重要的函数，它在指定的时间间隔内产生连续的数据分布。

*   `np.linspace()` —线性分布(等步长)

*   `np. logspace()` —对数分布

*   `np.geomspace()` —几何分布(有相等因子)

# 4.阵列复制

这是填充函数的一个很好的例子，它接受一个数组，并将其复制几次以获得一个矩阵。我们必须设置行数和一行中的重复次数(按照这个顺序)。

# 5.对角矩阵

对于矩阵的中间运算，我们可能需要对角运算。也是线性代数中的基本元素之一。不足为奇的是，NumPy 为它们的创建提供了几个函数。

*   来自数组

*   带 1 的方阵

*   具有自定义对角线的自定义大小的 1s 矩阵

# 6.np.zeros()

它创建了一个完全由 0 填充的矩阵。它是变量默认值或哑返回值的完美候选。

您也可以使用`np.zeros_like()`创建一个自定义大小的 0 矩阵。

# 7.np.ones()

默认值矩阵的另一个例子，这次用 1 填充。

同样，你可以使用另一个数组作为`np.ones_like()`的模板。

# 8.np.full()

前面的函数使用定义的填充值创建矩阵。但是 NumPy 还提供了一个自定义填充的功能。

# 9.三角形矩阵

线性代数运算的另一个重要元素:矩阵 a 的一半用 0 填充。NumPy 对三角形矩阵有三个主要函数:

*   `np.tri()` —创建一个下部填充的三角形 1s 矩阵
*   `np.tril()` —从自定义数组创建一个三角形矩阵，其下半部分被填充
*   `np.triu()` —从一个自定义数组创建一个三角形矩阵，填充上半部分

# 10 完全自定义矩阵

NumPy 函数山的顶峰是 np.fromfunction()。它根据用户定义的函数创建一个自定义大小的矩阵。函数应该接受与矩阵维数相等的参数个数，如果需要，还可以接受其他值。生成将从索引`(0,0)`开始，每个坐标的步长为 1。

你可以在我的 GitHub 上找到一个完整的带有示例的笔记本:

[](https://github.com/Midvel/medium_jupyter_notes/blob/master/matrices_generation/generate-matrices.ipynb) [## 中级/中等 _jupyter_notes

### permalink dissolve GitHub 是 4000 多万开发人员的家园，他们一起工作来托管和审查代码，管理…

github.com](https://github.com/Midvel/medium_jupyter_notes/blob/master/matrices_generation/generate-matrices.ipynb) 

NumPy 库是任何线性代数运算的完美工具。它充满了有趣的功能，可以创造性地使用。你可以自由分享你最喜欢的 NumPy 技巧。