# 在 tf.keras / TF2 中使用自定义验证指标的三种方式

> 原文：<https://towardsdatascience.com/three-ways-to-use-custom-validation-metrics-in-tf-keras-tf2-bb9c40a3076?source=collection_archive---------17----------------------->

## 如何在 TensorFlow 2 中使用自定义验证指标

![](img/ee83f7a55333b247e01a6591e05362f9.png)

照片由 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的[米切尔·布茨](https://unsplash.com/@valeon)拍摄

Keras 提供了一系列指标来验证测试数据集，如准确性、MSE 或 AUC。然而，有时您需要一个定制的指标来验证您的模型。在这篇文章中，我将展示三种不同的方法来实现您的指标并在 Keras 中使用它。

虽然最初 Keras 中只包含少数指标，但现在已经有了很多不同的指标。对于 TensorFlow 2 捆绑的 Keras 版本，所有指标都可以在 [tf.keras.metrics](https://www.tensorflow.org/api_docs/python/tf/keras/metrics) 中找到。

## 使用 tensorflow 插件

Tensoflow 插件库提供了一些额外的指标。在开始自己实现之前，最好检查一下你的度量标准是否可用。要使用 tensorflow 插件，只需通过 pip 安装即可:

```
pip install tensorflow-addons
```

如果您没有找到您的指标，我们现在可以看看这三个选项。函数、回调和度量对象。

## 简单的度量函数

在 Keras 中定义指标最简单的方法是简单地使用函数回调。该函数有两个参数。第一个参数是地面实况(y_true ),第二个参数是来自模型的预测(y_pred)。在运行验证时，这些参数是张量，所以我们必须使用 Keras 后端进行计算。

```
import tf.keras.backend as K
def matthews_correlation(y_true, y_pred):
    y_pred_pos = K.round(K.clip(y_pred, 0, 1))
    y_pred_neg = 1 - y_pred_pos

    y_pos = K.round(K.clip(y_true, 0, 1))
    y_neg = 1 - y_pos

    tp = K.sum(y_pos * y_pred_pos)
    tn = K.sum(y_neg * y_pred_neg)

    fp = K.sum(y_neg * y_pred_pos)
    fn = K.sum(y_pos * y_pred_neg)

    numerator = (tp * tn - fp * fn)
    denominator = K.sqrt((tp + fp) * (tp + fn) * (tn + fp) * (tn + fn))

    return numerator / (denominator + K.epsilon())
```

将此函数用于 Keras 模型:

```
model.compile(optimizer='rmsprop',
              loss='binary_crossentropy',
              metrics=[matthews_correlation])
```

调用 fit()现在输出:

```
245063/245063 [==============================] - 63s 256us/step - matthews_correlation: 0.0032 - val_matthews_correlation: 0.0039(Note that the name is the function name, while for validation data there is always the val_ prefix)
```

## 使用回调的间隔评估

如果您想要每 N 个时期在一个单独的数据集上评估模型，您可以使用自定义回调。在这种情况下，使用来自 [scikit-learn](https://scikit-learn.org/stable/) 的 AUC 分数。

```
from sklearn.metrics import roc_auc_score
from tf.keras.callbacks import Callback

class IntervalEvaluation(Callback):
    def __init__(self, validation_data=(), interval=10):
        super(Callback, self).__init__()

        self.interval = interval
        self.X_val, self.y_val = validation_data

    def on_epoch_end(self, epoch, logs={}):
        if epoch % self.interval == 0:
            y_pred = self.model.predict_proba(self.X_val, verbose=0)
            score = roc_auc_score(self.y_val, y_pred)
            print("interval evaluation - epoch: {:d} - score: {:.6f}".format(epoch, score))
```

为了用这个评估设置训练模型，对象的实例作为[回调](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/Callback)传递给 Keras。

```
ival = IntervalEvaluation(validation_data=(x_validate, y_validate), interval=10)
model.fit(x_train, y_train,
          batch_size=8196,
          epochs=25,
          validation_data=[x_test, y_test],   
          class_weight=class_weight,
          callbacks=[ival],
          verbose=1 )
```

结果是这样的:

```
interval evaluation - epoch: 0 - score: 0.545038
interval evaluation - epoch: 10 - score: 0.724098
interval evaluation - epoch: 20 - score: 0.731381
```

## 扩展 tf.keras.metrics.Metric

最后，可以扩展度量对象本身。(从[堆栈溢出](https://stackoverflow.com/questions/59305514/tensorflow-how-to-use-tf-keras-metrics-in-multiclass-classification)到 Geeocode 的积分)

```
class CategoricalTruePositives(tf.keras.metrics.Metric):
    def __init__(self, num_classes, batch_size,
                 name="categorical_true_positives", **kwargs):
        super(CategoricalTruePositives, self).__init__(name=name, **kwargs)
        self.batch_size = batch_size
        self.num_classes = num_classes    
        self.cat_true_positives = self.add_weight(name="ctp", initializer="zeros")
    def update_state(self, y_true, y_pred, sample_weight=None):     
        y_true = K.argmax(y_true, axis=-1)
        y_pred = K.argmax(y_pred, axis=-1)
        y_true = K.flatten(y_true)
        true_poss = K.sum(K.cast((K.equal(y_true, y_pred)), dtype=tf.float32))
        self.cat_true_positives.assign_add(true_poss)
    def result(self):
        return self.cat_true_positives
```

与第一个例子只有一个显著的不同。结果函数返回，update_state 方法必须被覆盖(这是计算发生的地方)。它还需要一个额外的参数(sample_weight)。最后用 re result 方法返回结果张量。

```
model.compile(optimizer='adam',
              loss='binary_crossentropy',
              metrics=[CategoricalTruePositives(num_classes, batch_size)])
```

我们已经看到了实现定制验证指标的三种不同方式。希望这有助于决定哪种方式适合您的用例，并且不要忘记检查您的度量是否已经在 tf.keras.metrics 中的大量预定义的度量中可用。

这篇文章最初发表于[digital-thinking . de](http://digital-thinking.de/keras-three-ways-to-use-custom-validation-metrics-in-keras/)(2018 年 12 月 19 日)