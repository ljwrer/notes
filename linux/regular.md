# 正则表达式

限定 POSIX 标准中描述的正则表达式

### grep

global regular expression print

```bash
[me@linuxbox ~]$ ls /usr/bin | grep zip
$ grep [options] regex [file...]
```

*grep 选项*

| 选项 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| -i   | 忽略大小写。不会区分大小写字符。也可用--ignore-case 来指定。 |
| -v   | 不匹配。通常，grep 程序会打印包含匹配项的文本行。这个选项导致 grep 程序只会打印不包含匹配项的文本行。也可用--invert-match 来指定。 |
| -c   | 打印匹配的数量（或者是不匹配的数目，若指定了-v 选项），而不是文本行本身。 也可用--count 选项来指定。 |
| -l   | 打印包含匹配项的文件名，而不是文本行本身，也可用--files-with-matches 选项来指定。 |
| -L   | 相似于-l 选项，但是只是打印不包含匹配项的文件名。也可用--files-without-match 来指定。 |
| -n   | 在每个匹配行之前打印出其位于文件中的相应行号。也可用--line-number 选项来指定。 |
| -h   | 应用于多文件搜索，不输出文件名。也可用--no-filename 选项来指定。 |



```bash
# 对文件列表执行简单的搜索
[me@linuxbox ~]$ grep bzip dirlist*.txt
```



### 元字符和文本

```bash
^ $ . [ ] { } - ? * + ( ) | \
```

其它所有字符都被认为是原义字符，虽然在个别情况下，反斜杠会被用来创建元序列， 也允许元字符被转义为原义字符，而不是被解释为元字符

> 当 shell 执行展开的时候，许多正则表达式元字符，也是对 shell 有特殊 含义的字符。当我们在命令行中传递包含元字符的正则表达式的时候，把元字符用引号引起来至关重要， 这样可以阻止 shell 试图展开它们

### 任何字符

圆点字符，其被用来匹配任意字符

```bash
# .zip中点代表任意字符
[me@linuxbox ~]$ grep -h '.zip' dirlist*.txt
```

### 锚点

插入符号和美元符号被看作是锚点。这意味着正则表达式 只有在文本行的开头或末尾被找到时，才算发生一次匹配。

```bash
[me@linuxbox ~]$ grep -h '^zip' dirlist*.txt
[me@linuxbox ~]$ grep -h 'zip$' dirlist*.txt
[me@linuxbox ~]$ grep -h '^zip$' dirlist*.txt
$ grep -i '^..j.r$' /usr/share/dict/words
```

> 注意正则表达式‘^$’（行首和行尾之间没有字符）会匹配空行。

### 中括号表达式和字符类

通过使用中括号表达式， 我们也能够从一个指定的字符集合中匹配单个字符

我们能够指定 一个待匹配字符集合（包含在不加中括号的情况下会被解释为元字符的字符）

```bash
# 匹配包含字符串“bzip”或者“gzip”的任意行
[me@linuxbox ~]$ grep -h '[bg]zip' dirlist*.txt
```

> 元字符被放置到中括号里面后会失去了它们的特殊含义。 然而，在两种情况下，会在中括号表达式中使用元字符，并且有着不同的含义。第一个元字符 是插入字符，其被用来表示否定；第二个是连字符字符，其被用来表示一个字符范围

### 否定

中括号表示式中的第一个字符是一个插入字符，则剩余的字符被看作是不会在给定的字符位置出现的字符集合

```bash
[me@linuxbox ~]$ grep -h '[^bg]zip' dirlist*.txt
```

> 插入字符如果是中括号表达式中的第一个字符的时候，才会唤醒否定功能；否则，它会失去 它的特殊含义，变成字符集中的一个普通字符。

### 传统的字符区域

```bash
[me@linuxbox ~]$ grep -h '^[A-Z]' dirlist*.txt
# 匹配包含一个连字符，或一个大写字母“A”，或一个大写字母“Z”的文件名
[me@linuxbox ~]$ grep -h '[-AZ]' dirlist*.txt
```

### POSIX 字符集

ASCII排序

```
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz
```

```
aAbBcCdDeEfFgGhHiIjJkKlLmMnNoOpPqQrRsStTuUvVwWxXyYzZ
```

不同的locale对应不同的排序

```bash
[me@linuxbox ~]$ echo $LANG
en_US.UTF-8
```

*POSIX 字符集*

| 字符集     | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| [:alnum:]  | 字母数字字符。在 ASCII 中，等价于：[A-Za-z0-9]               |
| [:word:]   | 与[:alnum:]相同, 但增加了下划线字符。                        |
| [:alpha:]  | 字母字符。在 ASCII 中，等价于：[A-Za-z]                      |
| [:blank:]  | 包含空格和 tab 字符。                                        |
| [:cntrl:]  | ASCII 的控制码。包含了0到31，和127的 ASCII 字符。            |
| [:digit:]  | 数字0到9                                                     |
| [:graph:]  | 可视字符。在 ASCII 中，它包含33到126的字符。                 |
| [:lower:]  | 小写字母。                                                   |
| [:punct:]  | 标点符号字符。在 ASCII 中，等价于：                          |
| [:print:]  | 可打印的字符。在[:graph:]中的所有字符，再加上空格字符。      |
| [:space:]  | 空白字符，包括空格、tab、回车、换行、vertical tab 和 form feed.在 ASCII 中， 等价于：[ \t\r\n\v\f] |
| [:upper:]  | 大写字母。                                                   |
| [:xdigit:] | 用来表示十六进制数字的字符。在 ASCII 中，等价于：[0-9A-Fa-f] |

> 通过字符集，仍然没有便捷的方法来表达部分区域，比如[A-M]

```bash
#  shell 正在执行路径名展开操作
[me@linuxbox ~]$ ls /usr/sbin/[[:upper:]]*
```

##### 恢复到传统的排列顺序

```bash
#添加到你的.bashrc 文件中
[me@linuxbox ~]$ export LANG=POSIX
```

### POSIX 基本的 Vs.扩展的正则表达式

 POSIX 把正则表达式的实现分成了两类： 基本正则表达式（BRE）和扩展的正则表达式（ERE）

BRE 可以辨别以下元字符：

```
^ $ . [ ] *
```

ERE 添加了以下元字符

```
( ) { } ? + |
```

> 在 BRE 中，字符“(”，“)”，“{”，和 “}”用反斜杠转义后，被看作是元字符, 相反在 ERE 中，在任意元字符之前加上反斜杠会导致其被看作是一个文本字符

 ERE 由 egrep 程序来执行这项操作，但是 GNU 版本的 grep 程序在使用了-E 选项之后也支持扩展的正则表达式

### Alternation

交替，允许从一系列表达式 之间选择匹配项的实用程序

以竖杠线元字符为标记

```bash
[me@linuxbox ~]$ echo "AAA" | grep -E 'AAA|BBB'
AAA
[me@linuxbox ~]$ echo "BBB" | grep -E 'AAA|BBB'
BBB
[me@linuxbox ~]$ echo "CCC" | grep -E 'AAA|BBB'
[me@linuxbox ~]$ echo "AAA" | grep -E 'AAA|BBB|CCC'
AAA
# 可以使用()来分离 alternation
# 匹配以“bz”，或“gz”，或“zip”开头的文件名
[me@linuxbox ~]$ grep -Eh '^(bz|gz|zip)' dirlist*.txt
# 匹配任意以“bz”开头，或包含“gz”，或包含“zip”的文件名
[me@linuxbox ~]$ grep -Eh '^bz|gz|zip' dirlist*.txt
```

### 限定符

扩展的正则表达式支持几种方法，来指定一个元素被匹配的次数。

#### ? - 匹配零个或一个元素

#### * - 匹配零个或多个元素

#### + - 匹配一个或多个元素

#### { } - 匹配特定个数的元素

*指定匹配的数目*

| 限定符 | 意思                                                     |
| ------ | -------------------------------------------------------- |
| {n}    | 匹配前面的元素，如果它确切地出现了 n 次。                |
| {n,m}  | 匹配前面的元素，如果它至少出现了 n 次，但是不多于 m 次。 |
| {n,}   | 匹配前面的元素，如果它出现了 n 次或多于 n 次。           |
| {,m}   | 匹配前面的元素，如果它出现的次数不多于 m 次。            |

```bash
^\(?[0-9][0-9][0-9]\)?  [0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]$
简化为
^\(?[0-9]{3}\)?  [0-9]{3}-[0-9]{4}$
```

### 正则表达式应用

```bash
# 扫描这个文件，查找无效的号码
[me@linuxbox ~]$ grep -Ev '^\([0-9]{3}\) [0-9]{3}-[0-9]{4}$' phonelist.txt
```

#### 用 find 查找丑陋的文件名

```bash
[me@linuxbox ~]$ find . -regex '.*[^-\_./0-9a-zA-Z].*'
```

> 两端使用了.*，来匹配零个或多个字符

#### 用 locate 查找文件

```bash
[me@linuxbox ~]$ locate --regex 'bin/(bz|gz|zip)'
```

#### 在 less 和 vim 中查找文本

less 和 vim 两者享有相同的文本查找方法

按下/按键，然后输入正则表达式，来执行搜索任务

vim 支持基本的正则表达式