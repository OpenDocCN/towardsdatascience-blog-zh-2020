# 基于混合规则的机器学习与 scikit-learn

> 原文：<https://towardsdatascience.com/hybrid-rule-based-machine-learning-with-scikit-learn-9cb9841bebf2?source=collection_archive---------9----------------------->

## 使用领域知识，通过硬编码的规则来扩充您的 scikit-learn 模型

![](img/3eed98e4009e9e01ea17200f4935a40d.png)

图片由 [You X Ventures](https://unsplash.com/@youxventures) 在 [Unsplash](https://unsplash.com/photos/Oalh2MojUuk)

***TL；DR***[*scikit-learn*](https://scikit-learn.org/stable/index.html)*不允许你将硬编码的规则添加到你的机器学习模型中，但是对于很多用例来说，你应该！本文探讨了如何利用领域知识和面向对象编程(OOP)在 scikit-learn 之上构建基于混合规则的机器学习模型。*

# 介绍

有监督的机器学习模型非常适合在不确定的情况下进行预测；他们从过去的数据中提取模式，并将其准确地推断到未来。机器学习已经推动了一些领域的发展，在这些领域中，确定最可能的结果(无论是类还是特定值)一直具有挑战性，容易出错，或者在规模上过于耗时或昂贵。

然而，在许多领域中，所有可能的结果都不是模糊的，而是根据定义确定的。您可能会遇到嵌入在特定于业务的流程或法规中的规则。在这样的环境中，让 ML 模型使用隐式学习来渐进地猜测预先制定的规则似乎是低效的。相反，我们希望模型关注所有不存在预定义规则的情况。

在本文中，您将了解到将预定义的领域规则合并到机器学习模型中有许多好处。为了获得更多的实际操作，我们将为 [scikit-learn](https://scikit-learn.org/stable/index.html) 估算器构建一个简单的包装器类，它考虑了显式规则，并让模型来解决困难的情况。

如果您不能等待，请跳到 Python 中的[完整文档实现。](#14fa)

# 领域知识的重要性

任何好的机器学习项目都是从领域知识的聚合开始的——收集关于业务问题的相关信息和专业知识的过程。通常，我们与行业从业者交谈，在线研究，并进行数据探索，以揭示有助于机器学习模型构建的特定趋势、模式或提示。

领域知识非常有用，原因有很多:它帮助我们平衡涉众的需求，理解我们的目标受众，但最重要的是，它给了我们关于特性工程的重要线索。虽然在试图识别照片中的猫时，这些线索是不言自明的，但在许多行业领域，如法律、保险或医疗诊断，特征工程远非直观。

为了说明这一点，假设您的目标是为一家电信公司建立一个 ML 模型来预测客户流失(计划取消率)。在设计和迭代可能的功能之前，收集该领域专家关于影响流失因素的意见当然会有所帮助——保留部门是一个合理的起点。保留部门甚至可以用数据来支持他们的观点，使用客户调查来揭示具体的痛点。在任何情况下，数据和行业从业者都可以为您指出正确的方向，节省时间，并可能揭示以前未考虑的数据源和功能组合。

## 如何从领域知识中获取规则

在许多工业领域中，您可以从已经存在的过程中推导出简单的确定性规则。例如，在特定的法律诉讼中，名誉损害索赔可能永远不会被批准，因为法律只是这样规定。同样，保险公司可能不会支付低于 1，000 美元的损失索赔，因为根据与被保险人的合同，他们没有责任这样做。如果我们想预测诉讼结果或保险损失，这种简单的规则可以直接构建到机器学习模型中，以提高性能。

由于机器学习在解决模糊和具有挑战性的情况方面非常出色，因此只有在确定性规则适用于每种情况并且不太多也不太复杂的情况下，将确定性规则纳入模型才有意义。然而，还存在其他用例，所以我整理了一个完整的列表，列出了您何时可能想要考虑部署混合的、基于规则的模型:

*   **预测过程的确定性规则已经存在** 如前所述，根据您试图预测的内容，评估过程的确定性规则可能已经存在。如果规则很简单，适用于所有情况，并且总体上没有太多规则，将它硬编码到机器学习模型中可以保证你已经可以非常准确地预测一部分情况。
*   **缺少特定类型预测案例的数据** 在某些类型预测案例的数据稀疏的情况下，您的模型可能很难开发出正确的隐式规则来正确分类或估计数据点。如果模型不能仅从其他特征准确推断目标变量，通常会出现这种情况。在上面的法律示例中，可能有不常见的索赔类别，如费用报销，对于这些类别，只存在少数数据点。由于索赔类别对于确定诉讼结果至关重要，因此在没有进一步了解报销索赔的情况下，该模型不可能正确预测诉讼结果。在这种情况下，通过简单地预测该类别所有实例的平均目标变量，例如所有费用报销的平均成功率，可以提高性能。
*   **高特征基数
    高特征基数(一个特征可能值的数量)是几乎所有机器学习模型的问题。特别是对于需要编码的分类数据，大量的唯一可能值会影响模型性能。因此，如果存在适当的经验法则或统计参数，逼近目标变量可能会产生有吸引力的折衷，因为它会使剩余数据具有较低的基数以帮助模型训练。**
*   **积极对抗数据中的偏差** 机器学习预测天生就有偏差，因为我们的训练数据中反映了真实世界的模式。在某些情况下，我们可以通过自己处理问题和硬编码覆盖模型行为的规则来防止有偏见的预测。

## 如何将确定性规则硬编码为逻辑公式

如前所述，机器学习模型隐式地学习规则。这种学习的集大成者是基于决策树的算法，如 *scikit-learn* 的 [*决策树分类器*](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html) 或[*GradientBoostingRegressor*](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingRegressor.html)，后者是决策树的[集合](https://scikit-learn.org/stable/modules/ensemble.html#ensemble)。

基于决策树的算法试图通过学习从提供的数据推断出的决策规则来预测目标变量。决策规则本身非常简单；它们是只使用基本逻辑操作符=、、≤、≥对数据进行的一系列分割。然而，所有的分割只是近似任何明确的规则，因此可能不准确。

我们可以使用相同的方法来构建简单的确定性规则作为逻辑公式，这样我们就可以将其翻译成代码。例如，让我们再次假设我们想要设计一个预测模型来估计一家保险公司的总损失，并且我们知道该公司拒绝小于或等于 1.000 美元的索赔。对该规则进行硬编码的一种方式是:

```
if claim_amount <= 1000:
   # reject claimelse:
   # use machine learning model
```

# 让我们开始编码吧

有许多方法可以将确定性规则集成到我们的机器学习管道中。作为数据预处理步骤逐步添加规则可能看起来很直观，但这不符合我们的目标。优选地，我们的目标是通过采用面向对象编程(OOP)来利用抽象的概念，以生成一个新颖的 ML 模型类。这个混合模型将包含所有确定性规则，使我们能够像其他任何机器学习模型一样训练它。

方便的是， *scikit-learn* 提供了一个[*base estimator*](https://scikit-learn.org/stable/modules/generated/sklearn.base.BaseEstimator.html?highlight=baseestimator)*类，我们可以继承它来自己构建 scikit-learn 模型，而不需要太多的努力。构建新估计器的优势在于，我们可以将规则直接与模型逻辑相结合，同时利用底层机器学习模型来处理规则不适用的所有数据。*

*让我们从构建新的混合模型类开始，并向它添加一个 *init* 方法。作为底层模型，我们将使用 *scikit-learn* 实现一个[*GradientBoostingClassifier*](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingClassifier.html#sklearn.ensemble.GradientBoostingClassifier)*；*我们称之为*基础模型*。*

```
*import pandas as pd
from typing import Dict, Tuple
from sklearn.base import BaseEstimatorclass RuleAugmentedGBC(BaseEstimator):

  **def __init__(self, base_model: BaseEstimator, rules: Dict, **base_params):**

    self.rules = rules
    self.base_model = base_model
    self.base_model.set_params(**base_params)*
```

*我们创建了继承自 *BaseEstimator* 的 *RuleAugmentedGBC* 类。我们的类还没有完成，仍然缺少一些基本的方法，但是从技术上来说，它现在是一个 scikit-learn 估计器。 *init* 方法利用一个 *base_model* 和一个规则字典初始化我们的估计器。我们可以在 *init* 方法中设置额外的参数，然后直接传递给底层的 *base_model* 。在我们的例子中，我们将使用一个*GradientBoostingClassifier*作为 *base_model* 。*

## *规则的通用格式*

*在本文的实现中，我们将以以下格式为模型提供规则:*

```
*{"House Price": [
    ("<", 1000.0, 0.0),
    (">=", 500000.0, 1.0)
],
 "...": [
    ...
    ...
]}*
```

*如上所示，我们将规则格式化为一个 *Python* 字典。字典键代表我们想要应用规则的特性列名。字典的值是元组列表，每个元组代表一个唯一的规则。元组的第一个元素是规则的逻辑操作符，第二个是拆分标准，最后一个对象是模型在规则适用时应该返回的值。*

*例如，上面示例中的第一条规则表明，如果*房价*特征列中的任何值小于 1000.0，模型应该返回值 0.0。*

## *该拟合方法*

*我们继续编写一个 *fit* 方法(在我们的 *RuleAugmentedGBC* 类中),以允许我们的模型对数据进行训练。这里需要注意的是，我们希望尽可能使用确定性规则，并且只在不受规则影响的数据上训练 *base_model* 。我们将通过制定一个名为 *_get_base_model_data* 的私有助手方法来分解这个步骤，以过滤出训练我们的 *base_model 所必需的数据。**

```
***def fit(self, X: pd.DataFrame, y: pd.Series, **kwargs):** train_x, train_y = self._get_base_model_data(X, y)
  self.base_model.fit(train_x, train_y, **kwargs)*
```

**fit* 方法非常简单:它首先应用要编码的 *_get_base_model_data* 方法来提取我们底层 *base_model* 的训练特征和标签，然后将模型拟合到数据。与之前类似，我们可以设置附加参数，随后将这些参数传递给 *base_model* 的 fit 方法。现在让我们实现 *_get_base_model_data* 方法:*

```
***def _get_base_model_data(self, X: pd.DataFrame, y: pd.Series) -> Tuple[pd.DataFrame, pd.Series]:** train_x = X

  for category, rules in self.rules.items(): if category not in train_x.columns.values: continue
    for rule in rules: if rule[0] == "=":
        train_x = train_x.loc[train_x[category] != rule[1]] elif rule[0] == "<":
        train_x = train_x.loc[train_x[category] >= rule[1]] elif rule[0] == ">":
        train_x = train_x.loc[train_x[category] <= rule[1]] elif rule[0] == "<=":
        train_x = train_x.loc[train_x[category] > rule[1]] elif rule[0] == ">=":
        train_x = train_x.loc[train_x[category] < rule[1]] else:
        print("Invalid rule detected: {}".format(rule)) indices = train_x.index.values
  train_y = y.iloc[indices]
  train_x = train_x.reset_index(drop=True)
  train_y = train_y.reset_index(drop=True) return train_x, train_y*
```

*我们的私有 *_get_base_model_data* 方法遍历规则字典键，最后遍历每个唯一的规则。在每个规则中，根据逻辑操作符，它缩小了 *train_x* pandas 数据帧的范围，只包括不受规则影响的数据点。一旦我们应用了所有规则，我们通过索引匹配相应的标签，并返回 *base_model* 的剩余数据。*

## *该预测方法*

**预测*方法的工作方式类似于*拟合*方法。只要有可能，就应该适用规则；如果没有适用的规则，*基本模型*应该产生一个预测。*

```
***def predict(self, X: pd.DataFrame) -> np.array:**

  p_X = X.copy()
  p_X['prediction'] = np.nan for category, rules in self.rules.items(): if category not in p_X.columns.values: continue
    for rule in rules: if rule[0] == "=":
        p_X.loc[p_X[category] == rule[1], 'prediction'] = rule[2] elif rule[0] == "<":
        p_X.loc[p_X[category] < rule[1], 'prediction'] = rule[2] elif rule[0] == ">":
        p_X.loc[p_X[category] > rule[1], 'prediction'] = rule[2] elif rule[0] == "<=":
        p_X.loc[p_X[category] <= rule[1], 'prediction'] = rule[2] elif rule[0] == ">=":
        p_X.loc[p_X[category] >= rule[1], 'prediction'] = rule[2] else:
        print("Invalid rule detected: {}".format(rule)) if len(p_X.loc[p_X['prediction'].isna()].index != 0): base_X = p_X.loc[p_X['prediction'].isna()].copy()
    base_X.drop('prediction', axis=1, inplace=True)
    p_X.loc[p_X['prediction'].isna(), 'prediction'] = self.base_model.predict(base_X) return p_X['prediction'].values*
```

**predict* 方法复制我们的输入 pandas 数据帧，以便不改变输入数据。然后我们添加一个*预测*列，在其中我们收集了所有混合模型的预测。就像在 *_get_base_model_data* 方法中一样，我们遍历所有规则，并在适用的情况下，在*预测*列中记录相应的返回值。一旦我们应用了所有的规则，我们检查是否有任何预测仍然丢失。如果是这种情况，我们返回到我们的*基础模型*来生成剩余的预测。*

## *其他要求的方法*

*为了获得从 *BaseEstimator* 类继承的工作模型，我们需要实现两个更简单的方法——get _ params*和 *set_params* 。这些允许我们设置和读取新模型的参数。由于这两种方法不是本文主题的组成部分，如果您想了解更多，请查看下面完整记录的实现。**

# *完整记录的实施*

*下面，您将找到我们在本文中构建的 *scikit-learn* 包装器类的完整代码，以及完整的文档。您可能会发现它对您的一个用例很有用。*

## *基于混合规则的模型使用示例*

*这里有一小段代码来说明如何利用 *RuleAugmentedEstimator* 包装类向[*GradientBoostingClassifier*](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingClassifier.html)添加规则。本例假设您已经初始化了变量*规则*、 *train_X* 、 *train_y* 和 *test_X* 。请参考 [**一节规则的通用格式**](#10ad) 来检查应该如何使用任何规则。*

```
*gbc = GradientBoostingClassifier(n_estimators=50)
**hybrid_model = RuleAugmentedEstimator(gbc, rules)****hybrid_model.fit(train_X, train_y)** predictions = **hybrid_model.predict(test_X)***
```

# *结论*

*恭喜你走到这一步！我希望这篇文章能够帮助您利用领域知识和面向对象编程(OOP)来构建基于混合规则的机器学习模型。正如您所看到的，抽象的概念对于直接将规则合并到 ML 模型中，同时保持您的数据管道的整洁非常有帮助。*

*写这篇文章有助于我深入探索这个主题及其应用。当我试图检查我的工作中的错误时，如果你发现任何错误，请让我知道。*

*我总是很高兴得到反馈，并对数据科学、机器学习和一般技术领域的话题讨论持开放态度。我很想收到你的来信，所以请随时通过[*LinkedIn*](https://www.linkedin.com/in/lukas-haas/)*与我联系。**