# 10 个超参数优化框架。

> 原文：<https://towardsdatascience.com/10-hyperparameter-optimization-frameworks-8bc87bc8b7e3?source=collection_archive---------5----------------------->

使用开源优化库调整您的机器学习模型

![](img/2e2dbe391ceabe83509dfda1053079ce.png)

图片由来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4747055) 的[jrg Felix](https://pixabay.com/users/FelixDesignStudio-2911466/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4747055)拍摄

# 介绍

**超参数**是用于在建立模型时控制算法行为的参数。这些参数不能从常规训练过程中学习到。它们需要在训练模型之前被分配。

例子:**n _ neighbors**(KNN)**kernel**(SVC)**max _ depth**&**criterion**(决策树分类器)等。

**机器学习中的超参数优化**或**调整**是选择提供最佳性能的超参数的最佳组合的过程。

存在各种自动优化技术，当应用于不同类型的问题时，每种技术都有自己的优点和缺点。

例如:网格搜索、随机搜索、贝叶斯搜索等。

[](https://medium.com/swlh/4-hyper-parameter-tuning-techniques-924cb188d199) [## 4 种超参数调整技术

### 每个数据科学家都应该知道的流行超参数调整技术

medium.com](https://medium.com/swlh/4-hyper-parameter-tuning-techniques-924cb188d199) [](/data-leakage-with-hyper-parameter-tuning-c57ba2006046) [## 超参数调整的数据泄漏

### 超参数调优有时会打乱您的模型，并导致对看不见的数据产生不可预测的结果。

towardsdatascience.com](/data-leakage-with-hyper-parameter-tuning-c57ba2006046) 

Scikit-learn 是我们可以用于**超参数优化的框架之一，**但是还有其他框架甚至可以表现得更好。

1.  射线调谐
2.  奥普图纳
3.  远视
4.  多层机器
5.  多轴
6.  贝叶斯最优化
7.  塔罗斯
8.  夏尔巴人
9.  sci kit-优化
10.  GPyOpt

# 1.射线调谐

[*Tune*](https://docs.ray.io/en/latest/tune/index.html) 是一个 Python 库，用于实验执行和任意比例的超参数调谐。[ [GitHub](https://github.com/ray-project/tune-sklearn) ]

## 关键特征

1.  用不到十行代码启动多节点[分布式超参数扫描](https://docs.ray.io/en/latest/tune/tutorials/tune-distributed.html#tune-distributed)。
2.  支持任何机器学习框架，[包括 PyTorch、XGBoost、MXNet、Keras](https://docs.ray.io/en/latest/tune/tutorials/overview.html#tune-guides) 。
3.  在最先进的算法中进行选择，如[基于人口的训练(PBT)](https://docs.ray.io/en/latest/tune/api_docs/schedulers.html#tune-scheduler-pbt) 、 [BayesOptSearch](https://docs.ray.io/en/latest/tune/api_docs/suggestion.html#bayesopt) 、 [HyperBand/ASHA](https://docs.ray.io/en/latest/tune/api_docs/schedulers.html#tune-scheduler-hyperband) 。
4.  Tune 的搜索算法是开源优化[库](https://docs.ray.io/en/latest/tune/api_docs/suggestion.html)的包装器，如 HyperOpt、SigOpt、蜻蜓和脸书 Ax。
5.  使用 TensorBoard 自动可视化结果。

> Scikit 学习调谐
> 
> 安装:pip install ray[tune]tune-sk learn

# 2.奥普图纳

[*Optuna*](https://optuna.readthedocs.io/en/stable/) 是一个自动超参数优化软件框架，专门为机器学习而设计。

## 关键特征

1.  [简单并行](https://optuna.readthedocs.io/en/stable/tutorial/004_distributed.html)
2.  [快速可视化](https://optuna.readthedocs.io/en/stable/reference/visualization.html)
3.  [高效优化算法](https://optuna.readthedocs.io/en/stable/tutorial/007_pruning.html)
4.  [轻量级、通用且平台无关的架构](https://optuna.readthedocs.io/en/stable/tutorial/001_first.html)
5.  [python 式搜索空间](https://optuna.readthedocs.io/en/stable/tutorial/002_configurations.html)

> 安装:pip 安装 optuna

# 3.远视

[Hyperopt](https://github.com/hyperopt/hyperopt) 是一个 Python 库，用于在笨拙的搜索空间上进行串行和并行优化，搜索空间可能包括实值、离散和条件维度。

目前它支持三种算法:

*   [随机搜索](http://www.jmlr.org/papers/v13/bergstra12a.html?source=post_page---------------------------)
*   [Parzen 估计器树(TPE)](https://papers.nips.cc/paper/4443-algorithms-for-hyper-parameter-optimization.pdf)
*   [自适应 TPE](https://www.electricbrain.io/blog/learning-to-optimize)

## 关键特征

1.  搜索空间**(你可以创建非常复杂的参数空间)**
2.  持续并重启**(您可以保存重要信息并在以后加载，然后继续优化过程)**
3.  速度和并行化**(您可以将计算分布在一个机器集群上)**

> 安装:pip 安装 hyperopt

[](/an-introductory-example-of-bayesian-optimization-in-python-with-hyperopt-aae40fff4ff0) [## 用 Hyperopt 实现 Python 中贝叶斯优化的介绍性示例

### 学习强大优化框架基础的实践示例

towardsdatascience.com](/an-introductory-example-of-bayesian-optimization-in-python-with-hyperopt-aae40fff4ff0) 

# 4.多层机器

[mlmachine](https://github.com/petersontylerd/mlmachine#Installation) 是一个 Python 包，它有助于进行整洁有序的基于笔记本的机器学习实验，并完成实验生命周期的许多关键方面。

mlmachine 通过贝叶斯优化对多个估值器执行[超参数调整](https://nbviewer.jupyter.org/github/petersontylerd/mlmachine/blob/master/notebooks/mlmachine_part_4.ipynb)，并包括可视化模型性能和参数选择的功能。

一篇关于 mlmachine 的很好的解释文章。

[](/mlmachine-hyperparameter-tuning-with-bayesian-optimization-2de81472e6d) [## 基于贝叶斯优化的多机超参数整定

### 这个新的 Python 包加速了基于笔记本的机器学习实验

towardsdatascience.com](/mlmachine-hyperparameter-tuning-with-bayesian-optimization-2de81472e6d) 

> 安装:pip 安装 mlmachine

# 5.多轴

[Polyaxon](https://polyaxon.com/docs/automation/optimization-engine/) 是一个用于构建、培训和监控大规模深度学习应用的平台。它构建了一个系统来解决机器学习应用的可重复性、自动化和可扩展性。

Polyaxon 执行超参数调谐的方式是提供一系列可定制的搜索算法。Polyaxon 既支持简单的方法，如`random search`和`grid search`，也为高级方法提供简单的接口，如`Hyperband`和`Bayesian Optimization`，它还集成了工具，如`Hyperopt`，并提供运行定制迭代过程的接口。所有这些搜索算法都以异步方式运行，并支持并发和路由，以最大限度地利用集群的资源。

## 关键特征

1.  易于使用:Polyaxon 的优化引擎是一项内置服务，可以通过向您的操作添加一个`matrix`部分来轻松使用，您可以使用 CLI、客户端和仪表板运行 hyperparameter tuning。
2.  可扩展性:调整超参数或神经架构需要利用大量的计算资源，使用 Polyaxon 可以并行运行数百次试验，并直观地跟踪它们的进展。
3.  灵活性:除了丰富的内置算法，Polyaxon 还允许用户定制各种超参数调谐算法、神经架构搜索算法、提前停止算法等。
4.  效率:我们正集中精力从系统级和算法级进行更有效的模型调优。例如，利用早期反馈来加速调优过程。

> [安装](https://github.com/polyaxon/polyaxon) : pip install -U polyaxon

# 6.贝叶斯优化

[贝叶斯优化](https://github.com/fmfn/BayesianOptimization)是另一个框架，是高斯过程贝叶斯全局优化的纯 Python 实现。这是一个基于贝叶斯推理和高斯过程的约束全局优化包，试图在尽可能少的迭代中找到未知函数的最大值。这种技术特别适合于高成本函数的优化，在这种情况下，勘探和开发之间的平衡非常重要。

> 安装:pip 安装贝叶斯优化

# 7.塔罗斯

[Talos](https://github.com/autonomio/talos) 通过完全自动化超参数调整和模型评估，彻底改变了普通的 Keras 工作流程。Talos 完全公开了 Keras 功能，不需要学习新的语法或模板。

## 关键特征

1.  单线优化预测流水线`talos.Scan(x, y, model, params).predict(x_test, y_test)`
2.  自动化超参数优化
3.  模型概括评估器
4.  实验分析
5.  伪、准和量子随机搜索选项
6.  网格搜索
7.  概率优化器
8.  单个文件自定义优化策略

[](https://autonomio.github.io/docs_talos/#introduction) [## Talos 用户手册

### 欢迎来到塔罗斯！您可以使用 Talos 对 Keras 模型进行超参数优化。Talos 允许您使用 Keras…

autonomio.github.io](https://autonomio.github.io/docs_talos/#introduction) [](/smart-hyperparameter-optimization-of-any-deep-learning-model-using-tpu-and-talos-9eb48d09d637) [## 使用 TPU 和塔罗斯对任何深度学习模型进行智能超参数优化

### Keras API + Colab 张量处理单元+ Talos

towardsdatascience.com](/smart-hyperparameter-optimization-of-any-deep-learning-model-using-tpu-and-talos-9eb48d09d637) 

> 安装:pip 安装 talos

# 8.夏尔巴人

[SHERPA](https://parameter-sherpa.readthedocs.io/en/latest/) 是一个 Python 库，用于机器学习模型的超参数调优。

## 它提供:

1.  机器学习研究人员的超参数优化
2.  超参数优化算法的选择
3.  能够满足用户需求的并行计算
4.  用于结果探索性分析的实时仪表板。

> 安装:pip 安装参数-sherpa

# 9.sci kit-优化

[Scikit-Optimize](https://scikit-optimize.github.io/stable/user_guide.html) 或`skopt`，是一个简单而高效的库，可以最小化(非常)昂贵且嘈杂的黑盒函数。它实现了几种基于模型的顺序优化方法。`skopt`旨在在多种环境下易于访问和使用。Scikit-Optimize 支持调整 scikit-learn 库提供的 ML 算法的超参数，即所谓的超参数优化。

该库建立在 NumPy、SciPy 和 Scikit-Learn 之上。

> 安装:pip 安装 scikit-优化

# 10.GPyOpt

[GPyOpt](https://github.com/SheffieldML/GPyOpt) 是一个使用高斯过程优化(最小化)黑盒函数的工具。它已经被谢菲尔德大学的机器学习小组用 Python 实现了。GPyOpt 基于 [GPy](https://github.com/SheffieldML/GPy) ，是 Python 中高斯流程建模的库。它可以通过稀疏高斯过程模型处理大型数据集。

## [主要特点](https://nbviewer.jupyter.org/github/SheffieldML/GPyOpt/blob/master/manual/index.ipynb)

1.  [任意约束的贝叶斯优化](https://nbviewer.jupyter.org/github/SheffieldML/GPyOpt/blob/master/manual/GPyOpt_constrained_optimization.ipynb)
2.  [并行贝叶斯优化](https://nbviewer.jupyter.org/github/SheffieldML/GPyOpt/blob/master/manual/GPyOpt_parallel_optimization.ipynb)
3.  [混合不同类型的变量](https://nbviewer.jupyter.org/github/SheffieldML/GPyOpt/blob/master/manual/GPyOpt_mixed_domain.ipynb)
4.  [调整 scikit-learn 模型](https://nbviewer.jupyter.org/github/SheffieldML/GPyOpt/blob/master/manual/GPyOpt_scikitlearn.ipynb)
5.  [整合模型超参数](https://nbviewer.jupyter.org/github/SheffieldML/GPyOpt/blob/master/manual/GPyOpt_integrating_model_hyperparameters.ipynb)
6.  [外部客观评价](https://nbviewer.jupyter.org/github/SheffieldML/GPyOpt/blob/master/manual/GPyOpt_external_objective_evaluation.ipynb)

> 安装:pip install gpyopt

# 感谢您的阅读！

非常感谢您的任何反馈和意见！

我的其他一些帖子你可能会感兴趣，

[](/normality-tests-in-python-31e04aa4f411) [## Python 中的 10 个正态性测试(分步指南 2020)

### 正态性检验检查变量或样本是否呈正态分布。

towardsdatascience.com](/normality-tests-in-python-31e04aa4f411) [](https://medium.com/swlh/is-and-in-python-f084f36cbc0e) [## Python 中的“是”和“==”

### 加快字符串比较的速度

medium.com](https://medium.com/swlh/is-and-in-python-f084f36cbc0e)