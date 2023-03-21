# 用 Scikit-Learn 构建机器学习管道

> 原文：<https://towardsdatascience.com/building-a-machine-learning-pipeline-3bba20c2352b?source=collection_archive---------14----------------------->

## 数据科学

## 使用 scikit-learn 构建简单管道的分步指南

![](img/87c2ea17db2dad77fd56c961335d691c.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com/s/photos/data-pipeline?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

数据科学项目需要迭代进展。例如，我们清理和准备用于建模的数据，将其转换为适当的格式，运行模型，获得结果，改进模型/更改模型，进行特征工程，获得新的结果，将它们与其他结果进行比较，等等。把每一步都做一遍又一遍，不容易，也不聪明。为了解决这个问题，我们可以使用管道来集成机器学习工作流的步骤。

管道对于快速转换和训练数据非常有用。此外，我们可以通过在管道中集成网格搜索来比较不同的模型和调整超参数。在本文中，我将讲述如何在 scikit 中创建管道——学习展示管道的神奇世界。

制作管道的方法有很多，但我将在这篇博客中展示其中一种最简单、最聪明的方法。

要使用 scikit-learn 的管道功能，我们必须导入管道模块。

```
from sklearn.pipeline import Pipeline
```

通过使用(键，值)对的列表，可以构建管道。这里，键是一个字符串，其中包含您要给出的名称，值是 estimator 对象。非常简单的示例代码，显示如何使用；

```
estimators = [('reduce_dim', PCA()), ('clf',SVC())]pipe = Pipeline(estimators)
```

更多细节，你可以查看 [scikit-learn 文档](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html)。

## 构造管道的简写:make_pipeline 函数

“make_pipeline”是一个实用函数，是构造管道的简写。它接受可变数量的估计，并通过自动填充名称来返回管道。

我想在示例中解释管道的用法，因为我认为，当我们看到代码中模块的应用时，更容易理解。在现实生活中，数据集通常由数字列和分类列组成。我们必须用不同的技术改造这些柱子。当我们使用缩放器缩放数字列时，我们应该用编码器对分类列进行编码。第一次进行这种转换很容易，但通常，在数据科学项目中，我们会尝试不同的缩放器和编码器。为了快速简便地实现这一点，我们使用了管道。

在我的一个项目中，我用分类技术预测了坦桑尼亚水井的状况。我对管道使用了不同的缩放器、编码器和分类模型。如果你想看完整的带数据的木星笔记本，以及如何在建模过程中使用管道，可以在我的 Github 上找到[这里](https://github.com/ezgigm/Project3_TanzanianWaterWell_Status_Prediction/blob/master/STEP2_Modeling.ipynb)。项目中的管道示例；

## 步骤 1:导入库和模块

我在这里只展示如何导入管道模块。但是当然，我们需要导入我们计划使用的所有库和模块，比如 pandas、NumPy、RobustScaler、category_encoders、train_test_split 等。

```
**from** **sklearn.pipeline** **import** make_pipeline
```

## 第二步:读取数据

```
df = pd.read_csv('clean_data.csv')
```

## 第三步:准备数据

如果您的数据包含一些无意义的要素、空值/错误值，或者需要任何类型的清理过程，您可以在此阶段进行清理。因为数据的质量影响模型的质量。在这篇博客中，我的目的是展示管道过程，所以我跳过这一部分，使用我的数据的清理版本。

## 步骤 4:定义分类和数字列

```
cat_col = ['basin','region','extraction_type_group','management','payment','water_quality','quantity','source','waterpoint_type','decade','installer_cat','funder_cat']

num_col = ['gps_height','longitude','latitude','district_code','population','public_meeting','permit']
```

## 步骤 5:分割特征/目标和训练/测试数据

```
target='status_group'used_cols = [c **for** c **in** df.columns.tolist() **if** c **not** **in** [target]]
X=df[used_cols]
y=df[target]X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

## 步骤 6:制作管道和建模

```
*# making pipeline*scaler = RobustScaler()
encoder = ce.TargetEncoder(cols=cat_col)*# putting numeric columns to scaler and categorical to encoder*
num_transformer = make_pipeline(scaler)
cat_transformer = make_pipeline(encoder)

*# getting together our scaler and encoder with preprocessor*
preprocessor = ColumnTransformer(
      transformers=[('num', num_transformer, num_col),
                    ('cat', cat_transformer, cat_col)])

*# choosing model*
model_name = LogisticRegression(class_weight = 'balanced', solver = 'lbfgs', random_state=42)

*# giving all values to pipeline*
pipe = make_pipeline(preprocessor,model_name)
pipe.fit(X_train, y_train)

*# make predictions on training set*
y_pred = pipe.predict(X_train)

*# make predictions on test set*
y_pred_test = pipe.predict(X_test)

*# to print the results in good way*
print("Accuracy:"); print("="*len("Accuracy:"))
print(f"TRAIN: {accuracy_score(y_train, y_pred)}")
print(f"TEST: {accuracy_score(y_test, y_pred_test)}")

print("**\n**Balanced Accuracy:"); print("="*len("Balanced Accuracy:"))
print(f"TRAIN: {balanced_accuracy_score(y_train, y_pred)}")
print(f"TEST: {balanced_accuracy_score(y_test, y_pred_test)}")
```

在这个例子中，我们可以看到，我们可以添加功能到我们的管道，如预处理器，其中包含缩放器和编码器。针对不同的问题，我们还可以在流水线中增加更多的函数。这有助于以快速简单的方式转换我们的数据。在这个例子中，make_pipeline 函数自动将 scaler、encoder 和我们的模型应用到管道中，我们可以非常轻松地对其进行拟合。

当我们写一个函数，把我们的管道放在这个函数中并返回结果时，改变模型也是非常容易的。对于这个例子，当我们只改变 model_name 来尝试另一个分类模型并运行调用相应函数的单元时，它很容易在管道内得到结果。简而言之，我们不需要为了转换数据而改变数据集。我们可以在管道中进行每一次转换，并保持我们的数据集不变。

我们也可以使用 make_pipeline 来集合数值列的估算器和缩放器；

```
# Imputing nulls and scaling for numeric columns
num_imputer = SimpleImputer(strategy='median')
scaler = RobustScaler()# Imputing nulls through the encoding for categorical columns
encoder = ce.TargetEncoder(cols=cat_cols, handle_missing="value")# Defining different transformers for numeric and categorical columns
num_transformer = make_pipeline(num_imputer, scaler)
cat_transformer = make_pipeline(encoder)*# getting together our scaler and encoder with preprocessor*
preprocessor = ColumnTransformer(
      transformers=[('num', num_transformer, num_col),
                    ('cat', cat_transformer, cat_col)])

*# choosing model*
model_name = LogisticRegression(class_weight = 'balanced', solver = 'lbfgs', random_state=42)

*# giving all values to pipeline*
pipe = make_pipeline(preprocessor,model_name)
```

在这个例子中，我还在我的管道中添加了 imputer。如您所见，当我们构建一个好的管道而不需要手动转换数据时，添加和更改模块是非常容易的。

## 结论:

管道的目的是在设置不同参数时，将几个可以交叉验证的步骤组合在一起。通过收集这些步骤，可以帮助我们轻松地添加新参数或对模型进行更改。此外，它使我们的代码更具可读性和可理解性。通过建立一个可理解的工作流程，它有助于项目的可重复性。通过使用管道，我们不需要在流程开始时转换数据。Pipeline 为我们完成所需的转换，并保留原始数据。

## 其他资源:

如果您想深入了解 scikit-learn 库文档，这里有一些有用的链接。

[](https://scikit-learn.org/stable/modules/compose.html) [## 6.1.管道和复合估计器-sci kit-了解 0.23.2 文档

### 转换器通常与分类器、回归器或其他估计器结合，以构建复合估计器。的…

scikit-learn.org](https://scikit-learn.org/stable/modules/compose.html) [](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html) [## sk learn . pipeline . pipeline-sci kit-learn 0 . 23 . 2 文档

### 具有最终估计器的变换流水线。依次应用一系列转换和一个最终估计器…

scikit-learn.org](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html) 

如果您对本文有任何反馈或建议，请随时通过 [LinkedIn 与我联系。](https://www.linkedin.com/in/ezgi-gumusbas-6b08a51a0/)