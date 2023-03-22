# 如何用 Python 翻译 PDF(Google vs AWS Translate)——第 1 部分:提取和翻译文本

> 原文：<https://towardsdatascience.com/how-to-translate-pdf-with-python-google-vs-aws-translate-part-1-extract-and-translate-text-7946604a48f4?source=collection_archive---------13----------------------->

## [实践教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

## 如何使用谷歌翻译 API 和 AWS 翻译 API 阅读和翻译 PDF 文件的逐步指南。

![](img/8d97fc16894694123c20c44d559d0401.png)

由 [Tessakay](https://pixabay.com/users/tessakay-5116088/) 在 [Pixabay](https://pixabay.com/images/id-2317654/) 上拍摄的照片

我需要将一个包含英文文本的 PDF 文件翻译成拉脱维亚文。事实证明，这比我最初想象的稍微更具挑战性，所以我决定写一篇教程来分享我学到的东西，希望能为你节省一些时间。我把我的项目分成两部分。

这篇文章是第一部分，重点是如何阅读你的 PDF 文件，提取文本，并翻译它。它着眼于两种翻译文本的方法——使用谷歌翻译和 AWS 翻译。

第 2 部分将着眼于如何从获得的翻译创建、格式化和保存一个新的 PDF 文件。你可以在 GitHub 中找到我的项目的链接，并在本文末尾找到完整的代码。

# 这篇文章涵盖了什么？

*   如何使用 Python `PyPDF2`库读取 PDF 文件并从 PDF 中提取文本
*   2 种翻译文本的方法:python `googletrans`库和`AWS Translate.`

## 使用 Python 读取 PDF 文件

要以编程方式读取 PDF 文件，您可以使用各种第三方库，本文使用 PyPDF2 库。

PyPDF2 可以使用 pip 软件包管理器安装:

```
pip install PyPDF2
```

要读取该文件，我们首先要以二进制读取模式打开该文件，并创建一个 PdfFileReader。

```
file = open("example.pdf", 'rb')
reader = PdfFileReader(file)
```

## 从 PDF 中提取文本

现在，您可以一次一页地阅读 PDF 文件。您可以通过调用 reader 对象上的一个`numPages`属性来获得文档的页数。然后逐一遍历所有页面，提取文本。要获取页面内容，使用`reader.getPage(<page number>`，要从页面内容中提取文本，使用`extractText`方法。

```
num_pages = reader.numPagesfor p in range(num_pages):
    page = reader.getPage(p)
    text = page.extractText()
```

# 翻译文本

一旦文本被提取出来，它就可以被翻译。我很好奇将使用谷歌翻译的 python 库与 AWS 翻译器进行比较。`googletrans`库是开源的，使用它没有额外的费用。AWS Translate 使用神经机器学习算法进行翻译，并在翻译时考虑上下文。要使用 AWS 翻译，您需要有一个 AWS 帐户。他们提供前 12 个月的免费等级，在此期间你可以翻译 200 万个字符。之后，采用现收现付的定价方式，每百万字符的费用为 15 美元。请注意，免费层是在您首次使用服务时激活的。

## 使用谷歌翻译 API 翻译文本

您可以使用 pip 安装该库:

```
pip install googletrans
```

在幕后，这个库使用 **Google Translate Ajax API** 调用 Translate 方法。您也可以指定 Google 服务域 URL。在下面的例子中，我声明了两个不同服务域的列表:`translate.google.com`和`translate.google.lv.`

您需要创建一个翻译器客户端实例，然后调用`translate`方法来翻译文本。默认的目标语言是英语。可以通过在`translate`方法中指定`dest`参数来改变。默认的源语言是自动检测的，但是您可以使用`src`参数专门定义源语言。在下面复制的例子中，它自动检测源语言并翻译成拉脱维亚语。

```
from googletrans import TranslatorURL_COM = 'translate.google.com'
URL_LV = 'translate.google.lv'
LANG = "lv"translator = Translator(service_urls=[URL_COM, URL_LV])
translation = translator.translate(text, dest=LANG)
```

## 使用亚马逊 AWS 翻译工具翻译文本

您需要有一个 AWS 帐户才能使用 AWS 翻译。请注意，如果您使用 python 以编程方式使用 AWS 服务，那么最简单的方法就是使用由 AWS 提供的名为`boto3`的 python SDK。AWS 的工作方式是，它向您选择的区域中的 AWS 翻译端点发送一个 HTTP API 调用。使用您的访问密钥和秘密访问密钥以及当前时间戳对该请求进行加密签名。您确实需要配置您的`aws_access_key_id`和`aws_secret_access_key.`，它使用一种叫做 AWS Signature Version 4 的协议来签署您的请求。您笔记本电脑上的时钟必须正确，因为该协议考虑了当前时间。它会在签署您的请求时包含时间戳，并且您发送给 AWS 的请求仅在 15 分钟内有效。如果在 15 分钟后收到，那么出于安全原因，AWS 将拒绝该请求。

要安装 AWS Python `boto3` SDK:

```
pip install boto3
```

如上所述，AWS 提供了一个免费的 AWS 翻译试用层，为期 12 个月，给你 200 万字免费翻译。之后是每 100 万字 15 美元。**请注意，AWS 也将空格算作字符。**

为了翻译，我们首先为`translate`服务创建一个客户端，并指定区域。然后在客户端对象上调用`translate_text`方法，提供`Text`参数，这是您想要翻译的文本。`SourceLanguageCode`允许指定源语言。如果使用值`auto`，那么将自动检测源语言。`TargetLanguageCode`允许指定目标语言。一旦从 AWS 收到结果，还有一个额外的步骤来获取翻译后的文本，那就是调用`result.get(‘TranslatedText’).`

```
import boto3LANG = "lv"
AWS_REGION = 'eu-west-1'translate = boto3.client(service_name='translate',
                         region_name=AWS_REGION,
                         use_ssl=True)result = translate.translate_text(Text=page.extractText(),
                                  SourceLanguageCode="auto",
                                  TargetLanguageCode=LANG)
translation = result.get('TranslatedText')
```

# 结论

感谢您抽出时间阅读。我希望这篇文章对你有用。您学习了如何使用 python 阅读 PDF 文件，以及如何从 PDF 文件中提取文本。然后，您看了如何自动翻译这段文本的两种不同方式。

请关注第 2 部分，它将介绍如何使用 python 库`reportlab`将翻译后的文本保存到一个新的 PDF 文件中。它还将格式化和保存新创建的文件。

在 GitHub 中查看该项目的完整代码:[https://github.com/akapne01/translator](https://github.com/akapne01/translator)