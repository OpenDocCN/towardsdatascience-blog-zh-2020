# 如何使用 TensorFlow 2.0 构建高效的音频数据管道

> 原文：<https://towardsdatascience.com/how-to-build-efficient-audio-data-pipelines-with-tensorflow-2-0-b3133474c3c1?source=collection_archive---------12----------------------->

## 使用 TensorFlow 的数据集 API 消除培训工作流程中的瓶颈

![](img/c3a0521ad4d29eab4b672ebb0c1af57b.png)

由[罗迪翁·库察耶夫](https://unsplash.com/@frostroomhead?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/pipeline?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

GPU 可以大大加快你的训练时间。为了获得最佳性能，您需要为他们的每个训练步骤提供足够的数据。否则，事情会变得*真的*缓慢。💤

作为一名研究人员，看着一个几乎没有移动的进度条，看着你的 GPU 闲置，肯定是你最沮丧的经历之一。

至少对我来说是这样。

尤其是在你研究的早期阶段，关键是你能尝试新的想法并快速测试你的假设，以朝着正确的方向前进。当你脑子里满是想法时，没有什么比等待几个小时才有结果更糟糕的了。

面临的挑战是在当前步骤完成之前，及时将数据从硬盘传输到 GPU，以便进行下一步培训。这包括从磁盘加载和解码原始字节，[预处理](/how-to-easily-process-audio-on-your-gpu-with-tensorflow-2d9d91360f06)并可能增加数据，以及准备批处理。

这正是你的*数据管道的*工作。

这篇文章将带领你使用`tf.data` API 为音频数据构建一个高效的数据管道。

> `tf.data` API 使得处理大量数据、读取不同的数据格式以及执行复杂的转换成为可能。— [张量流导](https://www.tensorflow.org/guide/data)

我找到了关于如何为分散在网络上的音频数据建立有效的数据管道的指导和提示。我将这些编译到这篇文章和代码示例中，供您开始构建自己的代码。

我会在这篇文章的最后链接到相关的资源。

# 首先，你真的需要一个数据管道吗？

看情况。

只要数据集足够小，能够放入内存，就可以在开始训练之前简单地加载一次，然后反复迭代。在我们开始处理更大的数据集之前，这是我们大多数人一直采用的标准方法。

当你*无法再*将所有数据放入内存时，你就需要按需加载数据，让你的 GPU 保持忙碌。但是，您仍然希望能够像以前一样对数据进行洗牌、批处理和预处理。

数据管道可以帮助你实现这一点。

# 如何用三个简单的步骤建立你的第一个数据管道

官方 TensorFlow 教程关于[加载和预处理数据](https://www.tensorflow.org/tutorials/load_data)包含了很多有用的信息和例子。不幸的是，没有一个例子是关于音频数据的。

不过，一个好的起点是关于[加载图像](https://www.tensorflow.org/tutorials/load_data/images#load_using_tfdata)的部分。

基于本教程，您可以通过三个简单的步骤使用`tf.data` API 构建您的第一个数据管道:

1.  创建一个由`file_paths`和`labels`组成的数据集
2.  编写一个解析函数，从文件路径中加载并解码 WAV 文件
3.  使用`Dataset.map()`创建一个`[audio, label]`对的数据集

把这些碎片放在一起，你可能会得到这样的结果:

使用 TensorFlow 数据集 API 的首个数据管道

很简单。

但这只是故事的一半。

不幸的是，对于任何合理大小的数据集，如果您遵循上面的方法，您的性能将会受到很大的影响。您的 GPU 和 CPU 将几乎闲置，但磁盘 I/O 达到最大。你的硬盘已经成为瓶颈。

让我解释一下。

你可能有一个强大的 GPU 和多核 CPU，但只有一个物理硬盘，只有一个磁头来读取数据。第 26 行的`.map()`调用在可用的 CPU 核上并行运行`load_audio`解析函数。

现在，有许多进程试图从磁盘上的随机位置读取数据，但仍然只有一个磁头进行读取。有了这些跳跃，这不会比一个*单个*读取过程快多少。

所以，如果你阅读成千上万的小文件，比如几秒钟长的 WAV 文件，寻找正确的位置和打开每个文件的开销将成为瓶颈。

但是不要担心，有一个解决办法。

# 使用 TFRecords 显著加快您的数据管道

TFRecords 可以存储二进制记录的序列。这意味着您可以将许多 WAV 文件放入一个 TFRecord 中，并增加每次磁盘读取的数据吞吐量。

理论上，你可以在一个单独的 TFRecord 文件中存储所有的 WAV 文件。TensorFlow 文档[建议将数据分成多个 TFRecord *碎片*，每个碎片的大小在 100 MB 到 200 MB 之间。实际上，为了利用并行化，拥有至少与 CPU 内核一样多的 TFRecord 碎片也是有意义的。](https://www.tensorflow.org/tutorials/load_data/tfrecord)

在一个 TFRecord 分片中有数百个 WAV 文件，您可以减少磁盘 I/O，因为您只需要打开一个文件来读取和访问许多音频文件。您增加了每次磁盘读取的数据吞吐量，并消除了磁盘 I/O 瓶颈。

这个概念实际上非常类似于在 web 和游戏开发中使用 sprite-sheets，将一堆小图像(sprite)组合成一个大图像(sheet)，以提高性能和启动时间，并减少整体内存使用。

但是有一个问题。

转换成 TFRecord 格式(并读取它)需要相当多的样板文件。要转换，您需要:

1.  打开一个 TFRecords 文件:`out = tf.io.TFRecordWriter(path)`
2.  将数据转换为三种数据类型之一:`tf.train.BytesList`、`tf.train.Int64List`或`tf.train.FloatList`
3.  通过将转换后的数据传递给`tf.train.Feature`来创建一个`feature`
4.  通过将`feature`传递给`tf.train.Example`来创建一个`example`协议缓冲区
5.  序列化`example`并写入 TFRecords 文件:`out.write(example.SerializeToString())`

如果这还不够混乱的话，看看它在代码中的样子:

要转换为 TFRecord 格式的样板文件

这里有一个小脚本来展示如何将 WAV 文件转换成 TFRecord 碎片:

将 WAV 文件转换为 TFRecord 格式

在开始复制和粘贴上面的代码之前，有必要考虑一下 [TensorFlow 文档](https://www.tensorflow.org/tutorials/load_data/tfrecord)中的这句话:

> 没有必要转换现有代码来使用 TFRecords，除非你正在使用`*tf.data*`并且读取数据仍然是训练的瓶颈。

如果您确实发现读取数据是您的瓶颈，您可能想要查看上面脚本的这个更精细的版本[。它与可用的 CPU 内核并行编写 TFRecord 碎片，并估计每个碎片应该包含的文件数量保持在 100 MB 到 200 MB 之间。](https://gist.github.com/dschwertfeger/3288e8e1a2d189e5565cc43bb04169a1)

最后，每个碎片中文件的数量很重要。它需要足够大，以消除磁盘 I/O 瓶颈。在我找到压缩 TFRecords 的选项之前，每个碎片只能容纳大约 100 个 WAV 文件。我最终得到了几千个碎片，这意味着对太少的数据进行了太多的读取。

# 使用`tf.data.TFRecordDataset`加载数据集

一旦你把 WAV 文件转换成 TFRecords，你需要对你的数据管道做一些调整:

1.  列出所有 TFRecord 文件`tf.data.Dataset.list_files('*.tfrecord')`
2.  将 TFRecord 文件读取为`tf.data.TFRecordDataset`
3.  解析和解码每个序列化的`tf.train.Example`

因为每个示例都存储为二进制协议缓冲区，所以您需要提供一个解析函数，将这些消息转换回张量值。

以下是这种情况的一个示例:

使用 TFRecords 的数据管道

# 享受高效的数据管道

就这样。这就是你如何为音频数据建立一个有效的数据管道。

我们从一个简单的数据管道开始，它基于 TensorFlow 指南中的一个介绍性示例。我们发现了磁盘 I/O 瓶颈。我们把 WAV 文件转换成 TFRecords 来消除它。最后，我们更新了数据管道来读取 TFRecords 并从中加载音频数据。

如果你想更深入一点，我会给你一些启发和帮助创建这篇文章的资源链接。你可以在下面找到它们。

但首先，我想听听你的意见。你采取的是相似还是完全不同的方法？你有什么见解想与我和其他读者分享吗？请把它们贴在评论里。

记住，一个系统的速度取决于它最慢的组件。

# 如果你想更深入一点

*   `[tf.data](https://www.tensorflow.org/guide/data)` [:构建 TensorFlow 输入管道](https://www.tensorflow.org/guide/data)
*   [使用](https://www.tensorflow.org/guide/data_performance) `[tf.data](https://www.tensorflow.org/guide/data_performance)` [API](https://www.tensorflow.org/guide/data_performance) 性能更佳
*   [TFRecord 和](https://www.tensorflow.org/tutorials/load_data/tfrecord)
*   [TPU 速度数据管道:](https://codelabs.developers.google.com/codelabs/keras-flowers-data/#0) `[tf.data.Dataset](https://codelabs.developers.google.com/codelabs/keras-flowers-data/#0)` [和 TFRecords](https://codelabs.developers.google.com/codelabs/keras-flowers-data/#0)
*   [斯坦福 CS230:构建数据管道](http://cs230.stanford.edu/blog/datapipeline/)