# 如何从亚马逊 SageMaker 上读取 S3 的数据文件

> 原文：<https://towardsdatascience.com/how-to-read-data-files-on-s3-from-amazon-sagemaker-f288850bfe8f?source=collection_archive---------1----------------------->

## 将您的数据科学工作流保存在云中

![](img/a32f325de428adeae82b8ad13d45a5ea.png)

[萨彦纳特](https://unsplash.com/@sayannath?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

[亚马逊 SageMaker](https://aws.amazon.com/sagemaker/) 是由亚马逊网络服务公司(AWS)提供的强大的云托管 Jupyter 笔记本服务。它用于创建、训练和部署机器学习模型，但它也非常适合进行探索性数据分析和原型制作。

虽然它可能不像一些替代品那样对初学者友好，如 [Google CoLab](https://colab.research.google.com/) 或 [Kaggle Kernels](https://www.kaggle.com/kernels) ，但有一些很好的理由让你想在 Amazon SageMaker 内从事数据科学工作。

我们来讨论几个。

## 托管在 S3 的私有数据

机器学习模型必须在数据上训练。如果您正在处理私有数据，那么在访问这些数据进行模型训练时必须特别小心。将整个数据集下载到您的笔记本电脑上可能违反您公司的政策，或者可能是轻率的。想象一下，您的笔记本电脑丢失或被盗，而您知道其中包含敏感数据。顺便提一下，这是你应该总是使用磁盘加密的另一个原因。

云中托管的数据也可能太大，不适合放在你个人电脑的磁盘上，所以更简单的做法是将数据托管在云中，直接访问。

## 计算资源

在云中工作意味着您可以访问强大的计算实例。AWS 或您首选的云服务提供商通常会允许您选择和配置您的计算实例。也许你需要高 CPU 或高内存——比你的个人电脑所能提供的更多。或者您可能需要在 GPU 上训练您的模型。云提供商提供了许多不同类型的实例。

## 模型部署

如何直接从 SageMaker 部署 ML 模型是另一篇文章的主题，但是 AWS 为您提供了这个选项。您不需要构建复杂的部署架构。SageMaker 将剥离一个托管计算实例，该实例在一个用于执行推理任务的 API 后面托管一个经过训练的 ML 模型的 Dockerized 版本。

![](img/eab4ed955b039d2aef9fbaa24eee4331.png)

由考特尼·摩尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 将数据加载到 SageMaker 笔记本中

现在让我们进入这篇文章的主题。我将向您展示如何使用 Python 加载保存为文件的数据到 S3 桶中。示例数据是我想加载到我的 SageMaker 笔记本中的 Python 字典。

加载其他数据类型(如 CSV 或 JSON)的过程类似，但可能需要额外的库。

## 第一步:知道你把文件放在哪里

你需要知道 S3 桶的名字。文件在 S3 桶中被表示为“键”，但是从语义上来说，我发现仅仅从文件和文件夹的角度来考虑更容易。

让我们定义文件的位置:

```
bucket = 'my-bucket'
subfolder = ''
```

## 步骤 2:获得从 S3 桶中读取的权限

SageMaker 和 S3 是 AWS 提供的独立服务，一个服务要对另一个服务执行操作，需要设置适当的权限。谢天谢地，预计 SageMaker 用户将从 S3 读取文件，所以标准权限是没问题的。

尽管如此，您仍然需要导入必要的执行角色，这并不难。

```
from sagemaker import get_execution_role
role = get_execution_role()
```

## 步骤 3:使用 boto3 创建一个连接

Python 库旨在帮助用户以编程方式在 AWS 上执行操作。它将有助于连接 S3 桶的 SageMaker 笔记本。

下面的代码列出了 S3 存储桶上特定子文件夹中包含的所有文件。这对于检查存在什么文件很有用。

如果要遍历许多文件，可以修改这段代码，用 Python 创建一个列表对象。

## 第 4 步:直接从 S3 存储桶加载经过酸洗的数据

Python 中的`pickle`库对于将 Python 数据结构保存到文件中很有用，以便以后可以加载它们。

在下面的例子中，我想加载一个 Python 字典，并将其赋给`data`变量。

这需要使用`boto3`来获得我想要加载的 S3 上的特定文件对象(pickle)。注意例子中的`boto3`客户端如何返回包含数据流的响应。我们必须用`pickle`库将数据流读入`data`对象。

与使用`pickle`加载本地文件相比，这种行为有点不同。

因为这是我总是忘记如何做好的事情，所以我将这些步骤编入了本教程，以便其他人可以受益。

## 替代方法:下载文件

有时，您可能希望以编程方式从 S3 下载文件。也许您想将文件下载到本地机器或连接到 SageMaker 实例的存储器中。

为此，代码略有不同:

# 结论

在这篇文章中，我主要关注 Amazon SageMaker，但是如果你在本地机器上正确安装了`boto3` SDK，你也可以从 S3 那里读取或下载文件。由于我自己的大部分数据科学工作都是通过 SageMaker 完成的，您需要记住设置正确的访问权限，所以我想为其他人(以及我未来的自己)提供一个资源。

显然 SageMaker 不是镇上唯一的游戏。如今有各种不同的云托管数据科学笔记本环境可供选择，与五年前(2015 年)我完成博士学位时相比，这是一个巨大的飞跃

我没有提到的一个考虑是成本:SageMaker 不是免费的，而是按使用量计费的。完成后，记得关闭笔记本实例。

如果你喜欢阅读这样的故事，并想支持我成为一名作家，可以考虑报名成为一名媒体成员。一个月 5 美元，这样你就可以接触到我所有的作品以及成千上万其他作家的作品。如果你使用[我的链接](https://mikhailklassen.medium.com/membership)注册，我会赚一小笔佣金，不需要你额外付费。

[](https://mikhailklassen.medium.com/membership) [## 通过我的推荐链接加入 Medium 米哈伊尔·克拉森

### 阅读米哈伊尔·克拉森(以及媒体上成千上万的其他作家)的每一个故事。您的会员费直接支持…

mikhailklassen.medium.com](https://mikhailklassen.medium.com/membership)