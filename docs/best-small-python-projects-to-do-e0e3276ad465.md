# 适合初学者的最佳简单 Python 项目

> 原文：<https://towardsdatascience.com/best-small-python-projects-to-do-e0e3276ad465?source=collection_archive---------23----------------------->

## 在你的周末做这些小项目，在学习 Python 的同时获得一些乐趣。

![](img/d0be4bb54af9a24439bfe7781e57381b.png)

照片由[屋大维丹](https://unsplash.com/@octadan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

读者们好，在这个故事中，我们来谈谈制作一些有趣的 Python 项目，以便更好地了解 Python，并在玩 Python 及其令人惊叹的库时获得一些乐趣。

这些项目很小，你可以在 1-2 小时内轻松完成。这些将帮助你更好地理解 Python 及其各种库。这些都是初学者友好的，所以每个人都应该尝试一下这些项目。

所以，事不宜迟，让我们一个一个地深入研究这些项目。

## 1)文本到语音

这是一个非常简单的 python 项目，但也是一个有趣的尝试。它利用了 Python 的 Google Text to Speech API，因此我们需要首先使用下面的命令导入库:

```
**pip install gTTS**
```

现在，在我们安装了所需的库之后，我们可以构建我们的 python 程序来将文本转换成语音。

如果我们想听程序中的音频，我们可以在 python 代码中导入 os 库。

所以，让我们来写程序的代码。下面是必需的代码:

```
#importing the gTTS library **from gtts import gTTS**#Asking the user for the required text **mt = input("Enter the required text:\t")**#Setting the output language **language = ‘en’**#Converting text to speech and choosing speed as fast **voice = gTTS(text=mt, lang=language, slow=False)**#Saving the speech as mp3 file **voice.save(“conv.mp3”)**
```

因此，通过这种方式，你可以很容易地使用 Python 进行文本到语音的转换。该文件将保存在执行该 python 代码的同一文件夹中(当前工作文件夹)。

你可以用很多方法让它变得更好，比如你可以从 pdf 中提取文本，然后把它转换成语音来完整地读出 pdf。你也可以从图像中提取文本，然后做同样的事情。

那么，让我们转到第二个简单的 Python 项目。

## 2)从图像中提取文本

您可以轻松地从图像中提取文本。我们将使用开放的 CV 和 pytesseract 库。

我们需要首先安装 openCV 和 pytesseract 库。为此，我们将使用以下命令在系统中安装 openCV。

```
**pip install opencv-python**
```

要安装 pytesseract，我们需要执行更多的任务。这是教程中最难的部分。

首先，我们需要使用以下命令:

```
**pip install pytesseract**
```

然后，我们需要下载下面的文件并安装在我们的系统中。从[这里](https://github.com/UB-Mannheim/tesseract/wiki)下载最适合你系统的文件。

完成所有这些工作后，我们就可以开始编写程序的最终代码了。

```
#Importing the libraries **import cv2
import pytesseract**
**pytesseract.pytesseract.tesseract_cmd = r'C:\\Program Files\\Tesseract-OCR\\tesseract.exe'**#Opening the image with open-cv
**image = cv2.imread('pic.png')**#Extracting text from image
**st = pytesseract.image_to_string(image)**#Printing the text
**print(st)**
```

因此，在 pytesseract 库的帮助下，我们可以很容易地从图像提取器中提取文本。

它在从图像中检测文本方面具有很高的准确性。因此，用相对简单的代码，我们已经从图像提取器中获得了高精度的文本。

## 3)玩 pdf

我们可以使用 Python 对 pdf 做很多事情。我们可以将它转换成文本，分割 PDF，合并 PDF，添加水印等等。所以，让我们写一个简单的 Python 代码来使用 Python 在 pdf 上执行所有这些很酷的东西。

我们将使用 PyPDF2 库对 PDF 执行操作。这个库帮助我们获得这样做的所有能力。

因此，让我们使用以下命令来安装这个库:

```
**pip install PyPDF2**
```

该命令将把所需的软件包安装到您的系统中。现在，我们已经准备好编写 Python 代码了。

因此，首先，让我们导入包并加载所需的 pdf。

```
#Importing the package
**import****PyPDF2**#Loading the PDF file
**pdffile =****open('first.pdf', 'rb')**#Reader object to read PDFs
**Reader =****PyPDF2.PdfFileReader(pdffile)**
```

现在，让我们从 PDF 中提取一些信息。

```
#Display the number of pages
**print(Reader.numPages)**#Getting a page by page number
**page = Reader.getPage(1)**#Extracting text from the page
**print(page.extractText())**
```

我们还可以合并 PDF 文件，旋转 PDF 文件中的页面，将 PDF 文件分割成多个部分等等。但是，我们不能在这里涵盖所有这些。

有关这个库及其用法的更多信息，请访问 [PyPDF2 这里](https://pythonhosted.org/PyPDF2/)。

## 4)语言翻译

有一个神奇的 API 可以帮助我们从一种语言翻译成另一种语言。这是谷歌翻译 API。我们需要在系统中安装 goslate 来运行执行翻译所需的 Python 代码。

```
**pip install goslate**
```

现在，让我们深入研究实际的 Python 代码。我们可以翻译成多种语言，这些语言都是由谷歌翻译 API 支持的。我们将把来自用户的输入作为一个字符串，但是我们也可以把输入作为一个文本文件的形式，转换成一个字符串并对它执行操作。

此外，我们可以制作一个实时翻译器，在其中我们可以大声说话，代码将转换语言。这将需要在这个主题上做更多的挖掘，所以我们改天再讨论它。

现在，我们将构建一个简单的语言翻译器。

```
#Importing the library package
**import goslate**#Taking input from user
**text = input("Enter the text:\t")**#Calling the goslate function
**gs = goslate.Goslate()**#Translating the text to our preferred language
**trans = gs.translate(text,'hi')**#Displaying the output **print(trans)**
```

所以，上面的代码为我们做了必要的工作。它把一个英语句子翻译成印地语。你可以选择任何你喜欢的语言。

因此，通过上面的四个 Python 项目，我希望你能学到一些东西，并通过更多的选项和更深入的分析做出这些简单项目的更好版本。

将这些项目作为起始代码，并使用它们来构建更好的项目。一旦掌握了正确的技能，您还可以创建更多的 Python 项目。所以，继续练习，继续前进。

在你读完这本书之后，我想推荐几个更好的故事给你读，它们提供了好的知识，帮助你学习新的东西。

[](https://shubhamstudent5.medium.com/build-an-e-commerce-website-with-mern-stack-part-1-setting-up-the-project-eecd710e2696) [## 用 MERN 堆栈构建一个电子商务网站——第 1 部分(设置项目)

### 让我们使用 MERN 堆栈(MongoDB，Express，React 和 Node)建立一个简单的电子商务网站，用户可以在其中添加项目…

shubhamstudent5.medium.com](https://shubhamstudent5.medium.com/build-an-e-commerce-website-with-mern-stack-part-1-setting-up-the-project-eecd710e2696) [](https://medium.com/javascript-in-plain-english/build-a-rest-api-with-node-express-and-mongodb-937ff95f23a5) [## 用 Node，Express 和 MongoDB 构建一个 REST API

### 让我们使用 Node、Express 和 MongoDB 构建一个遵循 CRUD 原则的 REST API，并使用 Postman 测试它。

medium.com](https://medium.com/javascript-in-plain-english/build-a-rest-api-with-node-express-and-mongodb-937ff95f23a5) [](https://medium.com/javascript-in-plain-english/build-a-simple-todo-app-using-react-a492adc9c8a4) [## 使用 React 构建一个简单的 Todo 应用程序

### 让我们用 React 构建一个简单的 Todo 应用程序，教你 CRUD 的基本原理(创建、读取、更新和…

medium.com](https://medium.com/javascript-in-plain-english/build-a-simple-todo-app-using-react-a492adc9c8a4) [](https://shubhamstudent5.medium.com/build-a-job-search-portal-with-django-overview-part-1-bec74d3b6f4e) [## 用 Django 构建求职门户——概述(第 1 部分)

### 让我们使用 Django 建立一个工作搜索门户，允许招聘人员发布工作和接受候选人，同时…

shubhamstudent5.medium.com](https://shubhamstudent5.medium.com/build-a-job-search-portal-with-django-overview-part-1-bec74d3b6f4e) [](/build-a-social-media-website-using-django-setup-the-project-part-1-6e1932c9f221) [## 使用 Django 构建一个社交媒体网站——设置项目(第 1 部分)

### 在第一部分中，我们集中在设置我们的项目和安装所需的组件，并设置密码…

towardsdatascience.com](/build-a-social-media-website-using-django-setup-the-project-part-1-6e1932c9f221) 

感谢您阅读这篇文章。我希望你喜欢这篇文章。