# Linux教程

## 1.Linux介绍

https://github.com/jast90/awesome-books/issues/1

创始人：Linuxs，吉祥物：企鹅tux

基于linux内核，各个公司又发行了不同版本的发行版，如Centos，ubantu，Redhat等，大数据主要用Centos，python主要用ubantu

## 2.Linux和Unix的关系

70‘s ——>贝尔实验室——>Ken tompson用B语言编写了最初的Unix——>和Dennis richers合作用C语言重写了Unix——>Richard Stallman 提出伟大的GNU计划——>内核采用了Linux

![image-20200325230806603](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200325230806603.png)

![image-20200325231345658](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200325231345658.png)

## 3.关机重启和注销

#### 3.1基本介绍

###### showdown

showdown -h now :表示立即关机

showdown -h 1 :表示1分钟后关机

showdun -r now :立即重启

showdown -c： 取消前一个关机命令

###### halt :直接使用，效果等价于关机

##### reboot  重启系统

##### poweroff 关机

##### init + 数字

0  关机

1  单用户

2  不完全多用户，不含NFS服务

3  完全多用户

4  未分配

5  图形界面

6  重启

##### sync  把内存的数据同步到磁盘

#### 注意：当我们关机或者重启之前，都应该先sync一下。

### 3.2用户切换和注销

#### logout 注销用户（在图形界面无效）

#### su + 用户名 ，切换用户

## 

### 4.Linux目录结构

#### 树状目录结构

#### 4.1linux的目录中有且只有一个根目录/

#### 4.2linux的各个目录存放的内容是规划好的，不要乱放文件

#### 4.3linux是以文件的形式管理我们的设备。

在linux世界里，一切皆文件

![image-20200331122450999](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200331122450999.png)

![image-20200331122815975](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200331122815975.png)

<img src="C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200331122944223.png" alt="image-20200331122944223"  />

![image-20200331123211821](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200331123211821.png)

![image-20200331123237576](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200331123237576.png)

![image-20200331123507348](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200331123507348.png)

## 5.vi和vim的使用

#### ![image-20200331125121359](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200331125121359.png)

![image-20200331125139710](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200331125139710.png)

## 6.用户创建和设置密码

useradd (-d /home/指定家目录) 用户名

useradd -g 组名 用户名 ：分配用户到指定的组，默认分配到相同用户名的组

passwd 用户名 ：给用户设置密码

userdel (-r 删除家目录)   用户名

id 用户名 ：查看用户信息

## 7.组的使用

组的作用：统一管理多个用户

groupadd 组名

groupdel 组名

usermod -g 组名 ：修改用户所属组

## 8.用户和组配置文件

/etc/passwd  存储用户的信息

用户名:x :用户id:组id

/etc/group  存储组的信息

组名:x :组id

/etc/shadown  存储加密过后的用户密码

## 9.系统的运行级别

![image-20200331191741656](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200331191741656.png)

init [0123456]

#### 如何找回root密码

进入单用户模式，修改root密码即可

开机->在引导时输入回车键->看到一个界面输入 e->看到一个新的界面，选中第二行（编辑kernel）再输入 e-> 在这行最后输入 1 ，再输入回车键 ->再次输入 b ,这时就会进入到单用户模式。

## 10.帮助指令

能查看其它命令的使用手册的命令

### man

#### 基本语法 

  man [命令或配置文件]  ：获得帮助信息

### help

help [命令]  ： 获得shell内置命令的帮助信息



## 11.cp

cp 源文件地址 目的文件地址   ：复制普通文件

cp -r 源目录文件地址 目的文件地址  ：复制目录文件

\cp -r 源目录文件地址 目的文件地址  ：表示强制复制文件



## 12.cat

cat -n /etc/profile  读取文件并显示行号在控制台

通常配合more分页显示

cat -n /etc/profile  | more

## 13.more

基本语法        more [文件]

进入后可用快捷键

![image-20200331200400108](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200331200400108.png)

## 14.less

less使用分屏显示,less指令与more相似，当比more强大，less并不是一次性把文件全部读取显示出来，而是按照要显示的内容读取文件，在读取大文件上效率比more高

![image-20200331200909111](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200331200909111.png)

## 15.ln

ln 是软连接命令，相当于windows下的快捷方式

基本语法

ln -s 目的文件地址  要关联的文件地址

## 16.history

查看系统历史命令

基本语法: history

查看前十条历史记录 :history 10

执行之前的第250条命令: !250

## 17.date

查看系统时间

基本语法

date  显示当前时间

date + %Y 显示当前年份

date + %m 显示当前月份

date + %d 显示当前是哪一天

date "+%Y-%m-%d %H:%M:%S" 表示显示当前时间到秒

date -s "年-月-日 时:分:秒"   设置系统时间

## 18.cal

查看日历的指令

基本语法 

cal  	显示当前月份日历

cal 2020 显示2020年日历

## 19.find

查找文件的绝对路径

基本语法

find  [搜索范围/地址]	 {-name(按文件名查找) 	 / 	 -user(按文件的拥有者查找)  	/	-size(按文件大小查找)}  [文件名/用户名/“+/-文件大小(代单位)”] 	+/-表示大于或小于 不写表示等于

## 20.locate

locate指令可以快速定位文件路径，locate采用实现建立的locate数据库查询文件，无需遍历整个文件系统，查询速度较快，为保证查询结果准确度，需要定期更新locate时刻。需要mysql库支持

基本语法

locate [文件名]

注意，locate第一次运行前，必须使用updatedb指令创建locate数据库。

## 21.grep和管道符|

grep过滤查找，管道符表示把前面执行的结果交给后面的命令处理

![image-20200331211802256](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200331211802256.png)

## 22.tar

tar -zcvf mytar.tar.gz a.txt b.txt 表示将a.txt和b.txt文件压缩成mytar.tar.gz文件

tar -zxvf mytar.tar.gz  表示把mytar.tar.gz解压到当前文件夹

tar -zxvf mytar.tar.gz -C /opt/module 表示把mytar.tar.gz解压到/opt/module目录下

## 23.组管理

##### 改变文件拥有者

chmod athgb(用户名)  hgb.txt

##### 改变文件所属组

chgrp athgb(组名) hgb.txt

##### 改变用户所属组

usermod -g athgb(组名) athgb(用户名)

## 24.权限详细介绍

![image-20200331215059321](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200331215059321.png)

d 表示目录文件,-表示普通文件,c表示字符设备文件（键盘，鼠标）,b表示块文件（硬盘）

![image-20200331215417777](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200331215417777.png)

![image-20200331221120869](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200331221120869.png)

## 25.权限管理

chmod u=rwx,g=rx,o=r a.txt

u:拥有者，g:所属组，o:其他人，a:全部用户

chown athgb(用户名)  a.txt

chown athgb(用户名):athgb(组名)  [-R] b.txt

-R 如果是目录，则使其下所有子文件或目录递归生效

## 26.任务调度crontab

可以理解为定时任务

![image-20200401172345854](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200401172345854.png)

任务调度：是指系统在某个时间执行的特定的命令或程序

任务调度分类：

1.系统工作，有些重要的工作必须周而复始的地执行。

2.个别用户工作：个别用户可能希望执行某些程序

### 基本语法

 crontab  [选项]

### 常用选项					

|  -e  |     编辑crontab定时任务     |
| :--: | :-------------------------: |
|  -l  |       查询crontab任务       |
|  -r  | 删除当前用户所有crontab任务 |

![image-20200401203414274](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200401203414274.png)

![image-20200401203601017](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200401203601017.png)

## 27.磁盘分区介绍

![image-20200401220052305](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200401220052305.png)

![image-20200401221417302](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200401221417302.png)

lsblk -f 或 lsblk 来查看磁盘分区情况

![image-20200401222109385](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200401222109385.png)

![image-20200401222127476](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200401222127476.png)



![image-20200401223549402](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200401223549402.png)

fdisk /dev/sda :进行磁盘分区

mkfs -t ext4  /dev/sda :用ext4类型格式化磁盘

mount 磁盘分区 挂载点目录  :磁盘挂载 

vim /etc/fstab   :编辑磁盘表以实现永久挂载

增加如下内容

/dev/sda          /home/athgb      			ext4       default      0    0       

## 28.磁盘查询使用指令

#### df -l 或 df -h  

 :查看系统整体磁盘使用情况

#### du -h /目录   

 :查看指定目录磁盘使用情况

![image-20200402170255219](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200402170255219.png)

| -s            | 指定目录占用大小汇总       |
| ------------- | -------------------------- |
| -h            | 带计量单位                 |
| -a            | 含文件                     |
| --max-depth=1 | 子目录深度为1              |
| -c            | 列出明细的同时。增加汇总值 |

1.统计普通文件的个数

2.统计目录文件的个数

3.统计普通文件及子文件的个数

4.统计目录文件及子目录的个数

![image-20200402170946293](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200402170946293.png)

## 29.进程管理ps

![image-20200402184236940](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200402184236940.png)

显示系统执行的进程

ps，一般来说使用的参数是ps -aux

ps显示的信息选项:

| PID  |       进程识别号       |
| ---- | :--------------------: |
| TTY  |        终端编号        |
| TIME |  此进程所消耗cup时间   |
| CMD  | 正在执行的命令或进程名 |

参数选项:

| -a   | 显示当前终端的所有进程信息 |
| ---- | :------------------------: |
| -u   |  以用户的格式显示进程信息  |
| -x   |   显示后台进程运行的参数   |
| -e   |        显示所有进程        |
| -f   |        以全格式显示        |

![image-20200402185417723](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200402185417723.png)

![image-20200402190132934](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200402190132934.png)

## 30.终止进程kill

killall和pstree命令需要安装psmisc包

#### kill [选项] 进程号  或 killall 进程名

常用选项:

-9 ：表示强制杀死进程

![image-20200402193330455](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200402193330455.png)

#### pstree 查看进程数

基本语法

pstree [选项] ，可以更加直观的查看进程信息

常用选项:

-p : 显示进程的PID

-u : 显示进程的所属用户

## 31.服务管理

### service 管理指令

service 服务名 [start|stop|restart|reload|status] 

在centos7.0以后使用systemctl代替service 

systemctl [start|stop|restart|reload|status]  服务名

### 使用案例

查看当前防火墙状态

service  iptables status centos7.0以后防火墙服务名为firewalld

在 /etc/init.d

### 服务的运行级别

##### 查看或修改默认级别

vim /etc/inittab

Linux系统有7种运行级别(runlevel) ，常用的是3和5

| 0    | 关机状态，不能设为默认级别，否则不能正常启动                 |
| ---- | ------------------------------------------------------------ |
| 1    | 单用户工作状态，root权限，无密码，用于系统维护，特殊方式启动，禁止远程登入 |
| 2    | 多用户状态（没有NFS），无网络                                |
| 3    | 完全的多用户状态（有NFS），登录后进入命令行模式              |
| 4    | 保留                                                         |
| 5    | X11控制台，登录后进入图形GUI模式                             |
| 6    | 系统重启，不能设为默认级别，否则不能正常启动                 |

### 开机流程说明

开机——>BIOS——>/boot——>init进程——>运行级别——>启动运行级别对应服务

### chkconfig指令

##### 介绍

通过chkconfig命令可以给每个服务的各个运行级别设置自启动/关闭

#### 基本语法

##### 1.查看服务chkconfig --list

##### 2.chkconfig 服务名 --list

##### 3.chkconfig --level 5 服务名 on/off   

  level不写则表示设置所有级别

## 32.动态监控进程top

### 介绍

top与ps命令相似，但top可以在执行一段时间更新正在运行的进程

### 基本语法

top [选项]

### 选项说明:

| -d 秒数 | 指定top命令刷新时间，可以在交互模式下使用 |
| ------- | ----------------------------------------- |
| -l      | 使top不显示任何闲置或者僵尸进程           |
| -p      | 通过指定监控进程ID来监控某个进程的状态    |

###  交互操作说明:

| P    |      以CPU使用率排序，默认启用      |
| ---- | :---------------------------------: |
| M    |          以内存使用率排序           |
| N    |              以PID排序              |
| q    |                退出                 |
| u    | 监控指定用户，输入u,输入用户名,回车 |
| k    | 杀死指定进程，输入k，输入PID，回车  |

 echo $$ :可以查看当前使用shell的PID



## 33.查看系统网络情况netstat

### 基本语法

netstat [选项]

选项说明:

| -a   | 显示所有状态的端口     |
| ---- | ---------------------- |
| -p   | 显示pid和程序名字      |
| -n   | 不做名字解析           |
| -t   | 显示tcp链接            |
| -u   | 显示udp链接            |
| -l   | 显示处于监听状态的端口 |

## 34.rpm包管理

![image-20200402213843166](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200402213843166.png)

### 基本语法

rpm [选项] [安装包名/文件名]

常用选项:

| -i    (install) | 安装包                                   |
| :-------------- | :--------------------------------------- |
| -v   (verbose)  | 显示调试信息                             |
| -h   (hash)     | 安装时输出hash记号                       |
| -e   (erase)    | 卸载包                                   |
| -u   (update)   | 更新包                                   |
| -q   (query)    | 查询一个包是否被安装                     |
| -qa             | 查询所有软件包                           |
| -ql             | 显示软件包里的所有文件                   |
| -qf             | 查询文件所依赖的包                       |
| -qi             | 得到被安装的包的信息                     |
| -nodeps         | 不检查依赖性                             |
| -c              | 显示软件包中文件列表并显示每个文件的状态 |
| -d              | 显示文档文件列表                         |
| -s              | 显示配置文件列表                         |
| -l              | 显示软件包中的文件列表                   |

## 35.yum 包管理

### 介绍

yum是一个shell前端软件包管理器，基于rpm包管理，能够从指定的服务器自动下载rpm包	并且安装，可以自动处理包的依赖关系，并且一次安装所有依赖的软件包

### 基本语法

yum install 包

yum list   :查询所有安装的包

## 36.查看磁盘读写能力iotop

基本语法

iotop