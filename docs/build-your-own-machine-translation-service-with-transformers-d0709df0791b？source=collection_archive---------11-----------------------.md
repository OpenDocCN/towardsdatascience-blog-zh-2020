# 用变形金刚构建你自己的机器翻译服务

> 原文：<https://towardsdatascience.com/build-your-own-machine-translation-service-with-transformers-d0709df0791b?source=collection_archive---------11----------------------->

## 使用 Transformers 库中可用的最新赫尔辛基 NLP 模型创建标准化的机器翻译服务

企业环境中需要机器翻译。全球公司能够用各种不同的语言与世界各地的人们共享文档、笔记、电子邮件和其他文本，这一点至关重要。

可以说，更重要的是需要在全球化的同时使信息民主化。不管你说什么语言，你都应该和那些主要语言被广泛使用的人一样，可以访问相同的开源出版物、医疗保健信息、工具和知识，比如英语。

![](img/4f6f1816baefd15cdfb69fdc36864849.png)

我用 powerpoint 做了这个。太恐怖了。

幸运的是，机器翻译已经存在了一段时间——我们中的许多人已经使用谷歌翻译来确认在线内容，或者只是为了听起来聪明和世故。如果你的翻译需要规模，Azure、AWS 和 GCP 有每百万字符 10 美元的可消费 API，支持*千*个语言对(一个语言对由源语言和目标语言组成)。

在开源领域，已经有大量跨多种语言的机器学习工作，人们早就能够训练自己的机器翻译模型。然而，就建模方法和支持的语言数量而言，大多数方法似乎是不同的(根据我自己天真的评估)。

最近， [Huggingface 发布了来自赫尔辛基大学](https://twitter.com/huggingface/status/1260942644286537728?s=20)的超过[1000 名预先训练好的语言模型。对于任何希望创建自己的 AWS 或 Google translate API 的人来说，这从未如此简单。所以，我想我应该利用别人的辛勤工作。这在功能上相当于“让我们把机器学习包装在一个 flask API 和 docker 映像中”，但是，不管怎样，这很有趣。让我们开始吧。](https://huggingface.co/models?search=Helsinki-NLP)

## 用变压器进行机器翻译

Huggingface 做了一件令人难以置信的工作，用一个简单的 Python API 为像我这样的复制+粘贴编码者提供了 SOTA(最先进的)模型。要在本地翻译文本，你只需要`pip install transformers`然后使用下面来自[变形金刚文档](https://huggingface.co/transformers/model_doc/marian.html)的片段。

这将下载所需的模型并翻译源文本->目标文本。有了这种格式支持的语言的绝对数量，把它扔进一个 dockerized flask 应用程序并结束它是非常容易的，但是我们确实有一点额外的工作要做…

请随意跟进 [git 回购](https://github.com/kylegallatin/machine-translation-service)。

## 下载模型

当您初始化上面的模型时，如果您本地没有所需的文件，transformers 会下载它们。这对于本地使用来说是很棒的，但是在使用 docker 容器时就不是这样了。因为容器存储是短暂的，每次我们退出容器时，我们都会丢失所有的模型，并且需要在下一次重新下载它们。

为了避免这种情况，我们可以单独处理模型下载，并将模型作为一个卷挂载。这也给了我们更多的控制语言，我们希望我们的服务从一开始就支持。Huggingface 保存了 S3 所有必要的文件，所以我们可以将它标准化…

然后创建一个简单的命令行实用程序来下载它们:

酷毙了。现在我们可以用一个命令下载我们需要的模型。以下面的日语->英语为例。

`python download_models.py --source ja --target en`

默认情况下，这会将它们下载到一个名为`data`的目录中，所以只需检查以确保该目录存在。

## Python 中的动态语言翻译

现在我们有了一个更好的方法来管理我们支持的语言，让我们来看看问题的实质，然后是一个类和相关的方法来管理我们的翻译。

这里我们需要几个函数:

*   翻译给定源语言和目标语言的文本(duh)
*   将我们没有的模型加载并管理到内存中(我使用一个简单的字典)
*   获取支持的语言(我们为此应用下载了哪些语言？)

由于 transformers 的用户友好性，几乎不需要任何时间就可以将这种功能封装到一个轻量级的类中供我们使用。

这个类用我们用来保存模型的路径初始化，并自己处理其余的部分。如果我们收到一个翻译请求，但是我们的内存中没有这个模型，那么`load_models`就会被调用来把它加载到`self.models`字典中。这将检查我们`data`目录以查看我们是否有该模型，并返回一条消息让我们知道我们是否有。

## 使其成为 API

我们现在需要做的就是将它包装在 flask 中，这样我们就可以对它进行 HTTP 调用。

要使用它，只需运行`python app.py`，然后调用服务。要检查它是否工作，你可以`curl localhost:5000`或者使用更复杂的东西，比如 Postman。我在这里使用 flask 服务器，但是您会希望一个生产 web 服务器在任何实际的地方使用它。

## 用 Docker 图像包装它

现在，我们的应用程序可以与基本 Python 一起工作，我们希望它可以与 Docker 或 docker-compose 一起工作，这样我们就可以根据需要扩展它。我们实际使用的`Dockerfile`非常简单。我们只需要确保在附加了卷的情况下运行服务，这样它就可以访问数据了。

我通常会使用较小的基本映像，但老实说，我不喜欢调试:

构建和运行命令:

```
docker build -t machine-translation-service .
docker run -p 5000:5000 -v $(pwd)/data:/app/data -it machine-translation-service
```

同样，我们可以通过调用服务来测试我们的端点。如果我已经下载了一个像 en -> fr 这样的语言路径，那么我应该能够使用`curl`进行如下 API 调用:

```
curl --location --request POST '[http://localhost:5000/translate'](http://localhost:5000/translate') \
--header 'Content-Type: application/json' \
--data-raw '{
 "text":"hello",
 "source":"en",
 "target":"fr"
}'
```

## 现在是 docker-compose

我将我的服务包装在一个小 flask API 中，甚至“正确地”使它成为一个 Docker 容器，可以以可复制的方式扩展。但是，我想分享一下这种方法的一些问题。

这些翻译模型相当大(每一个都有 300MB)。即使数据/模型是单独下载的，我们仍然需要在内存中加载我们想要支持的每一个语言对——这对于一个容器来说很快就会失去控制。

因此，为什么不创建一个可配置的映像，我们可以使用 docker-compose 为每个服务创建一个容器呢？这样，每个语言对就有一个服务，并且每个语言对都可以随着需求的增加而单独扩展。然后，我们可以编写一个单独的、公开的 API，将请求传递给网络中的所有容器。

> 免责声明:在这一点上，我只是开始边走边编，我绝不相信这是最佳的方法——但我想看看我能走多远。

首先，我稍微修改了一下目录。为了验证这一点，您可以探索我创建的 git 上的[分支:](https://github.com/kylegallatin/machine-translation-service/tree/docker-compose-flow)

`proxy`的目的是将请求重定向到每个运转起来的机器翻译服务。这将是唯一暴露的端点。我为支持 en - > fr 翻译的服务做了一个快速的`docker-compose`文件。

每个翻译服务都有相同的 env 变量(我可能只需要做一个`.env`文件)，相同的命令，以及包含我们模型的相同的卷。可能有一个更好的方法，用`yaml`自动化或类似的东西来扩展这个，但是我还没有做到。

之后，我只是对我的代理的`/translate`端点做了一些难看的工作，以便按需构造所请求的服务端点。当用户向这个公开的服务发出请求时，它将向只能从该网络内部到达的其他容器发出另一个请求。

我们所要做的就是构建基础映像。

```
cd translation_base
docker build -t translation_base 
```

然后启动服务。

```
docker-compose up
```

还有吧嗒吧嗒吧嗒。下面是使用 postman 的输出截图。

![](img/35ea7304550c908bf07b94e70e9add73.png)

## 最后是 Kubernetes

这里不打算深入探讨，但合乎逻辑的下一步是将它带到 kubernetes 进行真实的缩放。一个简单的起点是使用`kompose` CLI 将 docker-compose 转换成 kubernetes YAML。在 macOS 上:

```
$brew install kompose
$kompose convertINFO Kubernetes file "translation-proxy-service.yaml" created 
INFO Kubernetes file "en-fr-deployment.yaml" created 
INFO Kubernetes file "en-fr-claim0-persistentvolumeclaim.yaml" created 
INFO Kubernetes file "fr-en-deployment.yaml" created 
INFO Kubernetes file "fr-en-claim0-persistentvolumeclaim.yaml" created 
INFO Kubernetes file "translation-proxy-deployment.yaml" created
```

这应该会创建一些将服务和部署扩展到 K8s 所需的 YAML 文件。

下面是 fr-en 服务的示例部署:

一旦配置好 kubernetes 集群，您就可以`kubectl apply -f $FILENAME`创建您的服务、部署和持久卷声明。

当然，还有更多的工作要做，还有更多的 kubernetes 对象要创建，但我想在这里为任何真正希望扩展类似内容的人提供一个简单的起点。

## 结论

我希望 Huggingface 继续构建的工具(以及专门的研究人员训练的模型)继续提供智能机器学习的公平途径。开放机器翻译的努力不仅有助于研究领域，而且使全世界能够访问用单一语言编写的极其重要的资源。

我不知道自己托管这些庞大的模型是否比使用 AWS 或谷歌翻译 API 便宜一半，我也没有探索过质量。但这是一个超级有趣的话题，希望能给你提供一些见解，让你知道如何利用唾手可得的数以千计的 SOTA 机器学习模型。