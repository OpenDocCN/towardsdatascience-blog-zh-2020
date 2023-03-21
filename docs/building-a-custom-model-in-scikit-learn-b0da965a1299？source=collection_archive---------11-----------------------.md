# 在 Scikit-Learn 中构建定制模型

> 原文：<https://towardsdatascience.com/building-a-custom-model-in-scikit-learn-b0da965a1299?source=collection_archive---------11----------------------->

Scikit-Learn 令人难以置信。它允许其用户适应你能想到的几乎任何机器学习模型，加上许多你可能从未听说过的模型！所有这一切只需要两行代码！

然而，它没有*一切*。例如，有序回归无处可寻。而且它的深度学习能力……欠缺。但是谁在乎呢？你可以在其他地方找到这些东西，对吗？

没错。但是！Scikit-Learn 不仅仅是建模。还有一些非常棒的工具可以帮助你简化建模过程，比如`GridSearchCV`和`Pipeline`。这些工具是非常宝贵的，但它们只适用于 Scikit-Learn 模型。事实证明，如果 Scikit-Learn 和我们的 Google 霸主不直接给我们，**我们可以制作我们自己的定制 Scikit-Learn 兼容模型！**而且比你想象的要简单！

在这篇文章中，我将构建 Scikit-Learn 中明显缺失的东西:使用 *k* 的能力——意味着在`Pipeline`中进行*迁移学习*。也就是说，将聚类的结果输入到监督学习模型中，以便找到最佳值 *k* 。

# 警告:前方 OOP！

这篇文章会有点技术性。具体来说，我将假设您具有面向对象编程(OOP)的工作知识。也就是说，你知道如何以及为什么使用`class` Python 关键字。

![](img/8689385b33265b626919ea044e882929.png)

你必须有这么多`class`才能继续。

# Scikit-Learn 模板

Scikit-Learn 最棒的一点是它令人难以置信的一致性。拟合一种类型的模型名义上与拟合任何其他类型的模型是一样的。也就是说，在 Scikit-Learn 中建模非常简单:

```
model = MyModel(parameters)
model.fit(X, y)
```

就是这样！您现在可以分析您的模型了，可能是在模型的`.predict()`和`.score()`方法的帮助下。事实上，每个 Scikit-Learn 估计器都保证有 5 种方法:

*   `.fit()`
*   `.predict()`
*   `.score()`
*   `.set_params()`
*   `.get_params()`

# 构建您自己的

为了构建我们自己的模型，我们只需要构造一个具有上述 5 种方法的类，并以“通常的方式”实现它们。听起来工作量很大。幸运的是，Scikit-Learn 为我们做了艰苦的工作。为了构建我们自己的模型，我们可以从 Scikit-Learn 内置的基类中继承。

# 旁白:继承

在 OOP 中，如果我们指定一个类**从另一个继承**，那么“子类”将获得“超类”的所有方法

继承的语法如下所示:

```
class Car(Vehicle):
    # stuff...
```

汽车是一种交通工具。`Car`类将包括`Vehicle`类的每一个方法，加上更多我们可以在`Car`类定义中定义的方法。

# Scikit-Learn 给了我们什么？

为了符合 Scikit-Learn，我们的模型需要从一些 mixin 继承。mixin 只是一个从未打算独立工作的类，相反，它只是包含了许多可以通过继承添加到当前类中的方法。Scikit-Learn 为每一种通用类型的模型提供了一个模型:`RegressorMixin`、`ClassifierMixin`、`ClusterMixin`、`TransformerMixin`，以及其他几个我们不需要担心的模型。

我们想要自己创造的一切，都是通过简单地超越我们所继承的来实现的！

这是一个口无遮拦的例子。首先，一个简单的例子。之后，使用聚类的动机示例。

# 示例 1:空模型

零模型，有时称为“基线”模型，是除了随机猜测之外没有任何信息的模型。例如，回归问题的零模型将只是取训练数据的平均值 *y* 并将其用作每个预测。对于分类，它只是对每个预测取多数类。例如，如果你不得不预测某人是否会赢得彩票，零模型将指示你总是输，因为这是最有可能的结果。

![](img/86d62357d7ab99297169a82a84dc2067.png)

事实:这和现实并没有太大的差别。

空模型有助于判断当前模型的表现。毕竟，如果你的模型很好，它应该会超过基线。Scikit-Learn 中没有内置 null 模型，但是我们很容易实现它！

```
import numpy as np
from sklearn.base import RegressorMixinclass NullRegressor(*RegressorMixin*):
    def fit(*self*, *X*=None, *y*=None):
        # The prediction will always just be the mean of y
 *self*.y_bar_ = np.mean(y) def predict(*self*, *X*=None):
        # Give back the mean of y, in the same
        # length as the number of X observations
        return np.ones(X.shape[0]) * *self*.y_bar_
```

很简单！我们现在可以自由地做平常的事情了…

```
model = NullRegressor()
model.fit(X, y)
model.predict(X)
```

重要的是，我们的新`NullRegressor`现在兼容 Scikit-Learn 的所有内置工具，如`cross_val_score`和`GridSearchCV`。

# 示例 2:使用网格搜索“调优”集群器

这个例子是出于好奇，当一位同事问我是否可以使用`GridSearchCV`和`Pipeline`来“调整”一个*k*-意味着模型。我最初说*不*，因为您需要使用 clusterer 作为转换器来传递到您的监督模型中，这是 Scikit-Learn 不允许的。但为什么要让这阻止我们呢？我们刚刚学会了如何黑掉 sci kit——学会做我们想做的任何事情！说白了，本质上我想要的是创建以下管道:

```
Pipeline([
    ("sc", StandardScaler()),
    ("km", KMeansSomehow()),
    ("lr", LogisticRegression()
])
```

其中`KMeansSomehow()`是用作 Scikit-Learn *转换器*的群集器。也就是说，它将 onehot 编码的聚类标签附加到数据矩阵`X`中，然后传递到我们的模型中。为了让它工作，我们将从定义一个继承自`TransformerMixin`的类开始。然后我们会给它适当的`.fit()`、`.transform()`和`.fit_transform()`方法。

但首先，初始化:

```
from sklearn.base import TransformerMixin
from sklearn.cluster import KMeansclass KMeansTransformer(TransformerMixin):
    def __init__(self, *args, **args):
        self.model = KMeans(*args, **args)
```

`self.model`的目的是包含底层集群模型。但是你问什么是`*args`和`**kwargs`？它们是懒惰的程序员的捷径。它们本质上捕获了您传递给`__init__()`的所有其他参数，并将它们传递给`KMeans()`。这实质上是我在说“我传递给`KMeansTransformer`的任何东西也将传递给`KMeans`，我懒得去想那些参数将来会是什么。”

接下来，我们需要给它适当的拟合方法:

```
from self.preprocessing import OneHotEncoderclass KMeansTransformer(TransformerMixin):
    def __init__(self, *args, **args):
        self.model = KMeans(*args, **args) def fit(self, X):
        self.X = X
        self.model.fit(X) def transform(self, X):
        # Need to reshape into a column vector in order to use
        # the onehot encoder.
        cl = self.model.predict(X).reshape(-1, 1)

        self.oh = OneHotEncoder(
            categories="auto", 
            sparse=False,
            drop="first"
        ) cl_matrix = self.oh.fit_transform(cl)      

        return np.hstack([self.X, cl_matrix]) def fit_transform(self, X, y=None):
        self.fit(X)
        return self.transform(X)
```

应该就是这样了！我们现在可以像使用内置的 Scikit-Learn 转换器一样使用这个`KmeansTransformer`。最后，试试这个例子:

```
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.datasets import make_blobsX, y = make_blobs(
    n_samples=100,
    n_features=2,
    centers=3
)pipe = Pipeline([
    ("sc", StandardScaler()),
    ("km", KMeansTransformer()),
    ("lr", LogisticRegression(penalty="none", solver="lbfgs"))
])pipe.fit(X, y)
pipe.score(X, y)
# ==> 1.0
```

在不太人工的例子中，您可能还想使用`GridSearchCV`来找到传递到您的逻辑回归(或您拥有的任何模型)中的最佳聚类数。

# **结论**

现在，您应该明白如何在 Scikit-Learn 的框架内构建自己的定制机器学习模型，Scikit-Learn 是目前最受欢迎的(在许多情况下)强大的 ML 库。

这篇博文学究气吗？当然可以。你会需要这个吗？不，你可能永远不会*需要*这个，但这不是重点。这种技术被用于*建造*某种东西。需要一个*建造者*来认识到需要建造一个更有创造性的解决方案。另外，正如一位智者曾经说过的:

> 你永远不会因为知道得多而少。

我希望您喜欢将这个工具添加到您的工具带中。掌握了它，你可能才刚刚实现真正的*机器学习提升*。

![](img/0c5938d117fd08eba227abce87511f90.png)

图为:我们机器学习者每天实际做的事情。