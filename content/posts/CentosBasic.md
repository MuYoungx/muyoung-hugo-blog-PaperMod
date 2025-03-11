---
title: Centos基础语法
published: 2025-02-28T14:56:11+08:00
summary: "Centos基础语法"
cover:
  image: "文章封面图。也支持HTTPS"
tags: [Linux, CentOS]
categories: '经验分享'
draft: false 
lang: ''
---

<h1 id="EJYzu">用户管理</h1>
<h4 id="YK74k">添加用户</h4>
useradd用户名

useradd -d指定目录

<h4 id="rPSl6">查询用户信息</h4>
id 用户名

<h4 id="Grote">删除用户</h4>
userdel用户名

删除用户milan，但是要保留家目录，userdelmilan

删除用户以及用户主目录，比如tom，userdel-r tom

修改用户密码 passwd 用户名

<h4 id="SqW0G">查看当前登录用户</h4>
who am i

<h4 id="KeK0v">修改用户的组</h4>
usermod -g用户组 用户名

创建一个组mojiao

把zwj放入mojiao

指令:usermod-g mojiao zwj

<h4 id="eBZzH">etc/passwd文件</h4>
用户(user)的配置文件，记录用户的各种信息

每行的含义:用户名：口令：用户标识号：组标识号：注释性描述：主目录：登录shell

<h4 id="CwAi9">etc/shadow文件</h4>
口令的配置文件

每行的含义：登录名：加密口令：最后一次修改时间：最小时间间隔：最大时间间隔：警告时间：不活动时间：失效时间：标志

<h4 id="ecLjP">etc/group文件</h4>
组(group)的配置文件，记录linux包含的组的信息

每行含义：组名：口令：组标识号：组内用户列表

<h1 id="PbRtY">文件目录</h1>
<h4 id="rZP5j">cd指令</h4>
cd[参数]

cd ~或者cd回到自己的家目录

cd..回到当前目录的上一级目录

mkdir创建目录

-p创建多级目录

rmdir删除目录

rmdir[选项]要删除的空目录

rmdir删除的是空目录，如果目录下有内容时无法删除的

如果删除非空目录使用rm-rf

touch创建空文件

touch 文件名

<h4 id="OGzi0">cp指令 复制</h4>
cp[选项]source dest

-r递归复制整个文件夹

<h4 id="RTI0J">mv指令 移动</h4>
mv移动文件与目录或重命名

mv源目录与新目录相同(重命名)

mv/temp/movefile/targetFolder(移动文件)

cat指令 查看文件内容但不能修改

cat[选项]要查看的文件

-n:显示行号

cat只能浏览文件，而不能修复文件，为了浏览方便，一般会带上管道命令|more

cat -n/etc/profile |more [进行]

<h4 id="RwV0o">more指令</h4>
操作说明

more要查看的文件

空格键代表向下翻一页

enter代表向下翻一行

q代表立即离开more，不再显示该文件内容

ctrl+f向下滚动一屏

ctrl+b返回上一屏

=输出当前行的行号

:f输出文件名和当前行的行号

less指令

less指令用来分屏查看文件内容，它的功能与more指令类似，但是比more指令更加强大，支持各位显示终端。less指令在显示文件内容时，并不是一次将整个文件加载之后才显示，而是根据显示需要加载内容，对于显示大型文件具有较高的效率

操作说明

空白键 向下翻动一页

pagedown 向下反动一页

pageup 向上翻动一页

/字串 向下搜寻【字串】的功能；n:向下查找；N：向上查找

?字串 向上搜寻【字串】的功能；n：向上查找；N向下查找

q 离开less这个程序

echo指令

echo输出内容到控制台

echo [选项][输出内容]

head指令

head用于显示文件的开头部分内容，默认情况下显示文件的前十行内容

head文件名 查看文件头10行内容

head -n 5 文件名 查看文件头5行内容 5可以是任意行数

tail指令

tail用于输出文件中尾部的内容，默认情况下tail指令显示文件的前10行内容

tail 文件名 查看文件尾10行内容

tail -n 5 文件名 查看文件尾5行内容，5可以是任意行数

tail -f 文件名 实时追踪该文档的所有更新

> 指令
>
> 追加
>

ls -l >文件 列表的内容写入a.txt中(覆盖写)

ls -al>>文件列表的内容追加到文件aa.txt的末尾

cat文件1>文件2 将文件1的内容覆盖到文件2

echo “内容”>>文件

ls -l /home>/home/info.txt如果info.txt没有，则会创建

ln 指令 软连接也称符号链接类似于windows的快捷方式

ln -s[源文件或目录][软链接名] 给源文件创建一个软连接

history指令

查看已执行过历史命令，也可以执行历史指令

date指令-显示当前日期

date 显示当前时间

date+%Y 显示当前年份

date+%m 显示当前月份

date+%d 显示当前是哪一天

date "+%Y-%m-%d%H:%M:%S" 显示年月日时分秒

设置日期 date -s字符串时间

cal指令

cal [选项]

ls用法

ls [选项] [文件]

【 -f 】不排序目录内容；按它们在磁盘上存储的顺序列出。同时启 动“ -a ”选项

【 -1 】每行显示一条记录，即单列展示数据

【 -l 】长列表显示文件和目录，包括文件类型、大小、修改日期和时间、权限等信息

【 -lh 】人性化显示文件大小

【 -F 】 使用不同特殊字符归类不同的文件类型

【 -ld 】长列表格式列出某个目录的信息

【 -R 】递归地列出子目录的内容

【 -ltr 】将长列表格式按文件或目录的修改时间 倒序地 列出文件和目录

【 -ls 】将长列表格式按文件大小顺序列出文件和目录

【 -a 】显示包括隐藏文件或目录在内的所有文件和目录，包括 “.“（当前目录），“…“（父目录

【 -A 】显示包括隐藏文件或目录在内的所有文件和目录，但不列出 “.” (目前目录)及 “…” (父目录)

【 -i 】显示文件或目录的 inode 编号，可能会用在系统维护操作时

【 -n 】显示uid 和 gid ，代替显示所有者和用户组

<h1 id="zsOGR">搜索查找</h1>
<h4 id="m4WFm">find指令</h4>
find指令将从指定目录向下递归地遍历其各个子目录，将满足条件的文件或者目录显示在终端。

find[搜索范围] [选项]

-name按照指定的文件名查找模式查找文件

-user查找属于指定用户名所有文件

-size按照指定的文件大小查找文件+n大于 -n小于 n等于不加n也是等于 单位有k,M,G

<h4 id="xvqIU">locate指令</h4>
locate指令可以快速定位文件路径。locate指令利用事先建立的系统中所有文件名称及路径的locate数据库实现快速定位给定的文件。locate指令无需遍历整个文件系统，查询速度较快，为了保证数据查询结果的准确度管理员必须定期更新locate数据库

由于locate指令基于数据库进行查询，所以第一次运行前，必须使用updatedb指令创建locate数据库

<h4 id="E753h">which指令</h4>
which指令可以查看某个指令在哪个目录下

<h4 id="oZneH">grep指令和管道符号|</h4>
grep过滤查找，管道符"|",表示将前一个命令的处理结果输出传递给后面的命令处理。

grep[选项]查找内容 源文件

-n显示匹配行及行号。

-i忽略字母大小写

<h1 id="VXQVS">解压与压缩</h1>
<h4 id="zMS8y">gzip/gunzip指令</h4>
gzip用于压缩文件，gunzip用于解压

gzip文件压缩文件 只能将文件压缩为*.gz

gunzip文件.gz解压缩文件命令

<h4 id="aiSur">zip/unzip指令</h4>
zip用于压缩文件，unzip用于解压，这个在项目打包发布中很有用

zip [选项]XXX.zip 压缩内容

unzip[选项]XXX.zip 解压缩文件

zip常用选项

-r:递归压缩，即压缩目录

gunzip常用选项

-d指定解压后文件的存放目录

<h4 id="coGDI">tar指令</h4>
tar指令是打包指令，最后打包后的文件是.tar.gz的文件

tar[选项]XXX.tar.gz 打包内容

-c产生.tar打包文件

-v显示详细信息

-f指定压缩后的文件名

-z打包同时压缩

-x解压.tar文件

-C解压到的目录

```bash
tar -zxvf /home/myhome.tar.gz -C /opt/tmp2
```

<h1 id="i1gUv">文件与目录所有者</h1>
一般为文件的创建者，谁创建了该文件，就自然的成为该文件的所有者

查看文件的所有者

ls -ahl

修改文件所有者

chown 用户名文件名

-R递归修改

<h1 id="CE2Ut">组</h1>
组的创建

基本指令

groupadd 组名

文件/目录所在组

当某个用户创建了一个文件后，这个文件的所在组就是该用户所在的组

查看文件或目录所在组

ls -ahl 文件/目录名

修改文件所在组

chgrp 新组名 文件名

-R 递归修改

其他组

除文件的所有者和所在组的用户外，系统的其他用户都是文件的其他组

改变用户所在组

在添加用户时，可以指定将该用户添加到那个组中，同样的用root的管理权限可以改变某个用户所在的组

1.usermod -g 新组名 用户名

2.usermod -d 目录名 用户名 改变该用户登录的初始目录 用户需要有到新目录的权限

<h1 id="RUmYw">权限</h1>
基本介绍

ls-l中显示的内容如下:

-rw-r--r--.1 root root 0 6月 1 17:39 pig.txt

0-9位说明

1.第0位确定文件类型(d,-,l,c,b)

l是链接，相当于windows的快捷方式

d是目录，相当于windows的文件夹

c是字符设备文件，鼠标，键盘

b是块设备，比如硬盘

2.第1-3位确定所有者(该文件的所有者)拥有该文件的权限--User

3.第4-6位确定所属组(同用户组的)拥有该文件的权限，--Group

4.第7-9位确定其他用户拥有该文件的权限--Other

<h4 id="tG0ox">修改权限</h4>
通过chmod指令，可以修改文件或者目录的权限

第一种方式：+、-、=变更权限

u:所有者g:所有组 o:其他人 a:所有人(u、g、o的总和)

1）chmodu=rwx,g=rx,o=x 文件/目录名

2）chmodo+w 文件/目录名(给当前文件的其他用户单独添加写权限)

3）chmoda-x 文件/目录名(给当前文件的所有用户删除执行权限)

第二种方式：通过数字变更权限

r=4，w=2，x=1rwx=4+2+1=7

chmodu=rwx,g=rx,o=x 文件/目录名(相当于chmod 751 文件目录名)

<h4 id="EmjD6">修改文件所有者</h4>
chown 新所有者文件/目录 改变所有者

chown新所有者:新组 文件/目录 改变所有者和所在组

-R如果是目录，则使其下所有子文件或目录递归生效

<h1 id="QPMc2">定时任务调度</h1>
<h4 id="Sl07x">crontab 进行定时任务的设置</h4>
是指系统在某个时间执行的特定的命令或程序

crontab[选项]

-e编辑crontab定时任务

-l查询crontab任务

-r删除当前用户所有的crontab

参数细节说明

![](https://img.muyoung.com/202502281453916.png)

```bash
如*/1****ls-l /etc/ > /tmp/to.txt 意思说每小时的每分钟执行ls -l /etc/ > /tmp/to.txt
```

<h4 id="pAEFW">特殊符号的说明</h4>
![](https://img.muyoung.com/202502281454113.png)

<h4 id="LBFiX">at定时任务</h4>
![](https://img.muyoung.com/202502281454775.png)

ps -ef查看当前运行的所有进程

ps -ef |grep atd 检查atd是否在运行

at命令选项

![](https://img.muyoung.com/202502281454727.png)

Ctrl+D结束at命令的输入，输入两次才能退出

<h1 id="beEpf">磁盘分区机制</h1>
<h4 id="RH2uz">查看所有设备挂载情况</h4>
lsblk或者lsblk-f

虚拟机添加硬盘步骤2

分区命令 fdisk/dev/sdb

开始对/sdb分区

m 显示命令列表

p 显示磁盘分区同fdisk -l

n 新增分区

d 删除分区

w 写入并退出

说明：开始分区后输入n，新增分区，然后选择p，分区类型为主分区。两次回车默认剩余全部空间。最后输入w写入分区并退出，若不保存退出输入q

虚拟机添加硬盘步骤3

<h4 id="bnSEr">格式化磁盘</h4>
分区命令：mkfs-t ext4 /dev/sdb1

其中ext4是分区类型

虚拟机添加硬盘步骤4

挂载：将一个分区与一个目录联系起来

mount 设备名称挂载目录

例如：mount/dev/sdb1 /newdisk

卸载 umount设备名称或者挂载目录

例如：umount/dev/sdb1或者umount /newdisk

虚拟机添加硬盘步骤5

永久挂载：通过修改/etc/fstab实现挂载

添加完成后执行mount -a 即刻生效

磁盘整体使用情况查询

df -h

查询指定目录的磁盘使用情况

du -h /目录

查询指定目录的磁盘占用情况，默认为当前目录

-s指定目录占用大小汇总

-h 带计量单位

-a 含文件

--max-depth=1子目录深度

-c列出明细的同时，增加汇总值

![](https://img.muyoung.com/202502281454547.png)

<h1 id="C8RWX">网络配置</h1>
<h2 id="u2SQ1">查看网络ip和网关</h2>
+ 查看windows系统中的VMnet8网络配置(ipconfig指令)

![](https://img.muyoung.com/202502281454265.png)

+ 查看linux的网络配置(ifconfig指令)

![](https://img.muyoung.com/202502281455582.png)

+ 使用ping指令来判断网络连通性

![](https://img.muyoung.com/202502281455213.png)

<h2 id="XB0d2">linux网络环境配置</h2>
+ 第一种方式自动获取ip地址以及dns地址(DHCP)
+ 第二种方法(指定ip)  
说明：直接修改配置文件来指定ip，并可以连接到外网(程序员推荐)编辑  
vi /etc/sysconfig/network-scripts/ifcfg-ens33 将ip地址配置成静态的，  
比如ip地址为192.168.200.130  
ifcfg-ens33 文件说明

```bash
[root@localhost network-scripts]# cat ifcfg-ens33  
TYPE="Ethernet" #网卡类型（通常是Ethemet以太网）  
PROXY_METHOD="none" #代理方式：为关闭状态  
BROWSER_ONLY="no" #只是浏览器：否  
BOOTPROTO="static" #网卡的引导协议【static：静态IP dhcp：动态IP   none：不指定，不指定容易出现各种各样的网络受限】  
DEFROUTE="yes" #默认路由  
IPV4_FAILURE_FATAL="no" #是否开启IPV4致命错误检测  
IPV6INIT="yes" #IPV6是否自动初始化：是（现在还未用到IPV6，不会有任何影响）  
IPV6_AUTOCONF="yes" #IPV6是否自动配置：是（现在还未用到IPV6，不会有任何影响）  
IPV6_DEFROUTE="yes" #IPV6是否可以为默认路由：是（现在还未用到IPV6，不会有任何影响）  
IPV6_FAILURE_FATAL="no" #是否开启IPV6致命错误检测  
IPV6_ADDR_GEN_MODE="stable-privacy" #IPV6地址生成模型  
NAME="ens33" #网卡物理设备名称  
UUID="ab60d501-535b-49f5-a76b-3336a4120f64"#通用唯一识别码，每一个网卡都会有，不能重复，否则两台linux机器只有一台可上网  
DEVICE="ens33" #网卡设备名称，必须和‘NAME’值一样  
ONBOOT="yes" #是否开机启动，要想网卡开机就启动或通过 `systemctl restart network`控制网卡,必须设置为 `yes`  
IPADDR=192.168.137.129 # 本机IP  
NETMASK=255.255.255.0 #子网掩码  
GATEWAY=192.168.137.2 #默认网关  
DNS1=8.8.8.8#  
DNS2=8.8.8.5#  
ZONE=public#  
[root@localhost network-scripts]# service network restart #重启网卡  
Restarting network (via systemctl):                       [ 确定 ]  
[root@localhost network-scripts]#
```

设置主机名和hosts映射

设置主机名

+ 为了方便记忆，可以给linux系统设置主机名，也可以根据需要修改主机名
+ 指令hostname 查看主机名
+ 修改文件在 /etc/hostname指定
+ 修改后重启生效

设置hosts映射

作用:通过主机名找到(比如ping)某个linux系统

+ Windows  
在C:\Windows\System32\driver\etc\hosts文件指定即可  
案例:192.168.200.130 admin-pc
+ Linux  
在/etc/hosts文件指定  
案例:192.168.200.1 admin-pc

主机名解析过程分析(Hosts、DNS)

+ hosts是什么  
一个文本文件，用来记录IP和HOSTNAME(主机名)的映射关系
+ DNS
+ DNS，就是Domain Name System的缩写，翻译过来就是域名系统
+ 是互联网上作为域名和IP地址相互映射的一个分布式数据库

<h1 id="xWcr5">进程管理</h1>
+ 在Linux中，每个执行的程序都称为一个进程。每一个进程都分配一个ID号(pid，进程号)
+ 每个进程都可以以两种方式存在，前台和后台，所谓前台进程就是用户目前屏幕上可以  
进行操作的，后台进程则是实际在操作，但由于屏幕上无法看到的进程，通常使用后台  
方式执行
+ 一般系统的服务都是以后台进程的方式存在，而且都会常驻在系统中，直到关机才结束

PS命令 显示系统执行的进程

基本介绍

ps命令是用来查看目前系统中，有哪些正在执行，以及它们执行的状况，可以不加任何参数

ps-a 显示当前终端的所有进程信息

ps-u 以用户的格式显示进程信息

ps-x 显示后台进程运行的参数

ps详解

+ 指令：ps -aux|grep sshd,比如我看看有没有sshd服务
+ 指令说明
+ System V展示风格
+ USER 用户名称
+ PID 进程号
+ %CPU进程占用CPU的百分比
+ %MEM 进程占用物理内存的百分比
+ VSZ 进程占用的虚拟内存大小(单位:KB)
+ RSS 进程占用的物理内存大小(单位:KB)
+ TT 终端名称，缩写
+ STAT 进程状态，其中S-睡眠，s-表示该进程是会话的先导进程，N-表示进程拥有比普通优先级更低的优先级，R-正在运行，D-短期等待，Z-僵死进程，T-被跟踪或者被停止
+ STARTED 进程的启动时间
+ TIME CPU时间，即进程使用CPU的总时间
+ COMMAND 启动进程所用的命令和参数，如果过长会被截断显示

![](https://img.muyoung.com/202502281455363.png)

<h2 id="bwYK1">终止进程 kill和killall</h2>
常用选项

+ kill [选项]进程号
+ killall 进程名称

-9：表示强迫进程立即停止

终止多个gedit，演示 killall gedit

查看进程树pstree

pstree[选项]，可以更加直观的来看进程信息

常用选项

-p:显示进程的pid

-u:显示进程的所属用户

<h1 id="l140p">服务管理</h1>
+ 介绍

服务(service)本质就是进程，但是是运行在后台的，通常都会监听某个端口，等待其他程序的请求，比如(mysqld，sshd，防火墙等)，因此我们又称为守护进程，是linux中非常重要的知识点

+ server管理指令
1. service 服务名[start|stop|restart|reload|status]
2. 在Centos7.0后很多服务不再使用service，而是systemctl
3. service指令管理的服务在/etc/init.d查看

![](https://img.muyoung.com/202502281455987.png)

+ 服务的运行级别(runlevel)
+ Linux系统有7种运行级别(runlevel)：常用的是3跟5

运行级别0：系统停机状态，系统默认运行级别不能设为0，否则不能正常启动

运行级别1：单用户工作状态，root权限，用于系统维护，禁止远程登录

运行级别2：多用户状态(没有NFS)，不支持网络

运行级别3：完全的多用户状态(有NFS),登录后进入控制台命令行模式

运行级别4：系统未使用，保留

运行级别5：X11控制台，登录后进入图形GUI模式

运行级别6：系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动

+ 开机的流程说明:

![](https://img.muyoung.com/202502281455446.png)

l 设置运行级别

systemctl get-default  #获取当前的运行级别

systemctl set-default xxx #设置默认的运行级别为xxx

目前一般使用级别3跟5

multi-user.target:analogous to runlevel 3

systemctl set-default multi-user.target

graphical.target:analogous to runlevel 5

init 0

l systemctl管理指令

○ systemctl设置服务的自启动状态

1.systemctl list-unit-files [|grep服务名] 查看服务开机启动状态，grep可以进行过滤

2.systemctl enable 服务名 （设置服务开机启动）

3.systemctl disable 服务名 （关闭服务开机启动）

4.systemctl is-enabled 服务名 （查询某个服务是否是自启动的）

l firewall指令

○ 打开端口：firewall-cmd --permanent --add-port=端口号/协议

关闭端口：firewall-cmd --permanent --remove-port=端口号/协议

重新载入：firewall-cmd --reload

查询端口是否开放：firewall-cmd --query-port=端口/协议

<h1 id="uHn6Q">动态监控进程</h1>
+ 介绍

top与ps命令很相似，它们都用来显示正在执行的惊醒。top与ps最大的不同之处，在于top在执行一段时间可以更新正在运行的进程

+ 基本语法

top[选项]

+ 选项说明

![](https://img.muyoung.com/202502281455441.png)

![](https://img.muyoung.com/202502281455210.png)

+ 交互操作说明：

![](https://img.muyoung.com/202502281456346.png)

![](https://img.muyoung.com/202502281456645.png)

+ 查看系统网络情况netstat
+ 基本语法

netstat[选项]

+ 选项说明

-an 按一定顺序排列输出

-p 显示哪个进程在调用

+ 应用案例

请查看服务名为sshd服务的信息

+ 检测主机链接命令ping

ping是一种网络监测检测工具，它主要是用检测远程主机是否正常，或是两部主机间的网线或网卡故障

<h1 id="pGDC8">yum与rpm包的管理</h1>
+ rpm包的简单查询指令

查询已安装的rpm列表 rpm -qa|grep xx

+ rpm包名基本格式

一个rpm包名：firefox-60.2.2-1.el7.centos.x86_64

表示centos7.x的64位系统

如果是i686、i386表示32位系统、noarch表示通用

+ rpm包的其他查询指令

rpm -qa：查询所安装的所有rpm软件包

rpm -qa|more

rpm -qa| grep X[rpm -qa |grep firefox]

rpm -q 软件包名：查询软件包是否安装

案例：rpm -q |firefox

rpm -qi软件包名：查询软件包信息

案例：rpm -qi firefox

rpm -ql 软件包名：查询软件包中的文件

比如：rpm -ql firefox

rpm -qf 文件全路径名 查询文件所属的软件包

rpm -qf /etc/passwd

rpm -qf /root/install.log

<h3 id="AmotD">安装rpm包</h3>
rpm -ivh RPM包全路径名称

+ 参数说明

i=install 安装

v=verbose提示

h=hash 进度条

+ 卸载rpm包

rpm -e RPM包的名称

如果其他软件包依赖于你要卸载的软件包，卸载时则会产生错误信息

如 rpm-e foo

removing these packages would breakdependencies:foo is needed by bar-1.0-1

如果我们就是要删除foo这个软件包，可以增加参数--nodeps，就可以强制删除，但是一般不推荐这样做，因为依赖于该软件包的程序可能无法运行

如 rpm-e --nodeps foo

<h3 id="mQHQv">yum的使用</h3>
yum是一个shell前端软件包管理器，基于rpm包管理，能够从指定的服务器自动下载rpm包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包

<h3 id="FCn5v">yum的基本指令</h3>
查询yum服务器是否有需要安装的软件

yum list|grep xx软件列表

安装指定的yum包

yum install xxx下载安装

<h1 id="M7SxN">常用yum源配置</h1>
<h3 id="XtHs4">阿里云源配置方法</h3>
1. 备份

```bash
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

2. 下载新的 CentOS-Base.repo 到 /etc/yum.repos.d/

**CentOS 6**

```bash
wget -O /etc/yum.repos.d/CentOS-Base.repo [https://mirrors.aliyun.com/repo/Centos-6.repo](https://mirrors.aliyun.com/repo/Centos-6.repo)
```

或者

```bash
curl -o /etc/yum.repos.d/CentOS-Base.repo [https://mirrors.aliyun.com/repo/Centos-6.repo](https://mirrors.aliyun.com/repo/Centos-6.repo)
```

**CentOS 7**

```bash
wget -O /etc/yum.repos.d/CentOS-Base.repo [https://mirrors.aliyun.com/repo/Centos-7.repo](https://mirrors.aliyun.com/repo/Centos-7.repo)
```

或者

```bash
curl -o /etc/yum.repos.d/CentOS-Base.repo [https://mirrors.aliyun.com/repo/Centos-7.repo](https://mirrors.aliyun.com/repo/Centos-7.repo)
```

**CentOS 8**

```bash
wget -O /etc/yum.repos.d/CentOS-Base.repo [https://mirrors.aliyun.com/repo/Centos-8.repo](https://mirrors.aliyun.com/repo/Centos-8.repo)
```

或者

```bash
curl -o /etc/yum.repos.d/CentOS-Base.repo [https://mirrors.aliyun.com/repo/Centos-8.repo](https://mirrors.aliyun.com/repo/Centos-8.repo)
```

3. 运行 yum makecache 生成缓存
4. 其他

非阿里云ECS用户会出现 Couldn't resolve host '[mirrors.cloud.aliyuncs.com](http://mirrors.cloud.aliyuncs.com)' 信息，不影响使用。用户也可自行修改相关配置: eg:

```bash
sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.repo
```

<h3 id="CdHer">腾讯云源配置方法</h3>
备份系统旧配置文件

```bash
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

获取对应版本的CentOS-Base.repo 到/etc/yum.repos.d/目录[](https://mirrors.cloud.tencent.com/help/centos.html#centos-baserepo-etcyumreposd)

各版本源配置列表

CentOS5

```bash
wget -O /etc/yum.repos.d/CentOS-Base.repo [http://mirrors.cloud.tencent.com/repo/centos5_base.repo](http://mirrors.cloud.tencent.com/repo/centos5_base.repo)
```

CentOS6

```bash
wget -O /etc/yum.repos.d/CentOS-Base.repo [http://mirrors.cloud.tencent.com/repo/centos6_base.repo](http://mirrors.cloud.tencent.com/repo/centos6_base.repo)
```

CentOS7

```bash
wget -O /etc/yum.repos.d/CentOS-Base.repo [http://mirrors.cloud.tencent.com/repo/centos7_base.repo](http://mirrors.cloud.tencent.com/repo/centos7_base.repo)
```

CentOS8

```bash
wget -O /etc/yum.repos.d/CentOS-Base.repo [http://mirrors.cloud.tencent.com/repo/centos8_base.repo](http://mirrors.cloud.tencent.com/repo/centos8_base.repo)
```

更新缓存

```bash
yum clean all
yum makecache
```

<h1 id="wSkrK">防火墙</h1>
<h2 id="C3iI5">系统防火墙</h2>
```bash
启动防火墙
systemctl start firewalld
关闭防火墙
systemctl stop firewalld
查看状态
systemctl status firewalld
开机启用防火墙
systemctl enable firewalld
开机禁用防火墙
systemctl disable firewalld
```

<h2 id="yx2ph">某个端口的防火墙</h2>
```bash
开放某个端口，如8080端口
firewall-cmd --zone=public --add-port=8080/tcp --permanent
重新加载配置
firewall-cmd --reload
查看防火墙锁开放的端口
firewall-cmd --zone=public --list-ports
查看某个端口的访问权限，如8080
firewall-cmd --zone=public --query-port=8080/tcp
关闭某个端口的防火墙，如8080端口
firewall-cmd --zone=public --remove-port=8080/tcp --permanent

命令含义：
--zone #作用域
--add-port=80/tcp  #添加端口，格式为：端口/通讯协议
--permanent  #永久生效，没有此参数重启后失效
```

<h2 id="tEfI5">注册系统服务</h2>
<font style="color:rgb(86, 86, 86);">到</font>_<font style="color:rgb(86, 86, 86);">/usr/lib/systemd/system</font>_<font style="color:rgb(86, 86, 86);">目录下。</font>  
<font style="color:rgb(86, 86, 86);">单击新建，选择新建空白文件，文件名填 </font>_<font style="color:rgb(86, 86, 86);">cloudreve.service</font>_<font style="color:rgb(86, 86, 86);"> 。</font>  
<font style="color:rgb(86, 86, 86);">将下文 </font>_<font style="color:rgb(86, 86, 86);">PATH_TO_CLOUDREVE</font>_<font style="color:rgb(86, 86, 86);"> 更换为程序所在目录，复制到新建的文件中并</font>**<font style="color:rgb(86, 86, 86);">保存</font>**<font style="color:rgb(86, 86, 86);">。</font>

```basic
[Unit]
Description=Cloudreve
Documentation=https://docs.cloudreve.org
After=network.target
Wants=network.target

[Service]
WorkingDirectory=/PATH_TO_CLOUDREVE
ExecStart=/PATH_TO_CLOUDREVE/cloudreve
Restart=on-abnormal
RestartSec=5s
KillMode=mixed

StandardOutput=null
StandardError=syslog

[Install]
WantedBy=multi-user.target
```

<font style="color:rgb(86, 86, 86);">然后在服务器中先后执行下面的命令</font>

```basic
# 更新配置
systemctl daemon-reload

# 启动服务
systemctl start cloudreve

# 设置开机启动
systemctl enable cloudreve
```