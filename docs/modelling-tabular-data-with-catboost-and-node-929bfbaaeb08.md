# 用 CatBoost 和 NODE 对表格数据建模

> 原文：<https://towardsdatascience.com/modelling-tabular-data-with-catboost-and-node-929bfbaaeb08?source=collection_archive---------15----------------------->

俄罗斯在线搜索公司 Yandex 的 CatBoost 快速易用，但最近该公司的研究人员发布了一个新的基于神经网络的包 NODE，他们声称该包优于 CatBoost 和所有其他梯度提升方法。这可能是真的吗？让我们了解一下如何同时使用 CatBoost 和 NODE！

**这篇博文是写给谁的？**

虽然我写这篇博文是为了那些对机器学习，尤其是表格数据感兴趣的人，但是如果你熟悉 Python 和 scikit-learn 库，并且想了解代码，那么这篇博文会很有帮助。如果你不是，希望你会发现理论和概念部分很有趣！

# CatBoost 简介

[CatBoost](https://catboost.ai/) 是我用来建模表格数据的软件包。它是梯度增强决策树的一种实现，只做了一些调整，与 [xgboost](https://xgboost.readthedocs.io/en/latest/) 或 [LightGBM](https://lightgbm.readthedocs.io/en/latest/) 略有不同。它适用于分类和回归问题。

CatBoost 的一些优点:

*   它处理**猫**电气特征(明白吗？)开箱即用，所以不需要担心如何对它们进行编码。
*   它通常只需要很少的参数调整。
*   它避免了其他方法可能遭受的某些微妙类型的数据泄漏。
*   它很快，如果你想让它跑得更快，可以在 GPU 上运行。

对我来说，这些因素使得 CatBoost 成为当我需要分析新的表格数据集时首先要做的事情。

# CatBoost 的技术细节

***如果只是想用 CatBoost 就跳过这一节！***

在更技术性的层面上，关于 CatBoost 是如何实现的，有一些有趣的事情。如果你对细节感兴趣，我强烈推荐论文 [Catboost:具有分类特征的无偏增强](https://arxiv.org/pdf/1706.09516.pdf)。我只想强调两件事。

1.  在这篇论文中，作者表明，标准梯度推进算法受到模型迭代拟合方式导致的微妙类型的数据泄漏的影响。同样，对分类特征进行数字编码的最有效方法(如目标编码)也容易出现数据泄漏和过拟合。为了避免这种泄漏，CatBoost 引入了一个人工时间线，训练示例根据该时间线到达，以便在计算统计数据时只能使用“以前看到的”示例。
2.  CatBoost 其实不用常规决策树，而是*不经意决策树*。在这些树中，在树的每一层，*相同的特征和相同的分裂标准被到处使用*！这听起来很奇怪，但是有一些很好的特性。我们来看看这是什么意思。

![](img/eb05298b0e0c223d849235dd1887fcf1.png)![](img/141129634dd2e8ba2e2582e38ff7dea2.png)

***左:*** *浑然不觉决策树。每个级别都有相同的拆分。* ***右:*** *常规决策树。任何特征或分割点都可以出现在每个级别上。*

在普通决策树中，要分割的特征和截止值都取决于到目前为止您在树中选择的路径。这是有意义的，因为我们可以使用我们已经拥有的信息来决定下一个最有价值的问题(就像在“20 个问题”游戏中一样)。对于遗忘决策树，历史并不重要；不管怎样，我们都提出同样的问题。这些树被称为“健忘的”，因为它们总是“忘记”以前发生过的事情。

这为什么有用？不经意决策树的一个很好的特性是，一个例子可以很快被分类或评分——它总是提出相同的 N 个二元问题(其中 N 是树的深度)。对于许多示例，这可以很容易地并行完成。这是 CatBoost 速度快的一个原因。另一件要记住的事情是，我们在这里处理的是一个树集合。作为一个独立的算法，遗忘决策树可能不会工作得很好，但树集成的想法是，弱学习者的联盟通常工作得很好，因为错误和偏见被“淘汰”。通常，弱学习者是一个标准的决策树，这里它是一个更弱的东西，即不经意决策树。CatBoost 的作者认为，这种特殊的弱基础学习者很适合泛化。

# 安装 CatBoost

虽然安装 CatBoost 应该是一件简单的事情

```
pip install catboost
```

在 Mac 电脑上，我有时会遇到这方面的问题。在 Linux 系统上，比如我现在输入的 Ubuntu 系统，或者在 Google Colaboratory 上，它应该“正常工作”。如果你在安装时一直有问题，考虑使用 Docker 镜像，例如

```
docker pull yandex/tutorial-catboost-clickhouse
docker run -it yandex/tutorial-catboost-clickhouse
```

# 在数据集上使用 CatBoost

[链接到代码为](https://colab.research.google.com/drive/1WezJuc3ioEUZYKh_Mm7YVjWcMeYjDNKP)的 Colab 笔记本

让我们看看如何在表格数据集上使用 CatBoost。我们从下载[成人/人口普查收入](https://archive.ics.uci.edu/ml/datasets/adult)数据集的[轻度预处理版本](https://github.com/hussius/tabnet_fork/raw/master/data/adult.csv)开始，在下文中，假设该数据集位于 datasets/adult.csv 中。我选择该数据集是因为它混合了分类和数字特征，在数万个示例中具有良好的可管理大小，并且没有太多的特征。它经常被用来举例说明算法，例如在谷歌的[假设工具](https://pair-code.github.io/what-if-tool/uci.html)和许多其他地方。

成人人口普查数据集在 Colab 上有“年龄”、“工作阶级”、“教育”、“教育人数”、“婚姻状况”、“职业”、“关系”、“种族”、“性别”、“资本收益”、“资本损失”、“每周小时数”、“本国”和“T6”等列，但我将在此复制这些列以供参考。CatBoost 需要知道哪些特征是分类的，然后自动处理它们。在这个代码片段中，我还使用 5 重(分层)交叉验证来估计预测精度。

我们从运行这个(没有超参数优化的 CatBoost)中得到的是 85%到 86%之间的平均准确率。在我最后一次运行中，我得到了大约 85.7%。

如果我们想要尝试优化超参数，我们可以使用 hyperopt(如果您没有它，请使用 pip install hyperopt 安装它)。为了使用它，您需要定义一个 hyperopt 试图最小化的函数。我们将尝试优化这里的精度。也许对日志丢失进行优化会更好，但这是留给读者的练习；)

```
from catboost import CatBoostClassifier, Pool
from hyperopt import fmin, hp, tpe
import pandas as pd
from sklearn.model_selection import StratifiedKFolddf = pd.read_csv('https://docs.google.com/uc?' + 
                 'id=10eFO2rVlsQBUffn0b7UCAp28n0mkLCy7&' + 
                 'export=download')
labels = df.pop('<=50K')categorical_names = ['workclass', 'education', 'marital-status',
                     'occupation', 'relationship', 'race',
                     'sex', 'native-country']  
categoricals = [df.columns.get_loc(i) for i in categorical_names]nfolds = 5
skf = StratifiedKFold(n_splits=nfolds, shuffle=True)
acc = []for train_index, test_index in skf.split(df, labels):
  X_train, X_test = df.iloc[train_index].copy(), \
                    df.iloc[test_index].copy()
  y_train, y_test = labels.iloc[train_index], \
                    labels.iloc[test_index]
  train_pool = Pool(X_train, y_train, cat_features = categoricals)
  test_pool = Pool(X_test, y_test, cat_features = categoricals)
  model = CatBoostClassifier(iterations=100,
                             depth=8,
                             learning_rate=1,
                             loss_function='MultiClass') 
  model.fit(train_pool)
  predictions = model.predict(test_pool)
  accuracy = sum(predictions.squeeze() == y_test) / len(predictions)
  acc.append(accuracy)mean_acc = sum(acc) / nfolds
print(f'Mean accuracy based on {nfolds} folds: {mean_acc:.3f}')
print(acc)
```

要优化的主要参数可能是迭代次数、学习速率和树深度。还有许多与过拟合相关的其他参数，例如提前停止轮次等。请自行探索！

当我最后一次运行这段代码时，它花了 5 个多小时，但平均准确率为 87.3%，这与我在尝试 [Auger.ai AutoML 平台](https://auger.ai/)时获得的最好结果相当。

健全性检查:逻辑回归

```
# Optimize between 10 and 1000 iterations and depth between 2 and 12search_space = {'iterations': hp.quniform('iterations', 10, 1000, 10),
                'depth': hp.quniform('depth', 2, 12, 1),
                'lr': hp.uniform('lr', 0.01, 1)
               }def opt_fn(search_space): nfolds = 5
  skf = StratifiedKFold(n_splits=nfolds, shuffle=True)
  acc = [] for train_index, test_index in skf.split(df, labels):
    X_train, X_test = df.iloc[train_index].copy(), \
                      df.iloc[test_index].copy()
    y_train, y_test = labels.iloc[train_index], \
                      labels.iloc[test_index]
    train_pool = Pool(X_train, y_train, cat_features = categoricals)
    test_pool = Pool(X_test, y_test, cat_features = categoricals) model = CatBoostClassifier(iterations=search_space['iterations'],
                             depth=search_space['depth'],
                             learning_rate=search_space['lr'],
                             loss_function='MultiClass',
                             od_type='Iter') model.fit(train_pool, logging_level='Silent')
    predictions = model.predict(test_pool)
    accuracy = sum(predictions.squeeze() == y_test) / len(predictions)
    acc.append(accuracy) mean_acc = sum(acc) / nfolds
  return -1*mean_accbest = fmin(fn=opt_fn, 
            space=search_space, 
            algo=tpe.suggest, 
            max_evals=100)
```

在这一点上，我们应该问自己是否真的需要这些新奇的方法。一个好的旧逻辑回归在开箱即用和超参数优化后会有怎样的表现？

# 为了简洁起见，我在这里省略了复制代码，但它在[和之前](https://colab.research.google.com/drive/1WezJuc3ioEUZYKh_Mm7YVjWcMeYjDNKP)一样的 Colab 笔记本中是可用的。逻辑回归实现的一个细节是，它不像 CatBoost 那样处理开箱即用的分类变量，所以我决定使用目标编码对它们进行编码，特别是[留一目标编码](https://contrib.scikit-learn.org/categorical-encoding/leaveoneout.html)，这是 NODE 中采用的方法，与 CatBoost 中发生的情况非常接近，但不完全相同。

长话短说，使用这种编码类型的未调整的逻辑回归产生大约 80%的准确性，在超参数调整后大约 81%(在我最近的运行中是 80.7%)。这里，一个有趣的替代方法是尝试自动化预处理库，如 [vtreat](https://pypi.org/project/vtreat/) 和 [Automunge](https://github.com/Automunge/AutoMunge) ，但我将把它们留到下一篇博文中！

盘点

在尝试 NODE 之前，到目前为止我们有什么？

# 未调整的逻辑回归:80.0%

逻辑回归，调整率:80.7%

*   CatBoost，未调整:85.7%
*   催化增强，调优:87.2%
*   节点:神经不经意决策集成
*   Yandex 研究人员最近的一份手稿描述了一个有趣的神经网络版本的 CatBoost，或者至少是一个神经网络采用了不经意决策树集成(如果你想提醒自己“不经意”在这里是什么意思，请参见上面的技术部分。)这种体系结构称为节点，可用于分类或回归。

# 摘要中的一项声明写道:“*通过在大量表格数据集上与领先的 GBDT 软件包进行广泛的实验比较，我们展示了所提出的节点架构的优势，它在大多数任务上优于竞争对手*。”这自然激起了我的兴趣。这个工具会比 CatBoost 更好吗？

NODE 是如何工作的？

你应该去报纸上看完整的故事，但是一些相关的细节是:

# *entmax* 激活函数被用作常规决策树中分裂的软版本。正如论文所说，“*entmax 能够产生稀疏的概率分布，其中大多数概率恰好等于 0。在这项工作中，我们认为，在我们的模型中，entmax 也是一个适当的归纳偏差，它允许在内部树节点中进行可微分的分裂决策构建。直观地说，entmax 可以根据数据特征的一个小子集(最多一个，如在经典决策树中)学习拆分决策，避免来自其他人的不良影响*。entmax 函数允许神经网络模拟决策树类型的系统，同时保持模型可微分(权重可以基于梯度更新)。

作者提出了一种新类型的层，即“节点层”，可以在神经网络中使用(他们的实现是在 PyTorch 中)。一个节点层代表一个树集合。

*   几个节点层可以堆叠，产生一个层次模型，其中输入通过一个树集合一次馈入。输入表示的连续连接可以用于给出一个模型，该模型让人想起用于图像处理的流行的 DenseNet 模型，只是专门用于表格数据。
*   节点模型的参数包括:
*   学习率(论文中总是 0.001)

节点层数( *k*

*   每层树的数量( *m* )
*   每层树的深度( *d* )
*   节点与树集合有什么关系？
*   为了感受一下这种神经网络架构和决策树集合之间的相似性，这里复制了图 1。

# 节点层如何与决策树相关联。

应该如何选择参数？

![](img/63f6eed37efe989c24bd58cf4c6be741.png)

手稿中没有太多的指导；作者建议使用超参数优化。他们确实提到他们在以下领域进行了优化:

# *层数:{2，4，8}*

*总树数:{1024，2048}*

*   *树深度:{6，8}*
*   *树输出尺寸:{2，3}*
*   在我的代码中，我不进行网格搜索，而是让 hyperopt 在特定范围内采样值。我认为(这可能是错误的)每一层都代表一个树集合(比如说 CatBoost 的一个实例)。对于您添加的每一层，您可能会添加一些表示能力，但是您也会使模型变得更难训练，并可能有过度拟合的风险。总树数似乎大致类似于 CatBoost/xgboost/random 森林中的树数，并且具有相同的权衡:如果有许多树，您可以表达更复杂的函数，但是模型将需要更长的时间来训练，并且有过度拟合的风险。树的深度也有同样的权衡。至于输出维度，坦白说，我不太明白为什么是参数。看论文，好像回归应该等于 1，分类应该等于类数。
*   如何使用节点？

作者们已经在 GitHub 上发布了代码。他们不提供命令行界面，而是建议用户在提供的 Jupyter 笔记本上运行他们的模型。这些笔记本中提供了一个分类示例和一个回归示例。

# repo README 页面还强烈建议使用 GPU 来训练节点模型。(这是有利于 CatBoost 的一个因素。)

我准备了一个[合作笔记本](https://colab.research.google.com/drive/11VH01T5BNDGEBLlZlG28z_BB_3MIBGWt)，里面有一些关于如何在节点上运行分类以及如何用 hyperopt 优化超参数的示例代码。

*现在请移至* [合作笔记本](https://colab.research.google.com/drive/11VH01T5BNDGEBLlZlG28z_BB_3MIBGWt) *继续关注！*

这里我将只强调代码的一些部分。

改编代码的一般问题

我在改编作者的代码时遇到的问题主要与数据类型有关。重要的是，输入数据集(X_train 和 X_val)是 *float32* 格式的数组(numpy 或 torch );不是 *float64* 或 float 和 int 的混合。标签需要编码为长( *int64* )用于分类，编码为 *float32* 用于回归。(您可以在标题为“*加载、分割和预处理数据*”的单元中看到这一点。)

# 其他问题与记忆有关。这些模型可能会很快耗尽 GPU 内存，尤其是在作者的示例笔记本中使用的大批量数据。我通过在笔记本电脑上使用最大批量解决了这个问题。

不过，总的来说，让代码工作并不难。文档有点少，但是足够了。

分类变量处理

与 CatBoost 不同，NODE 不支持分类变量，所以您必须自己将它们准备成数字格式。我们对成人人口普查数据集采用与节点作者相同的方式进行处理，[使用 category_encoders 库](https://github.com/Qwicen/node/blob/3bae6a8a63f0205683270b6d566d9cfa659403e4/lib/data.py#L427-L431)中的 LeaveOneOutEncoder。为了方便起见，这里我们只使用常规训练/测试分割，而不是 5 重 CV，因为训练节点需要很长时间(特别是使用超参数优化)。

# 现在我们有了一个全数字的数据集。

模型定义和训练循环

```
from category_encoders import LeaveOneOutEncoder
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_splitdf = pd.read_csv('https://docs.google.com/uc' + 
                 '?id=10eFO2rVlsQBUffn0b7UCAp28n0mkLCy7&' + 
                 'export=download')
labels = df.pop('<=50K')
X_train, X_val, y_train, y_val = train_test_split(df,
                                                  labels,
                                                  test_size=0.2)class_to_int = {c: i for i, c in enumerate(y_train.unique())}                                                                                                               
y_train_int = [class_to_int[v] for v in y_train]                                                                                                                            
y_val_int = [class_to_int[v] for v in y_val] cat_features = ['workclass', 'education', 'marital-status',
                'occupation', 'relationship', 'race', 'sex',
                'native-country']

cat_encoder = LeaveOneOutEncoder()
cat_encoder.fit(X_train[cat_features], y_train_int)
X_train[cat_features] = cat_encoder.transform(X_train[cat_features])
X_val[cat_features] = cat_encoder.transform(X_val[cat_features])# Node is going to want to have the values as float32 at some points
X_train = X_train.values.astype('float32')
X_val = X_val.values.astype('float32')
y_train = np.array(y_train_int)
y_val = np.array(y_val_int)
```

代码的其余部分与作者报告中的基本相同(除了 hyperopt 部分)。他们创建了一个名为 DenseBlock 的 Pytorch 层，实现了节点架构。一个名为 Trainer 的类保存有关实验的信息，并且有一个简单的训练循环来跟踪迄今为止看到的最佳指标，并绘制更新的损失曲线。

# 结果和结论

通过一些最小的尝试和错误，我能够找到一个验证准确率大约为 86%的模型。用 hyperopt 进行超参数优化后(本该在 Colab 的一个 GPU 上通宵运行，但实际上在大约 40 次迭代后超时)，最佳性能为 87.2%。在其他跑步中，我取得了 87.4%的成绩。换句话说，经过 hyperopt 调优后，NODE 的性能确实优于 CatBoost，尽管只是略微优于 CatBoost。

# 然而，准确性并不是一切。为每个数据集进行昂贵的优化是不方便的。

NODE 与 CatBoost 的优势:

似乎可以得到稍微好一点的结果(基于节点纸和这个测试；我一定会尝试许多其他数据集！)

# CatBoost 与 NODE 的优点:

*   快多了

# 减少对超参数优化的需求

*   没有 GPU 也能正常运行
*   支持分类变量
*   我会在下一个项目中使用哪一个？可能 CatBoost 仍将是我的首选工具，但我会记住 NODE，并可能尝试它以防万一…
*   同样重要的是要认识到，性能是依赖于数据集的，成人人口普查收入数据集并不能代表所有情况。也许更重要的是，分类特征的预处理在 NODE 中可能相当重要。我将在以后的文章中回到预处理的主题！

Which one would I use for my next projects? Probably CatBoost will still be my go-to tool, but I will keep NODE in mind and maybe try it just in case…

It’s also important to realize that performance is dataset-dependent and that the Adult Census Income dataset is not representative of all scenarios. Perhaps more importantly, the preprocessing of categorical features is likely rather important in NODE. I’ll return to the subject of preprocessing in a future post!