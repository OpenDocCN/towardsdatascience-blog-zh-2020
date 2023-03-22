# 使用 Keras 调谐器进行超参数调谐

> 原文：<https://towardsdatascience.com/hyperparameter-tuning-with-keras-tuner-283474fbfbe?source=collection_archive---------6----------------------->

## 充分利用您的模型

![](img/2148cc5d55180a9dd54b83a6b5c2c154.png)

在 [Unsplash](https://unsplash.com/s/photos/knobs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由[尹卡·阿迪奥蒂](https://unsplash.com/@willyin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片

> 伟大的数据科学家不会满足于“还行”，他们会超越去实现非凡。

在这篇文章中，我们将回顾数据科学家用来创建模型的技术，这些模型工作良好并赢得竞争。充分利用我们的模型意味着为我们的学习算法选择最佳的超参数。这项任务被称为超参数优化或超参数调整。这在深度学习中尤其费力，因为神经网络充满了超参数。我假设您已经熟悉回归和均方差(MSE)指标等常见的数据科学概念，并且具有使用 tensorflow 和 keras 构建模型的经验。

为了演示超参数调优方法，我们将使用 [keras tuner](https://keras-team.github.io/keras-tuner/) 库来调优波士顿房价数据集上的回归模型。该数据集包含 13 个属性，分别具有 404 个和 102 个训练和测试样本。我们将使用 tensorflow 作为 keras 后端，因此请确保您的计算机上安装了 tensorflow。我用的是 tensorflow 版本' 2.1.0 '和 kerastuner 版本' 1.0.1 '。Tensorflow 2.0.x 附带了 keras，因此，如果您拥有 2.0.x 版本，则无需单独安装 keras。您可以使用以下代码检查您拥有的版本:

```
import tensorflow as tf
import kerastuner as ktprint(tf.__version__)
print(kt.__version__)
```

# 加载数据集

波士顿房价回归数据集可以使用 keras 直接下载。这是 keras 附带的数据集列表。若要加载数据集，请运行以下代码。

```
from tensorflow.keras.datasets import boston_housing(x_train, y_train), (x_test, y_test) = boston_housing.load_data()
```

请注意，如果这是您第一次在 keras 中使用该数据集，它将从外部源下载该数据集。

这是我将在演示中使用的回归模型。下面的代码显示了模型是如何在没有任何调整的情况下构建的。

```
from sklearn.preprocessing import StandardScaler
from tensorflow.keras import models, layers# set random seed
from numpy.random import seed
seed(42)
import tensorflow
tensorflow.random.set_seed(42)# preprocessing - normalization
scaler = StandardScaler()
scaler.fit(x_train)
x_train_scaled = scaler.transform(x_train)
x_test_scaled = scaler.transform(x_test)# model building
model = models.Sequential()
model.add(layers.Dense(8, activation='relu', input_shape=(x_train.shape[1],)))
model.add(layers.Dense(16, activation='relu'))
model.add(layers.Dropout(0.1))
model.add(layers.Dense(1))# compile model using rmsprop
model.compile(optimizer='rmsprop',loss='mse',metrics=['mse'])# model training
history = model.fit(x_train_scaled, y_train, validation_split=0.2, epochs=10)# model evaluation
model.evaluate(x_test_scaled, y_test)
```

该模型的 MSE 约为 434。我已经将 numpy 和 tensorflow 中的随机种子设置为 42，以获得可重复的结果。尽管这样做了，但每次运行代码时，我还是会得到稍微不同的结果。让我在评论中知道我还错过了什么，让这个可重复。

# 使用 Keras 调谐器调谐

要开始在 keras tuner 中调优模型，让我们首先定义一个**超级模型**。 **Hypermodel** 是一个 keras tuner 类，它允许您用可搜索空间定义模型并构建它。

创建一个从 kerastuner 继承的类。超模，像这样:

```
from kerastuner import HyperModelclass RegressionHyperModel(HyperModel):
    def __init__(self, input_shape):
        self.input_shape = input_shape def build(self, hp):
        model = Sequential()
        model.add(
            layers.Dense(
                units=hp.Int('units', 8, 64, 4, default=8),
                activation=hp.Choice(
                    'dense_activation',
                    values=['relu', 'tanh', 'sigmoid'],
                    default='relu'),
                input_shape=input_shape
            )
        )

        model.add(
            layers.Dense(
                units=hp.Int('units', 16, 64, 4, default=16),
                activation=hp.Choice(
                    'dense_activation',
                    values=['relu', 'tanh', 'sigmoid'],
                    default='relu')
            )
        )

        model.add(
            layers.Dropout(
                hp.Float(
                    'dropout',
                    min_value=0.0,
                    max_value=0.1,
                    default=0.005,
                    step=0.01)
            )
        )

        model.add(layers.Dense(1))

        model.compile(
            optimizer='rmsprop',loss='mse',metrics=['mse']
        )

        return model
```

这与我们之前构建的模型相同，只是对于每个超参数，我们定义了一个搜索空间。你可能已经注意到了惠普公司的 hp.Int。浮动，和 hp。Choice，它们用于定义超参数的搜索空间，该超参数分别接受整数、浮点和类别。超参数方法的完整列表可在[这里](https://keras-team.github.io/keras-tuner/documentation/hyperparameters/)找到。“hp”是 Keras Tuner 的超参数类的别名。

超参数如密集层中的单元数接受一个整数，因此，hp.Int 用于定义一个整数范围来尝试。类似地，辍学率接受浮点值，因此 hp。使用了 Float。无论是 hp.Int 还是惠普。Float 需要一个名称、最小值和最大值，而步长和默认值是可选的。

下面的 hp.Int 搜索空间被命名为“单位”，其值为 8 到 64 的 4 的倍数，默认值为 8。惠普。Float 的用法与 hp.Int 类似，但接受浮点值。

```
hp.Int('units', 8, 64, 4, default=8)
```

惠普。Choice 用于定义分类超参数，如激活函数。下面名为“dense_activation”的搜索空间将在“relu”、“tanh”和“sigmoid”函数之间进行选择，默认值设置为“relu”。

```
hp.Choice('dense_activation', values=['relu', 'tanh', 'sigmoid'], default='relu')
```

# 实例化超级模型

让我们实例化一个超级模型对象。输入形状因数据集和您试图解决的问题而异。

```
input_shape = (x_train.shape[1],)
hypermodel = RegressionHyperModel(input_shape)
```

开始调音吧！

# 随机搜索

顾名思义，这种超参数调优方法从给定的搜索空间中随机尝试超参数的组合。要在 keras tuner 中使用这种方法，让我们使用一个可用的调谐器来定义一个调谐器。这里有一个完整的名单[调谐器](https://keras-team.github.io/keras-tuner/documentation/tuners/)。

```
tuner_rs = RandomSearch(
            hypermodel,
            objective='mse',
            seed=42,
            max_trials=10,
            executions_per_trial=2)
```

使用*搜索*方法运行随机搜索调谐器。

```
tuner_rs.search(x_train_scaled, y_train, epochs=10, validation_split=0.2, verbose=0)
```

选择调谐器尝试并评估的最佳超参数组合。

```
best_model = tuner_rs.get_best_models(num_models=1)[0]
loss, mse = best_model.evaluate(x_test_scaled, y_test)
```

随机搜索的 MSE 是 53.48，与根本不执行任何调整相比，这是一个非常大的改进。

# 超波段

Hyperband 基于李等人的算法。al 。通过自适应资源分配和提前停止优化随机搜索方法。Hyperband 首先运行随机超参数配置一次或两次，然后选择表现良好的配置，然后继续调整表现最佳的配置。

```
tuner_hb = Hyperband(
            hypermodel,
            max_epochs=5,
            objective='mse',
            seed=42,
            executions_per_trial=2
        )tuner_hb.search(x_train_scaled, y_train, epochs=10, validation_split=0.2, verbose=0)best_model = tuner_hb.get_best_models(num_models=1)[0]
best_model.evaluate(x_test_scaled, y_test)
```

得到的 MSE 是 395.19，与随机搜索相比要差很多，但比完全不调优要好一点。

# 贝叶斯优化

贝叶斯优化是一种概率模型，将超参数映射到目标函数的概率得分。与随机搜索和超波段模型不同，贝叶斯优化跟踪其过去的评估结果，并使用它来建立概率模型。

```
tuner_bo = BayesianOptimization(
            hypermodel,
            objective='mse',
            max_trials=10,
            seed=42,
            executions_per_trial=2
        )tuner_bo.search(x_train_scaled, y_train, epochs=10, validation_split=0.2, verbose=0)best_model = tuner_bo.get_best_models(num_models=1)[0]
best_model.evaluate(x_test_scaled, y_test)
```

使用贝叶斯优化调整的最佳模型 MSE 是 46.47，比我们尝试的前两个调谐器要好。

# 结论

我们能够证明，实际上，调优帮助我们最大限度地利用我们的模型。这里讨论的只是众多超参数调整方法中的 3 种。当尝试上面的代码时，我们可能会得到稍微不同的结果，出于某种原因，尽管设置了 numpy、tensorflow 和 keras tuner 随机种子，但每次迭代的结果仍然略有不同。笔记本上传在我的 github [repo](https://github.com/cedricconol/keras-tuner-demo) 里。

此外，调谐器也可以调谐！是的，你没看错，调整调谐器。调谐器接受诸如 max_trials 和每次试验的执行次数之类的值，因此也可以进行调谐。尝试更改这些参数，看看是否能获得进一步的改进。

# 参考

[1] F. Chollet，*用 Python 进行深度学习* (2018)，曼宁出版公司。

[2] Keras 调谐器文档，【https://keras-team.github.io/keras-tuner/ 

[3]李，贾米森，德萨沃，罗斯塔米扎德，塔尔沃卡，*超波段:一种基于 Bandit 的超参数优化新方法(2018)，*