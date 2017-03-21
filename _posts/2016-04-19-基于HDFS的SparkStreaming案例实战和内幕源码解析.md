---
layout: post
title: 基于HDFS的SparkStreaming案例实战和内幕解析
date: 2016-04-19
categories: blog
tags: [Spark]
description: Spark实战
---


概要
------
	本节主要讲解在开发环境中编写SparkStreaming代码监控hdfs目录，实现实时wordCount计算。
	先通过Java方式演示过程，并在文末提供Scala版本代码。

## 一、环境准备
------

### 1.启动Hadoop集群

```
cd /usr/local/hadoop/hadoop-2.6.0/sbin/
./start-dfs.sh	//通过http://master:50070（50070为默认端口）查看datanode 的信息
./start-yarn.sh //启动Hadoop的资源管理框架Yarn
```

### 2.启动Spark集群

```
cd /usr/local/spark/spark-1.6.0-bin-hadoop2.6/sbin/
./start-all.sh				//打开浏览器访问http://master:8080查看spark控制台
./start-history-server.sh 	//运维,启动日志来记录spark集群运行的每一步信息
```
## 二、代码实战
------

### 1.定义成员变量

```
final String checkpointDirectory = "hdfs://Master:9000/library/SparkStreaming/CheckPoint_Data";//checkpoint存放数据的文件夹
final String dataDirectory = "hdfs://Master:9000/library/SparkStreaming/Data";//SparkStreaming监控的文件夹
```

### 2.配置SparkConf

```
final SparkConf conf = new SparkConf().setMaster("spark://Master:7077").
				setAppName("SparkStreamingOnHDFS");//设置Master端口和App名称。
```

### 3、创建SparkStreamingContext


#### 3.1 定义一个创建StreamingContext的方法

```
private static JavaStreamingContext createContext(
            String checkpointDirectory,SparkConf conf) {
		System.out.println("Creating new context");
		SparkConf sparkConf = conf;
		// Create the context with a 15 second batch size
		JavaStreamingContext ssc = new JavaStreamingContext(sparkConf, Durations.seconds(15));
		ssc.checkpoint(checkpointDirectory);
		return ssc;
	}
```

#### 3.2 定义工厂类创建Context

```
JavaStreamingContextFactory factory = new JavaStreamingContextFactory() {
		      @Override
		      public JavaStreamingContext create() {
		        return createContext(checkpointDirectory, conf);
		      }
		 };
```

#### 3.3 使用StreamingContext的getOrCreate创建Context

```
//传入参数为checkpoint目录和工厂
JavaStreamingContext jsc = JavaStreamingContext.getOrCreate(checkpointDirectory, factory);
```

### 4 编写业务代码

#### 4.1 创建Spark Streaming输入数据来源input Stream：
```
JavaDStream lines = jsc.textFileStream(dataDirectory);
```

#### 4.2 接下来就像对于RDD编程一样基于DStream进行编程

```
//4.2.1 读取数据并对每一行中的数据以空格作为split参数切分成单词以获得DStream<String>
JavaDStream<String> words = lines.flatMap(new FlatMapFunction<String, String>() { 
			@Override
			public Iterable<String> call(String line) throws Exception {
				return Arrays.asList(line.split(" "));
			}
		});
//4.2.2 使用mapToPair创建PairDStream
JavaPairDStream<String, Integer> pairs = words.mapToPair(new PairFunction<String, String, Integer>() {

			@Override
			public Tuple2<String, Integer> call(String word) throws Exception {
				return new Tuple2<String, Integer>(word, 1);
			}
		});
//4.2.3 使用reduceByKey进行累计操作
JavaPairDStream<String, Integer> wordsCount = pairs.reduceByKey(new Function2<Integer, Integer, Integer>() { //对相同的Key，进行Value的累计（包括Local和Reducer级别同时Reduce）
			@Override
			public Integer call(Integer v1, Integer v2) throws Exception {
				return v1 + v2;
			}
		});

wordsCount.print();
		/*
		* Spark Streaming执行引擎也就是Driver开始运行，Driver启动的时候是位于一条新的线程中的，当然其内部有消息循环体，用于
		 * 接受应用程序本身或者Executor中的消息；
		 */
		jsc.start();
		jsc.awaitTermination();
		jsc.close();

```

## 三、打包发布
------
![这里写图片描述](http://img.blog.csdn.net/20160419170815986)
由于项目是maven构建的，右键Spark程序的类，选择Run as选择Run Configurations。Goals设置为clean package。点击Run。构建成功后，可以在target文件夹找到名为SparkApps-0.0.1-SNAPSHOT-jar-with-dependencies.jar的文件。将此jar放置在Master机器的一个文件夹下，例如/root/Documents/SparkApps。
可能会出现的两个问题：

	1.maven-compiler-plugin 插件版本信息错误。

> 解决办法，增加一行版本信息。


```
<plugins>  
    <plugin>  
        <artifactId>maven-compiler-plugin</artifactId>  
        <version>2.3.2</version>  //增加的版本信息。
        <configuration>  
            <source>1.6</source>  
            <target>1.6</target>  
            <encoding>UTF-8</encoding>  
        </configuration>  
    </plugin>  
</plugins>
```

	2.Maven Unable to locate the Javac Compiler

> 解决办法：编辑jdk，把jre home的值修改成jdk home。
	
![这里写图片描述](http://img.blog.csdn.net/20160419171528991)


## 四、案例演示
------

### 4.1 在Hadoop上建立数据目录

	注：创建多级目录要在-mkdir后加上-p

```
hadoop dfs -mkdir -p /library/SparkStreaming/Data
hadoop dfs –mkdir /library/SparkStreaming/CheckPoint_Data
```
		
### 4.2 提交程序

```
//4.2.1 进入spark的bin目录
cd /usr/local/spark/spark-1.6.0-bin-hadoop2.6/bin/

//4.2.2 spark-submit
spark-submit --class com.dt.spark.MySparkApps.Streaming.SparkStreamingOnHDFS --master spark://Master:7077 /root/Documents/SparkApps/SparkApps-0.0.1-SNAPSHOT-jar-with-dependencies.jar

```

### 4.3 往HDFS里传文件

	hadoop dfs -put ./14.txt /library/SparkStreaming/Data
	
### 4.4 运行结果：
	上传了第一个文件
![这里写图片描述](http://img.blog.csdn.net/20160419165703076)

	运行结果
	
![这里写图片描述](http://img.blog.csdn.net/20160419165603685)
	
	继续上传文件到HDFS

![这里写图片描述](http://img.blog.csdn.net/20160419165815099)	

	运行结果：	
	
![这里写图片描述](http://img.blog.csdn.net/20160419165628971)

## 五、源码解析
------

### 5.1 JavaStreamingContextFactory

JavaStreamingContextFactory的create方法可以创建JavaStreamingContext，而我们在具体实现的时候覆写了该方法，内部就是调用createContext方法来具体实现。

```
/**
 * Factory interface for creating a new JavaStreamingContext
 */
trait JavaStreamingContextFactory {
  def create(): JavaStreamingContext
}

```

### 5.2 checkpoint方法

一方面：保持容错
一方面保持状态
在开始和结束的时候每个batch都会进行checkpoint

```
/**
 * Sets the context to periodically checkpoint the DStream operations for master
 * fault-tolerance. The graph will be checkpointed every batch interval.
 * @param directory HDFS-compatible directory where the checkpoint data will be reliably stored
 */
def checkpoint(directory: String) {
  ssc.checkpoint(directory)
}

```

### 5.3 remember方法

流式处理中过一段时间数据就会被清理掉，但是可以通过remember可以延长数据在程序中的生命周期，另外延长RDD更长的时间。

应用场景：
假设数据流进来，进行ML或者Graphx的时候有时需要很长时间，但是bacth定时定条件的清除RDD，所以就可以通过remember使得数据可以延长更长时间。

```
/**
 * Sets each DStreams in this context to remember RDDs it generated in the last given duration.
 * DStreams remember RDDs only for a limited duration of duration and releases them for garbage
 * collection. This method allows the developer to specify how long to remember the RDDs (
 * if the developer wishes to query old data outside the DStream computation).
 * @param duration Minimum duration that each DStream should remember its RDDs
 */
 
def remember(duration: Duration) {
  ssc.remember(duration)
}
```

### 5.4 getOrCreate方法

	如果设置了checkpoint ，重启程序的时候，getOrCreate()会重新从checkpoint目录中初始化出StreamingContext。

```
/**
 * Either recreate a StreamingContext from checkpoint data or create a new StreamingContext.
 * If checkpoint data exists in the provided `checkpointPath`, then StreamingContext will be
 * recreated from the checkpoint data. If the data does not exist, then the provided factory
 * will be used to create a JavaStreamingContext.
 *
 * @param checkpointPath Checkpoint directory used in an earlier JavaStreamingContext program
 * @param factory        JavaStreamingContextFactory object to create a new JavaStreamingContext
 * @deprecated As of 1.4.0, replaced by `getOrCreate` without JavaStreamingContextFactor.
 */
@deprecated("use getOrCreate without JavaStreamingContextFactor", "1.4.0")
def getOrCreate(
    checkpointPath: String,
    factory: JavaStreamingContextFactory
  ): JavaStreamingContext = {
  val ssc = StreamingContext.getOrCreate(checkpointPath, () => {
    factory.create.ssc
  })
  new JavaStreamingContext(ssc)
}

```

六、问题总结反思
------
**错误1**： 在checkpoint 过的基础上 启动程序，因为我们没有配置完整，会从checkpoint目录读取应用信息，无法初始化ShuffleDStream导致出错。
集群部署的情况下，第一次执行不会出错。

	出错原因：
	
1.	Streaming会定期的进行checkpoint。
2.	重新启动程序的时候，他会从曾经checkpoint的目录中，如果没有做额外配置的时候，所有的信息都会放在checkpoint的目录中(包括曾经应用程序信息)，因此下次再次启动的时候就会报错，无法初始化ShuffleDStream。


![这里写图片描述](http://img.blog.csdn.net/20160419172453010)

附录：
---
Scala版本代码：



