# 如何将 Tensorflow/Keras 模型构建到增强现实应用程序中

> 原文：<https://towardsdatascience.com/how-to-build-your-tensorflow-keras-model-into-an-augmented-reality-app-18405c36acf5?source=collection_archive---------12----------------------->

![](img/16f519f14013f823e5bb91335a5d58ba.png)

照片由[大卫·格兰穆金](https://unsplash.com/@davidgrdm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/augmented-reality?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 移动机器学习

## 在 AR 和 ML 这两个快速发展的领域中，集成这两者可能会令人困惑。这是一个将深度学习模型实现到 Unity/C#和 AR 基础中的指南(尽管它也适用于 Vuforia)。

这个过程很复杂，可能会随着时间的推移而略有变化。随着变化的出现，我会尽最大努力更新。我假设您已经设置了 Unity 和 Tensorflow。Unity 应该是 2017.x 或者更高版本，Tensorflow 应该可以用任何版本(我用的是 Unity 2019.3.3，还有 tf2.1)。在我目前的项目中，我还在想办法，所以这可能不是最好的方法，但绝对有效。随着我了解更多，我会更新这篇文章。

我们将按照以下平台方面的顺序进行，这看起来比实际情况更令人困惑:

*经过训练的 Python/Tensorflow/Keras →转换为 ONNX →实现 bara cuda/tensor flow Sharp→Unity/c#→构建 Android 应用*

## 保存和导出模型:

如果您有一个 Keras `.h5`文件，您首先需要将它保存到一个`saved_model.pb`中。我们现在将它转换成一个开放神经网络交换(ONNX)文件，使用 Tensorflow Sharp 将它读入 Unity。

然后，按照他们的自述文件(我克隆了他们的 repo，然后运行了 `setup.py install`)获取你的`saved_model.pb` Now install [onnx](https://github.com/onnx/tensorflow-onnx) 的文件路径。在同一个文件夹中运行下面的命令(它查找`saved_model.pb`,所以确保它是你的文件夹路径中的文件名)。

```
python -m tf2onnx.convert --saved-model tensorflow-model-path --output model.onnx
```

以下是 Barracuda 支持的内容，我们将使用这个包将`.onnx`文件读入 Unity。

**支持的架构列表:**

*   所有 ML-agent 模型(强化学习)。
*   MobileNet v1/v2 图像分类器。
*   微型 YOLO v2 物体探测器。
*   UNet 类型的模型。
*   全卷积模型。
*   全密集模型

**支持操作列表:**

![](img/f3d035552bb29892d48a3d39bcf0b79e.png)

[https://docs . unity 3d . com/Packages/com . unity . barracuda @ 0.7/manual/index . html # getting-unity-barracuda](https://docs.unity3d.com/Packages/com.unity.barracuda@0.7/manual/index.html#getting-unity-barracuda)

**支持的激活功能列表:**

![](img/b682364d44ce5cf7550ae82c4e99105e.png)

[https://docs . unity 3d . com/Packages/com . unity . barracuda @ 0.7/manual/index . html # getting-unity-barracuda](https://docs.unity3d.com/Packages/com.unity.barracuda@0.7/manual/index.html#getting-unity-barracuda)

现在您已经有了`.onnx`文件，我们需要将 Unity mlagents 引入您的项目。最简单的方法是按照 Youtube 教程中的[来做，它要求你在你的 Unity 包管理器中获取`mlagents`(启用预览包),并在视频描述中拉入 GitHub 文件夹。之所以这么做，是因为这些包里包含了](https://www.youtube.com/watch?v=_9aPZH6pyA8&t=639s)[梭鱼](https://docs.unity3d.com/Packages/com.unity.barracuda@1.0/manual/index.html)，Unity 就是用它来轻松的把`.onnx`文件作为`.nn`带进来，用 C#执行。我相信你也可以从软件包管理器中安装 mlagents 和 Barracuda，但是如果那不起作用，试试上面的指导。你就快到了！

![](img/5cde4fc7fc99d539e91080088c70b6a3.png)

。onnx 文件放入 Unity Assets 文件夹时应该是这样的

## 在 Unity 中加载模型、创建模型输入和读取模型输出

对于那些不熟悉 Unity 的人，我可能会在这里失去你。Unity 有很棒的文档，所以你应该能够搜索到任何让你困惑的东西。

我们将执行三个步骤:

1.  逐帧访问摄像机画面
2.  将该帧转换成(224，224，3)图像纹理 2D
3.  从模型中读取 Softmax 层输出

首先在层级中右键单击创建一个`AR Session Origin`和`AR Session`。要访问相机馈送，我们将通过右键单击项目文件夹来创建“渲染纹理”。点击纹理，并在检查器中改变其大小为 1080x1920(或任何你想它是)。如果你出于某种原因不想使用渲染纹理，那么[这是另一个](https://github.com/Unity-Technologies/arfoundation-samples/blob/2.1/Assets/Scripts/TestCameraImage.cs)访问相机帧的变通方法。将渲染纹理放置在 AR 相机的相机组件的“目标纹理”字段中。这将移除正常的相机视图，并将其渲染到放置该渲染纹理的对象上。

![](img/d2048c97aa9bcec53e1c6ff157bec52c.png)

我们将创建一个 UI RawImage，在“Rect Transform”组件中将它拉伸到画布上，然后将渲染纹理附加到它的 RawImage 组件上。它应该是这样的:

![](img/f2f7b764fc03b9fca822d88473d88f21.png)

现在我们可以进入代码了。你可以在他们的 Unity 文档中熟悉一下 [barracud](https://docs.unity3d.com/Packages/com.unity.barracuda@1.0/manual/index.html) a，但是我们基本上需要加载`.nn` 模型，然后创建一个执行该模型的方法。之后，我们要调用最后一层，把它从一个张量转换成一个浮点数组。

这是很多，但让我们走一遍。NNModel 将出现在检查器中，您可以从项目文件中拖放`.nn`。我们想加载它，而缓存在`m_RuntimeModel`的运行时开始。

之后，我们创建额外的输出来调用 softmax 层(正如我所做的那样，这样我就可以手动设置阈值，而不是只使用 argmax)，并创建一个引擎来在设备 GPU 上执行模型。我们必须调整渲染纹理的大小以适应模型的输入，对我来说是(224，224，3)。我用来转换为 Texture2D 然后调整大小的两个函数如下:

代码的其余部分非常简单明了，不要担心`dispose()`方法，我们将在最后讨论它。有了这个，你应该能够连接到一个`UI Text`元素，而不是使用`debug.log`，并在你的手机上建立应用程序！我喜欢在我的 pc 上使用 Vuforia 的相机进行测试，然后在构建时将其更改为 AR Foundation 的相机。

如果构建不工作，你必须使用 [android 调试器(adb)](https://www.youtube.com/watch?v=eI2GOuEMGfQ) 。这将允许您使用`adb logcat -s Unity`查看构建的应用程序的崩溃日志。有时候，我的电脑网络摄像头上的东西不在我的手机上，这一切都非常依赖于版本！只要不断调整它，直到它的工作。

## 优化您的 AR 应用

太好了，现在你的模型可以在你的手机上工作了！但不幸的是，你会很快注意到帧速率很糟糕，我的第一次构建时大约是 3fps。即使使用微小的 yolov2，帧速率也不惊人(特别是对于你牺牲的地图)。你需要做的是在 Unity 的作业系统包中学习多线程和垃圾收集器。Barracuda 的功能已经类似于一个作业系统，具有 worker (handle)、execute (check out executeasync)和 disposal 方法。

当你在你的应用程序中构建更多的交互来使它真正成为 AR 时，你需要尽可能地保存所有的处理。 [Unity Profiler](https://docs.unity3d.com/Manual/Profiler.html) 会帮助你，只要确保它设置为层次而不是时间线。对于这个简单的应用程序，我只是每 10-15 帧执行一次模型，而不是每一帧。

或者，你可以在你的模型上使用量化，但是那必须在它是一个`.onnx`文件之后完成(抱歉还没有`tflite`支持)。以下代码应该可以在 python 中运行，但请注意，量化会影响模型的准确性。

[https://gist . github . com/andrewhong 5297/6601 e 7 e 48 fc 862 e 60 a 3843 CD 8 fcfe 480](https://gist.github.com/andrewhong5297/6601e7e48fc862e60a3843cd8fcfe480)

现在你知道了！有很多移动和变化的部分，但都有很好的记录。不要害怕深入 Barracuda 和 Onnx GitHub 库或 Unity 论坛寻求帮助。

如果您有任何困惑或困惑，请留下您的评论。玩得开心！