# T5 变压器多任务处理指南

> 原文：<https://towardsdatascience.com/the-guide-to-multi-tasking-with-the-t5-transformer-90c70a08837b?source=collection_archive---------10----------------------->

## T5 变压器可以执行任何 NLP 任务。它可以在同一时间使用同一型号执行多项任务。祝您身体健康

![](img/0bcf3e3cae67f3f55dc1bf6ae02857d8.png)

照片由[马特·贝罗](https://unsplash.com/@mbeero?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

T5(文本到文本迁移转换器)模型是一项大规模研究的产物([论文](https://arxiv.org/abs/1910.10683))，旨在探索迁移学习的局限性。它建立在流行的架构之上，如 GPT、伯特和罗伯塔(仅举几个例子)模型，这些模型利用迁移学习取得了令人难以置信的成功。虽然可以对类似 BERT 的模型进行微调以执行各种任务，但体系结构的约束意味着每个模型只能执行一项任务。

通常，这是通过在 Transformer 模型之上添加一个特定于任务的层来实现的。例如，通过添加具有两个输出神经元(对应于每个类别)的全连接层，可以使 BERT 变换器适用于二进制分类。T5 模型通过将所有的自然语言处理任务重新组织为文本到文本的任务而背离了这一传统。这导致任何 NLP 任务的共享框架作为模型的输入，而模型的输出总是字符串。在二进制分类的例子中，T5 模型将简单地输出该类的字符串表示(即`"0"`或`"1"`)。

由于任何 NLP 任务的输入和输出格式都是相同的，相同的 T5 模型可以被训练来执行多个*任务！为了指定应该执行哪个任务，我们可以简单地在模型的输入前加上一个前缀(字符串)。谷歌人工智能博客文章中的动画(如下所示)展示了这一概念。*

![](img/33280ba1458e932920134893d25e0624.png)

来自文章[探索 T5 的迁移学习:文本到文本的迁移转换器](http://ai.googleblog.com/2020/02/exploring-transfer-learning-with-t5.html)

在本文中，我们将使用这种技术来训练一个能够执行 3 个 NLP 任务、二进制分类、多标签分类和回归的 T5 模型。

*所有代码也可以在*[*Github*](https://github.com/ThilinaRajapakse/simpletransformers/tree/master/examples/t5/mixed_tasks)*上找到。*

# 任务说明

## 二元分类

自然语言处理中二元分类的目标是将给定的文本序列分成两类。在我们的任务中，我们将使用 Yelp 评论数据集将文本的情感分为正面(`"1"`)或负面(`"0"`)。

## 多标签分类

在多标签分类中，给定的文本序列应该用一组预定义标签的正确子集来标记(注意，该子集可以包括*空集*和标签本身的完整集)。为此，我们将使用有毒评论数据集，其中每个文本都可以用标签的任何子集来标记`toxic, severe_toxic, obscene, threat, insult, identity_hate`。

## 回归

在回归任务中，目标变量是一个连续值。在我们的任务中，我们将使用 STS-B(语义文本相似性基准)数据集，其目标是预测两个句子的相似性。相似性由`0`和`5`之间的连续值表示。

# 数据准备

因为我们将使用 3 个数据集，所以我们将它们放在`data`目录中的 3 个单独的子目录中。

*   `data/binary_classification`
*   `data/multilabel_classification`
*   `data/regression`

## 下载

1.  下载 [Yelp 评论数据集](https://s3.amazonaws.com/fast-ai-nlp/yelp_review_polarity_csv.tgz)。
2.  抽出`train.csv`和`test.csv`至`data/binary_classification`。
3.  下载[有毒评论数据集](https://www.kaggle.com/c/jigsaw-toxic-comment-classification-challenge/)。
4.  将`csv`文件解压到`data/multilabel_classification`。
5.  下载 [STS-B 数据集](http://ixa2.si.ehu.es/stswiki/index.php/STSbenchmark)。
6.  将`csv`文件解压到`data/regression`。

## 组合数据集

如前所述，T5 模型的输入和输出总是文本。通过使用前缀为*的文本来指定一个特定的任务，让模型知道它应该对输入做什么。*

简单变压器中 T5 模型的输入数据格式反映了这一事实。输入是一个熊猫数据帧，有 3 列— `prefix`、`input_text`和`target_text`。这使得在多个任务上训练模型变得非常容易，因为你只需要改变`prefix`。

上面的笔记本加载每个数据集，为 T5 对它们进行预处理，最后将它们组合成一个统一的数据帧。

这给了我们一个具有 3 个唯一前缀的数据帧，即`binary classification`、`multilabel classification`和`similarity`。注意前缀本身是相当随意的，重要的是确保每个任务都有自己唯一的前缀。模型的输入将采用以下格式:

```
<prefix>: <input_text>
```

*`*": "*`*是训练时自动添加的。**

*其他一些需要注意的事项:*

*   *多标签分类任务的输出是预测标签的逗号分隔列表(`toxic, severe_toxic, obscene, threat, insult, identity_hate`)。如果没有预测到标签，输出应该是`clean`。*
*   *相似性任务的`input_text`包括两个句子，如下例所示；
    `sentence1: A man plays the guitar. sentence2: The man sang and played his guitar.`*
*   *相似性任务的输出是一个介于 0.0 和 5.0 之间的数字(字符串)，增量为 0.2。(如`0.0`、`0.4`、`3.0`、`5.0`)。这与 T5 论文作者使用的格式相同。*

*从不同输入和输出的表示方式可以看出，T5 模型的文本到文本方法在表示各种任务和我们可以执行的实际任务方面为我们提供了很大的灵活性。*

**例如；**

*[](/asking-the-right-questions-training-a-t5-transformer-model-on-a-new-task-691ebba2d72c) [## 问正确的问题:在新任务中训练 T5 变压器模型

### T5 转换器将任何 NLP 任务构造为文本到文本的任务，使其能够轻松学习新任务。让我们来教…

towardsdatascience.com](/asking-the-right-questions-training-a-t5-transformer-model-on-a-new-task-691ebba2d72c) 

唯一的限制就是想象力！*(嗯，想象力和计算资源，但那是另一回事了)*😅

回到数据，运行笔记本应该会给您一个`train.tsv`和一个`eval.tsv`文件，我们将在下一节中使用它们来训练我们的模型！

# 设置

我们将使用[简单变形金刚](https://github.com/ThilinaRajapakse/simpletransformers)库(基于拥抱脸[变形金刚](https://github.com/huggingface/transformers))来训练 T5 模型。

下面给出的说明将安装所有的要求。

1.  从[这里](https://www.anaconda.com/distribution/)安装 Anaconda 或 Miniconda 包管理器。
2.  创建新的虚拟环境并安装软件包。
    `conda create -n simpletransformers python`
    `conda activate simpletransformers`
    
3.  安装简单变压器。
    

*参见安装* [*文档*](https://simpletransformers.ai/docs/installation/#installation-steps)

# 培训 T5 模型

和往常一样，用简单的变形金刚训练模型非常简单。

这里使用的大多数参数都相当标准。

*   `max_seq_length`:选择时不截断大多数样本。增加序列长度会极大地影响模型的内存消耗，因此通常最好尽可能地缩短序列长度(最好不要截断输入序列)。
*   `train_batch_size`:越大越好(只要适合你的 GPU)
*   `eval_batch_size`:同`train_batch_size`
*   `num_train_epochs`:超过 1 个历元的训练可能会提高模型的性能，但它显然也会增加训练时间(在 RTX 泰坦上每个历元大约 7 个小时)。
*   我们将根据测试数据定期测试模型，看看它是如何学习的。
*   `evaluate_during_training_steps`:前述测试模型的时期。
*   `evaluate_during_training_verbose`:测试完成后显示结果。
*   `use_multiprocessing`:使用多处理大大减少了标记化所花费的时间(在训练开始前完成)，但是，这目前会导致 T5 实现的问题。所以，暂时没有多重处理。😢
*   `fp16` : FP16 或混合精度训练减少了训练模型的内存消耗(意味着更大的批量是可能的)。不幸的是，`fp16`训练目前与 T5 不稳定，所以也被关闭了。
*   `save_steps`:设置为`-1`表示不保存检查点。
*   `save_eval_checkpoints`:默认情况下，在训练过程中进行评估时，会保存一个模型检查点。因为这个实验只是为了演示，所以我们也不要浪费空间来保存这些检查点。
*   我们只有一个纪元，所以没有。也不需要这个。
*   `reprocess_input_data`:控制是否从缓存加载特征(保存到磁盘)或是否对输入序列再次进行标记化。只有在多次运行时才真正重要。
*   `overwrite_output_dir`:这将覆盖任何先前保存的模型，如果它们在相同的输出目录中。
*   `wandb_project`:用于培训进度的可视化。

说到可视化，你可以在这里查看我的训练进度[。为他们令人敬畏的图书馆大声喊出来！](https://app.wandb.ai/thilina/T5%20mixed%20tasks%20-%20Binary,%20Multi-Label,%20Regression?workspace=user-thilina)

# 测试 T5 型号

考虑到我们正在处理多个任务的事实，使用合适的度量来评估每个任务是一个好主意。考虑到这一点，我们将使用以下指标:

*   二元分类: [F1 得分](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)和[准确度得分](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)
*   多标签分类:F1 分数(拥抱脸队指标实现)和精确匹配(拥抱脸队指标实现)
*   相似度:[皮尔逊相关系数](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.pearsonr.html)和[斯皮尔曼相关](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.spearmanr.html)

请注意，在准备数据时，在`prefix`和`input_text`之间插入了一个`": “`。这是在训练时自动完成的，但需要手动处理以进行预测。

*如果你想了解更多关于解码论点的内容(* `*num_beams*` *，* `*do_sample*` *，* `*max_length*` *，* `*top_k*` *，* `*top_p*` *)，请参考本文*[](/asking-the-right-questions-training-a-t5-transformer-model-on-a-new-task-691ebba2d72c)**。**

*是时候看看我们的模型做得如何了！*

```
*-----------------------------------
Results: 
Scores for binary classification:
F1 score: 0.96044512420231
Accuracy Score: 0.9605263157894737Scores for multilabel classification:
F1 score: 0.923048001002632
Exact matches: 0.923048001002632Scores for similarity:
Pearson Correlation: 0.8673017763553101
Spearman Correlation: 0.8644328787107548*
```

*尽管在 3 个独立的任务上进行训练，该模型在每个任务上表现得相当好！在下一节中，我们将快速了解一下如何进一步提高模型的性能。*

# *结束语*

## *可能的改进*

*混合任务时出现的一个潜在问题是用于每个任务的数据集大小之间的差异。通过查看训练样本计数，我们可以在我们的数据集中看到这个问题。*

```
*binary classification        560000
multilabel classification    143613
similarity                     5702*
```

*数据集在本质上是不平衡的，任务`similarity`的困境看起来尤其可怕！这可以在评估分数中清楚地看到，其中`similarity`任务落后于其他任务(尽管重要的是要注意，我们**而不是**在任务之间查看相同的指标)。*

*对这个问题的一个可能的补救方法是*对`similarity`任务进行过采样*，以便模型。*

*除此之外，增加训练历元的数量(以及调整其他超参数)也有可能改进模型。*

*最后，调整解码参数也可以得到更好的结果。*

## *包扎*

*T5 模型的文本到文本格式为将 Transformers 和 NLP 应用于各种各样的任务铺平了道路，几乎不需要定制。T5 型号性能强劲，即使在使用同一型号执行多项任务时也是如此！*

*希望在不久的将来，这将导致许多创新的应用。*

# *参考*

1.  *用统一的文本到文本转换器探索迁移学习的极限—[https://arxiv.org/abs/1910.10683](https://arxiv.org/abs/1910.10683)*
2.  *谷歌人工智能博客—[https://AI . Google Blog . com/2020/02/exploring-transfer-learning-with-t5 . html](https://ai.googleblog.com/2020/02/exploring-transfer-learning-with-t5.html)**