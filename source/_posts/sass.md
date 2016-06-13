title: sass
date: 2016-06-13 11:12:18
categories:
- 前端
- css
tags:
- css
- sass
- compass
- 前端
---

## FAQ
### sass中文注释
在win7系统下，中文注释会出现报错：error style.scss (Line 7: Invalid GBK character "\xE5")

*解决方法*
1.打开这个文件：`C:\Ruby200\lib\ruby\gems\2.0.0\gems\sass-3.4.13\lib\sass\engine.rb`
2.在`require 'sass/supports'`中添加一行`Encoding.default_external = Encoding.find('utf-8')`

