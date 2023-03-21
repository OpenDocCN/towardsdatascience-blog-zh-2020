# 在下一个 Golang 项目中使用环境变量

> 原文：<https://towardsdatascience.com/use-environment-variable-in-your-next-golang-project-39e17c3aaa66?source=collection_archive---------2----------------------->

## golang 中有多种方法可以使用环境变量和文件。

![](img/1f65fa120eacaf4d88809b266b0c09a5.png)

照片由 [Moja Msanii](https://unsplash.com/@mojamsanii?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/security?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

当创建生产级应用程序时，在应用程序中使用环境变量是事实上的*。*

> *也可以看 youtube 视频。*

# *我们为什么要使用环境变量？*

*假设您有一个具有许多特性的应用程序，每个特性都需要访问数据库。您在每个特性中配置了所有的数据库信息，如`DBURL`、`DBNAME`、`USERNAME`和`PASSWORD`。*

*这种方法有几个主要缺点，但也有很多。*

## *安全问题:*

*   *你在输入密码中的所有信息。现在，所有未经授权的人也可以访问数据库。*
*   *如果你使用的是代码版本控制工具`git`，那么一旦你发布代码，你的数据库的细节就会公之于众。*

## *代码管理:*

*   *如果你改变一个单一的变量，那么你必须改变所有的特征。很有可能你会错过一两个。😌去过那里*
*   *您可以对环境变量进行分类，如`PROD`、`DEV`或`TEST`。只需给变量加上环境前缀。*

*开始时，它可能看起来像一些额外的工作，但是这将在你的项目中奖励你很多。*

> *⚠️只是不要忘记在`**.gitignore**`中包含你的环境文件。*

*是采取行动的时候了。🔨*

## *在这个教程中我们要做什么？*

*在本教程中，我们将以 3 种不同的方式访问环境变量。*

*你可以根据你的需要来使用它。*

*   *`os`包装*
*   *`godotenv`包装*
*   *`viper`包装*

# *创建项目*

*在`**$GOPATH**`之外创建一个项目`**go-env-ways**`。*

# *初始化模块*

*打开项目根目录下的终端，运行下面的命令。*

```
***go mod init goenv***
```

*这个模块将记录项目中使用的所有包及其版本。类似于`nodejs`中的`package.json`。*

*让我们从最简单的开始，使用`os`包。*

# *操作系统包*

*Golang 提供了`os`包，这是一种配置和访问环境变量的简单方法。*

*要设置环境变量，*

```
***os.Setenv(key, value)***
```

*要获得环境变量，*

```
***value := os.Getenv(key)***
```

*在项目中创建一个新文件`main.go`。*

```
***package main**

import (
  "fmt"
  "os"
)

// use os package to get the env variable which is already set
**func envVariable(key string) string** {

  // set env variable using os package
  **os.Setenv(key, "gopher")**

  // return the env variable using os package
  **return os.Getenv(key)**
}

**func main() {**
  // os package
 ** value := envVariable("name")**

  **fmt.Printf("os package: name = %s \n", value)** **fmt.Printf("environment = %s \n", os.Getenv("APP_ENV"))**
}*
```

*运行以下命令进行检查。*

```
***APP_ENV=prod go run main.go**

// Output
**os package: name = gopher
environment = prod***
```

# *GoDotEnv 包*

*加载`.env`文件最简单的方法是使用`godotenv`包。*

# *安装*

*在项目根目录下打开终端。*

```
***go get github.com/joho/godotenv***
```

*`godotenv` 提供了一个`Load` 方法来加载 env 文件。*

```
*// Load the .env file in the current directory
**godotenv.Load()**

// or

**godotenv.Load(".env")***
```

> **Load 方法可以一次加载多个 env 文件。这也支持* `*yaml*` *。更多信息请查看* [*文档*](https://github.com/joho/godotenv) *。**

*在项目根目录下创建一个新的`.env`文件。*

```
***STRONGEST_AVENGER=Thor***
```

*更新`main.go`。*

```
***package main**

import (

    ...
    // Import godotenv
  **"github.com/joho/godotenv"**
)

// use godot package to load/read the .env file and
// return the value of the key
**func goDotEnvVariable(key string) string {**

  // load .env file
  **err := godotenv.Load(".env")**

  if err != nil {
    log.Fatalf("Error loading .env file")
  }

  **return os.Getenv(key)**
}

**func main() {**
    // os package
    ... 

  // godotenv package
  **dotenv := goDotEnvVariable("STRONGEST_AVENGER")**

  fmt.Printf("godotenv : %s = %s \n", "STRONGEST_AVENGER", dotenv)
}*
```

*打开终端并运行`main.go`。*

```
***go run main.go**

// Output
**os package: name = gopher**

**godotenv : STRONGEST_AVENGER = Thor***
```

> *只需在主函数中添加 os 包末尾的代码即可。*

# *毒蛇包装*

*Viper 是 golang 社区中最受欢迎的软件包之一。许多围棋项目都是使用 Viper 构建的，包括 Hugo、Docker 公证人、Mercury。*

> **毒蛇🐍是 Go 应用的完整配置解决方案，包括 12 因素应用。它设计用于在应用程序中工作，可以处理所有类型的配置需求和格式。读取 JSON、TOML、YAML、HCL、envfile 和 Java 属性配置文件**
> 
> *更多信息请阅读[蝰蛇](https://github.com/spf13/viper)的官方文档*

# *安装*

*在项目根目录下打开终端。*

```
***go get github.com/spf13/viper***
```

*要设置配置文件和路径*

```
***viper.SetConfigFile(".env")***
```

*要读取配置文件*

```
***viper.ReadInConfig()***
```

*使用键从配置文件中获取值*

```
***viper.Get(key)***
```

*更新`main.go`。*

```
***import** (
  "fmt"
  "log"
  "os"

  "github.com/joho/godotenv"
  **"github.com/spf13/viper"**
)

// use viper package to read .env file
// return the value of the key
**func viperEnvVariable(key string) string {**

  // SetConfigFile explicitly defines the path, name and extension of the config file.
  // Viper will use this and not check any of the config paths.
  // .env - It will search for the .env file in the current directory
  **viper.SetConfigFile(".env")** 
  // Find and read the config file
  **err := viper.ReadInConfig()**

  if err != nil {
    log.Fatalf("Error while reading config file %s", err)
  }

  // viper.Get() returns an empty interface{}
  // to get the underlying type of the key,
  // we have to do the type assertion, we know the underlying value is string
  // if we type assert to other type it will throw an error
  **value, ok := viper.Get(key).(string)**

  // If the type is a string then ok will be true
  // ok will make sure the program not break
  if !ok {
    log.Fatalf("Invalid type assertion")
  }

 ** return value**
}

**func main() {**

    // os package  
    ...

  // godotenv package
  ...

  // viper package read .env
 ** viperenv := viperEnvVariable("STRONGEST_AVENGER")**

  fmt.Printf("viper : %s = %s \n", "STRONGEST_AVENGER", viperenv)
}*
```

*打开终端并运行`main.go`。*

# *毒蛇不仅限于。环境文件。*

*它支持:*

*   *设置默认值*
*   *读取 JSON、TOML、YAML、HCL、envfile 和 Java 属性配置文件*
*   *实时观察和重读配置文件(可选)*
*   *从环境变量中读取*
*   *从远程配置系统(etcd 或 Consul)读取，并观察变化*
*   *从命令行标志中读取*
*   *从缓冲区读取*
*   *设置显式值*

*Viper 可以被看作是满足应用程序所有配置需求的注册表。*

*让我们实验一下:💣*

*在项目根目录下创建一个新的`config.yaml`文件。*

```
***I_AM_INEVITABLE: "I am Iron Man"***
```

*要设置配置文件名*

```
***viper.SetConfigName("config")***
```

*要设置配置文件路径*

```
*// Look in the current working directory 
**viper.AddConfigPath(".")***
```

*要读取配置文件*

```
***viper.ReadInConfig()***
```

*更新`**main.go**`*

```
*// use viper package to load/read the config file or .env file and
// return the value of the key
**func viperConfigVariable(key string) string** {

  // name of config file (without extension)
  **viper.SetConfigName("config")**
  // look for config in the working directory
 ** viper.AddConfigPath(".")**

  // Find and read the config file
  **err := viper.ReadInConfig()**

  if err != nil {
    log.Fatalf("Error while reading config file %s", err)
  }

  // viper.Get() returns an empty interface{}
  // to get the underlying type of the key,
  // we have to do the type assertion, we know the underlying value is string
  // if we type assert to other type it will throw an error
  **value, ok := viper.Get(key).(string)**

  // If the type is a string then ok will be true
  // ok will make sure the program not break
  if !ok {
    log.Fatalf("Invalid type assertion")
  }

 ** return value**
}

**func main()** {

  // os package
  ...

  // godotenv package
  ...

  // viper package read .env
  ...

  // viper package read config file
  **viperconfig := viperConfigVariable("I_AM_INEVITABLE")**  

  fmt.Printf("viper config : %s = %s \n", "I_AM_INEVITABLE", viperconfig)  
}*
```

*打开终端并运行`main.go`*

```
***go run main.go**

// Output
**os package: name = gopher**

**godotenv : STRONGEST_AVENGER = Thor

viper : STRONGEST_AVENGER = Thor

viper config : I_AM_INEVITABLE = I am Iron Man***
```

# *结论*

*就这样，现在你可以探索他们更多的秘密了🔐。如果你发现一些值得分享的东西，不要犹豫😉。*

*完整的代码可以在 [GitHub](https://github.com/schadokar/blog-projects/tree/go-env-ways) 上找到。*