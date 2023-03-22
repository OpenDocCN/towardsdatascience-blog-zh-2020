# 没有控制，权力什么都不是

> 原文：<https://towardsdatascience.com/power-is-nothing-without-control-aa43523745b6?source=collection_archive---------40----------------------->

## 不要和 Jupyter 笔记本分手。也用 Kedro 就好！

这篇文章适合那些发现自己被 Jupyter 笔记本的易用性所吸引的人。虽然它针对的是数据科学相对较新的读者，但它同样适用于更有经验的数据科学家&工程师，他们正在考虑如何改进他们的日常工作流程。

## Jupyter 笔记本的便利性与 Kedro 的软件最佳实践相结合

我将简要介绍一下我们喜欢和不喜欢 Jupyter 笔记本的原因，并描述一下[开源 Kedro 框架](https://kedro.readthedocs.io/en/stable/01_introduction/01_introduction.html)如何帮助您解决令众多数据专业人士头疼和心痛的一些问题。而且不涉及分手！如果你不能完全放弃，你仍然可以在 Kedro 旁边使用笔记本。

![](img/bf1c3c8d00a071ac54b5ad4a7daebd11.png)

笔记本之恋:图片来自[达留什·桑科夫斯基](https://pixabay.com/users/dariuszsankowski-1441456)来自[皮克斯拜](https://pixabay.com/photos/paper-romance-symbol-valentine-1100254/)

如果你对笔记本不熟悉，或者想了解更多，这里有一个简短的历史题外话，总结自一篇名为[数据科学家喜欢 Jupyter 笔记本的 10 个原因](https://hub.packtpub.com/10-reasons-data-scientists-love-jupyter-notebooks/)的精彩文章。如果你已经知道所有你需要(或想要)的他们的历史，就跳到下一节“ ***为什么要用 Jupyter 笔记本*** ”。

早在 2011 年，IPython Notebook 作为交互式 Python 控制台的基于 web 的接口发布。它允许 Python 开发者在本地网页上操作代码、文本、数据图等等。IPython 笔记本迅速变得如此受欢迎，以至于它被扩展到包括其他编程语言，如 R 和 Julia。

2014 年，Jupyter 项目宣布以新的名称管理多种语言的 IPython 笔记本，而 IPython 继续作为 Python shell。

[维基百科解释了](https://en.wikipedia.org/wiki/Project_Jupyter)该项目的名称选择:

> “Project Jupyter 的名字是对 Jupyter 支持的三种核心编程语言的引用，这三种语言是 [Julia](https://en.wikipedia.org/wiki/Julia_(programming_language)) 、 [Python](https://en.wikipedia.org/wiki/Python_(programming_language)) 和 [R](https://en.wikipedia.org/wiki/R_(programming_language)) ，也是对伽利略记录木星卫星发现的笔记本的致敬。”

Project Jupyter 网站将自己描述为“一个非盈利、开源的项目……旨在支持跨所有编程语言的交互式数据科学和科学计算”。该项目自开始以来一直蓬勃发展，并于 2017 年获得了 [2017 ACM 软件系统奖](https://blog.jupyter.org/jupyter-receives-the-acm-software-system-award-d433b0dfe3a2)。

可以说，Jupyter 笔记本已经成为数据科学家工具箱中必不可少的一部分。

# 为什么要用 Jupyter 笔记本？

***❤️我们爱笔记本❤️*** 这是可以理解的

当你开始从事数据科学或数据工程时，有很多东西需要学习。有很多资源可以帮助你，包括在线课程。大多数都提供了示例项目，这些项目最有可能出现在笔记本中，因为它们对于初学者来说非常简单。它们使得获取代码、单步执行代码、绘制数据和实验变得特别容易。你一定会发现在笔记本上开始比设置一个 IDE 并通过脚本来使用它要简单得多。你只需要拿一个笔记本，只要你跑了`pip install Jupyter`就可以开始了。每次运行笔记本单元时，您都会立即看到代码的结果。不喜欢什么？

不可否认的是，无论你多么有经验，当你开始一个项目时，笔记本对于实验和想象都是有用的。随着你扩大生产规模或需要与他人合作，蜜月期会逐渐消失。即便如此，也有使用笔记本的方法:例如，[网飞数据团队](https://netflixtechblog.com/notebook-innovation-591ee3221233)已经描述了如何扩大笔记本的使用。但是，对许多人来说，一句谚语“没有控制，权力什么都不是”跃入脑海。

没有控制，权力什么都不是:来自倍耐力的视频

# 为什么不用 Jupyter 笔记本？

***💔这也可以理解为什么我们不再喜欢笔记本了💔***

在整个数据科学网站上，甚至仅在这个博客上，就有一系列[的](/5-reasons-why-jupyter-notebooks-suck-4dc201e27086) [的](/beyond-the-jupyter-notebook-how-to-build-data-science-products-50d942fc25d8) [文章](/5-reasons-why-you-should-switch-from-jupyter-notebook-to-scripts-cb3535ba9c95)详细介绍了 Jupyter 笔记本电脑在大型项目中成为问题的原因。一旦你到了需要使用软件最佳实践来控制你的复杂性的时候，很明显笔记本不支持那些理想。

作为一名前 C 程序员，我认识到这些缺陷来自一个强大而自由的开发环境。例如，随着代码变得越来越大，很难在单个笔记本中进行管理。大量的代码变得错综复杂。有时候，一个笔记本是不够的。假设您想一起运行多个模型，并比较结果？每个人都需要一个单独的笔记本。如何用相同的配置来设置它们，并随着实验的进展不断地改变它们呢？

假设您想要使用一组新数据进行测试，或者使用不同的方法处理现有数据，或者向您的算法输入不同的参数。你可以改变你的笔记本，但是你不能轻易地跟踪那些改变并回滚它们，如果你在你的笔记本中引入一个错误源，那就不简单了。笔记本格式不太适合源代码控制，调试也很棘手。

团队中的数据科学家遇到的另一个问题是环境再现性。假设您想与同事协作并共享您的笔记本。但是如果他们使用的是一个特定包的不同版本，比如已经随时间改变的`sklearn`，该怎么办呢？您如何向您的同事指定要设置什么环境，以便他们可以像您在自己的机器上一样运行笔记本电脑？你如何自动设置？更进一步:您希望将代码作为演示的一部分部署给团队中的许多人。但是如何管理配置和更新呢？甚至不要考虑如何发布和产品化您的笔记本电脑！

# 最佳实践:如何将 Jupyter 笔记本与优秀的工程技术相结合

与其完全扔掉你的笔记本，为什么不考虑用它们来做实验(例如探索性的数据分析)，同时在一个框架内构建你的项目呢？

早在 2019 年，我曾在这个博客的[帖子中介绍过](/kedro-prepare-to-pimp-your-pipeline-f8f68c263466) [Kedro](https://github.com/quantumblacklabs/kedro) ，尽管从那以后我们在 [readthedocs.io](https://kedro.readthedocs.io) 上创造了一些更简单的了解 Kedro 的方式。

Kedro 由高级分析公司 [QuantumBlack](https://www.quantumblack.com/) 创建，并于 2019 年开源，以扩展其对第三方数据专业人士的使用。它是一个开发工作流工具，将软件工程最佳实践应用于您的数据科学代码，因此它是可复制的、模块化的和有良好文档记录的。如果你使用 Kedro，你可以少担心学习如何写生产就绪的代码(Kedro 为你做了繁重的工作),你将标准化你的团队合作的方式，让你们所有人更有效地工作。

虽然 Kedro 有一个学习曲线，因为您将使用 Python 脚本和命令行界面，但随着您获得有组织的代码和数据的好处，它会很快得到回报。当你开始一个新的项目时，这是一个很好的时机，但是你也可以将一个现有的笔记本转化为一个 Kedro 项目，正如最近来自 DataEngineerOne 的一个视频所展示的那样。

一旦你建立了一个 Kedro 项目，你可以[使用一个 Jupyter 笔记本来开发你的代码](https://kedro.readthedocs.io/en/latest/11_tools_integration/02_ipython.html#kedro-and-jupyter)，如果你喜欢这种工作方式的话，然后把它转换成一个 Kedro `node`，这是一个 Python 函数，由 Kedro 框架在它管理`pipeline`的角色中调用。(该术语在文档中的 [Hello World 示例](https://kedro.readthedocs.io/en/latest/02_get_started/03_hello_kedro.html)中有进一步的解释，您将在设置 Kedro 安装的过程中学习该术语)。有一些简洁的扩展允许你在开发你的 Kedro 项目时在笔记本上工作，你可以找到一个视频如何使用[两个技巧使 Jupyter 笔记本在 Kedro](https://www.youtube.com/watch?v=ZHIqXJEp0-w&list=PLTU89LAWKRwEF1U28qtfPJXSPGZ8SVE2A&index=5&t=0s) 上更有用。

**总之**

> 阻力最小的道路可能很吸引人，但是一个训练有素的发展方法从长远来看是值得的。

虽然笔记本可以帮助您快速启动和运行，但当您尝试扩展项目时，可能会遇到问题，因为它们缺乏对版本、可再现性和模块化的支持。

Kedro 是一个构建代码的框架，它借鉴了大量数据科学家在一系列工业项目中的经验。它坚持让您与软件最佳实践保持一致，但它也允许您选择使用 Jupyter 笔记本进行实验，并简化代码从笔记本到 Kedro 项目的转移。

但是不要相信我的话。查看[团队使用它的原因](https://medium.com/quantumblack/beyond-the-notebook-and-into-the-data-science-framework-revolution-a7fd364ab9c4)，如果你想了解更多，请前往 [kedro.readthedocs.io](https://kedro.readthedocs.io) 或查看[不断增长的关于 kedro 的文章、播客和讲座列表](https://kedro.readthedocs.io/en/stable/11_faq/01_faq.html#articles-podcasts-and-talks)！

衷心感谢 QuantumBlack Labs 的 [Yetunde Dada](https://medium.com/u/e5bc6f3d62b7?source=post_page-----aa43523745b6--------------------------------) 和 [Lais Carvalho](https://medium.com/u/3da399388ac7?source=post_page-----aa43523745b6--------------------------------) 在我写这篇文章时提供的宝贵见解，并感谢整个 Kedro 团队愿意分享他们所知道的东西💚