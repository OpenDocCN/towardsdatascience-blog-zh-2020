# Hopsworks ML 实验 MLflow 的开源替代方案

> 原文：<https://towardsdatascience.com/one-function-is-all-you-need-for-ml-experiments-43c04bc331cb?source=collection_archive---------33----------------------->

TLDR；Hopsworks 为机器学习(ML)实验提供支持。也就是说，它可以自动跟踪您的 ML 程序的工件、图形、性能、日志、元数据和依赖性。你们中的许多人已经知道像 [MLflow](https://mlflow.org/) 这样的平台，那么为什么还要阅读 Hopsworks 实验呢？因为你不必重写你的 tensor flow/py torch/Scikit-learn 程序来获得免费的**跟踪和分发 ML**，TensorBoard 是内置的。我们将讨论 Hopsworks 如何独特地支持隐式起源来透明地创建元数据，以及它如何与遗忘训练函数相结合来使您的训练分布透明。

# Hopsworks 游戏攻略

Hopsworks 是数据科学和数据工程的单一平台，既有[开源平台](http://github.com/logicalclocks/hopsworks)又有 [SaaS 平台](http://www.hopsworks.ai/)，包括内置的[功能库](https://www.logicalclocks.com/hopsworks-featurestore)。您可以在 GPU 上大规模训练模型，使用 pip/conda 轻松安装任何您想要的 Python 库，将 Jupyter 笔记本作为作业运行，将这些作业放在气流管道中，甚至编写大规模运行的(Py)Spark 或 Flink 应用程序。

作为一个开发环境，Hopsworks 提供了一个集中的协作开发环境，使机器学习团队能够轻松地与队友分享结果和实验，或为项目利益相关者生成报告。所有资源在 Hopsworks 中都有强大的安全性、数据治理、备份和高可用性支持，而资产则存储在单一的分布式文件系统中(数据存储在云中的 S3 上)。

![](img/252ab3c3455fc851eb9fb85c323dd64d.png)

Hopsworks ML 实验存储关于您的 ML 训练运行的信息:日志、图像、感兴趣的度量(准确性、损失)、用于训练模型的程序、其输入训练数据以及所使用的 conda 依赖性。可选输出为超参数、张量板和火花历史服务器。

![](img/e85c28449468200fa665c51e62e24918.png)

每个超参数试验的日志都可以通过点击其日志来检索，TensorBoard 可以可视化不同的试验结果。TensorBoard HParams 插件也可用于进一步深入测试。

# 跟踪

当您在 Hopsworks 平台上运行 Python 或 PySpark 应用程序时，它可以创建一个**实验**，其中包括程序生成的传统信息(结果、日志、错误)以及特定于 ML 的信息，以帮助跟踪、调试和重现您的程序及其输入和输出:

*   **超参数**:ML 程序本身不更新的训练运行参数；
*   **度量**:在这个实验中训练的模型的损失或准确性；
*   **程序神器**:*python/pyspark/air flow**程序、*及其 *conda 环境*；
*   **模型工件**:序列化的*模型对象、* *模型模式*和*模型检查点*；
*   **执行**:能够重新执行实验的信息，包括参数、输入的版本化特征、输出文件等；
*   **版本化特征**:为了能够重现一个实验，我们需要来自运行的精确的训练/测试数据，以及它是如何从特征库中被创建的；
*   **可视化**:训练和评分过程中生成的图像。还可以使用 TensorBoard 可视化训练——hops works 透明地聚合所有员工的结果；
*   **日志(用于调试)**:模型权重、梯度、损耗、优化器状态；
*   **自定义元数据**:标记实验并对其进行自由文本搜索，管理实验(标记为‘PII’、‘数据保持期’等)，并重现训练运行。

![](img/bc0b3f75226c27bea131f1399b99b8bf.png)

# 在一个库中进行实验跟踪和分布式 ML

![](img/68b4882003f6f179f58457edd1b968bb.png)

```
def train(data_path, max_depth, min_child_weight, estimators):
    X_train, X_test, y_train, y_test = build_data(..)
    ...
    print("hello world") # monkeypatched - prints in notebook
    ...
    model.fit(X_train, y_train) # auto-logging
    ...
    hops.export_model(model, "tensorflow",..,model_name)
    ...
    # create local files ‘logile.txt’, ‘diagram.png’
    return {'accuracy': accuracy, 'loss': loss, 'logfile':
       'logfile.txt', 'diagram': 'diagram.png'} # track dictfrom maggy import experiment
experiment.lagom(train, name="My Experiment", ...)# To launch as a distributed ML HParam Tuning job:
# sp=Searchspace(max_depth=('INTEGER',[2,8]),min_child_weight
# =('INTEGER', [2, 8]), )
# experiment.lagom(train, name=“HP, optimizer='randomsearch',
# direction='max', num_trials=15,)
```

支持实验跟踪的平台要求用户在某个函数或某个显式作用域(如 MLFlow 中的“with … as xx:”参见附录 A)中重构他们的训练代码，以确定实验何时开始，何时结束。在 Hopsworks 中，我们要求开发人员在函数中编写他们的训练代码。

我们称这个 Python 函数为*不经意训练函数*，因为这个函数不知道它是在 Jupyter 笔记本的 Python 内核上运行还是在集群中的许多工人上运行，详见我们的[博客](https://www.logicalclocks.com/blog/unifying-single-host-and-distributed-machine-learning-with-maggy)和 [Spark/AI 峰会演讲](https://www.logicalclocks.com/blog/unifying-single-host-and-distributed-machine-learning-with-maggy)。也就是说，您只需编写一次训练代码，在笔记本电脑上训练小型模型时，或者在大型 GPU 或 CPU 集群上执行超参数调优或分布式训练时，就可以重用相同的函数。

我们还使用这个“包装器”Python 函数来启动/停止实验跟踪。实验跟踪和分布透明度在一个单一的功能，很好！

在 Hopsworks 中，[玛吉](https://github.com/logicalclocks/maggy)库运行实验，见上面的代码片段。如您所见，与最佳实践 TensorFlow 程序相比，用户需要的唯一代码更改是:

1.  因子训练代码在一个用户定义的函数( **def train(..):**)；
2.  返回一个 Python dict，其中包含用户希望在实验中被跟踪以及稍后在实验 UI 中可访问的结果、图像和文件；和
3.  使用*实验. lagom* 功能调用训练功能。

超参数可以在一次执行中固定下来，或者如代码片段的最后 4 行所示，您可以执行 *train 函数*，作为一个分布式超参数调优作业，跨多个并行工作器执行(如果需要，可以使用 GPU)。

Hopsworks 将自动:

*   追踪训练函数的所有参数作为这个实验的超参数，
*   在 model.fit 中使用 Keras 回调自动记录日志；
*   在 HopsFS 中创建一个版本化的目录，其中聚集了程序的副本、它的 conda 环境以及来自所有 workers 的所有日志；
*   跟踪此应用程序的所有出处信息—来自此实验中使用的 HopsFS 的输入数据(来自功能存储的训练/测试数据集)，以及所有输出工件(模型、模型检查点、应用程序日志)；
*   将 workers 中执行的所有 print 语句重定向到 Jupyter notebook 单元格，以便于调试(见下面的 GIF 每个 print 语句都以 worker ID 为前缀)。

![](img/90892cc26cb8d52891705885daf3e2ea.png)

在 Hopsworks，工人的日志可以在培训期间打印在你的 Jupyter 笔记本上。拿着数据块！

# 张量板支架

```
def train():
from maggy import tensorboard
...
model.fit(.., callbacks=[TensorBoard(log_dir=tensorboard.logdir(),..)], ...)
```

TensorBoard 可以说是用于可视化、分析和调试机器学习实验的最常见和最强大的工具。Hopsworks 实验与 TensorBoard 无缝集成。在训练函数内部，数据科学家可以简单地导入*tensorboard*python*模块，并获得写入所有 tensor board 文件的文件夹位置。然后从每个执行器收集文件夹的内容，并放在 HopsFS 的实验目录中。由于 TensorBoard 支持在同一个图形中显示多个实验运行，可视化和比较多个超参数组合变得像启动实验服务中集成的 TensorBoard 一样简单。默认情况下，Tensorboard 配置了有用的插件，如 HParam、Profiler 和调试。*

# *分析和调试*

*Hopsworks 1.4.0 附带了 TensorFlow 2.3，其中包括 TensorFlow 分析器。一个期待已久的新功能，最终允许用户分析模型训练，以确定训练过程中的瓶颈，如 CPU + GPU 配置中缓慢的数据加载或糟糕的操作放置。*

*TensorFlow 2.3 还包括调试器 V2，可以轻松找到 NaN 之类的模型问题，这对于在复杂模型中找到问题的根本原因非常重要。*

*![](img/5159645db32371c8e656ed0d243fd2d6.png)**![](img/e49966ea267ef4217c2ae2ab8f30cd59.png)*

# *模型注册表*

*![](img/4de7edc5c035b5ae60261dd15fa408b4.png)*

*在训练代码中，模型可以导出并保存到 HopsFS。使用[跳跃库](https://hops-py.logicalclocks.com/)中的*模型* python 模块，很容易对模型进行版本化并附加有意义的元数据，以反映给定模型版本的性能。*

*Hopsworks Model Registry 是一项服务，其中列出了所有模型以及有用的信息，如哪个用户创建了模型、不同的版本、创建时间和评估指标(如准确性)。*

*模型注册中心提供了基于模型名称、版本号和导出模型的用户进行过滤的功能。此外，可以在 UI 中对模型版本的评估度量进行排序，以找到给定模型的最佳版本。*

*在模型注册表 UI 中，您还可以导航到用于训练模型的实验，并从那里导航到用于训练模型的训练/测试数据，以及从那里导航到用于创建训练/测试数据的功能存储中的功能。谢谢出处！*

# *导出模型*

*通过使用*模型*模块中的*导出*功能，可以以编程方式导出模型。在导出模型之前，实验需要将模型写到 HopsFS 上的文件夹或路径中。然后，将该路径与模型的名称和应该附加的评估指标一起提供给函数。 *export* 调用将把文件夹的内容上传到您的模型数据集中，它也将出现在模型注册表中，每次导出都会增加一个版本号。*

```
*from hops import model# local path to directory containing model (e.g. .pb or .pk)
path = os.getcwd() + “/model_dir”# uploads path to the model repository, metadata is a dict of metrics
model.export(path, “mnist”, metrics={‘accuracy’: acc})*
```

# *获取最佳模型版本*

*当将模型部署到实时服务基础架构或加载模型以进行离线批量推理时，应用程序可以查询模型库，以根据附加到模型版本的元数据(如模型的准确性)找到最佳版本。在以下示例中，将返回精确度最高的 MNIST 模型版本。*

```
*from hops import model  F
from hops.model import Metric
MODEL_NAME=”mnist”
EVALUATION_METRIC=”accuracy”best_model = model.get_best_model(MODEL_NAME, EVALUATION_METRIC, Metric.MAX)print(‘Model name: ‘ + best_model[‘name’])
print(‘Model version: ‘ + str(best_model[‘version]))
print(best_model[‘metrics’])*
```

# *细节决定成败*

*以上是 Hopsworks 实验和模型注册表的简要概述。您现在可以在 [www.hopsworks.ai](http://www.hopsworks.ai/) 上试用它，或者在您可以接触到的任何服务器或虚拟机上安装 Hopsworks Community 或 Enterprise。如果您想了解更多关于我们如何实现管道的信息，请继续阅读。*

# *具有 PySpark 的透明分布式 ML*

*Hopsworks 使用 PySpark 透明地分发遗忘训练功能，以便在工人身上执行。如果 GPU 由工人使用，Spark 会将 GPU 分配给工人，并支持动态执行器，确保在训练功能返回后释放 GPU，[在此阅读更多信息](https://www.logicalclocks.com/blog/optimizing-gpu-utilization-in-hops)。这使您能够保持笔记本电脑打开，并以交互方式可视化培训结果，而不必担心您仍在为 GPU 付费。*

*与将[训练代码作为 Docker 映像(如 AWS Sagemaker](https://docs.aws.amazon.com/sagemaker/latest/dg/your-algorithms.html) )提供的方法相比，Hopsworks 编程模型的优势在于，您可以就地编写定制的训练代码，并直接在笔记本上进行调试。您也不需要为训练代码编写 Docker 文件，Python 的依赖关系可以通过使用来自 Hopsworks UI 的 [PIP 或 Conda 简单地安装库来管理(我们为您透明地编译 Docker 映像)。](https://hopsworks.readthedocs.io/en/latest/user_guide/hopsworks/python.html?highlight=pip#installing-libraries)*

*![](img/908457ef37e1c2f081a51f89668adb81.png)*

*遗忘训练函数可以在不同的执行环境中运行:Python 内核中的 Jupyter 笔记本(最左边)，并行 ML 实验(中间)，以及集体 allreduce 数据并行训练(最右边)。玛吉和霍普斯沃斯负责复杂的任务，如安排任务、收集结果和生成新的超参数试验。*

*![](img/4b2e01ce3f3721c5e779191570c69e61.png)*

*HopsFS 存储实验数据和工人在培训期间生成的日志。当通过 API 启动一个实验时，会在 HopsFS 的实验数据集中创建一个子文件夹，并将关于该实验的元数据附加到该文件夹。Hopsworks 使用隐式出处自动将这些元数据同步到 elasticsearch。*

*元数据可以包括诸如实验名称、实验类型、导出的模型等信息。由于实验的存在是由目录跟踪的，这也意味着删除文件夹也会从跟踪服务中删除实验及其关联的元数据。*

# *跟踪具有隐含出处的元数据*

*![](img/c319216063fc73393c4263ab6f3ff899.png)*

*用于跟踪 ML 工件谱系的现有系统，如 TensorFlow Extended 或 MLFlow，需要开发人员更改他们的应用程序或库代码，以将跟踪事件记录到外部元数据存储中。*

*在 Hopsworks 中，我们主要使用隐式出处来捕获元数据，在这里我们使用我们的分布式文件系统、HopsFS 和一些库来捕获对 ML 工件的更改，只需要对标准 TensorFlow、PyTorch 或 Scikit-learn 程序进行最少的代码更改(详见我们的[USENIX OpML’20 论文](https://www.usenix.org/conference/opml20/presentation/ormenisan))。*

*![](img/f91b2c6d26c049b8a7536dfb38d42145.png)*

*文件系统事件，例如从训练/测试数据集中读取特征，以及将模型保存到目录中，这些事件在 HopsFS 中被隐式记录为元数据，然后在 Elasticsearch 中被透明地索引。这允许在 UI 中对 ML 工件、元数据和实验进行自由文本搜索。*

*![](img/1bbf64fbaa4bb68533b63efee72790e3.png)*

*Hopsworks 中的实验是 ML 训练管道的第一部分，从特征存储开始，到模型服务结束。ML 工件(训练/测试数据集、实验、模型等)可以存储在 HopsFS 上，它们还可以附加定制的元数据。*

*定制元数据与工件紧密耦合(删除文件，其元数据将被自动清除)——这是通过将元数据存储在 HopsFS 使用的相同横向扩展元数据层中来实现的。这种定制元数据也会自动同步到 Elasticsearch(使用一种叫做 [ePipe](https://ieeexplore.ieee.org/document/8752956) 的服务)，从而在 Hopsworks 中实现对元数据的自由文本搜索。*

# *伙计们，现在就到这里吧！*

*在所有用于数据科学的开发工具中，管理 ML 实验的平台近年来出现了最多的创新。开源平台已经出现，如 MLFlow 和我们的 Hopsworks 平台，以及专有的 SaaS 产品，如 WandB、Neptune、Comet.ml 和 Valohai。*

*Hopsworks 实验的不同之处是什么？您可以编写干净的 Python 代码，并分别借助隐式出处和不经意训练函数免费获得实验跟踪和分布式 ML。*

*越来越多的人一致认为，平台应该跟踪 ML 实验中进出的东西，以便进行调试和再现。您可以检测您的代码来跟踪输入/输出，或者您可以让框架使用隐式来源来管理它。*

*Hopsworks 实验是我们降低 ML 投入生产的复杂性的任务中的一个关键组成部分。未来几个月，实时功能工程和监控运营模型领域将出现更多突破性创新。敬请期待！*

# *附录 A*

*在下面的代码片段中，我们比较了如何使用 MLFlow 编写 Hopsworks 实验。相似之处多于不同之处，但是在 Hopsworks 中不需要显式记录到跟踪服务器。*

```
*def train(data_path, max_depth, min_child_weight, estimators):
    X_train, X_test, y_train, y_test = build_data(..)
    ...
    print("hello world") # monkeypatched - prints in notebook
    ...
    model.fit(X_train, y_train) # auto-logging
    ...
    hops.export_model(model, "tensorflow",..,model_name)
    ...
    # create local files ‘logile.txt’, ‘diagram.png’
    return {'accuracy': accuracy, 'loss': loss, 'logfile':
       'logfile.txt', 'diagram': 'diagram.png'} # track dictfrom maggy import experiment
experiment.lagom(train, name="My Experiment", ...)# To launch as a distributed ML HParam Tuning job:
# sp=Searchspace(max_depth=('INTEGER',[2,8]),min_child_weight
# =('INTEGER', [2, 8]), )
# experiment.lagom(train, name=“HP, optimizer='randomsearch',
# direction='max', num_trials=15,)def train(data_path, max_depth, min_child_weight, estimators, model_name):  # distribution external
X_train, X_test, y_train, y_test = build_data(..)
mlflow.set_tracking_uri("jdbc:mysql://username:password@host:3306/database")
mlflow.set_experiment("My Experiment")
with mlflow.start_run() as run:
    ...
    mlflow.log_param("max_depth", max_depth)
    mlflow.log_param("min_child_weight", min_child_weight)
    mlflow.log_param("estimators", estimators)
    with open("test.txt", "w") as f:
        f.write("hello world!")
    mlflow.log_artifacts("/full/path/to/test.txt")
    ...
    model.fit(X_train, y_train) # auto-logging
    ...
    mlflow.tensorflow.log_model(model, "tensorflow-model",
      registered_model_name=model_name)*
```

*像 MLFlow，但是更好？*

# *附录 B*

***管道**是编排端到端培训和模型部署工作执行的程序。在 Hopsworks 中，您可以将 Jupyter 笔记本作为可调度作业在 Hopsworks 中运行，这些作业可以作为 Airflow 管道的一部分运行(Airflow 也是 Hopsworks 的一部分)。管道运行后，数据科学家可以在实验服务中快速检查训练结果。*

*构成完整培训和部署流程的典型步骤包括:*

*   *通过从特征存储中选择特征来具体化训练/测试数据，*
*   *对训练/测试数据进行模型训练并将模型导出到模型注册处，*
*   *模型的评估和验证，如果它通过了健壮性、偏差和准确性测试，模型部署。*

*这篇文章最初发表在逻辑时钟网站上。所有图片版权归 Logical Clocks AB 所有，经许可使用。*