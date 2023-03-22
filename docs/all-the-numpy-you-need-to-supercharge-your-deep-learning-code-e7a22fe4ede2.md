# 增强深度学习代码所需的所有数字

> 原文：<https://towardsdatascience.com/all-the-numpy-you-need-to-supercharge-your-deep-learning-code-e7a22fe4ede2?source=collection_archive---------28----------------------->

![](img/9002004ae3e349e6b275c027fc9225a9.png)

米克·豪普特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

NumPy 或 Numerical Python 是一个开源的 Python 库，使复杂的数值运算变得容易。使用机器学习和深度学习应用程序涉及大型数据集的复杂数值运算。与纯 Python 实现相比，NumPy 使得实现这些操作相对简单和有效。

在其核心，NumPy 实现了它的(n 维数组)数据结构，这类似于一个常规的 Python 列表。大多数编程语言只有数组的概念。Python 实现了列表，列表作为数组工作，但还是有区别的。

常规 python 列表和 NumPy 之间的一个关键区别是 python 列表可以包含各种数据类型。相反，NumPy 数组只能包含相同数据类型的对象。换句话说，数据结构是同质的。虽然这看起来像是一个缺点，但它允许 NumPy 操作更快，因为它们可以在进行计算时避免转换和约束。

## 为什么要关心 NumPy，为什么专门针对深度学习？

我将在本文中讨论几个具体的用例，但是与其他 python 数据结构相比，NumPy 最重要的特性之一是速度。NumPy 比使用常规 Python 列表快几个数量级。性能提升是可能的，因为 NumPy 函数是用 C 实现的，这除了使执行更快之外，还使 NumPy 能够分解大型作业并并行执行多个作业。深度学习涉及处理大型数据集，当处理这些大型数据集时，NumPy 数组可能是一种有效的技术。

## 创建 NumPy 数组

创建 NumPy 数组有几种不同的方法，一种直接的方法是使用`array()`函数创建它们。

```
#import NumPy
import numpy as np#create a NumPy array
a = np.array([1,2,3])#check the shape of the array
a.shape#get the dimensions 
a.dim
```

此外，你可以直接从标准的 ***Python 列表*** 中创建。Numpy 阵列是智能的。如果您将一个 Python 列表传递给`array()`函数，它将自动执行操作并返回一个 Numpy 数组。您不必先显式转换为 NumPy 数组。

```
#python List
my_list = [1,2,3,4,5]#creating a NumPy array from a list
np.array(my_list)
```

您还可以使用 pandas 函数将 Pandas 数据帧转换为 NumPy 数组。`to_numpy()`

```
#import Pandas
import pandas as pd#create a DataFrame
df = pd.DataFrame({"A":[1, 2],"B":[3, 4]})#convert DataFrame to NumPy array
df.to_numpy()
```

值得注意的是，由于 NumPy 数组只能包含相同数据类型的元素。默认情况下，当您转换数据帧时，返回数组的`dtype`将是数据帧中所有类型的公共 NumPy `dtype`。

## NumPy 热爱数学

NumPy 数组是专门用来做数学的。该软件包包括几个辅助数学函数，允许你做这些计算，而不需要自己快速编写。

常见的例子包括用于获得平方根的`sqrt()`函数、用于计算对数的`log()`函数和用于计算双曲正切的`tanh()`函数，后者通常用作深度神经网络中的激活函数。

```
a = np.array([1,2,3])#get the square root
np.sqrt(a)#get log
np.log(a)#find the tanh
np.tanh(a)
```

## 线性代数

线性代数广泛用于机器学习和深度学习应用。在处理深度学习时，你会发现自己正在处理高维数组，这些数组可以很容易地转化为线性方程，以分析给定空间中特征的相互作用。

NumPy 有几个内置的线性代数算法，作为`linalg`子模块中的方法。

一个常用的线性代数函数是`norm()`一个用于计算向量长度的函数，也被称为向量范数或向量幅度。向量范数作为一种归一化技术被应用于解决机器学习和深度学习应用中的过度填充问题。NumPy 的`linalg`子模块有`norm()`函数来计算向量范数

```
a = np.array([1,2,3])#getting a norm of the vector
np.linalg.norm(a)
```

使用线性代数时，遇到数学错误并不罕见。一些数学运算是不允许的。处理这些错误的一个好方法是使用`linalg.LinAlgError`异常处理，这是一个从 Python 的异常类派生的通用异常类。

例如，如果我们试图对一个奇异矩阵求逆，这个操作是不允许的，并且会抛出一个错误。

```
x=np.ones((2,2)) np.linalg.inv(x) #this will throw an error#exception handling using LinAlgError
try:
    np.linalg.inv(x)
except np.linalg.LinAlgError:
    print("Linear Algebra Error")
```

如果将几个矩阵堆叠在同一个数组上，这些线性代数函数可以计算出它们的结果。

## 广播

NumPy 进行算术计算的基本特征之一是通过广播。广播允许不同形状和大小的 NumPy 阵列之间的操作。

广播通过比较拖尾维度来工作，广播有效有两个主要规则。

*   尺寸相等，或
*   其中一个维度是 1

当您在 NumPy 中执行元素操作时，比如向 np.array 添加一个标量，这实际上就是广播。

```
x = np.ones((2,2))
y = np.ones((3,2,1))
a = np.ones((2,3,3))x+y #Broadcasting will work; trailing dimensions match
x+a #Broadcasting will fail; trailing dimensions do not match
```

## 数字和矩阵

NumPy 的真正潜力来自于处理矩阵。Numpy 支持各种易于使用的方法来进行标准矩阵运算，如点积、转置、获取对角线等。

矩阵乘法，具体来说，计算度量的 ***点积*** ，是深度学习中的常见任务，尤其是在处理卷积神经网络时。NumPy 函数`dot()`计算两个指标的点积。在 NumPy 的更高版本中，您还可以使用`@`操作符来计算点积。

```
x = np.array([[1,2],[3,4]])
y = np.array([[5,6],[7,8]])#dot product with dot()
x.dot(y)#dot product with @
x@y
```

深度学习中接下来两个常用的矩阵运算是 ***求逆和*** 转置。

我们先来看逆。什么是逆？一个数乘以它的倒数等于 1。重要的是要记住，不是所有的矩阵都有逆矩阵，在这种情况下，你会得到一个错误。你可以用`linalg.inv()`函数得到一个矩阵的逆矩阵。

```
x = np.array([[1, 2], [3, 4]])#getting the inverse
np.linalg.inv(x)
```

矩阵的转置是一种在矩阵对角线上翻转矩阵的操作。也就是说，它切换矩阵的行和列索引。在 NumPy 中，你可以用`T`得到一个矩阵的转置

```
x = np.array([[1, 2], [3, 4]])#getting the transpose
x.T
```

在深度学习中， ***特征值和特征向量*** 在实现主成分分析(PCA)等降维方法时很有用

你可以使用`linalg.eignvals()`函数计算一个向量或矩阵的特征值。

```
x = np.array([[1, 2], [3, 4]])#getting the eigenvectors
np.linalg.eigvals(x)
```

使用矩阵时，一定要记住不要使用 NumPy 类中的 matrix 函数，而要使用常规数组。

## 改变形状

使用深度学习时，您会经常遇到需要改变数组或矩阵形状的情况。您可以使用`reshape()`功能**重塑**NumPy 数组。该函数返回具有新形状的新 NumPy 数组。

```
x = np.array([[1, 2], [3, 4]])#Reshape 2X2 to 4X1
x.reshape(4,1)
```

如果需要改变原始 NumPy 数组的形状，可以使用`resize()`函数。它的工作方式与`reshape()`函数相似，但它改变了原始对象，而不是创建一个新对象。

```
x = np.array([[1, 2], [3, 4]]#Resize the original array 2X2 to 4X1
x.resize(4,1)
```

***扁平化*** 是另一种标准的深度学习操作，用于将数据或输入传递到你的神经网络的不同层。`flatten()` NumPy 功能将展平一个`ndarray`。

```
x = np.array([[1, 2], [3, 4]]#flatten the array
x.flatten()
```

除了使用`flatten()`之外，你还可以使用`ravel()`方法来做同样的事情。`flatten()`和`ravel()`的区别在于，`ravel()`可以直接用于常规链表，并将返回一个 NumPy 数组。

```
x_list = [[1, 2],[3, 4]]#flatten a list
np.ravel(x_list)
```

整形的一种形式是 ***给一个数组增加一个新的维度*** 。例如，您可以通过使用关键字添加一个新维度`newaxis`来取消展平

```
y = np.array([1,2,3,4,5])#add a new dimension
y[:,np.newaxis]
```

重塑 NumPy 数组的另一种方法是使用函数拆分它。该函数采用，并根据指定的索引或节返回子数组。

```
x = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8])#split array into 3 sections
np.split(x,3)
```

## 使用 NumPy 生成数据

NumPy 为数值运算提供了强大的工具包；但是，NumPy 也可以生成数据。让我们看看一些常见的场景。

一个 ***单位矩阵*** 是一个任意阶的正方形矩阵，沿主对角线为 1，所有其他元素为 0。NumPy 函数创建指定阶数的单位矩阵。

```
#generate a identity matrix 5
np.identity(5)
```

在深度学习中，你会遇到需要一个***0&1***的矩阵的情况。NumPy 有方便的函数`zeros()`和`ones()`可以用来生成 0 或 1 的矩阵。

```
#generate a 2X3 matrix of 0s
np.zeros((2,3))#generate a 2X3 matrix of 1s
np.ones((2,3))
```

使用 NumPy `random.rand()`函数，您可以创建一个由随机数元素组成的指定顺序的数组。

```
#generate a 2X3 matrix of random numbers
np.random.rand(2,3)#generate a 2X3 matrix of random integers
np.random.randint(2,3)
```

注意，如果您想要随机整数，您可以使用`random.randint()`函数获得一个包含随机整数的 NumPy 数组。

上面讨论的数据生成方法可以与它们的`_like`对应物一起使用，例如`zeros_like`、`ones_like`来生成数组，这些数组采用作为参数传递给函数的数组的形状。

```
x = np.array([[1, 2],[3, 4]])#generate a matrix of 1s based on dimensions of x 
np.ones_like(x)
```

换句话说，当使用函数的`_like`形式时，我们不指定形状，而是传递一个现有的 NumPy 数组。产生的 NumPy 数组采用传递的数组的形状。

## 总结想法

在这篇文章中，我涵盖了你需要开始的所有必要的数字。NumPy 比这里介绍的内容更多，但是我们在这里介绍的内容应该足以让您在深度学习项目中开始使用 NumPy。最好的学习方法是建立。当你开始从事深度学习项目时，你会遇到需要你使用额外技术和学习更多知识的情况。快乐大厦！