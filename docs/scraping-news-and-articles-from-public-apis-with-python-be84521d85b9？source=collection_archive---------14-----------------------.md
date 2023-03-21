# 用 Python 从公共 API 抓取新闻和文章

> 原文：<https://towardsdatascience.com/scraping-news-and-articles-from-public-apis-with-python-be84521d85b9?source=collection_archive---------14----------------------->

## 让我们探索纽约时报、卫报、黑客新闻和其他 API，并为您的下一个项目收集一些新闻数据！

无论你是数据科学家、程序员还是人工智能专家，你都可以很好地利用大量的新闻文章。尽管获得这些文章可能具有挑战性，因为你必须经过相当多的环节才能获得实际数据——找到正确的新闻来源，探索它们的 API，找出如何根据它们进行认证，最后收集数据。那是很多工作而且没有乐趣。

因此，为了节省您的时间并帮助您入门，这里列出了我能够找到的公共新闻 API，并解释了如何验证、查询它们，最重要的是如何从它们那里获取您需要的所有数据的示例！

![](img/9a0b0fc82162fe389451b14511659630.png)

[里沙布·夏尔马](https://unsplash.com/@rishabhben?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的原始照片

# 纽约时报

在我看来，第一个也是最好的数据来源是《纽约时报》。要开始使用它的 API，你需要在[https://developer.nytimes.com/accounts/create](https://developer.nytimes.com/accounts/create)创建一个账户，在[https://developer.nytimes.com/my-apps/new-app](https://developer.nytimes.com/my-apps/new-app)创建一个应用程序。创建应用程序时，您可以选择激活哪些 API——我建议至少激活*最受欢迎的*、*文章搜索*、*头条新闻*和*归档 API*。当您的应用程序被创建时，您将看到一个密钥，您将使用它来与所有选定的 API 进行交互，所以复制它，让我们开始查询吧！

我们可以用《T21 时报》API 做的最简单的查询就是查找当前的头条新闻:

上面的片段非常简单。我们针对提供了`section`名称和我们的 API 密钥的`topstories/v2`端点运行一个`GET`请求。在这种情况下，Section 是 *science* ，但是 *NY Times* 在这里提供了许多其他选项，例如时尚、健康、体育或戏剧。完整列表可在这里找到[。这个特定的请求将产生类似如下的响应:](https://developer.nytimes.com/docs/top-stories-product/1/overview)

接下来，当你试图获得一些特定的数据集时，可能是最有用的端点是*文章搜索*端点:

这个端点具有许多过滤选项。唯一的必填字段是`q`(查询)，这是搜索词。除此之外，您还可以混合匹配过滤查询、日期范围(`begin_date`、`end_date`)、页码、排序顺序和方面字段。过滤器查询(`fq`)是一个有趣的查询，因为它允许使用 *Lucene 查询语法*，可以使用逻辑操作符(`AND`、`OR`)、否定或通配符来创建复杂的过滤器。好看的教程可以在[这里](http://www.lucenetutorial.com/lucene-query-syntax.html)找到。

上述查询的示例响应可能是这样的(为了清楚起见，删除了一些字段):

我将在这里展示的纽约时报的最后一个端点是他们的归档 API，它返回了从 1851 年开始的给定月份的文章列表。如果您需要大量数据，并且不需要搜索特定术语，这将非常有用。

上面的查询搜索了 1852 年 6 月以来的所有文章，从下面的结果中我们可以看到，即使我们搜索了非常旧的文章，我们仍然得到了 1888 个命中结果。也就是说，其中大多数缺乏有用的数据，如关键词、字数、作者等。所以你最好还是搜索一些最近的文章。

这些只是纽约时报提供的一些(在我看来)更有用的 API。除了这些，在 https://developer.nytimes.com/apis 还有更多。为了探索每一个 API，我还推荐使用查询构建器，比如用于文章搜索的的[，它让你不用任何编码就可以在网站上构建并执行你的测试查询。](https://developer.nytimes.com/docs/articlesearch-product/1/routes/articlesearch.json/get)

# 《卫报》

接下来是另一个重要的新闻和文章来源——卫报。与纽约时报一样，我们首先需要注册一个 API 密钥。你可以在 https://bonobo.capi.gutools.co.uk/register/developer 办理，你会收到一封电子邮件，里面有你的钥匙。这样一来，我们可以导航到 [API 文档](https://open-platform.theguardian.com/documentation/)并开始查询 API。

让我们简单地从查询*卫报*的内容部分开始:

这些部分将内容按主题分组，如果您正在寻找特定类型的内容，例如*科学*或*技术*，这将非常有用。如果我们省略 query ( `q`)参数，我们将收到完整的部分列表，大约 75 条记录。

继续一些更有趣的事情——通过*标签*搜索:

这个查询看起来与前一个非常相似，也返回相似类型的数据。标签也将内容分成不同的类别，但是标签的数量(大约 50000 个)要比章节多得多。这些标签中的每一个都具有类似于例如`world/extreme-weather`的结构。这些在搜索实际文章时非常有用，这是我们接下来要做的。

你来这里的真正目的是文章搜索，为此我们将使用[https://open-platform.theguardian.com/documentation/search](https://open-platform.theguardian.com/documentation/search):

我首先向您展示*部分*和*标签*搜索的原因是，它们可以用于文章搜索。从上面可以看到，我们使用了`section`和`tag`参数来缩小搜索范围，这些值可以使用前面显示的查询找到。除了这些参数，我们还为我们的搜索查询包括了明显的`q`参数，但也使用了`from-date`和`show-fields`参数的开始日期，这允许我们请求与内容相关的额外字段——在这种情况下，这些将是标题、署名、评级和缩短的 URL。这里有更多的完整列表

与之前的所有问题一样，以下是示例响应:

# 黑客新闻

对于更多面向技术的新闻来源，人们可能会转向 *HackerNews* ，它也有自己的公共 REST API。https://github.com/HackerNews/API 上有记载。正如你将看到的，这个 API 是在版本`v0`中，目前非常简单，这意味着它并没有真正提供特定的端点，例如查询文章、评论或用户。

但是，即使它非常基础，它仍然提供了所有必要的东西，例如，获取头条新闻:

上面的代码片段没有前面的那么明显，所以让我们更仔细地看一下。我们首先向 API 端点(`v0/topstories`)发送请求，它不会像您所期望的那样返回热门故事，而实际上只是它们的 id。为了获得实际的故事，我们获取这些 id(前 10 个)并向`v0/item/<ID>`端点发送请求，端点返回每个单独项目的数据，在本例中恰好是一个故事。

您肯定注意到查询 URL 是用`query_type`参数化的。这是因为， *HackerNews* API 也为网站的所有顶部部分提供了类似的端点，即提问、展示、工作或新闻。

这个 API 的一个优点是它不需要认证，所以你不需要请求 API 密钥，也不需要像其他 API 那样担心速率限制。

运行这段代码将得到类似如下的响应:

如果你发现了一篇有趣的文章，想更深入地挖掘一下，那么 *HackerNews API* 也可以帮你。您可以通过遍历所述故事的`kids`字段找到每个提交的评论。这样做的代码如下所示:

首先，我们按照 ID 查找 story ( `item`)，就像我们在前面的例子中做的那样。然后，我们迭代它的`kids`,用各自的 id 运行相同的查询，检索条目，在本例中是指故事评论。如果我们想要构建特定故事的评论的整个树/线程，我们也可以递归地遍历这些。

和往常一样，以下是回答示例:

# 电流

找到受欢迎和高质量的新闻 API 是相当困难的，因为大多数经典报纸没有免费的公共 API。然而，总的新闻数据来源可以用来从报纸上获取文章和新闻，例如*金融时报*和*彭博*只提供付费 API 服务，或者 *CNN* 根本不公开任何 API。

其中一个聚合器叫做 [*Currents API*](https://currentsapi.services/en) 。它汇集了来自数千个来源、18 种语言和 70 多个国家的数据，而且还是免费的。

它类似于之前显示的 API。我们再次需要首先获得 API 密钥。为此，你需要在 https://currentsapi.services/en/register 注册。之后，在[https://currentsapi.services/en/profile](https://currentsapi.services/en/profile)访问您的个人资料并取回您的 API 令牌。

准备好密钥(令牌)后，我们可以请求一些数据。只有一个有趣的终点，那就是 https://api.currentsapi.services/v1/search 的:

这个端点包括许多过滤选项，包括语言、类别、国家等等，如上面的代码片段所示。所有这些都是不言自明的，但是对于我提到的前三个，您将需要一些额外的信息，因为它们可能的值并不是很明显。这些值来自可用的 API 端点[这里是](https://currentsapi.services/api/docs/)，在语言和区域的情况下，实际上只是值到代码的映射(例如`"English": "en"`)，在类别的情况下，只是可能值的列表。上面的代码中省略了它，以使它更短一些，但是我只是将这些映射复制到 Python `dict`中，以避免每次都调用 API。

对上述要求的回应如下:

如果您不搜索特定主题或历史数据，那么还有一个选项是 *Currents API* 提供的，即*最新新闻*端点:

它非常类似于`search`端点，但是这个端点只提供`language`参数并产生如下结果:

# 结论

互联网上有许多很棒的新闻网站和在线报纸，但在大多数情况下，你无法抓取它们的数据或以编程方式访问它们。本文中展示的是极少数具有良好 API 和免费访问的应用程序，您可以将其用于您的下一个项目，无论是一些数据科学、机器学习还是简单的新闻聚合器。如果你不介意为新闻 API 付费，你也可以考虑使用[金融时报](https://developer.ft.com/portal)或[彭博](https://www.bloomberg.com/professional/support/api-library/)。除了 API 之外，你还可以尝试用类似于 [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) 的东西抓取 HTML 并自己解析内容。如果你碰巧发现任何其他好的新闻数据来源，请告诉我，以便我可以将其添加到这个列表中。🙂

*本文最初发布于*[*martinheinz . dev*](https://martinheinz.dev/blog/31?utm_source=tds&utm_medium=referral&utm_campaign=blog_post_31)

[](/advanced-sqlalchemy-features-you-need-to-start-using-e6fc1ddafbdb) [## 您需要开始使用的高级 SQLAlchemy 功能

### 通过 SQLAlchemy 及其混合属性、嵌套查询、表元数据，在 Python 中使用 SQL 很容易

towardsdatascience.com](/advanced-sqlalchemy-features-you-need-to-start-using-e6fc1ddafbdb) [](/all-the-things-you-can-do-with-github-api-and-python-f01790fca131) [## 你可以用 GitHub API 和 Python 做的所有事情

### GitHub REST API 允许您管理问题、分支、回购、提交等等，所以让我们看看您如何使用…

towardsdatascience.com](/all-the-things-you-can-do-with-github-api-and-python-f01790fca131) [](/automating-every-aspect-of-your-python-project-6517336af9da) [## 自动化 Python 项目的各个方面

### 每个 Python 项目都可以从使用 Makefile、优化的 Docker 映像、配置良好的 CI/CD、代码…

towardsdatascience.com](/automating-every-aspect-of-your-python-project-6517336af9da)