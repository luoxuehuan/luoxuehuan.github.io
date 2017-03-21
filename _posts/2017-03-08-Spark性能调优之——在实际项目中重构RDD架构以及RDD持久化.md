

### 一、RDD架构重构与优化是什么。

尽量去复用RDD，差不多的RDD，可以抽取为一个共同的RDD，供后面的RDD计算时，反复使用。

### 二、怎么做？

#### 缓存级别：
```
	case "NONE" => NONE
    case "DISK_ONLY" => DISK_ONLY
    case "DISK_ONLY_2" => DISK_ONLY_2
    case "MEMORY_ONLY" => MEMORY_ONLY
    case "MEMORY_ONLY_2" => MEMORY_ONLY_2
    case "MEMORY_ONLY_SER" => MEMORY_ONLY_SER
    case "MEMORY_ONLY_SER_2" => MEMORY_ONLY_SER_2
    case "MEMORY_AND_DISK" => MEMORY_AND_DISK
    case "MEMORY_AND_DISK_2" => MEMORY_AND_DISK_2
    case "MEMORY_AND_DISK_SER" => MEMORY_AND_DISK_SER
    case "MEMORY_AND_DISK_SER_2" => MEMORY_AND_DISK_SER_2
    case "OFF_HEAP" => OFF_HEAP
```

####  使用示例：
```
sessionid2actionRDD = sessionid2actionRDD.persist(StorageLevel.MEMORY_ONLY());

/** 
cache就是一个特殊的默认在内存中的缓存。
Persist this RDD with the default storage level (`MEMORY_ONLY`). 
*/
def cache(): JavaPairRDD[K, V] = new JavaPairRDD[K, V](rdd.cache())
```


### 三、为什么需要重构优化RDD？

![这里写图片描述](http://img.blog.csdn.net/20161210150917120?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHhoYW5kbGJi/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

如图所示。如果rdd没有缓存。 
在计算RDD3的时候，会从hdfs读取一份，到RDD1到RDD2 到RDD3 需要15分钟。
再需要计算RDD4的时候，会重新从HDFS中读取，计算， 又需要耗时15分钟。 那么总共就需要30分钟。


如果把RDD1 缓存在内存或磁盘中。
那么 要计算的时候，直接从内存或磁盘中读取RDD1 即可，不需要再次读取HDFS，以及重新计算RDD1. 这样 总时间 就只需要20分钟。 大大提升了效率。

### 四、公共RDD一定要实现持久化。

对于多次计算和公共的RDD，一定要进行持久化。
持久化，也就是说，将RDD的数据缓存到内存中、磁盘中，BlockManager。
以后无论对这个RDD做多少次计算，那么都直接取这个RDD的持久化的数据，比如从内存中，或者磁盘中，直接提取一份数据。

### 五、持久化的时候是可以进行序列化的。

如果正常将数据持久化在内存中，那么可能会导致内存占用过大，这样的话，也许会导致OOM内存溢出。

当纯内存无法支撑公共RDD数据完全存放的时候，就优先考虑，使用序列化的方式，在纯内存中存储。
将RDD的每个partion的数据，序列化成一个大的字节数组，就一个对象；
序列化后，大大减少内存的空间占用。

> 序列化的方式，唯一的缺点，就是，获取数据的时候，需要反序列化。

如果序列化纯内存的方式，还是导致OOM，内存溢出。
就只能考虑磁盘的方式，内存+磁盘，普通方式（持久化）
内存+磁盘 ，序列化。

###  六、为了数据的高可靠，而且内存充足，可以使用双副本机制，进行持久化。

持久化双副本，持久化后的一个副本，因为机器宕机了，副本丢了，就还是得重新计算一次；

持久化的每个数据单元，存储一份副本，放在其他节点上，从而进行容错。一个副本丢了，可以使用另外一个。

这种方式，仅仅针对内存资源极度充足。！