# 如何高效地从 TensorFlow 2.0 中读取 BigQuery 数据

> 原文：<https://towardsdatascience.com/how-to-read-bigquery-data-from-tensorflow-2-0-efficiently-9234b69165c8?source=collection_archive---------22----------------------->

## 使用 tensor flow _ io . big query . bigqueryclient 创建 tf.data.dataset

TensorFlow 的 [BigQueryClient](https://github.com/tensorflow/io/tree/master/tensorflow_io/bigquery) 使用存储 API 直接从 BigQuery 存储中高效地读取数据(即，无需发出 BigQuery 查询)。在本文中，我将向您展示如何在 Keras/TensorFlow 2.0 模型中使用该类来创建 tf.data 数据集。你可以在 GitHub 上跟随这个[笔记本。](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/blogs/bigquery_datascience/bigquery_tensorflow.ipynb)

![](img/defea1804f960a851f4650039b52b5bb.png)

使用 BigQuery 作为数据湖:将数据直接从 BigQuery 读入 TensorFlow

作为示例，我将使用信用卡交易的数据集。这些交易中约有 0.17%是欺诈性的，挑战在于在这个非常非常不平衡的数据集上训练一个分类模型。因此，在这个过程中，您还将学习一些如何处理不平衡数据的技巧。

## 基准

开发机器学习模型的最佳实践是拥有一个简单的基准。在这种情况下，我将使用 BigQuery ML 开发基准模型:

```
CREATE OR REPLACE MODEL advdata.ulb_fraud_detection 
TRANSFORM(
    * EXCEPT(Amount),
    SAFE.LOG(Amount) AS log_amount
)
OPTIONS(
    INPUT_LABEL_COLS=['class'],
    AUTO_CLASS_WEIGHTS = TRUE,
    DATA_SPLIT_METHOD='seq',
    DATA_SPLIT_COL='Time',
    MODEL_TYPE='logistic_reg'
) ASSELECT 
 *
FROM `bigquery-public-data.ml_datasets.ulb_fraud_detection`
```

请注意，我在这个超级简单的模型中做了几件事情:

*   我正在训练一个逻辑回归模型(或线性分类器)——这是这个问题最简单的可能 ML 模型，可以作为对更复杂模型的检查。
*   我使用时间列划分数据，因此前 80%的事务是训练数据集，后 20%是评估数据集。这样，我们就不会在欺诈活动存在时间依赖性的情况下泄露信息。
*   我要求 BigQuery ML 根据它们在训练数据集中的出现频率自动对这些类进行加权。
*   我正在使用 log 函数转换范围很大的 Amount 列，这样它也是一个相对较小的数字。我使用 BigQuery ML 的 TRANSFORM 子句来实现这一点。
*   因为在某些情况下金额为零，所以我用 SAFE。记录以避免数字错误。

这给了我一个 AUC(曲线下面积)为 0.9863 的模型，说明了 BigQuery ML 有多么强大。让我们看看我们是否可以用 Keras 编写的更复杂的机器学习模型来击败这一点。

## 创建 tf.data.dataset

为了将 BigQuery 数据读入 Keras，我将使用 [BigQueryClient](https://github.com/tensorflow/io/tree/master/tensorflow_io/bigquery) 。下面是创建数据集的代码:

```
import tensorflow as tf
from tensorflow.python.framework import dtypes
from tensorflow_io.bigquery import BigQueryClient
from tensorflow_io.bigquery import BigQueryReadSessiondef features_and_labels(features):
  label = features.pop('Class') # this is what we will train for
  return features, labeldef read_dataset(client, row_restriction, batch_size=2048):
    GCP_PROJECT_ID='your_project_name'  # CHANGE
    COL_NAMES = ['Time', 'Amount', 'Class'] + ['V{}'.format(i) for i in range(1,29)]
    COL_TYPES = [dtypes.float64, dtypes.float64, dtypes.int64] + [dtypes.float64 for i in range(1,29)]
    DATASET_GCP_PROJECT_ID, DATASET_ID, TABLE_ID,  = 'bigquery-public-data.ml_datasets.ulb_fraud_detection'.split('.')
    bqsession = client.read_session(
        "projects/" + GCP_PROJECT_ID,
        DATASET_GCP_PROJECT_ID, TABLE_ID, DATASET_ID,
        COL_NAMES, COL_TYPES,
        requested_streams=2,
        row_restriction=row_restriction)
    dataset = bqsession.parallel_read_rows()
    return dataset.prefetch(1).map(features_and_labels).shuffle(batch_size*10).batch(batch_size)client = BigQueryClient()
train_df = read_dataset(client, 'Time <= 144803', 2048)
eval_df = read_dataset(client, 'Time > 144803', 2048)
```

本质上，我们使用 client.read_session()创建一个会话，传入要读取的表、要读取的表的列，以及对我们关心的行的简单限制。这些数据被并行读入 tf.data.dataset，我用它来创建一个训练数据集和一个评估数据集。

请注意，上面的代码使用了一些 tf.data.dataset 最佳实践，如预取、混排和批处理。

## 创建 Keras 模型输入层

创建 Keras 模型来读取结构化数据涉及到特性列。要了解更多，请参阅我的文章，关于在 Keras 中创建宽深模型和保持 T2 转换代码与输入分离。因此，不重复我自己，下面是创建 Keras 的输入层的代码:

```
# create inputs, and pass them into appropriate types of feature columns (here, everything is numeric)
inputs = {
    'V{}'.format(i) : tf.keras.layers.Input(name='V{}'.format(i), shape=(), dtype='float64') for i in range(1, 29)
}
inputs['Amount'] = tf.keras.layers.Input(name='Amount', shape=(), dtype='float64')
input_fc = [tf.feature_column.numeric_column(colname) for colname in inputs.keys()]# transformations. only the Amount is transformed
transformed = inputs.copy()
transformed['Amount'] = tf.keras.layers.Lambda(
    lambda x: tf.math.log(tf.math.maximum(x, 0.01)), name='log_amount')(inputs['Amount'])
input_layer = tf.keras.layers.DenseFeatures(input_fc, name='inputs')(transformed)
```

## 处理阶级不平衡

处理 Keras 模型中的类不平衡包括两个步骤:

*   指定对数输出图层的初始偏差(正/负)
*   以总权重等于训练样本数量的方式，对不频繁类进行比频繁类大得多的加权

我们可以使用 BigQuery 计算必要的值:

```
WITH counts AS (
SELECT
    APPROX_QUANTILES(Time, 5)[OFFSET(4)] AS train_cutoff
    , COUNTIF(CLASS > 0) AS pos
    , COUNTIF(CLASS = 0) AS neg
FROM `bigquery-public-data`.ml_datasets.ulb_fraud_detection
)SELECT
   train_cutoff
    , SAFE.LOG(SAFE_DIVIDE(pos,neg)) AS output_bias
    , 0.5*SAFE_DIVIDE(pos + neg, pos) AS weight_pos
    , 0.5*SAFE_DIVIDE(pos + neg, neg) AS weight_neg
FROM counts
```

这给了我以下数字:Keras 模型的输出偏差需要设置为-6.36，类权重需要为 289.4 和 0.5。

## 创建 Keras 模型

然后，我们可以创建一个 Keras 模型，其中包含两个隐藏的全连接层和一个丢弃层(以限制过拟合)，并注意为输出层提供初始偏差，为损失函数提供类权重:

```
# Deep learning model
d1 = tf.keras.layers.Dense(16, activation='relu', name='d1')(input_layer)
d2 = tf.keras.layers.Dropout(0.25, name='d2')(d1)
d3 = tf.keras.layers.Dense(16, activation='relu', name='d3')(d2)
output = tf.keras.layers.Dense(1, activation='sigmoid', name='d4', bias_initializer=tf.keras.initializers.Constant())(d3)model = tf.keras.Model(inputs, output)
model.compile(optimizer='adam',
              loss='binary_crossentropy',
              metrics=metrics)class_weight = {0: 0.5, 1: 289.4}
history = model.fit(train_df, validation_data=eval_df, epochs=20, class_weight=class_weight)
```

结果呢？经过 20 次迭代，我得到了:

```
val_accuracy: 0.9718 - val_precision: 0.0401 - val_recall: 0.8831 - val_roc_auc: 0.9865
```

这比标准的逻辑回归模型要好，但只是勉强好。我们将需要进一步超参数调整节点数、掉线等。深度学习模型比这做得更好。

## 将张量流模型加载到 BigQuery 中

可以将训练好的 TensorFlow 模型加载到 BigQuery 中，用它来做推理。要加载模型，请从 Keras 调用:

```
model.save('gs://{}/bqexample/export'.format(BUCKET))
```

然后，在 BigQuery 中，执行以下操作:

```
CREATE OR REPLACE MODEL advdata.keras_fraud_detection 
OPTIONS(model_type='tensorflow',   
        model_path='gs://BUCKETNAME/bqexample/export/*')
```

您可以使用此模型进行预测，就好像它是一个原生的 BigQuery ML 逻辑回归模型一样:

```
SELECT d4, Class
FROM ML.PREDICT( MODEL advdata.keras_fraud_detection,
  (SELECT * FROM `bigquery-public-data.ml_datasets.ulb_fraud_detection` WHERE Time = 85285.0)
)
```

上面将其称为 d4 的原因是我的 Keras 输出节点被称为 d4。

## 摘要

在本文中，您看到了如何:

*   直接从 BigQuery 读入 TensorFlow 2.0/Keras 模型
*   如何将训练好的模型加载到 BigQuery 中

在这个过程中，您还看到了如何在高度不平衡的数据上训练 BigQuery ML 模型和 Keras 模型。

## 后续步骤:

1.  本文中的代码在 GitHub 上的一个[笔记本里。在 AI 平台笔记本上试用一下。](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/blogs/bigquery_datascience/bigquery_tensorflow.ipynb)
2.  关于 BigQuery 的更多信息，请阅读由 O'Reilly Media 出版的《BigQuery:权威指南》一书:

*注意:根据贵公司的定价方案，使用超过一定限制的* [*存储 API 可能会产生 BigQuery 费用*](https://cloud.google.com/bigquery/pricing#storage-api) *。在我写这篇文章的时候，采用统一费率计划的客户如果每月的读取量超过 300 TB，就会开始产生这些费用。*