# Lesson 12: 查看数据

## 本课总结

本课学习了一些查看数据的函数。

1. `class()` 查看类别。
2. `dim()` 查看维度。
3. `nrow()` 和 `ncol()` 来查看行数和列数。
4. `object.size()` 查看数据集占用多大空间。
5. `names()` 返回列名称（变量）。
6. `head()` 和 `tail()` 预览头部或底部数据，可以加第二个参数。
7. `summary()` 查看数据的简单概述。
8. 最有用和最简洁的函数可能是 `str()`。

## 本课详细内容

无论何时使用新数据集，首先要做的就是查看它！数据的格式是什么？维度是什么？变量的名称是什么？变量是如何存储的？是否存在缺失值？是否存在缺陷？

本节课将叫你如何回答这些问题，并更多地使用 R 的内置函数。我们将使用美国农业部的植物数据库 ([http://plants.usda.gov/adv_search.html](http://plants.usda.gov/adv_search.html))。

我们已经将数据存储在一个名为 `plants` 的变量中。键入 `ls()` 来列出工作空间中的变量，其中应该包括 `plants`。

    > ls()
    [1] "flags"    "ok"       "plants"   "viewinfo"

我们首先使用 `class(plants)` 检查 `plants` 的类别。这将为我们提供有关数据总体结构的线索。

    > class(plants)
    [1] "data.frame"

在数据框中存储数据是很常见的。它是使用 `read.csv()` 和 `read.table()` 等函数将数据读入 R 的默认类，您将在另一节课中了解这些函数。

由于数据集存储在数据框中，因此我们知道它是矩形的。换句话说，它有两个维度（行和列），可以方便地放入表或电子表格中。使用 `dim(plants)` 来查看我们正在处理的行和列的确切数量。

    > dim(plants)
    [1] 5166   10

您看到的第一个数字（5166）是行数（观察值），第二个数字（10）是列数（变量）。

您还可以使用 `nrow(plants)` 来仅查看行数。使用 `ncol(plants)` 来仅查看列数。试一下。

    > nrow(plants)
    [1] 5166
    
    > ncol(plants)
    [1] 10

如果您想知道数据集在内存中占用了多少空间，可以使用 `object.size(plants)`。

    > object.size(plants)
    580840 bytes

现在，我们已经了解了数据集的形状和大小，让我们来了解一下其中的内容。`names(plants)` 将返回一个列（即变量）名称的字符向量。试一试吧。

    > names(plants)
     [1] "Scientific_Name"      "Duration"             "Active_Growth_Period" "Foliage_Color"        "pH_Min"              
     [6] "pH_Max"               "Precip_Min"           "Precip_Max"           "Shade_Tolerance"      "Temp_Min_F"

我们已经对这个数据集应用了相当多描述性的变量名，但情况并不全是这样的。逻辑上说，我们的下一步是查看实际数据。然而，我们的数据集包含了超过 5000 个观察值（行），因此一次查看全部内容是不现实的。

`head()` 函数的作用是预览数据集的顶部数据。用一个参数试一下。

    > head(plants)
                   Scientific_Name          Duration Active_Growth_Period Foliage_Color pH_Min pH_Max Precip_Min Precip_Max
    1                  Abelmoschus              <NA>                 <NA>          <NA>     NA     NA         NA         NA
    2       Abelmoschus esculentus Annual, Perennial                 <NA>          <NA>     NA     NA         NA         NA
    3                        Abies              <NA>                 <NA>          <NA>     NA     NA         NA         NA
    4               Abies balsamea         Perennial    Spring and Summer         Green      4      6         13         60
    5 Abies balsamea var. balsamea         Perennial                 <NA>          <NA>     NA     NA         NA         NA
    6                     Abutilon              <NA>                 <NA>          <NA>     NA     NA         NA         NA
      Shade_Tolerance Temp_Min_F
    1            <NA>         NA
    2            <NA>         NA
    3            <NA>         NA
    4        Tolerant        -43
    5            <NA>         NA
    6            <NA>         NA

花一分钟浏览并理解上面的输出。每一行都以观察号做为标记，每一列都用变量名作为标记。您的屏幕垦鞥不够宽，无法并排查看全部 10 列，在这种情况下，R 在每一行上显示尽可能多的列，然后再继续下一行。

默认情况下，`head()` 显示数据的前六行。您可以通过将希望查看的行数作为第二个参数传递来更改此行为。使用 `head()` 预览前 10 行 `plants`.

    > head(plants, 10)
                         Scientific_Name          Duration Active_Growth_Period Foliage_Color pH_Min pH_Max Precip_Min Precip_Max
    1                        Abelmoschus              <NA>                 <NA>          <NA>     NA     NA         NA         NA
    2             Abelmoschus esculentus Annual, Perennial                 <NA>          <NA>     NA     NA         NA         NA
    3                              Abies              <NA>                 <NA>          <NA>     NA     NA         NA         NA
    4                     Abies balsamea         Perennial    Spring and Summer         Green      4    6.0         13         60
    5       Abies balsamea var. balsamea         Perennial                 <NA>          <NA>     NA     NA         NA         NA
    6                           Abutilon              <NA>                 <NA>          <NA>     NA     NA         NA         NA
    7               Abutilon theophrasti            Annual                 <NA>          <NA>     NA     NA         NA         NA
    8                             Acacia              <NA>                 <NA>          <NA>     NA     NA         NA         NA
    9                  Acacia constricta         Perennial    Spring and Summer         Green      7    8.5          4         20
    10 Acacia constricta var. constricta         Perennial                 <NA>          <NA>     NA     NA         NA         NA
       Shade_Tolerance Temp_Min_F
    1             <NA>         NA
    2             <NA>         NA
    3             <NA>         NA
    4         Tolerant        -43
    5             <NA>         NA
    6             <NA>         NA
    7             <NA>         NA
    8             <NA>         NA
    9       Intolerant        -13
    10            <NA>         NA

同样的，使用 `tail()` 来预览数据集的结尾部分。使用 `tail()` 预览最后 15 行。

    > tail(plants, 15)
                          Scientific_Name  Duration Active_Growth_Period Foliage_Color pH_Min pH_Max Precip_Min Precip_Max
    5152                          Zizania      <NA>                 <NA>          <NA>     NA     NA         NA         NA
    5153                 Zizania aquatica    Annual               Spring         Green    6.4    7.4         30         50
    5154   Zizania aquatica var. aquatica    Annual                 <NA>          <NA>     NA     NA         NA         NA
    5155                Zizania palustris    Annual                 <NA>          <NA>     NA     NA         NA         NA
    5156 Zizania palustris var. palustris    Annual                 <NA>          <NA>     NA     NA         NA         NA
    5157                      Zizaniopsis      <NA>                 <NA>          <NA>     NA     NA         NA         NA
    5158             Zizaniopsis miliacea Perennial    Spring and Summer         Green    4.3    9.0         35         70
    5159                            Zizia      <NA>                 <NA>          <NA>     NA     NA         NA         NA
    5160                     Zizia aptera Perennial                 <NA>          <NA>     NA     NA         NA         NA
    5161                      Zizia aurea Perennial                 <NA>          <NA>     NA     NA         NA         NA
    5162                 Zizia trifoliata Perennial                 <NA>          <NA>     NA     NA         NA         NA
    5163                          Zostera      <NA>                 <NA>          <NA>     NA     NA         NA         NA
    5164                   Zostera marina Perennial                 <NA>          <NA>     NA     NA         NA         NA
    5165                           Zoysia      <NA>                 <NA>          <NA>     NA     NA         NA         NA
    5166                  Zoysia japonica Perennial                 <NA>          <NA>     NA     NA         NA         NA
         Shade_Tolerance Temp_Min_F
    5152            <NA>         NA
    5153      Intolerant         32
    5154            <NA>         NA
    5155            <NA>         NA
    5156            <NA>         NA
    5157            <NA>         NA
    5158      Intolerant         12
    5159            <NA>         NA
    5160            <NA>         NA
    5161            <NA>         NA
    5162            <NA>         NA
    5163            <NA>         NA
    5164            <NA>         NA
    5165            <NA>         NA
    5166            <NA>         NA

在预览了数据的顶部和底部之后，您可能注意到存在许多的 `NA`，它们是 R 中缺失值的占位符。使用 `summary(plants)` 可以更好地了解每个变量是如何分布的，以及数据集缺失了多少。

    > summary(plants)
                         Scientific_Name              Duration              Active_Growth_Period      Foliage_Color 
     Abelmoschus                 :   1   Perennial        :3031   Spring and Summer   : 447      Dark Green  :  82  
     Abelmoschus esculentus      :   1   Annual           : 682   Spring              : 144      Gray-Green  :  25  
     Abies                       :   1   Annual, Perennial: 179   Spring, Summer, Fall:  95      Green       : 692  
     Abies balsamea              :   1   Annual, Biennial :  95   Summer              :  92      Red         :   4  
     Abies balsamea var. balsamea:   1   Biennial         :  57   Summer and Fall     :  24      White-Gray  :   9  
     Abutilon                    :   1   (Other)          :  92   (Other)             :  30      Yellow-Green:  20  
     (Other)                     :5160   NA's             :1030   NA's                :4334      NA's        :4334  
         pH_Min          pH_Max         Precip_Min      Precip_Max         Shade_Tolerance   Temp_Min_F    
     Min.   :3.000   Min.   : 5.100   Min.   : 4.00   Min.   : 16.00   Intermediate: 242   Min.   :-79.00  
     1st Qu.:4.500   1st Qu.: 7.000   1st Qu.:16.75   1st Qu.: 55.00   Intolerant  : 349   1st Qu.:-38.00  
     Median :5.000   Median : 7.300   Median :28.00   Median : 60.00   Tolerant    : 246   Median :-33.00  
     Mean   :4.997   Mean   : 7.344   Mean   :25.57   Mean   : 58.73   NA's        :4329   Mean   :-22.53  
     3rd Qu.:5.500   3rd Qu.: 7.800   3rd Qu.:32.00   3rd Qu.: 60.00                       3rd Qu.:-18.00  
     Max.   :7.000   Max.   :10.000   Max.   :60.00   Max.   :200.00                       Max.   : 52.00  
     NA's   :4327    NA's   :4327     NA's   :4338    NA's   :4338                         NA's   :4328

`summary()` 为每个变量提供不同的输出，具体取决于其类别。对于 `Precip_Min` 等数字数据，`summary()` 展示最小值，第 1 四分位数，中位数，平均值，第 3 四分位数和最大数。这些值帮助我们理解数据是如何分布的。

对于分类变量（在 R 中称为 `因子(factor)` 变量），`summary()` 显示数据中每个值（或 `水平` (level)）出现的次数。例如，`Scientific Name` 的每个值只出现一次，因为它是特定植物所特有的。相反，`Duration` 概述（也是一个因子变量）告诉我们，我们的数据集包含 3031 个多年生植物，682 个一年生植物，等等。

您可以看到，R 通过包含一个名为 `Other` 的包罗万象的类别截断了 `Active_Growth_Period` 的概述。由于它是一个分类/分子变量，我们可以使用 `table(plants$Active_Growth_Period)` 来查看每个值的实际出现次数。

    > table(plants$Active_Growth_Period)
    
    Fall, Winter and Spring                  Spring         Spring and Fall       Spring and Summer    Spring, Summer, Fall 
                         15                     144                      10                     447                      95 
                     Summer         Summer and Fall              Year Round 
                         92                      24                       5

到目前为止，我们介绍的每个函数都有助于您更好地理解数据的结构。然而，我们把最好的留到了最后。

对于理解数据的结构，最有用和最简洁的函数可能是 `str()` （structure）。现在就试试吧。

    > str(plants)
    'data.frame':	5166 obs. of  10 variables:
     $ Scientific_Name     : Factor w/ 5166 levels "Abelmoschus",..: 1 2 3 4 5 6 7 8 9 10 ...
     $ Duration            : Factor w/ 8 levels "Annual", "Annual, Biennial",..: NA 4 NA 7 7 NA 1 NA 7 7 ...
     $ Active_Growth_Period: Factor w/ 8 levels "Fall, Winter and Spring",..: NA NA NA 4 NA NA NA NA 4 NA ...
     $ Foliage_Color       : Factor w/ 6 levels "Dark Green", "Gray-Green",..: NA NA NA 3 NA NA NA NA 3 NA ...
     $ pH_Min              : num  NA NA NA 4 NA NA NA NA 7 NA ...
     $ pH_Max              : num  NA NA NA 6 NA NA NA NA 8.5 NA ...
     $ Precip_Min          : int  NA NA NA 13 NA NA NA NA 4 NA ...
     $ Precip_Max          : int  NA NA NA 60 NA NA NA NA 20 NA ...
     $ Shade_Tolerance     : Factor w/ 3 levels "Intermediate",..: NA NA NA 3 NA NA NA NA 2 NA ...
     $ Temp_Min_F          : int  NA NA NA -43 NA NA NA NA -13 NA ...

`str()` 的美妙之处在于，它结合了您已经看到的其他函数的许多特性，所有这些特性都以简洁和可读的格式呈现。在最上面，它告诉我们 `plants` 的类别是 `data.frame`，它有 5166 个观测值和 10 个变量。然后，它给出每个变量的名称和类别。以及其内容的预览。

`str()` 实际上是一个非常通用的函数，您可以在 R 中对大多数对象使用它。任何时候，如果您想要了解某个对象的结构（数据集、函数等），`str()` 都是一个很好的开始。

在本课中，您学习了如何使用一组简单而有用的函数来感受新数据集的结构和内容。提前花点时间做这件事可以节省您的时间，并在以后的分析中减少您的挫败感。