# 在脚本模式下，将您的 TensorFlow 培训带到 AWS Sagemaker

> 原文：<https://towardsdatascience.com/bring-your-tensorflow-training-to-aws-sagemaker-with-script-mode-aba9e73aa36b?source=collection_archive---------37----------------------->

## 使用 Sagemaker 脚本模式节省深度学习计算成本！

# 介绍

我会尽量让这篇文章简单明了。首先，我们先从这种方法的一些利弊谈起。

![](img/824a9b938bca9b124942f70ea6a2702f.png)

Artem Sapegin 在 [Unsplash](https://unsplash.com/s/photos/script?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

**优点:**

*   允许您以更低的成本运行您的培训工作！为什么？因为你可以仅仅为了编码而启动一个非常廉价的实例；然后，您可以有一个单独的 GPU 实例，它只在训练期间启动，然后在您的训练代码运行完毕后关闭。这意味着你只在模型训练中消耗 GPU 时间，而不是在你编码的时候！

**CONS:**

*   任何事情都是一种交换，很少有新的优势没有新的劣势。在这种情况下，缺点是我们需要学习一些新的技能。假设你正在运行一个本地培训任务，你想把它带到 Sagemaker *(也许你想在一个更大的 GPU 上运行)*，你想利用这个方法来降低成本，那么你将需要编写更多的代码来让这个方法工作。你可能还需要了解线路和事物是如何一起工作的。

您可以在一个代码更改较少的笔记本实例中运行您的培训，但是您的作业不会自动关闭*(因为您在笔记本实例中执行所有操作)*。所以这是一个权衡。要在笔记本实例中运行您的作业，请参见步骤 1 中的链接。

# 步骤 1:开始使用 Amazon Sagemaker

如果你以前从未使用过 Amazon Sagemaker 或 S3，我推荐你阅读我在 Amazon Sagemaker 上的第一篇文章，我会一步一步地向你展示如何简单地将数据加载到 S3 并运行一个笔记本实例。你可以在这里找到文章[的链接](/a-very-simple-introduction-to-deep-learning-on-amazon-sagemaker-b6ca2426275a)。

# 步骤 2:启动一个廉价的笔记本实例。

当你遵循第一步的文章，你将会做一件不同的事情。我们将启动一个廉价的 CPU 实例，例如类似于 *ml.t3.medium* 的东西，而不是用 GPU 启动一个笔记本实例。

[](https://aws.amazon.com/sagemaker/pricing/) [## 亚马逊 SageMaker 定价-亚马逊网络服务(AWS)

### 有了亚马逊 SageMaker，你只需为你使用的东西付费。构建、培训和部署 ML 模型的费用由…

aws.amazon.com](https://aws.amazon.com/sagemaker/pricing/) 

# 第 3 步:创建一个培训脚本，打开一个新的笔记本并运行您的作业。

一旦运行了一个廉价的笔记本实例，并且访问了 Jupyter，您将创建一个新的 python 脚本并打开一个新的 Jupyter 笔记本。

1.  训练脚本-这个 python 文件是您将拥有训练代码的地方。
2.  朱庇特笔记本——这将是我们的训练指挥官。这个笔记本是要告诉 Sagemaker 我们的训练代码在哪里，我们的训练数据在哪里，模型训练要什么样的机器。

# 朱庇特笔记本代码

这个笔记本可以很短，这取决于你想让它有多复杂。同样，这个笔记本将作为我们的训练指挥官。我在 GitHub 上有代码，你可以在这里查看。在这篇文章中，我将强调一些基础知识。

首先，我们需要从 SAGEMAKER 导入 TensorFlow。这是非常不同的，因为我们不是导入普通的 TensorFlow，而是导入一个允许我们启动 TensorFlow 容器进行训练的对象(即云中包含 TensorFlow 的容器)。

```
**from** **sagemaker.tensorflow** **import** TensorFlow
```

接下来是代码的核心。这是我们为培训工作指定一切的地方。

参数*入口点*指向我们的训练脚本。在这种情况下，我的训练脚本名为 *jigsaw_train1_aws2.py* ，位于我的训练笔记本的工作目录中。

参数 *train_instance_type* 接受一个字符串，该字符串标识我们想要训练什么类型的实例。在本例中，我指定了一个包含 GPU 的 *ml.p2.xlarge* 实例。

参数 *output_path* 表示我希望我的所有输出被转储到哪里。我指着我在 S3 的一个地点。*(这将在我们的培训脚本中再次出现)*。

参数 *train_instance_count* 指定我们需要多少训练实例。在这种情况下，我只需要 *1* ，因为我们没有使用分布式代码，例如 Horovod。

参数 *framework_version* 指定我们想要 TensorFlow 的哪个版本。*(注意，Sagemaker 可能不支持 TensorFlow 的最新版本，所以请注意您指定的版本，并且不要太新；或者如果是，那就支持)*。

参数 *script_mode* 接受一个布尔值，在本例中为 *True* ，因为我们正在提供我们的训练代码。

```
estimator = TensorFlow(entry_point='jigsaw_train1_aws2.py',
                       train_instance_type='ml.p2.xlarge',
                       output_path="s3://jigsaw-toxic-mjy-output",
                       train_instance_count=1,
                       role=sagemaker.get_execution_role(), 
                       framework_version='1.11.0',
                       py_version='py3',
                       script_mode=**True**)
```

然后，如果我们想要提交我们的培训作业，我们只需要运行*估算器*。作为*估算器*的输入，我已经包含了一个关键字为“*data”*的字典，对应的值是一个包含我的数据存储在其中的 S3 位置的字符串。

```
data_s3 = 's3://jigsaw-toxic-mjy/'
inputs = {'data':data_s3}
estimator.fit(inputs)
```

在我们运行这个之前，我们需要确保我们的训练脚本已经准备好了。让我们快速回顾一下我们的培训脚本。

# 培训脚本代码

你可以在 GitHub [这里](https://github.com/yeamusic21/Bring_Your_Training_To_Sagemaker_Demo/blob/master/jigsaw_train1_aws2.py)阅读我的完整训练脚本代码示例。我不打算回顾这篇文章中的所有内容，只回顾要点。

首先，我们需要能够访问我们的数据。我们使用 argparse 来获取我们在 *estimator.fit(inputs)* 期间传递的数据目录。我们可以将数据目录与我们在 S3 获得的训练数据的名称连接起来。一旦我们有了这个字符串，我们就可以像往常一样使用 pandas 来读取我们的数据。

```
import argparse
import pandas as pd
import osparser = argparse.ArgumentParser()parser.add_argument('--data', type=str, default=os.environ.get('SM_CHANNEL_DATA'))args, _ = parser.parse_known_args()data_dir = args.datadf = pd.read_csv(os.path.join(data_dir, 'train.csv'))
```

接下来，我们可以像平常一样运行我们的训练代码了！

最后，如果我们想要保存任何信息，我们需要将它保存到 TensorFlow 容器知道的特定目录中。那个位置是 */opt/ml/model/* 。保存到这个目录的任何东西，都将被保存到我们在 Jupyter 代码中指定的输出路径。

```
data_location = “/opt/ml/model/submission.csv” submission.to_csv(data_location, index=False)
```

# 结束了！

就是这样！一旦您准备好培训脚本，您就可以从头到尾运行您的 Jupyter 笔记本，并观看您的培训工作开始！

再说一次，当你完成后，我会删除一切！这包括你的 S3 桶，你的实例，一切；因为如果你把所有这些工作都放在 AWS 上，即使你不再运行任何东西，也要花钱！

你可以在 GitHub [这里](https://github.com/yeamusic21/Bring_Your_Training_To_Sagemaker_Demo)找到我自己的脚本模式培训工作的例子。这个 repo 包含我的笔记本实例中的代码，因此，它是我的笔记本实例的快照。希望人们发现这是有益和快乐的学习！:-D