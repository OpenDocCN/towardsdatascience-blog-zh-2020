# 下降法——最速下降法和共轭梯度法

> 原文：<https://towardsdatascience.com/descent-method-steepest-descent-and-conjugate-gradient-math-explained-78601d8df3ce?source=collection_archive---------16----------------------->

## 数学解释

[](https://medium.com/@msdata/descent-method-steepest-descent-and-conjugate-gradient-in-python-85aa4c4aac7b) [## 下降法 Python 中的最速下降和共轭梯度

### 让我们从这个方程开始，我们想解出 x:

medium.com](https://medium.com/@msdata/descent-method-steepest-descent-and-conjugate-gradient-in-python-85aa4c4aac7b) 

让我们从这个方程开始，我们想解出 x:

![](img/6529068cacdf0150a681f9ff4c5a2ec3.png)

当 A 是**对称正定**时，解 x 最小化下面的函数(否则，x 可能是最大值)。这是因为 f(x)的梯度，∇f(x) = Ax- b .而当 Ax=b 时，∇f(x)=0 因而 x 是函数的最小值。

![](img/d3d15ad5e84f7731064323eace6553da.png)

在这篇文章中，我将向你展示两种求 x 的方法——最速下降法和共轭梯度法。

# 最速下降法

下降法的主要思想是，我们从 x 的起点开始，试图找到下一个更接近解的点，迭代这个过程，直到找到最终解。

例如，在步骤 k，我们在点𝑥(𝑘).我们如何决定下一步去哪里？我们应该往哪个方向走？我们应该去多少？

![](img/2571606e6e686277053ab14f5afe7831.png)

让我们假设我们决定去的方向是 p(k ),我们沿着这个方向走多远是𝛼.那么下一个数据点可以写成:

![](img/339bfb524060dd8c448104fa0202a0eb.png)

对于每一步，最速下降法都希望朝着最陡的斜坡下降，在那里最有效(这里 r 表示残差):

![](img/7d780309b0420b65e683247a99aeb64a.png)

一旦我们决定了前进的方向，我们要确保在这个方向上最小化函数:

![](img/e3a01238f482ae4020a98f6f8816ea34.png)![](img/89aa99d0c4766593f3f52df3039851ac.png)

现在我们可以计算下一个数据点的 x 和残差:

![](img/ea144fdedf2d540db4910b0787accc13.png)

这基本上是最速下降法背后的数学原理。通常我们会给残差一个停止准则，然后我们迭代这个过程，直到到达停止点。

对于 Python 实现，请查看:

[](https://medium.com/@msdata/descent-method-steepest-descent-and-conjugate-gradient-in-python-85aa4c4aac7b) [## 下降法 Python 中的最速下降和共轭梯度

### 让我们从这个方程开始，我们想解出 x:

medium.com](https://medium.com/@msdata/descent-method-steepest-descent-and-conjugate-gradient-in-python-85aa4c4aac7b) 

# 共轭梯度法

最速下降法是伟大的，我们在每一步的方向上最小化函数。但这并不能保证，我们要最小化的方向，来自于所有之前的方向。这里我们引入一个非常重要的术语共轭方向。方向 p 是共轭方向，如果它们具有下列性质(注 A 是对称正定的):

![](img/147f32f1b328d281ee0c1b0693c6a76b.png)

只有当前方向 p 与所有先前方向共轭时，下一个数据点才会在所有先前方向的跨度内最小化函数。

问题是，当 p 必须是共轭时，我们如何计算搜索方向 p？

记住最陡下降选择了最陡斜率，也就是每步的残差(r)。我们知道这是一个好的选择。不如我们找一个最接近最陡下降方向的 A 共轭方向，也就是说，我们最小化向量(r-p)的 2 范数。

通过计算，我们知道当前方向是当前残差和上一个方向的组合。

![](img/23880653bb9e9a48ca73f66fbfaa54a7.png)

因为α-共轭方向的性质:

![](img/720c5570814414f787516bfae67d5120.png)

然后我们可以计算𝛾_𝑘:

![](img/f923d4d5be1a0a1e916c5476fa4fc8b0.png)

总之，共轭梯度法如下:

![](img/edb45ed9a7dd62a2fd490d242123b492.png)![](img/60ee76d8871cfb03611ce1124bbd8095.png)

同样，对于 Python 实现，请查看:

[](https://medium.com/@msdata/descent-method-steepest-descent-and-conjugate-gradient-in-python-85aa4c4aac7b) [## 下降法 Python 中的最速下降和共轭梯度

### 让我们从这个方程开始，我们想解出 x:

medium.com](https://medium.com/@msdata/descent-method-steepest-descent-and-conjugate-gradient-in-python-85aa4c4aac7b) 

现在你知道如何用最速下降法和共轭梯度法解线性方程组了！尽情享受吧！

参考:

[](https://www.cs.utexas.edu/users/flame/laff/alaff/chapter08-important-observations.html) [## ALAFF 技术细节

### 这一单元可能是这门课程中技术难度最大的单元。为了完整起见，我们在这里给出了细节，但是…

www.cs.utexas.edu](https://www.cs.utexas.edu/users/flame/laff/alaff/chapter08-important-observations.html)