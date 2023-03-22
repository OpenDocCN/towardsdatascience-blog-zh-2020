# 使用 Github 页面创建全局 API

> 原文：<https://towardsdatascience.com/using-github-pages-for-creating-global-api-76b296c4b3b5?source=collection_archive---------17----------------------->

## 这篇文章是关于创建一个全球托管的静态 API 并将其用于你的网站的 API。

![](img/053cceee8684aa58e0b48a3869027525.png)

所以，我意外地发现，当我们访问任何一个 **GitHub Pages** 站点时，我们可以返回 **JSON** 响应，而不是标准的 HTML。所以，我想与其创建一个静态网站，为什么不做一些开发的乐趣；)并为我需要的数据创建一个 REST API。

例如，如果您在终端上键入以下内容

```
**curl https://gauravghati.github.io/apis/home.json**
```

给出以下结果:

```
{
    "name": "Gaurav Ghati",
    "phone": "+91 9067365762",
    "mail": "gauravghati225@gmail.com",

    "bio": "MY BIO",
    "fun": "FUN STATEMENT",

    "github": "https://github.com/gauravghati/",
    "twitter": "https://twitter.com/GauravGhati/",
    "linkedin": "http://linkedin.com/in/gauravghati/",

    "aboutme": {
        "para1": "This is Paragraph 1",
        "para2": "This is Paragraph 2"
    },

    "volunteer": {
        "tfi": {
            "heading": "Teach For India",
            "description": "description of TFI"
        },
        "pisb": {
            "heading": "PICT IEEE Student Branch",
            "description": "description of PISB"
        }
    }
}
```

我在我的网站上使用了这个 JSON API，现在每次当网站加载时，它都会请求 Github pages 链接，从那个 URL 获取数据，并在网站上显示获取的数据！

现在，每当我想编辑我网站上的任何数据时，我只需更改 JSON 文件并将它们推送到我的 Github 页面 [repo](http://github.com/gauravghati/gauravghati.github.io/) ，这些更改就会自动显示在主网站上。

可以查看网站的代码，[这里](https://github.com/gauravghati/dotworld)。在姜戈。

# **现在，如何创建自己的 API？**

通过使用 Github，我们可以在那里托管我们的 JSON 文件数据，因为它是一个静态站点，唯一的限制是它只支持 GET 请求。

## 设置 Github 页面

在 Github 上创建一个新的存储库，存储库名称为"**Github _ username . Github . io**"之后，您的新 GitHub 页面库就准备好了。该存储库的 URL 将是

> **https://github _ username . github . io/**

## 准备 JSON 文件

克隆上述存储库，或者如果您已经在本地保存了 JSON 文件，运行该文件夹中的 ***git init*** 并添加上述远程存储库，然后提交并推送您的。json 文件。将文件推送到那里后，您可以在点击 URL 时访问 JSON 响应:

> https://github _ 用户名. github.io/json_file.json

## 通过 JSON API 访问数据

我们点击 URL 后得到的数据是 JSON 对象的形式。我们可以在呈现 HTML 页面时直接将它发送到前端。

当你的后端在 **python** 中，即在 **Django 或 Flask** 中，你可以访问 JSON 文件的数据，如下所示:

```
import urllib.request, jsongithub_link = "https://github_username.github.io/json_file.json"with urllib.request.urlopen(github_link) as url: 
   json_data = json.loads(url.read().decode())print(json_data)
```

这段代码将输出所有 JSON 数据。

如果您想要使用 **JQuery** 获取 **JavaScript** 中的数据，您可以访问 JSON 文件的数据，如下所示:

```
let github_link = ‘https://github_username.github.io/jsonfile.json';$.getJSON(github_link, function(data) {
     //data is the JSON string
     console.log(data);
});
```

这就是你如何使用 Github 页面来创建全局 API。

> 感谢您的阅读，您可以通过 [LinkedIn](http://linkedin.com/in/gauravghati/) 、 [Twitter](https://twitter.com/GauravGhati/) 或我的[作品集](http://gauravg.in)与我联系