# 广播 PySpark 累加器

> 原文：<https://towardsdatascience.com/broadcasting-pyspark-accumulators-343104c18c44?source=collection_archive---------43----------------------->

## 以及如何管理它们

![](img/507c40e9ef6b831696d0ee623a7fac32.png)

照片由[格雷戈·拉科齐](https://unsplash.com/@grakozy?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在这篇文章中，我将讨论一个有趣的模式，用一个方便的广播。在进入更多细节之前，让我们回顾一下什么是火花累加器。

> 一种可以累加的共享变量，即有一个可交换的和相关的“加”操作。Spark 集群上的工作任务可以使用+=操作符将值添加到累加器中，但是只有驱动程序才允许使用`[**value**](https://spark.apache.org/docs/2.3.1/api/python/pyspark.html#pyspark.Accumulator.value)`来访问它的值。来自工人的更新自动传播到驱动程序。
> 
> 来源:[https://spark . Apache . org/docs/2 . 3 . 1/API/python/py spark . html # py spark。蓄能器](https://spark.apache.org/docs/2.3.1/api/python/pyspark.html#pyspark.Accumulator)

# 累加器的三大戒律

1.  累加器只能用于交换和结合的“加法”运算。对于任何其他操作，我们必须使用自定义实现。稍后会详细介绍。
2.  可以在工作任务上“更新”累加器，但该任务不能访问其值。
3.  累加器可以在驱动程序上更新和访问。

# 几行代码抵得上千言万语

让我们看一个简单的累加器例子

在上面的示例代码中， *cnt* 是在全局级别上定义的。 *add_items* 方法将输入 *x* 加到 *cnt* 上。 *add_items* 方法后来应用于 *global_accumulator* 方法中 rdd 的每一项。这是累加器的典型用法，最后对 *global_accumulator* 的调用将输出 6，这是 1、2 和 3 的总和。请注意，我们需要将 *cnt* 定义为全局变量，否则各种方法都无法访问它，并且它将是未定义的。

## 那是什么问题…

现在，为了激发对广播累加器的需求，让我们将代码分成几个文件，这就是模块化代码库中的代码。

*global _ accumulator _ module*从另一个模块 *accumulator_process* 中调用 *process_data* 方法(代码如下)。

如果我们执行*global _ accumulator _ module*，那么它会失败并出现以下错误。

```
NameError: name ‘cnt’ is not defined
```

问题是 Python 中的全局变量对于一个模块来说是全局的。

> Python 中的全局变量是模块的全局变量，而不是所有模块的全局变量。(与 C 不同，C 中的全局变量在所有实现文件中都是相同的，除非您显式地将其设为静态。).如果从导入的模块中需要真正的全局变量，可以在导入模块的属性中设置这些变量。
> 
> 来源:[https://www . tutorialspoint . com/Explain-the-visibility-of-global-variables-in-imported-modules-in-Python](https://www.tutorialspoint.com/Explain-the-visibility-of-global-variables-in-imported-modules-in-Python)

那么，如何让一个全局变量在模块间可用呢？Python 医生有一些想法。

> 在单个程序中跨模块共享信息的规范方法是创建一个特殊的模块(通常称为 config 或 cfg)。只需在应用程序的所有模块中导入配置模块；然后，该模块就可以作为全局名称使用了。因为每个模块只有一个实例，所以对模块对象的任何更改都会在任何地方得到反映。例如:
> 
> **配置文件:**
> 
> x = 0*# x 配置设置的默认值*
> 
> **mod.py:**
> 
> 导入配置
> config.x = 1
> 
> **main.py:**
> 
> 导入配置
> 导入模式
> 打印(config.x)
> 
> 注意，出于同样的原因，使用模块也是实现单例设计模式的基础。
> 
> 来源:[https://docs.python.org/3/faq/programming.html?highlight = global # how-do-I-share-global-variables-cross-modules](https://docs.python.org/3/faq/programming.html?highlight=global#how-do-i-share-global-variables-across-modules)

虽然这不适用于 PySpark，原因是每个 worker 节点上有不同的 module 实例。**因此需要累加器，因为共享变量的典型方式在 Spark 中不起作用。**

# 广播累加器

在这种情况下， *broadcast_accumulator* 方法调用 *accumulator_process* 模块中的*process _ data _ accumulator*方法，累加器 *acc* 。

这里实际发生的是，我们向 worker 节点发送一个对象引用 *acc* 。这样我们就不必定义它为全局。

现在我将讨论累加器的两个定制:

1.  将静态值与累加器一起广播
2.  广播多个累加器

# 过滤累加器

在此示例中，FilterAccumulator 类保存项目列表。如果在*过程*方法中传递的项目不在项目列表中，则更新累加器 *acc* 。如代码所示， *init* 和 *count* 应该在驱动程序上执行，而*进程*应该在 worker 节点上执行。执行上述操作得到的值为 4，这是正确的结果。

# 多个累加器

在本例中，我们有两个累加器 *sum* 和 *num* ，分别累加项目的值和项目的数量。同样，如代码所示， *init* 和*意味着*应该在驱动程序上执行，而*在 worker 节点上处理*。虽然平均值可以使用其他本机 spark 方法获得，但这是一个如何使用单个类管理多个累加器而不将其声明为全局的示例(这在模块化代码中不起作用)。

# 结论

希望现在已经很清楚如何使用类和广播来管理累加器，而不是声明为全局的。我希望在将来发布如何创建非整型和浮点型的累加器，因为 Spark 允许自定义累加器。