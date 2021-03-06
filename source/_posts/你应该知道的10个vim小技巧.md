title: 你应该知道的10个vim小技巧
date: 2016-02-19 15:54:11
categories:
- 工具
- 开发工具
- vim

tags:
- vim
- 编辑器
- 开发工具
- 翻译
---

我想你应该知道下面10条vim的小技巧

## 1. 星号`*`和井号`#`
在normal模式下，可以使用`*`和`#`查找当前光标位置的单词。`*`是向前查找,`#`是向后查找。

## 2. 在任何文档中简单补全
在插入模式使用`<C-n>`和`<C-p>`即可在当前文档中查找单词进行补全。`<C-n>`向后查找单词，`<C-p>`向前查找。

## 3. `.`号
输入`.`重复上一次改动。

## 4. `%`键
在编程中，常需要匹配括号。此时可以使用`%`键在2个括号上来回跳动。

## 5. 使用`==`和`=`对代码做缩进 
对于单行代码缩进,可以在normal模式下，使用`==`缩进代码。
需要缩进多行代码,可以使用visual模式，选择代码，再使用`=`进行缩进。

<!--more-->    

## 6. 撤销和重做
可以使用`u`撤销一次改动，使用`<C-r>`重做上次被撤销的改动,使用`U`将当前行的状态恢复到未改之前。
可以使用`g-`撤销,`g+`重做。如果想把文档状态恢复到一分钟以前,可以使用`:eariler 1m`。

## 7. 增量搜索
当使用增量搜索,当你输入关键字,文档会自动跳到关键字的位置。使用`set incsearch`打开这个设置。

## 8. 高亮搜索匹配
使用高亮匹配可以打开`:set hlsearch`配置,禁用可以使用`:nohlsearch`。

## 9. 当粘贴文本的时候禁用自动缩进
使用`:set pastetoggle=<F3>`配置,即可使用`F3`切换粘贴模式。

## 10. 使用宏来做重复性工作
在normal模式使用`qa`开始录制宏,此时的录制会存放在寄存器a,做完操作,使用`q`退出录制进入normal模式。然后可以使用`@a`开始做重复性工作。

>原文地址
[10 Vim tricks you should know](https://medium.com/hacking-and-gonzo/10-vim-tricks-you-should-know-6393842b3537#.4rm5stbsx)
