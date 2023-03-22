# GitHub URL 缩写

> 原文：<https://towardsdatascience.com/github-url-shortener-f1e0aeaf83b6?source=collection_archive---------27----------------------->

## 提示和技巧

## 使用 curl 和 Python 的 GitHub.com 网址缩写器

![](img/ae1f6b49c36c19cbe7c7df596e815c81.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4088111) 的 [Susanne Jutzeler，suju-foto](https://pixabay.com/users/suju-165106/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4088111)

GitHub 是最受欢迎的 Git 源代码存储库托管服务。然而，URL 可能会变得很长，因为我们可能会对存储库和文件使用很长的名称。此外，当我们需要在电子邮件和社交网络上与他人分享 URL 时，它看起来会很混乱。

当我们需要与同事分享项目时，短 URL 看起来更好。GitHub 提供了一种服务，可以将这些又长又乱的网址变成更短更干净的网址。答案是 [git.io](https://git.io/) 。

Git.io 是 GitHub 提供的缩短 URL 的服务。

![](img/946a4149dd73f71d56eb2b67400230cb.png)

要缩短网址，只需前往[https://git.io/](https://git.io/)并输入你的 GitHub 网址。URL 可以是 GitHub 存储库，甚至是 GitHub 页面的 URL。

*注意:这个网址缩写器只能用于 GitHub 网址，不能用于其他网址。*

# 使用 curl 缩短 URL

## 创建缩短的 URL

您也可以使用`curl`命令来缩短 URL:

```
curl -i https://git.io -F "url=GITHUB_URL"
```

***举例:***

```
curl -i https://git.io -F "url=https://github.com/jimit105/Sentiment-Analysis-of-Movie-Reviews-using-NLP"

Output:
Location: https://git.io/JfzQJ
```

要获得一个定制的简短 URL，使用参数`code`传递定制文本

```
curl -i https://git.io -F "url=GITHUB_URL" -F "code=CUSTOM_TEXT"
```

***举例:***

```
curl -i https://git.io -F "url=https://github.com/jimit105/Intro-to-Deep-Learning-with-PyTorch" -F "code=pytorch"

Output:
Location: https://git.io/pytorch
```

缩短的网址将出现在回复标题的`Location`字段中。

如果您得到 SSL 错误，那么通过命令传递`--insecure`标志来跳过证书验证。

## 检索完整的 URL

使用以下命令从缩短的 URL 中检索完整的 URL:

```
curl -i SHORTENED-URL
```

***举例:***

```
curl -i https://git.io/JfzQJ

Output:
Location: https://github.com/jimit105/Sentiment-Analysis-of-Movie-Reviews-using-NLP
```

# 使用 Python 缩短 URL

您还可以使用以下 Python 脚本来缩短 URL，并从缩短的 URL 中检索完整的 URL

## 创建缩短的 URL

```
import requests

url = 'https://git.io/'
data = {'url': 'https://github.com/jimit105/Intro-to-Deep-Learning-with-PyTorch', 
        'code': 'pytorch'}

r = requests.post(url, data=data)
print(r.headers.get('Location'))

# Output:
# https://git.io/pytorch
```

如果在运行上面的脚本时得到了`SSLError`，那么在`requests.post`方法中添加参数`verify=False`来跳过证书验证。

## 检索完整的 URL

```
import requests

r = requests.head('https://git.io/pytorch')
print(r.headers.get('Location'))

# Output:
# https://github.com/jimit105/Intro-to-Deep-Learning-with-PyTorch
```

**注意:**您只能为每个 URL 创建一个缩写 URL。如果 GitHub URL 已经被缩短，则不能创建另一个缩短的 URL。如果您再次尝试缩短相同的 URL，它将返回现有的缩短的 URL。

## 结论

在本文中，我们看到了如何使用 git.io 来缩短 GitHub 的 URL。该服务仅适用于 GitHub 网站，即您只能缩短 GitHub 网址，而不能缩短其他网址。因此，当你与他人分享缩短的网址时，他们可以相信链接会指向 GitHub 网站(就像 g.co 是谷歌官方网址缩写，它会带你去谷歌的产品或服务)。

## 参考

*   [https://github.blog/2011-11-10-git-io-github-url-shortener/](https://github.blog/2011-11-10-git-io-github-url-shortener/)

## 资源

本文中使用的所有代码片段都可以在我的 [GitHub 页面](https://jimit105.github.io/medium-articles/GitHub%20URL%20Shortener.html)上找到。

## 让我们连接

领英:【https://www.linkedin.com/in/jimit105/】
GitHub:[https://github.com/jimit105](https://github.com/jimit105)
推特:[https://twitter.com/jimit105](https://twitter.com/jimit105)