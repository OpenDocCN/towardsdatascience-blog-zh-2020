# Keras 中的自定义指标以及它们在 tensorflow2.2 中的使用有多简单

> 原文：<https://towardsdatascience.com/custom-metrics-in-keras-and-how-simple-they-are-to-use-in-tensorflow2-2-6d079c2ca279?source=collection_archive---------19----------------------->

## 使用 Keras 和 tensorflow2.2 无缝添加用于深度神经网络训练的复杂指标

Keras 已经大大简化了基于 DNN 的机器学习，并且越来越好。在这里，我们展示了如何基于混淆矩阵(回忆、精度和 f1)实现指标，并展示了在 tensorflow 2.2 中如何简单地使用它们。可以直接在 Google Colab 中运行[笔记本](https://colab.research.google.com/github/borundev/ml_cookbook/blob/master/Custom%20Metric%20(Confusion%20Matrix)%20and%20train_step%20method.ipynb)。

当考虑一个多类问题时，人们常说，如果类不平衡，准确性就不是一个好的度量。虽然这肯定是真的，但是当所有的类训练得不一样好时，即使数据集是平衡的，准确性也是一个糟糕的度量。

在这篇文章中，我将使用时尚 MNIST 来突出这一方面。然而，这不是本文的唯一目标，因为这可以通过在培训结束时在验证集上简单地绘制混淆矩阵来实现。我们在这里讨论的是轻松扩展`keras.metrics.Metric`类的能力，以便在训练期间生成一个跟踪混淆矩阵的指标，并可用于跟踪特定类的召回率、精确度和 f1，并以 keras 的常用方式绘制它们。

在训练期间获得特定类别的召回率、精确度和 f1 至少有两个好处:

1.  我们可以看到训练是否稳定——图不会跳得太多——在每个职业的基础上
2.  我们可以基于类统计实现更定制的训练——提前停止甚至动态改变类权重。

此外，自 tensorflow 2.2 以来，由于新的模型方法`train_step`和`test_step`，将这种自定义指标集成到训练和验证中变得非常容易。还有一个助手`predict_step`，我们在这里没有用到，但也以同样的精神工作。

所以让我们言归正传。我们首先创建一个定制的度量类。虽然还有更多的步骤，并在参考的 [jupyter 笔记本](https://colab.research.google.com/github/borundev/ml_cookbook/blob/master/Custom%20Metric%20(Confusion%20Matrix)%20and%20train_step%20method.ipynb)中显示，但重要的是实现与 Keras 培训和测试工作流的其余部分集成的 API。这就像实现和`update_state`一样简单，它接受真正的标签和预测，一个`reset_states`重新初始化指标。

```
class ConfusionMatrixMetric(tf.keras.metrics.Metric):

    def update_state(self, y_true, y_pred,sample_weight=None):
        self.total_cm.assign_add(self.confusion_matrix(y_true,y_pred))
        return self.total_cm

    def result(self):
        return self.process_confusion_matrix()

    def confusion_matrix(self,y_true, y_pred):
        """
        Make a confusion matrix
        """
        y_pred=tf.argmax(y_pred,1)
        cm=tf.math.confusion_matrix(y_true,y_pred,dtype=tf.float32,num_classes=self.num_classes)
        return cm

    def process_confusion_matrix(self):
        "returns precision, recall and f1 along with overall accuracy"
        cm=self.total_cm
        diag_part=tf.linalg.diag_part(cm)
        precision=diag_part/(tf.reduce_sum(cm,0)+tf.constant(1e-15))
        recall=diag_part/(tf.reduce_sum(cm,1)+tf.constant(1e-15))
        f1=2*precision*recall/(precision+recall+tf.constant(1e-15))
        return precision,recall,f1 
```

在正常的 Keras 工作流中，将调用方法`result`,它将返回一个数字，不需要做任何其他事情。然而，在我们的例子中，我们有三个返回的精度、召回和 f1 的张量，Keras 不知道如何开箱即用。这就是 tensorflow 2.2 新特性的用武之地。

自 tensorflow 2.2 以来，可以透明地修改每个训练步骤(即小批量训练)中发生的事情(而在早期，必须编写一个在自定义训练循环中调用的无界函数，并且必须用 tf.function 对其进行修饰以实现自动签名)。

同样，细节在参考的 [jupyter 笔记本](https://colab.research.google.com/github/borundev/ml_cookbook/blob/master/Custom%20Metric%20(Confusion%20Matrix)%20and%20train_step%20method.ipynb)中，但关键是以下几点

```
class MySequential(keras.Sequential):

    def train_step(self, data):
        # Unpack the data. Its structure depends on your model and
        # on what you pass to `fit()`. x, y = data with tf.GradientTape() as tape:
            y_pred = self(x, training=True)  # Forward pass
            # Compute the loss value.
            # The loss function is configured in `compile()`.
            loss = self.compiled_loss(
                y,
                y_pred,
                regularization_losses=self.losses,
            ) # Compute gradients
        trainable_vars = self.trainable_variables
        gradients = tape.gradient(loss, trainable_vars) # Update weights
        self.optimizer.apply_gradients(zip(gradients, trainable_vars))
        self.compiled_metrics.update_state(y, y_pred)
        output={m.name: m.result() for m in self.metrics[:-1]}
        if 'confusion_matrix_metric' in self.metrics_names:
            self.metrics[-1].fill_output(output)
        return output
```

就是这样。现在你可以创建(使用上面的类而不是 keras。Sequential)，编译并装配一个顺序模型(处理函数式和子类化 API 的过程非常简单，只需实现上述函数即可)。产生的历史现在有像`val_F1_1`等元素。

这样做的好处是我们可以看到各个班级是如何训练的

![](img/7009f3f6c49f44776b4c3c608721144c.png)

火车和 val F1s 的演变为 10 级时装 MNIST 作为时代的进步

我们看到第 6 类训练非常糟糕，在验证集上 F1 大约为 0.6，但是训练本身是稳定的(图没有太多跳跃)。

正如在开始时提到的，在培训期间获得每个类的指标至少有两个好处:

1.  我们可以看到训练是否稳定——图不会跳得太多——在每个职业的基础上
2.  我们可以基于基于类统计的早期停止或者甚至动态改变类权重来实现更定制的训练。

最后，让我们看看困惑矩阵，看看第 6 类发生了什么

![](img/508ca2911ed885e8a1bc9694ed27a863.png)

时尚 MNIST 的困惑矩阵

在混淆矩阵中，真实类在 y 轴上，预测类在 x 轴上。我们看到衬衫(6)被错误地标注为 t 恤(0)、套头衫(2)和外套(4)。相反，贴错标签的衬衫大多发生在 t 恤上。这种类型的错误是合理的，我将在另一篇文章中讨论在这种情况下如何改进培训。

参考:

1.  [github 上的代码](https://github.com/borundev/ml_cookbook/blob/master/Custom%20Metric%20(Confusion%20Matrix)%20and%20train_step%20method.ipynb)
2.  谷歌 Colab [笔记本](https://colab.research.google.com/github/borundev/ml_cookbook/blob/master/Custom%20Metric%20(Confusion%20Matrix)%20and%20train_step%20method.ipynb)