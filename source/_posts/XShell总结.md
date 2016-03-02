title: XShell总结
date: 2016-03-02 14:04:17
categories:
- 开发工具
- Xshell
tags:
- Xshell
- 开发工具
---
### FAQ
#### XShell 屏幕锁定的恢复方法(Ctrl+Q)
操作XShell过程中很多时间大家会习惯性的按Ctrl+S进行保存。
Ctrl+S在XShell的作用是屏幕锁定，很多朋友会无法操作，会直接把窗口关闭。
解决方法:
快捷键 Ctrl+Q 即能完成解锁!如果不想再遇到，可以在`.bashrc`写上
```
stty –ixon
```
>参考
>[XShell 屏幕锁定的恢复方法(Ctrl+Q)](http://www.cnblogs.com/liangle/p/3173475.html)
