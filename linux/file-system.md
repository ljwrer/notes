
# 2.Linux 操作系统
* ls — 列出目录内容

* file — 确定文件类型

* less — 浏览文件内容

## ls

	ls -l 长模式

### 长格式输出
<table class="multi">
<caption class="cap">表 4-2: ls 长格式列表的字段</caption>
<tr>
<th class="title">字段</th>
<th class="title">含义</th>
</tr>
<tr>
<td valign="top" width="20%">-rw-r--r--</td>
<td valign="top">对于文件的访问权限。第一个字符指明文件类型。在不同类型之间，
开头的“－”说明是一个普通文件，“d”表明是一个目录。其后三个字符是文件所有者的
访问权限，再其后的三个字符是文件所属组中成员的访问权限，最后三个字符是其他所
有人的访问权限。这个字段的完整含义将在第十章讨论。 </td>
</tr>
<tr>
<td>1</td>
<td>文件的硬链接数目。参考随后讨论的关于链接的内容。 </td>
</tr>
<tr>
<td>root</td>
<td>文件属主的用户名。</td>
</tr>
<tr>
<td>root</td>
<td>文件所属用户组的名字。</td>
</tr>
<tr>
<td>32059</td>
<td>以字节数表示的文件大小。</td>
</tr>
<tr>
<td>2007-04-03 11:05 </td>
<td>上次修改文件的时间和日期。</td>
</tr>
<tr>
<td>oo-cd-cover.odf </td>
<td>文件名。</td>
</tr>
</table>

## 选项和参数

	command -options arguments

## file
```
file filename
获取文件类型
```
>一切皆文件

## less


<table class="multi">
<caption class="cap">表 4-3: less 命令</caption>
<tr>
<th class="title" width="30%">命令</th>
<th class="title">行为</th>
</tr>
<tr>
<td valign="top">Page UP or b</td>
<td valign="top">向上翻滚一页</td>
</tr>
<tr>
<td valign="top">Page Down or space</td>
<td valign="top">向下翻滚一页</td>
</tr>
<tr>
<td valign="top">UP Arrow</td>
<td valign="top">向上翻滚一行</td>
</tr>
<tr>
<td valign="top">Down Arrow</td>
<td valign="top">向下翻滚一行</td>
</tr>
<tr>
<td valign="top">G</td>
<td valign="top">移动到最后一行</td>
</tr>
<tr>
<td valign="top">1G or g</td>
<td valign="top">移动到开头一行</td>
</tr>
<tr>
<td valign="top">/charaters</td>
<td valign="top">向前查找指定的字符串</td>
</tr>
<tr>
<td valign="top">n</td>
<td valign="top">向前查找下一个出现的字符串，这个字符串是之前所指定查找的</td>
</tr>
<tr>
<td valign="top">h</td>
<td valign="top">显示帮助屏幕</td>
</tr>
<tr>
<td valign="top">q</td>
<td valign="top">退出 less 程序</td>
</tr>
</table>

## 系统目录

<table class="multi">
<caption class="cap">表 4-4: Linux 系统中的目录</caption>
<tr>
<th class="title">目录</th>
<th class="title">评论</th>
</tr>
<tr>
<td valign="top">/</td>
<td valign="top">根目录，万物起源。</td>
</tr>
<tr>
<td valign="top">/bin</td>
<td valign="top">包含系统启动和运行所必须的二进制程序。</td>
</tr>
<tr>
<td valign="top">/boot</td>
<td valign="top"><p>包含 Linux 内核、初始 RAM 磁盘映像（用于启动时所需的驱动）和
启动加载程序。</p>
<p>有趣的文件：</p>
<ul>
<li>/boot/grub/grub.conf or menu.lst， 被用来配置启动加载程序。</li>
<li>/boot/vmlinuz，Linux 内核。</li>
</ul>
</td>
</tr>
<tr>
<td valign="top">/dev</td>
<td valign="top">这是一个包含设备结点的特殊目录。“一切都是文件”，也适用于设备。
在这个目录里，内核维护着所有设备的列表。</td>
</tr>
<tr>
<td valign="top">/etc</td>
<td valign="top"><p>这个目录包含所有系统层面的配置文件。它也包含一系列的 shell 脚本，
在系统启动时，这些脚本会开启每个系统服务。这个目录中的任何文件应该是可读的文本文件。</p>
<p>有趣的文件：虽然/etc 目录中的任何文件都有趣，但这里只列出了一些我一直喜欢的文件：</p>
<ul>
<li>/etc/crontab， 定义自动运行的任务。</li>
<li>/etc/fstab，包含存储设备的列表，以及与他们相关的挂载点。</li>
<li>/etc/passwd，包含用户帐号列表。 </li>
</ul>
</td>
</tr>
<tr>
<td valign="top">/home</td>
<td valign="top">在通常的配置环境下，系统会在/home 下，给每个用户分配一个目录。普通用户只能
在自己的目录下写文件。这个限制保护系统免受错误的用户活动破坏。</td>
</tr>
<tr>
<td valign="top">/lib </td>
<td valign="top">包含核心系统程序所使用的共享库文件。这些文件与 Windows 中的动态链接库相似。</td>
</tr>
<tr>
<td valign="top">/lost+found </td>
<td valign="top">每个使用 Linux 文件系统的格式化分区或设备，例如 ext3文件系统，
都会有这个目录。当部分恢复一个损坏的文件系统时，会用到这个目录。除非文件系统
真正的损坏了，那么这个目录会是个空目录。</td>
</tr>
<tr>
<td>/media </td>
<td>在现在的 Linux 系统中，/media 目录会包含可移动介质的挂载点，
例如 USB 驱动器，CD-ROMs 等等。这些介质连接到计算机之后，会自动地挂载到这个目录结点下。
</td>
</tr>
<tr>
<td>/mnt</td>
<td>在早些的 Linux 系统中，/mnt 目录包含可移动介质的挂载点。</td>
</tr>
<tr>
<td>/opt</td>
<td>这个/opt 目录被用来安装“可选的”软件。这个主要用来存储可能
安装在系统中的商业软件产品。</td>
</tr>
<tr>
<td>/proc</td>
<td>这个/proc 目录很特殊。从存储在硬盘上的文件的意义上说，它不是真正的文件系统。
相反，它是一个由 Linux 内核维护的虚拟文件系统。它所包含的文件是内核的窥视孔。这些文件是可读的，
它们会告诉你内核是怎样监管计算机的。</td>
</tr>
<tr>
<td>/root</td>
<td>root 帐户的家目录。</td>
</tr>
<tr>
<td>/sbin</td>
<td>这个目录包含“系统”二进制文件。它们是完成重大系统任务的程序，通常为超级用户保留。</td>
</tr>
<tr>
<td>/tmp</td>
<td>这个/tmp 目录，是用来存储由各种程序创建的临时文件的地方。一些配置导致系统每次
重新启动时，都会清空这个目录。</td>
</tr>
<tr>
<td>/usr</td>
<td>在 Linux 系统中，/usr 目录可能是最大的一个。它包含普通用户所需要的所有程序和文件。</td>
</tr>
<tr>
<td>/usr/bin</td>
<td>/usr/bin 目录包含系统安装的可执行程序。通常，这个目录会包含许多程序。</td>
</tr>
<tr>
<td>/usr/lib</td>
<td>包含由/usr/bin 目录中的程序所用的共享库。 </td>
</tr>
<tr>
<td>/usr/local</td>
<td>这个/usr/local 目录，是非系统发行版自带，却打算让系统使用的程序的安装目录。
通常，由源码编译的程序会安装在/usr/local/bin 目录下。新安装的 Linux 系统中，会存在这个目录，
但却是空目录，直到系统管理员放些东西到它里面。</td>
</tr>
<tr>
<td>/usr/sbin</td>
<td>包含许多系统管理程序。 </td>
</tr>
<tr>
<td>/usr/share</td>
<td>/usr/share 目录包含许多由/usr/bin 目录中的程序使用的共享数据。
其中包括像默认的配置文件、图标、桌面背景、音频文件等等。</td>
</tr>
<tr>
<td>/usr/share/doc</td>
<td>大多数安装在系统中的软件包会包含一些文档。在/usr/share/doc 目录下，
我们可以找到按照软件包分类的文档。</td>
</tr>
<tr>
<td>/var</td>
<td>除了/tmp 和/home 目录之外，相对来说，目前我们看到的目录是静态的，这是说，
它们的内容不会改变。/var 目录是可能需要改动的文件存储的地方。各种数据库，假脱机文件，
用户邮件等等，都位于在这里。</td>
</tr>
<tr>
<td>/var/log</td>
<td>这个/var/log 目录包含日志文件、各种系统活动的记录。这些文件非常重要，并且
应该时时监测它们。其中最重要的一个文件是/var/log/messages。注意，为了系统安全，在一些系统中，
你必须是超级用户才能查看这些日志文件。</td></tr>
</table>

[http://www.pathname.com/fhs/](http://www.pathname.com/fhs/)

## 符号链接
符号链接（也称为软链接或者 symlink ）
一个文件被多个文件名所指向
```
lrwxrwxrwx 1 root root 11 2007-08-11 07:34 libc.so.6 -> libc-2.6.so
```
`l`代表符号链接

还有一种链接类型，叫做硬链接。硬链接同样允许文件有多个名字， 但是硬链接以不同的方法来创建多个文件名


---

# 3.操作文件和目录
 - cp — 复制文件和目录
 - mv — 移动/重命名文件和目录
 - mkdir — 创建目录
 - rm — 删除文件和目录
 - ln — 创建硬链接和符号链接

命令行程序，功能强大灵活。虽然图形文件管理器能轻松地实现简单的文件操作，但是对于 复杂的文件操作任务，则使用命令行程序比较容易完成

## 通配符

接受文件名作为参数的任何命令，都可以使用通配符

<table class="multi">
<tr>
<th class="title">通配符</th>
<th class="title">意义</th>
</tr>
<tr>
<td valign="top">*</td>
<td valign="top">匹配任意多个字符（包括零个或一个）</td>
</tr>
<tr>
<td valign="top">?</td>
<td valign="top">匹配任意一个字符（不包括零个）</td>
</tr>
<tr>
<td valign="top">[characters]</td>
<td valign="top">匹配任意一个属于字符集中的字符</td>
</tr>
<tr>
<td valign="top">[!characters]</td>
<td valign="top">匹配任意一个不是字符集中的字符</td>
</tr>
<tr>
<td valign="top" width="25%">[[:class:]]</td>
<td valign="top">匹配任意一个属于指定字符类中的字符</td>
</tr>
</table>

### 普遍使用的字符类

<table class="multi">
<tr>
<th class="title">字符类</th>
<th class="title">意义</th>
</tr>
<tr>
<td>[:alnum:]</td>
<td>匹配任意一个字母或数字</td>
</tr>
<tr>
<td>[:alpha:]</td>
<td>匹配任意一个字母</td>
</tr>
<tr>
<td>[:digit:]</td>
<td>匹配任意一个数字</td>
</tr>
<tr>
<td>[:lower:]</td>
<td>匹配任意一个小写字母</td>
</tr>
<tr>
<td width="25%">[:upper:]</td>
<td>匹配任意一个大写字母</td>
</tr>
</table>

### 通配符范例
<table class="multi">
<tr>
<th class="title">模式</th>
<th class="title">匹配对象</th>
</tr>
<tr>
<td valign="top">*</td>
<td valign="top">所有文件</td>
</tr>
<tr>
<td valign="top">g*</td>
<td valign="top">文件名以“g”开头的文件</td>
</tr>
<tr>
<td valign="top">b*.txt</td>
<td valign="top">以"b"开头，中间有零个或任意多个字符，并以".txt"结尾的文件</td>
</tr>
<tr>
<td valign="top">Data???</td>
<td valign="top">以“Data”开头，其后紧接着3个字符的文件</td>
</tr>
<tr>
<td valign="top">[abc]*</td>
<td valign="top">文件名以"a","b",或"c"开头的文件</td>
</tr>
<tr>
<td valign="top">BACKUP.[0-9][0-9][0-9]</td>
<td valign="top">以"BACKUP."开头，并紧接着3个数字的文件</td>
</tr>
<tr>
<td valign="top">[[:upper:]]*</td>
<td valign="top">以大写字母开头的文件</td>
</tr>
<tr>
<td valign="top">[![:digit:]]*</td>
<td valign="top">不以数字开头的文件</td>
</tr>
<tr>
<td valign="top" width="25%">*[[:lower:]123]</td>
<td valign="top">文件名以小写字母结尾，或以 “1”，“2”，或 “3” 结尾的文件</td>
</tr>
</table>

## mkdir - 创建目录
```
mkdir directory...
```
>当有三个圆点跟在一个命令的参数后面， 这意味着那个参数可以重复

```
mkdir dir1 dir2 dir3
```

## cp - 复制文件和目录
```
cp item1 item2
cp item... directory
```
### cp 选项
<table class="multi">
<tr>
<th class="title">选项</th>
<th class="title">意义</th>
</tr>
<tr>
<td valign="top" width="25%">-a, --archive</td>
<td valign="top">复制文件和目录，以及它们的属性，包括所有权和权限。
通常，复本具有用户所操作文件的默认属性。</td>
</tr>
<tr>
<td valign="top">-i, --interactive</td>
<td valign="top">在重写已存在文件之前，提示用户确认。如果这个选项不指定，
cp 命令会默认重写文件。</td>
</tr>
<tr>
<td valign="top">-r, --recursive</td>
<td valign="top">递归地复制目录及目录中的内容。当复制目录时，
需要这个选项（或者-a 选项）。</td>
</tr>
<tr>
<td valign="top">-u, --update </td>
<td valign="top">当把文件从一个目录复制到另一个目录时，仅复制
目标目录中不存在的文件，或者是文件内容新于目标目录中已经存在的文件。</td>
</tr>
<tr>
<td valign="top">-v, --verbose</td>
<td valign="top">显示翔实的命令操作信息</td>
</tr>
</table>

## cp 实例
<table class="multi">
<tr>
<th class="title">命令</th>
<th class="title">运行结果</th>
</tr>
<tr>
<td valign="top" width="25%">cp file1 file2</td>
<td valign="top">复制文件 file1 内容到文件 file2。如果 file2 已经存在，
file2 的内容会被 file1 的内容重写。如果 file2 不存在，则会创建 file2。</td>
</tr>
<tr>
<td valign="top">cp -i file1 file2 </td>
<td valign="top">这条命令和上面的命令一样，除了如果文件 file2 存在的话，在文件 file2 被重写之前，
会提示用户确认信息。</td>
</tr>
<tr>
<td valign="top">cp file1 file2 dir1 </td>
<td valign="top">复制文件 file1 和文件 file2 到目录 dir1。目录 dir1 必须存在。
</td>
</tr>
<tr>
<td valign="top">cp dir1/* dir2 </td>
<td valign="top">使用一个通配符，在目录 dir1 中的所有文件都被复制到目录 dir2 中。
dir2 必须已经存在。</td>
</tr>
<tr>
<td valign="top">cp -r dir1 dir2 </td>
<td valign="top">复制目录 dir1 中的内容到目录 dir2。如果目录 dir2 不存在，
创建目录 dir2，操作完成后，目录 dir2 中的内容和 dir1 中的一样。
如果目录 dir2 存在，则目录 dir1 (和目录中的内容)将会被复制到 dir2 中。</td>
</tr>
</table>

## mv - 移动和重命名文件
```
mv item1 item2
mv item... directory
```
### mv 选项
<table class="multi">
<caption class="cap"></caption>
<tr>
<th class="title">选项</th>
<th class="title">意义</th>
</tr>
<tr>
<td valign="top" width="25%">-i --interactive</td>
<td valign="top">在重写一个已经存在的文件之前，提示用户确认信息。
<b>如果不指定这个选项，mv 命令会默认重写文件内容。</b></td>
</tr>
<tr>
<td valign="top">-u --update</td>
<td valign="top">当把文件从一个目录移动另一个目录时，只是移动不存在的文件，
或者文件内容新于目标目录相对应文件的内容。</td>
</tr>
<tr>
<td valign="top">-v --verbose</td>
<td valign="top">当操作 mv 命令时，显示翔实的操作信息。</td>
</tr>
</table>

### mv 实例
<table class="multi">
<caption class="cap"></caption>
<tr>
<td class="title">mv file1 file2</td>
<td class="title">移动 file1 到 file2。<b>如果 file2 存在，它的内容会被 file1 的内容重写。
</b>如果 file2 不存在，则创建 file2。<b> 每种情况下，file1 不再存在。</b></td>
</tr>
<tr>
<td valign="top" width="25%">mv -i file1 file2</td>
<td valign="top">除了如果 file2 存在的话，在 file2 被重写之前，用户会得到
提示信息外，这个和上面的选项一样。</td>
</tr>
<tr>
<td valign="top">mv file1 file2 dir1</td>
<td valign="top">移动 file1 和 file2 到目录 dir1 中。dir1 必须已经存在。</td>
</tr>
<tr>
<td valign="top">mv dir1 dir2</td>
<td valign="top">如果目录 dir2 不存在，创建目录 dir2，并且移动目录 dir1 的内容到
目录 dir2 中，同时删除目录 dir1。如果目录 dir2 存在，移动目录 dir1（及它的内容）到目录 dir2。</td>
</tr>
</table>

## rm - 删除文件和目录
```
rm item...
```
### rm 选项
<table class="multi">
<tr>
<th class="title">选项</th>
<th class="title">意义</th>
</tr>
<tr>
<td valign="top" width="25%">-i, --interactive </td>
<td
valign="top">在删除已存在的文件前，提示用户确认信息。
<b>如果不指定这个选项，rm 会默默地删除文件</b>
</td>
</tr>
<tr>
<td valign="top">-r, --recursive</td>
<td valign="top">递归地删除文件，这意味着，如果要删除一个目录，而此目录
又包含子目录，那么子目录也会被删除。要删除一个目录，必须指定这个选项。</td>
</tr>
<tr>
<td valign="top">-f, --force</td>
<td valign="top">忽视不存在的文件，不显示提示信息。这选项覆盖了“--interactive”选项。</td>
</tr>
<tr>
<td valign="top">-v, --verbose</td>
<td valign="top">在执行 rm 命令时，显示翔实的操作信息。</td>
</tr>
</table>

### rm 实例
<table class="multi">
<tr>
<th class="title">命令</th>
<th class="title">运行结果</th>
</tr>
<tr>
<td valign="top" width="25%">rm file1</td>
<td valign="top">默默地删除文件</td>
</tr>
<tr>
<td valign="top">rm -i file1</td>
<td valign="top">除了在删除文件之前，提示用户确认信息之外，和上面的命令作用一样。</td>
</tr>
<tr>
<td valign="top">rm -r file1 dir1</td>
<td valign="top">删除文件 file1, 目录 dir1，及 dir1 中的内容。</td>
</tr>
<tr>
<td valign="top">rm -rf file1 dir1</td>
<td valign="top">同上，除了如果文件 file1，或目录 dir1 不存在的话，rm 仍会继续执行。</td>
</tr>
</table>

>无论什么时候，rm 命令用到通配符（除了仔细检查输入的内容外！）， 用 ls 命令来测试通配符。这会让你看到要删除的文件列表。然后按下上箭头按键，重新调用 刚刚执行的命令，用 rm 替换 ls。

## ln — 创建链接
ln 命令既可创建硬链接，也可以创建符号链接。
```
# 创建硬链接
ln file link
# 创建符号链接
ln -s item link
```
### 硬链接
1. 一个硬链接不能关联它所在文件系统之外的文件。这是说一个链接不能关联与链接本身不在同一个磁盘分区上的文件。
2. 一个硬链接不能关联一个目录。

>一个硬链接和文件本身没有什么区别。不像符号链接，当你列出一个包含硬链接的目录 内容时，你会看到没有特殊的链接指示说明。当一个硬链接被删除时，这个链接 被删除，但是文件本身的内容仍然存在（这是说，它所占的磁盘空间不会被重新分配）

```
269112 -rw-rw-r-- 2 vagrant vagrant    0 Feb  9 01:45 hard
269112 -rw-rw-r-- 2 vagrant vagrant    0 Feb  9 01:45 origin
270076 lrwxrwxrwx 1 vagrant vagrant    6 Feb  9 01:46 soft -> origin
```
>硬链接区块指针(对硬盘的指针)与源文件一致

### 符号链接
创建符号链接是为了克服硬链接的局限性。符号链接生效，是通过创建一个特殊类型的文件，这个文件包含一个关联文件或目录的文本指针。在这一方面， 它们和 Windows 的快捷方式差不多

符号链接是文件的特殊类型，它包含一个指向 目标文件或目录的文本指针

一个符号链接指向一个文件，而且这个符号链接本身与其它的符号链接几乎没有区别。 例如，如果你往一个符号链接里面写入东西，那么相关联的文件也被写入。然而， 当你删除一个符号链接时，只有这个链接被删除，而不是文件自身。如果先于符号链接 删除文件，这个链接仍然存在，但是不指向任何东西。在这种情况下，这个链接被称为 坏链接。在许多实现中，ls 命令会以不同的颜色展示坏链接，比如说红色，来显示它们 的存在。

建立符号链接的目的是为了克服硬链接的两个缺点：硬链接不能跨越物理设备， 硬链接不能关联目录，只能是文件。符号链接是文件的特殊类型，它包含一个指向 目标文件或目录的文本指针。

>响应命令提示信息，输入”y”，文件就会被重写，其它的字符（例如，”n”）会导致 cp 命令不理会文件

```
-rw-rw-r-- 4 vagrant vagrant  721 Feb  9 02:49 hard
-rw-rw-r-- 4 vagrant vagrant  721 Feb  9 02:49 origin
```
4代表硬链接数，一个文件至少有一个硬链接，因为文件 名就是由链接创建的

>当考虑到硬链接的时候，我们可以假设文件由两部分组成：包含文件内容的数据部分和持有文件名的名字部分 ，这将有助于我们理解这个概念。当我们创建文件硬链接的时候，实际上是为文件创建了额外的名字部分， 并且这些名字都关系到相同的数据部分。这时系统会分配一连串的磁盘块给所谓的索引节点，然后索引节点与文 件名字部分相关联。因此每一个硬链接都关系到一个具体的包含文件内容的索引节点。
```
# 查看区块指针
ls -li 
```

符号链接文件的长度是指向文件名字符串所包含的字符数，而不是符号链接所指向的文件长度

当建立符号链接时，你既可以使用绝对路径名，也可用相对路径名。使用相对路径名更令人满意， 因为它允许一个包含符号链接的目录重命名或移动，而不会破坏链接。

对于符号链接，有一点值得记住，执行的大多数文件操作是针对链接的对象，而不是链接本身。 而 rm 命令是个特例。当你删除链接的时候，删除链接本身，而不是链接的对象

>GNOME 里面，当拖动文件时，同时按下 Ctrl+Shift 按键会创建一个链接，而不是 复制（或移动）文件。在 KDE 中，无论什么时候放下一个文件，会弹出一个小菜单， 这个菜单会提供复制，移动，或创建链接文件选项。

---
