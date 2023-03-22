# 让 python 代码看起来更好的九个简单步骤

> 原文：<https://towardsdatascience.com/nine-simple-steps-for-better-looking-python-code-87e5d9d3b1cf?source=collection_archive---------6----------------------->

![](img/659a66ad11f38b7e770eb7908409fe22.png)

来源[com break](https://pixabay.com/photos/work-workaholic-writer-programmer-1627703/)，经由 [pixabay](https://pixabay.com/photos/work-workaholic-writer-programmer-1627703/) 。(Pixabay 许可证)

我会定期查看补充学术论文、发布数据集的代码，或者分析 [Kaggle](https://www.kaggle.com/) 竞赛的解决方案。

我非常尊重分享代码以复制他们的结果的研究人员，以及参与机器学习(ML)竞赛并分享他们的解决方案的人。

有代码总比没有好，但我相信它的可读性还可以提高。

我看到的代码让我想起了我在学术界写的代码。我不喜欢它。这是我进入工业界的原因之一。在现代社会，能够编写代码变得和懂英语一样重要。

成为一个更好的程序员的最好方法是加入一个高编码标准的公司。尽管如此，在这篇博文中，我将谈论当你独自在代码库工作时的情况。

本文的目标读者是研究人员、数据科学家和初级软件开发人员。

成为一个更好的程序员是一个持续的过程。你写的代码越多，你就变得越好。

我不会告诉你如何变得更好，我会告诉你如何通过对你的开发过程做一些简单的调整来让你的代码看起来更好。

简单，我的意思是容易实现，每一步少于 5 分钟，并且改变应该迫使你做得更好。

我们很懒。如果某件事属于“很好做”的范畴，我们会找到很多正当的理由来避免去做。你被迫做的事情会更好。首先，你会去做，因为你没有选择，后来这就变得很自然了。

另一个原因是决策吸意志力。你在一个小决定上花了一点意志力，结果，增加了第二天早上在健身房睡过头的机会。当我们强迫行为时，我们节省了宝贵的能量。对那些感兴趣的人来说，《意志力:重新发现人类最大的力量》一书广泛地讨论了这个话题。

# **I:使用版本控制来跟踪代码中的变更**

对代码进行版本控制看起来似乎是一个显而易见的必备条件，但事实并非如此。在研究生院，我的导师是一个非常聪明的人，他开发了一种算法来执行哈伯德模型的量子蒙特卡罗。世界各地的许多研究人员使用该代码来推进理论凝聚态物理。当我开始使用它时，我很惊讶竟然没有一个集中的存储库。交换密码是通过电子邮件进行的。一位科学家开发的错误修复和新功能没有传播给其他用户。此外，我还见过我的同事如何使用不同的文件夹进行代码版本控制。

代码版本控制和在 GitHub 或其他服务上使用集中的存储库是必须的。许多研究人员正在这样做，但如果你离开计算机科学，大多数教授和研究生都处于用文件夹版本化和在电子邮件中发送代码的阶段。

**问:**你什么时候需要开始在你的项目中使用 git？
T5 A:从代码的第一行开始。

**问**:什么时候需要在 GitHub 上创建一个资源库，并将代码推送到那里？
**答:**从第一行代码开始。如果您认为由于法律或其他原因，您的代码不能公开共享，请创建一个私有存储库。在所有其他情况下，选择公开回购。

当人们等到代码看起来不错的时候才公开它，这是一个错误。我的许多同事感到非常不安全，因为他们的 GitHub 库中会有不完美的代码。他们担心人们会认为他们是糟糕的程序员。现实一点，没人会评判你。此外，你现在写的每一个代码在六个月后看起来都会很糟糕。

私有存储库比没有存储库要好。公共存储库比私有存储库更好。

我也喜欢 Kaggle 比赛的参与者在比赛结束后释放管道的方式。这是一个好习惯，对他们和社区都有帮助。

此外，有一个叫做 [Sourcegraph](https://sourcegraph.com/) 的工具，它允许你在所有公共 Github 库的代码中执行搜索。极其方便。这会让你更有效率。

*链接:*

*   [讲座:版本控制和 git](https://www.youtube.com/watch?time_continue=3540&v=2sjqTHE0zok&feature=emb_logo)
*   [Pro-git book](https://git-scm.com/book/en/v2)
*   从里到外的饭桶
*   [一个成功的 Git 分支模型](https://nvie.com/posts/a-successful-git-branching-model/)

# 第二:不要推到总分行

当你在一个团队中工作时，你不要直接推进到主分支。您创建一个单独的分支，在其中工作，创建一个拉请求(PR)，将拉请求合并到主服务器。这比直接推动掌握更复杂。原因是为了合并拉请求，您的更改应该通过各种检查。手动检查是由您的合作者进行的代码审查。自动检查包括语法、风格、测试等。

当您独自工作时，您没有机会审查代码，但是自动检查的力量仍然存在。

> *重要的是调整 GitHub 上的设置，这样即使你想推送也无法到达 master。正如我们上面讨论的，这可以防止滑倒，节省意志力。*

# **III:使用持续集成/持续交付(CI/CD)系统**

经历创建分支和合并的过程是一种开销。主要原因是它允许对代码变更进行检查。设置持续集成系统后，将检查您创建的每个拉式请求，只有在通过所有检查后，才会合并拉式请求。

有许多服务提供 CI/CD 功能。GitHub Actions、CircleCi、Travis、Buildkite 等

我会推荐 GitHub Actions。它是免费的，在私有和公共库上都可以工作，易于设置。

所有这些听起来可能非常复杂，但是实际上，您只需要向存储库添加一个配置文件。

*   一个简单的例子:我的带有帮助函数的库。我检查代码样式、格式，并运行测试。
*   [复杂](https://github.com/albumentations-team/albumentations/blob/master/.github/workflows/ci.yml)示例:在[albuminations](https://albumentations.ai/)库中，我们检查语法、代码风格、运行测试、检查自动文档构建，并且我们为不同的 python 版本和操作系统 Linux、Windows、Mac OS 执行这些操作

*重要！您需要更改 GitHub repo 中的设置，以便在所有检查都处于绿色区域的情况下，您将无法合并请求。*

*链接:*

*   博文:[马丁·福勒谈持续集成](https://martinfowler.com/articles/continuousIntegration.html)
*   教程:[使用 GitHub 动作设置持续集成](https://help.github.com/en/actions/building-and-testing-code-with-continuous-integration/setting-up-continuous-integration-using-github-actions)
*   书:[凤凰计划](https://amzn.to/2Y28zzk)。写得很好的书，描述了在 CI/CD 和其他现代实践引入之前公司中的混乱。

# **四:代码格式器**

有许多方法可以格式化同一段代码。

*   函数之间有多少空格？
*   代码中的行有多长？
*   进口的排序是怎样的？
*   应该用什么样的引号来定义字符串？
*   等等

有一些工具叫做[代码格式化器。如果你在你的代码上运行它们，它们会修改代码以符合格式化程序的要求。](https://github.com/rishirdua/awesome-code-formatters)

通用类型格式化程序:

*   YAPF:非常灵活，你可以配置它来适应你想要的风格。
*   [黑色:](https://black.readthedocs.io/en/stable/)无弹性。只能配置线路的长度。

选择一个并将其添加到您的 CI/CD 配置中。我喜欢黑色的。它让我所有的项目和使用黑色的项目看起来都一样。

代码格式化程序减少了上下文切换，这使得代码的阅读更加容易。

还有更具体的格式化程序。例如[**或**](https://github.com/timothycrosley/isort) 。Isort 只是对导入进行排序。

您需要在两个地方运行格式化程序:

1.  在 CI/CD 中。在这里，您以检查模式运行它:格式化程序会告诉您有他们想要格式化的文件，但是代码将保持不变。
2.  在提交更改之前。它会重新格式化你的代码。

*链接:*

*   Yapf
*   [黑色](https://github.com/psf/black)
*   [或](https://github.com/timothycrosley/isort)
*   博文:[与黑色一致的 python 代码](https://www.mattlayman.com/blog/2018/python-code-black/)
*   博客文章:[python 的自动格式化程序](https://medium.com/3yourmind/auto-formatters-for-python-8925065f9505)

# **V:预提交挂钩**

在上一步中，我们讨论了在提交之前在本地运行格式化程序的重要性。

比如说，我们用**黑**和**异色**。

我们需要运行:

```
black .isort
```

这很烦人。更好的解决方案是用这些命令创建一个 bash 脚本，比如“code_formatter.sh”并运行它。

创建这样的脚本是一种流行的方法，但是问题是，除非你强迫他们，否则人们不会这样做。

有一个更好的解决方案叫做[预提交挂钩](https://pre-commit.com/)。这个想法类似于 bash 脚本，但是您希望在每次提交之前运行的东西将会在您提交时准确地运行

```
git commit -m “<commit message>”
```

这看起来是一个很小的区别，其实不是。使用预提交，您的行为是强制的，并且，正如我们上面讨论的，它工作得更好。

Q : PyCharm 执行良好的格式化。为什么我需要一个预提交钩子？
**A** :你可能忘了用 PyCharm 格式化你的代码。此外，当你有两个或更多的人，你要确保他们的格式是相同的。对于预提交挂钩，所有人都将拥有相同的配置，这是您的存储库的一部分。我建议两者都做，使用预提交钩子并用 PyCharm 格式化代码。

问:如果我不从控制台执行提交，而是直接从 PyCharm 执行，会怎么样？
答:您可以配置 PyCharm 在每次提交时运行预提交钩子。

*链接:*

*   [预提交挂钩](https://pre-commit.com/)

# **六:棉绒**

不改变你的代码，但是检查它并且寻找可能的问题的工具，叫做 linters。最常见的是 [**flake8**](https://flake8.pycqa.org/en/latest/) **。**

它可以寻找:

*   [PEP8 错误和警告。](http://pep8.readthedocs.org/en/latest/intro.html#error-codes)
*   [一致的命名约定。](https://github.com/PyCQA/pep8-naming)
*   你的函数的圈复杂度。
*   还有其他带一套[插件](https://github.com/DmytroLitvinov/awesome-flake8-extensions)的东西。

它是一个强大的工具，我建议将它添加到您的 CI/CD 配置以及预提交钩子中。

*链接:*

*   [Flake8:您实施风格指南的工具](https://flake8.pycqa.org/en/latest/)
*   [Flake8 扩展](https://github.com/DmytroLitvinov/awesome-flake8-extensions)

# **VII: Mypy:静态类型检查器**

从 Python 3 开始，您可以向代码中的函数添加类型注释。这是可选的，但强烈推荐。

一个原因是有类型注释的代码更容易阅读。

当您在代码中看到:

```
x: pd.DataFrame
```

它的信息量远远超过

```
x
```

当然，你首先不应该把你的变量命名为“x”。尽管如此，在这篇博文中，我谈论的是改进代码的简单、自动的方法，好的命名比简单要难一些。

类型注释是可选的。没有它们，代码也能很好地工作。有一个叫做 [Mypy](https://mypy.readthedocs.io/en/stable/) 的工具可以检查:

*   函数和输入参数的类型注释
*   变量类型及其操作之间的一致性

这是一个非常有用的工具。所有大的科技公司都会检查他们 python 代码的每一个 pull 请求。

它迫使你编写更容易阅读的代码，重写过于复杂、写得不好的函数，并帮助你识别错误。

应将 Mypy 检查添加到 CI/CD 和预提交挂钩中。

*链接:*

*   [Mypy 文档](https://mypy.readthedocs.io/en/stable)
*   博客文章:[Mypy python 可选静态类型](https://medium.com/@thabraze/mypy-optional-static-typing-for-python-dbc53b82f1ef)
*   博文: [Mypy 和持续集成序列](https://medium.com/@quinn.dougherty92/mypy-and-continuous-integration-sequence-part-1-mypy-and-type-hints-ae69f66b6d9e)

# **VIII:对预提交挂钩进行更多检查**

你可以用许多不同的东西来扩展你的预提交钩子。

*   删除尾随空白。
*   文件的结尾在新的一行。
*   排序要求 txt。
*   检查 yaml 文件的正确格式。
*   等等

您可以为应该自动发生的代码格式化和检查创建自己的挂钩。

*链接:*

*   [预提交挂钩:插件](https://pre-commit.com/#plugins)

# **九:外部工具**

使用 ML 来分析每个拉请求的代码的工具有一个活跃的开发领域。

所有这些对公共存储库都是免费的，其中一些对私人存储库也是免费的。我看不出有什么理由不为您的公共代码启用它们。

*链接:*

*   [深源](https://deepsource.io/)
*   [深度代码](https://www.deepcode.ai/)
*   [Codacy](https://www.codacy.com/)
*   [编码因子](https://www.codefactor.io/)

# **结论**

如果一个人至少实现了这些技术中的一些，他/她的代码将变得更容易阅读。

但这只是第一步。

还有其他标准技术，如:

*   [单元测试](/writing-test-for-the-image-augmentation-albumentation-library-a73d7bc1caa7)
*   使用[假设](https://hypothesis.readthedocs.io/en/latest/)的单元测试
*   Python 环境
*   建筑包装
*   码头化
*   自动文档
*   等等

但是它们不能像我上面描述的步骤那样简单地实施。因此，我们将把它们留给另一个时间。:)

对于那些在实现我所描述的技术时有问题的读者，请随时联系我，我会扩展相应的要点。