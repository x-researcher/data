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

## R Programming 课程目录
- [x] 1. Basic Building Blocks
- [x] 2. Workspace and Files
- [x] 3. Sequences of Numbers
- [x] 4. Vectors
- [x] 5. Missing Values
- [x] 6. Subsetting Vectors
- [x] 7. Matrices and Data Frames
- [ ] 8. Logic
- [ ] 9. Functions
- [ ] 10. lapply and sapply
- [ ] 11. vapply and tapply 
- [ ] 12. Looking at Data
- [ ] 13. Simulation
- [ ] 14. Dates and Times
- [ ] 15. Base Graphics