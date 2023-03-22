# NumPy 入门

> 原文：<https://towardsdatascience.com/diving-into-numpy-b92abfd7a00f?source=collection_archive---------22----------------------->

![](img/44ddbc1949080e059edf7f9b21e4d10b.png)

照片由 [Myriam Jessier](https://unsplash.com/@mjessier?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

NumPy 是数据科学和机器学习中使用的最重要和最基本的库之一，它包含多维数组的功能，高级数学函数，

*   线性代数运算
*   傅里叶变换
*   随机生成器

NumPy 数组也构成了 scikit-learn 的基本数据结构。Numpy 的核心是经过很好优化的 C 代码，所以在使用 NumPy 的同时在 Python 中提高了执行速度。

> 用 Python 实现科学计算的基础包— [NumPy](https://numpy.org/)

本文包括 NumPy 中的基本操作和最常用的操作。这篇文章对初学者来说是友好的，对中级和高级读者来说也是复习的。

让我们从导入 NumPy 开始，

```
import numpy as np
```

`as`关键字使`np`成为 NumPy 的别名，所以我们可以用 np 代替 NumPy。这是一种常见的做法，可以节省时间，使工作更容易。

# **NumPy 数组**

为了创建一个 NumPy 数组，我们可以使用`np.array`函数来创建它，并使用`dtype`作为一个可选参数，将数组更改为所需的类型。下面是[数组数据类型](https://numpy.org/doc/stable/user/basics.types.html)的列表。当数组中的元素属于不同的数组数据类型时，那么这些元素将被*提升*到最高级别的类型。这意味着如果一个数组输入混合了`int`和`float`元素，那么所有的整数都将被转换成它们的*浮点等价物*。如果一个数组混合了`int`、`float`和`string`元素，那么所有的东西都会被转换成*字符串*。

为了将数组转换成所需的类型，我们可以使用`astype`函数。该函数的必需参数是数组的新类型，要知道数组的类型，我们可以使用`.dtype`

要复制一个数组，我们可以使用固有的`copy`函数并执行它。NaN(非数字)值也可以通过使用`np.nan`来使用。 *nan* 将作为一个占位符，不接受*整数值。*如果在包含 nan 时使用整数类型，将导致*错误*。

# 数字基础

NumPy 提供了一个使用`[np.arange](https://docs.scipy.org/doc/numpy/reference/generated/numpy.arange.html)`创建范围数据数组的选项。该函数的行为非常类似于 Python 中的`range`函数，并返回一个 1D 数组。如果一个数字`n`作为参数被传递，那么它将返回从 `0`到`n-1`的数字*。如果两个数字作为参数`m`和`n`传递，它将返回从`m`到`n-1`的数字。如果使用三个参数`m`、`n`和`s`，它将使用步长`s`返回从`m`到`n-1`的数字。*

`shape`函数用于知道数组的形状。使用`reshape`函数时，它将输入数组和新形状作为参数。例如，如果数组中元素的数量是 10，那么新形状应该是(5，2)或(2，5)，因为它们形成乘法结果。我们被允许在新形状的至多一个维度中使用特殊值-1 。带有-1 的维度将采用允许新形状包含数组的所有元素所必需的值。

函数`flatten`将*将任意大小的数组*整形为 *1D 数组。*

# 数学运算

借助 NumPy 数组，我们可以对数组中的每个元素应用算术、逻辑和其他运算。这有助于通过少量操作修改大量数值数据。NumPy 数组对数组中的每个元素执行基本的算术运算。除了基本的算术函数，NumPy 还可以执行其他的*三角函数、双曲线函数、指数函数、对数函数、*以及更多的函数。这些功能已经在这里列出[。](https://numpy.org/doc/stable/reference/routines.math.html)

函数`[np.exp](https://docs.scipy.org/doc/numpy/reference/generated/numpy.exp.html)`对数组执行以 e 为底的指数*运算，而`[np.log10](https://docs.scipy.org/doc/numpy/reference/generated/numpy.log10.html)`使用以 10 为底的*运算对输入数组执行对数运算。为了对任何基地进行常规的能量操作，我们使用`[np.power](https://docs.scipy.org/doc/numpy/reference/generated/numpy.power.html)`。该函数的第一个参数是基数*和幂*，第二个参数是幂*和幂*。为了在两个数组之间执行矩阵乘法，我们使用了函数`np.matmul`。`np.matmul`中两个输入矩阵的维数必须*服从矩阵乘法的原理*。第一个矩阵的第二个维度必须等于第二个矩阵的第一个维度，否则`np.matmul`将导致`ValueError`。**

# 随机发生器

NumPy 有一个名为`np.random`的模块，用于*伪随机数生成*，它执行从 1D 数组到多维数组的随机操作。`[np.random.randint](https://docs.scipy.org/doc/numpy-1.14.0/reference/generated/numpy.random.randint.html#numpy.random.randint)`函数将生成随机整数。该函数将接受一个必需的参数(`high`)。整数将在从`low` *(含)到* `high` *(不含)*的范围内生成。`size`参数将返回指定大小的数组。`[np.random.seed](https://docs.scipy.org/doc/numpy-1.14.0/reference/generated/numpy.random.seed.html#numpy.random.seed)`函数用于设置随机种子，允许我们控制*伪随机*函数的输出。这个函数接受一个代表随机种子的*参数。使用`np.random.seed`函数时，随机函数生成的输出在每次后续运行中都是相同的。在*多维数组*沿着*第一轴*洗牌时，我们使用`[np.random.shuffle](https://numpy.org/doc/stable/reference/random/generated/numpy.random.shuffle.html)`来洗牌。*

`np.random`可以从不同的概率分布中抽取样本。最常用的分布是`np.random.uniform`和`np.random.normal`，其他的是[这里是](https://numpy.org/doc/stable/search.html?q=Random%20sampling%20(numpy.random))。`np.random.uniform`函数为所提供的范围抽取随机样本，并根据提到的大小返回数组。`np.random.normal`按照*正态分布*抽取样本，其中`loc`和`scale`分别代表均值和标准差。这两个函数都没有必需的参数。

# 访问数组元素

访问 NumPy 中的数组元素类似于访问 Python 列表中的元素。对于一个多维数组，它将与访问 *Python 的列表列表*中的元素相同。借助`:` *(冒号)操作符*，切片可以像 Python 一样在 NumPy 中完成。会再次切分整个数组。在中，多维数组`,(comma)`用于在每个维度上分隔切片。负步进和切片将在*向后方向*上运行。

为了找出数组中最小和最大元素的索引，我们可以使用`np.argmin`和`np.argmax`。注意使用此函数 ***时，将返回元素的索引*** 。这两个函数的必需参数是*输入数组*。在上面的代码中，第`56`行，该函数将返回每列中最小的*行元素*的索引。在行`57`中，该函数将返回每行中最大*列元素*的索引。

# 数据提炼

由于有大量的数据，我们只需要过滤出分析所需的数据。这可以借助基本的*关系操作*来完成，如 *==、>、<、！*等。，NumPy 在数组上按元素执行这些操作。`~`操作代表一个[布尔否定](https://en.wikipedia.org/wiki/Negation)，即它翻转数组中的每个真值。

`np.isnan`函数用于确定数组的哪个位置包含`nan`值，如果有`nan`值，则返回*真*，否则返回*假。*`[np.where](https://numpy.org/doc/stable/reference/generated/numpy.where.html)`函数采用一个必需的第一个参数，即输入布尔数组。该函数将返回满足条件的元素的位置，即布尔值输出为真。当它与唯一的第一个参数*一起使用时，它返回一个一维数组的元组。与此同时，它将返回数组的数据类型。`np.where`功能必须与 *1 或 3 个参数*一起使用。在 3 个参数的情况下，第一个参数必须是输入布尔数组，第二个参数代表**真**替换值，第三个参数代表**假**替换值。*

如果我们想根据数据的行或列进行过滤，我们可以使用`[np.any](https://docs.scipy.org/doc/numpy/reference/generated/numpy.any.html)`和`[np.all](https://docs.scipy.org/doc/numpy/reference/generated/numpy.all.html)`函数。这两个函数接受相同的参数并返回一个布尔值。这两个函数的必需参数是一个布尔数组。`np.any`返回**真**如果*数组中至少有一个*元素满足所提供的条件，否则返回**假。** `np.all`返回**真**如果*数组中的所有元素*都满足给定的条件，否则返回**假**。`np.any`和`np.all`分别相当于逻辑 OR `||`和逻辑 AND `&&`运算符。

# 聚集技术

聚合将涉及一些技术，如*求和、串联、累积求和*等等。要对单个数组中的元素求和，我们可以使用`np.sum`函数。我们可以使用`axis`参数并跨行和列获取。要执行累积加法，我们可以使用`np.cumsum`功能。不设置`axis`将返回展平数组中所有值的累积和。设置`axis=0`返回每*列*的累积和数组，而`axis=1`返回每*行*的累积和数组。

为了执行相同大小的多个数组的连接，我们可以使用`np.concat`函数。这里`axis`的*默认值*将是`0`，因此垂直*发生串联*。如果`axis=1`水平*连接发生*。

# 统计操作

为了检查数组中的数据，我们可以使用 NumPy 执行一些统计操作。为了获得 NumPy 数组中的最小值和最大值，我们可以分别使用`min`和`max`函数。`axis`关键字参数与主题***‘访问数组元素’***的`np.argmin`和`np.argmax`中的用法相同。在这里，我们使用`axis=0`来查找`arr1`的每一列中的最小值的数组，使用`axis=1`来查找`arr1`的每一行中的最大值的数组。

我们还可以分别借助`np.mean`、`np.median`、`np.var`和`np.std`找到平均值、中值、方差和标准差。我们还可以使用`axis`关键字参数，并获得数组的行和列的度量。[这里是 NumPy 提供的全部统计操作](https://numpy.org/doc/stable/reference/routines.statistics.html)。

# 结束备注！！

从这篇文章中，我们已经介绍了 NumPy 中基本的和常用的操作。从 *NumPy* 开始将是你的数据科学或机器学习生涯的一个开端。要成为 NumPy 的大师，我建议你阅读整个 [NumPy 文档](https://numpy.org/doc/stable/reference/index.html)。我希望这篇文章对你有所帮助。