# 超调 ML 模型时提高效率的 3 个步骤

> 原文：<https://towardsdatascience.com/3-steps-to-improve-your-efficiency-when-hypertuning-ml-models-5a579d57065e?source=collection_archive---------40----------------------->

## 毫不费力地在不同型号之间切换，不会弄乱代码

![](img/c336ba4238a395a35b124ac38a0fc01e.png)

由[马库斯·佩特里茨](https://unsplash.com/@petritz?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 动机

你可能听说过“没有免费的午餐”(NFL)定理，该定理表明对于每一个数据都不存在最佳算法。一种算法可能在一种数据中表现良好，但在其他数据中表现不佳。这就是为什么有这么多的机器学习算法可以用来训练数据。

我们如何知道哪种机器学习模型是最好的？在我们实验和比较不同模型的性能之前，我们无法知道。但是试验不同的模型可能会很麻烦，尤其是当您使用 GridSearchCV 找到模型的最佳参数时。

例如，当我们完成对 RandomForestClassifier 的实验并切换到 SVC 时，我们可能希望保存 RandomForestClassifier 的参数，以备我们想要重现 RandomForestClassifier 的结果。但是我们如何高效的保存这些参数呢？

如果我们将每个型号的信息保存在不同的配置文件中，就像下面这样，不是很好吗？

```
experiments/
├── data_preprocess.yaml
├── hyperparameters.yaml
└── model
    ├── random_forest.yaml
    └── svc.yaml
```

`model`下的每个文件将像这样指定它们的参数

当我们想要使用一个特定的模型(假设是 RandomForestClassifier)时，我们需要做的就是运行训练文件，用`model=modelname`指定我们想要训练的模型

```
python train.py model=random_forest
```

能够做到这一点帮助我更快地试验不同的模型，而不用担心丢失用于 GridSearchCV 的特定模型的超参数。这篇文章将向你展示如何毫不费力地在不同的模型之间切换，就像上面的 Hydra 一样。

# 1:添加九头蛇

Hydra 是一个优雅地配置复杂应用程序的框架。我在这里写了如何使用 hydra 进行数据科学项目。除了配置一个配置文件之外，hydra 还使得处理不同的配置文件变得更加容易。

按照这个教程，简单地[克隆这个 repo](https://github.com/khuyentran1401/Machine-learning-pipeline) 。这是我们项目的树形结构

```
.
├── data
├── experiments
│   ├── data_preprocess.yaml
│   ├── hyperparameters.yaml
│   └── model
│       ├── random_forest.yaml
│       └── svc.yaml
├── predict.py
├── preprocessing.py
├── train_pipeline.py
└── train.py
```

为了访问`experiment`中的配置文件，我们将添加`hydra.main`作为`train.py`中函数的装饰器，如下所示。`config_path`指定配置文件的路径，`config_name`指定配置文件的名称

这就是`hyperparameters.yaml`的样子

YAML 是最容易理解和使用配置文件的语言。如您所见，要访问训练数据，我们只需使用`config.processed_data.text.train`

在`hyperparameters.yaml`中，我们放入了与训练相关的一般信息，如数据路径、GridSearchCV 的得分，而不是特定模型的超参数。我们希望在**根据我们使用的型号**更改型号**的配置**时，保持文件**静态**。

如果我们想用 SVC 开始训练我们的模型，在`hyperparameters.yaml`中，我们用

```
defaults:
   - model: svc
```

# 步骤 2:配置机器学习模型

现在当我们运行`python train.py`时，Hydra 将尝试在目录模型下搜索文件`svc.yaml`

因此，我们的下一步是在模型目录下创建一个名为`svc.yaml`(或者您喜欢的任何其他名称)的文件，如下所示

```
experiments/
├── data_preprocess.yaml
├── hyperparameters.yaml
└── model
    ├── random_forest.yaml
    └── svc.yaml
```

我们的`svc.yaml`文件将包含模型的名称和用于 GridSearchCV 搜索的超参数

现在当我们跑的时候

```
python train.py
```

Hydra 将自动访问模型目录中的`svc.yaml`配置并使用`svc.yaml`中的参数！

如果您想使用 RandomForestClassifier，创建名为`random_forest.yaml`的文件，然后插入关于我们的 RandomForestClassifier 的信息

我们可以在终端中覆盖默认模型，而不是在`hyperparameter.yaml`文件中更改默认模型！

```
python train.py model=random_forest
```

现在我们在`train.py`中的函数可以通过配置访问这些参数。例如，如果我使用`svc`模型，这将是我看到的

```
>>> print(config.model)
SVC>>> print(hyperparameters))
{classifier__C: [.05, .12]classifier__kernel: ['linear', 'poly']classifier__gamma: [0.1, 1]classifier__degree: [0, 1, 2]
}
```

相当酷！

# 最后一步:将配置文件放入 GridSearchCV 的参数中

要将字符串“SVC”转换成一个类，请使用`eval`

```
*from* sklearn.svm *import* SVCclassifier = eval(config.model)()
```

现在分类器像普通的 SVC 类一样工作了！

为了将配置文件中的超参数用于值为 Python list 的 Python 字典中，我们使用

现在，您可以像这样在终端的不同型号之间自由切换

```
python train.py model=random_forest
```

从现在开始，如果您想要使用一个具有不同超参数的新模型，您需要做的就是为该模型添加配置文件，然后运行

```
python train.py model=<newmodel>
```

您的模型无需更改`train.py!`中的代码即可运行

# 但是如果我想在配置文件中使用 Python 函数呢？

有时，我们可能想使用不被 YAML 识别的函数，比如`classifier__C: np.logspace(-4, 4, 20)`，而不是写下一些被 YAML 文件识别的特定值，比如列表`classifier__C: [.05, .12]`

```
model: LogisticRegressionhyperparameters: classifier__penalty: ['l1', 'l2'] classifier__C: np.logspace(-4, 4, 20) classifier__solver: ['qn', 'lbfgs', 'owl']
```

放心吧！我们仍然可以通过在字符串周围包装`eval()`函数来找到解决这种情况的方法！这将把你的字符串`np.logspace(-4, 4, 20)`变成一个真正的 python 函数！

```
*for* key, value *in* param_grid.items(): *if* isinstance(value, str): param_grid[key] = eval(param_grid[key])
```

然后将字典中的所有值转换成 Python 列表，这样它们就是 GridSearchCV 的有效参数

```
param_grid = {ele: (list(param_grid[ele])) *for* ele *in* param_grid}
```

# 结论

恭喜你！您刚刚学习了如何使用 Hydra 来试验不同的机器学习模型。使用这个工具，您可以将代码与数据的特定信息分开。

如果你想改变你的机器学习模型的超参数，你不需要回到你的代码。您只需要为额外的模型再添加一个配置文件，并使用

```
python train.py model=<modelname>
```

下面是使用 hydra.cc 和 config 文件的[示例项目](https://github.com/khuyentran1401/Machine-learning-pipeline)。

我喜欢写一些基本的数据科学概念，并尝试不同的算法和数据科学工具。你可以在 LinkedIn 和 Twitter 上与我联系。

如果你想查看我写的所有文章的代码，请点击这里。在 Medium 上关注我，了解我的最新数据科学文章，例如

[](/how-to-share-your-python-objects-across-different-environments-in-one-line-of-code-f30a25e5f50e) [## 如何在一行代码中跨不同环境共享 Python 对象

### 为建立与他人分享你的发现的环境而感到沮丧？以下是如何让它变得更简单

towardsdatascience.com](/how-to-share-your-python-objects-across-different-environments-in-one-line-of-code-f30a25e5f50e) [](/top-4-code-viewers-for-data-scientist-in-vscode-e275e492350d) [## VSCode 中数据科学家的 4 大代码查看器

### 让 YAML、JSON、CSV 和 Jupyter Notebook 为你工作，而不是与你作对

towardsdatascience.com](/top-4-code-viewers-for-data-scientist-in-vscode-e275e492350d) [](/how-to-create-and-view-interactive-cheatsheets-on-the-command-line-6578641039ff) [## 如何在命令行上创建和查看交互式备忘单

### 停止搜索命令行。用作弊来节省时间

towardsdatascience.com](/how-to-create-and-view-interactive-cheatsheets-on-the-command-line-6578641039ff) [](/how-to-fine-tune-your-machine-learning-models-with-ease-8ca62d1217b1) [## 如何有效地微调你的机器学习模型

### 发现为您的 ML 模型寻找最佳参数非常耗时？用这三招

towardsdatascience.com](/how-to-fine-tune-your-machine-learning-models-with-ease-8ca62d1217b1) [](/introduction-to-ibm-federated-learning-a-collaborative-approach-to-train-ml-models-on-private-data-2b4221c3839) [## IBM 联邦学习简介:一种在私有数据上训练 ML 模型的协作方法

### 在训练来自不同来源的数据时，如何保证数据的安全？

towardsdatascience.com](/introduction-to-ibm-federated-learning-a-collaborative-approach-to-train-ml-models-on-private-data-2b4221c3839)