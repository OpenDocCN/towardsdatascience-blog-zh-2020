# 自动编码器直观指南:理论、代码和可视化

> 原文：<https://towardsdatascience.com/an-intuitive-guide-to-auto-encoders-theory-code-and-visualization-3cc2a6a30d2c?source=collection_archive---------49----------------------->

![](img/b4d0b28ffbe454aba46b26a81df99af2.png)

照片由[像素](https://www.pexels.com/photo/ball-ball-shaped-blur-bubble-302743/)上的 [pixabay](https://www.pexels.com/@pixabay) 拍摄

自动编码器是最有用的非监督算法之一，可以洞察数据，从而优化训练算法的学习算法。

# 什么是自动编码器？

自动编码器是一种神经网络，它将数据作为输入，将数据作为输出。起初，这可能看起来很荒谬:一个数和它本身之间的关系仅仅是 1，为什么神经网络是必要的？这是真的，但是自动编码器已经创造了一种方法来绕过它。

利用瓶颈。瓶颈，从其常见的定义来看，意味着玻璃瓶颈部的狭窄缝隙。在这种情况下，瓶颈是指减少隐藏层中神经元的数量。因此，这就迫使数据只能由更少数量的神经元来表示。默认情况下，网络将学习压缩和解压缩数据的方法，将数据编码和解码为更大和更小的数据表示。

# 为什么自动编码器如此有用？

在一个数据集高达万亿字节的世界里，我们需要找到一种方法来有效地减少数据占用的内存。此外，处理信息的计算能力有限。由自动编码器创建的较小的表示变得如此重要，因为它占用的数据少得多。

此外，通过我将向你展示的可视化技术，你实际上可以可视化每个神经元在寻找什么。

# 代码:

既然自动编码器的理论已经很清楚了，我们可以继续看代码了。对于实际的自动编码器，我将使用 Keras，但是对于每一层所寻找的可视化，我将使用我自己的神经网络框架。

```
from keras.layers import Dense
from keras.models import Sequential
import requests
import json
```

除了标准的 keras 层和模型，我还将使用 requests 和 json，通过 alpha-vantage API 访问财务数据。

```
API_KEY = 'XXXXXXX'
from_symbol = 'EUR'
to_symbol = 'USD'close_price = [
r = requests.get(
        '[https://www.alphavantage.co/query?function=FX_INTRADAY&from_symbol='](https://www.alphavantage.co/query?function=FX_INTRADAY&from_symbol=') +
        from_symbol + '&to_symbol=' + to_symbol +
        '&interval=1min&outputsize=full&apikey=' + API_KEY)
jsondata = json.loads(r.content)
pre_data = list(jsondata['Time Series FX (1min)'].values())
fx_data = []
for data in pre_data: 
    fx_data.append(list(data.values()))
fx_data.reverse()
for term in fx_data:
    close_price.append(float(term[-1]))
```

这是我用来从 alpha-vantage 访问数据集的脚本。然而，要使用这个脚本，将 API 密钥更改为一个有效的密钥，您可以在这里得到。为了简单起见，脚本也只选择收盘价。

```
model = Sequential() 
model.add(Dense(10,input_shape = (None,10),activation = 'relu'))
model.add(Dense(1))
model.compile(optimizer = 'adam', loss = 'mse')
model.fit(close_price,close_price,epochs = 100)
```

这个脚本创建了模型，只是使用密集层。你可以使用 KL-Divergence，一种防止模型仅仅记忆输入数据的方法。它通过激活神经元的数量来惩罚网络，从而减少表示数据所需的神经元数量。

这是完整的程序！简单吧？

# 可视化:

自动编码器的主要优势是能够可视化每个神经元在寻找什么。首先，将这个框架复制到你的程序中。

```
import numpy as np
from matplotlib import pyplot as pltdef sigmoid(x):
    return 1/(1+np.exp(-x))def sigmoid_p(x):
    return sigmoid(x)*(1 -sigmoid(x))def relu(x):
    return np.maximum(x, 0)def relu_p(x):
    return np.heaviside(x, 0)def tanh(x):
    return np.tanh(x)def tanh_p(x):
    return 1.0 - np.tanh(x)**2def flatten(itr):
    t = tuple()
    for e in itr:
        try:
            t += flatten(e)
        except:
            t += (e,)
    return tdef deriv_func(z,function):
    if function == sigmoid:
        return sigmoid_p(z)
    elif function == relu:
        return relu_p(z)
    elif function == tanh:
        return tanh_p(z)class NeuralNetwork:
    def __init__(self):
        self.layers = []
        self.weights = []
        self.loss = []
    def add(self,layer_function):
        self.layers.append(layer_function)

    def initialize_weights(self):
        for layer in self.layers:
            index = self.layers.index(layer)
            weights = layer.initialize_weights(self.layers,index)
            self.weights.append(weights)

    def propagate(self,X):
        As,Zs = [],[]
        input_data = X
        for layer in self.layers:
            a,z = layer.propagate(input_data)
            As.append(a)
            Zs.append(z)
            input_data = a
        return As,Zs

    def train(self,X,y,iterations):
        loss = []
        for i in range(iterations):
            As,Zs = self.propagate(X)
            loss.append(sum(np.square(flatten(y - As[-1]))))
            As.insert(0,X)
            g_wm = [0] * len(self.layers)
            for i in range(len(g_wm)):
                pre_req = (y-As[-1])*2
                a_1 = As[-(i+2)]
                z_index = -1
                w_index = -1
                if i == 0:
                    range_value = 1
                else:
                    range_value = 2*i
                for j in range(range_value):
                    if j% 2 == 0:
                        pre_req = pre_req * sigmoid_p(Zs[z_index])
                        z_index -= 1
                    else:
                        pre_req = np.dot(pre_req,self.weights[w_index].T)
                        w_index -= 1
                gradient = np.dot(a_1.T,pre_req)
                g_wm[-(i+1)] = gradient
                for i in range(len(self.layers)):
                    self.layers[i].network_train(g_wm[i])

        return loss

    class Perceptron:
        def __init__(self,nodes,input_shape= None,activation = None):
            self.nodes = nodes
            self.input_shape = input_shape
            self.activation = activation
        def initialize_weights(self,layers,index):
            if self.input_shape:
                self.weights = np.random.randn(self.input_shape[-1],self.nodes)
            else:
                self.weights = np.random.randn(layers[index-1].weights.shape[-1],self.nodes)
            return self.weights
        def propagate(self,input_data):
            z = np.dot(input_data,self.weights)
            if self.activation:
                a = self.activation(z)
            else:
                a = z
            return a,z
        def network_train(self,gradient):
            self.weights += gradient

model = NeuralNetwork()Perceptron = model.Perceptron
```

我不会深入讨论框架的代码，但它本质上是 Keras 的透明版本，可以给出每一层的传播值。

```
X = np.array([[1, 1, 1, 1, 0],
   [1, 1, 0, 0, 1],
   [0, 0, 0, 1, 0],
   [1, 0, 0, 0, 0],
   [0, 0, 1, 1, 1],
   [1, 1, 0, 0, 0],
   [1, 0, 0, 1, 0],
   [1, 0, 1, 1, 0],
   [0, 0, 0, 0, 0],
   [1, 1, 0, 0, 1]])model.add(Perceptron(2,input_shape = (None,5),activation = sigmoid))
model.add(Perceptron(5,activation = sigmoid))model.initialize_weights()
loss = model.train(X,X,1000)
```

这个自动编码器仅用 MSE 训练了 1000 个时期。

```
As,Zs = model.propagate(X)
full_set = []
for a in range(len(As)):
    a_set_values = []
    for z in range(5):
        zeroes =np.zeros(5)
        zeroes[z] = 1
        As,Zs = model.propagate(zeroes)
        a_set_values.append(As[a])
    full_set.append(a_set_values)
full_setimport matplotlib.pyplot as plt
import numpy as npplt.subplot(212)
for sets in full_set:
    plt.imshow(sets, cmap='Greys',  interpolation='nearest')
    plt.show()full_set
```

然后，用数据传播网络。由于这里的数据很简单，我们遍历数据中的每一项，将其更改为 1，并查看它如何更改数据的每一层。然后我们绘制它的黑白图像，看看效果

谢谢你看我的文章！

# 我的链接:

如果你想看更多我的内容，点击这个 [**链接**](https://linktr.ee/victorsi) 。