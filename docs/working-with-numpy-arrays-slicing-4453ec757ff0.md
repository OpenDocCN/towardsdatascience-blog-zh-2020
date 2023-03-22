# 使用 Numpy:切片

> 原文：<https://towardsdatascience.com/working-with-numpy-arrays-slicing-4453ec757ff0?source=collection_archive---------34----------------------->

## [PyTrix 系列](https://towardsdatascience.com/tagged/pytrix-series)

## PyTrix #3:访问 Numpy 数组中的序列

![](img/8384cdfaf65f18e608e4bbf5cdba7098.png)

由[胡安·曼努埃尔·努涅斯·门德斯](https://unsplash.com/@juanma?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 限幅

当我们想要访问一个序列的一部分，比如一个字符串、元组或列表时，我们可以利用 Python 中的一个称为切片的特性，它使我们能够编写更加干净、简洁和可读的代码。

切片可用于查看、修改或删除可变序列中的项目，例如在列表数据类型中。此外，这个特性还可以很好地利用像 NumPy 和 Pandas 这样的框架。

有关本文中使用的代码，请参见:

[](https://github.com/kurtispykes/demo/blob/master/pytrix/pytrix_slicing.ipynb) [## kurtispykes/演示

### permalink dissolve GitHub 是超过 5000 万开发人员的家园，他们一起工作来托管和审查代码，管理…

github.com](https://github.com/kurtispykes/demo/blob/master/pytrix/pytrix_slicing.ipynb) 

# 数组与列表

分割一个数组和分割一个列表没有太大的不同。主要的区别是，数组，你可以在多个维度上分割数组，另一个重要的因素我将在后面讲到。

类似于我之前在 PyTrix 系列中提到的关于索引的内容；

[](/working-with-numpy-arrays-indexing-e4c08595ed57) [## 使用 Numpy 数组:索引

### 访问 Numpy 数组中的元素

towardsdatascience.com](/working-with-numpy-arrays-indexing-e4c08595ed57) 

当我们获取数组的一部分时，返回的数组是原始数组的视图——我们最终以不同的顺序访问了相同的数据。然而，当我们分割一个列表时，它将返回一个全新的列表。

# 切片列表数据类型

为了向您展示 Python 的内置切片特性是如何工作的，我现在将演示它的功能…

```
a = [1, 2, 3, 4, 5]
```

接下来，我将演示 python 切片是如何工作的，并截取这个列表的一部分并打印出来…

```
b = a[1:4]
print(b)
>>>> [2, 3, 4]
```

当我们使用切片表示法时，我们指定了我们希望从切片中得到的[start:end]索引。这也从开始复制列表，但不包括结束索引。

我们还可以做的一件很酷的事情是省略切片的开头或结尾。例如，如果我们省略开始并提供一个结束索引，Python 将从开始索引开始切片，但不包括结尾提供的索引。或者，如果我们省略结尾并提供一个开始，Python 将从提供的开始索引开始，并切片到列表的结尾。有关示例，请参见下面的代码单元格。

```
c = a[:3]
d = a[2:]
e = a[:]print(f"List c: {c}\nList d: {d}\nList e: {e}")
>>>> List c: [1, 2, 3]
     List d: [3, 4, 5]
     List e: [1, 2, 3, 4, 5]
```

请注意，在列表 e 中，我没有提供开始或结束。这只是整个列表的副本。

最后，为了不使索引变得复杂。让我们看看列表和切片的区别。

```
f = a[2]
g = a[2:3]print(f"f: {f}\ng: {g}")
>>>> f: 3
     g: [3] 
```

索引返回给定索引处的列表元素，而切片返回单个元素的列表。

# 切片 1D 阵列

如上所述，分割 1D Numpy 数组和 list 几乎是相同的任务，尽管如此，正如您将看到的，它们有一个明显的区别。

```
import numpy as npnp_a = np.array([1, 2, 3, 4, 5])
np_b = np_a[1:4]print(np_b)
>>>> [2 3 4] 
```

不同于切片时创建新列表的列表，NumPy 切片返回我们存储在`np_b`中的数据视图。因此，如果我们要改变`np_b`中的一个元素，那么`np_a`中的元素也会改变，反之亦然。请看下一个代码单元格中的示例:

```
print(f"Original Array {np_a}")
np_b[1] = 6
print(f"np_b: {np_b}\nOriginal Array: {np_a}")
>>>> Original Array: [1 2 3 4 5]
     np_b: [2 6 4]
     Original Array: [1 2 6 4 5]
```

# 切片 2D 阵列

2D 阵列可以在两个轴上切片…

```
a_2d = np.array([[1, 2, 3, 4, 5],
                 [6, 7, 8, 9, 10],
                 [11, 12, 13, 14, 15],
                 [16, 17, 18, 19, 20]])
print(a_2d[1:, 1:3])
>>>> [[7 8]
      [12 13]
      [17 18]]
```

为了进行分解，我们从 1:到数组的末尾取了一些行，从 1:3 取了一些列(因此是 1 和 2 列)。

# 切片 3D 数组

3D 阵列可以在所有 3 个轴上切片…

```
a_3d = np.array([[[1, 2, 3,], [4, 5, 6], [7, 8, 9]],
                 [[10, 11, 12], [13, 14, 15], [16, 17, 18]],
                 [[19, 20, 21], [22, 23, 24], [25, 26, 27]]])print(a_3d[1:, 1:, :])
>>>> [[ [13, 14, 15] [16, 17, 18]]
      [ [22, 23, 24] [25, 26, 27] ]]
```

我们选择了从索引 1 开始的所有平面、从索引 1 开始的所有行和所有列。出于清晰的原因，由于这可能相当棘手，我在下面再次解释了这一点；

*   飞机`1:`(最后两架飞机)
*   排`2:`(最后 2 排)
*   列`:`(所有列)

老实说，我没有必要在列中包含`:`。我们可以使用`:`功能获得平面、行或列的完整切片。然而，当我们有尾随索引时，我们仍然可以通过简单地省略索引来获得完整的切片。

简单地说，这意味着…

```
# 2D Arrays
print(a_2d[1:3, :] ==  a_2d[1:3])# 3D Arrays
print(a_3d[1:, :2, :] == a_3d[1:, :2])
print(a_3d[2:, :, :] == a_3d[2:, :])
>>>> [[ True  True  True  True  True]
      [ True  True  True  True  True]]

     [[[ True  True  True]
       [ True  True  True]]
      [[ True  True  True]
       [ True  True  True]]]pr [[[ True  True  True]
       [ True  True  True]
       [ True  True  True]]]
```

# 切片和索引

将此与之前关于索引的 PyTrix 系列相关联:

[](/working-with-numpy-arrays-indexing-e4c08595ed57) [## 使用 Numpy 数组:索引

### 访问 Numpy 数组中的元素

towardsdatascience.com](/working-with-numpy-arrays-indexing-e4c08595ed57) 

我们知道，我们可以使用索引来选择特定的平面、行或列。例如…

```
print(a_2d[1,2:4])
>>>> [8 9]
```

另一方面，我们也可以使用切片来做类似的操作…

```
print(a_2d[1:2,2:4])
*>>>> [[8 9]]*
```

请注意索引和切片返回的细微差别。索引返回一个 1D 数组，而切片方法返回一个只有一行的 2D 数组。

这个故事中使用的代码可以在…

[](https://github.com/kurtispykes/demo/blob/master/pytrix/pytrix_slicing.ipynb) [## kurtispykes/演示

### permalink dissolve GitHub 是超过 5000 万开发人员的家园，他们一起工作来托管和审查代码，管理…

github.com](https://github.com/kurtispykes/demo/blob/master/pytrix/pytrix_slicing.ipynb) 

## 一锤定音

如果您认为我遗漏了什么，或者您想向我指出什么，或者如果您仍然不确定什么，您的反馈是有价值的。发个回应！

然而，如果你想和我联系，我在 LinkedIn 上是最活跃的，我也很乐意和你联系。

[](https://www.linkedin.com/in/kurtispykes/) [## Kurtis Pykes -人工智能博客-走向数据科学| LinkedIn

### 在世界上最大的职业社区 LinkedIn 上查看 Kurtis Pykes 的个人资料。Kurtis 有两个工作列在他们的…

www.linkedin.com](https://www.linkedin.com/in/kurtispykes/) 

如果你喜欢这个 PyTrix 系列，你可以在下面的 PyTrix 系列链接中找到迄今为止的所有文章…

[](https://towardsdatascience.com/tagged/pytrix-series) [## Pytrix 系列-走向数据科学

### 阅读《走向数据科学》中关于 Pytrix 系列的文章。共享概念、想法和代码的媒体出版物。

towardsdatascience.com](https://towardsdatascience.com/tagged/pytrix-series) 

你可能会对我最近的帖子感兴趣…

[](/staying-motivated-for-your-data-science-career-e845f18421e1) [## 为您的数据科学事业保持动力

### 我们可以长期保持动力。

towardsdatascience.com](/staying-motivated-for-your-data-science-career-e845f18421e1)