# 用定制语料库的预训练来微调 Albert 来自真实应用程序训练的一些笔记和打嗝

> 原文：<https://towardsdatascience.com/fine-tune-albert-with-pre-training-on-custom-corpus-some-notes-and-hiccups-from-real-application-24e463d6ef01?source=collection_archive---------30----------------------->

![](img/89a59b7af892ed3c5cf5750cc0b1bf1a.png)

图片来自互联网海底捞汽船

这是我上一篇[文章](/fine-tune-albert-with-pre-training-on-custom-corpus-f56ea3cfdc82)的后续文章，这篇文章展示了使用玩具数据集对 Albert 进行预训练的详细步骤。在这篇文章中，我将分享我自己在实际应用中应用这个模型的经验。我的应用程序是一个象征性的分类，Albert 在包含 2 亿多句子的外语(非英语)的 chats 语料库上进行了预训练。

# 预培训

**创建训练前文件**

您需要遵循 Albert 的“创建预训练文件”脚本所要求的输入格式，即不同文档之间一行，文档中不同句子之间一行。

如果您直接在 200+M 的句子上运行创建预训练文件的脚本，将会花费很长时间。因此，我把文集分成 200 多个文件，每个文件有 1 百万行。对于每个 1M 行的文件，在 CPU 上运行大约需要 2 个小时，最终创建的文件大约需要 4.5G 空间。我在分布式机器上使用并行运行的批处理作业来创建所有文件(我公司现有的基础设施)

**预训练设置**

我使用谷歌的免费 300 信用点进行预训练。继[https://cloud.google.com/tpu/docs/quickstart](https://cloud.google.com/tpu/docs/quickstart)之后，我设置了一辆 v2–8 TPU。为了避免 OOM，我将批量减少到 512(默认为 4096)。我已经将预训练的所有数据放入 vocab 文件，并将生成的预训练模型检查点也存储在 GCS 中。我已经将训练输出控制台保存到日志文件中，以便稍后我可以[从日志文件中提取训练损失。](https://github.com/LydiaXiaohongLi/Albert_Finetune_with_Pretrain_on_Custom_Corpus/blob/master/misc/albert_pretrain_log_proc.ipynb)

**预训练参数**

由于聊天句子往往很短，因此我将最大长度设置为 64(默认为 512)。因为我已经减少了批量大小，所以我相应地线性增加了训练步骤(到 1Mil，缺省值是 125000)和预热步骤(到 25000，缺省值是 3125)。我已经线性地降低了学习率(到 0.00022，缺省值是 0.00176，虽然学习率和批量大小不是线性关系，但是 0.00176 这个很好的缺省值正好可以除以 8，所以我就这样设置了，:P，但是反正对我来说是有效的)。我将 save_checkpoints_steps 设置为较小的最小值 1000，希望减少内存使用。

**初始化**

我没有对 Albert 模型进行随机初始化，而是使用 tf-hub 的预训练 Albert v2 base(英语，并且在撰写本文时，只有英语预训练模型可正式用于 Albert)权重进行初始化。一些打嗝是:

1.  来自 tf-hub 的模型没有 adam_m 和 adam_v 变量，我们必须手动添加它们
2.  tf-hub 模型中的变量以前缀“module/”存储，我们必须删除它才能使用 Albert repo 的训练前脚本
3.  我们还需要手动添加 glonal_step 变量(要求是 int64 类型)

参考这个[笔记本](https://github.com/LydiaXiaohongLi/Albert_Finetune_with_Pretrain_on_Custom_Corpus/blob/master/misc/albert_ckpt_convert.ipynb)。

**预训练结果**

训练损失从大约 4+开始，在 300k 训练步数后稳定到大约 1.8，此后在该范围内波动，直到 700k 步数，因此我在 700k 步数时提前停止(为了节省一些积分:P)。我尝试了另一个更低的学习率(0.00005)，损失减少到 2.7 左右，没有进一步发展。这确实因情况而异，我在网上看到一些训练损失稳定在 0.3 范围，而其他稳定在> 3。

完成 70 万步需要大约 2-3 天的时间，几乎用光了 300 美元的积分。

# 微调

我在之前的[帖子](/fine-tune-albert-with-pre-training-on-custom-corpus-f56ea3cfdc82)中已经提供了两个微调笔记本，一个用于 tensorflow，一个用于 pytorch(使用 huggingface 团队的变形金刚回购)，实际应用中使用的是 pytorch 版本。

**微调设置**

我用了 8 个 CPU，30 GB 内存+ 2 个英伟达特斯拉 T4 GPU。

**微调参数**

在我的标记分类微调任务(二元分类，要么标记是期望短语的一部分，要么不是)中，我用 500k+的训练样本进行了训练，其中有 50%的负样本(即对于负样本，整句中没有期望的标记，类比于餐馆评论玩具样本是，评论句子中没有菜名)。用 5 个 epoches 训练，学习率为 0.000001，批量大小为 32，具有完全微调的选项，即更新艾伯特模型以及最终微调 FCL，使用来自所有样本的所有表征的平均交叉熵损失。

**微调结果**

我能够达到大约 94%的验证准确率(所有样本中准确分类的令牌总数)，同样，这个数字没有意义，并且确实因情况而异。但总的来说，它对我的应用程序是有用的。