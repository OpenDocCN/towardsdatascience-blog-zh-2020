# 保管东西。秘密

> 原文：<https://towardsdatascience.com/keeping-things-secret-d9060c73089b?source=collection_archive---------45----------------------->

## 向黑客大军隐藏 API 密钥的指南。

![](img/46523a14fb438d04bb34a2261272ac93.png)

照片由卢瑟·博特里尔([https://unsplash.com/photos/EsBufnuK4NE](https://unsplash.com/photos/EsBufnuK4NE))拍摄

作为一名数据科学家或软件工程师，在你的职业生涯和个人项目中，你将不可避免地与 API(应用编程接口)进行交互。虽然许多 API 提供“免费增值”服务，允许用户每天免费请求一定数量的请求，但一旦超过这一限制，就会产生巨额账单。[北卡罗来纳州立大学(NCSU)](https://nakedsecurity.sophos.com/2019/03/25/thousands-of-coders-are-leaving-their-crown-jewels-exposed-on-github/) 完成的一项研究扫描了 13%的 GitHub 存储库，发现每天都有数千个新的独特代码被泄露。黑客不仅可以为他们不幸的目标积累荒谬的账单，他们甚至可以对国家安全构成威胁。NCSU 能够在一个西欧国家的主要政府机构的网站上找到 AWS 凭证。正如您在本文中可能想到的，我们需要一种方法来保持这些 API 凭证的秘密，同时仍然能够在线呈现我们的代码。

与编程世界中的大多数事情一样，这可以通过几种方式来实现。我将在本文中概述我最喜欢的方法，主要是因为它简单易用。此外，本教程是为使用 Unix 命令行(macOS 或 Linux)的人编写的，但稍加修改也可以在 Windows 操作系统上复制。

# 制作一个秘密文件夹

我们要保存这些密钥的位置是在我们主目录的一个文件夹中。首先导航到主目录并编写以下代码:

```
mkdir .secret
```

这将在主目录中创建一个名为`.secret`的新目录。“.”意味着这个文件夹是隐藏的，它不会显示在主目录中。如果你想检查它是否确实存在，你可以在主目录中输入`ls -a`,它会在你所有的其他文件中列出。

# 创建 JSON 文件

使用您选择的文本编辑器，我的偏好是 [Sublime Text](https://www.sublimetext.com/) ，您现在需要创建一个 JSON 字典，其中包含您给定项目所需的所有 API 键。该结构的一个示例如下所示:

```
{
      "API_Key" : "<YOUR API KEY GOES HERE>"
  }
```

记得把这个文件保存在你的主目录下，扩展名为`.json`。这就是将文件定义为 JSON 的原因，与代码的格式无关。

# 将 JSON 文件移动到。秘密目录

在您的主目录中，编写以下代码，将 JSON 文件从主目录移动到我们刚刚创建的`.secret`文件中。

```
mv ~/<JSON FILE NAME> ~/.secret/<JSON FLE NAME>
```

# 从文件中提取 API 密钥

现在，在你的 python 脚本中，你需要定义一个使用`json`库来解码 JSON 文件的函数。

剩下的就是使用上面的函数，将文件路径作为参数。要访问该字典中的特定 API 键，只需选择相应的字典键，在本例中为“API_Key”。瞧，API 键现在作为它自己的对象存储在您的脚本中。

> 这是所有的乡亲。我希望这对你有帮助，在你编码的时候保持安全！