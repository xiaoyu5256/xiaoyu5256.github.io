title: python总结
date: 2016-03-24 13:46:17
categories:
- 后端
- python
tags:
- python
- 编程语言
- 总结
---

### pip无法安装PyInstaller
使用`pip install PyInstaller`安装PyInstaller,提示：
```
Could not find any downloads that satisfy the requirement pyinstaller
```
原因是pip版本过低，更新pip版本
```
pip install -U pip setuptools requests
```
