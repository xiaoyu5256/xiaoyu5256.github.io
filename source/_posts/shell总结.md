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
