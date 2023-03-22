# 通过深度学习为您最喜欢的类型生成新鲜的电影故事

> 原文：<https://towardsdatascience.com/generate-fresh-movie-stories-for-your-favorite-genre-with-deep-learning-143da14b29d6?source=collection_archive---------17----------------------->

## 微调 GPT 新协议，根据类型生成故事

![](img/d4a0ad28f36490c2664fb718b4e0089b.png)

[盛鹏鹏摄蔡](https://unsplash.com/@tsaichinghsuan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> **在发现时间旅行**后，地球居民现在生活在由政府控制的未来派城市中，为期十年。政府计划向该市派遣两个精英科学家小组，以便调查这些机器的起源并发现“上帝”的存在。

为你喜欢的类型创作故事不是很有趣吗？这就是我们今天要做的。我们将学习如何构建一个基于流派创建故事的故事生成器，就像上面创建科幻故事的生成器一样(在上面的故事中，用户提供的输入是粗体的)。

你也可以把这篇文章作为开发你自己的文本生成器的起点。例如，您可以生成科技、科学和政治等主题的标题。或者生成你喜欢的艺术家的歌词。

# 为什么是故事生成？

作为一个狂热的电影和电视剧迷，我喜欢故事生成器的想法，它可以根据类型、输入提示甚至标题生成故事。在了解了 GPT 2 号之后，我想把这个想法变成现实。这就是我建造这个模型的原因。

**预期用途**:寻找乐趣并测试融合讲故事的想法，我们可以通过混合我们的创造力(通过提供提示)和模型的创造力(通过使用提示生成故事的其余部分)来生成故事。

# 预告片时间:测试故事生成器！

![](img/c5b5e1cc5548b8536aab38af9121085d.png)

Alex Litvin 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在深入构建生成器之前，让我们先尝试一下故事生成。在这个 [**Huggingface 链接**](https://huggingface.co/pranavpsv/gpt2-genre-story-generator) 查看我的故事生成器或者运行这个 [**Colab 笔记本**中的单元格来生成故事](https://colab.research.google.com/drive/17dEk6VJk_jnd7d5-jbaOrwsS367M2pWw?usp=sharing)。模型输入格式的形式如下:

**< BOS > <流派>可选提示……**

**例如**<BOS><sci _ fi>发现时间旅行后，

**所属类型**所属:超级英雄、科幻、动作、剧情、恐怖、惊悚

模型将使用这个提示生成它的故事。还有一种更直观的方式来生成故事:一个使用我的模型 的 [**网络应用(记住这个应用的生成比**](http://54.173.99.218:8501)**[**hugging face link**](https://huggingface.co/pranavpsv/gpt2-genre-story-generator)慢)。**

现在你已经完成了对模型的实验，让我们来探索这个想法:我们正在一个包含不同流派电影情节的数据集上微调 OpenAI GPT-2 模型。本演练遵循**三幕结构:**

*   第一幕:什么是新 GPT 协议？
*   **第二幕:微调时间……**
*   **第三幕:生成时间！**

# 第一幕:什么是 GPT-2？

![](img/cac5908c56459796acf9b99d0a4816de.png)

马特·波波维奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

如果你熟悉 GPT 2 的关键思想，你可以快进到下一幕。否则，请快速复习下面的 GPT 新协议。

最近，我们看到了**文本生成模型 GPT-3** 背后的巨大宣传，以及它在使用零触发学习生成代码[](/will-gpt-3-kill-coding-630e4518c04d)**等任务中令人眼花缭乱的表现。GPT 2 号是 GPT 3 号的前身。**

**等等，为什么我们用 GPT 2 号而不是 GPT 3 号？嗯，还在测试阶段。还有…**

**正如其炒作一样，GPT-3 的尺寸也是 [**硕大**](https://github.com/openai/gpt-3) ( [传言要求**350 GB RAM**](https://github.com/openai/gpt-3/issues/1))。虽然体积仍然庞大，但 GPT-2 占用的空间少得多(最大的变体占用 [**6GB 磁盘空间**](https://github.com/openai/gpt-3/issues/1) )。它甚至有不同的尺寸[和](https://huggingface.co/transformers/model_doc/gpt2.html#:~:text=GPT%2D2%20is%20one%20of,code%20can%20be%20found%20here.)。所以，我们可以在很多设备上使用它(包括[iphone](https://twitter.com/julien_c/status/1154771974213816321?lang=en))。**

**如果你在寻找 GPT-2 的详细概述，你可以直接从[马嘴](https://openai.com/blog/better-language-models/#fn1)那里得到。**

**但是，如果你只是想快速总结一下 GPT 2 号，这里有一个经过提炼的纲要:**

*   **GPT-2 是一个**文本生成** [**语言模型**](https://web.stanford.edu/class/cs124/lec/languagemodeling.pdf) 使用一个[解码器专用变压器](http://jalammar.github.io/illustrated-gpt2/)(一个[变压器架构](https://papers.nips.cc/paper/7181-attention-is-all-you-need.pdf)的变体)。如果这看起来像是胡言乱语，只需知道 **Transformer** 是用于 [NLP](/your-guide-to-natural-language-processing-nlp-48ea2511f6e1) 模型的最先进的架构。**
*   **它被预先训练([具有 **40GB 的文本数据**](https://openai.com/blog/better-language-models/) ),任务是预测作为输入的前一文本在每个时间步长的下一个单词(更正式地说，令牌)。**

# **第二幕:微调时间…**

**![](img/5b9cfc3e21c9f285a6277006df27ccd4.png)**

**照片由 [Kushagra Kevat](https://unsplash.com/@kushagrakevat?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄**

**我们可以在选定的数据集上[微调](https://www.youtube.com/watch?v=5T-iXNNiwIs)(进一步训练)像 GPT-2 这样的预训练模型，以适应该数据集的性能。为了微调和使用 GPT-2 预训练模型，我们将使用 [**拥抱脸/变形金刚**](https://huggingface.co/transformers/) 库。它为我们做了所有繁重的工作。**

**出于这个想法，我通过清理、转换和组合 [**Kaggle** **维基百科电影情节数据集**](https://www.kaggle.com/jrobischon/wikipedia-movie-plots) 以及从维基百科搜集的超级英雄漫画情节来创建数据集文件。**

**[培训文件](https://drive.google.com/file/d/11FgexOt7PWxFnn9TbkMaEYVEo7gkHwkl/view)有 3 万多个故事。文件中的每一行都是这种格式的故事:**

****<博斯> <流派>故事到此为止……<EOS>****

******类型**:超级英雄、科幻、恐怖、动作、剧情、惊悚****

****要为另一项任务创建自己的数据集，如基于主题生成研究论文摘要，每个示例的格式可以如下所示:****

******< BOS > <科目>摘要此处……<EOS>******

****科目:物理、化学、计算机科学等。****

****我推荐你在 [**Google Colab**](/getting-started-with-google-colab-f2fff97f594c) 上训练模型(设置运行时为 **GPU** )。用于微调的 colab 笔记本的**链接我们的型号是这里的[](https://colab.research.google.com/drive/1l8rqAB4KMzAIDhqRN_DzHaxukDsEhrTX?usp=sharing)**。您可以创建此 [**笔记本**](https://colab.research.google.com/drive/1l8rqAB4KMzAIDhqRN_DzHaxukDsEhrTX?usp=sharing) 的副本，并为您自己的数据集进行修改。********

**这里有[**6 _ genre _ clean _ training _ data . txt**](https://drive.google.com/file/d/11FgexOt7PWxFnn9TbkMaEYVEo7gkHwkl/view?usp=sharing)(训练文件)和[**6 _ genre _ eval _ data . txt**](https://drive.google.com/file/d/1aaXwQ_hkOfSDhqqTsKTvlfIySi_0kX2x/view?usp=sharing)**(评估文件)，你需要在运行代码之前上传到你的 Colab 笔记本的环境中。****

****在这篇文章中，我将介绍 Colab 笔记本中的一些核心代码。****

```
**!pip install transformers torch**
```

****上面，我们正在安装核心库。我们需要**变形金刚**库来加载预先训练好的 GPT-2 检查点并对其进行微调。我们需要**手电筒**，因为它是用来训练的。****

******注意**:我从这个变形金刚示例[文件](https://github.com/huggingface/transformers/blob/master/examples/language-modeling/run_language_modeling.py)中取出代码，并对其进行了修改。****

****在上面的代码片段中，我们指定了模型的参数(**模型参数**)、数据参数(**数据训练参数**)和训练参数(**训练参数**)。让我们快速浏览一下这些参数的关键论点。要正确检查所有参数，查看[这里的](https://github.com/huggingface/transformers/blob/3dcb748e31be8c7c9e4f62926c5c144c62d07218/examples/language-modeling/run_language_modeling.py#L55)。****

## ****`ModelArguments`****

*   ******型号名称或路径:**用于指定型号名称或其路径。然后，模型被下载到我们的缓存目录中。查看[此处](https://huggingface.co/models?search=gpt2)的所有可用型号。这里，我们将**模型名称或路径**指定为 **gpt2** 。我们还有其他选项，如 gpt2-medium 或 gpt2-xl。****
*   ******model_type** :我们指定想要一个 gpt2 模型。这与上面的参数不同，因为，我们只指定型号类型，不指定名称(名称指 gpt2-xl、gpt2-medium 等。).****

## ****数据训练参数****

*   ******train_data_file** :我们提供培训文件。****
*   ******eval_data_file** :我们提供一个数据文件来评估模型(使用困惑)。困惑衡量我们的语言模型从 *eval_data_file* 生成文本的可能性。困惑度越低越好。****
*   ******逐行**:设置为 true，因为我们文件中的每一行都是一个新的故事。****
*   ******block_size** : [将每个训练示例缩短为最多只有 *block_size* 个令牌](https://github.com/huggingface/transformers/blob/3dcb748e31be8c7c9e4f62926c5c144c62d07218/examples/language-modeling/run_language_modeling.py#L119)。****

## ******训练参数******

****下面， *n* 是指这些参数的值。****

*   ******output_dir** :微调后的最终模型检查点保存在哪里。****
*   ******do_train，do_eval** :设置为 true，因为我们正在训练和评估。****
*   ******logging_steps** :每经过 *n* 个优化步骤，记录模型的损耗。****
*   ******per _ device _ train _ batch _ size**:每个训练优化步骤涉及 *n* 个训练样本。****
*   ******num_train_epochs** :训练数据集的完整遍数。****
*   ******save_steps** :在每个 *n* 优化步骤后保存中间检查点(如果 Colab 在几个小时后保持断开，建议使用)。****
*   ******save_total_limit** :任意点存储的中间检查点数。****

****现在，是时候加载模型、它的配置和标记器了。****

## ****正在加载模型、标记器和配置****

****类 **AutoConfig** 、 **AutoTokenizer** 、 **GPT2LMHeadModel** 根据 model_name 加载各自的配置( **GPT2Config** )、标记器( **GPT2Tokenizer** )和模型( **GPT2LMHeadModel** )。****

******GPT2Tokenizer** 对象对文本进行标记(将文本转换成标记列表)并将这些标记编码成数字。注意，令牌可以是单词，甚至是子单词(GPT-2 使用[字节对编码](https://huggingface.co/transformers/_modules/transformers/tokenization_gpt2.html)来创建令牌)。下面是一个标记化的例子(没有编码)。****

****在此之后，我们必须将代币编码成数字，因为计算机只处理数字。****

****我们有 **GPT2Config** 对象用于根据型号名称加载 **GPT-2 型号**的配置。****

****最后，我们有[**GPT 2 lmheadmodel**](https://huggingface.co/transformers/model_doc/gpt2.html#gpt2lmheadmodel)**对象，它根据模型名和 GPT2Config 加载模型。******

******有些人可能想知道:到底什么是" **LMHeadModel** ？简单地说，对于文本生成，我们需要一个语言模型(一个为词汇中的标记分配概率分布的模型)。GPT2LMHeadModel 是一个语言模型(它为词汇表中的每个标记分配分数)。所以，我们可以用这个模型来生成文本。******

****下一个主要步骤是添加特殊的令牌来将模型与我们的数据集成在一起。****

****数据集有一些特殊的标记，我们需要向模型指定这些标记是什么。我们可以传入一个 **special_tokens_dict** ，它可以有一些类似“bos_token”的键。要查看哪些键是允许的，请访问此[链接](https://huggingface.co/transformers/main_classes/tokenizer.html#transformers.SpecialTokensMixin)。****

*   ******bos_token** (" < BOS >")是出现在每个故事开头的令牌。****
*   ******EOS _ token**(“<EOS>”)是出现在每个故事结尾的 token。****
*   ******PAD _ token**(“<PAD>”)是指将较短的输出填充为固定长度的填充令牌。****

****然而，我们有一些**额外的特殊记号**(在本例中是我们的**流派记号**)在我们的 special_tokens_dict 中没有它们自己的键。这些额外令牌可以作为列表放在一个集合密钥“附加 _ 特殊 _ 令牌”下。****

****如果您正在使用自己的数据集，请用自己的类别替换流派标记。****

****我们已经为训练做好了准备，所以让我们创建**训练者**对象。****

******训练器**对象可用于训练和评估我们的模型。****

****为了创建**教练**对象，我们指定了模型、数据集以及训练参数(如上所示)。 **data_collator 参数**用于批量训练和评估示例。****

****然后，我们调用训练者的训练方法，并在训练后将模型保存到我们的输出目录中。是时候放松一下，让机器训练模型了。****

****训练完模型后，将模型检查点文件夹从 Colab 下载到您的计算机上。你可以将其部署为 [**Huggingface 社区模型**](https://huggingface.co/transformers/model_sharing.html) ，开发一个 [**web app**](https://medium.com/datadriveninvestor/streamlit-framework-every-data-scientist-must-know-7fa0ae775d6a) ，甚至可以使用该模型开发一个 [**手机 app**](/on-device-machine-learning-text-generation-on-android-6ad940c00911) 。****

# ****第三幕:世代时间！****

****![](img/32a9f1d53875bd1c4baaae142d3653b7.png)****

****埃里克·维特索在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片****

****激动人心的部分来了！如果你渴望创作故事，请在你的 Colab 笔记本中运行下面的单元格！****

****这里，我们使用了[**TextGenerationPipeline**](https://huggingface.co/transformers/main_classes/pipelines.html#textgenerationpipeline)对象，它简化了我们模型的文本生成，因为它抽象了输入预处理和输出后处理步骤。****

```
**text_generator = TextGenerationPipeline(model=model, tokenizer=tokenizer)**
```

******text_generator** 是 **TextGenerationPipeline** 类的一个对象。我们如何将这个对象作为一个函数来使用？这个类有一个 [__call__ method](https://www.geeksforgeeks.org/__call__-in-python/) ，允许它的对象像函数一样工作(当使用对象作为函数时调用这个方法)。这里，我们像使用函数一样使用这个对象，通过提供 input_prompt 来生成文本。这就是全部了。****

****我们还没有解释一些文本生成参数，比如 top_p。如果你想了解这些参数，请跟在片尾字幕后面。****

# ****尾声和片尾字幕****

****概括地说，我们采用了各种类型的电影情节数据集，并将其馈送给 **GPT2LMHeadModel** 来微调我们的模型，以生成特定类型的故事。使用这些想法，您还可以创建其他数据集来基于这些数据集生成文本。****

****要测试我的电影故事生成器，请单击此处的[](https://huggingface.co/pranavpsv/gpt2-genre-story-generator)****(或使用 web app [**此处的**](http://52.91.134.248:8501/) )。********

****非常感谢 [**Huggingface** **团队**](https://huggingface.co/) 提供的变形金刚库和详细的例子，以及 Raymond Cheng 撰写的这篇 [**文章**](/fine-tuning-gpt2-for-text-generation-using-pytorch-2ee61a4f1ba7) **帮助我创建了我的模型。******

# ****可选:探索文本生成参数****

****首先，让我们了解一下 **GPT2LMHeadModel** 的输出是什么:****

****当预测下一个标记时，该模型为其词汇表中所有可能的标记生成逻辑。为了简化起见，可以将这些逻辑看作每个标记的分数。具有较高分数(logits)的记号意味着它们有较高的概率成为合适的下一个记号。然后，我们对所有令牌的 logits 应用 softmax 操作。我们现在得到每个令牌的 softmax 分数，该分数在 0 和 1 之间。所有令牌的 softmax 分数总和为 1。这些 softmax 分数可以被认为是在给定一些先前文本的情况下，某个标记成为合适的下一个标记的概率(尽管它们不是)。****

****以下是参数的基本大纲:****

*   ******max_length** [int]:指定要生成的令牌的最大数量。****
*   ******do _ sample**【bool】:指定是否使用[采样](https://huggingface.co/blog/how-to-generate)(比如 top_p 或者使用贪婪搜索)。贪婪搜索是在每个时间步选择最可能的单词(不推荐，因为文本会重复)。****
*   ******repetition _ penalty**【float】:指定对重复的惩罚。增加**重复 _ 惩罚**参数以减少重复。****
*   ******温度**【浮动】:用于增加或减少对高逻辑令牌的依赖。增加温度值会减少只有少数令牌具有非常高的 softmax 分数的机会。****
*   ******top _ p**【float】:指定仅考虑概率(形式上，softmax)得分之和不超过 **top_p** 值的令牌****
*   ******top_k**【int】:告诉我们在生成文本时只考虑 top _ k 个标记(按它们的 softmax 分数排序)。****

****仅此而已。****