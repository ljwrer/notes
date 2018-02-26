# 8. 权限

 - id – 显示用户身份号
 - chmod – 更改文件模式
 - umask – 设置默认的文件权限
 - su – 以另一个用户的身份来运行 shell
 - sudo – 以另一个用户的身份来执行命令
 - chown – 更改文件所有者
 - chgrp – 更改文件组所有权
 - passwd – 更改用户密码

## 拥有者、组成员和其他人
在 Unix 安全模型中，一个用户可能拥有文件和目录。当一个用户拥有一个文件或目录时， 用户对这个文件或目录的访问权限拥有控制权。用户反过来又属于一个由一个或多个 用户组成的用户组，用户组成员由文件和目录的所有者授予对文件和目录的访问权限。除了 对一个用户组授予权限之外，文件所有者可能会给每个人一些权限

```
[me@linuxbox ~]$ id
uid=500(me) gid=500(me) groups=500(me)
```
> Fedora 系统 从500开始进行普通用户帐户的编号，而 Ubuntu 从1000开始

用户帐户 定义在/etc/passwd 文件里面，用户组定义在/etc/group 文件里面。当用户帐户和用户组创建以后， 这些文件随着文件/etc/shadow 的变动而修改，文件/etc/shadow 包含了关于用户密码的信息。 对于每个用户帐号，文件/etc/passwd 定义了用户（登录）名、uid、gid、帐号的真实姓名、家目录 和登录 shell

## 读取、写入和执行
```
-rw-rw-r--
```
列表的前十个字符是文件的属性
### 文件类型
这十个字符的第一个字符表明文件类型
<table class="multi">
<tr>
<th class="title" width="15%">属性</th>
<th class="title">文件类型</th>
</tr>
<tr>
<td valign="top">-</td>
<td valign="top">一个普通文件</td>
</tr>
<tr>
<td valign="top">d </td>
<td valign="top">一个目录</td>
</tr>
<tr>
<td valign="top">l</td>
<td valign="top"> 一个符号链接。注意对于符号链接文件，剩余的文件属性总是"rwxrwxrwx"，而且都是
虚拟值。真正的文件属性是指符号链接所指向的文件的属性。</td>
</tr>
<tr>
<td valign="top">c</td>
<td valign="top"> 一个字符设备文件。这种文件类型是指按照字节流来处理数据的设备。
比如说终端机或者调制解调器</td>
</tr>
<tr>
<td valign="top">b</td>
<td valign="top"> 一个块设备文件。这种文件类型是指按照数据块来处理数据的设备，例如一个硬盘或者 CD-ROM 盘。 </td>
</tr>
</table>

### 文件模式
剩下的九个字符叫做文件模式

![](http://billie66.github.io/TLCL/book/images/101.png)

<table class="multi">
<caption class="cap">表 10-2: 权限属性</caption>
<tr>
<th class="title" width="15%">属性</th>
<th class="title">文件</th>
<th class="title">目录</th>
</tr>
<tr>
<td valign="top">r</td>
<td valign="top">允许打开并读取文件内容。</td>
<td valign="top">允许列出目录中的内容，前提是目录必须设置了可执行属性（x）。</td>
</tr>
<tr>
<td valign="top">w</td>
<td valign="top">允许写入文件内容或截断文件。但是不允许对文件进行重命名或删除，重命名或删除是由目录的属性决定的。</td>
<td valign="top">允许在目录下新建、删除或重命名文件，前提是目录必须设置了可执行属性（x）。</td>
</tr>
<tr>
<td valign="top">x</td>
<td valign="top">允许将文件作为程序来执行，使用脚本语言编写的程序必须设置为可读才能被执行。</td>
<td valign="top">允许进入目录，例如：cd directory 。</td>
</tr>
</table>

#### 权限属性示例
<table class="multi">
<tr>
<th class="title" width="15%">文件属性</th>
<th class="title">含义</th>
</tr>
<tr>
<td valign="top">-rwx------</td>
<td valign="top">一个普通文件，对文件所有者来说可读、可写、可执行。其他人无法访问。</td>
</tr>
<tr>
<td valign="top">-rw-------</td>
<td valign="top">一个普通文件，对文件所有者来说可读可写。其他人无法访问。</td>
</tr>
<tr>
<td valign="top">-rw-r--r--</td>
<td valign="top">一个普通文件，对文件所有者来说可读可写，文件所有者的组成员可以读该文件，其他所有人都可以读该文件。</td>
</tr>
<tr>
<td valign="top">-rwxr-xr-x</td>
<td valign="top">一个普通文件，对文件所有者来说可读、可写、可执行。也可以被其他的所有人读取和执行。</td>
</tr>
<tr>
<td valign="top">-rw-rw----</td>
<td valign="top">一个普通文件，对文件所有者以及文件所有者的组成员来说可读可写。</td>
</tr>
<tr>
<td valign="top">lrwxrwxrwx</td>
<td valign="top">一个符号链接，符号链接的权限都是虚拟的，真实的权限应该以符号链接指向的文件为准。</td>
</tr>
<tr>
<td valign="top">drwxrwx---</td>
<td valign="top">一个目录，文件所有者以及文件所有者的组成员可以访问该目录，并且可以在该目录下新建、重命名、删除文件。</td>
</tr>
<tr>
<td valign="top">drwxr-x---</td>
<td valign="top">一个目录，文件所有者可以访问该目录，并且可以在该目录下新建、重命名、删除文件，文件所有者的组成员可以访问该目录，但是不能新建、重命名、删除文件。</td>
</tr>
</table>

## chmod － 更改文件模式
更改文件或目录的模式（权限），可以利用 chmod 命令。只有文件的所有者或者超级用户才 能更改文件或目录的模式。

chmod 命令支持两种不同的方法来改变文件模式：八进制数字表示法或 符号表示法。

### 八进制表示法

|-|-|
|Octal| Binary | File Mode|
|0 | 000 | -\-\-|
|1 | 001 | -\-x|
|2 | 010 | -w-|
|3 | 011 | -wx|
|4 | 100 | r-\-|
|5 | 101 | r-x|
|6 | 110 | rw-|
|7 | 111 | rwx|

通过使用3个八进制数字，我们能够设置文件所有者、用户组和其他人的权限

常见的映射关系： 7 (rwx)，6 (rw-)，5 (r-x)，4 (r--)，和 0 (---)

### 符号表示法
符号表示法分为三部分：更改会影响谁， 要执行哪个操作，要设置哪种权限

#### chmod 命令符号表示法
<table class="multi">
<tr>
<td class="title" width="15%">u</td>
<td class="title"> "user"的简写，意思是文件或目录的所有者。</td>
</tr>
<tr>
<td valign="top">g</td>
<td valign="top"> 用户组。</td>
</tr>
<tr>
<td valign="top">o</td>
<td valign="top"> "others"的简写，意思是其他所有的人。</td>
</tr>
<tr>
<td valign="top">a</td>
<td valign="top"> "all"的简写，是"u", "g"和“o”三者的联合。</td>
</tr>
</table>

如果没有指定字符，则假定使用”all”。执行的操作可能是一个“＋”字符，表示加上一个权限， 一个“－”，表示删掉一个权限，或者是一个“＝”，表示只有指定的权限可用，其它所有的权限被删除。

#### chmod 符号表示法实例
<table class="multi">
<tr>
<td class="title" width="15%">u+x </td>
<td class="title"> 为文件所有者添加可执行权限。</td>
</tr>
<tr>
<td valign="top">u-x</td>
<td valign="top"> 删除文件所有者的可执行权限。</td>
</tr>
<tr>
<td valign="top">+x</td>
<td valign="top"> 为文件所有者，用户组，和其他所有人添加可执行权限。 等价于 a+x。</td>
</tr>
<tr>
<td valign="top">o-rw</td>
<td valign="top"> 除了文件所有者和用户组，删除其他人的读权限和写权限。</td>
</tr>
<tr>
<td valign="top">go=rw</td>
<td valign="top"> 给群组的主人和任意文件拥有者的人读写权限。如果群组的主人或全局之前已经有了执行的权限，他们将被移除。 </td>
</tr>
<tr>
<td valign="top">u+x,go=rw</td>
<td valign="top"> 给文件拥有者执行权限并给组和其他人读和执行的权限。多种设定可以用逗号分开。</td>
</tr>
</table>

>符号表示法的优点是， 允许你设置文件模式的某个属性，而不影响其他的属性

>谨慎使用chmod -r

## umask － 设置默认权限
umask 命令控制着文件的默认权限

umask 命令使用八进制表示法来表达 从文件模式属性中删除一个位掩码

默认0002 -> rw-rw-r

默认0022 -> mask: 000 000 010 010 -> rw-r-r

0000 ->rw-rw-rw

>大多数情况下，你不必修改掩码值

## 特殊权限
### setuid
当应用到一个可执行文件时，它把有效用户 ID 从真正的用户（实际运行程序的用户）设置成程序所有者的 ID。这种操作通常会应用到 一些由超级用户所拥有的程序。当一个普通用户运行一个程序，这个程序由根用户(root) 所有，并且设置了 setuid 位，这个程序运行时具有超级用户的特权，这样程序就可以 访问普通用户禁止访问的文件和目录。很明显，因为这会引起安全方面的问题，所有可以 设置 setuid 位的程序个数，必须控制在绝对小的范围内
>普通用户运行需要程序需要访问root权限时比较有用

### setgid
这个相似于 setuid 位，把有效用户组 ID 从真正的 用户组 ID 更改为文件所有者的组 ID。如果设置了一个目录的 setgid 位，则目录中新创建的文件 具有这个目录用户组的所有权，而不是文件创建者所属用户组的所有权。对于共享目录来说， 当一个普通用户组中的成员，需要访问共享目录中的所有文件，而不管文件所有者的主用户组时， 那么设置 setgid 位很有用处
>创建share文件夹时比较有用

### sticky
在 Unix 中，它可能把一个可执行文件 标志为“不可交换的”。在 Linux 中，会忽略文件的 sticky 位，但是如果一个目录设置了 sticky 位， 那么它能阻止用户删除或重命名文件，除非用户是这个目录的所有者，或者是文件所有者，或是 超级用户。这个经常用来控制访问共享目录，比方说/tmp。


```
# 授予一个程序 setuid 权限
chmod u+s program

# 授予一个目录 setgid 权限：
chmod g+s dir

# 授予一个目录 sticky 权限：
chmod +t dir

# 当浏览 ls 命令的输出结果时，你可以确认这些特殊权限
# 程序被设置为setuid属性：
-rwsr-xr-x

# 具有 setgid 属性的目录：
drwxrwsr-x

# 设置了 sticky 位的目录：
drwxrwxrwt

```


## 更改身份
有三种方式，可以拥有多重身份
1. 注销系统并以其他用户身份重新登录系统。
2. 使用 su 命令。
3. 使用 sudo 命令。

### su － 以其他用户身份和组 ID 运行一个 shell
```
su [-[l]] [user]
```
果包含”-l”选项，那么会为指定用户启动一个需要登录的 shell,这意味着会加载此用户的 shell 环境， 并且工作目录会更改到这个用户的家目录。
如果不指定用户，那么就假定是 超级用户。注意（不可思议地），选项”-l”可以缩写为”-“，

```
[me@linuxbox ~]$ su -
Password:
[root@linuxbox ~]#
```
exit返回到原来的 shell
```
[root@linuxbox ~]# exit
[me@linuxbox ~]$
```
命令传递到一个新 shell 中执行。把命令用单引号引起来不在我们的 shell 中展开，但需要在新 shell 中展开
```
su -c 'command'
```

### sudo － 以另一个用户身份执行命令
管理员能够配置 sudo 命令，从而允许一个普通用户以不同的身份（通常是超级用户），通过一种非常可控的方式来执行命令

sudo 命令不要求超级用户的密码（需配置）。使用 sudo 命令时，用户使用他/她自己的密码 来认证

```
[me@linuxbox ~]$ sudo backup_script
Password:
System Backup Starting...
```

su 和 sudo 之间的一个重要区别是 sudo 不会重新启动一个 shell，也不会加载另一个 用户的 shell 运行环境。这意味者命令不必用单引号引起来

想知道 sudo 命令可以授予哪些权限，使用”-l”选项，列出所有权限：
```
[me@linuxbox ~]$ sudo -l
User me may run the following commands on this host:
(ALL) ALL
```

>普通用户完成某些需要超级用户权限的任务。这些任务 包括安装和更新软件，编辑系统配置文件，和访问设备。在 Windows 世界里，这些任务是 通过授予用户管理员权限来完成的。这允许用户执行这些任务。Unix 采取的方法是只有在需要的时候，才授予普通用户超级用户权限。

>默认情况下，Ubuntu 不允许用户登录 到 root 帐号（因为不能为 root 帐号设置密码），而是使用 sudo 命令授予普通用户超级用户权限。 通过 sudo 命令，最初的用户可以拥有超级用户权限，也可以授予随后的用户帐号相似的权力。

## chown － 更改文件所有者和用户组
chown 命令被用来更改文件或目录的所有者和用户组。使用这个命令需要超级用户权限。
```
chown [owner][:[group]] file...
```
### chown 参数实例
<table class="multi">
<tr>
<th class="title">参数</th>
<th class="title">结果</th>
</tr>
<tr>
<td valign="top" width="15%">bob</td>
<td valign="top"> 把文件所有者从当前属主更改为用户 bob。</td>
</tr>
<tr>
<td valign="top">bob:users</td>
<td valign="top"> 把文件所有者改为用户 bob，文件用户组改为用户组 users。</td>
</tr>
<tr>
<td valign="top">:admins</td>
<td valign="top"> 把文件用户组改为组 admins，文件所有者不变。</td>
</tr>
<tr>
<td valign="top">bob:</td>
<td valign="top"> 文件所有者改为用户 bob，文件用户组改为用户 bob 登录系统时所属的用户组。</td>
</tr>
</table>

```
[janet@linuxbox ~]$ sudo cp myfile.txt ~tony
Password:
[janet@linuxbox ~]$ sudo ls -l ~tony/myfile.txt
-rw-r--r-- 1 root  root 8031 2008-03-20 14:30 /home/tony/myfile.txt
[janet@linuxbox ~]$ sudo chown tony: ~tony/myfile.txt
[janet@linuxbox ~]$ sudo ls -l ~tony/myfile.txt
-rw-r--r-- 1 tony  tony 8031 2008-03-20 14:30 /home/tony/myfile.txt
```
>在大多数的配置中，sudo 命令会相信你几分钟，直到计时结束

## chgrp － 更改用户组所有权
在旧版 Unix 系统中，chown 命令只能更改文件所有权，而不是用户组所有权

## 创建共享文件夹 
 /usr/local/share/dir

1. 系统中默认的掩码值是0022,把用户使用的掩码值改为0002
2. 用户组成员创建的文件和目录的用户组，将会设置为用户的主要组，而不是用户组,通过设置此目录的 setgid 位来解决这个问题

```
[bill@linuxbox ~]$ sudo chmod g+s /usr/local/share/Music
[bill@linuxbox ~]$ ls -ld /usr/local/share/Music
drwxrwsr-x 2 root music 4096 2008-03-24 20:03 /usr/local/share/Music
```

>umask 命令设置的掩码值只能在当前 shell 会话中生效

## 更改用户密码
```
passwd [user]
```
```
[me@linuxbox ~]$ passwd
(current) UNIX password:
New UNIX password:
```
如果你具有超级用户权限，你可以指定一个用户名作为 passwd 命令的参数，这样可以设置另一个 用户的密码