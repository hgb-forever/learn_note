# Nginx基本概念

Nginx是一个高性能的HTTP和反向代理服务器，特点是内存占用少，并发能力强。Nginx专为性能优化而开发。支持**热部署**。

# Nginx安装

安装成功后，需要在开放linux的端口，或者关闭防火墙

# Nginx常用命令

使用命令需要进入/usr/local/nginx/sbin目录下

## 查看版本号

```sh
./nginx -v
```

## 启动nginx

```sh
./nginx
```

## 关闭nginx

```sh
./nginx -s stop
```

## 重新加载nginx

```sh
./nginx -s reload
```



# Nginx配置文件

文件位置:/usr/local/nginx/conf/nginx.conf

##  文件组成

由三部分组成

- 全局块
- events块
- http块

## 全局块

从文件头到events块之间的内容，主要会设置一些影响nginx服务器整体运行的配置指令，比如

```sh
worker_processes 1; //值越大，表示可以支持的并发量越多
```

## events块

events块涉及的指令主要影响Nginx服务与用户的网络连接，比如

```sh
worker_connections 1024;//支持的最大连接数
```

## http块

Nginx配置最频繁的部分

组成：

- http全局块
- server块

# 反向代理

正向代理：用户通过代理服务器访问互联网

![image-20210526103404025](.\images\image-20210526103404025.png)

反向代理：反向代理服务器隐藏了具体的请求地址，对用户而言，用户请求的是反向代理服务器的地址，而不是真正获取到资源的地址。

![image-20210526103413529](.\images\image-20210526103413529.png)

![image-20210526111659235](.\images\image-20210526111659235.png)



# 负载均衡

## 概念

单个服务器解决不了大量请求，我们增加服务器的数量，将请求分发到各个服务器上，将原先请求集中到单个服务器上的情况改为将请求分发到多个服务器上，将负载分发到不同的服务器，也就是我们所说的负载均衡。

## 实现

在nginx.conf配置文件中的http代码块中添加upstream myserver{}代码块

在代码块中添加负载均衡服务器列表。

![image-20210526115846215](.\images\image-20210526115846215.png)

修改server_name为ip地址，在location / {}中添加proxy_pass为刚刚添加的服务器列表别名

![image-20210526120002793](.\images\image-20210526120002793.png)

## 服务器分配策略

轮询：按照请求的时间顺序依次分配到每台服务器（默认策略）

weight：依据权重分配，权重越大服务器分配到的请求越多（在服务器列表的ip地址后加上weight=?）

ip_hash：根据客户端ip地址的hash结果分配，这样每个用户固定请求一个后端服务器，可以解决session不一致的问题。（在upstream代码块中添加ip_hash）

fair：按照后端服务器的响应时间来分配，响应时间短的服务器优先被分配（在upstream代码块中加入fair）

# 动静分离

将静态资源（css/html）和动态资源（js/jsp）放在不同的服务器上

具体实现：将静态资源和动态资源分别放在不同的两个文件夹下，配置两者的映射路径和文件路径即可。

# Nginx高可用集群

# Nginx原理

![image-20210526141545578](.\images\image-20210526141545578.png)

master发布任务，worker去竞争，竞争到的worker去执行具体的任务。一个master对应多个worker，服务可以保持不间断运行。每个worker是一个独立的进程，worker数量与cpu核数相同可以发挥最大的性能。即work_processes = cpu_kernel_num。

worker_connection表示一个worker进程所能建立连接的最大值。所以一个nginx能建立的最大连接数是worker_processes*worker_connecton。

最大并发数：对于不同的静态资源访问，一个请求需要两个连接，则最大并发数是worker_processes*worker_connection/2。对于反向代理的http请求，则需要四个连接，最大并发数是worker_processes\*worker_connection/4