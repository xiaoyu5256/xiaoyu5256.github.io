
title: Haskell入门
date: 2016-07-21 20:10:08
categories:
- 后端
- Haskell

tags:
- Haskell
- 函数式编程
---

## Haskell入门
[Haskell Platform](http://hackage.haskell.org/platform)

### 基本语法

#### 函数
---
打开编辑器，输入内容，保存为`fun.hs`
```
doubleMe x = x + x
doubleUs x y = doubleMe x + doubleMe y
doubleSmallNumber' x = (if x>100 then x else x+x) + 1
```
打开命令行，执行：
```
ghci> :l fun
ghci> doubleMe 9
9
ghci>doubleUs 10 20
60

ghci>doubleSmallNumber' 10
21
ghci>dou
ghci>doubleSmallNumber' 101
102

```

#### 列表
---
**拼接,使用`++`**

```
ghci>[1,2,3] ++ [4,5,6]
[1,2,3,4,5,6]

ghci>"Hello" ++ " " ++ "World!"
"Hello World!"

ghci>['h','e'] ++ ['l','l','o']
"hello"
```
"Hello"等价于['H','e','l','l','o']

**在头部插入,使用`:`**

```
ghci>'H':"ello"
"Hello"

ghci>5:[6,1,3]
[5,6,1,3]
```
**访问列表，使用`!!`**

```
ghci>"Hello"!!2
'l'
ghci>[1,2,3,4,5]!!2
3

```
```
ghci>5 `elem` [5,4,3,2,1]
True
ghci>6 `elem` [5,4,3,2,1]
False
ghci>elem 5 [5,4,3,2,1]
True
ghci>elem 6 [5,4,3,2,1]
False
```

**比较列表，使用`>`,`<`,`<=`,`>=`**
```
ghci>[3,2,1] > [3,4,1]
False

```

**其他操作**
```
ghci>head [5,4,3,2,1]
5
ghci>tail [5,4,3,2,1]
[4,3,2,1]
ghci>last [5,4,3,2,1]
1
ghci>init [5,4,3,2,1]
[5,4,3,2]
ghci>length [5,4,3,2,1]
5
ghci>null [1,2,3]
False
ghci>null []
True
ghci>reverse [5,4,3,2,1]
[1,2,3,4,5]
ghci>take 3 [5,4,3,2,1]
[5,4,3]
ghci>take 0 [5,4,3,2,1]
[]
ghci>drop 3  [5,4,3,2,1]
[2,1]
ghci>drop 100  [5,4,3,2,1]
[]
ghci>maximum [5,4,3,2,1]
5
ghci>minimum [5,4,3,2,1]
1
ghci>sum [5,4,3,2,1]
15
ghci>product [5,4,3,2,1]
120
ghci>5 `elem` [5,4,3,2,1]
True
ghci>6 `elem` [5,4,3,2,1]
False
ghci>elem 5 [5,4,3,2,1]
True
ghci>elem 6 [5,4,3,2,1]
False

```

#### 区间
---
```
ghci>[1..30]
[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30]

ghci>[1,3..30]
[1,3,5,7,9,11,13,15,17,19,21,23,25,27,29]

ghci>['a'..'f']
"abcdef"

ghci>[2,4..6*2]
[2,4,6,8,10,12]

ghci>take 6 [2,4..]
[2,4,6,8,10,12]

ghci>take 5 (cycle [1,2,3])
[1,2,3,1,2]

ghci>take 5 (repeat 1)
[1,1,1,1,1]

ghci>replicate 4 10
[10,10,10,10]

```

#### 列表推导
---
```
ghci>[x*2|x<-[1..10]]
[2,4,6,8,10,12,14,16,18,20]

ghci>[x*2|x<-[1..10],x>5]
[12,14,16,18,20]

ghci>[x+y|x<-[1,2,3],y<-[100,101,102]]
[101,102,103,102,103,104,103,104,105]

```

#### 元组
---
```
ghci>fst (1,"hello")
1

ghci>snd (1,"hello")
"hello"

ghci>zip [1,2,3] [3,2,1]
[(1,3),(2,2),(3,1)]

ghci>zip [1..] ["a","b","c"]
[(1,"a"),(2,"b"),(3,"c")]

```

#### 模式匹配
---
**简单模式匹配**

```
lucky :: Int -> String
lucky 7 = "LUCKY NUMBER SEVEN"
lucky x = "Default Hello!"
```
由上到下进行匹配，`x`为匹配任何值,执行结果如下：
```
ghci>lucky 7
"LUCKY NUMBER SEVEN"
ghci>lucky 9
"Default Hello!"
```

**阶乘计算：**
```
factorial :: Int -> Int
factorial 0 = 1
factorial n = n*(factorial (n-1))
```
执行结果：
```
ghci>factorial 10
3628800
```

**元组模式匹配**
```
addVector :: (Double,Double) -> (Double,Double) -> (Double,Double)
addVector (x1,y1) (x2,y2) = (x1+x2,y1+y2)
```
执行结果：
```
ghci>addVector (1,2) (3,4)
(4.0,6.0)
```

**列表模式匹配**
```
head' :: [a]->a
head' [] = error "list can't be empty"
head' (x:_) = x
```
执行结果：

```
ghci>head' [1,2,3,4]
1
ghci>head' []
*** Exception: list can't be empty
CallStack (from HasCallStack):
  error, called at fun.hs:18:12 in main:Main

```

**匹配时使用`@`引用全部**
```
firstletter :: String->String
firstletter ""="Empty String,whoops"
firstletter all@(x:_) = "The first letter of " ++ all ++ " is " ++ [x]
```
执行结果：
```
ghci>firstletter "Hello"
"The first letter of Hello is H"
```

**检查参数是否为真**
```
bmiTell::Double->Double->String
bmiTell weight height
  | bmi <= 18.5 = "You're underweight,you emo,you" 
  | bmi <= 25.5 = "Normal"
  | bmi <= 30.0 = "Fat"
  | otherwise = "Whale"
  where bmi = weight/height^2

```
运行结果：
```
ghci>bmiTell 70 1.69
"Normal"
```

**where中模式匹配**
```
initials :: String -> String -> String
initials firstname lastname = [f] ++ "." ++ [l]
    where (f:_) = firstname
          (l:_) = lastname
```
运行结果：
```
ghci>initials "xiaoyu" "cheng"
"x.c"
```

**where中使用函数**
```
calBmi::(Double,Double)->Double
calBmi (w,h) = bmi w h
  where bmi weight height = weight/height^2
```
运行结果：
```
ghci>calBmi (70,1.69)
24.508945765204302
```

**let**
`let`表达式的格式为`let <bindings> in <expressions>`。`let`中绑定的名字仅对`in`部分可见。

* 在局部作用域中定义函数
```
ghci>[let square x = x*x in (square 2,square 3,square 4)]
[(4,9,16)]
```
* 一行中绑定多个名字
```
ghci>(let a=100;b=200;c=300 in a*b*c,let foo="Hey";bar="there" in foo ++ bar)
(6000000,"Heythere")
```
* 从元组中匹配取值
```
ghci>(let (a,b,c) =(1,2,3) in a+b+c)*100
600
```
* 列表推导中使用`let`
```
calcBmis :: [(Double,Double)] -> [Double]
calcBmis xs = [bmi|(w,h)<-xs,let bmi=w/h^2,bmi>25]
```
执行结果:
```
ghci>calcBmis [(70,1.65),(70,1.6)]
[25.71166207529844,27.343749999999996]
```

**case表达式**
> case表达式语法结构：
> case expression of pattern -> result
>                    pattern -> result
>                    pattern -> result

```
describeList :: [a] -> String
describeList ls = "The list is " ++ case ls of [] -> "empty."
                                               [x] -> "a singleton list."
                                               xs -> "a longer list."
```
执行结果：
```
ghci>describeList []
"The list is empty."
```

#### 递归
---
**简单递归**
```
maximum' ::(Ord a)=> [a]->a
maximum' []= error "maximum of empty list"
maximum' [x]=x
maximum' (x:xs)=max x (maximum' xs)
```
执行结果：
```
ghci>maximum [54,2,4,5,6,756,34]
756
```
**replicate**
```
replicate' :: Int -> a -> [a]
replicate' n x
    | n <= 0 = []
    | otherwise = x:replicate' (n-1) x
```
执行结果：
```
ghci>replicate' 10 'x'
"xxxxxxxxxx"
ghci>replicate' 10 8
[8,8,8,8,8,8,8,8,8,8]
```

**take**
```
take' :: (Num i,Ord i) => i ->[a]->[a]
take' n _
    |n<=0 = []
take' _ [] = []
take' n (x:xs)= x:take' (n-1) xs
```
运行结果:
```
ghci>take' 2 [1,23,4,5]
[1,23]
```

**reverse**
```
reverse' :: [a]->[a]
reverse' []=[]
reverse' (x:xs) = reverse' xs ++ [x]
```
运行结果：
```
ghci>reverse' [1,2,3,4,5]
[5,4,3,2,1]
```

**repeat**
```
repeat' :: a->[a]
repeat' a = a:repeat' a
```
执行结果：
```
repeat' :: a->[a]
repeat' a = a:repeat' a
```

**zip**
```
zip' :: [a]->[b]->[(a,b)]
zip' _ [] = []
zip' [] _ = []
zip' (x:xs) (y:ys) = (x,y):zip' xs ys
```
执行结果：
```
ghci>zip [1,2] ['a','b']
[(1,'a'),(2,'b')]
```

**elem**
```
elem' :: (Eq a)=>a->[a]->Bool
elem' a [] = False
elem' a (x:xs)
    | a==x = True
    | otherwise = elem' a xs
```
执行结果：
```
ghci>elem' 1 [2,3,4,1]
True
```

**快速排序**
```
quicksort :: (Ord a) => [a]->[a]
quicksort [] = []
quicksort (x:xs) = 
    let smaller = [a|a<-xs,a<=x]
        larger = [a|a<-xs,a>x]
    in quicksort smaller ++ [x]  ++ quicksort larger
```
执行结果：
```
ghci>quicksort [2,3,4,5,8,67,4,3,6,5,7,6,7,7]
[2,3,3,4,4,5,5,6,6,7,7,7,8,67]
```
