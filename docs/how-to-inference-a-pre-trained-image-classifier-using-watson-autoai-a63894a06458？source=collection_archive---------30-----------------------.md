# 如何使用 IBM Watson NLC 服务推断预训练的图像分类器

> 原文：<https://towardsdatascience.com/how-to-inference-a-pre-trained-image-classifier-using-watson-autoai-a63894a06458?source=collection_archive---------30----------------------->

## 使用预先训练的模型进行推理

![](img/5cc1ea7c82010c15a6e093c01e37a34a.png)

约翰·西门子的图片——Unsplash

## 分类

图像分类是属于数据科学和机器学习的另一项任务，其中我们为每个图像分配一个或多个类别或种类。

## 推理

在 IBM Watson 上创建一个项目。您应该能够看到您的项目列表。单击其中一个项目开始。

![](img/a3136aece02aafa23b30bb0d201e6fc0.png)

单击“添加到项目”以添加服务

![](img/9458c9c5a139c40eb826376553c342df.png)

选择*【视觉识别模式】*

![](img/f09fe4ebc76caafadea70038c016858d.png)

如果这是您的第一次，那么您将被重定向到创建服务。点击*【此处】*

![](img/8ad7458a01157899438557d6a7277fec.png)

您可以选择一个以前的服务(如果有)或创建一个新的服务。

![](img/1041fa2970b7409e0ac2656de63a25b4.png)

选择一个符合你需求的计划。

![](img/9219ea61eea36c6aa8fcd551a5a7ea01.png)

您可以选择更改区域、计划、资源组等。我更喜欢默认值。

![](img/5a67cad5f063fdbdb6191f115bcee7aa.png)

之后，您应该能够看到三种不同的训练模型，您可以使用。

![](img/52971930487b754869262ae95599e125.png)

先说*“将军”。*点击*“常规”后，*会看到三个选项卡。概览将显示型号信息。

![](img/342a50f94ca8f0e32cc2676a6e1cbd10.png)

点击*“测试”*开始推理模型

![](img/ab14f84ab19f8ba69f1749641f35a146.png)

拖放你想要推论的图像。每幅图像都将显示其相关类别及其置信度得分。

![](img/d6eb37eef144469d555c11c55cf2fd8d.png)

点击*“实现”*，将提供几种使用终端远程推断您的模型的方法。

![](img/bc506059fa56c7fe3f2b4a98355b64db.png)

让我们试试*【食物】*模式。

![](img/c4e0fe50b93975947672724ed4fa818c.png)

*【显式】*模型预测

![](img/417b7a445801d9773975a5937fb49ae8.png)

## 资源

*   [IBM Watson AutoAI 文档](https://dataplatform.cloud.ibm.com/docs/content/wsj/analyze-data/autoai-overview.html)
*   Youtube 上的 IBM 沃森学习中心