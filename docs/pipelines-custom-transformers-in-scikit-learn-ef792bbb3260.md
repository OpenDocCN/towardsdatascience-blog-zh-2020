# Scikit 中的管道和定制变压器-了解

> 原文：<https://towardsdatascience.com/pipelines-custom-transformers-in-scikit-learn-ef792bbb3260?source=collection_archive---------11----------------------->

## 带有代码片段的介绍性说明…

机器学习学术课程往往几乎只关注模型。有人可能会说，模型是表演魔术的东西。这种说法可能有一定的道理，但这种魔力只有在数据形式正确的情况下才能发挥作用。此外，让事情变得更复杂的是，“正确的形式”取决于模型的类型。

![](img/94e3a09623fe1f0e8b574e83b34b88e4.png)

演职员表:[https://www . free pik . com/free-vector/pipeline-brick-wall-background _ 3834959 . htm](https://www.freepik.com/free-vector/pipeline-brick-wall-background_3834959.htm)(*我更喜欢 MarioBros。图片…但是你知道:版权)

以正确的形式获取数据被业界称为预处理。这花费了机器学习从业者大量的时间。对于工程师来说，预处理和拟合或者预处理和预测是两个不同的过程，但是在生产环境中，当我们为模型服务时，没有区别。它只是数据输入，预测输出。管道就是用来做这个的。它们将预处理步骤和拟合或预测集成到单个操作中。除了帮助模型生产就绪，他们还为实验阶段增加了大量的可重复性。

# 学习目标

*   什么是管道
*   什么是变压器
*   什么是定制变压器

# 资源

*   [管道&sci kit 中的自定义变压器-学习:分步指南(带 Python 代码)](/pipelines-custom-transformers-in-scikit-learn-the-step-by-step-guide-with-python-code-4a7d9b068156)
*   [使用 Python 定制转换器的 ML 数据管道](/custom-transformers-and-ml-data-pipelines-with-python-20ea2a7adb65)
*   [如何在 Python 中转换回归的目标变量](https://machinelearningmastery.com/how-to-transform-target-variables-for-regression-with-scikit-learn/)
*   [使用 Scikit-learn 管道和特征联合](http://zacstewart.com/2014/08/05/pipelines-of-featureunions-of-pipelines.html)

# 参考

[Scikit 学习。数据集转换](https://scikit-learn.org/stable/data_transforms.html)

从 Scikit 学习文档中，我们可以获得:

> 数据集变换…与其他估计器一样，这些估计器由具有拟合方法和变换方法的类表示，拟合方法从训练集中学习模型参数(例如，归一化的均值和标准差)，变换方法将此变换模型应用于看不见的数据。fit_transform 可以更方便和有效地同时对训练数据进行建模和转换。

我们将重点介绍两种变压器类型，即:

*   [预处理数据](https://scikit-learn.org/stable/modules/preprocessing.html)
*   [缺失值插补](https://scikit-learn.org/stable/modules/impute.html)

# 定制变压器

虽然 Scikit learn 附带了一套标准的变压器，但我们将从一个定制的变压器开始，以了解它们的功能和工作原理。首先要记住的是，自定义转换器既是估计器又是转换器，所以我们将创建一个从 BaseEstimator 和 TransformerMixin 继承的类。用 super()初始化它是一个很好的做法。__init__()。通过继承，我们免费得到了 get_params、set_params 这样的标准方法。在 init 中，我们还想创建模型参数或我们想学习的参数。

```
**class** **CustomScaler**(BaseEstimator, TransformerMixin):
    **def** __init__(self):
        super().__init__()
        self.means_ = **None**
        self.std_ = **None**

    **def** fit(self, X, y=**None**):
        X = X.to_numpy()
        self.means_ = X.mean(axis=0, keepdims=**True**)
        self.std_ = X.std(axis=0, keepdims=**True**)

        **return** self

    **def** transform(self, X, y=**None**):
        X[:] = (X.to_numpy() - self.means_) / self.std_

        **return** X
```

fit 方法是“学习”发生的地方。这里，我们基于生成模型参数的训练数据来执行操作。

在转换方法中，我们将在 fit 中学习到的参数应用于看不见的数据。请记住，预处理将成为整个模型的一部分，因此在训练期间，拟合和变换将应用于同一个数据集。但是之后，当您使用经过训练的模型时，您只能根据训练数据集而不是看不见的数据，应用带有通过 fit 学习的参数的变换方法。

重要的是，无论要应用的数据是什么，学习到的参数以及变压器的操作都是相同的。

# 标准变压器

Scikit learn 自带各种开箱即用的标准变压器。考虑到它们几乎不可避免的使用，你应该熟悉数字数据的[标准化，或均值去除和方差缩放](https://scikit-learn.org/stable/modules/preprocessing.html#standardization-or-mean-removal-and-variance-scaling)和[简单估算器](https://scikit-learn.org/stable/modules/generated/sklearn.impute.SimpleImputer.html)，以及分类的[编码分类特征](https://scikit-learn.org/stable/modules/preprocessing.html#encoding-categorical-features)，特别是 one-of-K，也称为 one-hot 编码。

# 管道

## 链接估计量

请记住，变压器是一个估计，但你的模型也是(逻辑回归，随机森林等)。).你可以把它想象成台阶垂直堆叠。这里秩序很重要。所以你要把预处理放在模型之前。关键是一步输出是下一步输入。

## 特征联合:复合特征空间

通常，您希望对某些要素应用不同的变换。数字数据和分类数据所需的转换是不同的。这就好像你有两条平行的路，或者它们是水平堆叠的。

并行路径的输入是相同的。因此，转换方法必须从选择与转换相关的特征开始(例如，数字特征或分类特征)。

# 例子

我们将为 Kaggle 的[泰坦尼克号数据集](https://www.kaggle.com/c/titanic/data)做预处理流水线。你可以在这里找到卡格勒斯的教程。

![](img/aaf62dca234d207d0deae6ec2f5c5d1f.png)

演职员表:[https://commons . wikimedia . org/wiki/RMS _ Titanic #/media/File:Titanic _ in _ color . png](https://commons.wikimedia.org/wiki/RMS_Titanic#/media/File:Titanic_in_color.png)

对于我们的工作，您可以遵循下面提供的要点中的步骤(在一个新的选项卡中打开它，然后跟着做)。它包含了所有的代码。为了更好地理解，我们将把它分开。

现在让我们开始吧。解压文件并加载数据后，进行快速浏览。

```
# loading and explorationfilename = '/content/working_directory/train.csv'
raw_train = pd.read_csv(filename)
print('data set shape: ', raw_train.shape, '**\n**')
print(raw_train.head())data set shape:  (891, 12) 

   PassengerId  Survived  Pclass  ...     Fare Cabin  Embarked
0            1         0       3  ...   7.2500   NaN         S
1            2         1       1  ...  71.2833   C85         C
2            3         1       3  ...   7.9250   NaN         S
3            4         1       1  ...  53.1000  C123         S
4            5         0       3  ...   8.0500   NaN         S

[5 rows x 12 columns]
```

现在，在删除我们将不使用的功能(乘客 Id、姓名、机票、客舱、已登机)并分离标签(幸存)后，六(6)个功能保留下来，即:Pclass、性别、年龄、SibSp、Parch 和 Fare。

```
dr = ['PassengerId','Name','Ticket','Cabin','Embarked']
train = raw_train.drop(labels = dr, axis = 1)

X = train.drop('Survived', axis=1)
y = train['Survived'].values
print('data set shape: ', X.shape, '**\n**')
print(X.head())
print(X.describe())data set shape:  (891, 6) 

   Pclass     Sex   Age  SibSp  Parch     Fare
0       3    male  22.0      1      0   7.2500
1       1  female  38.0      1      0  71.2833
2       3  female  26.0      0      0   7.9250
3       1  female  35.0      1      0  53.1000
4       3    male  35.0      0      0   8.0500
           Pclass         Age       SibSp       Parch        Fare
count  891.000000  714.000000  891.000000  891.000000  891.000000
mean     2.308642   29.699118    0.523008    0.381594   32.204208
std      0.836071   14.526497    1.102743    0.806057   49.693429
min      1.000000    0.420000    0.000000    0.000000    0.000000
25%      2.000000   20.125000    0.000000    0.000000    7.910400
50%      3.000000   28.000000    0.000000    0.000000   14.454200
75%      3.000000   38.000000    1.000000    0.000000   31.000000
max      3.000000   80.000000    8.000000    6.000000  512.329200
```

注意，有数字(Pclass '，' Age '，' SibSp '，' Parch '，' Fare ')和分类(Sex ')特征，它们的预处理是不同的。还要注意，并非所有乘客年龄值都可用。

```
*# count missing values*
X.isna().sum()Pclass      0
Sex         0
Age       177
SibSp       0
Parch       0
Fare        0
dtype: int64
```

## 自定义估算器

年龄大概是预测存活几率的一个关键因素。因此，为了让模型充分发挥作用，我们需要填充缺失的值。一种替代方法是使用数据集年龄平均值。但是性别、阶级和年龄之间是有关联的。男性比女性年长，而且上层阶级的乘客也比下层阶级的乘客年长。我们可以用它来得出一个比一般平均值更好的重置价值。我们将使用由性别和阶级给出的类别的平均值。请注意，我们使用了两个分类特征(Pclass 和 Sex)来对点进行分组，以填充数字特征(年龄)的缺失值。

```
*# Custom Transformer that fills missing ages*
**class** **CustomImputer**(BaseEstimator, TransformerMixin):
    **def** __init__(self):
        super().__init__()
        self.age_means_ = {}

    **def** fit(self, X, y=**None**):
        self.age_means_ = X.groupby(['Pclass', 'Sex']).Age.mean()

        **return** self

    **def** transform(self, X, y=**None**):
        *# fill Age*
        **for** key, value **in** self.age_means_.items():
            X.loc[((np.isnan(X["Age"])) & (X.Pclass == key[0]) & (X.Sex == key[1])), 'Age'] = value

        **return** X
```

# 数字特征流水线

选择适当的特性后，我们将执行一个简单的估算器和一个标准缩放器。前面介绍的 CustomScaler 与预构建的 Scikit-learn StandardScaler 执行相同的操作。

```
**class** **NumericalTransformer**(BaseEstimator, TransformerMixin):
    **def** __init__(self):
        super().__init__()

    **def** fit(self, X, y=**None**):
        **return** self

    **def** transform(self, X, y=**None**):
        *# Numerical features to pass down the numerical pipeline*
        X = X[['Pclass', 'Age', 'SibSp', 'Parch', 'Fare']]
        X = X.replace([np.inf, -np.inf], np.nan)
        **return** X.values*# Defining the steps in the numerical pipeline*
numerical_pipeline = Pipeline(steps=[
    ('num_transformer', NumericalTransformer()),
    ('imputer', SimpleImputer(strategy='median')),
    ('std_scaler', StandardScaler())])
```

# 分类特征管道

在选择适当的特性(性别)后，我们将通过预构建的转换 OneHotEncoder 执行热编码。

```
**class** **CategoricalTransformer**(BaseEstimator, TransformerMixin):
    **def** __init__(self):
        super().__init__()

    *# Return self nothing else to do here*
    **def** fit(self, X, y=**None**):
        **return** self

    *# Helper function that converts values to Binary depending on input*
    **def** create_binary(self, obj):
        **if** obj == 0:
            **return** 'No'
        **else**:
            **return** 'Yes'

    *# Transformer method for this transformer*
    **def** transform(self, X, y=**None**):
        *# Categorical features to pass down the categorical pipeline*
        **return** X[['Sex']].values*# Defining the steps in the categorical pipeline*
categorical_pipeline = Pipeline(steps=[
    ('cat_transformer', CategoricalTransformer()),
    ('one_hot_encoder', OneHotEncoder(sparse=**False**))])
```

# 水平堆叠

分类管道和数字管道并行但独立运行。它们有相同的输入，但产生独立的输出，我们将重新连接这些输出。为了重新加入它们，我们使用 FeatureUnion。

```
*# Combining numerical and categorical pipeline into one full big pipeline horizontally*
*# using FeatureUnion*
union_pipeline = FeatureUnion(transformer_list=[
    ('categorical_pipeline', categorical_pipeline),
    ('numerical_pipeline', numerical_pipeline)])
```

# 垂直堆叠

因为我们需要自定义估算器的分类和数字特征(我们在其中填充缺失的年龄值)，所以它出现在并行管道之前，现在一起作为预处理管道。为此，我们使用 Scikit Learn 的管道。

```
*# Combining the custom imputer with the categorical and numerical pipeline*
preprocess_pipeline = Pipeline(steps=[('custom_imputer', CustomImputer()),
                                      ('full_pipeline', union_pipeline)])
```

# 模型

我们将使用 Scikit Learn 的决策树分类器。这里的重点不是模型，而是查看转换和管道运行的借口。DecisionTreeClassifier 是我们在预处理管道之后堆叠的另一个估计器。

为了查看所有正在进行的操作，我们将在 full_pipeline 上调用 fit，即预处理和建模，以及稍后的预测。

```
*# MODEL*
**from** **sklearn** **import** tree

*# Decision Tree*
decision_tree = tree.DecisionTreeClassifier()*# define full pipeline --> preprocessing + model*
full_pipeline = Pipeline(steps=[
    ('preprocess_pipeline', preprocess_pipeline),
    ('model', decision_tree)])

*# fit on the complete pipeline*
training = full_pipeline.fit(X, y)
print(full_pipeline.get_params())

*# metrics*
score_test = \
    round(training.score(X, y) * 100, 2)
print(f"**\n**Training Accuracy: **{**score_test**}**")
```

最后是预测部分:

```
*# Prediction*

my_data = X.iloc[[77]]
y = full_pipeline.predict(my_data)
print(my_data, y)Pclass   Sex       Age  SibSp  Parch  Fare
77       3  male -0.211777      0      0  8.05 [0]
```

# 关闭

这个非常简短的关于 Scikit learn 的转换和管道的叙述，应该已经为您提供了以生产就绪和可重复的方式集成机器学习模型中的预处理阶段的工具。希望你喜欢。编码快乐！