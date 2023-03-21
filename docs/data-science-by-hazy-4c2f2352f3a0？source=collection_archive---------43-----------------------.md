# 优化深度学习神经网络

> 原文：<https://towardsdatascience.com/data-science-by-hazy-4c2f2352f3a0?source=collection_archive---------43----------------------->

## Hazy 的数据科学

## 深度学习神经网络有一系列令人眼花缭乱的元参数。了解如何将 GANs 应用于神经网络优化。

![](img/1cb4937b9bbdbb0c29921691bbbb4a11.png)

来源: [WOCinTech](https://www.flickr.com/photos/wocintechchat/) ，知识共享

道格拉斯·亚当斯在《银河系漫游指南》中曾断言:“空间很大。你不会相信它有多么巨大，令人难以置信的大。我的意思是，你可能认为去药店的路很长，但那对太空来说只是微不足道。”

神经网络也很大。T2 微软图灵自然语言生成或 T-NLG 网络有大约 190 亿个参数。以大多数人的标准来看，这已经很大了。

然而，我们关心的不仅仅是参数的数量，还有元参数。研究生成对抗网络(GAN)的数据科学家通常必须运行数百万次实验，以优化他们的神经网络。在这篇文章中，我们解释了在 Hazy，我们如何将自动元参数优化注入到我们的 GANs 中，然后让您在较少挫折的情况下训练更好的模型。

## 调整元参数:成为机器学习工程师的尝试

深度学习神经网络有一系列令人眼花缭乱的元参数。

元参数是提供给网络的参数，用于指导网络的训练过程，控制网络如何修改参数。

元参数包括学习速率、动量、隐藏层的数量、每层神经元的数量以及要使用的优化器的类型。

机器学习工程师工作的一个标准部分是调整网络，也就是选择产生最佳性能的元参数。这可能是一个耗时的过程，因为网络越大，收敛的时间就越长，并且通常需要调整和调整的元参数就越多。

调整网络很像一个科学家，他有一台复杂的设备，有许多旋钮要旋转，要进行大量的实验，看哪一个能给你最好的结果。

幸运的是，有一些软件包可以让这项任务变得更容易。

## 应用于 GANs 的 Optuna 优化

在合成数据生成公司 [Hazy](https://www.hazy.com) ，我们是 [Optuna Python 包](https://optuna.readthedocs.io/en/latest/)的忠实粉丝。

Optuna 是一个自动元参数优化软件框架，专门为机器学习而设计。该代码是高度模块化和强制性的。它支持并行，分布式优化，动态修剪试验。它与机器学习框架无关，并且高度可定制。

我们举个例子。假设我们有一个 GAN，我们希望优化它的生成器和鉴别器的学习速率。我们该怎么做呢？

## 成为神经网络的考验

Optuna 有两个基本概念:研究和试验。

该研究是优化的总体任务，基于返回优化结果的函数。这个函数通常被称为目标函数。试验是目标函数的一次执行。

让我们来看一个例子，这个例子取自 Optuna 网站，并应用于 Hazy 的 GAN 模型，让您了解什么是可能的。

首先，我们定义一个目标函数来研究。

目标函数封装了整个训练过程，并返回元参数的这个特定实例的值。

```
def objective(trial):
    iris = sklearn.datasets.load_iris()

    n_estimators = trial.suggest_int('n_estimators', 2, 20)
    max_depth = int(trial.suggest_loguniform('max_depth', 1, 32))

    clf = sklearn.ensemble.RandomForestClassifier(
        n_estimators=n_estimators, max_depth=max_depth)

    return sklearn.model_selection.cross_val_score(
        clf, iris.data, iris.target, n_jobs=-1, cv=3).mean()
```

该函数加载了[虹膜数据集](https://scikit-learn.org/stable/auto_examples/datasets/plot_iris_dataset.html) —一个众所周知的用于评估机器学习分类器的数据集。然后，它从试验对象获得建议的估计数和最大深度。然后它实例化一个[随机森林分类器](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)并返回分数。

让我们更详细地讨论一下。

数据集由三种不同种类的虹膜组成，机器学习**任务**是在给定四个测量值的情况下，将给定的数据点正确地分配给正确的虹膜种类:

*   萼片长度
*   萼片宽度
*   花瓣长度
*   花瓣宽度

Iris 数据集被认为是一个相当困难的分类问题，因为就这些测量而言，物种之间有相当多的重叠。很难在物种之间划出一个清晰的界限。

我们希望优化的元参数是**n _ estimates**和 **max_depth** 。

[试验对象](https://optuna.readthedocs.io/en/latest/reference/trial.html)为 **n_estimators** 建议一个 2≤𝑛≤20 范围内的整数:

```
n_estimators = trial.suggest_int('n_estimators', 2, 20)
```

Optuna 有许多不同的机制来提供元参数的值，以便在每次试验中进行测试；使用对数均匀分布分配**最大深度**的值:

```
max_depth = int(**trial**.suggest_loguniform('max_depth', 1, 32))
```

[**suggest _ log uniform**](https://optuna.readthedocs.io/en/latest/reference/trial.html#optuna.trial.Trial.suggest_loguniform)函数接受一个范围，在本例中为 1≤ 𝑥 ≤32，并返回该范围内的浮点值。这被转换为整数。

然后创建随机森林分类器，具有建议的**最大深度**和 **n 估计器**。对它进行评估，并返回一个分数:

```
clf = sklearn.ensemble.RandomForestClassifier(
        n_estimators=n_estimators, max_depth=max_depth)

return sklearn.model_selection.cross_val_score(
        clf, iris.data, iris.target, n_jobs=-1, cv=3).mean()
```

创建了目标函数后，我们需要创建一个 Optuna 研究，并创建一些试验。然后，我们输出元参数的最佳值:

```
import optuna
import sklearn
import sklearn.datasets
import sklearn.ensemble

study = optuna.create_study(direction='maximize')
study.optimize(objective, n_trials=100)

trial = study.best_trial

print('Accuracy: {}'.format(trial.value))
print("Best hyperparameters: {}".format(trial.params))
```

这将输出每次试验的分数，以及所使用的元参数。最终，在研究完成后，它输出最佳元参数:

```
[I 2020-04-23 17:54:52,817] Finished trial#98 with value: 0.9738562091503268 with parameters: {'n_estimators': 17, 'max_depth': 3.303148836378194}. Best is trial#43 with value: 0.9738562091503268.
[I 2020-04-23 17:54:52,899] Finished trial#99 with value: 0.960375816993464 with parameters: {'n_estimators': 17, 'max_depth': 3.136433926827928}. Best is trial#43 with value: 0.9738562091503268.

Accuracy: 0.9738562091503268
Best hyperparameters: {'n_estimators': 12, 'max_depth': 4.419437654165229}
```

## 优化 GANs

我们如何利用这一点来优化由生成性对抗网络所代表的高度复杂的系统呢？

让我们为那些想要优化的元参数定义几个函数。

GAN 由两个神经网络组成，即*发生器*和*鉴别器*。生成器的任务是试图通过创建假数据来欺骗鉴别器；给定真实和虚假数据的输入，鉴别器必须能够区分两者。

假设我们想要优化两个网络的学习速率:

```
def opt_learning_rate(lr_key, trial):
    """
    lr_key: label to use for this learning rate
    trial: optuna trial object
    (one of those hyperparameters which may vary by orders of magnitude!)
    Returns: a string containing which learning rate is being optimised, & a suggested learning rate
    """
    return trial.suggest_loguniform(
        lr_key,  1e-5, 1000
    )
```

我们使用了**建议 _ 日志统一**功能，并赋予其广泛的范围。

下面我们使用[成人数据集](http://archive.ics.uci.edu/ml/datasets/Adult)来设置优化两种学习率的代码。

让我们从建立一个 Python 字典开始，该字典将包含用于构建网络的默认元参数。

```
default_network_dict = {
    "epochs": 250,
    "batch_size": 64,
    "discriminator_learning_rate": 1e-5,
    "generator_learning_rate": 5e-4,
    "latent_dim": 100,
    "input_output_dim": 20,
    "num_bins": 100,
    "layers": 3,
    "hidden_dims": [64, 128, 256],
    "num_critics": 4,
    "dropout": 0.1,
    "neuron_type": "LeakyReLU",
    "optimiser": "RMSProp",
    "output_folder": False,
}
```

注意，我们已经为 GAN 的学习速率提供了默认值；这允许用户选择他们想要优化的元参数。所有这些都有可能得到优化；然而，字典也可以按原样使用，以创建神经网络。

用户也可以从命令行指定元参数，所以让我们通过从 **argparse** 创建一个命令行解析器名称空间来适应这种情况。

```
import argparse

params = argparse.Namespace(
    experiment_name = 'my-experiment',
    location = 'metaparameter-optimisation',
    dataset_name = 'adult',
    #output_folder = 'output',
    num_bins = 100,
    epochs = 500,
    batch_size = 64,
    discriminator_rate = 1e-05,
    generator_rate = 0.0005,
    sigma = 0.1,
    latent_dim = 200,
    num_critics = 4,
    cuda = 0,
    optimise = ['generator_learning_rate', 'discriminator_learning_rate'],
    hidden_dims = [64, 128, 256],
    structure = False,
)
```

我们需要一种将来自名称空间的命令行元参数与默认网络字典相结合的方法:

```
def override_params(default_params, optimisable, structure=False):
    if structure:
        default_params['layers'] = 'METAPARAM'
        default_params['hidden_dims'] = 'METAPARAM'
    tmp = dict.fromkeys(optimisable, 'METAPARAM')
    return { **default_params, **tmp }
```

最后，我们定义我们的目标函数。

目标函数必须覆盖许多基础，因为它必须包含创建、训练和测试 GAN 的所有代码。该函数创建一个 GAN。然后它设置日志，这样我们就可以看到运行试验的结果。然后用给定的元参数调用一个函数来运行试验。

```
def objective(trial, params, network_dict):
    # need to change all the requested metaparams

    logging.basicConfig(level=logging.INFO)
    logger = logging.getLogger('__metaparams__')

    processor, df, processed_df, categories_n = initialise_processor(params['num_bins'],                                                                  params['dataset_name'],
logger)
   network_dict['input_output_dim'] = categories_n
    fixed_params = fix_metaparams(trial, network_dict)
    generator = build_generator_network(fixed_params)
    discriminator = build_discriminator_network(fixed_params)

    # build the network's optimisers
    gen_optimiser = build_network_optimiser(network_dict['optimiser'],
                                            network_dict['generator_learning_rate'],
                                            generator)
    disc_optimiser = build_network_optimiser(network_dict['optimiser'],
                                            network_dict['discriminator_learning_rate'],
                                            discriminator)

    return run_experiment(
        generator=generator,
        discriminator=discriminator,
        generator_solver=gen_optimiser,
        discriminator_solver=disc_optimiser,
        processor=processor,
        df=df,
        processed_df=processed_df,
        latent_dim=fixed_params["latent_dim"],
        output_folder=fixed_params["output_folder"],
        num_bins=fixed_params["num_bins"],
        epochs=fixed_params["epochs"],
        batch_size=fixed_params["batch_size"],
        sigma=params["sigma"],
        num_critics=fixed_params["num_critics"],
        cuda=params["cuda"],
    )
```

目标函数和它调用的 **run_experiment** 函数看起来都有点复杂，但本质上它们只是分配和解析参数。这两段代码中有相当多的内容。然而，他们实际上只是设置了生成器和鉴别器网络、数据集和赋值器。

```
import logging
from hazy_auto_tuning import initialise_processor, run_experiment

from hazy_network_metaparameters import check_requested_metaparameters, optimisable, fix_metaparams
# from metaparameter_tuning import build_discriminator_network, build_generator_network, build_network_optimiser

from metaparameter_tuning import build_discriminator_network, build_generator_network, build_network_optimiser
```

在从 [Hazy](https://hazy.com/) 代码库中再导入几次之后，你可以建立一个 Optuna 研究对象，并要求它为我们优化我们的元参数。

```
study = optuna.create_study(direction="maximize")
study.optimize(lambda trial: objective(trial, params_dict,       network_dict), n_trials=20)
```

请注意，对于一项有用的研究来说，这(可能)是太少的试验。对于元参数，如鉴别器和生成器的学习率，我们需要更多的试验。类似地，对于 GANs 性能的精确评估，历元的数量可能不够大。这些仅作为例子给出。

作为机器学习工程师，能够自动化元参数优化令人兴奋，因此我们可以花更多时间探索优化它们对所提供的合成数据集的影响。我们已经将 Optuna 代码应用到模型中，这将节省我们所有人的时间。