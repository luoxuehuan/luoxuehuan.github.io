---
layout: post
title: spark调优-jvm调优原理以及降低cache操作的内存占比
date: 2015-04-02
categories: blog
tags: [spark调优]
description: spark调优
---

###  一、性能调优分类：


#### 1.常规性能调优： 分配资源，并行度。。等。

#### 2.JVM调优：JVM相关的参数。

通常情况下，如果你的硬件配置，基础的JVM的配置，都ok的话，JVM通常不会造成太严重的性能问题，反而更多的是，
在troubleshooting中，JVM占了很重要的位置！！

JVM造成 线上的spark作业运行报错，甚至失败（比如OOM）
 
#### 3.shuffle 调优: spark 在执行groupByKey，reduceByKey等操作时，shuffle环节的调优，这个很重要。

shuffle 调优，其实对spark作业的性能影响，是相当之高！！！

经验： 在spark作业的运行过程中。

只要一牵扯到shuffle的操作，基本上shuffle操作的性能消耗，要占到spark作业的50%~90%。

10%用来运行map等操作。
90% 耗费在两个shuffle操作。groupByKey、CountByKey。

#### 4. spark操作调优 （Spark算子调优） ： groupByKey ，countByKey 或aggregateByKey来重构实现。

有些算子的性能，是比其他一些算子的性能要高的。

例如：foreachPartion 替代 foreach。


### 二、调优层次：

- 1.分配资源并行度，RDD架构与缓存。

- 2.shuffle调优

- 3.spark算子调优

- 4.JVM，广播大变量



### 三、JVM原理概述

降低cache操作的内存占比

#### 1.理论基础：
spark是用scala开发的。spark的scala代码调用了很多java api。
scala也是运行在java虚拟机中的。spark是运行在java虚拟机中的。

java虚拟机可能会产生什么样的问题：内存不足？？！！

我们的RDD的缓存、task运行定义的算子函数，可能会创建很多对象。
都可能会占用大量内存，没搞好的话，可能导致JVM出问题。

当一个eden，survivor 区域放满之后，触发minor gc 小型垃圾回收。

eden：survivor：survivor = 8:1:1 

如果一次gc后，存活下来的对象需要占用1.5 个survivor。
那么一个survivor 放不下。
就会根据**担保机制**，把多余的对象，直接放入老年代了。

如果你的JVM内存不够大，可能导致年轻代频繁地内存满溢，频繁进行minor gc。

	频繁的minor gc会导致 短时间内，有些存活的对象，多次垃圾回收都没有回收掉。会导致这种短生命周期（其实不一定是长期使用的），垃圾回收次数太多，还没没被回收，跑到 老年代中去（担保机制）


而，理想情况下，老年代放生命周期长的，比如数据库连接池！！

导致：老年代中，可能会因为内存不足，囤积着一大堆，短生命周期的，本来应该在年轻代中的，可能马上就要回收掉的对象。

此时可能导致老年代频繁满溢，频繁进行full gc （全局/ 全面） 。
而full gc很慢。（ stop the word ，简而言之，gc的时候，spark 停止工作了。 等着垃圾回收结束。）


full gc 这个算法的设计，是针对 ，老年代中的对象数量很少，满溢进行full gc的频率应该很少，因此采取了不太复杂，但是消耗性能和时间的垃圾回收算法。

full gc/minor gc 无论是快还是慢， 都会导致jvm的 工作线程停止工作。

#### 2.内存不充足的时候，导致：
- 1.频繁minor gc 也会导致spark停止工作。

- 2.老年代 囤积大量活跃对象（短生命周期的对象。） 导致full gc 。full gc 短则数十秒，长则几分钟，甚至几分钟。
导致spark 长时间停止工作。

- 3.严重影响spark的性能和运行的速度。


### 四、如何调优

> 1.Spark中为何需要降低cache操作的内存占比。

spark中，堆内存又被划分成两块（一块专门用来给RDD的cache ，persist操作进行rdd数据缓存用）
另外一块 就是给spark算子，运行使用，存放自己创建的对象。


默认情况下，给rdd cache 操作的内存占比 是 0.6  60% 都给 cache操作了。

但是有些情况下cache不是那么紧张，导致 算子操作的对象，内存不足，因此导致full gc。

> 2.观察方法：

针对上述情况，大家可以 在 spark webui观察。
 yarn 去运行的话，那么就通过yarn的界面，去查看你的spark作业的 运行统计。
很简单， 大家一层一层点击进去就好。

可以看到每个stage 的运行情况。 包括每个task的运行时间统计。gc时间。

如果发现gc太贫乏。时间太长，那么就可以适当调整这个比例。


降低cache操作的内存占比，大不了用persist 操作。选择一部分缓存的rdd数据写入磁盘，或者序列化的的方式。配合kryo序列化类。
减少rdd缓存的内存占用，降低cache操作内存占比。

对应的 算子函数的内存占比，就提升了。
这个时候，可能就减少minor gc 的频率，同时减少full gc的频率，对性能的 提升 有一定帮助的。






> 简而言之就是，让task 执行算子的时候，有更多的内存！！！！


###五、调参

	spark.storage.memoryFraction , 0.6->0.5->0.4->0.3(视实际情况)














 







































