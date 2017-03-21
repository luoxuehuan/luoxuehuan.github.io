---
layout: post
title:  kafka和Zookeeper操作指南
date:   2017-03-21 21:44:07 +08:00
category: 分布式系统
tags: zookeeper kafka
comments: true
---
{:toc}

## 集群配置：

`--zookeeper master106:2181,node110:2181,node210:2181`

`--kafka node81:9092,node110:9092`


## 二、Kafka操作：

### 0.启动Kafka

`kafka-server-start.sh ../config/server.properties &`

### 1.查看topic
`./kafka-topics.sh --list --zookeeper master106:2181,node110:2181,node210:2181`


### 2.查看某个topic
`./kafka-topics.sh --list --zookeeper master106:2181,node110:2181,node210:2181 --topic online_model_topic_132_4`

spark-shell --jars biz_bidding-1.0-SNAPSHOT-jar-with-dependencies.jar

### 3.创建topic：

创建名为test的topic， 8个分区分别存放数据，数据备份总共2份

`./kafka-topics.sh --create --zookeeper master106:2181,node110:2181,node210:2181 --replication-factor 2 --partitions 1 --topic kafka_topic_0318`

### 4.生产消息：

`./kafka-console-producer.sh --broker-list node81:9092,node110:9092 --topic test`


`./kafka-console-producer.sh --broker-list node81:9092,node110:9092 --topic kafka_topic_0318`

### 5.消费消息：
`./kafka-console-consumer.sh --zookeeper master106:2181,node110:2181,node210:2181 --from-beginning --topic kafka_topic_0318`




### 6.重新分配分区kafka-reassign-partitions.sh

`bin/kafka-reassign-partitions.sh --topics-to-move-json-file topics-to-move.json --broker-list "171"--zookeeper master106:2181,node110:2181,node210:2181 --execute` 


### 7.为Topic增加 partition数目kafka-add-partitions.sh

`bin/kafka-add-partitions.sh --topic test --partition 2  --zookeeper master106:2181,node110:2181,node210:2181` （为topic test增加2个分区）


> auto.create.topics.enable=true
default.replication.factor=2
num.partitions=3



========================

## zookeeper 命令：

### 1.启动：

`bin/zkServer.sh start`

> 启动后，jps查看，会多一个进程：5901 ZooKeeperMain

### 2.进入客户端：
`$ bin/zkCli.sh -server 127.0.0.1:2181`

### 3.在客户端内操作：
#### 帮助：
`help`

#### 新增：
`create /zk_test my_data`

`[zkshell: 8] ls /`

#### 创建一个node 内容为my_data
`[zkshell: 9] create /zk_test my_data`

#### 查看目录下文件：
`[zkshell: 11] ls /`

#### 修改一个node 为 

`set /zk_text junk`

#### 删除一个node 

`delete /zk_test`




Next, verify that the data was associated with the znode by running the get command, as in:


`[zkshell: 12] get /zk_test`




### 文件和节点的区别：

```
[zk: localhost:2181(CONNECTED) 16] get /alioth_dev/dipper/alioth/notify/frame/model_instance_id_1/ES/alioth_dev/frame_test
{"schema":"alioth_dev","storage_class":"ES","entity":"frame_test"}
cZxid = 0x5002948bf
ctime = Fri Jan 06 17:07:42 CST 2017
mZxid = 0x5002948bf
mtime = Fri Jan 06 17:07:42 CST 2017
pZxid = 0x5002948bf
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 66
numChildren = 0
[zk: localhost:2181(CONNECTED) 17] get /alioth_dev/dipper/alioth/notify/frame/model_instance_id_1/ES/alioth_dev

cZxid = 0x5002947ef
ctime = Fri Jan 06 15:56:23 CST 2017
mZxid = 0x5002947ef
mtime = Fri Jan 06 15:56:23 CST 2017
pZxid = 0x5002948bf
cversion = 13
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 0
numChildren = 1
[zk: localhost:2181(CONNECTED) 18]
```


```
[zk: 127.0.0.1:2181(CONNECTED) 0] ls /
[controller_epoch, text_create_zk_group_withdata, brokers, zookeeper, admin, isr_change_notification, consumers, config]
[zk: 127.0.0.1:2181(CONNECTED) 1] ls /brokers
[ids, topics, seqid]
[zk: 127.0.0.1:2181(CONNECTED) 3] ls /brokers/ids
[]
此时去启动Kafka：

kafka-server-start.sh ../config/server.properties &

再次查看ids：
[zk: 127.0.0.1:2181(CONNECTED) 4] ls /brokers/ids
[0]

可以看到多了一个 0 brokers.

[zk: 127.0.0.1:2181(CONNECTED) 5] ls /brokers/ids
[0]
[zk: 127.0.0.1:2181(CONNECTED) 6] ls /brokers/topics
[HelloKafka3, HelloKafka2, HelloKafka1, HelloKafka]

[zk: 127.0.0.1:2181(CONNECTED) 7] ls /brokers/seqid
[]

关闭Kafka集群，再次查看brokers：
[zk: 127.0.0.1:2181(CONNECTED) 8] ls /brokers/ids
[]
[zk: 127.0.0.1:2181(CONNECTED) 9]
```