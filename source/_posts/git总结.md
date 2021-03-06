title: git总结
date: 2016-02-18 08:31:27
categories:
- 工具
- 开发工具
- git
tags:
- git
- 开发工具
- 版本管理
---
### 子模块使用
使用hexo时候，主题和Blog分为2个项目，Blog依赖主题。于是使用子模块分开管理。
#### 添加子模块

```
    cd ~/xiaoyu5256.github.io
    git submodule add git@github.com:xiaoyu5256/hexo-theme-next.git themes/next
```

#### 更新子模块
克隆blog项目，查看目录下的themes/next，这时，themes/next下为空。
执行：
```
    git submodule init
    git submodule update
```
或者
```
git submodule update --init --recursive
```

<!--more-->

>参考
[Git 工具 - 子模块](https://git-scm.com/book/zh/v1/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)

### `git status`显示中文unicode编码问题
执行`git config --global core.quotepath false`
>参考
[Git Status 中文乱码解决](http://blog.crhan.com/2012/09/git-status-%E4%B8%AD%E6%96%87%E4%B9%B1%E7%A0%81%E8%A7%A3%E5%86%B3/)
