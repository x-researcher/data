# Lesson 5: 缺失值
缺失值在统计和数据分析中发挥着重要的作用。通常情况下，缺失值不能被忽略，而且应该被仔细研究来判断是否存在潜在的模式或原因导致了缺失值的形成。

## NA
在 R 语言中，NA 代表的意思是不可用(`not availabel`) 或 缺失(`missing`，在统计学意义上)，本课程，我们将进一步探讨缺失值。

任何涉及 NA 的操作通常都会生成 NA。为了说明，我们创建一个向量 c(44, NA, 5, NA)，并将其值赋予变量 x。
~~~r
> x <- c(44, NA, 5, NA)
> x * 3
[1] 132  NA  15  NA
~~~
注意，向量 x 中的 NA 值元素，在结果向量中仍是 NA。

为了让事情更加有趣，我们创建一个向量 y，使其包含 1000 个符合标准正态分布的值。
~~~r
> y <- rnorm(1000)
~~~
然后，创建一个包含 1000 个 NA 的向量 z：
~~~r
> z <- rep(NA, 1000)
~~~
最后，从这 2000 个值(y 和 z)中随机选择 100 个，我们不清楚这些值中包含了多少个 NA 值：
~~~r
> my_data <- sample(c(y, z), 100)
~~~
首先，我们想知道 NA 位于 `my_data` 的位置，is.na()函数会告诉我们向量的每一个元素是否是 NA：
~~~r
my_na <- is.na(my_data)
> my_na
  [1] FALSE  TRUE  TRUE FALSE  TRUE FALSE  TRUE FALSE FALSE  TRUE  TRUE FALSE
 [13] FALSE FALSE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE  TRUE
 [25]  TRUE FALSE FALSE FALSE  TRUE  TRUE  TRUE FALSE  TRUE FALSE  TRUE FALSE
 [37]  TRUE FALSE FALSE  TRUE  TRUE  TRUE FALSE FALSE FALSE  TRUE  TRUE  TRUE
 [49] FALSE FALSE FALSE  TRUE  TRUE  TRUE  TRUE FALSE FALSE  TRUE  TRUE FALSE
 [61]  TRUE  TRUE FALSE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE FALSE  TRUE  TRUE
 [73] FALSE  TRUE  TRUE FALSE  TRUE  TRUE FALSE  TRUE FALSE  TRUE FALSE  TRUE
 [85] FALSE  TRUE  TRUE FALSE FALSE FALSE FALSE  TRUE  TRUE FALSE FALSE  TRUE
 [97] FALSE  TRUE FALSE  TRUE
~~~
每一个 TRUE，表示的即是 NA，而每一个 FALSE，表示这个值是来自标准正态分布的随机值。

在之前的逻辑运算符的讨论中，我们介绍了 `==` 运算符，可以测试两个项目是否相等。因此，我们认为 `my_data == NA` 也能生成和 `is.na()` 一样的运算符。来试一下：
~~~r
> my_data == NA
  [1] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
 [26] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
 [51] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
 [76] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
~~~
我们得到的结果却是一个全是 NA 的向量，这是因为，确切的说 NA 不是一个具体的值，它仅是一个不可用的数据的占位符，因此这个逻辑表达式是不完整的，R 只能返回一个和 `my_data` 长度一致，全是 NA 的向量。

不用担心是否这点让人困惑。关键是任何时候使用逻辑表达式时都要谨慎，因为单个 NA 可能会破环掉整个过程。

回到手头的任务，现在我们有了一个向量 `my_na`，它包含了每个元素是否是 NA 的逻辑值，我们也能计算出数据中有多少个 NA。

诀窍是认识到在表面之下，R 表示数字 1 为真，数字 0 为假。因此，如果我们取一堆真值和假值的和，我们就得到真值的总数。

我们来试一试，在 `my_na` 上调用 `sum()` 函数来计算其真值的数量，也就是 `my_data` 中 NA 的数量。
~~~r
> sum(my_na)
[1] 56
~~~
最后，让我们来看看数据，一切都“合起来”。将 `my_data` 输出到控制台：
~~~r
> my_data
  [1] -0.28801435          NA          NA  0.36748726          NA -1.19633286
  [7]          NA  0.66454307 -1.00578082          NA          NA  1.81471621
 [13]  2.17424903 -1.71439226          NA -0.24878883          NA          NA
 [19]          NA          NA          NA          NA -1.02406254          NA
 [25]          NA -1.15460010 -0.39492211  0.26074351          NA          NA
 [31]          NA -2.90531031          NA -2.33724788          NA -1.13759834
 [37]          NA  0.26175013 -1.52020692          NA          NA          NA
 [43] -2.23693352  0.92138647 -1.46456656          NA          NA          NA
 [49]  1.36848127 -0.41126305 -1.05480267          NA          NA          NA
 [55]          NA -0.32308052  0.37748786          NA          NA  0.52518884
 [61]          NA          NA -0.24748836          NA -1.42900373          NA
 [67]          NA          NA          NA -0.72784037          NA          NA
 [73] -0.39686325          NA          NA -2.35212605          NA          NA
 [79] -0.02377080          NA -0.00310782          NA  1.41064101          NA
 [85] -0.50119562          NA          NA  1.95195002  0.29982655 -1.01580805
 [91] -0.65818903          NA          NA  2.24820040 -0.31970510          NA
 [97] -1.09487594          NA -0.16355008          NA
~~~
## NaN
现在我们已经了解了 NA，让我来看缺失值的第二种类型：NaN，它代表的是非数字(not a number)。要生成 NaN，现在试一下 0 除于 0(正斜杠)：
~~~r
> 0/0
[1] NaN
~~~
我们再做一次，只是为了好玩。在 R 中，Inf 代表的是无穷，如果 Inf 减去 Inf 会发生什么？
~~~r
> Inf - Inf
[1] NaN
~~~