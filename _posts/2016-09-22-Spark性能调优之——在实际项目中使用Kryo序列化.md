

### 一、Java的序列化机制

ObjectOutputStream/ObjectInputStream 对象输入输入流机制，来进行序列化。

这种默认序列化机制，的好处在于，处理方便，不需要手动做什么事，只要在算子里面使用的变量，实现Serializable接口的，可序列化即可。

但是缺点在于，默认的序列化机制的效率不高，序列化的速度比较慢，序列化以后的数据，占用的内存空间相对还是比较大。


可以手动序列化格式的优化。

Spark支持Kryo序列化机制。Kryo序列化机制，比默认的Java序列化机制，速度要快，序列化的数据要更小。
大概是Java的1/10.

所以减少传输数据，减少内存消耗。



###  二、Kryo序列化机制：

1.算子函数中使用到的外部变量。
2.持久化RDD时进行序列化，StorageLever.Memory_only_ser
3.shuffle


###  三、效果

- 1.算子函数中使用到的外部变量，使用Kryo以后： 优化网络传输的性能，可以优化集群中内存的占用。

- 2.持久化RDD，优化内存的占用和消耗； 持久化RDD占用的内存越少，task执行的时候，创建的对象，就不至于频繁的占满内存，频繁发生GC。

- 3.shuffle : 可以优化网络传输的性能。



### 四、怎么用？

#### 第一步： 在sparkConf中设置一个属性。

	set（“spark.serializer”,"org.apache.spark.serializer.KryoSerializer"）

Kryo之所以没有被作为默认序列化类库的原因：  因为Kryo要求，如果要达到它的最佳性能的话，那么就一定要
注册你自定义的类（比如，你的算子函数中使用到了外部自定义类型的对象变量，这时，就要求必须注册你的类，否则Kryo达不到最佳性能）


#### 第二步： 注册你使用到的，需要通过Kryo序列化，一些自定义的类。

	.registerKryoClasses(new Class[]{CategorySortKey.class});