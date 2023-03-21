# Python 中的私有成员🤔

> 原文：<https://towardsdatascience.com/private-members-in-python-217baf02a162?source=collection_archive---------33----------------------->

![](img/06df20f3feac6393cc06182f4225f233.png)

蒂姆·莫斯霍尔德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

如果你有其他编程语言(Java、C#、C++、Kotlin)的经验，并且已经开始学习 python，你肯定会寻找私有成员来提供封装，因为这是面向对象编程的一个关键原则。每个人都这样做，直到熟悉了“*python”*的编码风格。

通过查看互联网上的一些资源，我发现了一些非常有趣的想法，这些想法很有见地，也很幽默。你一定要看看这个我最喜欢的链接。

Python 不会阻止您访问私有的和受保护的属性和方法，因为它们在默认情况下是公共的😎。所以，实现的决定留给了开发者。

**受保护的**

字段和方法前的单下划线(_)用于表示受保护的访问修饰符。

```
class User:
    def __init__(self, username, password):
        self._username = username  # i act like a protected but you can change me
        self._password = password  # i act like a protected but you can change me

    def _login(self):
        print('i am like a protected method but you can access me')
```

**私人**

字段和方法前的双下划线(__)用于指示私有访问修饰符。

```
class User:
    def __init__(self, username, password):
        self.__username = username  # i act like a private but you can change me
        self.__password = password  # i act like a private but you can change me

    def __login(self):
        print('i am like a private method but you can access me')
```

**访问私有修改器**

```
class Doctor:
    def __init__(self,
                 name,
                 address,
                 specialities,
                 schedule,
                 degree):
        self.name = name
        self.address = address
        self.specialities = specialities
        self.schedule = schedule
        self.__degree = degree

    def __diagnose_patient(self):
        print(F'diagnosed patient bob by doctor {self.name}')

if __name__ == '__main__':
    doctor = Doctor('Dr. Bob', 'KTM', 'OPD', 'SUN: 7-8', 'MBBS')
    doctor._Doctor__diagnose_patient()
```

在上面的代码片段中可以看到，私有方法

```
def __diagnose_patient(self): # convention class name + method name
```

可以通过以下方式访问

```
doctor._Doctor__diagnose_patient()
```

只是约定俗成，虽然这些都可以访问。这是向其他开发者传达信息的一种清晰的方式。**不要在不清楚它的确切作用的情况下使用它**。

**参考文献**

*   [https://radek . io/2011/07/21/private-protected-and-public-in-python/](https://radek.io/2011/07/21/private-protected-and-public-in-python/)
*   [https://stack overflow . com/questions/2064202/private-members-in-python](https://stackoverflow.com/questions/2064202/private-members-in-python)
*   [https://mail . python . org/piper mail/tutor/2003-10 月/025932.html](https://mail.python.org/pipermail/tutor/2003-October/025932.html)