# fastML 指南

> 原文：<https://towardsdatascience.com/the-fastml-guide-9ada1bb761cf?source=collection_archive---------60----------------------->

## 作为一名数据科学家、人工智能或 ML 工程师，速度是从事一个项目的基本要素。fastML 为您提供了速度和灵活性，让您可以针对多种算法测试您的模型。

![](img/176761d8a704d1e10b06d872ebcfb358.png)

FastML 标志由 [Divine Kofi Alorvor](https://www.linkedin.com/in/divine-kofi-alorvor-86775117b)

数据科学家、人工智能和机器学习工程师的工作很大程度上围绕着许多不同算法的使用，这些算法使我们的工作变得更容易和相对更快。然而，我们经常发现，为特定的用例选择特定的算法是相当困难的，因为有许多算法可以同等地执行我们需要执行的任务。

fastML 是一个 python 包，它允许您使用自己喜欢的测试大小来测试和训练已处理的数据，并使用很少几行代码对准备好的数据运行多种分类算法。这使您可以观察您决定运行的所有算法的行为，以确定哪些算法最适合您的数据，以便使用您选择的算法进行进一步开发。这也为您节省了在不使用 fastML 的情况下手动编写近 300 行代码的压力。

# 入门指南

fastML 发布到**[**pypi**](https://pypi.org/project/fastML/)**，这使得使用 python 包安装程序 pip 在本地安装变得容易。要安装 FastML 进行本地开发，请确保您已经安装了 python 和 pip 并将其添加到 path 中。如果你需要帮助，你可以在这里查看 Python 文档。****

****要安装 fastML，请打开您的终端(Linux/mac)或命令提示符(windows)并输入命令:****

```
**pip install fastML**
```

# ****使用 fastML****

****在本指南中，我将通过一个流行的 Iris 数据集的例子来教你如何在你的项目中使用 fastML 包。在处理任何项目时，要做的第一件事就是导入项目所需的库和包。****

```
**##importing needed libraries and packages including fastMLfrom fastML import fastML
from sklearn import datasets**
```

****现在要做的下一件事是将 Iris 数据集加载到我们的项目中以供使用。****

```
**##loading the Iris datasetdf = datasets.load_iris()**
```

****由于虹膜数据集已经进行了预处理，因此没有必要再次处理我们的数据。但是，对于您将在自己的项目中使用的数据，您必须确保数据经过良好的处理，以避免在项目中遇到错误和不希望的输出。****

****现在要做的下一件事是准备用于训练和测试的数据，并将所需的数据列指定为特性和目标列。****

```
**##assigning the desired columns to X and Y in preparation for running fastMLX = df.data[:, :4]
Y = df.target**
```

****根据您拥有的数据类型，您的目标数据可能最需要编码。对你的目标数据进行编码是很好的，因为它选取了可以解释目标数据的值，并帮助机器学习算法理解目标数据到底是什么。使用 fastML python 包对目标数据进行编码也非常容易。****

```
**##importing the encoding function from the fastML package and running the EncodeCategorical function from fastML to handle the process of categorial encodingfrom fastML import EncodeCategorical
Y = EncodeCategorical(Y)**
```

****接下来，我们将所需的 test_size 值赋给变量“size”。****

```
**size = 0.3**
```

****最后要做的事情是了解我们想要用来测试数据的所有算法，并将它们全部导入到我们的项目中。fastML 附带了一个用 keras 构建的准备好的神经网络分类器，用于深度学习分类。您可以将包括神经网络分类器在内的所有算法导入到您的项目中。例如:****

```
**##importing the desired algorithms into our projectfrom sklearn.ensemble import RandomForestClassifier
from sklearn.tree import DecisionTreeClassifier
from sklearn.neighbors import KNeighborsClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.svm import SVC##importing the neural net classifier from fastMLfrom nnclassifier import neuralnet**
```

****最后，我们运行主要的 fastML 函数。该功能具有灵活性，允许您自由调整任何单个算法的超参数。****

```
**## running the fastML function from fastML to run multiple classification algorithms on the given datafastML(X, Y, size, SVC(), RandomForestClassifier(), DecisionTreeClassifier(), KNeighborsClassifier(), LogisticRegression(max_iter = 7000), special_classifier_epochs=200,special_classifier_nature ='fixed',          include_special_classifier = True)**
```

****下面是运行主 fastML 函数后的类似输出:****

```
**Using TensorFlow backend.

   __          _   __  __ _      
  / _|        | | |  \/  | |     
 | |_ __ _ ___| |_| \  / | |        
 |  _/ _` / __| __| |\/| | |     
 | || (_| \__ \ |_| |  | | |____ 
 |_| \__,_|___/\__|_|  |_|______|

____________________________________________________
____________________________________________________
Accuracy Score for SVC is 
0.9811320754716981

Confusion Matrix for SVC is 
[[16  0  0]
 [ 0 20  1]
 [ 0  0 16]]

Classification Report for SVC is 
              precision    recall  f1-score   support

           0       1.00      1.00      1.00        16
           1       1.00      0.95      0.98        21
           2       0.94      1.00      0.97        16

    accuracy                           0.98        53
   macro avg       0.98      0.98      0.98        53
weighted avg       0.98      0.98      0.98        53

____________________________________________________
____________________________________________________
____________________________________________________
____________________________________________________
Accuracy Score for RandomForestClassifier is 
0.9622641509433962

Confusion Matrix for RandomForestClassifier is 
[[16  0  0]
 [ 0 20  1]
 [ 0  1 15]]

Classification Report for RandomForestClassifier is 
              precision    recall  f1-score   support

           0       1.00      1.00      1.00        16
           1       0.95      0.95      0.95        21
           2       0.94      0.94      0.94        16

    accuracy                           0.96        53
   macro avg       0.96      0.96      0.96        53
weighted avg       0.96      0.96      0.96        53

____________________________________________________
____________________________________________________
____________________________________________________
____________________________________________________
Accuracy Score for DecisionTreeClassifier is 
0.9622641509433962

Confusion Matrix for DecisionTreeClassifier is 
[[16  0  0]
 [ 0 20  1]
 [ 0  1 15]]

Classification Report for DecisionTreeClassifier is 
              precision    recall  f1-score   support

           0       1.00      1.00      1.00        16
           1       0.95      0.95      0.95        21
           2       0.94      0.94      0.94        16

    accuracy                           0.96        53
   macro avg       0.96      0.96      0.96        53
weighted avg       0.96      0.96      0.96        53

____________________________________________________
____________________________________________________
____________________________________________________
____________________________________________________
Accuracy Score for KNeighborsClassifier is 
0.9811320754716981

Confusion Matrix for KNeighborsClassifier is 
[[16  0  0]
 [ 0 20  1]
 [ 0  0 16]]

Classification Report for KNeighborsClassifier is 
              precision    recall  f1-score   support

           0       1.00      1.00      1.00        16
           1       1.00      0.95      0.98        21
           2       0.94      1.00      0.97        16

    accuracy                           0.98        53
   macro avg       0.98      0.98      0.98        53
weighted avg       0.98      0.98      0.98        53

____________________________________________________
____________________________________________________
____________________________________________________
____________________________________________________
Accuracy Score for LogisticRegression is 
0.9811320754716981

Confusion Matrix for LogisticRegression is 
[[16  0  0]
 [ 0 20  1]
 [ 0  0 16]]

Classification Report for LogisticRegression is 
              precision    recall  f1-score   support

           0       1.00      1.00      1.00        16
           1       1.00      0.95      0.98        21
           2       0.94      1.00      0.97        16

    accuracy                           0.98        53
   macro avg       0.98      0.98      0.98        53
weighted avg       0.98      0.98      0.98        53

____________________________________________________
____________________________________________________
Included special classifier with fixed nature
Model: "sequential_1"
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
dense_1 (Dense)              (None, 4)                 20        
_________________________________________________________________
dense_2 (Dense)              (None, 16)                80        
_________________________________________________________________
dense_3 (Dense)              (None, 3)                 51        
=================================================================
Total params: 151
Trainable params: 151
Non-trainable params: 0
_________________________________________________________________
Train on 97 samples, validate on 53 samples
Epoch 1/200
97/97 [==============================] - 0s 1ms/step - loss: 1.0995 - accuracy: 0.1443 - val_loss: 1.1011 - val_accuracy: 0.3019
97/97 [==============================] - 0s 63us/step - loss: 0.5166 - accuracy: 0.7010 - val_loss: 0.5706 - val_accuracy: 0.6038
Epoch 100/200
97/97 [==============================] - 0s 88us/step - loss: 0.5128 - accuracy: 0.7010 - val_loss: 0.5675 - val_accuracy: 0.6038
Epoch 200/200
97/97 [==============================] - 0s 79us/step - loss: 0.3375 - accuracy: 0.8969 - val_loss: 0.3619 - val_accuracy: 0.9057
97/97 [==============================] - 0s 36us/step
____________________________________________________
____________________________________________________
Accuracy Score for neuralnet is 
0.8969072103500366

Confusion Matrix for neuralnet is 
[[16  0  0]
 [ 0 16  5]
 [ 0  0 16]]

Classification Report for neuralnet is 
              precision    recall  f1-score   support

           0       1.00      1.00      1.00        16
           1       1.00      0.76      0.86        21
           2       0.76      1.00      0.86        16

    accuracy                           0.91        53
   macro avg       0.92      0.92      0.91        53
weighted avg       0.93      0.91      0.91        53

____________________________________________________
____________________________________________________
                    Model            Accuracy
0                     SVC  0.9811320754716981
1  RandomForestClassifier  0.9622641509433962
2  DecisionTreeClassifier  0.9622641509433962
3    KNeighborsClassifier  0.9811320754716981
4      LogisticRegression  0.9811320754716981
5               neuralnet  0.8969072103500366**
```

****有了这个输出，我们可以确定什么算法最适合我们的用例，并选择该算法进行进一步的开发和部署。****

****fastML 是免费开源的，你可以在 [Github](https://github.com/Team-fastML/fastML) 上找到源代码和测试文件。当你在使用 fastML 的过程中遇到问题或错误时，我们的贡献者团队可以随时提供帮助。此外，您可以提交您希望我们实施的 bug 或功能更新问题，我们会完成它。检查项目，如果你喜欢这个项目，别忘了留下一颗星。****