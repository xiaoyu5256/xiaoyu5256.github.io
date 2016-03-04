title: shell总结
date: 2016-02-26 22:35:08
categories:
- 后端
- shell

tags:
- shell
---

## 基本语法
### 变量
#### 赋值
变量赋值时，如果值(字符串)中包含空格，则必须使用单引号或者双引号括起来,如果不包含空格，可不使用单引号或双引号。
>等号两边一定不能出现空格，切记！


```
myvar=Hello
myvar="Hello Wolrd"
myvar='Hello World'
```

#### 使用变量
使用`$`加变量名使用变量，`\$`则无法使用变量，在单引号里的也将无法使用变量。
```
myvar="Hello World"

echo $myvar     #Hello World
echo "$myvar"   #Hello World
echo ${myvar}   #Hello World
echo '$myvar'   #$myvar
echo \$myvar    #$myvar
```

<!--more-->

#### 环境变量

变量|描述
---|---
$HOME| 用户目录
$PATH| 搜索命令的目录
$PS1| 命令提示符
$PS2| 二级命令提示符,通常时`>`
$IFS| 输入域分隔符,通常是空格,制表符,换行
$0| 脚本名字 
$#| 脚本参数个数
$$| shell的进城id

#### 参数变量

变量|描述
---|---
$1,$2,... |脚本参数
`$*` | 以IFS分隔的所有参数
$@| 所有参数,则以空格分隔

>获取参数列表，使用`$@`更好


### 条件判断
使用`test`或者`[]`判断
```
if test -f fred.c
then
    echo It's file
fi
```
或者
```
if [ -f fred.c ]
then
    echo It's file
fi
```

>`if`和`then`如果想放在一行，必须使用`;`分隔。`[`必须和后面内容有空格。


```
if [ -f fred.c ]; then
    echo It's file
fi
```


字符串比较
>注意`=`两侧的空格,没有空格则会整体被当作字符串处理，返回结果永远为`true`,切记！

```
string1 = string2
string1 != string2
-n string           #字符串不为空，则为真
-z string           #字符串为空，则为真
```

算术比较使用`-eq`,`-ne`,`-gt`,`-ge`,`-lt`,`-le`,`!`
```
exp1 -eq exp2
```

文件条件测试
```
-d file     目录为真
-f file     文件为真
-g file     如果文件set-group-id位被设置则为真
-r file     文件可读为真
-s file     文件大小不为0则为真
-u file     如果文件set-user-id位被设置则为真
-w file     文件可读为真
-x file     文件可执行为真
```

`||`,`&&`
```
if ([ 1 = 1 ] || [ 1 = 2 ]) && echo "cond here" && [ 1 -eq 1 ]; then
    echo "here"
fi
```

### 控制结构
#### if
```
if test 1 -eq 1; then
    echo 1=1
elif test 2 -eq 2; then 
    echo 2=2
else
    echo 'else'
fi

```

#### for
```
for file in $(ls f*.sh); do
    echo $file
done
```

#### while
```
echo -n "Enter password "
read passwd
while [ "$passwd" != "screct" ]; do
    read passwd
done
exit 0
```
>如果在条件中，变量不包含在引号中，变量内容恰好没有，就会出错，导致无法比较。因此，在条件中，字符串比较的变量必须包含在引号里。`"$passwd" != "screct"`

#### case
```
echo "choose a fruit "
read fruit

case "$fruit" in
    banana) echo "you select banana";;
    cherry) echo "you select cherry";;
    *) echo "you select a fruit not exist"
esac

```
```
echo -n "if delete is?"
read opt
case "$opt" in
    yes|Yes|YES) echo "done";;
    n*|N*) echo "cancel";;
    [bB]*)
        echo "back"
        echo "goto back"
        ;;
    *) echo "I don't know";;
esac
```

#### 语句块
语句块可以和`&&`或`||`使用
```
echo -n "delete it? "
read isdelete
[ "$isdelete" = "yes" ]&&{
    echo "delete is step1"
    echo "delete is step2"
}
```

#### 函数
函数参数获取和shell获取参数一样。返回值只能返回数字，通过`$?`获取返回值。
```
#!/bin/sh

foo() {
    echo "$1"
    read x
    return $x 
}

foo "I'm input'"
if [ $? = 13 ]
then
    echo "right"
else
    echo "error"
fi
```
>参考
[Shell函数](http://www.runoob.com/linux/linux-shell-func.html)

#### 命令
1.`break;`跳出循环
2.`:`空语句
3.`continue`
4.`.`又名`source`,执行shell会重新创建一个环境，不会调用的环境，`.`表示用调用的环境执行shell
5.`echo -n`不换行，`echo -e`内容转义
6.`eval`求值
7.`exec`会使用`exec`后的命令替换调用的进程
8.`exit 0`表示执行成功，退出
9.`export`导出变量，即在其他的shell中可以读取改变量,设置环境变量
10.`expr`表达式求值，如`x=$(expr $x+1)`
11.`printf`格式化输出字符串，如:`printf "%s %d %c" "hello" 13 'c'`
12.`return`函数返回
13.`set`为shell设置参数变量,`set 2 3 4;echo $1 $3`输出为`2 4`
14.`unset`从环境中删除变量或函数
15.`shift`参数左移
```
while [ "$1" != "" ];do
    echo "$1"
    shift
done
```

16.`trap`信号处理

信号|描述
---|---
HUP(1) |挂起，通常因终端掉线或用户退出引起
INI(2) |中断，按Ctrl+c触发
QUIT(3)|退出，按Ctrl+\触发
ABRT(6)|中止，因严重错误引发
ALRM(14)|报警，通常用来处理超时
TERM(15)|终止，通常在系统关机时触发
如`trap 'rm -f /tmp/my_tmp_file_$$' INT`,在按`<c-c>`触发删除命令，此时默认中止操作不执行。若trap后没有命令，则执行默认操作。

17.`find`
`find / -name test`

选项|描述
---|---
-mount 或-xdev|不搜索挂载的其他文件系统目录
-depth |优先搜索目录内容
-follow|跟随符号链接
-maxdepths N|最多搜索N层目录
-atime N|文件N天之前被最后访问过
-mtime N|文件N天之前被最后修改过,负数表示N天内改动过的
-name pattern|文件名(不包括路径)匹配提供的模式pattern，pattern必须总是用括号括
-newer otherfile|文件比otherfile要新
-type d 或-type f|文件类型为目录或文件
-user username|文件的拥有者是username

`find . \( -name "_*" -o -mtime -1\) ! -type f `

选项|完整选项|描述
---|---|---
!|-not|测试取反
-a|-and|且
-o|-or|或

`find . newer while2 -type f -exec ls -l {} \;`
动作 |描述
---|---
-exec command |执行一个动作，动作必须以`\;`结束
-ok command |与exec类似,每次动作执行需要用户确认,动作必须以`\;`结束
-print |打印文件名
-ls |对当前文件使用命令ls-dils

18.`grep [options] PATTERN [files]`
options
选项 |描述
---|---
-c | 输出匹配的数目，而不是输出匹配的行
-E | 启用扩展表达式
-h | 取消每个输出行的普通前缀，即匹配查询模式的文件名
-i | 忽略大小写
-l | 只列出包含匹配行的文件名，而不输出真正的匹配行
-v | 对匹配取反，即不包含匹配行

`grep -E [a-z]\{10\} word2.txt`:查找长度为10的单词

#### 命令执行
1. 算术扩展
```
x=0
while [ "$x" -ne 10 ];do
    echo "$x"
    x=$((x+1))
done
exit 0
```
2. 参数扩展
```
for image in *.gif;do
    echo ${image%%gif}jpg
done
```
参数扩展 |描述
---|---
${param:-defalt}|如果param为空，它的默认值为default 
${ #param}|param的长度
${param%word}|从尾部开始删除与word匹配最小部分，返回剩余部分
${param%%word}|从尾部开始删除与word匹配最大部分，返回剩余部分
${param%%word}|从头部开始删除与word匹配最小部分，返回剩余部分
${param##word}|从头部开始删除与word匹配最大部分，返回剩余部分
