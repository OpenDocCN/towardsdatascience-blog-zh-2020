# 仅用 40 行代码构建一个卷积神经网络

> 原文：<https://towardsdatascience.com/building-a-convolutional-neural-network-in-only-40-lines-of-code-bef8ce38bf6d?source=collection_archive---------39----------------------->

## 最简单的 CNN 可能使用 Keras 和 Tensorflow

![](img/3d89b121d8a5caddd4cc08fdfb246f93.png)

[来源](https://unsplash.com/s/photos/monkeys)

对于任何进入深度学习的人来说，卷积神经网络都可能令人困惑和恐惧。在本文中，我将展示构建一个简单的 CNN 实际上是相当容易的。

首先，我们需要导入模块。

```
import numpy as np
import pandas as pd
import tensorflow as tf
from tensorflow.keras import datasets, layers, models
from keras.preprocessing.image import ImageDataGenerator
```

Keras 是一个运行在 Tensorflow 之上的高级 Python 神经网络库。其简单的架构、可读性和整体易用性使其成为使用 Python 进行深度学习时最受欢迎的库之一。

在这篇文章中，我使用了“10 种猴子”的数据集，可以在 Kaggle 上找到:https://www.kaggle.com/slothkong/10-monkey-species。它包含 1098 个训练图像和 272 个验证图像，分在 10 类猴子中。

在我们开始之前，请确保图像存储正确。下面看到的结构是 flow_from_directory 函数(我们很快就会讲到)工作所必需的。

```
Directory
   - Training_images
      - Class_1
         - image_1
         - image_2
      - Class_2
         - image_1 - Validation_images
      - Class_1
         - image_1
...
```

现在我们开始。首先，我们必须指定图像的路径，以及它们的目标大小。

```
*#****Path*** 
train_dir = Path('../input/10-monkey-species/training/training/')
test_dir = Path('../input/10-monkey-species/validation/validation/')

***#Images target size*** target_size = (100,100)
channels = 3 *#RGB*
```

然后，我们创建我们的生成器，这将使我们能够对图像进行数据扩充和缩放。请注意，只增加训练图像，但重新缩放一切。重新缩放是必要的，因为让每个图像都在相同的[0，1]范围内将意味着它们在训练期间的贡献更均匀。

作为澄清，请注意 ImageDataGenerator 函数不会创建新图像。相反，它修改了我们当前的一些训练图像，以便在更大范围的样本上训练该模型。这有助于避免过度拟合，并使模型更易于预测新猴子的类别。

```
***#Data augmentation*** train_generator = ImageDataGenerator(rescale=1/255,
                                    rotation_range=40,
                                    shear_range=0.2,
                                    zoom_range=0.2,
                                    horizontal_flip=True,
                                    fill_mode='nearest')

valid_generator = ImageDataGenerator(rescale = 1/255)
```

然后我们可以导入图像。flow_from_directory 函数在指定的路径中查找图像，并将它们调整到目标大小。

```
epochs = 20
batch_size = 64 

***#Finds images, transforms them***train_data = train_generator.flow_from_directory(train_dir,                                                                                target_size=target_size, batch_size=batch_size,                                           class_mode='categorical')

test_data = valid_generator.flow_from_directory(test_dir,       target_size=target_size, batch_size=batch_size,                                                 class_mode='categorical', shuffle=False)
```

时期和批量是两个非常重要的超参数。当整个数据集通过网络时，一个历元完成。批量大小是模型更新前处理的图像数量。调整这些参数会极大地改变训练的速度和长度。

然后我们可以建立模型本身。在这里，我创建了一个非常简单的模型，除了输入/输出层之外，只有一个卷积层和一个池层。显然，一个更复杂的模型将有助于提高性能，添加层是非常简单的，但对于本教程，我将把它留在那里。

```
**#Number of images we have** train_samples = train_data.samples
valid_samples = test_data.samples**#Building the model** model = models.Sequential()
model.add(layers.Conv2D(32, (3, 3), activation='relu', input_shape (100, 100, channels)))
model.add(layers.MaxPooling2D((2, 2)))
model.add(layers.Dense(10, activation='softmax'))

***#Compile model*** model.compile(loss='categorical_crossentropy',
              optimizer='adam',
              metrics=['accuracy'])
```

第一卷积层需要输入形状，即图像的形状。这个 CNN 的最后一层使用 softmax 激活函数，这在我们有多个类(这里有 10 个)时是合适的，因为它允许模型计算图像属于每个类的概率。最后， *model.compile* 函数允许我们指定模型在训练期间将如何学习。

然后，我们可以拟合模型并评估验证集的性能。

```
***#Fit the model*** model.fit_generator(generator=train_data,
                    steps_per_epoch=train_samples/batch_size,
                    validation_data=test_data,
                    validation_steps=valid_samples/batch_size,
                    epochs=epochs)
```

这个超级简单的模型仅经过 20 个时期就在验证集上达到了几乎 63%的准确率！

```
Epoch 20/20
17/17 [===============================] - 81s 5s/step - loss: 0.6802 - accuracy: 0.7395 - val_loss: 1.4856 - val_accuracy: 0.6287
```

在实践中，有更多的纪元是有意义的，只要模型改进，就让它训练。

这就是一个卷积神经网络，从头到尾只有 40 行代码。不再那么可怕了，是吗？非常感谢你的阅读！