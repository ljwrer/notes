

# shell 环境

shell 在 shell 会话中保存着大量信息。这些信息被称为 (shell 的) 环境

- printenv - 打印部分或所有的环境变量
- set - 设置 shell 选项
- export — 导出环境变量，让随后执行的程序知道。
- alias - 创建命令别名

## 检查环境变量

```bash
# 输出友好地按照首字母顺序排列的环境变量
[me@linuxbox ~]$ set | less
```



#### 环境变量

| 变量    | 内容                                                         |
| ------- | ------------------------------------------------------------ |
| DISPLAY | 如果你正在运行图形界面环境，那么这个变量就是你显示器的名字。通常，它是 ":0"， 意思是由 X 产生的第一个显示器。 |
| EDITOR  | 文本编辑器的名字。                                           |
| SHELL   | shell 程序的名字。                                           |
| HOME    | 用户家目录。                                                 |
| LANG    | 定义了字符集以及语言编码方式。                               |
| OLD_PWD | 先前的工作目录。                                             |
| PAGER   | 页输出程序的名字。这经常设置为/usr/bin/less。                |
| PATH    | 由冒号分开的目录列表，当你输入可执行程序名后，会搜索这个目录列表。 |
| PS1     | Prompt String 1. 这个定义了你的 shell 提示符的内容。随后我们可以看到，这个变量 内容可以全面地定制。 |
| PWD     | 当前工作目录。                                               |
| TERM    | 终端类型名。类 Unix 的系统支持许多终端协议；这个变量设置你的终端仿真器所用的协议。 |
| TZ      | 指定你所在的时区。大多数类 Unix 的系统按照协调时间时 (UTC) 来维护计算机内部的时钟 ，然后应用一个由这个变量指定的偏差来显示本地时间。 |
| USER    | 你的用户名                                                   |

## 建立 shell 环境

bash 程序启动，并且会读取一系列称为启动文件的配置脚本， 这些文件定义了默认的可供所有用户共享的 shell 环境。然后是读取更多位于我们自己家目录中 的启动文件，这些启动文件定义了用户个人的 shell 环境

#### 登录 shell 会话的启动文件

| 文件            | 内容                                                         |
| --------------- | ------------------------------------------------------------ |
| /etc/profile    | 应用于所有用户的全局配置脚本。                               |
| ~/.bash_profile | 用户个人的启动文件。可以用来扩展或重写全局配置脚本中的设置。 |
| ~/.bash_login   | 如果文件 ~/.bash_profile 没有找到，bash 会尝试读取这个脚本。 |
| ~/.profile      | 如果文件 ~/.bash_profile 或文件 ~/.bash_login 都没有找到，bash 会试图读取这个文件。 这是基于 Debian 发行版的默认设置，比方说 Ubuntu。 |

#### 非登录 shell 会话的启动文件

| 文件             | 内容                                                         |
| ---------------- | ------------------------------------------------------------ |
| /etc/bash.bashrc | 应用于所有用户的全局配置文件。                               |
| ~/.bashrc        | 用户个人的启动文件。可以用来扩展或重写全局配置脚本中的设置。 |

### 启动文件的内容

```bash
# .bash_profile
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
. ~/.bashrc
fi
# User specific environment and startup programs
PATH=$PATH:$HOME/bin
export PATH
```

PATH 变量经常（但不总是，依赖于发行版）在 /etc/profile 启动文件中设置，通过这些代码：

```bash
PATH=$PATH:$HOME/bin
```

export命令告诉 shell 让这个 shell 的子进程可以使用 PATH 变量的内容

```bash
export PATH
```



## 修改 shell 环境

按照通常的规则，添加目录到你的 PATH 变量或者是定义额外的环境变量，要把这些更改放置到 .bash_profile 文件中。 对于其它的更改，要放到 .bashrc 文件中。

> 备份文件的名字无关紧要，只要选择一个容易理解的文件名。扩展名 “.bak”、”.sav”、 “.old”和 “.orig” 都是用来指示备份文件的流行方法

```bash
umask 0002
export HISTCONTROL=ignoredups
export HISTSIZE=1000
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
```

| 文本行                           | 含义                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| umask 0002                       | 设置掩码来解决共享目录的问题。                               |
| export HISTCONTROL=ignoredups    | 使得 shell 的历史记录功能忽略一个命令，如果相同的命令已被记录。 |
| export HISTSIZE=1000             | 增加命令历史的大小，从默认的 500 行扩大到 1000 行。          |
| alias l.='ls -d .* --color=auto' | 创建一个新命令，叫做'l.'，这个命令会显示所有以点开头的目录项。 |
| alias ll='ls -l --color=auto'    | 创建一个叫做'll'的命令，这个命令会显示长格式目录列表。       |

> Shell 脚本和 bash 启动文件都使用 “#” 符号来开始注释

### 激活修改

```Bash
[me@linuxbox ~]$ source .bashrc
```

