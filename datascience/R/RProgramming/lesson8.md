# Lesson 8: 逻辑

本课是对 R 中的逻辑运算的简单介绍。

在 R 中，有两个逻辑值，也被称为`布尔值`。分别是 `TRUE` 和 `FALSE` 。在 R 中，你可以构造逻辑表达式，其结果是 `TRUE` 或 `FALSE`。本课的许多问题都涉及到逻辑表达式的计算。

## 逻辑运算符

创建逻辑表达式需要逻辑运算符，"+", "-", "*" 和 "/"。我们讨论的第一个逻辑运算符是等于运算，符号是 "=="。接下来使用等号运算符来看一下 TRUE 是否等于 FALSE。

    > TRUE == FALSE
    [1] FALSE

使用等号运算符，键入 `TURE == TRUE` 试试。

    > TRUE == TRUE
    [1] TRUE

就像数学一样，逻辑表达式可以通过括号进行分组，这样整个表达式 `(TRUE == TRUE) == TRUE` 的计算结果就为 `TRUE`.

要测试此属性，请尝试 `(FALSE == TRUE) == FALSE`。

    > (FALSE == TRUE) == FALSE
    [1] TRUE

等号运算符也可以用来比较数字。使用 "==" 看看 6 是否等于 7.

    > 6 == 7
    [1] FALSE

值得庆幸的是，有一些不等式运算符允许我们测试一个值是否小于或大于另一个值。

小于操作符可以判断左边的数字是否小于右边的数字。写一个表达式来测试 6 是否小于 7.

    > 6 < 7
    [1] TRUE

还有一个小于等于操作符 "<="，用于测试左边的数字是否小于或等于右边的数字。编写一个表达式来测试 10 是否小于等于 10.

    > 10 <= 10
    [1] TRUE

当然，也有对应的 大于等于操作符 ">=".

我们将讨论下一个运算符，不等于号 "!="

    > 5 != 7
    [1] TRUE

    # 等于运算符
    > 5 == 7
    [1] FALSE
    
    # 上述表达式的否定式
    > !5 == 7
    [1] TRUE
    
    # 还有其他的运算符，比如 "and" 和 "and and"，运算符为 "&" 和 "&&"。
    > TRUE & TRUE
    [1] TRUE
    > FALSE & FALSE
    [1] FALSE
    > TRUE & c(TRUE, FALSE, FALSE)
    [1]  TRUE FALSE FALSE
    # && 与 & 不同，&& 只与向量的第一个元素做比较
    > TRUE && c(TRUE, FALSE, FALSE)
    [1] TRUE
    # |和||与&和||的用法类似，|是OR的意思。
    > TRUE | c(TRUE, FALSE, FALSE)
    [1] TRUE TRUE TRUE
    > TRUE || c(TRUE, FALSE, FALSE)
    [1] TRUE

逻辑运算符的执行顺序。

    > 5 > 8 || 6 != 8 && 4 > 3.9
    [1] TRUE

从上述结果也可以看出，R的运算逻辑是先执行 &&，再执行 ||。

## 逻辑函数

还有其他函数也可以执行逻辑运算，比如 `isTRUE(6 > 4)`

 

    > isTRUE(6 > 4)
    [1] TRUE

    Which of the following evaluates to TRUE?
    
    1: isTRUE(NA)
    2: isTRUE(!TRUE)
    3: !isTRUE(4 < 3)
    4: isTRUE(3)
    5: !isTRUE(8 != 5)
    
    Selection: 3

`identical()` 函数也可以做逻辑运算。

    > identical('twins', 'twins')
    [1] TRUE

    Which of the following evaluates to TRUE?
    
    1: identical(5 > 4, 3 < 3.1)
    2: !identical(7, 7)
    3: identical('hello', 'Hello')
    4: identical(4, 3.1)
    
    Selection: 4

`xor()` 函数也可执行逻辑运算。

`xor()` 函数的含义是判断两个元素是否互斥，如果两个元素一个为 TRUE，另一个为 FALSE，则 xor() 的返回结果为 TRUE，否则返回结果为 FALSE。

    > xor(5 == 6, !FALSE)
    [1] TRUE

    Which of the following evaluates to FALSE?
    
    1: xor(!isTRUE(TRUE), 6 > -1)
    2: xor(identical(xor, 'xor'), 7 == 7.0)
    3: xor(4 >= 9, 8 != 8.0)
    4: xor(!!TRUE, !!FALSE)
    
    Selection: 3

为了下面几个问题，我们需要创建一个称为 `ints` 的整数向量。

    > ints <- sample(10)
    > ints
     [1]  6  2  3  5  8  9 10  1  4  7

执行逻辑运算 `ints > 5`

    > ints > 5
     [1]  TRUE FALSE FALSE FALSE  TRUE  TRUE  TRUE FALSE FALSE  TRUE

使用 `which()` 函数找出 ints 中大于 7 的元素序号。

    > which(ints > 7)
    [1] 5 6 7

    Which of the following commands would produce the indices of the elements in ints that are less than or equal to 2?
    
    1: which(ints <= 2)
    2: ints <= 2
    3: which(ints < 2)
    4: ints < 2
    
    Selection: 1

使用 `any()` 函数查看 ints 中是否有小于 0 的元素。

    > any(ints < 0)
    [1] FALSE

使用 `all()` 查看 ints 中所有元素都是大于零。

    > all(ints > 0)
    [1] TRUE

    Which of the following evaluates to TRUE?
    
    1: all(c(TRUE, FALSE, TRUE))
    2: any(ints == 10)
    3: any(ints == 2.5)
    4: all(ints == 10)
    
    Selection: 2