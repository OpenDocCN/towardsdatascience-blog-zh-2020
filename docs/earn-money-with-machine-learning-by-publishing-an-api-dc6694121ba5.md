# 通过部署 API 利用机器学习赚钱

> 原文：<https://towardsdatascience.com/earn-money-with-machine-learning-by-publishing-an-api-dc6694121ba5?source=collection_archive---------29----------------------->

## 使用 FastAPI、uvicon/guni corn 和 Python 将您的作品发布到 RapidAPI 市场💸

![](img/7c452fbbd13196a952da72101698a18c.png)

来源:@[denarium _ bit coin](https://unsplash.com/@denarium_bitcoin)via[unsplash](https://unsplash.com/photos/DCCt1CQT8Os)

如果你正在读这篇文章，那是因为你已经创造了一些有价值的代码，你希望能够发表。无论您只是简单地将一些开源技术打包在一起，还是构建了一个出色的新算法，其他技术专家都很有可能会发现您的解决方案很有用。

在本文中，您将利用开源框架在**的 3 个简单步骤**中将您引以为豪的东西部署到 API 市场。你甚至可能获得一点被动收入。要获得好评和/或利润，需要一点努力和一点运气——让我们全力以赴吧🏀！

## 我可以在哪里分发我的作品？

作为一名有才华的数据专家，您对 API 非常熟悉。如果你需要快速复习，这里有一些我最喜欢的:

[](/top-20-apis-you-should-know-in-ai-and-machine-learning-8e08515198b3) [## 人工智能和机器学习中你应该知道的 20 大 API

### 应用程序编程接口是一个现成的代码，可以简化程序员的生活。它有助于数字化…

towardsdatascience.com](/top-20-apis-you-should-know-in-ai-and-machine-learning-8e08515198b3) 

API marketplace 允许开发者发布和货币化他们的工作，而不需要管理定制的支付系统和复杂的基础设施。RapidAPI 是我最喜欢的*应用程序，为你能想到的每一种应用程序提供“免费增值”和付费订阅计划。假设您正在构建一个自定义应用程序，该应用程序将财务数据与新冠肺炎感染统计数据相关联，您将使用 RapidAPI 来发现、学习和扩展。*

## 我为什么要发布 API？

开源软件运动推动了世界上一些最受欢迎、最赚钱的科技公司。开源脚本语言(Python、R、Julia)、数据库技术(SQL、MongoDB)和计算库(SciPi、Scikit-learn、tidyr)的存在使越来越多的数据专业人员能够构建各种规模的有价值的解决方案。为什么不贡献一个惊人的开发者资源生态系统呢？

如果你是一名新兴的数据科学家，这是推销你的才华和创业技能的最佳方式。我们开始吧！

## 我如何部署一个 API？

有多种方法可以让 API 启动并运行。以下是我最喜欢的几个 python 部署:

1.  [使用 Zappa 在 AWS Lambda 上部署无服务器 Web 应用](https://pythonforundergradengineers.com/deploy-serverless-web-app-aws-lambda-zappa.html)
2.  [用 GitHub 页面、Flask 和 Heroku 构建一个简单的 Web 应用](/build-a-simple-web-app-with-github-pages-flask-and-heroku-bcb2dacc8331)
3.  **使用 FastAPI 和 Uvicorn(我们将使用这种方法😉)**

> FastAPI 是一个闪电般快速、直观和健壮的 API 框架，它简化了开发过程。[uvicon](https://www.uvicorn.org/)是一个流行的 [WSGI 服务器](https://www.fullstackpython.com/wsgi-servers.html)，它使得在云中运行 python 应用程序变得很容易。

# 简单的部署步骤

我最近开发了一个简单的程序，从一篇新闻文章中抓取文本，为[命名实体识别](/named-entity-recognition-applications-and-use-cases-acdbf57d595e)和[词性标注](https://en.wikipedia.org/wiki/Part-of-speech_tagging)处理文本，然后执行一些高级分析。

## I .将此程序部署到我的本地计算机:

1.  安装依赖项:

```
pip install fastapi uvicorn gunicorn
```

2.格式化端点。

> 在格式化您的代码之前，看一下 path params [文档](https://fastapi.tiangolo.com/tutorial/path-params/):

3.运行它！

```
uvicorn main:app --reload
```

4.在 Swagger UI 中查看(生成的)文档:

```
[http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)
```

5.带着 API 兜一圈！

## 二。作为微服务部署到 Heroku:

1.  将您的代码保存到一个 [GitHub 库](https://github.com/GarrettEichhorn/fastapi-analyze-text)。
2.  打开 Heroku，创建一个新的应用程序，连接你的 GitHub 帐户，链接你想要发布的特定存储库，并启用自动部署。
3.  通过运行以下命令构建 requirements.txt 文件:

```
pip freeze > requirements.txt
```

4.制作一个 [Procfile](https://devcenter.heroku.com/articles/procfile) 以便 Heroku 知道如何部署应用程序:

```
web: gunicorn -w 4 -k uvicorn.workers.UvicornWorker main:app
```

5.推送至 GitHub，等待 Heroku 部署您的解决方案！您可以查看我完成的例子如下适当的格式👇

*   [GitHub 回购](https://github.com/GarrettEichhorn/fastapi-analyze-text)
*   [Heroku App](https://fastapi-analyze-text.herokuapp.com/)
*   [API 文档](https://fastapi-analyze-text.herokuapp.com/docs)

> 如果您在将本地部署添加到 Heroku 时遇到问题，请查看此视频！

来源:[TutLinks](https://www.tutlinks.com/create-and-deploy-fastapi-app-to-heroku/)via[YouTube](https://youtu.be/H7zAJf20Moc)

## 三。通过 RapidAPI Marketplace 分发，以便开发人员可以使用它:

1.  注册一个 RapidAPI 帐户并[“添加一个新的 API”](https://docs.rapidapi.com/docs/getting-started)。
2.  添加一个基本 URL(链接到 Heroku app)并格式化相关的[端点](https://docs.rapidapi.com/docs/endpoints)！
3.  给 API 打上品牌！添加一个高分辨率的图像，雄辩的描述和诱人的功能。
4.  格式化公共计划的定价属性。RapidAPI 提供多种[订阅计划](https://docs.rapidapi.com/docs/api-pricing)，可随消费而扩展。根据你的代码的*，从一个**完全免费的计划**或**免费层**(‘免费增值’计划)开始让其他开发者上钩可能是有意义的。如果你更有信心，直接去“付费”并要求一个订阅的信用卡。*
5.  最后，将 API 发布到市场上！开发人员将能够使用你出色的新应用程序，你将获得有价值的反馈。未来的更新/附加特性将更好地服务于那些使用你的 API 的开发者。

## 让其他开发者订阅付费！

很多 API 是由公司开发的，而不是个人开发的。随着有才能的数据专业人员的数量随着需求的增加而增加，对有价值的算法和数据管道的廉价、复杂的替代品的需求也会增加。你可能不会马上获得被动收入*，但是在寻找与数据相关的职位时，你已经很好地展示了独特的才能和企业家精神，提高了你的市场竞争力。我鼓励您尝试 FastAPI 和其他替代方案，看看哪种类型的解决方案最受欢迎。*

*如果您想讨论 API 格式和最佳实践、开源理念或您可以开发的各种按需软件选项，请联系我。我很乐意收到你的来信。*

*感谢阅读！😃*