# 让我们谈谈测试驱动的开发

> 原文：<https://towardsdatascience.com/lets-talk-test-driven-development-c73551ca2871?source=collection_archive---------40----------------------->

## 干净的代码

## 它的意思是通过在“如何”之前关注“什么”来支持你。

![](img/fbaad8cb8b5f3c374e8bbf8f2fa1c600.png)

照片由[凯·皮尔格](https://unsplash.com/@kaip?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

许多程序员认为在编写代码之前编写测试的想法是荒谬的。他们认为这是没有用的，并减缓了开发过程。从某种意义上说，这是正确的，它确实影响了发展的速度。但是如果你的系统没有弹性，那么开发的速度就没什么关系了。

开发人员的工作是交付不仅实用，而且可读和可维护的代码。测试驱动的开发可以帮助你做到这一点。

根据 Martin Fowler 的说法，TDD 是:

*   为您想要添加的下一个功能编写测试。
*   编写功能代码，直到测试通过。
*   重构新旧代码，使其结构良好。

TDD 最重要的一个方面是它将焦点从如何转移到问题的什么上。

# 让我们编码

我们将把 TDD 应用到面试中普遍被问到的一个问题上，并尝试提出一个解决方案。这个问题叫做[反向波兰符号](https://leetcode.com/problems/evaluate-reverse-polish-notation/)。

*在逆波兰记法中，运算符跟在它们的操作数后面；例如，要将 3 和 4 相加，可以写成* `*3 4 +*` *而不是* `*3 + 4*` *。如果有多个操作，操作符在第二个操作数后立即给出；因此，在传统符号中，表达式被写成* `*3 − 4 + 5*` *，而在逆波兰符号中，表达式被写成* `*3 4 − 5 +*` *:首先从 3 中减去 4，然后加 5。*

我们将把问题分成更小的步骤，并思考每一步期望我们做什么。我们稍后将考虑“如何”。用来求解的语言是 Java。

# 测试 1

我们的起始状态是一个输入字符串:`3 4 +`

我们想要实现的是对每个 char 进行控制，以便我们可以对其进行操作。所以我们想从一个字符串中得到一个字符串数组。

我们的测试将如下所示:

```
[@Test](http://twitter.com/Test)
public void removeSpacesFromTheString() {
        ReversePolishNotation rpn = new ReversePolishNotation();
        String[] result = rpn.getStringArray("3 4 +");
        assertEquals("3", result[0]);
        assertEquals("4", result[1]);
        assertEquals("+", result[2]);
    }
```

一旦我们有了确定**我们需要什么的测试，我们将考虑**如何**去做。将字符串转换成字符串数组非常简单。**

```
public String[] getStringArray(String s) {
        return s.split(" ");
    }
```

# 测试 2

我们的第一个测试及其实现已经准备好了。让我们看看下一步我们想要什么。我们希望将运算符应用于我们遇到的数字。

所以在这种情况下，我们想把两个数相加。

```
[@Test](http://twitter.com/Test)
public void addNumbersWhenPlusOperatorIsFound() {
        ReversePolishNotation rpn = new ReversePolishNotation();
        double result = rpn.evaluate("3 4 +");
        assertEquals(7.0, result, 0.1);
    }
```

因此，我们需要一种方法将运算符应用于我们的数字。因为我们只有两个数，我们可以把它们相加。

```
public double evaluate(String s) {
        String[] elements = getStringArray(s);
        double result = 0.0;
        for (int i = 0; i < elements.length; i++) {
            String c = elements[i];
            if (c.equals("+") && i >= 2) {
                result = parseDouble(elements[i - 1]) + parseDouble(elements[i - 2]);
            }
        }
        return result;
    }
```

我们迭代数组，并添加我们遇到的数字`+`。

# 测试 3

现在让我们通过在字符串中添加多个`+`操作符来增加趣味性。我们的测试会失败，因为它是硬编码的。对于输入`5 5 2 + +`，我们的实现会失败。

我们希望我们的函数返回这个字符串`5 + (5 + 2)`的结果。我们的代码应该这样评估。

```
[@Test](http://twitter.com/Test)
public void evaluateWhenMultiplePlusOperatorsAreFound() {
    ReversePolishNotation rpn = new ReversePolishNotation();
    double result = rpn.evaluate("5 5 2 + +");
    assertEquals(12.0, result, 0.1);
}
```

要做到这一点，我们需要想出一种方法，使用某种数据结构来处理运算符和数字。在这种情况下，我们可以使用一个`Stack`来推动数字，直到我们找到一个操作符，并通过弹出最后两个值来应用它。

```
public double evaluate(String s) {
        String[] elements = getStringArray(s);
        Stack<Double> numbers = new Stack();
        double result = 0.0;
        for (int i = 0; i < elements.length; i++) {
            String c = elements[i];
            if (c.equals("+") && i >= 2) {
                result = numbers.push(numbers.pop() + numbers.pop());
            } else {
                numbers.push(Double.parseDouble(c));
            }
        }
        return result;
    }
```

# 测试 4

下一步是确保我们能够应用所有的四个操作符，这现在非常简单。我们只需要检查不同的操作符，并相应地应用它们。

```
[@Test](http://twitter.com/Test)
public void addNumbersWhenMinusOperatorIsFound() {
    ReversePolishNotation rpn = new ReversePolishNotation();
    double result = rpn.evaluate("5 5 2 + -");
    assertEquals(2.0, result, 0.1);
}[@Test](http://twitter.com/Test)
public void multiplyNumbersWhenMultiplyOperatorIsFound() {
    ReversePolishNotation rpn = new ReversePolishNotation();
    double result = rpn.evaluate("5 5 2 + *");
    assertEquals(35.0, result, 0.1);
}
```

我们可以跳过一步，为所有操作符实现该方法。它也会处理乘法和除法。

```
public double evaluate(String s) {
    String[] elements = getStringArray(s);
    Stack<Double> numbers = new Stack();
    double result = 0.0;
    for (String c : elements) {
        switch (c) {
            case "+":
               result = numbers.push(numbers.pop() + numbers.pop());
               break;
            case "-":
               result = numbers.push(numbers.pop() - numbers.pop());
               break;
            case "*":
               result = numbers.push(numbers.pop() * numbers.pop());
               break;
            case "/":
               result = numbers.push(numbers.pop() / numbers.pop());
               break;
            default:
                numbers.push(Double.parseDouble(c));
                break;
        }
    }
    return result;
}
```

# 最后试验

一切正常，似乎我们已经达到了我们的最终结果。我们来测试一下大输入`10 6 9 3 + -11 * / * 17 + 5 +`。输入包含多个运算符和负数。

```
[@Test](http://twitter.com/Test)
public void evaluateWhenAllOperatorAreFound() {
    ReversePolishNotation rpn = new ReversePolishNotation();
    double result = rpn.evaluate("10 6 9 3 + -11 * / * 17 + 5 +");
    assertEquals(21.5, result, 0.1);
}
```

我们在这一点上的实现失败了。这有点令人惊讶，因为它应该已经工作了，但似乎操作符`—`和`\`的工作方式与`+`和`*`不同。让我们更改实现来处理这个问题。

```
public double evaluate(String s) {
    String[] elements = getStringArray(s);
    Stack<Double> n = new Stack();
    double result = 0.0;
    for (String c : elements) {
        switch (c) {
            case "+": {
                result = n.push(n.pop() + n.pop());
                break;
            }
            case "-": {
                double fNumber = n.pop();
                double sNumber = n.pop();
                result = n.push(sNumber - fNumber);
                break;
            }
            case "*": {
                result = n.push(n.pop() * n.pop());
                break;
            }
            case "/": {
                double fNumber = n.pop();
                double sNumber = n.pop();
                result = n.push(sNumber / fNumber);
                break;
            }
            default:
                n.push(Double.*parseDouble*(c));
                break;
        }
    }
    return result;
}
```

# 优势

我们已经找到了最终的解决方案。花点时间理解这一点。在我们开始实现算法之前，焦点总是在我们期望从算法中得到什么。TDD 的优势是真实的:

*   你写出更好的软件。
*   你避免过度工程化。
*   引入新功能时，您会受到保护。
*   你的软件是自我记录的。

如果您想了解如何在解决方案中消除切换阶梯，您可以阅读这篇文章。

[](https://levelup.gitconnected.com/how-to-get-rid-of-if-else-ladder-b70f36cd834d) [## 如何摆脱 If-Else 阶梯

### 使用高阶函数和 HashMaps 编写干净代码的简单方法

levelup.gitconnected.com](https://levelup.gitconnected.com/how-to-get-rid-of-if-else-ladder-b70f36cd834d) 

# 最终想法

我试图解释为什么我们应该使用测试驱动开发。有更好的文章可以帮助你理解如何去做。其中一个是这里的。TDD 实际上是一种设计技术。TDD 的基础集中在使用小型测试以紧急的方式从头开始设计系统。

一开始可能看起来很慢，但是通过练习，你会获得动力。

我希望这有助于您理解 TDD 背后的原因。