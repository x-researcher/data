# Lesson 3: 数列
这节课，学习在 R 中如何创建数列。

在 R 中，最简单的创建数列的方式是使用 `:` 运算符，输入 `1：20` 查看它是如何运作的：
~~~r
> 1:20
 [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
~~~
它的输出值为 1-20 的所有整数。

我们也能用它创建实数数列。例如：
~~~r
> pi:10
[1] 3.141593 4.141593 5.141593 6.141593 7.141593 8.141593 9.141593
~~~
结果是从 pi 开始，递增间隔为 1 的实数向量。上限 10 是不可能达到的，因为它的下一个数据大于 10.

如果输入 `15:1` 会发生什么？ 试一下：
~~~r
> 15:1
 [1] 15 14 13 12 11 10  9  8  7  6  5  4  3  2  1
~~~
返回的结果为一组递减数列。

学习 R 时，有任何不清楚的函数功能，都可以通过查询 R 自带的帮助文档获得，此处查看一下':'运算符的详细信息：
~~~
> ?':'

     The binary operator ‘:’ has two meanings: for factors ‘a:b’ is
     equivalent to ‘interaction(a, b)’ (but the levels are ordered and
     labelled differently).

     For other arguments ‘from:to’ is equivalent to ‘seq(from, to)’,
     and generates a sequence from ‘from’ to ‘to’ in steps of ‘1’ or
     ‘-1’.  Value ‘to’ will be included if it differs from ‘from’ by an
     integer up to a numeric fuzz of about ‘1e-7’.  Non-numeric
     arguments are coerced internally (hence without dispatching
     methods) to numeric-complex values will have their imaginary parts
     discarded with a warning.

Value:

     For numeric arguments, a numeric vector.  This will be of type
     ‘integer’ if ‘from’ is integer-valued and the result is
     representable in the R integer type, otherwise of type ‘"double"’
     (aka ‘mode’ ‘"numeric"’).

     For factors, an unordered factor with levels labelled as ‘la:lb’
     and ordered lexicographically (that is, ‘lb’ varies fastest).

References:
···
Examples:

     1:4
     pi:6 # real
     6:pi # integer

     f1 <- gl(2, 3); f1
     f2 <- gl(3, 2); f2
     f1:f2 # a factor, the "cross"  f1 x f2
~~~
通常情况下，在创建数列时，我们想获得比 ':' 运算符提供的更多的自由度， `seq()` 函数可以满足我们的目的。

`seq()` 函数最基本的用法和 `:` 运算符一样，例如 `seq(1, 10)`:
~~~r
> seq(1, 20)
 [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
~~~
结果与 `1:20` 一致。如果要创建一个增幅为 0.5，范围在 0-10 的数列，可以使用 `seq(0, 10, by=0.5)`
~~~r
> seq(0, 10, by = 0.5)
 [1]  0.0  0.5  1.0  1.5  2.0  2.5  3.0  3.5  4.0  4.5  5.0  5.5  6.0  6.5  7.0
[16]  7.5  8.0  8.5  9.0  9.5 10.0
~~~
如果我们不关心增幅，只是想获得5-10范围内的30个数字，`seq(5 ,10, length=30)` 可以达到这个目的。尝试一下，并将结果赋值给变量 `my_seq`
~~~r
> my_seq <- seq(5, 10, length=30)
~~~
为了确认 `my_seq` 变量的长度为 30，可以使用 `length()` 函数。
~~~r
> length(my_seq)
[1] 30
~~~
假设我们不知道 `my_seq` 的长度，但是我们想生成一个从1到N的整数数列，N代表的是向量 `my_seq` 的长度。 换句话说，我们想创建一个新的向量 (1,2,3,···)，它的长度与 `my_seq` 相同。

有几种方法可以实现。一种方式是结合 `:` 运算符和 `length()` 函数，就像 `1:length(my_seq)` 这样，我们试一下：
~~~r
> 1:length(my_seq)
 [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
[26] 26 27 28 29 30
~~~
另一种方式是使用 `seq(along.with = my_seq)` 函数：
~~~r
> seq(1, along.with = my_seq)
 [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
[26] 26 27 28 29 30
~~~
不管怎么样，与许多常见任务一样，R 有一个独立的能提供此功能的内置函数，称为 `seq_along()`, 我们试一下：
~~~r
> seq_along(my_seq)
 [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
[26] 26 27 28 29 30
~~~
在 R 语言中，还有几种方式可以解决这个问题。当你成为高级的R语言程序员，你可以设计你自己的函数来解决那些没有更好选择时的问题。在将来的课程中，我们将探索如何编写你自己的函数。

另一个与创建数列相关的函数是 `rep()`, 其含义是 `replicate`, 下面来看一下它的一些用法。

如果我们想要创建一个包含 40个零的向量，可以使用 `rep(0, times = 40)`:
~~~r
> rep(0, times = 40)
 [1] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
[39] 0 0
~~~
如果想创建一个重复 10次 (0, 1, 2)的向量，可以执行 `rep(c(0, 1, 2), times = 10)`.
~~~r
> rep(c(0, 1, 2), times = 10)
 [1] 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2
~~~
如果想创建一个重复10次0，然后重复10次1，最后再重复10次2的向量，可以使用 `each` 参数，试一下 `rep(c(0, 1, 2), each = 10)`:
~~~r
> rep(c(0, 1, 2), each = 10)
 [1] 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2
~~~