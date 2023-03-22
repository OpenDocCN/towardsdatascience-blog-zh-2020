# 线性回归中的一个实用建议

> 原文：<https://towardsdatascience.com/a-practical-suggestion-in-linear-regression-cb639fd5ccdb?source=collection_archive---------26----------------------->

## 机器学习

## 从弹性网开始，记得调好定义 l1 范数之比的超参数。

![](img/43ea264f8c1953e7b6cc1bb21c916c9c.png)

贾斯汀·科布利克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

不无论你是一名经验丰富的数据科学家还是机器学习的初学者，线性回归仍然是你需要掌握的最基本的模型之一。

简单而有用的线性回归长期以来一直受到多个领域研究人员的青睐，比如生物学和金融学。原因是线性回归的优点和它的局限性一样明显。

例如，即使它依赖于 Xs 和 Y 之间的线性关系的假设，它仍然比复杂的网络更容易实现，比如深度神经网络。而且模型本身更容易解读。

然而，在一个真实的项目中，实现线性回归就像输入下面几行代码一样简单吗？

```
**from** **sklearn.linear_model** **import** LinearRegression
my_model = LinearRegression()
my_model.fit(X,y)
```

不，不是真的。

上面的代码很容易遇到 ***过拟合问题*** 。过度拟合意味着与测试数据相比，您的模型在训练数据中表现得更好。

导致过度拟合问题的原因通常是模型的高度复杂性。在上面的代码中，将 X 中的所有特征汇集到线性回归模型中，将很难对未来的数据点做出预测。

通常可以通过两种方式降低线性模型的复杂度， ***特征工程、*** 和 ***正则化*** 。

特征工程意味着从原始 X 表中手动选择一个特征子集，然而，该过程还需要数据集的先验知识来指导特征过滤。我们不会谈论太多。

正如我所说，降低线性模型复杂性的第二种方法是通过正则化。*脊*回归、*套索*回归，以及*弹性网*是三种实现方式。

我不打算在这里涉及任何数学方程，所以对于那些感兴趣的人，请参考[这个不错的帖子](https://www.datacamp.com/community/tutorials/tutorial-ridge-lasso-elastic-net)。

## 里脊回归

Ridge 在线性回归的原始损失函数中添加了一个“l2 范数”惩罚项，该惩罚项倾向于将所有变量的系数收缩到某一水平(基于正则化的强度)。代码如下。

```
**from** **sklearn.linear_model** **import** Ridgemy_alpha = 0.1
my_model = Ridge(alpha = my_alpha)
my_model.fit(X, y)
```

这里“my_alpha”是提到的强度。my_alpha 越高，正则化越强，模型的方差越低。

## 套索回归

套索回归的思想类似于岭回归的思想，但是在罚函数中用‘L1 范数’代替‘L2 范数’。

套索和脊之间的这种差异会导致套索可以将系数缩小到 0，而脊则不能。这就是为什么人们也说 LASSO 有做特征选择的能力。代码如下。

```
**from** **sklearn.linear_model** **import** Lassomy_alpha = 0.1
my_model = Lasso(alpha = my_alpha)
my_model.fit(X, y)
```

如果 my_alpha 足够大，它会将所有系数收缩为零，这就产生了最有偏差的线性模型。

## 弹性网

创建弹性网来混合套索和脊，其中惩罚项是 l1 和 l2 范数正则化项的组合。它引入了另一个参数 l1_ratio，以将两个不同的权重分配给 l1 和 l2 范数，其总和为 1。代码如下。

```
**from** **sklearn.linear_model** **import** ElasticNetmy_alpha = 0.1
my_l1ratio = 0.5
my_model = ElasticNet(alpha = my_alpha, l1_ratio = my_l1ratio)
my_model.fit(X, y)
```

在实际操作中，最好的 l1_ratio 总是通过交叉验证来调整。

## 选哪个？脊，套索，还是弹力网？

![](img/9121f7acbf0ed471e15e179646ada590.png)

莎伦·麦卡琴在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我的实际建议是**总是从弹性网**开始。

让我先解释一下*弹力网*相对于其他方法的缺点。前面提到过，*弹性网*归纳出一个新的参数 ***l1_ratio*** ，无法高效手动赋值。

交叉验证是调整最佳 ***l1_ratio*** 最常用的方法。因此，与*脊*和*套索*相比，弹性网的计算成本更高。

然而，我想忽略这种计算成本的增加，因为我正在工作的大多数平台都可以轻松处理更复杂的模型，如*随机森林*和*深度神经网络*。

当我对数据集有一些预先了解时，我只使用*脊*或*套索*。举个例子，如果我知道 X 中的所有特征对预测 Y 都是有用的，那么 Ridge 是首选，因为我不想丢失任何一个变量。如果我知道只有一小部分特征是有用的，我想把它们选出来，那么 LASSO 当然是更好的选择。

然而，根据我的经验，在建模步骤之前，我对数据一无所知。我更关心的是选择一个基于错误假设的模型，而不是花更长的时间来调整我的模型。这就是为什么*弹性网*总是我在分析中使用的第一个线性模型。

## 弹性网实现的真实例子

![](img/5385284ef24c85542bce664bce5335f9.png)

奥雷连·罗曼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

这是我的示例代码，用于分析时间序列数据和驱动变化的因素之间的关系。

```
from sklearn.impute import SimpleImputer
from sklearn.linear_model import ElasticNet
from sklearn.model_selection import TimeSeriesSplit
from sklearn.pipeline import Pipelinedef train_EN(X,y):
    # X are features
    # y is output
    # set cross-validation
    tscv = TimeSeriesSplit(n_splits=10)
    data_split = tscv.split(X)

    # build a pipeline of pre-processing and elasticNet regression model
    my_pipe = Pipeline([
        ('imputer', SimpleImputer(strategy="median")), 
        ('std_scaler', StandardScaler()),
        ('en_model', ElasticNet(random_state=42))
    ]) # parameter space
    param_grid_en = {
        'en_model__alpha' : [1e-2,1e-1,1,10,100,1000,10000,],
        'en_model__l1_ratio' : [0,0.25,0.5,0.75,1]
    } # gridsearch for best hyper-parameters
    gs_en = GridSearchCV(my_pipe, param_grid=param_grid_en, cv=data_split, scoring='neg_mean_squared_error', n_jobs=-1)
    # fit dataset
    gs_en.fit(X, y)
```

预处理(pre_pipeline)分两步完成，缺失值输入和数据规范化。为了超参数的调整( ***GridSearchCV*** )，整个训练数据集被分成 10 倍(***TimeSeriesSplit(n _ splits = 10)***)。所选择的最佳模型将是来自超参数空间(***param _ grid _ en***)的一个 alpha 和一个 l1_ratio 以及特征系数的组合。

## 外卖

1.  当你想对数据进行线性回归时，别忘了应用正则化。
2.  **始终从弹性网开始。**
3.  不要忘记调整你的训练数据集中的超参数。

我希望这篇文章对你也有帮助。

![](img/39fbcb4e1a98987ebf2b16349135dd8d.png)

格伦·杰克逊在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 参考资料:

[https://sci kit-learn . org/stable/modules/classes . html # module-sk learn . linear _ model](https://scikit-learn.org/stable/modules/classes.html#module-sklearn.linear_model)