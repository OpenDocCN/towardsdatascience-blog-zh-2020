# 如何在 Golang 中加入弦乐？

> 原文：<https://towardsdatascience.com/how-to-join-strings-in-golang-beca5223e2b9?source=collection_archive---------15----------------------->

![](img/7dc1ad755d40a2d3a439d6d539db417a.png)

马修·皮尔斯在 [Unsplash](https://unsplash.com/s/photos/tower-bridge?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

golang 中有多种连接字符串的方式。

让我们从简单的开始。

> *最初发布于*[*https://schadokar . dev*](https://schadokar.dev/to-the-point/how-to-join-strings-in-golang/)*。*

# 使用+运算符🔧

```
**package main****import (
 "fmt"
)****func main() {
 str1 := "Hello"
 // there is a space before World
 str2 := " World!"****fmt.Println(str1 + str2)
}**
```

**输出**

```
**Hello World!**
```

# 使用 Sprint，Sprintf，Sprintln 🛠

[fmt](https://golang.org/pkg/fmt/) 包有 [Sprint](https://golang.org/pkg/fmt/#Sprint) 、 [Sprintf](https://golang.org/pkg/fmt/#Sprintf) 和 [Sprintln](https://golang.org/pkg/fmt/#Sprintln) 函数，可以使用默认或自定义格式格式化字符串。

所有的`Sprint`函数都是变量函数。

> [*可变函数*](https://medium.com/rungo/variadic-function-in-go-5d9b23f4c01a) *可以用任意数量的尾随参数调用。*

## 冲刺

`Sprint`使用默认格式格式化并返回结果字符串。

`Sprint`接受空接口。这意味着它可以接受 n 个任意类型的元素。
如果没有传递字符串类型的元素，那么结果字符串将在元素之间添加一个空格。

```
**package main****import (
 "fmt"
)****func main() {
   num := 26** **str := "Feb"** **boolean := true** **withStr := fmt.Sprint(num, str, boolean)** **fmt.Println("With string: ", withStr)** **withOutStr := fmt.Sprint(num, boolean)** **fmt.Println("Without string: ", withOutStr)
}**
```

**输出**

```
With string: 26Febtrue 
Without string: 26 true
```

## Sprintf

`Sprintf`根据格式说明符格式化并返回结果字符串。

**格式说明符**

```
%v	the value in a default format 
%s	the uninterpreted bytes of the string or slice
```

检查`fmt`包中所有可用的[格式指定符](https://golang.org/pkg/fmt/#hdr-Printing)。

> *您可以使用* `*Sprintf*` *功能来创建 DB 的* `*connection string*` *。*

例如我们将创建一个 ***Postgres 连接 URL*** 。

*连接 URL 格式:****postgres://username:password @ hostname/databasename***

```
**package main****import (
 "fmt"
)****func main() {** **dbname := "testdb"** **username := "admin"** **password := "test1234"** **hostname := "localhost"** **connectionURL := fmt.Sprintf("postgres://%s:%s@%v/%v", username, password, hostname, dbname)** **fmt.Println(connectionURL)
}**
```

**输出**

```
**postgres://admin:test1234@localhost/testdb**
```

## Sprintln

`Sprintln`使用默认格式格式化元素或参数。在元素之间添加空格，并在末尾添加一个新行。

```
**package main****import (
 "fmt"
)****func main() {** **str1 := "Hello"
 str2 := "Gophers!"** **msg := fmt.Sprintln(str1, str2)** **fmt.Println(msg)
}**
```

**输出**

```
**Hello Gophers!**
```

# 使用连接功能🔩

golang 标准库在[字符串](https://golang.org/pkg/strings/)包中提供了一个[连接](https://golang.org/pkg/strings/#Join)函数。

`Join`函数接受一个字符串数组和一个分隔符来连接它们。

```
**func Join(elems []string, sep string) string**
```

该示例包含一组工作日。`Join`函数将返回由`,` 分隔的一串工作日。

> *使用* `*Join*` *你可以将一个字符串数组转换成一个字符串。*

```
**package main****import (
 "fmt"
 "strings"
)****func main() {
    weekdays := []string{"Monday", "Tuesday", "Wednesday", "Thursday", "Friday"}
    // there is a space after comma
    fmt.Println(strings.Join(weekdays, ", "))
}**
```

**输出**

```
**Monday, Tuesday, Wednesday, Thursday, Friday**
```

*原载于*[*https://schadokar . dev*](https://schadokar.dev/to-the-point/how-to-join-strings-in-golang/)*。*

*你可以在* [*上看我最新的 golang 教程我的博客*](https://schadokar.dev) *。*