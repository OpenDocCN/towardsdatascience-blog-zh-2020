# 面向完全初学者的 Tensorflow:张量入门

> 原文：<https://towardsdatascience.com/tensorflow-for-complete-begineers-getting-started-with-tensors-780f846f007?source=collection_archive---------7----------------------->

## 对“张量”及其运算的理解，可以帮助完全初学者从零开始。

![](img/b5a72c280577068cfb638cc34316e579.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1013613) (免费使用)

在这个令人兴奋的人工智能(AI)世界，Tensorflow 如今是一个热门词汇，尤其是在深度学习继续快速加速人工智能进展的情况下。但是对于刚开始使用 Tensorflow 的人来说，这种体验可能会令人害怕，因为漂亮的库的术语和用法可能会让完全的初学者感到困惑。当我刚开始学习 Tensorflow 时，我面临着类似的挑战，希望通过这篇文章简化一些错综复杂的事情。本文需要对 Python 有一个基本的了解，才能对 Tensorflow 有一个更清晰的了解。

**张量流是如何工作的？**

描述 Tensorflow 的基本(也是最简单的)方式是，它是 Python(可能还有其他编程语言)中一个很酷的库，允许我们创建计算图来开发神经网络模型。组成 Tensorflow 对象的基本元素是一个 ***张量*** ，所有执行的计算都发生在这些张量中。所以从字面上看(用我的话说)，当你开发任何神经网络模型时，这些张量以有序的方式流动，并在评估时产生最终输出。所以是的，了解张量到底是什么以及我们如何使用它们是开始使用 Tensorflow 的第一步。

**那么什么是张量呢？**

我们都很熟悉编程中的数据类型，对吗？ *1、2、3* 等。是整数，带小数点的数字 *(1.5，3.141，5.2555 等。)*是浮点值，另一个常见的是字符串(比如*“Hello。你今天怎么样？”*)。当我们把多个元素集合在一起时，我们一般称之为**(例如【1，4，7】)****。****

*列表集合可用于开发如下矩阵:*

**[[1，4，7]，[5，8，12]，[1，66，88]]**

*现在，让我们来看看张量——它们基本上是常规矩阵和向量的高维表示/概括。它甚至可以是 1 维矩阵、2 维矩阵、4 维矩阵或 n 维矩阵！所以我们可以有多维数组，而不仅仅是张量中的一个列表。就是这样，这就是张量在最基本的意义上的真正含义。就像我们可以在传统编程中给变量赋值一样，我们也可以在 Tensorflow 中这样做。只是有一种专门的方法来做这件事。类似地，就像你可以在编程中对常规变量(或常数)执行多重操作一样，我们也可以用张量来做。我们可以将张量切片并选择一部分元素，张量有不同的数据类型(整数、浮点、字符串等。)等等。*

***开始使用 Python 中的 tensor flow***

*第一步是安装漂亮的库！*匹普*是你在这里需要的一切。请注意，如果您使用的是 Google Colab(与 Python 中的 Jupyter notebooks 非常相似)，您甚至不需要安装这个库，因为它已经安装好了，可以随时使用。谢谢你，谷歌！*

```
*#Install the tensorflow module (not required in Google Colab, but is needed in your local PC)!pip install tensorflow*
```

*当我们在本文中使用 Colab 时，输出中显示了消息“需求已经满足”。*

> **需求已经满足:tensor flow in/usr/local/lib/python 3.6/dist-packages(2 . 3 . 0)**

*Tensorflow 目前有一个更老的版本 type 1.x，而最新发布的版本使用的是 2.x，所以我们选择当前的 tensorflow 版本。*

```
*#Here, we would go with the latest (2.x) release of tensorflow by selecting the version#Note: By default, Colab tends to use the 2.x version%tensorflow_version 2.x*
```

*让我们使用黄金关键词 *import 来导入 tensorflow 模块！是的，你可以通过简单地打印版本属性来检查你正在使用的库的版本。**

```
*#import the tensorflow moduleimport tensorflow as tf#Display the tensorflow version we are usingprint(tf.version)*
```

*现在，让我们开始创建张量。我们将在这里为 int，float 和 string 创建一些。注意使用 *tf 创建张量的特殊方式。变量*属性。这意味着我们正在创建一个具有可变性质的张量，因此我们可以通过执行专门的操作来改变/修改它的值，就像我们在使用变量的常规编程中所做的那样。请注意，虽然我们在传统编程中已经使用了 *int、float 和 string* 来声明这些数据类型的相应变量，但我们将在 Tensorflow 中使用 *tf.int16* (这意味着我们正在定义一个 16 位整数)、 *tf.float32* (定义一个 32 位浮点值)和 *tf.string* 来这样做。注意，我们也可以使用 *tf.float16、tf.int32 等等，这取决于我们想要存储的值的需求。*如果你曾经使用过 C++，你应该知道像 *int，short int，long long int，float，double* 等。用于声明位数更少(或更多)的变量，所以我们在 Tensorflow 中做一些类似的事情。*

```
*#The science (and art) of creating tensorsscalar_val = tf.Variable(123,tf.int16)floating_val = tf.Variable(123.456,tf.float32)string_val = tf.Variable(“hello everyone. Nice to learn tensorflow!”,tf.string)*
```

*而现在，非常容易打印出这些张量的值！*

```
*#Let us display the values (print) these tensorsprint(scalar_val)print(floating_val)print(string_val)*
```

> *<tf.variable shape="()" dtype="int32," numpy="123"></tf.variable>*
> 
> *<tf.variable shape="()" dtype="float32," numpy="123.456"><tf.variable shape="()" dtype="string," numpy="b’hello" everyone.="" nice="" to="" learn="" tensorflow=""></tf.variable></tf.variable>*

*我们可以确定张量的**形状**和**等级**，就像你可能在学校的数学模块中听到的那样。如果不是，形状只是指张量的每个维度所包含的元素的数量。而秩是张量中嵌套的最深层次。进入一些代码可能会更清楚。*

```
*#The idea behind shape and rank of tensors#Shape: Describes the dimension of the tensor (total elements contained along each dimension)scalar_val_shap = tf.shape(scalar_val)print(scalar_val_shap)floating_val_shap = tf.shape(floating_val)print(floating_val_shap)*
```

> *tf。张量([]，shape=(0，)，dtype=int32)*
> 
> *tf。张量([]，shape=(0，)，dtype=int32)*

*所以我们得到 0 的形状，对吗？这只是意味着这些是标量值，而不是列表或嵌套列表。*

```
*#Now, if we use e.g. lists/nested lists instead of just a “single” scalar valuelist_tensor1 = tf.Variable([1,3,5,6],tf.int16)print(list_tensor1)print(tf.shape(list_tensor1))list_tensor2 = tf.Variable([[1,2,3],[4,5,6]],tf.int16)print(list_tensor2)print(tf.shape(list_tensor2))#how about the rank? It describes the level of nesting within the tensor in simple words.print(tf.rank(list_tensor1))print(tf.rank(list_tensor2))*
```

*你可以看到 *list_tensor1* 有 4 个元素，所以它的形状是(4，)。注意,( 4，)只不过是(4，1 ),只显示了单个列表中的 4 个元素。接下来，对于 *list_tensor2* ，观察(2，3)的形状，表示我们有一个嵌套列表——有 2 个列表，每个列表包含 3 个元素。*

> *<tf.variable shape="(4,)" dtype="int32," numpy="array([1,"></tf.variable>*
> 
> *tf。张量([4]，shape=(1，)，dtype=int32)*
> 
> *<tf.variable shape="(2," dtype="int32," numpy="array([[1,"></tf.variable>*
> 
> *tf。张量([2 ^ 3]，shape=(2，)，dtype=int32)*
> 
> *tf。张量(1，shape=()，dtype=int32)*
> 
> *tf。张量(2，shape=()，dtype=int32)*

*类似地，如果你看一下秩，你会看到, *list_tensor1，*的秩为 1，因为我们只有一个列表，这里没有嵌套！现在，对于 *list_tensor2，*你可以看到等级为 2，表示我们有一个 2 级嵌套，即一个大列表包含更多的列表。如果它是一个大列表，包含另一个小列表，而那个小列表包含更多更小的列表，我们的排名就会是 3。希望这能让你明白。*

> *tf。张量(1，shape=()，dtype=int32)*
> 
> *tf。张量(2，shape=()，dtype=int32)*

*我们可以使用 *tf.reshape.* 来改变张量的形状(当然，直到它在数学上有效为止)。同样，请注意使用 Python 来做这件事的特殊方式。*

```
*#Reshaping tensorsreshaped_list_tensor2 = tf.reshape(list_tensor2,[6])print(reshaped_list_tensor2)list_tensor3 = tf.Variable([[1,2,3,1],[1,9,10,11],[1,5,11,22],[16,17,18,19]],tf.int16)print(list_tensor3)print(tf.rank(list_tensor3))print(tf.shape(list_tensor3))reshaped_list_tensor3 = tf.reshape(list_tensor3,[2,8,1])print(reshaped_list_tensor3)#or like thisreshaped_list_tensor3 = tf.reshape(list_tensor3,[8,2,1])print(reshaped_list_tensor3)#or automatically determine the shape by only giving one dimension!reshaped_list_tensor3 = tf.reshape(list_tensor3,[1,-1])print(reshaped_list_tensor3)*
```

*你会得到下面的输出。它太长了，可能无法阅读，但我猜如果你在这里查看 Colab 笔记本[会更清楚。这里发生的基本事情是，如果你在这里为形状指定[6]，tensorflow 将把你的张量重新整形为具有 6 个一维元素(因此它将是一个简单的 1D 列表)。如果你做一个[2，8，1]，它会产生 2 个列表，每个列表有 8 个元素。你也可以做[8，2，1]-创建 8 个列表，每个列表有 2 个元素，就像你看到的那样。如果您不想亲自指定整形的整个尺寸，您也可以简单地使用-1。因此，[1，-1]的整形将整形 *list_tensor3* 以创建*reshaved _ list _ tensor 3，那么* 1 list 与？？？是的，16 种元素。因此，请注意，尽管您可以按照自己的意愿使用 reshape，但必须确保 reshape 后的元素总数在整个过程中不会发生变化。](https://github.com/joyjitchatterjee/DeepLearning-Tensorflow-Basics/blob/master/Tensorflow_Beginning.ipynb.)*

> *tf。张量([1 2 3 4 5 6]，shape=(6，)，dtype=int32)*
> 
> *<tf.variable shape="(4," dtype="int32," numpy="array([["></tf.variable>*
> 
> *tf。张量(2，shape=()，dtype=int32)*
> 
> *tf。张量([4 ^ 4]，shape=(2，)，dtype=int32)*
> 
> *tf。张量([[1][2][3][1][1][9][10][11]][[1][5][11][22][16][17][18][19]])，shape=(2，8，1)，dtype=int32)*
> 
> *tf。张量([[[1][2]][[3][1]][[1][9]][[10][11]][[1][5]][[11][22]][[16][17]][[18][19]])，shape=(8，2，1)，dtype=int32)*
> 
> *tf。张量([[ 1 2 3 1 1 9 10 11 1 5 11 22 16 17 18 19]]，shape=(1，16)，dtype=int32)*

*你可以使用特殊命令 *tf.ones* 或 *tf.zeros.* 创建一个全是 1(或 0)的张量，然后也可以对它们执行类似的操作。*

*现在，我们来切片。就像你可以从一个矩阵或者一个 Python 列表中切分和提取一些元素一样，你也可以对张量做同样的事情。请看下面的例子，其中，我们首先创建一个全张量(我的名字是全 1 张量)和另一个全 0 张量。您想要创建的张量的维数在方括号[]内，因此 a [4，4，4，1]创建了一个 4D 张量，而 a [5，5]创建了一个 2D 张量——在最简单的意义上，它有 5 行 5 列(不过是一个矩阵)。*

*下面显示了这些创建的张量的一些切片操作。这样你就可以从张量中得到你喜欢的元素而不考虑你不喜欢的:-)*

```
*#creating a tensor full of 1s (or 0s)tensor_onefull = tf.ones([4,4,4,1])print(tensor_onefull)tensor_zerofull = tf.zeros([5,5])print(tensor_zerofull)#extracting specific values from tensors (similar to slicing in conventional programming)tensor_sliced_onefull = tensor_onefull[0]print(tensor_sliced_onefull)tensor_sliced_zerofull = tensor_zerofull[0,1]print(tensor_sliced_zerofull)*
```

*这是很好的输出(在 Colab 笔记本中可读性更好)。*

> *tf。张量([[[[1。] [1.] [1.] [1.]] [[1.] [1.] [1.] [1.]] [[1.] [1.] [1.] [1.]] [[1.] [1.] [1.] [1.]]] [[[1.] [1.] [1.] [1.]] [[1.] [1.] [1.] [1.]] [[1.] [1.] [1.] [1.]] [[1.] [1.] [1.] [1.]]] [[[1.] [1.] [1.] [1.]] [[1.] [1.] [1.] [1.]] [[1.] [1.] [1.] [1.]] [[1.] [1.] [1.] [1.]]] [[[1.] [1.] [1.] [1.]] [[1.] [1.] [1.] [1.]] [[1.] [1.] [1.] [1.]] [[1.] [1.] [1.] [1.]]]]，shape=(4，4，4，1)，dtype=float32)*
> 
> *tf。张量([[0。0.0.0.0.] [0.0.0.0.0.] [0.0.0.0.0.] [0.0.0.0.0.] [0.0.0.0.0.]]，shape=(5，5)，dtype=float32)*
> 
> *tf。张量([[[1。] [1.] [1.] [1.]] [[1.] [1.] [1.] [1.]] [[1.] [1.] [1.] [1.]] [[1.] [1.] [1.] [1.]]]，shape=(4，4，1)，dtype=float32)*
> 
> *tf。张量(0.0，shape=()，dtype=float32)*

```
*#another example from previously created tensorprint(list_tensor3)tf_slicedexampleagain = list_tensor3[0,-2:]print(tf_slicedexampleagain)#selecting multiple rowstf_slicedexampleagain = list_tensor3[1::]print(tf_slicedexampleagain)*
```

*同样，一些很酷的输出(基本但没错，最简单的东西是最酷的)。*

> *<tf.variable shape="(4," dtype="int32," numpy="array([["></tf.variable>*
> 
> *tf。张量([3 1]，shape=(2，)，dtype=int32)*
> 
> *tf。张量([[ 1 9 10 11] [ 1 5 11 22] [16 17 18 19]]，shape=(3，4)，dtype=int32)*

*你可以在我的 Github[https://Github . com/joyjitchatterjee/deep learning-tensor flow-Basics/blob/master/tensor flow _ beginning . ipynb](https://github.com/joyjitchatterjee/DeepLearning-Tensorflow-Basics/blob/master/Tensorflow_Beginning.ipynb)上找到这篇文章的完整的 Colab 笔记本。*

*仅此而已。希望你喜欢阅读这篇文章。感谢阅读，并为任何错别字/错误道歉。我希望在我的下一篇文章中涉及更多关于 Tensorflow 的内容。*

*如果你愿意，你可以通过 LinkedIn 联系我，电话是[http://linkedin.com/in/joyjitchatterjee/](http://linkedin.com/in/joyjitchatterjee/)*

***参考文献***

1.  *张量流文档([https://www.tensorflow.org/guide](https://www.tensorflow.org/guide))*
2.  *TensorFlow 2.0 完整教程— Python 神经网络初学者教程([https://www.youtube.com/watch?v=tPYj3fFJGjk&list = ply lya 78 q 4 izualaaoer 3 qwuzkqltzf 7 qk](https://www.youtube.com/watch?v=tPYj3fFJGjk&list=PLYLyA78Q4izuAlaaOER3qwUZkqLTZf7qk))*
3.  *TensorFlow 和深度学习入门([https://medium . com/@ rishit . dagli/get-started-with-tensor flow-and-Deep-Learning-part-1-72c 7d 67 f 99 fc](https://medium.com/@rishit.dagli/get-started-with-tensorflow-and-deep-learning-part-1-72c7d67f99fc))*