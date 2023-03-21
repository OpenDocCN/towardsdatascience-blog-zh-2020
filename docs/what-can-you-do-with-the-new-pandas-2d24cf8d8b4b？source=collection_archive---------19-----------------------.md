# 你能用新的“熊猫”做什么？

> 原文：<https://towardsdatascience.com/what-can-you-do-with-the-new-pandas-2d24cf8d8b4b?source=collection_archive---------19----------------------->

## 熊猫 1.0.0 版本出来了。这些更新不仅是 Python 社区期待已久的，也是整个数据科学社区期待已久的。在这篇文章中，我谈论了我认为在这个版本中很重要的 5 个主要变化。

欢迎发表评论，让我知道您认为重要的其他变化以及数据科学社区如何利用它们。

![](img/a19ff8c7c5f7017d251a7a01db5f8f59.png)

新熊猫[【来源】](https://www.chinadaily.com.cn/a/201910/09/WS5d9da7b1a310cf3e3556f80d.html)。

# **但是，熊猫是什么？**

Pandas 是 Python 中数据分析使用最多的库。它的框架结构和方法使得来自所有起步背景的数据分析师更容易入门数据科学。这是一个灵活、强大且易于使用的工具。

我们开始吧，好吗？👌

**a)使用 Numba 让熊猫跑得更快**

什么是 Numba？它是一个开源的实时(JIT)编译器，将 Python 和 Numpy 代码翻译成更快的机器翻译代码。换句话说，它可以帮助在多个 CPU 内核上执行 Numpy 表达式。

***apply()*** 函数可以通过将引擎关键字指定为‘Numba’来利用 Numba。这使得对大型数据集(100 万行或更大)执行函数变得更加容易。

对于此功能，Numba 库应该有版本 **0.46.0 或更高版本。**

```
**In [1]: data = pd.Series(range(1000))****In [2]: roll = data.rolling(10)****In [3]: %timeit -r 1 -n 1 roll.apply(lambda x: np.sum(x), engine='numba',  raw=True)** 1.15 s ± 0 ns per loop (mean ± std. dev. of 1 run, 1 loop each)**## Function is cached when you run the second time - so faster
In [4]: %timeit roll.apply(lambda x: np.sum(x), engine='numba', raw=True)** 368 ms ± 3.65 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

**b)一种“专用”管柱类型**

在以前版本的 Pandas 中，字符串存储在 *object* numpy 数组中。在新版本中，增加了一个专用于字符串数据的类型— `StringDType`。这解决了许多问题，例如:

1.  它消除了包含字符串和非字符串的混合型数组的混乱
2.  它帮助我们执行只针对字符串的函数。

为了使用这个功能，我们可以显式地将**类型**设置为`string`。

```
**# Can also string type using `pd.StringDType()` 
In [1]: x = pd.Series(['name', None, 'class'], dtype='string')****In [2]: x** 0     name
1    <NA>
2     class
dtype: string
```

**c)一个‘专用’布尔类型**

在以前版本的 Pandas 中，布尔数组只能保存`True`或`False`值。丢失的数据无法合并到这些数组中。现在创建了一个专门用于布尔类型的新类型— `BooleanDType`，它可以保存缺失值。已经为缺少的值创建了一个新的掩码。

这对于数据分析师来说很重要，因为他们总是在处理缺失值，使用这种类型会使数据集更加明确。

```
**# Can also string type using `pd.BooleanDType()` 
In [1]: x = pd.Series([True, None, False], dtype='boolean')****In [2]: x** 0     True
1    <NA>
2     False
dtype: boolean
```

**d)处理** `**parquet**` **文件变得更加容易**

对于那些一直在使用 PySpark 的人来说，有时数据分析师会在 Spark 中处理数据，并将其保存为一个 Parquet 文件。然后，他们喜欢在拼花地板上用熊猫来做不那么“繁重”的工作。但是如果 parquet 文件有一个用户定义的模式，Pandas 中以前的版本会抛出一个错误，指出它不能集成 Pandas 中的模式。

在新版本中，Pandas 处理用户定义模式的模式参数。`schema`现在与 *pyarrow* 引擎一起处理模式参数。

```
**In [1]:** import pyarrow as pa ## pyspark engine import**In [2]: x = pd.DataFrame({'a': list('abc'), 'b': list(range(1, 4))})****In [3]:** **schema = pa.schema([('a', pa.string()), ('b', pa.int64())])****# Can also be used for `read_parquet` as well
In [4]: x.to_parquet('test.parquet', schema=schema)**
```

**e)要降价的桌子！**

当你训练了一个模型或者对数据做了一些分析之后，你就可以生成表格(用于结果)。这些表格需要以 markdown 或 latex 的形式填写——在工作过程中或工作后进行报告。

由于可以将 DataFrame 对象转换成 **markdown 格式**作为**输出**，因此用新的格式编写文档变得更加容易。这确实是一个很好的特性，因为现在表格的降价可以动态生成。

```
**In [1]:** x = pd.DataFrame({'A': [1,2], 'B': [3,4]})**In [2]: print(x.to_markdown())** |    |   A |   B |
|---:|----:|----:| 
|  0 |   1 |   3 |
|  1 |   2 |   4 |
```

此外，还修复了多个不同主题的错误，如*绘图*、 *I/O* 、*日期时间*等。关于这些的更多信息可以在 1.0.0 的 [Pandas 文档中找到。](https://pandas.pydata.org/pandas-docs/version/1.0.0/whatsnew/v1.0.0.html#bug-fixes)

# **想安装/升级熊猫？**

要使用 pip 安装 Pandas(针对首次用户):

```
pip install pandas
```

对于已经有旧版本 Pandas 但想升级的用户:

```
pip install --upgrade pandas
```

也感谢所有管理和开发熊猫的贡献者/开发者🎉。

我希望你觉得这篇文章有趣并且有用。如果你有，请分享。

此外，如果你觉得有更重要的变化，请随时写在评论中。

🐼🐼🐼🐼🐼🐼谢谢大家！