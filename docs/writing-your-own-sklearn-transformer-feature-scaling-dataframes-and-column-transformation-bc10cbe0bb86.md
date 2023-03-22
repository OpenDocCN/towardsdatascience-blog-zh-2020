# 编写自己的 sklearn 转换器:功能缩放、数据框架和列转换

> 原文：<https://towardsdatascience.com/writing-your-own-sklearn-transformer-feature-scaling-dataframes-and-column-transformation-bc10cbe0bb86?source=collection_archive---------53----------------------->

## 编写自己的 sklearn 函数，第 2 部分

自从`scikit-learn`不久前给 API 增加了`DataFrame`支持，修改和编写你自己的变形金刚变得更加容易——并且[工作流程](https://medium.com/dunder-data/from-pandas-to-scikit-learn-a-new-exciting-workflow-e88e2271ef62)也变得更加容易。

许多`sklearns`的补救措施仍然在内部使用`numpy`数组或返回数组，这在谈到[性能](https://penandpants.com/2014/09/05/performance-of-pandas-series-vs-numpy-arrays/)时通常很有意义。性能在管道中尤其重要，因为如果一个转换器比其他转换器慢得多，它会很快引入瓶颈。当预测对时间要求很高时，这种情况尤其糟糕，例如，将模型用作实时预测的端点时。如果性能不重要或没有仔细评估，许多转换器可以调整和改进，以使用并返回`DataFrames`，这有一些优点:它们是数据科学工作流中非常自然的一部分，它们可以包含不同的数据类型并存储列名。

一个例子是特征标准化，如果你使用我们神经网络的线性模型，这可能是至关重要的:

```
import pandas as pd
import numpy as npdata = pd.DataFrame({
  'num1': [1.0, 2.0, 10.0, 1.0, 3.0, 0.0],
  'num2': [2.0, 3.0, 20.0, -3.0, 5.0, 0.5],
})data
##    num1  num2
## 0   1.0   2.0
## 1   2.0   3.0
## 2  10.0  20.0
## 3   1.0  -3.0
## 4   3.0   5.0
## 5   0.0   0.5
```

根据[文档](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)的说明，`StandardScaler`可以“通过去除平均值并缩放至单位方差来标准化特征”。在`fit`期间，它学习训练数据的平均值和标准偏差，这可用于在`transform`期间标准化特征。默认情况下，转换器将转换后的数据强制转换为一个`numpy`数组，因此列名被删除:

```
from sklearn.preprocessing import StandardScalerstandard_scaler = StandardScaler()
standard_scaler.fit(data);
standard_scaler.transform(data)
## array([[-0.54931379, -0.35307151],
##        [-0.24968808, -0.21639867],
##        [ 2.14731753,  2.10703964],
##        [-0.54931379, -1.03643571],
##        [ 0.04993762,  0.05694702],
##        [-0.84893949, -0.55808077]])
```

我们可以很容易地将转换器修改为返回`DataFrames`，要么继承现有的转换器，要么封装它:

```
another_standard_scaler = AnotherStandardScaler()
another_standard_scaler.fit_transform(data)##        num1      num2
## 0 -0.549314 -0.353072
## 1 -0.249688 -0.216399
## 2  2.147318  2.107040
## 3 -0.549314 -1.036436
## 4  0.049938  0.056947
## 5 -0.848939 -0.558081
```

我们可以进一步修改它，接受一个`cols`参数，只针对特定的列:

```
column_standard_scaler = ColumnStandardScaler(cols=['num1'])
column_standard_scaler.fit_transform(data)
##        num1  num2
## 0 -0.549314   2.0
## 1 -0.249688   3.0
## 2  2.147318  20.0
## 3 -0.549314  -3.0
## 4  0.049938   5.0
## 5 -0.848939   0.5
```

封装后的版本如下，我们仍然继承了`BaseEstimator`和`TransformerMixin`，因为`BaseEstimator`免费给了我们`get_params`和`set_params`，而`TransformerMixin`提供了`fit_transform`。原谅这个愚蠢的名字，即`AnotherColumnStandardScaler`:

```
another_column_standard_scaler = AnotherColumnStandardScaler(cols=['num1'])
another_column_standard_scaler.fit_transform(data)
##        num1  num2
## 0 -0.549314   2.0
## 1 -0.249688   3.0
## 2  2.147318  20.0
## 3 -0.549314  -3.0
## 4  0.049938   5.0
## 5 -0.848939   0.5
```

如果我们想开发自己的转换器，而不是修改或封装现有的转换器，我们可以如下创建它:

```
custom_standard_scaler = CustomStandardScaler(cols=['num1'])
custom_standard_scaler.fit_transform(data)
##        num1  num2
## 0 -0.549314   2.0
## 1 -0.249688   3.0
## 2  2.147318  20.0
## 3 -0.549314  -3.0
## 4  0.049938   5.0
## 5 -0.848939   0.5
```

结果与普通 sklearn 缩放器相同:

```
custom_standard_scaler.transform(data)['num1'].values == standard_scaler.transform(data)[:,0]
## array([ True,  True,  True,  True,  True,  True])
```

除了编写我们自己的转换器，我们还可以使用`sklearns` `ColumnTransformer`将不同的转换器应用于不同的列(并通过传递`passthrough`保留其他的)。但是，这个函数将返回数组，因此会删除列名:

```
from sklearn.compose import ColumnTransformercolumn_transformer = ColumnTransformer(
    transformers=[
        ('scaler', AnotherStandardScaler(), ['num1']),
    ],
    remainder='passthrough')column_transformer.fit_transform(data)
## array([[-0.54931379,  2\.        ],
##        [-0.24968808,  3\.        ],
##        [ 2.14731753, 20\.        ],
##        [-0.54931379, -3\.        ],
##        [ 0.04993762,  5\.        ],
##        [-0.84893949,  0.5       ]])
```

*原载于*[*https://blog.telsemeyer.com*](https://blog.telsemeyer.com/2019/12/15/writing-your-own-sklearn-transformer-dataframes-feature-scaling-columntransformer-writing-your-own-sklearn-functions-part-two/)*。*