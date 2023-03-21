# 加速 Python 运行时的 10 种技术

> 原文：<https://towardsdatascience.com/10-techniques-to-speed-up-python-runtime-95e213e925dc?source=collection_archive---------9----------------------->

## 用代码**运行时**比较好的写法和不好的写法

![](img/1d109177a63c45b7a12092e41fff9113.png)

哈雷戴维森在 Unsplash 上拍摄的照片

Python 是一种脚本语言。与 C/C++这样的编译语言相比，Python 在效率和性能上有一定的劣势。然而，我们可以使用一些技术来提高 Python 代码的效率。在本文中，我将向您展示我在工作中通常使用的加速技术。

测试环境是 Python 3.7，macOS 10.14.6，2.3 GHz 英特尔酷睿 i5。

# 0.优化原则

在深入代码优化的细节之前，我们需要了解一些代码优化的基本原则。

1.  **先确认代码能正常工作。**因为让正确的程序更快比让快程序正确容易得多。
2.  **权衡优化成本**。优化是有代价的。例如，较少的运行时间通常需要更多的空间使用，或者较少的空间使用通常需要更多的运行时间。
3.  **优化不能牺牲代码可读性。**

# 1.Python 中正确的数据类型用法

## 1.1 用 set 替换 list 以检查元素是否在序列中

根据 Python 的 [TimeComplexity](https://wiki.python.org/moin/TimeComplexity) 可知`list`的`x in s`操作的平均情况为 O(n)。另一方面，`set`的`x in s`运算的平均情况是 O(1)。

## 1.2 用 defaultdict 初始化字典

我们应该使用`defaultdict`进行初始化。

# 2.用生成器表达式替换列表理解

```
# Bad: 447ms
nums_sum_list_comprehension = sum([num**2 for num in range(1000000)])# Good: 300ms
nums_sum_generator_expression = sum((num**2 for num in range(1000000)))
```

生成器表达式的另一个好处是，我们可以在迭代之前获得结果，而无需在内存中构建和保存整个列表对象。换句话说，生成器表达式节省了内存使用。

```
import sys# Bad
nums_squared_list = [num**2 for num in range(1000000)]
print(sys.getsizeof(nums_squared_list))  # 87632# Good
nums_squared_generator = (num**2 for num in range(1000000))
print(sys.getsizeof(nums_squared_generator))  # 128
```

# 3.用局部变量替换全局变量

我们应该把全局变量放入函数中。局部变量比全局变量快。

# 4.避免点操作

## 4.1 避免功能访问

每次我们用`.`访问函数，都会触发特定的方法，像`__getattribute__()`和`__getattr__()`。这些方法将使用字典操作，这将导致时间成本。我们可以用`from xx import xx`去掉这样的成本。

根据技术 3，我们也可以将全局函数分配给局部函数。

此外，我们可以将`list.append()`方法分配给一个局部函数。

## 4.2 避免类属性访问

访问`self._value`的速度比访问局部变量慢。我们可以将 class 属性赋给一个局部变量来加快运行速度。

# 5.避免不必要的抽象

当使用额外的处理层(如 decorators、property access、descriptors)来包装代码时，会使代码变慢。在大多数情况下，需要重新考虑是否有必要使用这些层。一些 C/C++程序员可能会遵循使用 getter/setter 函数来访问属性的编码风格。但是我们可以使用更简单的写作风格。

# 6.避免数据重复

## 6.1 避免无意义的数据复制

`value_list`没有意义。

## 6.2 更改值时避免使用 temp 变量

`temp`是没有必要的。

## 6.3 连接字符串时，将`+`替换为`join()`

使用`a + b`串接字符串时，Python 会申请内存空间，将 a 和 b 分别复制到新申请的内存空间。这是因为 Python 中的字符串数据类型是不可变的对象。如果连接`n`字符串，将生成`n-1`中间结果，每个中间结果将申请内存空间并复制新字符串。

另一方面， `join()`会节省时间。它会先计算出需要申请的总内存空间，然后一次性申请所需内存，将每个字符串元素复制到内存中。

# 7.利用`if`语句的短路评估

Python 使用[短路技术](https://www.pythoninformer.com/python-language/intermediate-python/short-circuit-evaluation/)来加速真值评估。如果第一个语句是假的，那么整个事情一定是假的，所以它返回那个值。否则，如果第一个值为真，它将检查第二个值并返回该值。

因此，为了节省运行时间，我们可以遵循以下规则:

*   `if a and b`:变量`a`应该有很大概率为假，所以 Python 不会计算 b。
*   `if a or b`:变量`a`应该有更高的概率为真，所以 Python 不会计算 b。

# 8.循环优化

## 8.1 将`while`替换为`for`

`for`循环比`while`循环快。

## 8.2 用隐式 for 循环替换显式 for 循环

我们用上面的例子。

## 8.3 减少内部 for 循环的计算

我们将`sqrt(x)`从内部 for 循环移动到外部 for 循环。

# 9.使用 numba.jit

Numba 可以将 Python 函数 JIT 编译成机器码执行，大大提高了代码的速度。想了解更多关于 numba 的信息，请看[主页](http://numba.pydata.org/)。

我们用上面的例子。

我们将`sqrt(x)`从内部 for 循环移动到外部 for 循环。

# 10.使用 cProfile 定位时间成本函数

“cProfile”将输出每个功能的时间使用情况。所以我们可以找到时间成本函数。

> ***查看我的其他帖子*** [***中等***](https://medium.com/@bramblexu) ***同*** [***一分类查看***](https://bramblexu.com/posts/eb7bd472/) ***！
> GitHub:***[***bramble Xu***](https://github.com/BrambleXu) ***LinkedIn:***[***徐亮***](https://www.linkedin.com/in/xu-liang-99356891/) ***博客:***[***bramble Xu***](https://bramblexu.com/)

# 参考

*   [https://wiki.python.org/moin/PythonSpeed/PerformanceTips](https://wiki.python.org/moin/PythonSpeed/PerformanceTips)
*   [https://real python . com/introduction-to-python-generators/# building-generators-with-generator-expressions](https://realpython.com/introduction-to-python-generators/#building-generators-with-generator-expressions)
*   编写可靠的 Python 代码 91 建议
*   Python 食谱，第三版
*   [https://zhuanlan.zhihu.com/p/143052860](https://zhuanlan.zhihu.com/p/143052860)
*   [https://pybit.es/faster-python.html](https://pybit.es/faster-python.html)