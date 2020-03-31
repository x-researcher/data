# Lesson 9: 函数

函数是 R 语言的基本构件之一。可以像对待其他 R 对象一样看待，是可重复使用的小段代码。

如果你已经学过这门课的其他部分，你可能已经用过一些函数。函数的特征通常是函数的名称后跟括号。

让我们尝试使用一些基本的函数来获得乐趣。`Sys.Date()` 函数的作用是返回一个代表今天日期的字符串。在下面输入 `Sys.Date()`，看看会发生什么。

    > Sys.Date()
    [1] "2020-03-25"

R 中的大多数函数都返回一个值。`Sys.Date()` 之类的函数基于计算机环境返回一个值，而其他函数则通过操作输入数据来计算返回值。

`mean()` 函数可以计算向量所有元素的平均值，下面我们以 `c(2, 4, 5)` 为参数，来计算其平均值。

    > mean(c(2, 4, 5))
    [1] 3.666667

函数的参数通常是函数所处理的变量。例如，`mean()` 函数介绍一个向量作为参数，就像例子 `mean(c(2,6,8))` 一样。向量 mean() 将向量中的所有数字相加，然后除以向量的长度。

下面来定义一个函数，使其没有任何调整的输出输入的值。

    boring_function <- function(x) {
      x
    }

试一下这个函数：

    > boring_function('My first function!')
    [1] "My first function!"

要理解 R 中的计算，有两个口号(slogan)是有用的：1、存在的一切都是一个对象。2、发生的一切都是一个函数调用。

如果你想查看任何函数的源代码，只需键入函数名（不带括号和参数）。让我们来试一下，查看 `boring_function` 的源代码。

    > boring_function
    function(x) {
      x
    }
    <bytecode: 0x375dd70>

下面我们来创建一个函数 `my_mean()`，使其能实现 `mean()` 的功能。

    my_mean <- function(my_vector) {
      sum(my_vector) / length(my_vector)
    }

现在试一下使用 `my_mean()` 函数来求向量 `c(4, 5, 10)` 的平均值。

    > my_mean(c(4, 5, 10))
    [1] 6.333333

接下来，让我们尝试写一个带有默认参数的函数。你可以为函数的参数设置默认值，如果使用此函数的人会在大多数情况下将某个参数设置为相同的值，那么这将非常有用。

    remainder <- function(num, divisor = 2) {
        num %% divisor
    }

我们来测试一下 `remainder` 函数。`运行 remainder(5)` 看看会发生什么。

    > remainder(5)
    [1] 1

当只提供一个参数时，R 会匹配 `num` ，因为 `num` 是第一个参数。`divisor` 的默认值是 2，所以结果就和我们看到的一样。

接下来我们来测试一下提供两个参数的情况。键入 `remainder(11, 5)` 看看会发生什么。

    > remainder(11, 5)
    [1] 1

参数再次被适当地匹配。

您还可以在函数中指定参数，当您通过名称指定参数值时，参数的顺序就不在重要了。键入 `remainder(divisor = 11, num = 5)` 来试一下。

    > remainder(divisor = 11, num = 5)
    [1] 5

和你看到的一样，`remainder(11, 5)` 和 `remainder(divisor = 11, num = 5)` 明显不同。

R 中也可以只指定部分参数。试一下 `remainder(4, div = 2)` .

    > remainder(4, div = 2)
    [1] 0

警告：通常情况下，您希望使您的代码尽可能的容易理解。通过指定参数名或仅使用部分参数名而改变参数的顺序，这可能会造成混淆，因此要谨慎使用这些特性。

在所有这些关于参数的讨论中，您可能想知道是否还有方法来查看函数的参数（除查看文档外）。幸运的是，您可以使用 `args()` 函数。键入 `args(remainder)` 来查看 remainder 函数的参数。

    > args(remainder)
    function (num, divisor = 2) 
    NULL

您可能没有意识到我们骗你做了一些有趣的事情。`args()` 是函数，`remainder()` 也是函数，同时也 `args()` 的参数。是的，你可以把函数当作参数。这是个非常强大的概念。让我们来写个脚本来看一下它是如何作用的。

    evaluate <- function(func, dat){
        func(dat)
    }

我们来测试一下新函数，使用 `evalute` 函数来求向量 `c(1.4, 3.6, 7.9, 8.8)` 的标准差。

    > evaluate(sd, c(1.4, 3.6, 7.9, 8.8))
    [1] 3.514138

将函数作为其他函数的参数来传递的想法是编程中一个重要且基本的概念。

您可能会惊讶地发现，您可以将函数作为参数传递，而无需首先定义传递的函数。没有命名的函数被称为匿名函数。

我们使用 evaluate 函数来探索匿名函数是如何工作的。对于 evaluate 函数的第一个参数，我们要写一个很小的只适合一行的函数。在第二个参数中，我们将向第一个参数中的微型匿名函数传递一些数据。

键入 `evaluate(function(x){x+1}, 6)` 来讨论它是如何作用的。

    > evaluate(function(x){x+1}, 6)
    [1] 7

第一个参数是一个微型的匿名函数，它使用参数 `x` 然后返回 `x+1`。我们将数字 6 传递给函数，因此整个表达式的计算结果是 7。

尝试使用 `evaluate` 函数来写一个匿名函数，使其返回向量 `c(8, 4, 0)` 的第一个元素。您的匿名函数应该仅采用一个变量 `x` 作为参数。 

    > evaluate(function(x){x[1]}, c(8, 4, 0))
    [1] 8

现在尝试返回向量的最后一个元素。

    > evaluate(function(x){x[length(x)]}, c(8, 4, 0))
    [1] 0

本节课剩下的课程会频繁用到 `paste()` 函数。键入 `?paste` 来查看文档。

正如您看到的，`paste()` 的第一个参数是 `...` ，被称为省略号。省略号允许向函数传递不确定数量的参数。对于 `paste()` ，可以将任意数量的字符作为参数传递，而 `paste()` 将返回一个组合所有字符串的字符串。

看一下 paste() 是如何运作的，键入 `paste("Programming", "is", "fun!")`

    > paste("Programming", "is", "fun!")
    [1] "Programming is fun!"

是时候编写我们自己的修改版的 `paste()`.

    telegram <- function(...){
      paste("START", ..., "STOP")
    }

现在我们来测试您自己的 `telegram` 函数。

    > telegram("x")
    [1] "START x STOP"

我们尝试另一个函数。

    mad_libs <- function(...){
      args <- list(...)
      place <- args[["place"]]
      adjective <- args[["adjective"]]
      noun <- args[["noun"]]
      paste("News from", place, "today where", adjective, "students took to the streets in protest of the new", noun, "being installed on campus.")
    }

是时候使用你的 `mad_libs` 函数了。确保为你的函数指定各个参数。

    > mad_libs('x','y','z')
    [1] "News from  today where  students took to the streets in protest of the new  being installed on campus."

之前已经熟悉了加减乘除这些二进制运算符。在 R 中，你可以定义你自己的二进制运算符，在下一个脚本中，我将展示给你怎么做。

    "%p%" <- function(x, y){
      paste(x, y)
    }

您创建了自己的二进制运算符！让我们来测试一下。使用新的二进制运算符将"I", "love", "R!" 粘贴在一起。

    > "I" %p% "love" %p% "R!"
    [1] "I love R!"