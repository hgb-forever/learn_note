# 概述

## 1.1 定义

Kafka 是一个分布式的基于**发布**/**订阅**模式的**消息队列**（Message Queue），主要应用于大数据实时处理领域。

![image-20200413151231797](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200413151231797.png)

（1）点对点模式（一对一，消费者主动拉取数据，消息收到后消息清除）

（2）发布/订阅模式（一对多，数据生产后，推送给所有订阅者）

## 1.2为什么需要消息队列

- 解耦
- 冗余
- 扩展性
- 灵活性
- 可恢复性
- 顺序保证
- 缓冲
- 异步通信

## 1.3什么是kafka

是Apache软件基金会开发的一个开源消息系统项目，Kafka是一个分布式消息队列，依赖zookeeper集群保存一些meta信息。

## 1.4Kafka框架

![image-20200413152901867](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200413152901867.png)

# Kafka集群部署

## 2.1环境准备

### 2.1.1jar包下载

http://kafka.apache.org/downloads.html

## 2.2集群部署

1.解压安装包

2.在kafka目录下创建logs文件夹

3.在conf文件夹中修改配置文件server.properties

```
#broker的全局唯一编号，不能重复
broker.id=0
#删除topic功能使能
delete.topic.enable=true
#处理网络请求的线程数量
num.network.threads=3
#用来处理磁盘IO的现成数量
num.io.threads=8
#发送套接字的缓冲区大小
socket.send.buffer.bytes=102400
#接收套接字的缓冲区大小
socket.receive.buffer.bytes=102400
#请求套接字的缓冲区大小
socket.request.max.bytes=104857600
#kafka运行日志存放的路径	
log.dirs=/opt/module/kafka/logs
#topic在当前broker上的分区个数
num.partitions=1
#用来恢复和清理data下数据的线程数量
num.recovery.threads.per.data.dir=1
#segment文件保留的最长时间，超时将被删除
log.retention.hours=168
#配置连接Zookeeper集群地址
zookeeper.connect=hadoop102:2181,hadoop103:2181,hadoop104:2181
```

4.配置环境变量，分发kafka到集群

5.在修改其他服务器上的borker.id

6.集群启动kafka

```
[atguigu@hadoop102 kafka]$ bin/kafka-server-start.sh config/server.properties &
[atguigu@hadoop103 kafka]$ bin/kafka-server-start.sh config/server.properties &
[atguigu@hadoop104 kafka]$ bin/kafka-server-start.sh config/server.properties &
```

7.关闭集群

```
[atguigu@hadoop102 kafka]$ bin/kafka-server-stop.sh stop
[atguigu@hadoop103 kafka]$ bin/kafka-server-stop.sh stop
[atguigu@hadoop104 kafka]$ bin/kafka-server-stop.sh stop
```

## 2.3kafka命令行操作

1）查看当前服务器中的所有topic

```
bin/kafka-topics.sh --zookeeper hadoop102:2181 --list
```

2）创建topic

```
bin/kafka-topics.sh --zookeeper hadoop102:2181 --create --replication-factor 3 --partitions 1 --topic first
```

3）删除topic

```
 bin/kafka-topics.sh --zookeeper hadoop102:2181 --delete --topic first
```

需要server.properties中设置delete.topic.enable=true否则只是标记删除或者直接重启。

4）发送消息

```
bin/kafka-console-producer.sh --broker-list hadoop102:9092 --topic first
```

```
>hello world
>atguigu  atguigu
```

5）消费消息

```
 bin/kafka-console-consumer.sh --zookeeper hadoop102:2181 --from-beginning --topic first
```

--from-beginning：会把first主题中以往所有的数据都读取出来。根据业务场景选择是否增加该配置。

6）查看某个Topic的详情

```
bin/kafka-topics.sh --zookeeper hadoop102:2181 -describe --topic first
```

# Kafka工作流程分析

![image-20200413155018412](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200413155018412.png)

## 3.1kafka生产过程分析

### 3.1.1写入方式

​	producer采用推（push）模式将消息发布到broker，每条消息都被追加（append）到分区（patition）中，属于顺序写磁盘（顺序写磁盘效率比随机写内存要高，保障kafka吞吐率）。

### 3.1.2分区

​	消息发送时都被发送到一个topic，其本质就是一个目录，而topic是由一些Partition Logs(分区日志)组成，其组织结构如下图所示：

![image-20200413155312727](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200413155312727.png)

![image-20200413155320398](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200413155320398.png)

​	我们可以看到，每个Partition中的消息都是有序的，生产的消息被不断追加到Partition log上，其中的每一个消息都被赋予了一个唯一的offset值。

分区的原则

（1）指定了patition，则直接使用；

（2）未指定patition但指定key，通过对key的value进行hash出一个patition；

（3）patition和key都未指定，使用轮询选出一个patition。

### 3.1.3 副本（Replication）

同一个partition可能会有多个replication（对应 server.properties 配置中的 default.replication.factor=N）。没有replication的情况下，一旦broker 宕机，其上所有 patition 的数据都不可被消费，同时producer也不能再将数据存于其上的patition。引入replication之后，同一个partition可能会有多个replication，而这时需要在这些replication之间选出一个leader，producer和consumer只与这个leader交互，其它replication作为follower从leader 中复制数据。

3.1.4写入过程

![img](file:///C:\Users\甘正\AppData\Local\Temp\ksohtml\wpsDFE1.tmp.png)

## 3.2Borker保存信息

### 3.2.1存储方式

物理上把topic分成一个或多个patition（对应 server.properties 中的num.partitions=3配置），每个patition物理上对应一个文件夹（该文件夹存储该patition的所有消息和索引文件）。

### 3.2.2 存储策略

无论消息是否被消费，kafka都会保留所有消息。有两种策略可以删除旧数据：
1）基于时间：log.retention.hours=168
2）基于大小：log.retention.bytes=1073741824
需要注意的是，因为Kafka读取特定消息的时间复杂度为O(1)，即与文件大小无关，所以这里删除过期文件与提高 Kafka 性能无关。

关于时间复杂度：https://blog.csdn.net/qq_41523096/article/details/82142747

### 3.2.3zookeeper存储结构

![image-20200413163420554](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200413163420554.png)

注意：producer不在zk中注册，消费者在zk中注册。

## 3.3kafka消费过程分析

### 3.3.1 高级API

#### 1）高级API优点

高级API 写起来**简单**
不需要自行去管理offset，系统通过zookeeper自行管理。
不需要管理分区，副本等情况，系统自动管理。
消费者断线会自动根据上一次记录在zookeeper中的offset去接着获取数据（默认设置1分钟更新一下zookeeper中存的offset）
可以使用group来区分对同一个topic 的不同程序访问分离开来（不同的group记录不同的offset，这样不同程序读取同一个topic才不会因为offset互相影响）

#### 2）高级API缺点

不能自行控制offset（对于某些特殊需求来说）
不能细化控制如分区、副本、zk等

### 3.3.2 低级API

#### 1）低级 API 优点

能够让开发者自己控制offset，想从哪里读取就从哪里读取。
自行控制连接分区，对分区自定义进行负载均衡
对zookeeper的依赖性降低（如：offset不一定非要靠zk存储，自行存储offset即可，比如存在文件或者内存中）

#### 2）低级API缺点

太过**复杂**，需要自行控制offset，连接哪个分区，找到分区leader 等。

### 3.3.3 消费者组

![img](file:///C:\Users\甘正\AppData\Local\Temp\ksohtml\wps915.tmp.png)

​		消费者是以consumer group消费者组的方式工作，由一个或者多个消费者组成一个组，共同消费一个topic。每个分区在同一时间只能由group中的一个消费者读取，但是多个group可以同时消费这个partition。在图中，有一个由三个消费者组成的group，有一个消费者读取主题中的两个分区，另外两个分别读取一个分区。某个消费者读取某个分区，也可以叫做某个消费者是某个分区的拥有者。

​	在这种情况下，消费者可以通过水平扩展的方式同时读取大量的消息。另外，如果一个消费者失败了，那么其他的group成员会自动负载均衡读取之前失败的消费者读取的分区。

### 3.3.4 消费方式

​	consumer采用pull（拉）模式从broker中读取数据。
​	push（推）模式很难适应消费速率不同的消费者，因为消息发送速率是由broker决定的。它的目标是尽可能以最快速度传递消息，但是这样很容易造成consumer来不及处理消息，典型的表现就是拒绝服务以及网络拥塞。而pull模式则可以根据consumer的消费能力以适当的速率消费消息。