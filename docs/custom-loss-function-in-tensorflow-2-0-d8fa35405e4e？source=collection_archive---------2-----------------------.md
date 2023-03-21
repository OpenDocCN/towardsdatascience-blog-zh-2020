# Tensorflow 2.0 中的自定义损失函数

> 原文：<https://towardsdatascience.com/custom-loss-function-in-tensorflow-2-0-d8fa35405e4e?source=collection_archive---------2----------------------->

## 高低位执行亏损。

![](img/0a89e2904d9efa86a6fe971eca3f20a3.png)

2020 年不逃 2.0 版本

当我学习在 Tensorflow (TF)中编写自己的图层时，我遇到的第一个困难是如何编写一个损失函数。TF 包含了几乎所有你需要的损失函数，但有时这还不够。当实现深度强化学习或构建您自己的模型时，您可能需要编写您的损失函数。这正是这篇博文想要阐明的。我们将以两种不同的方式编写损失函数:

1.  对于 tf.keras 模型(高级)
2.  对于定制 TF 型号(低级别)

对于这两种情况，我们将构建一个简单的神经网络来学习数字的平方。网络将接收一个输入，并有一个输出。这个网络绝不是成功或完整的。这是非常初级的，只是为了演示不同的损失函数实现。

# 1.tf.keras 自定义损失(高级别)

我们来看一个高阶损失函数。我们假设已经使用 tf.keras 构建了一个模型。可以通过以下方式实现模型的自定义损失函数:

tf.keras 中的高级损耗实现

首先，自定义损失函数**总是**需要两个参数。第一个是实际值(y_actual)，第二个是通过模型预测的值(y_model)。需要注意的是，这两个都是 **TF 张量**和**而不是** Numpy 数组。在函数内部，你可以随意计算损失，除了返回值必须是一个 TF 标量。

在上面的例子中，我使用 tensor flow . keras . back end . square()计算了平方误差损失。然而，没有必要使用 Keras 后端，任何有效的张量运算都可以。

一旦损失函数被计算出来，我们需要在模型编译调用中传递它。

```
model.compile(loss=custom_loss,optimizer=optimizer)
```

完整的代码可以在这里找到:[链接](https://github.com/sol0invictus/Blog_stuff/blob/master/custom%20loss/high_level_keras.py)

# 2.自定义 TF 损失(低水平)

在前一部分中，我们看了一个 tf.keras 模型。如果我们想在 TF 中从头开始写一个网络，在这种情况下我们会如何实现 loss 函数呢？这将是模型的低级实现。让我们再写一个同样的模型。下面给出了完整的实现，

TF 2.0 中模型的底层实现

乌夫！代码太多了。让我们解开信息。

**__init__()** :构造器构造模型的层(不返回 tf.keras.model。

**run()** :通过将输入手动传递到各层来运行给定输入的模型，并返回最后一层的输出。

**get_loss()** :计算损失并将其作为 TF 张量值返回

到目前为止，实现 loss 似乎很容易，因为我们直接处理模型，但现在我们需要通过自动微分来执行学习。这是在 TF 2.0 中使用 TF 实现的。GradientTape()。函数 **get_grad()** 计算层变量的梯度 wrt。需要注意的是，所有的变量和自变量都是 TF 张量。

**network_learn()** :我们使用从 get_grad()函数获得的梯度在这个函数中应用梯度下降步骤。

写完所有内容后，我们可以如下实现训练循环:

```
x=[1,2,3,4,5,6,7,8,9,10]
x=np.asarray(x,dtype=np.float32).reshape((10,1))
y=[1,4,9,16,25,36,49,64,81,100]
y=np.asarray(y,dtype=np.float32).reshape((10,1))
model=model()
for i in range(100):
    model.network_learn(x,y)
```

完整代码可以在这里找到:[链接](https://github.com/sol0invictus/Blog_stuff/blob/master/custom%20loss/low_level_tf.py)

# 结论

在这篇文章中，我们看到了 TensorFlow 2.0 中自定义损失函数的高级和低级植入。知道如何实现自定义损失函数在强化学习或高级深度学习中是不可或缺的，我希望这篇小帖子能让你更容易实现自己的损失函数。关于自定义损失函数的更多细节，我们请读者参考[张量流文档](https://www.tensorflow.org/guide/keras/train_and_evaluate)。