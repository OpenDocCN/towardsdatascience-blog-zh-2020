# 增强优化的 PySpark 查询

> 原文：<https://towardsdatascience.com/enhancing-optimized-pyspark-queries-1d2e9685d882?source=collection_archive---------19----------------------->

> 梦想成真的故事

![](img/89fd4fa69c385f051e69dc8b8b5a07fe.png)

亚历山大·雷德尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

随着我们不断增加处理和存储的数据量，随着技术进步的速度从线性转变为对数，从对数转变为水平渐近，改进我们软件和分析运行时间的创新方法是必要的。

这些必要的创新方法包括利用两个非常流行的框架:Apache Spark 和 Apache Arrow。这两个框架使用户能够以分布式方式处理大量数据。这两个框架也使用户能够通过使用矢量化方法更快地处理大量数据。这两个框架可以轻松促进大数据分析。然而，尽管有这两个框架和它们赋予用户的能力，仍然有改进的空间，特别是在 python 生态系统中。为什么我们可以自信地确定在 python 中利用这些框架的改进之处？让我们研究一下 python 的一些特性。

## Python 的特性

作为一种编程语言，python 实现了纯粹的灵活性。开发人员不需要在实例化或定义变量之前指定变量的类型。开发者不需要指定函数的返回类型。Python 在运行时解释每个对象的类型，这允许这些限制(或护栏)被移除。python 的这些特性缩短了开发时间，提高了生产率。然而，众所周知，这些相同的特性对程序运行时间有负面影响。由于 python 在运行时解释每个对象的类型，所以一些软件需要很长时间才能运行，尤其是那些需要过多循环的软件。即使在利用向量化操作的程序中，考虑到一些编程逻辑的复杂性，仍然会对运行时性能产生一些影响。我们能做些什么来减轻这些与性能相关的影响吗？让我们把注意力转向我选择的解决方案:Numba

## Numba 来救援了

Numba 对 python 函数执行即时编译，与 C/C++和 Java 编译的执行方式非常相似。仅包含标准内置函数或一组 NumPy 函数的 Python 函数可以使用 Numba 进行改进。这里有一个例子:

```
from time import time
from numba import jit
import numpy as np@jit(nopython=True, fastmath=True)
def numba_sum(x): 
    return np.sum(x)# this returns the median time of execution
def profileFunct(funct, arraySize, nTimes):_times = [] 
    for _ in range(nTimes):
        start = time()
        funct(np.random.random((arraySize,)))
        end = time()
        _times.append(end - start)return np.median(_times)# this is the numba time
numba_times = [profileFunct(numba_sum, i, 1000) for i in range(100, 1001, 100)]# this is the standard numpy timing
numpy_times = [profileFunct(np.sum, i, 1000) for i in range(100, 1001, 100)]speed_up_lst = list(map(lambda x: x[1] / x[0], zip(numba_times, numpy_times)))
```

在上面的例子中，我们只计算一个 numpy.array 的和，然后相互比较性能。在这个简单的例子中，我们看到了适度的性能提升。根据您的本地机器，您可以看到 10%到 150%的性能提升。通常，您会看到类似的性能提升，或者如果您在函数中迭代，甚至会更多。

如果我们可以加速一个简单的加法示例，让我们来看一个使用 Numba + Apache Arrow + Apache Spark 的示例。

## 三个火枪手

下面是一个简单的例子，展示了创建实时编译函数，然后通过 pandas_udf 使用它们的可能性。

```
from numba import jit
import numpy as np
import pandas as pdimport pyspark.sql.functions as F
from pyspark.sql import SparkSession
from pyspark.sql.types import DoubleTypespark = SparkSession.builder.appName("test").getOrCreate()spark.conf.set("spark.sql.execution.arrow.enabled", "true")df = pd.DataFrame(data=np.random.random((100,)), columns=["c1"])sdf = spark.createDataFrame(df)# JIT compiled function
@jit(nopython=True, fastmath=True)
def numba_add_one(x):
    return x + np.ones(x.shape)# this is needed to use apache arrow
@F.pandas_udf(DoubleType())
def add_one(x):
    return pd.Series(numba_add_one(x.values))sdf = sdf.withColumn("c1_add_one", add_one(F.col("c1")))sdf.toPandas()
```

如前所述，这是一个简单的例子。然而，对于更复杂的应用程序，这是非常有价值的，将使它加速，即使是最基本的 Apache Spark SQL 查询。

我希望你喜欢你的阅读！如果您对此感兴趣，那么您会对下面的文章感兴趣:

[](/scaling-dag-creation-with-apache-airflow-a7b34ba486ac) [## 使用 Apache Airflow 扩展 DAG 创建

### 数据科学社区中最困难的任务之一不是设计一个结构良好的模型…

towardsdatascience.com](/scaling-dag-creation-with-apache-airflow-a7b34ba486ac) 

如果您想了解更多信息，请在 LinkedIn 上关注我，或者访问我的主页，在那里联系我

[](https://www.linkedin.com/in/edward-turner-polygot/) [## 爱德华·特纳——数据科学家——pay locity | LinkedIn

### 爱德华·特纳(Edward Turner)是一名多语言开发人员，懂 Python、R 和 Scala，懂 Java 和 C/C++的语法。他…

www.linkedin.com](https://www.linkedin.com/in/edward-turner-polygot/)  [## 主页

### 在这里，您将找到有关 Edward Turner 所做工作的信息，以及…

ed-特纳. github.io](https://ed-turner.github.io/) 

再次感谢！一如既往#happycoding