# 使用 Numpy 数组:索引

> 原文：<https://towardsdatascience.com/working-with-numpy-arrays-indexing-e4c08595ed57?source=collection_archive---------68----------------------->

## [PyTrix 系列](https://towardsdatascience.com/tagged/pytrix-series)

## PyTrix#2:访问 Numpy 数组中的元素

![](img/913d3de15a015db401979ebea62d3d6e.png)

照片由[阿丽莎·基比洛斯基](https://unsplash.com/@arkibbles?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

PyTrix 现在将成为一个每周一期的系列，在这里我将展示可以用 Python 完成的对数据科学家有用的很酷的东西。上一个 PyTrix 教程可以在下面的链接中找到…

[](/vectorization-in-python-46486819d3a) [## Python 中的矢量化

### PyTrix#1:加速我们的 Python 代码

towardsdatascience.com](/vectorization-in-python-46486819d3a) 

下面是 Github 知识库的链接:

[](https://github.com/kurtispykes/demo/tree/master/pytrix) [## kurtispykes/演示

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/kurtispykes/demo/tree/master/pytrix) 

## 什么是 Numpy 数组？

首先，我们从解释 Numpy 开始。Numpy 是用于科学编程的事实上的库，在从业者中使用如此普遍，以至于当我们导入它时，它有自己的标准。

Numpy 框架为我们提供了一个高性能的多维数组对象，以及操作数组的有用工具。关于这一点，我们可以将 numpy 数组描述为一个相同类型值的网格，它通过一个非负整数元组进行索引。

我们通过数组的秩来区分维数。此外，数组的形状是一个整数元组，表示数组沿其每个维度的大小。

> 注:如果你不熟悉排名术语，在下面的链接中，史蒂文·斯坦克有一个精彩的解释。

[](https://medium.com/@quantumsteinke/whats-the-difference-between-a-matrix-and-a-tensor-4505fbdc576c) [## 矩阵和张量有什么区别？

### 这个问题有个简短的答案，就从这里开始吧。然后，我们可以看一看应用程序，以获得…

medium.com](https://medium.com/@quantumsteinke/whats-the-difference-between-a-matrix-and-a-tensor-4505fbdc576c) 

然而，今天的 PyTrix 系列是关于索引数组的，所以不再多说，让我们开始吧！

# 索引

从多维数组中提取单个元素、行、列或平面的过程称为索引。

在很多情况下，我们可能希望在数据科学项目中从数组中获得一些值。根据矩阵的维度(或等级),我们必须采用不同的策略来有效地执行这项任务。

对于那些想了解更多关于`np.array()`类的人，文档可以在[这里找到](https://numpy.org/doc/1.18/reference/generated/numpy.array.html)！

# 一维索引

在第一个例子中，我将初始化一个一维 numpy 数组并打印它:

> 注意:假设上一节中 numpy 的导入已经完成

```
numbers= [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
a= np.array(numbers)**print**(a)
>>>> [ 1  2  3  4  5  6  7  8  9 10]
```

为了访问数组中的元素，我们遵循与普通 python 列表或 tuple 数据类型完全相同的索引约定:

```
**print**(f"Index 0: {a[0]}\nIndex 5: {a[5]}\nIndex 7: {a[7]}")
*>>>>* Index 0: 1
     Index 5: 6
     Index 7: 8
```

# 二维索引

接下来，我们将从一个列表实例化一个二维 numpy 数组。我们已经有一个初始化为`numbers`的列表，我们必须再创建一个。

```
big_numbers= [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
b= np.array([numbers, big_numbers])
**print**(b)
>>>> [[  1   2   3   4   5   6   7   8   9  10]
      [ 10  20  30  40  50  60  70  80  90 100]]
```

如果我们想从二维数组中提取一个元素，我们必须首先选择索引， **i，**，它是指向矩阵行的指针，然后选择索引， **j** ，来选择列。

```
**print**(b[1, 4]) 
>>>> 50
```

这表示我们从数组中选择了第 1 行和第 4 列。

# 选择行或列向量

我知道我的读者是非常好奇的人。因此，你可能想知道当你把索引传递给我们的列表时会发生什么…

```
**print**(b[0]) 
>>>> [ 1  2  3  4  5  6  7  8  9 10]
```

这将返回索引为 0 的行向量。秩为 1 的数组。如果我们想访问一个列向量呢？

```
**print**(b[:, 8])
>>>> [ 9 90]
```

`:`告诉 python 我们应该获取每一行，我们用“，”和 8 将它分开，告诉 python 只从第 8 列获取行。然而，我们有点超前了，因为这关系到切片，我们将在下周讨论 PyTrix:)。

> 注意:当我们对 numpy 数组进行索引或切片时，相同的数据作为原始数组的视图返回，但是按照我们从索引或切片中声明的顺序访问。

# 三维索引

如果一个二维数组可以用一个列表实例化，那么…你猜对了。一个三维数组是用一个列表列表实例化的——花一点时间来理解它。

```
c= np.array([[[1, 2, 3], [4, 5, 6], [7, 8, 9]],
             [[10, 11, 12], [13, 14, 15], [16, 17, 18]],
             [[19, 20, 21], [22, 23, 24], [25, 26, 27]]])print(c)
>>>> [[[ 1  2  3]
       [ 4  5  6]
       [ 7  8  9]]

      [[10 11 12]
       [13 14 15]
       [16 17 18]]

      [[19 20 21]
       [22 23 24]
       [25 26 27]]]
```

我们可以把一个三维数组想象成一堆矩阵，其中第一个索引 **i** 选择矩阵。第二个索引**j**选择行，第三个索引 **k** 选择列。

例如，我们将选择第一个矩阵:

```
print(c[0])
>>>> [[1 2 3]
      [4 5 6]
      [7 8 9]]
```

如果我们想选择数组中的单个元素，可以这样做:

```
print(c[2, 1, 1])
>>>> 23
```

为了解释上面的代码，我们打印了 matrix 中索引 2、行索引 1 和列索引 1 处的三维数组。

# 选择三维数组中的行或列

对于三维数组，我将把对行或列的访问分成三种情况。

***场景 1*** *:* 当我们只指定矩阵时， **i** and row， **j** ，这将依次从我们选择的矩阵中返回一个特定的行。

```
**print**(c[0, 0]) 
>>>> [1 2 3]
```

这将返回索引为 0 的矩阵和索引为 0 的行。

***场景 2*** *:* 当我们想要访问一个特定矩阵的列元素时，我们用`:`填充第 j 个索引来表示我们想要一个完整的切片(所有的行)。

```
**print**(c[1, :, 2])
>>>> [12 15 18]
```

这将返回矩阵 1 中位于列索引 2 中的所有行。

**场景 3** :有时，我们可能希望为每个索引访问同一行和同一列中的值。

```
**print**(c[:, 2, 0])
>>>> [ 7 16 25]
```

这是每个矩阵的行索引 2 和列索引 0 中的每个元素的列表。

## 结论

索引有许多选项，学习它们是一组非常有用的技能，可以放在您的数据科学工具包中。关于索引的更多信息，你可以阅读 numpy [文档](https://numpy.org/devdocs/user/basics.indexing.html)，它深入研究了索引。

参见下面的链接，获取本文中使用的代码。

[](https://github.com/kurtispykes/demo/blob/master/pytrix/pytrix_working-with-arrays.ipynb) [## kurtispykes/演示

### permalink dissolve GitHub 是超过 5000 万开发人员的家园，他们一起工作来托管和审查代码，管理…

github.com](https://github.com/kurtispykes/demo/blob/master/pytrix/pytrix_working-with-arrays.ipynb) 

## 一锤定音

如果您认为我遗漏了什么，或者您想向我指出什么，或者如果您仍然不确定什么，您的反馈是有价值的。发个回应！

然而，如果你想和我联系，我在 LinkedIn 上是最活跃的，我也很乐意和你联系。

[](https://www.linkedin.com/in/kurtispykes/) [## 人工智能博主 kurtis Pykes——走向数据科学| LinkedIn

### 在世界上最大的职业社区 LinkedIn 上查看 Kurtis Pykes 的个人资料。Kurtis 有两个工作列在他们的…

www.linkedin.com](https://www.linkedin.com/in/kurtispykes/) 

以下是我最近的一些作品，你可能也会感兴趣:

[](/the-reason-youre-frustrated-when-trying-to-become-a-data-scientist-2d2b8b402811) [## 当你试图成为一名数据科学家时感到沮丧的原因

### 将最优秀的人与众不同的隐藏技能

towardsdatascience.com](/the-reason-youre-frustrated-when-trying-to-become-a-data-scientist-2d2b8b402811) [](/3-stages-of-learning-data-science-9a04e96ba415) [## 学习数据科学的 3 个阶段

### 了解学习的 3 个阶段，以及我们如何将其有效地应用于数据科学学习

towardsdatascience.com](/3-stages-of-learning-data-science-9a04e96ba415) [](/forecasting-with-stochastic-models-abf2e85c9679) [## 用随机模型预测

### 使用 ARIMA 模型预测沃尔玛的销售数据

towardsdatascience.com](/forecasting-with-stochastic-models-abf2e85c9679)