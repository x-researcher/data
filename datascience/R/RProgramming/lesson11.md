# Lesson 11: vapply 和 tapply

## 本课内容总结

1. `vapply()` 明确指定了 `sapply()` 结果的格式，可以作为 `sapply()` 的更安全的替代函数，这在编写自己的函数时非常有用。
2. `tapply()` 根据某个变量的值将数据划分为多个组，然后对每个组应用一个函数。

## 本课详细内容

在上一课中，您学习了 R 的 `apply` 函数家族中两个最基本的成员：`lapply()` 和 `sapply()` 。两者都以一个列表作为输入，对列表中的每个元素应用一个函数，然后返回结果。`lapply()` 总是返回一个列表，而 `sapply()` 则试图简化结果。

本节课，您将学习如何使用 `vapply()` 和 `tapply()`，它们在 `Split-Apply-Combine` 方法中都有特定的用途。为了一致性，我们将使用与 `lapply()` 和 `sapply()` 课程中相同的数据集。

来自 UCI 机器学习存储库的国旗数据集包含各个国家及其国旗的详细信息。更多信息参见： [http://archive.ics.uci.edu/ml/datasets/Flags](http://archive.ics.uci.edu/ml/datasets/Flags) 

我将数据存储在一个名为 `flags` 的变量中。如果已经有一段时间没有学习 `lapply` 和 `sapply` 了，您可能想使用 `dim()`、`head()`、`str()` 和 `summary()` 等函数来重新熟悉数据。让我们开始吧。

正如您在上一课中看到的，`unique()` 函数返回传递给它的对象中包含的唯一值的向量。因此，`sapply(flags, unique)` 返回一个列表，其中包含标示数据集的每一列的唯一值向量。现在再试一次。

    > sapply(flags, unique)
    $name
      [1] Afghanistan              Albania                  Algeria                  American-Samoa          
      [5] Andorra                  Angola                   Anguilla                 Antigua-Barbuda         
      [9] Argentina                Argentine                Australia                Austria                 
     [13] Bahamas                  Bahrain                  Bangladesh               Barbados                
     [17] Belgium                  Belize                   Benin                    Bermuda                 
     [21] Bhutan                   Bolivia                  Botswana                 Brazil                  
     [25] British-Virgin-Isles     Brunei                   Bulgaria                 Burkina                 
     [29] Burma                    Burundi                  Cameroon                 Canada                  
     [33] Cape-Verde-Islands       Cayman-Islands           Central-African-Republic Chad                    
     [37] Chile                    China                    Colombia                 Comorro-Islands         
     [41] Congo                    Cook-Islands             Costa-Rica               Cuba                    
     [45] Cyprus                   Czechoslovakia           Denmark                  Djibouti                
     [49] Dominica                 Dominican-Republic       Ecuador                  Egypt
    ...

`sapply()` 可以尝试简化 `lapply()` 的结果，当以交互方式工作时，这并不是什么大问题，因为您可以立即看到结果并很快地认识到自己的错误。然而，在非交互的情况下（例如，编写自己的函数），错误可能不会被发现，并在以后导致不正确的结果。因为，您可能希望更加小心，这就是 `vapply()` 的用处所在。

`sapply()` 尝试 `猜测` 结果的正确格式，而 `vapply()` 允许您明确地指定它。如果结果与您指定的格式不匹配，`vapply()` 将抛出一个错误，导致操作停止。这可以防止由于从 `sapply()` 获取意外的返回值而导致的代码中的重大问题。

尝试 `vapply(flags, unique, numeric(1))`，它表示您希望结果的每个元素都是长度为 1 的数字向量。由于实际情况并非如此，您将得到一个错误。得到错误后，输入 `ok()` 继续下一个问题。

    > vapply(flags, unique, numberic(1))
    Error in numberic(1) : could not find function "numberic"
    > ok()

回想前面的课程，`sapply(flags, class)` 将返回一个字符向量，其中包含数据集中每一列的类。现在再试一次，看看结果。

    > sapply(flags, class)
          name   landmass       zone       area population   language   religion       bars    stripes    colours        red 
      "factor"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer" 
         green       blue       gold      white      black     orange    mainhue    circles    crosses   saltires   quarters 
     "integer"  "integer"  "integer"  "integer"  "integer"  "integer"   "factor"  "integer"  "integer"  "integer"  "integer" 
      sunstars   crescent   triangle       icon    animate       text    topleft   botright 
     "integer"  "integer"  "integer"  "integer"  "integer"  "integer"   "factor"   "factor"

如果我们希望明确我们期望的结果的格式，我们可以使用 `vapply(flags, class, character(1))`。`character(1)` 参数告诉 R，我们期望 `class` 函数在应用到标记数据集的每一列时返回长度为 1 的字符向量。现在试一试。

    > vapply(flags, class, character(1))
          name   landmass       zone       area population   language   religion       bars    stripes    colours        red 
      "factor"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer" 
         green       blue       gold      white      black     orange    mainhue    circles    crosses   saltires   quarters 
     "integer"  "integer"  "integer"  "integer"  "integer"  "integer"   "factor"  "integer"  "integer"  "integer"  "integer" 
      sunstars   crescent   triangle       icon    animate       text    topleft   botright 
     "integer"  "integer"  "integer"  "integer"  "integer"  "integer"   "factor"   "factor"

注意，由于我们的期望是正确的(即 `character(1)`)，所以 `vapply()` 结果与 `sapply()` 结果相同——`sapply()` 结果是列 `class` 的字符向量。

您可能认为 `vapply()` 比 `sapply()` 更 `安全`，因为它要求您预先指定输出的格式，而不是让 R 来 `猜测` 您想要的格式。此外，对于大型数据集，`vapply()` 可能比 `sapply()` 执行得更快。然而，当交互地(命令行)进行数据分析时，`sapply()` 可以节省一些输入，而且通常已经足够了。

作为一名数据分析师，您通常希望根据某个变量的值将数据分成多个组，然后将一个函数应用于每个组的成员。我们将看到的下一个函数 `tapply()` 正是这样做的。

使用 `?tapply` 调出文档。

数据集中的 `landmass` 变量取 1 到 6 之间的整数值，每个整数值代表世界的不同部分。使用 `table(flags$landmass)` 查看每个部分有多少个国旗或国家。

    > table(flags$landmass)
    
     1  2  3  4  5  6 
    31 17 35 52 39 20

如果一个国家的国旗包含有动画图像(如鹰、树、人手)，则数据集中的 `animate` 变量的值为 1，否则为 0。使用 `table(flags$animate)` 查看有多少个旗帜包含动画图像。

    > table(flags$animate)
    
      0   1 
    155  39

结果告诉我们，39 个国旗包含一个 `animate` 对象，而 155 个国旗不包含 `animate` 对象。

如果你计算一堆 `0` 和 `1` 的算术平均值，你会得到数字 `1` 所占的比例。使用 `tapply(flags$animate, flags$landmass, mean)` 将平均值函数分别应用于 6 个 `landmass` 组中的每个 `animate` 变量，从而给出每个 `landmass` 组中包含动画图像的国旗的比例。

    > tapply(flags$animate, flags$landmass, mean)
            1         2         3         4         5         6 
    0.4193548 0.1764706 0.1142857 0.1346154 0.1538462 0.3000000

第一个 `landmass` 组(`landmass = 1`)对应于北美，它包含了国旗中，动画图像最高的比例(0.4194)。

类似地，我们可以查看 `tapply(flags$population, flags$red, summary)` 中包含或不包含国旗为红色的国家的人口数量的总和(以百万为单位)。

    > tapply(flags$population, flags$red, summary)
    $`0`
       Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
       0.00    0.00    3.00   27.63    9.00  684.00 
    
    $`1`
       Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
        0.0     0.0     4.0    22.1    15.0  1008.0
    
    | What is the median population (in millions) for countries *without* the color red on their flag?
    
    1: 3.0
    2: 22.1
    3: 0.0
    4: 27.6
    5: 4.0
    6: 9.0
    
    Selection: 1

最后，使用相同的方法来查看六个大陆中每个大陆的人口数量的概况。

    > tapply(flags$population, flags$landmass, summary)
    $`1`
       Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
       0.00    0.00    0.00   12.29    4.50  231.00 
    
    $`2`
       Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
       0.00    1.00    6.00   15.71   15.00  119.00 
    
    $`3`
       Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
       0.00    0.00    8.00   13.86   16.00   61.00 
    
    $`4`
       Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
      0.000   1.000   5.000   8.788   9.750  56.000 
    
    $`5`
       Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
       0.00    2.00   10.00   69.18   39.00 1008.00 
    
    $`6`
       Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
       0.00    0.00    0.00   11.30    1.25  157.00
    
    | What is the maximum population (in millions) for the fourth landmass group (Africa)?
    
    1: 157.00
    2: 56.00
    3: 1010.0
    4: 5.00
    5: 119.0
    
    Selection: 2

本节课中，您学习了如何使用 `vapply()` 作为 `sapply()` 的更安全的替代方法，这在编写自己的函数时非常有用。您还学习了如何使用 `tapply()` 根据某个变量的值将数据划分为多个组，然后对每个组应用一个函数。这些功能将为您成为更好的数据分析师提供便利。