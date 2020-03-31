# Lesson 10: lapply 和 sapply

## 本课总结

在本课中，您学习了如何使用强大的 `lapply()` 和 `sapply()` 函数对列表的元素进行操作。

1. `lapply()` 对列表应用一个函数，结果为列表。
2. `sapply()` 和 `lapply()` 的用法一样，但是它尝试简化 `lapply()` 的结果，所以结果可能是矩阵或向量。

## 本课详细内容

本节课中，您将学习如何使用 `lapply()` 和 `sapply()` 函数，这是 R 的 `apply()` 函数家族中最重要的两个成员，也称为循环函数。

这些强大的函数以及它们的近亲 ( `vapply()` 和 `tapply()` 等) 提供了一种简洁而方便的方法来实现用于数据分析的`Split-Apply-Combine Strategy`（化整为零的策略）。

每个应用函数将一些数据分割成更小的块，对每个块应用一个函数，然后合并出结果。

在本课中，我们将使用 UCI 机器学习仓库中的国旗数据集。此数据集中包含了各个国家及其国旗的详细信息。更多的信息可以参考： [http://archive.ics.uci.edu/ml/datasets/Flags](http://archive.ics.uci.edu/ml/datasets/Flags)

让我们直接开始，这样你就能对这些特殊的函数是如何工作的有个感觉。

我将数据集存储在一个名为 `flags` 的变量中。键入 `head(flags)` 来预览数据集的前六行。

    > head(flags)
                name landmass zone area population language religion bars stripes colours red green blue gold white black orange
    1    Afghanistan        5    1  648         16       10        2    0       3       5   1     1    0    1     1     1      0
    2        Albania        3    1   29          3        6        6    0       0       3   1     0    0    1     0     1      0
    3        Algeria        4    1 2388         20        8        2    2       0       3   1     1    0    0     1     0      0
    4 American-Samoa        6    3    0          0        1        1    0       0       5   1     0    1    1     1     0      1
    5        Andorra        3    1    0          0        6        0    3       0       3   1     0    1    1     0     0      0
    6         Angola        4    2 1247          7       10        5    0       2       3   1     0    0    1     0     1      0
      mainhue circles crosses saltires quarters sunstars crescent triangle icon animate text topleft botright
    1   green       0       0        0        0        1        0        0    1       0    0   black    green
    2     red       0       0        0        0        1        0        0    0       1    0     red      red
    3   green       0       0        0        0        1        1        0    0       0    0   green    white
    4    blue       0       0        0        0        0        0        1    1       1    0    blue      red
    5    gold       0       0        0        0        0        0        0    0       0    0    blue      red
    6     red       0       0        0        0        1        0        0    1       0    0     red    black

您可能需要向上滚动才能看到所有的输出结果。现在，让我们使用 `dim(flags)` 来检查数据集的维度。

    > dim(flags)
    [1] 194  30

结果告诉我们，有 194 行，或观察值，和 30 列，或变量。每个观察值是一个国家，每个变量描述的是国家或国旗的特征。使用 `viewinfo()` 可以查看数据集的完整描述。

对于任何数据集，我们都想知道变量是以什么格式存储的。换句话说，每个变量是什么类型的。可以使用 `class(flags)` 查看。

    > class(flags)
    [1] "data.frame"

它告诉我们的是这个数据集是作为数据框存储的，这没有回答我们的问题。我们需要的是对每一列都调用 `class()` 函数。虽然我们可以手动完成（即一次一列），但是如果我们能自动化这个过程，速度会快得多。听起来像一个循环！

`lapply()` 函数接收列表作为输入，对列表中的每个元素应用一个函数，然后返回一个与原始列表长度相同的列表。实际上数据框只是向量的列表，所以我们可以使用 `lapply()` 将 `class()` 函数应用到 `flags` 数据集的每一列。我们来看一下它是如何运作的。

键入 `cls_list <- lapply(flags, class)` 来对数据集的每一列应用 `class()` 函数，然后将结果储存在名为 `cls_list` 的变量里。注意，您只需要提供要应用的函数名称，后面不需要括号。

    > cls_list <- lapply(flags, class)
    > cls_list
    $name
    [1] "factor"
    
    $landmass
    [1] "integer"
    
    $zone
    [1] "integer"
    
    $area
    [1] "integer"
    
    $population
    [1] "integer"
    
    $language
    [1] "integer"
    
    $religion
    [1] "integer"
    
    $bars
    [1] "integer"
    
    $stripes
    [1] "integer"
    
    $colours
    [1] "integer"
    
    $red
    [1] "integer"
    
    $green
    [1] "integer"
    
    $blue
    [1] "integer"
    
    $gold
    [1] "integer"
    
    $white
    [1] "integer"
    
    $black
    [1] "integer"
    
    $orange
    [1] "integer"
    
    $mainhue
    [1] "factor"
    
    $circles
    [1] "integer"
    
    $crosses
    [1] "integer"
    
    $saltires
    [1] "integer"
    
    $quarters
    [1] "integer"
    
    $sunstars
    [1] "integer"
    
    $crescent
    [1] "integer"
    
    $triangle
    [1] "integer"
    
    $icon
    [1] "integer"
    
    $animate
    [1] "integer"
    
    $text
    [1] "integer"
    
    $topleft
    [1] "factor"
    
    $botright
    [1] "factor"

`lapply` 中的 `l` 代表的是 `list`。键入 `class(cls_list)` 来确认 `lapply()` 的输出结果为列表。

    > class(cls_list)
    [1] "list"

和意想中的一样，我们得到了一个长度为 30 的列表，每一列或者是每一个变量是一个元素。如果我们可以把它表示成一个向量，而不是一个列表，输出将会更加紧凑。

您可能还记得之前的课程中提到的列表对于存储各个数据类型最有帮助。在这种情况下，由于 `lapply()` 返回的列表中的每个元素都是长度为 1 的字符向量（即"整数"和"向量"），所以 `cls_list` 可以简化为一个字符向量。要手动执行此操作，请键入 `as.character(cls_list)`

    > as.character(cls_list)
     [1] "factor"  "integer" "integer" "integer" "integer" "integer" "integer" "integer" "integer" "integer" "integer" "integer"
    [13] "integer" "integer" "integer" "integer" "integer" "factor"  "integer" "integer" "integer" "integer" "integer" "integer"
    [25] "integer" "integer" "integer" "integer" "factor"  "factor"

`sapply()` 允许您通过在幕后调用 `lapply()` 来自动化这个过程，但是随后尝试为您简化结果（`sapply` 中的 `s` ）。使用 `sapply()` 的方式与使用 `lapply()` 的方式相同，可以获得标记数据集的每一列的类型，并将结果存储在 `cls_vect` 中。

    > cls_vect <- sapply(flags, class)
    > cls_vect
          name   landmass       zone       area population   language   religion       bars    stripes    colours        red 
      "factor"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer"  "integer" 
         green       blue       gold      white      black     orange    mainhue    circles    crosses   saltires   quarters 
     "integer"  "integer"  "integer"  "integer"  "integer"  "integer"   "factor"  "integer"  "integer"  "integer"  "integer" 
      sunstars   crescent   triangle       icon    animate       text    topleft   botright 
     "integer"  "integer"  "integer"  "integer"  "integer"  "integer"   "factor"   "factor"

键入 `class(cls_vect)` 来确认 `sapply()` 返回的是一个字符向量。

    > class(cls_vect)
    [1] "character"

通常情况下，如果结果是每个元素都是长度为 1 的列表，使用 `sapply()` 返回的就是一个向量。如果结果是每个元素都是相同长度(>1)的向量，那么 `sapply()` 返回的是一个矩阵。如果 `sapply()` 无法分辨，那么它返回的就是一个列表，和 `lapply()` 返回的结果并无不同。

让我们再练习 `lapply()` 和 `sapply()` !

数据集的第11-17列是指示变量，每个代表的是一个不同的颜色。如果对应的国家的国旗存在这种颜色，则值为 1，否则为 0。

因此，如果我们想知道数据集中含有某指标的国家数，例如，国旗中包含橘色的国家数，我们可以计算橘色列的所有的 1 和 0 的总和。尝试 `sum(flags$orange)` 来查看结果。

    > sum(flags$orange)
    [1] 26

现在我们想为数据集中每个记录的颜色都重复此操作。

首先，使用 `flag_colors <- flags[, 11:17]` 来提取所有包含颜色的列数据，并存储在名为 `flag_colors` 的数据框中。（注意，在11:17之前的逗号，它的意思是告诉 R，我们想要所有的行，但是仅需11至17列。）

    > flag_colors <- flags[, 11:17]

使用 `head()` 函数来查看 `flag_colors` 的前 6 行数据。

    > head(flag_colors)
      red green blue gold white black orange
    1   1     1    0    1     1     1      0
    2   1     0    0    1     0     1      0
    3   1     1    0    0     1     0      0
    4   1     0    1    1     1     0      1
    5   1     0    1    1     0     0      0
    6   1     0    0    1     0     1      0

为了获得一个包含 `flag_colors` 每一列总和的列表，调用有两个参数的 `lapply()` 函数。第一个参数是需要执行循环的对象（例 `flag_colors`），第二个参数是我们希望应用到每一列的函数（例 `sum`）。记住第二个参数不需要函数名字后面的双括号。

    > lapply(flag_colors, sum)
    $red
    [1] 153
    
    $green
    [1] 91
    
    $blue
    [1] 99
    
    $gold
    [1] 91
    
    $white
    [1] 146
    
    $black
    [1] 52
    
    $orange
    [1] 26

结果告诉我们，在数据集中的194个国旗中，153个包含了红色，91个包含了绿色，等等。

结果是个列表，因此 `lapply()` 返回的也是一个列表。这个列表的每个元素都是长度为 1 的列表，所以结果通过调用 `sapply()` 而不是 `lapply()` 简化成一个向量。现在试一下。

    > sapply(flag_colors, sum)
       red  green   blue   gold  white  black orange 
       153     91     99     91    146     52     26

包含每个颜色的国旗所占的比例，也许是更加重要的信息。因为每一列都是一串 1, 0 数据，每一列的算术平均值就是值为 1 的数据所占的比例。

使用 `sapply()` 调用 `mean()` 函数来计算 `flag_color` 每一列的平均值。记住，第二个参数只列出函数的名字，不加双括号。

    > sapply(flag_colors, mean)
          red     green      blue      gold     white     black    orange 
    0.7886598 0.4690722 0.5103093 0.4690722 0.7525773 0.2680412 0.1340206

在我们目前看到的示例中，`sapply()` 已经将结果简化为一个向量。这是因为 `lapply()` 返回的结果是每个元素都是长度为 1 的向量组成的列表。如果每个元素的长度大于 1，返回的结果应该是个矩阵。

为了阐明这点，让我们提取出数据集的第19-23列，并存储在名为 `flag_shapes` 的数据框里。`flag_shapes <- flags[, 19:23]` 可以完成任务。 

    > flag_shapes <- flags[, 19:23]

每一列（例，变量）代表的是某一特定形状或图案出现在一国国旗上的次数。我们对每个形状或图案出现的最大和最小次数感兴趣。

`range()` 函数可以返回它的第一个参数的最小和最大值，结果应该是一个数字向量。使用 `lapply()` 对 `flag_shapes` 调用 range 函数。

    > lapply(flag_shapes, range)
    $circles
    [1] 0 4
    
    $crosses
    [1] 0 2
    
    $saltires
    [1] 0 1
    
    $quarters
    [1] 0 4
    
    $sunstars
    [1]  0 50

再执行一次相同的操作，但是要使用 `sapply()` 函数，并将结果存储为名称是 `shape_mat` 的变量。

    > shape_mat <- sapply(flag_shapes, range)
    > shape_mat
         circles crosses saltires quarters sunstars
    [1,]       0       0        0        0        0
    [2,]       4       2        1        4       50

使用 `class()` 函数来确认 `shape_mat` 是一个矩阵。

    > class(shape_mat)
    [1] "matrix"

正如我们看到的，`sapply()` 总是尝试简化 `lapply()` 的结果。到目前为止，我们所看到的每个例子都成功做到了这一点。让我们来看一个例子，在这个例子中，`sapply()` 不知道如何简化结果，因此返回一个列表，这与 `lapply()` 没有什么不同。

当给出一个向量时，`unique()` 函数返回一个删除了所有重复元素的向量。换句话说，`unique()` 只返回 `unique` 元素的向量。要查看它时如何工作的，可以试试 `unique(c(3, 4, 5, 5, 5, 6, 6))`。

    > unique(c(3, 4, 5, 5, 5, 6, 6))
    [1] 3 4 5 6

我们想知道在国旗数据集中的每个变量的唯一值。为此，使用 `lapply()` 将 `unique()` 函数应用到国旗数据集中的每一列，并将结果存储在一个名为 `unique_vals` 的变量中。

    > unique_vals <- lapply(flags, unique)
    > unique_vals
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
     [53] El-Salvador              Equatorial-Guinea        Ethiopia                 Faeroes                 
     [57] Falklands-Malvinas       Fiji                     Finland                  France                  
     [61] French-Guiana            French-Polynesia         Gabon                    Gambia                  
     [65] Germany-DDR              Germany-FRG              Ghana                    Gibraltar               
     [69] Greece                   Greenland                Grenada                  Guam                    
     [73] Guatemala                Guinea                   Guinea-Bissau            Guyana                  
     [77] Haiti                    Honduras                 Hong-Kong                Hungary                 
     [81] Iceland                  India                    Indonesia                Iran                    
     [85] Iraq                     Ireland                  Israel                   Italy                   
     [89] Ivory-Coast              Jamaica                  Japan                    Jordan                  
     [93] Kampuchea                Kenya                    Kiribati                 Kuwait                  
     [97] Laos                     Lebanon                  Lesotho                  Liberia                 
    [101] Libya                    Liechtenstein            Luxembourg               Malagasy                
    [105] Malawi                   Malaysia                 Maldive-Islands          Mali                    
    [109] Malta                    Marianas                 Mauritania               Mauritius               
    [113] Mexico                   Micronesia               Monaco                   Mongolia                
    [117] Montserrat               Morocco                  Mozambique               Nauru                   
    [121] Nepal                    Netherlands              Netherlands-Antilles     New-Zealand             
    [125] Nicaragua                Niger                    Nigeria                  Niue                    
    [129] North-Korea              North-Yemen              Norway                   Oman                    
    [133] Pakistan                 Panama                   Papua-New-Guinea         Parguay                 
    [137] Peru                     Philippines              Poland                   Portugal                
    [141] Puerto-Rico              Qatar                    Romania                  Rwanda                  
    [145] San-Marino               Sao-Tome                 Saudi-Arabia             Senegal                 
    [149] Seychelles               Sierra-Leone             Singapore                Soloman-Islands         
    [153] Somalia                  South-Africa             South-Korea              South-Yemen             
    [157] Spain                    Sri-Lanka                St-Helena                St-Kitts-Nevis          
    [161] St-Lucia                 St-Vincent               Sudan                    Surinam                 
    [165] Swaziland                Sweden                   Switzerland              Syria                   
    [169] Taiwan                   Tanzania                 Thailand                 Togo                    
    [173] Tonga                    Trinidad-Tobago          Tunisia                  Turkey                  
    [177] Turks-Cocos-Islands      Tuvalu                   UAE                      Uganda                  
    [181] UK                       Uruguay                  US-Virgin-Isles          USA                     
    [185] USSR                     Vanuatu                  Vatican-City             Venezuela               
    [189] Vietnam                  Western-Samoa            Yugoslavia               Zaire                   
    [193] Zambia                   Zimbabwe                
    194 Levels: Afghanistan Albania Algeria American-Samoa Andorra Angola Anguilla Antigua-Barbuda Argentina Argentine ... Zimbabwe
    
    $landmass
    [1] 5 3 4 6 1 2
    
    $zone
    [1] 1 3 2 4
    
    $area
      [1]   648    29  2388     0  1247  2777  7690    84    19     1   143    31    23   113    47  1099   600  8512     6   111
     [21]   274   678    28   474  9976     4   623  1284   757  9561  1139     2   342    51   115     9   128    43    22    49
     [41]   284  1001    21  1222    12    18   337   547    91   268    10   108   249   239   132  2176   109   246    36   215
     [61]   112    93   103  3268  1904  1648   435    70   301   323    11   372    98   181   583   236    30  1760     3   587
     [81]   118   333  1240  1031  1973  1566   447   783   140    41  1267   925   121   195   324   212   804    76   463   407
    [101]  1285   300   313    92   237    26  2150   196    72   637  1221    99   288   505    66  2506    63    17   450   185
    [121]   945   514    57     5   164   781   245   178  9363 22402    15   912   256   905   753   391
    
    $population
     [1]   16    3   20    0    7   28   15    8   90   10    1    6  119    9   35    4   24    2   11 1008    5   47   31   54
    [25]   17   61   14  684  157   39   57  118   13   77   12   56   18   84   48   36   22   29   38   49   45  231  274   60
    
    $language
     [1] 10  6  8  1  2  4  3  5  7  9
    
    $religion
    [1] 2 6 1 0 5 3 4 7
    
    $bars
    [1] 0 2 3 1 5
    
    $stripes
     [1]  3  0  2  1  5  9 11 14  4  6 13  7
    
    $colours
    [1] 5 3 2 8 6 4 7 1
    
    $red
    [1] 1 0
    
    $green
    [1] 1 0
    
    $blue
    [1] 0 1
    
    $gold
    [1] 1 0
    
    $white
    [1] 1 0
    
    $black
    [1] 1 0
    
    $orange
    [1] 0 1
    
    $mainhue
    [1] green  red    blue   gold   white  orange black  brown 
    Levels: black blue brown gold green orange red white
    
    $circles
    [1] 0 1 4 2
    
    $crosses
    [1] 0 1 2
    
    $saltires
    [1] 0 1
    
    $quarters
    [1] 0 1 4
    
    $sunstars
     [1]  1  0  6 22 14  3  4  5 15 10  7  2  9 50
    
    $crescent
    [1] 0 1
    
    $triangle
    [1] 0 1
    
    $icon
    [1] 1 0
    
    $animate
    [1] 0 1
    
    $text
    [1] 0 1
    
    $topleft
    [1] black  red    green  blue   white  orange gold  
    Levels: black blue gold green orange red white
    
    $botright
    [1] green  red    white  black  blue   gold   orange brown 
    Levels: black blue brown gold green orange red white

因为 unique_vals 是一个列表，你可以使用学到的知识来确定 unique_vals 的每个元素的长度（即每个变量的唯一值的数量）。尽可能简化结果。提示：将 length() 函数应用到 unique_vals 的每个元素中。

    > sapply(unique_vals, length)
          name   landmass       zone       area population   language   religion       bars    stripes    colours        red 
           194          6          4        136         48         10          8          5         12          8          2 
         green       blue       gold      white      black     orange    mainhue    circles    crosses   saltires   quarters 
             2          2          2          2          2          2          8          4          3          2          3 
      sunstars   crescent   triangle       icon    animate       text    topleft   botright 
            14          2          2          2          2          2          7          8

事实上，unique_vals 列表的元素是长度不同的向量，这导致了 sapply() 的一个问题，因为没有明显的方式可以简化结果。

使用 sapply() 应用 unique() 函数到 flags 数据集的每一列，以查看是否得到与 lapply() 相同的未简化列表。

    > sapply(flags, unique)
    $name
      [1] Afghanistan              Albania                  Algeria                  American-Samoa          
      [5] Andorra                  Angola                   Anguilla                 Antigua-Barbuda         
      [9] Argentina                Argentine                Australia                Austria                 
     [13] Bahamas                  Bahrain                  Bangladesh               Barbados                
     [17] Belgium                  Belize                   Benin                    Bermuda                 
     [21] Bhutan                   Bolivia                  Botswana                 Brazil                  
     [25] British-Virgin-Isles     Brunei                   Bulgaria                 Burkina                 
     ...

有时，您可能需要应用一个尚未定义的函数，因此需要编写自己的函数。在 R 中编写函数超出了本课的范围，但是让我们看一个快速的例子。

假设您只对刚刚创建的 `unique_vals` 列表中每个元素的第二个项目刚兴趣。因为 `unique_vals` 列表中的每个元素都是一个向量，我们不知道 R 中是否有内置函数返回向量的第二个元素，所以我们将构件自己的函数。

`lapply(unique_vals, function(elem) elem[2])` 将返回一个包含 `unique_vals` 列表每个元素第二项目的列表。记住，我们的函数只包含一个参数 elem，这是一个匿名函数，依次取 `unique_vals` 的每个元素的值。

    > lapply(unique_vals, function(elem) elem[2])
    $name
    [1] Albania
    194 Levels: Afghanistan Albania Algeria American-Samoa Andorra Angola Anguilla Antigua-Barbuda Argentina Argentine ... Zimbabwe
    
    $landmass
    [1] 3
    
    $zone
    [1] 3
    
    $area
    [1] 29
    
    $population
    [1] 3
    
    $language
    [1] 6
    
    $religion
    [1] 6
    
    $bars
    [1] 2
    
    $stripes
    [1] 0
    
    $colours
    [1] 3
    
    $red
    [1] 0
    
    $green
    [1] 0
    
    $blue
    [1] 1
    
    $gold
    [1] 0
    
    $white
    [1] 0
    
    $black
    [1] 0
    
    $orange
    [1] 1
    
    $mainhue
    [1] red
    Levels: black blue brown gold green orange red white
    
    $circles
    [1] 1
    
    $crosses
    [1] 1
    
    $saltires
    [1] 1
    
    $quarters
    [1] 1
    
    $sunstars
    [1] 0
    
    $crescent
    [1] 1
    
    $triangle
    [1] 1
    
    $icon
    [1] 0
    
    $animate
    [1] 1
    
    $text
    [1] 1
    
    $topleft
    [1] red
    Levels: black blue gold green orange red white
    
    $botright
    [1] red
    Levels: black blue brown gold green orange red white

前面的示例和这个示例之间的唯一区别是，我们在调用 `lapply()` 时定义并使用了自己的函数。我们的函数没有名称，一旦 `lapply()` 使用完毕，他就会消失。当 R 的某个内置函数不可用时，所谓的`匿名函数`就会非常有用。

在本课中，您学习了如何使用强大的 `lapply()` 和 `sapply()` 函数对列表的元素进行操作。下一节课，我们将了解它们的一些近亲函数。