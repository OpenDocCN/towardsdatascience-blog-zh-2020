# 使用 Python 实现 gRPC 服务器

> 原文：<https://towardsdatascience.com/implementing-grpc-server-using-python-9dc42e8daea0?source=collection_archive---------6----------------------->

## 你的下一个 API 不需要用 REST 和 JSON 来构建。gRPC 和协议缓冲区如何更好的性能和结构？

如今，当人们想要实现后端 API 时，他们会直接使用 RESTful API 创建使用 JSON 通信的应用程序，甚至不考虑其他选项。然而最近几年，gRPC 和它的 *protobufs* 由于它们的许多优点开始得到一些关注和欢迎。因此，让我们来看看有什么值得关注的，并使用 Python 实现 gRPC 服务器！

*TL；DR:这里是我的带有* `*grpc*` *分支的存储库，包含本文的所有源代码:*[*https://github . com/MartinHeinz/python-project-blue print/tree/grpc*](https://github.com/MartinHeinz/python-project-blueprint/tree/grpc)

![](img/8709d8a5e16a5854bc0c3b3931ed1716.png)

[Zak Sakata](https://unsplash.com/@zaks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的原始照片

# gRPC 到底是什么？

*gRPC* 是远程过程调用(RPC)协议，利用*协议缓冲区(protobufs)* 作为其消息格式。使用 gRPC，客户端应用程序可以使用方法存根直接调用远程服务器上的可用方法。只要有客户端语言的存根(生成),服务器端应用程序用什么语言实现并不重要。gRPC 支持多种语言，包括 Go、Java、Ruby、C#或者我们选择的语言——Python。你可以在这个[概述](https://grpc.io/docs/guides/)中找到更多信息。

现在，什么是*协议缓冲区(protobufs)* ？Protobufs 是 JSON 或 XML 等格式的替代品。它们是一种更小、更简单、更有效的序列化数据的方式。要使用 *protobufs* ，您需要定义您希望交换的消息看起来是什么样子，例如(更多细节参见[本语言指南](https://developers.google.com/protocol-buffers/docs/proto)):

除了消息，我们还需要定义将在服务器端实现并从客户端调用的`service`及其`rpc`方法:

# 我为什么要在乎呢？

与 REST 相比，使用 gRPC 有很多优点。首先，gRPC 在性能方面要好得多，这要归功于 *protobufs* 的紧密封装，它减少了发送的有效载荷的大小。它还使用 HTTP/2，而不是 REST 使用的 HTTP/1.1。由于这些原因，它是物联网、移动设备或其他受限/低功耗环境的绝佳选择。

选择 gRPC 而不是 REST 的另一个原因是 REST 不要求任何真正的结构。您可以使用 OpenAPI 定义请求和响应的格式，但是这是松散的和可选的。另一方面，gRPC 合同更加严格且定义明确。

如前所述，gRPC 使用 HTTP/2，值得一提的是它充分利用了它的特性。举几个例子:并发请求，流而不是请求-响应，对延迟的敏感度更小。

也就是说，也有缺点，最大的缺点是采用。并非所有的客户端(浏览器)都支持 HTTP/2 的使用，这使得它在外部使用时存在问题。考虑到性能优势，它显然是您可以控制的内部服务和通信的最佳选择。

# 安装

要使用 gRPC 和 *protobufs* 做任何事情，我们需要安装它的编译器:

考虑到我们使用 python 来构建我们的应用程序，我们还需要`grpcio`和`grpcio-tools`库:

# 让我们建造一些东西

准备好所有工具后，我们现在可以开始构建应用程序了。对于这个例子，我选择了一个简单的 echo 服务器，它可以向您发回自己的消息。

不过，我们首先应该讨论的是项目布局。我选择了以下目录/文件结构:

这种布局帮助我们清楚地分离 *protobuf* 文件(`.../proto`)、生成的源代码(`.../generated`)、实际的源代码和我们的测试套件。要了解更多关于如何用这种布局设置 Python 项目的信息，可以查看我以前的文章:

[](/ultimate-setup-for-your-next-python-project-179bda8a7c2c) [## 下一个 Python 项目的最终设置

### 从零开始任何项目都可能是一项艰巨的任务…但如果您有这个最终的 Python 项目蓝图就不会了！

towardsdatascience.com](/ultimate-setup-for-your-next-python-project-179bda8a7c2c) 

因此，要构建 gRPC 服务器，我们首先需要定义*消息*和*服务*，它将使用您与客户端进行通信:

在这个`echo.proto`文件中，我们可以看到一个非常简单的消息类型定义——一个用于请求(`EchoRequest`)，一个用于来自服务器的回复(`EchoReply`)。然后这些消息被由一个名为`Reply`的 RPC 方法组成的`Echo`服务使用。

为了能够在 Python 代码中使用这些定义，我们需要生成服务器和客户端接口。为此，我们可以运行以下命令:

我们指定了相当多的参数。第一个——`-I blueprint/proto`，告诉`grpc_tools`在哪里寻找我们的`.proto`文件(它定义了`PATH`)。接下来的两个，`--python-out`和`--grpc_python_out`分别指定将生成的`*_pb2.py`和`*_grpc_pb2.py`文件输出到哪里。最后一个参数——`./blueprint/proto/*.proto`是指向`.proto`文件的实际路径——这看起来有些多余，因为我们用`-I`指定了`PATH`，但是你需要两者来完成这项工作。

然而，当您运行这一个命令时，您将在这些生成的文件中结束一些中断的导入。在`grpc`和`protobuf`存储库中有多个提出的[问题](https://github.com/protocolbuffers/protobuf/issues/1491)，最简单的解决方案是用`sed`修复那些导入。

写出这个命令不会很愉快，也没有效率，所以我把它包装在`make` target 中，让你(和我)的生活更轻松。你可以在我的资源库[找到完整的`Makefile`这里](https://github.com/MartinHeinz/python-project-blueprint/blob/grpc/Makefile)。

有相当多的代码是使用这个命令生成的，所以我不会一一查看，但是有一些重要的事情需要注意。这些`*_pb2_grpc.py`文件中的每一个都有以下三样东西:

*   *存根*——第一个是*存根*，在我们的例子中是`EchoStub`——是客户端用来连接 gRPC 服务的类
*   *服务器*——在我们的例子中是`EchoServicer`——被服务器用来实现 gRPC 服务
*   *注册功能* —最后一项，`add_EchoServicer_to_server`需要向 gRPC 服务器注册服务器。

所以，让我们检查一下代码，看看如何使用这个生成的代码。

我们要做的第一件事是实现实际的服务。我们将在`grpc.py`文件中这样做，在那里我们将保存所有 gRPC 特定的代码:

上面，我们创建了从生成的`EchoServicer`类继承而来的`Echoer`类，它包含了`.proto`文件中定义的所有方法。所有这些方法都旨在我们的实现中被覆盖，这里我们就是这么做的。我们通过返回之前在`.proto`文件中定义的`EchoReply`消息来实现唯一的方法`Reply`。我想指出的是，这里的`request`参数是`EchoReply`的一个实例——这就是为什么我们可以从中取出`message`。这里我们不使用`context`参数，但是它包含一些有用的特定于 RPC 的信息，比如超时限制。

我还想提到的一个巧妙的特性是，如果您想利用*响应流*，您可以用`yield`替换`return`，并返回多个响应(在`for`周期中)。

既然我们已经实现了 gRPC 服务，我们想运行它。为此，我们需要一台服务器:

所有这些都只是一个静态方法。它用几个工人从`grpc`库创建服务器——在本例中是 10 个。之后，它使用前面提到的注册函数(`add_EchoServicer_to_server`)将我们的`Echoer`服务绑定到服务器。剩下的 3 行只是添加监听端口，启动服务器并等待中断。

对于服务器端来说，剩下的就是`__main__.py`，因此我们可以将它作为 Python 模块启动:

这样，我们就可以启动服务器了。你可以用`python -m blueprint`来做，或者如果你用的是我的模板，那么就用`make run`。

我们有一个正在运行的服务器，但是我们没有办法调用它…这就是客户端调用它的地方。出于演示目的，我们将使用为我们生成的存根用 Python 创建客户机，但是您可以用完全不同的语言编写客户机。

对于客户端，我们只需要一个函数，我们称之为`run`。在连接服务器之后，它会创建一个存根，允许我们调用服务器方法，这是下一步。它通过传入带有一些有效载荷的`EchoRequest`消息来调用在服务器端实现的`Reply`方法。剩下的就是打印了。

现在，让我们运行客户端，看看是否一切正常:

而且确实有效！

# 使用 Pytest 进行测试

正如我所有的小项目和文章一样，在对所有代码进行单元测试之前，我们还没有完成。为了编写这个 gRPC 服务器的示例测试，我将使用 *Pytest* 及其插件`pytest-grpc`。

让我们首先来看看用于模拟客户机和服务器之间的请求-响应交换的装置:

我认为这些都很简单。只要确保为这些设备使用这些特定的名称，因为这是插件所寻找的。需要注意的一点是`grpc_stub`中的`grpc_channel`论证。这是`pytest-grpc`插件提供的假频道。要了解更多信息，我建议直接去`pytest-grpc` [源代码](https://github.com/kataev/pytest-grpc/blob/master/pytest_grpc/plugin.py)，因为这个插件非常简单。现在，让我们继续实际的测试:

我们通过利用在上一步中编写的`grpc_stub` fixture 来创建这个测试。我们创建了传递给`grpc_stub.Reply`的`EchoRequest`，后面是简单的`assert`。并且，当我们运行测试(`make run`)时:

我们通过了！我们完成了！

# 结论

如果你从这篇文章中只学到一样东西，那么我认为应该是这样一个事实:当我们决定我们想要用于某个项目的解决方案/技术时，我们应该总是考虑可能的替代方案。并不总是需要 REST 和 JSON。有时 gRPC 可能更符合要求。这种想法也适用于任何其他技术或工具。要查看使用`Makefile`自动化的完整代码清单、准备好的 Docker 映像，甚至是部署到 Kubernetes 的设置，请查看我的存储库中的`grpc`分支:[https://github . com/Martin Heinz/python-project-blue print/tree/master](https://github.com/MartinHeinz/python-project-blueprint/tree/master)。感谢任何反馈，如果你喜欢这类内容，star 或 fork 也是如此。😉

*本文最初发布于*[*martinheinz . dev*](https://martinheinz.dev/blog/23?utm_source=tds&utm_medium=referral&utm_campaign=blog_post_23)