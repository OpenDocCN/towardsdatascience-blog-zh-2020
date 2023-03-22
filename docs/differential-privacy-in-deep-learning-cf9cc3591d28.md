# 深度学习中的差分隐私

> 原文：<https://towardsdatascience.com/differential-privacy-in-deep-learning-cf9cc3591d28?source=collection_archive---------14----------------------->

## 关于如何将差分隐私集成到图像分类的深度学习架构中的实用指南

![](img/971673ff4d6f070dd87488c21de5a565.png)

[马太·亨利](https://unsplash.com/@matthewhenry?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

我要感谢[阿克谢·库尔卡尼](https://www.linkedin.com/in/akshay-kulkarni-1a562679/)先生在我发表我的第一篇文章的征途上指引我。

# 介绍

随着我们大量的日常活动被转移到网上，被记录的个人和敏感数据的数量也在增加。数据的激增也导致了机器学习和深度学习形式的数据分析工具的增加，这些工具正在渗透到每个可能的行业。这些技术也用于敏感用户数据，以获得可操作的见解。这些模型的目标是发现整体模式，而不是个人习惯。

![](img/a82fac4cc9e4a14822626be1b856efc3.png)

凯文·Ku 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

深度学习正在发展成为许多自动化程序的行业标准。但它也因学习训练数据集的微小细节而臭名昭著。这加剧了隐私风险，因为模型权重现在编码了更精细的用户细节，这些细节在恶意检查时可能会泄露用户信息。例如，弗雷德里克松等人演示了一种从面部识别系统中恢复图像的模型反转攻击[1]。给定大量可免费获得的数据，可以有把握地假设，确定的对手可以获得从模型权重中提取用户信息所需的必要辅助信息。

# 什么是差分隐私？

差分隐私是一种为我们提供用户信息隐私的某些数学保证的理论。它旨在减少任何一个人的数据对整体结果的影响。这意味着人们会对一个人的数据做出同样的推断，不管它是否出现在分析的输入中。随着数据分析数量的增加，暴露用户信息的风险也在增加。差分保密计算的结果不受各种隐私攻击的影响。

![](img/14e9cd644cbdbae907535e00fd8d5ae5.png)

这里 X 是一个个体。图片作者。

这是通过在计算过程中添加精心调整的噪声(由*ε*表征)来实现的，使黑客难以识别任何用户。这种噪声的增加也导致计算精度的下降。因此，在准确性和提供的隐私保护之间存在权衡。隐私级别由*ε*来衡量，并与提供的隐私保护程度成反比。这意味着ε越高，数据的保护程度越低，泄露用户信息的可能性就越大。实现ε差分隐私是一种理想情况，在实际场景中很难实现，因此使用(ε，δ)-差分隐私。通过使用(ε，δ)-差分隐私，该算法是ε-差分隐私，具有概率*(1-δ)。因此，δ越接近 0 越好。Delta 通常被设置为训练样本数量的倒数。*

*![](img/f88c87b328cb3e21c8f6d7f5efe4b4f3.png)*

*渐变操作。作者图片*

*请注意，我们的目标是保护模型而不是数据本身免受恶意检查。这是通过在模型权重的计算期间添加噪声来实现的，这具有模型正则化的额外优点。具体来说，在深度学习中，我们通过选择差分隐私优化器来集成差分隐私，因为这是大多数计算发生的地方。梯度首先通过获取损失相对于重量的梯度来计算。然后根据 *l2_norm_clip* 参数对这些梯度进行剪切，并添加噪声，这由 [TensorFlow 隐私库](https://github.com/tensorflow/privacy)中的*噪声 _ 乘数*参数控制。*

*我们的目标是通过以差分隐私—随机梯度下降(DP-SGD)优化器的形式应用差分隐私来保持模型权重的隐私性。这里尝试将差分隐私应用于深度神经网络 [*VGG19*](https://github.com/BIGBALLON/cifar-10-cnn/blob/master/3_Vgg19_Network/Vgg19_keras.py) ，其任务是对 *CIFAR10* 数据集进行图像分类，以了解对模型性能和隐私的影响。如果这样的模型被训练在敏感和私人的图像上，那么这些图像不被泄露将变得非常重要，因为它们可能被恶意使用。*

# *DP-SGD 的实施*

*[TensorFlow Privacy](https://github.com/tensorflow/privacy) 库用于实现 DP-SGD 优化器和计算 epsilon。超参数不是非常精确地调整以获得基准的最大精度，因为我们只需要它进行比较研究，过度调整可能会扭曲预期的比较。计算ε的函数将'*步数'*作为输入，计算为*(历元*训练样本数)/批量*。*步数*是模型查看训练数据次数的度量。随着步数的增加，模型看到的训练数据越多，它会通过过度拟合将更好的细节纳入自身，这意味着模型有更高的机会暴露用户信息。*

```
*num_classes = 10# data loading
x_train, y_train), (x_test, y_test) = cifar10.load_data()
x_train = x_train.astype('float32')
x_test = x_test.astype('float32')
y_train = tf.keras.utils.to_categorical(y_train, num_classes)
y_test = tf.keras.utils.to_categorical(y_test, num_classes)# network params
num_classes  = 10
batch_size   = 128
dropout      = 0.5
weight_decay = 0.0001
iterations   = len(x_train) // batch_size# dpsgd params
dpsgd = True
learning_rate = 0.15
noise_multiplier = 1.0
l2_norm_clip = 1.0
epochs = 100
microbatches = batch_sizeif dpsgd:
   optimizer = DPGradientDescentGaussianOptimizer(
   l2_norm_clip=l2_norm_clip,
   noise_multiplier=noise_multiplier,
   num_microbatches=microbatches,
   learning_rate=learning_rate)
   loss = tf.keras.losses.CategoricalCrossentropy(
   reduction = tf.compat.v1.losses.Reduction.NONE)
   # reduction is set to NONE to get loss in a vector form
   print('DPSGD')else:
   optimizer = optimizers.SGD(lr=learning_rate)
   loss = tf.keras.losses.CategoricalCrossentropy()
   print('Vanilla SGD')model.compile(loss=loss, optimizer=optimizer, metrics=['accuracy'])
# where model is the neural network architecture you want to use# augmenting images
datagen = ImageDataGenerator(horizontal_flip=True,
          width_shift_range=0.125, 
          height_shift_range=0.125,        
          fill_mode='constant',cval=0.)datagen.fit(x_train)# start training
model.fit(datagen.flow(x_train,y_train,batch_size=batch_size), steps_per_epoch=iterations,epochs=epochs,validation_data=(x_test, y_test))if dpsgd:
   eps = compute_epsilon(epochs * len(x_train) // batch_size)
   print('Delta = %f, Epsilon = %.2f'(1/(len(x_train)*1.5),eps))
else:
   print('Trained with vanilla non-private SGD optimizer')*
```

*基准使用 Keras 库提供的 SGD，学习率与 DP-SGD 相同。网络参数保持不变，以便进行公平的比较。*

*注意:由于 DP-SGD 将权重衰减作为一个参数，因此将权重衰减应用于神经网络的每一层都会出现错误。*

*![](img/2278a83bee2ed10aff6b549fcab20cdf.png)*

*作者图片*

*一些观察结果:*

*   *使用 DP-SGD 的模型训练比基准慢。*
*   *使用 DP-SGD 的模型训练比基准更嘈杂，这可能是由于梯度削波和噪声的添加。*
*   *最终，与基准测试相比，具有 DP-SGD 的模型实现了相当不错的性能。*

# *处理不平衡数据*

*我们试图通过制作不平衡的数据集，然后训练模型来模拟真实世界的场景。这是通过向每个类随机分配用户定义的权重来实现的。我们人为地制造了一个严重的不平衡，导致模型表现不佳。为了克服这种不平衡，我们采用数据级方法，而不是对模型本身进行更改。实例数量的减少导致ε增加。使用合成少数过采样技术(SMOTE)进行过采样后，我们看到精度有所提高。这使我们能够在不改变模型本身的情况下处理不平衡。*

*每个类的实例数(共 10 个类):
[3000 3500 250 2000 1000 1500 500 50 5000 2500]*

```
*def imbalance(y, weights, x_train, y_train):
    random.shuffle(weights)
    indices = []
    for i in range(10):
       inds = np.where(y==i)[0]
       random.shuffle(inds)
       print(i,int(weights[i]*len(x_train)))
       indices += list(inds[0:int(weights[i]*len(x_train))])
       x_train = x_train[indices]
       y_train = y_train[indices]
    return x_train, y_trainweights = [0.01,0.05,0.1,0.2,0.3,0.4,0.5,0.6,0.7,1]
x_train, y_train = imbalance(y_train,weights,x_train, y_train)# Implementing SMOTE
from imblearn.over_sampling import SMOTE
smote = SMOTE(random_state=42)
x_train = x_train.reshape(len(x_train),32*32*3) 
# where shape of image is (32,32,3)
x_train, y_train = smote.fit_resample(x_train, y_train)
x_train = x_train.reshape(len(x_train),32,32,3)*
```

*![](img/4df38bbfe41dd736c387f694bcccb5b9.png)*

*我们注意到:*

*   *使用 SMOTE 对数据进行过采样后，模型性能比在不平衡数据集上训练的模型有所提高。*
*   *基于原始平衡数据训练的模型取代了基于过采样数据训练的模型的性能。*

*从实验中，我们观察到数据级方法足以解决不平衡的问题，即使是有差别的私有优化器。*

# *调整 NOISE_MULTIPLIER 参数*

*张量流隐私中对ε有直接影响的超参数是噪声乘数。*

*![](img/ceca944465e94cd4ed7b185fae72c98c.png)*

*作者图片*

*噪声= 100.0，ε= 0.18
噪声= 10.0，ε= 0.36
噪声= 1.0，ε= 3.44*

*我们注意到:*

*   *增加噪声会降低ε的值，这意味着更好的隐私保护。*
*   *众所周知，隐私保护级别与模型性能成反比*
*   *但是我们在实践中没有观察到这种关系，并且增加噪声乘数对性能几乎没有影响。*

# *结论*

*在调整*噪声乘数*时发现的与理论的偏离可能归因于模型的深度较大，其中梯度操作无效。从 TensorFlow 隐私源中给出的代码中，我们看到ε的值独立于模型训练，仅取决于我们选择的*噪声 _ 乘数*、*批量 _ 大小*、*时期*和*增量*值。Epsilon 有很好的理论支持来衡量隐私风险的大小。它考虑了在确定模型查看训练数据的次数时很重要的所有因素，并且直观地感觉到模型查看数据越多，用户信息暴露于模型权重的风险就越大。但它仍然缺乏实际测量模型重量对恶意检查的安全性。这是一个令人担忧的原因，因为现在我们不知道我们的模型权重是否对黑客攻击免疫。现在需要一个度量标准来衡量模型权重的不透明程度，即它们透露的用户信息有多少。*

# *未来的工作*

*正如结论中所讨论的，首先也是最重要的是提出一个衡量标准来判断模型的隐私性。它甚至可能是一个试图从模型权重中获取用户信息的攻击模型。模型能够正确识别的训练数据点的数量可以量化为隐私风险。一旦一个可靠的度量被形式化，我们就可以着手将不同的私有优化器扩展到计算机视觉和自然语言处理中更复杂的任务，以及其他架构。*

# *参考*

1.  *米（meter 的缩写））弗雷德里克松、s·贾和 t·里斯坦帕尔。[利用置信度信息和基本对策的模型反转攻击](https://dl.acm.org/doi/10.1145/2810103.2813677)。*见 CCS，第 1322-1333 页。2015 年 ACM* 。*
2.  *Abadi，Martin 等.“[深度学习与差异隐私](https://arxiv.org/abs/1607.00133)”*2016 年 ACM SIGSAC 计算机与通信安全会议论文集*。2016.*
3.  *张量流隐私，[https://github.com/tensorflow/privacy](https://github.com/tensorflow/privacy)*