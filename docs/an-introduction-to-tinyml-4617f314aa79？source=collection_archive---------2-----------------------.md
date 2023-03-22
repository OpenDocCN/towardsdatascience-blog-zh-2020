# TinyML 简介

> 原文：<https://towardsdatascience.com/an-introduction-to-tinyml-4617f314aa79?source=collection_archive---------2----------------------->

## 机器学习遇上嵌入式系统

![](img/5f16f15d8d1dc7f6921aebb43d11ee74.png)

Alexandre Debiève 在 [Unsplash](https://unsplash.com/s/photos/electronics?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

我们目前生活在一个被机器学习模型包围的世界。在一天中，你对这些模型的利用比你意识到的要多。日常任务，如滚动社交媒体，拍照，查看天气，都依赖于机器学习模型。你甚至可能会看到这个博客，因为一个机器学习模型向你建议了这一点。

![](img/7136bb0c42fda2231a0ffbb1a99eaed7.png)

照片由[泰勒维克](https://unsplash.com/@tvick?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/data-center?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

我们都知道训练这些模型在计算上是昂贵的。但是大多数时候，在这些模型上运行推理在计算上也是昂贵的。我们使用机器学习服务的速度，我们需要足够快的计算系统来处理它。因此，这些模型中的大多数都运行在由 CPU 和 GPU(有时甚至是 TPU)集群组成的大型数据中心上。

> 大并不总是更好

当你拍照时，你希望机器学习的神奇瞬间发生。你不想等待图像被发送到数据中心，在那里进行处理并再次发送回来。在这种情况下，您希望机器学习模型在本地运行。

当你说“Alexa”或“好的，谷歌”时，你希望你的设备能立即响应你。等待设备将您的语音发送到服务器进行处理，并检索信息。这需要时间，并削弱了用户体验。同样，在这种情况下，您希望机器学习模型在本地运行。

## TinyML 是什么

TinyML 是机器学习和嵌入式系统的一个研究领域，探索可以在小型低功耗设备(如微控制器)上运行的模型类型。它支持边缘设备的低延迟、低功耗和低带宽模型推断。标准消费类 CPU 的功耗在 65 瓦到 85 瓦之间，标准消费类 GPU 的功耗在 200 瓦到 500 瓦之间，而典型的微控制器的功耗在毫瓦或微瓦的数量级。这大约减少了 1000 倍的功耗。这种低功耗使 TinyML 设备能够在不使用电池的情况下运行数周、数月，在某些情况下甚至数年，同时在 edge 上运行 ML 应用程序。

> ML 的未来是微小而光明的

## TinyML 的优势

1.  **低延迟:**由于模型运行在边缘，数据不必发送到服务器来运行推理。这减少了输出的延迟。
2.  **低功耗:**正如我们之前讨论过的，微控制器功耗非常低。这使得它们可以长时间不充电运行。
3.  **低带宽:**由于数据不需要不断发送到服务器，所以使用的互联网带宽较少。
4.  **隐私:**由于模型运行在边缘，你的数据不会存储在任何服务器上。

## TinyML 的应用

通过总结和分析低功耗设备的边缘数据，TinyML 提供了许多独特的解决方案。尽管 TinyML 是一个新兴领域，但它已经在生产中使用了很多年。“OK Google”、“Alexa”、“嘿 Siri”等唤醒词就是 TinyML 的一个例子。在这里，设备总是开着的，并分析你的声音来检测唤醒词。这里我再补充一些 TinyML 的应用。

1.  **工业预测性维护:**机器容易出故障。在低功耗设备上使用 TinyML，可以持续监控机器并提前预测故障。这种预测性维护可以显著节约成本。澳大利亚初创公司 Ping Services 推出了一种物联网设备，该设备通过磁性附着在涡轮机外部，并在边缘分析详细数据，来自主监控风力涡轮机。该设备可以在潜在问题发生之前就向当局发出警报。
2.  **医疗保健:**太阳能驱蚊项目利用 TinyML 遏制登革热、疟疾、寨卡病毒、基孔肯雅热等蚊媒疾病的传播。它的工作原理是检测蚊子滋生的条件，并搅动水以防止蚊子滋生。它依靠太阳能运行，因此可以无限期运行。
3.  **农业:**Nuru 应用程序通过使用 TensorFlow Lite 在设备上运行机器学习模型，只需拍摄一张照片，就可以帮助农民检测植物中的疾病。因为它在设备上工作，所以不需要互联网连接。这对于偏远地区的农民来说是一个至关重要的要求，因为他们所在的地方可能没有合适的互联网连接。
4.  **海洋生物保护:**智能 ML 供电设备用于实时监控西雅图和温哥华周围水道的鲸鱼，以避免繁忙航道上的鲸鱼袭击。

## 我该如何开始？

1.  **硬件:****Arduino Nano 33 BLE Sense**是在 edge 上部署机器学习模型的建议硬件。它包含一个 32 位 ARM Cortex-M4F 微控制器，运行频率为 64MHz，具有 1MB 程序存储器和 256KB RAM。这个微控制器提供足够的马力来运行 TinyML 模型。Arduino Nano 33 BLE 传感器还包含颜色、亮度、接近度、手势、运动、振动、方向、温度、湿度和压力传感器。它还包含一个数字麦克风和一个蓝牙低能耗(BLE)模块。这种传感器套件对于大多数应用来说已经足够了。
2.  **机器学习框架:**迎合 TinyML 需求的框架屈指可数。其中， **TensorFlow Lite** 最受欢迎，拥有最多的社区支持。使用 TensorFlow Lite Micro，我们可以在微控制器上部署模型。
3.  **学习资源:**由于 TinyML 是一个新兴领域，所以到目前为止学习资料还不多。但也有少数优秀的资料像 Pete Warden 和 Daniel Situnayake 的书《TinyML:在 Arduino 和超低功耗上用 TensorFlow Lite 进行机器学习》[哈佛大学关于 TinyML 的课程 Vijay Janapa Reddi](https://www.edx.org/professional-certificate/harvardx-tiny-machine-learning) 和 [Digikey 关于 TinyML 的博客和视频](https://www.digikey.in/en/maker/projects/intro-to-tinyml-part-1-training-a-model-for-arduino-in-tensorflow/8f1fc8c0b83d417ab521c48864d2a8ec)。

## 结论

微控制器无处不在，它们收集大量的数据。有了 TinyML，我们可以利用这些数据来构建更好的产品。今天有超过 2500 亿个微控制器单元，并且这个数字在未来只会增加。这会导致价格降低(因为，经济学！咄！).在微控制器中实现机器学习将开启新的机遇。

## 关于我

我是一个对 AI 研发感兴趣的学生。我喜欢写一些较少谈论的人工智能主题，如联邦学习，图形神经网络，人工智能中的公平，量子机器学习，TinyML 等。关注我，了解我未来所有博客的最新动态。

你可以在 [Twitter](https://twitter.com/CleanPegasus) 、 [LinkedIn](https://www.linkedin.com/in/arunkumar-l/) 和 [Github](https://github.com/CleanPegasus) 上找到我。