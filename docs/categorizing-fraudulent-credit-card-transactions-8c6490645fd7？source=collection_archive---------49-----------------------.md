# 演练:对欺诈性信用卡交易进行分类

> 原文：<https://towardsdatascience.com/categorizing-fraudulent-credit-card-transactions-8c6490645fd7?source=collection_archive---------49----------------------->

## 尝试各种分类器模型并深入研究 XG Boost

![](img/7e1b05eabdc9d73cad1b3c568957baa6.png)

Pexels.com[的](https://www.pexels.com/photo/making-a-payment-with-a-debit-card-4968635/)卡罗琳娜·格拉博斯卡的[照片](https://www.pexels.com/@karolina-grabowska)

**简介:**

作为 Metis 数据科学训练营的一名学生，我选择探索欺诈卡交易作为我的第二个个人项目。这个项目有三个需求:用 SQL 从 postgreSQL 数据库中查询数据，开发一个分类模型，以及创建一个交互式可视化。我发现 postgreSQL 和交互式可视化组件相当简单，所以这篇文章的大部分讨论了一个可用模型的开发，但是，我包含了其他两个组件的一些见解和技巧。此外，查看 [Github 库](https://github.com/josephpcowell/cowell_proj_3)了解项目背后的所有代码。

**背景:**

在寻找一些有趣的数据进行分析后，我在 Kaggle 上偶然发现了[这个比赛](https://www.kaggle.com/c/ieee-fraud-detection)。电子商务支付解决方案公司 Vesta Corporation 收集了一些信息。包含卡交易信息的 csv 文件，包括交易金额、卡类型、购买浏览器、一些其他各种基本信息，以及该公司设计的但在数据描述中未定义的 300 多项功能。目标包含在名为“isFraud”的列中，用 1 定义欺诈交易，用 0 定义有效交易。与。手里拿着 csv 文件，是时候将它们放入 postgreSQL 数据库并将这些信息转化为见解了。

在深入探讨之前，我想先介绍一下这个项目:

*   SQL 技能(postgreSQL、SQLAlchemy、Psycopg2)
*   分类建模(逻辑回归、XG Boost、随机森林等等)
*   交互式可视化(Streamlit)

**步骤 1:使用 postgreSQL 🗄**

正如我前面提到的，这个项目的一个要求是使用 SQL 来查询数据。将数据放入。csv 文件，我可以将它们转换成数据帧，然后放入 SQL 数据库。

代码如下，我将讨论它的全部含义。首先，我阅读了。csv 文件转换成数据帧，然后创建一个引擎来连接本地 postgreSQL 数据库，我将它命名为“project3”。这个 SQLAlchemy 引擎是整个代码块的关键。它允许 jupyter 笔记本与 postgreSQL 交互，并在给定的数据库中创建表。还有，这些。csv 文件包含多种类型(对象、整型、浮点型等)。)，而且 SQLAlchemy 能够解释 DataFrame 并创建具有相应数据类型的列的方式令人惊叹。此外，它使这个过程比使用命令行快得多(更大。csv 文件超过 300 列)。

```
*# First, turn those csv files into DataFrames.*train_identity = pd.read_csv('../Project-3/train_identity.csv')train_transaction = pd.read_csv('../Project-3/train_transaction.csv')# Create a connection to the project3 postgreSQL database.engine = create_engine(‘postgresql://[USERNAME]:[PASSWORD]@localhost:5432/project3’)# Turn the DataFrames into tables in the postgreSQL databasetable_name = 'train_ident'train_identity.to_sql(table_name,
engine,
*if_exists*='replace',
*index*=False,
*chunksize*=500)table_name = 'train_trans'train_transaction.to_sql(table_name,
engine,
*if_exists*='replace',
*index*=False,
*chunksize*=500)
```

这是该项目的大部分 SQL 组件。我不会在这方面做更多的讨论，但我会发布连接两个表的更复杂的 SQL 查询来进一步研究数据。

```
mastercard_full = pd.read_sql(
"""SELECT * FROM train_trans LEFT JOIN train_ident ON train_trans.”TransactionID” = train_ident.”TransactionID” WHERE train_trans.card4=’mastercard’""", *con*=engine)
```

**第二步:分类建模🎯**

随着数据成功地存储在 postgreSQL 数据库中，然后查询到 DataFrame 中，我从一些基本的 EDA 开始，并很快理解了我的主要挑战:类不平衡。

![](img/e2b6072e1a1a1f84feb5b19ba76f2273.png)

作者图片

阶级不平衡的问题归结到如何决定模型的成功。如果我只关心准确性，我可以预测所有的交易都是有效的，并且有 97%的准确性。对于未经训练的人来说，97%的准确率看起来非常理想，但这意味着该模型预测欺诈交易的比例为 0%。这个 0%的指标被称为召回。回忆回答了这个问题:在所有欺诈交易中，模型正确预测的百分比是多少？另一方面，有效事务的召回将是 100%，因为所有有效事务都被正确预测为有效。因为大家都知道问题在于类别不平衡，而目标是预测欺诈性交易，所以衡量成功的标准就变成了欺诈性交易的召回率和整体准确性。

那么，我是如何解决阶级不平衡的问题的呢？有几种方法可以解决类不平衡问题，但我选择关注过采样。我探索的三种方法是 RandomOverSampler、SMOTE 和 ADASYN。这些方法中的每一种都采用少数类并对其进行过采样，直到它与多数类平衡。RandomOverSampler 随机复制少数类中的数据点。合成少数过采样技术(SMOTE)在少数类中创建新点，但使用线性插值和 K-最近邻，因此它是一种更高级的过采样技术。最后，自适应合成采样(ADASYN)根据密度分布在少数类中创建新点。

关于代码，实现这些过采样技术非常简单，如下所示。值得注意的是，训练数据是过采样的，然后用于拟合模型，从而得到整体更好的模型。

```
# RandomOverSampler
ros = RandomOverSampler(*random_state*=0)
X_tr_sam, y_tr_sam = ros.fit_sample(X_train,y_train)# SMOTE
X_smoted, y_smoted = SMOTE(*random_state*=42).fit_sample(X_train,y_train)# ADASYN
X_adasyn, y_adasyn = ADASYN(*random_state*=42).fit_sample(X_train,y_train)
```

评估二元分类问题的另一个常用指标是 ROC 曲线和曲线下面积(AUC)。下面是三条 ROC 曲线，对应于不同的过度拟合方法和我想要测试的几个分类器模型。很明显，Random Forest 和 XG Boost 是最好的模型，它们也是集合模型，这表明了集合模型在整体上创建更好的模型的卓越质量。

![](img/8fec7fde843c88a209c47dfc1a4285c2.png)![](img/f9a60a0f554d4a09bf2676cef90db13d.png)![](img/48ec49ec5ad260fba1aae43b58b68c88.png)

作者图片

利用这些视觉效果和度量标准，为前进创造了更清晰的愿景。我决定用随机过采样来研究 XG Boost 分类器。此外，值得注意的是，XG Boost 的性能比随机森林分类器稍差，但是，我选择 XG Boost 是因为 Kaggler 大肆宣传 XG Boost，所以我想更深入地研究这种特定类型的建模。

至于特性和特性工程，最好查看[笔记本](https://github.com/josephpcowell/cowell_proj_3/blob/main/Notebooks/fraud_prediction.ipynb)了解更多信息。关于 XG 增强，我非常兴奋地调整超参数以增加 ROC AUC 分数，从而增加模型召回。为了找到最佳的超参数，我查看了[文档](https://xgboost.readthedocs.io/en/latest/tutorials/param_tuning.html)，以找到要调整的具体参数。参数 max_depth、min_child_weight 和 gamma 与模型复杂度相关，而 colsample_bytree 和 subsample 与处理噪声的模型相关。在使用 GridSearchCV 之后，我找到了这些参数的最优值。GridSearchCV 是一个非常密集的过程，所以由于计算机的压力，我分别运行我的参数，同时运行所有的参数可能更好。

```
# Choose the parameters to tunecv_params = {'max_depth': [4,5,6], 'min_child_weight': [1,2,3], 'gamma': [0,1,2]}# Run gridsearchxgbc1 = GridSearchCV(*estimator*=xgb, *param_grid*=cv_params,*scoring*=’roc_auc’)xgbc1.fit(X_tr_sam, y_tr_sam)# Observe the best parametersxgbc1.best_params_
{'gamma': 2, 'max_depth': 6, 'min_child_weight': 1}# Repeat with other parameterscv_params = {'subsample': [0.5,0.75,1], 'colsample_bytree': [0.5,0.75,1]}fix_params = {'gamma': 2, 'max_depth': 6, 'min_child_weight': 1}xgbc = GridSearchCV(*estimator*=XGBClassifier(**fix_params), *param_grid*=cv_params,*scoring*='roc_auc')xgbc.fit(X_tr_sam, y_tr_sam)xgbc.best_params_{'colsample_bytree': 1, 'subsample': 0.75}
```

随着数据过采样(以对抗阶级不平衡)，特征的选择和设计，以及模型参数的调整，是时候看看结果了。下图结合了 ROC 曲线和混淆矩阵。混淆矩阵是另一种轻松查看分类模型结果的方式。首先，AUC 得分为 0.96，比上面 ROC 曲线中显示的任何模型都好。关于混淆矩阵，很容易看出模型哪里工作得好，哪里不理想。至于这个项目的目标，在 1949 个欺诈交易中有 1589 个被正确预测似乎非常好，但是还能做什么呢？

![](img/1ee47351c61ffe2171cbb043cb7e6741.png)

大多数(如果不是全部的话)分类模型输出特定预测的概率。通常有一个概率阈值参数，它可以告诉模型在哪里截断正预测。对于 XG Boost，我无法找出一种方法来调整模型中的概率阈值，但使用…

```
xgb.predict_proba(X_test)
```

…输出一个概率数组，描述有问题的特定点被分类为欺诈的概率。我将重点放在这个预测概率数组上，作为 Streamlit 应用程序的基础。

**第三部分:streamlit 应用**

[Streamlit](https://www.streamlit.io/) 是一个为编程新手创建应用程序的神奇工具。由于我今年才开始大量编写代码，并重点关注 python，因此跳入高级 flask、html、d3 或任何更深入的开发语言和工具似乎有点超出了这个简短分类项目的范围。幸运的是，作为 python 程序员，Steamlit 非常容易使用，生成的应用程序看起来很专业。

该应用程序的代码在[这个 python 文件](https://github.com/josephpcowell/cowell_proj_3/blob/main/streamlit_app.py)中，但我将贯穿我最终应用程序的基本概念。如果一家银行要使用这个模型，我想提供一个助手来确定这个模型的概率阈值。由于阈值非常低，所有欺诈性交易都会被正确预测，但许多有效交易会被错误地预测为欺诈。另一方面，更高的概率阈值将导致更多的欺诈交易被分类为有效交易。该应用程序允许银行查看在不同阈值下会发生什么，并选择适合其需求的特定阈值。

第一个图表显示了召回率和精确度与阈值之间的关系，金色线条代表所选的阈值。第一张图表下方是召回和通过识别欺诈交易节省的金额。在这些数字的更下方是一个混淆矩阵，提供了一个额外的视觉辅助来帮助确定最佳概率阈值。

作者提供的视频

**结论:📗**

在处理分类问题时，类不平衡是一个常见的问题，在处理大型(或小型)数据集时，过采样是解决这个问题的一个很好的方法。另外，python 使得用基本代码比较多个模型，然后用 GridSearchCV 调优超参数变得很容易。这些技术对于数据科学家早期学习非常重要，因为有了这些基础知识，模型改进可以很快完成。现在，我觉得有必要回到我的[以前的项目](/predicting-imdb-movie-ratings-using-supervised-machine-learning-f3b126ab2ddb)，调整超参数并测试更多的模型，以增加该项目的有效性。无论如何，这里有一些关于这个项目的基本要点:

*   学习 SQL！
*   总是花时间探索数据
*   尽早开发一个 MVP，并在该模型的基础上进行构建
*   Streamlit 让创建应用变得简单
*   给利益相关者一个容易理解的解决方案或解释

查看 [Github 仓库](https://github.com/josephpcowell/cowell_proj_3)以获得关于这个项目的更多信息——我真的喜欢让我的笔记本容易跟随。此外，如果您有任何问题或意见，请联系我们。

我真的很喜欢我的数据科学之旅，并希望继续分享我的学习经历。请随意查看[关于我的第二个项目](/predicting-imdb-movie-ratings-using-supervised-machine-learning-f3b126ab2ddb)的博客，并在我继续探索数据科学世界时回来获取更多更新。

伸手:
LinkedIn|[Twitter](https://twitter.com/josephpcowell)