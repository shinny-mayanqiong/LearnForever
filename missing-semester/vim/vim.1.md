# Vim 设计哲学

在编程的时候，你会把大量时间花在阅读/编辑而不是在写代码上。所以， Vim 是一个多模态编辑器： 它对于插入文字和操纵文字有不同的模式。 
Vim 既是可编程的 （可以使用 Vimscript 或者像 Python 一样的其他程序语言）
Vim 的接口本身也是一个程序语言： 键入操作 （以及其助记名） 是命令， 这些命令也是可组合的。

## 编辑模式

Vim 的设计以大多数时间都花在阅读、浏览和进行少量编辑改动为基础，因此它具有多种操作模式：

+ 正常模式：在文件中四处移动光标进行修改
+ 插入模式：插入文本
+ 替换模式：替换文本
+ 可视化（一般，行，块）模式：选中文本块
+ 命令模式：用于执行命令

在不同的操作模式下，键盘敲击的含义也不同。比如，x 在`插入模式`会插入字母x，但是在`正常模式`会删除当前光标所在下的字母，在`可视模式`下则会删除选中文块。

在默认设置下，Vim会在左下角显示当前的模式。Vim启动时的默认模式是正常模式。通常你会把大部分时间花在`正常模式`和`插入模式`。

你可以按下 <ESC> 从任何其他模式返回正常模式。

在正常模式，键入:
* i 进入`插入模式`
* R 进入`替换模式`
* v 进入`可视（一般）模式`， V 进入`可视（行）模式`， <C-v> （Ctrl-V, 有时也写作 ^V）进入`可视（块）模式` 
: 进入`命令模式`

因为你会在使用 Vim 时大量使用 <ESC> 键，可以考虑把大小写锁定键重定义成 <ESC>

## 基本操作

### 插入文本

在正常模式， 键入 i 进入插入模式。 现在 Vim 跟很多其他的编辑器一样， 直到你键入<ESC> 返回正常模式。

### 缓存， 标签页， 窗口

Vim 会维护一系列打开的文件，称为 “缓存”。 一个 Vim 会话包含一系列标签页，每个标签页包含一系列窗口（分隔面板）。每个窗口显示一个缓存。 

跟网页浏览器等其他你熟悉的程序不一样的是，缓存和窗口不是一一对应的关系；窗口只是视角。 一个缓存可以在多个窗口打开，甚至在同一个标签页内的多个窗口打开。

这个功能其实很好用， 比如在查看同一个文件的不同部分的时候。

Vim 默认打开一个标签页，这个标签也包含一个窗口。

### 命令行

在正常模式下键入 : 进入命令行模式。 在键入 : 后，你的光标会立即跳到屏幕下方的命令行。 这个模式有很多功能， 包括打开， 保存， 关闭文件， 以及 退出 Vim。

:q 退出 （关闭窗口）
:w 保存 （写）
:wq 保存然后退出
:e {文件名} 打开要编辑的文件
:ls 显示打开的缓存
:help {标题} 打开帮助文档
:help :w 打开 :w 命令的帮助文档
:help w 打开 w 移动的帮助文档

## Vim 的接口其实是一种编程语言

Vim 最重要的设计思想是 Vim 的界面本身是一个程序语言。 键入操作 （以及他们的助记名） 本身是命令， 这些命令可以组合使用。 这使得移动和编辑更加高效，特别是一旦形成肌肉记忆。

### 移动

多数时候你会在正常模式下，使用移动命令在缓存中导航。在 Vim 里面移动也被成为 “名词”， 因为它们指向文字块。

+ 基本移动: hjkl （左， 下， 上， 右）
+ 词： w （下一个词）， b （词初）， e （词尾）
+ 行： 0 （行初）， ^ （第一个非空格字符）， $ （行尾）
+ 屏幕： H （屏幕首行）， M （屏幕中间）， L （屏幕底部）
+ 翻页： Ctrl-u （上翻）， Ctrl-d （下翻）
+ 文件： gg （文件头）， G （文件尾）
+ 行数： :{行数}<CR> 或者 {行数}G ({行数}为行数)
+ 杂项： % （找到配对，比如括号或者 /* */ 之类的注释对）
+ 查找： f{字符}， t{字符}， F{字符}， T{字符}
  - 查找/到 向前/向后 在本行的{字符}
  -, / ; 用于导航匹配
+ 搜索: /{正则表达式}, n / N 用于导航匹配

### 选择

可视化模式:

+ 可视化
+ 可视化行
+ 可视化块

可以用移动命令来选中。

### 编辑

所有你需要用鼠标做的事， 你现在都可以用键盘：采用编辑命令和移动命令的组合来完成。 这就是 Vim 的界面开始看起来像一个程序语言的时候。

Vim 的编辑命令也被称为 “动词”， 因为动词可以施动于名词。

+ i 进入插入模式; 但是对于操纵/编辑文本，不单想用退格键完成
+ O / o 在之上/之下插入行
+ d{移动命令} 删除 {移动命令}
  - 例如， dw 删除词, d$ 删除到行尾, d0 删除到行头。
+ c{移动命令} 改变 {移动命令}
  - 例如， cw 改变词, 比如 d{移动命令} 再 i
+ x 删除字符 （等同于 dl）
+ s 替换字符 （等同于 xi）
+ 可视化模式 + 操作
  - 选中文字, d 删除 或者 c 改变
+ u 撤销, <C-r> 重做
+ y 复制 / “yank” （其他一些命令比如 d 也会复制）
+ p 粘贴
+ 更多值得学习的: 比如 ~ 改变字符的大小写

### 计数

你可以用一个计数来结合“名词” 和 “动词”， 这会执行指定操作若干次。

3w 向前移动三个词
5j 向下移动5行
7dw 删除7个词

### 修饰语

你可以用修饰语改变 “名词” 的意义。修饰语有 i， 表示 “内部” 或者 “在内“， 和 a， 表示 ”周围“。

ci( 改变当前括号内的内容
ci[ 改变当前方括号内的内容
da' 删除一个单引号字符窗， 包括周围的单引号
