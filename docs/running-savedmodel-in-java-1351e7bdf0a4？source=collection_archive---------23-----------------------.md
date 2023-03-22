# Java 中的 Tensorflow SavedModel

> 原文：<https://towardsdatascience.com/running-savedmodel-in-java-1351e7bdf0a4?source=collection_archive---------23----------------------->

## 如何将您的 TF SavedModel 与当前版本的 [Tensorflow Java](https://github.com/tensorflow/java) 一起使用

![](img/22db01d78781dd7d507dabac5c4eb7ff.png)

来源:https://unsplash.com/@ricaros

# 介绍

机器学习最流行的编程语言是 Python。大多数数据科学家和 ML 工程师用 Python 来构建他们管道。尽管 Python 是一个广泛使用的解决方案，但它可能不是所有栈或用例的最佳选择。在本文中，我将向您展示如何在当前版本的 [Tensorflow Java](https://github.com/tensorflow/java) 中使用您的 SavedModel，这可能会有用。

**注:如你所见**[**tensor flow Java**](https://github.com/tensorflow/java)**还在建设中。因此，可能会发生一些意想不到的行为。请记住这一点。**

# 设置 maven 项目

首先创建一个 Maven 项目并打开 pom.xml。我用 Java 11，因为它是 LTS。

```
<properties>
  <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  <maven.compiler.source>1.11</maven.compiler.source>
  <maven.compiler.target>1.11</maven.compiler.target>
</properties>
```

接下来，将 Tensorflow Java 工件添加到项目中。在撰写本文时，它们还没有托管在 maven 上。因此，您必须手动添加 Sonatype 的 OSS 库。你总能在他们的 Github 资源库 [Tensorflow/Java](https://github.com/tensorflow/java) 中找到最新的指令。

```
<repositories>
    <repository>
        <id>tensorflow-snapshots</id>               <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
</repositories>

<dependencies>
    <dependency>
       <groupId>org.tensorflow</groupId>
       <artifactId>tensorflow-core-platform</artifactId>
       <version>0.2.0-SNAPSHOT</version>
    </dependency>
</dependencies>
```

# 准备 SavedModel

出于演示的目的，我将使用 GPT-2 模型。但是任何其他 SavedModel 都可以。下载/创建 SavedModel 文件后，我将整个文件夹移动到 Java 项目的 resource 文件夹中。然后转到终端，输入以下行:

```
saved_model_cli show --dir PATH/TO/SAVEDMODEL/FOLDER --all
# my output:
MetaGraphDef with tag-set: 'serve' contains the following SignatureDefs:signature_def['predict']:
  The given SavedModel SignatureDef contains the following input(s):
  inputs['context'] tensor_info:
    dtype: DT_INT32
    shape: (1, -1)
    name: Placeholder:0
  The given SavedModel SignatureDef contains the following          output(s):
  outputs['sample'] tensor_info:
    dtype: DT_INT32
    shape: (1, -1)
    name: sample_sequence/while/Exit_3:0
Method name is: tensorflow/serving/predict
```

这为您提供了关于模型的所有相关信息，例如标签集、输入和输出节点等。执行推理时需要这些信息。

# 运行 SavedModel

创建一个新的 Java 类，创建一个 main 方法，并添加以下代码行来加载带有特定标记集的 SavedModel:

```
SavedModelBundle model = SavedModelBundle.load(modelPath, "serve");
```

此时，我们需要为模型创建输入张量。所有可用转换的概述可在[TensorCreation.java](https://github.com/tensorflow/java-models/blob/master/tensorflow-examples/src/main/java/org/tensorflow/model/examples/tensors/TensorCreation.java)中找到。在我的例子中，GPT-2 取一个带有形状(1，-1)和 int 值的张量。所以我用一些随机手写的 int 值创建了一个形状为(1，9)的张量

```
IntNdArray input_matrix = NdArrays.ofInts(Shape.of(1, 9));
input_matrix.set(NdArrays.vectorOf(1, 2, 3, 5, 7, 21, 23, 43, 123), 0);
Tensor<TInt32> input_tensor = TInt32.tensorOf(input_matrix);
```

然后创建一个映射，它有一个字符串键和一个张量作为值。关键是输入张量的名称和您将用作输入的张量的值。这应该会让你想起以前的 Tensorflow 1。x 天，您必须创建一个 feed_dict，并在会话期间将其插入到图表中。在 Java 中，我们可以简单地创建一个散列表，并将我们创建的输入传感器插入其中:

```
Map<String, Tensor<?>> feed_dict = new HashMap<>();
feed_dict.put("context", input_tensor);
```

剩下要做的就是调用 signature_def 函数。

```
model.function("predict").call(feed_dict)
```

并且你完成了，所有的代码都可以在这个[要点](https://gist.github.com/steven-mi/c4578d312546bd2414b152331100ac26)中找到。