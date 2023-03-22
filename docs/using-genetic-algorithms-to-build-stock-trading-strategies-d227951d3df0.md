# 使用遗传算法建立交易策略

> 原文：<https://towardsdatascience.com/using-genetic-algorithms-to-build-stock-trading-strategies-d227951d3df0?source=collection_archive---------8----------------------->

## 展示遗传算法找到创造性解决方案的能力

![](img/04c9bdd92df80bb4675286f8c892f72a.png)

由[马腾·戴克斯](https://unsplash.com/@maartendeckers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/architecture?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

***来自《走向数据科学》编辑的提示:*** *虽然我们允许独立作者根据我们的* [*规则和指导方针*](/questions-96667b06af5) *发表文章，但我们并不认可每个作者的贡献。你不应该在没有寻求专业建议的情况下依赖一个作者的作品。详见我们的* [*读者术语*](/readers-terms-b5d780a700a4) *。*

遗传算法经常被忽略，作为另一种未能有效收敛的无监督学习算法。这是部分正确的，因为遗传算法不使用偏导数，因此训练算法不太直接。然而，遗传算法允许使用传统的基于梯度的优化来制定不可能的解决方案。

为什么会这样呢？如果基于梯度下降的模型从局部最小值开始，它将被永久卡住，因为每一侧上的点的梯度会更高。然而，由于遗传算法更具随机性，它最终会找到解决方案。

此外，神经网络需要具有明确定义的损失函数的标记数据。遗传算法只需要数据，可以巧妙地设计一个定制的损失函数(通常称为遗传算法的适应度函数)来优化某些特征。这种灵活性使得遗传算法从其余的无监督算法中脱颖而出。

# 遗传算法是如何工作的？

遗传算法是对该问题的一种强力攻击形式。唯一的区别是使用进化的概念来加速这个过程。遗传算法是这样工作的:

1.  生成一个代理。它包含一组与神经网络兼容的权重。
2.  计算代理的适应值
3.  重复多次，直到你有一个代理“人口”
4.  按适合度对人群进行排序，适合度最好的代理位于列表顶部。
5.  一起“繁殖”顶级特工。
6.  重复进行，直到达到满意的损失值。

通过改变适应度函数和输入数据，遗传算法可以应用于几乎每个问题！

# 当不应使用遗传算法时:

对于回归和分类等基本任务，使用基于梯度的优化器训练的深度神经网络总是比遗传算法收敛得更快。

遗传算法并不擅长寻找模式，而是有着广阔的视野，去寻找尚未被考虑的解决方案。

# 将遗传算法应用于股票交易:

股票交易是复杂的，因为不同来源的噪音冲淡了数据背后的真实模式。使用遗传算法就像走捷径一样；忽略了模式识别和复杂分析。它只是测试不同的策略，并找到交易证券的最佳策略。如果遗传算法不能找到一个好的解决方案，这只能是一个(或两个！)的这两个原因:

1.  遗传算法没有被训练足够长的时间

遗传算法是一种蛮力算法，需要很长时间来缩小结果范围。这是一个很大的障碍，因为计算能力必须非常高才能克服这个问题

2.损失函数有问题

这个就常见多了。评估网络的损失函数并没有封装损失函数的目标。

# 代码:

**免责声明:**此代码改编自此处[的代码](https://gitlab.com/evolutionary-computation/genetic_algorithm/-/blob/master/ga.py)，使其兼容优化创建交易策略的神经网络。

# 步骤 1|先决条件:

```
import random
import numpy as npdef sigmoid(x):
    return 1/(1+np.exp(-x))
```

这些是项目最基本的依赖项。为了使学习过程更好，您可以为您合适的数据集添加更多的激活函数。

# 第 2 步|代理:

```
class Agent:
            def __init__(self,network):
                class neural_network:
                    def __init__(self,network):
                        self.weights = []
                        self.activations = []
                        for layer in network:
                            if layer[0] != None:
                                input_size = layer[0]
                            else:
                                input_size = network[network.index(layer)-1][1]
                            output_size = layer[1]
                            activation = layer[2]
                            self.weights.append(np.random.randn(input_size,output_size))
                            self.activations.append(activation)
                    def propagate(self,data):
                        input_data = data
                        for i in range(len(self.weights)):
                            z = np.dot(input_data,self.weights[i])
                            a = self.activations[i](z)
                            input_data = a
                        yhat = a
                        return yhat
                self.neural_network = neural_network(network)
                self.fitness = 0
            def __str__(self):
                    return 'Loss: ' + str(self.fitness[0])
```

每个代理都有自己的一组权重，这些权重是根据神经网络设置的架构随机生成的。论证网络是一个列表，看起来像这样:

```
network = [[5,10,sigmoid],[None,1,sigmoid]]
```

列表中的每个术语都描述了神经网络中的一层。每个嵌套列表中的第一项是该层的输入神经元的数量，第二项是输出，第三项是要应用于该层的激活函数。

# 步骤 3|生成代理:

```
def generate_agents(population, network):
            return [Agent(network) for _ in range(population)]
```

这用于生成代理，如运行遗传算法的方法的步骤 1 中所详述的。

# 第 4 步|计算健康度:

```
def fitness(agents,X):
            neutral_range = 0
            for agent in agents:
                profit = 0
                qty = 10
                yhat = agent.neural_network.propagate(X[1:])
                for i in range(len(yhat)):
                    if 0.5 + neutral_range > yhat[i] and yhat[i] > 0.5 -neutral_range:
                        yhat[i] = None
                    elif 1-yhat[i] > yhat[i]-0:
                        yhat[i] = 0
                    elif 1-yhat[i] < yhat[i]-0:
                        yhat[i] = 1

                correct_trades = []
                for i in range(1,len(X)):
                    if X[i][3] > X[i-1][3]:
                        correct_trades.append(1)
                    elif X[i][3] < X[i-1][3]:
                        correct_trades.append(0)
                for i in range(len(yhat)):
                    if yhat[i]:
                        if correct_trades[i] == yhat[i]:
                            profit += abs(X[i+1][3] - X[i][3])*qty
                        elif correct_trades[i] != yhat[i]:
                            profit -= abs(X[i+1][3] - X[i][3])*qty
                agent.fitness = profit
            return agents
```

这个适应度函数是一个交易模拟，它使用过去的数据来计算模型的利润。每个代理通过输出一个 sigmoid 值生成一个交易列表。然后将这些值四舍五入到最接近的值。如果该值为 0，模拟将卖出股票。如果该值为 1，模拟将买入股票。

然后，它通过比较最佳交易和实际交易来计算利润。

# 第 5 步|代理选择:

```
def selection(agents):
            agents = sorted(agents, key=lambda agent: agent.fitness, reverse=False)
            print('\n'.join(map(str, agents)))
            agents = agents[:int(0.2 * len(agents))]
            return agents
```

这段代码本质上是根据代理的适应值对它们进行排序，从最低值到最高值。记住，适应值越低越好。然后，它将代理列表缩短为仅前 20%的代理。

**第六步|代理交叉:**

```
def unflatten(flattened,shapes):
            newarray = []
            index = 0
            for shape in shapes:
                size = np.product(shape)
                newarray.append(flattened[index : index + size].reshape(shape))
                index += size
            return newarray

        def crossover(agents,network,pop_size):
            offspring = []
            for _ in range((pop_size - len(agents)) // 2):
                parent1 = random.choice(agents)
                parent2 = random.choice(agents)
                child1 = Agent(network)
                child2 = Agent(network)

                shapes = [a.shape for a in parent1.neural_network.weights]

                genes1 = np.concatenate([a.flatten() for a in parent1.neural_network.weights])
                genes2 = np.concatenate([a.flatten() for a in parent2.neural_network.weights])

                split = random.randint(0,len(genes1)-1)child1_genes = np.array(genes1[0:split].tolist() + genes2[split:].tolist())
                child2_genes = np.array(genes1[0:split].tolist() + genes2[split:].tolist())

                child1.neural_network.weights = unflatten(child1_genes,shapes)
                child2.neural_network.weights = unflatten(child2_genes,shapes)

                offspring.append(child1)
                offspring.append(child2)
            agents.extend(offspring)
            return agents
```

这是程序中最复杂的部分。在交叉过程中，从选定的代理中随机选择两个父代理。权重的嵌套列表被简化为一个长列表。分裂点是随机选取的。这决定了子对象将由每个父对象的多少权重组成。然后重新格式化该子代的权重，并将该子代添加到代理列表中。

# 第七步|突变:

```
def mutation(agents):
            for agent in agents:
                if random.uniform(0.0, 1.0) <= 0.1:
                    weights = agent.neural_network.weights
                    shapes = [a.shape for a in weights]flattened = np.concatenate([a.flatten() for a in weights])
                    randint = random.randint(0,len(flattened)-1)
                    flattened[randint] = np.random.randn()newarray = []
                    indeweights = 0
                    for shape in shapes:
                        size = np.product(shape)
                        newarray.append(flattened[indeweights : indeweights + size].reshape(shape))
                        indeweights += size
                    agent.neural_network.weights = newarray
            return agents
```

遗传算法在技术上是“盲目的”,因为它们无法洞察由损失函数绘制的表面看起来是什么样子。这意味着遗传算法很容易卡在某个点上。突变允许轻微的、随机的、不频繁的变化来搅动锅，使系统脱离局部最小值。

# 第八步|执行程序:

```
def split_sequences(sequences, n_steps):
    X, y = list(), list()
    for i in range(len(sequences)):
        end_ix = i + n_steps
        if end_ix > len(sequences)-1:
            break
        seq_x, seq_y = sequences[i:end_ix, :], sequences[end_ix, :]
        X.append(seq_x)
        y.append(seq_y)
    return np.array(X), np.array(y)
np.random.seed(0)            
X,y = split_sequences(np.random.randn(1000,1),5)
X = np.reshape(X,(X.shape[0],X.shape[1]))
network = [[5,10,sigmoid],[None,1,sigmoid]]
ga = genetic_algorithm
agent = ga.execute(100,100,10000,X,network)
```

为了执行这个计划，我们需要数据。出于某种原因，SSL 证书不允许我从 yfinance 或 alphavantage 获取财务数据。我还不能运行这个程序，所以我只是使用随机 numpy 数组作为数据。

如果你想使用你自己的财务数据，这个概念就是用一组收盘价数据把它分成 n 个大小的块。

为了再现性，这个程序将设置自己的种子，使随机性固定。这是为了减少随机性，允许调整参数，如人口规模，时代和神经网络架构。这里更有趣的参数是输入网络的每个数据块的长度。调整这个参数可以找到程序运行的最佳时间。

# 结论:

正如我在自己的许多其他文章中所说的，这仅仅是用这个概念可以做什么的框架。你可以做无数的事情来改进我的程序。以下是其中的几个例子:

1.  记录过去代理的生成

通过记录过去代理的生成，它可以防止程序重新生成以前已经生成的代理。

2.将该程序链接到一个纸交易账户

羊驼是使用纸面交易组合的明显选择。它可以快速进行交易，便于分析程序的性能。

3.创建更多贸易类型

这是三个中最难的。添加不同的交易，如止损和买入卖出交易，允许更微妙和分层的交易策略，将不同的交易类型相互叠加，以实现利润最大化。

# 我的链接:

如果你想看更多我的内容，点击这个 [**链接**](https://linktr.ee/victorsi) 。