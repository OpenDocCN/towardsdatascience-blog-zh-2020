# 数据库中的机器学习:我们就不能好好相处吗？

> 原文：<https://towardsdatascience.com/in-database-machine-learning-cant-we-all-just-get-along-658b00234b1a?source=collection_archive---------26----------------------->

![](img/d1c94d93e3df8d34153bbf600baaf6de.png)

[ev](https://unsplash.com/@ev?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/data-science?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 意见/供应商观点

## *简单看看 Splice Machine 最近融合 ML 和关系世界观的努力*

如果数据在企业内部驱动机器学习(ML)，如果企业数据存在于数据库中，那么为什么两者不能和睦相处？为什么数据科学家用来构建 ML 结果的 ML 算法、模型和其他工件在物理上距离它们所依赖的数据如此之远？

简单的回答是，数据科学的工作方式不同于数据库，尤其是传统的关系数据库管理系统(RDMSes ),它强调定向事务，新数据随着时间的推移不断增加。相反，ML 项目是高度迭代的，探索性的，完全实验性的。

即使是最直接的企业 ML 项目，如预测客户流失，数据科学家也必须安排一个极其复杂和高度迭代的工作流程。这些工作通常在像 Project Jupyter 这样的基于 web 的笔记本环境中进行，需要不断地与支持性数据库资产进行交换。随着 ML 项目的临近和进入生产，这种来回变得更加困难和关键。这是 ML 的“最后一英里”问题:企业如何更快地将 ML 项目投入生产，然后随着时间的推移成功地将它们保持在那里。

作为回应，技术提供商开始推出更多生命周期完整的人工智能开发工具，如 [AWS Sagemaker 套件](https://aws.amazon.com/sagemaker/)。有趣的是，除了 AWS 广泛的数据库组合之外，这个神奇的工具独立存在，从任何和所有可用的数据存储中提取数据。但这并不意味着两者合不来。许多数据库供应商希望通过将 Sagemaker 所做的一些事情转移到数据库本身来参与其中。

# 表格功能:旧思想的新生命

一个恰当的例子是，数据库供应商 Splice Machine 在其最近的版本( [version 3.0](https://splicemachine.com/whats-new-in-splice-machine-3-0/) )中引入了一种有趣的方法，使用一种古老的数据库技术来解决 ML“最后一英里问题”。供应商已经嵌入了自己的 Jupyter 笔记本环境实现，该环境可以支持 Python 和 r 之外的多种语言。该环境利用了流行的 ML 生命周期管理平台 MLflow 的产品内实现。或者，模型可以部署到 AWS SageMaker 或[微软 Azure ML](https://azure.microsoft.com/en-us/services/machine-learning/) 。

使用这些工具，开发人员可以调用一个“deploy()”函数来将他们的最终模型推向生产。该函数采用最终的 ML 模型加上支持的数据库表，并在数据库中自动生成代码作为表函数(类似于存储过程)。每当新记录进入数据库时，这个原生表资源就会执行，生成新的输出预测。

这种方法的固有优势包括大大简化了模型部署例程，更好地了解关键的部署后问题(如模型漂移),由于数据不必离开数据库而提高了性能，以及由于解决方案包含自己的容器管理平台而减轻了管理负担。转化为业务成果，这意味着交付 ML 解决方案所需时间的显著减少，以及长期维护该解决方案的成本的降低。

这是在企业中实施 ML 问题的唯一解决方案吗？当然不是。与任何现实世界的 ML 实现一样，期望的结果取决于所选工具和可用资源(技术和专业知识)之间的完美结合。幸运的是，由于 ML 市场严重依赖开源软件，企业购买者可以混合搭配语言、库、编排器等。

这也意味着买家可以投资像 Splice Machine 这样的基于数据库的解决方案，并且仍然可以利用熟悉的资源。目前 3.0 版本可以把模型推送到比如 AWS SageMaker 和微软 Azure ML。在未来，Splice Machine 很可能允许用户运行其他 ML 编排工具，如 [Kubeflow](https://www.kubeflow.org/) 与 MLflow 并行或代替 ML flow。

我们应该会看到更广泛的数据库社区使用这种方法进行更多的投资。Oracle 最近已经开始将许多 ML 特性推送到 Oracle 自治数据库中；[微软也这么做了](https://docs.microsoft.com/en-us/sql/advanced-analytics/what-is-sql-server-machine-learning?view=sql-server-ver15)。接下来还会有更多，尤其是随着行业开始处理更多极端环境下的 ML 用例(制造业、医药、石油和天然气等)。)，其中时间和性能是最重要的。