# 使用 TF 在 AWS Sagemaker 上快速入门多 GPU 深度学习。分配

> 原文：<https://towardsdatascience.com/quick-start-to-multi-gpu-deep-learning-on-aws-sagemaker-using-tf-distribute-9ee08bc9612b?source=collection_archive---------33----------------------->

## 向霍洛沃德说再见，向 TF 问好。分配

# 介绍

本文是使用 AWS Sagemaker 和 TensorFlow 2.2.0 tf.distribute 运行分布式多 GPU 深度学习的快速入门指南。

![](img/eba57569ffb904ce7f91b1af2c59ab3b.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com/s/photos/distribute?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 密码

我所有与本文相关的代码都可以在我的 GitHub 资源库中找到，这里是。我的存储库中的代码是在 Kaggle 的数据上运行 BERT 版本的一个例子，具体来说就是 [Jigsaw 多语言有毒评论分类](https://www.kaggle.com/c/jigsaw-multilingual-toxic-comment-classification)竞赛。*(我的很多代码都是采用了很棒的* [*顶级公共内核*](https://www.kaggle.com/xhlulu/jigsaw-tpu-xlm-roberta) *。)*

# 了解信息的需要

## 入门指南

首先，我们需要了解我们在 AWS Sagemaker 上运行深度学习的选项。

1.  在笔记本实例中运行您的代码
2.  在定制的 Sagemaker TensorFlow 容器中运行您的代码

在本文中，我们将重点放在选项#2 上，因为它更便宜，并且是 Sagemaker 的预期设计。

*(选项#1 是一个很好的开始方式，但是它更贵，因为你要为笔记本实例运行的每一秒付费)。*

## 运行 Sagemaker TensorFlow 容器

Sagemaker TensorFlow 容器有很大的灵活性，但我们将重点放在最基本的东西上。

![](img/70cfb8b9aa1826ee98b0a30ab673da1f.png)

照片由[乌帕德克·马特米](https://unsplash.com/@thefallofmath?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/container?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

首先，我们需要启动一个 Sagemaker 笔记本实例，并将我们的数据存储在 S3 上。如果你不知道如何做到这一点，我在我的博客上回顾了一些简单的选择。一旦我们在 S3 有了数据，我们就可以启动一个 Jupyter 笔记本*(从我们的笔记本实例)*并开始编码。该笔记本将负责启动您的培训工作，即您的 Sagemaker TensorFlow 容器。

同样，我们将把重点放在最基本的东西上。我们需要一个变量来指示数据的位置，然后我们需要将该位置添加到字典中。

```
data_s3 = 's3://<your-bucket>/'
inputs = {'data':data_s3}
```

很简单。现在我们需要创建一个 Sagemaker TensorFlow 容器对象。

我们的**入口点**是一个 Python 脚本*(我们稍后会制作)*，它包含了我们所有的建模代码。我们还希望 **script_mode** =True，因为我们正在运行自己的训练脚本。

我们的 **train_instance_type** 是一个多 GPU Sagemaker 实例类型。您可以在这里找到 Sagemaker 实例类型的完整列表[。注意一个 ml.p3.8xlarge 运行 4 个](https://aws.amazon.com/sagemaker/pricing/instance-types/)[V100 NVIDIA GPU](https://www.nvidia.com/en-us/data-center/v100/)。由于我们将使用 MirroredStrategy *(稍后将详细介绍)*我们需要*train _ instance _ count = 1。也就是说，1 台机器配有 4 台 V100s。*

*我们还需要将**输出路径**设置到 S3 的一个位置。这是 Sagemaker 将我们保存到路径“/opt/ml/model”中的所有内容自动存储到的地方。例如，如果我们将最终的模型保存到训练脚本中的容器路径“/opt/ml/model”中，那么当训练工作完成时，Sagemaker 会将模型加载到我们的 S3 位置。*

*其他的设置你可以暂时不管，或者根据需要进一步研究。综上所述，我们需要弄对的主要设置有**入口 _ 点、脚本 _ 模式、训练 _ 实例 _ 类型、**和**输出 _ 路径**。*(然后对于 MirroredStrategy 我们需要 train_instance_count=1)。**

```
*# create estimator
estimator = TensorFlow(entry_point='jigsaw_DistilBert_SingleRun_v1_sm_tfdist0.py',
                       train_instance_type='ml.p3.8xlarge',
                       output_path="s3://<your-bucket>",
                       train_instance_count=1,
                       role=sagemaker.get_execution_role(),
                       framework_version='2.1.0',
                       py_version='py3',
                       script_mode=True)*
```

*我们可以通过运行下面的代码来开始我们的培训工作。*

```
*estimator.fit(inputs)*
```

*注意，我们包含了我们的字典*(包含我们在 S3 的位置)*作为‘fit()’的输入。在运行这段代码之前，我们需要创建 Python 脚本，并将其分配给 **entry_point** *(否则我们的容器将没有任何代码可以运行:-P)* **。***

***创建培训脚本***

*我在 GitHub 上的训练脚本非常繁忙，因为我正在对 Kaggle 的一些数据运行 BERT 版本。我们唯一需要的是访问我们的数据，然后我们可以将任何结果保存到容器路径“/opt/ml/model”。*

*![](img/dae7c855a8ee0f1b7f05a87cd14eb6cc.png)*

*在 [Unsplash](https://unsplash.com/s/photos/script?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [Brooks Leibee](https://unsplash.com/@baleibee?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片*

*获取数据的最简单方法是在培训脚本中硬编码您在 S3 的位置。*

*我们还可以从“estimator.fit(inputs)”传递的值中获取 S3 位置。我们可以使用 [argparse](https://pypi.org/project/argparse/) 来做到这一点。*

```
*def parse_args(): 
    parser = argparse.ArgumentParser()
    parser.add_argument(‘ — data’, 
                        type=str,   
                        default=os.environ.get(‘SM_CHANNEL_DATA’)) 
    return parser.parse_known_args()args, _ = parse_args()*
```

*如果我们想做的只是在 Sagemaker 容器中运行我们的培训工作，那基本上就是我们所需要的！现在，如果我们想使用 tf.distribute 运行多 GPU 训练，我们还需要一些东西。*

## *和霍洛佛德说再见，和 TF 打招呼。分配*

*![](img/81d754f8558135d11b98564c4745513f.png)*

*照片由[泰勒维克](https://unsplash.com/@tvick?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/servers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄*

*首先，我们需要表明我们想要运行多 GPU 训练。我们可以用下面的代码很容易地做到这一点。*

```
*strategy = tf.distribute.MirroredStrategy()*
```

*有关 MirroredStrategy 的更多信息，我会查看 [TensorFlow 文档](https://www.tensorflow.org/api_docs/python/tf/distribute/MirroredStrategy)。现在我们将在整个训练代码中使用我们的**策略**对象。接下来，我们需要通过包含以下行来调整多 GPU 训练的批量大小。*

```
*BATCH_SIZE = 16 * strategy.num_replicas_in_sync*
```

*我们将在后面的“model.fit()”调用中使用变量 BATCH_SIZE。*

*最后，在定义模型时，我们需要再次使用**策略**。*

```
*with strategy.scope():
    # define model here*
```

*就是这样！然后，我们可以继续运行我们通常使用的“model.fit()”。*

*同样，与本文相关的完整代码可以在我的 GitHub 存储库中找到，这里是。*

*感谢您的阅读，并希望您发现这是有帮助的！*