# 通过 AWS 机器学习专业考试的五个技巧

> 原文：<https://towardsdatascience.com/five-tips-for-passing-the-aws-machine-learning-specialty-exam-a2977654d324?source=collection_archive---------13----------------------->

## 数据科学家的视角

![](img/7f27fd6122e0833e152624ebac5fc28d.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Rishabh Agarwal](https://unsplash.com/@rishu556?utm_source=medium&utm_medium=referral) 拍摄

我最近通过了 [AWS 机器学习专业考试](https://aws.amazon.com/certification/certified-machine-learning-specialty/)，收到了不少关于如何准备的问题。所以我想从数据科学家的角度写一些小技巧。这绝不是全面的 AWS ML 考试指南；相反，我把重点放在了我认为不应该丢分的几个方面，否则我经常在练习考试中出错。

AWS 的[考试指南](https://d1.awsstatic.com/training-and-certification/docs-ml/AWS%20Certified%20Machine%20Learning%20-%20Specialty_Exam%20Guide%20(1).pdf)说，你将在四个领域接受测试:数据工程、探索性数据分析、建模以及机器学习实现和操作。建模部分占整个考试的 36%；这是测试你的*实用机器学习知识*的部分，测试你如何将正确的 ML 解决方案应用于*现实世界的商业*问题。此外，作为 AWS 考试，这一部分有许多关于 SageMaker 和其他 AWS ML 服务的问题。

我购买了弗兰克·坎和夏羽·马雷克的 Udemy 课程，发现它非常有用。实际上，我发现这次考试比我去年春天通过的 [AWS 解决方案架构师助理](https://aws.amazon.com/certification/certified-solutions-architect-associate/)考试需要的准备要少得多，这主要是因为作为一名数据科学家，我每天都在处理 ML 问题，而且我通常不必太担心设置网络和安全性。所以，如果你和我有类似的背景，AWS ML 考试是值得参加的。准备工作将有助于您更好地了解 AWS 提供的服务。在这里，我总结了五个可能帮助你通过 ML 考试的技巧:

# 1.了解您的发行版

这真的是统计学/机器学习的基础知识。你会在上面做测试，你应该能很快回答这些问题。该问题将解释一个场景，并询问您哪一个发行版最能描述该场景。作为一名统计学专业的学生，这些是我发现最简单的问题，但这也意味着你不应该在这些问题上丢分——因为其他考生可能也在这些问题上得了满分。

# 2.亚马逊 Kinesis 家族

如果你像我一样，更习惯于处理“*离线*数据集，你可能没有处理实时数据的经验，这就是 AWS Kinesis 系列工具的意义。kinesis 系列服务，连同 AWS Glue，将构成 ML 测试的大部分数据工程领域(20%)。我总结了一开始让我困惑的几个要点:

*   只有 Kinesis Data Firehose 可以将流数据加载到 S3；它还可以提供数据压缩(针对 S3)，以及到 Parquet/ORC 的数据转换。
*   虽然 Kinesis Data Firehose 不能提供数据转换(如 CSV->JSON)，但它可以通过 AWS Lambda 来实现。
*   Kinesis Analytics 主要用于通过 SQL 进行实时分析；有两个自定义 AWS ML SQL 函数:RANDOM_CUT_FOREST(用于检测异常)和 HOTSPOTS(用于识别密集区域)。
*   Kinesis Analytics 将使用 IAM 权限来访问流媒体源和目的地。
*   还有 S3 分析，这是不要与 Kinesis 分析混淆。S3 分析用于存储类别分析。

# 3.AWS 胶水

AWS Glue 是另一个我喜欢的 AWS 工具——我只是觉得它太酷了，它可以自动抓取我在 S3 上的拼花文件，并自动生成一个模式。当我们在 S3 托管的 Parquet 中有大量数据时，查询 Glue 生成的 Athena 表进行数据探索是非常方便的。在准备考试的时候，我又发现了胶水可以做的一些小把戏:

*   除了 DropFields、filter 和 join 等标准数据转换之外，它还附带了一个 AWS 自定义算法 FindMatches ML，该算法可以识别潜在的重复记录，即使这两个记录可能不完全匹配。
*   Glue 将建立弹性网络接口，使作业能够安全地连接到其他资源
*   Glue 可以运行 Spark 作业，同时支持 Python 2.7 和 Python 3.6。

# 4.安全性

同样，您的里程数可能会有所不同，但对我来说，我更习惯于 IT 部门告诉我，由于安全问题，我不能这样或那样做，而不是自己必须过多地担心它。虽然 ML 考试没有解决方案架构师考试那么多关于安全性的问题，但您不想在这方面丢分。至少，你需要知道安全与 AWS S3 一起工作；此外，围绕使用 SageMaker 的安全性，如何保护您的数据进出 SageMaker。

# 5.亚马逊 SageMaker

我有一些使用 Amazon SageMaker 的经验，我喜欢它让一些事情变得非常方便，比如提供预装在容器中的最流行的 ML 框架。我只是不喜欢它的成本，但是，我跑题了😔。Amazon SageMaker 是 AWS 全面管理的 ML 服务的旗舰产品，将在 ML 考试中受到严峻考验。除了我上面提到的安全性，您需要知道的一些事情包括:

*   了解 SageMaker 所有内置算法；你将在这些上被测试！还有，了解哪个算法可以通过多核，多实例，或者 GPU 来加速。
*   SageMaker 只能从 S3 获取数据，有管道模式和文件模式；通常，当数据非常大时，管道模式可以加快速度。
*   SageMaker 中的超参数调优，了解有哪些选项，了解自动模型调优如何与 SageMaker 一起工作。
*   众所周知，使用 Amazon 弹性推理(EI)和 Amazon SageMaker 托管端点可以加快推理时间，但是您只能在实例启动期间而不是在部署之后附加 EI。

# 额外提示:以一次考试的价格参加两次考试

从成本角度来看，我也建议在参加机器学习专业考试之前先参加 AWS 解决方案架构师助理认证考试(我就是这么做的)。AWS 解决方案架构师助理考试费用为 150 美元，通过考试后，您将收到下次考试的半价优惠券。AWS 机器学习专业考试是 300 美元，在应用折扣后将是 150 美元。所以花 300 美元你就可以获得两个 AWS 证书！祝你好运！