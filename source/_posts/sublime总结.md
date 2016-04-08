title: sublime总结
date: 2016-04-08 10:12:01
categories:
- 工具
- 开发工具
- sublime

tags:
- sublime
- 编辑器
- 开发工具
---

## 序列号
直接输入注册码就可以了
```
----- BEGIN LICENSE -----
Andrew Weber
Single User License
EA7E-855605
813A03DD 5E4AD9E6 6C0EEB94 BC99798F
942194A6 02396E98 E62C9979 4BB979FE
91424C9D A45400BF F6747D88 2FB88078
90F5CC94 1CDC92DC 8457107A F151657B
1D22E383 A997F016 42397640 33F41CFC
E1D0AE85 A0BBD039 0E9C8D55 E1B89D5D
5CDB7036 E56DE1C0 EFCC0840 650CD3A6
B98FC99C 8FAC73EE D2B95564 DF450523
------ END LICENSE ------
```

## 快捷键
       ctrl+p  搜索文件
       esc  退出弹出的窗口
       ctrl+N 新建标签页
       ctrl+shift+N 新建窗口
       ctrl+W 关闭标签页
       ctrl+O 打开当前文件的目录
       ctrl+G跳转到多少行
       ctrl+M 匹配括号
       ctrl+shift+p 打开命令窗口
       ctrl+· 打开控制台，python
       ctrl+F 搜索单文件
       ctrl+shift+F 搜索多文件
       shift+鼠标右键(window)  option+鼠标左键 （mac）  选择多行
       ctrl+； （win）  commad+p 搜索字符
       ctrl+R（win）  commad+R（mac）    查找函数或者类
       ctrl+X  或 ctrl+shift+k 删除一行
       ctrl+KK 删除光标到行尾的字符
       ctrl+k+backspace  删除光标到行头
       ctrl+enter 在当前行之后插入新行
       ctrl+shift+enter 在当前行之前插入行
       ctrl+shift+up或者down  将当前行向上或者向下移动
       ctrl+L 选择一行
       ctrl+D 选择一个单词
       ctrl+【  和ctrl+】 缩进
       ctrl+shift+D 复制一行
       ctrl+/ 注释
       ctrl+shift+/ 多行注释
       ctrl+J  合并多行
       ctrl+shift+v 缩进粘贴

<!--more-->
## 插件
### 插件安装方法：

1. 直接安装
安装Sublime text 2插件很方便，可以直接下载安装包解压缩到Packages目录（菜单->preferences->packages）.

2. 使用Package Control组件安装

	按Ctrl+`调出console,粘贴以下代码到底部命令行并回车：

	```python
import urllib2,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();os.makedirs(ipp) if not os.path.exists(ipp) else None;open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())
    ```
    重启Sublime Text 2。
    如果在Perferences->package settings中看到package control这一项，则安装成功。

3. 用Package Control安装插件的方法：
    按下Ctrl+Shift+P调出命令面板
    输入install 调出 Install Package 选项并回车，然后在列表中选中要安装的插件。


### 常用插件
#### 主题
Theme - Flatland

#### Sublime Prefixr
Prefixr，CSS3 私有前缀自动补全插件，显然也很有用哇

快捷键：
```
ctrl+alt+x
```
![Sublime Prefixr](/images/pimg/Prefixr.jpg "Sublime Prefixr")

#### JS Format
一个JS代码格式化插件。

快捷键：
```
Ctrl+Alt+f
```
#### jQuery
#### coffeescript
#### sass
#### sass build
#### sass snippets
#### compass
#### SublimeOnSaveBuild
#### codeformatter
#### bootstrap 3 snippets

#### MultiEditUtils
[MultiEditUtils](https://sublime.wbond.net/packages/MultiEditUtils)

#### SideBarEnhancements
[SideBarEnhancements](https://sublime.wbond.net/packages/SideBarEnhancements)

#### Base16 Color Schemes
[Base16 Color Schemes](https://sublime.wbond.net/packages/Base16%20Color%20Schemes)



#### Sublime Alignment
用于代码格式的自动对齐。传说最新版Sublime 已经集成。

![Sublime Alignment](/images/pimg/SublimeAlignment.jpg "Sublime Alignment")

#### Sublime CodeIntel
代码自动提示

#### markdown编写
MarkdownEditing：https://github.com/SublimeText-Markdown/MarkdownEditing

#### Emmet

#### GitGutter

#### SublimeREPL

#### DashDoc

#### CanIUse
CanIUse: https://sublime.wbond.net/packages/Can%20I%20Use

#### Djaneiro

#### phpcs
#### phpintel
#### sublimeint
sublimeint 需要系统有php命令。 所以需要设置好php的环境变量。

#### SublimeLinter
配置
    打开 SublimeLinter 的配置文件，Preferences->Package Settings->SublimeLinter->Settings - User，进行如下配置：

运行模式
"sublimelinter": "save-only",
　　
SublimeLinter 的运行模式，总共有四种，含义分别如下：
    true - 在用户输入时在后台进行即时校验；
    false - 只有在初始化的时候才进行校验；
    "load-save" - 当文件加载和保存的时候进行校验；
    "save-only" - 当文件被保存的时候进行校验；
推荐设置为 “save-only”，这样只在编写完代码，保存的时候才校验，Sublime Text 运行会更加流畅。

校验引擎
这里是配置 JavaScript 和 CSS 校验需要用到的 JS 引擎（这里用的是 Node.js）的安装路径。
```json
"sublimelinter_executable_map":
{
    "javascript":"D:/nodejs/node.exe",
    "css":"D:/nodejs/node.exe"
}
```

JSHint 选项
SublimeLinter 使用 JSHint 作为默认的 JavaScript 校验器，也可以配置为 jslint 和 gjslint（closure linter）。下面我使用的 jshint 校验选项，大家可以根据自己的编码风格自行配置，选项的含义可以参考这里：http://www.jshint.com/docs/#options
```json
"jshint_options":
{
    "strict": true,
    "noarg": true,
    "noempty": true,
    "eqeqeq": true,
    "undef": true,
    "curly": true,
    "forin": true,
    "devel": true,
    "jquery": true,
    "browser": true,
    "wsh": true,
    "evil": true
}
```

CSSLint 选项
SublimeLinter 使用 CSSLint 作为 CSS 的校验器，下面是默认配置的校验选项，可以根据个人编码风格修改：
```json
"csslint_options":
{
    "adjoining-classes": "warning",
    "box-model": true,
    "box-sizing": "warning",
    "compatible-vendor-prefixes": "warning",
    "display-property-grouping": true,
    "duplicate-background-images": "warning",
    "duplicate-properties": true,
    "empty-rules": true,
    "errors": true,
    "fallback-colors": "warning",
    "floats": "warning",
    "font-faces": "warning",
    "font-sizes": "warning",
    "gradients": "warning",
    "ids": "warning",
    "import": "warning",
    "important": "warning",
    "known-properties": true,
    "outline-none": "warning",
    "overqualified-elements": "warning",
    "qualified-headings": "warning",
    "regex-selectors": "warning",
    "rules-count": "warning",
    "shorthand": "warning",
    "star-property-hack": "warning",
    "text-indent": "warning",
    "underscore-property-hack": "warning",
    "unique-headings": "warning",
    "universal-selector": "warning",
    "vendor-prefix": true,
    "zero-units": "warning"
}
```


## FAQ
### 无法调出package control
检查`Preferences -> Settings - User`,查看`Package Control`是否在`ignored_packages`中
