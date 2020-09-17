# 第一章 介绍

## 一.介绍

Spark是基于内存的迭代计算



## 二.Local模式

仅仅本机运行

Local表示所有计算都运行在一个线程当中。通常用于测试

Local[k]代表有k个线程在跑

Local[*]代表跑满



## 三.spark使用

1.bin/spark-submit 参数，可以用来提交任务

```scala
--master 指定Master的地址，默认为Local
--class: 你的应用的启动类 (如 org.apache.spark.examples.SparkPi)
--deploy-mode: 是否发布你的驱动到worker节点(cluster) 或者作为一个本地客户端 (client) (default: client)*
--conf: 任意的Spark配置属性， 格式key=value. 如果值包含空格，可以加引号“key=value” 
application-jar: 打包好的应用jar,包含依赖. 这个URL在集群中全局可见。 比如hdfs:// 共享存储系统， 如果是 file:// path， 那么所有的节点的path都包含同样的jar
application-arguments: 传给main()方法的参数
--executor-memory 1G 指定每个executor可用内存为1G
--total-executor-cores 2 指定每个executor使用的cup核数为2个
```

案例

```scala
bin/spark-submit \
--class org.apache.spark.examples.SparkPi \
--executor-memory 1G \
--total-executor-cores 2 \
./examples/jars/spark-examples_2.11-2.1.1.jar \
```

2.bin/spark-shell，进入命令行环境，默认很多东西会创建好，比如sc变量

jsp命令查看java运行的程序

spark-shell提示的，网址，比如hadoop102:4040，是查看网页版的程序运行状态器，即Spark Jobs

yarn application -list，查看应用id



## 四.WordCount程序

1.load(textFile)

2.flat(flatMap)

3.group(groupBy或者map为元组)

4.聚合(reduceByKey)

5.打印（collect）

6.停止（sc.stop）

![image-20200522164439514](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200522164439514.png)

load:sc.textFile("")

扁平化: flatmap()

分组聚合前的重构数据:map()

分组聚合:reduceBykey()

## 五.spark简易流程

![img](https://img-blog.csdnimg.cn/20190224143830959.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzOTY1MA==,size_16,color_FFFFFF,t_70)

## 六.Idea开发

1.maven项目

2.引入spark依赖

3.添加build段落

4.新建scala，标记为source dir

5.新建scala文件。因为是maven build有引入，所以可以自动添加scala标记

6.new SparkContext，创建context

7.new SparkConf，创建配置，有一系列set方法

```xml
<!-- pom.xml -->
<dependencies>
    <dependency>
        <groupId>org.apache.spark</groupId>
        <artifactId>spark-core_2.11</artifactId>
        <version>2.1.1</version>
    </dependency>
</dependencies>
<build>
        <finalName>WordCount</finalName>
        <plugins>
<plugin>
                <groupId>net.alchim31.maven</groupId>
<artifactId>scala-maven-plugin</artifactId>
                <version>3.2.2</version>
                <executions>
                    <execution>
                       <goals>
                          <goal>compile</goal>
                          <goal>testCompile</goal>
                       </goals>
                    </execution>
                 </executions>
            </plugin>
        </plugins>
</build>
```

编写代码：

```scala
import org.apache.spark.{SparkConf, SparkContext}

object WordCount{

  def main(args: Array[String]): Unit = {

//1.创建SparkConf并设置App名称
    val conf = new SparkConf().setAppName("WC")

//2.创建SparkContext，该对象是提交Spark App的入口
    val sc = new SparkContext(conf)

    //3.使用sc创建RDD并执行相应的transformation和action
    sc.textFile(args(0)).flatMap(_.split(" ")).map((_, 1)).reduceByKey(_+_, 1).sortBy(_._2, false).saveAsTextFile(args(1))

//4.关闭连接
    sc.stop()
  }
}
```

打包插件：

```xml
<plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-assembly-plugin</artifactId>
                <version>3.0.0</version>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>WordCount</mainClass>
                        </manifest>
                    </archive>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
      </plugin>
```

本地Spark程序调试需要使用local提交模式，即将本机当做运行环境，Master和Worker都为本机。运行时直接加断点调试即可。如下：
创建SparkConf的时候设置额外属性，表明本地执行：
val conf = new SparkConf().setAppName(“WC”).setMaster(“local[*]”)
如果本机操作系统是windows，如果在程序中使用了hadoop相关的东西，比如写入文件到HDFS，则会遇到如下异常：
![img](https://img-blog.csdnimg.cn/20190224151201889.png)

出现这个问题的原因，并不是程序的错误，而是用到了hadoop相关的服务，解决办法是将附加里面的hadoop-common-bin-2.7.3-x64.zip解压到任意目录。

在IDEA中配置Run Configuration，添加HADOOP_HOME变量

![img](https://img-blog.csdnimg.cn/20190224151348361.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzOTY1MA==,size_16,color_FFFFFF,t_70)





## 七.Yarn部署

![img](https://img-blog.csdnimg.cn/20190224150829271.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDAzOTY1MA==,size_16,color_FFFFFF,t_70)

![image-20200523164150925](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200523164150925.png)

yarn-client：有交互

yarn-cluster：无交互

区别是driver程序要哪里执行，运行节点

需要修改以下配置文件

需要启动HDFS和Yarn集群

###  安装使用

1）修改hadoop配置文件yarn-site.xml,添加如下内容：

```xml
[atguigu@hadoop102 hadoop]$ vi yarn-site.xml
        <!--是否启动一个线程检查每个任务正使用的物理内存量，如果任务超出分配值，则直接将其杀掉，默认是true -->
        <property>
                <name>yarn.nodemanager.pmem-check-enabled</name>
                <value>false</value>
        </property>
        <!--是否启动一个线程检查每个任务正使用的虚拟内存量，如果任务超出分配值，则直接将其杀掉，默认是true -->
        <property>
                <name>yarn.nodemanager.vmem-check-enabled</name>
                <value>false</value>
        </property>
```

2）修改spark-env.sh，添加如下配置：

```xml
[atguigu@hadoop102 conf]$ vi spark-env.sh

YARN_CONF_DIR=/opt/module/hadoop-2.7.2/etc/hadoop
```

3）分发配置文件：

```
[atguigu@hadoop102 conf]$ xsync /opt/module/hadoop-2.7.2/etc/hadoop/yarn-site.xml
```

4）执行一个程序：

```shell
[atguigu@hadoop102 spark]$ bin/spark-submit \
--class org.apache.spark.examples.SparkPi \
--master yarn \
--deploy-mode client \
./examples/jars/spark-examples_2.11-2.1.1.jar \
100
```



### 日志查看

1.修改配置文件spark-defaults.conf

```
spark.yarn.historyServer.address=hadoop102:18080
spark.history.ui.port=18080
```

2.重启历史服务

```bash
MR历史服务操作mr-jobhistory-daemon.sh start historyserver
[atguigu@hadoop102 spark]$ sbin/stop-history-server.sh 
stopping org.apache.spark.deploy.history.HistoryServer
[atguigu@hadoop102 spark]$ sbin/start-history-server.sh 
starting org.apache.spark.deploy.history.HistoryServer, logging to /opt/module/spark/logs/spark-atguigu-org.apache.spark.deploy.history.HistoryServer-1-hadoop102.out
```

3.提交任务

```
bin/spark-submit \
--class org.apache.spark.examples.SparkPi \
--master yarn \
--deploy-mode client \
./examples/jars/spark-examples_2.11-2.1.1.jar \
100
```

4.去hadoop历史服务网页查看

spark-shell设置为master yarn模式

```
spark-shell --master yarn
```

## 2.6 Standalone模式

Spark客户端直接连接Mesos；不需要额外构建Spark集群。国内应用比较少，更多的是运用yarn调度。

## 2.7 几种模式对比

![img](https://img-blog.csdnimg.cn/20190224151119346.png)



## 十.Driver和Executor关系

Driver：创建Spark应用上下文环境对象的程序，是个驱动器

Executor：接收任务并执行任务

Driver发送任务

Executor向Driver反馈任务执行情况

所有算子里的计算功能(参数函数)都是Executor执行

如果Executor里用到了Driver里的变量，这个变量要可以序列化，方便Driver传送这个变量给Executor



## 十二.高可用HA

利用zookeeper实现高可用，编写配置文件即可



# 第二章.RDD

## 一.JAVA IO回顾

 体现了装饰着模式-设计模式

输入 输出

字节 字符

 FileInputStream

BufferedInputStream

BufferedReader,InputStreamReader

![image-20200523202258425](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200523202258425.png)



## 二.RDD概述

![image-20200523202902911](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200523202902911.png)

不使用collect方法是不会读取数据的，和java Io类似

RDD运算是装饰者模式，将数据处理逻辑封装，即对数据计算的抽象，不保留数据

> RDD的中文解释为：弹性分布式数据集，全称Resilient Distributed Datasets。宾语是dataset，即内存中的数据库。RDD 只读、可分区，这个数据集的全部或部分可以缓存在内存中，在多次计算间重用。 所谓弹性，是指内存不够时可以与磁盘进行交换。这涉及到了RDD的另一特性：内存计算，就是将数据保存到内存中。同时，为解决内存容量限制问题，Spark为我们提供了最大的自由度，所有数据均可由我们来进行cache的设置，包括是否cache和如何cache。

RDD属性：

1)   一组分区（Partition），即数据集的基本组成单位;

2)   一个计算每个分区的函数;

3)   RDD之间的依赖关系;

4)   一个Partitioner，即RDD的分片函数;

5)   一个列表，存储存取每个Partition的优先位置（preferred location）。

RDD特点：

1）可分区：适合并行计算。

2）只读：要想改变RDD中的数据，只能在现有的RDD基础上创建新的RDD

3）缓存：可缓存数据，重复利用



依赖关系：血缘。窄依赖：一一对应；宽依赖：多对多

![image-20200525151709659](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200525151709659.png)

移动数据不如移动计算

算子：

1.转换算子(transformations)，进行数据变换，构建RDD的血缘关系

2.行动算子(actions)，触发计算，产生输出或者保存到文件系统中



checkpoint：使用检查点将数据保存到持久化，来切断较长的血缘关系。



## 二.RDD编程

> 在Spark中，RDD被表示为对象，通过对象上的方法调用来对RDD进行转换。经过一系列的transformations定义RDD之后，就可以调用actions触发RDD的计算，action可以是向应用程序返回结果(count, collect等)，或者是向存储系统保存数据(saveAsTextFile等)。在Spark中，==只有遇到action，才会执行RDD的计算(即延迟计算)==，这样在运行时可以通过管道的方式传输多个转换。

> 要使用Spark，开发者需要编写一个Driver程序，它被提交到集群以调度运行Worker，如下图所示。Driver中定义了一个或多个RDD，并调用RDD上的action，==Worker则执行RDD分区计算任务。==

1.RDD创建

从集合里：sc.makeRDD(集合，创建时可以指定分区数量)，sc.parallelize(集合)

`val rdd = sc.parallelize(1 to 16,4)`

从文件：sc.textFile

从其他RDD

保存到文件：rdd.saveAsTextFile

RDD分区规则用hadoopFile，分片规则完全一样

计算数据的分区与存储数据到分区是分开的

hadoopfs按行存储于读取



## 三.RDD转换算子

分为Value类型和Key-Value类型



### 四.Vaule类型转换算子

1.map(func)

返回一个新的RDD，该RDD由每一个输入元素经过func函数转换后组成



2.mapPartitions(func)

类似于map，但独立地在RDD的每一个分片上运行，因此在类型为T的RDD上运行时，func的函数类型必须是Iterator[T] => Iterator[U]。假设有N个元素，有M个分区，那么map的函数的将被调用N次,而mapPartitions被调用M次,一个函数一次处理所有分区。

目的是将分区里的计算集中分给一个Executor，容易OOM(堆内存溢出)内存不够用



3.mapPartitionsWithInex(func)

类似于mapPartitions，但func带有一个整数参数表示分片的索引值，因此在类型为T的RDD上运行时，func的函数类型必须是(Int, Interator[T]) => Iterator[U]；

可以根据分区号追踪任务



4.flatMap(func)

类似于map，但是每一个输入元素可以被映射为0或多个输出元素（所以func应该返回一个序列，而不是单一元素）



5.glom()

将每一个分区形成一个数组，形成新的RDD类型时RDD[Array[T]]



6.groupBy(func)

分组，按照传入函数的返回值进行分组。将相同的key对应的值放入一个迭代器。返回对偶元祖



7.filter(func)

过滤。返回一个新的RDD，该RDD由经过func函数计算后返回值为true的输入元素组成



8.sample(withReplacement, fraction, seed)

以指定的随机种子随机抽样出数量为fraction的数据，withReplacement表示是抽出的数据是否放回，true为有放回的抽样，false为无放回的抽样，seed用于指定随机数生成器种子，用于给数据打分。fraction指定分数，数据得分大于该值则留下。



9.distinct([numTasks])

对源RDD进行去重后返回一个新的RDD。默认情况下，只有8个并行任务来操作，但是可以传入一个可选的numTasks参数改变它



10.coalesce(numPartitions)

缩减分区数，用于大数据集过滤后，提高小数据集的执行效率



11.repatition(numPartitions) 

根据分区数，重新通过网络随机洗牌所有数据。repartition实际上是调用的coalesce，默认是进行shuffle的



12.sortBy(func,[ascending], [numTasks])

使用func先对数据进行处理，按照处理后的数据比较结果排序，默认为正序

分区数量改变，task改变，并行度度改变



13.pipe(command, [envVars]) 

管道，针对每个分区，都执行一个shell脚本，返回输出的RDD

注意：脚本需要放在Worker节点可以访问到的位置



### 五.双Value

 两个RDD集合操作，交并差笛卡尔积zip等

1.union(otherDataset) 

对源RDD和参数RDD求并集后返回一个新的RDD——A∪B



2.subtract (otherDataset)

计算差的一种函数，去除两个RDD中相同的元素，不同的RDD将保留下来——A-B



3.intersection(otherDataset)

对源RDD和参数RDD求交集后返回一个新的RDD——A∩B



4.cartesian(otherDataset)

笛卡尔积（尽量避免使用）——A×B



5.zip(otherDataset)

将两个RDD组合成Key/Value形式的RDD,这里默认两个RDD的partition数量以及元素数量都相同，否则会抛出异常。



### 六.kv算子

这些算子被调用，要求数据一定要是kv格式



1.partitionBy

对pairRDD进行分区操作，如果原有的partionRDD和现有的partionRDD是一致的话就不进行分区， 否则会生成ShuffleRDD，即会产生shuffle过程。



2.groupBykey

groupByKey也是对每个key进行操作，但只生成一个sequence



3.reduceBykey(func, [numTasks])

在一个(K,V)的RDD上调用，返回一个(K,V)的RDD，使用指定的reduce函数，将相同key的值聚合到一起，reduce任务的个数可以通过第二个可选的参数来设置



4.aggregateBykey(zeroValue:U,[partitioner: Partitioner])

在kv对的RDD中，按key将value进行分组合并，合并时，==将每个value和初始值作为seq函数的参数==，进行计算，返回的结果作为一个新的kv对，然后再将结果按照key进行合并，最后将每个分组的value传递给combine函数进行计算（先将前两个value进行计算，将返回结果和下一个value传给combine函数，以此类推），将key与计算结果作为一个新的kv对输出



5.foldBykey(zeroValue: V)(func: (V, V) => V): RDD[(K, V)]

aggregateByKey的简化操作，==seqop和combop相同==



6.combineBykey(createCombiner: V => C, mergeValue: (C, V) => C, mergeCombiners: (C, C) => C) 

对相同K，把V合并成一个二元组。对分区内的相同key对应的值做合并，再对每个分区相同key对应的值做合并。



7.sortByKey([ascending], [numTasks])

在一个(K,V)的RDD上调用，K必须实现Ordered接口，返回一个按照key进行排序的(K,V)的RDD



8.mapValues

 针对于(K,V)形式的类型只对V进行操作



9.join(otherDataset, [numTasks]) 

在类型为(K,V)和(K,W)的RDD上调用，返回一个相同key对应的所有元素对在一起的(K,(V,W))的RDD



10.cogroup(otherDataset, [numTasks])

在类型为(K,V)和(K,W)的RDD上调用，返回一个(K,(Iterable<V>,Iterable<W>))类型的RDD



## 七.行动算子

把RDD里的数据当做一个数组来处理

 

1.reduce

通过func函数聚集RDD中的所有元素，先聚合分区内数据，再聚合分区间数据

 

2.collect

在驱动程序中，以数组的形式返回数据集的所有元素

 

3.count

返回RDD中元素的个数

 

4.first

返回RDD中元素的个数

 

5.take

返回一个由RDD的前n个元素组成的数组

 

6.takeOrdered

返回该RDD排序后的前n个元素组成的数组

 

7.aggregate

aggregate函数将每个分区里面的元素通过seqOp和初始值进行聚合，然后用combine函数将每个分区的结果和初始值(zeroValue)进行combine操作。这个函数最终返回的类型不需要和RDD中元素类型一致

 

8.fold

折叠操作，aggregate的简化操作，seqop和combop一样

 

9.saveAsTextFile

将数据集的元素以textfile的形式保存到HDFS文件系统或者其他支持的文件系统，对于每个元素，Spark将会调用toString方法，将它装换为文件中的文本

 

10.saveAsSequenceFile

将数据集中的元素以Hadoop sequencefile的格式保存到指定的目录下，可以使HDFS或者其他Hadoop支持的文件系统

 

11.saveAsObjectFile
用于将RDD中的元素序列化成对象，存储到文件中

 

12.countByKey

针对(K,V)类型的RDD，返回一个(K,Int)的map，表示每一个key对应的元素个数

 

13.foreach(func)

 在数据集的每一个元素上，运行函数func进行更新

 

## 八.Task执行序列化

网络里传递的对象一定要序列化

 

## 九.RDD依赖关系

 RDD里保存了依赖关系，出错了可以重来一遍

toDebugString:查看血缘

dependicies:查看依赖

 

窄依赖：窄依赖指的是每一个父RDD的Partition最多被子RDD的一个Partition使用,窄依赖我们形象的比喻为独生子女

 

宽依赖：宽依赖指的是多个子RDD的Partition会依赖同一个父RDD的Partition，会引起shuffle,总结：宽依赖我们形象的比喻为超生

 

## 十.DAG

有向无环图 

DAG划分stage，**宽依赖是划分Stage的依据**

 

## 十一.任务划分

RDD任务切分中间分为：Application、Job、Stage和Task

 

## 十二.缓存

persist或者cache方法内存缓存

为的是存储耗时动作

 

## 十三.检查点 

 checkponit函数持久化，要先用setCheckoutDir设置

缓存和检查点适合长链计算，防出错

 

十四.RDD分区器

 通过使用RDD的partitioner 属性来获取 RDD 的分区方式

 

Hash分区

 HashPartitioner分区的原理：对于给定的key，计算其hashCode，并除以分区的个数取余，如果余数小于0，则用余数+分区的个数（否则加0），最后返回的值就是这个key所属的分区ID。

 

Ranger分区

HashPartitioner分区弊端：可能导致每个分区中数据量的不均匀，极端情况下会导致某些分区拥有RDD的全部数据。

RangePartitioner作用：将一定范围内的数映射到某一个分区内，尽量保证每个分区中数据量的均匀，而且分区与分区之间是有序的，一个分区中的元素肯定都是比另一个分区内的元素小或者大，但是分区内的元素是不能保证顺序的。简单的说就是将一定范围内的数映射到某一个分区内

 

自定义分区

要实现自定义的分区器，你需要继承 org.apache.spark.Partitioner 类并实现下面三个方法。

（1）numPartitions: Int:返回创建出来的分区数。

（2）getPartition(key: Any): Int:返回给定键的分区编号(0到numPartitions-1)。

（3）equals():Java 判断相等性的标准方法。这个方法的实现非常重要，Spark 需要用这个方法来检查你的分区器对象是否和其他分区器实例相同，这样 Spark 才可以判断两个 RDD 的分区方式是否相同

 

 使用自定义的 Partitioner 是很容易的:只要把它传给 partitionBy() 方法即可。Spark 中有许多依赖于数据混洗的方法，比如 join() 和 groupByKey()，它们也可以接收一个可选的 Partitioner 对象来控制输出数据的分区方式

 

 

 十五.数据读取与保存

Spark的数据读取及数据保存可以从两个维度来作区分：文件格式以及文件系统。

文件格式分为：Text文件、Json文件、Csv文件、Sequence文件以及Object文件；

文件系统分为：本地文件系统、HDFS、HBASE以及数据库。

使用RDD读取JSON文件处理很复杂，同时SparkSQL集成了很好的处理JSON文件的方式，所以应用中多是采用SparkSQL处理JSON文件

 

读取mysql：引入mysql依赖，通过JdbcRDD访问

为避免连接数过多，可以以partition为单位操作数据库，可能OOM

 

读取hbase：引入hbase依赖，newAPIHadoopRDD创建

 

十六.累加器

Spark三大数据结构：

1.RDD：分布式数据集

2.广播变量：分布式只读共享变量

3.累加器：分布式只写共享变量

 

创建累加器：sc.longAccumulator

add，value

也可以自定义累加器

 

十七.广播变量

目的是提高效率，减少传输，传送大变量

创建：broadcast

 

# 第三章 SparkSql



## 一.概述

Spark SQL是Spark用来处理结构化数据的一个模块，它提供了2个编程抽象：DataFrame和DataSet，并且作为分布式SQL查询引擎的作用

Spark SQL是将SQL转换成RDD，然后提交到集群执行，执行效率非常快

HDFS：数据没有结构存储；数仓：数据结构化（加标签）分门别类存储

Spark SQL通过DataFrame和DataSet解决了RDD里的数据没有结构的问题

 

## 二.DataFrame于DataSet介绍

RDD是计算的抽象，DataFrame更近一步，是结构（Schema）的抽象，不过只包含数据表的字段名及给字段类型没有类ORM映射（只能在SQL语句里用，不能在后续程序取结果集的时候用） 。DataSet添加了类型静态编译检查，最新版本的DataFrame是Dataset[Row]的特例不能直接获取列的属性名，即ORM（把数据库的东西放进类里映射到相应属性），但区别是DataSet是以类来指定结构的，DataFram是一个个基本类型来指定结构的，即Row。

DataFrame没有类，那么获得的Row类型对象，只能用类似SQL获取结果集的方式->getInt(0)这样的方式取列的值，而不是直接用属性名（或者用case匹配）

==RDD，增加结构信息->DataFrame，增加类->DataSet==

 DataFrame只是知道字段，但是不知道字段的类型,而DataSet不仅仅知道字段，而且知道字段类型

三.DataFrame操作

SparkSession是Spark最新的SQL查询起始点

命令行里SparkSession的对象叫做spark

创建DataFrame方式：

1.通过Spark的数据源进行创建

```
spark.read.
csv   format   jdbc   json   load   option   options   orc   parquet   schema   table   text   textFile
```

2.从一个存在的RDD进行转换

3.还可以从Hive Table进行查询返回

 

df.show:打印数据集结果

 

语法风格：一种是传统SQL语句，一种是DSL以函数风格调用

 

SQL风格：

视图：只能查不能改

表：能查能改

 

createTempView：创建临时视图

createGlobalView：创建全局视图，访问的时候加全局限定名，修饰视图名

 

spark.sql：传入SQL语句

 

show：显示

 

DSL风格：

printSchema：打印表结构

select函数的参数即是列名，也是要计算的函数体语句

$"name"：传参的时候，不会与后面的数据进行计算来得到列名。遍历每一行的时候，引用name列的值

好处一是编写简单，一个函数可以完成很多SQL语句，二是有类型检查

 

RDD与DataFrame转换

如果需要RDD与DF或者DS之间操作，那么都需要引入 import spark.implicits._ **【spark不是包名，而是sparkSession对象的名称**】

手动转换：**toDF**("name","age"），转为DataFrame并制定Schema

 

反射转换：

（1）创建一个样例类

scala> case class People(name:String, age:Int)

（2）根据样例类将RDD转换为DataFrame

scala> peopleRDD.map{ x => val para = x.split(",");People(para(0),para(1).trim.toInt)}.toDF

res2: org.apache.spark.sql.DataFrame = [name: string, age: int]

就是RDD的范型要是一个类，toDF可以反射

 

编程方式：

（1）导入所需的类型

scala> import org.apache.spark.sql.types._

import org.apache.spark.sql.types._

（2）创建Schema

scala> val structType: StructType = StructType(StructField("name", StringType) :: StructField("age", IntegerType) :: Nil)

structType: org.apache.spark.sql.types.StructType = StructType(StructField(name,StringType,true), StructField(age,IntegerType,true))

（3）导入所需的类型

scala> import org.apache.spark.sql.Row

import org.apache.spark.sql.Row

（4）根据给定的类型创建二元组RDD

scala> val data = peopleRDD.map{ x => val para = x.split(",");Row(para(0),para(1).trim.toInt)}

data: org.apache.spark.rdd.RDD[org.apache.spark.sql.Row] = MapPartitionsRDD[6] at map at <console>:33

（5）根据数据及给定的schema创建DataFrame

scala> val dataFrame = spark.createDataFrame(data, structType)

dataFrame: org.apache.spark.sql.DataFrame = [name: string, age: int]

手动构建DataFrame需要的源RDD数据和数据结构

 

DataFrame转为为RDD

DataFrame的rdd方法，RDD里的泛型为Row

 

四.DataSet操作

Dataset是具有强类型的数据集合，需要提供对应的类型信息

创建：

1）创建一个样例类

scala> case class Person(name: String, age: Long)

defined class Person

2）创建DataSet

scala> val caseClassDS = Seq(Person("Andy", 32)).toDS()

caseClassDS: org.apache.spark.sql.Dataset[Person] = [name: string, age: bigint]

 

RDD转换为DataSet

1）创建一个RDD

scala> val peopleRDD = sc.textFile("examples/src/main/resources/people.txt")

peopleRDD: org.apache.spark.rdd.RDD[String] = examples/src/main/resources/people.txt MapPartitionsRDD[3] at textFile at <console>:27

2）创建一个样例类

scala> case class Person(name: String, age: Long)

defined class Person

3）将RDD转化为DataSet

scala> peopleRDD.map(line => {val para = line.split(",");Person(para(0),para(1).trim.toInt)}).**toDS**()

 

DataSet转为RDD：

调用rdd方法即可

 

DataFrame与DataSet互相操作：

1.DataFrame转为DataSet

思路是提供类信息

（1）导入隐式转换

import spark.implicits._

（2）创建样例类

case class Coltest(col1:String,col2:Int)extends Serializable //定义字段名和类型

（3）转换

val testDS = testDF.as[Coltest]

 

2.DataSet转为DataFrame

这个很简单，因为只是把case class封装成Row

（1）导入隐式转换

**import spark.implicits._**

（2）转换

val testDF = testDS.toDF

 

RDD，DataFrame，DataSet关系

RDD (Spark1.0) —> Dataframe(Spark1.3) —> Dataset(Spark1.6)

 

如果同样的数据都给到这三个数据结构，他们分别计算之后，都会给出相同的结果。不同是的他们的执行效率和执行方式。

 

在后期的Spark版本中，DataSet会逐步取代RDD和DataFrame成为唯一的API接口。

 

五.IDEA写Spark SQL

引入Spark SQL依赖



# 第四章 structure Streaming

### 一、为何要有StructuredStreaming

Spark Streaming处理的数据流图：

  ![img](https://img-blog.csdn.net/20180816204434276?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xvdmVjaGVuZG9uZ3hpbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

  Spark Streaming的处理机制如下图：

  ![img](https://img-blog.csdn.net/20180816204442401?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xvdmVjaGVuZG9uZ3hpbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

Structured Streaming是Spark2.0版本提出的新的实时流框架（2.0和2.1是实验版本，从Spark2.2开始为稳定版本），相比于Spark Streaming，优点如下：

同样能支持多种数据源的输入和输出，参考如上的数据流图
以结构化的方式操作流式数据，能够像使用Spark SQL处理离线的批处理一样，处理流数据，代码更简洁，写法更简单
基于Event-Time，相比于Spark Streaming的Processing-Time更精确，更符合业务场景
解决了Spark Streaming存在的代码升级，DAG图变化引起的任务失败，无法断点续传的问题（Spark Streaming的硬伤！！！）

## 二、StructuredStreaming的特性

### 1、结构化流式处理

​    a、如下图。Structured Streaming将实时流抽象成一张无边界的表，输入的每一条数据当成输入表的一个新行，同时将流式计算的结果映射为另外一张表，完全以结构化的方式去操作流式数据。

​    ![img](https://img-blog.csdn.net/20180816204451201?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xvdmVjaGVuZG9uZ3hpbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 

b、输入的流数据是以batch为单位被处理，每个batch会触发一次流式计算，计算结果被更新到Result Table。

​    如下图，设定batch长度为1s，每一秒从输入源读取数据到Input Table，然后触发Query计算，将结果写入Result Table.


​    ![img](https://img-blog.csdn.net/20180816204501227?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xvdmVjaGVuZG9uZ3hpbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

c、最后，Result Table的结果写出到外部存储介质（如Kafka）。

一共有三种Output模式：

- **Append模式：只有自上次触发后在Result Table表中附加的新行将被写入外部存储器。重点模式，一般使用它。**
- Complete模式： 将整个更新表写入到外部存储。每次batch触发计算，整张Result Table的结果都会被写出到外部存储介质。

- Update模式：只有自上次触发后在Result Table表中更新的行将被写入外部存储器。注意，这与完全模式不同，因为此模式不输出未更改的行。


### 2、基于Event-Time聚合&延迟数据处理

​    StructedStreaming相比于SparkStreaming的另一个优点是基于Event time（事件时间）聚合

a、**基于Event time聚合**，更准确

​    首先，介绍下两种时间的概念：

-   **Event time** 事件时间: 就是数据真正发生的时间，比如用户浏览了一个页面可能会产生一条用户的该时间点的浏览日志。        
-   **Process time** 处理时间: 则是这条日志数据真正到达计算框架中被处理的时间点，简单的说，就是你的Spark程序是什么时候读到这条日志的。

​    事件时间是嵌入在数据本身中的时间。对于许多应用程序，用户可能希望在此事件时间操作。例如，如果要获取IoT设备每分钟生成的事件数，则可能需要使用生成数据的时间（即数据中的事件时间），而不是Spark接收他们的时间。事件时间在此模型中非常自然地表示 - 来自设备的每个事件都是表中的一行，事件时间是该行中的一个列值。这允许基于窗口的聚合（例如每分钟的事件数）仅仅是时间列上的特殊类型的分组和聚合 - 每个时间窗口是一个组，并且每一行可以属于多个窗口/组。因此，可以在静态数据集（例如，来自收集的设备事件日志）以及数据流上一致地定义这种基于事件时间窗的聚合查询，操作起来更方便。

**b、延迟数据处理Watermark**

**Structured Streaming基于Event time能自然地处理延迟到达的数据，保留或者丢弃。**

由于Spark正在更新Result Table，因此当存在延迟数据时，它可以完全控制更新旧聚合，以及清除旧聚合以限制中间状态数据的大小。

使用Watermark，允许用户指定数据的延期阈值，并允许引擎相应地清除旧的状态。

 基于Event-Time聚合&延迟数据处理的详细实例参考 《StructuredStreaming + Kafka程序怎么写》

### 3、容错性

​    **a、容错语义**

流式数据处理系统的可靠性语义通常是通过系统可以处理每个记录的次数来定义的。系统可以在所有可能的操作情形下提供三种类型的保证（无论出现何种故障）：

- **At most once**：每个记录将被处理一次或不处理。
- **At least once**:  每个记录将被处理一次或多次。 这比“最多一次”更强，因为它确保不会丢失任何数据。但可能有重复处理。
- **Exactly once**：每个记录将被精确处理一次 - 不会丢失数据，并且不会多次处理数据。 这显然是三者中最强的保证。

**Structured Streaming能保证At least once的语义，目标是Exactly once。**

**b、容错机制**

​    在故障或故意关闭的情况下，用户可以恢复先前进度和状态，并继续在其停止的地方，简称**断点续传**。这是通过使用检查点checkpoint和预写日志write ahead logs来完成的。
​    用户可以指定checkpoint的位置，Spark将保存所有进度信息（如每个触发器中处理的offset偏移范围）和正在运行的聚合到checkpoint中。任务重启后，使用这些信息继续执行。

​    注：checkpoint的目录地址必须是HDFS兼容文件系统中的路径。

![image-20200616124843090](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200616124843090.png)

