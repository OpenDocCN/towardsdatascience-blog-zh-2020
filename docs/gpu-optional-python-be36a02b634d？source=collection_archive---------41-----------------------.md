# GPU-可选 Python

> 原文：<https://towardsdatascience.com/gpu-optional-python-be36a02b634d?source=collection_archive---------41----------------------->

## 编写代码，在 GPU 可用和需要时利用它，但在不可用时在 CPU 上运行良好

![](img/effc4396e8fa702244923b2f08bb760c.png)

照片由[弗雷德里克·滕东](https://unsplash.com/@frdx?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

奇妙的 CuPy 库允许你在 NVIDIA GPU 上轻松运行 NumPy 兼容代码。然而，有时

*   您(或您的用户)没有兼容的 GPU，或者
*   你没有兼容 CuPy 的 Python 环境，或者
*   您的代码在我们的 GPU 上运行比在您的多个 CPU 上运行慢。

通过定义三个简单的实用函数，您可以使您的代码 *GPU 可选。(*在[这个小的 GitHub 项目](https://github.com/CarlKCarlK/gpuoptional/)中找到实用函数的定义。)

当我们试图使用 CuPy 为我们的开源基因组包 [FaST-LMM](https://fastlmm.github.io/) 添加 GPU 支持时，我们遇到了三个问题。我们用一个简单的效用函数解决了每个问题。让我们看看每个问题，解决它的效用函数，以及例子。

## 问题 1:控制使用 NumPy 还是 CuPy

假设您想要生成一组简单的随机 DNA 数据。行代表个人。列代表 DNA 位置。值代表“等位基因计数”，可以是 0、1、2 或 NaN(代表缺失)。此外，您希望

*   默认生成一个 NumPy 数组
*   通过字符串、数组模块或环境变量请求时生成 CuPy 数组
*   当对 CuPy 的请求失败时，回退到 NumPy 例如，因为您的计算机不包含 GPU 或者因为没有安装 CuPy。

效用函数`array_module`(在 [GitHub](https://github.com/CarlKCarlK/gpuoptional/) 中定义)解决了这个问题。下面是使用`array_module`生成的数据生成器:

```
def gen_data(size, seed=1, **xp=None**):
    **xp = array_module(xp)**
    rng = xp.random.RandomState(seed=seed)
    a = rng.choice([0.0, 1.0, 2.0, xp.nan], size=size)
    return a
```

*输入:*

```
a = gen_data((1_000,100_000))# Python 3.6+ allows _ in numbers
print(type(a))
print(a[:3,:3]) # print 1st 3 rows & cols
```

*输出:*

```
<class 'numpy.ndarray'>
[[ 1\. nan  0.]
 [ 0\.  2\.  2.]
 [nan  1\.  0.]]
```

根据需要，DNA 生成器默认返回一个`numpy`数组。

注意`gen_data`的可选`xp`参数。当`xp`通过`array_module`效用函数时，会发生以下情况:

*   如果你还没有安装 CuPy 包，`xp`就会是`numpy`。
*   否则；如果您指定了字符串`'cupy'`或`'numpy'`，那么您的规范将会得到遵守。
*   否则；如果您指定数组模块`cupy`或`numpy`，您的规范将会得到尊重。
*   否则，如果您将`'ARRAY_MODULE'`环境变量设置为`'cupy'`或`'numpy'`，您的规范将会得到遵守。
*   否则，`xp`就是`numpy`。

让我们看看它在我安装了 GPU 和 CuPy 的机器上的工作情况:

*输入:*

```
a = gen_data((1_000,100_000),xp='cupy')
print(type(a))
print(a[:3,:3]) # print 1st 3 rows & cols
```

*输出:*

```
<class 'cupy.core.core.ndarray'>
[[ 0\. nan  0.]  
 [ 2\.  2\.  2.]
 [ 0\. nan  1.]]
```

正如所料，它按照要求生成一个`cupy`数组。

> 旁白:注意 NumPy 和 CuPy 生成不同的随机数，即使给定相同的种子。

接下来，让我们通过环境变量请求 CuPy。

*输入:*

```
# 'patch' is a nice built-in Python function that can temporarily
# add an item to a dictionary, including os.environ.
from unittest.mock import patchwith patch.dict("**os.environ", {"ARRAY_MODULE": "cupy"}**) as _:
    a = gen_data((5, 5))
    print(type(a))
```

*输出:*

```
<class 'cupy.core.core.ndarray'>
```

正如所料，我们可以通过环境变量请求一个`cupy`数组。(同样，使用`patch`，我们可以临时设置一个环境变量。)

## 问题 2:从数组中提取“`xp`”数组模块

假设您想要“标准化”一组 DNA 数据。这里的“标准化”是指使每一列的值具有平均值 0.0 和标准差 1.0，并用 0.0 填充缺失值。此外，您希望这能起作用

*   对于 NumPy 阵列，即使您还没有或不能安装 CuPy 包
*   对于 NumPy 阵列和 CuPy 阵列

效用函数`get_array_module`(在 [GitHub](https://github.com/CarlKCarlK/gpuoptional/) 中定义)解决了这个问题。以下是使用`get_array_module`的标准化器:

```
def unit_standardize(a):
    """
    Standardize array to zero-mean and unit standard deviation.
    """
 **xp = get_array_module(a)** assert a.dtype in [
        np.float64,
        np.float32,
    ], "a must be a float in order to standardize in place." imissX = xp.isnan(a)
    snp_std = xp.nanstd(a, axis=0)
    snp_mean = xp.nanmean(a, axis=0)
    # avoid div by 0 when standardizing
    snp_std[snp_std == 0.0] = xp.inf a -= snp_mean
    a /= snp_std
    a[imissX] = 0
```

注意我们如何使用`get_array_module`将`xp`设置为数组模块(或者`numpy`或者`cupy`)。然后我们用`xp`调用`xp.isnan`之类的函数。

让我们标准化一个 NumPy 数组:

*输入:*

```
a = gen_data((1_000,100_000))
unit_standardize(a)
print(type(a))
print(a[:3,:3]) #1st 3 rows and cols
```

*输出:*

```
<class 'numpy.ndarray'>
[[-0.0596511   0\.         -1.27903946]
 [-1.32595873  1.25433129  1.21118591] 
 [ 0\.          0.05417923 -1.27903946]]
```

在我的电脑上，这运行得很好，大约在 5 秒钟内返回一个答案。

接下来，让我们标准化一个 CuPy 阵列:

*输入:*

```
a = gen_data((1_000,100_000), xp='cupy')
unit_standardize(a)
print(type(a))
print(a[:3,:3]) #1st 3 rows and cols
```

*输出:*

```
<class 'cupy.core.core.ndarray'>
[[-1.22196758  0\.         -1.23910541]
 [ 1.24508589  1.15983351  1.25242913]
 [-1.22196758  0\.          0.00666186]]
```

在我的电脑上，标准化在 CuPy 阵列上运行得更快，大约在 1 秒钟内返回一个答案。

> 旁白:那么 GPU 是不是更快了？不一定。上面运行的 CPU 只用了我六个 CPU 中的一个。当我使用其他技术——例如，Python 多处理或多线程 C++代码——在所有 6 个 CPU 上运行时，运行时变得可比。

## 问题 3:在 NumPy 和"`xp`"之间转换

假设您的数据以 NumPy 数组开始，您需要将它转换成您想要的`xp`数组模块。稍后，假设您的数据是一个`xp`数组，您需要将它转换成一个 NumPy 数组。(使用熊猫等知道 NumPy 但不知道 CuPy 的包时会出现这些情况。)

内置函数`xp.asarray`和实用函数`asnumpy`(在 [GitHub](https://github.com/CarlKCarlK/gpuoptional/) 中定义)解决了这个问题。这里有一个例子:

*输入:*

```
a = gen_data((1_000,100_000))
print(type(a)) # numpy
xp = array_module(xp='cupy')
**a = xp.asarray(a)**
print(type(a)) # cupy
unit_standardize(a)
print(type(a)) # still, cupy
**a = asnumpy(a)**
print(type(a)) # numpy
print(a[:3,:3]) # print 1st 3 rows and cols
```

*输出 1:*

```
<class 'numpy.ndarray'>
<class 'cupy.core.core.ndarray'>
<class 'cupy.core.core.ndarray'>
<class 'numpy.ndarray'>
[[-0.0596511   0\.         -1.27903946]
 [-1.32595873  1.25433129  1.21118591]
 [ 0\.          0.05417923 -1.27903946]]
```

此示例生成一个随机 NumPy 数组。将其转换为 CuPy(如果可能的话)。标准化它。转换(如有必要)为 NumPy。在我的电脑上，它大约运行 2 秒钟。

如果没有安装 CuPy，代码仍然运行良好(大约 5 秒钟)，产生以下输出:

*输出 2:*

```
WARNING:root:Using numpy. (No module named 'cupy')
<class 'numpy.ndarray'>
<class 'numpy.ndarray'>
<class 'numpy.ndarray'>
<class 'numpy.ndarray'>
[[-0.0596511   0\.         -1.27903946] 
 [-1.32595873  1.25433129  1.21118591] 
 [ 0\.          0.05417923 -1.27903946]]
```

> 旁白:请注意，无论是否安装了 CuPy，我们现在看到的结果都是一样的。为什么？因为这个例子总是使用 NumPy 来生成随机数据。

# 结论

我们已经看到了三个实用函数如何使您能够编写使用或不使用 GPU 都可以工作的代码。

*   `array_module` —如果可能，根据用户输入(包括环境变量)设置`xp`。默认并在必要时回退到 NumPy。
*   `get_array_module` —根据数组设置`xp`。即使没有或不能安装`cupy`包也能工作。
*   `xp.asarray`和`asnumpy` —与 NumPy 相互转换

除了支持使用或不使用 GPU 的代码之外，编写 GPU 可选的代码还有一个额外的好处。GPU 有时会无法加速你的工作。通过让你的代码成为可选的 GPU，你可以在 GPU 不起作用的时候关闭它。

你可以在这个小小的 GitHub 项目中找到这些实用函数的定义。