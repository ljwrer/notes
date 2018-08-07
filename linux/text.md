# 文本处理

- cat – 连接文件并且打印到标准输出
- sort – 给文本行排序
- uniq – 报告或者省略重复行
- cut – 从每行中删除文本区域
- paste – 合并文件文本行
- join – 基于某个共享字段来联合两个文件的文本行
- comm – 逐行比较两个有序的文件
- diff – 逐行比较文件
- patch – 给原始文件打补丁
- tr – 翻译或删除字符
- sed – 用于筛选和转换文本的流编辑器
- aspell – 交互式拼写检查器

#### cat

```bash
[me@linuxbox ~]$ cat > foo.txt
    The quick brown fox jumped over the lazy dog.
[me@linuxbox ~]$
-A 选项， 其用来在文本中显示非打印字符
[me@linuxbox ~]$ cat -A foo.txt
^IThe quick brown fox jumped over the lazy dog.       $
[me@linuxbox ~]$
```

```bash
[me@linuxbox ~]$ cat > foo.txt
The quick brown fox


jumped over the lazy dog.
# -n，其给文本行添加行号
# -s， 禁止输出多个空白行
[me@linuxbox ~]$ cat -ns foo.txt
1   The quick brown fox
2
3   jumped over the lazy dog.
[me@linuxbox ~]$
```

#### sort

 ```bash
[me@linuxbox ~]$ sort > foo.txt
c
b
a
[me@linuxbox ~]$ cat foo.txt
a
b
c
 ```

*表21-1: 常见的 sort 程序选项*

| 选项 | 长选项                  | 描述                                                         |
| ---- | ----------------------- | ------------------------------------------------------------ |
| -b   | --ignore-leading-blanks | 默认情况下，对整行进行排序，从每行的第一个字符开始。这个选项导致 sort 程序忽略 每行开头的空格，从第一个非空白字符开始排序。 |
| -f   | --ignore-case           | 让排序不区分大小写。                                         |
| -n   | --numeric-sort          | 基于字符串的数值来排序。使用此选项允许根据数字值执行排序，而不是字母值。 |
| -r   | --reverse               | 按相反顺序排序。结果按照降序排列，而不是升序。               |
| -k   | --key=field1[,field2]   | 对从 field1到 field2之间的字符排序，而不是整个文本行。看下面的讨论。 |
| -m   | --merge                 | 把每个参数看作是一个预先排好序的文件。把多个文件合并成一个排好序的文件，而没有执行额外的排序。 |
| -o   | --output=file           | 把排好序的输出结果发送到文件，而不是标准输出。               |
| -t   | --field-separator=char  | 定义域分隔字符。默认情况下，域由空格或制表符分隔。           |

>du 命令可以 确定最大的磁盘空间用户

```bash
# 列出的输出结果按照路径名来排序
[me@linuxbox ~]$ du -s /usr/share/* | head
252     /usr/share/aclocal
96      /usr/share/acpi-support
8       /usr/share/adduser
196     /usr/share/alacarte
344     /usr/share/alsa
8       /usr/share/alsa-base
12488   /usr/share/anthy
8       /usr/share/apmd
21440   /usr/share/app-install
48      /usr/share/application-registry
```

```bash
# 使用此 -nr 选项，产生了一个反向的数值排序，最大数值排列在第一位 
[me@linuxbox ~]$ du -s /usr/share/* | sort -nr | head
509940         /usr/share/locale-langpack
242660         /usr/share/doc
197560         /usr/share/fonts
179144         /usr/share/gnome
146764         /usr/share/myspell
144304         /usr/share/gimp
135880         /usr/share/dict
76508          /usr/share/icons
68072          /usr/share/apps
62844          /usr/share/foomatic
```

> 空白字符（空格和制表符）被当作是字段间的界定符，当执行排序时，界定符会被 包含在字段当中

sort 程序允许多个 -k 选项的实例，所以可以指定多个排序关键值

- -k 1,1 始于并且结束于第一个字段
- -k 2n  第二个字段是排序的键值， 并且按照数值排序
- 这些 选项字母和 sort 程序的全局选项一样：b（忽略开头的空格），n（数值排序），r（逆向排序），等等



```bash
# key 选项允许在字段中指定偏移量
[me@linuxbox ~]$ sort -k 3.7nbr -k 3.1nbr -k 3.4nbr distros.txt
Fedora         10    11/25/2008
Ubuntu         8.10  10/30/2008
SUSE           11.0  06/19/2008
...
```

```bash
# sort 程序提供 了一个 -t 选项来定义分隔符
[me@linuxbox ~]$ sort -t ':' -k 7 /etc/passwd | head
me:x:1001:1001:Myself,,,:/home/me:/bin/bash
root:x:0:0:root:/root:/bin/bash
dhcp:x:101:102::/nonexistent:/bin/false
gdm:x:106:114:Gnome Display Manager:/var/lib/gdm:/bin/false
```

#### uniq

当给定一个 排好序的文件（包括标准输出），uniq 会删除任意重复行，并且把结果发送到标准输出

*常用的 uniq 选项*

| 选项 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| -c   | 输出所有的重复行，并且每行开头显示重复的次数。               |
| -d   | 只输出重复行，而不是特有的文本行。                           |
| -f n | 忽略每行开头的 n 个字段，字段之间由空格分隔，正如 sort 程序中的空格分隔符；然而， 不同于 sort 程序，uniq 没有选项来设置备用的字段分隔符。 |
| -i   | 在比较文本行的时候忽略大小写。                               |
| -s n | 跳过（忽略）每行开头的 n 个字符。                            |
| -u   | 只输出独有的文本行。这是默认的。                             |

### 切片和切块

#### cut

cut 程序被用来从文本行中抽取文本，并把其输出到标准输出。它能够接受多个文件参数或者 标准输入

*cut 程序选择项*

| 选项          | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| -c char_list  | 从文本行中抽取由 char_list 定义的文本。这个列表可能由一个或多个逗号 分隔开的数值区间组成。 |
| -f field_list | 从文本行中抽取一个或多个由 field_list 定义的字段。这个列表可能 包括一个或多个字段，或由逗号分隔开的字段区间。 |
| -d delim_char | 当指定-f 选项之后，使用 delim_char 做为字段分隔符。默认情况下， 字段之间必须由单个 tab 字符分隔开。 |
| --complement  | 抽取整个文本行，除了那些由-c 和／或-f 选项指定的文本。       |

cut 命令最好用来从其它程序产生的文件中 抽取文本，而不是从人们直接输入的文本中抽取

```bash
# 从列表中抽取发行年份，通过展开 此文件，再使用 cut 命令，来抽取从位置 23 开始到行尾的每一个字符
[me@linuxbox ~]$ expand distros.txt | cut -c 23-
```

```bash
# -d 选项，我们能够指定冒号做为字段分隔符
[me@linuxbox ~]$ cut -d ':' -f 1 /etc/passwd | head
```

#### paste

 paste 命令的功能正好与 cut 相反。它会添加一个或多个文本列到文件中

```bash
[me@linuxbox ~]$ paste distros-dates.txt distros-versions.txt
```

#### join

需要共享键值

默认情况下,join 命令使用空白字符做为输入字段的界定符，一个空格作为输出字段 的界定符

```bash
[me@linuxbox ~]$ join distros-key-names.txt distros-key-vernums.txt | head
```

### 比较文本

#### comm

 comm 程序会比较两个文本文件，并且会显示每个文件特有的文本行和共有的文把行

#### diff

类似于 comm 程序，diff 程序被用来监测文件之间的差异

软件开发员经常使用 diff 程序来检查不同程序源码 版本之间的更改

```bash
[me@linuxbox ~]$ diff file1.txt file2.txt
```

*diff 更改命令*

| 改变  | 说明                                                         |
| ----- | ------------------------------------------------------------ |
| r1ar2 | 把第二个文件中位置 r2 处的文件行添加到第一个文件中的 r1 处。 |
| r1cr2 | 用第二个文件中位置 r2 处的文本行更改（替代）位置 r1 处的文本行。 |
| r1dr2 | 删除第一个文件中位置 r1 处的文本行，这些文本行将会出现在第二个文件中位置 r2 处。 |

最流行的两种格式是上下文模式和统一模式。

```bash
使用上下文模式（带上 -c 选项）
[me@linuxbox ~]$ diff -c file1.txt file2.txt
```



diff 上下文模式更改指示符*

| 指示符 | 意思                                                         |
| ------ | ------------------------------------------------------------ |
| blank  | 上下文显示行。它并不表示两个文件之间的差异。                 |
| -      | 删除行。这一行将会出现在第一个文件中，而不是第二个文件内。   |
| +      | 添加行。这一行将会出现在第二个文件内，而不是第一个文件中。   |
| !      | 更改行。将会显示某个文本行的两个版本，每个版本会出现在更改组的各自部分。 |

```bash
# 统一模式相似于上下文模式，但是更加简洁。通过 -u 选项来指定它
[me@linuxbox ~]$ diff -u file1.txt file2.txt
```



*diff 统一模式更改指示符*

| 字符 | 意思                       |
| ---- | -------------------------- |
| 空格 | 两个文件都包含这一行。     |
| -    | 在第一个文件中删除这一行。 |
| +    | 添加这一行到第一个文件中。 |



#### patch

 patch 程序被用来把更改应用到文本文件中。它接受从 diff 程序的输出，并且通常被用来 把较老的文件版本转变为较新的文件版本

1. 一个 diff 文件非常小，与整个源码树的大小相比较而言。
2. 一个 diff 文件简洁地显示了所做的修改，从而允许程序补丁的审阅者能快速地评估它。

```bash
# 准备一个 diff 文件供 patch 程序使用
diff -Naur old_file new_file > diff_file
# 应用它，把旧文件修补成新文件
patch < diff_file
```



### 运行时编辑

#### tr

tr 程序被用来更改字符,是一种基于字符的查找和替换操作

```bash
echo "lowercase letters" | tr a-z A-Z
```

tr 命令接受两个参数：要被转换的字符集以及 相对应的转换后的字符集

字符集可以用三种方式来表示：

1. 一个枚举列表。例如， ABCDEFGHIJKLMNOPQRSTUVWXYZ
2. 一个字符域。例如，A-Z 。注意这种方法有时候面临与其它命令相同的问题，归因于 语系的排序规则，因此应该谨慎使用。
3. POSIX 字符类。例如，[:upper:]

大多数情况下，两个字符集应该长度相同；然而，有可能第一个集合大于第二个，尤其如果我们 想要把多个字符转换为单个字符

```bash
[me@linuxbox ~]$ echo "lowercase letters" | tr [:lower:] A
AAAAAAAAA AAAAAAA
```

```bash
# tr 命令能允许字符从输入流中简单地被删除
tr -d '\r' < dos_file > unix_file
```

```bash
# 使用-s 选项，tr 命令能“挤压”（删除）重复的字符实例
[me@linuxbox ~]$ echo "aaabbbccc" | tr -s ab
abccc
```

#### sed

它对文本流进行编辑，要不是一系列指定的文件， 要不就是标准输入

sed 的工作方式是要不给出单个编辑命令（在命令行中）要不就是包含多个命令的脚本文件名,然后它就按行来执行这些命令

```bash
# 对流文本执行指令 s/front/back/，随后输出“back”。我们也能够把这个命令认为是相似于 vi 中的“替换” （查找和替代）命令
# s搜索 front 替换成 back、
# 斜杠字符做为分隔符。分隔符的选择是随意的
[me@linuxbox ~]$ echo "front" | sed 's/front/back/'
back
```

sed 中的大多数命令之前都会带有一个地址，其指定了输入流中要被编辑的文本行

如果省略了地址， 然后会对输入流的每一行执行编辑命令。最简单的地址形式是一个行号

```bash
# 命令添加地址 1，就导致只对仅有一行文本的输入流的第一行执行替换操作
[me@linuxbox ~]$ echo "front" | sed '1s/front/back/'
back
```

*sed 地址表示法*

| 地址        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| n           | 行号，n 是一个正整数。                                       |
| $           | 最后一行。                                                   |
| /regexp/    | 所有匹配一个 POSIX 基本正则表达式的文本行。注意正则表达式通过 斜杠字符界定。选择性地，这个正则表达式可能由一个备用字符界定，通过\cregexpc 来 指定表达式，这里 c 就是一个备用的字符。 |
| addr1,addr2 | 从 addr1 到 addr2 范围内的文本行，包含地址 addr2 在内。地址可能是上述任意 单独的地址形式。 |
| first~step  | 匹配由数字 first 代表的文本行，然后随后的每个在 step 间隔处的文本行。例如 1~2 是指每个位于偶数行号的文本行，5~5 则指第五行和之后每五行位置的文本行。 |
| addr1,+n    | 匹配地址 addr1 和随后的 n 个文本行。                         |
| addr!       | 匹配所有的文本行，除了 addr 之外，addr 可能是上述任意的地址形式。 |



使用 p 命令， 其就是简单地把匹配的文本行打印出来。然而为了高效，我们必须包含选项 -n（不自动打印选项）， 让 sed 不要默认地打印每一行

```bash
# 打印出一系列的文本行，开始于第一行，直到第五行
[me@linuxbox ~]$ sed -n '1,5p' distros.txt
```



正则表达式

```bash
[me@linuxbox ~]$ sed -n '/SUSE/p' distros.txt
```

由斜杠界定的正则表达式 \/SUSE\/，我们能够孤立出包含它的文本行，和 grep 程序的功能 是相同的

```bash
# 否定上面的操作
[me@linuxbox ~]$ sed -n '/SUSE/!p' distros.txt
```



*sed 基本编辑命令*

| 命令                  | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| =                     | 输出当前的行号。                                             |
| a                     | 在当前行之后追加文本。                                       |
| d                     | 删除当前行。                                                 |
| i                     | 在当前行之前插入文本。                                       |
| p                     | 打印当前行。默认情况下，sed 程序打印每一行，并且只是编辑文件中匹配 指定地址的文本行。通过指定-n 选项，这个默认的行为能够被忽略。 |
| q                     | 退出 sed，不再处理更多的文本行。如果不指定-n 选项，输出当前行。 |
| Q                     | 退出 sed，不再处理更多的文本行。                             |
| s/regexp/replacement/ | 只要找到一个 regexp 匹配项，就替换为 replacement 的内容。 replacement 可能包括特殊字符 &，其等价于由 regexp 匹配的文本。另外， replacement 可能包含序列 \1到 \9，其是 regexp 中相对应的子表达式的内容。更多信息，查看 下面 back references 部分的讨论。在 replacement 末尾的斜杠之后，可以指定一个 可选的标志，来修改 s 命令的行为。 |
| y/set1/set2           | 执行字符转写操作，通过把 set1 中的字符转变为相对应的 set2 中的字符。 注意不同于 tr 程序，sed 要求两个字符集合具有相同的长度。 |

```bash
# 添加 g 标志， 我们能够更改所有的实例
[me@linuxbox ~]$ echo "aaabbbccc" | sed 's/b/B/g'
aaaBBBccc
```

```bash
# 使用-f 选项，也有可能在一个脚本文件中构建更加复杂的命令
[me@linuxbox ~]$ sed -f distros.sed distros.txt
```

> 一个连行符由一个反斜杠字符其后紧跟一个回车符组成。它们之间不允许有空白字符



#### aspell

```bash
aspell check textfile
```