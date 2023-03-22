# Tensorflow 最佳实践:命名输入和输出

> 原文：<https://towardsdatascience.com/tensorflow-best-practises-named-inputs-and-outputs-ed80c45eebd4?source=collection_archive---------21----------------------->

## 根据位置索引和输入值排序退出。开始依赖指定的输入和输出。避免数据布线错误

![](img/bdb142ba393c094a311886a8502438f1.png)

[图片来自 Pixabay](https://pixabay.com/photos/fiber-cable-wire-connection-4814456/)

> **命名输入和输出本质上是带有字符串关键字和张量值的字典**。

# 利益

1.  防止特征重新排序
2.  提供签名和元数据的自给自足模型
3.  重命名和缺少功能保护

大多数机器学习管道从结构化源(数据库、CSV 文件/ Pandas 数据帧、TF 记录)读取数据，执行特征选择、清理(以及可能的)预处理，将原始多维数组(张量)与表示每个输入样本的正确预测的另一个张量一起传递给模型。

*在生产中重新排序或重命名输入特征？* → **无效结果或客户端生产中断**

*缺项特征？缺失数据？产值解读不好？误混整数索引？* → **无效结果或客户端生产中断**

*想知道训练时使用了哪些特征列，以便为推理提供相同的特征列？* → **你不能——曲解错误**

*想知道输出值代表什么值？* → **你不能——曲解错误**

# 不要在模型输入层上删除列名。

默认情况下，`tf.data.Dataset`已经允许您这样做，将输入视为一个字典。

这些年来，上述问题变得更容易处理了。以下是 Tensorflow `2.x`生态系统中可用解决方案的简要概述。

*   [tfRecords 和 TF。示例](https://www.tensorflow.org/tutorials/load_data/tfrecord)无疑是用于任何规模深度学习项目的最佳数据格式。默认情况下，每个功能都以**命名。**

*   [Tensorflow Transform](https://www.tensorflow.org/tfx/tutorials/transform/simple) 使用指定的输入并产生指定的输出，鼓励你对你的模型做同样的事情。

*   Keras 支持图层字典作为输入*和*输出

添加多种尺寸的特征很简单:只需为 window_size 添加另一个参数，或者将特征形状与特征名称一起传递。

*   [TensorSpec](https://datascience.stackexchange.com/questions/71724/what-is-a-tensorspec-tensorflow-2-0) 和服务签名定义默认支持命名 IOs。

通过使用这个`serving_raw`签名定义，您可以通过 JSON 有效负载直接调用 [Tensorflow 服务端点，而无需序列化到`tf.Example`。](https://www.tensorflow.org/tfx/serving/serving_basic)

看看 TF 服务上的`metadata`签名，我目前正在做一个比特币预测模式的例子:

> 最后，如果您正在使用 TFX 或获得了用于输入的协议缓冲模式，您应该使用它来发送用于推断的数据，因为这样会更有效率，并且错误会更快地出现在客户端，而不是服务器端。*即使在这种情况下，也要为您的模型使用命名的输入和输出。*

**感谢一路看完！**

还想学习如何正确构建您的下一个机器学习项目吗？

*   查看我的[**结构化 ML 管道项目文章**](http://Want to create) **。**