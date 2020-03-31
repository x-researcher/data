# Lesson 14: 日期和时间

## 本课总结

1. 日期和时间可分为 `POSIXct` 和 `POSIXlt` 两大类。
2. `class()` 可查看其类别，`unclass()` 查看其内部情况。
3. `Sys.time()` 返回的是类别为 `POSIXct` 的对象，但是我们可以使用 `as.POSIXlt(Sys.time())` 将结果强制转换为 `POSIXlt`。
4. `POSIXlt` 对象是个列表。
5. 可以对时间和日期执行算术运算和比较运算。
6. 您可以使用 `difftime()` 函数，它允许您指定一个 `单位` 参数。例  `difftime(Sys.time(), t1, units = 'days')` 。

## 本课详细内容

R 有一种特殊的表示日期和时间的方法，如果您处理的数据展示了某件事情是如何随时间变化的（即时间序列数据），或者您的数据包含一些其他的时间信息，比如出生日期，那么这种方法就很有帮助。

日期属于 `Date` 类别，时间分为 `POSIXct` 和 `POSIXlt` 类别。在运算的内部，日期存储为 `1970-01-01` 之后的天数，时间存储为 `1970-01-01` 之后的秒数（对于 `POSIXct` 来说）或秒、分钟、小时等列表（对于 `POSIXlt`）。

我们首先使用 `d1 <- Sys.Date()` 来获取当前日期并将其存储在变量 `d1` 中。

    > d1 <- Sys.Date()

使用 `class()` 变量来确认 d1 是一个时间对象。

    > class(d1)
    [1] "Date"

我们可以使用 `unclass()` 函数来查看 d1 的内部情况。试一下。

    > unclass(d1)
    [1] 18350

这是从 `1970-01-01` 开始的确切天数！

但是，如果您将 d1 打印到控制台，您将得到今天的日期，以 `年-月-日` 的方式展示。试试吧。

    > d1
    [1] "2020-03-29"

如果我们需要引用一个 `1970-01-01` 之前的日期，请创建一个变量 d2，包含 `as.Date("1969-01-01")`。

    > d2 <- as.Date("1969-01-01")

现在再次使用 `unclass()` 函数来查看在内部 d2 看起来时什么样的。

    > unclass(d2)
    [1] -365

正如您所预料的，您得到了一个负数。在本例中，它是 `-365`，因为 `1969-01-01` 正好时 `1970-01-01` 之前的一个日历年（即 365 天）。

现在，让我们看看 R 是如何存储时间的。可以使用不带参数的 `Sys.time()` 函数访问当前日期和时间。并将结果存储在一个名为 t1 的变量中，并查看内容和类别。

    > t1 <- Sys.time()
    > t1
    [1] "2020-03-29 08:48:08 UTC"
    > class(t1)
    [1] "POSIXct" "POSIXt"

如前所述，`POSIXct` 只是 R 表示时间信息的两种方式之一。您可以忽略上面的第二个值 `POSIXt`，它只是 `POSIXct` 和 `POSIXlt` 之间的一种通用语言。使用 `unclass()` 查看 t1 的内部情况，自 1970 年初以来的秒数。

    > unclass(t1)
    [1] 1585471689

默认情况下，`Sys.time()` 返回的是类别为 `POSIXct` 的对象，但是我们可以使用 `as.POSIXlt(Sys.time())` 将结果强制转换为 `POSIXlt`。试一试，将结果存储在 t2 中。并查看其类别和内容。

    > class(t2)
    [1] "POSIXlt" "POSIXt"
    
    > t2
    [1] "2020-03-29 08:58:30 UTC"

t2 的输出格式与 t1 相同。现在试试 `unclass(t2)` ，看看它内部有什么不同。

    > unclass(t2)
    $sec
    [1] 30.97262
    
    $min
    [1] 58
    
    $hour
    [1] 8
    
    $mday
    [1] 29
    
    $mon
    [1] 2
    
    $year
    [1] 120
    
    $wday
    [1] 0
    
    $yday
    [1] 88
    
    $isdst
    [1] 0
    
    $zone
    [1] "UTC"
    
    $gmtoff
    [1] 0
    
    attr(, "tzone")
    [1] ""    "UTC" "UTC"

与所有 `POSIXlt` 对象一样，t2 是一个由日期和时间组成的列表。使用 `str(unclass(t2))` 获得更紧凑的预览。

    > str(unclass(t2))
    List of 11
     $ sec   : num 31
     $ min   : int 58
     $ hour  : int 8
     $ mday  : int 29
     $ mon   : int 2
     $ year  : int 120
     $ wday  : int 0
     $ yday  : int 88
     $ isdst : int 0
     $ zone  : chr "UTC"
     $ gmtoff: int 0
     - attr(*, "tzone")= chr [1:3] "" "UTC" "UTC"

例如，如果我们只需要存储在 t2 中时间的分钟数，我们可以使用 t2$min 访问他们。试试吧。

    > t2$min
    [1] 58

现在我们已经探索了日期和时间对象的三种类型，接下来让我们看一些可以从这些对象中提取有用信息的一些函数，`weekday()`，`month()` 和 `quarter()`。

    > weekdays(d1)
    [1] "Sunday"
    
    > months(t1)
    [1] "March"
    
    > quarters(t2)
    [1] "Q1"

通常，数据集中的日期和时间是 R 中不认识的格式。在这种情况下，`strptime()` 函数很有用。

`strptime()` 将字符向量转换为 `POSIXlt`。在这个意义上，它类似于 `as.POSIXlt()`，只是不必输入特定的格式 `（YYYY-MM-DD）`。

我们创建一个变量来看一下它是如何发挥作用的。

    > t3 <- "October 17, 1986 08:24"
    > t4 <- strptime(t3, "%B %d, %Y %H:%M")
    > t4
    [1] "1986-10-17 08:24:00 UTC"

我就是我们期待的格式。现在，我们来检查下它的类别。

    > class(t4)
    [1] "POSIXlt" "POSIXt"

最后，您可以对日期和时间执行许多操作，包括算术运算（+和-）和比较运算（<，==，等等）。

变量 t1 是您之前使用 `Sys.time()` 创建的时间对象。我们可以使用 `>` 操作运算符将其与当前时间比较。

    > Sys.time() > t1
    [1] TRUE

我们也可以执行算术运算。

    > Sys.time() - t1
    Time difference of 1.13737 hours

同样的思路也适用于加法和其他比较运算符。如果您希望在查看上述时间差异是对时间有更多的控制权，您可以使用 `difftime()` 函数，它允许您指定一个 `单位` 参数。

使用 `difftime(Sys.time(), t1, units = 'days')` 来查看自从创建 t1 过去了总共多少天。

    > difftime(Sys.time(), t1, units = 'days')
    Time difference of 0.05028701 days

在这节课中，你学习了如何在 R 中处理日期和时间。虽然了解这些基础知识很重要，但是如果你发现自己经常处理日期和时间数据，你可能会想看看 `Hadley Wickham` 的 `lubridate` 软件包。