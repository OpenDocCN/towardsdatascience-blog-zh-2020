# 我作为数据科学家犯下的 10 个错误

> 原文：<https://towardsdatascience.com/10-mistakes-ive-made-as-a-data-scientist-f6118184e69?source=collection_archive---------18----------------------->

## 可能出错的地方以及如何解决的例子

![](img/8496d83c34a549312721347e00ea967f.png)

与 Raj 在[Unsplash](https://unsplash.com/s/photos/spiderman?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上的[公路旅行照片。](https://unsplash.com/@roadtripwithraj?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 目录

1.  介绍
2.  错误
3.  摘要
4.  参考

# 介绍

做了几年数据科学家后，我开始意识到自己适合做什么，哪里需要帮助，以及哪里出了问题。承认错误会令人尴尬和不舒服；然而，我认为这不仅是作为一名数据科学家成长的最佳方式，也是作为一个人成长的最佳方式。如果你敞开心扉接受反馈，你会发现自己学到了更多。数据科学是你需要不断学习的关键领域之一。数据科学家能做的最糟糕的事情就是隐藏他们的错误。尽管老生常谈，我还是会引用曾经非常重要的蜘蛛侠电影——

> 权力越大，责任越大——本叔叔

因为数据科学对一个企业有如此巨大的影响，所以你会犯的错误也是如此(是的，我们并不完美，最好是预测一个错误及其相应的解决方案，而不是认为你的 100%正确的机器学习模型不是一点点*可疑*)。

下面，我将包括我过去作为数据科学家所犯的 10 个关键错误，以及我为解决这些错误所做的事情，或者我将来会做的事情。我希望这些突出的错误也能成为有用的指南，供您在作为数据科学家的旅程和努力中使用。

# 错误

![](img/3802cc78a2aa82a6600dea1812b84af1.png)

照片由 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/mistake?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【2】上拍摄。

你可以为任何职业撰写大量关于错误的文章，尤其是在数据科学领域，所以我想强调一下我的个人经历，因为我确信我们过去曾犯过类似的错误，或者将来会犯类似的错误。以下是我作为数据科学家犯下的 10 大错误。

1.  **跳过面向对象编程(OOP)** — 以为自己不需要了解软件工程或者面向对象编程。当我学习数据科学时，我对编码或不同的通用语言(如 Python 或 r)了解不多。我对 SQL 有一些经验，但 Python(我现在的主要编程语言)是慢慢引入的，只用作基本操作数据科学模型和库的方法。当引入更多 OOP 时，您可以模块化您的代码，这最终将变得更具可伸缩性。一旦你的蟒蛇进去了。OOP 格式的 py 文件，你将能够把它交给机器学习工程师或软件工程师，以便你的模型可以很容易地在生产中执行(如果你是超级数据科学家或没有任何额外数据工程资源的较小公司，你也可以自己完成所有步骤)。

当创建我的数据科学项目时，我通常开始时不太像 OOP，但是一旦我得到了所有的概念和基本代码，我就会专注于模块化。

> 例如，您不需要定义一个函数和类来读入 CSV 文件以开始您的数据科学过程，但是最终，您可以构建一个模块，该模块通过一个活动连接从 SQL 查询中自动获取新数据。

[这里](/how-to-write-a-production-level-code-in-data-science-5d87bd75ced)【3】是 [Venkatesh Pappakrishnan 博士](https://medium.com/u/509cb51eb23?source=post_page-----f6118184e69--------------------------------)关于生产级代码的一篇很棒的文章。

2.**不使用 GitHub** — 我写过两篇关于 GitHub 和 Git 的关于数据科学的文章。也可以随意查看，我会在本文末尾提供这两个链接。

Git 的版本控制和 GitHub 的平台总体上非常有利于与团队中的其他数据科学家共享您的代码库。像 GitHub pull requests 这样的检查和平衡对于确保您的代码不仅在本地更新，而且您推送到主分支的内容也得到您的专业同行的批准非常有用。

人们很容易犯这样的错误:拥有数百本笔记本和。ipynb 文件，代码只有很小的不同。最好的解决方案是使用 GitHub 环境进行简单的协作。

3.**写糟糕的 Git 会犯**——虽然这个错误在你的整个过程中并不完整，但它会累积起来，你的 GitHub 空间会看起来杂乱无章。当您提交对本地分支的更改以便在 pull 请求后推送到主分支时，您将必须添加一个提交行。以下是一些例子:

```
**Bad practice:** Adding the next iteration for bugs**Good practice:**Add error handling 
```

4.不写单元测试——这仍然是我需要集中精力的事情。很多时候，数据科学家会把时间花在模型构建、参数调整和进一步迭代上。然而，当您开始将您的模型部署到产品中时，单元测试将会提供极大的价值。它们可以用来说明未来的错误，这样您的整个管道就不会受到过程中一个简单变化的负面影响。[这里](/unit-testing-and-logging-for-data-science-d7fb8fd5d217) [4]是一篇来自 [Ori Cohen](https://medium.com/u/4dde5994e6c1?source=post_page-----f6118184e69--------------------------------) 的关于单元测试和数据科学的优秀文章。

5.忘记记录代码——也许最容易犯的错误之一就是忘记对代码进行注释。你可能经常很匆忙或者认为你现在知道了，所以你以后也会知道。然而，你很可能不仅会与自己分享代码，还会与他人分享——数据科学家、数据工程师、机器学习工程师和软件工程师。除了其他人稍后查看您的代码之外，您将回头参考您的代码，并且很可能您会忘记您在代码中的意思，因此，最终，最好注释掉您的代码，以及列出您正在编写的代码的关键功能和含义。

*   *写 Python 代码有一种官方的方式，叫 PEP 8，在这里*[](https://www.python.org/dev/peps/pep-0008/)**【5】。**

*6.**依靠 Jupyter 笔记本** —当然，Jupyter 笔记本对于开始您的数据科学探索性数据分析和模型构建非常有用，但是在某个点上，您、您自己或另一位工程师将不得不将您的代码投入生产，并使用 Visual Studio 或 Sublime Text 等工具将您的代码组织成。py 模块。下面列出了一些有益的工具:*

*   *可视化工作室*
*   *崇高的文本*
*   *原子*

*7.**不考虑模型训练时间** —当在你的大学课堂上为你的 100 行数据集创建复杂的 XGBoost 模型时，或者可能是一个教程，一旦你进入了“*大数据*”，你将不再能够轻松地将这么多数据消化到你的模型中，训练将需要几天时间。了解您正在使用的模型的容量非常重要。您还可以求助于其他算法，这些算法实际上可能会导致更高的准确性，比如说您使用随机森林来代替并限制树的最大深度，您可能会提高您的准确性，降低过度拟合的可能性，并大大减少模型训练时间。*

*另一个小技巧是，不要每天训练你的模型，或者每天你得到新的数据，但是如果你得到一定量的数据。一个例子是:*

```
*train this model if we get 1000 new rows:else:use the previously stored model that was already trained*
```

*8.**不练软技能** —刚学数据科学的时候，重点是建模和编码。不一定需要教授或需要实际生活经验的，是在你的角色中取得成功所需的软技能。作为一名数据科学家，您可能会在整个项目中遇到多个非技术用户。你将不得不与软件工程师或数据工程师会面，以了解数据及其来源，然后你将与产品经理和主题专家会面，讨论业务问题和预期的解决方案，最后 UX/UI 设计师有时会将你的模型结果实现到一个制作精美的交互式界面中。总的来说，软技能在通过面对面(或视频)会议和解释项目的过程和结果来实现数据科学模型的成功方面非常出色。*

*9.**忘记实现维护计划**——类似于单元测试，过程的这一部分经常被忽略。有像 CircleCI 和 Airflow 这样的工具，以及通常从仪表板创建描述模型行为的可视化。对我来说，为了找到与模型错误相关的紧急问题的解决方案，知道某个模型何时没有被训练是很重要的。下面列出了这些工具:*

*   *切尔莱西*
*   *气流*
*   *（舞台上由人扮的）静态画面*
*   *谷歌数据工作室*

*10.**忘记学习新的数据科学** —一旦你发现自己在职业生涯中安全稳定，你就可以忘记学习新的东西，尤其是新的和即将到来的数据科学方法。为你的机器学习模型保持更有效的工具是很重要的，同时通过学习或写文章来学习，就像我在 Medium 上看到的那样。*

# *摘要*

*![](img/8e4ba1de21ae83f0c6f3fa3f6ca6381b.png)*

*照片由 [Eunice De Guzman](https://unsplash.com/@edg0308?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/success?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【6】上拍摄。*

*成为一名数据科学家可能充满挑战、乐趣和权力；然而，向利益相关者和最终用户提供快速、准确和可信的结果会带来巨大的压力。关键是你要让自己熟悉你过去犯过的常见错误，以及其他人犯过的错误，这样你才能预见未来可能犯的错误。*

> *一旦你对自己和他人开诚布公，错误就不那么容易被评判，而更容易被视为成为最好的数据科学家的机会。*

*为了便于查看，这里列出了所有的错误:*

*   ***跳过面向对象编程***
*   ***不使用 GitHub***
*   ***写烂 Git 提交***
*   ***不编写单元测试***
*   ***忘记记录代码***
*   ***依靠 Jupyter 笔记本***
*   ***不计入模型训练时间***
*   ***没有练软技能***
*   ***忘记实施维护计划***
*   ***忘记学习新的数据科学***

*我希望你觉得我的文章有趣且有用。感谢您的阅读！*

*链接到我前面提到的 GitHub 文章[7]和[8]。*

*[](/a-succesful-data-science-model-needs-github-heres-why-da1ad019f4e0) [## 一个成功的数据科学模型需要 GitHub。原因如下。

### GitHub 给你的数据科学项目带来的好处。

towardsdatascience.com](/a-succesful-data-science-model-needs-github-heres-why-da1ad019f4e0) [](/common-github-commands-every-data-scientist-needs-to-know-e7d5d9c4f080) [## 每个数据科学家都需要知道的通用 GitHub 命令

### 通过使用 GitHub 成为更好的数据科学家指南

towardsdatascience.com](/common-github-commands-every-data-scientist-needs-to-know-e7d5d9c4f080) 

参考文章，[3]和[4]:

[](/how-to-write-a-production-level-code-in-data-science-5d87bd75ced) [## 数据科学如何写一个生产级代码？

### 编写生产级代码的能力是数据科学家角色的抢手技能之一——无论是发布…

towardsdatascience.com](/how-to-write-a-production-level-code-in-data-science-5d87bd75ced) [](/unit-testing-and-logging-for-data-science-d7fb8fd5d217) [## 数据科学的单元测试和日志记录

### 在本教程中，我将创建输入输出单元测试，一个日志类和一个 API 合适的部署。

towardsdatascience.com](/unit-testing-and-logging-for-data-science-d7fb8fd5d217) 

# 参考

[1]2018 年[与 Raj](https://unsplash.com/@roadtripwithraj?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/mistake?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的公路旅行照片

[2]照片由 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/mistake?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄，(2017)

[3] V.Pappakrishnan 博士[。](https://towardsdatascience.com/@venkatesh.kumaran?source=post_page-----5d87bd75ced----------------------)、[数据科学如何写一个生产级代码？](/how-to-write-a-production-level-code-in-data-science-5d87bd75ced) (2018)

[4] O.Cohen，[数据科学的单元测试与记录](/unit-testing-and-logging-for-data-science-d7fb8fd5d217)，(2018)

[5] [Python 软件基础](https://www.python.org/psf-landing/)，[PEP 8—Python 代码风格指南](https://www.python.org/dev/peps/pep-0008/)，(2001–2020)

[6]照片由[尤妮斯·德·古兹曼](https://unsplash.com/@edg0308?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/success?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2019)上拍摄

[7] M.Przybyla. [一个成功的数据科学模型需要 GitHub](/a-succesful-data-science-model-needs-github-heres-why-da1ad019f4e0) 。原因如下。, (2020)

[8] M.Przybyla，[每个数据科学家都需要知道的通用 Git 命令](/common-github-commands-every-data-scientist-needs-to-know-e7d5d9c4f080)，(2020)*