# 通过在 Julia 中实现来理解卷积

> 原文：<https://towardsdatascience.com/understanding-convolution-by-implementing-in-julia-3ed744e2e933?source=collection_archive---------22----------------------->

## 一个关于我在 Julia 中实现卷积的实验以及从中得到的启示的故事。

![](img/19351a90bc258e5d92d96d3daecea8b4.png)

罗马法师在 [Unsplash](https://unsplash.com/s/photos/math?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

在 Fast.ai 深度学习课程的[第二部分中，我了解到不仅要能够使用 Tensorflow / PyTorch 等深度学习库，而且要真正理解其背后的想法和实际发生的事情，这很重要。没有比我们自己尝试和实现它更好的理解它的方法了。](https://course.fast.ai/part2)

在机器学习实践中，卷积是我们都非常熟悉的东西。所以我想，为什么不试一试呢？下面是我在 Julia 中实现卷积的实验结果。

## 为什么是朱莉娅？

当我上[吴恩达的机器学习](https://www.coursera.org/learn/machine-learning)课程时，我主要是用 MATLAB 来理解机器学习算法。但是 MATLAB 是一种商业产品，可能非常昂贵。

Julia 非常快，并且专门设计为非常擅长数值和科学计算，这是我们在实现机器学习算法时需要的。也很好学。最重要的是，它是免费和开源的。

![](img/5d3f5303fa0a513c11aa5f4b6815167c.png)

与其他语言相比。[来源](https://julialang.org/benchmarks/)

# 开始实验

好了，现在是开始工作的时候了。你看，已经有这么多关于 CNN 的文章了，所以我就不多解释了。如果你正在研究 CNN 的基本概念，我推荐吴恩达的这个 Youtube 播放列表。

从简单的事情开始总是好的。所以对于我的实验，我将使用 6x6 矩阵作为输入，3x3 矩阵作为滤波器，以及 4x4 矩阵的预期输出。我从小矩阵开始，这样我可以检查我的函数的准确性。

![](img/96e20f39ba13807991adcea7f7a3c194.png)

通过电子表格卷积。

我通过 Julia CLI 定义输入、过滤器和预期输出。输入将是 6x6 矩阵，滤波器将是 3x3 矩阵，预期输出将是 4x4 矩阵，具有与上图相同的值。

```
**julia>** input = [[3,2,4,2,7,6] [8,0,2,1,7,8] [2,2,10,4,1,9] [1,5,4,6,5,0] [5,4,1,7,5,6] [5,0,2,7,6,8]]6×6 Array{Int64,2}:
3  8   2  1  5  5
2  0   2  5  4  0
4  2  10  4  1  2
2  1   4  6  7  7
7  7   1  5  5  6
6  8   9  0  6  8**julia>** filter = [[1,1,1] [0,0,0] [-1,-1,-1]]3×3 Array{Int64,2}:
1  0  -1
1  0  -1
1  0  -1**julia>** output = [[-5,-8,-2,1] [0,-12,-5,5] [4,4,2,-4] [3,6,0,-10]]4×4 Array{Int64,2}:
-5    0   4    3
-8  -12   4    6
-2   -5   2    0
 1    5  -4  -10
```

## 迭代#1

因为我们已经知道输入(6x6)和滤波器(3x3)，所以输出应该是 4x4。在第一次迭代中，如果我们手动进行计算，并假设过滤器为 3x3，代码将如下所示:

第一次迭代

我们可以在 CLI 中复制整个函数并查看结果。

```
**julia>** conv_1(input, filter) == outputtrue
```

而且很管用！Julia 的另一个很棒的特性是我们可以对我们的函数进行基准测试。我们可以通过使用[基准工具](https://github.com/JuliaCI/BenchmarkTools.jl)来完成。

```
**julia>** using BenchmarkTools**julia>** @benchmark conv_1(input, filter)BenchmarkTools.Trial:
memory estimate:  208 bytes
allocs estimate:  1
--------------
minimum time:     196.880 ns (0.00% GC)
median time:      200.165 ns (0.00% GC)
mean time:        212.749 ns (0.82% GC)
maximum time:     1.828 μs (0.00% GC)
--------------
samples:          10000
evals/sample:     616
```

正如你所看到的，朱莉娅速度惊人！但是如果我们有超过 3x3 的滤镜呢？

## 迭代#2

在第二次迭代中，让我们通过遍历一个滤波器矩阵来尝试这样做。代码将如下所示:

第二次迭代

```
**julia>** conv_2(input, filter) == outputtrue
```

代码正在工作，现在它可以接受任何大小的过滤器。它的性能怎么样？

```
**julia>** @benchmark conv_2(input, filter)BenchmarkTools.Trial:
memory estimate:  208 bytes
allocs estimate:  1
--------------
minimum time:     485.372 ns (0.00% GC)
median time:      488.586 ns (0.00% GC)
mean time:        517.087 ns (0.33% GC)
maximum time:     13.017 μs (0.00% GC)
--------------
samples:          10000
evals/sample:     191
```

还是很快的！但是我能让它更快吗？也许如果我找到另一种方式，而不是通过过滤器循环，这将使它更快，更简洁地阅读。

## 迭代#3

在 Julia 中，我们可以用点语法做到这一点。因此，我们可以这样做，而不是遍历过滤器:

第三次迭代。

第三次迭代现在更加简洁。让我们试一试:

```
**julia>** conv_3(input, filter) == outputtrue**julia>** @benchmark conv_3(input, filter)BenchmarkTools.Trial:
memory estimate:  5.20 KiB
allocs estimate:  33
--------------
minimum time:     2.378 μs (0.00% GC)
median time:      2.482 μs (0.00% GC)
mean time:        2.679 μs (2.05% GC)
maximum time:     71.501 μs (95.64% GC)
--------------
samples:          10000
evals/sample:     9
```

令我惊讶的是，它不但没有变快，反而变得更慢了，而且差别很大！！这是为什么呢？文档是这样写的:

> 在其他语言中，为了提高性能，通常也需要矢量化:如果循环很慢，函数的“矢量化”版本可以调用用低级语言编写的快速库代码。在 Julia 中，矢量化函数不是性能所需的*而不是*，事实上，编写自己的循环通常是有益的。

所以第二次迭代是目前为止卷积的最佳实现。但是这太简单了。如果我更进一步呢？

## 带衬垫

卷积中常用的另一个概念是填充。如果我们有 6×6 输入矩阵和 3×3 滤波器矩阵，而不是 4×4 矩阵作为输出，通过填充，我们可以得到 6×6 矩阵。这种方法通常被称为“相同”卷积。如果你想了解更多关于填充的概念，请尝试观看来自吴恩达的视频 [C4W1L04](https://www.youtube.com/watch?v=smHa2442Ah4) 。

如何在我们的卷积函数中实现填充？一种方法是重新创建输入矩阵，在真实值周围填充。计算我们需要多少衬垫的公式是:

```
padding = (filter - 1) / 2
```

上面的公式假设滤波器矩阵的大小是奇数。即使不总是，这也是很奇怪的。所以在我的实现中，我假设滤波器矩阵的大小是奇数，我将用 0 填充填充。

带衬垫

当我试图计算填充值时，您可能会注意到\运算符。这是因为 Julia 的类型稳定性特征。基本上所有的`/`运算符都会返回 Float，所有的\都会返回 Integer。这种类型稳定性是 Julia 超快的一个主要原因！

让我们试试这个函数，以获得与输入和滤波器矩阵“相同”的卷积。请注意，从[2，2]到[5，5]的值与前一个函数完成的“有效”卷积相同。

```
**julia>** padding_conv_1(input, filter, "same")6×6 Array{Float64,2}:
 -8.0   1.0    2.0  -5.0    1.0   9.0
-10.0  -5.0    0.0   4.0    3.0  10.0
 -3.0  -8.0  -12.0   4.0    6.0  12.0
-10.0  -2.0   -5.0   2.0    0.0  13.0
-16.0   1.0    5.0  -4.0  -10.0  18.0
-15.0   3.0   10.0  -1.0   -9.0  11.0**julia>** padding_conv_1(input, filter, "same")[2:5,2:5] == conv_2(input, filter)true
```

我们的 padding_conv 函数的性能也不差。

```
**julia>** @benchmark padding_conv_1(input, filter, "same")BenchmarkTools.Trial:
memory estimate:  992 bytes
allocs estimate:  2
--------------
minimum time:     1.746 μs (0.00% GC)
median time:      1.803 μs (0.00% GC)
mean time:        1.865 μs (0.72% GC)
maximum time:     70.076 μs (96.21% GC)
--------------
samples:          10000
evals/sample:     10
```

虽然与预期的“有效”卷积相比相当慢，但内存分配仍然很低。

## 步进卷积

同样，我想通过尝试实现“步进”卷积来改进我的卷积。通常情况下，步幅=1。如果你想了解更多的概念，请观看来自吴恩达的视频 [C4W1L05](https://www.youtube.com/watch?v=tQYZaDn_kSg) 。

实现步进卷积有点棘手。首先，我需要根据输入、过滤器和步幅找到输出矩阵的大小。该尺寸的公式为:

```
result = (input-filter) ÷ stride + 1
```

因此，如果我们有 7×7 输入矩阵和 3×3 滤波器矩阵，并且跨距=2，我们将有 3×3(而不是跨距=1 的 5×5)。棘手的部分是迭代输入矩阵。在我们的简单卷积中，输入和输出矩阵之间的变化率是相同的，即 1。这就是为什么我们在迭代输入矩阵时可以使用变量`i`和`j`。

但是当步幅不为 1 时，我们需要一个额外的变量来迭代输入矩阵。代码如下:

迈着大步

让我们看看性能结果。在这次检查中，我使用 7x7 输入矩阵和 3x3 滤波器。这是为了让我可以尝试 stride=1(预期 5x5 过滤器)和 stride=2 (3x3 过滤器)。

```
**julia>** input7 = rand(7,7)**julia>** @benchmark stride_conv_1(input7, filter)BenchmarkTools.Trial:
memory estimate:  288 bytes
allocs estimate:  1
--------------
minimum time:     742.395 ns (0.00% GC)
median time:      747.081 ns (0.00% GC)
mean time:        772.902 ns (0.40% GC)
maximum time:     5.709 μs (86.29% GC)
--------------
samples:          10000
evals/sample:     124**julia>** @benchmark stride_conv_1(input7, filter, 2)BenchmarkTools.Trial:
memory estimate:  160 bytes
allocs estimate:  1
--------------
minimum time:     319.876 ns (0.00% GC)
median time:      322.438 ns (0.00% GC)
mean time:        327.930 ns (0.56% GC)
maximum time:     2.753 μs (87.78% GC)
--------------
samples:          10000
evals/sample:     233
```

步幅越大，速度越快！这是有意义的，因为我们比以前迭代得少。

## 把所有的放在一起

我们已经看到了如何使用填充和步幅> 1 进行简单卷积。让我们看看能否将它们放在一个函数中。

基本二维卷积

我们可以在 CLI 上尝试一下。

```
**julia>** @benchmark conv2d(input7, filter, 2, "same")BenchmarkTools.Trial:
memory estimate:  944 bytes
allocs estimate:  2
--------------
minimum time:     980.100 ns (0.00% GC)
median time:      1.044 μs (0.00% GC)
mean time:        1.072 μs (1.00% GC)
maximum time:     55.501 μs (97.63% GC)
--------------
samples:          10000
evals/sample:     10
```

# 结论

我所做的只是一个非常基本的二维卷积算法。在实践中，我们通常使用多层的三维卷积，并且通常与池算法配对。但对我来说，在 Julia 中学习并实现一个非常基础的卷积已经是非常令人兴奋的事情了。

我也还在学习朱莉娅。请随时告诉我如何改进我的代码。感谢阅读。

*如果你对代码感兴趣，这里有 GitHub* *上资源库的* [*链接。*](https://github.com/yos1p/ml-julia)