# 如何赢得黑客马拉松 Tensorflow Lite 中的实时移动野火检测。

> 原文：<https://towardsdatascience.com/how-to-win-a-hackathon-real-time-mobile-wildfire-detection-in-tensorflow-lite-b0ee838a4c18?source=collection_archive---------22----------------------->

## 深度学习的好处。

# 介绍

自 2010 年以来，黑客马拉松作为编码社区中的一种文化现象，已经获得了越来越多的关注。除了学生主导的活动，脸书、谷歌和微软等行业领袖都认识到了这类活动的效用，赞助或举办了自己的黑客马拉松。黑客马拉松将密集的编码和演示项目与除臭剂、零食和睡袋组成的网络相结合，为学生提供了在高度紧张的条件下评估他们解决方案构建能力的绝佳机会。

你甚至会学到处理压力的新方法。

从本质上来说，黑客马拉松是发展一家初创公司的流水线过程的一个缩影:你自己做研究，建立 MVP，并在长达 48 小时的过程中向观众进行推介。你会经历一些情绪波动，睡眠不足，但会学到很多在压力、逆境下工作的能力，以及实现想法的能力。特别是，这种挑战的团队导向是对一个人团队合作能力的有效测试:事实上，虽然专业黑客马拉松“团队”很常见，但学习与新队友合作是对企业工程环境的更现实的模拟。

但是考虑到黑客马拉松中可能有大量的团队，我们如何脱颖而出并创建一个获奖的参赛项目呢？当然，总有运气的成分在里面，但是我们确实可以控制一些关键的方面:

*   **新颖性** —你的解决方案的独创性水平。以前做过一千次吗？请注意，新颖性同样适用于应用程序和方法——在新的应用程序领域中经过实践检验的方法仍然可以是原创的解决方案。
*   **特异性** —您的解决方案如何应对挑战陈述？例如，喷水的人工智能机器人可能不是解决投资银行问题的最佳方案。
*   **执行**——你的解决方案交付得如何？这包括演示以及演示和投球方面，后两者通常在较短的比赛中更为重要。记住，执行得好的坏主意总是胜过执行得不好的好主意。

为了在实践中说明这些观点，我们将回顾我们的获奖作品，即 48 小时新加坡 2018 年美国宇航局太空应用挑战赛的环境主题——Sammuta，一种野火管理的多模式早期检测解决方案。

![](img/182b819f537a26254e9f1af5fb954598.png)

请注意，由于我们之前已经在 GradientCrescent 的一篇独立文章中介绍过投球和演示，我们在这里不会花太多时间。我们也不会讨论团队建设和其他人际交往技巧，而是将本文的重点放在解决方案本身。

# 用于早期野火检测的深度学习

在过去的一年里，澳大利亚 T4 的野火吸引了全世界的关注。超过 1500 万英亩的土地被烧焦，超过 100 种物种现在恐怕已经灭绝，这一事件悲惨地提醒人们人为气候变化的影响。

虽然本季火灾的确切原因仍有争议，但气候变化导致野火发生率加快，气温升高和大量干燥易燃物导致火药桶点火。[加利福尼亚州是众所周知的野火中心，现在已经超越了“火灾季节”，成为一种全年现象，](https://www.nbcnews.com/news/us-news/there-s-no-more-typical-wildfire-season-california-it-may-n934521)以前安全的地区，如瑞典，现在也开始出现野火。从 2017 年到 2018 年，已有超过 2330 亿美元用于野火相关活动。这些都是你应该在演讲中利用的事实，因为用情感背景来框定问题是让听众理解你的信息的关键。 [**这在我们之前关于投球的文章中已经详细讨论过了。**](https://medium.com/gradientcrescent/aside-pitching-101-df01b7a381a5)

![](img/365c1b58f46363c2e6e1691350943929.png)

萨姆塔的开场幻灯片

我们对此问题的解决方案包括一个早期多模式野火检测模型，该模型包含三个关键部分:

*   具有传输能力的廉价传感器网格充当粗略的热探测图。
*   一种可编程的空中无人驾驶飞机，最小提升能力为 2 公斤，航程足以到达网格空间的任何一点。
*   基于视觉的反应灵敏的野火探测系统。

这直观地呈现在下面:

![](img/c119999d532f2a032e62b6dd56da6a4a.png)

所有这些组件都可以从市场上购买，从而形成一个经济高效、可快速实施的解决方案。通过为每个社区使用一次性、坚固、廉价的检测地图，我们可以确保有效覆盖，同时保持经济和节能。对空中观察无人机进行编程，使其飞向跳闸传感器的特定坐标是很容易的，之前已经为[搜索和救援服务](https://www.google.com/search?safe=off&sxsrf=ACYBGNS7kTsMGm0O_yy-68mqQuE1XnHJ2g%3A1579442990257&ei=LmMkXsO0D9TCxgPDjY7gAg&q=search+and+rescue+drone+bbc&oq=search+and+rescue+drone+bbc&gs_l=psy-ab.3..33i22i29i30.11618.12371..12714...0.2..0.107.291.3j1......0....1..gws-wiz.......0i71j0i67j0j0i20i263j0i22i30.cy2NIM66_eo&ved=0ahUKEwjDg4LT64_nAhVUoXEKHcOGAywQ4dUDCAs&uact=5) s 进行了演示。

然而，为了竞赛的目的，缩小有效演示组件的范围是很重要的。我们认为基于视觉的移动检测系统是最可行、最引人注目的解决方案。鉴于黑客马拉松的时间和资源限制，任何解决方案都必须满足以下标准:

*   计算量小且响应迅速
*   在一系列模拟野火上的高准确度(> 80%)
*   易于实施，最好在 12 小时内。
*   易于培训，便于维护和更新
*   未来更新可能的在线学习兼容性

为了满足这些要求，我们决定利用迁移学习和 Tensorflow Lite 为 Android 设备训练一个实时移动 wildfire 分类器。

# **实施**

我们最终针对 Android 设备的 Tensorflow Lite 实现，以及训练所需的 Python 脚本，可以在**[**GradientCrescent 资源库**](https://github.com/EXJUSTICE/GradientCrescent) **中找到。** [您需要将 Tensorflow (1.9 或更高版本)与 TOCO 转换器](https://www.tensorflow.org/lite/guide/python)一起安装到您的工作空间中，以尝试重新训练过程。您还需要 Android Studio 来编译和构建最终版本。apk 文件，尽管我们在存储库中提供了一个副本。**

**Tensorflow Lite 是一种**压缩技术**，旨在将标准 Tensorflow 图形模型改编为轻量级、高响应的包，适合在轻量级上实现快速性能。这些应用包括适合半一次性应用的低端移动设备。它允许使用[量化](https://www.tensorflow.org/lite/performance/post_training_quantization)转换技术，其中模型的参数可以转换为 8 位格式，显示出在减少模型大小的同时改善了延迟。在推断时，这种数据被重新转换回其原始格式，从而对分类准确性产生最小的影响。**

**我们已经在之前的文章的[中讨论过迁移学习。本质上，迁移学习是指在新的数据集上对预先训练好的神经网络进行微调。这里的原则是旧类和新数据类中的特性是共享的，因此可以应用于新的目标类。虽然使用 Tensorflow 库重新训练预训练的 MobileNetV1 网络是一个简单的过程，**我们可以通过脚本直接从终端执行迁移学习。**](https://medium.com/gradientcrescent/improving-classification-accuracy-using-data-augmentation-segmentation-a-hybrid-implementation-8ec29fa97043)**

**首先，我们定义了一些类，以便进行再训练过程。出于演示的目的，我们进行了 Google 图片搜索，获得了大约 250 张关于野火、森林和三明治的图片(毕竟幽默不会伤害任何提交的图片)。我们的再训练脚本可以在脚本 *"retrain.py "，*由官方 [Tensorflow 资源库](https://github.com/tensorflow/hub/blob/master/examples/image_retraining/retrain.py)修改而来。这个脚本可以从终端调用，对于初学者来说非常友好，因为不需要额外的脚本。**

**我们的最终模型采用了预训练的 MobileNetV1 架构，用于 224 x 224 分辨率的输入图像，具有 75%的量化。在此之前，我们对各种架构进行了广泛的测试，以便在毫秒级响应时间和分类准确性之间取得平衡。通常，体系结构越复杂，量化级别越低，模型的执行速度就越慢。**

**然后，我们用终端命令调用我们的再训练脚本，指定训练时间、特定的预训练模型架构、预处理参数以及我们的输入和输出目录:**

```
python retrain.py --image_dir=C:\tensorflow_work\Firepics --output_graph=retrained_graph.pb --output_labels=retrained_labels.txt --bottleneck_dir=bottlenecks --learning_rate=0.0005 --testing_percentage=10 --validation_percentage=10 --train_batch_size=32 --validation_batch_size=-1 --flip_left_right True --random_scale=30 --random_brightness=30 --eval_step_interval=100 --how_many_training_steps=4000 --tfhub_module [https://tfhub.dev/google/imagenet/mobilenet_v1_075_224/quantops/feature_vector/1](https://tfhub.dev/google/imagenet/mobilenet_v1_075_224/quantops/feature_vector/1)
```

**我们这里的重新训练模型是重新训练的图形输出，但是这还不是 Android 兼容的 Tensorflow Lite。tflite)文件。为了便于转换，我们必须首先使用 *"freeze.py"* 脚本冻结我们的图形文件，然后利用 [TOCO 转换器](https://www.tensorflow.org/lite/convert/cmdline_examples)通过另一个终端命令转换我们的文件:**

```
python freeze_graph.py — input_graph=retrained_graph.pb — input_checkpoint=mobilenet_v1_0.5_224.ckpt.data-00000-of-00001 — input_binary=true — output_graph=/tmp/frozen_retrained_graph.pb — output_node_names=MobileNetV1/Predictions/Reshape_1tflite_convert — output_file=retrainedlite.tflite — graph_def_file=retrained_graph.pb — inference_type=QUANTIZED_UINT8 — input_arrays=input — output_arrays=MobilenetV1/Predictions/Reshape_1 \ — mean_values=224 — std_dev_values=127
```

**转换完成后，我们将模型转移到 Android studio 中的 Android 平台。为了节省时间，我们在 Tensorflow 存储库中可用的[演示应用程序的基础上构建了我们的解决方案。](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/lite/java/demo)请注意，对应用程序、Android Studio 或 Java 的所有元素的解释超出了本教程的范围——我们将专注于构建我们的演示功能。**

**在一个简单的过程中，我们移动我们的 Tensorflow Lite 解决方案，包括标签和。tflite 模型文件，放入 Android Studio 项目中的*“assets”*资源目录。**

**![](img/f6357755d0dfc1585c7fc7dad5ac6b65.png)**

**最终模型，与标签和旧模型一起位于资产资源文件夹中。**

**然后，我们在 java 文件*中指定模型和标签的名称。***

```
@Override
protectedString getModelPath() {
  *// you can download this file from
  // https://storage.googleapis.com/download.tensorflow.org/models/tflite/mobilenet_v1_224_android_quant_2017_11_08.zip* return "retrained_graph.tflite";
}

@Override
protected String getLabelPath() {
  return "retrained_labels.txt";
}
```

**然后，我们通过修改 *camerafragment.xml* 布局文件为我们的应用程序构建了一个新的外观布局:**

**![](img/d2d2b713c824115a1f7226bbb73dd34a.png)**

**最后，我们可以为我们的分类器创建一个阈值，我们在这里使用它来启动可视化祝酒词，但也可以用于最终产品中的通信应用程序。**

```
public void splitCheck(String entry){
  String components[]  =entry.split(":");
  String mostlikely = components[0];
  String mostlikelyprob = components[1];
  resultString = mostlikely;
  resultProb = mostlikelyprob.substring(3,5);
  *//TODO managed to implement check every x seconds, DONE. Only choose the last two numbers, cast as int* if (Integer.*parseInt*(resultProb)>=65){
    match = mostlikely+ " detected";
    itemForCount = mostlikely;

  }else{
    match = "No match";
  }
```

**然后，我们可以编译我们的解决方案并构建我们的。apk 文件，并将其传输到我们的 Android 设备上。**

**这是我们最终解决方案的现场演示。**

**凭借我们的演示和幻灯片，我们能够与一个由不同背景的陌生人组成的团队一起，经过大约 12 个小时的工作，在 NASA SpaceApps 挑战赛上获得第一名。您可以在下面观看我们的全球总决赛演示视频:**

**这就把这一小段总结成了 Tensorflow Lite。在我们的下一篇文章中，我们将通过展示强化学习在一个厄运健身房环境中的效用，来重新探索强化学习。**

**我们希望你喜欢这篇文章，也希望你[看看其他许多](https://medium.com/@adrianitsaxu)涵盖人工智能应用和理论方面的文章。为了保持对 [GradientCrescent](https://medium.com/@adrianitsaxu) 的最新更新，请考虑关注该出版物并关注我们的 Github 资源库。**