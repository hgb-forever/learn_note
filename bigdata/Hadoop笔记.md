

# 大数据技术之Hadoop

## 1.1大数据概念

![image-20200403162623969](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200403162623969.png)

主要解决，海量数据的存储和海量数据的分析计算问题

### 特点：

Volume（大量），Velocity（高速），Variety（多样），Value（低价值密度）

### 业务流程

产品人员提需求——>数据部门搭建数据平台、分析数据指标——>数据可视化（报表展示、邮件发送、大屏幕展示等）

![image-20200403164954266](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200403164954266.png)

## 1.2hadoop是什么

1.hadoop是一个由Apache基金会所开发的分布式系统基础架构

2.主要解决海量数据的**存储**和海量数据的**分析计算**问题

3.广义上来说，hadoop通常是指一个更广泛的概念——hadoop生态圈

## 1.3Hadoop发展史

![image-20200403165649988](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200403165649988.png)

![image-20200403165819273](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200403165819273.png)

## 1.4Hadoop三大发行版本

安装Hadoop之前，安装对应JDK版本,并配置JAVA_HOME环境变量

Hadoop三大发行版本：Apache、Cloudera、Hortonworks。

Apache版本最原始（最基础）的版本，对于入门学习最好。

Cloudera在大型互联网企业中用的较多。

Hortonworks文档较好。

##### 	1.Apache Hadoop

官网地址：http://hadoop.apache.org/releases.html

下载地址：https://archive.apache.org/dist/hadoop/common/

##### 	2.Cloudera Hadoop 

官网地址：https://www.cloudera.com/downloads/cdh/5-10-0.html

下载地址：http://archive-primary.cloudera.com/cdh5/cdh/5/

（1）2008年成立的Cloudera是最早将Hadoop商用的公司，为合作伙伴提供Hadoop的商用解决方案，主要是包括支持、咨询服务、培训。

   (2）2009年Hadoop的创始人Doug Cutting也加盟Cloudera公司。Cloudera产品主要为CDH，Cloudera Manager，Cloudera Support

（3）CDH是Cloudera的Hadoop发行版，完全开源，比Apache Hadoop在兼容性，安全性，稳定性上有所增强。

（4）Cloudera Manager是集群的软件分发及管理监控平台，可以在几个小时内部署好一个Hadoop集群，并对集群的节点及服务进行实时监控。Cloudera Support即是对Hadoop的技术支持。

（5）Cloudera的标价为每年每个节点4000美元。Cloudera开发并贡献了可实时处理大数据的Impala项目。

##### 	3.Hortonworks Hadoop

官网地址：https://hortonworks.com/products/data-center/hdp/
下载地址：https://hortonworks.com/downloads/#data-platform

## 1.5Hadoop的优势

![image-20200403170918868](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200403170918868.png)

## 1.6Hadoop组成

![image-20200403171116746](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200403171116746.png)

[^耦合性]: 也叫耦合度，是对模块间关联程度的度量，耦合的强弱取决于模块间接口的复杂性、调用模块的方式以及通过界面传送数据的多少。划分模块的一个准则就是高内聚低耦合。
[^高内聚]: 一个模块只需做好一件事件，不要过分关心其他任务。高内聚性的好处是可以提高程序的可靠性。

## 1.7HDFS架构

HDFS（Hadoop Distributed File System）的架构概述

#### 1.Namenode    -----Master

1. 存储文件的元数据(存储在FsImage和Edits文件中)
2. 管理HDFS的名称空间
3. 配置副本策略
4. 管理Block映射信息
5. 处理Client读写请求

#### 2.DataNode      -------Slave

1. 存储实际的数据块（Block）
2. 执行数据块的读写操作
3. 周期性向NameNode上报所有的块信息
4. 心跳机制：每3秒一次，心跳返回NameNode下达的命令，如果超时10无应答，则认为该节点死亡

#### 3.Secondary NameNode——存档

用来监控HDFS状态的辅助后台程序，每隔一段时间获取HDFS元数据的快照。

1. 辅助NameNode，分担其工作量，定期合并Fsimage和Edits，并推送给NameNode；
2. 在紧急情况下，可辅助回复NameNode；

#### 4.Client

就是客户端

1. 文件切分。文件上传HDFS的时候，CLient将文件切分成一个一个的Block，然后进行上传。
2. 与NameNode交互，获取文件的位置信息；
3. 与DataNode交互，读取或者写入数据；
4. Client提供一些命令来管理和访问HDFS

#### 5.HDFS文件块大小（面试重点）

![image-20200404202825615](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200404202825615.png)

![image-20200404202853013](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200404202853013.png)

#### 6.Hadoop的Shell操作（开发重点）

##### 1．基本语法

bin/hadoop fs 具体命令  OR  bin/hdfs dfs 具体命令

dfs是fs的实现类。

##### 2.常用命令大全

| hadoop fs 开头          | 说明                                        |
| ----------------------- | ------------------------------------------- |
| -help                   | 输出这个命令参数                            |
| -ls                     | 显示目录信息                                |
| -mkdir                  | 在HDFS上创建目录                            |
| -moveFromLocal          | 从本地剪切粘贴到HDFS                        |
| -appendToFile           | 追加一个文件到已经存在的文件末尾            |
| -cat                    | 显示文件内容                                |
| -chgrp 、-chmod、-chown | Linux文件系统中的用法一样，修改文件所属权限 |
| -copyFromLocal          | 从本地文件系统中拷贝文件到HDFS路径去        |
| -copyToLocal            | 从HDFS拷贝到本地                            |
| -cp                     | 从HDFS的一个路径拷贝到HDFS的另一个路径      |
| -mv                     | 在HDFS目录中移动文件                        |
| -get                    | 从HDFS下载文件到本地                        |
| -getmerge               | 合并下载多个文件                            |
| -put                    | 同于copyFromLocal                           |
| -tail                   | 显示一个文件的末尾                          |
| -rm                     | 删除文件或文件夹                            |
| -rmdir                  | 删除空目录                                  |
| -du                     | 统计文件夹的大小信息                        |
| -setrep                 | 设置HDFS中文件的副本数量                    |

只有节点数的增加到10台时，副本数才能达到10。

#### 7.Hadoop客户端操作（开发重点）

详情看word文档

#### 8. HDFS 2.x新特性

##### 1．scp实现两个远程主机之间的文件复制

##### 2．采用distcp命令实现两个Hadoop集群之间的递归数据复制

```
[atguigu@hadoop102 hadoop-2.7.2]$  bin/hadoop distcp
hdfs://haoop102:9000/user/atguigu/hello.txt hdfs://hadoop103:9000/user/atguigu/hello.txt
```

##### 3.小文件归档

把/user/atguigu/input目录里面的所有文件归档成一个叫input.har的归档文件，并把归档后文件存储到/user/atguigu/output路径下。

```
[atguigu@hadoop102 hadoop-2.7.2]$ bin/hadoop archive -archiveName input.har –p  /user/atguigu/input   /user/atguigu/output
```

解归档文件

```
[atguigu@hadoop102 hadoop-2.7.2]$ hadoop fs -cp har:/// user/atguigu/output/input.har/*    /user/atguigu
```

##### 4.回收站

![image-20200404222552088](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200404222552088.png)

开启回收站功能，可以将删除的文件在不超时的情况下，恢复原数据，起到防止误删除、备份等作用。

启用回收站，修改core-site.xml，配置垃圾回收时间为1分钟

```xml
<property>
   <name>fs.trash.interval</name>
<value>1</value>
</property>`
```

回收站在集群中的路径：/user/atguigu/.Trash/….

进入垃圾回收站用户名称，默认是dr.who，修改为atguigu用户
	[core-site.xml]

```xml
<property>
  <name>hadoop.http.staticuser.user</name>
  <value>atguigu</value>
</property>
```

通过程序删除的文件不会经过回收站，需要调用moveToTrash()才进入回收站

```java
Trash trash = New Trash(conf);

trash.moveToTrash(path);
```

 清空回收站

```
[atguigu@hadoop102 hadoop-2.7.2]$ hadoop fs -expunge
```

##### 5.快照管理

![image-20200404223022607](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200404223022607.png)

开启/禁用指定目录的快照功能

```
[atguigu@hadoop102 hadoop-2.7.2]$ hdfs dfsadmin -allowSnapshot /user/atguigu/input

[atguigu@hadoop102 hadoop-2.7.2]$ hdfs dfsadmin -disallowSnapshot /user/atguigu/input
```

对目录创建快照

```
[atguigu@hadoop102 hadoop-2.7.2]$ hdfs dfs -createSnapshot /user/atguigu/input
```

指定名称创建快照

```
[atguigu@hadoop102 hadoop-2.7.2]$ hdfs dfs -createSnapshot /user/atguigu/input  miao170508
```

恢复快照

```
[atguigu@hadoop102 hadoop-2.7.2]$ hdfs dfs -cp
/user/atguigu/input/.snapshot/s20170708-134303.027 /user
```

#### 9. HDFS-HA 高可用（难点）



## 1.8 Yarn架构

### 1.ResourceManager    CEO

主要作用如下:   

- 处理客户端请求
- 监控NodeManager
- 启动或监控ApplicationMaster
- 资源的分配与调度

### 2.NodeManager   项目经理

主要作用如下    :     

- 管理单个节点上的资源
- 处理来自ResourceManager的命令
- 处理来自ApplicationMaster的命令

### 3.ApplicationMaster   项目员工

作用如下     :  

- 负责数据的切分
- 为应用程序申请资源并分配给内部的任务
- 任务的监控与容错

### 4.Container    会虚拟化的打工仔

- Container是YARN中的资源抽象，它封装了某个节点上的维度资源，如内存、CPU、磁盘、网络等。

![image-20200403173242610](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200403173242610.png)

## 1.9MapReduce架构

![image-20200405102220558](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200405102220558.png)

MapReduce将计算过程分为两个阶段：Map和Reduce，如图2-25所示
1）Map阶段并行处理输入数据，MapTask并发实例，完全并行运行，互不相干。
2）Reduce阶段对Map结果进行汇总，ReduceTask并发实例互不相干，但是他们的数据依赖于上一个阶段的所有MapTask并发实例的输出。

3）MapReduce编程模型只能包含一个Map阶段和一个Reduce阶段，如果用户的业务逻辑非常复杂，那就只能多个MapReduce程序，串行运行。

4）MrAppMaster：负责整个程序的过程调度及状态协调。

5）Map方法之后，Reduce方法之前的数据处理过程为shuffle

![image-20200405103144397](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200405103144397.png)

## 2.0Hadoop的三种模式

### Local or Standalone Mode

本地模式

for example ：bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar grep input output 'dfs[a-z.]+'

### Pseudo-Distributed Mode

伪分布式模式

#### 一、配置集群

1.配置hadoop-env.sh

```
export JAVA_HOME=/opt/module/jdk1.8.0_144
```

2.配置core-site.xml

```xml
<!-- 指定HDFS中NameNode的地址 -->
<property>
<name>fs.defaultFS</name>
    <value>hdfs://hadoop101:9000</value>
</property>

<!-- 指定Hadoop运行时产生文件的存储目录 -->
<property>
	<name>hadoop.tmp.dir</name>
	<value>/opt/module/hadoop-2.7.2/data/tmp</value>
</property>
```

3.配置hdfs-site.xml

```xml
<!-- 指定HDFS副本的数量 -->
<property>
	<name>dfs.replication</name>
    <!-- 默认值为3 -->
	<value>1</value>
</property>
```

#### 二、启动集群

格式化NameNode（第一次启动时格式化，以后就不要总格式化）

```
bin/hdfs namenode -format
```

启动NameNode

```
sbin/hadoop-daemon.sh start namenode
```

启动DataNode

```
sbin/hadoop-daemon.sh start datanode
```

三、查看集群

##### 1.查看是否启动成功

```
[atguigu@hadoop101 hadoop-2.7.2]$ jps
13586 NameNode
13668 DataNode
13786 Jps
```

##### 2.web端查看HDFS文件系统

http://hadoop101:50070/dfshealth.html#tab-overview

##### 3.查看产生的Log日志

```
cd /opt/module/hadoop-2.7.2/logs
```

> ##### 注意：格式化NameNode，会产生新的集群**id**,导致NameNode和DataNode的集群id不一致，集群找不到已往数据。所以，格式NameNode时，一定要先删除data数据和log日志，然后再格式化NameNode。

##### 4.操作集群

###### 1.将文件上传到hdfs

```
[atguigu@hadoop101 hadoop-2.7.2]$bin/hdfs dfs -put wcinput/wc.input /user/atguigu/input/
```

###### 2.将文件下载到本地

```
[atguigu@hadoop101 hadoop-2.7.2]$ hdfs dfs -get /user/atguigu/output/part-r-00000 ./wcoutput/
```

#### 三、启动YARN并运行MapReduce程序

##### 1.执行步骤

（1）配置集群
（a）配置yarn-env.sh
          配置一下JAVA_HOME

```
export JAVA_HOME=/opt/module/jdk1.8.0_144
```

（b）配置yarn-site.xml

```xml
<!-- Reducer获取数据的方式 -->
<property>
 		<name>yarn.nodemanager.aux-services</name>
 		<value>mapreduce_shuffle</value>
</property>

<!-- 指定YARN的ResourceManager的地址 -->
<property>
<name>yarn.resourcemanager.hostname</name>
<value>hadoop101</value>
</property>
```

（c）配置：mapred-env.sh

配置一下JAVA_HOME

```
export JAVA_HOME=/opt/module/jdk1.8.0_144
```

（d）配置： (对mapred-site.xml.template重新命名为) mapred-site.xml

```xml
[atguigu@hadoop101 hadoop]$ mv mapred-site.xml.template mapred-site.xml
[atguigu@hadoop101 hadoop]$ vi mapred-site.xml

<!-- 指定MR运行在YARN上 -->
<property>
		<name>mapreduce.framework.name</name>
		<value>yarn</value>
</property>
```

##### 2.启动集群

（a）启动前必须保证NameNode和DataNode已经启动

（b）启动ResourceManager

```
[atguigu@hadoop101 hadoop-2.7.2]$ sbin/yarn-daemon.sh start resourcemanager
```

（c）启动NodeManager

```
[atguigu@hadoop101 hadoop-2.7.2]$ sbin/yarn-daemon.sh start nodemanager
```

##### 3.集群操作

（a）YARN的浏览器页面查看

http://hadoop101:8088/cluster

#### 四、配置历史服务器

为了查看程序的历史运行情况，需要配置一下历史服务器。具体配置步骤如下：

1. 配置mapred-site.xml

   ```
   [atguigu@hadoop101 hadoop]$ vi mapred-site.xml
   ```

   在该文件里面增加如下配置。

   ```xml
   <!-- 历史服务器端地址 -->
   <property>
   <name>mapreduce.jobhistory.address</name>
   <value>hadoop101:10020</value>
   </property>
   <!-- 历史服务器web端地址 -->
   <property>
   <name>mapreduce.jobhistory.webapp.address</name>
   <value>hadoop101:19888</value>
   </property>
   ```

   

2. 启动历史服务器

   ```
   [atguigu@hadoop101 hadoop-2.7.2]$ sbin/mr-jobhistory-daemon.sh start historyserver
   ```

   

3. 查看历史服务器是否启动

   ```
   [atguigu@hadoop101 hadoop-2.7.2]$ jps
   ```

4.	查看JobHistory
http://hadoop101:19888/jobhistory

#### 五、配置日志的聚集

日志聚集概念：应用运行完成以后，将程序运行日志信息上传到HDFS系统上。
日志聚集功能好处：可以方便的查看到程序运行详情，方便开发调试。
注意：开启日志聚集功能，需要重新启动NodeManager 、ResourceManager和HistoryManager。

##### 1.配置yarn-site.xml

```
[atguigu@hadoop101 hadoop]$ vi yarn-site.xml
```

在该文件里面增加如下配置。

```xml
<!-- 日志聚集功能使能 -->
<property>
<name>yarn.log-aggregation-enable</name>
<value>true</value>
</property>

<!-- 日志保留时间设置7天 -->
<property>
<name>yarn.log-aggregation.retain-seconds</name>
<value>604800</value>
</property>
```

##### 2.关闭NodeManager 、ResourceManager和HistoryManager

```
[atguigu@hadoop101 hadoop-2.7.2]$ sbin/yarn-daemon.sh stop resourcemanager
[atguigu@hadoop101 hadoop-2.7.2]$ sbin/yarn-daemon.sh stop nodemanager
[atguigu@hadoop101 hadoop-2.7.2]$ sbin/mr-jobhistory-daemon.sh stop historyserver
```

##### 3.启动NodeManager 、ResourceManager和HistoryManager

```
[atguigu@hadoop101 hadoop-2.7.2]$ sbin/yarn-daemon.sh start resourcemanager
[atguigu@hadoop101 hadoop-2.7.2]$ sbin/yarn-daemon.sh start nodemanager
[atguigu@hadoop101 hadoop-2.7.2]$ sbin/mr-jobhistory-daemon.sh start historyserver
```

##### 4.删除HDFS上已经存在的输出文件

```
[atguigu@hadoop101 hadoop-2.7.2]$ bin/hdfs dfs -rm -R /user/atguigu/output
```

##### 5.执行WordCount程序

```
[atguigu@hadoop101 hadoop-2.7.2]$ hadoop jar
 share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar wordcount /user/atguigu/input /user/atguigu/output
```

##### 6.查看日志，如图2-37，2-38，2-39所示

http://hadoop101:19888/jobhistory

#### 六、配置文件说明

Hadoop配置文件分两类：默认配置文件和自定义配置文件，只有用户想修改某一默认配置值时，才需要修改自定义配置文件，更改相应属性值。

（1）默认配置文件：

| 要获取的默认文件     | 文件存放在Hadoop的jar包中的位置                            |
| -------------------- | ---------------------------------------------------------- |
| [core-default.xml]   | hadoop-common-2.7.2.jar/ core-default.xml                  |
| [hdfs-default.xml]   | hadoop-hdfs-2.7.2.jar/ hdfs-default.xml                    |
| [yarn-default.xml]   | hadoop-yarn-common-2.7.2.jar/ yarn-default.xml             |
| [mapred-default.xml] | hadoop-mapreduce-client-core-2.7.2.jar/ mapred-default.xml |

​	（2）自定义配置文件：

​	*core-site.xml**、**hdfs-site.xml**、**yarn-site.xml**、**mapred-site.xml*四个配置文件存放在$HADOOP_HOME/etc/hadoop这个路径上，用户可以根据项目需求重新进行修改配置。

### Fully-Distributed Mode（开发重点）

完全分布式模式

分析：

​	1）准备3台客户机（关闭防火墙、静态ip、主机名称）

​	2）安装JDK

​	3）配置环境变量

​	4）安装Hadoop

​	5）配置环境变量

​	6）配置集群

​	7）单点启动

​	8）配置ssh

​	9）群起并测试集群

#### 编写集群分发脚本xsync

##### scp（secure copy）安全拷贝

基本语法

```
scp  -r  $pdir/$fname    $user@hadoop$host:$pdir/$fname
```

命令  递归    要拷贝的文件路径/名称   目的用户@主机:目的路径/名称

案例实操

（a）在hadoop101上，将hadoop101中/opt/module目录下的软件拷贝到hadoop102上。

```
[atguigu@hadoop101 /]$ scp -r /opt/module  root@hadoop102:/opt/module
```

（b）在hadoop103上操作将hadoop101中/opt/module目录下的软件拷贝到hadoop104上。

```
[atguigu@hadoop103 opt]$ scp -r atguigu@hadoop101:/opt/module root@hadoop104:/opt/module
```

##### rsync 远程同步工具

rsync和scp区别：用rsync做文件的复制要比scp的速度快，rsync只对**差异文件**做更新。scp是把所有文件都复制过去。

基本语法

```
rsync -rvl  $pdir/$fname  $user@hadoop$host:$pdir/$fname
```

命令  选项参数  要拷贝的文件路径/名称   目的用户@主机:目的路径/名称

​	选项参数说明

表2-2

| 选项 | 功能         |
| ---- | ------------ |
| -r   | 递归         |
| -v   | 显示复制过程 |
| -l   | 拷贝符号连接 |

```shell
#!/bin/bash
#1 获取输入参数个数，如果没有参数，直接退出
pcount=$#
if((pcount==0)); then
echo no args;
exit;
fi

#2 获取文件名称
p1=$1
fname=`basename $p1`
echo fname=$fname

#3 获取上级目录到绝对路径
pdir=`cd -P $(dirname $p1); pwd`
echo pdir=$pdir

#4 获取当前用户名称
user=`whoami`

#5 循环
for((host=103; host<105; host++)); do
        echo ------------------- hadoop$host --------------
        rsync -rvl $pdir/$fname $user@hadoop$host:$pdir
done
```

#### 配置hadoop文件

基本配置与伪分布式相似，下面只列出不同处

配置hdfs-site.xml，增加下面参数

```xml
<!-- 指定Hadoop辅助名称节点主机配置 -->
<property>
      <name>dfs.namenode.secondary.http-address</name>
      <value>hadoop104:50090</value>
</property>
```

#### SSH无密登录配置

![image-20200404164939348](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200404164939348.png)

1.生成公钥和私钥：

```
cd /.ssh
[atguigu@hadoop102 .ssh]$ ssh-keygen -t rsa
```

然后敲（三个回车），就会生成两个文件id_rsa（私钥）、id_rsa.pub（公钥）

2.将公钥拷贝到要免密登录的目标机器上

```
[atguigu@hadoop102 .ssh]$ ssh-copy-id hadoop102
[atguigu@hadoop102 .ssh]$ ssh-copy-id hadoop103
[atguigu@hadoop102 .ssh]$ ssh-copy-id hadoop104
```

*注意：还需要在hadoop102上采用root账号，配置一下无密登录到hadoop102、hadoop103、hadoop104；*

#### 群起集群

1.	配置slaves

```
/opt/module/hadoop-2.7.2/etc/hadoop/slaves

[atguigu@hadoop102 hadoop]$ vi slaves
```

在该文件中增加如下内容：

```
hadoop102
hadoop103
hadoop104
```

注意：该文件中添加的内容结尾不允许有空格，文件中不允许有空行。

同步所有节点配置文件

```
[atguigu@hadoop102 hadoop]$ xsync slaves
```

2.	启动集群

（1）如果集群是第一次启动，需要格式化NameNode（注意格式化之前，一定要先停止上次启动的所有namenode和datanode进程，然后再删除data和log数据）

```
[atguigu@hadoop102 hadoop-2.7.2]$ bin/hdfs namenode -format
```

（2）启动HDFS

```
[atguigu@hadoop102 hadoop-2.7.2]$ sbin/start-dfs.sh

[atguigu@hadoop102 hadoop-2.7.2]$ jps

4166 NameNode

4482 Jps

4263 DataNode
```

```
[atguigu@hadoop103 hadoop-2.7.2]$ jps

3218 DataNode

3288 Jps
```



```
[atguigu@hadoop104 hadoop-2.7.2]$ jps

3221 DataNode

3283 SecondaryNameNode

3364 Jps
```

（3）启动YARN

```
[atguigu@hadoop103 hadoop-2.7.2]$ sbin/start-yarn.sh
```

注意：NameNode和ResourceManager如果不是同一台机器，不能在NameNode上启动 YARN，应该在ResouceManager所在的机器上启动YARN。

（4）Web端查看SecondaryNameNode

（a）浏览器中输入：http://hadoop104:50090/status.html



#### 集群时间同步

![image-20200404170024478](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200404170024478.png)