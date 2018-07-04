---
layout: post
title:  spark+carbondata使用
date:   2017-07-12 09:54:07 +08:00
category: carbondata
tags: Spark 性能优化
comments: true
---
* content
{:toc}
spark+carbondata使用





### 一、部署

#### 下载源码编译


#### 修改配置文件

#### 注意： 1.1.1 不支持spark2.2 会报错。


### 二、启动：
> spark-shell --jars carbonlib/carbondata_2.11-1.1.1-shade-hadoop2.7.2.jar


### 三、使用

### 3.1创建上下文

```
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.CarbonSession._
val carbon = SparkSession.builder().config(sc.getConf).getOrCreateCarbonSession("hdfs://127.0.0.1:9000/user/carbon/carbonstore")
carbon.sql("show tables")
```


### 3.2创表

```
carbon.sql("CREATE TABLE dtwave_dev.carbon_tablename_new (name String, PhoneNumber String) STORED BY 'carbondata'")

carbon.sql("insert into table dtwave_dev.carbon_tablename_new select * from dtwave_dev.spark_test")
carbon.sql("select * from dtwave_dev.carbon_tablename_new").show
```




### 3.3 入库：


> SQL:

```
CREATE TABLE tablename (name String, PhoneNumber String) STORED BY "carbondata"
TBLPROPERTIES (...)
LOAD DATA [LOCAL] INPATH 'folder path' [OVERWRITE] INTO TABLE tablename OPTIONS(...)
INSERT INTO TABLE tablennme select_statement1 FROM table1;
```

> DataFrame:

```
df.write.format(“carbondata").options("tableName", "t1")) .mode(SaveMode.Overwrite).save()
```


### 3.4 查询：

SELECT project_list FROM t1 WHERE cond_list
GROUP BY columns
ORDER BY columns



### 3.5 更新和删除


> 更新一列

```
UPDATE table1 A
SET A.REVENUE = A.REVENUE - 10 WHERE A.PRODUCT = 'phone'
Modify two columns in table1
```

> 更新两列

```
UPDATE table1 A
SET (A.PRODUCT, A.REVENUE) =
(
SELECT PRODUCT, REVENUE
FROM table2 B
WHERE B.CITY = A.CITY AND B.BROKER = A.BROKER
)
WHERE A.DATE BETWEEN '2017-01-01' AND '2017-01-31'



```

### 3.6 插入

```

scala> carbon.sql("insert into table dtwave_dev.carbon_tablename_new select * from dtwave_dev.carbon_tablename_new")

17/09/20 16:51:56 AUDIT rdd.CarbonDataRDDFactory$: [hulbdeMacBook-Air.local][hulb][Thread-1]Data load request has been received for table dtwave_dev.carbon_tablename_new
17/09/20 16:51:56 WARN util.CarbonDataProcessorUtil: [Executor task launch worker-7][partitionID:new;queryID:164589401701872] sort scope is set to LOCAL_SORT
17/09/20 16:51:56 WARN util.CarbonDataProcessorUtil: [Executor task launch worker-7][partitionID:new;queryID:164589401701872] batch sort size is set to 0
17/09/20 16:51:56 WARN util.CarbonDataProcessorUtil: [Executor task launch worker-7][partitionID:new;queryID:164589401701872] sort scope is set to LOCAL_SORT
17/09/20 16:53:28 AUDIT rdd.CarbonDataRDDFactory$: [hulbdeMacBook-Air.local][hulb][Thread-1]Data load is successful for dtwave_dev.carbon_tablename_new



carbon.sql("select count(1) from dtwave_dev.carbon_tablename_new").show

```

### 3.6 删除


```
DELETE FROM table1 A WHERE A.CUSTOMERID = ‘123’


scala>   carbon.sql("select * from dtwave_dev.carbon_tablename_new").show
+----+-----------+
|name|PhoneNumber|
+----+-----------+
|   1|          2|
|   3|          4|
+----+-----------+

scala> carbon.sql("delete from dtwave_dev.carbon_tablename_new a WHERE a.name='1'")

17/09/20 14:51:43 AUDIT command.ProjectForDeleteCommand: [hulbdeMacBook-Air.local][hulb][Thread-1] Delete data request has been received for dtwave_dev.carbon_tablename_new.
17/09/20 14:51:47 AUDIT command.deleteExecution$: [hulbdeMacBook-Air.local][hulb][Thread-1]Delete data operation is successful for dtwave_dev.carbon_tablename_new
res1: org.apache.spark.sql.DataFrame = []




scala> carbon.sql("select * from dtwave_dev.carbon_tablename_new").show
+----+-----------+
|name|PhoneNumber|
+----+-----------+
|   3|          4|
+----+-----------+



carbon.sql("update dtwave_dev.carbon_tablename_new A SET (A.name) = A.name WHERE A.PhoneNumber = '4'")


carbon.sql("UPDATE dtwave_dev.carbon_tablename_new a SET (a.name, a.PhoneNumber) = ( SELECT '5' as name ,'6' from dtwave_dev.carbon_tablename_new b)")


carbon.sql("UPDATE dtwave_dev.carbon_tablename_new a SET (a.name, a.PhoneNumber) = ( SELECT '5' as name ,'6' as PhoneNumber)")

```





### 四、HDFS对应的文件



### 4.1 HDFS 目录：
```
/user/carbon/carbonstore/dtwave_dev/carbon_tablename_new

drwxr-xr-x	hulb	supergroup	0 B	0	0 B	Fact
drwxr-xr-x	hulb	supergroup	0 B	0	0 B	Metadata


```



#### 4.2 数据文件

```
/user/carbon/carbonstore/dtwave_dev/carbon_tablename_new/Fact/Part0

Permission	Owner	Group	Size	Replication	Block Size	Name
drwxr-xr-x	hulb	supergroup	0 B	0	0 B	Segment_0
drwxr-xr-x	hulb	supergroup	0 B	0	0 B	Segment_1

```


#### 4.3 元数据文件

```
/user/carbon/carbonstore/dtwave_dev/carbon_tablename_new/Metadata

Go!

Permission	Owner	Group	Size	Replication	Block Size	Name
-rw-r--r--	hulb	supergroup	16 B	2	128 MB	62acf472-3574-434e-a53f-f45901dff949.dict
-rw-r--r--	hulb	supergroup	11 B	2	128 MB	62acf472-3574-434e-a53f-f45901dff949.dictmeta
-rw-r--r--	hulb	supergroup	11 B	2	128 MB	62acf472-3574-434e-a53f-f45901dff949_16.sortindex
-rw-r--r--	hulb	supergroup	16 B	2	128 MB	c5c7949a-a437-41d1-8f47-a7a81e68c4ba.dict
-rw-r--r--	hulb	supergroup	11 B	2	128 MB	c5c7949a-a437-41d1-8f47-a7a81e68c4ba.dictmeta
-rw-r--r--	hulb	supergroup	11 B	2	128 MB	c5c7949a-a437-41d1-8f47-a7a81e68c4ba_16.sortindex
-rw-r--r--	hulb	supergroup	387 B	2	128 MB	schema
-rw-r--r--	hulb	supergroup	7.37 KB	2	128 MB	tablestatus
-rw-r--r--	hulb	supergroup	243 B	2	128 MB	tableupdatestatus-1505890299461



```













