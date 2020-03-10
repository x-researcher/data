# 使用 `swirl` 学习 `R` 语言
## 启动 `R`
~~~r
R
~~~
## 安装 `swirl`
~~~r
> install.packages("swirl")
~~~
## 调用、安装课程并启动 `swirl`
~~~r
> library("swirl")
> install_course("Course Name Here")
# 课程列表参考 https://github.com/swirldev/swirl_courses

> swirl()
~~~
后面就是按照提醒一步步操作，先给自己起个名字，然后选择课程。我选择的是 R Programming，最后选择课时。
## `swirl` 的其他命令
可输入 `info()` 查看
~~~r
> info()

| When you are at the R prompt (>):
| -- Typing skip() allows you to skip the current question.
| -- Typing play() lets you experiment with R on your own; swirl will ignore
| what you do...
| -- UNTIL you type nxt() which will regain swirl's attention.
| -- Typing bye() causes swirl to exit. Your progress will be saved.
| -- Typing main() returns you to swirl's main menu.
| -- Typing info() displays these options again.
~~~
