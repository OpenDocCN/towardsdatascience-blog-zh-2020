# 为您的下一个机器学习项目提供 7 个 PyTorch 函数

> 原文：<https://towardsdatascience.com/useful-pytorch-functions-356de5f31a1e?source=collection_archive---------34----------------------->

## 机器学习

## 探索各种 PyTorch 函数

![](img/1ca4e44f20f200c220b9b8908c26f592.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1995786) 的 [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1995786)

PyTorch 是一个越来越受欢迎的机器学习库。在本文中，我们将探索 PyTorch 中的七个可用函数。

首先，我们将使用`import torch`导入 PyTorch

## 功能一:torch.linspace

`torch.linspace`用于在值`start`和`end`之间创建一个 1D 等距张量。我们可以用`steps`参数指定张量的大小。默认为`steps=100`

***例-1:***

```
torch.linspace(1, 10)**Output:**
tensor([ 1.0000, 1.0909, 1.1818, 1.2727, 1.3636, 1.4545, 1.5455, 1.6364, 1.7273, 1.8182, 1.9091, 2.0000, 2.0909, 2.1818, 2.2727, 2.3636, 2.4545, 2.5455, 2.6364, 2.7273, 2.8182, 2.9091, 3.0000, 3.0909, 3.1818, 3.2727, 3.3636, 3.4545, 3.5455, 3.6364, 3.7273, 3.8182, 3.9091, 4.0000, 4.0909, 4.1818, 4.2727, 4.3636, 4.4545, 4.5455, 4.6364, 4.7273, 4.8182, 4.9091, 5.0000, 5.0909, 5.1818, 5.2727, 5.3636, 5.4545, 5.5455, 5.6364, 5.7273, 5.8182, 5.9091, 6.0000, 6.0909, 6.1818, 6.2727, 6.3636, 6.4545, 6.5455, 6.6364, 6.7273, 6.8182, 6.9091, 7.0000, 7.0909, 7.1818, 7.2727, 7.3636, 7.4545, 7.5455, 7.6364, 7.7273, 7.8182, 7.9091, 8.0000, 8.0909, 8.1818, 8.2727, 8.3636, 8.4545, 8.5455, 8.6364, 8.7273, 8.8182, 8.9091, 9.0000, 9.0909, 9.1818, 9.2727, 9.3636, 9.4545, 9.5455, 9.6364, 9.7273, 9.8182, 9.9091, 10.0000])
```

***例-2:***

```
torch.linspace(start=1, end=10, steps=5)**Output:**
tensor([ 1.0000,  3.2500,  5.5000,  7.7500, 10.0000])
```

## 功能二:手电筒.眼睛

`torch.eye`返回对角线值为 1，其他值为 0 的 2D 张量
该函数需要两个参数— `n`和`m`。如果没有指定`m`，那么它返回一个大小为`nxn`的 2D 张量

***例-1:***

```
torch.eye(n=4, m=5)**Output:** tensor([[1., 0., 0., 0., 0.],
        [0., 1., 0., 0., 0.],
        [0., 0., 1., 0., 0.],
        [0., 0., 0., 1., 0.]])
```

***例-2:***

```
torch.eye(n=3)**Output:**
tensor([[1., 0., 0.],
        [0., 1., 0.],
        [0., 0., 1.]])
```

## 功能三:火炬。满

`torch.full`返回一个大小为`size`的张量，其值填充为`fill_value`
`size`可以是一个列表或元组。

***例题-1:***

```
torch.full(size=(3,2), fill_value=10)**Output:** tensor([[10., 10.],
        [10., 10.],
        [10., 10.]])
```

***例-2:***

```
torch.full(size=[2, 3, 4], fill_value=5)**Output:**
tensor([[[5., 5., 5., 5.],
         [5., 5., 5., 5.],
         [5., 5., 5., 5.]],[[5., 5., 5., 5.],
         [5., 5., 5., 5.],
         [5., 5., 5., 5.]]])
```

## 功能四:torch.cat

`torch.cat`连接指定维度上的一系列张量`dim`。所有的张量必须是相同的形状

***例-1:***

```
a = torch.ones(3,2)
b = torch.zeros(3,2)
torch.cat((a, b)) # default dim=0**Output:** tensor([[1., 1.],
        [1., 1.],
        [1., 1.],
        [0., 0.],
        [0., 0.],
        [0., 0.]])
```

***例-2:***

```
x = torch.full((3,3), fill_value=4)
y = torch.full((3,3), fill_value=7)
torch.cat((x, y), dim=1)**Output:** tensor([[4., 4., 4., 7., 7., 7.],
        [4., 4., 4., 7., 7., 7.],
        [4., 4., 4., 7., 7., 7.]])
```

## 功能五:火炬.拿

`torch.take`返回给定索引处输入张量元素的张量。输入张量被视为 1D 张量来返回值。

***例-1:***

```
# 1D input Tensor
b = torch.tensor([10, 20, 30, 40, 50])
torch.take(b, torch.tensor([2]))**Output:**
tensor([30])
```

***例-2:***

```
# 2D input tensor
a = torch.tensor([[1, 2, 3],
                  [4, 5, 6]])
torch.take(a, torch.tensor([3,4]))**Output:** tensor([4, 5])
```

## 功能 6: torch.unbind

`torch.unbind`沿给定维度`dim`
删除一个张量维度，默认维度为 0，即`dim=0`

***例-1:***

```
a = torch.tensor([[1, 2, 3],
                  [4, 5, 6]])
torch.unbind(a)**Output:**
(tensor([1, 2, 3]), tensor([4, 5, 6]))
```

***例-2:***

```
a = torch.tensor([[1, 2, 3],
                  [4, 5, 6]])
torch.unbind(a, dim=1)**Output:**
(tensor([1, 4]), tensor([2, 5]), tensor([3, 6]))
```

## 功能七:火炬。张量克隆

`torch.Tensor.clone`返回相同大小和数据类型的张量副本。

当我们使用`x=y`创建张量的副本时，改变一个变量也会影响另一个变量，因为它指向相同的内存位置。

举个例子，

```
a = torch.tensor([[1., 2.],
                  [3., 4.],
                  [5., 6.]])
b = a
a[1,0]=9
b**Output:**
tensor([[1., 2.],
        [9., 4.],
        [5., 6.]])
```

为了避免这种情况，我们可以使用`.clone`方法创建张量的深度副本。

***举例:***

```
a = torch.tensor([[1., 2.],
                  [3., 4.],
                  [5., 6.]])
b = a.clone()
a[1,0]=9
b**Output:**
tensor([[1., 2.],
        [3., 4.],
        [5., 6.]])
```

# 结论

在本文中，我们看到了 PyTorch 中七个可用函数的工作原理。希望这篇文章能帮助你理解这些功能。还有许多其他有用的功能。您可以参考[官方文档](https://pytorch.org/docs/stable/index.html)获取可用功能的完整列表。

## 参考

*   【https://pytorch.org/docs/stable/torch.html】
*   [https://pytorch.org/docs/stable/tensors.html](https://pytorch.org/docs/stable/tensors.html)

## 资源

本文中使用的代码片段可以在我的 GitHub 页面上找到。

## 让我们连接

领英:[https://www.linkedin.com/in/jimit105/](https://www.linkedin.com/in/jimit105/)
GitHub:[https://github.com/jimit105](https://github.com/jimit105)
推特:[https://twitter.com/jimit105](https://twitter.com/jimit105)