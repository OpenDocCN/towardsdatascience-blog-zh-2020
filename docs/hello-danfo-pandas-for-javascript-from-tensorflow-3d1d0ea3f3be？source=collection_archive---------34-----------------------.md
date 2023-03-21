# Hello Danfo:用于 Javascript 的熊猫，来自 Tensorflow

> 原文：<https://towardsdatascience.com/hello-danfo-pandas-for-javascript-from-tensorflow-3d1d0ea3f3be?source=collection_archive---------34----------------------->

## Tensorflow.js 刚刚获得了更多端到端

![](img/36ce71b69ed9dc00c656f4f948fb17ff.png)

图多尔·巴休在 [Unsplash](https://unsplash.com/s/photos/javascript-code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 什么和为什么

如今，绝大多数数据科学家都生活在 python 和熊猫的世界里。有了 python 的金星科学计算堆栈，很难解释为什么它应该有所不同。通过绑定将“尽可能的方便”和“必须的性能”结合起来是无与伦比的。

然而，有一个类似的流行使用渠道，它为应用程序开发人员提供了更多的服务，而不是普通的数据科学家:javascript 中的机器学习。

等等……*什么*？

# Javascript 中的机器学习

这并不像听起来那么荒谬。嗯，大部分是。

当然，绝大多数数据科学家应该继续使用现有的和流行的基于 python 的框架(PyTorch、Tensorflow、ONNX 等)。这些框架已经过高度优化，以支持小规模和大规模的快速研究和应用。

然而，机器学习的民主化正在进行中，从爱好者到应用程序开发者再到物理学家，每个人都想分一杯羹。自 NodeJS 兴起以来，应用程序开发人员经历了端到端功能的爆炸式增长，因此有理由希望机器学习也能在浏览器中实现。请注意，这意味着浏览器真正地将样本处理成预测，而不仅仅是将请求发送到远程后端。

这种浏览器内的 ML 运动已经发展了一段时间，主要是通过 TensorflowJS。谷歌基于 Javascript 的 Tensorflow 库为大量全栈开发人员带来了机器学习，这些开发人员不想仅仅为了让一个基于 python 的服务可以完成所有算法处理而旋转多个服务。

# 一个肮脏的数据仓库

尽管如此，仍有一个关键环节缺失。向普通开发人员提供 ML 建模只有在没有强大灵活的数据处理库的情况下才有意义。

输入 DanfoJS。

> Danfo.js 是一个开源的 JavaScript 库，为操作和处理结构化数据提供了高性能、直观且易于使用的数据结构。
> - [DanfoJS 文档](https://danfo.jsdata.org/)

![](img/c13341d244dc9ebeb6258547a0b4dc53.png)

照片由[亚伦·伯顿](https://unsplash.com/@aaronburden?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

想象一下，如果没有我们心爱的 pandas、NumPy、scikit-learn 和其他工具使预处理和 ETL 代码变得更加简单和简洁，会有多么冗长。除了许多额外的定制工作时间之外，每个人的 ML 管道最终看起来*比 python 环境中的*更加不同和支离破碎。丹佛的开发者明白。只需看看主页上宣传的一些要点:

> -轻松处理浮点和非浮点数据
> 中缺失的数据(表示为`NaN`)-大小可变性:可以从数据帧
> 中插入/删除列-自动和显式对齐:对象可以显式对齐到一组标签，或者用户可以简单地忽略标签，让`[Series](https://danfo.jsdata.org/api-reference/series)`、`[DataFrame](https://danfo.jsdata.org/api-reference/dataframe)`等。在计算中为您自动对齐数据
> -强大、灵活的分组功能，可对数据集执行拆分-应用-组合操作，用于聚合和转换数据
> -轻松将数组、JSON、列表或对象、张量和不同索引的数据结构转换为 DataFrame 对象
> -智能的基于标签的切片、花式索引和大型数据集查询
> -直观的合并和连接数据集
> -强大的 IO 工具，用于从[平面文件](https://danfo.jsdata.org/api-reference/input-output/danfo.read_csv)加载数据
> -强大、灵活、直观的 API，用于交互式绘制数据帧和系列。
> -特定于时间序列的功能:日期范围生成以及日期和时间属性。
> 
> - [DanfoJS 文档](https://danfo.jsdata.org/)

# 第一眼

请看下面的片段，它摘自一个示例笔记本，该笔记本用 TensorflowJS 训练了一个泰坦尼克号生存预测模型。如果你问我的话，这和典型的`pandas`语法没有太大区别。

仅从这个片段中就可以看出一些东西:

*   python 数据生态系统用户非常熟悉该语法
*   代码还有一个 [MinMaxScaler](https://danfo.jsdata.org/api-reference/general-functions/danfo.minmaxscaler) 助手
*   数据类型对张量有一流的支持

很好，不用再搜索“np.array to tensor”😅。

该库的产品包括建模库中通常存在的附加缩放/标记助手: [OneHotEncoder](https://danfo.jsdata.org/api-reference/general-functions/danfo.onehotencoder) 、 [StandardScaler](https://danfo.jsdata.org/api-reference/general-functions/danfo.standardscaler) 、 [MinMaxScaler](https://danfo.jsdata.org/api-reference/general-functions/danfo.minmaxscaler) 和 [LabelEncoder](https://danfo.jsdata.org/api-reference/general-functions/danfo.labelencoder) 。

![](img/75ddc5e9a5ba2ad9fa2e2893ceea7e0a.png)

由 [Max Duzij](https://unsplash.com/@max_duz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

许多人认为浏览器内的机器学习是一只跛脚鸭。我认为它有有效的用例，比如端到端的 javascript 应用程序。随着 [DanfoJS](https://danfo.jsdata.org/) 的发展，TensorflowJS 中的 ML 管道可以清理很多，也可以适当地从其他地方移入数据处理代码。

除了验证这一运动，我更感兴趣的是随着时间的推移会有什么样的项目出现。你有什么项目想法吗？在这里让我知道[。](https://www.lifewithdata.org/contact)

# 资源

# 资源

*   [DanfoJS 主页](https://danfo.jsdata.org/)
*   10 分钟后到达丹福斯
*   [DanfoJS API 参考](https://danfo.jsdata.org/api-reference)
*   [TensorflowJS 示例](https://www.tensorflow.org/js/demos)
*   [用丹佛斯& TensorflowJS](https://danfo.jsdata.org/examples/titanic-survival-prediction-using-danfo.js-and-tensorflow.js) 进行泰坦尼克号的生存预测

# 保持最新状态

除了在 Medium 上，用 [LifeWithData](https://lifewithdata.org/) 博客、[机器学习 UTD 时事通讯](http://eepurl.com/gOe01T)和我的 [Twitter](https://twitter.com/@anthonyagnone) 让自己保持更新。通过这些平台，我分别提供了更多的长文和简文思想。

如果你不是电子邮件和社交媒体的粉丝，但仍然想留在圈子里，可以考虑在 Feedly 聚合设置中添加[lifewithdata.org/blog](https://lifewithdata.org/blog)和[lifewithdata.org/newsletter](https://www.lifewithdata.org/newsletter)。