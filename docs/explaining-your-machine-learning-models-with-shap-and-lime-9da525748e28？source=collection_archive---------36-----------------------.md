# 用 SHAP 和莱姆解释你的机器学习模型！

> 原文：<https://towardsdatascience.com/explaining-your-machine-learning-models-with-shap-and-lime-9da525748e28?source=collection_archive---------36----------------------->

![](img/f6df4841c6087ceb956892897c3debda.png)

## 帮助您揭开一些人可能认为您的机器学习模型是“黑箱”的神秘面纱

大家好！欢迎再次回到另一个数据科学快速技巧。这篇特别的文章对我来说是最有趣的，不仅因为这是我们迄今为止处理的最复杂的主题，而且也是我刚刚花了几个小时自学的一个主题。当然，还有什么比想出如何把它教给大众更好的学习方法呢？

在开始之前，我已经把这篇文章中展示的所有作品上传到了一个奇异的 Jupyter 笔记本上。如果你想更深入地了解，你可以在[我的个人 GitHub](https://github.com/dkhundley/ds-quick-tips/blob/master/006_shap_lime/titanic_shap_lime.ipynb) 找到它。

因此，尽管这是一个幕后的非常复杂的话题，但我还是打算尽可能地为尽可能多的观众淡化这个话题。尽管这最终是一篇为数据科学从业者设计的帖子，但我认为对于任何业务人员来说，理解他们为什么应该关注这个话题也同样重要。

在进入如何计算/可视化这些值之前，让我们先对为什么我们会关心这个话题建立一些直觉。

# 为什么你应该关心 ML 的可解释性

如果你对机器学习有所了解，你会知道你将一些数据放入一个预测模型，它会在另一端产生一个输出预测。但是让我们诚实地说:你知道在那个模型里面到底发生了什么吗？请记住，我们正在谈论的是在引擎盖下进行大量复杂数学运算的算法。实际上，这可能就是你的 ML 模型现在的样子:

![](img/a3714b8a8b8307de925e0bb0fcba701f.png)

你真的不知道那个黑盒里发生了什么。数学完全掩盖了这些特性在模型中的重要性。那么，如果特性 1 和 2 承担了所有的重量，而特性 3 和 4 没有给模型增加任何价值呢？您更希望看到的是类似这样的东西:

![](img/e92e5b45c86c4e1cfeda3d8f2382e505.png)

好的，在我们的例子中，我们显然使用了外行人的术语。但这一点很重要，因为如果你的模型依赖一两个特征来做所有的预测，这可能表明有问题。这也可能表明你正在使用某种可能被认为是不道德的功能。例如，今天许多公司会避免在他们的模型中使用性别或种族之类的东西，因为它们往往会在模型中产生许多不希望的偏见。

通过一个具体的例子来理解这些概念总是更有意义，所以我们将回到我们在最近几篇文章中使用的数据集来具体演示 SHAP 和莱姆:泰坦尼克号数据集。

# 用泰坦尼克号数据集建模

好吧，所以在我们使用泰坦尼克号数据集的最后几个帖子中，我们创建了非常差的模型，因为我们的重点更多的是学习新技能。为了有效地演示 SHAP 和莱姆，我们真的需要一个模型来显示某种程度的“尝试”(？)这样我们就可以看到 ML 的可解释性是如何发挥作用的。现在，我不打算在这篇文章中花时间向您展示我的数据清理和建模部分的所有代码(您肯定可以在我的 GitHub 中找到[这里](https://github.com/dkhundley/ds-quick-tips/blob/master/006_shap_lime/titanic_shap_lime.ipynb)，但是我将快速总结产生最终模型的特性。

*   **PClass** :记录这个人上船的船票等级。(1 档、2 档或 3 档。)
*   **年龄箱**:根据人的年龄，他们被分成一般人群。这些群体包括儿童、青少年、年轻人、成年人、老年人和未知人群。
*   **性别(Gender)** :注明该人是男是女。
*   **船舱段**:如果这个人有船舱，我就拉出他们可能在哪个舱。这些可以是 A 舱、B 舱、C 舱，也可以是无舱。
*   **登船**:记下此人最初出发的三个地点之一。
*   **SibSp(兄弟姐妹/配偶)**:记录此人在船上有多少兄弟姐妹和/或配偶。
*   **Parch (Parent / Child)** :记录这个人在船上有多少父母/孩子。

对于实际的模型本身，我选择使用来自 Scikit-Learn 的一个简单的随机森林分类器。将数据分为训练集和验证集，我得到了验证集的以下指标:

![](img/9609606111c476d6efcfecf8a7d2a3a9.png)

这些指标并不是很好，但是对我们的学习来说很好。至少我们将在下一节看到这些特性有多重要。让我们继续前进！

# 用 SHAP 和石灰解释

在这一点上，我们目前还不知道我们的随机森林模型是如何使用这些特征进行预测的。我猜性别和年龄是影响因素，但我们还不太清楚。

这是聪明人基于博弈论开发复杂算法来生成这些东西的地方，以更好地解释我们当前的黑匣子。当然，我指的是 SHAP 和酸橙。现在，我要 100%诚实:我不太熟悉这些算法是如何在引擎盖下工作的，所以我甚至不打算在这篇文章中尝试这样做。相反，我会给你介绍[这篇关于 SHAP 的文章](/shap-explained-the-way-i-wish-someone-explained-it-to-me-ab81cc69ef30)和[另一篇关于石灰的文章](https://medium.com/analytics-vidhya/explain-your-model-with-lime-5a1a5867b423)。

就像 Scikit-Learn 抽象出我们的随机森林分类器的底层算法一样，我们将使用一些简洁的 Python 库来抽象出 SHAP 和莱姆的内部工作原理。要使用它们，您所需要做的就是对两者进行简单的 pip 安装:

```
pip install shappip install lime
```

在高层次上，这两种工作方式都是将您的训练数据和模型交给“解释者”，然后您可以将任何观察结果传递给“解释者”，它会告诉您该模型的特性重要性。这听起来像是天书，但是我们会用泰坦尼克号的例子来说明这一点。

首先，我们需要从我们的验证集中选出两个人，我们知道他们分别没有幸存和幸存。这是因为我们将通过 SHAP 和莱姆的解释来了解这些人的哪些特征对我们的模型的生存能力有最大的影响。(还是那句话，你可以在我的笔记本里看到这个作品。)

## 通过 SHAP 解释

让我们从 SHAP 开始。这里的语法非常简单。我们将首先实例化 SHAP 解释器对象，使我们的随机森林分类器(rfc)适合该对象，并插入每个人以生成他们可解释的 SHAP 值。下面的代码向您展示了如何为第一个人做这件事。要对人员 2 进行同样的操作，只需交换适当的值。

```
# Importing the SHAP library
import shap# Instantiating the SHAP explainer using our trained RFC model
shap_explainer = shap.TreeExplainer(rfc)
shap.initjs()# Getting SHAP values for person 1
person_1_shap_values = shap_explainer.shap_values(person_1)# Visualizing plot of expected survivability of person 1
shap.force_plot(shap_explainer.expected_value[1], person_1_shap_values[1], person_1)# Visualizing impact of each feature for person 1
shap.summary_plot(person_1_shap_values, person_1)
```

这真的就是全部了！让我们在下面的截图中看看我们刚才所做的实际输出。首先，一号人物。

![](img/923826050382e70662bc74e16bd4330e.png)

根据上图，我们的模型预测第一个人有 94%的机会幸存，这是正确的。阅读起来有点困难，但看看第二个视觉，看起来最有影响的因素包括这个人的性别，这个人是否有三等票，以及这个人是否是个孩子。在这种情况下，第一个人是一个女孩，所以这两个因素促使我们的模型说是的，这个人可能活了下来。

现在让我们看看第二个人。

![](img/39841f7ac98d4646aa1b60d68b51f997.png)

我们的模型预测这个人可能没有活下来，不幸的是这是正确的。再看看这里的影响因素，看起来第二个人是一个没有小屋的成年男性。我不是历史爱好者，但我猜想妇女和儿童首先被救出船外，所以对我来说，第一个人(一个女孩)幸存而第二个人(一个成年男子)没有生还是很有意义的。

好吧，让我们继续我们的石灰解释！

## 通过石灰的可解释性

使用 LIME 的语法有点不同，但是从概念上讲，SHAP 和 LIME 做了很多相似的事情。

```
# Importing LIME
import lime.lime_tabular# Defining our LIME explainer
lime_explainer = lime.lime_tabular.LimeTabularExplainer(X_train.values, mode = 'classification', feature_names = X_train.columns, class_names = ['Did Not Survive', 'Survived'])# Defining a quick function that can be used to explain the instance passed
predict_rfc_prob = lambda x: rfc.predict_proba(x).astype(float)# Viewing LIME explainability for person 1
person_1_lime = lime_explainer.explain_instance(person_1.iloc[0].values, predict_rfc_prob, num_features = 10)
person_1_lime.show_in_notebook()
```

让我们来看看 LIME 为每个人展示了什么。

![](img/7f4e6b8d54d35218791bda1428e995cf.png)

我在这里有点偏见，但我认为这些石灰值的用户界面更容易阅读 SHAP。我们看到每个人是否幸存的预测概率是一样的。在中间，我们看到了 10 大特征的影响力。与 SHAP 相比，莱姆在解释能力上有一点点不同，但它们大体上是相同的。我们再次看到，性别是一个巨大的影响因素，无论这个人是否是个孩子。在右边，它们很好地显示了这个特定观察的精确值。

那么 SHAP 和莱姆哪个更好呢？我会说都不是。虽然两者都可以很好地相互补充，但我认为向您的业务用户展示这两者是很重要的，这样可以让他们更好地了解哪些功能最容易解释。我认为没有人能肯定地说 SHAP 和莱姆哪个更准确，所以两者都要展示出来。无论如何，语法很容易理解！

这就结束了另一个帖子！希望你们都喜欢这个。我要说这是我迄今为止最喜欢学习和写的，我期待着在我自己的工作中应用这一点。在下一篇文章中与大家见面！