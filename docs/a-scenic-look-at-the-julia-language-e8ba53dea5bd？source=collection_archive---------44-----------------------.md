# 朱莉娅语言快速浏览

> 原文：<https://towardsdatascience.com/a-scenic-look-at-the-julia-language-e8ba53dea5bd?source=collection_archive---------44----------------------->

## 体验朱莉娅，而不必做任何困难的事情

![](img/aff6a30b56ac61298072fbd9f1a7cc0c.png)

钱德勒·陈在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 介绍

Julia 是一种新的多用途编程语言，旨在通过提供易用性*和*速度来解决“两种语言问题”。

2010 年代开发的另外两种最受欢迎的新语言，Go 和 Rust，看起来都像是经过清理的 C 或 C++。相比之下，Julia 看起来像 Python 或 Matlab，只是稍微有点变化。这就是为什么 Julia 在科学家和学者中引起了很大的兴趣，他们通常更喜欢更容易开发和理解的语言。然而，随着 Julia 的库的成熟，Julia 在数据科学社区中获得了一些人气，尤其是在深度学习方面。

## 朱莉娅越来越受欢迎

如今，Julia 的下载量超过 100 万次，年增长率为 161%。包括亚马逊、苹果、迪士尼、脸书、福特、谷歌、IBM、微软、NASA、甲骨文和优步在内的大公司都是 Julia 的用户、合作伙伴或者正在雇佣 Julia 程序员。

![](img/e2f09b28a4dfa1e5a133ff884f821184.png)

纽约美联储银行的经济学家已经采用茱莉亚作为模型。他们报告说，有了 Julia，他们估算模型的速度比 T8 快了 10 倍，代码行数减少了一半，节省了时间，提高了可读性，减少了错误

用他们[的话说](https://juliacomputing.com/case-studies/ny-fed.html):

*“我们希望用我们的模型解决困难的问题——从理解金融市场发展到对家庭异质性建模——只有当我们接近编程的***前沿时，我们才能做到这一点。”**

## *一粒盐*

*虽然朱莉娅很令人兴奋，但重要的是不要轻信所有关于朱莉娅的炒作(T21)。处于编程的前沿并不总是一件好事。因为它太新了，所以 Julia 的代码库可能会有所欠缺。许多 Julia 库仍在更新和扩展，每隔几个月就有新的特性添加到语言中。随着旧功能的贬值，这可能会带来一些成长的烦恼。*

*是时候让朱莉娅成为你的主要语言了吗？也许吧。这取决于你需要从你的编程语言中得到什么。如果你需要更广泛使用的东西，Julia 可能不是最好的选择。但是，不管它是否应该是你的主要语言，朱莉娅是值得调查的。*

# *茱莉亚代码的风景照*

*接下来的部分包含解决不同领域问题的 Julia 代码片段。这些代码的目的是以简单易懂的方式展示日常编程在 Julia 中的样子。*

## *数学和统计学*

*基础数学:*

```
**# Polynomial function. Evaluating p(1) returns 9* p(x) = x^2 + 2x + 6# Make a (2,2) matrix of 1s
A = fill(1, 2, 2)# Matrix vector multiplication
x = rand(2)
y = A * x# Elementwise multiplication
B = A .* 5# Square every element of an array
B = B .^ 2*
```

*从分布中随机抽取样本:*

```
*using Distributions*# Take 100 samples from a normal distribution* dist = Normal(0, 1)
x = rand(dist, 100)* 
```

*求矩阵的特征值:*

```
*using LinearAlgebraA = rand(10, 10)
evals = eigvals(A)*
```

## *文件 IO 和字符串*

*读入文件并列出单词列表:*

```
*fstream = open("my_file_name.ext")
lines = readlines(fstream)
split_space(x) = split(x, " ")
words = map(split_space, lines)
all_words = reduce(append!, words)*
```

*在目录中查找`.txt`文件:*

```
*current_directory = @__DIR__
files = readdir(current_directory)
is_txt(x) = endswith(x, ".txt")
txt_files = filter(is_txt, files)*
```

## ***数据结构***

*创建最大堆并获得最大值:*

```
*using DataStructuresdata = rand(100)
heap = BinaryMaxHeap(data)
max_val = top(heap)*
```

## *机器学习*

*制作并训练一个密集的神经网络。(感谢[吉米·罗耶](https://medium.com/u/5b5fd49ae01e?source=post_page-----e8ba53dea5bd--------------------------------)更新了代码)*

```
*using Flux
using Flux: msemodel = Chain(
    Dense(2,100, relu),
    Dropout(.5),
    Dense(100, 100, relu),
    Dropout(.5),
    Dense(100, 1, relu),
    softmax
)# Random points in the plane
X = rand(2, 1000)# Label where points are within the unit circle or not
Y = sum(X .^ 2, dims=1) .< 1println("Example data")
for index in 1:10
    println("x1, x2, y = ", X[1, index], ", ", X[2, index], ", ", Y[index])
end# Mean squared error loss function
loss(x, y) = mse(model(x), y)# Format data and extract trainable parameters
data = [(X, Y)]
ps = Flux.params(model)# Train with stochastic gradient descent
optimizer = Descent(0.01)for epoch in 1:300
    Flux.train!(loss, ps, data, optimizer)
end*
```

# *结论*

*我已经用 Julia 写了一年多的代码，对这门语言非常感兴趣。虽然它要像 Python 一样成熟还有很长的路要走，但我相信它会成为科学计算和数据科学的顶级语言之一。*

*[](/how-to-learn-julia-when-you-already-know-python-641ed02b3fa7) [## 已经会 Python 了怎么学 Julia

### 跳到好的方面

towardsdatascience.com](/how-to-learn-julia-when-you-already-know-python-641ed02b3fa7) [](https://medium.com/swlh/how-julia-uses-multiple-dispatch-to-beat-python-8fab888bb4d8) [## Julia 如何利用多重调度击败 Python

### 亲自看

medium.com](https://medium.com/swlh/how-julia-uses-multiple-dispatch-to-beat-python-8fab888bb4d8)*