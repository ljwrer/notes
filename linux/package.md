# 软件包管理

大多数发行版分别属于两大包管理技术阵营： Debian 的”.deb”，和红帽的”.rpm”。也有一些重要的例外，比方说 Gentoo， Slackware，和 Foresight，但大多数会使用这两个基本系统中的一个。

##### 主要的包管理系统家族

| 包管理系统           | 发行版 (部分列表)                                            |
| -------------------- | ------------------------------------------------------------ |
| Debian Style (.deb)  | Debian, Ubuntu, Xandros, Linspire                            |
| Red Hat Style (.rpm) | Fedora, CentOS, Red Hat Enterprise Linux, OpenSUSE, Mandriva, PCLinuxOS |

### 包文件

包管理系统中软件的基本单元是包文件。包文件是一个构成软件包的文件压缩集合。一个软件包 可能由大量程序以及支持这些程序的数据文件组成。除了安装文件之外，软件包文件也包括 关于这个包的元数据，如软件包及其内容的文本说明。另外，许多软件包还包括预安装和安装后脚本， 这些脚本用来在软件安装之前和之后执行配置任务。

### 资源库

系统发行版的用户可以在一个中心资源库中得到这些软件包，这个资源库可能 包含了成千上万个软件包，每一个软件包都是专门为这个系统发行版建立和维护的。

### 依赖性

程序很少独立工作；他们需要依靠其他程序的组件来完成他们的工作。程序所共有的活动，如输入/输出， 就是由一个被多个程序调用的子例程处理的。这些子例程存储在动态链接库中。动态链接库为多个程 序提供基本服务。如果一个软件包需要一些共享的资源，如一个动态链接库，它就被称作有一个依赖。 现代的软件包管理系统都提供了一些依赖项解析方法，以确保安装软件包时，其所有的依赖也被安装。

### 上层和底层软件包工具

软件包管理系统通常由两种工具类型组成：底层工具用来处理这些任务，比方说安装和删除软件包文件， 和上层工具，完成元数据搜索和依赖解析。

##### 包管理工具

| 发行版                                   | 底层工具 | 上层工具          |
| ---------------------------------------- | -------- | ----------------- |
| Debian-Style                             | dpkg     | apt-get, aptitude |
| Fedora, Red Hat Enterprise Linux, CentOS | rpm      | yum               |

### 查找资源库中的软件包

| 风格    | 命令                                           |
| ------- | ---------------------------------------------- |
| Debian  | apt-get update; apt-cache search search_string |
| Red Hat | yum search search_string                       |

### 从资源库中安装一个软件包

| 风格    | 命令                                         |
| ------- | -------------------------------------------- |
| Debian  | apt-get update; apt-get install package_name |
| Red Hat | yum install package_name                     |

```bash
apt-get update; apt-get install emacs 
```

### 通过软件包文件来安装软件

| 风格    | 命令                        |
| ------- | --------------------------- |
| Debian  | dpkg --install package_file |
| Red Hat | rpm -i package_file         |

```bash
rpm -i emacs-22.1-7.fc7-i386.rpm
```

> 因为这项技术使用底层的 rpm 程序来执行安装任务，所以没有运行依赖解析。 如果 rpm 程序发现缺少了一个依赖，则会报错并退出。

### 卸载软件

| 风格    | 命令                        |
| ------- | --------------------------- |
| Debian  | apt-get remove package_name |
| Red Hat | yum erase package_name      |

```bash
apt-get remove emacs
```

### 经过资源库来更新软件包

| 风格    | 命令                            |
| ------- | ------------------------------- |
| Debian  | apt-get update; apt-get upgrade |
| Red Hat | yum update                      |

```bash
apt-get update; apt-get upgrade
```

### 经过软件包文件来升级软件

| 风格    | 命令                        |
| ------- | --------------------------- |
| Debian  | dpkg --install package_file |
| Red Hat | rpm -U package_file         |

```bash
rpm -U emacs-22.1-7.fc7-i386.rpm
```

> rpm 程序安装一个软件包和升级一个软件包所用的选项是不同的，而 dpkg 程序所用的选项是相同的

### 列出所安装的软件包

| 风格    | 命令        |
| ------- | ----------- |
| Debian  | dpkg --list |
| Red Hat | rpm -qa     |

### 确定是否安装了一个软件包

| 风格    | 命令                       |
| ------- | -------------------------- |
| Debian  | dpkg --status package_name |
| Red Hat | rpm -q package_name        |

```bash
dpkg --status emacs
```

### 显示所安装软件包的信息

| 风格    | 命令                        |
| ------- | --------------------------- |
| Debian  | apt-cache show package_name |
| Red Hat | yum info package_name       |

```bash
apt-cache show emacs
```

### 查找安装了某个文件的软件包

| 风格    | 命令                    |
| ------- | ----------------------- |
| Debian  | dpkg --search file_name |
| Red Hat | rpm -qf file_name       |

```bash
rpm -qf /usr/bin/vim
```

