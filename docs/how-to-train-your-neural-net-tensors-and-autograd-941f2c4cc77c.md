# PyTorch【基础】—张量和亲笔签名

> 原文：<https://towardsdatascience.com/how-to-train-your-neural-net-tensors-and-autograd-941f2c4cc77c?source=collection_archive---------21----------------------->

![](img/840b4e62b667c78ce2f768c8b11e4488.png)

如何训练你的神经网络[图片[0]]

## [如何训练你的神经网络](https://towardsdatascience.com/tagged/akshaj-wields-pytorch)

## 这篇博文将带您了解一些最常用的张量运算，并演示 PyTorch 中的自动签名功能。

随着 PyTorch 的大肆宣传，我决定冒险在 2019 年底学习它。开始有点困难，因为没有太多适合初学者的教程(在我看来)。有一些博客文章只是浏览了一个模型实现，其中到处都有一些深度学习理论，或者有一个巨大的 Github repo，其中有几个 python 脚本，实现了一个具有多个依赖关系的超级复杂的模型。

在这两种情况下，你都会想——“*呃……什么？”*

![](img/0f17cfb5668e72875b66f09aa4f98ee0.png)

安德烈·卡帕西为 PyTorch 发的推文[图片[1]]

在使用 PyTorch 一段时间后，我发现它是最好的深度学习框架。因此，在 2020 年，我决定每两周(希望是:P)发表一篇博文，介绍我在 **PyTorch 1.0+** 中在时间序列预测、NLP 和计算机视觉领域实现的一些东西。

> 这 4 大类是——py torch[基础]、py torch[表格]、PyTorch[NLP]和 py torch[远景]。

在这篇博文中，我们将实现一些最常用的张量运算，并谈一谈 PyTorch 中的自动签名功能。

# 导入库

```
import numpy as np

import torch
from torch.autograd import grad
```

# 张量运算

## 创建一个单位化张量

长张量。

```
x = torch.LongTensor(3, 4)
print(x) ################## OUTPUT ##################tensor([[140124855070912, 140124855070984, 140124855071056, 140124855071128],
        [140124855071200, 140124855071272, 140124855068720, 140125080614480],
        [140125080521392, 140124855066736, 140124855066800, 140124855066864]])
```

浮点张量。

```
x = torch.FloatTensor(3, 4)
print(x) ################## OUTPUT ##################tensor([[1.7288e+25, 4.5717e-41, 1.7288e+25, 4.5717e-41],
        [0.0000e+00, 0.0000e+00, 0.0000e+00, 0.0000e+00],
        [0.0000e+00, 2.7523e+23, 1.8788e+31, 1.7220e+22]])
```

## 手动创建张量

```
x1 = torch.tensor([[1., 2., 3.], [4., 5., 6.]])
x1 ################## OUTPUT ##################tensor([[1., 2., 3.],
        [4., 5., 6.]])
```

## 从列表中创建张量

```
py_list = [[1, 2, 3], [4, 5, 6]]
print('List: \n', py_list, '\n')print('Tensor:')
print(torch.tensor(py_list)) ################## OUTPUT ##################List: 
 [[1, 2, 3], [4, 5, 6]] Tensor:
tensor([[1, 2, 3],
        [4, 5, 6]])
```

## 从 numpy 数组创建张量

```
numpy_array = np.random.random((3, 4)).astype(float)
print('Float numpy array: \n', numpy_array, '\n')print('Float Tensor:')
print(torch.FloatTensor(numpy_array)) ################## OUTPUT ##################Float numpy array: 
 [[0.03161911 0.52214984 0.06134578 0.03143846]
 [0.72253513 0.1396692  0.14399402 0.02879052]
 [0.01618331 0.41498778 0.60040201 0.95173069]] Float Tensor:
tensor([[0.0316, 0.5221, 0.0613, 0.0314],
        [0.7225, 0.1397, 0.1440, 0.0288],
        [0.0162, 0.4150, 0.6004, 0.9517]])
```

## 将张量转换为 numpy 数组

```
print('Tensor:')
print(x1)print('\nNumpy array:')
print(x1.numpy()) ################## OUTPUT ##################Tensor:
tensor([[1., 2., 3.],
        [4., 5., 6.]])Numpy array:
[[1\. 2\. 3.]
 [4\. 5\. 6.]]
```

## 在一个范围内创建张量

长型张量。

```
# Create tensor from 0 to 10.
torch.arange(10, dtype=torch.long) ################## OUTPUT ##################tensor([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

浮点型张量。

```
torch.arange(10, dtype=torch.float) ################## OUTPUT ##################tensor([0., 1., 2., 3., 4., 5., 6., 7., 8., 9.])
```

## 用随机值创建张量

```
torch.randn(3, 4) ################## OUTPUT ##################tensor([[ 0.6797,  1.3238, -0.5602, -0.6977],
        [ 1.1281, -0.8198, -0.2968, -0.1166],
        [ 0.8521, -1.0367,  0.5664, -0.7052]])
```

只有正数。

```
torch.rand(3, 4) ################## OUTPUT ##################tensor([[0.8505, 0.8817, 0.2612, 0.8152],
        [0.6652, 0.3061, 0.0246, 0.4880],
        [0.7158, 0.6929, 0.4464, 0.0923]])
```

## 创建张量作为单位矩阵

```
torch.eye(3, 3) ################## OUTPUT ##################tensor([[1., 0., 0.],
        [0., 1., 0.],
        [0., 0., 1.]])
```

## 创建全零张量

```
torch.zeros(3, 4) ################## OUTPUT ##################tensor([[0., 0., 0., 0.],
        [0., 0., 0., 0.],
        [0., 0., 0., 0.]])
```

## 创建全 1 张量

```
torch.ones(3, 4) ################## OUTPUT ##################tensor([[1., 1., 1., 1.],
        [1., 1., 1., 1.],
        [1., 1., 1., 1.]])
```

## 索引张量

```
print(x1, '\n')
print(f"Tensor at x1[0,0] = {x1[0, 0]}")
print(f"Tensor at x1[1,2] = {x1[1, 2]}")
print(f"0th column of x1 = {x1[:, 0]}")
print(f"1st row of x1 = {x1[1, :]}") ################## OUTPUT ##################tensor([[1., 2., 3.],
        [4., 5., 6.]]) Tensor at x1[0,0] = 1.0
Tensor at x1[1,2] = 6.0
0th column of x1 = tensor([1., 4.])
1st row of x1 = tensor([4., 5., 6.])
```

## 张量的形状

```
print(x1, '\n')
print(x1.shape) ################## OUTPUT ##################tensor([[1., 2., 3.],
        [4., 5., 6.]]) torch.Size([2, 3])
```

## 重塑张量

创建一个张量。

```
x2 = torch.arange(10, dtype=torch.float)
x2 ################## OUTPUT ##################tensor([0., 1., 2., 3., 4., 5., 6., 7., 8., 9.])
```

使用`.view`重塑张量。

```
x2.view(2, 5) ################## OUTPUT ##################tensor([[0., 1., 2., 3., 4.],
        [5., 6., 7., 8., 9.]])
```

`-1`根据张量的大小自动识别维度。

```
x2.view(5, -1) ################## OUTPUT ##################tensor([[0., 1.],
        [2., 3.],
        [4., 5.],
        [6., 7.],
        [8., 9.]])
```

使用`.reshape`整形。

```
x2.reshape(5, -1) ################## OUTPUT ##################tensor([[0., 1.],
        [2., 3.],
        [4., 5.],
        [6., 7.],
        [8., 9.]])
```

## 改变张量轴

`view`和`permute`略有不同。`view`改变张量的顺序，而`permute`仅改变轴。

```
print("x1: \n", x1)
print("\nx1.shape: \n", x1.shape)
print("\nx1.view(3, -1): \n", x1.view(3, -1))
print("\nx1.permute(1, 0): \n", x1.permute(1, 0)) ################## OUTPUT ##################x1: 
 tensor([[1., 2., 3.],
        [4., 5., 6.]])x1.shape: 
 torch.Size([2, 3])x1.view(3, -1): 
 tensor([[1., 2.],
        [3., 4.],
        [5., 6.]])x1.permute(1, 0): 
 tensor([[1., 4.],
        [2., 5.],
        [3., 6.]])
```

## 获取张量的内容

```
print(x2)
print(x2[3])
print(x2[3].item()) ################## OUTPUT ##################tensor([0., 1., 2., 3., 4., 5., 6., 7., 8., 9.])
tensor(3.)
3.0
```

## 得到张量的平均值

```
print(x1)
torch.mean(x1) ################## OUTPUT ##################tensor([[1., 2., 3.],
        [4., 5., 6.]])tensor(3.5000)
```

## 获取张量中值的和

```
print(x1)
torch.sum(x1) ################## OUTPUT ##################tensor([[1., 2., 3.],
        [4., 5., 6.]])tensor(21.)
```

## 获取张量的 e^x 值

```
print(x1)
torch.exp(x1) ################## OUTPUT ##################tensor([[1., 2., 3.],
        [4., 5., 6.]])tensor([[  2.7183,   7.3891,  20.0855],
        [ 54.5981, 148.4132, 403.4288]])
```

## 张量算术运算

```
x3 = torch.randn(5, 2)
x4 = torch.randn(5, 2)print(f"x3 + x4 = \n{x3 + x4}\n")
print(f"x3 - x4 = \n{x3 - x4}\n")
print(f"x3 * x4  (element wise)= \n{x3 * x4}\n")
# You can also do [x3 @ x4.t()] for matrix multiplication.
print(f"x3 @ x4  (matrix mul)= \n{torch.matmul(x3, x4.t())}\n") ################## OUTPUT ##################x3 + x4 = 
tensor([[-0.1989,  1.9295],
        [-0.1405, -0.8919],
        [-0.6190, -3.6546],
        [-1.4263, -0.1889],
        [ 0.7664, -0.6130]])x3 - x4 = 
tensor([[ 1.5613, -1.8650],
        [-2.2483, -1.9581],
        [ 0.4915,  0.6334],
        [ 2.0189, -2.8248],
        [-2.5310, -4.7337]])x3 * x4  (element wise)= 
tensor([[-0.5995,  0.0612],
        [-1.2588, -0.7597],
        [ 0.0354,  3.2387],
        [-0.5104, -1.9860],
        [-1.4547, -5.5080]])x3 @ x4  (matrix mul)= 
tensor([[-0.5384,  0.7351, -0.4474, -1.1310,  1.1895],
        [-1.6525, -2.0185,  3.7184,  0.1793, -4.9052],
        [-2.8099, -0.8725,  3.2741, -1.8812, -3.2174],
        [-3.1196, -0.4
```

## 连接张量

连接行。

```
x5, x6 = torch.randn(4, 7), torch.randn(3, 7)
print('X5:', x5.shape)
print(x5)
print('\nX6:', x6.shape)
print(x6)print('\nConcatenated tensor', torch.cat((x5, x6)).shape)
print(torch.cat((x5, x6))) ################## OUTPUT ##################X5: torch.Size([4, 7])
tensor([[ 0.9009, -0.7907,  0.2602, -1.4544,  1.0479, -1.1085,  0.9261],
        [ 0.2041, -1.2046, -0.8984,  1.6531,  0.2567, -0.0466,  0.1195],
        [-0.7890, -0.0156,  0.2190, -1.5073, -0.2212,  0.4541,  0.7874],
        [ 0.7968, -0.1711, -1.0618, -0.1209, -0.4825, -0.7380, -2.1153]])X6: torch.Size([3, 7])
tensor([[-1.1890,  0.1310,  1.7379,  1.8666,  1.4759,  1.9887,  1.1943],
        [ 0.1609, -0.3116,  0.5274,  1.3037, -1.2190, -1.6068,  0.5382],
        [ 1.0021,  0.5119,  1.7237, -0.3709, -0.1801, -0.3868, -1.0468]])Concatenated tensor torch.Size([7, 7])
tensor([[ 0.9009, -0.7907,  0.2602, -1.4544,  1.0479, -1.1085,  0.9261],
        [ 0.2041, -1.2046, -0.8984,  1.6531,  0.2567, -0.0466,  0.1195],
        [-0.7890, -0.0156,  0.2190, -1.5073, -0.2212,  0.4541,  0.7874],
        [ 0.7968, -0.1711, -1.0618, -0.1209, -0.4825, -0.7380, -2.1153],
        [-1.1890,  0.1310,  1.7379,  1.8666,  1.4759,  1.9887,  1.1943],
        [ 0.1609, -0.3116,  0.5274,  1.3037, -1.2190, -1.6068,  0.5382],
        [ 1.0021,  0.5119,  1.7237, -0.3709, -0.1801, -0.3868, -1.0468]])
```

串联列。

```
x7, x8 = torch.randn(3, 3), torch.randn(3, 5)
print('X7:', x7.shape)
print(x7)
print('\nX8:', x8.shape)
print(x8)print('\nConcatenated tensor', torch.cat((x7, x8), 1).shape)
print(torch.cat((x7, x7), 1)) ################## OUTPUT ##################X7: torch.Size([3, 3])
tensor([[-1.1606,  1.5264,  1.4718],
        [ 0.9691, -0.1215, -0.1852],
        [-0.5792, -1.2848, -0.8485]])X8: torch.Size([3, 5])
tensor([[ 0.3518, -0.2296,  0.5546,  1.5093,  1.6241],
        [ 0.8275, -1.1022, -0.1441, -0.3518,  0.4338],
        [-0.1501, -1.1083,  0.3815, -0.5076,  0.5819]])Concatenated tensor torch.Size([3, 8])
tensor([[-1.1606,  1.5264,  1.4718, -1.1606,  1.5264,  1.4718],
        [ 0.9691, -0.1215, -0.1852,  0.9691, -0.1215, -0.1852],
        [-0.5792, -1.2848, -0.8485, -0.5792, -1.2848, -0.8485]])
```

## 查找张量中的最小值/最大值

```
print('Tensor x1:')
print(x1)print('\nMax value in the tensor:')
print(torch.max(x1))print('\nIndex of the max value in the tensor')
print(torch.argmax(x1))print('\nMin value in the tensor:')
print(torch.min(x1))print('\nIndex of the min value in the tensor')
print(torch.argmin(x1)) ################## OUTPUT ##################Tensor x1:
tensor([[1., 2., 3.],
        [4., 5., 6.]]) Max value in the tensor:
tensor(6.) Index of the max value in the tensor
tensor(5) Min value in the tensor:
tensor(1.) Index of the min value in the tensor
tensor(0)
```

## 从张量中删除维度

`torch.squeeze()`从张量中删除所有一维部分。

```
x9 = torch.zeros(2, 1, 3, 1, 5)
print('Tensor:')
print(x9)print('\nTensor shape:')
print(x9.shape)print('\nTensor after torch.squeeze():')
print(x9.squeeze())print('\nTensor shape after torch.squeeze():')
print(x9.squeeze().shape) ################## OUTPUT ##################Tensor:
tensor([[[[[0., 0., 0., 0., 0.]], [[0., 0., 0., 0., 0.]], [[0., 0., 0., 0., 0.]]]], [[[[0., 0., 0., 0., 0.]], [[0., 0., 0., 0., 0.]], [[0., 0., 0., 0., 0.]]]]])Tensor shape:
torch.Size([2, 1, 3, 1, 5])Tensor after torch.squeeze():
tensor([[[0., 0., 0., 0., 0.],
         [0., 0., 0., 0., 0.],
         [0., 0., 0., 0., 0.]], [[0., 0., 0., 0., 0.],
         [0., 0., 0., 0., 0.],
         [0., 0., 0., 0., 0.]]])Tensor shape after torch.squeeze():
torch.Size([2, 3, 5])
```

另一个例子来阐明它是如何工作的。

```
x10 = torch.zeros(3, 1)
print('Tensor:')
print(x10)print('\nTensor shape:')
print(x10.shape)print('\nTensor shape after torch.squeeze(0):')
print(x10.squeeze(0).shape)print('\nTensor shape after torch.squeeze(1):')
print(x10.squeeze(1).shape) ################## OUTPUT ##################Tensor:
tensor([[0.],
        [0.],
        [0.]])Tensor shape:
torch.Size([3, 1])Tensor shape after torch.squeeze(0):
torch.Size([3, 1])Tensor shape after torch.squeeze(1):
torch.Size([3])
```

## 使用自动签名的反向传播

如果`requires_grad=True`，张量对象保持跟踪它是如何被创建的。

```
x = torch.tensor([1., 2., 3.], requires_grad = True)
print('x: ', x)y = torch.tensor([10., 20., 30.], requires_grad = True)
print('y: ', y)z = x + y 
print('\nz = x + y')
print('z:', z) ################## OUTPUT ##################x:  tensor([1., 2., 3.], requires_grad=True)
y:  tensor([10., 20., 30.], requires_grad=True)z = x + y
z: tensor([11., 22., 33.], grad_fn=<AddBackward0>)
```

既然，`requires_grad=True`，`z`知道它是由两个张量`z = x + y`相加而产生的。

```
s = z.sum()
print(s) ################## OUTPUT ##################tensor(66., grad_fn=<SumBackward0>)
```

`s`知道它是由它的数字之和创造出来的。当我们在`s`上调用`.backward()`时，backprop 从`s`开始运行。然后我们可以计算梯度。

```
s.backward()
print('x.grad: ', x.grad)
print('y.grad: ', y.grad) ################## OUTPUT ##################x.grad:  tensor([1., 1., 1.])
y.grad:  tensor([1., 1., 1.])
```

# 亲笔签名示例

> 我在这里引用了一些很好的资源来研究 PyTorch 上的亲笔签名
> 
> [奥斯陆大学的幻灯片](https://www.uio.no/studier/emner/matnat/ifi/IN5400/v19/material/week4/IN5400_2019_week4_intro_to_pytorch.pdf#page=39&zoom=100,-87,505)
> 
> [埃利奥特·瓦特——YouTube](https://www.youtube.com/watch?v=MswxJw-8PvE)

```
x1 = torch.tensor(2, dtype = torch.float32, requires_grad = True)
x2 = torch.tensor(3, dtype = torch.float32, requires_grad = True)
x3 = torch.tensor(1, dtype = torch.float32, requires_grad = True)
x4 = torch.tensor(4, dtype = torch.float32, requires_grad = True)z1 = x1 * x2 
z2 = x3 * x4
f = z1 + z2gradients = grad(outputs=f, inputs = [x1, x2, x3, x4, z1, z2])
gradients ################## OUTPUT ##################(tensor(3.), tensor(2.), tensor(4.), tensor(1.), tensor(1.), tensor(1.))
```

让我们打印出所有的梯度。

```
print(f"Gradient of x1 = {gradients[0]}")
print(f"Gradient of x2 = {gradients[1]}")
print(f"Gradient of x3 = {gradients[2]}")
print(f"Gradient of x4 = {gradients[3]}")
print(f"Gradient of z1 = {gradients[4]}")
print(f"Gradient of z2 = {gradients[5]}") ################## OUTPUT ##################Gradient of x1 = 3.0
Gradient of x2 = 2.0
Gradient of x3 = 4.0
Gradient of x4 = 1.0
Gradient of z1 = 1.0
Gradient of z2 = 1.0
```

**叶张量**是直接创建的张量，不是任何算术运算的结果。
在上面的例子中， *x1，x2，x3，x4* 是叶张量，而 *z1* 和 *z2* 不是。

我们可以使用`tensor.backward()`自动计算所有的梯度，而不是使用`grad(outputs=f, inputs = [x1, x2, x3, x4, z1, z2])`指定所有的输入来计算梯度。

```
x1 = torch.tensor(2, dtype = torch.float32, requires_grad = True)
x2 = torch.tensor(3, dtype = torch.float32, requires_grad = True)
x3 = torch.tensor(1, dtype = torch.float32, requires_grad = True)
x4 = torch.tensor(4, dtype = torch.float32, requires_grad = True)z1 = x1 * x2 
z2 = x3 * x4
f = z1 + z2f.backward()print(f"Gradient of x1 = {x1.grad}")
print(f"Gradient of x2 = {x2.grad}")
print(f"Gradient of x3 = {x3.grad}")
print(f"Gradient of x4 = {x4.grad}")
print(f"Gradient of z1 = {z1.grad}")
print(f"Gradient of z2 = {z2.grad}") ################## OUTPUT ##################Gradient of x1 = 3.0
Gradient of x2 = 2.0
Gradient of x3 = 4.0
Gradient of x4 = 1.0
Gradient of z1 = None
Gradient of z2 = None
```

我们可以使用`x.grad.zero_`使渐变为 0。

```
print(x1.grad.zero_())################## OUTPUT ##################tensor(0.)
```

感谢您的阅读。欢迎提出建议和建设性的批评。:)你可以在 [LinkedIn](https://www.linkedin.com/in/akshajverma7/) 和 [Twitter](https://twitter.com/theairbend3r) *找到我。*

你也可以在这里查看我的其他博客文章。

[![](img/041a0c7464198414e6ce355f9235099e.png)](https://www.buymeacoffee.com/theairbend3r)