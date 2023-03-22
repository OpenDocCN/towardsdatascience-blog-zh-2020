# Java 中的代码，作为 C++执行。

> 原文：<https://towardsdatascience.com/code-in-java-execute-as-c-921f5db45f20?source=collection_archive---------22----------------------->

## 如何使用 GraalVM 原生映像从 C++调用 Java 方法而不遭受大量的性能下降。

![](img/8916c54b3770a5e028edce0bb3ff5c7f.png)

马克西米利安·魏斯贝克尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Java 和 C++仍然是两种最流行的编程语言。这两种语言有不同的设计和特点。根据问题的不同，一个可能比另一个更好。然而，在某些时候，我们需要集成这些语言，例如调用用 Java 编写的方法到你的 C++代码中。需要集成 Java 和 C++并不是什么新鲜事。事实上，我们可以在[找到一个可以追溯到 1996 年的教程](https://www.javaworld.com/article/2077513/java-tip-17--integrating-java-with-c--.html)。从那以后，这两种语言有了显著的发展。

在本教程中，我们将学习如何使用 GraalVM 本机映像从 C++调用 java 方法。如果这是你第一次听说 GraalVM，你可能会有兴趣看看他们的[主页](https://www.graalvm.org/)。GraalVM 是一个虚拟机，可以用来运行 Java 应用程序。它基于 JVM，但是增加了额外的特性，比如提前编译、更少的内存占用和其他高级优化。此外，GraalVM 提供了一个本机映像插件，您可以使用它将 Java 应用程序编译成本机应用程序。这意味着您的应用程序可以在不需要安装和运行 JVM 的情况下运行。有趣的是，您还可以使用这个原生图像插件将您的 Java 应用程序编译成一个共享库，并从您的 C++代码中加载它。当你的代码主要是用 C++写的，但是你想在它的某个部分使用一个现有的 Java 库时，这就非常方便了。

# 它是如何工作的

在深入教程之前，我们先来看看这个东西是如何工作的。基本上，你要做的如下:

1.  用 Java 写你的方法。
2.  使用 GraalVM 将 Java 代码编译成 C++共享库(yourlib.so)和头文件(yourlib.h)。
3.  将库和头文件加载到 C++项目中。

在这个例子中，我们将模拟一个用例，您想要在您的 C++项目中使用来自第三方 Java 库的函数。特别是，我们有兴趣使用谷歌番石榴库[中的一个数学函数。](https://github.com/google/guava)

# 准备 Java 项目

首先，我们需要为我们的 java 方法建立一个 maven 项目。在 maven 中构建我们的项目有助于我们导入依赖项，并利用 GraalVM 原生映像插件来创建共享库。创建项目后，将依赖项添加到 Guava 库(或您需要的任何库)中，并将 GraalVM 库添加到 pom.xml 文件中:

```
<dependencies>
    <!-- https://mvnrepository.com/artifact/org.graalvm.nativeimage/svm -->
    <dependency>
        <groupId>org.graalvm.nativeimage</groupId>
        <artifactId>svm</artifactId>
        <version>19.3.1</version>
    </dependency>
    <dependency>
        <groupId>com.google.guava</groupId>
        <artifactId>guava</artifactId>
        <version>28.2-jre</version>
    </dependency>
</dependencies>
```

接下来，添加以下插件，将 java 代码构建到 C++共享库中:

```
<build>
 **<finalName>libmymath</finalName>**    <plugins>
        <plugin>
            <groupId>com.oracle.substratevm</groupId>
            <artifactId>native-image-maven-plugin</artifactId>
 **<version>20.0.0</version>**            <executions>
                <execution>
                    <goals>
                        <goal>native-image</goal>
                    </goals>
                    <phase>package</phase>
                </execution>
            </executions>
            <configuration>
 **<buildArgs>--shared -H:Name=libmymath</buildArgs>**            </configuration>
        </plugin>
    </plugins>
</build>
```

我们来看看加粗的部分。在 *< finalName >* 标签上，输入您想要的库名。记下您在 *<版本>* 标签上指定的版本，因为您稍后会需要它。在 *< buildArgs >，*上，我们指定了 *shared* 和 *name* 参数来指示 GraalVM 本机映像创建一个共享库，而不是一个本机可执行文件。将与 *finalName* 相同的名称放在 *Name* 参数上。

现在让我们准备我们的 Java 方法。给定一个整数输入 *x* ，我们感兴趣的是计算 2 的最大幂 *x* 。例如，如果 x 是 14，它将返回 16，因为 16 是大于 14 的最接近 2 的幂。下面是我们函数的代码片段。注意，我们需要放置一个额外的第一个参数*类型的线程*isolate thread 来桥接我们的 Java 和 C++执行。该参数必须在第一个位置。我们还需要在*centry point*annotation*中指定方法的名称。这是我们将在 C++代码*中引用的方法名。*你可以在[这个库](https://github.com/dpanugroho/graalvm-java-method-embedding)里查看完整的 java 项目。*

```
import **com.google.common.math.IntMath**;
import **org.graalvm.nativeimage.IsolateThread**;
import **org.graalvm.nativeimage.c.function.**CEntryPoint;

public class **MyMath** {
    @CEntryPoint (name = "ceilingPowerOfTwo")
    public static int ceilingPowerOfTwo(**IsolateThread** thread, int x) {
       return **IntMath**.*ceilingPowerOfTwo*(x);
    }
}
```

# 设置 GraalVM

设置好项目后，我们现在需要运行 *mvn 包*来将我们的 Java 方法编译到一个共享库中。然而，由于该功能仅在 GraalVM 上可用，我们需要设置 GraalVM 并将 JAVA_HOME 指向 GraalVM 二进制文件。因此，您需要执行以下操作:

1.  [下载 GraalVM](https://github.com/graalvm/graalvm-ce-builds/releases) 。确保您选择了正确的架构、java 版本以及您在 *pom.xml* 中指定的 GraalVM 版本。具体来说，这个示例项目使用 GraalVM 20.0.0 for Java 8。
2.  使用 gu 二进制文件安装本机映像扩展。在 Unix 环境中，您可以使用:
    *<path _ to _ graalvm _ directory>*graalvm-ce-Java 8–20 . 0 . 0 2*bin/gu install native-image*
3.  更新 JAVA_HOME 以指向下载的 GraalVM 目录。在 Unix 环境下，可以使用 export JAVA _ HOME =<*path _ to _ graalvm _ directory/*graalvm-ce-JAVA 8–20 . 0 . 0 2/>。

# 编译到共享库

现在您可以调用 *mvn 包* install 来构建共享库。会生成几个文件: *graal_isolate.h* 、 *graal_isolate_dynamic.h* 、 *libmymath.h* 、 *libmymath_dynamic.h、libmymath.so* (或者其他 OS 中的其他格式)。除了我们自己的库，它还会生成 Graal VM C++库。

# 将库导入到 C++代码中

现在让我们在下面的简单 C++项目中使用我们的共享库。将生成的文件放在这个 C++项目的目录中。例如，您可以创建一个 *include* 文件夹，并将这些文件放在这个文件夹中。

```
#include <iostream>
#include <libmymath.h>

int main() {
    graal_isolate_t *isolate = NULL;
    graal_isolatethread_t *thread = NULL;

    if (graal_create_isolate(NULL, &isolate, &thread) != 0) {
        fprintf(stderr, "initialization error\n");
        return 1;
    }

    printf("Result> %d\n",ceilingPowerOfTwo(thread, 14));
    return 0;
}
```

使用以下命令编译 C++代码，并尝试运行它。

```
*g++ ceilingPowerOfTwoCpp.cpp -L includes/ -I includes/ -lmymath -o ceilingPowerOfTwoCpp*
```

注意:您可能需要使用以下命令将 *includes* 目录设置为 LD_LIBRARY_PATH:

```
export LD_LIBRARY_PATH=<path_to_includes_directory>
```

# 结论

在编程语言同时发展的世界里，互操作性是一个不可避免的要求。GraalVM 原生映像的共享库特性提供了一种将 Java 方法嵌入到 C++程序的简单方法，这使得能够编写复杂的用户定义函数和使用第三方库。一个有希望的例子可能是开发一个以高性能和高级 API 为目标的数据处理框架。

如果你想了解更多关于 GraalVM 的内容，请访问他们的主页。本教程的完整代码可以在[这个资源库](https://github.com/dpanugroho/graalvm-java-method-embedding)中找到。