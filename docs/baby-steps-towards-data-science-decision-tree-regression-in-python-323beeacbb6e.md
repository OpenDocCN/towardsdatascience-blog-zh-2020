# 迈向数据科学的一小步:Python 中的决策树回归

> 原文：<https://towardsdatascience.com/baby-steps-towards-data-science-decision-tree-regression-in-python-323beeacbb6e?source=collection_archive---------40----------------------->

## 理解决策树回归背后的直觉，并用 python 实现。提供源代码和数据集。

![](img/11f117f32cee43c454b5a6078893dac0.png)

埃里克·马塞利诺在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 什么是决策树回归？

决策树主要用于分类问题，但是，让我们试着理解它在回归中的含义，并且试着理解为什么在回归中使用它不是一个好主意。

决策树回归可以将数据分成多个部分。这些拆分通常回答一个简单的 if-else 条件。该算法决定数据中的最佳分割数。由于这种拆分数据的方法非常类似于树的分支，因此这种方法可能被称为决策树。事实上，最后一层(即适合、不适合)被称为叶子。

例如，看上面的图片，有关于顾客的数据，他们的年龄，他们是否吃比萨饼和他们是否锻炼。通过执行决策树回归，数据按照年龄被分成两类，即年龄< 30 and age> 30。年龄内< 30 category, the data is again split into 2 categories by their eating habits i.e., people eating pizza and people not eating pizza. The same goes for exercise as well. By doing these splits, we can simply account for the behavior of the customers based on their choices and we end up deciding whether they are fit or unfit.

# Implementation in Python

Let us deep dive into python and build a polynomial regression model and try to predict the salary of an employee of 6.5 level(hypothetical).

Before you move forward, please download the CSV data file from my GitHub Gist.

```
[https://gist.github.com/tharunpeddisetty/433e5fe5af0e6b6cdd9d7df3339931a5](https://gist.github.com/tharunpeddisetty/433e5fe5af0e6b6cdd9d7df3339931a5)
Once you open the link, you can find "Download Zip" button on the top right corner of the window. Go ahead and download the files.
You can download 1) python file 2)data file (.csv)
Rename the folder accordingly and store it in desired location and you are all set.If you are a beginner I highly recommend you to open your python IDE and follow the steps below because here, I write detailed comments(statements after #.., these do not compile when our run the code) on the working of code. You can use the actual python as your backup file or for your future reference.
```

***导入库***

```
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
```

***导入数据并定义 X 和 Y 变量***

```
dataset = pd.read_csv(‘/Users/tharunpeddisetty/Desktop/Position_Salaries.csv’) #add your file path

X = dataset.iloc[:,1:-1].values
y = dataset.iloc[:, -1].values#iloc takes the values from the specified index locations and stores them in the assigned variable as an array
```

**让我们看看我们的数据，了解变量:**

![](img/2e7b6c01f37c646b6c4973e7acef3f3f.png)

该数据描述了员工的职位/级别及其工资。这与我在多项式回归文章中使用的数据集相同。

***可视化数据***

```
plt.scatter(X, y, color = ‘red’)
plt.plot(X,regressor.predict(X), color = ‘blue’)
plt.title(‘Decision Tree’)
plt.xlabel(‘Position level’)
plt.ylabel(‘Salary’)
plt.show()
```

![](img/3a57fbb8349b90e433e17e541e8db67f.png)

作者图片

***训练模特***

```
from sklearn.tree import DecisionTreeRegressor
regressor = DecisionTreeRegressor(random_state=0)
regressor.fit(X,y)#random_state is the seed value, just to make sure we both get same results.
```

我们将使用模型中的全部数据，因此您不需要将数据分成训练和测试数据。

***可视化决策树回归的结果***

```
X_grid = np.arange(min(X), max(X), 0.1)
X_grid = X_grid.reshape((len(X_grid), 1))
plt.scatter(X, y, color = 'red')
plt.plot(X_grid,regressor.predict(X_grid), color = 'blue')
plt.title('Decision Tree')
plt.xlabel('Position level')
plt.ylabel('Salary')
plt.show()
```

![](img/ecc53fd4cbae7ccdabba83fad04224f1.png)

作者图片

您可以看到我们的模型如何预测与数据点相同的精确数据水平。这种可视化仅用于说明目的。实际上，我们几乎没有任何只有 X 和 Y 变量的数据集(X=职位级别，Y =薪水)。因为这个数据是二维的，我们能够绘制它。如果数据是多维的，我们就不会绘制它。这显然是有道理的。

***使用决策树回归预测 6.5 级结果***

```
print(regressor.predict([[6.5]]))
# predict method expects a 2D array thats the reason you see [[6.5]] 
```

***结果***

决策树回归:150000

# 结论

我们可以看到，我们的模型预测了与 6 级员工相同的工资。这是由决策树算法的本质决定的。它的工作假设是，如果一个雇员有某些特征(在这种情况下是水平)，那么另一个具有相似特征的雇员也获得相同的工资。

我把这个留给你的智慧去理解这个模型有多好。开个玩笑！你可以清楚地看到这是不公平的，对吗？同样，我们可以得出结论，这种模式表现不佳。正如我在本文开始时已经提到的，这个模型更适合于分类问题，我将在以后的文章中处理这个问题。我将确保在我的下一篇文章中使用相同的数据集来实现不同的回归技术。通过这样做，您可以比较结果，并决定哪个模型是最好的。

我希望这对你来说是有趣的。恭喜你！您已经用最少的代码行实现了决策树回归。当我说婴儿迈向数据科学时，我是认真的。现在您有了代码的模板，您可以在其他数据集上实现它并观察结果。机器学习快乐！