# Lesson 2: 工作空间和文件
在本节课，你将能学到，在 R 语言中如何检查你的当地工作空间，以及开始探索工作空间与机器的文件系统之间的关系。

因为不同操作系统在处理事情比如文件路径等有不同的规定，这些命令的输出结果也可能因机器而异。不管怎么说，重要的是，R 语言为与文件交互提供了一个通用 API.
## getwd()
获取本 R 课程当前运行的目录：
~~~r
> getwd()
[1] "/root"
~~~
## ls()
列出您当前工作空间的所有项目：
~~~r
> ls()
[1] "my"      "my_div"  "my_sqrt" "mydiv"   "x"       "y"       "z"
~~~
对 x 赋值 9
~~~r
> x <- 9
> ls()
[1] "my"      "my_div"  "my_sqrt" "mydiv"   "x"       "y"       "z"
~~~
## list.files() 或 dir()
列出在本工作目录下的所有文件：
~~~r
> list.files()
 [1] "1"                           "anaconda3"
 [3] "gitback.sh"                  "gitback.sh.save"
 [5] "install.sh"                  "JupyterLab"
~~~
使用 `?list.files` 查看更多信息
## args()
查看函数可以采用什么样的参数。

例如，查看 `list.files()` 函数可以使用什么参数：
~~~r
> args(list.files)
function (path = ".", pattern = NULL, all.files = FALSE, full.names = FALSE,
    recursive = FALSE, ignore.case = FALSE, include.dirs = FALSE,
    no.. = FALSE)
NULL
~~~
将当前工作目录赋值给变量 `old.dir`:
~~~r
> old.dir <- getwd()
~~~
我们可以在课程结束时用此变量返回到我们开始的地方。许多像 `getwd()` 这样的询问函数有个非常有用特性，可以返回问题的答案为一个函数的结果。
## dir.create()
在当前工作目录下创建一个名为 `testdir` 的目录。
~~~r
> dir.create("testdir")
~~~
## setwd()
设置当前目录为 `testdir`
~~~r
> setwd("testdir")
~~~
通常，你想设置你的工作目录为某些敏感的地方，也许需要创建你正在运行的特有项目。事实上，组织你的 R 包方面的工作，使用 RStudio 是个明智的选择。下载 Rstudio 在 http://www.rstudio.com/
## file 系类函数
### file.create()
在当前工作目录下创建一个名为 `mytest.R` 的文件：
~~~r
> file.create('mytest.R')
[1] TRUE
# 这个文件就时该工作目录下的唯一的文件，查看一下：
> list.files()
[1] "mytest.R"
~~~
### file.exists()
检查当前工作目录下是否存在 `mytest.R` 文件：
~~~r
> file.exists("mytest.R")
[1] TRUE
~~~
### file.info()
查看文件 `mytest.R` 的信息：
~~~r
> file.info("mytest.R")
         size isdir mode               mtime               ctime
mytest.R    0 FALSE  644 2020-03-09 22:49:36 2020-03-09 22:49:36
                       atime uid gid uname grname
mytest.R 2020-03-09 22:49:36   0   0  root   root
~~~
可以使用 $ 运算符，比如 `file.info('mytest.R')$mode ---` 获得特定的项目
### file.rename()
重命名 `mytest.R` 为 `mytest2.R`
~~~r
> file.rename('mytest.R','mytest2.R')
[1] TRUE
~~~
### file.copy()
备份 `mytest2.R` 为 `mytest3.R`:
~~~r
> file.copy('mytest2.R','mytest3.R')
[1] TRUE
~~~
### file.remove()
移除文件
### file.path()
提供某文件的相对路径：
~~~r
> file.path('mytest3.R')
[1] "mytest3.R"
~~~
可以利用这个特性获得文件夹的相对路径，例如：
~~~r
> file.path("folder1", "folder2")
[1] "folder1/folder2"
~~~
## dir.create()
`?dir.create` 查看帮助文件，注意 `recursive` 参数，为了创建嵌套目录，`recursive` 应设为 `TRUE`.
~~~r
> ?dir.create
files2                  package:base                   R Documentation

Manipulation of Directories and File Permissions

Description:

     These functions provide a low-level interface to the computer's
     file system.

Usage:

     dir.exists(paths)
     dir.create(path, showWarnings = TRUE, recursive = FALSE, mode = "0777")
     Sys.chmod(paths, mode = "0777", use_umask = TRUE)
     Sys.umask(mode = NA)

Arguments:

    path: a character vector containing a single path name.  Tilde
          expansion (see ‘path.expand’) is done.

   paths: character vectors containing file or directory paths.  Tilde
          expansion (see ‘path.expand’) is done.

showWarnings: logical; should the warnings on failure be shown?

recursive: logical. Should elements of the path other than the last be
          created?  If true, like the Unix command ‘mkdir -p’.

    mode: the mode to be used on Unix-alikes: it will be coerced by
          ‘as.octmode’.  For ‘Sys.chmod’ it is recycled along ‘paths’.

use_umask: logical: should the mode be restricted by the ‘umask’
          setting?

Details:
···
~~~
在当前工作目录下，创建一个名为 `testdir2` 的文件夹，以及一个名为 `testdir3` 的子文件夹（这利用到之前提到的 `file.path()` 的特性）。
~~~r
> dir.create(file.path('testdir2', 'testdir3'), recursive = TRUE)
~~~
使用 `setwd()` 函数返回原来的工作目录（可重新调用之前创建的变量 `old.dir`）
~~~r
> setwd(old.dir)
~~~
这对于保存之前的设置，开始一个分析，然后在结束时返回起始位置十分有帮助。
