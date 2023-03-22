# Rick 和 Morty 使用变形金刚和 Streamlit 通过 GPT2 生成故事

> 原文：<https://towardsdatascience.com/rick-and-morty-story-generation-with-gpt2-using-transformers-and-streamlit-in-57-lines-of-code-8f81a8f92692?source=collection_archive---------25----------------------->

![](img/0fe4919b09233bcc97640a5d79020e22.png)

贝尼尼奥·霍尤拉在 [Unsplash](https://unsplash.com/) 上拍摄的照片。

## 这篇文章将向你展示如何使用 Hugging Face 的 Transformers 库在 Rick 和 Morty 的抄本上*微调*一个*预训练的* GPT2 模型，构建一个演示应用程序，并使用 Streamlit 共享部署它。

# 介绍

随着机器学习(ML)和自然语言处理(NLP)的快速发展，新算法能够生成看起来越来越像人类制作的文本。其中一种算法 GPT2 已经在许多开源应用程序中使用。GPT2 在 WebText 上接受训练，WebText 包含来自 Reddit 的 4500 万个出站链接(即评论引用的网站)。排名前 10 的出站域名包括*谷歌*、*存档*、 *Blogspot* 、 *Github* 、 *NYTimes* 、 *WordPress* 、*华盛顿邮报*、 *Wikia* 、 *BBC* 和*卫报*。*预训练* GPT2 模型可以在特定数据集上*微调*，例如，以“获取”数据集的样式或学习对文档进行分类。这是通过*迁移学习*完成的，迁移学习可以定义为“从源设置中提取知识并将其应用于不同目标 setting"⁴.的一种方式有关 GPT2 及其架构的详细解释，请参见原版 paper⁵、OpenAI 的博客 post⁶或杰伊·阿拉姆马的插图 guide⁷.

# 资料组

用于*微调* GPT2 的数据集由 [Rick 和 Morty 抄本](https://rickandmorty.fandom.com/wiki/Category:Transcripts)的前 3 季组成。我过滤掉了所有不是由里克、莫蒂、萨默、贝丝或杰瑞产生的对话。数据被下载并以原始文本格式存储。每行代表一个说话者和他们的话语或一个动作/场景描述。数据集被分为训练和测试数据，分别包含 6905 和 1454 行。原始文件可以在这里找到[。训练数据用于*微调*模型，而测试数据用于评估。](https://github.com/e-tony/Story_Generator_RnM/tree/main/data)

# 训练模型

抱抱脸的变形金刚库提供了一个[简单脚本](https://github.com/huggingface/transformers/tree/master/examples/language-modeling#gpt-2gpt-and-causal-language-modeling)来*微调*一个定制的 GPT2 模型。你可以使用[这款](https://colab.research.google.com/drive/1opXtwhZ02DjdyoVlafiF3Niec4GqPJvC?usp=sharing)谷歌 Colab 笔记本来微调你自己的模型。一旦您的模型完成了训练，确保您下载了包含所有相关模型文件的已训练模型输出文件夹(这对于以后加载模型是必不可少的)。你可以在拥抱脸的模型 Hub⁸上传你的定制模型，让公众可以访问它。当在测试数据上评估时，该模型获得了大约 17 的困惑分数。

# 构建应用程序

首先，让我们为 Python 3.7 创建一个名为`Story_Generator`的新项目文件夹和一个虚拟环境:

```
mkdir Story_Generator
cd Story_Generator
python3.7 -m venv venv
source venv/bin/activate
```

接下来，我们要安装项目的所有依赖项:

```
pip install streamlit-nightly==0.69.3.dev20201025
pip install torch==1.6.0+cpu torchvision==0.7.0+cpu -f [https://download.pytorch.org/whl/torch_stable.html](https://download.pytorch.org/whl/torch_stable.html)
pip install git+git://github.com/huggingface/transformers@59b5953d89544a66d73
```

我们的整个应用程序将驻留在`app.py`中。让我们创建它并导入新安装的依赖项:

```
import urllib
import streamlit as st
import torch
from transformers import pipeline
```

在我们做任何处理之前，我们希望我们的模型加载。通过使用`@st.cache`装饰器，我们可以执行一次`load_model()`函数，并将结果存储在本地缓存中。这将提高我们的应用程序性能。然后，我们可以使用`pipeline()`功能简单地加载一个模型来生成文本(将模型路径替换为您的定制模型，或者从模型中心使用 [my *预训练的*模型](https://huggingface.co/e-tony/gpt2-rnm)):

```
[@st](http://twitter.com/st).cache(allow_output_mutation=True, suppress_st_warning=True)
def load_model():
    return pipeline("text-generation", model="e-tony/gpt2-rnm")model = load_model()
```

我们可以使用 Streamlit 的`text_area()`函数来制作一个简单的文本框。我们还可以提供高度和允许的最大字符数(因为大文本需要更长的时间来生成):

```
textbox = st.text_area('Start your story:', '', height=200, max_chars=1000)
```

现在我们已经有了第一行代码，我们可以通过运行应用程序来查看它是什么样子的(我们也可以通过刷新页面来查看实时更改):

```
streamlit run app.py
```

接下来，我们可以添加一个 slider 小部件，允许用户决定模型应该生成多少个字符:

```
slider = st.slider('Max story length (in characters)', 50, 200)
```

我们现在准备生成文本！让我们创建一个执行文本生成的按钮:

```
button = st.button('Generate')
```

我们希望我们的应用程序监听“按钮按压”动作。这可以通过一个简单的条件语句来完成。然后我们可以生成文本并将其输出到屏幕上:

```
if button:
    output_text = model(textbox, max_length=slider)[0]['generated_text']

    for i, line in enumerate(output_text.split("\n")):
        if ":" in line:
            speaker, speech = line.split(':')
            st.markdown(f'__{speaker}__: {speech}')
        else:
            st.markdown(line)
```

让我们在文本框中输入提示并生成一个故事:

```
Rick: Come on, flip the pickle, Morty. You're not gonna regret it. The payoff is huge.
```

输出:

```
Rick: Come on, flip the pickle, Morty. You're not gonna regret it. The payoff is huge. You don't have to be bad, Morty.
(Rick breaks up)
[Trans. Ext. Mortys home]
```

太好了！模型正在输出新的文本，看起来不错。我们可以通过调整*解码*方法的参数来提高输出质量。参见拥抱脸在 decoding⁹的帖子，了解不同方法的详细概述。让我们替换我们的`model()`函数，并应用更多的参数:

```
output_text = model(textbox, do_sample=True, max_length=slider, top_k=50, top_p=0.95, num_returned_sequences=1)[0]['generated_text']
```

简而言之，`do_sample`随机挑选下一个单词，`top_k`过滤最有可能的 *k* 下一个单词，`top_p`允许动态增加和减少可能的下一个单词的数量，`num_returned_sequences`输出多个独立样本(在我们的例子中只有 1 个)用于进一步过滤或评估。您可以使用这些值来获得不同类型的输出。

让我们使用这种解码方法生成另一个输出。

输出:

```
Rick: Come on, flip the pickle, Morty. You're not gonna regret it. The payoff is huge.
Morty: Ew, no, Rick! Where are you?
Rick: Morty, just do it! [laughing] Just flip the pickle!
Morty: I'm a Morty, okay?
Rick: Come on, Morty. Don't be ashamed to be a Morty. Just flip the pickle.
```

我们的输出看起来更好！这种模式仍然会产生不合逻辑和无意义的文本，但新的模式和解码方法可能会解决这个问题。

不幸的是，我们的模型有时会产生伤害性的、粗俗的、暴力的或歧视性的语言，因为它是根据来自互联网的数据训练的。我们可以通过简单地从 451 个单词的列表[中检查粗俗的单词来应用不良单词过滤器来审查有害的语言。我敦促读者考虑使用进一步的过滤器，比如过滤仇恨言论。该滤波器可以按如下方式实现:](https://raw.githubusercontent.com/RobertJGabriel/Google-profanity-words/master/list.txt)

```
def load_bad_words() -> list:
    res_list = []file = urllib.request.urlopen("[https://raw.githubusercontent.com/RobertJGabriel/Google-profanity-words/master/list.txt](https://raw.githubusercontent.com/RobertJGabriel/Google-profanity-words/master/list.txt)")
    for line in file:
        dline = line.decode("utf-8")
        res_list.append(dline.split("\n")[0])

    return res_listBAD_WORDS = load_bad_words()

def filter_bad_words(text):
    explicit = False

    res_text = text.lower()
    for word in BAD_WORDS:
        if word in res_text:
            res_text = res_text.replace(word, word[0]+"*"*len(word[1:]))
            explicit = Trueif not explicit:
        return textoutput_text = ""
    for oword,rword in zip(text.split(" "), res_text.split(" ")):
        if oword.lower() == rword:
            output_text += oword+" "
        else:
            output_text += rword+" "return output_textoutput_text = filter_bad_words(model(textbox, do_sample=True, max_length=slider, top_k=50, top_p=0.95, num_returned_sequences=1)[0]['generated_text'])
```

我们最终的`app.py`文件现在看起来像这样:

```
import urllib
import streamlit as st
import torch
from transformers import pipelinedef load_bad_words() -> list:
    res_list = []file = urllib.request.urlopen("[https://raw.githubusercontent.com/RobertJGabriel/Google-profanity-words/master/list.txt](https://raw.githubusercontent.com/RobertJGabriel/Google-profanity-words/master/list.txt)")
    for line in file:
        dline = line.decode("utf-8")
        res_list.append(dline.split("\n")[0])

    return res_listBAD_WORDS = load_bad_words()

[@st](http://twitter.com/st).cache(allow_output_mutation=True, suppress_st_warning=True)
def load_model():
    return pipeline("text-generation", model="e-tony/gpt2-rnm")def filter_bad_words(text):
    explicit = False

    res_text = text.lower()
    for word in BAD_WORDS:
        if word in res_text:
            res_text = res_text.replace(word, word[0]+"*"*len(word[1:]))
            explicit = Trueif not explicit:
        return textoutput_text = ""
    for oword,rword in zip(text.split(" "), res_text.split(" ")):
        if oword.lower() == rword:
            output_text += oword+" "
        else:
            output_text += rword+" "return output_textmodel = load_model()
textbox = st.text_area('Start your story:', '', height=200, max_chars=1000)
slider = slider = st.slider('Max text length (in characters)', 50, 1000)
button = st.button('Generate')if button:
    output_text = filter_bad_words(model(textbox, do_sample=True, max_length=slider, top_k=50, top_p=0.95, num_returned_sequences=1)[0]['generated_text'])

    for i, line in enumerate(output_text.split("\n")):
        if ":" in line:
            speaker, speech = line.split(':')
            st.markdown(f'__{speaker}__: {speech}')
        else:
            st.markdown(line)
```

您还可以在 Github [资源库](https://github.com/e-tony/Story_Generator)中查看我的[演示](https://share.streamlit.io/e-tony/story_generator/main/app.py)的代码，因为它包含了修改应用程序功能和外观的有用代码。

它现在已经准备好上线了！

# 部署应用程序

可以使用 Streamlit 共享⁰.部署该应用程序您只需要有一个公共的 Github 存储库，存储库中有一个`requirements.txt`和一个`app.py`文件。您的`requirements.txt`文件应该是这样的:

```
-f [https://download.pytorch.org/whl/torch_stable.html](https://download.pytorch.org/whl/torch_stable.html)
streamlit-nightly==0.69.3.dev20201025
torch==1.6.0+cpu
torchvision==0.7.0+cpu
transformers @ git+git://github.com/huggingface/transformers@59b5953d89544a66d73
```

在 Streamlit Sharing [网站](https://share.streamlit.io/)上，您可以简单地链接您的存储库，您的模型将很快上线！

# 道德考量

本文中介绍的应用程序仅用于娱乐目的！应该仔细考虑在其他场景中应用 GPT2 模型。虽然从原始训练数据中删除了某些域，但 GPT2 模型是在来自互联网的大量未经过滤的内容上进行预训练的，这些内容包含有偏见和歧视性的语言。OpenAI 的[型号卡](https://github.com/openai/gpt-2/blob/master/model_card.md#intended-uses)指出了这些注意事项:

> 以下是我们认为可能的一些次要使用案例:
> 
> -写作辅助:语法辅助、自动补全(针对普通散文或代码)
> 
> -创造性写作和艺术:探索创造性虚构文本的生成；帮助诗歌和其他文学艺术的创作。
> 
> -娱乐:创造游戏、聊天机器人和娱乐一代。
> 
> 范围外的使用案例:
> 
> 因为像 GPT-2 这样的大规模语言模型不能区分事实和虚构，所以我们不支持要求生成的文本是真实的用例。此外，像 GPT-2 这样的语言模型反映了他们接受培训的系统固有的偏见，所以我们不建议将它们部署到与人类交互的系统中，除非部署者首先进行与预期用例相关的偏见研究。我们发现 774M 和 1.5B 之间在性别、种族和宗教偏见调查方面没有统计学上的显著差异，这意味着所有版本的 GPT-2 都应该以类似的谨慎程度对待对人类属性偏见敏感的用例。

下面的例子展示了模型如何产生有偏差的预测(另一个例子可以在[这里找到](https://huggingface.co/gpt2#limitations-and-bias)):

```
>>> from transformers import pipeline, set_seed
>>> generator = pipeline('text-generation', model='gpt2')
>>> set_seed(42)
>>> generator("The man worked as a", max_length=10, num_return_sequences=5)[{'generated_text': 'The man worked as a waiter at a Japanese restaurant'},
 {'generated_text': 'The man worked as a bouncer and a boun'},
 {'generated_text': 'The man worked as a lawyer at the local firm'},
 {'generated_text': 'The man worked as a waiter in a cafe near'},
 {'generated_text': 'The man worked as a chef in a strip mall'}]>>> set_seed(42)
>>> generator("The woman worked as a", max_length=10, num_return_sequences=5)[{'generated_text': 'The woman worked as a waitress at a Japanese restaurant'},
 {'generated_text': 'The woman worked as a waitress at a local restaurant'},
 {'generated_text': 'The woman worked as a waitress at the local supermarket'},
 {'generated_text': 'The woman worked as a nurse in a health center'},
 {'generated_text': 'The woman worked as a maid in Daphne'}]
```

我敦促读者仔细考虑这些模型在真实场景中的应用和使用。有许多资源(如 EML、艾诺)可用于了解道德规范。

# 结论

恭喜你！您的应用程序现已上线！

通过使用开源框架，我们能够快速微调 GPT2 模型，原型化有趣的应用程序，并部署它。通过使用更先进的*预训练*模型、*解码*方法，甚至*结构化语言预测*，可以进一步改进生成的故事。

*你可以测试出*[*demo*](https://share.streamlit.io/e-tony/story_generator/main/app.py)*或者在*[*Github*](https://github.com/e-tony/Story_Generator)*上查看项目。*

# 参考

[1]: [GPT2](https://github.com/openai/gpt-2)

[2]: [排名前 30 的 Gpt2 开源项目](https://awesomeopensource.com/projects/gpt-2)

[3]: [自然语言处理中迁移学习的状态](https://ruder.io/state-of-transfer-learning-in-nlp/)

[4]: [网络文本中出现的前 1000 个域名](https://github.com/openai/gpt-2/blob/master/domains.txt)

[5]: A .、Jeffrey Wu、R. Child、David Luan、Dario Amodei 和 Ilya Sutskever 2019。语言模型是无人监督的多任务学习者。

[6]: [更好的语言模型及其含义](https://openai.com/blog/better-language-models/)

[7]: [图解 GPT-2(可视化变压器语言模型)](http://jalammar.github.io/illustrated-gpt2/)

[8]: [模型分享上传](https://huggingface.co/transformers/model_sharing.html)

[9]: [如何生成文本:用变形金刚使用不同的解码方法进行语言生成](https://huggingface.co/blog/how-to-generate)

[10]: [部署一个 app](https://docs.streamlit.io/en/stable/deploy_streamlit_app.html)

[11]: [伦理人工智能研究所&机器学习](https://ethical.institute/)

[12]: [艾现在研究所](https://ainowinstitute.org/)