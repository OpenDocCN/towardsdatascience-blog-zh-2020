# TensorFlow +类继承=漂亮的代码

> 原文：<https://towardsdatascience.com/tensorflow-class-inheritance-beautiful-code-59d2eb7cdfce?source=collection_archive---------22----------------------->

## 用这种方法深入研究更多的技术模型。

![](img/20e686388c225966a00b26a9e33893eb.png)

照片由[丹·列斐伏尔](https://unsplash.com/@danlefeb?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 简介:

学习开发深度学习模型并不是一件容易完成的任务——尤其是对于那些可能在该领域没有丰富经验的人来说。当开始学习如何在 TensorFlow 中构建 DL 模型时，工作流程通常如下:

但是，如果部署或更多的技术模型出现，这种构建模型的格式可能会对您的时间造成负担，而不是资产。Python 的面向对象特性很好地发挥了作用，使它更多地属于后者而不是前者。本教程将分解开发更健壮的模型的工作流程。

# 继承:

当观察上面的`gist`时，我们可以看到我们初始化了`keras.Sequential`，它的类类型是`tf.keras.Model`。通过添加一个层列表作为`parameter`，我们初始化要调用的层——按顺序——用于向前传递。

我们可以使用所谓的`class inheritance`来构建[张量流模型](https://www.tensorflow.org/api_docs/python/tf/keras/Model)。通过继承`tf.keras.Model`类，我们可以将我们自己的层合并到模型中，并从那里构建我们的向前传递。

对于简单分类的例子，比如上面的例子，这样做可能有点复杂*(双关语。考虑到这一点，我仍然会继续上面的模式。如果您理解 tensor flow API 的基础，那么您将能够看到上面要点中的调用如何转化为开发更多面向对象的代码，这是本文的主要目的。*

当继承一个`tf.keras.Model`类时，有两个主要步骤需要考虑:

*   在`__init__`内初始化模型中的必要层。
*   在`call`内订购模型的向前传球。

## 正在初始化类:

对于`__init__`，我们有 4 个主要步骤。

*   首先，我们引入`super`的`__init__`——在本例中是`tf.keras.Model`。
*   其次，我们初始化`Flatten`层。
*   第三，我们用`128`单元初始化`Dense`层，并激活`tf.nn.relu`。值得注意的是，当我们在第一个要点中调用激活函数时，我们使用了一个字符串(`'relu'`而不是`tf.nn.relu`)。)当构建我们自己的模型类时，我们必须从`tf.nn`模块中调用我们的激活函数。
*   第四，我们用`10`单元初始化最后的`Dense`层。

为了一致性，*总是*首先初始化`super`的`__init__`。这将确保当您在使用中调用该类时，它将作为`tf.keras.Model`加载。层的初始化顺序无关紧要。

## 打电话:

当定义`call`时，我们正在定义模型的正向传递。我们有参数`inputs`，在拟合时，这些参数将是`x_train`。因此，在`call`中，它将类似于第一个要点中的层的排序。然而，与层列表相反——并且依赖于`fit`来确定向前传递——我们必须明确地确定`x`首先进入`Flatten`层——也就是`self.flatten`,然后将其传递到`self.dense1`层，最后输出`self.dense2`层的结果。

# 结论:

本文应该作为构建更多面向对象代码的通用指南。了解自定义模型的工作流程可以让您在研究中获得更多自由和试验。我希望这篇文章对你有益。

> 感谢您的阅读。