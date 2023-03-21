# 部署 NLP Web 应用程序:棘手的问题

> 原文：<https://towardsdatascience.com/deploying-an-nlp-web-app-with-heroku-the-pickle-problem-f50113ed2367?source=collection_archive---------34----------------------->

![](img/6a3a674b3ff367bee0948813fac9dc95.png)

[玛吉·马德，](https://www.flickr.com/photos/maggiemuddphotography/)通过 [Flickr](https://www.flickr.com/photos/maggiemuddphotography/4347393210/in/photolist-7Cawxm-pnUB3n-nUFPvn-2hVrtDE-bteENQ-8xJkN6-21FkzyJ-6Lfm48-7VvCLn-7oQNFY-G7ssZ4-GdiPB2-Rpj4w-e5mcue-cwtvgj-GdiPzZ-Gb21Eb-f4vh7p-c2yQA3-oncDGe-x93PvQ-58Ayrf-c2yQwQ-FhRRu9-G59xpW-Gb21A3-Fi3uFH-G59xm9-Fi3uDD-2iuR8gW-25kimfP-2iuUUaX-2iuR8hs-abWTnC-q9KQnd-ZEYXaC-a93Dn6-dsywaV-23E6kd5-24JQM92-9egiHq-2gtLhYH-G59xoy-G7ssU4-82bLgY-d7Yfbu-87DkkZ-cH8ANU-7jcaRM-dB6sJx) (CC BY 2.0)

你是否已经建立了一个很酷的 NLP 模型，并试图部署到 web 上，以便向你的朋友和家人展示你有多酷？如果是这样，那么这篇文章可能会让你感兴趣！

在游戏的这个阶段，你已经完成了 Jupyter，你正在使用你的 Flask 应用程序向人们展示你的模型是多么的平庸。你的应用程序看起来很简单，但在本地可以正常工作。

你现在可能处于这样一个阶段，你已经浪费了无数个小时试图用 Google App Engine 进行部署，结果却放弃了，并开始在 Heroku 上投入无数个小时。这两种解决方案基本上都应该是一行代码，但它们并不适合您。

您最终在 Procfile 中正确地设置了端口，但却遇到了新的障碍。该应用程序将顺利部署，但不会启动，并将抛出 H10 或 H13 错误。“应用崩溃”。本地一切都正常，您已经正确设置了端口，并验证了确实有 Dynos 启动并运行。

您现在已经调试了整个过程，并且您意识到 pickle 文件中有可疑之处。你为什么要加载 pickle 文件？因为这是你保存矢量器和模型的方法。

鉴于这是一个 NLP 应用程序，您至少有一个 pickle 用于您的矢量器(因为您需要对输入文本进行矢量化)，至少有一个 pickle 用于您的模型(因为您需要基于来自矢量器的矢量进行预测)。

你开始尝试。你把它拆下来，在没有腌渍文件附带的功能的情况下运行，它就工作了。很好。看来你已经把问题隔离了。pickle 文件一定太大了，或者 pickle 文件可能有某种问题。

到目前为止，您已经通过简单地上传静态文件夹中的 pickle 文件进行了部署。尽管当您在本地运行它时这样做是可行的，但是您决定将它们全部放入 s3 桶中，并在启动时从那里访问它们。

H10 应用程序崩溃

几个小时的调试之后，您发现罪魁祸首实际上只是经过腌制的矢量器。

因为您使用了定制的预处理器和记号赋予器，所以当您加载 pickle 矢量器时，您必须在加载 pickle 的模块中使用这两个函数。

正如用户[blckknight](https://stackoverflow.com/users/1405065/blckknght)在 StackOverflow ( [link](https://stackoverflow.com/questions/49621169/joblib-load-main-attributeerror) )上比我更优雅地解释的那样:“Pickle 不转储实际的代码类和函数，只转储它们的名字。它包含了每个定义模块的名字，所以它可以再次找到它们。如果您转储在作为脚本运行的模块中定义的类，它将转储名称`__main__`作为模块名称，因为这是 Python 使用的主模块名称(如`if __name__ == "__main__"`样板代码所示)。”

这很有道理，但你已经知道这一点，因为你在最初构建应用程序时已经处理过这个问题。当问题最初出现时，由于 IDE 中更好的错误追溯，您无需任何谷歌搜索就能进行调试。因此，在主应用程序模块中已经有了预处理程序和标记器函数。为什么这他妈的又成问题了？

我不清楚具体是如何和为什么，但我清楚的是你的主文件——你运行 Flask 应用程序的那个——不是 Heroku 服务器作为 __main__ 运行的。这意味着当你加载你的 pickled 矢量器时，它会寻找类 __main__。我的 _ 预处理器，它找不到。

现在，解决方案非常简单，在上面提到的论坛帖子中有概述:重新拾取矢量器，但这次是通过导入预处理器，并且从一个可访问的位置将标记器函数从文件中取出。

现在

您可能会注意到，当您处理这些不同的文件时，模型将占用大约 50Kb，然而，您的矢量器可能会占用大约 500Mb。只是重申一下:您的矢量器可能是 500Mb +！这是疯狂的，将挫败任何部署这种模式的企图。

我在 BrainStation 的一位导师的帮助下找到了下一个技巧，在此之前，我尝试了几天不同的部署方法，最终建立了我自己的 Linux 服务器。事实证明，与大多数代码相关的问题一样，解决方案实际上一直摆在我面前。

在所选矢量器(在我的例子中是 tfidf)的文档页面上的一个小部分中，有一个小注释，说明默认情况下，由于某种不为人知的原因，fit 矢量器将所有停用词(在这种情况下，不仅是常规停用词，实际上还有所有未包含在矢量化中的标记)作为属性存储。

[完整注释](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html#sklearn.feature_extraction.text.TfidfVectorizer)声明:“酸洗时，`stop_words_`属性会变大并增加模型尺寸。此属性仅用于自检，可以使用 delattr 安全删除，或者在酸洗前设置为 None。

在我的例子中，如上所述，完整的 pickle 文件大约有 500Mb。由于没有逻辑原因而作为属性存储的无用令牌列表被删除后，它下降到大约 200Kb。那是 3 个数量级。

这是一个很小的腌渍错误案例，但我希望它能帮助人们不像我一样用头撞墙几个小时。

#泡菜# TFIDF # Heroku #烧瓶#NLP