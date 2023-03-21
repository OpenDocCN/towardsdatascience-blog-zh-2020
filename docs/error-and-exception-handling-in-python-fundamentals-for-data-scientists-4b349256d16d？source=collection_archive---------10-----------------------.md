# Python 中的错误和异常处理:数据科学家基础

> 原文：<https://towardsdatascience.com/error-and-exception-handling-in-python-fundamentals-for-data-scientists-4b349256d16d?source=collection_archive---------10----------------------->

## 用一个具体的例子来理解基础！

![](img/05c08a7ef44dc1e1ee8d3610246a7add.png)

图片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Alexandru Acea](https://unsplash.com/@alexacea?utm_source=medium&utm_medium=referral) 拍摄

Python 中主要有三种可区分的错误:**语法错误**、**异常和逻辑错误**。

*   **语法错误**类似于语言中的语法或拼写错误。如果您的代码中有这样的错误，Python 就无法开始执行您的代码。您会得到一个明确的错误消息，指出什么是错误的，什么需要修复。因此，这是最容易修复的错误类型。
*   Python 中常见的**语法错误**有符号缺失(如逗号、括号、冒号)、关键字拼写错误、缩进不正确。
*   **异常**可能出现在运行时语法正确的代码块中。当 Python 无法执行请求的操作时，它会终止代码并引发一条错误消息。
*   试图从一个不存在的文件中读取数据，对不兼容类型的变量执行操作，将一个数除以零是 Python 中常见的引发错误的**异常**。
*   我们必须消除**语法错误**来运行我们的 Python 代码，而**异常**可以在运行时处理。
*   逻辑错误是最难修复的错误，因为它们不会让你的代码崩溃，你也不会得到任何错误消息。
*   如果您有逻辑错误，您的代码将不会按预期运行。
*   使用不正确的变量名、不能正确反映算法逻辑的代码、在布尔运算符上出错将导致**逻辑错误。**

```
**# Syntax Error-1: Misusing the Assignment Operator** len("data") **=** 4
**Output:** File "ErrorsAndExceptions.py", line 1
    len("data") = 4
SyntaxError: can't assign to function call**# Syntax Error-2: Python Keyword Misuse
fr** k in range(10):
    print(k)
**Output:**
  File "ErrorsAndExceptions.py", line 4
    fr k in range(10):
       ^
SyntaxError: invalid syntax**# Exception-1: ZeroDivisionError**
print (5/0)
**Output:** Traceback (most recent call last):
  File "ErrorsAndExceptions.py", line 12, in <module>
    print (5/0)
ZeroDivisionError: integer division or modulo by zero**# Exception-2: TypeError**
print ('4' + 4)
**Output:**
Traceback (most recent call last):
  File "ErrorsAndExceptions.py", line 10, in <module>
    print ('4' + 4)
TypeError: cannot concatenate 'str' and 'int' objects
```

![](img/bee8d210dd4d9e8ba961c3b3a3c80a50.png)

Uchral Sanjaadorj 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 语法错误

当您的代码有语法错误时，Python 会停止并通过提供代码中错误的位置和明确的错误定义来引发错误。

```
**# Syntax Error-1: Misusing the Assignment Operator** len("data") **=** 4
**Output:** File "ErrorsAndExceptions.py", line 1
    len("data") = 4
SyntaxError: can't assign to function call
```

您需要更正语法错误才能运行代码。

```
len("data") **==** 4
**Output:** True
```

# 异常处理

异常处理的主要目的是防止潜在的故障和不受控制的停止。

我们可以用一个 **try-except** 块来捕捉和处理一个异常:

*   **try 块**包含要监控异常的代码。
*   **except 块**包含特定异常发生时要做的事情。如果没有异常发生，则跳过这一部分，并结束 **try-except 语句**。
*   如果引发的异常与**中指定的异常匹配，则执行 except 块中的代码来处理该异常。**
*   如果没有在关键字之外的**中指定任何类型的异常，您可以捕捉任何异常。**
*   **当第一个异常发生时，try 块**执行终止。
*   **try-except 块**可能有不止一个 **except 块**来处理几种不同类型的异常。
*   可选的 **else 块**只有在 **try 块没有出现异常的情况下才会执行。**如果出现任何异常，将不会执行 else 块。
*   可选的**最后块**用于清理代码。这个块总是被执行。

```
**try:**
 **#code to monitor**
 print (5/0)**#this code will not be executed
 #as above line raises a ZeroDivisionError**
 print ("this code will not be executed")**except** *ZeroDivisionError* as e: **#code to handle the exception when it occurs**
 print(e)
 print("OPPS")**else:**
 print("try block executed if there is no error")**finally:**
 print("finally block is always executed")**Output:** integer division or modulo by zero
OPPS
finally block is always executed
```

![](img/269f65f0d6caf3f10b673fe499325833.png)

照片由 [Serenity Mitchell](https://unsplash.com/@mundane?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 引发异常

如果特定条件发生，您可以使用 **raise** 关键字手动引发异常。

```
student_age = -4if student_age < 2:
 **raise** *Exception*("Sorry, student age cannot be lower than 2")**Output:** Traceback (most recent call last):
  File "ErrorsAndExceptions.py", line 49, in <module>
    raise Exception("Sorry, student age cannot be lower than 2")
Exception: Sorry, student age cannot be lower than 2
```

# 定义自定义例外

如果我们希望对特定条件下发生的事情有更多的控制，我们可以定义自己的异常类型。

为此，我们从**异常**类继承。

```
*class* **InvalidStudentAgeException(*Exception*)**:**"""Exception for errors in the student age
  Attributes:
  student_age -- student_age that we monitor
  error_message -- explanation of the error
 """***def* **__init__**(*self*, *student_age*, error_*message*="Student age should 
 be in 2-10 range"):self.student_age = student_age
 self.error_message = error_message
 *super*().__init__(self.error_message)student_age = *int*(input("Enter Student age: "))if not 1 < student_age < 11:
 raise **InvalidStudentAgeException(student_age)****Output:**
Enter Student age: 54
Traceback (most recent call last):
  File "ErrorsAndExceptions.py", line 68, in <module>
    raise InvalidStudentAgeException(student_age)
__main__.InvalidStudentAgeException: Student age should be in 2-10 range: 54 is not a valid age
```

上面，我们覆盖了**异常**基类的构造函数，以使用我们自己的自定义参数**学生年龄**和**错误消息**。配合使用 **super()。_ _ init _ _(self . error _ message)**，我们实例化了基类 **Exception** ，参数帮助我们显示错误消息。

![](img/adf65e14319bc3e92a5299b2c28a4a06.png)

照片由 [Serenity Mitchell](https://unsplash.com/@mundane?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 关键要点

*   我们必须消除**语法错误**来运行我们的 Python 代码，而**异常**可以在运行时处理。
*   我们可以用 **try-except** 块捕捉并处理异常。
*   如果我们希望对特定条件下发生的事情有更多的控制，我们可以定义自己的异常类型。

# 结论

在这篇文章中，我解释了在 Python 中**错误和异常处理的基础。**

这篇文章中的代码可以在[我的 GitHub 库中找到。](https://github.com/eisbilen/ExceptionHandling)

我希望这篇文章对你有用。

感谢您的阅读！