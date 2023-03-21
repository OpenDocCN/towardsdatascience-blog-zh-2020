# 什么是 Gumbel-Softmax？

> 原文：<https://towardsdatascience.com/what-is-gumbel-softmax-7f6d9cdcb90e?source=collection_archive---------3----------------------->

## 离散数据采样的可微近似法

![](img/1aa4d0ea4c820592753de4933ef7b4cb.png)

paweczerwi ski 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在深度学习中，我们经常想要对离散数据进行采样。举个例子，

*   基于生成对抗网络的文本生成
*   离散隐变量变分自动编码器
*   具有离散动作空间的深度强化学习

然而，来自分类分布的离散数据的采样过程是不可微的，这意味着反向传播将不起作用。Gumbel-Softmax 分布是一种连续分布，它近似于分类分布中的样本，也适用于反向传播。

## Gumbel-Softmax 分布

设 *Z* 是分类分布*分类* (𝜋₁，…，𝜋ₓ)的分类变量，其中𝜋ᵢ是我们的神经网络要学习的分类概率。假设我们的离散数据被编码为一个热点向量。最常见的采样方式 Z*由下式给出*

```
*Z* = onehot(max{*i* | 𝜋₁ + ... + 𝜋ᵢ₋₁ ≤ *U*})
```

其中 *i* = 1，…， *x* 为类索引， *U* ~ *Uniform* (0，1)。由于 max 函数的存在，这个采样公式是不可微的。为了获得可微分的近似值，我们应用以下公式:

1.  **甘贝尔-马克斯诡计**

Gumbel-Max 技巧提供了一个不同的采样公式 *Z*

```
*Z* = onehot(argmaxᵢ{*G*ᵢ + log(𝜋ᵢ)})
```

其中 *G* ᵢ ~ *冈贝尔* (0，1)是从标准冈贝尔分布中抽取的同分布样本。这是一个“重新参数化的把戏”，将 *Z* 的采样重构为参数的确定性函数和一些固定分布的独立噪声。

采样过程的重构并没有使它变得可区分。在我们的例子中，不可微性来自 argmax 函数。然而，一旦我们有了 argmax 的可微近似值，我们就可以很容易地进行反向传播，因为它只是关于计算确定性函数的梯度 w.r.t .参数。另一方面，如果没有重新参数化的技巧，我们将不得不计算分布的梯度 w.r.t .参数，这是一个更困难的问题。

2.**使用 softmax 作为可微分近似值**

我们使用 softmax 作为 argmax 的可微分近似值。样本向量 *y* 现在由下式给出

```
*y*ᵢ = exp((*G*ᵢ + log(𝜋ᵢ)) / 𝜏) / 𝚺ⱼ exp((*G*ⱼ + log(𝜋ⱼ)) / 𝜏)
```

对于每一个 *i* = 1，…， *x* 。*当𝜏 → 0 时，softmax 计算平滑地接近 argmax，并且样本向量接近 one-hot；随着𝜏 → ∞，样本向量变得一致。*

具有上述抽样公式的分布称为 Gumbel-Softmax 分布。注意，在训练期间使用连续向量，但是在评估期间样本向量被离散化为单热点向量。

## 使用

每当我们有一个离散变量的随机神经网络，我们可以使用 Gumbel-Softmax 分布来近似离散数据的采样过程。然后可以使用反向传播来训练网络，其中网络的性能将取决于温度参数𝜏.的选择

## 直通 Gumbel-Softmax

有些情况下，我们会希望在训练期间对离散数据进行采样:

*   我们受限于离散值，因为实值连续逼近是不允许的。比如在动作空间离散的深度强化学习中。
*   在训练期间使用连续近似和在评估期间使用一次性向量具有非常不同的动态。这导致评估期间模型性能的大幅下降。

我们可以在正向传递中使用 argmax 将 Gumbel-Softmax 样本离散化，使其与原始分类分布中的样本相同。这确保了培训和评估动态是相同的。然而，我们仍然在反向传递中使用 Gumbel-Softmax 样本来近似梯度，因此反向传播仍然有效。这种技术在文献中被称为直通 Gumbel-Softmax。

## 履行

*   tensor flow:[TFP . distributions . relaxedonehotcategorial](https://www.tensorflow.org/probability/api_docs/python/tfp/distributions/RelaxedOneHotCategorical)
*   py torch:[torch . nn . functional . gum bel _ soft max](https://pytorch.org/docs/stable/nn.functional.html#gumbel-softmax)

## 常见问题

问:我们如何选择温度参数𝜏？

答:我们可以给𝜏.一个固定的值最佳值是通过反复试验确定的，通常小于等于 1。我们也可以使用退火时间表，或者让它成为一个学习参数。

问:为什么我们不需要使用 Gumbel-Softmax 进行分类任务，这涉及到离散的类别标签？

答:这是因为在解决分类问题时通常不涉及抽样过程。相反，我们只需要我们的模型输出属于不同类别的特征向量的概率。简单地应用 softmax 函数就足够了。

## 进一步阅读

1.  Gumbel-Softmax 分布是由[2]和[3]独立发现的，在[3]中称为具体分布。两篇论文都是很好的参考，特别是关于分布的理论方面，以及关于重新参数化的技巧。
2.  [5]是[2]第一作者写的博文。这是一个很好的教程，有一个关于 Gumbel-Softmax 发行版采样的交互式小部件。
3.  对于 Gumbel-Softmax 分布在 GAN 中的应用，我们参考[4],其中给出了 Gumbel-Softmax 如何解决用离散数据训练 GAN 的问题的大图。
4.  [1]提供了一个需要直通 Gumbel-Softmax 而不是普通 Gumbel-Softmax 的用例。

## 参考

1.  南哈夫里洛夫和我蒂托夫。[用多智能体游戏出现语言:学习用符号序列交流](https://papers.nips.cc/paper/6810-emergence-of-language-with-multi-agent-games-learning-to-communicate-with-sequences-of-symbols.pdf) (2017)，NIPS 2017。
2.  E.张，s .顾和 b .普尔。[使用 Gumbel-Softmax](https://openreview.net/pdf?id=rkE3y85ee) (2017)，ICLR 2017 进行分类重新参数化。
3.  C.马迪森，Mnih 和叶维庆。[《具体分布:离散随机变量的一个连续松弛》](http://www.stats.ox.ac.uk/~cmaddis/pubs/concrete.pdf) (2017)，ICLR 2017。
4.  W.聂、纳洛德茨卡和帕特尔。 [RelGAN:用于文本生成的关系生成对抗网络](https://openreview.net/pdf?id=rJedV3R5tm) (2019)。ICLR 2019。
5.  教程:使用 Gumbel-Softmax 的分类变分自动编码器，[https://blog . ev jang . com/2016/11/tutorial-categorial-variable . html](https://blog.evjang.com/2016/11/tutorial-categorical-variational.html)