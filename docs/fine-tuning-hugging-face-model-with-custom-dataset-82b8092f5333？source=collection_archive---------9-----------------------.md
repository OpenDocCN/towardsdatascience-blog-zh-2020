# 使用自定义数据集微调拥抱人脸模型

> 原文：<https://towardsdatascience.com/fine-tuning-hugging-face-model-with-custom-dataset-82b8092f5333?source=collection_archive---------9----------------------->

## 端到端示例，解释如何使用 TensorFlow 和 Keras 通过自定义数据集微调拥抱人脸模型。我展示了如何保存/加载训练好的模型，并使用标记化的输入执行预测函数。

![](img/2d08d19688b604fc8bb204325b576efb.png)

作者:安德烈·巴拉诺夫斯基

有很多文章是关于用自己的数据集进行拥抱人脸微调的。很多文章都是用 PyTorch，有些是用 TensorFlow。我有一项任务是基于自定义投诉数据集实现情感分类。我决定用拥抱脸变形金刚，因为 LSTM 的效果不太好。尽管有大量可用的文章，但我花了大量的时间将所有的信息整合在一起，并用 TensorFlow 训练的拥抱脸实现了我自己的模型。似乎大多数(如果不是全部的话)文章在解释训练的时候就停止了。我认为分享一个完整的场景并解释如何保存/加载训练好的模型并执行推理会很有用。这篇文章基于 TensorFlow 的拥抱脸 API。

你应该从拥抱脸文档开始。有一个非常有用的部分— [使用定制数据集进行微调](https://huggingface.co/transformers/master/custom_datasets.html)。为了了解如何使用自己的句子分类数据来微调拥抱人脸模型，我建议学习这一部分的代码— *与 IMDb 评论的序列分类*。拥抱脸文档为 PyTorch 和 TensorFlow 都提供了例子，非常方便。

我正在使用[tfdistilbertfsequenceclassification](https://huggingface.co/transformers/model_doc/distilbert.html#tfdistilbertforsequenceclassification)类来运行句子分类。关于[distil Bert](https://huggingface.co/transformers/model_doc/distilbert.html#)—*distil Bert 是由蒸馏 Bert base 训练出来的小型快速廉价轻便的变压器模型。根据 GLUE 语言理解基准测试*，它比 bert-base-uncased 少 40%的参数，运行速度快 60%，同时保留了超过 95%的 bert 性能。

```
from transformers import DistilBertTokenizerFast
from transformers import TFDistilBertForSequenceClassificationimport tensorflow as tf
```

1.  **导入并准备数据**

一个例子是基于使用报纸标题的讽刺分类。数据由劳伦斯·莫罗尼准备，作为他的 [Coursera](https://www.coursera.org/professional-certificates/tensorflow-in-practice) 培训的一部分(源代码可在 [GitHub](https://github.com/lmoroney/dlaicourse/blob/master/TensorFlow%20In%20Practice/Course%203%20-%20NLP/Course%203%20-%20Week%203%20-%20Lesson%202c.ipynb) 上获得)。我直接从劳伦斯的博客中获取数据:

```
!wget --no-check-certificate \
    [https://storage.googleapis.com/laurencemoroney-blog.appspot.com/sarcasm.json](https://storage.googleapis.com/laurencemoroney-blog.appspot.com/sarcasm.json) \
    -O /tmp/sarcasm.json
```

然后是数据处理步骤，读取数据，将其分成训练/验证步骤，并提取一组标签:

```
training_size = 20000with open("/tmp/sarcasm.json", 'r') as f:
    datastore = json.load(f)sentences = []
labels = []
urls = []
for item in datastore:
    sentences.append(item['headline'])
    labels.append(item['is_sarcastic'])training_sentences = sentences[0:training_size]
validation_sentences = sentences[training_size:]
training_labels = labels[0:training_size]
validation_labels = labels[training_size:]
```

有 20000 个条目用于训练，6709 个条目用于验证。

2.**设置 BERT 并运行训练**

接下来，我们将加载记号赋予器:

```
tokenizer = DistilBertTokenizerFast.from_pretrained('distilbert-base-uncased')
```

标记培训和验证句子:

```
train_encodings = tokenizer(training_sentences,
                            truncation=True,
                            padding=True)
val_encodings = tokenizer(validation_sentences,
                            truncation=True,
                            padding=True)
```

创建 TensorFlow 数据集，我们可以将其输入 TensorFlow *fit* 函数进行训练。这里我们用标签映射句子，不需要单独将标签传入 *fit* 函数:

```
train_dataset = tf.data.Dataset.from_tensor_slices((
    dict(train_encodings),
    training_labels
))val_dataset = tf.data.Dataset.from_tensor_slices((
    dict(val_encodings),
    validation_labels
))
```

我们需要一个预先训练好的拥抱脸模型，我们将根据我们的数据对其进行微调:

```
# We classify two labels in this example. In case of multiclass 
# classification, adjust num_labels valuemodel = TFDistilBertForSequenceClassification.from_pretrained('distilbert-base-uncased', num_labels=2)
```

通过调用 TensorFlow *fit* 函数，用我们的数据微调模型。它来自*tfdistillbertforsequenceclassification*模型。您可以使用参数进行游戏和试验，但是所选择的选项已经产生了相当好的结果:

```
optimizer = tf.keras.optimizers.Adam(learning_rate=5e-5)
model.compile(optimizer=optimizer, loss=model.compute_loss, metrics=['accuracy'])
model.fit(train_dataset.shuffle(100).batch(16),
          epochs=3,
          batch_size=16,
          validation_data=val_dataset.shuffle(100).batch(16))
```

在 3 个时期中，它达到 0.0387 的损失和 0.9875 的精度，具有 0.3315 的验证损失和 0.9118 的验证精度。

用抱脸 *save_pretrained* 功能保存微调后的模型。使用 Keras 保存函数 *model.save* 进行保存确实有效，但是这种模型无法加载。这就是我使用 *save_pretrained* 的原因:

```
model.save_pretrained("/tmp/sentiment_custom_model")
```

保存模型是基本步骤，运行模型微调需要时间，您应该在训练完成时保存结果。另一个选择是——你可以在云 GPU 上运行 fine-running 并保存模型，在本地运行它以进行推理。

3.**加载保存的模型并运行预测功能**

我使用 TFDistilBertForSequenceClassification 类来加载保存的模型，方法是从 _pretrained 调用拥抱脸函数*(指向保存模型的文件夹):*

```
loaded_model = TFDistilBertForSequenceClassification.from_pretrained("/tmp/sentiment_custom_model")
```

现在我们想运行*预测*功能，并使用微调模型对输入进行分类。为了能够执行推理，我们需要像对训练/验证数据那样对输入句子进行标记。为了能够读取推理概率，将 *return_tensors="tf"* 标志传递给 tokenizer。然后使用保存的模型调用*预测*:

```
test_sentence = "With their homes in ashes, residents share harrowing tales of survival after massive wildfires kill 15"
test_sentence_sarcasm = "News anchor hits back at viewer who sent her snarky note about ‘showing too much cleavage’ during broadcast"# replace to test_sentence_sarcasm variable, if you want to test 
# sarcasmpredict_input = tokenizer.encode(test_sentence,
                                 truncation=True,
                                 padding=True,
                                 return_tensors="tf")tf_output = loaded_model.predict(predict_input)[0]
```

*预测*在拥抱人脸模型上运行的函数返回 logit(soft max 之前的分数)。我们需要应用 SoftMax 函数来获得结果概率:

```
tf_prediction = tf.nn.softmax(tf_output, axis=1).numpy()[0]
```

**结论**

这篇文章的目标是展示一个完整的场景，用自定义数据微调拥抱人脸模型——从数据处理、训练到模型保存/加载和推理执行。

**源代码**

*   GitHub 回购
*   在 [Colab](https://colab.research.google.com/drive/1yi9N-ZnQHtYfR3QDiwsPxYCYU6WyjwlQ) 笔记本中自己运行