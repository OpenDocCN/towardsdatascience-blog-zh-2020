# 如何扩展 Keras 模型

> 原文：<https://towardsdatascience.com/how-to-extend-a-keras-model-5effc083265c?source=collection_archive---------53----------------------->

## 使用 Keras 模型时传递实例键和特征

通常，您只需要 Keras 模型返回预测值，但是在某些情况下，您希望您的预测保留一部分输入。一个常见的例子是在执行批量预测时转发唯一的“实例键”。在这篇博客和[对应的笔记本代码](https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/blogs/batch_predictions/batch_predictions_keras.ipynb)中，我将演示如何修改一个经过训练的 Keras 模型的签名，以将特性转发到输出或传递实例键。

![](img/76c3825dabd98a6e6e043272ae3522cd.png)

通过实例键排序。照片由 [Samantha Lam](https://unsplash.com/@contradirony?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 如何将实例键转发到输出

有时，您会有一个与每一行相关联的唯一实例键，并且您希望该键与预测一起输出，以便您知道该预测属于哪一行。当使用类似[云人工智能平台批量预测](http://l)的服务执行分布式批量预测时，您需要添加密钥。此外，如果您正在对模型执行连续评估，并且想要记录有关预测的元数据以供以后分析。 [Lak Lakshmanan](https://medium.com/u/247b0630b5d6?source=post_page-----5effc083265c--------------------------------) 展示了[如何使用张量流估值器](/how-to-extend-a-canned-tensorflow-estimator-to-add-more-evaluation-metrics-and-to-pass-through-ddf66cd3047d)实现这一点，但是 Keras 呢？

假设您有一个以前训练过的模型，它已经用`tf.saved_model.save()`保存了。运行以下代码，您可以检查模型的服务签名，并查看预期的输入和输出:

```
tf.saved_model.save(model, MODEL_EXPORT_PATH)!saved_model_cli show — tag_set serve — signature_def serving_default — dir {MODEL_EXPORT_PATH}The given SavedModel SignatureDef contains the following input(s):
  inputs['image'] tensor_info:
      dtype: DT_FLOAT
      shape: (-1, 28, 28)
      name: serving_default_image:0
The given SavedModel SignatureDef contains the following output(s):
  outputs['preds'] tensor_info:
      dtype: DT_FLOAT
      shape: (-1, 10)
      name: StatefulPartitionedCall:0
Method name is: tensorflow/serving/predict
```

要传递唯一的行键和先前保存的模型，请加载您的模型，创建一个替代的服务函数，然后重新保存，如下所示:

```
loaded_model = tf.keras.models.load_model(MODEL_EXPORT_PATH)@tf.function(input_signature=[tf.TensorSpec([None], dtype=tf.string),tf.TensorSpec([None, 28, 28], dtype=tf.float32)])
def keyed_prediction(key, image):
    pred = loaded_model(image, training=False)
    return {
        'preds': pred,
        'key': key
    }# Resave model, but specify new serving signature
KEYED_EXPORT_PATH = './keyed_model/'loaded_model.save(KEYED_EXPORT_PATH, signatures={'serving_default': keyed_prediction})
```

现在，当我们检查模型的服务签名时，我们会看到它将键作为输入和输出:

```
!saved_model_cli show --tag_set serve --signature_def serving_default --dir {KEYED_EXPORT_PATH}The given SavedModel SignatureDef contains the following input(s):
  inputs['image'] tensor_info:
      dtype: DT_FLOAT
      shape: (-1, 28, 28)
      name: serving_default_image:0
  inputs['key'] tensor_info:
      dtype: DT_STRING
      shape: (-1)
      name: serving_default_key:0
The given SavedModel SignatureDef contains the following output(s):
  outputs['key'] tensor_info:
      dtype: DT_STRING
      shape: (-1)
      name: StatefulPartitionedCall:0
  outputs['preds'] tensor_info:
      dtype: DT_FLOAT
      shape: (-1, 10)
      name: StatefulPartitionedCall:1
Method name is: tensorflow/serving/predict
```

您的模型服务现在将在任何预测调用中同时期待一个`image`张量和一个`key`，并将在其响应中输出`preds`和`key`。这种方法的一个好处是，您不需要访问生成模型的代码，只需要访问序列化的 SavedModel。

# 如何利用多个服务签名

有时保存具有两个服务签名的模型是方便的，或者是出于兼容性原因(即默认签名是未加密的)，或者是为了使单个服务基础结构可以处理加密和未加密的预测，并且用户决定执行哪一个。您需要从加载的模型中提取服务函数，并在再次保存时将其指定为服务签名之一:

```
inference_function = loaded_model.signatures['serving_default']loaded_model.save(DUAL_SIGNATURE_EXPORT_PATH, signatures={'serving_default': keyed_prediction,
    'unkeyed_signature': inference_function})
```

# 如何将输入要素转发到输出

出于模型调试的目的，您可能还希望转发某些输入要素，或者计算特定数据切片的评估指标(例如，根据婴儿是早产还是足月来计算婴儿体重的 RMSE)。

本例假设您使用了多个命名输入，如果您想利用 TensorFlow 特性列，您可以这样做，如这里的[所述](/how-to-build-a-wide-and-deep-model-using-keras-in-tensorflow-2-0-2f7a236b5a4b)。您的第一个选择是像往常一样训练模型，并利用 Keras Functional API 创建略有不同的模型签名，同时保持相同的权重:

```
tax_rate = Input(shape=(1,), dtype=tf.float32, name="tax_rate")
rooms = Input(shape=(1,), dtype=tf.float32, name="rooms")x = tf.keras.layers.Concatenate()([tax_rate, rooms])
x = tf.keras.layers.Dense(64, activation='relu')(x)
price = tf.keras.layers.Dense(1, activation=None, name="price")(x)# Functional API model instead of Sequential
model = Model(inputs=[tax_rate, rooms], outputs=[price])# Compile, train, etc...
#
#
#forward_model = Model(inputs=[tax_rate, rooms], outputs=[price, tax_rate])
```

另一种方法，如果您没有生成模型的代码，则特别有用，就是像使用键控预测模型一样修改服务签名:

```
@tf.function(input_signature=[tf.TensorSpec([None, 1], dtype=tf.float32), tf.TensorSpec([None, 1], dtype=tf.float32)])
def feature_forward_prediction(tax_rate, rooms):
    pred = model([tax_rate, rooms], training=False)
    return {
        'price': pred,
        'tax_rate': tax_rate
    }model.save(FORWARD_EXPORT_PATH, signatures={'serving_default': feature_forward_prediction})
```

尽情享受吧！

*感谢* [*拉克什马南*](https://medium.com/@lakshmanok) *帮我把他原来的估计器帖子更新到 Keras。*