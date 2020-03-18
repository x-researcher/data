# Lesson 7: 矩阵和数据框

在本节课中，我们将学习矩阵和数据框。它们都表示”矩形“数据类型，这意味着他们用于存储具有行和列的表格数据。

正如你将看到的，主要区别在于，矩阵只能包含一类数据，而数据框可以包含许多不同类的数据。

让我们使用 `:` 创建一个包含数字 1 到 20 的向量。将结果存储在名为 `my_vector` 的向量中。

    > my_vector <- 1:20
    > my_vector
     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20

`dim()` 函数告诉我们对象的”维数“。如果我们输入 `dim(my_vector)` 会发生什么？试一下。

    > dim(my_vector)
    NULL

显然，这没有多大帮助！因为 `my_vector` 是一个向量，它没有 `dim` 属性，但是我们可以使用 `length()`  函数来得到它的长度。现在试一试。

    > length(my_vector)
    [1] 20

这就是我们想要的答案。但是，如果我们给向量一个 `dim` 属性会发生什么？让我们来试一下。输入 `dim(my_vector) <- c(4, 5)` .

    > dim(my_vector) <- c(4, 5)

如果你觉得最后一条命令有点奇怪，也没关系，它本来就是这样。`dim()` 函数允许你获取或者设置 R 对象的 `dim` 属性。在本例中，我们将值 `c(4, 5)` 赋给向量的 `dim` 属性。

使用 `dim(my_vector)` 来确认我们已经正确地设置了 `dim` 属性。

    > dim(my_vector)
    [1] 4 5

另一种查看的方式是对 `my_vector` 使用 `attributes()` 函数。

    > attributes(my_vector)
    $dim
    [1] 4 5

就像在数学课上一样，当我们处理一个二维对象（比如矩形表）时，第一个是行数，第二个是列数。因此，我们赋予了 `my_vector` 4 行 5 列。

但是，等等！这听起来不再像向量了。是的，现在它是一个矩阵。现在查看向量的内容，看看它是什么样子的。

    > my_vector
         [, 1] [, 2] [, 3] [, 4] [, 5]
    [1,]    1    5    9   13   17
    [2,]    2    6   10   14   18
    [3,]    3    7   11   15   19
    [4,]    4    8   12   16   20

现在，让我们使用 `class()` 函数来确认一下它是否是一个矩阵。输入 `class(my_vector)` 开查看我的意思。

    > class(my_vector)
    [1] "matrix"

当然，`my_vector` 现在是一个矩阵。我们应该储存它用一个新的变量来帮助我们记住它是什么。存储 `my_vector` 的值为新向量 `my_matrix` 。

    > my_matrix <- my_vector

到目前为止我们使用的示例是为了说明一个观点，即矩阵只是一个具有维度属性的原子向量。创建相同矩阵的更直接的方法是使用 `matrix()` 函数。

现在使用 `?` 函数打开 `matrix()` 函数的帮助文件。

    > ?matrix
    matrix                  package:base                   R Documentation
    
    Matrices
    
    Description:
    
         ‘matrix’ creates a matrix from the given set of values.
    
         ‘as.matrix’ attempts to turn its argument into a matrix.
    
         ‘is.matrix’ tests if its argument is a (strict) matrix.
    
    Usage:
    
         matrix(data = NA, nrow = 1, ncol = 1, byrow = FALSE,
                dimnames = NULL)
    
         as.matrix(x, ...)
         ## S3 method for class 'data.frame'
         as.matrix(x, rownames.force = NA, ...)
    
         is.matrix(x)
    ...
    Examples:
    
         is.matrix(as.matrix(1:10))
         ! is.matrix(warpbreaks)  # data.frame, NOT matrix!
         warpbreaks [1:10,]
         as.matrix(warpbreaks [1:10,])  # using as.matrix.data.frame(.) method
    
         ## Example of setting row and column names
         mdat <- matrix(c(1, 2, 3, 11, 12, 13), nrow = 2, ncol = 3, byrow = TRUE,
                        dimnames = list(c("row1", "row2"),
                                        c("C.1", "C.2", "C.3")))
         mdat
    
    
    (END)

现在，查看 `matrix` 函数的文档，看看是否可以通过调用 `matrix()` 函数来创建包含相同数字（1-20）和维数（4 行，5 列）的矩阵。将结果存储在一个名为 `my_matrix2` 的变量中。

    > my_matrix2 <- matrix(1:20, nrow = 4, ncol = 5)

最后，让我们来确认一下 `my_matrix` 和 `my_matrix2` 是否完全一致。 `identical()` 函数将告诉我们两个参数是否相同。试一下。

    > identical(my_matrix, my_matrix2)
    [1] TRUE

现在，假设表格中的数字表示来自临床实验的一些测量值，其中每一行表示一个患者，每一列表示测量的一个变量。

我们可能想要标记这些行，以便我们知道哪些数字属于实验中的哪位病人，一种方法是向矩阵中添加一列，其中包含所有四个人的名字。

让我们从创建一个字符向量开始，其中包含我们的病人的名字：`Bill`、`Gina`、`Kelly` 和 `Sean`。记住，双引号告诉 R 某个东西是字符串。将结果存储在一个名为 `patients` 的变量中。

    > patients <- c("Bill", "Gina", "Kelly", "Sean")

现在我们将使用 `cbind()` 函数合并列。不要担心将结果存储在新变量中。只需调用 `cbind()` 的两个参数，`patients` 和 `my_matrix`。

    > cbind(patients, my_matrix)
         patients
    [1,] "Bill"   "1" "5" "9"  "13" "17"
    [2,] "Gina"   "2" "6" "10" "14" "18"
    [3,] "Kelly"  "3" "7" "11" "15" "19"
    [4,] "Sean"   "4" "8" "12" "16" "20"

我们的结果有点可疑！其将字符向量与我们的数字矩阵相结合，使得所有东西都用双引号括起来。这意味着剩下的是一个字符串矩阵，这可不行。

如果你还记得这堂课的开始，我告诉过你，矩阵只能包含一类数据。因此，当我们试图将字符向量与数字矩阵组合时，R 被迫将数字 `“强迫”` 转换成字符，因此也使用双引号。

这被称为 `“隐性强迫”`，因为我们没有要求它这么做。它只是发生。但是为什么不把病人的名字转换成数字呢我让你们自己思考这个问题。

因此，我们仍然面临一个问题，如何在不破坏数字数据完整性的情况下，表格中包含患者的姓名。试试下面的指令 `my_data <- data.frame(patients, my matrix)`

    > my_data <- data.frame(patients, my_matrix)
    > my_data
      patients X1 X2 X3 X4 X5
    1     Bill  1  5  9 13 17
    2     Gina  2  6 10 14 18
    3    Kelly  3  7 11 15 19
    4     Sean  4  8 12 16 20

看起来 `data.frame()` 函数允许我们将名称的字符向量存储在数字矩阵旁边。这正是我们所希望的。

在后台，`data.frame()` 函数接收任何数量的参数，并返回由原始对象组成的类 `data.frame` 的单个对象。

让我们调用 `class()` 函数来验证新创建的数据框的类别。

    > class(my_data)
    [1] "data.frame"

还可以为数据框架的行和列分配名称，这是确定表格中哪一行值属于哪位患者的另一种可能的方法。

但是，既然我们已经解决了这个问题，那么让我们通过为数据框架的列指定名称来解决另一个问题，这样我们就可以知道每一列代表的度量类型。

因为我们有 6 列（包括病人姓名列），我们需要首先创建一个向量，其包含每一列的一个元素。创建一个包含 `"patient"`, `"age"`, `"weight"`, `"bp"`, `"rating"`, `"test"` 名为 `cnames` 的字符向量。

    > cnames <- c("patient", "age", "weight", "bp", "rating", "test")

现在，使用 `colnames()` 函数为数据框设置 `colnames` 属性。这和本课前面学到 `dim()` 函数的用法相似。

    > colnames(my_data) <- cnames
    > my_data
      patient age weight bp rating test
    1    Bill   1      5  9     13   17
    2    Gina   2      6 10     14   18
    3   Kelly   3      7 11     15   19
    4    Sean   4      8 12     16   20

在本节课中，我们学习了两个非常重要和常见的数据结构：矩阵和数据框，的基础知识。在以后的课程中，我们将学习更多的内容，特别是关于数据框的进阶课题。