# 使用 Flask 的机器学习模型的端到端部署

> 原文：<https://towardsdatascience.com/end-to-end-deployment-of-a-machine-learning-model-using-flask-dc456abcc6da?source=collection_archive---------35----------------------->

## 使用 Flask 设计和构建利用机器学习的端到端 Web 应用程序

![](img/376c6b2223d00d7da978dda7bb6dba8a.png)

利用数据和可视化技能——照片由 [Arian Darvishi](https://unsplash.com/@arianismmm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/machine-learning?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

**机器学习**很漂亮——也很漂亮，如果它工作正常的话。然而，你只能把结果展示给和你一样理解过程的人。当有人知道它是如何工作的时候，魔力就消失了，并且在没有视觉帮助的情况下带领其他人经历它是什么…就像看着油漆变干一样令人兴奋。

因此，一个有效的思路是有一些视觉辅助，一些易于使用的东西，以及一些可以显示输出的东西，而不局限于 Jupyter 笔记本或终端。这里介绍 [**Flask**](https://flask.palletsprojects.com/en/1.1.x/) **:** 一个完全用 Python 编写的 web 应用框架。它拥有最小的设置和快速的开发时间。这意味着学习一门只用于 web 部署的语言没有额外的麻烦——普通的 Python 也可以。

使用 Flask，我们现在可以将我们的机器学习模型带入生活(或者至少将它们带出笔记本和终端)。我们开始吧！

# 示例问题—问题分类

![](img/63ca39235dcfff7c5f3157d92287ce2c.png)

工作流程—由[活动创建者](https://unsplash.com/@campaign_creators?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/workflow?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

机器学习可以用来解决分类问题，并建立一个实用的模型，我们来看看一个简单而有效的挑战——问题分类。有关挑战的详细信息可在[这里](https://www.kaggle.com/ananthu017/question-classification)找到。

这个问题的目标是开发一个简单的机器学习模型，稍后可以使用 Flask 部署该模型。

# **准备数据集**

数据集非常标准，有 5 列:Index、Questions、Cat0、Cat1、Cat2。

Cat1 只是 Cat0 中对应数据点的缩写。第一类只有 6 个类别，而第二类更深入，对每个问题都有更细的分类。作为准备数据集的一部分，我们可以检查唯一值以处理类不平衡，删除重复值并移除空值(如果有)。需要注意的另一个有趣的点是，我们不能直接使用分类标签，所以我们必须将 Cat1 和 Cat2 数据点中的每一个都转换成一个类别号，以便在我们的模型中用作目标变量。

这可以简单地通过以下方式完成:

```
#read the dataset
df = pd.read_csv('dataset.csv')#drop duplicates and values, if any
df.drop_duplicates(inplace=True)
df.dropna(inplace=True)
df.reset_index(inplace=True, drop=True)#convert categorical values into class numbers for our target variables
df['cat_label'] = pd.factorize(df['Category1'])[0]
df['topic_label'] = pd.factorize(df['Category2'])[0]
```

# 决定特征提取方法

如前所述，文本/分类值不能直接用于机器学习模型。要使用它们，我们需要将它们转换成数值或特征。**文本中的特征提取和特征工程**是指从数字格式的数据中提取信息。为了简单起见，我们使用简单直观的方法将文本转换成可用的特征。TF-IDF 是一种将文本数据建模为可用特征的非常直观的方法:

![](img/5cdb2387a9b6bbb5d4b3365191020c67.png)

TF-IDF:语料库中所有单词的术语频率 x 逆文档频率。

# 制作分类模型

我们现在已经选择了一种将文本转换成特征的方法，并且已经形成了目标变量。现在剩下的就是拟合一个模型…..但是，哪一个呢？在决定最终的模型之前，测试不同的模型是很重要的。此外，通过 **sklearn —** 建模最佳分类算法是一项非常简单的任务，我们可以使用简单的 fit()和 predict()查看不同模型的输出。

对于这个项目，我们选择 SVM，线性支持向量机，决策树和随机森林作为我们的分类器。

将我们到目前为止讨论的内容放入一个简单的工作流中，我们得到:

为了评估我们的哪个模型工作得更好，我们需要计算和比较分类器之间的精确度。由于这是一个重复的过程，我们可以通过定义一个函数来评估分类器，使我们更容易。

运行评估脚本后，我们发现 LinearSVC 比其他的要好一些。在这一点上，我们可以将它作为我们的机器学习模型，但我们也可以调整并可能提高我们模型的性能。因此，下一步是使用 GridSearchCV 为我们的模型找到最佳参数。

对于任务 2，我们也可以遵循相同的流程。没有什么变化，除了通过记住我们数据集中的类不平衡来建模。

因为我们已经找到了我们的最佳模型并可能对其进行了调整，所以我们希望保存它以直接运行未来的预测，而不必再次训练分类器。回想一下，我们还使用了一个适合我们数据的矢量器。因此，也必须为我们的测试和预测数据存储相同的矢量器。

请记住，我们在类别 1 中有 6 个班级，在类别 2 中有 40+个班级。记住它们的一个简单方法是在 json/text 文件中保存 category_var:class_num 的映射。这很容易做到，就像这样:

```
#dictionaries to map category to label
cat_label_dict, topic_label_dict = {},{}

for val in df['Category1'].unique():
    cat_label_dict[val] = df[df['Category1']==val]['cat_label'].unique()[0]

for val in df['Category2'].unique():
    topic_label_dict[val] = df[df['Category2']==val]['topic_label'].unique()[0]#print and check the dictionaries
print(cat_label_dict, topic_label_dict)#save the mappings in text files
with open('category_labels.txt', 'w') as f:
    f.write(str(cat_label_dict))

with open('topic_labels.txt', 'w') as f:
    f.write(str(topic_label_dict)) 
```

# 从保存的文件中制作预测函数

我们已经成功地完成了模型的训练、调整和保存！然而，我们的代码有点乱:到处都是，而且结构也不是很好。为了预测结果，我们需要一个函数将输入句子转换为小写，使用保存的矢量器提取特征，并使用保存的模型调用预测函数。为此，让我们从保存的文件中创建一个简单的预测函数。这段模块化代码也将在我们部署应用程序时对我们有所帮助。

# 制作 web 应用程序来部署模型

因为我们的 ML 模型现在已经准备好了，并且可以使用单个函数调用，所以我们准备部署它。对于这一部分，我们需要在本地机器上安装 Flask。

可以通过在终端中运行以下命令来简单地安装 Flask:

```
sudo apt-get install python3-flask
pip install flask
```

对于这部分，首选 VSCode 之类的 IDE 或者 Sublime Text 之类的编辑器。它们有助于自动完成，并对我们的代码结构有很好的了解。

每个基于 Flask 的 web 应用程序都有 3 个不同的东西——一个**‘模板’**文件夹(存储 html 页面)，一个**‘静态’**文件夹(存储 css 设计和一个可选的**‘图像’**文件夹:存储图像)，以及一个运行应用程序的 python 文件(通常按照惯例命名为 app.py)。

由于许多人(像我一样)不太擅长设计 Web 应用程序，一个快速的解决方法是使用已经可用的 HTML 模板。选择一个适合您的用例，或者可以修改以适合您的用例。[这里的](https://www.w3schools.com/w3css/w3css_templates.asp)是免费 html 模板设计的起点。

可以演示一下模板，自己试试。您也可以在在线编辑器中进行更改，并直接在浏览器中查看。如果你不习惯使用传统的编辑器和检查输出，它可以帮助你快速设计网页。

对于这个项目，我为我的 web 应用程序选择了一个主页和一个预测页面。确保所有的 html 文件都在 templates 文件夹中，它们的 css 在 static 文件夹中。

在应用程序的预测页面中可以看到主要的实现—

最后，我们构建我们的 app.py，它运行我们的后端:ML 模型。为此，我们使用前面编写的预测函数，并为每个 html 页面定义路径。通过定义路由，我们的意思是告诉代码当点击一个特定的本地 html 页面引用时要呈现什么页面。

此外，因为我们接受模型的输入(通常通过 HTML 表单)，所以我们需要相应地设置表单动作和方法。一个非常简单的方法是使用 **url_for** ，它将路由直接设置到大括号内的参数。在这里，我们将其设置为 predict()，这是 app.py 中预测函数的名称。

为了访问在 html 页面上使用表单进行的任何输入，我们使用 requests . form['【T2]']。

将所有这些合并到一个 app.py 文件中，我们将它编写如下—

要运行我们的应用程序，我们只需输入

```
python3 app.py
```

结果应该是这样的:

![](img/9eace7ff19dc8df21e48ed96181ba5bd.png)

在端口 5000 上成功运行 Flask 应用程序

这标志着使用 Flask 部署我们的项目的结束。

# 为什么要学习如何为项目制作 WebApps？

作为一名机器学习工程师/数据科学家，或者是任何数据相关领域的爱好者/实践者，展示产品端到端开发的知识非常重要。Flask 与 pure Python 无缝集成，因此很容易开发应用程序。Flask 应用于构建概念证明、演示解决方案以及展示发展和增长。

学习 Flask 这样的简单框架也有助于完善开发人员的技能。一般来说，数据科学更依赖于你如何有效地向一群可能并不特别关注技术或数据的人展示你的想法。

数据科学家本质上仍然是软件工程师。因此，构建、测试、开发和部署应用程序应该是每个软件工程师技能的一部分。

如果你喜欢这篇文章，发表评论并鼓掌表达你的感激之情！您也可以使用我的 [linkedin 个人资料](https://www.linkedin.com/in/aamir-syed/)直接与我联系。

这个项目的全部代码可以在我的 [github 仓库](https://github.com/Viole-Grace/Question_Classification)中找到。