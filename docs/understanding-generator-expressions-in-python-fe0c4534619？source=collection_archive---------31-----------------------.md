# 理解 Python 中的生成器表达式

> 原文：<https://towardsdatascience.com/understanding-generator-expressions-in-python-fe0c4534619?source=collection_archive---------31----------------------->

## 技术的

## 通过利用一个优雅且内存高效的解决方案在 python 中生成序列类型，提高您的 python 编程语言技能。

![](img/f5b883e33e7a9d0ae84861770929c2ab.png)

制造者在 [Unsplash](https://unsplash.com/s/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [NESA 的照片](https://unsplash.com/@nesabymakers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

本文是对 Python 编程语言中的[生成器表达式(gene XP)](https://www.python.org/dev/peps/pep-0289/)的介绍。

本文面向所有级别的开发人员。如果你是初学者，你可以学习一些新概念，比如生成器表达式、列表理解(listcomps)和序列类型生成。中级开发人员可以学习一两件关于可伸缩性和内存效率的事情，或者只是将本文作为复习。

在本文中，您会发现以下内容:

*   **生成器表达式描述**
*   **如何利用生成器表达式(代码)**
*   **生成器表达式相对于其他类似解决方案的优势**
*   **gene XP 和 listcomps 的内存和时间测量**

# 生成器表达式

> enexps 是在 python 中生成序列类型(如数组、元组、集合)的优雅且节省内存的解决方案。

生成器表达式类似于[list comprehensions(list comps)](/basics-of-list-comprehensions-in-python-e8b75da50b30)——在 python 中构造列表序列类型的另一种方法。Genexps 和 listcomps 在实现方式上有相似之处。

下面的短代码片段描述了它们的语法相似性:

在下面的代码中，生成器表达式用于计算按整数递增的一系列值的总和。

```
#Generator Expression
accumulated_gexp = sum((1 + x for x in range(2000000)))
print(accumulated_gexp)
>> 2000001000000#List Comprehension
accumulated_listcomp = sum([1 + x for x in range(2000000)])
print(accumulated_listcomp)
>>2000001000000
```

虽然很微妙，但上面的 genexp 和 listcomp 示例之间的主要区别是 genexp 以方括号开始和结束，而 listcomp 以方括号开始和结束——更好的术语应该是'*括号*'。

下面是另一个使用生成器表达式创建元组的例子。

```
beginning_topic = ['Machine', 'Deep', 'Reinforcement']
ending_topic = 'Learning'tuple(print(beginning + " " + ending_topic) for beginning in beginning_topic)>> Machine Learning
>> Deep Learning
>> Reinforcement Learning
```

# 优势:内存效率

![](img/e679db099bc8756ac95e0a6e09269885.png)

由 [Franck V.](https://unsplash.com/@franckinjapan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/compute-memory?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

与 listcomps 相比，生成表达式更节省内存。Genexps 内存效率是利用 python 迭代器协议“*产生*或返回迭代器中的项目的结果。相比之下，列表理解为生成的列表及其内容利用内存。

一个生成器只会在需要的时候产生迭代器中的条目，因此给了 genexps 内存高效的特性。

如果上面写的还不够清楚，下面的代码片段会让它更加清楚。下面的代码片段显示了 genexps 和 listcomps 示例的内存大小要求(以字节为单位)。

```
from sys import getsizeofaccumulated_gexp = (1 + x for x in range(2000000))print(type(accumulated_gexp))
print(getsizeof(accumulated_gexp))
>> <class 'generator'>
>> 112accumulated_listcomp = [1 + x for x in range(2000000)]
print(type(accumulated_listcomp))
print(getsizeof(accumulated_listcomp))
>> <class 'list'>
>> 17632624
```

在上面的代码片段中，我们可以观察到 list comprehension 使用了 17632624 字节；而生成器表达式只使用了少得可怜的 112 字节内存。

还可以通过使用迭代器的 next 函数来访问序列的内容，用于生成器表达式和列表索引来理解列表。

```
print(next(accumulated_gexp))
>> 1print(accumulated_listcomp[0])
>> 1
```

# 优势:时间效率

![](img/84b150e2826bc6ebaf0e80ea6e34419d.png)

由[盖尔·马塞尔](https://unsplash.com/@gaellemarcel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

与列表理解相比，生成器表达式的另一个优点是它们的时间效率特性。

对于许多开发人员来说，尤其是初学者，您更熟悉并接触到列表理解的使用。

但是在大多数情况下，使用生成器表达式的好处不是那么容易被忽略的，尤其是当程序执行的速度非常重要的时候。

在实际场景中，当你在一个序列上迭代一次时，你很可能会更好地利用 genexps。为了更大的灵活性和序列的多次迭代，您可能会使用 listcomps。

下面的代码片段演示了 listcomps 和 genexps 之间的执行时间差异。

```
import timeitgenerator_exp_time = timeit.timeit('''accumulated_gexp = (1 + x for x in range(200))''', number=1000000)
print(generator_exp_time)
>> 1.5132575110037578list_comp_time = timeit.timeit('''accumulated_listcomp = [1 + x for x in range(200)]''', number=1000000)
print(list_comp_time)
>> 29.604462443996454
```

使用 [timeit](https://docs.python.org/3/library/timeit.html) python 模块，我们可以测量代码行的执行时间，如上所示。

正如我们所观察到的，生成器表达式用时不到两秒(generator_exp_time: 1.51…)，而列表理解执行时间几乎多了 20 倍。

# 优势

下面总结了 python 中生成表达式的优点:

*   python 中生成序列类型的高效内存方法。
*   为编写的代码增加了进一步的简洁性和可读性。生成器表达式被[生成器函数](https://wiki.python.org/moin/Generators)缩短。
*   与列表比较相比，时间效率高。

# 结论

生成器表达式一点也不复杂，它们使得 python 编写的代码高效且可伸缩。

对于初学者来说，学习何时使用列表理解和生成器表达式是在职业生涯早期掌握的一个极好的概念。

# 我希望这篇文章对你有用。

要联系我或找到更多类似本文的内容，请执行以下操作:

1.  订阅我的 [**YouTube 频道**](https://www.youtube.com/channel/UCNNYpuGCrihz_YsEpZjo8TA) 即将上线的视频内容 [**这里**](https://www.youtube.com/channel/UCNNYpuGCrihz_YsEpZjo8TA)
2.  跟我上 [**中**](https://medium.com/@richmond.alake)
3.  通过 [**LinkedIn**](https://www.linkedin.com/in/richmondalake/) 联系我