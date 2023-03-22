# 创建 python CLI 应用程序的简单方法

> 原文：<https://towardsdatascience.com/a-simple-way-to-create-python-cli-app-1a4492c164b6?source=collection_archive---------13----------------------->

## 将机器学习应用程序包装到命令行界面的简单示例

![](img/8d867ace60517aac80568921fcd25bae.png)

克里斯里德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

当我们创建任何类型的应用程序时，我们必须提供一个其他人有机会使用的接口。我想向您展示一个将 ML 应用程序包装到 CLI 界面的简单示例。

我认为可以肯定地说，我们希望用最少的努力获得最好的结果。在这种情况下，`fire`库将是最好的选择。`fire`是一个库，由 Google 开发并广泛使用。让我们通过具体的例子来探索它的功能:

```
main.pyimport fire

def add(a: int, b: int):
    *"""
    Returns sum of a and b* ***:param*** *a: first argument* ***:param*** *b: second argument* ***:return****: sum of a and b
    """* return a+b

if __name__ == "__main__":
    fire.Fire({
        "sum": add
    })
```

这里`fire`从函数`add`创建 CLI 命令`sum`。从 python doc 中，它创建一个命令描述。

`python main.py sum --help`的输出:

```
NAME
    main.py sum - Returns sum of a and bSYNOPSIS
    main.py sum A BDESCRIPTION
    Returns sum of a and bPOSITIONAL ARGUMENTS
    A
        first argument
    B
        second argumentNOTES
    You can also use flags syntax for POSITIONAL ARGUMENTS
```

因此，您现在可以通过多种方式使用该命令:

`python main.py 1 2`

`python main.py --a 1 --b 2`

此外，您可以通过在函数定义中定义默认值来定义可选的命令参数:

```
def add(a: int, b: int = 2):
    *"""
    Returns sum of a and b* ***:param*** *a: first argument* ***:param*** *b: second argument (default: 2)* ***:return****: sum of a and b
    """* return a+b
```

现在`python main.py sum --help`的结果是:

```
NAME
    main.py sum - Returns sum of a and bSYNOPSIS
    main.py sum A <flags>DESCRIPTION
    Returns sum of a and bPOSITIONAL ARGUMENTS
    A
        first argumentFLAGS
    --b=B
        second argument (default: 2)NOTES
    You can also use flags syntax for POSITIONAL ARGUMENTS
```

因此，您现在可以通过多种方式使用该命令:

`python main.py 1`

`python main.py 1 2`

`python main.py 1 --b 2`

`python main.py --a 1 --b 2`

无需多言，您可以看到 CLI 应用程序配置是多么简单快捷。

现在让我们为 ML 应用程序的一个常见用例创建一个程序框架。假设您需要创建一个具有两个函数`train`和`predict`的 CLI 应用程序。训练函数输入是训练数据和一些模型参数，输出是经过训练的模型文件。Predict 接受用于预测的输入数据、定型模型，并将预测保存到输出文件中。

除了函数映射，`fire`可以从 python 类实例创建 CLI 接口，在这种情况下，这可能是一个更好的选择。在我们的例子中，代码可能是:

```
import fire
import pickle
import logging

import pandas as pd
from sklearn.neighbors import KNeighborsClassifier

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

def load_model(model_path: str):
    *"""Loads model from `model_path`"""* with open(model_path, 'rb') as file:
        saved_model = pickle.load(file)
        return saved_model

def save_model(model, model_path: str):
    *"""Saves `model` to `model_path`"""* with open(model_path, 'wb') as file:
        pickle.dump(model, file)

class Classifier:
    *"""
    Some classifier, that makes some random classifications.
    """* def train(self, train_data_path: str, model_path: str, k: int = 5):
        *"""
        Trains model on `train_data_path` data and saves trained model to `model_path`.
        Additionaly you can set KNN classifier `k` parameter.* ***:param*** *train_data_path: path to train data in csv format* ***:param*** *model_path: path to save model to.* ***:param*** *k: k-neighbors parameter of model.
        """* logger.info(f"Loading train data from {train_data_path} ...")
        df = pd.read_csv(train_data_path)
        X = df.drop(columns=['y'])
        y = df['y']

        logger.info("Running model training...")
        model = KNeighborsClassifier(n_neighbors=k)
        model.fit(X, y)

        logger.info(f"Saving model to {model_path} ...")
        save_model(model, model_path)

        logger.info("Successfully trained model.")

    def predict(self, predict_data_path: str, model_path: str, output_path: str):
        *"""
        Predicts `predict_data_path` data using `model_path` model and saves predictions to `output_path`* ***:param*** *predict_data_path: path to data for predictions* ***:param*** *model_path: path to trained model* ***:param*** *output_path: path to save predictions
        """* logger.info(f"Loading data for predictions from {predict_data_path} ...")
        X = pd.read_csv(predict_data_path)

        logger.info(f"Loading model from {model_path} ...")
        model = load_model(model_path)

        logger.info("Running model predictions...")
        y_pred = model.predict(X)

        logger.info(f"Saving predictions to {output_path} ...")
        pd.DataFrame(y_pred).to_csv(output_path)

        logger.info("Successfully predicted.")

if __name__ == "__main__":
    fire.Fire(Classifier)
```

`python main.py`的输出为:

```
NAME
    main.py - Some classifier, that makes some random classifications.SYNOPSIS
    main.py COMMANDDESCRIPTION
    Some classifier, that makes some random classifications.COMMANDS
    COMMAND is one of the following:predict
       Predicts `predict_data_path` data using `model_path` model and saves predictions to `output_path`train
       Trains model on `train_data_path` data and saves trained model to `model_path`. Additionaly you can set KNN classifier `k` parameter.
```

以及相应的命令文档:

`python main.py train --help`:

```
NAME
    main.py train - Trains model on `train_data_path` data and saves trained model to `model_path`. Additionaly you can set KNN classifier `k` parameter.SYNOPSIS
    main.py train TRAIN_DATA_PATH MODEL_PATH <flags>DESCRIPTION
    Trains model on `train_data_path` data and saves trained model to `model_path`. Additionaly you can set KNN classifier `k` parameter.POSITIONAL ARGUMENTS
    TRAIN_DATA_PATH
        path to train data in csv format
    MODEL_PATH
        path to save model to.FLAGS
    --k=K
        k-neighbors parameter of model.NOTES
    You can also use flags syntax for POSITIONAL ARGUMENTS
```

`python main.py predict --help`:

```
NAME
    main.py predict - Predicts `predict_data_path` data using `model_path` model and saves predictions to `output_path`SYNOPSIS
    main.py predict PREDICT_DATA_PATH MODEL_PATH OUTPUT_PATHDESCRIPTION
    Predicts `predict_data_path` data using `model_path` model and saves predictions to `output_path`POSITIONAL ARGUMENTS
    PREDICT_DATA_PATH
        path to data for predictions
    MODEL_PATH
        path to trained model
    OUTPUT_PATH
        path to save predictionsNOTES
    You can also use flags syntax for POSITIONAL ARGUMENTS
```

使用示例:

```
python main.py train train.csv knn.sav --k 7
python main.py predict test.csv knn.sav out.csv
```

现在，您可以使用这个代码片段来创建自己的 CLI 应用程序。进一步改进的一些提示:

1.  使用日志记录代替打印
2.  创建参数验证
3.  为最终用户创建带有简短应用程序使用说明的自述模板。
4.  在[https://github.com/google/python-fire](https://github.com/google/python-fire)上探索更多酷炫功能

我希望这篇文章将是有用的和信息丰富的。期待您的反馈！