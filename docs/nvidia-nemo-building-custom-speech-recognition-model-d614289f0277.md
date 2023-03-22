# NVIDIA NeMo —构建自定义语音识别模型

> 原文：<https://towardsdatascience.com/nvidia-nemo-building-custom-speech-recognition-model-d614289f0277?source=collection_archive---------42----------------------->

![](img/503bbfe0985936e8e5652c18628a6530.png)

[https://www . pexels . com/photo/aluminum-audio-battery-broadcast-270288/](https://www.pexels.com/photo/aluminum-audio-battery-broadcast-270288/)

## 介绍

NVIDIA NeMo 是一个对话式人工智能工具包。该工具包是一个加速器，它帮助研究人员和从业人员对复杂的神经网络架构进行实验。语音处理(识别和合成)和自然语言处理是该平台的重要功能。因为它来自 NVIDIA，所以完全支持 GPU。该框架依赖 PyTorch 作为深度学习框架。

在本笔记本中，我们将尝试如何创建自动语音识别(ASR)。在本教程中，我们将使用 LibriSpeech 数据集。

## 设置

对于这个实验，以下软件:Ubuntu 16.04 Anaconda 4 . 7 . 11 NeMo—【https://github.com/NVIDIA/NeMo】T2 卡拉迪—[https://github.com/kaldi-asr/kaldi](https://github.com/kaldi-asr/kaldi)按照软件自述文件中的说明运行代码。确保您安装的 PyTorch 支持 GPU。硬件规格至少需要 6g 的 GPU RAM。

## 数据

LibriSpeech 是一个开放域语音识别数据集。我们可以从这里下载数据[http://www.openslr.org/12.](http://www.openslr.org/12.)对于本教程，我们使用的是 dev-clean 数据集—[http://www.openslr.org/resources/12/dev-clean.tar.gz](http://www.openslr.org/resources/12/dev-clean.tar.gz)。为了在一个非常小的 GPU 占用空间中轻松进行培训，我们从文件夹“dev-clean/84/121123/84”和“dev-clean/84/121550/”中选择数据。

语音文件存储在。flac 格式，应该转换成。' wav '格式的 NeMo 工作。NeMo 培训需要一个“清单”文件。“清单”文件包含“”的路径。wav '(演讲录音)，演讲持续时间，以及每个录音的抄本。

为了让生活变得简单，我们创建了一个实用程序来转换。flac' to '。“wav”和元数据文件。

```
from wavconvert import create_nemo_manifest
```

## 创建培训清单文件

```
flac_path = "/home/jaganadhg/AI_RND/nvidianemo/LibriSpeech/dev-clean/84/121550/"
meta_apth = "meta_train.json"

create_nemo_manifest(flac_path,
    meta_apth)flac_path = "/home/jaganadhg/AI_RND/nvidianemo/LibriSpeech/dev-clean/84/121123/"
meta_apth = "meta_val.json"

create_nemo_manifest(flac_path,
    meta_apth)
```

## 模型训练

让我们跳到建立一个模型。我们稍后将讨论 FFT、频谱和语言模型。创建一个实用程序脚本来抽象该过程。QuartzNet15x5 型号用作基础型号。语音识别结果用单词错误率(WER)来评估。实用程序脚本实现了一个 WER 计算器。

## 注意-相应地调整历元值以得到一个合适的模型。

```
from asrtrainer import (train_model,
        computer_wer)
from ruamel.yaml import YAMLconfig_path = 'quartznet_15x5.yaml'
train_manfest = "metadata.json"
val_manifest = "metadata_validation.json"

yaml = YAML(typ='safe')
with open(config_path) as f:
    model_params = yaml.load(f)

my_asr_model = train_model(model_params,
                            train_manfest,
                            val_manifest,
                            5,
                            False)

wer = computer_wer(model_params,
                    my_asr_model)
```

## 保存的模型可以保存到。nemo 的格式。

```
my_asr_model.save_to("tutorial.nemo")
```

## 后续步骤

在本教程中，我们创建了一个非常简单的模型，它的性能可能并不好。我们可以尝试构建一个更大的数据集，也许是整个 LibriSpeech dev-clean。时代的增加(我尝试了 1000 个时代，转录看起来不错！).

如果您有兴趣进一步了解，可以在“quartznet_13x5.yaml”文件中找到模型配置。

该代码可在 https://github.com/jaganadhg/nemoexamples 获得。