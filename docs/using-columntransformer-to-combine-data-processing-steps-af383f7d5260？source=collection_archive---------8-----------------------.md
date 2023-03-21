# 使用 ColumnTransformer 组合数据处理步骤

> 原文：<https://towardsdatascience.com/using-columntransformer-to-combine-data-processing-steps-af383f7d5260?source=collection_archive---------8----------------------->

## 在不同列需要不同技术的情况下，创建内聚管道来处理数据

这个 scikit-learn 工具非常方便，但也有自己的一些怪癖。今天，我们将使用它来转换华盛顿州渡轮埃德蒙兹-金斯顿航线的渡轮等待时间数据。(谢谢 WSF 提供的数据！).完全公开:我们今天只使用数据集的一小部分。

更全面的披露——来自 scikit-learn 的警告:“**警告:**`[**compose.ColumnTransformer**](https://scikit-learn.org/stable/modules/generated/sklearn.compose.ColumnTransformer.html#sklearn.compose.ColumnTransformer)`类是实验性的，API 可能会改变。”

![](img/fcbd79ce6645d80132cbb197dfad17d9.png)

华盛顿州渡口。布莱恩·汉森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 一般概念和用途

当您创建一个不同列需要不同转换的数据管道时，ColumnTransformers 就派上了用场。也许你有分类和数字特征的组合。也许您希望使用不同的插补策略来填充不同数字列中的 nan。您可以分别转换每一列，然后将它们缝合在一起，或者您可以使用 ColumnTransformer 来完成这项工作。

这里有一个基本的例子。在这种情况下，我们的输入要素是工作日(0–6 周一至周日)、小时(0–23)以及最高、平均和最低日温度。我想标准规模的温度功能和一个热编码的日期功能。

假设我已经加载了输入和目标数据帧(X_train，y_train ):

```
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.impute import SimpleImputer
from sklearn.linear_model import LinearRegression
from sklearn.pipeline import Pipeline# define column transformer and set n_jobs to use all cores
col_transformer = ColumnTransformer(
                    transformers=[
                        ('ss', StandardScaler(), ['max_temp', 
                                                  'avg_temp', 
                                                  'min_temp']),
                        ('ohe', OneHotEncoder(), ['weekday', 
                                                  'hour'])
                    ],
                    remainder='drop',
                    n_jobs=-1
                    )
```

然后我们就可以开始转型了！

```
X_train_transformed = col_transformer.fit_transform(X_train)
```

我们得到:

```
<465x30 sparse matrix of type '<class 'numpy.float64'>'
	with 2325 stored elements in Compressed Sparse Row format>
```

更有可能的是，您将把 ColumnTransformer 添加为管道中的一个步骤:

```
lr = LinearRegression()pipe = Pipeline([
            ("preprocessing", col_transformer),
            ("lr", lr)
       ])pipe.fit(X_train, y_train)
```

现在你的烟斗可以做预测了！或者用于交叉验证，而不会跨切片泄漏信息。

注意，我们需要以转换器期望的格式来指示列。如果转换器需要一个 2D 数组，那么传递一个字符串列的列表(即使它只有一列——例如`[‘col1']`)。如果转换器需要一个 1D 数组，只需传递字符串列名，例如`'col1'`。

但是事情并不总是这么简单——可能您的数据集具有空值，并且需要在同一列上进行多次转换，您想要一个自定义的转换器，或者您想要更深入地挖掘功能的重要性，可能并不是所有 OneHotEncoder 类别实际上都保证会出现在所有数据片中。

# 技巧 1:对任何需要多次转换的列使用管道

我第一次使用 ColumnTransformer 时，我认为它会按顺序执行转换，我可以从在任何列上简单地输入 nan 开始，然后 StandardScale(r)一个重叠的列子集，然后 OneHotEncode 另一个重叠的列子集，等等。**我错了。如果您想要在同一列上进行多次转换，您需要一个管道。这意味着每组得到相同处理的列都有一个管道，例如:**

```
# define transformers
si_0 = SimpleImputer(strategy='constant', fill_value=0)
ss = StandardScaler()
ohe = OneHotEncoder()# define column groups with same processing
cat_vars = ['weekday', 'hour']
num_vars = ['max_temp', 'avg_temp', 'min_temp']# set up pipelines for each column group
categorical_pipe = Pipeline([('si_0', si_0), ('ohe', ohe)])
numeric_pipe = Pipeline([('si_0', si_0), ('ss', ss)])# set up columnTransformer
col_transformer = ColumnTransformer(
                    transformers=[
                        ('nums', numeric_pipe, num_vars),
                        ('cats', categorical_pipe, cat_vars)
                    ],
                    remainder='drop',
                    n_jobs=-1
                    )
```

# 技巧 2:跟踪你的列名

来自 scikit-learn 文档:“转换后的特征矩阵中的列顺序遵循在`transformers`列表中指定列的顺序。除非在`passthrough`关键字中指定，否则原始特征矩阵中未指定的列将从结果转换后的特征矩阵中删除。用`passthrough`指定的那些列被添加到变压器输出的右边。”

对于上面的例子，预处理的数组列是:

```
[‘max_temp’, ‘avg_temp’, ‘min_temp, ‘weekday_0’, ‘weekday_1’, ‘weekday_2’, ‘weekday_3’, ‘weekday_4’, ‘weekday_5’, ‘weekday_6’, ‘hour_0’, ‘hour_1’, ‘hour_2’, ‘hour_3’, ‘hour_4’, ‘hour_5’, ‘hour_6’, ‘hour_7’, ‘hour_8’, ‘hour_9’, ‘hour_10’, ‘hour_11’, ‘hour_12’, ‘hour_13’, ‘hour_14’, ‘hour_15’, ‘hour_16’, ‘hour_17’, ‘hour_18’, ‘hour_19’, ‘hour_20’, ‘hour_21’, ‘hour_22’, ‘hour_23’] 
```

这是非常繁琐的手工操作。对于提供功能名称的转换，您可以像这样访问它们:

```
col_transformer.named_transformers_['ohe'].get_feature_names()
```

这里，“ohe”是第一个示例中我的转换器的名称。不幸的是，不能创建更多特性/列的转换器通常没有这个方法，ColumnTransformer 依赖于其内部转换器的这个属性。如果你只使用有这个方法的变压器，那么你可以调用`col_transformer.get_feature_names()`来轻松地得到它们。我还没有这个机会，但我们可能会在某个时候。或者这个列跟踪功能可能会被添加到未来的 ColumnTransformer 版本中。

注意:如果你使用管道(就像技巧 1 中一样)，你需要更深入一点，使用管道属性`named_steps`。在这种情况下:

```
col_transformer.named_transformers_['cats'].named_steps['ohe']\
     .get_feature_names()
```

# 技巧 3:随意创造你自己的变形金刚

ColumnTransformer 可以与任何转换器一起工作，所以您可以随意创建自己的转换器。我们今天不打算太深入地研究定制转换器，但是在使用定制转换器和 ColumnTransformer 时，我想指出一点。

对于我们的 ferry 项目，我们可以用一个定制的转换器提取日期特性:

```
from sklearn.base import TransformerMixin, BaseEstimatorclass DateTransformer(TransformerMixin, BaseEstimator):
    """Extracts features from datetime column

    Returns:
      hour: hour
      day: Between 1 and the number of days in the month
      month: Between 1 and 12 inclusive.
      year: four-digit year
      weekday: day of the week as an integer. Mon=0 and Sun=6
   """def fit(self, x, y=None):
        return selfdef transform(self, x, y=None):
        result = pd.DataFrame(x, columns=['date_hour'])
        result['hour'] = [dt.hour for dt in result['date_hour']]
        result['day'] = [dt.day for dt in result['date_hour']]
        result['month'] = [dt.month for dt in result['date_hour']]
        result['year'] = [dt.year for dt in result['date_hour']]
        result['weekday'] = [dt.weekday() for dt in 
                             result['date_hour']]
        return result[['hour', 'day', 'month', 'year', 'weekday']]

def get_feature_names(self):
        return ['hour','day', 'month', 'year', 'weekday']
```

注意，ColumnTransformer 以 numpy 数组的形式“发送”列。为了从字符串转换这些时间戳，我将它们转换为 pandas 数据帧(可能不是最优雅的解决方案)。

注意，ColumnTransformer 将所有指定的列一起“发送”到我们的转换器。这意味着您需要设计您的转换器来同时获取和转换多个列，或者确保在 ColumnTransformer 的单独一行中发送每个列。由于我们的自定义转换器仅设计用于处理单个列，因此我们需要像这样定制 ColumnTransformer(假设我们希望在有两个 datetime 列的情况下重用它，我们希望扩展这两个列):

```
transformers=[(‘dates1’, DateTransformer, [‘start_date’])ct = ColumnTransformer(
          transformers=[
              (‘dates1’, DateTransformer, [‘start_date’]),
              (‘dates2’, DateTransformer, [‘end_date’])
          ])
```

# 提示 4:对“罕见的”分类特征或标志要积极主动

这里的关键是您的模型期望在训练集、测试集和生产输入中有相同数量的特征。

如果我们有任何罕见的分类特征最终没有出现在这些组中，默认的 OneHotEncoding 设置将为不同的输入集产生不同数量的列。

类似地，如果有任何要估算的 nan，SimpleImputer 只创建一个“flag”列。如果一个或多个输入集碰巧没有任何 nan，那么在预处理阶段之后，列的数量将再次不同。

这可能会引发一些不同的错误，包括:

```
ValueError: Found unknown categories [0] in column 0 during transform
```

和

```
ValueError: The features [0] have missing values in transform but have no missing values in fit.
```

对于 OneHotEncoding 问题，可以在初始化 ohe 时列出类别。如果您有两个分类特征，第一个具有类别“一”和“二”，第二个具有“三月”、“四月”，您可以这样表示:`OneHotEncoder(categories=[['one', 'two'], ['March', 'April]])`。

对于 simple imputr，您不能使用一个标志，删除带有 NaN 的列(如果 NaN 很少)，调整您的训练测试分割(并确保您的生产输入考虑到这个差异)，或者通过添加一个标志列来创建您自己的基于 simple imputr 的转换器，而不管 NaN 是否存在。

今天，这个数据准备步骤感觉有点不令人满意，因为我们还没有从我们的数据集得出任何结论或有趣的事实。但是你们都知道，这是我们预测轮渡等待时间(或者任何你想预测/分类/等等的事情)的必不可少的一步。

和往常一样，查看 [GitHub repo](https://github.com/allisonhonold/column_transformer_ferry_wait_blog) 获取完整代码。编码快乐！

![](img/5ad6d8e0a056dae35915d27ea0720343.png)

照片由[帕特里克·罗宾逊](https://unsplash.com/@patrickrobinson?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄