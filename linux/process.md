# 9.进程

* ps – 报告当前进程快照

* top – 显示任务

* jobs – 列出活跃的任务

* bg – 把一个任务放到后台执行

* fg – 把一个任务放到前台执行

* kill – 给一个进程发送信号

* killall – 杀死指定名字的进程

* shutdown – 关机或重启系统

系统启动的时，运行一个init程序。init依次地运行一系列的称为 init 脚本的 shell 脚本（位于/etc），它们可以启动所有的系统服务。 

许多系统服务以守护（daemon）程序的形式实现，守护程序仅在后台运行，没有任何用户界面。 这样，即使我们没有登录系统，至少系统也在忙于执行一些例行事务。

在进程方案中，一个程序可以发动另一个程序被表述为一个父进程可以产生一个子进程

内核维护每个进程的信息，以此来保持事情有序。例如，系统分配给每个进程一个数字，这个数字叫做 进程 ID 或 PID。PID 号按升序分配，init 进程的 PID 总是1。内核也对分配给每个进程的内存和就绪状态进行跟踪以便继续执行这个进程。 像文件一样，进程也有所有者和用户 ID，有效用户 ID，等等。

## 查看进程
### 查看当前终端进程
```
[me@linuxbox ~]$ ps
PID TTY           TIME CMD
5198 pts/1    00:00:00 bash
10129 pts/1   00:00:00 ps
```

>TTY 是 “Teletype” 的简写，是指进程的控制终端
>TIME 字段表示 进程所消耗的 CPU 时间数量

### 查看所有终端进程
```
[me@linuxbox ~]$ ps x
PID TTY   STAT   TIME COMMAND
2799 ?    Ssl    0:00 /usr/libexec/bonobo-activation-server –ac
2820 ?    Sl     0:01 /usr/libexec/evolution-data-server-1.10 --

and many more...
```
STAT 是 “state” 的简写，它揭示了进程当前状态

```
[me@linuxbox ~]$ ps aux
USER   PID  %CPU  %MEM     VSZ    RSS  TTY   STAT   START   TIME  COMMAND
root     1   0.0   0.0    2136    644  ?     Ss     Mar05   0:31  init
root     2   0.0   0.0       0      0  ?     S&lt;     Mar05   0:00  [kt]

and many more...
```
这个选项组合，能够显示属于每个用户的进程信息。使用这个选项，可以唤醒 “BSD 风格” 的输出结果

#### 进程状态

<table class="multi">
<thead>
<tr>
<th class="title">状态</th>
<th class="title">含义</th>
</tr>
</thead>
<tbody>
<tr>
<td valign="top" width="15%">R</td>
<td valign="top">运行中。这意味着，进程正在运行或准备运行。
</td>
</tr>
<tr>
<td valign="top">S</td>
<td valign="top">正在睡眠。进程没有运行，而是，正在等待一个事件，
比如说，一个按键或者网络分组。
</td>
</tr>
<tr>
<td valign="top">D</td>
<td valign="top">不可中断睡眠。进程正在等待 I/O，比方说，一个磁盘驱动器的 I/O。</td>
</tr>
<tr>
<td valign="top">T</td>
<td valign="top">已停止. 已经指示进程停止运行。稍后介绍更多。</td>
</tr>
<tr>
<td valign="top">Z</td>
<td
valign="top">一个死进程或“僵尸”进程。这是一个已经终止的子进程，但是它的父进程还没有清空它。
（父进程没有把子进程从进程表中删除）</td>
</tr>
<tr>
<td valign="top"><</td>
<td
valign="top">一个高优先级进程。这可能会授予一个进程更多重要的资源，给它更多的 CPU 时间。
进程的这种属性叫做 niceness。具有高优先级的进程据说是不好的（less nice），
因为它占用了比较多的 CPU 时间，这样就给其它进程留下很少时间。
</td>
</tr>
<tr>
<td valign="top">N</td>
<td valign="top">低优先级进程。
一个低优先级进程（一个“好”进程）只有当其它高优先级进程被服务了之后，才会得到处理器时间。
</td>
</tr>
</tbody>
</table>

#### BSD 风格的 ps 命令列标题
<table class="multi">
</caption>
<thead>
<tr>
<th class="title">标题</th>
<th class="title">含义</th>
</tr>
</thead>
<tbody>
<tr>
<td valign="top" width="15%">USER</td>
<td valign="top">用户 ID. 进程的所有者。
</td>
</tr>
<tr>
<td valign="top">%CPU</td>
<td valign="top">以百分比表示的 CPU 使用率</td>
</tr>
<tr>
<td valign="top">%MEM</td>
<td valign="top">以百分比表示的内存使用率</td>
</tr>
<tr>
<td valign="top">VSZ</td>
<td valign="top">虚拟内存大小</td>
</tr>
<tr>
<td valign="top">RSS</td>
<td valign="top">进程占用的物理内存的大小，以千字节为单位。</td>
</tr>
<tr>
<td valign="top">START</td>
<td valign="top">进程启动的时间。若它的值超过24小时，则用天表示。</td>
</tr>
</tbody>
</table>

## 用 top 命令动态查看进程
```
[me@linuxbox ~]$ top
```
ps命令只能提供 ps 命令执行时刻的机器状态快照

top 程序以进程活动顺序显示连续更新的系统进程列表。（默认情况下，每三秒钟更新一次）

top 显示结果由两部分组成： 最上面是系统概要，下面是进程列表，以 CPU 的使用率排序

#### top 命令信息字段
<table class="multi">
<thead>
<tr>
<th class="title">行号</th>
<th class="title">字段</th>
<th class="title">意义</th>
</tr>
</thead>
<tbody>
<tr>
<td valign="top" width="10%">1</td>
<td valign="top" width="15%">top</td>
<td class="title">程序名。</td>
</tr>
<tr>
<td valign="top"></td>
<td valign="top">14:59:20</td>
<td valign="top">当前时间。
</td>
</tr>
<tr>
<td valign="top"></td>
<td valign="top">up 6:30 </td>
<td valign="top">这是正常运行时间。它是计算机从上次启动到现在所运行的时间。
在这个例子里，系统已经运行了六个半小时。  </td>
</tr>
<tr>
<td valign="top"></td>
<td valign="top">2 users</td>
<td valign="top">有两个用户登录系统。</td>
</tr>
<tr>
<td valign="top"></td>
<td valign="top">load average: </td>
<td
valign="top">加载平均值是指，等待运行的进程数目，也就是说，处于可以运行状态并共享 CPU 的进程个数。
这里展示了三个数值，每个数值对应不同的时间段。第一个是最后60秒的平均值，
下一个是前5分钟的平均值，最后一个是前15分钟的平均值。若平均值低于1.0，则指示计算机
工作不忙碌。</td>
</tr>
<tr>
<td valign="top">2</td>
<td valign="top">Tasks:</td>
<td valign="top">总结了进程数目和这些进程的各种状态。</td>
</tr>
<tr>
<td valign="top">3</td>
<td valign="top">Cpu(s):</td>
<td valign="top">这一行描述了 CPU 正在进行的活动的特性。</td>
</tr>
<tr>
<td valign="top"></td>
<td valign="top">0.7%us </td>
<td valign="top">0.7% 的 CPU 被用于用户进程。这意味着进程在内核之外。</td>
</tr>
<tr>
<td valign="top"></td>
<td valign="top">1.0%sy </td>
<td valign="top">1.0%的 CPU 时间被用于系统（内核）进程。
</td>
</tr>
<tr>
<td valign="top"></td>
<td valign="top">0.0%ni </td>
<td valign="top">0.0%的 CPU 时间被用于"nice"（低优先级）进程。
</td>
</tr>
<tr>
<td valign="top"></td>
<td valign="top">98.3%id </td>
<td valign="top">98.3%的 CPU 时间是空闲的。</td>
</tr>
<tr>
<td valign="top"></td>
<td valign="top">0.0%wa </td>
<td valign="top">0.0%的 CPU 时间来等待 I/O。</td>
</tr>
<tr>
<td valign="top">4</td>
<td valign="top">Mem:</td>
<td valign="top">展示物理内存的使用情况。</td>
</tr>
<tr>
<td valign="top">5</td>
<td valign="top">Swap:</td>
<td valign="top">展示交换分区（虚拟内存）的使用情况。
</td>
</tr>
</tbody>
</table>

## 控制进程
### 中断
按下 Ctrl-c

### 放置到后台(执行)
在程序命令之后，加上”&”字符
```
[me@linuxbox ~]$ xlogo &
# 已经启动了 任务号为1（“［1］”），PID 为28236的程序
[1] 28236
[me@linuxbox ~]$
```

### 任务列表
jobs 命令，我们可以看到任务列表：
```
[me@linuxbox ~]$ jobs
[1]+ Running            xlogo &
```

### 返回到前台
fg 命令之后，跟随着一个百分号和任务序号（叫做 jobspec,单任务时可缺省)
```
[me@linuxbox ~]$ jobs
[1]+ Running        xlogo &
[me@linuxbox ~]$ fg %1
xlogo
```

### 停止
停止一个进程，而不是终止它，通常是为了允许前台进程被移动到后台

输入 Ctrl-z，可以停止一个前台进程

```
[me@linuxbox ~]$ xlogo
[1]+ Stopped                 xlogo
[me@linuxbox ~]$
```
### 移到后台
```
[me@linuxbox ~]$ bg %1
[1]+ xlogo &
[me@linuxbox ~]$
```

## Signals
kill 命令被用来“杀死”程序
```
[me@linuxbox ~]$ xlogo &
[1] 28401
[me@linuxbox ~]$ kill 28401
[1]+ Terminated               xlogo
```
```
kill [pid]|[jobspec]
```

kill 命令不是真的“杀死”程序，而是给程序 发送信号

在使用 Ctrl-c 的情况下，会发送一个叫做 INT（中断）的信号；当使用 Ctrl-z 时，则发送一个叫做 TSTP（终端停止）的信号

```
kill [-signal] PID|jobspec...
```
默认情况下，发送 TERM（终止）信号

### 常用信号
<table class="multi">
<tr>
<th class="title">编号</th>
<th class="title">名字</th>
<th class="title">含义</th>
</tr>
<tr>
<td valign="top" width="10%">1</td>
<td valign="top" width="10%">HUP</td>
<td valign="top">挂起。这是美好往昔的残留部分，那时候终端机通过电话线和调制解调器连接到
远端的计算机。这个信号被用来告诉程序，控制的终端机已经“挂起”。
通过关闭一个终端会话，可以展示这个信号的作用。在当前终端运行的前台程序将会收到这个信号并终止。
<p>许多守护进程也使用这个信号，来重新初始化。这意味着，当一个守护进程收到这个信号后，
这个进程会重新启动，并且重新读取它的配置文件。Apache 网络服务器守护进程就是一个例子。</p>
</td>
</tr>
<tr>
<td valign="top">2</td>
<td valign="top">INT</td>
<td valign="top">中断。实现和 Ctrl-c 一样的功能，由终端发送。通常，它会终止一个程序。
</td>
</tr>
<tr>
<td valign="top">9</td>
<td valign="top">KILL</td>
<td
valign="top">杀死。这个信号很特别。尽管程序可能会选择不同的方式来处理发送给它的
信号，其中也包含忽略信号，但是 KILL 信号从不被发送到目标程序。而是内核立即终止
这个进程。当一个进程以这种方式终止的时候，它没有机会去做些“清理”工作，或者是保存工作。
因为这个原因，把 KILL 信号看作最后一招，当其它终止信号失败后，再使用它。
</td>
</tr>
<tr>
<td valign="top">15</td>
<td valign="top">TERM</td>
<td valign="top">终止。这是 kill 命令发送的默认信号。如果程序仍然“活着”，可以接受信号，那么
这个它会终止。 </td>
</tr>
<tr>
<td valign="top">18</td>
<td valign="top">CONT</td>
<td valign="top">继续。在一个停止信号后，这个信号会恢复进程的运行。</td>
</tr>
<tr>
<td valign="top">19</td>
<td valign="top">STOP</td>
<td
valign="top">停止。这个信号导致进程停止运行，而不是终止。像 KILL 信号，它不被
发送到目标进程，因此它不能被忽略。
</td>
</tr>
</table>

```
[me@linuxbox ~]$ xlogo &
[1] 13546
[me@linuxbox ~]$ kill -1 13546
[1]+ Hangup         xlogo
```

进程，和文件一样，拥有所有者，所以为了能够通过 kill 命令来给进程发送信号， 你必须是进程的所有者（或者是超级用户）

### 其它常用信号
<table class="multi">
<tr>
<th class="title">编号</th>
<th class="title">名字</th>
<th class="title">含义</th>
</tr>
<tr>
<td valign="top" width="10%">3</td>
<td valign="top" width="10%">QUIT</td>
<td valign="top">退出</td>
</tr>
<tr>
<td valign="top">11</td>
<td valign="top">SEGV</td>
<td
valign="top">段错误。如果一个程序非法使用内存，就会发送这个信号。也就是说，
程序试图写入内存，而这个内存空间是不允许此程序写入的。</td>
</tr>
<tr>
<td valign="top">20</td>
<td valign="top">TSTP</td>
<td
valign="top">终端停止。当按下 Ctrl-z 组合键后，终端发送这个信号。不像 STOP 信号，
TSTP 信号由目标进程接收，且可能被忽略。</td>
</tr>
<tr>
<td valign="top">28</td>
<td valign="top">WINCH</td>
<td valign="top">改变窗口大小。当改变窗口大小时，系统会发送这个信号。
一些程序，像 top 和 less 程序会响应这个信号，按照新窗口的尺寸，刷新显示的内容。
</td>
</tr>
</table>

完整的信号列表：
```
[me@linuxbox ~]$ kill -l
```

## 通过 killall 命令给多个进程发送信号
```
killall [-u user] [-signal] name...
```
```
[me@linuxbox ~]$ xlogo &
[1] 18801
[me@linuxbox ~]$ xlogo &
[2] 18802
[me@linuxbox ~]$ killall xlogo
[1]- Terminated                xlogo
[2]+ Terminated                xlogo
```

## 其它与进程相关的命令

<table class="multi">
<tr>
<th class="title">命令名</th>
<th class="title">命令描述</th>
</tr>
<tr>
<td valign="top" width="15%">pstree </td>
<td valign="top">输出一个树型结构的进程列表，这个列表展示了进程间父/子关系。</td>
</tr>
<tr>
<td valign="top">vmstat</td>
<td valign="top">输出一个系统资源使用快照，包括内存，交换分区和磁盘 I/O。
为了看到连续的显示结果，则在命令名后加上更新操作延时的时间（以秒为单位）。例如，“vmstat 5”。
，按下 Ctrl-c 组合键, 终止输出。</td>
</tr>
<tr>
<td valign="top">xload</td>
<td valign="top">一个图形界面程序，可以画出系统负载随时间变化的图形。</td>
</tr>
<tr>
<td valign="top">tload</td>
<td valign="top">与 xload 程序相似，但是在终端中画出图形。使用 Ctrl-c，来终止输出。</td>
</tr>
</table>