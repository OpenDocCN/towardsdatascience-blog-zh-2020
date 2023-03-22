# 通过 Keras 在 Google Colab 上使用 TPU

> 原文：<https://towardsdatascience.com/using-tpus-on-google-colab-966239d24573?source=collection_archive---------51----------------------->

![](img/b57819ec94c135ebb031259b711b475a.png)

罗曼·维涅斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 今天，我在温习一门关于情感分类的旧 NLP 课程，突然感觉到在一些大型数据集上尝试一下的冲动。我玩得很开心，也看到了 Colab 上的免费 TPU 到底有多快。

速度快了 10 倍，但过程并不简单。包括代码变更(大部分是样板文件)，因此这篇文章。

作为第一步，我只是想建立一个简单的模型来编译和提供一些预测。我选择了 Kaggle 上的[sensition 140 数据集](https://www.kaggle.com/kazanova/sentiment140)。该数据集有 160 万条带有积极和消极情绪标签的推文。它的大小是 288 MB，这对于我的目的来说是很好的。我为训练模型编写的代码非常简单，因为我正在做非常基本的预处理。

**训练时间——几乎每个时代 1 小时 30 分！**这对于最初的模型来说是行不通的。是时候测试一下 Colab 上提供的免费 TPU 了。

我最初以为这只是一个简单的设置变化。所以我进入编辑菜单中的笔记本设置，要求一个 TPU 硬件加速器。训练仍然需要一个多小时，所以很明显没有提供 TPU。在浏览 TPU 文档时(这里:[使用 TPU](https://www.tensorflow.org/guide/tpu) )，很明显我们必须明确设置什么是*计算分布策略*并在它下面构建我们的模型。以下文本中的解释，以及相关的样板文件:

*   首先，我们必须明确要求在代码中使用 TPU。Colab 和真实的 GCP 云 TPU 是不同的，所以必须小心。

```
*import tensorflow as tf
#Get a handle to the attached TPU. On GCP it will be the CloudTPU itself
resolver = tf.distribute.cluster_resolver.TPUClusterResolver(tpu=’grpc://’ + os.environ[‘COLAB_TPU_ADDR’])**#Connect to the TPU handle and initialise it
tf.config.experimental_connect_to_cluster(resolver)
tf.tpu.experimental.initialize_tpu_system(resolver)*
```

接下来，我们设定分销策略

```
*strategy = tf.distribute.experimental.TPUStrategy(resolver)*
```

*   之后，我们创建模型

```
*with strategy.scope():
 model = create_model()#Build your model
 model.compile(optimizer=…)#Set your parameters*
```

*   然后用通常的方式训练它

```
*model.fit(…)*
```

这现在起作用了。过去需要 90 分钟的训练，现在只需 9.5 分钟就能完成。毫无疑问非常有效和高效，尽管主题相当神秘。

问题是:它所需要的只是一些样板代码，那么它为什么没有隐藏在一些 Keras 抽象下呢？也许在未来的版本中会有。

下一篇: [**风雨:在科莱布和 TPU 身上使用 Keras**](https://medium.com/@umash4/trials-and-tribulations-using-keras-on-colab-and-tpu-69378762468d)