# 如何在一行代码中跨不同环境共享 Python 对象

> 原文：<https://towardsdatascience.com/how-to-share-your-python-objects-across-different-environments-in-one-line-of-code-f30a25e5f50e?source=collection_archive---------32----------------------->

## 对设置环境与他人分享您的分析感到沮丧？以下是如何让它变得更简单

![](img/97a9028417c37ced74903d45c00f7b42.png)

由[阿克塞尔·阿霍伊](https://unsplash.com/@axelahoi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 动机

你有没有想过在不同的脚本之间共享 Python 对象？或者在你和你的队友之间分享发现，这样每个人都可以工作在最新的结果上？你可能会遇到三个困难:

*   您的队友可能会部署与您的脚本不同的环境
*   由你的队友训练的模型**每天都在变化**。所以他们需要每天和你分享模型。
*   文件或模型**很大。每次部署脚本时重新上传它们是很麻烦的。‌**

如果你所需要做的就是像这样共享数据帧，不是更容易吗

```
import datapane as dpdf = dp.Blob.get('profile', owner='khuyentran1401').download_df()df
```

或者类似这样的分享你的模型？

```
import datapane as dppredictor = dp.Blob.get(*name*='predictor',*owner*='khuyentran1401').download_obj()X_TEST = [[10, 20, 30]]outcome = predictor.predict(*X*=X_TEST)
```

这时我们需要 Datapane 的 Blob API

# 什么是 Datapane 的 Blob？

[Datapane](https://datapane.com/) 提供了一个 API 集合，使得与其他人共享 Python 分析变得容易。如果你还不知道 Datapane，我在这里写了一篇关于 Datapane 的文章。

[](/introduction-to-datapane-a-python-library-to-build-interactive-reports-4593fd3cb9c8) [## Datapane 简介:构建交互式报表的 Python 库

### 创建精美报告并与您的团队分享分析结果的简单框架

towardsdatascience.com](/introduction-to-datapane-a-python-library-to-build-interactive-reports-4593fd3cb9c8) 

除了用于创建报告和部署笔记本的 API(在另一篇博文中显示……等等), Datapane 还提供了用于其他常见用例的 API，例如**共享 blob 和管理秘密**。

为了说明 Blob 的使用，我使用了来自本文的一个机器学习模型的例子。假设您正在训练一个这样的模型，它在测试集上有很好的性能。

您希望将此模型发送给您的团队成员，以便他们可以使用您的模型来预测新的数据集。你怎么能这样做？

你可以建立一个 docker 环境，并把它发送给你的队友。但如果他们只是想快速测试你的模型，而不需要设置环境，`datapane.Blob`会是更好的选择。

如果您希望其他人访问它，请确保拥有`visibily='PUBLIC'`。现在其他人可以用你的 blob 写他们的代码了！

```
Outcome : [140.]
Coefficients : [1\. 2\. 3.]`
```

尝试运行相同的代码，看看是否可以访问和使用预测器！在复制和粘贴代码之前，请确保预先在 Datapane 上注册以获得令牌。然后使用您的令牌
`datapane login — server=[https://datapane.com/](https://datapane.com/) — token=yourtoken`登录终端

现在试试

```
dp.Blob.get(name='predictor', owner='khuyentran1401').download_obj()
```

看看您是否能够访问和使用预测器并产生与上面相同的结果！

如果您想在您的组织中私下共享您的 blob，请遵循相同的过程，但是将 blob 的可见性设置为`ORG`

# 我还能用 Blob 做什么？

除了上传 Python 对象，您还可以使用 blob 来上传 Pandas Dataframe，一个文件。

并从 blob 中获取数据帧、文件或对象

例如，在文章中，我写了我在搜集了超过 1k 个 Github 概要文件后的发现

[](/i-scraped-more-than-1k-top-machine-learning-github-profiles-and-this-is-what-i-found-1ab4fb0c0474) [## 我收集了超过 1k 的顶级机器学习 Github 配置文件，这就是我的发现

### 从 Github 上的顶级机器学习档案中获得见解

towardsdatascience.com](/i-scraped-more-than-1k-top-machine-learning-github-profiles-and-this-is-what-i-found-1ab4fb0c0474) 

为了让你访问我的数据集，我需要做的就是给你数据集的名称和我的数据面板的帐户。现在您已经准备好访问它了！

```
import datapane as dpdf = dp.Blob.get('profile', owner='khuyentran1401').download_df()df
```

当然，为了让您访问我的数据，我需要将 blob 的可见性设置为“PUBLIC”

# 可变的

像数据库密钥、密码或令牌这样的变量不应该嵌入到我们的代码中，尤其是当你的脚本对外界可见的时候。Datapane 的`Variable`对象为**提供了一种安全可靠的方式**来创建、存储和检索脚本所需的值。

例如，在这个脚本中，我使用了`dp.Variable`来存储我的 Github 令牌和我的用户名，这样当我在 GitHub 上共享我的代码时，我就不需要担心我的令牌被暴露了。

这个变量对我来说是完全保密的，除非我用`visibility='PUBLIC'.`指定每个人都可以访问它

如果另一个拥有不同账户的人想要访问我的`github_token`变量并指定我为所有者，他们将会得到一个错误

```
requests.exceptions.HTTPError: 403 Client Error: Forbidden for url
```

因为我在私有模式下使用变量。

另一个好处是:如果我碰巧在另一个脚本中重用了这个变量，我可以很容易地再次访问这个令牌，而不需要记住它是什么！

# 但是这不跟泡菜差不多吗？

对于那些熟悉 Python pickle 的人来说，您可能会看到 Datapane 的`Blob`和`Variable`的功能与 Python pickle 相似。是的，它们几乎是相似的，但是`Blob`和`Variable`与泡菜相比有 3 个主要的优点

*   有了`Blob,`，其他人不需要为了得到相同的模型而建立与你相似的环境。例如，您可能[在 Sagemaker](/guides/guide-creating-ml-model-form) 上训练一个模型，并希望在您的脚本中使用它。
*   您只需使用`upload`上传更改，然后您的队友就可以使用`get`查看更改，而无需将 pickle 文件发送给他人。
*   您可能会在新版本中意外出错，并希望回到旧版本(即训练错误的数据)。你可以在`get`的参数中用`version =`回到老版本

# 结论

恭喜你！您刚刚学习了如何使用 Datapane 的`Blob`和`Variable`来跨不同的脚本或不同的环境共享您的数据帧、文件、Python 对象和变量。

如果你的队友只需要一行代码就能看到你的结果，而不需要设置环境，他们会很高兴的。当你的队友可以像你一样访问相同的数据时，你会很高兴。

在[这个 Github repo](https://github.com/khuyentran1401/Data-science/blob/master/data_science_tools/blob_datapane.ipynb) 中，您可以随意派生和使用本文的代码。

我喜欢写一些基本的数据科学概念，并尝试不同的算法和数据科学工具。你可以在 LinkedIn 和 Twitter 上与我联系。

如果你想查看我写的所有文章的代码，请点击这里。在 Medium 上关注我，了解我的最新数据科学文章，例如:

[](/how-to-create-fake-data-with-faker-a835e5b7a9d9) [## 如何用 Faker 创建假数据

### 您可以收集数据或创建自己的数据

towardsdatascience.com](/how-to-create-fake-data-with-faker-a835e5b7a9d9) [](/dictionary-as-an-alternative-to-if-else-76fe57a1e4af) [## 字典作为 If-Else 的替代

### 使用字典创建一个更清晰的 If-Else 函数代码

towardsdatascience.com](/dictionary-as-an-alternative-to-if-else-76fe57a1e4af) [](/how-to-monitor-and-log-your-machine-learning-experiment-remotely-with-hyperdash-aa7106b15509) [## 如何使用 HyperDash 远程监控和记录您的机器学习实验

### 培训需要很长时间才能完成，但你需要去洗手间休息一下…

towardsdatascience.com](/how-to-monitor-and-log-your-machine-learning-experiment-remotely-with-hyperdash-aa7106b15509) [](/how-to-accelerate-your-data-science-career-by-putting-yourself-in-the-right-environment-8316f42a476c) [## 如何通过将自己置于合适的环境中来加速您的数据科学职业生涯

### 我感到增长数据科学技能停滞不前，直到我有了一个飞跃

towardsdatascience.com](/how-to-accelerate-your-data-science-career-by-putting-yourself-in-the-right-environment-8316f42a476c) [](/how-to-create-reusable-command-line-f9a2bb356bc9) [## 如何创建可重用的命令行

### 你能把你的多个有用的命令行打包成一个文件以便快速执行吗？

towardsdatascience.com](/how-to-create-reusable-command-line-f9a2bb356bc9)