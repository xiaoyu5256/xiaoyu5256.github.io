title: vim总结
date: 2016-02-19 17:12:36
categories:
- 开发工具
- vim

tags:
- vim
- 编辑器
- 开发工具

---

记录vim使用过程中遇到的问题和小tip。

## 窗口外观

## 文件操作
```
" 跳转当前光标所对应的文档
gf
" 从跳转的文档回退
<C-o>
" 从跳转的文档回退
:bf

```

## gvim相关
```
" 设置gvim在window下窗口最大化
autocmd GUIEnter * simalt ~x

" gvim nerdtree 切换盘符
:NERDTREE D:\

```

