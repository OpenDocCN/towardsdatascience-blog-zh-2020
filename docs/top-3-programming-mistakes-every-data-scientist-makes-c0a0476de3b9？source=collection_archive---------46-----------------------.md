# 每个数据科学家都会犯的 3 大编程错误

> 原文：<https://towardsdatascience.com/top-3-programming-mistakes-every-data-scientist-makes-c0a0476de3b9?source=collection_archive---------46----------------------->

## 编程；编排

## 如何使用熊猫、Sklearn 和函数

![](img/d5f1a464e496c8cd03205b4a1cf5d6aa.png)

照片由[埃米尔·佩龙](https://unsplash.com/@emilep?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

本文指出了前景数据科学家最常犯的错误，并讨论了如何避免这些错误。事不宜迟，让我们直入主题吧。

# 熊猫的低效利用

Pandas 是数据科学家经常用来处理结构化数据的库。最常见的错误是使用“for 循环”遍历数据帧中的行。Pandas 具有内置函数，例如`apply`或`applymap`，使您能够对数据帧的所有行的选择或所有列应用函数，或者在满足条件时应用函数。然而，最理想的情况是，您可以使用效率最高的 Numpy 数组。

效率低下:

```
my_list=[] 
for i in range(0, len(df)):        
   l = myfunction(df.iloc[i]['A'], df.iloc[i]['B'])
   my_list.append(l)
df['newColumn'] = my_listTime:640ms
```

高效:

```
df['newColumn'] = myfunction( df['A'].values, df['B'].values)Time: 400 µs
```

或者给定如下数据帧:

```
df = pd.DataFrame([[4, 9]] * 3, columns=['A', 'B'])
**>>>** df
   A  B
0  4  9
1  4  9
2  4  9
```

计算每个元素的平方根

```
df.apply(np.sqrt)
**>>>** df
     A    B
0  2.0  3.0
1  2.0  3.0
2  2.0  3.0
```

来源:[https://pandas . pydata . org/pandas-docs/stable/reference/API/pandas。DataFrame.apply.html](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.apply.html)

# 2.Sklearn 的低效使用

Sklearn (scikit-learn)是数据科学家用来训练机器学习模型的 Python 库。通常在训练一个模型之前需要转换数据，但是如何建立一个转换数据然后训练的管道呢？您可能希望自动化这一点，尤其是当您希望在交叉验证中每个文件夹都应用转换时。遗憾的是，库中最常用的函数是`train_test_split`。许多数据科学家只是转换数据，使用`train_test_split`，在一组上训练，在另一组上测试。

这听起来没那么糟糕，但是当你没有一个大的数据集时可能会特别糟糕。交叉验证对于评估模型如何对看不见的数据进行归纳非常重要。因此，一些数据科学家可能热衷于在每次验证迭代之前实现一个函数来进行转换。

Sklearn 有一个 Pipeline 对象，可以用来在交叉验证过程中转换数据和训练机器学习模型。

在下面的例子中，PCA 在逻辑回归之前应用于交叉验证的每个折叠，同时，进行网格搜索以找到模型的参数以及 PCA 的参数。

```
import numpy as np import matplotlib.pyplot as plt
import pandas as pd  
from sklearn import datasets 
from sklearn.decomposition import PCA 
from sklearn.linear_model import LogisticRegression 
from sklearn.pipeline import Pipeline 
from sklearn.model_selection import GridSearchCV # Define a pipeline to search for the best combination of PCA truncation 
# and classifier regularization. 
pca = PCA() # set the tolerance to a large value to make the example faster 
logistic = LogisticRegression(max_iter=10000, tol=0.1) **pipe = Pipeline(steps=[('pca', pca), ('logistic', logistic)])**

X_digits, y_digits = datasets.load_digits(return_X_y=True)

# Parameters of pipelines can be set using ‘__’ separated parameter
names: 
param_grid = {     
'pca__n_components': [5, 15, 30, 45, 64],     
'logistic__C': np.logspace(-4, 4, 4), 
} search = GridSearchCV(pipe, param_grid, n_jobs=-1) search.fit(X_digits, y_digits) print("Best parameter (CV score=%0.3f):" % search.best_score_) print(search.best_params_)
```

来源:[https://sci kit-learn . org/stable/tutorial/statistical _ inference/putting _ together . html](https://scikit-learn.org/stable/tutorial/statistical_inference/putting_together.html)

# 3.不使用函数

一些数据科学家不太关心他们的代码表示和格式，但是他们应该关心。与只在单个文件或笔记本中编写脚本相比，编写函数有几个好处。这不仅更容易调试，而且代码可以重用，更容易理解。这篇[帖子](/5-reasons-why-you-should-switch-from-jupyter-notebook-to-scripts-cb3535ba9c95)更详细地展示了函数在数据科学中的应用，尽管我不同意作者的观点，即你不能在 Jupyter 笔记本中复制这种方法。此外，作为一名数据科学家，你应该致力于保持代码干燥。那就是[不要重复自己](https://github.com/davified/clean-code-ml)！消除重复使代码可以被你和其他可能接触到你的代码的人重用。此外，它还有助于维护和识别错误。

例如，您可能有这样几行数据帧操作:

```
df.drop(columns=['A','B'],inplace=True) df['datetime']=pd.to_datetime(df['dt']) 
df.dropna(inplace=True)
```

但是效率不高，因为每次要预处理数据帧时，都必须复制粘贴所有代码行，并根据需要编辑列名。

相反，如果有一个处理所有操作的函数，那就更好了。

```
processor = Preprocess(columns_to_drop, datetime_column, dropna_columns)
```

你可以在这里找到预处理[的实现。](/5-reasons-why-you-should-switch-from-jupyter-notebook-to-scripts-cb3535ba9c95)

如果您是数据科学新手或正在准备数据科学面试，您可能也会对 [3 个数据科学家最常问的 Python 面试问题](/3-most-asked-python-interview-questions-for-data-scientists-1a2ad63ebe56)感兴趣