# 为什么会变成咸菜？

> 原文：<https://towardsdatascience.com/why-turn-into-a-pickle-b45163007dac?source=collection_archive---------43----------------------->

![](img/925097f696df30a6a91631f0ca3f5964.png)

图片来自 [Pixabay](https://pixabay.com/) 的[照片组合](https://pixabay.com/users/photomix-company-1546875/)【我没有得到免费使用的泡菜里克图片:(】

## 泡菜是什么

Pickle 是一个 python 模块，用于将 python 对象序列化为二进制格式，并将其反序列化回 python 对象。

## 使用 pickle 的两个重要用例

> ***第一种情况:***

你在做一个机器学习问题，用的是最爱: *jupyter 笔记本*。您分析了数据并确定了将有助于您的模型的主要特征。您执行特征工程，并且您知道这将是您将传递给机器学习模型的最终数据。但是特征工程需要大量时间，并且您不希望在关闭笔记本电脑后丢失特征工程数据。

> 这就是泡菜的用武之地。

您只需将特征工程数据传递到 Pickle 并以二进制格式保存它。然后，当您准备好执行建模时，就可以加载这些数据了。

**转储数据(保存)**

```
#Code Example
#Import the module
import pickle#Do Some Feature Engineering
feature_engineered_data = do_feature_engineering(data)#Dump it(save it in binary format)
with open('fe_data.pickle','wb') as fe_data_file:
     pickle.dump(feature_engineered_data, fe_data_file)
```

**加载回数据**

```
#Code Example
#Import the module
import pickle#Load the data - No need to do Feature Engineering again
with open('fe_data.pickle','rb') as fe_data_file:
     feature_engineered_data = pickle.load(fe_data_file)#Continue with your modeling
```

那么到底有什么优势呢？

功能工程可能是一个繁重的过程，您不希望在笔记本电脑关机的情况下重做它。因此，以可重用的格式存储它是有益的。

假设您为 EDA、功能工程和建模管理不同的笔记本。使用 Pickle，您可以将数据从一个笔记本转储到另一个笔记本。

> ***第二种情况:***

一个更流行的案例是 pickle*机器学习模型*对象。

您已经完成了特征工程、建模，并获得了相当好的精确度，万岁！你在这里的工作已经完成，所以你关掉笔记本电脑，好好睡一觉。

第二天早上，您得到另一个测试集来测试模型，但是因为您关闭了笔记本电脑，所以您必须再次训练模型，这需要 6 个小时！

> 这就是泡菜再次出现的地方

保存训练好的模型，并在有新数据进行测试时加载该模型。

**甩掉模特**

```
#import module
import pickle#Train the data
model.fit(X_train, X_test)#Dump the model
with open('fitted_model.pickle','wb') as modelFile:
     pickle.dump(model,modelFile)
```

**加载模型**

```
#import module
import pickle#Load the model - No need to TRAIN it again(6 hours saved)
with open('fitted_model.pickle','rb') as modelFile:
     model = pickle.load(modelFile)#Predict with the test set
prediction = model.predict(X_test)
```

> **可以使用 Pickle 保存最终数据，用多个模型进行训练，也可以保存模型，用多个数据进行测试，不需要再次训练模型。多棒啊。**

你对这些用例有什么看法，你还能想到哪些其他用例？请在评论中告诉我！