# 开始使用端到端语音翻译

> 原文：<https://towardsdatascience.com/getting-started-with-end-to-end-speech-translation-3634c35a6561?source=collection_archive---------19----------------------->

使用 Pytorch，您只需几个步骤就可以翻译英语演讲

![](img/467413cac7e017271019377bf69544d7.png)

更新 24–05–2021:本教程中使用的 github 库不再开发。如果有兴趣，你应该参考正在积极开发的这个叉子。

## 介绍

语音到文本的翻译是将源语言的语音翻译成不同目标语言的文本。这个任务的历史可以追溯到 1983 年[的一个演示中。处理这项任务的经典方法是训练一系列系统，包括自动语音识别(ASR)和机器翻译(MT)。你可以在你的谷歌翻译应用中看到，你的演讲首先被转录，然后被翻译(尽管翻译看起来是实时的)](https://en.wikipedia.org/wiki/Speech_translation#History)

![](img/d10b8cf87c33bee451f94932f79ffca6.png)

人工智能和机器翻译的任务已经被研究了很长时间，并且随着深度学习技术的采用，系统的质量已经经历了显著的飞跃。事实上，大数据的可用性(至少对某些语言而言)、强大的计算能力和清晰的评估，使这两项任务成为像谷歌这样在研究上投入大量资金的大公司的完美目标。参见关于[变压器](https://ai.googleblog.com/2017/08/transformer-novel-neural-network.html)【1】和[spec augment](https://ai.googleblog.com/2019/04/specaugment-new-data-augmentation.html)2】的论文作为参考。由于这篇博客文章不是关于级联系统的，我向感兴趣的读者推荐在上次 [IWSLT](https://workshop2019.iwslt.org/) 竞赛中获胜的系统[3]。

IWSLT 是致力于口语翻译的主要年度研讨会。每个版本都有一个“共享任务”，一种竞赛，目的是记录口语技术的进步。自 2018 年以来，共享任务开始对“端到端”系统进行单独评估，这些系统由单一模型组成，学习直接从音频翻译成目标语言的文本，没有中间步骤。我们小组从第一版开始就一直参与这个新的评估，我在之前的报道中报道了我们的第一次参与。

[](https://medium.com/machine-translation-fbk/machines-can-learn-to-translate-directly-your-voice-fbk-iwslt18-bb284ccae8bc) [## 机器可以学习翻译你的声音

### 我们如何为 IWSLT 2018 评估活动构建我们的端到端语音到文本翻译系统。

medium.com](https://medium.com/machine-translation-fbk/machines-can-learn-to-translate-directly-your-voice-fbk-iwslt18-bb284ccae8bc) 

与级联方法相比，端到端模型的质量仍在讨论中，但这是一个不断发展的研究课题，质量改进的报告非常频繁。本教程的目标是通过为读者提供一步一步的指南来训练端到端系统，从而降低这个领域的入门门槛。特别是，我们将关注一个可以将英语语音翻译成意大利语的系统，但它可以很容易地扩展到其他七种语言:荷兰语、法语、德语、西班牙语、葡萄牙语、罗马尼亚语或俄语。

## 你需要什么

最低要求是访问至少一个 GPU，可以通过安装 Colab 和 Pytorch 0.4 免费获得。

[](https://medium.com/deep-learning-turkey/google-colab-free-gpu-tutorial-e113627b9f5d) [## Google Colab 免费 GPU 教程

### 现在，您可以使用 Keras 在免费的 Tesla K80 GPU 上与 Google 合作开发深度学习应用程序

medium.com](https://medium.com/deep-learning-turkey/google-colab-free-gpu-tutorial-e113627b9f5d) 

然而，K80 GPUs 非常慢，需要几天的训练。接入更好或者更多的 GPU 会有很大的帮助。

## 获取数据

我们将使用 MuST-C，这是可用于直接语音翻译任务的最大的多语言语料库。您可以在介绍它的论文[4]或下面的媒体故事中找到详细的描述:

[](https://medium.com/machine-translation-fbk/must-c-a-large-corpus-for-speech-translation-8e2350d01ea3) [## MuST-C:一个用于语音翻译的大型语料库

### …你一定要看看！

medium.com](https://medium.com/machine-translation-fbk/must-c-a-large-corpus-for-speech-translation-8e2350d01ea3) 

要获取语料库，请前往[https://mustc.fbk.eu/,](https://mustc.fbk.eu/)点击“点击此处下载语料库”按钮，然后填写表格，您很快就可以下载了。

MuST-C 分为 8 个部分，每个部分对应一种目标语言，您可以随意下载其中的一种或全部，但是对于本教程，我们将使用意大利目标语言(it)作为示例。每一部分都包含用英语发表的 TED 演讲，并翻译成目标语言(翻译由 [Ted 网站](https://www.ted.com/)提供)。训练集的大小取决于给定语言的翻译的可用性，而验证集和测试集是从公共的谈话库中提取的。

MuST-C 的每个部分都分为列车、开发、tst-COMMON 和 tst-HE。Train、dev 和 tst-COMMON 表示我们分为训练、验证和测试集，而您可以安全地忽略 tst-HE。在这三个目录的每一个中，您都会发现三个子目录:wav/、txt/和 h5/。`wav/`包含了音频端的设置形式。wav 文件，每个对话一个。`txt`包含抄本和翻译，对于我们的意大利语示例，您将在`train/txt`目录下找到文件`train.it, train.en, train.yaml`。前两个分别是文本翻译和转录。`train.yaml`是一个包含音频分段的文件，其方式与文本文件一致。作为奖励。恩还有。it 文件是并行的，因此可以用来训练机器翻译系统。如果您不知道如何处理 yaml 文件提供的分段，请不要害怕！在`h5/`目录中有一个单独的. h5 文件，该文件包含已经被分割和转换以提取 40 个 [Mel 滤波器组](https://haythamfayek.com/2016/04/21/speech-processing-for-machine-learning.html)特征的音频。

**注意:**数据集将从 Google Drive 下载，如果你想从没有 GUI 的机器上下载，可以尝试使用工具 [gdown](https://pypi.org/project/gdown/) 。然而，它并不总是正确地工作。如果您无法使用 gdown 下载，请在几个小时后重试。

## 获取软件

我们将使用[FBK-公平序列-ST](https://github.com/mattiadg/FBK-Fairseq-ST) ，这是脸书为机器翻译开发的[公平序列](https://github.com/pytorch/fairseq)工具，适用于直接语音翻译任务。从 github 克隆存储库:

```
git clone [https://github.com/mattiadg/FBK-Fairseq-ST.git](https://github.com/mattiadg/FBK-Fairseq-ST.git)
```

然后，还要克隆 [mosesdecoder，](https://github.com/moses-smt/mosesdecoder/tree/master/scripts)它包含了对文本预处理有用的脚本。

```
git clone [https://github.com/moses-smt/mosesdecoder.git](https://github.com/moses-smt/mosesdecoder.git)
```

## 数据预处理

数据的音频部分已经在. h5 文件中进行了预处理，所以我们只需要关心文本部分。

让我们首先创建一个存放令牌化数据的目录。

```
> mkdir mustc-tokenized
> cd mustc-tokenized
```

然后，我们可以继续对我们的意大利文本进行标记化(其他目标语言需要类似的过程):

```
> for file in $MUSTC/en-it/data/{train,dev,tst-COMMON}/txt/*.it; do
   $mosesdecoder/scripts/tokenizer/tokenizer.perl -l it < $file |
   $mosesdecoder/scripts/tokenizer/deescape-special-chars.perl > $file
done> mkdir tokenized> for file in *.it; do 
    cp $file tokenized/$file.char
    sh word_level2char_level.sh tokenized/$file
done
```

第二个 for 循环将单词拆分成字符，就像我们在为所有 MuST-C 语言设置基线的论文中所做的那样[5]。

现在，我们必须将数据二进制化，以便为 fairseq 制作单一格式的音频和文本。首先，链接数据目录中的 h5 文件。

```
> cd tokenized
> for file in $MUSTC/en-it/data/{train,dev,tst-COMMON}/h5/*.h5; do
    ln -s $file
done
```

然后，我们可以移动到实际的二值化

```
> python $FBK-Fairseq-ST/preprocess.py --trainpref train --validpref dev --testpref tst-COMMON -s h5 -t it --inputtype audio --format h5 --destdir bin
```

这将需要几分钟的时间，最终您应该会得到如下结果:

```
> ls bin/
dict.it.txt         train.h5-it.it.bin  valid.h5-it.it.idx
test.h5-it.it.bin   train.h5-it.it.idx  valid.h5-it.h5.bin
test.h5-it.it.idx   train.h5-it.h5.bin  valid.h5-it.h5.idx
test.h5-it.h5.bin   train.h5-it.h5.idx
test.h5-it.h5.idx   valid.h5-it.it.bin
```

我们有一个目标语言的字典(dict.it.txt)，对于数据的每个分割，源端有一个索引和一个内容文件(*.h5.idx 和*.h5.bin)，目标端也有相同的索引和内容文件(*.it.idx 和*.it.bin)。

至此，我们已经完成了数据预处理，可以继续训练了！

## 训练您的模型

> 注意:在培训之前，我**强烈**建议您使用相同架构的 ASR 系统对编码器进行预培训。训练脚本和这里一样，但是先按照这篇文章末尾的说明来做。没有编码器预训练，这些模型是不稳定的，并且可能不收敛。

对于培训，我们将复制[5]中报告的内容。您只需要运行以下命令:

```
> mkdir models
> CUDA_VISIBLE_DEVICES=$GPUS python $FBK-Fairseq-ST/train.py bin/ \
    --clip-norm 20 \
    --max-sentences 8 \
    --max-tokens 12000 \
    --save-dir models/ \
    --max-epoch 50 \
    --lr 5e-3 \
    --dropout 0.1 \
    --lr-schedule inverse_sqrt \
    --warmup-updates 4000 --warmup-init-lr 3e-4 \
    --optimizer adam \
    --arch speechconvtransformer_big \    
    --distance-penalty log \
    --task translation \
    --audio-input \
    --max-source-positions 1400 --max-target-positions 300 \
    --update-freq 16 \
    --skip-invalid-size-inputs-valid-test \
    --sentence-avg \
    --criterion label_smoothed_cross_entropy \
    --label-smoothing 0.1
```

让我一步一步来解释。`bin/`是包含二进制数据的目录，如上所述，而`models/`是保存检查点的目录(每个时期结束时一个)。`--clip-norm`指的是[渐变裁剪，](/what-is-gradient-clipping-b8e815cdfb48)和`--[dropout](https://medium.com/@amarbudhiraja/https-medium-com-amarbudhiraja-learning-less-to-learn-better-dropout-in-deep-machine-learning-74334da4bfc5)`熟悉深度学习的应该很清楚。`--max-tokens`是每次迭代在**单个** GPU 中可以加载的音频帧的最大数量，`--max-sentences`是最大批量，也受 max-tokens 的限制。`--update-freq`还会影响批量大小，因为我们在这里说权重必须在 16 次迭代后更新。它基本上模拟了 16x GPUs 的训练。现在，优化策略:`--optimizer` adam 用于使用 [Adam 优化器，](/adam-latest-trends-in-deep-learning-optimization-6be9a291375c) `--lr-schedule inverse_sqrt`使用 Transformer 论文[1]中介绍的时间表:学习速率在`--warmup-updates`步(4000)中从`--warmup-init-lr` (0.0003)线性增长到`--lr` (0.005)，然后随着步数的平方根下降。要优化的损失是使用 0.1 的`--label-smoothing`与[标签平滑](/what-is-label-smoothing-108debd7ef06) ( `--criterion`)的交叉熵。损失在句子中平均，而不是在具有`--sentence-avg`的记号中平均。`--arch`定义了要使用的架构和超参数，这些可以在运行训练时更改，但是`speechconvtransformer_big`使用了与我们论文中相同的超参数，除了我们命令中指定的距离惩罚。

深度学习架构是转换器对语音翻译任务的适应，它修改编码器以处理输入中的频谱图。我将在以后的博客中描述它。

在训练期间，将在每个时期结束时保存一个检查点，并相应地称为 checkpoint1.pt、checkpoint2.pt 等。此外，在每个时期结束时还会更新另外两个检查点:checkpoint_best.pt 和 checkpoint_last.pt。前者是验证损失最大的检查点的副本，后者是最后保存的检查点的副本。

## 生成和评估

当您准备好从音频(实际上是预处理的光谱图)运行翻译时，您可以运行以下命令:

```
python $FBK-Fairseq-ST/generate.py tokenized/bin/ --path models/checkpoint_best.pt --audio-input \
 [--gen-subset valid] [--beam 5] [--batch 32] \
[--skip-invalid-size-inputs-valid-test] [--max-source-positions N] [--max-target-positions N] > test.raw.txt
```

这里绝对需要的是带有二进制数据的目录`bin/`，到检查点的路径`--path models/checkpoint_best.pt`，但它可以是任何保存的检查点，`--audio-input`，并通知软件它必须期待音频(而不是文本)输入。

根据设计，该命令将在给定的目录中查找数据集的“测试”部分。如果要翻译另一个，valid 或者 train，用`--gen-subset {valid,train}`就可以了。通过`--beam`和`--batch`可以分别修改光束大小和批量大小。`--skip-invalid-size-inputs-valid-test`让软件跳过比`--max-source-positions`和`--max-target-positions`设定的限制更长的段。

输出将是这样的:

![](img/6163bcb8f08a714c3e1844324e40e3fb.png)

带有翻译参数和数据信息的报头

![](img/569337005656c34231b1f8d57e92a189.png)

段号 371、2088 和 713 的翻译

它首先列出用于翻译的参数，然后是一些数据信息:首先是字典，然后是数据集，最后是模型的路径。

在序言之后，我们有按照源代码长度排序的翻译列表**。**这个长度在翻译中并不明显，但在翻译长度中有部分体现。每个线段由其原始位置标识，每个线段有四行。源片段 S，这里总是音频，参考句子 T，翻译假设 H 和每个单词 p 的对数概率

最后一行打印的是 [BLEU](https://en.wikipedia.org/wiki/BLEU) 分数，但这里是在角色级别计算的，对我们来说意义不大。

![](img/851e0b84b5c021016f5706005242aaf7.png)

为了提取翻译，按照职位 id 对它们进行排序，并将翻译带到单词级别，您可以使用以下脚本

```
> python $FBK-Fairseq-ST/scripts/sort-sentences.py test.raw.txt 5 >     test.lines.txt
> bash $FBK-Fairseq-ST/scripts/extract_words.sh test.lines.txt
```

这将创建单词级文件 test.lines.txt.word，您可以使用它来测量 BLEU 分数，例如使用 mosesdecoder 存储库中的脚本:

```
> $mosesdecoder/scripts/generic/multi-bleu.perl test.it < test.lines.txt.word
```

## 奖励:ASR 预培训

众所周知，在语音翻译文献中，用在 ASR 任务上训练的模型的权重来初始化我们的模型的编码器有利于最终的翻译质量和加速训练。这也很容易用我们的软件和数据复制。首先，使用相同的音频和文本的英语部分对数据进行预处理。音频片段和英文文本已经对齐，这意味着您可以对音频端使用完全相同的二进制数据，并处理文本以生成剩余的二进制数据。对于 ASR，您可能希望删除标点符号，只预测单词，以使任务更容易。标点符号可以用以下方式删除:

```
sed "s/[[:punct:]]//g" $file.tok > $file.depunct
```

按照上面列出的步骤在新目录`models-asr`中训练 ASR 系统。然后，一旦训练结束，跑步

```
$FBK-Fairseq-ST/strip_modules.py --model-path models-asr/checkpoint_best.pt --new-model-path models/pretrain_encoder.pt --strip-what decoder
```

该命令将创建一个新的检查点文件，其编码器与 models-asr/checkpoint_best.pt 相同，但没有解码器。最后，为了使用它来初始化新的解码器，只需像上面一样运行训练命令，但是用新的

```
cp models/pretrain_encoder.pt models/checkpoint_last.pt
```

默认情况下，训练脚本会检查检查点目录中是否存在名为 checkpoint_last.pt 的检查点，如果存在，它将用于初始化模型的权重。就这么简单！

## **结论**

最近在直接语音到文本翻译方面的研究提供了软件和数据资源，可以很容易地用于启动您在这一领域的工作。希望这个教程对你“弄脏”手，熟悉工具有用。下一步就是研究如何改进现有的方法！

[](/tips-for-reading-and-writing-an-ml-research-paper-a505863055cf) [## 阅读和撰写 ML 研究论文的技巧

### 从几十次同行评审中获得的经验教训

towardsdatascience.com](/tips-for-reading-and-writing-an-ml-research-paper-a505863055cf) 

## 参考

[1]瓦斯瓦尼等人，“你所需要的只是关注。”*神经信息处理系统的进展*。(2017)
【2】朴，DS。一种用于自动语音识别的简单数据扩充方法。*继续。inter seech 2019*(2019)
【3】Pham，NQ 等.[IWS lt 2019 KIT 语音翻译系统。](https://zenodo.org/record/3525564#.XmvD2HJ7k2w)“芝诺多。(2019)
【4】迪甘吉，马。MuST-C:一个多语言语音翻译语料库。*HLT-NAACL 2019*(2019)
【5】迪甘吉，马。《让变压器适应端到端口语翻译》*散客 2019* (2019)