# 归档和备份

- gzip – 压缩或者展开文件
- bzip2 – 块排序文件压缩器
- tar – 磁带打包工具
- zip – 打包和压缩文件
- rsync – 同步远端文件和目录

### 压缩文件

#### gzip

```bash
[me@linuxbox ~]$ gzip foo.txt
# 保留原文件
[me@linuxbox ~]$ gzip -k foo.txt
[me@linuxbox ~]$ gunzip foo.txt.gz
```

 *gzip选项*

| 选项    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| -c      | 把输出写入到标准输出，并且保留原始文件。也有可能用--stdout 和--to-stdout 选项来指定。 |
| -d      | 解压缩。正如 gunzip 命令一样。也可以用--decompress 或者--uncompress 选项来指定. |
| -f      | 强制压缩，即使原始文件的压缩文件已经存在了，也要执行。也可以用--force 选项来指定。 |
| -h      | 显示用法信息。也可用--help 选项来指定。                      |
| -l      | 列出每个被压缩文件的压缩数据。也可用--list 选项。            |
| -r      | 若命令的一个或多个参数是目录，则递归地压缩目录中的文件。也可用--recursive 选项来指定。 |
| -t      | 测试压缩文件的完整性。也可用--test 选项来指定。              |
| -v      | 显示压缩过程中的信息。也可用--verbose 选项来指定。           |
| -number | 设置压缩指数。number 是一个在1（最快，最小压缩）到9（最慢，最大压缩）之间的整数。 数值1和9也可以各自用--fast 和--best 选项来表示。默认值是整数6。 |



```bash
# 浏览压缩文本文件的内容
[me@linuxbox ~]$ gunzip -c foo.txt | less
# zcat，同于带有-c 选项的 gunzip 命令
[me@linuxbox ~]$ zcat foo.txt.gz | less
# 与上面的管道线有相同的功能
[me@linuxbox ~]$ zless foo.txt.gz
```

#### bzip2

与 gzip 程序相似，但是使用了不同的压缩算法， 舍弃了压缩速度，而实现了更高的压缩级别

```bash
[me@linuxbox ~]$ bzip2 foo.txt
[me@linuxbox ~]$ bunzip2 foo.txt.bz2 
```

 bzip2recover 程序，会试图恢复受损的 .bz2 文件。

> 不要强迫性压缩

### 归档文件

#### tar

扩展名为 .tar 或者 .tgz 的文件，它们各自表示“普通” 的 tar 包和被 gzip 程序压缩过的 tar 包

```bash
$ tar mode[options] pathname...
```

*tar模式*

| 模式 | 说明                               |
| ---- | ---------------------------------- |
| c    | 为文件和／或目录列表创建归档文件。 |
| x    | 抽取归档文件。                     |
| r    | 追加具体的路径到归档文件的末尾。   |
| t    | 列出归档文件的内容                 |



```bash
# 模式 c 和选项 f,必须首先指定模式,然后才是其它的选项
[me@linuxbox ~]$ tar cf playground.tar playground
# 详细的列表信息
[me@linuxbox ~]$ tar tvf playground.tar
[me@linuxbox ~]$ tar xf ../playground.tar
```

> 除非你是超级用户，要不然从归档文件中抽取的文件 和目录的所有权由执行此复原操作的用户所拥有，而不属于原始所有者

##### tar 命令处理归档文件路径名的方式

默认情况下，路径名是相对的，而不是绝对 路径。当以相对路径创建归档文件的时候，tar 命令会简单地删除路径名开头的斜杠。绝对路径名则会带上完整的路径，如/home/me/playground



```bash
# 抽取一个归档文件时，有可能限制从归档文件中抽取什么内容
$ tar xf archive.tar pathname
```

路径名必须是完全的，精准的相对路径名，就如存储在归档文件中的一样。当指定路径名的时候， 通常不支持通配符；然而，GNU 版本的 tar 命令（在 Linux 发行版中最常出现）通过 --wildcards 选项来 支持通配符

```bash
[me@linuxbox foo]$ tar xf ../playground2.tar --wildcards 'home/me/playground/dir-\*/file-A'
```



##### 结合 find 命令一起来制作归档文件

```bash
# 追加模式（r）
[me@linuxbox ~]$ find playground -name 'file-A' -exec tar rf playground.tar '{}' '+'
```



使用 tar 和 find 命令，来创建逐渐增加的目录树或者整个系统的备份。通过 find 命令匹配新于某个时间戳的文件

```bash
[me@linuxbox ~]$ find playground -name 'file-A' | tar cf - --files-from=- | gzip > playground.tgz
```



现在的 GUN 版本的 tar 命令 ，gzip 和 bzip2 压缩两者都直接支持，各自使用 z 和 j 选项

```bash
[me@linuxbox ~]$ find playground -name 'file-A' | tar czf playground.tgz -T -
# tbz后缀
[me@linuxbox ~]$ find playground -name 'file-A' | tar cjf playground.tbz -T -
```



##### tar 命令与标准输入和输出的使用

```bash
# 远程压包至标准输出流 本地从标准输入流解包， - 代表标准流
[me@linuxbox remote-stuff]$ ssh remote-sys 'tar cf - Documents' | tar xf -
```



#### zip

```bash
$ zip options zipfile file...
```

```bash
# -r 递归压缩
[me@linuxbox ~]$ zip -r playground.zip playground
```

“store”表示没有压缩的文件，“deflate”表示执行压缩操作

```bash
[me@linuxbox foo]$ unzip ../playground.zip
```

对于 zip 命令（与 tar 命令相反）要注意一点，就是如果指定了一个已经存在的文件包，其被更新 而不是被替代。这意味着会保留此文件包，但是会添加新文件，同时替换匹配的文件。



可以列出 文件或者有选择地从一个 zip 文件包中抽取文件，只要给 unzip 命令指定文件名：

```bash
# -l 选项，导致 unzip 命令只是列出文件包中的内容而没有抽取文件
[me@linuxbox ~]$ unzip -l playground.zip playground/dir-87/file-Z
[me@linuxbox foo]$ unzip ./playground.zip playground/dir-87/file-Z
```



zip 命令能够利用标准输入和输出，虽然它的实施不大有用。通过-@选项，有可能把一系列的 文件名管道到 zip 命令。

```bash
[me@linuxbox ~]$ find playground -name "file-A" | zip -@ file-A.zip
```



zip 命令也支持把它的输出写入到标准输出，可以接受标准输入

unzip 程序，不接受标准输入

```bash
[me@linuxbox ~]$ ls -l /etc/ | zip ls-etc.zip -
```

 unzip 程序允许它的输出发送到标准输出，指定了-p 选项

```bash
[me@linuxbox ~]$ unzip -p ls-etc.zip | less
```



### 同步文件和目录

通过使用 rsync 远端更新协议，此协议 允许 rsync 快速地检测两个目录的差异，执行最小量的复制来达到目录间的同步

```bash
$ rsync options source destination
```

 source 和 destination 是下列选项之一：

- 一个本地文件或目录
- 一个远端文件或目录，以[user@]host:path 的形式存在
- 一个远端 rsync 服务器，由 rsync://[user@]host[:port]/path 指定

source 和 destination 两者之一必须是本地文件。rsync 不支持远端到远端的复制

```bash
# -a 选项（递归和保护文件属性）和-v 选项（冗余输出
[me@linuxbox ~]$ rsync -av playground foo
```



```bash
# 系统备份
# –delete 这个选项，来删除可能在备份设备中已经存在但却不再存在于源设备中的文件
[me@linuxbox ~]$ mkdir /media/BigDisk/backup
[me@linuxbox ~]$ sudo rsync -av --delete /etc /home /usr/local /media/BigDisk/backup
```



#### 在网络间使用 rsync 命令



```bash
# 添加了--rsh=ssh 选项，其指示 rsync 使用 ssh 程序作为它的远程 shell
# 通过在目标路径名前加上远端主机的名字（远端主机名为 remote-sys），来指定远端主机
[me@linuxbox ~]$ sudo rsync -av --delete --rsh=ssh /etc /home /usr/local remote-sys:/backup
```



rsync 可以被用来在网络间同步文件的第二种方式是通过使用 rsync 服务器

```bash
[me@linuxbox ~]$ mkdir fedora-devel
[me@linuxbox ~]$ rsync -av -delete rsync://rsync.gtlib.gatech.edu/fedora-linux-
 core/development/i386/os fedora-devel
```

