# Keras 深度学习入门

> 原文：<https://towardsdatascience.com/deep-learning-primer-with-keras-3958705882c5?source=collection_archive---------53----------------------->

![](img/f36fd420ba9733313a9387366f74dfc9.png)

图片由[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3706562)的 Gerd Altmann 提供

# Keras 是什么？

Keras 是一个深度学习框架，位于 TensorFlow 等后端框架之上。

# 为什么要用 Keras？

Keras 是优秀的，因为它允许你以很高的速度实验不同的神经网络！它位于 TensorFlow 等其他优秀框架之上，非常适合有经验的数据科学家和新手！它不需要那么多代码就可以启动并运行！

Keras 为您提供了构建所有类型架构的灵活性；这可以是递归神经网络、卷积神经网络、简单神经网络、深度神经网络等。

# 有什么区别？

你可能会问自己 Keras 和 TensorFlow 之间有什么区别…让我们来澄清一下！Keras 实际上是集成到 TensorFlow 中的。它是 TensorFlow 后端的包装器(从技术上讲，您可以将 Keras 与各种潜在的后端一起使用)。那是什么意思？在 TensorFlow 中，您可以根据需要拨打任何 Keras 电话。您可以享受 TensorFlow 后端，同时利用 Keras 的简单性。

# 神经网络最适合处理什么问题？

# 特征抽出

神经网络和传统机器学习的主要区别是什么？特征提取！传统上，无论是谁在操作机器学习模型，都是在执行与特征提取相关的所有任务。神经网络的不同之处在于，它们非常擅长为你执行这一步。

**非结构化数据**

当涉及到本质上不是表格并且是非结构化格式的数据时；即音频、视频等。；很难执行特征工程。你方便的神经网络将会在这类任务中表现得更好。

# 当你不需要解释的时候

当涉及到神经网络时，你对你的模型的结果没有太多的可视性。虽然这可能是好的，但取决于神经网络的应用，这也可能是棘手的。如果你能正确地将一幅图像归类为一匹马，那就太好了！你做到了；你其实不需要知道你的神经网络是怎么算出来的；而对于其他问题，这可能是模型价值的一个关键方面。

# 神经网络是什么样子的？

你最基本的神经网络将由三个主要层组成:

你的输入层，将由你所有的训练数据组成，

您的隐藏层，这是所有参数加权发生的地方，

最后是你的输出层——你的预测将在这里得到体现！

# 重量调整

当谈到应用于神经网络隐藏层的权重时，有几个主要因素可以帮助我们优化神经网络以获得正确的权重。其中之一是使用激活函数来确定权重。激活函数有助于网络识别数据中复杂的非线性模式。您可能会发现自己在使用 sigmoid、tanh、relu 和 softmax。

# 把手弄脏！

您需要从 Keras 导入所需的包。`Sequential`允许你实例化你的模型，&允许你添加每个层，基础，隐藏，&输出。此外，`model.add()`调用是我们如何将每一层添加到深度学习网络，直到最终输出层。

```
from keras.models import Sequential
from keras.layers import Dense# instantiate model
model = Sequential()
model.add(Dense(4, input_shape=(2,), activation="relu"))
model.add(Dense(1))
model.summary()
```

# 结论

如果你一路走到这里，那么你已经成功地运行了你的第一个神经网络！我希望这个对 Keras 的快速介绍是有益的。如果你想了解更多的其他话题或原则，请告诉我。在那之前，祝数据科学快乐！