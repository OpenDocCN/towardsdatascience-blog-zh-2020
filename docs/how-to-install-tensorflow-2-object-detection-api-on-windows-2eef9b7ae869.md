# 如何在 Windows 上安装 TensorFlow 2 对象检测 API

> 原文：<https://towardsdatascience.com/how-to-install-tensorflow-2-object-detection-api-on-windows-2eef9b7ae869?source=collection_archive---------10----------------------->

## 对最初的基于 Docker 的教程进行了一些修正

![](img/8f950f73893036b19309632eefe88c8c.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [You X Ventures](https://unsplash.com/@youxventures?utm_source=medium&utm_medium=referral) 拍摄。使用本教程中提供的代码进行对象检测

R 最近，我需要为正在进行的概念验证运行一个对象检测模型。由于我使用的是 TensorFlow 2，对象检测 API 看起来很合适。然而，不幸的是，安装过程比看起来要麻烦得多，而且官方文档是基于 Docker 的，这对一个普通的 pip 包来说似乎是一个不必要的负担。长话短说，我卷起袖子穿过它，一步一步修复，直到它工作。在这里，我向你介绍如何做。

**免责声明:**我并不是以任何方式反对 Docker。

# 摘要

*   先决条件
*   在 Windows 上安装 *pycocotools*
*   介绍对象检测 API
*   测试您的安装
*   结束语

## 先决条件

我假设您熟悉 Python，正在使用某种虚拟环境，并且熟悉 TensorFlow。我使用 TensorFlow 2.3 和 Python 3.7.6 进行了安装过程。前者可能会在此过程中更新，因此如果您需要特定的版本，请记住这一点。

除了 Python 环境，您还需要一个文本编辑器和一个 Git 客户机。

如果你不是 TensorFlow 用户或数据科学家，只是想让对象检测算法工作，除了 Python 之外的所有依赖项都会被安装，所以你不需要自己安装 TensorFlow。在本文的最后，我提供了一个代码片段来对一个测试图像运行对象检测，以后您可以根据需要修改它。

## 在 Windows 上安装 pycocotools

TF 的许多对象检测 API 模块依赖于 *pycocotools* 包，该包无法通过 pip 安装在 Windows 上。如果您已经安装了这个包，您可以安全地跳过这一部分。如果没有，请按照下列步骤操作:

1.  [克隆官方知识库](https://github.com/cocodataset/cocoapi)
2.  导航到 PythonAPI 文件夹并打开 [*setup.py*](https://github.com/cocodataset/cocoapi/blob/master/PythonAPI/setup.py) 文件
3.  将第 12 行编辑为`extra_compile_args=[]`。这里的基本原理是删除特定的铿锵论点，这在 MVCC 不起作用。
4.  在 CMD 终端的 PythonAPI 文件夹中，运行:
    `python setup.py build_ext --inplace`

这最后一个命令将在您当前的环境中构建和安装这个包，准备就绪。为了测试安装是否成功，启动 Python 并将其导入为:`import pycocotools`。

你可能会问，我们是否不应该添加 MVCC 特有的旗帜来取代铿锵的旗帜。我也有同样的疑惑，但是它工作得很好，我没有遇到任何错误。据我所知，最初的 Clang 标志是用来禁用某些警告并强制 C99 遵从性的。

## 安装对象检测 API

准备好 coco 工具后，我们可以转到实际的对象检测 API。遵循这些步骤(注意有些命令以点结束！):

1.  克隆 [TensorFlow 模型库](https://github.com/tensorflow/models)。这个 repo 是一组 TF 相关项目的保护伞，Object Detection API 就是其中之一。
2.  导航到”。/research/object _ detection/packages/tf2/"并编辑 [*setup.py*](https://github.com/tensorflow/models/blob/master/research/object_detection/packages/tf2/setup.py) 文件。从*必需 _ 包*列表中，删除 *pycocotools* 引用(第 20 行)。这一改变将防止安装过程试图从 pip 重新安装 *pycocotools* ，这将失败并中止整个过程。
3.  将这个 *setup.py* 文件复制到。/research”文件夹，替换已经存在的 *setup.py* 。
4.  打开*研究*文件夹中的 CMD，用
    `protoc object_detection/protos/*.proto --python_out=.`编译协议缓冲区
5.  如果前面的命令有效，应该不会出现任何内容。我知道这不是最直观的东西。之后，运行下面的:
    `python -m pip install .`

如果我解释的一切都正确，并且你严格按照字面意思去做，安装过程应该会很顺利，大量的软件包现在已经安装到你的系统中了。

如果您的系统上没有*protocol*命令，您需要做的就是下载一个[协议缓冲库](https://github.com/protocolbuffers/protobuf/releases/tag/v3.13.0)的预编译版本，并将其添加到您的系统路径(或将其放在 Windows/system32/😈).

奇怪的是，对我来说，在这一切之后，我的 *OpenCV* 和 *MatplotLib* 包不再显示数字(*cv2 . im show*/*PLT . show*)。我通过使用`pip install -U opencv-python opencv-contrib-python matplitlib`重新安装两个包修复了这个问题。如果任何其他软件包在安装对象检测 API 后出现故障，尝试像我上面做的那样从 pip 更新它。

## 测试您的安装

有两种方法可以确定您当前的安装是否正确。第一个是运行库测试套件。在*研究*文件夹下打开 CMD，运行:
`python object_detection/builders/model_builder_tf2_test.py`

它应该打印很多东西，并告诉你所有的测试都成功了。

第二种方法是下载一个模型，并尝试用它进行一些推断。这里是用于[推理](https://github.com/tensorflow/models/blob/master/research/object_detection/colab_tutorials/inference_tf2_colab.ipynb)和[训练](https://github.com/tensorflow/models/blob/master/research/object_detection/colab_tutorials/eager_few_shot_od_training_tf2_colab.ipynb)的官方协作笔记本。我不是笔记本的粉丝，所以[这里是我的测试脚本](https://gist.github.com/nuzrub/ee3dc19242915278e95cb75014e29083)，它期望一个[*test.png*](https://unsplash.com/photos/Oalh2MojUuk)文件作为输入，一个 [*coco 标签图*(你可以在这里找到)](https://github.com/tensorflow/models/blob/master/research/object_detection/data/mscoco_label_map.pbtxt)，以及 [CenterNet 模型](http://download.tensorflow.org/models/object_detection/tf2/20200711/centernet_resnet50_v1_fpn_512x512_coco17_tpu-8.tar.gz)。将所有内容放在一个文件夹中，并将下面的脚本添加到这个文件夹中:

最后，应该会弹出一个包含原始图像和检测覆盖图的图像，还应该会创建一个名为*output.png*的新文件。

这篇文章的封面图片是使用上面的脚本和来自 Unsplash 的这张[图片生成的。如果一切都设置正确，并且您使用此图像作为输入，您应该得到与我相同的结果。](https://unsplash.com/photos/Oalh2MojUuk)

我希望这有助于您使用 TensorFlow 2 对象检测 API，并使您能够使用开箱即用的模型进行推理和训练。有了上面的脚本，应该不难理解如何在您的管道上应用这个 API，并将其更改为使用其他模型。有关实现及其质量/速度权衡的列表，[请参考此列表。](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2_detection_zoo.md)

如果您对本教程有任何问题，请留下您的评论，我将非常乐意帮助您跟踪问题，并用更新的信息更新本文。

如果你刚接触媒体，我强烈推荐[订阅](https://ygorserpa.medium.com/membership)。对于数据和 IT 专业人员来说，中型文章是 StackOverflow 的完美组合，对于新手来说更是如此。注册时请考虑使用[我的会员链接。你也可以直接给我买杯咖啡来支持我](https://ygorserpa.medium.com/membership)

感谢您的阅读:)