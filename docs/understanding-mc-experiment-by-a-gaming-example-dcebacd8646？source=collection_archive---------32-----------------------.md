# 通过一个游戏实例理解 MC 实验。

> 原文：<https://towardsdatascience.com/understanding-mc-experiment-by-a-gaming-example-dcebacd8646?source=collection_archive---------32----------------------->

![](img/133a36ac0cb87ff32354b1501a2ab1dd.png)

鸣谢:来自 Dota 2 的截图

我们经常会在项目中遇到不确定性，在这种情况下，我们需要对不同的成功机会进行评估。解决这种情况的一个非常好的方法是使用从潜在的不确定概率中得出的结果的重复模拟。这种技术被称为蒙特卡罗方法(或简称为 MC)，它是以摩纳哥的蒙特卡洛赌场命名的，斯坦尼斯瓦夫·乌拉姆的叔叔在那里赌博。斯坦尼斯劳·乌兰和约翰·冯·努曼在秘密的曼哈顿计划中研究了这些方法。

这些方法的目的是为未知的确定性值找到一个数值近似值。这些方法强调在大规模试验中使用重复抽样，并使用 CLT 等工具逼近结果，这些工具将保证样本均值遵循正态分布，而不管随机过程的基本分布如何。

从观察者的角度来看，进行这样的实验大致需要以下步骤:

a.建立一个分布或概率向量，从中得出结果

b.多次重复得出结果的实验

c.如果结果是一个数字标量，则结果的平均值应按照 CLT 正态分布。

d.估计平均值、标准偏差，并构建预期结果的置信区间。

尽管通过阅读上面的内容看起来很简单，但问题的关键是编写适当的函数和代码来完成上面的步骤。如果潜在的随机变量是一个已知的常见分布，或者你需要使用一个随机发生器来选择一个结果，这个问题就简化了。

下面使用游戏 Dota 2 的例子来解释该方法，问题被定义如下:

“在 Dota 2 游戏中，玩家可以选择从宝藏中购买物品。这些物品通常是特定 Dota 2 英雄或另一个 NPC 的皮肤。在每个宝藏中，有两类物品，普通物品和稀有物品。玩家将购买宝藏的钥匙，然后旋转轮盘，轮盘将从常规物品或稀有物品中随机选择一个物品。每个物品的赔率是不相等的，稀有物品有不同的赔率，赔率随着每次旋转而减少，而普通物品的赔率是恒定的。

假设有 9 个常规物品和一个稀有物品，它们的赔率由赔率向量给出。每次旋转的费用是 175 英镑。在第 40 轮，获得稀有物品的几率是 1，这意味着它肯定会以 40 * 175 = 7000 的最高成本被选中。

odds _ to _ get _ rare _ item =[2000.0 583.0 187.0 88.0 51.0 33.0 23.0 17.0 13.1 10.4 8.5 7.1 6.0 5.2 4.5 4.0 3.6 3.2 2 2.9 2.6 2.4 2.2 1 1.9 1.8 1

我们如何估计收到稀有物品的预期成本？"

解决这个问题的方法如下:

a.定义一个模拟实验的函数，如下所示(用 Python 语言):

```
def simulate_cost_of_getting_rare_item(no_of_items,prob_to_get_rare_item,cost_of_each_round):
```

这个函数将接受。项目作为输入，赔率向量获得稀有项目，在我们的情况下是一个和每次旋转的成本。这个函数的输出将是所有旋转的总费用，直到找到稀有物品。

b.定义保存累积成本和计算旋转或迭代次数的变量。

```
 cumulative_cost = 0 
   iteration = 1
```

然后我们有

c.在最初几次旋转中，必须至少选择一次常规物品，而不能得到一个重复的常规物品，直到所有常规物品都被选择。因此，您可以将模拟逻辑分为两部分，一部分用于初始回合，另一部分用于后续回合。

```
# for the first few picks; ensure that regular items are picked without repetition. This is achived by restricting the prob_items array with only availabe items subtracting the # already selected items in previous iterations from available items unless rare item is found 
   while iteration < no_of_items: 
      cumulative_cost += 175 
      prob_items = []
```

从概率向量中取出稀有物品的概率，将其从 1 中减去，并将剩余的概率平均分配给被遗漏的常规物品。根据计算的概率，使用随机方法(来自 random.choices())比较选择的项目。检查在物品概率向量中最后一个位置的是不是稀有物品。

```
 left_out_prob = 1 - prob_to_get_rare_item[iteration-1] 
      prob_items = [left_out_prob/(no_of_items - iteration) for i in
                    range(no_of_items-iteration)]
      prob_items.append(prob_to_get_rare_item[iteration-1]) 
      item_chosen = choices([i for i in range(no_of_items-
                    iteration+1)],prob_items) 
      if item_chosen[0] == len(prob_items)-1: 
         return iteration, prob_items, item_chosen[0], 
                cumulative_cost
   iteration += 1
```

d.一旦所有常规物品至少被选择一次，它们将在随后的回合中再次进入战斗(即，如果稀有物品在前 9 回合中没有被选择，则从第 10 回合开始)，因此在随后的每次旋转中，要选择的物品数量将保持为 no_of_items。

```
 # once all regular items are picked atleast once, then can repeat
   #  but since the decreasing odds of rare item, their probabilities
   #  for any of them getting selected over rare item is minimized 
   while True: 
      cumulative_cost += 175 
      prob_items = [] 
      left_out_prob = 1 - prob_to_get_rare_item[iteration-1] 
      prob_items = [left_out_prob/(no_of_items - 1) for i in
                    range(no_of_items-1)]       
      prob_items.append(prob_to_get_rare_item[iteration-1]) 
      item_chosen = choices([i for i in
                            range(no_of_items)],prob_items) 
      if item_chosen[0] == len(prob_items)-1: 
         return iteration, prob_items, item_chosen[0], 
                cumulative_cost
   iteration += 1
```

e.函数定义完成后，在 100 * 1000 次试验中重复采样该函数，并计算预期成本

```
# now perform monte carlo simulations over 100 * 1000 trails 
mean_cost = [] 
for outer_trials in range(100): 
   cost_vec = [] 
   for inner_trials in range(1000): 
      iteration, prob_items, pick, cost =   
             simulate_cost_of_getting_rare_item
             (no_of_items,prob_to_get_rare_item,cost_of_each_round)    
   cost_vec.append(cost)
   mean_cost.append(sum(cost_vec)/len(cost_vec))
```

使用上面的技巧，我们可以估计得到一个稀有物品的预期成本大约是 2250。

azure 笔记本中提供了完整的代码:

 [## 微软 Azure 笔记本电脑

### 提供对运行在微软 Azure 云上的 Jupyter 笔记本的免费在线访问。

notebooks.azure.com](https://notebooks.azure.com/spaturu/projects/dota-cost-of-rare-item)