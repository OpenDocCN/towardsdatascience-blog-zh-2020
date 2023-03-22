# 使用 Python 简化 URL

> 原文：<https://towardsdatascience.com/best-apis-for-url-shortening-using-python-2db09d1f86f0?source=collection_archive---------11----------------------->

## 我们将讨论并学习如何使用各种 Python APIs，只用几行代码来缩短 URL。

![](img/b75ad1215e42e266504fb910ca835559.png)

奥比·奥尼耶德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

读者你好！所以，你会在各种地方(社交媒体、网站、信息平台等)看到短网址。).短网址容易记忆或输入，所以很受欢迎。没有人喜欢长 URL，所以我们经常需要缩短冗长的 URL。

你可能已经在网上使用过各种 URL 缩短服务，它们都做得很好！甚至谷歌表单，LinkedIn 等等。，缩短 URL 以方便使用。所以，它是互联网上广泛使用的东西。

那么，你有没有想过或者尝试过制作自己的网址缩短器呢？希望有许多可用的库和 API 来帮助我们以编程方式做同样的事情，而不需要访问任何网站和使用任何人的服务。

我们可以根据需要用 Python 语言编写程序。然后我们可以给出一个长的 URL 作为输入，我们会得到短的 URL 作为输出，这也只需要很少几行代码。是不是很刺激？使用各种 API 可以非常容易地做到这一点，而无需深入研究复杂的主题。

因此，有各种各样的 API 可以完成这项工作，所以让我们来看看一些 API，让我们实现它们，看看我们如何使用它们来缩短链接。

## 一点点网址缩写

Bitly 网址缩写是非常简单的使用。你需要在[上做一个小小的账户](https://bitly.com/)。然后，进入群组设置，点击高级设置。在那里你会找到 API 选项。因为 API 现在已经贬值，所以单击 OAuth 选项。然后，生成 OAuth 令牌。复制令牌。

现在，安装 *bitly_api* 。为此，单击[这个](https://github.com/bitly/bitly-api-python)链接并下载存储库。然后，将其解压缩，并通过以下操作移入文件夹:

```
cd bitly-api-python-master
```

然后，你会在文件夹里面；然后，您需要使用以下命令:

```
python setup.py install
```

现在，安装工作已经完成。 *bitly_api* 现在已经安装在您的机器上了。现在，让我们转移到真正的编码部分，这是非常容易的，只有几行。

所以，如你所见，这样做很简单。我们首先需要在代码中导入 *bitly_api* 。接下来，我们需要放入我们之前生成的访问令牌，并调用包含该访问令牌的连接。

现在，我们需要为用户请求链接。接下来，我们将通过调用之前通过调用 Connection 创建的*访问*的 shorten 函数来缩短它。

然后，我们将只打印 *short_url* 函数的*‘URL’*部分。它也有各种其他信息，如哈希，完整的链接和其他信息，我们不需要。

是的，我们完成了！使用 Bitly URL Shortener API 来缩短链接非常容易。

## 严格的网址缩写

Cuttly URL Shortener 是另一个我们可以使用的很棒的 URL Shortener。它也很容易使用，虽然需要 2-3 行代码，但不需要安装，所以总体上更简单。

首先，搬到[并注册一个新账户。接下来，点击编辑配置文件，并点击生成新的 API 密钥。这将生成一对新的 API 密钥供我们使用。复制那些 API 键。](https://cutt.ly/)

因此，我们可以直接跳到代码中，而不需要安装任何东西。虽然我们需要一个简单的安装，我想我们大多数人已经安装了。

```
pip install requests
```

因此，如果您以前没有安装这个简单的库，现在就安装吧。接下来，让我们来看看它的代码。

如你所见，我们从将请求导入代码开始。接下来，我们输入 API 密钥。然后我们要求用户输入 URL。此外，我们需要指定 api_url 参数。然后，我们把它发给请求获取数据的人。

如果数据是有效的，那么我们获取数据的 *shortLink* 部分，即缩短的 URL，并打印出来。如果无效，我们返回一个错误。

## 皮肖特纳

pyshortener 是一个 python 模块，可以使用它的访问键来使用各种 URL shortener 服务。我们不需要为不同的提供者安装单独的库。例如，我们可以使用 *Google URL shortener、Bitly shortener、Adf.ly shortener 等*。

这也有助于我们从缩短的 URL 中获取原始 URL。所以，它有双重用途。

要使用任何缩短服务，我们首先需要注册该服务并获得其访问令牌，就像我们在最后两种方法中所做的那样。

然后，我们需要为 pyshorteners 安装 python 模块。

```
pip install pyshorteners
```

在这个例子中，我们将使用 Bitly shortener。我们已经以不同的方式使用了 Bitly，所以让我们以不同的方式尝试相同的提供者。

所以，你可以在下面看到使用 pyshorteners 模块是多么容易。这非常简单，只需要很少几行代码。将收到的访问令牌放入 Bitly OAuth 中。然后只需输入需要缩短的链接。

此外，正如你所看到的，我们也可以很容易地扩展短链接。只需输入短网址，使用*。展开*以我们使用的相同方式展开短 URL。*短*将其缩短。

所以，以同样的方式，你可以使用各种缩短服务提供商做同样的工作。

所以，我希望你今天学到了一些新东西，你会尝试一些其他的网址提供商，如 adf.ly，Google Shortener 等。你也可以用它来尝试其他各种复杂的事情。

是最基础的部分，也是最重要的部分。你也可以在现有的网站或应用程序中使用它来提高工作效率，或者只是玩玩它，找点乐子！

希望你喜欢这篇文章。在完成这篇文章之后，这里还有一些其他的好文章可以阅读！

[](https://javascript.plainenglish.io/build-a-blog-app-with-react-intro-and-set-up-part-1-ddf5c674d25b) [## 使用 React 构建一个博客应用程序——介绍和设置(第 1 部分)

### 在第一部分中，我们处理项目的基础并设置它。

javascript.plainenglish.io](https://javascript.plainenglish.io/build-a-blog-app-with-react-intro-and-set-up-part-1-ddf5c674d25b) [](https://shubhamstudent5.medium.com/build-an-e-commerce-website-with-mern-stack-part-1-setting-up-the-project-eecd710e2696) [## 用 MERN 堆栈构建一个电子商务网站——第 1 部分(设置项目)

### 让我们使用 MERN 堆栈(MongoDB，Express，React 和 Node)建立一个简单的电子商务网站，用户可以在其中添加项目…

shubhamstudent5.medium.com](https://shubhamstudent5.medium.com/build-an-e-commerce-website-with-mern-stack-part-1-setting-up-the-project-eecd710e2696) [](https://medium.com/javascript-in-plain-english/build-a-simple-todo-app-using-react-a492adc9c8a4) [## 使用 React 构建一个简单的 Todo 应用程序

### 让我们用 React 构建一个简单的 Todo 应用程序，它教你 CRUD 的基本原理(创建、读取、更新和…

medium.com](https://medium.com/javascript-in-plain-english/build-a-simple-todo-app-using-react-a492adc9c8a4) 

如果您对 Django 和 Django Rest 框架感兴趣，请尝试这些文章系列:

[](/build-a-blog-website-using-django-rest-framework-overview-part-1-1f847d53753f) [## 使用 Django Rest 框架构建博客网站——概述(第 1 部分)

### 让我们使用 Django Rest 框架构建一个简单的博客网站，以了解 DRF 和 REST APIs 是如何工作的，以及我们如何添加…

towardsdatascience.com](/build-a-blog-website-using-django-rest-framework-overview-part-1-1f847d53753f) [](/build-a-social-media-website-using-django-setup-the-project-part-1-6e1932c9f221) [## 使用 Django 构建一个社交媒体网站——设置项目(第 1 部分)

### 在第一部分中，我们集中在设置我们的项目和安装所需的组件，并设置密码…

towardsdatascience.com](/build-a-social-media-website-using-django-setup-the-project-part-1-6e1932c9f221)