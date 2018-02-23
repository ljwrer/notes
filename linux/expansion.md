# 6. expansion(展开)

 - echo － 显示一行文本


## （字符）展开
输入一个命令并按下 enter 键，bash 会在执行你的命令之前对输入 的字符完成几个步骤的处理,这背后的的过程叫做（字符）展开。

通过展开， 你输入的字符，在 shell 对它起作用之前，会展开成为别的字符。

```bash
echo *
Desktop Documents ls-output.txt Music Pictures Public Templates Videos
```
shell 在 echo 命 令被执行前把”*“展开成了另外的东西

## 路径名展开
通配符所依赖的工作机制叫做路径名展开
```
[me@linuxbox ~]$ ls
Desktop   ls-output.txt   Pictures   Templates
[me@linuxbox ~]$ echo D*
Desktop  Documents
[me@linuxbox ~]$ echo *s
Documents Pictures Templates Videos
[me@linuxbox ~]$ echo [[:upper:]]*
Desktop Documents Music Pictures Public Templates Videos
[me@linuxbox ~]$ echo /usr/*/share
/usr/kerberos/share  /usr/local/share
```

### 隐藏文件路径名展开
以圆点字符开头的文件名是隐藏文件
```bash
# 不会显示隐藏文件
echo *
# ”.” 和”..”也出现在结果中
echo .*
# 以圆点开头，第二个字符不包含圆点，再包含至少一个字符
echo .[!.]?*
la -A
```

## 波浪线展开
波浪线字符(“~”)有特殊的含义。当它用在一个单词的开头时，它会展开成指定用户的家目录名，如果没有指定用户名，则展开成当前用户的家目录
```bash 
$ echo ~vagrant
/home/vagrant
$ echo ~
/home/vagrant
```

## 算术表达式展开
支持`+ - * / % **(取幂)`
```
$((expression))
$ echo $((2 + 2))
4
```
表达式可以嵌套
```
$ echo $(($((5**2)) * 3))
75
```

## 花括号展开
可以从一个包含花括号的模式中 创建多个文本字符串
```
[me@linuxbox ~]$ echo Front-{A,B,C}-Back
Front-A-Back Front-B-Back Front-C-Back
```
花括号展开模式可能包含一个开头部分叫做报头，一个结尾部分叫做附言。花括号表达式本身可 能包含一个由逗号分开的字符串列表，或者一系列的整数，或者单个的字符串。这种模式不能 嵌入空白字符。
```
[me@linuxbox ~]$ echo Number_{1..5}
Number_1  Number_2  Number_3  Number_4  Number_5
```
花括号展开可以嵌套：
```
[me@linuxbox ~]$ echo a{A{1,2},B{3,4}}b
aA1b aA2b aB3b aB4b
[me@linuxbox Pics]$ mkdir {2007..2009}-0{1..9} {2007..2009}-{10..12}
```

## 参数展开
环境变量
```
[me@linuxbox ~]$ printenv | less
```
查看环境变量
```
[me@linuxbox ~]$ echo $USER
me
```

## 命令替换
命令替换允许我们把一个命令的输出作为一个展开模式来使用：
```
[me@linuxbox ~]$ echo $(ls)
Desktop Documents ls-output.txt Music Pictures Public Templates Videos
[me@linuxbox ~]$ ls -l $(which cp)
-rwxr-xr-x 1 root root 71516 2007-12-05 08:58 /bin/cp
```
也可以使用整个管道线
```
[me@linuxbox ~]$ file $(ls /usr/bin/* | grep zip)
/usr/bin/bunzip2:     symbolic link to `bzip2'
....
# 倒引号``在旧版本可代替$()
```

## 引用
```
# shell 利用单词分割删除掉 echo 命令的参数列表中多余的空格
[me@linuxbox ~]$ echo this is a    test
this is a test
# 参数展开把 $1 的值替换为一个空字符串
[me@linuxbox ~]$ echo The total is $100.00
The total is 00.00
```
shell 提供了一种 叫做引用的机制，来有选择地禁止不需要的展开

### 双引号
如果你把文本放在双引号中， shell 使用的特殊字符，都失去它们的特殊含义，被当作普通字符来看待。 有几个例外： $，\(反斜杠），和 `（倒引号）
```
ls -l "two words.txt"
```
这意味着单词分割、路径名展开、波浪线展开和花括号展开都将失效，然而参数展开、算术展开和命令替换仍然执行
```
[me@linuxbox ~]$ echo "$USER $((2+2)) $(cal)"
me 4    February 2008
Su Mo Tu We Th Fr Sa
....
```
在默认情况下，单词分割机制会在单词中寻找空格，制表符，和换行符，并把它们看作 单词之间的界定符。这意味着无引用的空格，制表符和换行符都不是文本的一部分， 它们只作为分隔符使用
```
# 没有引用的命令替换导致命令行包含38个参数
[me@linuxbox ~]$ echo $(cal)
February 2008 Su Mo Tu We Th Fr Sa 1 2 3 4 5 6 7 8 9 10 11 12 13 14
15 16 17 18 19 20 21 22 23 24 25 26 27 28 29
# 命令行只有一个参数，参数中包括嵌入的空格和换行符
[me@linuxbox ~]$ echo "$(cal)"
February 2008
....
```

### 单引号
如果需要禁止所有的展开，我们要使用单引号
```
[me@linuxbox ~]$ echo text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER
text /home/me/ls-output.txt a b foo 4 me
[me@linuxbox ~]$ echo "text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER"
text ~/*.txt   {a,b} foo 4 me
[me@linuxbox ~]$ echo 'text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER'
text ~/*.txt  {a,b} $(echo foo) $((2+2)) $USER
```

### 转义字符
使用转义字符来消除文件名中一个字符的特殊含义,这些字符包括”$”, “!”, “ “等字符。

#### 反斜杠转义字符序列
> |转义序列|含义
> |\a|响铃（"警告"－导致计算机嘟嘟响）
> |\b|退格符
> |\n|新的一行。在类 Unix 系统中，产生换行。
> |\r|回车符
> |\t|制表符

> sleep命令:休眠

> echo 命令带上 ‘-e’ 选项,能够解释转义序列,你可以把转义序列放在 $' ' 里面