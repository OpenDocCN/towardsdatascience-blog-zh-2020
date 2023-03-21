# NumPy 速成班:阵列基础

> 原文：<https://towardsdatascience.com/numpy-crash-course-array-basics-35c83ea147f5?source=collection_archive---------35----------------------->

## 几乎所有数据科学工具的核心数据结构。

![](img/7685635ffb132c80167b1afebe8f677a.png)

亨利&公司在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 列表与数组

我们都熟悉标准的 Python list——一个可变的对象，它具有很大的灵活性，因为不是列表中的所有元素都需要是同质的数据类型。也就是说，您可以拥有一个包含整数、字符串、浮点甚至其他对象的列表。

```
my_list = [2, {'dog': ['Rex', 3]}, 'John', 3.14]
```

上面是一个包含多种数据类型作为元素的完美有效的列表——甚至是一个包含另一个列表的字典！

然而，为了支持所有这些并发数据类型，每个 Python 列表元素都必须包含自己的唯一信息。每个元素都充当指向唯一 Python 对象的指针。由于这种低效率，随着列表变得越来越大，使用列表变得更加费力。

```
>>>for element in my_list:
    print(type(element))<class 'int'>
<class 'dict'>
<class 'str'>
<class 'float'>
```

有了数组，我们摆脱了列表的灵活性，取而代之的是一个由相同数据类型(通常是整数)的元素组成的多维表。
这使得大型数据集的存储和操作更加高效。

## 数组的属性

NumPy 数组的每个维度称为一个轴。例如，如果我们如下声明一个数组，我们有一个 2 轴数组:

```
>>> import numpy as np
>>> a = np.arange(10).reshape(2,5)
>>> a
array([[0, 1, 2, 3, 4],
       [5, 6, 7, 8, 9]])
```

上面的代码使用 arrange()函数创建一个范围为 10 的数组，并将其重塑为一个 2 x 5 的数组。这个方法只是标准 range()函数的数组版本。

你可以看到你的数组属性使用了内置的数组属性 shape、ndim、dtype、itemsize 和 size。

```
**>>> # Returns a tuple of the size of each dimension**
>>> a.shape
(2, 5)**>>> # Returns the number of axes of the array**
>>> a.ndim
2**>>> # Returns the description of the data type of the array**
>>> a.dtype
dtype('int32')**>>> # Returns the byte size of each element in the array**
>>> a.itemsize
4**>>> # Returns the total number of elements in the array**
>>> a.size
10
```

## 创建数组

从头开始创建 NumPy 数组的方法有很多——这通常取决于您的应用程序使用哪种方法，但是下面列出了一些更常用的技术。

注意:如果要显式指定类型，dtype 参数是可选的，否则它将默认为最适合您在创建时传递的数据的类型。

```
**>>> # Create an array of specified size filled with 0's**
>>> np.zeros((3,3), dtype=int)
array([[0, 0, 0],
       [0, 0, 0],
       [0, 0, 0]])**>>> # Create an array of specified size filled with the given value**
>>> np.full((4,2), 1.23)
array([[1.23, 1.23],
       [1.23, 1.23],
       [1.23, 1.23],
       [1.23, 1.23]])**>>> # Create a linear array with values from the arange() function**
>>> np.arange(10)
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])**>>> # Create an array of N evenly spaced values between x and y**
>>> # np.linspace(x, y, N)
array([0\.        , 0.11111111, 0.22222222, 0.33333333, 0.44444444,
       0.55555556, 0.66666667, 0.77777778, 0.88888889, 1\.        ])**>>> # Create an array of random values between 0 and 1**
>>> np.random.random((2,2))
array([[0.90416154, 0.56633881],
       [0.09384551, 0.23539769]])**>>> # Create an array of random integers between a given range**
>>> np.random.randint(0, 5, (2,2))
array([[4, 3],
       [3, 3]])
```

## 重塑数组

你已经看到我们使用了 shape()方法，它对操纵你的数组非常有用。对于 arange()和 linspace()这样的函数，可以使用 shape()创建任意大小的数组。
然而，**非常重要的一点是**要注意，为了能够重塑数组，新数组的大小必须与原始数组的大小相匹配。例如，以下内容不是有效的整形:

```
>>> b = np.arange(0, 6)
>>> b
array([0, 1, 2, 3, 4, 5])
>>> b.reshape((3,3))Traceback (most recent call last):
  File "<pyshell#40>", line 1, in <module>
    b.reshape((3,3))
ValueError: cannot reshape array of size 6 into shape (3,3)
```

我们有 6 个元素，但正试图改造成一个 3 x 3，这将需要 9 个元素。然而，我们可以把它改造成 3 x 2:

```
>>> b.reshape((3,2))
array([[0, 1],
       [2, 3],
       [4, 5]])
```

我们还可以通过使用 ravel()方法将多维数组“展平”成一维:

```
>>> b.ravel()
array([0, 1, 2, 3, 4, 5])
```

## 索引/切片数组

数组索引的工作方式非常类似于列表索引和切片，我们只需要注意数据的维度。

例如，假设我们正在使用如下所示的阵列:

```
>>> x = np.arange(12).reshape((4,3))
>>> x
array([[ 0, 1, 2],
       [ 3, 4, 5],
       [ 6, 7, 8],
       [ 9, 10, 11]])
```

我们有一个总共包含 12 个元素的 4 x 3。要访问一个元素，我们需要使用一个元组。因此，如果我们想在多维数组中返回值，我们可以这样做:

```
**>>> # Return an entire axis**
>>> x[0]
array([0, 1, 2])**>>> # Return a specific element**
>>> x[3][1]
10
```

我们也可以用它来修改数组中的值。
请注意，如果您尝试将该值修改为与您的数组不同的数据类型，您可能会遇到问题！当我们试图修改一个 int 类型的数组时，我们的 float 被转换为 int。

```
**>>> # Modify a value using a correct data type**
>>> x[1][1] = 30
>>> x
array([[ 0,  1,  2],
       [ 3, 30,  5],
       [ 6,  7,  8],
       [ 9, 10, 11]])**>>> # Modify a value using an incorrect data type**
>>> x[0][1] = 3.14
>>> x
array([[ 0,  **3**,  2],
       [ 3, 30,  5],
       [ 6,  7,  8],
       [ 9, 10, 11]])
```

对于切片，我们再次使用熟悉的列表切片符号，但是当我们处理一维以上的数组时，需要用逗号分隔每个维度。
我们使用通常的 **x【开始:停止:步骤】**格式访问行和列。

```
**>>> x**
array([[ 0,  3,  2],
       [ 3, 30,  5],
       [ 6,  7,  8],
       [ 9, 10, 11]])**>>> # Return a slice of the first 2 rows and all columns**
>>> x[:2, :]
array([[ 0,  3,  2],
       [ 3, 30,  5]])**>>> # Return a slice of all rows up to the first column**
>>> x[:, :1]
array([[0],
       [3],
       [6],
       [9]])**>>> # Return every other row, every column**
>>> x[::2, :]
array([[0, 3, 2],
       [6, 7, 8]])
```

既然您已经掌握了数组的基础知识，那么您就可以理解 Python 中最强大的数据结构之一了！

接下来，我们将研究对数组的基本操作，因此，随着本系列的进展，请务必遵循本系列。
未来故事的链接将会随着它们的发布而增加——以及一个索引页。

![](img/fbb8fb0e972b79dd8fcec885411b5696.png)

照片由 [Gaelle Marcel](https://unsplash.com/@gaellemarcel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄