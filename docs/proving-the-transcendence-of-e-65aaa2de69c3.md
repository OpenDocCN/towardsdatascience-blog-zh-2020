# 证明 e 的超越性

> 原文：<https://towardsdatascience.com/proving-the-transcendence-of-e-65aaa2de69c3?source=collection_archive---------23----------------------->

## 欧拉数超越性的初等证明

![](img/be7f98d704da843d245399eddab0bf8b.png)

图片由来自[皮克斯拜](https://pixabay.com/fr/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1943505)的[皮特·林福思](https://pixabay.com/fr/users/TheDigitalArtist-202249/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1943505)拍摄

[欧拉数*e*T7 是一个数学常数~2.718，定义如下:](https://en.wikipedia.org/wiki/E_(mathematical_constant))

![](img/cd3af336972336b55a0c609ff7986548.png)

等式 1:欧拉数 e 的一个可能定义。

这个常数是瑞士数学家雅各布·伯努利发现的。初等函数

![](img/3effad602508407c772f483a739596b4.png)

等式 2:指数函数，唯一等于其导数的函数。

被称为[指数函数](https://en.wikipedia.org/wiki/Exponential_function)，它等于它自己的导数(它是唯一具有这种性质的函数)。

![](img/7b69cbf1b47727bb9e1f9ecef891ce76.png)

图 1:方程 *y* = 1/ *x 的绘图欧拉数 e* 是唯一使阴影面积等于 1 的数(大于 1)([来源](https://en.wikipedia.org/wiki/Exponential_function))。

# **超越数**

实数可以是代数的，也可以是超越的。根据定义，n 次代数数 *n* 满足具有整系数的多项式方程，例如:

![](img/ba7149df7c36b46aa9db85bb892c9c0d.png)

方程 3:整系数多项式方程。它由代数数来满足。

一个代数数为 n 次的事实意味着 xⁿ的系数非零。[超越数](https://en.wikipedia.org/wiki/Transcendental_number)是**不**满足等式等方程的实数。3.

# e 的超越

我们在这篇文章中的目标是证明 *e* 是一个超越数。这个证明的最初版本是由法国数学家查尔斯·埃尔米特提供的，但这里给出的版本是由德国数学家戴维·希尔伯特简化的。

![](img/7fc13e72c9b3d7afc9789a3ab48c4a67.png)

图 2:法国数学家查尔斯·埃尔米特([来源](https://en.wikipedia.org/wiki/Charles_Hermite))和德国数学家戴维·希尔伯特([来源](https://en.wikipedia.org/wiki/David_Hilbert))。

我们首先假设与我们试图证明的相反的情况，即 *e* **是**次数为 *n:* 的代数数

![](img/a89729fe5388e1c1d256152967325f2f.png)

等式 4:如果 e 在 n 次代数数中，它满足这个等式，其中第一个和最后一个 a 系数非空。

我们开始证明用有理数逼近 e 的幂。我们定义了以下对象:

![](img/69885fd70198c6e532cf9c60098d30af.png)

等式 5:用有理数逼近 e 的幂。

在哪里

![](img/faad23ef4ac4ba77fb884aa8cd058579.png)

对于等式中 *e* 的每个功率。5 我们有:

![](img/2277b86c8593b28a29c26cd940e464f0.png)

对于非常小的ϵ来说，这个等式意味着所有的 eᵗ都非常接近于一个有理数。我们现在用等式代替。5 转换成等式 4 并抵消因子 *M* 。我们获得:

![](img/b438471a9746c7e8ac9f91cb03eea334.png)

等式 6:代入等式的结果。情商 5。4.

注意等式中的 *n* 。4 和 Eq。6 是相同的数量。情商。6 有两个明显的特点:

1.  第一个括号内的表达式是一个整数，并且选择了 M 个整数，使得表达式不为零
2.  在第二个表达式中， *ϵ's* 将被选择得足够小，使得表达式的绝对值为< 1

![](img/7382cd2d7ba46100199b1f1dc1ddaf77.png)

等式 7:等式中第二项所服从的性质。6.

我们的证明将包括证明那个等式。6 不可能是真的，它构成了一个矛盾。这是因为一个非零整数和一个带绝对值的表达式的和< 1 cannot vanish.

## Defining the M’s and *ϵ's*

埃尔米特从定义 m 和ϵ开始。首先，他将 m 定义为:

![](img/11312151564845c83d853941b078fd19.png)

等式 8:定义 m 的积分。

其中 *p* 被选为素数，这将在后面确定。质数 *p* 可以取我们想要的那么大(但是 *M* 对于 *p* 的任意值都是整数)。其他 m 和 *ϵ* 定义如下:

![](img/55588f452989821ac74be28e9e086739.png)

等式 9:定义等式中引入的 Ms 和ϵs 的积分 6.

我们现在通过选择 *p* 来继续，以便满足上面的属性 1 和 2。

让我们首先计算积分 *M.* 乘以分子中的二项式并将结果提升到 p，我们得到

![](img/c266d367c7304a01bec2b90f50de6603.png)

等式 10:将等式分子中的二项式相乘。9

其具有整系数。将其代入 *M* 并使用

![](img/3900cf8678fdc2141c12b665867ad6cd.png)

等式 11:阶乘 m 的积分表达式。

我们得出:

![](img/25aacd5cb365765bf7e9fdc56042767b.png)

等式 12:等式。8 限制 p 大于 n。

将我们自己限制为大于 *n* 的素数，我们立即看到这个等式的第一项是**而不是**可被*p*整除，然而，我们可以很快看到第二项**是** *。*展开阶乘:

![](img/e673690dde78e650a4107f9503cab157.png)

方程式 13:方程式中的第二项。12 能被 p 整除。

由于 M 不能被 *p* 整除，等式中的第一个括号。6 也不能被 p 整除。现在考虑等式中的顶部积分。9.引入变量 y:

![](img/c2e8afda426514bba665b5e46bedb7ea.png)

积分变成:

![](img/8aff64600e0385a9a59b809ff73e83db.png)

分子中括号内的多项式具有整系数，其项从

![](img/dba051c320d85672f0bedd475b23db93.png)

几步之后，我们到达:

![](img/01497e3c29a988fd009f95d08ef7570c.png)

对于整数 *c* s(其中等式使用了 11)。每个 M( *k* )是一个可被 *p* 整除的整数，因此等式中的第一个括号。6 不能被 p 整除，因此我们得出结论，等式第一个括号中的项。6 是非零整数。如果它是零，它会被 p 整除，我们只是得出结论，它不是。

剩下的最后一块就是展示那个情商。假设我们选择足够大的 *p* 值，则 7 为真。使用 Eq。9、经过几个步骤后我们发现:

![](img/fc47c75a3de6ba36c21af962a2df4b1e.png)

如果二项式乘积的绝对值对于 x ∈ [0， *n* )有一个上界 B，我们得到:

![](img/723a7a01bd017d68869c28517988746b.png)

由于 RHS → 0 as *p* →∞，证明结束。

我的 [Github](https://github.com/marcotav) 和个人网站 [www.marcotavora.me](https://marcotavora.me/) 有一些关于数学和其他主题的有趣材料，如物理、数据科学和金融。看看他们！