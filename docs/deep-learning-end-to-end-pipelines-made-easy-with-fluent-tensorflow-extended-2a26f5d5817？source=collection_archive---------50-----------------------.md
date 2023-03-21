# 借助 Fluent Tensorflow Extended 简化深度学习端到端管道

> 原文：<https://towardsdatascience.com/deep-learning-end-to-end-pipelines-made-easy-with-fluent-tensorflow-extended-2a26f5d5817?source=collection_archive---------50----------------------->

## 快速 api 概述和 fluent-tfx 的自包含示例

如果这种生产 e2e ML 管道的事情对你来说似乎是新的，[请先阅读 TFX 指南。](https://www.tensorflow.org/tfx)

另一方面，如果你以前用过 TFX，或者计划部署一个机器学习模型，那你就找对地方了。

![](img/ad4e01efe48322451815778a3d30fbe0.png)

图片由[米歇尔·贾莫卢克](https://pixabay.com/users/jarmoluk-143740/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=264778)从[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=264778)拍摄

## 但是 Tensorflow Extended 已经完全有能力自己建设 e2e 管道，为什么还要麻烦使用另一个 API 呢？

*   冗长的代码定义。实际的预处理和训练代码可能与实际的管道组件定义一样长。
*   缺乏合理的违约。您必须手动指定所有东西的输入和输出。这一方面提供了最大的灵活性，但在其他 99%的情况下，大多数 io 都可以自动连接。*例如，您的预处理组件可能会读取您的第一个输入组件的输入，并将输出传递给培训。*
*   太多样板代码。通过 TFX CLI 搭建可在 4-5 个目录中生成 15-20 个文件。

## 更易于使用的 API 层的优势

*   流畅紧凑的管道定义和运行时配置。不再需要在构建管道的 300 多行庞大的函数中滚动
*   没有脚手架，很容易通过使用几行代码来设置
*   额外的有用的工具，以加快常见的任务，如数据输入，TFX 组件建设和布线
*   合理的默认值和 99% —合适的组件 IO 接线

> *声明:我是*[*fluent-tfx*](https://github.com/ntakouris/fluent-tfx)的作者

# 通过示例进行 API 概述

*这本质上是*[*fluent-tfx/examples/usage _ guide/simple _ e2e . py*](https://github.com/ntakouris/fluent-tfx/blob/master/examples/usage_guide/simple_e2e.py)*，但是请继续读下去。*

如果你已经了解 TFX 的基本情况，滚动到**管道建设**。

## 数据输入

文件`data/data.csv`实质上是 4 列:`a, b, c, lbl`。`a`和`b`是随机采样的浮点，`c`是二进制特征(`0`或`1)`)`lbl`是二进制标签(值`0`或`1`)。那是个玩具问题，只是为了演示。

## 模型代码

工程师或“用户”提供预处理、模型构建、超参数搜索和保存带有正确签名的模型的功能。我们将展示如何在另一个文件中轻松定义这些函数(比如 [model_code.py](https://github.com/ntakouris/fluent-tfx/blob/master/examples/usage_guide/model_code.py) )

我们将利用 TFX 的所有优势来完成大部分工作。

[**预处理的张量流变换**](https://www.tensorflow.org/tfx/transform/get_started) **:**

[**【tensor flow】数据集，为模型提供有效的输入:**](https://www.tensorflow.org/guide/data)

[**keras Tuner**](https://keras-team.github.io/keras-tuner/)**和** [**TFX 调谐器——用于超参数搜索和建模的训练器:**](https://www.tensorflow.org/tfx/guide/tuner)

## 管道建设

构建管道并不简单:提供一些带有 [Tensorflow 模型分析](https://www.tensorflow.org/tfx/tutorials/model_analysis/tfma_basic)的评估配置，只需使用[*fluent-tfx*](https://github.com/ntakouris/fluent-tfx/blob/master/examples/usage_guide/simple_e2e.py)；

## 滑行装置

在 TFX 支持的[不同管道上运行管道不需要额外的努力](https://beam.apache.org/documentation/runners/capability-matrix/)，也不需要额外的依赖:`PipelineDef`生产一个普通的 TFX 管道。

但是，如果您在管道函数中使用`ftfx`工具，[请确保将这个包包含在 requirements.txt beam 参数](https://beam.apache.org/documentation/sdks/python-pipeline-dependencies/)中。

# 附录:自由度和限制

自定义组件在很大程度上受到支持，但是仍然会有一些边缘情况，这些情况只适用于冗长简单的旧 TFX api。

假设与组件 DAG 布线、路径和命名相关。

## **路径**

*   `PipelineDef`需要一个`pipeline_name`和一个可选的`bucket`路径。
*   二进制/临时/暂存工件存储在`{bucket}/{name}/staging`下
*   除非另有说明，否则默认 ml 元数据 sqlite 路径设置为`{bucket}/{name}/metadata.db`
*   `bucket`默认为`./bucket`
*   推进者的`relative_push_uri`将模型发布到`{bucket}/{name}/{relative_push_uri}`

## **组件 IO 和名称**

*   一个输入，或者一个`example_gen`组件提供`.tfrecord`给下一个组件(可能是 gzipped 格式)
*   流畅的 TFX 遵循 TFX 命名的默认组件的一切。当提供定制组件时，确保输入和输出与 TFX 相同。
*   例如，您的定制`example_gen`组件应该有一个`.outputs['examples']`属性
*   当使用来自`input_builders`的额外组件时，确保您提供的名称不会覆盖默认值，例如标准 tfx 组件名称`snake_case`和`{name}_examples_provider`、`user_{x}_importer`。

## **元件接线默认**

*   如果提供了用户提供的模式 uri，它将用于数据验证、转换等。如果声明的话，生成的模式组件仍然会生成工件
*   如果用户没有提供模型评估步骤，它将不会被连线到推送器
*   默认训练输入源是转换输出。用户可以指定他是否想要原始 tf 记录
*   如果指定了超参数并声明了 tuner，tuner 仍将测试配置并生成超参数工件，但将使用提供的超参数

谢谢你一直读到最后！如果这种生产 ML 管道的事情对你来说似乎是新的，请阅读 TFX 指南。