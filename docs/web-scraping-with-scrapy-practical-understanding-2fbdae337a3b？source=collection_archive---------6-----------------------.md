# 用 Scrapy 抓取网页:实践理解

> 原文：<https://towardsdatascience.com/web-scraping-with-scrapy-practical-understanding-2fbdae337a3b?source=collection_archive---------6----------------------->

## 网刮系列

## 与 Scrapy 一起动手

![](img/d2a621ddbb16e26afc861c6d83198cbb.png)

由[伊利亚·巴甫洛夫](https://unsplash.com/@ilyapavlov?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/web-scraping?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

在 [***第 1 部分***](https://medium.com/@karthikn2789/web-scraping-with-scrapy-theoretical-understanding-f8639a25d9cd) 中讨论了使用 Scrapy 的所有理论方面，现在是时候给出一些实际例子了。我将把这些理论方面放到越来越复杂的例子中。有 3 个例子，

*   一个通过从天气站点提取城市天气来演示单个请求和响应的例子
*   一个通过从虚拟在线书店提取图书详细信息来演示多个请求和响应的示例
*   一个演示图像抓取的例子

你可以从我的 [GitHub 页面](https://github.com/karthikn2789/Scrapy-Projects)下载这些例子。这是关于使用 Scrapy 和 Selenium 进行网络抓取的 4 部分教程系列的第二部分。其他部分可在以下网址找到

[第 1 部分:用刮刀刮网:理论理解](/web-scraping-with-scrapy-theoretical-understanding-f8639a25d9cd)

[第 3 部分:用硒刮网](/web-scraping-with-selenium-d7b6d8d3265a)

[第 4 部分:用硒刮网&刮屑](https://medium.com/swlh/web-scraping-with-selenium-scrapy-9d9c2e9d83b1)

***重要提示:***
在你尝试抓取任何网站之前，请先通读其 *robots.txt* 文件。可以像[www.google.com/robots.txt](http://www.google.com/robots.txt)一样访问。在那里，你会看到一个允许和不允许抓取谷歌网站的页面列表。您只能访问属于`User-agent: *`的页面和`Allow:`之后的页面。

## 示例 1 —通过从天气站点提取城市天气来处理单个请求和响应

我们这个例子的目标是从[weather.com](https://weather.com)提取今天的“Chennai”城市天气预报。提取的数据必须包含温度、空气质量和条件/描述。你可以自由选择你的城市。只需在蜘蛛代码中提供您所在城市的 URL。如前所述，该网站允许抓取数据，前提是抓取延迟不少于 10 秒，也就是说，在从 weather.com 请求另一个 URL 之前，你必须等待至少 10 秒。这个可以在网站的 [robots.txt](https://weather.com/robots.txt) 中找到。

```
User-agent: *
# Crawl-delay: 10
```

我用`scrapy startproject`命令创建了一个新的 Scrapy 项目，并用

`scrapy genspider -t basic weather_spider weather.com`

开始编码的第一个任务是遵守网站的政策。为了遵守 weather.com 的爬行延迟政策，我们需要在 scrapy 项目的`settings.py`文件中添加下面一行。

`DOWNLOAD_DELAY = 10`

这一行让我们项目中的蜘蛛在发出新的 URL 请求之前等待 10 秒钟。我们现在可以开始编码我们的蜘蛛。

如前所示，生成了模板代码。我对代码做了一些修改。

```
import scrapy
import re
from ..items import WeatherItemclass WeatherSpiderSpider(scrapy.Spider):
    name = "weather_spider"
    allowed_domains = ["weather.com"]def start_requests(self):
        # Weather.com URL for Chennai's weather
        urls = [
            "https://weather.com/en-IN/weather/today/l/bf01d09009561812f3f95abece23d16e123d8c08fd0b8ec7ffc9215c0154913c"
        ]
        for url in urls:
            yield scrapy.Request(url=url, callback=self.parse_url)def parse_url(self, response):# Extracting city, temperature, air quality and condition from the response using XPath
        city = response.xpath('//h1[contains(@class,"location")]/text()').get()
        temp = response.xpath('//span[@data-testid="TemperatureValue"]/text()').get()
        air_quality = response.xpath('//span[@data-testid="AirQualityCategory"]/text()').get()
        cond = response.xpath('//div[@data-testid="wxPhrase"]/text()').get()temp = re.match(r"(\d+)", temp).group(1) + " C"  # Removing the degree symbol and adding C
        city = re.match(r"^(.*)(?: Weather)", city).group(1)  # Removing 'Weather' from location# Yielding the extracted data as Item object. You may also extract as Dictionary
        item = WeatherItem()
        item["city"] = city
        item["temp"] = temp
        item["air_quality"] = air_quality
        item["cond"] = cond
        yield item
```

我认为这个例子的代码是不言自明的。然而，我将解释流程。希望你能从 [***最后一部分***](https://medium.com/@karthikn2789/web-scraping-with-scrapy-theoretical-understanding-f8639a25d9cd) 记住 Scrapy 的整体流程图。我希望能够控制请求，所以我使用`start_requests()`而不是`start_urls`。在`start_requests()`中，指定了钦奈天气页面的 URL。如果你想把它改成你喜欢的城市或者增加更多的城市，请随意。对于 URL 列表中的每个 URL，生成一个请求并产生它。所有这些请求都将到达调度程序，调度程序将在引擎请求时分派这些请求。在对应于该请求的网页被下载器下载之后，响应被发送回引擎，引擎将其定向到相应的蜘蛛。在这种情况下，WeatherSpider 接收响应并调用回调函数`parse_url()`。在这个函数中，我使用 XPath 从响应中提取所需的数据。

你可能理解到这一部分，代码的下一部分对你来说是新的，因为它还没有被解释。我使用了刺儿头 ***物品*** 。这些是定义键值对的 Python 对象。您可以参考此[链接](https://docs.scrapy.org/en/latest/topics/items.html#module-scrapy.item)了解更多关于物品的信息。如果您不希望使用条目，您可以创建一个字典并放弃它。
可能会出现一个问题，在哪里定义这些所谓的项目。请允许我提醒你一下。在创建一个新项目时，我们看到 Scrapy 正在创建一些文件。记得吗？

```
weather/
├── scrapy.cfg
└── weather
    ├── __init__.py
    ├── items.py
    ├── middlewares.py
    ├── pipelines.py
    ├── __pycache__
    ├── settings.py
    └── spiders
        ├── WeatherSpider.py
        ├── __init__.py
        └── __pycache__
```

如果您耐心地沿着这棵树看，您可能会注意到一个名为`items.py`的文件。在这个文件中，你需要定义项目对象。

```
# -*- coding: utf-8 -*-# Define here the models for your scraped items
#
# See documentation in:
# [https://docs.scrapy.org/en/latest/topics/items.html](https://docs.scrapy.org/en/latest/topics/items.html)import scrapyclass WeatherItem(scrapy.Item):
    city = scrapy.Field()
    temp = scrapy.Field()
    air_quality = scrapy.Field()
    cond = scrapy.Field()
```

Scrapy 已经创建了这个类，你需要做的就是定义键值对。在这个例子中，因为我们需要城市名称、温度、空气质量和条件，所以我创建了 4 项。您可以根据项目需要创建任意数量的项目。

当您使用下面的命令运行项目时，将会创建一个包含抓取项目的 JSON 文件。

`scrapy crawl weather_spider -o output.json`

里面的东西看起来会像，

```
output.json
------------[
{"city": "Chennai, Tamil Nadu", "temp": "31 C", "air_quality": "Good", "cond": "Cloudy"}
]
```

万岁！！。你已经成功地执行了一个简单的 Scrapy 项目，处理一个请求和响应。

## 示例 2 —通过从虚拟在线书店提取图书详细信息来处理多个请求和响应

本例中我们的目标是从网站[books.toscrape.com](http://books.toscrape.com)中搜集所有书籍(确切地说是 1000 本)的详细信息。不要担心 robots.txt。这个网站是专门为练习网络抓取而设计和托管的。所以，你是清白的。这个网站是这样设计的，它有 50 页，每页列出 20 本书。您无法从列表页面提取图书详细信息。你必须导航到单本书的网页，以提取所需的细节。这是一个需要抓取多个网页的场景，所以我将使用*爬行蜘蛛*。像前面的例子一样，我已经创建了一个新项目和一个爬行蜘蛛，使用了`scrapy startproject`和

`scrapy genspider -t crawl crawl_spider books.toscrape.com`

对于这个例子，我将提取书名，它的价格，评级和可用性。这个`items.py`文件应该是这样的。

```
class BookstoscrapeItem(scrapy.Item):
    title = scrapy.Field()
    price = scrapy.Field()
    rating = scrapy.Field()
    availability = scrapy.Field()
```

现在项目所需的一切都准备好了，让我们看看`crawl_spider.py`。

```
class CrawlSpiderSpider(CrawlSpider):
    name = "crawl_spider"
    allowed_domains = ["books.toscrape.com"]
    # start_urls = ["http://books.toscrape.com/"] # when trying to use this, comment start_requests()rules = (Rule(LinkExtractor(allow=r"catalogue/"), callback="parse_books", follow=True),)def start_requests(self):
        url = "http://books.toscrape.com/"
        yield scrapy.Request(url)def parse_books(self, response):
        """ Filtering out pages other than books' pages to avoid getting "NotFound" error.
        Because, other pages would not have any 'div' tag with attribute 'class="col-sm-6 product_main"'
        """
        if response.xpath('//div[@class="col-sm-6 product_main"]').get() is not None:
            title = response.xpath('//div[@class="col-sm-6 product_main"]/h1/text()').get()
            price = response.xpath('//div[@class="col-sm-6 product_main"]/p[@class="price_color"]/text()').get()
            stock = (
                response.xpath('//div[@class="col-sm-6 product_main"]/p[@class="instock availability"]/text()')
                .getall()[-1]
                .strip()
            )
            rating = response.xpath('//div[@class="col-sm-6 product_main"]/p[3]/@class').get()# Yielding the extracted data as Item object.
            item = BookstoscrapeItem()
            item["title"] = title
            item["price"] = price
            item["rating"] = rating
            item["availability"] = stock
            yield item
```

你注意到`start_requests()`的变化了吗？为什么我生成一个没有回调的请求？是我在最后一部分说每个请求都必须有相应的回调吗？如果你有这些问题，我赞赏你对细节和批判性推理的关注。向你致敬！！拐弯抹角说够了，让我继续回答你的问题。我没有在初始请求中包含回调，因为`rules`中指定了回调和 URL，后续请求将使用该 URL。

流程将从我用[http://books.toscrape.com 显式生成一个请求开始。](http://books.toscrape.com.)紧随其后的是 LinkExtractor 用模式[http://books.toscrape.com/catalogue/.](http://books.toscrape.com/catalogue/.)提取链接，爬行蜘蛛开始用 LinkExtractor 用`parse_books`作为回调函数创建的所有 URL 生成请求。这些请求被发送到调度程序，当引擎发出请求时，调度程序依次调度请求。像以前一样，通常的流程继续进行，直到调度程序中不再有请求。当您使用 JSON 输出运行这个蜘蛛时，您将获得 1000 本书的详细信息。

```
scrapy crawl crawl_spider -o crawl_spider_output.json
```

示例输出如下所示。

```
[
  {
    "title": "A Light in the Attic",
    "price": "\u00a351.77",
    "rating": "star-rating Three",
    "availability": "In stock (22 available)"
  },
  {
    "title": "Libertarianism for Beginners",
    "price": "\u00a351.33",
    "rating": "star-rating Two",
    "availability": "In stock (19 available)"
  },
  ...
]#Note: /u00a3 is the unicode representation of £
```

如前所述，这不是提取所有 1000 本书的细节的唯一方法。*一个基本的蜘蛛也可以用来*提取精确的细节。我已经用一个基本的蜘蛛程序包含了代码。使用以下命令创建一个基本的蜘蛛。

```
scrapy genspider -t basic book_spider books.toscrape.com
```

基本的蜘蛛包含以下代码。

```
class BookSpiderSpider(scrapy.Spider):
    name = "book_spider"
    allowed_domains = ["books.toscrape.com"]def start_requests(self):
        urls = ["http://books.toscrape.com/"]
        for url in urls:
            yield scrapy.Request(url=url, callback=self.parse_pages)def parse_pages(self, response):
        """
        The purpose of this method is to look for books listing and the link for next page.
        - When it sees books listing, it generates requests with individual book's URL with parse_books() as its callback function.
        - When it sees a next page URL, it generates a request for the next page by calling itself as the callback function.
        """books = response.xpath("//h3")""" Using response.urljoin() to get individual book page """
        """
        for book in books:
            book_url = response.urljoin(book.xpath(".//a/@href").get())
            yield scrapy.Request(url=book_url, callback=self.parse_books)
        """""" Using response.follow() to get individual book page """
        for book in books:
            yield response.follow(url=book.xpath(".//a/@href").get(), callback=self.parse_books)""" Using response. urljoin() to get next page """
        """
        next_page_url = response.xpath('//li[@class="next"]/a/@href').get()
        if next_page_url is not None:
            next_page = response.urljoin(next_page_url)
            yield scrapy.Request(url=next_page, callback=self.parse_pages)
        """""" Using response.follow() to get next page """
        next_page_url = response.xpath('//li[@class="next"]/a/@href').get()
        if next_page_url is not None:
            yield response.follow(url=next_page_url, callback=self.parse_pages)def parse_books(self, response):
        """
        Method to extract book details and yield it as Item object
        """title = response.xpath('//div[@class="col-sm-6 product_main"]/h1/text()').get()
        price = response.xpath('//div[@class="col-sm-6 product_main"]/p[@class="price_color"]/text()').get()
        stock = (
            response.xpath('//div[@class="col-sm-6 product_main"]/p[@class="instock availability"]/text()')
            .getall()[-1]
            .strip()
        )
        rating = response.xpath('//div[@class="col-sm-6 product_main"]/p[3]/@class').get()item = BookstoscrapeItem()
        item["title"] = title
        item["price"] = price
        item["rating"] = rating
        item["availability"] = stock
        yield item
```

你有没有注意到两只蜘蛛都用了同样的`parse_books()`方法？提取图书详细信息的方法是相同的。唯一不同的是，我在基本蜘蛛中用一个专用的长函数`parse_pages()`替换了爬行蜘蛛中的`rules`。希望这能让你看到爬行蜘蛛和基础蜘蛛的区别。

## 示例 3 —图像刮擦

在开始这个例子之前，让我们看一下 Scrapy 如何抓取和处理文件和图像的简要概述。要从网页中抓取文件或图像，您需要使用内置管道，具体来说就是`FilesPipeline`或`ImagesPipeline`，分别用于各自的目的。我将解释使用`FilesPipeline`时的典型工作流程。

1.  您必须使用蜘蛛抓取一个项目，并将所需文件的 URL 放入一个`file_urls`字段。
2.  然后，您返回该项目，该项目将进入项目管道。
3.  当项目到达`FilesPipeline`时，`file_urls`中的 URL 被发送到调度器，由下载器下载。唯一的区别是这些`file_urls`被赋予了更高的优先级，并在处理任何其他请求之前被下载。
4.  当文件被下载后，另一个字段`files`将被结果填充。它将包括实际的下载网址，一个相对的路径，在那里存储，其校验和和状态。

`FilesPipeline`可用于抓取不同类型的文件(图片、pdf、文本等。).`ImagesPipeline`专门用于图像的抓取和处理。除了`FilesPipeline`的功能外，它还具有以下功能:

*   将所有下载的图像转换为 JPG 格式和 RGB 模式
*   生成缩略图
*   检查图像宽度/高度，确保它们满足最小限制

此外，文件名也不同。使用`ImagesPipeline`时，请用`image_urls`和`images`代替`file_urls`和`files`。如果您希望了解更多关于文件和图像处理的信息，您可以随时点击此[链接](https://docs.scrapy.org/en/latest/topics/media-pipeline.html#downloading-and-processing-files-and-images)。

我们这个例子的目标是从网站[books.toscrape.com](http://books.toscrape.com)上抓取所有书籍的封面图片。为了实现我们的目标，我将重新利用前面例子中的*爬行蜘蛛*。在开始编写代码之前，有一个重要的步骤需要完成。您需要设置`ImagesPipeline`。为此，将以下两行添加到项目文件夹中的`settings.py`文件中。

```
ITEM_PIPELINES = {"scrapy.pipelines.images.ImagesPipeline": 1}
IMAGES_STORE = "path/to/store/images"
```

现在您已经准备好编码了。因为我重用了爬行蜘蛛，所以爬行蜘蛛的代码不会有太大的不同。唯一的区别是，您需要创建包含`images`、`image_urls`的 Item 对象，并从 spider 中产生它。

```
# -*- coding: utf-8 -*-
import scrapy
from scrapy.linkextractors import LinkExtractor
from scrapy.spiders import CrawlSpider, Rule
from ..items import ImagescraperItem
import reclass ImageCrawlSpiderSpider(CrawlSpider):
    name = "image_crawl_spider"
    allowed_domains = ["books.toscrape.com"]
    # start_urls = ["http://books.toscrape.com/"]def start_requests(self):
        url = "http://books.toscrape.com/"
        yield scrapy.Request(url=url)rules = (Rule(LinkExtractor(allow=r"catalogue/"), callback="parse_image", follow=True),)def parse_image(self, response):
        if response.xpath('//div[@class="item active"]/img').get() is not None:
            img = response.xpath('//div[@class="item active"]/img/@src').get()"""
            Computing the Absolute path of the image file.
            "image_urls" require absolute path, not relative path
            """
            m = re.match(r"^(?:../../)(.*)$", img).group(1)
            url = "http://books.toscrape.com/"
            img_url = "".join([url, m])image = ImagescraperItem()
            image["image_urls"] = [img_url]  # "image_urls" must be a listyield image
```

`items.py`文件看起来像这样。

```
import scrapyclass ImagescraperItem(scrapy.Item):
    images = scrapy.Field()
    image_urls = scrapy.Field()
```

当您使用输出文件运行蜘蛛程序时，蜘蛛程序将抓取[http://books.toscrape.com 的所有网页，](http://books.toscrape.com,)抓取书籍封面的 URL 并将其作为`image_urls`输出，然后将其发送给调度程序，工作流将继续进行，如本例开头所述。

`scrapy crawl image_crawl_spider -o output.json`

下载的图像将被存储在由`IMAGES_STORE`指定的位置，并且`output.json`将看起来像这样。

```
[
  {
    "image_urls": [
      "http://books.toscrape.com/media/cache/ee/cf/eecfe998905e455df12064dba399c075.jpg"
    ],
    "images": [
      {
        "url": "http://books.toscrape.com/media/cache/ee/cf/eecfe998905e455df12064dba399c075.jpg",
        "path": "full/59d0249d6ae2eeb367e72b04740583bc70f81558.jpg",
        "checksum": "693caff3d97645e73bd28da8e5974946",
        "status": "downloaded"
      }
    ]
  },
  {
    "image_urls": [
      "http://books.toscrape.com/media/cache/08/e9/08e94f3731d7d6b760dfbfbc02ca5c62.jpg"
    ],
    "images": [
      {
        "url": "http://books.toscrape.com/media/cache/08/e9/08e94f3731d7d6b760dfbfbc02ca5c62.jpg",
        "path": "full/1c1a130c161d186db9973e70558b6ec221ce7c4e.jpg",
        "checksum": "e3953238c2ff7ac507a4bed4485c8622",
        "status": "downloaded"
      }
    ]
  },
  ...
]
```

如果你想抓取其他不同格式的文件，你可以使用`FilesPipeline`来代替。我将把这个留给你的好奇心。你可以从这个[链接](https://github.com/karthikn2789/Scrapy-Projects)下载这 3 个例子。

## 避免被禁止

热衷于网络抓取的初学者可能会走极端，以更快的速度抓取网站，这可能会导致他们的 IP 被网站禁止/列入黑名单。一些网站实施了特定的措施来防止机器人爬取它们，这些措施的复杂程度各不相同。

以下是在处理这类网站时需要记住的一些技巧，摘自 [Scrapy Common Practices](https://docs.scrapy.org/en/latest/topics/practices.html?highlight=DOWNLOAD_DELAY#avoiding-getting-banned) :

*   从浏览器中众所周知的用户代理中轮换你的用户代理(谷歌一下可以得到他们的列表)。
*   禁用 cookie(参见 COOKIES_ENABLED ),因为一些网站可能会使用 cookie 来发现机器人行为。
*   使用下载延迟(2 或更高)。参见 DOWNLOAD_DELAY 设置。
*   如果可能的话，使用谷歌缓存获取页面，而不是直接访问网站
*   使用轮换 IP 池。比如免费的 Tor 项目或者像 ProxyMesh 这样的付费服务。一个开源的选择是 scrapoxy，一个超级代理，你可以附加你自己的代理。
*   使用高度分布式的下载器，在内部绕过禁令，这样你就可以专注于解析干净的页面。这种下载程序的一个例子是 Crawlera

## 结束语

因为我的目标是让你在读完这篇教程后自信地与 Scrapy 一起工作，所以我克制自己不去深入 Scrapy 的各种错综复杂的方面。但是，我希望我已经向您介绍了与 Scrapy 一起工作的概念和实践，明确区分了基本蜘蛛和爬行蜘蛛。如果你有兴趣游到这个池子的更深的一端，请随意接受通过点击[这里](https://docs.scrapy.org/en/latest/index.html)可以到达的零碎的官方文件的指导。

在本网刮系列的 [***下一部分***](/web-scraping-with-selenium-d7b6d8d3265a) 中，我们将着眼于硒。

在那之前，祝你好运。保持安全和快乐的学习。！