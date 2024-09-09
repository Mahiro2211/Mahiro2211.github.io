---
layout: post
title: Vim快捷键记录
categories: [Vim]
description: 相关快捷键记录
keywords: Vim
mermaid: false  
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
topmost: false
---

相关快捷键记录

# 删除指定括号里的内容
di" ---- 删除""中的内容

# 如何注释代码想IDE一样
1. 用插件
vim-commentary：这是一个非常流行的插件，可以轻松地注释和取消注释代码。安装后，你可以使用 gc 命令来注释选中的行。例如：
选择多行后，按 gc 注释这些行。
按 gcc 注释当前行。
2. NERD Commenter：另一个功能强大的注释插件，支持多种语言的注释风格。安装后，你可以使用 <Leader>cc 来注释选中的行，使用 <Leader>cu 来取消注释。

# 回到上次（下次）  光标的位置
1. **使用 `Ctrl + o`**：这个命令可以让你回到之前的光标位置。每按一次 `Ctrl + o`，光标就会回到上一个位置。

2. **使用 `Ctrl + i`**：与 `Ctrl + o` 相对，`Ctrl + i` 可以让你前进到下一个光标位置。这在你多次使用 `Ctrl + o` 后想要返回时非常有用。

3. **使用两个单引号 `''`**：这个命令会将光标移动到上一个位置，但会保持在同一行的列位置。

4. **使用两个反引号 `` ``**：这个命令会将光标移动到上一个位置，并且保持在同一行的确切位置。

5. **使用 `:jumps`**：这个命令可以查看跳转列表，显示你最近的光标位置。你可以通过这个列表选择想要跳转的位置。
# Vim 删除行，向上删除
vim 向上删除 n 行
我们都知道从当前行往下删除 n 行很容易， ndd 就好了。但往上删除呢？ 我以前是不知道的，最近搜索了一下，找到了答案，而且额外学到了其他的技巧，你想听吗（不想听我也会写，哈哈

8dd其实是dd重复8次，容易记忆，但效率很低。正确的姿势是 7dj 或者 d7j。我想你看到这里已经猜到应该怎么向上删除了。如果打算从当前行往上删除n行，你需要 {n-1}dk 或者 d{n-1}k

故事到这里应该结束了，然而.... 试试 V3kd, V4jd，注意是大写的V。你会对vim的强大有更多的理解。

By the way, 如果你要从当前行删除到你知道行号的某一行，举例来说，你要从当前行一直删到第434行，无所谓是向上删还是向下删，用 d434G 就可以了。
字符删除
x            删除光标所在处字符
X            删除光标所在前字符
这里没有什么可注意的地方，但需要说明一下的是

通常情况下，新手一旦着急便会按着x不动，从而达到删除一大块文本的目的

如果是头几天使用还好说，但从长久考虑，你还需要学习下面的删除命令

 

单词删除
dw            删除到下一个单词开头
de            删除到本单词末尾
dE            删除到本单词末尾包括标点在内
db            删除到前一个单词
dB            删除到前一个单词包括标点在内
很明显，d是delete的缩写，而上面的x则是老式的清除意思

这里e表示往前删除一个单词，b表示往后删除一个单词，第一节中移动写的很清楚

要注意的是e b会忽略标点，如don't，它们会把这当做三个单词don、‘ 和 t 来删除

而大写的E B则不会

 

行删除
dd            删除一整行
D d$          删除光标位置到本行结尾
d0            删除光标位置到本行开头
这三种用法是最好理解的

我一开始便说过，删除命令需要配合移动命令才能发挥更多作用

你可以看看第一节内容，然后自己尝试着删除一节或一段内容等

tips：3dd代表删除三行，聪明的你一定早就知道了

<<<<<<< HEAD
=======
# 不移动指针的情况下，向上向下滚动
在 Vim 中，可以使用 `Ctrl + e` 和 `Ctrl + y` 来实现不移动光标的情况下让页面向下滚动一行。[1](https://stackoverflow.com/questions/3458689/how-to-move-screen-without-moving-cursor-in-vim)

要向下滚动多行，可以使用 `n Ctrl + e` 或 `n Ctrl + y`，其中 `n` 代表要滚动的行数。[1](https://stackoverflow.com/questions/3458689/how-to-move-screen-without-moving-cursor-in-vim)

例如，要向下滚动 5 行，可以使用 `5 Ctrl + e` 或 `5 Ctrl + y`。[1](https://stackoverflow.com/questions/3458689/how-to-move-screen-without-moving-cursor-in-vim)

此外，还可以使用 `Ctrl + d` 和 `Ctrl + u` 来实现半页滚动，以及 `Ctrl + f` 和 `Ctrl + b` 来实现整页滚动。[1](https://stackoverflow.com/questions/3458689/how-to-move-screen-without-moving-cursor-in-vim)

需要注意的是，`Ctrl + e` 和 `Ctrl + y` 仅在光标位于屏幕边缘时才会移动光标。[1](https://stackoverflow.com/questions/3458689/how-to-move-screen-without-moving-cursor-in-vim)

# Vim删除到下一个空格
在 Vim 中，你可以使用以下命令直接删除到下一个空格：

* **`dt `**：这个命令会删除从当前光标位置到下一个空格之间的所有字符，但不包括空格本身。[1](https://stackoverflow.com/questions/1607904/vim-deleting-from-current-position-until-a-space)
* **`df `**：这个命令会删除从当前光标位置到下一个空格之间的所有字符，包括空格本身。[1](https://stackoverflow.com/questions/1607904/vim-deleting-from-current-position-until-a-space)

**解释：**

* `d`：代表删除（delete）操作。
* `t`：代表“直到”（till）操作，它会将光标移动到下一个指定字符之前。
* `f`：代表“找到”（find）操作，它会将光标移动到下一个指定字符处。
* ` `：代表空格字符。

**示例：**

假设你有一行文本：

```
This is a line of text.
```

你的光标位于 `is` 的 `i` 上。

* 如果你输入 `dt `，则会删除 `is a`，留下 `This  line of text.`。
* 如果你输入 `df `，则会删除 `is a `，留下 `This line of text.`。
>>>>>>> 0061e2a5af001df7de9f34fc5052db8359c5ffe2

