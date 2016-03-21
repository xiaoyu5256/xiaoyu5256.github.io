title: vim总结
date: 2016-02-19 17:12:36
categories:
- 工具
- 开发工具
- vim

tags:
- vim
- 编辑器
- 开发工具

---

记录vim使用过程中遇到的问题和小tip。

## 安装
### centos升级vim
```
rpm -qa | grep vim  
yum remove vim vim-enhanced vim-common vim-minimal
yum -y install vim*
```

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
## 键盘映射
### 将 CapsLock 键映射成 Esc键
1.linux系统下,在`~/.profile`文件里添加
```
xmodmap -e 'clear Lock' -e 'keycode 0x42 = Escape'
```
2.windows系统下,将下面代码保存为 capslock2esc.reg：
```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]
"Scancode Map"=hex:00,00,00,00,00,00,00,00,03,00,00,00,3a,00,01,00,01,00,3a,00,00,00,00,00
```
然后双击写入注册表，重启系统。
>参考
[Vim技巧——将 CapsLock 键映射成 Esc键](http://alfred-sun.github.io/blog/2015/02/04/remap-capslock-key/)


## gvim相关
```
" 设置gvim在window下窗口最大化
autocmd GUIEnter * simalt ~x

" gvim nerdtree 切换盘符
:NERDTREE D:\

```


