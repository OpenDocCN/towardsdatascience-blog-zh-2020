# NVIDIA NeMo 入门指南

> 原文：<https://towardsdatascience.com/beginners-guide-to-nvidia-nemo-e5512862cd8e?source=collection_archive---------34----------------------->

## 开发和训练语音和语言模型的工具包

![](img/b648dc68463cdc5ecdbaef19803f089f.png)

戴维·克洛德在 [Unsplash](https://unsplash.com/s/photos/nemo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

这篇文章让你一瞥 NVIDIA NeMo 背后的基本概念。当涉及到为对话式人工智能建立你自己的艺术模型时，它是一个非常强大的工具包。供您参考，一个典型的对话式 AI 管道由以下领域组成:

1.  自动语音识别(ASR)
2.  自然语言处理
3.  文本到语音(TTS)

如果您正在寻找一个成熟的工具包来训练或微调这些领域的模型，您可能想看看 NeMo。它允许研究人员和模型开发人员使用称为**神经模块(NeMo)** 的可重用组件来构建他们自己的神经网络架构。基于[官方文档](https://github.com/NVIDIA/NeMo)，神经模块为

> “……采用*类型的*输入并产生*类型的*输出的神经网络概念模块。这种模块通常代表数据层、编码器、解码器、语言模型、损失函数或组合激活的方法。”

NeMo 的一个主要优点是，它可以用于训练新模型或在现有的预训练模型上执行迁移学习。最重要的是，在 [NVIDIA GPU Cloud (NGC)](https://nvda.ws/3hDVQKv) 有相当多的预训练模型可供您使用。在撰写本文时，GPU 加速的云平台具有以下预训练模型:

## 自动语音识别(Automatic Speech Recognition)

*   碧玉 10x5 — Librispeech
*   [多数据集 Jasper 10x5](https://nvda.ws/3jVCClA) — LibriSpeech、Mozilla Common Voice、WSJ、Fisher 和 Switchboard
*   [AI-shell 2 Jasper 10x 5](https://nvda.ws/2CW5Dg5)—AI-shell 2 普通话
*   [夸兹涅特](https://nvda.ws/39zNssC)——速度扰动下的自由探索
*   QuartzNetLibrispeechMCV—Librispeech，Mozilla common voice
*   [多数据集 Quartznet](https://nvda.ws/2EqtfKl) — LibriSpeech、Mozilla Common Voice、WSJ、Fisher 和 Switchboard
*   [WSJ-Quartznet](https://nvda.ws/2P3BIoR)——华尔街日报、Librispeech、Mozilla common voice
*   [AI-shell 2 Quartznet](https://nvda.ws/39zV5zh)—AI-shell 2 普通话

## 自然语言处理

*   [bertalgeuncased](https://nvda.ws/3hGJtNB)—使用 BERT Large 对序列长度为 512 的未分类维基百科和图书语料库进行分类
*   [BertBaseCased](https://nvda.ws/2Df8DnS) —使用 BERT Base 对序列长度为 512 的维基百科和图书语料库进行装箱
*   [BertBaseUncased](https://nvda.ws/32VyCLM) —使用 BERT Base 对序列长度为 512 的维基百科和图书语料库进行脱壳
*   [变压器-大](https://nvda.ws/32Z6EP7) — WikiText-2

## 自文本至语音的转换

*   [Tacotron2](https://nvda.ws/2P23rGk) — LJSpeech
*   [波辉](https://nvda.ws/331eRlO) — LJSpeech

# 设置

确保您满足以下要求:

*   Python 3.6 或 3.7
*   PyTorch 1.4。*支持 GPU
*   NVIDIA APEX(可选)

供您参考，NVIDIA APEX 是一个实用程序，有助于简化 Pytorch 中的混合精度和分布式培训。这不是必需的，但它有助于提高性能和训练时间。如果您打算使用 NVIDIA APEX，强烈建议使用 Linux 操作系统，因为对 Windows 的支持仍处于实验阶段。

如果您使用 docker，安装非常简单。

```
docker run --runtime=nvidia -it --rm -v --shm-size=16g -p 8888:8888 -p 6006:6006 --ulimit memlock=-1 --ulimit stack=67108864 nvcr.io/nvidia/nemo:v0.11
```

如果你已经注册了 NVIDIA [NGC PyTorch 容器](https://nvda.ws/3f50Lm1)，一个接一个地执行下面的命令。

拉码头工人

```
docker pull nvcr.io/nvidia/pytorch:20.01-py3
```

运行以下命令

```
docker run --gpus all -it --rm -v <nemo_github_folder>:/NeMo  --shm-size=8g -p 8888:8888 -p 6006:6006 --ulimit memlock=-1 --ulimit  stack=67108864 nvcr.io/nvidia/pytorch:20.01-py3
```

以下步骤是安装其余部分的继续。如果您在本地运行它或者通过 Google Colab 运行它，您应该从这里开始安装。运行以下命令安装必要的依赖项。

```
apt-get update && apt-get install -y libsndfile1 ffmpeg && pip install Cython
```

一旦你完成了，下一步是`pip install` NeMo 模块取决于你的用例。如果您使用它来训练新模型，请运行以下命令

```
pip install nemo_toolkit
```

获得自动语音识别集合附带的 NeMo

```
pip install nemo_toolkit[asr]
```

NeMo 和自然语言处理集合可以通过

```
pip install nemo_toolkit[nlp]
```

要安装 NeMo 和文本到语音集合，请运行以下命令

```
pip install nemo_toolkit[tts]
```

如果您正在寻找包含所有集合的完整安装，您可以通过

```
pip install nemo_toolkit[all]
```

# 程序设计模型

基于 NeMo API 的每个应用程序通常使用以下工作流程:

1.  神经调节因子和必要神经调节因子的产生
2.  定义神经模块的有向无环图(DAG)
3.  号召“行动”,如训练

需要注意的一点是，NeMo 遵循**懒惰执行**模型。这意味着在调用推理之前或训练之后，不会执行任何实际的计算。

# 神经类型

NeMo 中每个神经模块的所有输入和输出端口都是类型化的。它们是用 Python 类 NeuralType 和从 ElementType、AxisType 和 AxisKindAbstract 派生的辅助类实现的。神经类型由以下数据组成:

*   `axes` —表示改变特定轴的含义(批次、时间)
*   `elements_type` —表示激活中存储内容的语义和属性(音频信号、文本嵌入、逻辑)

## 初始化

实例化主要是在模块的`input_ports`和`output_ports`属性中完成的。你可以如下实例化一个`Neural Type`

```
axes: Optional[Tuple] = None, elements_type: ElementType = VoidType(), optional=False
```

让我们看看下面的(音频)数据层输出端口示例。

```
{
    'audio_signal': NeuralType(('B', 'T'), AudioSignal(freq=self._sample_rate)),
    'a_sig_length': NeuralType(tuple('B'), LengthsType()),
    'transcripts': NeuralType(('B', 'T'), LabelsType()),
    'transcript_length': NeuralType(tuple('B'), LengthsType()),
}
```

*   `B` —代表 AxisKind。一批
*   `T` —代表 AxisKind。时间
*   `D` —代表 AxisKind。尺寸

## 比较

您可以通过`compare()`功能比较两个`NeuralType`。它将返回一个`NeuralTypeComparisonResult`,传达如下意思

*   **相同** = 0
*   **减去** = 1 (A 为 B)
*   **大于** = 2 (B 是 A)
*   **尺寸不兼容** = 3(尺寸不兼容。调整连接器大小可能会修复不兼容性)
*   **TRANSPOSE_SAME** = 4(数据格式不兼容，但列表和张量之间的转置和/或转换会使它们相同)
*   **CONTAINER _ SIZE _ MISMATCH**= 5(A 和 B 包含不同数量的元素)
*   **不相容** = 6 (A 和 B 不相容)
*   **SAME _ TYPE _ INCOMPATIBLE _ PARAMS**= 7(A 和 B 属于同一类型，但参数化不同)

让我们转到下一节，深入探讨示例。

# 例子

## 基本示例

让我们看看下面的例子，它建立了一个模型，学习 y=sin(x)的泰勒系数。

```
import nemo

# instantiate Neural Factory with supported backend
nf = nemo.core.NeuralModuleFactory()

# instantiate necessary neural modules
# RealFunctionDataLayer defaults to f=torch.sin, sampling from x=[-4, 4]
dl = nemo.tutorials.RealFunctionDataLayer(
    n=10000, batch_size=128)
fx = nemo.tutorials.TaylorNet(dim=4)
loss = nemo.tutorials.MSELoss()

# describe activation's flow
x, y = dl()
p = fx(x=x)
lss = loss(predictions=p, target=y)

# SimpleLossLoggerCallback will print loss values to console.
callback = nemo.core.SimpleLossLoggerCallback(
    tensors=[lss],
    print_func=lambda x: logging.info(f'Train Loss: {str(x[0].item())}'))

# Invoke "train" action
nf.train([lss], callbacks=[callback],
         optimization_params={"num_epochs": 3, "lr": 0.0003},
         optimizer="sgd")
```

## 自动语音识别(Automatic Speech Recognition)

查看以下笔记本，开始您的语音识别项目:

*   [端到端自动语音识别简介](https://github.com/NVIDIA/NeMo/blob/master/examples/asr/notebooks/1_ASR_tutorial_using_NeMo.ipynb)
*   [NeMo 中麦克风流的自动语音识别](https://github.com/NVIDIA/NeMo/blob/master/examples/asr/notebooks/2_Online_ASR_Microphone_Demo.ipynb)
*   [基于 QuartzNet 模型的语音命令识别](https://github.com/NVIDIA/NeMo/blob/master/examples/asr/notebooks/3_Speech_Commands_using_NeMo.ipynb)

## 自然语言处理

使用 NeMO 进行自然语言处理任务的笔记本集合:

*   [伯特预训](https://github.com/NVIDIA/NeMo/blob/master/examples/nlp/language_modeling/BERTPretrainingTutorial.ipynb)
*   [用于问答的生物机器人](https://github.com/NVIDIA/NeMo/blob/master/examples/nlp/biobert_notebooks/biobert_qa.ipynb)
*   [用于命名实体识别的 BioBERT】](https://github.com/NVIDIA/NeMo/blob/master/examples/nlp/biobert_notebooks/biobert_ner.ipynb)
*   [用于关系提取的 BioBERT](https://github.com/NVIDIA/NeMo/blob/master/examples/nlp/biobert_notebooks/biobert_re.ipynb)

## 自文本至语音的转换

使用 NeMo 执行文本到语音转换任务的笔记本示例:

*   [Tacotron + WaveGlow 生成音频](https://github.com/NVIDIA/NeMo/blob/master/examples/tts/notebooks/1_Tacotron_inference.ipynb)

# 结论

让我们回顾一下今天所学的内容。

我们首先简要介绍了 NVIDIA NeMo toolkit。此外，我们接触了一些预先训练好的模型，这些模型在 [NVIDIA GPU Cloud (NGC)](https://nvda.ws/3hDVQKv) 很容易获得。

然后，我们通过 docker 或使用 pip install 的本地安装来安装工具包。

我们深入探讨了构成 NeMo 背后的基本概念的编程模型和神经类型。

最后，我们测试了几个自动语音识别、自然语言处理和文本到语音转换任务的例子。

感谢你阅读这篇文章。希望在下一篇文章中再见到你！

# 参考

1.  [英伟达 NeMo Github](https://github.com/NVIDIA/NeMo)
2.  [英伟达 NeMo 文档](https://docs.nvidia.com/deeplearning/nemo/neural_mod_bp_guide/)
3.  [英伟达尼莫的例子](https://github.com/NVIDIA/NeMo/tree/master/examples)