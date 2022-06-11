- #### [apt 和 apt-get的区别](https://blog.csdn.net/nzjdsds/article/details/86290329)

1. 其实apt和npm和maven本质是一样的，都是**包管理工具**嘛，具有自己的语法。但是这里要细化一下，应该叫做**软件**包管理系统，这么说是区别于如maven、pip管理的开发包。

2. > Debian 使用一套名为 [Advanced Packaging Tool](https://wiki.debian.org/Apt)（APT）的工具来管理这种包系统

   > apt 命令的引入就是为了解决命令过于分散的问题，它包括了 apt-get 命令出现以来使用最广泛的功能选项，以及 apt-cache 和 apt-config 命令中很少用到的功能。

   > 简单来说就是：apt = apt-get、apt-cache 和 apt-config 中最常用命令选项的集合。

- #### [Linux软件包管理基本操作入门](https://www.sysgeek.cn/linux-package-management/)

1. > Debian 及其衍生产品如：Ubuntu、Linux Mint 和 Raspbian 的包格式为**.deb**文件，APT 是最常见包操作命令，如：搜索库、安装包及其依赖和管理升级。**而要直接安装现成.deb包时需要使用dpkg命令**。

   > CentOS、Fedora 及 Red Hat 系列 Linux 使用**RPM包文件**，并使用yum命令管理包文件及与软件库交互。

   已经说到这份上了。原来Ubuntu是这样的地位。需要注意两点：1. .deb和.rpm不同系列的包文件后缀；2. dpkg命令不熟悉，请注意。

   我在jdk的下载官网上面也看到了[链接](https://www.oracle.com/java/technologies/javase-jdk14-downloads.html)中：

   <img src="C:\Users\liuhaodong\AppData\Roaming\Typora\typora-user-images\image-20200910231851305.png" alt="image-20200910231851305" style="zoom:80%;" />

   