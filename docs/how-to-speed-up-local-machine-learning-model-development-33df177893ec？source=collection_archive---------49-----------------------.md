# 如何加快本地机器学习模型的开发

> 原文：<https://towardsdatascience.com/how-to-speed-up-local-machine-learning-model-development-33df177893ec?source=collection_archive---------49----------------------->

## 介绍一个强大的机器学习模板

![](img/af72e4e896814998ec268493f6e2d95c.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的 [ThisisEngineering RAEng](https://unsplash.com/@thisisengineering?utm_source=medium&utm_medium=referral)

*TL/DR:我在 Github 上开发了一个包，*[*ml-template*](https://github.com/eddiepease/ml-template)*，通过*加速了本地机器学习模型的开发

1.  *提供一个结构良好的通用代码库，可以根据您的用例轻松调整*
2.  *使用最新的软件包(DVC 和 MLFlow)确保模型结果的再现性和有效的模型性能跟踪*

# 开发包装的动机

虽然这似乎就发生在昨天，但我现在已经在 4 年多前开始了我的机器学习之旅。在那段时间里，我很幸运地解决了一些很酷的问题。从编写算法根据某人鞋子的照片检测其性别，到使用自然语言处理[根据你的简历](https://github.com/eddiepease/career_path_recommendation)预测你的下一份工作，再到[预测临床试验的结果](https://medium.com/swlh/three-crucial-lessons-for-launching-an-ai-startup-976d1d44f370)，我处理过各种各样的问题。

这些问题显然大相径庭，有其独特的挑战。然而，这篇文章让我感兴趣的不是这些问题有什么不同，而是它们有什么共同点。每当我开始编写机器学习问题的代码时，我都必须编写相同的对象和函数来分割训练集和测试集，训练算法，执行交叉验证，保存训练好的模型等等。无论是自然语言问题、机器学习视觉还是任何其他类型的问题，这一点都适用。

如果你正在匆忙地开发你的代码，你也会写出结构不良的代码。我不需要告诉你，糟糕的结构代码会让任何人在以后的日子里更难发现，也意味着更不容易发现错误。

![](img/27dce2ac9b167c18e57e1d8cc502f499.png)

约书亚·索蒂诺在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我遇到的另一个挫折是，当主体结构建成时，我要监控模型。根据我的经验，建立机器学习模型是一个迭代过程，需要对不同版本的数据和不同的转换、算法和超参数进行大量实验。

我试图通过在 Excel 电子表格中记录模型“主要”运行的参数和输出来解决这个问题，但我发现这很快变得难以操作。如果您需要将模型切换回上周运行的数据/代码组合，会发生什么呢？这可能很棘手。

为了尝试解决这些痛点，我开发了一个面向对象的机器学习模板，可以根据您的特定问题进行相应的调整。我认为这个项目相当于建造一栋房子的地基和结构——在完工之前还有相当多的工作要做，但它极大地加快了这个过程。

但是首先，让我带您看一下我用于数据和模型版本控制的两个包。

# DVC 和 MLFlow 是什么？

在 Git 出现之前，对代码的修改是作为补丁和归档文件传递的。这使得两个或更多的人很难处理同一个代码，并且很难快速切换到以前的版本。Git 的采用极大地提高了软件开发的效率。

机器学习目前感觉像是处于“前 Git”时代。当然，您可以对您的代码进行版本控制，但是今天大多数数据科学家不会对他们的数据或模型进行版本控制。这意味着，你猜对了，在一个模型上合作和切换到以前的版本变得更加困难。DVC 和 MLFlow 是两个旨在提供帮助的软件包。

![](img/85b70fc06c01a2380114bc574c92bd91.png)

由[克里斯多夫·伯恩斯](https://unsplash.com/@christopher__burns?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

[DVC](https://dvc.org/) 是数据的版本控制包。还有其他的数据版本控制包，比如“Git-LFS ”,但是这个有很多问题(更多细节见[这篇](/why-git-and-git-lfs-is-not-enough-to-solve-the-machine-learning-reproducibility-crisis-f733b49e96e8)博客)。DVC 的巧妙之处在于它如何将 Git 中的相同概念运用到数据管理领域。它很容易在许多不同的文件系统(本地、S3、GCP 等)上存储数据，它可以处理大量的文件大小，并与 Git 无缝集成。DVC 正在积极开发和不断扩展其功能，以处理改进模型管理和模型部署。一个面向未来的包裹。

[MLFlow](https://mlflow.org/) 是一个开源包，最初由 Databricks 开发，用于管理从实验到模型部署的整个机器学习生命周期。如果你看看他们的网站，他们有大量与所有流行的机器学习工具的集成，令人印象深刻的一系列公司正在使用并为此做出贡献。我只使用了软件包的“跟踪”功能，这使得在一个好看的用户界面上跟踪不同的机器学习实验变得非常容易。

# 使用软件包的提示

因此，使用 DVC 来控制数据的版本，使用 MLFlow 来记录实验结果，我创建了一个 Github 包， [ml-template](https://github.com/eddiepease/ml-template) ，来帮助加速开发机器学习模型的过程。我的希望是，当你将来开始一个机器学习项目时，你会发现从 Github 克隆 [ml-template](https://github.com/eddiepease/ml-template) 然后为你的项目定制代码是有帮助的。

现在，当然，当任何过程都是自动化的，是不是总是很容易使用“自动驾驶”软件，而不去想一些正在做的默认假设。特别是，每次你克隆 [ml-template](https://github.com/eddiepease/ml-template) 的时候，我觉得你应该思考以下几点:

*   数据转换-确保适当完成必要的缩放、分类编码和特征转换(默认为无转换)。
*   训练/测试分割—确保分割适合您的问题(例如，默认为随机训练/测试分割，但分层分割可能更合适)。
*   算法—确保它适合任务(默认为随机森林)
*   度量——确保度量对于您的任务是合理的。一个典型的错误是在数据集高度不平衡时使用准确性作为衡量标准(默认的衡量标准是 AUC)

此外，这里有一些最有效地使用软件包的技巧:

*   每次进行“主”运行时，都要重新提交代码和数据。这确保了数据/代码/模型输出和保存的模型是一致的
*   调试时，设置 mlflow_record = False。这确保了只有重要的运行被记录，这有助于下游的模型分析
*   当试验新数据/新方法时，在 git 中创建一个新的分支。这利用了该框架的力量，使得比较/切换回现有设置变得非常容易
*   使用 MLFlow 中的实验对运行进行分组

# 结论

我希望人们发现这个包在更好地设置和管理他们的本地机器学习模型开发方面是有用的。

请不要客气贡献和建议的方式，以进一步改善包向前发展。未来发展的思路包括:

*   添加一组测试
*   用于直观显示模型结果的基本 UI 框架(使用工具，如 [Streamlit](https://www.streamlit.io/)