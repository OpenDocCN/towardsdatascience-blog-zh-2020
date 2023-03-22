# 在 2.0 中使用 TensorFlow Serving + Docker 部署文本分类器

> 原文：<https://towardsdatascience.com/deploying-a-text-classifier-with-tensorflow-serving-docker-in-2-0-cba6851e46ed?source=collection_archive---------17----------------------->

## 在本教程中，我们使用一个接受原始字符串进行推理的 API 端点来简化模型预测。我们还对模型签名进行了实验，以获得更加可定制的有效负载。

![](img/5d9def44ded0a45bcf567d512c95f6a7.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## **这里是**[**GitHub repo**](https://github.com/happilyeverafter95/tensorflow-serving-2.0)**的全部代码。**

大约一年前，我写了[*Deploy a Keras Model for Text Classification 使用 tensor flow Serving*](/deploying-kaggle-solution-with-tensorflow-serving-part-1-of-2-803391c9648)*在 TensorFlow 1.X 中部署文本分类器。x 更容易使用，我很难找到我需要的文档，特别是关于部署和迁移 TensorFlow 服务的已弃用的 V1 功能。*

*我已经收集了我的知识，并将通过一个玩具示例演示模型部署。*

*如果这是你第一次用 TensorFlow 2。x，我建议学习这个课程来构建、训练和部署模型。*

*[](https://click.linksynergy.com/link?id=J2RDo*Rlzkk&offerid=759505.12488204614&type=2&murl=https%3A%2F%2Fwww.coursera.org%2Flearn%2Fintro-tensorflow) [## 张量流简介

### 本课程的重点是利用 TensorFlow 2.x 和 Keras 的灵活性和“易用性”来构建、培训和…

click.linksynergy.com](https://click.linksynergy.com/link?id=J2RDo*Rlzkk&offerid=759505.12488204614&type=2&murl=https%3A%2F%2Fwww.coursera.org%2Flearn%2Fintro-tensorflow) 

## 在 TF 1 中，文本预处理是一场噩梦。X …

![](img/77ec5ab2c013bf7e1f78b2db9d1726eb.png)

Sebastian Herrmann 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

特别是 2。x 引入了几个新功能，使得使用文本模型变得更加容易。1 的最大痛点之一。x 是如何处理矢量化层。要求消费者应用程序为我的推理 API 提供向量输入并不总是可行的。

因此，创建了许多丑陋的变通方法来允许模型接受原始字符串。点击这里查看我的标记化/截断/填充回退[。](https://github.com/happilyeverafter95/toxic-comment-classifer/blob/master/profanity_detector/model.py#L52)

在许多情况下，预测端点必须包装在另一个端点中，以处理预处理步骤。这造成了一种令人讨厌的情况，即预处理逻辑位于两个不同的地方:用于训练模型的模块和将客户机的原始文本转换成向量的包装器。

让我们看看如何在 2.X 中解决这个问题。* 

# *目标:创建一个 API 端点，该端点以原始字符串的形式接受电影评论，并返回评论为正面的概率*

*这是一个标准的情绪分析任务。为了确保我们构建的解决方案是端到端的:*

*   *服务端点将负责预处理原始字符串，并为模型对其进行矢量化*
*   *除了 TensorFlow 服务，我们不会使用任何其他框架*

*如果你想更进一步，有很多关于将 TensorFlow 模型部署到云平台(如 GCP 或 AWS)的课程和资源。*

*[](https://click.linksynergy.com/link?id=J2RDo*Rlzkk&offerid=759505.13223275109&type=2&murl=https%3A%2F%2Fwww.coursera.org%2Flearn%2Fend-to-end-ml-tensorflow-gcp) [## 基于 GCP tensor flow 的端到端机器学习

### 由谷歌云提供。在本专业的第一门课程中，我们将回顾机器中涵盖的内容…

click.linksynergy.com](https://click.linksynergy.com/link?id=J2RDo*Rlzkk&offerid=759505.13223275109&type=2&murl=https%3A%2F%2Fwww.coursera.org%2Flearn%2Fend-to-end-ml-tensorflow-gcp) 

# 属国

我建议分叉或克隆我的 [GitHub repo](https://github.com/happilyeverafter95/tensorflow-serving-2.0) 并从那里开始跟进。

本文对每个步骤进行了更详细的解释。如果您只是想让一些东西启动并运行起来，那么可以按照我的 README.md 中的说明来做。README.md 包括如何训练和服务模型的细节。

*   [安装 Docker](https://docs.docker.com/get-docker/)
*   转到[我的回购](https://github.com/happilyeverafter95/tensorflow-serving-2.0)并运行`pip install -r requirements.txt`
*   我的代码是为 Python 3.8 编写和测试的

# 数据集:IMDB 电影评论

![](img/41016f1c84e0b5a15eabcbe4a9b703fa.png)

乔恩·泰森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

该数据集包含 25，000 条用于训练的高度极性电影评论，以及另外 25，000 条用于测试的评论。每条影评都标注为 0(负面情绪)或 1(正面情绪)。关于数据集的更多信息可以在[这里](https://www.tensorflow.org/datasets/catalog/imdb_reviews)找到。

我们将训练一个二元分类器来预测给定电影评论是正面的概率。

# 代码

我将在一个`ModelTrainer`类中组织我的大部分代码，该类处理模型的端到端训练和部署。让我们一步一步来。

# 依赖关系

首先，我们加载将要使用的库。我还定义了一个`logger`来跟踪培训过程中的重要事件。在较大的项目中，日志记录通常比打印语句更受青睐，因为它更容易跟踪(更容易看到日志来自哪里)、可配置并且更容易存储。

# 模特教练班

我在这里将模型架构参数定义为属性。在一个更复杂的项目中，将它们移动到一个配置文件中可能更有意义。下面是每个属性所指内容的快速分类:

*   `embed_size`:每个令牌嵌入的维度
*   `max_features`:你词汇量的最大值
*   `epochs`:训练集的迭代次数
*   `batch_size`:一次处理的观察次数
*   `max_len`:每次观察的令牌数；较短的长度将被填充，而较长的长度将被截断

稍后我们将讨论`tf_model_wrapper`属性。

# 获取数据

在这个类中，我定义了一个名为`fetch_data`的方法，它将通过编程从`tensorflow_datasets`下载训练和测试数据。这种下载数据集的方法效率很低。数据集不会保存在任何地方，每次执行脚本时都需要重新加载。

# 矢量化图层

这是获取原始字符串并将其转换为与我们的模型架构兼容的矢量化输出的层。我们还在这一层中构建了预处理步骤，以便在矢量化之前对原始文本进行预处理。

预处理在`custom_preprocessing`方法中定义。我们在这里做三件事:

*   将文本转换为小写
*   在我们的影评中经常出现的脱衣`<br />`
*   去除标点符号

在`init_vectorize_layer`中返回的`TextVectorization`对象将返回一个完全适合的层，该层适应于所提供的文本(这是来自我们的训练数据的电影评论)。除了自定义预处理功能，我们还定义了最大令牌数、输出序列长度和输出模式。

# 初始化模型

首先，我们创建`vectorize_layer`并定义输入类型(单个字符串)。我们通过矢量化层传递原始输入，然后将输出传递给模型架构的其余部分。

我已经把一个简单的 RNN 和一个双向 GRU 层放在了一起。出于说明发球的目的，我没有花太多时间来完善这个模型。我敢肯定，找到一个针对该数据集实现更好结果的模型架构并不困难。

# 训练模型

我们在`train`方法中做的第一件事是使用`fetch_data`加载训练数据。在这个玩具示例中，我们将不会加载测试数据，因为我们跳过了模型评估。*模型评估非常重要，在更实际的场景中不应该被忽略。*

我们使用`init_model`方法初始化模型，并通过`train_examples`来拟合我们的矢量化图层。接下来是在训练数据上拟合模型。

在这个要点的第 4 行，我们最终通过创建一个新的`TFModel`类的实例来定义我们的`tf_model_wrapper`对象。该对象的目的是将*签名*合并到服务模型中。

我将在这里绕道谈论模型签名，以及为什么您可能想要合并它们。

# 模型签名

TensorFlow 文档上的这个[页面解释了签名用于提供“识别函数输入和输出的一般支持，并且可以在构建](https://www.tensorflow.org/tfx/serving/signature_defs) [SavedModel](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/saved_model/builder.py) 时指定”。

> 签名定义需要指定:
> 
> `inputs`作为字符串到 TensorInfo 的映射。
> 
> `outputs`作为字符串到 TensorInfo 的映射。
> 
> `method_name`(对应加载工具/系统中支持的方法名)。

通过定制输入，我们可以在推理之前加入额外的预处理步骤。通过定制输出，我们可以定制来自服务模型的有效负载。

在本例中，我们将创建一个签名，以便在预测中包含一些有用的元数据。

# 创建支持签名的包装器

我们将创建一个`TFModel`作为模型的包装器。初始化时，`TFModel`对象将接受一个`tf.keras.Model`并将它保存为一个`model`属性。这个初始化步骤非常灵活，可以用来传递额外的参数。

我们使用`@tf.function`装饰器定义了一个`prediction`方法。这个装饰器将函数转换成可调用的张量流图。`input_signature`参数指定将提供给签名的形状和数据类型。这个方法的输出成为我们服务模型时的输出。

我们的服务模型有效载荷将有两个字段:一个包含模型输出的`prediction`字段和一个硬编码的`description`字段。

对于我们的用例，模型输出相当直观，不完全需要`description`。然而，当类标签不清楚或者额外的元数据可能有帮助时，这就变成了一个有用的指示器。

# 我们还想传递哪些元数据？

世界是你的！这里有一些想法可能会让你以后的生活更轻松。

## 可听度和再现性

考虑添加有助于调查和重现特定预测的元数据。

*   用于训练该模型的数据集是什么？
*   如果我想在我的机器上重建这个模型，我可以在哪里找到模型资产？
*   用于训练模型的代码在哪里？使用了什么版本的代码(即 Git SHA/pull 请求编号)来训练模型？

## 解释

你的模型的最终消费者可能不是熟悉机器学习的人。在没有任何解释的情况下传递类别概率和相应标签的列表可能会令人困惑。考虑通过顶部预测和相关概率。

## 使用

有时，输出反映您的预期用途可能会有所帮助。也许你有一个不同型号的标量输出的阈值？您可以使用元数据来明确您的预期用途。* 

# *部署模型*

*部署步骤非常简单。我们将模型保存到本地目录，并将`serving_default`签名定义为来自我们`tf_model_wrapper.`的`prediction`方法*

## *实际上拯救了什么？*

*该模型被序列化为 SavedModel 对象，可用于推理。在此过程中，整个 TensorFlow 会话被导出为包含在单个目录中的语言不可知格式。*

*该目录具有以下结构:*

```
*assets/
assets.extra/
variables/
    variables.data-?????-of-?????
    variables.index
saved_model.pb*
```

*   ***saved_model.pb** 定义数据流结构*
*   ***资产**是包含所有辅助模型文件的子目录*
*   ***assets.extra** 是包含由其他库生成的资源的子目录*
*   ***变量**是一个子目录，包含用于恢复模型的权重*

## *指定模型版本*

*默认情况下，TensorFlow 服务将始终为您的模型提供最新版本。“最新”版本是从 SavedModel 目录名推断出来的。对于这个例子，我根据训练时的整数时间戳命名了 SavedModel 目录，因此总是提供最新的模型。*

# *为模型服务*

*确保您在此步骤之前训练了模型。可以通过在克隆的 repo 的根目录下运行`python -m classifier.train`来训练模型。这将运行上述代码。*

*是时候为模特服务了！确保为这一步安装了 Docker。*

1.  *使用`docker pull tensorflow/serving`获取 TensorFlow 服务图像的最新版本*
2.  *在您克隆 GitHub repo 的根目录中，根据 SavedModel 文件的路径创建一个环境变量。如果你按照自述文件上的说明，这将是`export ModelPath=”$(pwd)/classifier”`*
3.  *启动服务器并为 REST API 端点公开端口 8501:*

```
*docker run -t --rm -p 8501:8501 \
    -v "$ModelPath/saved_models:/models/sentiment_analysis" \
    -e MODEL_NAME=sentiment_analysis \
    tensorflow/serving*
```

*会出现一堆日志。每个日志中的第一个字符将指示进程的状态。*

*   ***E =错误***
*   ***W =警告***
*   ***I =信息***

*下面是最初几行可能的样子。*

```
*docker run -t --rm -p 8501:8501    -v "$ModelPath/:/models/sentiment_analysis"    -e MODEL_NAME=sentiment_analysistensorflow/servingI tensorflow_serving/model_servers/server.cc:82] Building single TensorFlow model file config:  model_name: sentiment_analysis model_base_path: /models/sentiment_analysis
I tensorflow_serving/model_servers/server_core.cc:461] Adding/updating models.*
```

*如果每个日志都以 **I、**开头，那么恭喜您——该模型已经成功提供了！*

***注意:**让日志淹没你的终端可能有点烦人。使用分离模式，所以它不会这样做。下面命令的唯一区别是增加了一个`-d`标志。*

```
*docker run -t -d --rm -p 8501:8501 \
    -v "$ModelPath/saved_models:/models/sentiment_analysis" \
    -e MODEL_NAME=sentiment_analysis \
    tensorflow/serving*
```

# *有用的 Docker 命令*

*这里有两个有用的 Docker 命令，在使用模型服务器时可能会派上用场:*

*   *`docker ps` -显示哪些 Docker 容器正在运行；这对于获取容器 id 以便进一步操作非常有用*
*   *`docker kill [container id]` -如果您构建了错误的模型，您可以终止当前容器来释放端口并重启服务器*

# *发布请求*

*既然我们的模型服务器已经在本地机器上启动并运行，我们就可以发送一个示例 POST 请求了。POST 请求可以通过 [curl](https://curl.haxx.se/docs/manpage.html) 发送，这是一个用于在服务器之间传输数据的简单工具，也可以通过 Python 中的`request`库发送。*

***样本卷曲命令:***

*我们通过了“史上最差电影”的评论。*

```
*curl -d '{"inputs":{"review": ["worst movie EVER"]}}' \
  -X POST http://localhost:8501/v1/models/sentiment_analysis:predict*
```

***对应输出:***

*不出所料，“史上最差电影”被认为不太可能是正面评价。*

```
*{ "outputs": { "prediction": [[ 0.091893291 ]], "description": "prediction ranges from 0 (negative) to 1 (positive)" } }*
```

*这次到此为止。希望这篇文章对你有帮助！快乐大厦。*

# *参考*

*   *[https://keras . io/examples/NLP/text _ class ification _ from _ scratch/](https://keras.io/examples/nlp/text_classification_from_scratch/?fbclid=IwAR1_T5oLgYdLfGFllj79BHkZNTuOgJA8m3fQ4awjQkckY9jUQw1eyFDU_rU)*
*   *https://www.tensorflow.org/api_docs*

# *感谢您的阅读！*

*[通过 Medium](https://medium.com/@mandygu) 关注我的最新动态。😃*

*作为一个业余爱好项目，我还在[www.dscrashcourse.com](http://www.dscrashcourse.com/)建立了一套全面的**免费**数据科学课程和练习题。*

*再次感谢您的阅读！📕*