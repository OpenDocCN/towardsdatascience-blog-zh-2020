# 使用 Microsoft Custom Vision 进行文档分类

> 原文：<https://towardsdatascience.com/document-classification-with-custom-vision-handwriting-typed-text-and-signatures-d1f75dee21d3?source=collection_archive---------17----------------------->

## 使用微软的自定义视觉服务根据手写、键入的文本和签名进行分类

# 介绍

从实体文件向数字文件转变的一个巨大优势是现在可以使用快速有效的搜索和知识提取方法。具有自动实体和知识提取的 NLP 驱动的应用程序现在可以很容易地从文档中提取特定的短语，甚至是一般的概念。虽然文件本身的类型可能有所不同——合同、表格、发票等。即使在每一类文档中，这些特征也常常包含很大程度的可变性。

![](img/60eef52784cbc3b0ce85a775eeaa12b2.png)

要提取的文本的特征在多个维度上有所不同。该实验集中于基于手写、键入文本和签名的存在的分类

具有挑战性的是:

1.  当将传统的搜索和知识提取方法容易地应用于这些文档时，实现可接受的提取准确性。
2.  创建一个健壮的模型，以推广到所有文档

因此，导出结构化见解、帮助文档搜索和从文档的非结构化源提取知识的文档智能管道通常依赖于由多层分类和提取模型组成的集成方法，以基于适当的信号将文档路由到正确的模型。

![](img/6bb00906fe901706d4a7cef2a8041a46.png)

本文只关注分类层，并展示了如何使用 Azure Custom Vision 服务基于手写、键入的文本和签名进行文档分类。

# 为什么要手写、打字和签名？

分类和提取中的主要挑战是较差的文档图像质量和手写部分/注释。当以预期的格式呈现正确的数据时，所有基于现代 AI/ML 的模型都表现良好。模型如何处理异常值、意外数据点，并能够对大量输入文档进行归纳，这成为一个显著因素。

图像质量差的原因在于，这些文档通常是已签署协议的扫描副本，以 pdf 格式存储，通常与原件相差一两代。这将导致许多光学字符识别(OCR)错误，从而引入伪像和错误的单词。

对于手写文本，易读性、风格和方向变化很大；并且手写可以出现在机器打印的合同页上的任何位置。手写注释也用于修改或定义关键部分/项目，例如，合同中的协议条款。手写指针和下划线通常还会指出手写内容应该合并到文档的其余打印文本中的位置。

# **微软定制视觉**

近年来，使用深度神经网络的计算机视觉[对象检测](https://en.wikipedia.org/wiki/Object_detection)模型已被证明在许多不同的对象识别任务中是有效的，但需要大量专业标记的训练数据。幸运的是，[转移学习](https://en.wikipedia.org/wiki/Transfer_learning)使模型能够使用预先训练的数据集，如 [COCO](http://cocodataset.org/#home) 用有限的数据创建强大的定制检测器。

有大量的预训练模型和 API 来构建对象检测模型；许多解决方案遵循裁剪感兴趣的特定区域的方法，但是这通常依赖于文档模板/格式/部分/布局等。这个实验的焦点是能够创建与文档类型无关的模型。我应该注意到，虽然我通常支持构建定制模型，因为它提供了控制，但我选择了 [Custom Vision](https://www.customvision.ai/) ，因为它使用起来非常简单。

Custom Vision 服务采用 Azure 提供的预建图像识别模型，并通过提供一组用于更新的图像来定制它以满足用户的需求。所有的模型训练和预测都在云中完成，模型是预先训练好的，因此用户不需要非常大的数据集或很长的训练时间来获得良好的预测(理想情况下)。微软也有更全面的 C [计算机视觉认知服务](https://docs.microsoft.com/en-us/azure/cognitive-services/computer-vision/home)，它允许用户使用 [VOTT](https://github.com/microsoft/VoTT) 标记工具来训练你自己的定制神经网络，但是定制视觉服务对于这项任务来说要简单得多。

自定义视觉由训练 API 和预测 API 组成。这个实验使用用于训练和预测 API 的 webapp 用户界面来从已训练的模型中检索结果。您可以在此阅读更多关于定制视觉服务[的详细工作内容。](https://www.customvision.ai/)

![](img/b1a5532284d2b146e3ef9b14b82d47ae.png)

webapp 允许我们一步就将整批图像(页面)标记到手写类。其他类也可以这样做。

# 实验

我们的训练集总共包含 126 幅图像。按照[微软的文档](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/getting-started-improving-your-classifier)，图像最少的标签和图像最多的标签之间保持 1:2 的比例。

![](img/aafb9cf5cb36cadba9177923493e2bd4.png)

每类训练集中的图像数(页数)

您可能会注意到这里没有为带有签名的手写文本页面创建一个类。这是因为区分签名和常规笔迹需要更多的数据强调特征，如章节信息、位置信息等。此外，还值得考虑的是，定制视觉预训练的通用对象检测模型本身是否可以被调整到将签名(也基本上是手写文本)与文档页面的图像内的常规手写文本区分开的程度。

# **结果**

快速测试功能有助于检查少量样本测试文档的预测，以评估模型性能。如前所述，我们训练过的模型应该能够概括任何文档类型。训练集仅由表单中的页面组成，但应该仍然能够预测任何文档类型的类别。

![](img/c813ec29356fa559d99bb5c0f6144f99.png)

使用经过训练的模型进行快速测试，可以正确地对只有键入文本的页面的随机图像进行分类

![](img/7598be5b5a3571f4aaa7015212edefa7.png)

用另一页手写文本进行快速测试也能正确分类

一旦发布了定型模型，就可以使用预测 API 以编程方式检索结果。这里需要注意的是，这是您的模型的第一次迭代，随着您继续完善您的训练方法，您应该会看到准确性的提高。

![](img/8dce7facb445ae15683982ab97569557.png)

webapp 仪表板还显示模型准确性的指标

# 结论

利用自定义视觉服务实现基于手写、键入文本和签名的分类层对我们的文档智能渠道非常有用，帮助我们节省了培训和构建自定义实施的时间。该服务的另一个好处是可以轻松集成 Azure Cognitive Services 的更大功能套件。如果需要，带标签的文档也可以适当地发送到手写 OCR 工具的替代 API 模型。该实验的下一步将集中在如何改进模型性能和解决区分签名和笔迹的问题上。

# 参考

1.  【https://en.wikipedia.org/wiki/Object_detection 
2.  [https://en.wikipedia.org/wiki/Transfer_learning](https://en.wikipedia.org/wiki/Transfer_learning)
3.  [http://cocodataset.org/#home](http://cocodataset.org/#home)
4.  [https://docs . Microsoft . com/en-us/azure/cognitive-services/computer-vision/home](https://docs.microsoft.com/en-us/azure/cognitive-services/computer-vision/home)
5.  https://github.com/microsoft/VoTT
6.  【https://www.customvision.ai/ 
7.  [https://docs . Microsoft . com/en-us/azure/cognitive-services/custom-vision-service/getting-started-improving-your-classifier](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/getting-started-improving-your-classifier)
8.  [https://dev blogs . Microsoft . com/CSE/2018/05/07/扫描文档中的手写检测和识别-使用-azure-ml-package-computer-vision-azure-cognitive-services-ocr/](https://devblogs.microsoft.com/cse/2018/05/07/handwriting-detection-and-recognition-in-scanned-documents-using-azure-ml-package-computer-vision-azure-cognitive-services-ocr/)