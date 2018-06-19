# 网络系统

- ping - 发送 ICMP ECHO_REQUEST 数据包到网络主机
- traceroute - 打印到一台网络主机的路由数据包
- netstat - 打印网络连接，路由表，接口统计数据，伪装连接，和多路广播成员
- ftp - 因特网文件传输程序
- wget - 非交互式网络下载器
- ssh - OpenSSH SSH 客户端（远程登录程序）

### 检查和监测网络

最基本的网络命令是 ping。这个 ping 命令发送一个特殊的网络数据包，叫做 ICMP ECHO_REQUEST，到 一台指定的主机。大多数接收这个包的网络设备将会回复它，来允许网络连接验证。

注意：大多数网络设备（包括 Linux 主机）都可以被配置为忽略这些数据包。通常，这样做是出于网络安全 原因，部分地遮蔽一台主机免受一个潜在攻击者地侵袭。配置防火墙来阻塞 IMCP 流量也很普遍。

```bash
[me@linuxbox ~]$ ping linuxcommand.org
```

#### traceroute

```bash
[me@linuxbox ~]$ traceroute slashdot.org
```

显示从本地到指定主机 要经过的所有“跳数”的网络流量列表

对于那些 提供标识信息的路由器，我们能看到它们的主机名，IP 地址和性能数据，这些数据包括三次从本地到 此路由器的往返时间样本。对于那些没有提供标识信息的路由器（由于路由器配置，网络拥塞，防火墙等 方面的原因），我们会看到几个星号

#### netstat

使用“-ie”选项，我们能够查看系统中的网络接口

```bash
[me@linuxbox ~]$ netstat -ie
eth0    Link encap:Ethernet HWaddr 00:1d:09:9b:99:67
        inet addr:192.168.1.2 Bcast:192.168.1.255 Mask:255.255.255.0
        inet6 addr: fe80::21d:9ff:fe9b:9967/64 Scope:Link
        UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1
        RX packets:238488 errors:0 dropped:0 overruns:0 frame:0
        TX packets:403217 errors:0 dropped:0 overruns:0 carrier:0
        collisions:0 txqueuelen:100 RX bytes:153098921 (146.0 MB) TX
        bytes:261035246 (248.9 MB) Memory:fdfc0000-fdfe0000

lo      Link encap:Local Loopback
        inet addr:127.0.0.1 Mask:255.0.0.0
...
```

第一个，叫做 eth0，是 以太网接口，和第二个，叫做 lo，是内部回环网络接口，它是一个虚拟接口，系统用它来 “自言自语”

“UP”，说明这个网络接口已经生效，还要查看第二行中 inet addr 字段出现的有效 IP 地址。对于使用 DHCP（动态主机配置协议）的系统，在 这个字段中的一个有效 IP 地址则证明了 DHCP 工作正常。



“-r”选项会显示内核的网络路由表。这展示了系统是如何配置网络之间发送数据包的

```bash
[me@linuxbox ~]$ netstat -r
Kernel IP routing table
Destination     Gateway     Genmask         Flags    MSS  Window  irtt Iface

192.168.1.0     *           255.255.255.0   U        0    0          0 eth0
default         192.168.1.1 0.0.0.0         UG       0    0          0 eth0
```

IP 地址以零结尾是指网络，而不是独立主机， 所以这个目的地意味着局域网中的任何一台主机。

Gateway， 是网关（路由器）的名字或 IP 地址，用它来连接当前的主机和目的地的网络。 若这个字段显示一个星号，则表明不需要网关。

default。指的是发往任何表上没有列出的目的地网络的流量

### 网络中传输文件

#### ftp

以协议 ftp://开头,FTP（它的原始形式）并不是安全的

几乎因特网中所有 FTP 服务器 都是匿名的。一个匿名服务器能允许任何人使用注册名“anonymous”和无意义的密码登录系统

```bash
[me@linuxbox ~]$ ftp fileserver
Connected to fileserver.localdomain.
220 (vsFTPd 2.0.1)
Name (fileserver:me): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> cd pub/cd\_images/Ubuntu-8.04
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-rw-r-- 1 500 500 733079552 Apr 25 03:53 ubuntu-8.04- desktop-i386.iso
226 Directory send OK.
ftp> lcd Desktop
Local directory now /home/me/Desktop
ftp> get ubuntu-8.04-desktop-i386.iso
local: ubuntu-8.04-desktop-i386.iso remote: ubuntu-8.04-desktop-
i386.iso
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for ubuntu-8.04-desktop-
i386.iso (733079552 bytes).
226 File send OK.
733079552 bytes received in 68.56 secs (10441.5 kB/s)
ftp> bye
```

| 命令                             | 意思                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| ftp fileserver                   | 唤醒 ftp 程序，让它连接到 FTP 服务器，fileserver。           |
| anonymous                        | 登录名。输入登录名后，将出现一个密码提示。一些服务器将会接受空密码， 其它一些则会要求一个邮件地址形式的密码。如果是这种情况，试着输入 “user@example.com”。 |
| cd pub/cd_images/Ubuntu-8.04     | 跳转到远端系统中，要下载文件所在的目录下， 注意在大多数匿名的 FTP 服务器中，支持公共下载的文件都能在目录 pub 下找到 |
| ls                               | 列出远端系统中的目录。                                       |
| lcd Desktop                      | 跳转到本地系统中的 ~/Desktop 目录下。在实例中，ftp 程序在工作目录 ~ 下被唤醒。 这个命令把工作目录改为 ~/Desktop |
| get ubuntu-8.04-desktop-i386.iso | 告诉远端系统传送文件到本地。因为本地系统的工作目录 已经更改到了 ~/Desktop，所以文件会被下载到此目录。 |
| bye                              | 退出远端服务器，结束 ftp 程序会话。也可以使用命令 quit 和 exit。 |

#### lftp - 更好的 ftp

包括 多协议支持（包括 HTTP），若下载失败会自动地重新下载，后台处理，用 tab 按键来补全路径名

#### wget

若想从网络和 FTP 网站两者上都能下载数据，wget 是很有用处的。 不只能下载单个文件，多个文件，甚至整个网站都能下载

这个程序的许多选项允许 wget 递归地下载，在后台下载文件（你退出后仍在下载），能完成未下载 全的文件

### 与远程主机安全通信

#### ssh

首先，它要认证远端主机是否为它 所知道的那台主机（这样就阻止了所谓的“中间人”的攻击），其次，它加密了本地与远程主机之间 所有的通讯信息。

SSH 由两部分组成。SSH 服务端运行在远端主机上，在端口 22 上监听收到的外部连接，而 SSH 客户端用在本地系统中，用来和远端服务器通信。

大多数 Linux 发行版自带一个提供 SSH 功能的软件包，叫做 OpenSSH，来自于 BSD 项目。

安装 OpenSSH-server 软件包，配置，运行它， 并且（如果系统正在运行，或者系统在防火墙之后）它必须允许在 TCP 端口 22 上接收网络连接。

```bash
[me@linuxbox ~]$ ssh remote-sys
The authenticity of host 'remote-sys (192.168.1.4)' can't be
established.
RSA key fingerprint is
41:ed:7a:df:23:19:bf:3c:a5:17:bc:61:b3:7f:d9:bb.
Are you sure you want to continue connecting (yes/no)?
```

输入 exit 命令后，则关闭了远程连接



在远端系统中指定帐号名

```bash
[me@linuxbox ~]$ ssh bob@remote-sys
bob@remote-sys's password:
Last login: Sat Aug 30 13:03:21 2008
[bob@remote-sys ~]$ 
```

ssh 验证远端主机的真实性。如果远端主机不能成功地通过验证

有两种可能的情形会提示这些信息。第一，某个攻击者企图制造“中间人”袭击。这很少见， 因为每个人都知道 ssh 会针对这种状况发出警告。最有可能的罪魁祸首是远端系统已经改变了； 例如，它的操作系统或者是 SSH 服务器重新安装了。然而，为了安全起见，第一个可能性不应该 被轻易否定。当这条消息出现时，总要与远端系统的管理员查对一下。

使用文本编辑器（可能是 vim）从文件~/.ssh/known_hosts 中删除废弃的钥匙， 则 ssh 程序 就能够从远端系统接受新的身份验证凭据



ssh 程序也允许我们在远端系统中执行单个命令

```bash
# 在名为 remote-sys 的远端主机上，执行 free 命令:
[me@linuxbox ~]$ ssh remote-sys free
me@twin4's password:
            total   used       free     shared buffers cached

Mem:        775536  507184   268352          0  110068 154596

-/+ buffers/cache: 242520  533016
Swap: 0 1572856 0 110068 154596

[me@linuxbox ~]$
```

```bash
# 在远端系统中执行 ls 命令， 并把命令输出重定向到本地系统中的一个文件里面
# 使用了单引号。这样做是因为我们不想路径名展开操作在本地执行，而希望 它在远端系统中被执行
[me@linuxbox ~]$ ssh remote-sys 'ls \*' > dirlist.txt
me@twin4's password:
[me@linuxbox ~]$
```



> SSH 通道
>
> SSH 协议允许大多数 网络流量类型通过这条加密通道来被传送，在本地与远端系统之间创建一种 VPN
>
> 这个特性的最普遍的用法是允许传递 X 窗口系统流量。在运行着 X 服务端的系统（也就是， 能显示 GUI 的机器）上，能登录远端系统并运行一个 X 客户端程序（一个图形化应用）， 而应用程序的显示结果出现在本地



假设我们正坐在一台名为 linuxbox 的 Linux 系统前，且系统中运行着 X 服务端，现在我们想要在名为 remote-sys 的远端系统中 运行 xload 程序，但是要在我们的本地系统中看到这个程序的图形化输出。我们可以这样做：

```bash
[me@linuxbox ~]$ ssh -X remote-sys
me@remote-sys's password:
Last login: Mon Sep 08 13:23:11 2008
[me@remote-sys ~]$ xload
```

这个 xload 命令在远端执行之后，它的窗口就会出现在本地。在某些系统中，你可能需要 使用 “－Y” 选项，而不是 “－X” 选项来完成这个操作。



#### scp 和 sftp

OpenSSH 软件包也包含两个程序，它们可以利用 SSH 加密通道在网络间复制文件

scp（安全复制）被用来复制文件，与熟悉的 cp 程序非常相似。最显著的区别就是 源或者目标路径名要以远端主机的名字，后跟一个冒号字符开头

```bash
[me@linuxbox ~]$ scp remote-sys:document.txt .
me@remote-sys's password:
document.txt
100%        5581        5.5KB/s         00:00
[me@linuxbox ~]$
```

和 ssh 命令一样，如果所需的远端主机帐户名与本地系统中的不一致， 那么你可以把用户名添加到远端主机名的开头：

```bash
[me@linuxbox ~]$ scp bob@remote-sys:document.txt .
```

第二个 SSH 文件复制程序是 sftp，顾名思义，它是 ftp 程序的安全替代品。

它不用明码形式来传递数据，它使用加密的 SSH 通道

 sftp 不需要远端系统中运行 FTP 服务端。它仅仅需要 SSH 服务端,任何一台能用 SSH 客户端连接的远端机器，也可当作类似于 FTP 的服务器来使用



```bash
[me@linuxbox ~]$ sftp remote-sys
Connecting to remote-sys...
me@remote-sys's password:
sftp> ls
ubuntu-8.04-desktop-i386.iso
sftp> lcd Desktop
sftp> get ubuntu-8.04-desktop-i386.iso
Fetching /home/me/ubuntu-8.04-desktop-i386.iso to ubuntu-8.04-
desktop-i386.iso
/home/me/ubuntu-8.04-desktop-i386.iso 100% 699MB 7.4MB/s 01:35
sftp> bye
```

> SFTP 协议被许多 Linux 发行版中的图形化文件管理器支持



>*Windows 中的 SSH 客户端*
>
>PuTTY

 