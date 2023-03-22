# 单元测试 MLflow 模型相关的业务逻辑

> 原文：<https://towardsdatascience.com/unit-testing-mlflow-model-dependent-business-logic-9e4e7ca16fca?source=collection_archive---------37----------------------->

![](img/ae916e1c542c7ba2dc30df307c39da31.png)

费伦茨·阿尔马西在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 培训、测试和部署

很多时候，模型的开发者不同于在他们的应用程序中使用模型的开发者。例如，开发人员创建一个获取输入图像并对其进行分类的 API 时，不会知道也不需要知道模型的内外。开发人员需要知道的只是从哪里加载模型，以及如何进行推理。MLflow 提供了非常好的 API 来实现这一点。

# 定制的 MLflow 模型

> mlflow.pyfunc.PythonModel:表示一个通用 Python 模型，它评估输入并生成 API 兼容的输出。通过子类化，用户可以创建具有“python _ function”(“py func”)风格的定制 MLflow 模型，利用定制的推理逻辑和工件依赖性。
> 
> 来源:[https://www . ml flow . org/docs/latest/python _ API/ml flow . py func . html # ml flow . py func . python model](https://www.mlflow.org/docs/latest/python_api/mlflow.pyfunc.html#mlflow.pyfunc.PythonModel)

MLflow 允许用户创建定制的模型，除了他们本身支持的模型，比如 keras。该模型可以保存为 mlflow 模型，供以后使用。因此，不管您的生产模型类型是什么，都可以创建一个使用*ml flow . py func . python model*的示例模型来模拟它。

以下是 MLflow 文档中的 AddN 模型示例。在这里只实现了 *init* 和 *predict* 方法，我们将做同样的事情

```
**import** mlflow.pyfunc

*# Define the model class*
**class** **AddN**(mlflow**.**pyfunc**.**PythonModel):

    **def** __init__(self, n):
        self**.**n **=** n

    **def** **predict**(self, context, model_input):
        **return** model_input**.**apply(**lambda** column: column **+** self**.**n)

*# Construct and save the model*
model_path **=** "add_n_model"
add5_model **=** AddN(n**=**5)
mlflow**.**pyfunc**.**save_model(path**=**model_path, python_model**=**add5_model)

*# Load the model in `python_function` format*
loaded_model **=** mlflow**.**pyfunc**.**load_model(model_path)

*# Evaluate the model*
**import** pandas **as** pd
model_input **=** pd**.**DataFrame([range(10)])
model_output **=** loaded_model**.**predict(model_input)
**assert** model_output**.**equals(pd**.**DataFrame([range(5, 15)]))Source: [https://www.mlflow.org/docs/latest/models.html#example-creating-a-custom-add-n-model](https://www.mlflow.org/docs/latest/models.html#example-creating-a-custom-add-n-model)
```

# 额外使用案例

在进入实现之前，让我们激励一个用例。假设 Acne Inc .决定使用预测模型向其员工提供奖金。基于某些输入，该模型预测该员工的奖金应该是多少。

该模型的功能和输入超出了本文的范围。更有趣的是开发者如何使用这个模型。将该模型集成到 HR 系统中的开发者被提供该模型的位置，并被要求公开一个方法 *amount* ，该方法给出输入，输出奖金金额。开发者被告知模型是 mlflow 模型，可以使用提供的 API 进行预测。

我们的开发人员提出了一个简单的实现如上。现在的挑战是对这个实现进行单元测试，以确保它能够工作。理想情况下，单元测试应该是 CI/CD 兼容的，因此与其为测试训练模型，解决方案应该是轻量级的。输入 *mlflow.pyfunc.PythonModel！*

# 测试奖金模型

开发人员提出了一个可以由字典初始化的测试模型。字典基本上是给定输入的预定义结果。*预测*法就是这么做的。在这种情况下，假设 *model_input* 的每个元素都是支持哈希方法的类型，所以它可以是字典的一个键。

# 单元测试…耶！

现在最精彩的部分来了，开发者用一个测试奖金模型测试它创建的奖金类。

在 *test_model* 中，开发者定义了一个*输出*字典，用于初始化*模型*。然后，模型被保存在临时目录中的特定位置。用保存模型的路径初始化 *Bonus* 类，然后测试 *amount* 方法，它给出与*输出*字典相同的值。瞧啊。

# 结论

测试依赖于 MLflow 模型的业务逻辑非常简单。所有人需要做的就是定义一个测试模型并适当地保存/加载。这种机制在 spark udf 和 MLFlow 中都能很好地工作。