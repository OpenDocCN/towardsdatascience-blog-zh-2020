# 我如何在 Amazon SageMaker 上建立一个简单的假新闻检测器

> 原文：<https://towardsdatascience.com/how-i-built-a-simple-fake-news-detector-on-amazon-sagemaker-808bf4e0c490?source=collection_archive---------36----------------------->

最近，我决定注册一个 Udacity 纳米学位，因为这个想法在我脑海里盘旋了很久。

在过去的两个月里，每天晚饭后和周末，我都跟着机器学习工程师 Nanodegree 课程，我遇到了亚马逊 SageMaker。

![](img/0d7bfa15a7f217dc5b569fedb04052c4.png)

归功于 aws.amazon.com

Amazon SageMaker 是一项完全托管的服务，允许数据科学家和开发人员大规模构建、训练和部署机器学习模型。

令人惊讶的是，你真的可以在同一个平台上执行整个端到端的数据科学管道。
事实上，通过 Amazon SageMaker，您可以在一组不同的机器上创建 Jupyter 笔记本实例，这些机器基于计算(CPU/GPU)、RAM 和网络功能而有所不同。

您可以从导入数据、探索和清理数据开始，来训练模型并快速将其投入生产环境。

与 SageMaker 共同的工作流程(至少从我的小经验中学到的是这样的):

*   **数据整合与处理**
*   整合来自任何来源的数据集；
*   探索它，做可视化和汇总统计，了解数据；
*   如有必要，清理必须清理的部分，预处理和设计您的特征；
*   将处理后的数据保存到 S3 存储桶中，该存储桶可以是默认的 SageMaker 实例存储桶，也可以是您选择的其他存储桶。
*   **模型搭建&部署**
*   要建立模型，Amazon SageMaker 自带了一套有监督和无监督的模型，但是你也可以为你的定制模型提供一个你自己选择的框架(Scikit-learn，TensorFlow，MXNet…)连同一个训练脚本；
*   使用您在 S3 上保存的数据在一个或多个计算实例上训练模型
*   在 SageMaker 端点上部署估计器以进行推断

我发现 SageMaker 对于数据科学项目来说是一个非常有价值的选择。从现在开始，我将与你分享我在 SageMaker 上进行 Udacity Capstone 项目的最后一次体验。

这个项目处理假/真新闻检测。可以毫无疑问地插入到自然语言处理问题的语境中。
当我在 Kaggle 上导航时，我发现了这个有趣的[数据集](https://www.kaggle.com/clmentbisaillon/fake-and-real-news-dataset/kernels)。
该数据集由 2 个 CSV 文件(真实、虚假新闻)组成，其中存储了文章的标题、文章、日期和主题。

# 问题陈述

所以，这个问题可以这样陈述:给定一篇文章的文本，我希望算法能够预测它指的是真的还是假的新闻。特别是，我将问题的解决方案组织如下:
·来自不同来源(CSV)的数据将被标记和堆叠；
“标题”和“文章”等文本特征被堆叠后，将被处理，以便生成有意义的词汇(没有标签、URL、奇怪的标点符号和停用词)
从这里，可以遵循两条道路，这取决于算法的选择。
如果使用机器学习算法，则有必要创建文本的单词包表示，或者通过使用单词计数，一种术语频率逆文档频率的热编码，可以与其他特征(例如，从日期中提取)一起使用来训练模型；
相反，如果选择深度学习模型，如递归神经网络，人们可以想到只直接使用填充到相同长度的文本序列，并用 word_to_integer 词汇表进行映射。
然后，可以训练神经网络来解决具有二元交叉熵损失的二元分类问题。

由于我的报告长达 10 页，我将只报告主要步骤:

**预处理** 关于 LSTM 模型的预处理步骤:

*   我只考虑文章文本作为一个特征，过滤长度在 20 个单词以下和 500 个单词以上的文本，以避免空序列或太长的序列。这些文本已经被停用词、奇怪的标点符号过滤掉，并被转换成小写字母。
*   我使用来自 Sklearn 的 train_test_split 来拆分训练、验证和测试数据集中的数据
*   我将 keras 的标记器应用于训练集，然后我使用它来转换验证和测试数据集(以避免数据泄漏)，然后将所有序列填充到 max_len 为 500

```
from tf.keras.preprocessing.text import Tokenizer
from tf.keras.preprocessing import sequencetokenizer = Tokenizer(num_words=80000)
text_tokenizer.fit_on_texts(X_train['article'].astype(str))X_train = text_tokenizer.texts_to_sequences(X_train)
X_val = text_tokenizer.texts_to_sequences(X_val)
X_test = text_tokenizer.texts_to_sequences(X_test)X_train = sequence.pad_sequences(X_train, maxlen=500, padding='post')
X_val= sequence.pad_sequences(X_val, maxlen=500, padding='post')
X_test= sequence.pad_sequences(X_test, maxlen=500, padding='post')
```

**型号**

在培训脚本中(请记住我在 SageMaker 上),我定义了环境变量，您可以在这里定义模型结构，对其进行拟合，并在 S3 上保存其工件。这是我使用的网络结构(Keras)。

```
from tf.keras.layers import Embedding, Bidirectional, LSTM,Dense,Activation
from tf.keras.models import Sequentialdef RNN():model = Sequential()
    layer = model.add(Embedding(80000, 128, input_length = 500))
    layer = model.add(Bidirectional(LSTM(128))
    layer = model.add(Dense(128))   
    layer = model.add(Activation('relu'))
    layer = model.add(Dense(1))
    layer = model.add(Activation('sigmoid')) return model
```

通过在 LSTM 层添加双向功能，我将精确度提高了 15%以上。

然后您添加代码以适应并保存模型；该代码将由 SageMaker 在培训工作中调用。

```
model.fit(train_X,
          train_y, 
          batch_size=256,
          epochs=args.n_epochs,
          validation_data=(val_X,val_y))model_path = '/opt/ml/model/'
model.save(os.path.join(model_path,'bi_lstm/1'), save_format='tf')
```

相反，在实例端，我实例化了一个 TensorFlow 对象，其中设置了训练脚本的路径、我要选择的实例的数量和类型、IAM 角色和超参数:

```
input_channels = {"train":train_data,
                  "validation":val_data}from sagemaker.tensorflow import TensorFlowestimator = TensorFlow(entry_point = 'source_train/train_keras_lstm.py',
                       train_instance_type='ml.p2.xlarge',
                       train_instance_count=1, 
                       role=role,
                       framework_version='2.1.0',
                       py_version='py3',
                       hyperparameters={"n_epochs":3}
```

正如你所看到的，我选择了一个' ml.p2.xlarge '实例，这是一个带有 GPU 访问的亚马逊入门级机器。

使用相同的策略，我在训练模型后部署了它:

```
predictor = estimator.deploy(initial_instance_count=1,
                             instance_type='ml.c4.xlarge')
```

并对测试集执行推断(这可以通过 predict() API 来完成，也可以通过在数据较大的情况下创建批处理转换作业来完成):

```
from sklearn.metrics import accuracy_scorepreds_df = pd.DataFrame(predictor.predict(X_test)
target_preds = pd.concat([y_test,preds_df], axis=1)
target_preds.columns=['targetClass','preds']print(accuracy_score(target_preds['targetClass'],
                     target_preds['preds']))0.986639753940792
```

我在测试集上获得了 98%的准确率。

除了模型本身，我希望我引起了你对 SageMaker 功能的注意。

如果你想看完整个步骤，看报告或者看一下培训脚本，[这是该项目的 GitHub repo](https://github.com/guidiandrea/udacityCapstone-FakeNewsDetector) 。

下次见，再见，感谢阅读！