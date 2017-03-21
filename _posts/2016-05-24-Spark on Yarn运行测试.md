## 1.启动hdfs：

>root@Master:/usr/local/hadoop/hadoop-2.6.0/sbin# ./start-dfs.sh 

###检查：


>root@Master:/usr/local/hadoop/hadoop-2.6.0/sbin# jps


	3026 NameNode
	3366 Jps
	3240 SecondaryNameNode
	
## 2.启动yarn：

```
root@Master:/usr/local/hadoop/hadoop-2.6.0/sbin# ./start-yarn.sh 
```

### 检查：


>root@Master:/usr/local/hadoop/hadoop-2.6.0/sbin# jps


	3026 NameNode
	3474 ResourceManager
	3732 Jps
	3240 SecondaryNameNode


## 3.在spark的bin目录提交作业：

运行：

>root@Master:/usr/local/spark/spark-1.6.0-bin-hadoop2.6/bin# ./spark-submit --class org.apache.spark.examples.SparkPi --master yarn-cluster ../lib/spark-examples-1.6.0-hadoop2.6.0.jar 10


结果：
```
16/05/24 15:17:53 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
16/05/24 15:17:54 INFO client.RMProxy: Connecting to ResourceManager at Master/10.137.12.15:8032
16/05/24 15:17:55 INFO yarn.Client: Requesting a new application from cluster with 3 NodeManagers
16/05/24 15:17:55 INFO yarn.Client: Verifying our application has not requested more than the maximum memory capability of the cluster (8192 MB per container)
16/05/24 15:17:55 INFO yarn.Client: Will allocate AM container, with 1408 MB memory including 384 MB overhead
16/05/24 15:17:55 INFO yarn.Client: Setting up container launch context for our AM
16/05/24 15:17:55 INFO yarn.Client: Setting up the launch environment for our AM container
16/05/24 15:17:55 INFO yarn.Client: Preparing resources for our AM container
16/05/24 15:17:57 INFO yarn.Client: Uploading resource file:/usr/local/spark/spark-1.6.0-bin-hadoop2.6/lib/spark-assembly-1.6.0-hadoop2.6.0.jar -> hdfs://Master:9000/user/root/.sparkStaging/application_1464072222929_0003/spark-assembly-1.6.0-hadoop2.6.0.jar
16/05/24 15:18:23 INFO yarn.Client: Uploading resource file:/usr/local/spark/spark-1.6.0-bin-hadoop2.6/lib/spark-examples-1.6.0-hadoop2.6.0.jar -> hdfs://Master:9000/user/root/.sparkStaging/application_1464072222929_0003/spark-examples-1.6.0-hadoop2.6.0.jar
16/05/24 15:18:42 INFO yarn.Client: Uploading resource file:/tmp/spark-801e775e-e08d-402e-be12-f48eb2c8b0a6/__spark_conf__2217083403663461580.zip -> hdfs://Master:9000/user/root/.sparkStaging/application_1464072222929_0003/__spark_conf__2217083403663461580.zip
16/05/24 15:18:43 INFO spark.SecurityManager: Changing view acls to: root
16/05/24 15:18:43 INFO spark.SecurityManager: Changing modify acls to: root
16/05/24 15:18:43 INFO spark.SecurityManager: SecurityManager: authentication disabled; ui acls disabled; users with view permissions: Set(root); users with modify permissions: Set(root)
16/05/24 15:18:44 INFO yarn.Client: Submitting application 3 to ResourceManager
16/05/24 15:18:45 INFO impl.YarnClientImpl: Submitted application application_1464072222929_0003
16/05/24 15:18:46 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:18:46 INFO yarn.Client: 
	 client token: N/A
	 diagnostics: N/A
	 ApplicationMaster host: N/A
	 ApplicationMaster RPC port: -1
	 queue: default
	 start time: 1464074325159
	 final status: UNDEFINED
	 tracking URL: http://Master:8088/proxy/application_1464072222929_0003/
	 user: root
16/05/24 15:18:48 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:18:49 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:18:50 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:18:51 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:18:52 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:18:53 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:18:54 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:18:55 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:18:56 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:18:57 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:18:59 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:00 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:01 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:02 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:03 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:04 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:05 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:06 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:07 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:08 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:09 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:10 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:11 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:12 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:13 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:14 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:15 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:17 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:18 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:19 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:20 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:21 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:22 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:23 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:24 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:25 INFO yarn.Client: Application report for application_1464072222929_0003 (state: ACCEPTED)
16/05/24 15:19:26 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:26 INFO yarn.Client: 
	 client token: N/A
	 diagnostics: N/A
	 ApplicationMaster host: 10.137.12.64
	 ApplicationMaster RPC port: 0
	 queue: default
	 start time: 1464074325159
	 final status: UNDEFINED
	 tracking URL: http://Master:8088/proxy/application_1464072222929_0003/
	 user: root
16/05/24 15:19:27 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:28 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:29 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:30 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:31 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:32 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:33 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:34 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:35 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:36 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:37 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:38 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:40 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:41 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:42 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:43 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:44 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:45 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:46 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:47 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:48 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:49 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:50 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:51 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:52 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:53 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:54 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:55 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:56 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:57 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:58 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:19:59 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:00 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:01 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:02 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:03 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:04 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:05 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:06 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:07 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:08 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:09 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:10 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:11 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:12 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:13 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:14 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:15 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:16 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:17 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:18 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:19 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:20 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:21 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:22 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:23 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:24 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:25 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:26 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:27 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:28 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:29 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:30 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:31 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:32 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:33 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:34 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:35 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:36 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:37 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:38 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:40 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:41 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:42 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:43 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:44 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:45 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:46 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:47 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:48 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:49 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:50 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:51 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:52 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:53 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:54 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:55 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:56 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:57 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:58 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:20:59 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:00 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:01 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:02 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:03 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:04 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:05 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:06 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:07 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:08 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:09 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:10 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:11 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:12 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:13 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:14 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:15 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:16 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:17 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:18 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:19 INFO yarn.Client: Application report for application_1464072222929_0003 (state: RUNNING)
16/05/24 15:21:20 INFO yarn.Client: Application report for application_1464072222929_0003 (state: FINISHED)
16/05/24 15:21:20 INFO yarn.Client: 
	 client token: N/A
	 diagnostics: N/A
	 ApplicationMaster host: 10.137.12.64
	 ApplicationMaster RPC port: 0
	 queue: default
	 start time: 1464074325159
	 final status: SUCCEEDED
	 tracking URL: http://Master:8088/proxy/application_1464072222929_0003/history/application_1464072222929_0003/1
	 user: root
16/05/24 15:21:20 INFO util.ShutdownHookManager: Shutdown hook called
16/05/24 15:21:20 INFO util.ShutdownHookManager: Deleting directory /tmp/spark-801e775e-e08d-402e-be12-f48eb2c8b0a6
```

### 运行状态：
![这里写图片描述](http://img.blog.csdn.net/20160524161613308)

### 详细信息：
![这里写图片描述](http://img.blog.csdn.net/20160524161624534)

### Pi的计算结果：
![这里写图片描述](http://img.blog.csdn.net/20160524161633503)


# 二、Client模式

###执行：
>root@Master:/usr/local/spark/spark-1.6.0-bin-hadoop2.6/bin# spark-submit --class org.apache.spark.examples.SparkPi --deploy-mode client ../lib/spark-examples-1.6.0-hadoop2.6.0.jar 10

###结果：
```
16/05/24 16:31:37 INFO spark.SparkContext: Running Spark version 1.6.0
16/05/24 16:31:38 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
16/05/24 16:31:38 INFO spark.SecurityManager: Changing view acls to: root
16/05/24 16:31:38 INFO spark.SecurityManager: Changing modify acls to: root
16/05/24 16:31:38 INFO spark.SecurityManager: SecurityManager: authentication disabled; ui acls disabled; users with view permissions: Set(root); users with modify permissions: Set(root)
16/05/24 16:31:39 INFO util.Utils: Successfully started service 'sparkDriver' on port 57232.
16/05/24 16:31:40 INFO slf4j.Slf4jLogger: Slf4jLogger started
16/05/24 16:31:40 INFO Remoting: Starting remoting
16/05/24 16:31:40 INFO Remoting: Remoting started; listening on addresses :[akka.tcp://sparkDriverActorSystem@10.137.12.15:42108]
16/05/24 16:31:40 INFO util.Utils: Successfully started service 'sparkDriverActorSystem' on port 42108.
16/05/24 16:31:40 INFO spark.SparkEnv: Registering MapOutputTracker
16/05/24 16:31:40 INFO spark.SparkEnv: Registering BlockManagerMaster
16/05/24 16:31:40 INFO storage.DiskBlockManager: Created local directory at /tmp/blockmgr-da720587-401c-4960-88a8-0fd2195a6084
16/05/24 16:31:40 INFO storage.MemoryStore: MemoryStore started with capacity 511.1 MB
16/05/24 16:31:40 INFO spark.SparkEnv: Registering OutputCommitCoordinator
16/05/24 16:31:41 INFO server.Server: jetty-8.y.z-SNAPSHOT
16/05/24 16:31:41 INFO server.AbstractConnector: Started SelectChannelConnector@0.0.0.0:4040
16/05/24 16:31:41 INFO util.Utils: Successfully started service 'SparkUI' on port 4040.
16/05/24 16:31:41 INFO ui.SparkUI: Started SparkUI at http://10.137.12.15:4040
16/05/24 16:31:41 INFO spark.HttpFileServer: HTTP File server directory is /tmp/spark-273678ff-5643-4a70-821f-b7a9fed4b321/httpd-7855134c-f391-4811-b5b3-720c4750e41d
16/05/24 16:31:41 INFO spark.HttpServer: Starting HTTP Server
16/05/24 16:31:41 INFO server.Server: jetty-8.y.z-SNAPSHOT
16/05/24 16:31:41 INFO server.AbstractConnector: Started SocketConnector@0.0.0.0:58161
16/05/24 16:31:41 INFO util.Utils: Successfully started service 'HTTP file server' on port 58161.
16/05/24 16:31:44 INFO spark.SparkContext: Added JAR file:/usr/local/spark/spark-1.6.0-bin-hadoop2.6/bin/../lib/spark-examples-1.6.0-hadoop2.6.0.jar at http://10.137.12.15:58161/jars/spark-examples-1.6.0-hadoop2.6.0.jar with timestamp 1464078704771
16/05/24 16:31:45 INFO executor.Executor: Starting executor ID driver on host localhost
16/05/24 16:31:45 INFO util.Utils: Successfully started service 'org.apache.spark.network.netty.NettyBlockTransferService' on port 41423.
16/05/24 16:31:45 INFO netty.NettyBlockTransferService: Server created on 41423
16/05/24 16:31:45 INFO storage.BlockManagerMaster: Trying to register BlockManager
16/05/24 16:31:45 INFO storage.BlockManagerMasterEndpoint: Registering block manager localhost:41423 with 511.1 MB RAM, BlockManagerId(driver, localhost, 41423)
16/05/24 16:31:45 INFO storage.BlockManagerMaster: Registered BlockManager
16/05/24 16:31:46 INFO scheduler.EventLoggingListener: Logging events to hdfs://Master:9000/historyserverforSpark/local-1464078704938
16/05/24 16:31:47 INFO spark.SparkContext: Starting job: reduce at SparkPi.scala:36
16/05/24 16:31:47 INFO scheduler.DAGScheduler: Got job 0 (reduce at SparkPi.scala:36) with 10 output partitions
16/05/24 16:31:47 INFO scheduler.DAGScheduler: Final stage: ResultStage 0 (reduce at SparkPi.scala:36)
16/05/24 16:31:47 INFO scheduler.DAGScheduler: Parents of final stage: List()
16/05/24 16:31:47 INFO scheduler.DAGScheduler: Missing parents: List()
16/05/24 16:31:47 INFO scheduler.DAGScheduler: Submitting ResultStage 0 (MapPartitionsRDD[1] at map at SparkPi.scala:32), which has no missing parents
16/05/24 16:31:47 INFO storage.MemoryStore: Block broadcast_0 stored as values in memory (estimated size 1888.0 B, free 1888.0 B)
16/05/24 16:31:47 INFO storage.MemoryStore: Block broadcast_0_piece0 stored as bytes in memory (estimated size 1202.0 B, free 3.0 KB)
16/05/24 16:31:47 INFO storage.BlockManagerInfo: Added broadcast_0_piece0 in memory on localhost:41423 (size: 1202.0 B, free: 511.1 MB)
16/05/24 16:31:47 INFO spark.SparkContext: Created broadcast 0 from broadcast at DAGScheduler.scala:1006
16/05/24 16:31:47 INFO scheduler.DAGScheduler: Submitting 10 missing tasks from ResultStage 0 (MapPartitionsRDD[1] at map at SparkPi.scala:32)
16/05/24 16:31:47 INFO scheduler.TaskSchedulerImpl: Adding task set 0.0 with 10 tasks
16/05/24 16:31:47 INFO scheduler.TaskSetManager: Starting task 0.0 in stage 0.0 (TID 0, localhost, partition 0,PROCESS_LOCAL, 2155 bytes)
16/05/24 16:31:47 INFO scheduler.TaskSetManager: Starting task 1.0 in stage 0.0 (TID 1, localhost, partition 1,PROCESS_LOCAL, 2155 bytes)
16/05/24 16:31:47 INFO executor.Executor: Running task 0.0 in stage 0.0 (TID 0)
16/05/24 16:31:47 INFO executor.Executor: Running task 1.0 in stage 0.0 (TID 1)
16/05/24 16:31:47 INFO executor.Executor: Fetching http://10.137.12.15:58161/jars/spark-examples-1.6.0-hadoop2.6.0.jar with timestamp 1464078704771
16/05/24 16:31:48 INFO util.Utils: Fetching http://10.137.12.15:58161/jars/spark-examples-1.6.0-hadoop2.6.0.jar to /tmp/spark-273678ff-5643-4a70-821f-b7a9fed4b321/userFiles-59ac0eb0-22a3-4d73-9c64-7af082c25799/fetchFileTemp1263431100853004677.tmp
16/05/24 16:31:51 INFO executor.Executor: Adding file:/tmp/spark-273678ff-5643-4a70-821f-b7a9fed4b321/userFiles-59ac0eb0-22a3-4d73-9c64-7af082c25799/spark-examples-1.6.0-hadoop2.6.0.jar to class loader
16/05/24 16:31:52 INFO executor.Executor: Finished task 0.0 in stage 0.0 (TID 0). 1031 bytes result sent to driver
16/05/24 16:31:52 INFO executor.Executor: Finished task 1.0 in stage 0.0 (TID 1). 1031 bytes result sent to driver
16/05/24 16:31:52 INFO scheduler.TaskSetManager: Starting task 2.0 in stage 0.0 (TID 2, localhost, partition 2,PROCESS_LOCAL, 2155 bytes)
16/05/24 16:31:52 INFO executor.Executor: Running task 2.0 in stage 0.0 (TID 2)
16/05/24 16:31:52 INFO scheduler.TaskSetManager: Starting task 3.0 in stage 0.0 (TID 3, localhost, partition 3,PROCESS_LOCAL, 2155 bytes)
16/05/24 16:31:52 INFO executor.Executor: Running task 3.0 in stage 0.0 (TID 3)
16/05/24 16:31:52 INFO scheduler.TaskSetManager: Finished task 0.0 in stage 0.0 (TID 0) in 4629 ms on localhost (1/10)
16/05/24 16:31:52 INFO scheduler.TaskSetManager: Finished task 1.0 in stage 0.0 (TID 1) in 4584 ms on localhost (2/10)
16/05/24 16:31:52 INFO executor.Executor: Finished task 2.0 in stage 0.0 (TID 2). 1031 bytes result sent to driver
16/05/24 16:31:52 INFO scheduler.TaskSetManager: Starting task 4.0 in stage 0.0 (TID 4, localhost, partition 4,PROCESS_LOCAL, 2155 bytes)
16/05/24 16:31:52 INFO executor.Executor: Running task 4.0 in stage 0.0 (TID 4)
16/05/24 16:31:52 INFO scheduler.TaskSetManager: Finished task 2.0 in stage 0.0 (TID 2) in 170 ms on localhost (3/10)
16/05/24 16:31:52 INFO executor.Executor: Finished task 3.0 in stage 0.0 (TID 3). 1031 bytes result sent to driver
16/05/24 16:31:52 INFO scheduler.TaskSetManager: Starting task 5.0 in stage 0.0 (TID 5, localhost, partition 5,PROCESS_LOCAL, 2155 bytes)
16/05/24 16:31:52 INFO executor.Executor: Running task 5.0 in stage 0.0 (TID 5)
16/05/24 16:31:52 INFO scheduler.TaskSetManager: Finished task 3.0 in stage 0.0 (TID 3) in 171 ms on localhost (4/10)
16/05/24 16:31:52 INFO executor.Executor: Finished task 4.0 in stage 0.0 (TID 4). 1031 bytes result sent to driver
16/05/24 16:31:52 INFO scheduler.TaskSetManager: Starting task 6.0 in stage 0.0 (TID 6, localhost, partition 6,PROCESS_LOCAL, 2155 bytes)
16/05/24 16:31:52 INFO scheduler.TaskSetManager: Finished task 4.0 in stage 0.0 (TID 4) in 102 ms on localhost (5/10)
16/05/24 16:31:52 INFO executor.Executor: Running task 6.0 in stage 0.0 (TID 6)
16/05/24 16:31:52 INFO executor.Executor: Finished task 5.0 in stage 0.0 (TID 5). 1031 bytes result sent to driver
16/05/24 16:31:52 INFO scheduler.TaskSetManager: Starting task 7.0 in stage 0.0 (TID 7, localhost, partition 7,PROCESS_LOCAL, 2155 bytes)
16/05/24 16:31:52 INFO executor.Executor: Running task 7.0 in stage 0.0 (TID 7)
16/05/24 16:31:52 INFO scheduler.TaskSetManager: Finished task 5.0 in stage 0.0 (TID 5) in 114 ms on localhost (6/10)
16/05/24 16:31:52 INFO executor.Executor: Finished task 6.0 in stage 0.0 (TID 6). 1031 bytes result sent to driver
16/05/24 16:31:52 INFO scheduler.TaskSetManager: Starting task 8.0 in stage 0.0 (TID 8, localhost, partition 8,PROCESS_LOCAL, 2155 bytes)
16/05/24 16:31:52 INFO executor.Executor: Running task 8.0 in stage 0.0 (TID 8)
16/05/24 16:31:52 INFO scheduler.TaskSetManager: Finished task 6.0 in stage 0.0 (TID 6) in 191 ms on localhost (7/10)
16/05/24 16:31:52 INFO executor.Executor: Finished task 7.0 in stage 0.0 (TID 7). 1031 bytes result sent to driver
16/05/24 16:31:52 INFO scheduler.TaskSetManager: Starting task 9.0 in stage 0.0 (TID 9, localhost, partition 9,PROCESS_LOCAL, 2155 bytes)
16/05/24 16:31:52 INFO scheduler.TaskSetManager: Finished task 7.0 in stage 0.0 (TID 7) in 180 ms on localhost (8/10)
16/05/24 16:31:52 INFO executor.Executor: Running task 9.0 in stage 0.0 (TID 9)
16/05/24 16:31:52 INFO executor.Executor: Finished task 8.0 in stage 0.0 (TID 8). 1031 bytes result sent to driver
16/05/24 16:31:53 INFO executor.Executor: Finished task 9.0 in stage 0.0 (TID 9). 1031 bytes result sent to driver
16/05/24 16:31:53 INFO scheduler.TaskSetManager: Finished task 8.0 in stage 0.0 (TID 8) in 106 ms on localhost (9/10)
16/05/24 16:31:53 INFO scheduler.TaskSetManager: Finished task 9.0 in stage 0.0 (TID 9) in 100 ms on localhost (10/10)
16/05/24 16:31:53 INFO scheduler.DAGScheduler: ResultStage 0 (reduce at SparkPi.scala:36) finished in 5.163 s
16/05/24 16:31:53 INFO scheduler.TaskSchedulerImpl: Removed TaskSet 0.0, whose tasks have all completed, from pool 
16/05/24 16:31:53 INFO scheduler.DAGScheduler: Job 0 finished: reduce at SparkPi.scala:36, took 5.894735 s

//*****************************************************
//Pi的计算结果：
//*****************************************************

Pi is roughly 3.14058
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/metrics/json,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/stages/stage/kill,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/api,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/static,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/executors/threadDump/json,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/executors/threadDump,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/executors/json,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/executors,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/environment/json,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/environment,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/storage/rdd/json,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/storage/rdd,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/storage/json,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/storage,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/stages/pool/json,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/stages/pool,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/stages/stage/json,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/stages/stage,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/stages/json,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/stages,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/jobs/job/json,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/jobs/job,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/jobs/json,null}
16/05/24 16:31:53 INFO handler.ContextHandler: stopped o.s.j.s.ServletContextHandler{/jobs,null}
16/05/24 16:31:53 INFO ui.SparkUI: Stopped Spark web UI at http://10.137.12.15:4040
16/05/24 16:31:53 INFO spark.MapOutputTrackerMasterEndpoint: MapOutputTrackerMasterEndpoint stopped!
16/05/24 16:31:53 INFO storage.MemoryStore: MemoryStore cleared
16/05/24 16:31:53 INFO storage.BlockManager: BlockManager stopped
16/05/24 16:31:53 INFO storage.BlockManagerMaster: BlockManagerMaster stopped
16/05/24 16:31:53 INFO scheduler.OutputCommitCoordinator$OutputCommitCoordinatorEndpoint: OutputCommitCoordinator stopped!
16/05/24 16:31:53 INFO remote.RemoteActorRefProvider$RemotingTerminator: Shutting down remote daemon.
16/05/24 16:31:53 INFO remote.RemoteActorRefProvider$RemotingTerminator: Remote daemon shut down; proceeding with flushing remote transports.
16/05/24 16:31:54 INFO spark.SparkContext: Successfully stopped SparkContext
16/05/24 16:31:54 INFO util.ShutdownHookManager: Shutdown hook called
16/05/24 16:31:54 INFO util.ShutdownHookManager: Deleting directory /tmp/spark-273678ff-5643-4a70-821f-b7a9fed4b321
16/05/24 16:31:54 INFO util.ShutdownHookManager: Deleting directory /tmp/spark-273678ff-5643-4a70-821f-b7a9fed4b321/httpd-7855134c-f391-4811-b5b3-720c4750e41d
16/05/24 16:31:54 INFO remote.RemoteActorRefProvider$RemotingTerminator: Remoting shut down.
```