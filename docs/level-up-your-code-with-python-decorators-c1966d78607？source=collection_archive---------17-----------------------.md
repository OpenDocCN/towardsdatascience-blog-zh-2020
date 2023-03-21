# 用 Python decorators 提升你的代码

> 原文：<https://towardsdatascience.com/level-up-your-code-with-python-decorators-c1966d78607?source=collection_archive---------17----------------------->

## 日志记录、类型检查、异常处理等等！

![](img/a063054df8c7cae8474856e5c8844171.png)

由 pexels 上的 [kaboompics](https://www.pexels.com/photo/brush-painting-the-white-wall-6368/)

在每一个 Python 用户的生活中，都会有一个阶段，你可以从写好代码提升到伟大的代码。

一旦你掌握了 Python 的核心功能，比如列表理解和三元运算符，你就应该准备好编写更具可读性的 Python 了。装饰器是提升代码可读性的关键，这里我们将介绍它们的基础知识，以及使用它们的 3 种方法。

## 什么是室内设计师？

简而言之，装饰器是包装其他函数的函数。如果你在一个功能的开始和结束都想要一些东西，那么装饰者会让你的生活变得更容易。

装饰者可以被看作是函数定义前的一个`@`符号。你可能已经在一个 [flask app](https://flask.palletsprojects.com/en/1.1.x/) 或 [click CLI](https://click.palletsprojects.com/en/7.x/) 中发现了它们，但是解释它们如何工作的最简单的方法是通过一个简短的例子来完成。

假设在函数的开始，我们想打印“开始”，在函数的结尾，我们想打印“完成”。为了实现这一目标，我们可以采取以下措施:

```
def circle_area(radius):
    print("Started: circle_area")
    area = 3.142 * radius ** 2
    print("Finished")
    return areaarea = circle_area(2)
```

这种方法是可行的，但是现在我们的代码充斥着打印语句。函数的实际内容可以只用一行来写，但是我们用了三行！因此，让我们通过创建一个包含我们操作的主要成分的新函数和另一个仅包含打印语句的函数来使这一点更清楚:

```
def circle_area(radius):
    return 3.142 * radius ** 2def circle_area_and_print(radius):
    print("Started: circle_area_and_print")
    area = circle_area(radius)
    print("Ended")
    return areaarea = circle_area_and_print(2)
```

乍一看，你可能会说这看起来并不清楚，因为我们比以前拥有更多的代码。乍一看，您可能会注意到我们添加的代码可读性更好。然而，我们的工作还没有完成，如果我们想从代码中得到更多呢？

比方说，我们也想在其他函数之前和之后打印。作为懒惰的数据科学家，我们不希望多次编写同一行代码，所以让我们尝试概括我们已经拥有的代码。我们可以用这个新的`printer`函数做到这一点。

```
def printer(function):
    def new_function(*args):
        print(f"Started: {function.__name__}")
        output = function(*args)
        print("Finished")
        return output
    return new_functiondef circle_area(radius):
    return 3.142 * radius ** 2area = printer(circle_area)(2)
```

那么`printer`函数在这里做什么呢？

这个函数的美妙之处在于，它构造了一个新函数，方法是在执行传递的函数之前和之后，将我们的原始函数添加到打印语句中。现在`printer`返回了新的函数句柄，允许我们获取任意函数并使用打印语句返回它。

这就是 Python 装饰器真正发挥作用的地方。现在我们已经编写了`printer`函数来返回原始函数的修改版本，我们需要做的就是在函数定义前加上`@printer`:

```
@printer
def circle_area(radius):
    return 3.142 * radius ** 2area = circle_area(2)
```

即使在这个简单的例子中，当您将 printer decorator 的使用与我们在开始时编写的代码进行比较时，我们的代码要清晰得多。

## 我还能用它们做什么？

装饰者有一些巧妙的技巧，可以让你的代码更容易理解和阅读。

我们来看看:

1.  日志记录(类似于第一个例子，但更有用)
2.  类型检查
3.  错误处理

## 1.记录

对于第一个示例，我们需要在脚本中编写一个小函数来设置日志记录:

```
import loggingdef setup_logging(name="logger",
                  filepath=None,
                  stream_log_level="DEBUG",
                  file_log_level="DEBUG"):

    logger = logging.getLogger(name)
    logger.setLevel("DEBUG")
    formatter = logging.Formatter(
        '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
    ) ch = logging.StreamHandler()
    ch.setLevel(getattr(logging, stream_log_level))
    ch.setFormatter(formatter)
    logger.addHandler(ch) if filepath is not None:
        fh = logging.FileHandler(filepath)
        fh.setLevel(getattr(logging, file_log_level))
        fh.setFormatter(formatter)
        logger.addHandler(fh) return loggerlogger = setup_logging(name="default_log",filepath="logger.log")
```

现在我们可以编写新的日志装饰器了:

```
def log_decorator(log_name):
    def log_this(function):
        logger = logging.getLogger(log_name)
        def new_function(*args,**kwargs):
            logger.debug(f"{function.__name__} - {args} - {kwargs}")
            output = function(*args,**kwargs)
            logger.debug(f"{function.__name__} returned: {output}")
            return output
        return new_function
    return log_this
```

这比我们之前看到的`printer`装饰稍微复杂一些。这一次，我们想要构建一个可以接受参数(记录器的名称)的装饰器。

然而装饰函数只能接受一个参数，所以我们将这个函数包装在另一个可以接受许多参数的函数中。你需要一段时间来理解这种类似于 inception 的函数包装，但是一旦你这样做了，它就是数据科学家工具箱中的无价工具。

## 2.类型检查

现在这是一个有争议的问题，所以抓紧你的帽子，我们可能会经历一段颠簸的旅程…

有些人会说这种修饰类型检查函数输入的方式被许多人认为是“非 Pythonic 化的”。Python 是一种动态类型的语言，所以冒着惹起太多麻烦的风险，重要的是要注意，一般来说，在编写 Python 时，我们应该请求原谅，而不是请求许可。

然而，如果你愿意对 Python 的法则有点厚脸皮，那么你会发现这是一个非常方便的工具，尤其是在开发代码的时候。当识别正在传递的错误类型时，它变得非常清楚。

记住所有这些，下面是代码:

```
def accepts(*types):
    def check_accepts(function):
        assert len(types) == function.__code__.co_argcount,\
            "Number of typed inputs must match the function inputs"
        def new_function(*args, **kwargs):
            for (a, t) in zip(args, types):
                assert isinstance(a, t), \
                       "arg %r does not match %s" % (a,t)
            return function(*args, **kwargs)
        return new_function
    return check_accepts@accepts((int,float))
def circle_area(radius):
    return 3.142 * radius ** 2
```

所以现在我们已经确保了`circle_area`函数的输入只接受类型`ints`或`floats`，否则它将引发一个`AssertionError`。

## 3.错误处理

对于这个例子，让我们假设我们试图从一个 API 访问数据，但是这个 API 每分钟只允许给定数量的请求。我们可以编写一个装饰器来包装 API 调用，并继续尝试获取数据，直到成功。

```
import api
import time
API_WAIT_TIME = 5 #minutes
MAX_RETRIES = 10def error_handling(api_function):
    def trial(*args, num_retries=0, **kwargs):
        try:
            return api_function(*args, **kwargs)
        except api.error.RateLimitError:
            if num_retries > MAX_RETRIES:
                raise RuntimeError("Too many retries")
            else:
                msg = f"rate limit reached. Waiting {API_WAIT_TIME} minutes ..."
                time.sleep(API_WAIT_TIME * 60)
                return trial(*args, num_retries=num_retries + 1, **kwargs) return trial
```

每次我们从`api`模块请求数据时，我们都可以使用这个装饰器。装饰器将继续尝试，直到它获得数据或达到允许的最大重试次数，如果是这种情况，将引发一个`RuntimeError`。

同样，这个函数的一个非常相似的版本可以用来任意捕捉不同的异常，并以不同的方式处理它们。我将让读者自己去思考使用这种装饰器的新方法。

## 我们学到了什么？

希望您现在对什么是 Python 装饰器有了更好的了解，并且有信心在您的代码库中使用它(如果没有，那么我让您失望了……)。

这里的关键是，如果你有一个函数经常出现在另一个函数的开头和结尾，那么装饰器将是你新的最好的朋友。装饰者将帮助你的代码不仅可读性更好，而且更加模块化和可重用。