# 4. 使用命令

 - type – 说明怎样解释一个命令名
 - which – 显示会执行哪个可执行程序
 - man – 显示命令手册页
 - apropos – 显示一系列适合的命令
 - info – 显示命令 info
 - whatis – 显示一个命令的简洁描述
 - alias – 创建命令别名

## 命令
1. 是一个可执行程序，就像我们所看到的位于目录/usr/bin 中的文件一样。
这一类程序可以是用诸如 C 和 C++语言写成的程序编译的二进制文件,
也可以是由诸如shell，perl，python，ruby等等脚本语言写成的程序 。

2. 是一个内建于 shell 自身的命令。bash 支持若干命令，内部叫做 shell 内部命令
(builtins)。例如，cd 命令，就是一个 shell 内部命令。

3. 是一个 shell 函数。这些是小规模的 shell 脚本，它们混合到环境变量中。
在后续的章节里，我们将讨论配置环境变量以及书写 shell 函数。但是现在，
仅仅意识到它们的存在就可以了。

4. 是一个命令别名。我们可以定义自己的命令，建立在其它命令之上。

## type － 显示命令的类型
```
type command
```

## which － 显示一个可执行程序的位置
```
[me@linuxbox ~]$ which ls
/bin/ls
```
这个命令只对可执行程序有效，不包括内建命令和命令别名，别名是真正的可执行程序的替代物

## help － 得到 shell 内建命令的帮助文档
```
[me@linuxbox ~]$ help cd
cd: cd [-L|-P] [dir]
Change ...
```
出现在命令语法说明中的方括号，表示可选的项目。一个竖杠字符 表示互斥选项

## --help - 显示用法信息
```
[me@linuxbox ~]$ mkdir --help
Usage: mkdir [OPTION] DIRECTORY...
Create ...
```

## man － 显示程序手册页
```
man program
```
在大多数 Linux 系统中，man 使用 less 工具来显示参考手册

### 手册页的组织形式
<table class="multi">
<thead>
<tr>
<th class="title">章节</th>
<th class="title">内容</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>用户命令</td>
</tr>
<tr>
<td>2</td>
<td>程序接口内核系统调用</td>
</tr>
<tr>
<td>3</td>
<td>C 库函数程序接口</td>
</tr>
<tr>
<td>4</td>
<td>特殊文件，比如说设备结点和驱动程序</td>
</tr>
<tr>
<td>5</td>
<td>文件格式</td>
</tr>
<tr>
<td>6</td>
<td>游戏娱乐，如屏幕保护程序</td>
</tr>
<tr>
<td>7</td>
<td>其他方面</td>
</tr>
<tr>
<td>8</td>
<td>系统管理员命令</td>
</tr>
</tbody>
</table>

```
# 指定章节号
man section search_term
```

## apropos － 显示适当的命令
```
[me@linuxbox ~]$ apropos floppy
create_floppy_devices (8)   - udev callout to create all possible
```
可能搜索参考手册列表，基于某个关键字的匹配项。虽然很粗糙但有时很有用

输出结果每行的第一个字段是手册页的名字，第二个字段展示章节。注意，man 命令加上”-k”选项， 和 apropos 完成一样的功能

## whatis － 显示非常简洁的命令说明
whatis 程序显示匹配特定关键字的手册页的名字和一行命令说明

## info － 显示程序 Info 条目
GNU 项目提供了一个命令程序手册页的替代物，称为”info”。info 内容可通过 info 阅读器 程序读取。info 页是超级链接形式的，和网页很相似

info 程序读取 info 文件，info 文件是树型结构，分化为各个结点，每一个包含一个题目。 info 文件包含超级链接，它可以让你从一个结点跳到另一个结点。一个超级链接可通过 它开头的星号来辨别出来，把光标放在它上面并按下 enter 键，就可以激活它

### info 命令

<table class="multi">
<thead>
<tr>
<th class="title">命令</th>
<th class="title">行为</th>
</tr>
</thead>
<tbody>
<tr>
<td valign="top" width="25%">?</td>
<td valign="top">显示命令帮助</td>
</tr>
<tr>
<td valign="top">PgUp or Backspace</td>
<td valign="top">显示上一页 </td>
</tr>
<tr>
<td valign="top">PgDn or Space</td>
<td valign="top">显示下一页</td>
</tr>
<tr>
<td valign="top">n</td>
<td valign="top">下一个 - 显示下一个结点</td>
</tr>
<tr>
<td valign="top">p</td>
<td valign="top">上一个 - 显示上一个结点</td>
</tr>
<tr>
<td valign="top">u</td>
<td valign="top">Up - 显示当前所显示结点的父结点，通常是个菜单</td>
</tr>
<tr>
<td valign="top">Enter</td>
<td valign="top">激活光标位置下的超级链接</td>
</tr>
<tr>
<td valign="top">q</td>
<td valign="top">退出</td>
</tr>
</tbody>
</table>

### README 和其它程序文档
许多安装在你系统中的软件，都有自己的文档文件，这些文件位于/usr/share/doc 目录下
>zless 可以显示由 gzip 压缩的文本文件的内容

## 用别名（alias）创建你自己的命令
```
alias foo="cd /usr; ls; cd -"
unalias foo
```
>在命令行中定义别名,当shell会话结束时,它们会消失