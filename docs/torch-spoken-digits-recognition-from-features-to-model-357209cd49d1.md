# Torch:从特征到模型的语音数字识别

> 原文：<https://towardsdatascience.com/torch-spoken-digits-recognition-from-features-to-model-357209cd49d1?source=collection_archive---------20----------------------->

![](img/bbceeae61b106b62f5bb1898217817d9.png)

詹姆斯·奥尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 探索从语音数据中提取的特征，以及基于这些特征构建模型的不同方法。

语音数字数据集是 Tensorflow 语音命令数据集的子集，包括除数字 0-9 之外的其他录音。这里，我们只关注识别说出的数字。

数据集可以按如下方式下载。

辐条数字特征提取. ipynb

辐条数字特征提取. ipynb

## 评估指标

数字录音的子集相当平衡，每类大约有 2300 个样本。因此，准确性是评估模型性能的一个很好的方法。准确性是正确预测数与预测总数的比较。对于不平衡的数据集，这不是一个很好的性能测量方法，因为多数类的个体精度可能会盖过少数类。

## 循环学习率

在训练模型时，学习率逐渐降低以微调训练。为了提高学习率效率，可以应用循环学习率过程。这里，学习率在各时期的最小值和最大值之间波动，而不是单调下降。

初始训练率对模型的性能至关重要，低训练率可防止在训练开始时停滞不前，随后的波动会抑制局部最小值和平台值。

可以使用优化器实例来监控每个时期中使用的学习率。

该项目有三种方法对记录进行分类:

1.  使用五个提取特征的逻辑回归— 76.19%的准确率。
2.  CNN 使用 Mel 光谱图—准确率 95.81%。

通过改变时期的数量和训练速率来重复训练模型。隐藏层的数量和每个层中的节点也是不同的。这里描述了每种方法的最佳架构和超参数。由于训练验证分割中的随机性，在重新训练时精度可能略有不同。

该项目的源代码是[这里](https://github.com/AyishaR/Spokendigit)。

有五个。ipynb 文件:

1.  特征提取-提取三种方法使用的必要 CSV 文件和特征。
2.  要素可视化-为每个类中的两个示例绘制要素。
3.  五个特征——使用五个提取的特征实现逻辑回归。
4.  使用 Mel 谱图实现 CNN。

# 1.使用五个提取特征的逻辑回归

## 特征

提取的特征包括:

*   **梅尔频率倒谱系数(MFCC)**—构成声音频谱表示的系数，基于根据人类听觉系统响应(梅尔标度)间隔的频带。
*   **色度** —与 12 个不同的音高等级相关。
*   **梅尔谱图的平均值** —基于梅尔标度的谱图。
*   **光谱对比度** —表示光谱的质心。
*   **Tonnetz** —代表色调空间。

这些特征是大小为(20)、(12)、(128)、(7)和(6)的 NumPy 数组。这些被连接以形成大小为(173)的特征数组。标签被附加到数组的头部，并写入每个记录的 CSV 文件。

spoke ndigit-feature-extraction . ipynb

## 模型

线性回归模型总共有 1 个输入层、2 个隐藏层和 1 个 ReLu 激活的输出层。

Spokendigit —五个功能。ipynb

## 火车

Spokendigit —五个功能。ipynb

Spokendigit —五个功能。ipynb

Spokendigit —五个功能。ipynb

该模型在 CPU 上训练约 3 分钟，准确率为 76.19%。

验证损失图

最终验证损失从最小值开始增加很大程度。

验证准确度图

每个时期的最后学习率图

# 2.CNN 使用梅尔光谱图图像。

## 特征

该模型使用记录的 Mel 谱图图像。梅尔频谱图是频率被转换成梅尔标度的频谱图。从记录中提取特征并存储在驱动器中。这花了 4.5 个多小时。

spoke ndigit-feature-extraction . ipynb

## 模型

CNN.ipynb

## 火车

CNN.ipynb

CNN.ipynb

CNN.ipynb

该模型在 Colab GPU 上训练约 5 小时，准确率为 95.81%。

高精度再次归因于 Mel 标度。

验证损失图

验证准确度图

每个时期的最后学习率图

# 参考

*   [https://musicinformationretrieval.com/](https://musicinformationretrieval.com/)
*   [https://github . com/jurgenarias/Portfolio/tree/master/Voice % 20 分类/代码](https://github.com/jurgenarias/Portfolio/tree/master/Voice%20Classification/Code)
*   [https://arxiv.org/abs/1506.01186](https://arxiv.org/abs/1506.01186)
*   [https://en.wikipedia.org/wiki/Mel-frequency_cepstrum](https://en.wikipedia.org/wiki/Mel-frequency_cepstrum)