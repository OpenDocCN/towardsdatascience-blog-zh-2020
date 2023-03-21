# Python:面向(有抱负的)数据科学家的(非官方)OOP 速成班！

> 原文：<https://towardsdatascience.com/python-the-unofficial-oop-crash-course-for-aspiring-data-scientists-ed2ed3789bcf?source=collection_archive---------7----------------------->

## 类、属性、方法、继承、封装和所有其他你不确定是否真的需要的晦涩难懂的东西(剧透:为了你的理智，你需要！)

![](img/0930752ee7837ba94e875157312ca143.png)

信用:Pixabay

## 背景

Python 正经历着市场需求和用户基础的巨大增长；无论您是开发人员、分析师、研究人员还是工程师，python 都有可能在您的领域中得到应用。这么多免费的教育资料(比如*自动化枯燥的东西)进入门槛不能再低了。)*奇怪的是，然而，我们看到了一个不寻常的结果:python 爱好者过早地停滞不前，终止了他们对 python 基础的研究，转而支持特定领域的库和框架学习。对于当今的数据科学文化来说尤其如此；在对字符串、列表和字典有了中等程度的熟悉之后，未来的 python 爱好者可以直接进入 Numpy、Pandas 和 Scikit-Learn(如果不是 Keras、TensorFlow 或 PyTorch 的话)。)

那么应该学习数据结构与算法(DS&A)，从零开始实现二叉树或链表吗？开始特定领域的旅程，什么是合适的“我已经学了足够多的 python 基础知识”门槛？没有“一刀切”的答案；然而，我建议在进入回归、分类和聚类等主题之前，至少要熟悉 OOP。

## 什么是面向对象编程(OOP)？

为了理解 OOP，我们需要简单讨论一下*函数式编程*；无需深究这种范式，只需说，函数式编程将函数(动作)与数据(信息)分开，而 OOP 认为这是一种错误的二分法。你以前用过 python 的内置列表吗？(**是**)想必你已经注意到了`append`方法允许你在当前最后一个元素之后插入一个元素，自动增加列表的长度？(**是**

*恭喜恭喜！你喜欢 OOP。数据类型及其方法(附属于对象的函数)是一个内聚的整体，这是 OOP 的核心“本质”。*

在下面的代码片段中，我将定义一两个类，并围绕一个已建立的机器学习任务演示一些基本的 OOP 概念，以便您可以看到 OOP 如何为数据科学家带来好处。我们将使用各种 ML 算法对 Yelp 评论进行分类。事实上，这个类将只接收两个强制参数，数据和您希望使用的模型。(当然还会有其他几种算法)，但是，这个类架构将允许我们做以下事情:

(1)将数据分为训练集和测试集，(2)将数据预处理并向量化为 TF-IDF 令牌，(3)训练模型，(4)计算测试集性能的准确性和相关度量，以及(5)通过 pickle 保存模型。

这条管道将大大提高你的效率。和许多其他人一样，我在对基础知识有一个坚实的掌握之前就一头扎进了数据科学的材料中。我会上下滚动笔记本，寻找我之前定义的*右*变量。如果我想比较 2 或 3 个模型的性能，这将成为一个噩梦，确保我引用了适当的变量名。您将会看到，使用下面的方法，比较模型性能将是一项微不足道的任务，并且(如果幸运的话)我将会把您变成一名 OOP 的传道者！

## 面向对象的情感分析

关于显示问题，见[本](https://gist.github.com/jdmoore7/e1a5696f65c49f17fdb716a746c220b5) GitHub 要诀。

GitHub Gist

正如你在上面注意到的，我定义了两个类:`DataSplitter`和`Classifier`。首先，让我们看看 DataSplitter 稍后我们将访问分类器。

DataSplitter 接收一个 dataframe、文本列的名称和情感列的名称。然后，train_test_split 用于为类分配以下属性:x_train、x_test、y_train 和 y_test。请注意，random_state 和 test_percent 参数有默认值。这意味着——除非您特别更改这些参数中的任何一个，否则两个类实例将具有相同的 x_train、x_test、y_train 和 y_test 属性。这将是有用的，因为我们可以直接比较 ML 模型，而不用担心它们是在(稍微)不同的数据集上训练的。

当 DataSplitter 类对象被实例化时，您可以非常简单地访问这些值:

```
import pandas as pd
data = pd.read_csv('yelp_samples_500k.csv')
d = data.sample(n=1000)ds = DataSplitter(data=d,x_var='text',y_var='sentiment')
ds.x_test>>>
267157    This place always has a line so I expected to ...
197388    Would give them a zero stars if I could. They ...
```

如您所见，`self`关键字*将这些属性绑定到对象上。使用点符号，检索这些属性很简单。*

继续我们的下一个 OOP 概念:*继承*！对于这个例子，我们将进入第二个类，分类器。继承仅仅意味着一个类从以前定义的类继承功能。在我们的例子中，分类器将继承 DataSplitter 的所有功能；请注意两点:(1) DataSplitter 在其定义`DataSplitter()`中没有接收任何要继承的类，而`Classifier(DataSplitter)` *接收了一个要继承的类。(2)分类器的`__init__`方法中使用了`super`关键字。这样做的效果是执行 DataSplitter 的 init 方法，然后继续执行所有其他特定于分类器自己的 init 方法的指令。*底线是，我们训练/测试/分割我们的数据，而不需要重新输入所有代码！**

在 init 方法之后，你会看到`__vectorize`。注意定义前面的双*下划线。这就是在 python 中实现*封装*的方式。封装意味着~对象拥有程序员不具备的访问属性和方法。换句话说，为了不分散程序员的注意力，它们被抽象(或封装)了。*

```
from sklearn.naive_bayes import MultinomialNB
nb = MultinomialNB()
c = Classifier(data=d,model_instance=nb,
x_var='text',y_var='sentiment')c.__vectorize('some text')
>>>
AttributeError: 'Classifier' object has no attribute '__vectorize'
```

换句话说，对象可以访问这些方法，但是，*我们*不能！

说到方法，如果你没有猜到的话——方法只是内置于类对象中的函数。我们上面定义的方法包括 _ _ 矢量化、_ _ 拟合、_ _ 评估准确性、度量、预测和保存。从向量化开始，我们使用 NLTK 的 word_tokenize 函数将所有单词和标点符号标记为一元语法。接下来，我们使用 NLTK 的 ngrams 函数来创建二元模型和三元模型

```
'I like dogs' #text
['I', 'like', 'dogs'] #unigrams
['I like', 'like dogs'] #bigrams
['I like dogs'] #trigram 
```

这种方法大大改进了 unigrams，因为它允许 ML 模型学习“好”和“不好”之间的区别。请注意，我没有删除停用词或标点符号，也没有词干标记。如果你对扩展功能感兴趣，这可能是一个很好的家庭作业！__vectorize 方法被传递给由 __fit 方法创建的管道属性。同样，__fit 方法训练提供给 init 方法的 ML 模型，并创建预测(preds。)下面的方法 __evaluate_accuracy 确定模型的二进制精度，并赋值为一个类属性，以便以后访问(无需多次重新计算。)接下来，我们的度量方法将为我们检索二进制准确性，或者打印分类报告(精度、召回等)。)我们的预测方法简单地产生了这个代码。向方法调用提供文本，要么分配一个类，要么返回属于类 1 的概率。(如果你对多类分类感兴趣，一些调整将是必要的——我将把它作为家庭作业留给你！)最后，save 方法接收一个文件路径并挑选整个对象。这很简单——我们所要做的就是打开 pickled 文件来访问所有的类方法和属性，包括完全训练好的模型！

```
from sklearn.naive_bayes import MultinomialNB
nb = MultinomialNB()
nb_model = Classifier(data=data,model_instance=nb
,x_var='text',y_var='sentiment')nb_model.metrics()
>>>
'92.77733333333333 percent accurate'
```

让我们将它与随机森林分类器进行比较！

```
from sklearn.ensemble import RandomForestClassifier
rf = RandomForestClassifier()
rf_model = Classifier(data=data,model_instance=rf,
x_var='text',y_var='sentiment')rf_model.metrics()
>>>
'86.29666666666667 percent accurate'
```

似乎我们的朴素贝叶斯模型优于我们的随机森林分类器。保存我们的对象(并再次打开它以备后用)非常简单:

```
## saving
nb_model.save('nb_model') #will append .pkl to end of input## opening for later use
with open('nb_model.pkl','rb') as f:
    loaded_model = pickle.load(f)loaded_model.predict("This tutorial was super awesome!",prob=True)
>>>
0.943261472480177 # 94% certain of positive sentiment
```

我希望你喜欢这个教程；我的目标是——简洁、信息丰富、实用。为了满足所有这三个目标，一些 OOP 主题没有入选(例如，*多态性*)。)如果您觉得这有帮助，请发表评论。同样，*如果你想让我探索类似的概念，请发表评论，我会看看是否能在未来的文章中有所体现。*

**感谢您的阅读——如果您认为我的内容没问题，请订阅！:)**