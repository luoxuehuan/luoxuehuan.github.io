# 1、安装和配置Zookeeper

## 1)	下载Zookeeper

进入http://www.apache.org/dyn/closer.cgi/zookeeper/，你可以选择其他镜像网址去下载，用官网推荐的镜像：http://mirror.bit.edu.cn/apache/zookeeper/

下载zookeeper-3.4.6.tar.gz

## 2)	安装Zookeeper

提示：下面的步骤发生在Master服务器。

以ubuntu14.04举例，把下载好的文件放到/root目录，用下面的命令解压：

```
cd /root
tar -zxvf zookeeper-3.4.6.tar.gz
```

解压后在/root目录会多出一个zookeeper-3.4.6的新目录，用下面的命令把它剪切到指定目录即安装好Zookeeper了：

```
cd /root
mv zookeeper-3.4.6 /usr/local/spark
```

之后在/usr/local/spark目录会多出一个zookeeper-3.4.6的新目录。下面我们讲如何配置安装好的Zookeeper。

## 3)	配置Zookeeper

提示：下面的步骤发生在master服务器。

###a.	配置.bashrc

-	打开文件：`vi /root/.bashrc`
-	在PATH配置行前添加：

```
export ZOOKEEPER_HOME=/usr/local/spark/zookeeper-3.4.6
```

-	最后修改PATH：

```
export PATH=${JAVA_HOME}/bin:${ZOOKEEPER_HOME}/bin:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin:${SCALA_HOME}/bin:${SPARK_HOME}/bin:${SPARK_HOME}/sbin:${HIVE_HOME}/bin:${KAFKA_HOME}/bin:$PATH
```

-	使配置的环境变量立即生效：source /root/.bashrc

### b.	创建data目录

```
	cd $ZOOKEEPER_HOME
```

```
	mkdir data
```

### c.	创建并打开zoo.cfg文件

```
	cd $ZOOKEEPER_HOME/conf
	cp zoo_sample.cfg zoo.cfg
	vi zoo.cfg
```

### d.	配置zoo.cfg

```
# 配置Zookeeper的日志和服务器身份证号等数据存放的目录。
# 千万不要用默认的/tmp/zookeeper目录，因为/tmp目录的数据容易被意外删除。
dataDir=../data
# Zookeeper与客户端连接的端口
clientPort=2181
# 在文件最后新增3行配置每个服务器的2个重要端口：Leader端口和选举端口
# server.A=B：C：D：其中 A 是一个数字，表示这个是第几号服务器；
# B 是这个服务器的hostname或ip地址；
# C 表示的是这个服务器与集群中的 Leader 服务器交换信息的端口；
# D 表示的是万一集群中的 Leader 服务器挂了，需要一个端口来重新进行选举，
# 选出一个新的 Leader，而这个端口就是用来执行选举时服务器相互通信的端口。
# 如果是伪集群的配置方式，由于 B 都是一样，所以不同的 Zookeeper 实例通信
# 端口号不能一样，所以要给它们分配不同的端口号。
server.1=Master:2888:3888
server.2=Worker1:2888:3888
#server.3=Worker2:2888:3888
```

### e.	创建并打开myid文件

```
-	cd $ZOOKEEPER_HOME/data
-	touch myid
-	vi myid
```

### f.	配置myid

按照zoo.cfg的配置，myid的内容就是1。

##4)	同步master的安装和配置到slave1和slave2
-	在master服务器上运行下面的命令

```
cd /root
scp ./.bashrc root@slave1:/root
scp ./.bashrc root@slave2:/root
cd /usr/local/spark
scp -r ./zookeeper-3.4.6 root@slave1:/usr/local/spark
scp -r ./zookeeper-3.4.6 root@slave2:/usr/local/spark
```

-	在slave1服务器上运行下面的命令

```
vim $ZOOKEEPER_HOME/data/myid
```

按照zoo.cfg的配置，myid的内容就是2。
-	在slave2服务器上运行下面的命令

```
vim $ZOOKEEPER_HOME/data/myid
```

按照zoo.cfg的配置，myid的内容就是3。
5)	启动Zookeeper服务
-	在Master服务器上运行下面的命令

```
zkServer.sh start
```

-	在slave1服务器上运行下面的命令

```
source /root/.bashrc
```

zkServer.sh start
-	在slave1服务器上运行下面的命令

```
source /root/.bashrc
zkServer.sh start
```

6)	验证Zookeeper是否安装和启动成功

-	在master服务器上运行命令：jps和zkServer.sh status

```
root@Master:/usr/local/spark/zookeeper-3.4.6/bin# jps
3844 QuorumPeerMain
4790 Jps
zkServer.sh status
root@Master:/usr/local/spark/zookeeper-3.4.6/bin# zkServer.sh status
JMX enabled by default
Using config: /usr/local/spark/zookeeper-3.4.6/bin/../conf/zoo.cfg
Mode: follower
```


-	在Worker1服务器上运行命令：jps和zkServer.sh status

```
root@Worker1:/usr/local/spark/zookeeper-3.4.6/bin# jps
4073 Jps
3277 QuorumPeerMain
root@Worker1:/usr/local/spark/zookeeper-3.4.6/bin# zkServer.sh status
JMX enabled by default
Using config: /usr/local/spark/zookeeper-3.4.6/bin/../conf/zoo.cfg
Mode: leader //可以看到这个服务器被选举为leader
```

	至此，代表Zookeeper已经安装和配置成功。


# 2、安装和配置Kafka

## 1)	下载Kafka

进入http://kafka.apache.org/downloads.html，左键单击kafka_2.10-0.9.0.1.tgz。

## 2)	安装Kafka
提示：下面的步骤发生在master服务器。
以ubuntu14.04举例，把下载好的文件放到/root目录，用下面的命令解压：

```
cd /root
tar -zxvf kafka_2.10-0.9.0.1.tgz
```

解压后在/root目录会多出一个kafka_2.10-0.9.0.1的新目录，用下面的命令把它剪切到指定目录即安装好Kafka了：

```
cd /root
mv kafka_2.10-0.9.0.1 /usr/local
```

之后在/usr/local目录会多出一个kafka_2.10-0.9.0.1的新目录。下面我们讲如何配置安装好的Kafka。


把jar包slf4j-nop-1.7.6.jar拷入到Kafka的libs里面，可以让Kafka后台运行！命令后面的&生效 。否则不生效。

## 3)	配置Kafka

提示：下面的步骤发生在master服务器。

### a.	配置.bashrc
-	打开文件：vi /root/.bashrc
-	在PATH配置行前添加：

```
export KAFKA_HOME=/usr/local/kafka_2.10-0.9.0.1
```

-	最后修改PATH：

```
export PATH=${JAVA_HOME}/bin:${ZOOKEEPER_HOME}/bin:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin:${SCALA_HOME}/bin:${SPARK_HOME}/bin:${SPARK_HOME}/sbin:${HIVE_HOME}/bin:${KAFKA_HOME}/bin:$PATH
```

-	使配置的环境变量立即生效：

```
source /root/.bashrc
```

###b.	打开server.properties

```
cd $ZOOKEEPER_HOME/config
vi server.properties
```

###c.	配置server.properties

```
broker.id=0
port=9092
zookeeper.connect=master:2181,slave1:2181,slave2:2181
```

>注意：2181就是和Zookeeper通信的端口。 因为在Zookeeper里面 有个配置为：clientPort = 2181
再备注： Zookeeper中的配置 server.0 = Master:2888:3888 其中一个端口用于通信，另一个端口用于选举！

## 4)	同步master的安装和配置到slave1和slave2
-	在master服务器上运行下面的命令
cd /root
scp ./.bashrc root@slave1:/root
scp ./.bashrc root@slave2:/root
cd /usr/local
scp -r ./kafka_2.10-0.9.0.1 root@slave1:/usr/local
scp -r ./kafka_2.10-0.9.0.1 root@slave2:/usr/local
-	在slave1服务器上运行下面的命令
vi $KAFKA_HOME/config/server.properties
修改broker.id=1。
-	在slave2服务器上运行下面的命令
vi $KAFKA_HOME/config/server.properties
修改broker.id=2。
## 5)	启动Kafka服务
-	在master服务器上运行下面的命令

```
cd $KAFKA_HOME/bin
kafka-server-start.sh ../config/server.properties &
```

-	在slave1服务器上运行下面的命令

```
source /root/.bashrc
cd $KAFKA_HOME/bin
kafka-server-start.sh ../config/server.properties &
```

-	在slave2服务器上运行下面的命令

```
source /root/.bashrc
cd $KAFKA_HOME/bin
kafka-server-start.sh ../config/server.properties &
```

6)	验证Kafka是否安装和启动成功（话题、生产者、消费者）
-	在任意服务器上运行命令创建Topic“HelloKafka”：
kafka-topics.sh --create --zookeeper master:2181,slave1:2181,slave2:2181 --replication-factor 3 --partitions 1 --topic HelloKafka

```
kafka-topics.sh --create --zookeeper Master:2181,Worker1:2181 --replication-factor 2 --partitions 1 --topic HelloKafka
```

注意：create 用的2181端口；

-	在任意服务器上运行命令为创建的Topic“HelloKafka”生产一些消息：
kafka-console-producer.sh --broker-list master:9092,slave1:9092,slave2:9092 --topic HelloKafka


```
kafka-console-producer.sh --broker-list Master:9092, Worker1:9092 --topic HelloKafka
```

注意：producer 用的9092端口；发给Kafka，Kafka用的端口号是9092。

输入下面的消息内容：
This is DT_Spark!
I’m Rocky!
Life is short, you need Spark!
-	在任意服务器上运行命令从指定的Topic“HelloKafka”上消费（拉取）消息：
kafka-console-consumer.sh --zookeeper master:2181,slave1:2181,slave2:2181 --from-beginning --topic HelloKafka

>2181就是和Zookeeper通信的端口


```
kafka-console-consumer.sh --zookeeper Master:2181, Worker1:2181 --from-beginning --topic HelloKafka
```

注释：from-beginning 
过一会儿，你会看到打印的消息内容：
	This is DT_Spark!
	I’m Rocky!
	Life is short, you need Spark!
-	在任意服务器上运行命令查看所有的Topic名字：
kafka-topics.sh --list --zookeeper master:2181,slave1:2181,slave2:2181
-	在任意服务器上运行命令查看指定Topic的概况：
kafka-topics.sh --describe --zookeepermaster:2181,slave1:2181,slave2:2181 --topic HelloKafka
至此，代表Kafka已经安装和配置成功。

###升级测试： 

测试Kafka 的大吞吐量。可以用更大的数据！！
在某个目录用hadoop dfs –get /library/…./read.me 可以把hdfs上的文件下载到这个目录。
拷贝6万行数据进入，那样 zerocopy 基本没延迟！！！
flume接上Kafka ，直接把文件扔给flume，flume接上Kafka，消费者就能直接拿到数据。

备注:
 Zookeeper和大数据没关系，也可以用于JavaEE

传输数据以broker为单位。指定了Master:9092, Worker1:9092。



