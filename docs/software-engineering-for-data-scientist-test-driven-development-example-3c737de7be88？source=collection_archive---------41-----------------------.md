# 面向数据科学家的软件工程——测试驱动开发(示例)

> 原文：<https://towardsdatascience.com/software-engineering-for-data-scientist-test-driven-development-example-3c737de7be88?source=collection_archive---------41----------------------->

![](img/7f2fae616fb72ebec5b1a6d47471edcd.png)

图片来源 pix abay—[https://www . pexels . com/photo/abstract-business-code-coder-270348/](https://www.pexels.com/photo/abstract-business-code-coder-270348/)

这是本系列的第四篇文章。有关本系列文章的列表，请查看前几篇文章部分。

上一篇文章可在— **数据科学家软件工程—测试驱动开发**[https://medium . com/@ jaganadhg/Software-Engineering-for-Data-Scientist-Test-Driven-Development-65 f1 CDF 52d 58](https://medium.com/@jaganadhg/software-engineering-for-data-scientist-test-driven-development-65f1cdf52d58)获得

## 介绍

在上一篇文章中，我们讨论了数据科学中的测试驱动开发。本文介绍的两个具体测试案例包括针对虚拟/猜测机器的模型检查和预测一致性检查。本文是同一主题的快速教程。

在本教程中，我们正在建立一个二元分类器。本练习中使用的数据是取自 Kaggle [1]的口袋妖怪数据。本练习将构建一个随机森林分类器，并将其与猜测机器(参考 ROC AUC 得分)进行比较，以及预测的一致性。

## 资料组

这个练习的数据是“用于数据挖掘和机器学习的口袋妖怪”[1]。该数据集包括第 6 代之前的口袋妖怪。该数据共有 21 个属性，包括身份属性(“数字”)。我们为该练习选择了以下属性:“isLegendary”、“Generation”、“Type_1”、“Type_2”、“HP”、“Attack”、“Def”、“Sp_Atk”、“Sp_Def”、“Speed”、“Color”、“Egg_Group_1”、“Height_m”、“Weight_kg”、“Body_Style”。属性“Generation”用于拆分数据，然后从数据集中删除。属性“isLegendary”是此处的目标。有五个分类属性，它们是‘Egg _ Group _ 1’，‘Body _ Style’，‘Color’，‘Type _ 1’，‘Type _ 2’。我们在训练/验证/测试之前一次性转换了这些属性。

## 软件

对于本教程，我们将使用以下 Python 包。

熊猫

sklearn 0.23.2

pytest 6.1.0

ipytest 0.9.1

数字版本 1.19.1

工具 z 0.11.1

## 模型结构

让我们建立我们的模型！

```
%matplotlib inline
import matplotlib.pyplot as plt
import pandas as pd
import sklearn as sl
import pytest
import ipytest
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import MinMaxScaler
from sklearn.dummy import DummyClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import (confusion_matrix,
                            classification_report,
                            roc_auc_score)
import toolzipytest.autoconfig()data = pd.read_csv("../data/pokemon_alopez247.csv")
```

我们从数据中选择以下属性。

```
selected_cols = ['isLegendary','Generation', 'Type_1', 'Type_2', 'HP', 'Attack',
                'Defense', 'Sp_Atk', 'Sp_Def', 'Speed','Color','Egg_Group_1',
                'Height_m','Weight_kg','Body_Style']data = data[selected_cols]
```

属性“isLegendary”是我们的类标签。让我们把这个转换成 int。

```
data.isLegendary = data.isLegendary.astype(int)
```

## 转换分类变量

以下函数用于转换分类变量。

```
def create_dummy(data: pd.DataFrame, categories: list) -> pd.DataFrame:
    """ Create dummay varibles a.k.a categorical encoding in a DataFrame
        Parameters
        -----------
        data : A pandas DataFrame containing the original data
        categories: the arribute/column names to be transfromed

        Returns
        -----------
        data_tf : Transfromed DataFrame
    """

    for category in categories:
        dummy_df = pd.get_dummies(data[category])
        data = pd.concat([data,dummy_df],
                          axis=1)
        data.drop(category,
                 axis=1,
                 inplace=True)

    return datacateg_cols = ['Egg_Group_1', 'Body_Style', 'Color','Type_1', 'Type_2']data = create_dummy(data,categ_cols)
```

## 创建培训测试和验证数据

首先，我们通过训练和测试来拆分数据。所有属于第一代的口袋妖怪都被选作训练。其余数据用作测试数据。训练数据被进一步分割以创建验证数据集。

```
train_data = data[data.Generation != 1]
test_data = data[data.Generation == 1]train_data.drop("Generation",
                axis=1,
                inplace=True)
test_data.drop("Generation",
               axis=1,
               inplace=True)d:\anaconda2\envs\sweng\lib\site-packages\pandas\core\frame.py:4167: SettingWithCopyWarning: 
A value is trying to be set on a copy of a slice from a DataFrame

See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
  errors=errors,def create_x_y(data: pd.DataFrame, col_name : str) -> list:
    """ Cerate training X and y values
        Parameters
        -----------
        data : DataFrame of data
        col_name : Y column name
        Returns
        -----------
        x_y_list: a list contaning DataFrem and y as list
    """
    X = data.drop(col_name,
                 axis=1).values
    y = data[col_name].values

    return (X,y)X_train, y_train = create_x_y(train_data, "isLegendary")X_train, X_val, y_train, y_val = train_test_split(X_train,
                                                  y_train,
                                                  stratify=y_train,
                                                  test_size=0.25)X_test, y_test = create_x_y(test_data, "isLegendary")
```

**最小-最大缩放比例**

使用最小-最大缩放技术进一步处理数据。

```
def scale_data(dataset: pd.DataFrame) -> np.array:
    """ Scle the data 
        Parameters
        -----------
        dataset : input DataFrame
        Returns
        -----------
        scled_data : a numpy array
    """
    transformer = MinMaxScaler()

    sclaed_data = transformer.fit_transform(dataset)

    return sclaed_dataX_train_scaled = scale_data(X_train)
X_test_scaled = scale_data(X_test)
X_val_scaled = scale_data(X_val)
```

## 创造一个猜测机器！

为了创建一个猜测机器，我们使用了 scikit-learn 的“DummyClassifier”。虚拟分类器设计用于制作快速基线模型。分类器为基线提供了不同的策略。分类器忽略除目标之外的输入数据。在本练习中，我们将使用分层方法来构建模型。

```
dummy_classifier = DummyClassifier(strategy="stratified",
                                  random_state=1995)
_ = dummy_classifier.fit(X_train_scaled,
                        y_train)
```

## 创建随机森林分类器

现在让我们创建一个随机森林分类器。

```
rf_classifier = RandomForestClassifier(criterion='entropy',
                                      max_depth=5,
                                      warm_start=True,
                                      random_state=1925)_ = rf_classifier.fit(X_train_scaled,
                     y_train)
```

## 编写测试用例

现在我们正在编写模型和预测一致性的测试用例。我们使用 pytest 和 ipytest 来编写和执行 JupyterNotebook 中的测试用例。为测试用例创建了两个夹具。

pytest fixture 'fixture_isbetter '使用虚拟分类器和随机森林分类器来创建预测。它返回虚拟分类器、随机森林分类器和实际标签的结果。下一个 fixture 是‘pred _ consist _ data’；它从验证数据中随机挑选五个例子。

测试用例“test_is_better”检查随机森林分类器的 ROC AUC 分数是否优于猜测机器。理想情况下，应该是通过考验！接下来的两个测试用例“test_prediction_consistency”和“test_pred_proba_consistency”测试模型的一致性。测试背后的直觉是，如果相同的输入被提供给模型 n 次，它应该提供相同的结果(除了 Reenforcemtn 学习)。这里，第一个测试用例检查标签的一致性，第二个测试用例检查概率的一致性。

例如，一致性测试的目的被分成两个测试用例。它也可以和一个结合。

```
%%run_pytest[clean]

@pytest.fixture
def fixture_isbetter():
    """ Fixture for checking if model is better than guess"""
    dumm_pred = dummy_classifier.predict(X_val_scaled)
    clf_pred = rf_classifier.predict(X_val_scaled)

    return (dumm_pred,clf_pred, y_val)

@pytest.fixture
def pred_consist_data():
    """ Generate random 5 records from validation data
        for prediction consistency test.
    """
    num_records = X_val.shape[0]
    raandom_indics = np.random.choice(num_records,
                                     size=5,
                                     replace=False)
    sample_for_test = X_val[raandom_indics, :]

    return sample_for_test

def test_is_better(fixture_isbetter):
    """ Test if the target classifier is better than the dummy classifier
        Parameters
    """
    actuls = fixture_isbetter[-1]
    pred_dummy = fixture_isbetter[0]
    pred_clf = fixture_isbetter[1]
    roc_auc_dummy = roc_auc_score(actuls, pred_dummy)
    roc_auc_clf = roc_auc_score(actuls, pred_clf)

    print(f"ROC AUC for dummy classifer is {roc_auc_dummy} \
          and ROC AUC score for RandomForest is {roc_auc_clf}")

    assert round(roc_auc_clf,4) > round(roc_auc_dummy,4)

def test_prediction_consistency(pred_consist_data):
    """ Test the prediction consistency"""
    #import toolz

    predictions_list = list()

    for iteration in range(5):
        preds = rf_classifier.predict(pred_consist_data)
        predictions_list.append(list(preds))

    unique_if = list(map(list, toolz.unique(map(tuple,predictions_list))))

    assert len(unique_if) == 1

def test_pred_proba_consistency(pred_consist_data):
    """ Test prediction consistency"""
    #import toolz

    pridct_proba = list()

    for iteration in range(5):
        preds = rf_classifier.predict(pred_consist_data)
        pridct_proba.append(list(preds))

    unique_if = list(map(list, toolz.unique(map(tuple,pridct_proba))))

    assert len(unique_if) == 1...                                                                                                              [100%]
3 passed in 0.17s
```

瞧。三个测试用例全部通过！。可以有额外的测试用例来部署重新训练的模型。在这种情况下，最好用先前的模型替换测试用例的虚拟分类器。sklearn 提供了一个虚拟分类器和回归器。如果您想将它应用到任何其他类型的模型，我们可能需要编写我们的模块来实现目标。！

## 以前的文章

面向数据科学家的软件工程—测试驱动开发[https://medium . com/@ jaganadhg/software-Engineering-for-Data-Scientist-Test-Driven-Development-65 f1 CDF 52d 58](https://medium.com/@jaganadhg/software-engineering-for-data-scientist-test-driven-development-65f1cdf52d58)

面向数据科学家的软件工程—编写干净代码的艺术—[https://medium . com/@ jaganadhg/software-Engineering-for-Data-Scientist-Art-of-Writing-Clean-Code-f 168 bf8a 6372](https://medium.com/@jaganadhg/software-engineering-for-data-scientist-art-of-writing-clean-code-f168bf8a6372)

AI/ML/数据科学项目的软件工程—[https://medium . com/@ jaganadhg/software-Engineering-for-AI-ML-Data-Science-Projects-bb73e 556620 e](https://medium.com/@jaganadhg/software-engineering-for-ai-ml-data-science-projects-bb73e556620e)

# 参考

[1]口袋妖怪数据集，[https://www.kaggle.com/alopez247/pokemon](https://www.kaggle.com/alopez247/pokemon)