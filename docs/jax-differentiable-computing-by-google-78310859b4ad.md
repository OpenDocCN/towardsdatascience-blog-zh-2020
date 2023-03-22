# JAX:谷歌的差异化计算

> 原文：<https://towardsdatascience.com/jax-differentiable-computing-by-google-78310859b4ad?source=collection_archive---------22----------------------->

## 革命性的机器学习框架

![](img/c6db06eda2bea13289bfcbe5c599ef03.png)

图片由来自[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=969757)的[皮特·林福思](https://pixabay.com/users/thedigitalartist-202249/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=969757")拍摄

自从深度学习在 2010 年代初起飞以来，许多框架被编写来促进研究和生产中的深度学习。为了记录在案，让我们提一下 [Caffe](https://caffe.berkeleyvision.org/) 、 [Theano](http://deeplearning.net/software/theano/) 、 [Torch](http://torch.ch/) 、[千层面](https://lasagne.readthedocs.io/en/latest/)、 [Tensorflow](https://www.tensorflow.org/) 、 [Keras](https://keras.io/) 或 [PyTorch](https://pytorch.org/) 。一段时间后，这些框架中的一些消失了。其他的幸存下来并发展到今天，主要是 PyTorch 和 Tensorflow。随着时间的推移，这些框架演变成具有许多不同功能的大型生态系统。一方面，它们支持新模型的训练，包括在多 GPU 系统上的大规模并行训练。另一方面，它们允许用户在云中或移动设备上轻松部署训练好的模型。

**不幸的是，这种精简有时会以机器学习研究人员测试新模型想法所需的灵活性为代价。谷歌通过其名为 [JAX](https://github.com/google/jax) 的新框架瞄准的正是这个用户群体。在这篇博文中，我将向你展示 JAX 是如何通过向量化计算和梯度计算提供一个低级的、高性能的接口来鼓励实验的。**

# JAX 的设计哲学

**JAX 的目标是允许用户通过即时编译和自动并行化来加速原始 Python 和 NumPy 函数，并计算这些函数的梯度**。为了做到这一点，JAX 采用了功能设计。**梯度计算等函数被实现为作用于用户定义的 Python 函数的函数变换**(或函子)。例如，要计算绝对值函数的梯度，您可以写:

```
from jax import graddef abs_val(x):
  if x > 0:
    return x
  else:
    return -x

abs_val_grad = grad(abs_val)
```

如你所见， *abs_val* 是一个普通的 Python 函数，由仿函数 *grad* 转化而来。

为了能够使用 JAX 函子，用户定义的函数必须遵循一些限制:

1.  **JAX 处理的每一个函数都要求是纯的。这意味着当用相同的输入调用时，它应该总是返回相同的结果。从功能上来说，**可能没有任何副作用**。否则，JAX 无法保证使用实时编译或并行化时的正确性。功能纯度实际上并没有被强制。虽然有时可能会抛出错误，但确保这种情况主要是程序员的责任。**
2.  人们不得不使用 *jax.ops* 包中的替代功能，而不是像 *x[i] += 1* 这样的就地突变更新。
3.  实时(JIT)编译对 Python 控制流有一些限制。

就哲学而言，JAX 类似于你在使用纯函数式语言(如 Haskell)时会遇到的情况。事实上，开发人员甚至在文档中使用 Haskell 类型的签名来解释一些转换。

# 即时编译

**JAX 的主要特性之一是能够通过 JIT 加速 Python 代码的执行。在内部，JAX 使用 XLA 编译器来完成这项工作。XLA 不仅能为 CPU 编译代码，还能为**GPU 甚至是**TPUs 编译代码。这使得 JAX 非常强大和多才多艺。为了使用 XLA 编译一个函数，你可以像这样使用 *jit* 仿函数:**

```
from jax import jit
import jax.numpy as jnp**def** selu(x, alpha=1.67, lmbda=1.05):
  **return** lmbda * jnp.where(x > 0, x, alpha * jnp.exp(x) - alpha)selu_jit = jit(selu)
```

或者，您也可以使用 *jit* 作为函数定义顶部的装饰器:

```
**@jit
def** selu(x, alpha=1.67, lmbda=1.05):
  **return** lmbda * jnp.where(x > 0, x, alpha * jnp.exp(x) - alpha)
```

*jit* 仿函数**通过将输入函数的编译版本返回给调用者来转换**输入函数。调用原来的*卢瑟*函数会使用 Python 解释器，而调用*卢瑟 _jit* 会调用编译后的版本，应该会快很多，特别是对于 numpy 数组这样的矢量化输入。此外， **JIT 编译只发生一次，并在其后被缓存**，使得函数的后续调用非常高效。

# 使用 vmap 的自动矢量化

在训练机器学习模型时，通常会计算输入数据子集的一些损失，然后更新模型参数。由于为每个输入顺序计算模型的正向函数会太慢，所以习惯上是将数据的子集一起分批到一个批次中，并在诸如 GPU 的加速器上以并行方式计算正向函数。这是在 JAX 通过使用函数 *vmap* 完成的:

```
def forward(input, w, b):
    input = jnp.dot(input, w) + b
    return jnp.max(input, 0)jax.vmap(forward, in_axes=(0, None, None), out_axes=0)(inputs, w, b)
```

该代码片段采用一个具有 ReLU 激活的全连接层的转发功能，并在一批输入上并行执行该功能。参数*输入轴*和*输出轴*指定在哪些参数和轴上发生并行化。在这种情况下，(0，None，None)意味着输入在第 0 轴上并行化，而 w 和 b 保持不变。输出在 0 轴上并行化。

# 计算梯度

让机器学习研究人员对 JAX 特别感兴趣的是它计算任意纯函数梯度的能力。JAX 从 autograd 继承了这种能力，autograd 是一个计算 NumPy 数组导数的包。

要计算函数的梯度，只需使用 *grad* 变换:

```
import jax.numpy as jnpgrad(jnp.tanh))(2.0)[0.070650816]
```

如果您想计算高阶导数，您可以简单地将多个 *grad* 变换链接在一起，如下所示:

```
import jax.numpy as jnpgrad(grad(jnp.tanh))(2.0)[-0.13621889]
```

虽然将 *grad* 应用于 R(实数集)中的函数会得到一个单一的数字作为输出，但您也可以将其应用于向量值函数以获得雅可比矩阵:

```
def f(x):
    return jnp.asarray(
        [x[0], 5*x[2], 4*x[1]**2 - 2*x[2], x[2] * jnp.sin(x[0])])print(jax.jacfwd(f)(jnp.array([1., 2., 3.])))[[ 1\.       0\.       0\.     ]
 [ 0\.       0\.       5\.     ]
 [ 0\.      16\.      -2\.     ]
 [ 1.6209   0\.       0.84147]]
```

这里，我们不使用 *grad* 变换，因为它只对标量输出函数有效。相反，我们使用类似的函数 jacfwd 进行自动前向模式区分(关于前向和后向模式区分的详细讨论，请参考 [JAX 文档](https://jax.readthedocs.io/en/latest/jax.html?highlight=grad#jax.jacfwd))。

**注意，我们可以在 JAX 将功能转换链接在一起。**例如，我们可以使用 *vmap* 对我们的函数进行矢量化，使用 *grad* 计算梯度，然后使用 *jit* 编译结果函数，例如深度学习中的标准小批量训练循环:

```
def **loss**(x, y):
    out = net(x)
    cross_entropy = -y * np.log(out) - (1 - y)*np.log(1 - out)
    return cross_entropyloss_grad = jax.jit(jax.vmap(jax.grad(loss), in_axes=(0, 0), out_axes=0))
```

这里， *loss_grad* 计算交叉熵损失的梯度，同时在输入(x，y)和单个输出(损失)上并行。整个函数是 jitted 的，以允许在 GPU 上快速计算。产生的函数被缓存，允许用不同的输出调用它，而没有任何额外的开销。

多种转换的组合使得框架非常强大，并为用户设计数据流提供了极大的灵活性。

# 结论

对于需要额外灵活性的研究人员来说，JAX 为 PyTorch 或 Tensorflow 等更高级的框架提供了一个有用的替代方案。通过原生 Python 和 NumPy 函数进行区分的能力令人惊叹，JIT 编译和自动矢量化功能极大地简化了为 GPU 或 TPUs 等大规模并行架构编写高效代码的工作。然而，我最喜欢 JAX 的是它干净的功能界面。一个成熟的高级 API 生态系统将围绕它发展，这肯定只是时间问题。敬请期待！