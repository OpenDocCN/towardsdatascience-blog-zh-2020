# TensorFlow 中的调试

> 原文：<https://towardsdatascience.com/debugging-in-tensorflow-392b193d0b8?source=collection_archive---------8----------------------->

## 如何在不失去理智的情况下调试 TensorFlow 训练程序

![](img/82af9bfa4a1e78badc8bb23dcda675b6.png)

戴维·克洛德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

```
If debugging is the process of removing software bugs, then programming must be the process of putting them in.
Edsger Dijkstra. From https://www.azquotes.com/quote/561997
```

在我以前的一些帖子中([这里](https://medium.com/@julsimon/making-amazon-sagemaker-and-tensorflow-work-for-you-893365184233)、[这里](https://medium.com/@julsimon/deep-dive-on-tensorflow-training-with-amazon-sagemaker-and-amazon-s3-12038828075c)和[这里](/tensorflow-performance-analysis-314b56dceb59))，我告诉过你一些关于我在 Mobileye(官方名称为 Mobileye，英特尔公司)的团队如何使用 [TensorFlow](https://www.tensorflow.org/) 、亚马逊 SageMaker[和亚马逊 s3](https://aws.amazon.com/sagemaker/) 来训练我们的深度神经网络处理大量数据。在这篇文章中，我想谈谈 TensorFlow 中的调试。

众所周知，程序调试是软件开发中不可或缺的一部分，花费在调试上的时间常常超过了编写原始程序的时间。

调试是困难的，已经有很多关于如何设计和实现一个人的程序以增加错误的再现性，并简化根本原因分析的过程的文章。

在机器学习中，调试任务由于机器学习算法固有的随机性以及算法通常在远程机器上的专用硬件加速器上运行的事实而变得复杂。

TensorFlow 中的调试*由于使用了符号执行(也称为图形模式)而变得更加复杂，这提高了训练会话的运行时性能，但同时也限制了自由读取图形中任意张量的能力，这种能力对于调试非常重要。*

在这篇文章中，我将阐述调试 TensorFlow 训练程序的困难，并提供一些如何解决这些困难的建议。

出于法律目的，我想澄清，尽管我精心选择了副标题，但我不能保证我在这里写的任何东西都会防止你失去理智。相反，我认为我几乎可以保证，不管我写了什么，当你调试 TensorFlow 程序时，你可能会失去理智。但是，也许，你会少失去一点理智。

在我们开始之前，让我们澄清一下我们讨论的范围。

# 调试的类型

在本文的上下文中，调试指的是识别代码或数据中导致培训突然中断的错误的艺术。

另一种类型的调试(不在本文讨论范围之内)指的是修复或调整一个不收敛的模型，或者对某一类输入产生不令人满意的预测的模型(例如，车辆检测模型无法识别粉红色汽车)。该过程可能涉及定义和评估模型指标，收集和统计分析模型工件(如梯度、激活和权重)，使用工具，如 [TensorBoard](https://www.tensorflow.org/tensorboard) 和 [Amazon Sagemaker 调试器](https://medium.com/@chaimrand/debugging-in-tensorflow-392b193d0b8)，超参数调整，重新架构，或使用增强和增强等技术修改您的数据输入。调整模型可能是一项极具挑战性、耗时且常常令人沮丧的任务。

## 错误的类型

在解决代码或数据中的错误方面，我喜欢区分两类错误:*错误*和*怪物错误*。

通过*bug*我指的是相对容易重现的问题。*错误*的例子是对输入张量大小的假设与训练数据不匹配，试图连接不匹配的张量，或对无效数据类型执行 tf 运算。这些通常不依赖于特定的模型状态和数据，并且通常相对容易重现。它们不一定容易修复，但与怪物 bug 相比简直是小儿科。

*怪物 bug*是偶发的、不可预测的 bug。仅在模型的特定状态、特定数据样本或模型状态和数据输入的特定组合上重现的错误可能会带来严重的挑战，并可能构成一个*怪物错误*。

以下是一个基于真实事件的场景示例，它肯定会增加您的血压:

现在是星期五下午，你的模特已经成功训练了几天。损失似乎正在很好地融合，你开始想象一个放松的，释放后的周末假期，在你选择的地方。你回头看了一会儿屏幕，注意到突然之间，没有任何警告，你的损失变成了 NaN。“当然”，你对自己说，“这一定是由于一些完全随机的、瞬间的、宏观的故障”，然后你立即从你最后一个有效的模型检查点恢复训练。又过了几个小时，这种情况又发生了，一次又一次。现在你开始恐慌，你周末天堂的梦幻图片现在被需要解决一个*怪物错误*的诱人努力的想法所取代。

我们一会儿将回到这个令人悲伤的例子。但是首先，让我们检查一些强制性的“调试”复选框。

# Tensorflow 中的调试技巧

在调试的艺术上，更重要的是，在开发可调试代码的艺术上，已经花了很多笔墨。在本节中，我将提到一些与张量流应用相关的技术。这份清单并不全面。

## 保存模型检查点

这可能是我在这篇文章中写的最重要的东西。始终配置您的培训课程，使其定期保存您的模型的快照。

编程错误并不是你的训练失败的唯一原因...如果您在云中运行，您可能会得到一个现场实例终止，或者遇到一个内部服务器错误。如果您在本地运行，可能会停电，或者您的 GPU 可能会爆炸。如果你已经训练了几天，没有储存中间检查点，伤害可能是极端的。如果您每小时保存一个检查点，那么您最多丢失一个小时。TensorFlow 提供了用于存储检查点的实用程序，例如 [keras 模型检查点回调](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/ModelCheckpoint)。您需要做的只是，通过权衡存储检查点的开销和培训课程中意外中断的成本，来决定捕获此类快照的频率。

## 接触者追踪

我为我选择这一小节的标题向我的同辈人道歉，我实在忍不住了。通过联系跟踪，我指的是跟踪输入培训管道的培训数据的能力。

假设您的训练数据被分成 100，000 个 tfrecord 文件，其中一个文件有格式错误，导致您的程序崩溃或停止。缩小问题文件搜索范围的一种方法是记录进入管道的每个文件。一旦你点击崩溃，你可以回头看看你的日志，看看最近输入的文件是什么。正如我在以前的帖子中提到的，我们使用 Amazon SageMaker 管道模式功能进行训练。管道模式中最近增加的一项功能是管道模式服务器端日志，它记录了进入管道的文件。

记录进入 pipeline 的数据有助于提高重现 bug 的能力，这就引出了我们的下一点。

## 可复制性

bug 重现的容易程度直接影响到解决它的容易程度。我们总是想写我们的代码，以确保可重复性。这在 TensorFlow 程序中并不容易。机器学习应用通常依赖于随机变量的使用。我们随机初始化模型权重，我们随机增加数据，我们随机分割数据用于分布式训练，我们随机应用漏失，我们在每个时期之前混洗我们的输入数据，然后在创建批次之前再次混洗它(使用 tf.dataset.shuffle)。我们可以用我们记录的伪随机种子来播种所有的伪随机操作，但是请记住，可能有许多不同的地方引入了随机化，并且跟踪所有这些很容易成为簿记噩梦。我无法告诉你有多少次我认为我已经去除了随机化的所有元素，却发现我漏掉了一个。此外，还有一些无法植入的随机流程。如果使用多个过程来导入训练数据，您可能无法控制数据记录的实际输入顺序(例如，如果在 [tf.data.Options](https://www.tensorflow.org/api_docs/python/tf/data/Options) ())中将 experimental_deterministic 设置为 false)。当然，您可以在每个样本进入管道时对其进行记录，但这将会产生很高的开销，而且可能会令人望而却步。

底线是，虽然构建可重复的训练程序是绝对可能的，但我认为更明智的做法是接受非确定性，接受训练的不可重复性质，并找到克服这种调试限制的方法。

## 模块化程序设计

创建可调试程序的一个关键技术是以模块化的方式构建应用程序。应用于 TensorFlow 训练循环，这意味着能够分别测试训练管道的不同子集，如数据集、损失函数、不同的模型层和回调。这并不总是容易做到的，因为一些训练模块(如损失函数)非常依赖于其他模块。但是有很大的创造空间。例如，在应用数据集操作的子集时，可以通过简单地迭代数据集来测试输入管道上的不同函数。可以通过创建一个只运行损失函数或回调的应用程序来测试损失函数或回调。人们可以通过用虚拟损失函数代替它来抵消损失函数。我喜欢使用多个输出点来构建我的模型，即能够轻松修改模型中的层数，以便测试不同层的影响。

在构建程序时，你对程序的模块化和可调试性考虑得越多，你以后遭受的痛苦就越少。

## 急切的执行

如果您是 TensorFlow 的普通用户，您可能会遇到诸如“急切执行模式”、“图形模式”和“tf 函数限定符”之类的术语。你可能听过一些(有些误导)的说法，比如“在急切执行模式下调试是小菜一碟”，或者“tensorflow 2 在急切执行模式下运行”。你可能和我一样，狂热地一头扎进了 [tensorflow 源代码](https://github.com/tensorflow/tensorflow)，试图弄清楚不同的执行模式，结果却在抽泣中崩溃了，你的自尊从此粉碎。为了全面理解它是如何工作的，我向你推荐[张量流文档](https://www.tensorflow.org/guide/function)，祝你好运。这里我们将提到它的要点，因为它与调试有关。运行 TensorFlow 训练的最佳方式是在[图形模式](https://www.tensorflow.org/guide/intro_to_graphs)下运行。图形模式是一种符号执行模式，这意味着我们不能任意访问图形张量。用 [tf.function](https://www.tensorflow.org/api_docs/python/tf/function) 限定符包装的函数将在图形模式下运行。当您使用 [tf.keras.model.fit](https://www.tensorflow.org/api_docs/python/tf/keras/Model#fit) 训练时，默认情况下，训练步骤以图形模式执行。当然，不能访问任意的图形张量，使得在图形模式下调试很困难。在急切执行模式下，您可以访问任意张量，甚至可以使用调试器进行调试，(前提是您将断点放在 model.call()函数中的适当位置)。当然，当你在热切执行模式下运行时，你的训练会运行得慢很多。要对您的模型进行编程以在急切执行模式下进行训练，您需要调用函数 [model.compile()](https://www.tensorflow.org/api_docs/python/tf/keras/Model#compile) ,并将 run _ eagerly 标志设置为 true。

底线是，当你训练时，以图形模式运行，当你调试时，以急切执行模式运行。不幸的是，某些 bug 只在图形模式下重现而不在急切执行模式下重现的情况并不少见，这实在令人失望。此外，当您在本地环境中调试时，急切执行很有帮助，但在云中就不那么有用了。在调试*怪物 bug*的时候往往不是很有用...除非你首先找到一种方法在你的本地环境中重现这个 bug(下面会详细介绍)。

## TensorFlow 日志记录和调试实用程序

尽量利用 TensorFlow 测井仪。调试问题时，请将记录器设置为信息最丰富的级别。

[tf.debugging](https://www.tensorflow.org/api_docs/python/tf/debugging) 模块提供了一堆断言实用程序以及数字检查功能。特别是，[TF . debugging . enable _ check _ numerics](https://www.tensorflow.org/api_docs/python/tf/debugging/enable_check_numerics)实用程序有助于找出有问题的函数。

[tf.print](https://www.tensorflow.org/api_docs/python/tf/print) 函数能够打印出任意的图形张量，这是一个额外的实用程序，我发现它对调试非常有用。

最后但同样重要的是，添加您自己的打印日志(在代码的非图形部分)，以便更好地了解您的程序出故障的地方。

## 解密张量流错误消息

有时候，你会幸运地得到一个 TensorFlow 错误消息。不幸的是，如何使用它们并不总是一目了然。我经常收到同事发来的带有神秘 TensorFlow 信息的邮件，请求帮助。当我看到消息时，例如:

```
tensorflow.python.framework.errors_impl.InvalidArgumentError: ConcatOp : Dimensions of inputs should match: shape[0] = [5,229376] vs. shape[2] = [3,1]
```

或者

```
node DatasetToGraphV2 (defined at main.py:152) (1) Failed precondition: Failed to serialize the input pipeline graph: Conversion to GraphDef is not supported.
```

或者

```
ValueError: slice index -1 of dimension 0 out of bounds. for 'loss/strided_slice' (op: 'StridedSlice') with input shapes: [0], [1], [1], [1] and with computed input tensors: input[1] = <-1>, input[2] = <0>, input[3] = <1>.
```

我问自己(稍微修改了一下，使帖子对孩子友好)“我到底应该怎么做？”或者“为什么友好的 TensorFlow 工程师不能给我更多的工作呢？”。但我很快让自己平静下来，(有时借助酒精饮料)，并说:“Chaim，不要被宠坏了。回去工作吧，感谢你收到了任何信息。”你应该做的第一件事，是尝试在急切执行模式下重现错误，和/或使用调试器。不幸的是，如上所述，这并不总是有帮助。

无可争议的事实是，像上面这样的信息是没有多大帮助的。但是不要绝望。有时候，在一些调查工作的帮助下，你会发现一些线索，可能会把你引向正确的方向。仔细检查调用堆栈，看它是否提供了任何提示。如果信息包括形状大小，试着将它们与你的图形中可能具有相同形状的张量进行匹配。当然还有网上搜一下，看看别人有没有遇到过类似的问题，在什么场景下。不要绝望。

## 在本地环境中运行

自然，在本地环境中调试比在远程机器或云中调试更容易。当您第一次创建模型时尤其如此。你的目标应该是在开始远程训练之前，在你当地的环境中解决尽可能多的问题。否则，你很可能会浪费大量的时间和金钱。

为了提高可再现性，您应该尽量使您的本地环境与远程环境相似。如果您在远程环境中使用 docker 映像或虚拟环境，请尝试在本地使用相同的映像或虚拟环境。(如果你的远程培训是在亚马逊 SageMaker 上，你可以[调出所用的 docker 图片](https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-pull-ecr-image.html)。)

当然，远程培训环境中的某些要素可能无法在本地复制。例如，你可能遇到了一个只有在使用[亚马逊 SageMaker 管道模式](https://aws.amazon.com/blogs/machine-learning/using-pipe-input-mode-for-amazon-sagemaker-algorithms/)时才会重现的 bug，目前只有在云中运行时才支持。(在这种情况下，您可以考虑使用[替代方法从 s3](https://medium.com/@julsimon/deep-dive-on-tensorflow-training-with-amazon-sagemaker-and-amazon-s3-12038828075c) 访问您的数据。)

我希望我能告诉你，这里描述的技术将解决你所有的问题。但是，唉，事实并非如此。在下一节中，我们将回到我们上面举例说明的*怪物 bug* 场景，并介绍最后一种调试技术。

# 使用 TensorFlow 自定义训练循环进行调试

在我们上面描述的场景中，经过几天的训练，模型的特定状态和特定训练批次样本的组合，突然导致损失变成 NaN。

让我们评估一下如何使用上面的调试技术来调试这个问题。

*   如果我们对用于所有随机操作的种子保持细致的跟踪，并且没有不受控制的非确定性事件，那么理论上我们可以通过从头开始训练来重现 bug...但那需要几天时间。
*   在本地环境中或在急切执行模式下进行复制可能需要几周时间。
*   我们可以从最近的检查点恢复，但是如果我们可以从完全相同的样本恢复，并且具有所有伪随机生成器的完全相同的状态，我们将只能再现相同的模型状态和批样本。
*   添加 [tf.prints](https://www.tensorflow.org/api_docs/python/tf/print) 会有所帮助，但是会带来巨大的开销
*   添加[TF . debugging . enable _ check _ numerics](https://www.tensorflow.org/api_docs/python/tf/debugging/enable_check_numerics)将非常有助于查明它失败的函数。如果函数中有明显的 bug，这可能就足够了。但是这并不能让我们重现这个错误。

理想情况下，我们将能够在损失变得不可收拾之前捕获输入和模型状态。然后，我们可以在受控(本地)环境中，以急切执行模式和调试器重现该问题。

问题是我们不知道问题将要发生，直到它真正发生。当损失被报告为 NaN 时，模型已经用 NaN 权重更新，并且导致错误的批次样本已经被迭代。

我想提出的解决方案是定制训练循环，以便我们记录每一步的当前样本，并且仅在梯度有效时更新模型权重。如果梯度无效，我们将停止训练并连同当前模型快照一起转储出最后一批样本。这可以带到您的本地环境中，在那里您加载模型，并在急切执行模式下输入捕获的数据样本，以便重现(并解决)bug。

我们一会儿将讨论代码，但是首先，说几句使用定制训练循环的利与弊。

## 定制培训循环与高级 API

TensorFlow 用户之间有一个由来已久的争议，即是否要编写定制的训练循环或依赖于高级 API，如 [tf.keras.model.fit](https://www.tensorflow.org/api_docs/python/tf/keras/Model#fit) ()。

定制培训循环的支持者，预示着对如何执行培训进行逐行控制的能力，以及创造性的自由。高级 API 的支持者称它提供了许多便利，最显著的是内置的回调实用程序和分布式策略支持。使用高级 API 还可以确保您使用的是一个无错误的、高度优化的训练循环实现。

从 2.2 版本开始，TensorFlow 引入了覆盖 [tf.keras.model](https://www.tensorflow.org/api_docs/python/tf/keras/Model) 类的 [train_step](https://www.tensorflow.org/api_docs/python/tf/keras/Model#train_step) 和 [make_train_function](https://www.tensorflow.org/api_docs/python/tf/keras/Model#make_train_function) 例程的能力。这使用户能够引入某种程度的定制，同时继续享受 model.fit()的便利。我们将演示如何以这样一种方式覆盖这些函数，使我们能够捕获有问题样本输入和模型状态，以便进行本地调试。

## 自定义采集循环

在下面的代码块中，我们使用 train_step 和 make_train_functions 例程的自定义实现来扩展 tf.keras.models.Model 对象。为了全面理解这个实现，我建议您将它与 github 中例程的[默认实现进行比较。您会注意到，为了使代码更具可读性，我删除了所有与指标计算和策略支持相关的逻辑。需要注意的主要变化是:](https://github.com/tensorflow/tensorflow/blob/v2.3.0/tensorflow/python/keras/engine/training.py#L716-L760)

*   在将梯度应用到模型权重之前，我们测试 NaN 的梯度。只有当 NaN 不出现时，渐变才会应用于权重。否则，向训练循环发送遇到错误的信号。信号的一个例子可以是将损耗设置为预定值，例如零或 NaN。
*   训练循环存储每一步的数据特征和标签(x 和 y)。注意，为了做到这一点，我们将数据集遍历(下一个(迭代器)调用)移到了@tf.function 范围之外。
*   该类有一个布尔“崩溃”标志，通知主函数是否遇到了错误。

```
**class** **CustomKerasModel**(tf.keras.models.Model):
    **def** __init__(self, **kwargs):
        super(CustomKerasModel, self).__init__(**kwargs)

        *# boolean flag that will signal to main function that 
        # an error was encountered*
        self.crash = **False**

    @tf.function
    **def** train_step(self, data):
        x, y = data

        **with** tf.GradientTape() **as** tape:
            y_pred = self(x, training=**True**)  *# Forward pass*
            *# Compute the loss value*
            *# (the loss function is configured in `compile()`)*
            loss = self.compiled_loss(
                     y, y_pred, regularization_losses=self.losses)

        *# Compute gradients*
        trainable_vars = self.trainable_variables
        gradients = tape.gradient(loss, trainable_vars)

        *# concatenate the gradients into a single tensor for testing*
        concat_grads = 
                tf.concat([tf.reshape(g,[-1]) **for** g **in** gradients],0)

        *# In this example, we test for NaNs, 
        # but we can include other tests*
        **if** tf.reduce_any(tf.math.is_nan(concat_grads)):
            *# if any of the gradients are NaN, send a signal to the  
            # outer loop and halt the training. We choose to signal
            # to the outer loop by setting the loss to 0.*
            **return** {'loss': 0.}
        **else**:
            *# Update weights*
            self.optimizer.apply_gradients(
                       zip(gradients, trainable_vars))
            **return** {'loss': loss}

    **def** make_train_function(self):
        **if** self.train_function **is** **not** **None**:
            **return** self.train_function

        **def** train_function(iterator):
            data = next(iterator)
            *# records the current sample*
            self.x, self.y = data
            res = self.train_step(data)
            **if** res['loss'] == 0.:
                self.crash = **True**
                **raise** **Exception**()
            **return** res

        self.train_function = train_function
        **return** self.train_function

**if** __name__ == '__main__':

    *# train_ds =* 
    *# inputs =* 
    *# outputs =*
    *# optimizer =*
    *# loss =* 
    *# epochs =*
    *# steps_per_epoch =* 
    model = CustomKerasModel(inputs=inputs, outputs=outputs)
    opt = tf.keras.optimizers.Adadelta(1.0)

    model.compile(loss=loss, optimizer=optimizer)

    **try**:
        model.fit(train_ds, epochs=epochs,                         
                  steps_per_epoch=steps_per_epoch)
    **except** **Exception** **as** e:
        *# check for signal*
        **if** model.crash:
            model.save_weights('model_weights.ckpt')
            *# pickle dump model.x and model.y*
            features_dict = {}
            **for** n, v **in** model.x.items():
                features_dict[n] = v.numpy()
            **with** open('features.pkl','wb') **as** f:
                pickle.dump(features_dict,f)
            labels_dict = {}
            **for** n, v **in** model.y.items():
                labels_dict[n] = v.numpy()
            **with** open('labels.pkl', 'wb') **as** f:
                pickle.dump(labels_dict, f)
            **raise** e
```

值得注意的是，这种技术有一个小的训练运行时成本，它来自于以急切执行模式而不是图形模式从数据集中读取数据。(天下没有免费的午餐。)精确的成本将取决于模型的大小；模型越大，感觉到的变化就越小。您应该在自己的模型上评估这种技术的开销，然后决定是否以及如何使用它。

# 摘要

只要我们人类参与到人工智能应用的开发中，编程错误的流行几乎是肯定的。在设计代码时考虑到可调试性，并获得解决 bug 的工具和技术，可能会防止一些严重的问题。

最重要的是，不要绝望。