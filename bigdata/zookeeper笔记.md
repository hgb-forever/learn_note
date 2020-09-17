# zookeeper入门

## 1.1概述

Zookeeper是一个开源的分布式的，为分布式应用提供协调服务的Apache项目。它负责存储和管理大家都关心的数据，然后接受观察者的注册，一旦这些数据的状态发生变化。zookeeper就将负责通知已经在zookeeper上注册的那些观察者做出相应的反应。就好像银行管理人们的账户。

zookeeper = 文件系统 + 通知机制

## 1.2特点

1. zookeeper是由**一个leader**和多个follower组成的**集群**

2. 集群中只要有**半数以上**的节点存活，就能正常运行
3. **一致性**，每个zookeeper服务器保存相同的数据副本
4. 更新请求**顺序**进行，来自同一个Client的请求按其发送顺序进行。
5. 数据更新**原子性**，一次数据更新要么成功，要么失败。
6. **实时性**，在一定时间范围内，Client能及时获得最新信息

## 1.3数据结构

zookeeper的数据结构与Linux文件系统相似，**树形结构**，每个节点称作**ZNode**，每个ZNode默认能**存储1MB**的数据，每个ZNode都可以通过绝对路径**唯一标识**。

![image-20200405220703951](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200405220703951.png)

## 1.4应用场景

提供的服务包括：统一命名服务、**统一配置管理**、**统一集群管理**、服务器节点动态上下线、软负载均衡等。

![image-20200405221002109](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200405221002109.png)

## 1.5 下载地址

### 官网首页：

https://zookeeper.apache.org/

# Zookeeper安装

## 1．安装前准备

（1）安装Jdk

（2）拷贝Zookeeper安装包到Linux系统下

（3）解压到指定目录

## 2．配置修改

（1）将/opt/module/zookeeper-3.4.10/conf这个路径下的zoo_sample.cfg修改为zoo.cfg；

```
[atguigu@hadoop102 conf]$ mv zoo_sample.cfg zoo.cfg
```

（2）打开zoo.cfg文件，修改dataDir路径：

```
[atguigu@hadoop102 zookeeper-3.4.10]$ vim zoo.cfg
```

修改如下内容：

```
dataDir=/opt/module/zookeeper-3.4.10/zkData
```

（3）在/opt/module/zookeeper-3.4.10/这个目录上创建zkData文件夹

```
[atguigu@hadoop102 zookeeper-3.4.10]$ mkdir zkData
```

## 3．操作Zookeeper

（1）启动Zookeeper：

```
[atguigu@hadoop102 zookeeper-3.4.10]$ bin/zkServer.sh start
```

（2）查看进程是否启动：

```
[atguigu@hadoop102 zookeeper-3.4.10]$ jps
4020 Jps
4001 QuorumPeerMain
```

（3）查看状态：

```
[atguigu@hadoop102 zookeeper-3.4.10]$ bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /opt/module/zookeeper-3.4.10/bin/../conf/zoo.cfg
Mode: standalone
```

（4）启动客户端：

```
[atguigu@hadoop102 zookeeper-3.4.10]$ bin/zkCli.sh
```

（5）退出客户端：

```
[zk: localhost:2181(CONNECTED) 0] quit
```

（6）停止Zookeeper

```
[atguigu@hadoop102 zookeeper-3.4.10]$ bin/zkServer.sh stop
```

## 4.配置参数解读

1．tickTime =2000：通信心跳数，Zookeeper服务器与客户端心跳时间，单位毫秒

2．initLimit =10：LF初始通信时限，Leader初始连接时能容忍的最多心跳数（tickTime的数量）

3．syncLimit =5：LF同步通信时限。假如响应超过syncLimit * tickTime，Leader认为Followers死掉，从服务器列表中删除Follwer。

4．dataDir：数据文件目录+数据持久化路径。主要用于保存Zookeeper中的数据。

5．clientPort =2181：客户端连接端口

# zookeeper内部原理

## 1.选举机制（面试重点）

半数机制：集群中半数以上机器存活，集群可用。所以Zookeeper适合安装奇数台服务器。

假设有五台服务器组成的Zookeeper集群，它们的id从1-5，同时它们都是最新启动的，也就是没有历史数据，在存放数据量这一点上，都是一样的。假设这些服务器依序启动，来看看会发生什么，如图所示。

![img](file:///C:\Users\甘正\AppData\Local\Temp\ksohtml\wps34D6.tmp.png)

（1）服务器1启动，此时只有它一台服务器启动了，它发出去的报文没有任何响应，所以它的选举状态一直是LOOKING状态。

（2）服务器2启动，它与最开始启动的服务器1进行通信，互相交换自己的选举结果，由于两者都没有历史数据，所以id值较大的服务器2胜出，但是由于没有达到超过半数以上的服务器都同意选举它(这个例子中的半数以上是3)，所以服务器1、2还是继续保持LOOKING状态。

（3）服务器3启动，根据前面的理论分析，服务器3成为服务器1、2、3中的老大，而与上面不同的是，此时有三台服务器选举了它，所以它成为了这次选举的Leader。

（4）服务器4启动，根据前面的分析，理论上服务器4应该是服务器1、2、3、4中最大的，但是由于前面已经有半数以上的服务器选举了服务器3，所以它只能接收当小弟的命了。

（5）后续增加节点，leader不再发生变化，除非leader死亡，重新选举。

## 2.节点类型

![image-20200406161607585](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200406161607585.png)

## 3.Stat结构体

1）czxid-创建节点的事务zxid

每次修改ZooKeeper状态都会收到一个zxid形式的时间戳，也就是ZooKeeper事务ID。

事务ID是ZooKeeper中所有修改总的次序。每个修改都有唯一的zxid，如果zxid1小于zxid2，那么zxid1在zxid2之前发生。

2）ctime - znode被创建的毫秒数(从1970年开始)

3）mzxid - znode最后更新的事务zxid

4）mtime - znode最后修改的毫秒数(从1970年开始)

5）pZxid-znode最后更新的子节点zxid

6）cversion - znode子节点变化号，znode子节点修改次数

7）dataversion - znode数据变化号

8）aclVersion - znode访问控制列表的变化号

9）ephemeralOwner- 如果是临时节点，这个是znode拥有者的session id。如果不是临时节点则是0。

10）dataLength- znode的数据长度

11）numChildren - znode子节点数量

## 4.监听器原理

![image-20200406162226201](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200406162226201.png)

## 5.写数据流程

当Client向Server1请求写入数据是，server1通知leader，leader通知所有server。当半数以上的server写数据成功，则认为操作成功，leader通知server1.server1通知Client写入数据成功。

# zookeeper实战

## 分布式安装部署

在三个节点上下载安装zookeeper

创建zKData文件，在该目录下创建myid文件并添加serverId唯一编号

重命名/opt/module/zookeeper-3.4.10/conf这个目录下的zoo_sample.cfg为zoo.cfg，并修改数据存储路径配置

```
dataDir=/opt/module/zookeeper-3.4.10/zkData
```

- 增加如下配置

```xml
#######################cluster##########################
server.2=hadoop102:2888:3888
server.3=hadoop103:2888:3888
server.4=hadoop104:2888:3888
```

- 同步zoo.cfg到三台服务器，2888是服务器与leader交换信息的端口，3888是选举leader的端口。

## 客户端命令行操作

| 命令基本语法     | 功能描述                                         |
| ---------------- | ------------------------------------------------ |
| help             | 显示所有操作命令                                 |
| ls path [watch]  | 使用 ls 命令来查看当前znode中所包含的内容        |
| ls2 path [watch] | 查看当前节点数据并能看到更新次数等数据           |
| create           | 普通创建-s  含有序列-e  临时（重启或者超时消失） |
| get path [watch] | 获得节点的值                                     |
| set              | 设置节点的具体值                                 |
| stat             | 查看节点状态                                     |
| delete           | 删除节点                                         |
| rmr              | 递归删除节点                                     |

具体操作看web文档，以及Java连接ZkClient。

zookeeper集群最少需要3台服务器