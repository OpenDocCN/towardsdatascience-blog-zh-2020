# LSTMs 直观指南

> 原文：<https://towardsdatascience.com/an-intuitive-guide-to-lstms-e26f06273ad3?source=collection_archive---------46----------------------->

## 从零开始创造它们

![](img/a21768ce11a787d26e544c9d96705034.png)

布伦特·德·兰特在 [Unsplash](https://unsplash.com/s/photos/building?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

LSTMs 是机器学习中最重要的突破之一；赋予机器学习算法回忆过去信息的能力，允许实现时间模式。理解一个概念比从头开始创造一个概念更好吗？

# 什么是 LSTMs？

## 宽泛:

LSTM 代表长期短期记忆，表示其利用过去信息进行预测的能力。LSTM 背后的机制非常简单。LSTMs 具有来自不同时间步长的不同处理信息源作为网络的输入，而不是用于数据传播的单一前馈过程，因此能够访问数据内与时间相关的模式。

LSTM 架构不仅仅由一个神经网络组成，而是由至少三个同时训练的神经网络组成的电池。此外，LSTM 体系结构还包含一些门，这些门为神经网络的最终预测赋予某些数据更高的权重。

## 详细信息:

以下是 LSTM 传播一条输入数据所采取的步骤的详细列表:

1.  输入数据提供给三个不同的神经网络，即遗忘网络、选择网络和细胞状态网络
2.  输入数据通过这些神经网络传播，给出输出
3.  适当的激活函数用于每个输出，以防止消失或爆炸梯度。
4.  对于遗忘门，数据然后与从上一时间步收集的可能性交叉相乘
5.  将为收集的可能性交叉相乘的值存储为收集的可能性
6.  将该值乘以选择神经网络的预测，以给出 LSTM 的预测

这是一个复杂的过程，但理解每一步是至关重要的，以便正确地应用导数。

# 代码:

现在，您已经很好地理解了 LSTM 应该如何工作(理论上)，我们可以继续实际的程序。

# 代码:

**第一步|设置:**

```
import numpy 
from matplotlib import pyplot as plt
def sigmoid(x):
    return 1/(1+np.exp(-x))def sigmoid_p(x):
    return sigmoid(x)*(1 -sigmoid(x))def relu(x):
    return np.maximum(x, 0)def relu_p(x):
    return np.heaviside(x, 0)def tanh(x):
    return np.tanh(x)def tanh_p(x):
    return 1.0 - np.tanh(x)**2def deriv_func(z,function):
    if function == sigmoid:
        return sigmoid_p(z)
    elif function == relu:
        return relu_p(z)
    elif function == tanh:
        return tanh_p(z)
```

在这段代码中，我安装了程序的唯一依赖项，Numpy 用于矩阵乘法和数组操作，不同的激活函数和它们的导数用于神经网络的灵活性

**第二步| LSTM 框架:**

```
class LSTM:
    def __init__(self,network):

        def plus_gate(x,y):
            return np.array(x) + np.array(y)

        def multiply_gate(x,y):
            return np.array(x) * np.array(y)

        class NeuralNetwork:
            def __init__(self,network):
                self.weights = []
                self.activations = []
                for layer in network:
                    input_size = layer[0]
                    output_size = layer[1]
                    activation = layer[2]
                    index = network.index(layer)
                    if layer[3] == 'RNN':
                        increment = network[-1][1]
                    else:
                        increment = 0
                    self.weights.append(np.random.randn(input_size+increment,output_size))
                    self.activations.append(activation)
```

在这一部分，我初始化了 LSTM 类，并定义了其中的关键组件。两个门和神经网络组件的框架。

**第三步|神经网络功能:**

```
def propagate(self,data):
                input_data = data
                Zs = []
                As = []
                for i in range(len(self.weights)):
                    z = np.dot(input_data,self.weights[i])
                    if self.activations[i]:
                        a = self.activations[i](z)
                    else:
                        a = z
                    As.append(a)
                    Zs.append(z)
                    input_data = a
                return As,Zs

            def network_train(self, As,Zs,learning_rate,input_data,extended_gradient):
                As.insert(0,input_data)
                g_wm = [0] * len(self.weights)
                for z in range(len(g_wm)):
                    a_1 = As[z].T
                    pre_req = extended_gradient
                    z_index = 0
                    weight_index = 0for i in range(0,z*-1 + len(network)):
                        if i % 2 == 0:
                            z_index -= 1
                            if self.activations[z]:
                                pre_req = pre_req * deriv_func(Zs[z_index],self.activations[z])
                            else:
                                pre_req = pre_req * Zs[z_index]
                        else:
                            weight_index -= 1
                            pre_req = np.dot(pre_req,self.weights[weight_index].T)
                    a_1 = np.reshape(a_1,(a_1.shape[0],1))
                    pre_req = np.reshape(pre_req,(pre_req.shape[0],1))
                    pre_req = np.dot(a_1,pre_req.T)
                    g_wm[z] = pre_req
                for i in range(len(self.weights)):
                    self.weights[i] += g_wm[i]*learning_rate
```

该部分包含网络的定义，以传播数据并计算每个相应权重的梯度，能够引入外部梯度以将权重直接链接到损失函数。

**步骤 4|创建组件:**

```
self.plus_gate = plus_gate
self.multiply_gate = multiply_gate
self.recurrent_nn = NeuralNetwork(network)
self.forget_nn = NeuralNetwork(network)
self.select_nn = NeuralNetwork(network)
```

有了这个框架，我只需要创建 LSTM 运行所需的每个组件。

**步骤 5|定义结构:**

```
def cell_state(self,input_data,memo,select):
        global rnn_As,rnn_Zs
        rnn_As,rnn_Zs = lstm.recurrent_nn.propagate(input_data)
        yhat_plus = tanh(rnn_As[-1])
        plus = self.plus_gate(yhat_plus,memo)
        collect_poss = plus
        yhat_mult = tanh(plus)
        mult = self.multiply_gate(yhat_mult,select)
        pred = mult
        return pred,collect_poss

def forget_gate(self,input_data,colposs):
        global forget_As,forget_Zs
        forget_As,forget_Zs = lstm.forget_nn.propagate(input_data)
        yhat_mult = sigmoid(forget_As[-1])
        mult = self.multiply_gate(colposs,yhat_mult)
        memo = mult
        return memodef select_gate(self,input_data):
        global select_As,select_Zs
        select_As,select_Zs = lstm.select_nn.propagate(input_data)
        yhat_mult = sigmoid(select_As[-1])
        select = yhat_mult
        return select
```

在上面的部分中，我定义了网络的三个主要架构元素，添加了全局变量以便于在培训期间访问。

**步骤 6|定义传播:**

```
def propagate(self,X,network):
        colposs = 1
        As = []
        for i in range(len(X)):
            input_data = X[i]
            if i == 0:
                increment = network[-1][1]
                input_data = list(input_data) + [0 for _ in range(increment)]
            else:
                input_data = list(input_data) + list(pred)
            input_data = np.array(input_data)
            memory = self.forget_gate(input_data,colposs)
            select = self.select_gate(input_data)
            pred,colposs = self.cell_state(input_data,memory,select)
            As.append(pred)
        return As
```

现在，它就像浏览图表并按正确的顺序排列组件一样简单。比较图表和代码:这会让你更好地理解 LSTMs 是如何工作的。

**第七步|训练:**

```
def train(self,X,y,network,iterations,learning_rate):
        colposs = 1
        loss_record = []
        for _ in range(iterations):
            for i in range(len(X)):
                input_data = X[i]
                if i == 0:
                    increment = network[-1][1]
                    input_data = list(input_data) + [0 for _ in range(increment)]
                else:
                    input_data = list(input_data) + list(pred)
                input_data = np.array(input_data)
                memory = self.forget_gate(input_data,colposs)
                select = self.select_gate(input_data)
                pred,colposs = self.cell_state(input_data,memory,select)loss = sum(np.square(y[i]-pred).flatten())gloss_pred = (y[i]-pred)*2
                gpred_gcolposs = selectgpred_select = colposs
                gloss_select = gloss_pred * gpred_selectgpred_forget = select*sigmoid_p(colposs)*colposs
                gloss_forget = gloss_pred * gpred_forgetgpred_rnn = select*sigmoid_p(colposs)
                gloss_rnn = gloss_pred*gpred_rnnself.recurrent_nn.network_train(rnn_As,rnn_Zs,learning_rate,input_data,gloss_rnn)
                self.forget_nn.network_train(forget_As,forget_Zs,learning_rate,input_data,gloss_forget)
                self.select_nn.network_train(select_As,select_Zs,learning_rate,input_data,gloss_select)
            As = self.propagate(X,network)
            loss = sum(np.square(y[i]-pred))
            loss_record.append(loss)
        return loss_record
```

继续前进！我已经检查了 96%的代码。解释每一个导数要花很长时间，所以我简单解释一下。画一条从最终预测到你想要计算偏导数的项的路径，把所有垂直于路径的项加到方程中。逐元素相乘。使用这些作为扩展梯度来训练嵌套神经网络内的权重。

**第八步|运行程序:**

```
X = np.array([[0,1,1],[1,1,0],[1,0,1]])
y = np.array([0,1,1])
network = [[3,5,sigmoid,'RNN'],[5,5,sigmoid,'Dense'],[5,1,sigmoid,'Dense']]
lstm = LSTM(network)
loss_record = lstm.train(X,y,network,5000,0.1)
plt.plot(loss_record)
```

该脚本的最后一段应该使用嵌套神经网络执行程序，以具有在网络变量下定义的架构。您可以在这里更改数字，但要确保矩阵是对齐的。

**完整源代码:**

```
import numpy 
from matplotlib import pyplot as pltdef sigmoid(x):
    return 1/(1+np.exp(-x))def sigmoid_p(x):
    return sigmoid(x)*(1 -sigmoid(x))def relu(x):
    return np.maximum(x, 0)def relu_p(x):
    return np.heaviside(x, 0)def tanh(x):
    return np.tanh(x)def tanh_p(x):
    return 1.0 - np.tanh(x)**2def deriv_func(z,function):
    if function == sigmoid:
        return sigmoid_p(z)
    elif function == relu:
        return relu_p(z)
    elif function == tanh:
        return tanh_p(z)class LSTM:
    def __init__(self,network):

        def plus_gate(x,y):
            return np.array(x) + np.array(y)

        def multiply_gate(x,y):
            return np.array(x) * np.array(y)

        class NeuralNetwork:
            def __init__(self,network):
                self.weights = []
                self.activations = []
                for layer in network:
                    input_size = layer[0]
                    output_size = layer[1]
                    activation = layer[2]
                    index = network.index(layer)
                    if layer[3] == 'RNN':
                        increment = network[-1][1]
                    else:
                        increment = 0
                    self.weights.append(np.random.randn(input_size+increment,output_size))
                    self.activations.append(activation)def propagate(self,data):
                input_data = data
                Zs = []
                As = []
                for i in range(len(self.weights)):
                    z = np.dot(input_data,self.weights[i])
                    if self.activations[i]:
                        a = self.activations[i](z)
                    else:
                        a = z
                    As.append(a)
                    Zs.append(z)
                    input_data = a
                return As,Zs

            def network_train(self, As,Zs,learning_rate,input_data,extended_gradient):
                As.insert(0,input_data)
                g_wm = [0] * len(self.weights)
                for z in range(len(g_wm)):
                    a_1 = As[z].T
                    pre_req = extended_gradient
                    z_index = 0
                    weight_index = 0for i in range(0,z*-1 + len(network)):
                        if i % 2 == 0:
                            z_index -= 1
                            if self.activations[z]:
                                pre_req = pre_req * deriv_func(Zs[z_index],self.activations[z])
                            else:
                                pre_req = pre_req * Zs[z_index]
                        else:
                            weight_index -= 1
                            pre_req = np.dot(pre_req,self.weights[weight_index].T)
                    a_1 = np.reshape(a_1,(a_1.shape[0],1))
                    pre_req = np.reshape(pre_req,(pre_req.shape[0],1))
                    pre_req = np.dot(a_1,pre_req.T)
                    g_wm[z] = pre_req
                for i in range(len(self.weights)):
                    self.weights[i] += g_wm[i]*learning_rate

        self.plus_gate = plus_gate
        self.multiply_gate = multiply_gate
        self.recurrent_nn = NeuralNetwork(network)
        self.forget_nn = NeuralNetwork(network)
        self.select_nn = NeuralNetwork(network)

    def cell_state(self,input_data,memo,select):
            global rnn_As,rnn_Zs
            rnn_As,rnn_Zs = lstm.recurrent_nn.propagate(input_data)
            yhat_plus = tanh(rnn_As[-1])
            plus = self.plus_gate(yhat_plus,memo)
            collect_poss = plus
            yhat_mult = tanh(plus)
            mult = self.multiply_gate(yhat_mult,select)
            pred = mult
            return pred,collect_poss

    def forget_gate(self,input_data,colposs):
        global forget_As,forget_Zs
        forget_As,forget_Zs = lstm.forget_nn.propagate(input_data)
        yhat_mult = sigmoid(forget_As[-1])
        mult = self.multiply_gate(colposs,yhat_mult)
        memo = mult
        return memodef select_gate(self,input_data):
        global select_As,select_Zs
        select_As,select_Zs = lstm.select_nn.propagate(input_data)
        yhat_mult = sigmoid(select_As[-1])
        select = yhat_mult
        return select

    def propagate(self,X,network):
        colposs = 1
        As = []
        for i in range(len(X)):
            input_data = X[i]
            if i == 0:
                increment = network[-1][1]
                input_data = list(input_data) + [0 for _ in range(increment)]
            else:
                input_data = list(input_data) + list(pred)
            input_data = np.array(input_data)
            memory = self.forget_gate(input_data,colposs)
            select = self.select_gate(input_data)
            pred,colposs = self.cell_state(input_data,memory,select)
            As.append(pred)
        return As

    def train(self,X,y,network,iterations,learning_rate):
        colposs = 1
        loss_record = []
        for _ in range(iterations):
            for i in range(len(X)):
                input_data = X[i]
                if i == 0:
                    increment = network[-1][1]
                    input_data = list(input_data) + [0 for _ in range(increment)]
                else:
                    input_data = list(input_data) + list(pred)
                input_data = np.array(input_data)
                memory = self.forget_gate(input_data,colposs)
                select = self.select_gate(input_data)
                pred,colposs = self.cell_state(input_data,memory,select)loss = sum(np.square(y[i]-pred).flatten())gloss_pred = (y[i]-pred)*2
                gpred_gcolposs = selectgpred_select = colposs
                gloss_select = gloss_pred * gpred_selectgpred_forget = select*sigmoid_p(colposs)*colposs
                gloss_forget = gloss_pred * gpred_forgetgpred_rnn = select*sigmoid_p(colposs)
                gloss_rnn = gloss_pred*gpred_rnnself.recurrent_nn.network_train(rnn_As,rnn_Zs,learning_rate,input_data,gloss_rnn)
                self.forget_nn.network_train(forget_As,forget_Zs,learning_rate,input_data,gloss_forget)
                self.select_nn.network_train(select_As,select_Zs,learning_rate,input_data,gloss_select)
            As = self.propagate(X,network)
            loss = sum(np.square(y[i]-pred))
            loss_record.append(loss)
        return loss_record   

X = np.array([[0,1,1],[1,1,0],[1,0,1]])
y = np.array([0,1,1])
network = [[3,5,sigmoid,'RNN'],[5,5,sigmoid,'Dense'],[5,1,sigmoid,'Dense']]
lstm = LSTM(network)
loss_record = lstm.train(X,y,network,5000,0.1)
plt.plot(loss_record)
```

# 我的链接:

如果你想看更多我的内容，点击这个 [**链接**](https://linktr.ee/victorsi) 。