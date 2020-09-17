# 概述

## 1.1 Flume定义

Flume是Cloudera提供的一个高可用的，高可靠的，分布式的海量日志采集、聚合和传输的系统。Flume基于流式架构，灵活简单。![image-20200409094748533](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200409094748533.png)

## 1.2Flume组成架构

![image-20200409095340448](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200409095340448.png)

![image-20200409095403181](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200409095403181.png)

## 1.2Agent

​	Agent是一个JVM进程，它以事件的形式将数据从源头送至目的，是Flume数据传输的基本单元。

​	Agent主要有3个部分组成，Source、Channel、Sink。

​	**Source**是负责接收数据到Flume Agent的组件。source和Channel之间存在put事务，成功则提交事务到Channel，失败回滚到source

​	**Channel**是位于Source和Sink之间的缓冲区。因此，Channel允许Source和Sink运作在不同的速率上。Channel是**线程安全**的，可以同时处理几个Source的写入操作和几个Sink的读取操作。Flume自带两种Channel：Memory Channel和File Channel。

​	**Sink**不断地轮询Channel中的事件且批量地移除它们，并将这些事件批量写入到存储或索引系统、或者被发送到另一个Flume Agent。Sink是完全事务性的。Sink与Channel为一个take事务，数据写入成功，则Channel删除事务，清除缓存区,否则回滚到Channel。

## 1.3Event

​	传输单元，Flume数据传输的基本单元，以事件的形式将数据从源头送至目的地。

## 1.4Flume Agent内部原理

![image-20200409101929562](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200409101929562.png)

# 快速入门

## 2.1Flume安装地址

1） Flume官网地址

http://flume.apache.org/

2）文档查看地址

http://flume.apache.org/FlumeUserGuide.html

3）下载地址

http://archive.apache.org/dist/flume/

## 2.2安装部署

1. 下载并安装Flume
2.  将flume/conf下的flume-env.sh.template文件修改为flume-env.sh，并配置flume-env.sh文件

```
[atguigu@hadoop102 conf]$ mv flume-env.sh.template flume-env.sh
[atguigu@hadoop102 conf]$ vi flume-env.sh
export JAVA_HOME=/opt/module/jdk1.8.0_144
```

## 2.3监控端口数据官方案例

在$flume_home/conf/flume-telnet-logger.conf文件中添加如下内容。

```
# Name the components on this agent
a1.sources = r1
a1.sinks = k1
a1.channels = c1

# Describe/configure the source
a1.sources.r1.type = netcat
a1.sources.r1.bind = localhost
a1.sources.r1.port = 44444

# Describe the sink
a1.sinks.k1.type = logger

# Use a channel which buffers events in memory
a1.channels.c1.type = memory
a1.channels.c1.capacity = 1000
a1.channels.c1.transactionCapacity = 100

# Bind the source and sink to the channel
a1.sources.r1.channels = c1
a1.sinks.k1.channel = c1
```

### 先开启flume监听端口

```
[atguigu@hadoop102 flume]$ bin/flume-ng agent --conf conf/ --name a1 --conf-file job/flume-telnet-logger.conf -Dflume.root.logger=INFO,console
```

参数说明：

​	--conf conf/  ：表示配置文件存储在conf/目录

​	--name a1	：表示给agent起名为a1

​	--conf-file job/flume-telnet.conf ：flume本次启动读取的配置文件是在job文件夹下的flume-telnet.conf文件。

​	-Dflume.root.logger==INFO,console ：-D表示flume运行时动态修改flume.root.logger参数属性值，并将控制台日志打印级别设置为INFO级别。日志级别包括:log、info、warn、error。

### 使用telnet工具向本机的44444端口发送内容

```
telnet localhost 44444
```

在会话窗口发送消息，flume服务端接送消息。