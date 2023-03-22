# 用于快速并行强化学习的 Ray 和 RLlib

> 原文：<https://towardsdatascience.com/ray-and-rllib-for-fast-and-parallel-reinforcement-learning-6d31ee21c96c?source=collection_archive---------11----------------------->

## 使用 Ray 进行 RL 训练的介绍教程

![](img/d501b823eda02714793b7c125a17c336.png)

照片由 [Jean Gerber](https://unsplash.com/@the_gerbs1?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Ray 不仅仅是一个用于多处理的库；Ray 的真正能力来自 RLlib 和 Tune 库，它们利用这种能力进行强化学习。它使您能够将培训扩展到大规模分布式服务器，或者只是利用并行化属性，使用您自己的笔记本电脑更高效地进行培训。选择权在你。

# TL；速度三角形定位法(dead reckoning)

我们展示了如何使用 Ray 和 RLlib 来训练一个定制的强化学习环境，这个环境是在 OpenAI Gym 的基础上构建的。

# 一个温和的 RLlib 教程

一旦你用`pip install ray[rllib]`安装了 Ray 和 RLlib，你就可以用命令行中的一个命令训练你的第一个 RL 代理了:

`rllib train --run=A2C --env=CartPole-v0`

这将告诉你的计算机使用`CartPole`环境使用[优势演员评论家算法(A2C)](https://www.datahubbs.com/policy-gradients-and-advantage-actor-critic/) 进行训练。A2C 和许多其他算法已经内置到库中，这意味着你不必担心自己实现这些算法的细节。

这真的很棒，特别是如果你想用标准的环境和算法来训练的话。然而，如果你想做得更多，你必须挖掘得更深一点。

# RLlib 代理

您可以通过`ray.rllib.agents`访问各种算法。在这里，您可以找到 PyTorch 和 Tensorflow 中不同实现的[长列表](https://github.com/ray-project/ray/tree/master/rllib/agents),并开始使用。

这些都是使用算法的训练器方法访问的。例如，如果您想要使用如上所示的 A2C，您可以运行:

```
import ray
from ray.rllib import agentsray.init()
trainer = agents.a3c.A2CTrainer(env='CartPole-v0')
```

如果您想尝试 DQN，您可以拨打:

```
trainer = agents.dqn.DQNTrainer(env='CartPole-v0') # Deep Q Network
```

所有算法都遵循相同的基本结构，从小写 algo 缩写到大写 algo 缩写，后跟“Trainer”

更改超参数就像将配置字典传递给`config`参数一样简单。查看可用内容的一个快速方法是调用`trainer.config`来打印出适用于您选择的算法的选项。一些例子包括:

*   `fcnet_hiddens`控制隐藏单元和隐藏层的数量(作为一个名为`model`的字典传递给`config`，然后是一个列表，下面我会给出一个例子)。
*   `vf_share_layers`确定您是否拥有[一个具有多个输出头的神经网络](https://www.datahubbs.com/two-headed-a2c-network-in-pytorch/)或独立的价值和策略网络。
*   `num_workers`设置并行化的处理器数量。
*   `num_gpus`设置您将使用的 GPU 数量。

从网络(通常位于`model`字典中)到各种回调和多代理设置，还有许多其他的需要设置和定制。

# 示例:为`CartPole`培训 PPO

我想转而展示一个快速的例子来让你开始，并向你展示这是如何在一个标准的开放的健身房环境中工作的。

选择您的 IDE 或文本编辑器，并尝试以下操作:

```
import ray
from ray.rllib import agents
ray.init() # Skip or set to ignore if already called
config = {'gamma': 0.9,
          'lr': 1e-2,
          'num_workers': 4,
          'train_batch_size': 1000,
          'model': {
              'fcnet_hiddens': [128, 128]
          }}
trainer = agents.ppo.PPOTrainer(env='CartPole-v0', config=config)
results = trainer.train()
```

`config`字典更改了上述值的默认值。您可以看到我们如何通过在`config`字典中嵌套一个名为`model`的字典来影响网络的层数和节点数。一旦我们指定了我们的配置，在我们的`trainer`对象上调用`train()`方法将会把环境发送给工人并开始收集数据。一旦收集到足够的数据(根据我们上面的设置，有 1000 个样本)，模型将更新并将输出发送到一个名为`results`的新字典。

如果您想要运行多个更新，那么您可以设置一个训练循环来连续调用`train()`方法，以达到给定的迭代次数，或者直到达到某个其他阈值。

# 定制您的 RL 环境

OpenAI Gym 和它的所有扩展都很棒，但是如果你正在寻找 RL 的新颖应用或者在你的公司使用它，你将需要一个定制的环境。

不幸的是，当前版本的 Ray (0.9) [明确声明](https://ray.readthedocs.io/en/latest/rllib-env.html)与健身房注册表不兼容。幸运的是，整合一个助手函数来让定制的健身房环境与 Ray 一起工作并不太困难。

让我们假设您有一个名为`MyEnv-v0`的环境，它已经被正确注册，这样您就可以像调用任何其他健身房环境一样使用`gym.make('MyEnv-v0')`来调用它(如果您还没有，您可以在这里查看我关于设置环境的[逐步过程](https://www.datahubbs.com/building-custom-gym-environments-for-rl/))。

要从 Ray 调用定制环境，您需要将它封装在一个函数中，该函数将返回环境类，*而不是*一个实例化的对象。[我发现做这件事的最好方法](https://stackoverflow.com/questions/58551029/rllib-use-custom-registered-environments/60792871#60792871)是使用一个`create_env()`助手函数:

```
def env_creator(env_name):
    if env_name == 'MyEnv-v0':
        from custom_gym.envs.custom_env import CustomEnv0 as env
    elif env_name == 'MyEnv-v1':
        from custom_gym.envs.custom_env import CustomEnv1 as env
    else:
        raise NotImplementedError
    return env
```

从这里，您可以设置您的代理，并在这个新环境中对它进行训练，只需对`trainer`稍加修改。

```
env_name = 'MyEnv-v0'
config = {
    # Whatever config settings you'd like...
    }
trainer = agents.ppo.PPOTrainer(
    env=env_creator(env_name), 
    config=config)
max_training_episodes = 10000
while True:
    results = trainer.train()
    # Enter whatever stopping criterion you like
    if results['episodes_total'] >= max_training_episodes:
        break
print('Mean Rewards:\t{:.1f}'.format(results['episode_reward_mean']))
```

注意，在上面，我们用`env_creator`来称呼环境，其他一切保持不变。

# 使用自定义环境的提示

如果您习惯于从环境到网络和算法构建自己的模型，那么在使用 Ray 时，您需要了解一些特性。

首先，Ray 遵循 [OpenAI Gym API](http://gym.openai.com/docs/) ，这意味着您的环境需要有`step()`和`reset()`方法，以及精心指定的`observation_space`和`action_space`属性。对于最后两个，我总是有点懒惰，因为我可以简单地定义我的网络输入和输出维度，而不必考虑输入值的范围，例如，`gym.spaces`方法所要求的。Ray 检查所有的输入以确保它们都在指定的范围内(我花了太多时间调试运行，才意识到我的`gym.spaces.Box`上的`low`值被设置为 0，但是环境返回了-1e-17 数量级的值并导致它崩溃)。

当建立你的行动和观察空间时，坚持`Box`、`Discrete`和`Tuple`。`MultiDiscrete`和`MultiBinary`不工作([目前为](https://github.com/ray-project/ray/issues/6372))，将导致运行崩溃。相反，在`Tuple`函数中换行`Box`或`Discrete`空格。

尽可能利用定制预处理。Ray 对您的状态输入进行假设，这通常很好，但它也使您能够定制预处理步骤，这可能有助于您的训练。

# 超越 RLlib

Ray 可以大大加快训练速度，并使深度强化学习的开始变得容易得多。RLlib 并不是最终的结果(我们在这里只是触及了其功能的表面)，它有一个强大的表亲，称为 Tune，它使您能够调整模型的超参数，并为您管理所有重要的数据收集和后端工作。请务必回来查看如何将此库引入您的工作流程的更新。