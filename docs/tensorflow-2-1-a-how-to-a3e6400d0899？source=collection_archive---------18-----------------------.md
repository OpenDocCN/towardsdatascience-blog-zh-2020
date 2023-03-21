# TensorFlow 2.1:操作指南

> 原文：<https://towardsdatascience.com/tensorflow-2-1-a-how-to-a3e6400d0899?source=collection_archive---------18----------------------->

![](img/35430c976d5ba2221165c7d7e18fde06.png)

我们深入吧！—[Dids](https://www.instagram.com/Didssph/)的惊人画面

## keras 模式、渴望模式和图形模式

如果你和我一样，是一个正常人，你可能已经经历过有时你是如何陷入你的应用程序的开发中，以至于很难找到一个时刻停下来思考我们是否以最有效的方式做事:*我们是否使用了正确的工具？哪个框架最适合我的用例？这种方法可扩展吗？我们考虑到可伸缩性了吗？*

在 AI 领域更是如此。我们都知道人工智能是一个快速发展的领域。每天都有新的研究发表。正在高速开发的主要人工智能框架之间存在巨大的竞争。新的硬件架构、芯片和优化被发布以支持日益增长的人工智能应用的部署…然而，尽管有所有的华而不实，有时你需要停下来重新考虑。

什么时候是停下来重新考虑的好时机？只有你会知道。对我来说，这一刻是最近才到来的。自从我进入这个领域以来，我一直在工作和个人项目中使用 [Keras](https://keras.io/) 和 [Tensorflow 1.x](https://www.tensorflow.org/versions/r1.15/api_docs/python/tf) (TF1)。我完全喜欢 Keras 库的高级方法和 Tensorlfow 的低级方法，当您需要更多定制时，它们可以让您在引擎盖下进行更改。

尽管我是 Keras-Tensorflow 婚姻的超级粉丝，但总有一个非常具体的负面因素让这对夫妇远离田园生活:**调试功能**。正如你已经知道的，在 Tensorflow 中，有一个先定义计算图，然后编译它(或者移动到 GPU)然后非常高效地运行它的范例。这种范式非常好，从技术上讲也很有意义，但是，一旦你在 GPU 中有了模型，就几乎不可能调试它。

这就是为什么在 TensorFlow 2.0 的 alpha 版本[发布大约一年后，我决定尝试 TensorFlow 2.1(我可以从 TF2.0 开始，但我们都知道我们喜欢新软件)，并与您分享它的进展。](https://github.com/tensorflow/tensorflow/releases/tag/v2.0.0-alpha0)

# 张量流 2.1

令人难过的事实是，我很难弄清楚我应该如何使用这个新的 TensorFlow 版本，即著名的 2.1 稳定版本。我知道，这里有[大量的教程](https://www.tensorflow.org/tutorials)，笔记本和代码手册……然而，我发现困难并不在于编程，因为归根结底，这只是 Python，而是范式的转变。简而言之:TensorFlow 2 编程不同于 TensorFlow 1，就像面向对象编程不同于函数式编程一样。

在做了一些实验后，我发现在 TensorFlow 2.1 中有 3 种建模方法:

*   Keras 模式(`tf.keras`):基于图形定义，稍后运行图形。
*   急切模式:基于定义一个迭代地执行定义一个图的所有操作。
*   图表模式(`tf.function`):之前两种方法的混合。

无聊到此为止。给我看看代码！

## Keras 模式

这是我们都习惯的标准用法。只使用普通的 Keras 和自定义损失函数，以平方误差损失为特征。该网络是一个 3 密集层深度网络。

```
# The network
x = Input(shape=[20])
h = Dense(units=20, activation='relu')(x)
h = Dense(units=10, activation='relu')(h)
y = Dense(units=1)(h)
```

这里的目标是教一个网络学习如何对一个 20 个元素的向量求和。因此，我们向网络输入一个数据集`[10000 x 20]`，即 10000 个样本，每个样本有 20 个特征(要求和的元素)。那是在:

```
# Training samples
train_samples = tf.random.normal(shape=(10000, 20))
train_targets = tf.reduce_sum(train_samples, axis=-1)
test_samples = tf.random.normal(shape=(100, 20))
test_targets = tf.reduce_sum(test_samples, axis=-1)
```

我们可以运行这个例子，得到通常漂亮的 Keras 输出:

```
Epoch 1/10
10000/10000 [==============================] - 10s 1ms/sample - loss: 1.6754 - val_loss: 0.0481
Epoch 2/10
10000/10000 [==============================] - 10s 981us/sample - loss: 0.0227 - val_loss: 0.0116
Epoch 3/10
10000/10000 [==============================] - 10s 971us/sample - loss: 0.0101 - val_loss: 0.0070
```

这里发生了什么？嗯，没什么，只是一个 Keras 玩具的例子训练在 10 秒每时代(在一个英伟达 GTX 1080 Ti)。编程范式呢？和之前一样，就像在 TF1.x 中，你定义了图形，然后你通过调用`keras.models.Model.fit`来运行它。调试功能呢？和以前一样…没有。你甚至不能在损失函数中设置一个简单的断点。

运行完这段代码后，您可能会疑惑一个非常明显的问题:TensorFlow 2 版本承诺的所有优秀特性都到哪里去了？你是对的。如果与 Keras 包的集成意味着不需要安装额外的包，那么优势是什么？

除此之外，还有一个更重要的问题:众所周知的调试特性在哪里？幸运的是，这是急切模式来拯救。

## 渴望模式

如果我告诉你有一种方法可以交互地构建你的模型，并且可以访问运行时的所有操作，那会怎么样？— 如果你激动得发抖，这意味着你已经经历了 10 个纪元后随机批次运行时错误的深刻痛苦……是的，我知道，我也去过那里，我们可以在那些战斗后开始称自己为战友。

嗯，是的，这就是你要找的操作模式。在这种模式下，所有的张量操作都是交互式的，你可以设置一个断点并访问任何中间张量变量。然而，这种灵活性是有代价的:更显式的代码。让我们来看看:

阅读完代码后，你脑海中出现的第一件事可能是:许多代码只是为了做一个`model.compile`和一个`model.fit`。是的，没错。但另一方面，你可以控制之前发生的一切。引擎盖下发生了什么？训练循环。

所以现在情况变了。在这种方法中，你可以从头开始设计事情如何进行。以下是您现在可以指定的内容:

*   度量:曾经想要测量每个样本、批次或任何其他自定义统计的结果吗？没问题，我们掩护你。现在，您可以使用传统的移动平均线或任何其他自定义指标。
*   损失函数:曾经想做疯狂的多参数依赖损失函数？嗯，这也解决了，你可以在损失函数定义中得到所有你想要的技巧，而不用 Keras 用它的`_standarize_user_data` ( [链接](https://github.com/keras-team/keras/blob/f242c6421fe93468064441551cdab66e70f631d8/keras/engine/training.py#L470))来抱怨它
*   梯度:您可以访问梯度，并定义向前和向后传递的细节。是的，最后，请和我一起欢呼吧！

这些指标是用新的`tf.keras.metrics` [API](https://www.tensorflow.org/api_docs/python/tf/keras/metrics/Mean) 指定的。您只需获取想要的指标，定义它并像这样使用它:

```
# Getting metric instanced
metric = tf.keras.metrics.Mean() # Run your model to get the loss and update the metric
loss = [...]
metric(loss)# Print the metric 
print('Training Loss: %.3f' % metric.result().numpy())
```

损失函数和梯度分别在正向和反向过程中计算。在这种方法中，向前传球必须由`tf.GradientTape`记录。`tf.GradientTape`将跟踪(或记录)前向传递中完成的所有张量操作，因此它可以计算后向传递中的梯度。换句话说:为了向后跑，你必须记住你向前走过的路。

```
# Forward pass: needs to be recorded by gradient tape
with tf.GradientTape() as tape:
    y_pred = model(x)
    loss = loss_compute(y_true, y_pred)# Backward pass:
gradients = tape.gradient(loss, model.trainable_weights)
optimizer.apply_gradients(zip(gradients, model.trainable_weights))
```

这非常简单，在向前传递中，你运行你的预测，并通过计算损失来看你做得有多好。在反向过程中，通过计算梯度来检查权重如何影响损失，然后通过更新权重(在优化器的帮助下)来尝试最小化损失。

您还可以在代码中注意到，在每个时期结束时，会计算验证损失(通过只运行正向传递而不更新权重)。

让我们看看这与之前的方法相比如何(我将输出减少了一点，这样它就可以放在这里):

```
Epoch 1:
Loss: 1.310: 100%|███████████| 10000/10000 [00:41<00:00, 239.70it/s]
Epoch 2:
Loss: 0.018: 100%|███████████| 10000/10000 [00:41<00:00, 240.21it/s]
Epoch 3:
Loss: 0.010: 100%|███████████| 10000/10000 [00:41<00:00, 239.28it/s]
```

发生了什么事？你注意到了吗？在同一台机器上，每个时期花费了 41 秒，即 4 倍的时间增量…这只是一个虚拟模型。你能想象这对一个真实的用例模型，如 RetinaNet、YOLO 或 MaskRCNN，会有多大的影响吗？

幸运的是，优秀的 TensorFlow 人员意识到了这一点，并实现了图形模式。

## 图表模式

图表模式(来自 AutoGraph 或`tf.function`)是前两种模式的混合模式。这里的[这里的](https://www.tensorflow.org/guide/function#keras_and_autograph)和这里的[这里的](https://www.tensorflow.org/guide/effective_tf2#recommendations_for_idiomatic_tensorflow_20)你可以大致了解一下这是什么。但是我发现那些指南有点混乱，所以我用我自己的话来解释。

如果 Keras 模式是关于定义图形并稍后在 GPU 中运行它，而 eager 模式是关于交互式地执行每个步骤，那么 graph 模式允许您像在 eager 模式中一样编码，但运行训练的速度几乎与在 Keras 模式中一样快(所以是的，在 GPU 中)。

关于 eager 模式的唯一变化是，在 graph 模式中，您将代码分解成小函数，并用`@tf.function`对这些函数进行注释。让我们来看看事情是如何变化的:

现在你可以看到前向和后向传递计算是如何被重构为两个用`@tf.function`装饰器注释的函数的。

那么这里到底发生了什么？简单。每当你用`@tf.function` decorator 注释一个函数时，你就像 Keras 一样将这些操作“编译”到 GPU 中。因此，通过注释您的函数，您可以告诉 TensorFlow 在 GPU 中的优化图形中运行这些操作。

在引擎盖下，真正发生的是函数正在被[亲笔](https://www.tensorflow.org/api_docs/python/tf/autograph)、`tf.autograph`解析。AutoGraph 将获取函数输入和输出，并从它们生成一个张量流图，这意味着，它将解析操作，以将输入的输出转换为张量流图。这个生成的图形将非常高效地运行到 GPU 中。

这就是为什么它是一种混合模式，因为除了用`@tf.function`装饰器注释的操作之外，所有的操作都是交互运行的。

这也意味着你可以访问所有的变量和张量，除了用`@tf.function`修饰的函数中的变量和张量，你只能访问它的输入和输出。这种方法建立了一种非常清晰的调试方式，在这种方式下，您可以在渴望模式下开始交互式开发，然后，当您的模型准备就绪时，使用`@tf.function`将其推向生产性能。听起来不错吧？让我们看看进展如何:

```
Epoch 1:
Loss: 1.438: 100%|████████████| 10000/10000 [00:16<00:00, 612.3it/s]
Epoch 2:
Loss: 0.015: 100%|████████████| 10000/10000 [00:16<00:00, 615.0it/s]
Epoch 3:
Loss: 0.009:  72%|████████████| 7219/10000  [00:11<00:04, 635.1it/s]
```

嗯，一个惊人的 16s/纪元。您可能认为它不如 Keras 模式快，但另一方面，您可以获得所有的调试功能和非常接近的性能。

## 结论

如果您一直在关注这篇文章，那么您将不会感到惊讶，所有这些最终都归结为一个非常古老的软件问题:灵活性还是效率？渴望模式还是 Keras 模式？为什么要和解？使用图形模式！

在我看来，TensorFlow 的工作人员在为我们这些开发人员提供更多灵活性方面做得非常出色，而且没有在效率方面做出太大的牺牲。所以从我的立场来看，我只能为他们说 *bravo* 。