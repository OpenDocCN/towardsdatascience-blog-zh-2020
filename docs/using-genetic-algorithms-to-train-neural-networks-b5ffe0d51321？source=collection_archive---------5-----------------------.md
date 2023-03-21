# 使用遗传算法训练神经网络

> 原文：<https://towardsdatascience.com/using-genetic-algorithms-to-train-neural-networks-b5ffe0d51321?source=collection_archive---------5----------------------->

![](img/369f067d6a4a5edc5c318210d5508da6.png)

[图像来源](https://www.pexels.com/photo/low-angle-view-of-spiral-staircase-against-black-background-247676/)

许多人使用遗传算法作为无监督算法，来优化特定环境中的代理，但是没有意识到将神经网络应用到代理中的可能性。

# 什么是遗传算法？

遗传算法是一种学习算法，它使用的思想是，交叉两个好的神经网络的权重，会产生更好的神经网络。

遗传算法如此有效的原因是因为没有直接的优化算法，允许可能有极其不同的结果。此外，他们通常会提出非常有趣的解决方案，这些方案通常会对问题提供有价值的见解。

# 它们是如何工作的？

生成一组随机权重。这是第一个代理的神经网络。在代理上执行一组测试。代理会收到基于测试的分数。重复几次，创建一个群体。选择人口中前 10%的人进行杂交。从前 10%中选择两个随机亲本，并且它们的权重是交叉的。每次交叉发生时，都有很小的变异机会:这是一个不在双亲权重中的随机值。

随着代理慢慢适应环境，这个过程会慢慢优化代理的性能。

# 优点和缺点:

优势:

*   计算不密集

没有线性代数计算要做。唯一需要的机器学习计算是通过神经网络的正向传递。因此，与深度神经网络相比，系统要求非常广泛。

*   适合的

人们可以适应和插入许多不同的测试和方法来操纵遗传算法的灵活性。人们可以在遗传算法中创建一个 GAN，方法是让代理传播生成器网络，测试作为鉴别器。这是一个至关重要的好处，它使我相信遗传算法的使用在未来将会更加广泛。

*   可理解的

对于正常的神经网络来说，算法的学习模式充其量也是神秘的。对于遗传算法来说，很容易理解为什么会发生一些事情:例如，当遗传算法处于井字游戏环境中时，某些可识别的策略会慢慢发展。这是一个很大的好处，因为机器学习的使用是利用技术来帮助我们获得对重要问题的洞察力。

缺点:

*   需要很长一段时间

不幸的交叉和突变可能会对程序的准确性产生负面影响，因此使程序收敛更慢或达到某个损失阈值。

# 代码:

现在你已经对遗传算法及其优势和局限性有了相当全面的了解，我现在可以向你展示这个程序了:

```
import random
import numpy as np
```

这只是这个程序的两个依赖项。这是因为实现的神经网络基础设施是我自己创建的简单版本。要实现更复杂的网络，可以导入 keras 或 tensorflow。

```
class genetic_algorithm:

    def execute(pop_size,generations,threshold,X,y,network):
        class Agent:
            def __init__(self,network):
```

这是“genetic_algorithm”类的创建，它包含了与遗传算法以及它应该如何工作有关的所有函数。主函数是 execute 函数，它将 pop_size、generations、threshold、X，y、network 作为参数。pop_size 是生成种群的大小，generations 是历元的术语，threshold 是您满意的损失值。x 和 y 用于标记数据的遗传算法的应用。对于没有数据或数据未标记的问题，可以删除 X 和 y 的所有实例。网络是神经网络的网络结构。

```
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
```

该脚本描述了每个代理的神经网络的权重初始化和网络传播。

```
def generate_agents(population, network):
            return [Agent(network) for _ in range(population)]
```

该函数创建将被测试的第一个代理群体。

```
def fitness(agents,X,y):
            for agent in agents:
                yhat = agent.neural_network.propagate(X)
                cost = (yhat - y)**2
                agent.fitness = sum(cost)
            return agents
```

因为我使用的例子利用了标记数据。适应度函数仅仅是计算预测的 MSE 或成本函数。

```
def selection(agents):
            agents = sorted(agents, key=lambda agent: agent.fitness, reverse=False)
            print('\n'.join(map(str, agents)))
            agents = agents[:int(0.2 * len(agents))]
            return agents
```

这个函数模仿了进化论中的选择理论:最优秀的生存下来，而其他的则任其自生自灭。在这种情况下，他们的数据会被遗忘，不会被再次使用。

```
def unflatten(flattened,shapes):
            newarray = []
            index = 0
            for shape in shapes:
                size = np.product(shape)
                newarray.append(flattened[index : index + size].reshape(shape))
                index += size
            return newarray
```

为了执行交叉和变异功能，权重需要展平和取消展平为原始形状。

```
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

                split = random.ragendint(0,len(genes1)-1)child1_genes = np.asrray(genes1[0:split].tolist() + genes2[split:].tolist())
                child2_genes = np.array(genes1[0:split].tolist() + genes2[split:].tolist())

                child1.neural_network.weights = unflatten(child1_genes,shapes)
                child2.neural_network.weights = unflatten(child2_genes,shapes)

                offspring.append(child1)
                offspring.append(child2)
            agents.extend(offspring)
            return agents
```

交叉函数是程序中最复杂的函数之一。它产生两个新的“子”代理，它们的权重被替换为两个随机产生的父代理的交叉。这是创建权重的过程:

1.  拉平父母的权重
2.  生成两个分割点
3.  使用分割点作为索引来设置两个子代理的权重

这就是智能体交叉的全过程。

```
def mutation(agents):
            for agent in agents:
                if random.uniform(0.0, 1.0) <= 0.1:
                    weights = agent.neural_network.weights
                    shapes = [a.shape for a in weights]flattened = np.concatenate([a.flatten() for a in weights])
                    randint = random.randint(0,len(flattened)-1)
                    flattened[randint] = np.random.randn()newarray = [a ]
                    indeweights = 0
                    for shape in shapes:
                        size = np.product(shape)
                        newarray.append(flattened[indeweights : indeweights + size].reshape(shape))
                        indeweights += size
                    agent.neural_network.weights = newarray
            return agents 
```

这就是变异函数。展平与交叉功能相同。不是分割点，而是选择一个随机点，用一个随机值替换。

```
for i in range(generations):
            print('Generation',str(i),':')
            agents = generate_agents(pop_size,network)
            agents = fitness(agents,X,y)
            agents = selection(agents)
            agents = crossover(agents,network,pop_size)
            agents = mutation(agents)
            agents = fitness(agents,X,y)

            if any(agent.fitness < threshold for agent in agents):
                print('Threshold met at generation '+str(i)+' !')

            if i % 100:
                clear_output()

        return agents[0]
```

这是 execute 函数的最后一部分，它执行所有已定义的函数。

```
X = np.array([[0, 0, 1], [1, 1, 1], [1, 0, 1], [0, 1, 1]])
y = np.array([[0, 1, 1, 0]]).T
network = [[3,10,sigmoid],[None,1,sigmoid]]
ga = genetic_algorithm
agent = ga.execute(100,5000,0.1,X,y,network)
weights = agent.neural_network.weights
agent.fitness
agent.neural_network.propagate(X)
```

这执行整个遗传算法。对于网络变量，每个嵌套列表保存输入神经元数目、输出神经元数目和激活函数。执行函数返回最佳代理。

# 我的链接:

如果你想看更多我的内容，点击这个 [**链接**](https://linktr.ee/victorsi) 。