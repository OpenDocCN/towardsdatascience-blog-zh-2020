# 在 Google Colab 上运行并分享深度学习网络应用

> 原文：<https://towardsdatascience.com/how-to-run-and-share-your-deep-learning-web-app-on-colab-a13f9d2cbc4e?source=collection_archive---------34----------------------->

## 使用 Streamlit 快速构建原型，并在 Google Colab 上部署，只需几行代码

![](img/bbe155165c024e6082dd4e5bc1ff4d0d.png)

[KOBU 机构](https://unsplash.com/@kobuagency?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 介绍

在我之前的工作中，我使用 Fastai 建立了一个深度学习模型，对黑白照片进行彩色化，并使用 Streamlit 建立了一个 web 应用原型。我希望与我的同事和朋友分享它，以获得一些反馈，所以我试图将 web 应用程序部署到 AWS 和 Heroku。

[](/colorize-black-and-white-photos-by-ai-cc607e164160) [## 人工智能给黑白照片上色。

### 利用 Fastai 的生成性对抗网络(GAN)使黑白照片变得丰富多彩

towardsdatascience.com](/colorize-black-and-white-photos-by-ai-cc607e164160) 

我能够将我的应用程序部署到这些云服务提供商并运行它，但是，他们的*免费层帐户*没有为我的应用程序提供足够的内存，它总是崩溃。为了让至少一些东西在网上运行，我不得不把照片的尺寸做得很小，结果质量很差。我不想在这个早期阶段花很多钱升级账户。我只是想要一个功能齐全的快速原型运行，这样我就可以测试它，并得到一些反馈。

从那以后，我一直在寻找其他解决方案来部署和共享应用程序。我找到的一个解决方案是在 Google Colab 上运行这个应用程序，使用其免费且强大的计算服务。在这篇短文中，我将描述我如何在 Colab 上运行 web 应用程序。

# 斯特雷姆利特

如果您不熟悉 [Stremlit](https://www.streamlit.io/) ，它是用 Python 创建简单 web 应用程序的强大工具。对于我这样没有 web 开发背景和技能的人来说非常方便。

他们为初学者提供了很好的教程。

[](https://github.com/streamlit/demo-self-driving) [## 简化/演示-自动驾驶

### 该项目展示了 Udacity 自动驾驶汽车数据集和 YOLO 物体检测到一个互动的流线…

github.com](https://github.com/streamlit/demo-self-driving) 

在这篇文章中，我还记录了我使用 Stremlit 构建 web 应用程序的工作:

[](/generative-adversarial-network-build-a-web-application-which-colorizes-b-w-photos-with-streamlit-5118bf0857af) [## 生成性对抗网络:构建一个用 Streamlit 给 B&W 照片着色的 web 应用程序

### 使用 Streamlit 快速将生成式对抗网络模型转换为 web 应用程序，并部署到 Heroku

towardsdatascience.com](/generative-adversarial-network-build-a-web-application-which-colorizes-b-w-photos-with-streamlit-5118bf0857af) 

# 在 Colab 上运行应用程序

一旦你在本地成功运行了网络应用程序，你可以试着在 Google Colab 上运行它

首先把 app 克隆到 colab，进入目录。

```
!git clone [http://github.com/xxx/xxxxx.git](http://github.com/xxx/xxxxx.git) color
cd color
```

然后安装需求

```
!pip install -r requirements_colab.txt
```

在我的例子中，它包含 stremlit、opencv、fastai 和 scikit_image。

然后我们需要安装 pyngrok。

```
!pip install pyngrok
```

要通过 Colab 上的公共 URL 运行和共享应用程序，我们需要使用 ngrok，这是一种安全的隧道解决方案。详情[可以在这里](https://ngrok.com/)找到。

安装完成后，我们可以在后台运行应用程序。

```
!streamlit run run.py &>/dev/null&
```

然后使用 ngrok 创建公共 URL。

```
from pyngrok import ngrok
# Setup a tunnel to the streamlit port 8501
public_url = ngrok.connect(port='8501')
public_url
```

你应该会得到这样一个网址“[http://ea41c 43860 D1 . ngrok . io](http://ea41c43860d1.ngrok.io)”。

就是这样！非常简单的步骤。现在，这款网络应用可以通过强大的 Google Colab 免费在线使用。

![](img/86740ef7444ebe86a8a41993e9c1ec96.png)

照片由[阿齐兹·阿查基](https://unsplash.com/@acharki95?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在 Colab 上运行 web 应用程序的优势在于，应用程序不会受到自由层帐户的有限计算的限制，您甚至可以使用 GPU 运行。然而，它的主要缺点是 URL 只是临时的，一旦 Colab 断开，URL 将被禁用。所以你只能在短时间内分享链接(< 12 小时)。但是我认为这仍然是一个快速部署、测试和共享 web 应用原型的好方法。

感谢阅读。欢迎提出建议和意见。