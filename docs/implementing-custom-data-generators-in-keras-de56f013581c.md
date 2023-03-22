# 在 Keras 中实现自定义数据生成器

> 原文：<https://towardsdatascience.com/implementing-custom-data-generators-in-keras-de56f013581c?source=collection_archive---------7----------------------->

## 如何实现自定义数据生成器以在 Keras 模型中启用动态数据流

![](img/abc4bb95bfaf3efca0c2c524add85d77.png)

亚历山大·辛恩在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

数据生成器是 Keras API 最有用的特性之一。考虑这样一种情况，您有大量的数据，多到无法一次将所有数据都存储在 RAM 中。Wyd？购买更多的内存显然不是一个选项。

这个问题的解决方案可以是动态地装载小批量数据给**模型。**这正是数据生成器的工作。它们可以动态生成模型输入，从而形成从存储器到 RAM 的管道，以便在需要时加载数据。这种管道的另一个优点是，在准备向模型提供数据时，可以很容易地对这些小批量数据应用预处理例程。

在本文中，我们将看到如何子类化 *tf.keras.utils.Sequence* 类来实现定制数据生成器。

# 图像数据生成器

首先，我们现在将了解如何使用 ImageDataGenerator API 进行动态图像流水线操作，从而满足实现自定义 API 的需求。

```
datagen = ImageDataGenerator(
        rescale=1./255,
        shear_range=0.2,
        zoom_range=0.2,
        horizontal_flip=True
)
```

ImageDataGenerator API 提供了从目录以及数据帧中提到的路径中管道传输图像数据的功能。其中一个可能包括预处理步骤，如图像的缩放、增强，这些步骤将直接实时应用于图像**。**

# 那么，为什么要定制呢？

模型训练不限于单一类型的输入和目标。有时一个模型会同时被输入多种类型的输入。例如，假设您正在处理一个多模态分类问题，您需要同时处理文本和图像数据。在这里，显然不能使用 ImageDataGenerator。而且，一次加载所有数据是不可承受的。因此，我们通过实现自定义数据生成器来解决这个问题。

# 实现自定义数据生成器

我们最后从实现开始。

> 这将是一个非常通用的实现，因此可以直接复制。你只需要用自己的逻辑来填空/替换某些变量。

如前所述，我们将子类化*TF . keras . utils . sequence*API。

```
def __init__(
     self, 
     df, 
     x_col, 
     y_col=None, 
     batch_size=32, 
     num_classes=None,
     shuffle=True
):
     self.batch_size = batch_size
     self.df = dataframe
     self.indices = self.df.index.tolist()
     self.num_classes = num_classes
     self.shuffle = shuffle
     self.x_col = x_col
     self.y_col = y_col
     self.on_epoch_end()
```

首先，我们定义构造函数来初始化生成器的配置。注意，这里我们假设数据的路径在 dataframe 列中。因此，我们定义 x_col 和 y_col 参数。这也可以是您可以从中加载数据的目录名。

*on_epoch_end* 方法是在每个 epoch 之后调用的方法。我们可以在这里加入洗牌之类的套路。

```
def on_epoch_end(self):
     self.index = np.arange(len(self.indices))
     if self.shuffle == True:
          np.random.shuffle(self.index)
```

基本上，我们在这个代码片段中打乱了数据帧行的顺序。

我们的另一个实用方法是 *__len__* 。它实际上使用样本和批量大小返回一个时期中的步骤数。

```
def __len__(self):
     **# Denotes the number of batches per epoch**
     return len(self.indices) // self.batch_size
```

接下来是 *__getitem__* 方法，使用批号作为参数调用该方法以获得给定的一批数据。

```
def __getitem__(self, index):
 **# Generate one batch of data
     # Generate indices of the batch**
     index = self.index[index * self.batch_size:(index + 1) * self.batch_size] **# Find list of IDs**
     batch = [self.indices[k] for k in index] **# Generate data**
     X, y = self.__get_data(batch)
     return X, y
```

基本上，我们只是获得混洗的索引，并从不同的方法调用数据集，然后将其返回给调用者。数据集生成的逻辑可以在这里自己实现。但是，将它抽象到其他地方是一个很好的做法。

最后，我们在 __ *get_data* 方法中编写数据生成的逻辑。既然这个方法要被我们调用，我们可以随便取什么名字。此外，这个方法没有理由是公共的，因此我们将其定义为私有的。

```
def __get_data(self, batch): **# X.shape : (batch_size, *dim)
     # We can have multiple Xs and can return them as a list** X = **# logic to load the data from storage** y = **# logic for the target variables** **# Generate data**
     for i, id in enumerate(batch):
     **# Store sample**
          X[i,] = **# logic** **# Store class**
     y[i] = **# labels**
     return X, y
```

此外，我们可以添加预处理/增强例程来实时启用它们。在上面这段代码中，X 和 y 是根据方法中传递的批处理索引参数从数据源加载的。这可以是从加载图像到加载文本或同时加载两者或任何其他类型的数据。

合并所有方法后，完整的生成器如下所示:

完整的自定义数据生成器类

# 结论

在本文中，我们看到了数据生成器在用大量数据训练模型时的用处。我们查看了 ImageDataGenerator API，以了解它是什么，并解决对自定义 API 的需求。然后，我们最终了解了如何通过子类化*TF . keras . utils . sequence*API 来实现定制数据生成器。

请随意复制这段代码，并在其中添加您自己的生成器逻辑。

# 参考

 [## TF . keras . utils . sequence | tensor flow Core v 2 . 2 . 0

### 通过 TensorFlow 学习 ML 基础知识的教育资源

www.tensorflow.org](https://www.tensorflow.org/api_docs/python/tf/keras/utils/Sequence)  [## TF . keras . preprocessing . image . imagedata generator

### 通过 TensorFlow 学习 ML 基础知识的教育资源

www.tensorflow.org](https://www.tensorflow.org/api_docs/python/tf/keras/preprocessing/image/ImageDataGenerator) [](https://stanford.edu/~shervine/blog/keras-how-to-generate-data-on-the-fly) [## 使用 Keras 的数据生成器的详细示例

### python keras 2 fit _ generator Afshine Amidi 和 Shervine Amidi 的大型数据集多重处理您是否曾经不得不…

stanford.edu](https://stanford.edu/~shervine/blog/keras-how-to-generate-data-on-the-fly)