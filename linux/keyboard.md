# 7. 键盘高级操作技巧

 - clear － 清空屏幕
 - history － 显示历史列表内容

## 命令行编辑
Readline库

### 移动光标
#### 光标移动命令
<table class="multi">
<tr>
<th class="title">按键</th>
<th class="title">行动</th>
</tr>
<tr>
<td valign="top" width="25%">Ctrl-a</td>
<td valign="top">移动光标到行首。</td>
</tr>
<tr>
<td valign="top">Ctrl-e</td>
<td valign="top">移动光标到行尾。</td>
</tr>
<tr>
<td valign="top">Ctrl-f</td>
<td valign="top">光标前移一个字符；和右箭头作用一样。</td>
</tr>
<tr>
<td valign="top">Ctrl-b</td>
<td valign="top">光标后移一个字符；和左箭头作用一样。</td>
</tr>
<tr>
<td valign="top">Alt-f</td>
<td valign="top">光标前移一个字。</td>
</tr>
<tr>
<td valign="top">Alt-b</td>
<td valign="top">光标后移一个字。</td>
</tr>
<tr>
<td valign="top">Ctrl-l</td>
<td valign="top">清空屏幕，移动光标到左上角。clear 命令完成同样的工作。</td>
</tr>
</table>

### 修改文本
#### 文本编辑命令
<table class="multi">
<tr>
<th class="title"> 按键</th>
<th class="title"> 行动</th>
</tr>
<tr>
<td valign="top" width="25%">Ctrl-d</td>
<td valign="top"> 删除光标位置的字符。</td>
</tr>
<tr>
<td valign="top">Ctrl-t</td>
<td valign="top"> 光标位置的字符和光标前面的字符互换位置。</td>
</tr>
<tr>
<td valign="top">Alt-t</td>
<td valign="top"> 光标位置的字和其前面的字互换位置。</td>
</tr>
<tr>
<td valign="top">Alt-l</td>
<td valign="top"> 把从光标位置到字尾的字符转换成小写字母。</td>
</tr>
<tr>
<td valign="top">Alt-u</td>
<td valign="top"> 把从光标位置到字尾的字符转换成大写字母。</td>
</tr>
</table>

### 剪切和粘贴文本
#### 剪切和粘贴命令
<table class="multi">
<tr>
<th class="title"> 按键</th>
<th class="title"> 行动</th>
</tr>
<tr>
<td valign="top" width="25%">Ctrl-k</td>
<td valign="top"> 剪切从光标位置到行尾的文本。</td>
</tr>
<tr>
<td valign="top">Ctrl-u</td>
<td valign="top"> 剪切从光标位置到行首的文本。</td>
</tr>
<tr>
<td valign="top">Alt-d</td>
<td valign="top"> 剪切从光标位置到词尾的文本。</td>
</tr>
<tr>
<td valign="top">Alt-Backspace</td>
<td valign="top"> 剪切从光标位置到词头的文本。如果光标在一个单词的开头，剪切前一个单词。</td>
</tr>
<tr>
<td valign="top">Ctrl-y</td>
<td valign="top"> 把剪切环中的文本粘贴到光标位置。</td>
</tr>
</table>

> 元键:在当今的键盘上，这个元键是指 Alt 键，但并不总是这样

### 自动补全
当你敲入一个命令时，按下 tab 键，自动补全就会发生

自动补全能对路径名,变量（如果 字的开头是一个”$”）、用户名字（单词以”~”开始）、命令（如果单词是一行的第一个单词） 和主机名（如果单词的开头是”@”）起作用。主机名自动补全只对包含在文件/etc/hosts 中的主机名有效

#### 自动补全命令
</table>
<table class="multi">
<tr>
<th class="title">按键</th>
<th class="title">行动</th>
</tr>
<tr>
<td valign="top" width="25%">Alt-?</td>
<td valign="top"> 显示可能的自动补全列表。在大多数系统中，你也可以完成这个通过按
两次 tab 键，这会更容易些。</td>
</tr>
<tr>
<td valign="top">Alt-*</td>
<td valign="top">插入所有可能的自动补全。当你想要使用多个可能的匹配项时，这个很有帮助。</td>
</tr>
</table>

>可编程自动补全:可编程自动补全是由 shell 函数实现 set | less

## 利用历史命令
### 搜索历史命令
bash 会存储你所输入的最后 500 个命令
```
[me@linuxbox ~]$ history | less
[me@linuxbox ~]$ history | grep /usr/bin
88  ls -l /usr/bin > ls-output.txt
```
使用`!`展开历史命令
```
[me@linuxbox ~]$ !88
```

#### 递增搜索
启动递增搜索， 输入 Ctrl-r，其后输入你要寻找的文本。当你找到它以后，你可以敲入 Enter 来执行命令， 或者输入 Ctrl-j，从历史列表中复制这一行到当前命令行。再次输入 Ctrl-r，来找到下一个 匹配项（向上移动历史列表）。输入 Ctrl-g 或者 Ctrl-c，退出搜索

### 历史命令
<table class="multi">
<tr>
<th class="title">按键</th>
<th class="title">行为</th>
</tr>
<tr>
<td valign="top" width="25%">Ctrl-p</td>
<td valign="top">移动到上一个历史条目。类似于上箭头按键。</td>
</tr>
<tr>
<td valign="top">Ctrl-n</td>
<td valign="top">移动到下一个历史条目。类似于下箭头按键。</td>
</tr>
<tr>
<td valign="top">Alt-<</td>
<td valign="top"> 移动到历史列表开头。</td>
</tr>
<tr>
<td valign="top">Alt-></td>
<td valign="top">移动到历史列表结尾，即当前命令行。</td>
</tr>
<tr>
<td valign="top">Ctrl-r</td>
<td valign="top">反向递增搜索。从当前命令行开始，向上递增搜索。</td>
</tr>
<tr>
<td valign="top">Alt-p</td>
<td valign="top">反向搜索，不是递增顺序。输入要查找的字符串，然后按下 Enter，执行搜索。</td>
</tr>
<tr>
<td valign="top">Alt-n</td>
<td valign="top"> 向前搜索，非递增顺序。</td>
</tr>
<tr>
<td valign="top">Ctrl-o</td>
<td valign="top">执行历史列表中的当前项，并移到下一个。如果你想要执行历史列表中一系列的命令，这很方便。</td>
</tr>

### 历史命令展开

<table class="multi">
<tr>
<th class="title">序列</th>
<th class="title">行为</th>
</tr>
<tr>
<td valign="top" width="25%">!!</td>
<td valign="top">重复最后一次执行的命令。可能按下上箭头按键和 enter 键更容易些。</td>
</tr>
<tr>
<td valign="top">!number</td>
<td valign="top">重复历史列表中第 number 行的命令。</td>
</tr>
<tr>
<td valign="top">!string</td>
<td valign="top">重复最近历史列表中，以这个字符串开头的命令。</td>
</tr>
<tr>
<td valign="top">!?string</td>
<td valign="top">重复最近历史列表中，包含这个字符串的命令。</td>
</tr>
</table>

谨慎地使用 “!string” 和 “!?string” 格式

>script [file]
>命令中的 file 是指用来存储 shell 会话记录的文件名