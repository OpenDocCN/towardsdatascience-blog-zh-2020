# AWS Lambda & Amazon API Gateway——不像听起来那么令人生畏

> 原文：<https://towardsdatascience.com/aws-lambda-amazon-api-gateway-not-as-daunting-as-they-sound-part-1-d77b92f53626?source=collection_archive---------23----------------------->

## 在这篇博客中，我们将创建一个简单的 OpenCV + Tesseract 算法，并使用 lambda 函数和 API 网关将其作为 API 部署在 AWS 上

![](img/977d9ea2eeb3d07adf3ee85b5b703ac5.png)

“牛逼。你创造了一个伟大的机器学习算法。我对它的准确性印象深刻，也喜欢它的技术。如何在我们的系统中实现？”—这是你在客户或业务会议中多次听到但从未能如实回答的事情吗？别担心，这可能是 90% ( *保守点说*)的数据科学家同事面临的问题。AWS 有许多解决方案。其中之一就是——***集成亚马逊 API 网关*** 的 AWS lambda 功能。

# 什么是 AWS Lambda？

AWS Lambda 是一个事件驱动、 ***无服务器*** 计算平台，由亚马逊提供，作为亚马逊网络服务的一部分。它是一种计算服务，运行代码以响应事件，并自动管理该代码所需的计算资源。 ***无服务器*** 计算的概念是指你不需要维护自己的服务器来运行这些功能的想法。这意味着你可以上传你的代码 *(Python，Node.js，Go，。NET、Java、Ruby)* 和 Lambda 将管理其他事情，如运行时、缩放、负载平衡等。

# 什么是亚马逊 API 网关？

Amazon API Gateway 是一个完全托管的服务，使开发人员可以轻松地创建、发布、维护、监控和保护任何规模的 API。API 充当应用程序从后端服务访问数据、业务逻辑或功能的前门。使用 API Gateway，我们可以创建 RESTful APIs 和 WebSocket APIs，实现实时双向通信应用程序。API Gateway 支持容器化和无服务器的工作负载，以及 web 应用程序。

*我们的博客行动计划*

# **索引**

1.  登录 AWS***Lambda***
2.  理解并创建一个***λ_ function***
3.  使用 Docker 下载并定义 ***层*** (又名依赖关系)
4.  测试我们的功能
5.  配置 ***亚马逊 API 网关***
6.  部署 API 并使用 ***Postman*** 进行测试

# 让我们从简单的开始

对于所有没有 AWS 账户的人，请前往
→在“[https://signin.aws.amazon.com/](https://signin.aws.amazon.com/)”
创建新账户→记住添加账单详情，因为没有它，一些 AWS 功能将无法工作。*(别担心，AWS 提供免费等级&因此你不会被收费，除非你超过那个等级，这是一个很大的数目！)*

> 有关限额的更多信息，您可以查看以下内容:aws.amazon.com/free

# 登录 AWS**λ**

→登录 AWS 控制台，在搜索栏中搜索 Lambda

![](img/be99c1f1dfb4c049d034e9fc4f36fee2.png)

Lambda 登陆页面，有*函数、层*等多个对象。
→点击 ***创建功能*→**

![](img/0084ac4952f9a2c8d870e5c35276bc8f.png)

分配函数名称、运行时和执行角色如:
→函数名称:**→ml Function** *→运行时:***Python 3.7*** *→*执行角色: ***新建一个具有基本 Lambda 权限的角色****

*![](img/2ce02e94f48bc14bb487a8218e0cff46.png)*

# *理解并创建一个***λ_ function****

## ***什么是 *lambda_function？****

*运行在 AWS Lambda 上的代码*(这里:python)* 作为 Lambda 函数上传。它可以包括库，甚至是本地库。*

*它有两个参数——事件和上下文。*

*   *`Event`表示导致 lambda 调用的事件或触发器。这就像函数的输入。例如:Lambda 函数可以通过上传到 S3 或通过 API 网关传递一些输入/数据来触发。*(我们将主要关注事件)**
*   *`Context`提供关于 lambda 的调用、函数和执行环境的信息。可用于检查内存分配或检索执行超时前剩余的毫秒数。这就像 AWS 提供的 *extra 或 meta* 信息来理解函数的运行时。*

## *创建一个λ函数*

*让我们创建一个执行以下操作的代码:
→输入:图像文件(。jpg，。png 等)
→ OpenCV:读取图像
→镶嵌:对图像进行 OCR&打印出文本*

```
*import base64
import pytesseract
import json
import io
import cv2def write_to_file(save_path, data):
  with open(save_path, "wb") as f:
    f.write(base64.b64decode(data))def ocr(img):
  ocr_text = pytesseract.image_to_string(img, config = "eng")
  return ocr_textdef lambda_handler(event, context=None):

    write_to_file("/tmp/photo.jpg", event["body"])
    im = cv2.imread("/tmp/photo.jpg")

    ocr_text = ocr(im)

    # Return the result data in json format
    return {
      "statusCode": 200,
      "body": ocr_text
    }*
```

*![](img/864947d2138f7305434e23c3b8cf48c0.png)*

*记得“保存”该功能*

# ***使用 Docker** 下载&定义*层*(又名依赖**)***

*虽然我们已经创建了 lambda 函数，但它不会立即执行或运行。原因是我们没有下载或安装代码运行所需的库*(依赖关系)*。像在正常环境中，我们需要 *pip 或 conda* 安装库，我们需要为 AWS lambda 做同样的事情。我们通过向 AWS lambda 添加*层*来做到这一点。它被定义为:*

> *包含库、自定义运行时或其他依赖项的 ZIP 存档。使用**层**，您可以在函数中使用库，而无需将它们包含在您的部署包中。*

## *使用 Docker 下载依赖项*

*如果我们中的任何人以前使用过*宇宙魔方*，我们就知道安装这个库有多困难。*

*[Benjamin Genz](https://github.com/bweigel) 在简化整个流程方面做了大量工作。*

*→转到您的终端并克隆回购*

```
*git clone [https://github.com/amtam0/lambda-tesseract-api.git](https://github.com/amtam0/lambda-tesseract-api.git)
cd [lambda-tesseract-api/](https://github.com/hazimora33d/lambda-tesseract-api.git)*
```

*![](img/8f729fe3539dbaed56f81902b8d179cd.png)*

*文件夹结构将如下所示*

****重要:
1。将使用 docker 来安装所有的库，所以请确保您已经下载了
2。不要详细检查 Docker 文件，而是进行必要的修改****

*转到命令和运行*

```
*bash build_tesseract4.sh*
```

*这将为函数下载宇宙魔方层*

*→ `build_py37_pkgs.sh`包含需要安装的附加库的列表。移除“枕头”,因为我们不需要它。*

```
*declare -a arr=(“opencv-python” “pytesseract” “Pillow”)*
```

*到*

```
*declare -a arr=(“opencv-python” “pytesseract”)*
```

*→转到命令和运行*

```
*bash build_py37_pkgs.sh*
```

*需要一些时间，你会看到所有相关的库已经下载到一个 zip 文件中。*

*![](img/e941e4584040745ddfc82b395055b4d2.png)*

## *向 AWS Lambda 添加层*

*→返回 AWS lambda 控制台，点击*创建图层**

*![](img/2c1bdb50da5d0222fae0a0bf7c5be820.png)*

*→添加 ***名称，兼容运行时&上传****zip 文件**

*![](img/2e7fd6d6cdefd3034bd108a1fadf612c.png)*

*→ *瞧，*你已经成功添加了*立方体*层*

*![](img/950acc3b4d63a6fd99eabdbf03a1e989.png)*

*→重复同样的过程，通过 zip 文件上传***OpenCV&tesse ract***库。*

## *定义*图层*的目录*

*在我们开始使用上传的依赖项之前，还有最后一件事。*

*→回到你的 ***功能*** ，点击 ***图层****

*![](img/52d602da69f1e47b3b0a9203e0e81a48.png)*

*→添加 ***立方体*** 作为兼容层*

*![](img/ae5ba878149b8e691b55346aa882b16d.png)*

*→重复同样的过程添加***OpenCV******&宇宙魔方*** 作为该功能的另一层。最后，你应该看到两个图层都被添加到 ***图层功能****

*![](img/e91e3a7ad58890a8fbd12da37900537b.png)*

*现在我们已经添加了层，我们只需要确保这些库可以被 MLFunction 读取。*

*→转到 ***环境变量****

*![](img/7dbede76d953c4597b675be815c4528d.png)*

*→添加以下路径，以便上传的 ***图层*** 可以通过 lambda 函数访问*

*`PYTHONPATH = /opt/`*

*![](img/96d5e2a843978c8522aba2c0bf234a43.png)*

*您可能想要添加*超时会话*，因为图像可能需要一些时间来加载并使用 tesseract 进行处理*

*→转到基本设置，超时 1 分钟*

*![](img/280db3c48b1950363fc3e4bd323f79a5.png)*

*让我们测试我们的 lambda 函数*

# *测试我们的功能*

*→转到**测试***

*![](img/ba693cec2ab3a110a1d74e98a50932c6.png)*

*→创建一个新事件，添加以下内容*

*![](img/1a52e97af71d8b9b48b7c85d5385008f.png)*

*这是下图的 base64 编码版本:*

*![](img/09e461b883cd5528567732e4f2fc3829.png)*

****提示:Lambda 函数将所有图像(事件)转换为 base64。因此，在您的代码中进一步使用它们之前，请确保对它们进行解码****

*→点击 ***测试*** ，您应该会看到以下内容:*

*![](img/594afec6610b5945a0345021e4cde142.png)*

*您不仅成功地创建了一个新的 lambda 函数，还测试了它。*拍拍背！**

# *配置 Amazon API 网关*

*→登录 AWS 控制台，在搜索栏中搜索 API*

*![](img/7c385c82340ba01875abe4be6430ae35.png)*

*→点击 ***创建 API****

*![](img/a4fd7979434aaf85207a8852da05f211.png)*

*→选择 ***Rest API*** *(非私有)**

*![](img/102479433f058c760055028b2b34b75c.png)*

*→为您的 API 命名。 ***(此处:OCR API)****

*![](img/0067e594115ee2833f0c10250a07785a.png)*

*现在你在你的 API 控制台中。我们需要执行几个操作来配置我们的应用程序。*

## *创建帖子资源*

*我们需要创建一个 POST 方法，以便用户可以上传图像，并通过我们的 lambda 函数传递它。*

*→转到 ***资源→*** ***动作→创建方法→*** *选择* ***发布****

*![](img/4fd5df039ea9199032aaaeb4d73942c5.png)*

*→选择**使用 Lambda 代理集成**选择 **lambda 函数*(此处:ml function)****&点击保存**

*![](img/7926d493c2b73dd98b3a21ebab402ff9.png)*

*点击 ***确定*** *到* ***添加权限给 Lambda 函数****

*→转到 ***方法响应****

*![](img/f7dc21d833abb1b2a8f805d7c0a9144f.png)*

*→在 HTTP 状态下进行以下更改:*

*   *响应头: ***内容类型****
*   *回复正文:将内容编辑为 ***图片/jpeg****

***注意:**这意味着 API 要以**图像的形式发送 lambda 函数**内容**。这是一个非常重要的步骤，除非我们做出这个改变，否则 API 将无法理解什么类型的文件需要流向 lambda 函数。
—如果是 HTML 或 XML 等其他*类型*，我们应该将响应体改为 *application/html* 或 *application/xml****

*再做一次改变，然后我们就可以开始了。*

***添加二进制选项***

*我们希望用户**上传**文件，为此我们需要启用二进制选项。*

*→转到 ***API*** 部分下的 ***设置****

*![](img/6c7c345a9ce33996d27e8ce68cd4f88b.png)*

*在*二进制媒体类型&中添加`*/*`点击保存更改。(这意味着用户可以上传任何***类型的文件——虽然它会根据* ***内容发送到 lambda 函数——我们前面定义的****)***

**![](img/2c0f3bd65e07c95fcae96a4540143a9d.png)**

**您已经准备好部署和测试您的 API 了。**

# **部署 API 并使用 Postman 或 curl 测试它**

**→转到 ***资源→操作→部署 API*****

**![](img/e47e5ca0ddb877738e66cab5210ac6fe.png)**

**→命名为 ***部署阶段(这里:测试)→部署*****

**![](img/d9c847b1b4322acf983b66fe47596669.png)**

**哇——您已经部署了您的 API。现在您应该会看到您的 API 的 URL。**

**![](img/bc39112e8a383834b94839755d43596a.png)**

## **测试 API**

**我们将使用 ***Postman*** 来测试我们的 API。你可以从[https://www.postman.com/downloads/](https://www.postman.com/downloads/)下载**

**→打开 Postman，您应该能够看到一个仪表板**

**![](img/dc9dd4d63b100a9fb90519abafeff5f8.png)**

**→新建一个标签页→选择 ***POST*** 选项→在*输入请求 URL* 中添加 ***API URL*****

**![](img/2a098bc22761212790e5f3fd346dd3df.png)**

**→选择 ***二进制**正文&*** 上传 ***Hello World*** 镜像文件**

**![](img/ed56aaf5f853fabfa319dabba8bbf6c7.png)****![](img/09e461b883cd5528567732e4f2fc3829.png)**

****输出—** 您应该得到以下输出**

**![](img/6a13ea1cff2f1ae0cd06600e87abfea7.png)**

*****瞧*** —您已经成功地创建、部署并测试了您 API。与你的朋友和家人分享你的网址。**

**另外，如果你有兴趣在 AWS EC2 实例或 **Heroku** 上托管 **OpenCV &宇宙魔方，那么请阅读以下博客。****

**[](/deploy-python-tesseract-opencv-fast-api-using-aws-ec2-instance-ed3c0e3f2888) [## 使用 AWS EC2 实例部署 Python + Tesseract + OpenCV 快速 API

### 关于如何创建 AWS EC2 实例并为…部署代码的分步方法(包括截图和代码)

towardsdatascience.com](/deploy-python-tesseract-opencv-fast-api-using-aws-ec2-instance-ed3c0e3f2888) [](/deploy-python-tesseract-ocr-on-heroku-bbcc39391a8d) [## 在 Heroku 上部署 Python Tesseract OCR

### 如何在 Heroku 上创建 OpenCV + Tesseract OCR 的一步一步的方法(包括截图和代码)

towardsdatascience.com](/deploy-python-tesseract-ocr-on-heroku-bbcc39391a8d) 

暂时结束了。有什么想法来改善这一点或希望我尝试任何新的想法？请在评论中给出你的建议。再见。**