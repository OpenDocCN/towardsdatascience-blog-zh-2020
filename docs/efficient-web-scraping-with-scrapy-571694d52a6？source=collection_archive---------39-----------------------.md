# 使用 Scrapy 进行有效的网页抓取

> 原文：<https://towardsdatascience.com/efficient-web-scraping-with-scrapy-571694d52a6?source=collection_archive---------39----------------------->

![](img/1426df8f8449a46c47df178404c14708.png)

[https://unsplash.com/@markusspiske](https://unsplash.com/@markusspiske)

## Scrapy 的新功能使您的刮削效率

Scrapy 作为一个网页抓取框架是强大的和可扩展的。它有一个活跃的用户群，每次更新都会有新的功能出现。在本文中，我们将介绍其中的一些特性，以充分利用您的抓取项目。

# 在本文中，您将了解到

1.  更有效地跟踪链接
2.  html 属性的更清晰提取
3.  Scrapy 中函数间更清晰的变量传递
4.  使用 attribute 属性在没有 xpath 或 css 选择器的情况下获取 html 属性

# 1.以下链接

为了让你的蜘蛛跟踪链接，这是通常的做法

```
links = response.css("a.entry-link::attr(href)").extract()
for link in links:
    yield scrapy.Request(url=response.urljoin(link),  callback=self.parse_blog_post)
```

现在使用 requests 方法就可以了，但是我们可以使用另一个名为 response.follow()的方法来清理这个问题。

```
links = response.css("a.entry-link")
for link in links:
    yield response.follow(link, callback=self.parse_blog_post)
```

看看我们为什么不必提取链接或使用 urljoin，这是因为 response.follow 接受标签。Response.follow()自动使用 href 属性。

```
for link in response.css("a.entry-link"):
  yield response.follow(link, callback=self.parse_blog_post)
```

事实上，scrapy 可以使用 follow_all()方法处理多个请求。这样做的好处是 follow_all 将直接接受 css 和 xpath。

```
yield from response.follow_all(css='a.entry-link', allback=self.parse_blog_post) 
```

![](img/63cd8df6c1b9047b9ebc9d590226eabd.png)

[https://unsplash.com/@nasa](https://unsplash.com/@nasa)

# 2.提取数据

从标签中提取数据的常用方法是`extract()`和`extract_first()`。我们可以用一个方法`get()`和`get_all()`，看起来干净一点。

从

```
def parse_blog_post(self, response):
    yield {
        "title": response.css(".post-title::text").extract_first(),
        "author": response.css(".entry-author::text").extract_first(),
        "tags": response.css(".tag::text").extract(),
    }
```

到

```
def parse_blog_post(self, response):
    yield {
        "title": response.css(".post-title::text").get(),
        "author": response.css(".entry-author::text").get(),
        "tags": response.css(".tag::text").getall(),
    }
```

![](img/95376128fabd9e9791679d0d21c99258.png)

[https://unsplash.com/@franki](https://unsplash.com/@franki)

# 3.使用属性选择数据

如果您不习惯 xpath 或 css 选择器，Scrapy 可以让您以类似字典的方式获取属性。

从

```
**>>** response.css('a::attr(href)').getall()
['image1.html', 'image2.html', 'image3.html', 'image4.html', 'image5.html']
```

到

```
**>>** [a.attrib['href'] **for** a **in** response.css('a')]
['image1.html', 'image2.html', 'image3.html', 'image4.html', 'image5.html']
```

使用`attrib`你可以获取 html 属性，而不是使用 xpath 或 css！

![](img/c1857b82822b3893d2352dbe578b4dc9.png)

[https://unsplash.com/@marius](https://unsplash.com/@marius)

# 4.从回调传递数据

通常当你做一些网络抓取时，你需要将信息从一个函数传递到另一个函数。这是您通常使用 response.meta 函数的地方。

```
def parse_blog_post(self, response):

    for link in links:
        yield scrapy.Request(
            link,
            meta={"author": author, "date": post_date},
            callback=self.parse_full_blog_post,
        )

def parse_full_blog_post(self, response):
    author = response.meta["author]
    post_date = response.meta["post_date]
```

现在我们可以使用 follow_all()函数和名为`cb_kwargs`的新关键字，这允许我们传递一个值的字典，然后我们可以在回调函数中访问它。

```
def parse_blog_post(self, response):
    yield from response.follow_all(
        links,
        cb_kwargs={"author": author, "date": post_date},
        callback=self.parse_full_blog_post,
    )

def parse_full_blog_post(self, response, author, post_date):
```

我们在字典中定义变量`author`和`post_date`，并在`parse_full_blog_post`中声明它们。干净多了！

我希望你会发现这些技巧对充分利用 Scrapy 框架很有用。

## 参考

1.  [https://stummjr.org/post/scrapy-in-2020/](https://stummjr.org/post/scrapy-in-2020/)——这篇文章的来源和一篇很棒的博客文章详细介绍了这些观点！

# 相关文章

[](/approach-to-learning-python-f1c9a02024f8) [## 学习 Python 的方法

### 今天如何最大限度地学习 python

towardsdatascience.com](/approach-to-learning-python-f1c9a02024f8) [](https://medium.com/swlh/5-python-tricks-you-should-know-d4a8b32e04db) [## 你应该知道的 5 个 Python 技巧

### 如何轻松增强 python 的基础知识

medium.com](https://medium.com/swlh/5-python-tricks-you-should-know-d4a8b32e04db) [](/scrapy-this-is-how-to-successfully-login-with-ease-ea980e2c5901) [## Scrapy:这就是如何轻松成功登录

### 揭秘用 Scrapy 登录的过程。

towardsdatascience.com](/scrapy-this-is-how-to-successfully-login-with-ease-ea980e2c5901) 

请看[这里](http://www.coding-medic.com/)关于我在我的博客和其他帖子上关于项目的更多细节。更多技术/编码相关内容，请点击这里订阅我的简讯

我将非常感谢任何评论，或者如果你想与 python 合作或需要帮助，请联系我。如果你想和我联系，请在这里 asmith53@ed.ac.uk 或在 twitter 上联系。