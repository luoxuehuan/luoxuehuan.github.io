DataFrame可以从结构化文件、hive表、外部数据库以及现有的RDD加载构建得到。具体的结构化文件、hive表、外部数据库的相关加载可以参考其他章节。这里主要针对从现有的RDD来构建DataFrame进行实践与解析。

	Spark SQL 支持两种方式将存在的RDD转化为DataFrame。

> 第一种方法是使用反射来推断包含特定对象类型的RDD的模式。在写Spark程序的同时，已经知道了模式，这种基于反射的方法可以使代码更简洁并且程序工作得更好。

> 第二种方法是通过一个编程接口来实现，这个接口允许构造一个模式，然后在存在的RDD上使用它。虽然这种方法代码较为冗长，但是它允许在运行期间之前不止列以及列的类型的情况下构造DataFrame。

DataFrame是一个带有列名的分布式数据集合。等同于一张关系型数据库中的表或者R/Python中的data frame，不过在底层做了很多优化；我们可以使用结构化数据文件、Hive tables，外部数据库或者RDDS来构造DataFrames。
   
## 一、利用反射推断Schema 
Spark SQL能够将含Row对象的RDD转换成DataFrame，并推断数据类型。通过将一个键值对（key/value）列表作为kwargs传给Row类来构造Rows。key定义了表的列名，类型通过看第一列数据来推断。（所以这里RDD的第一列数据不能有缺失）未来版本中将会通过看更多数据来推断数据类型，像现在对JSON文件的处理一样。

## 二、编程指定Schema 
	
	通过编程指定Schema需要3步：
	
### 1.从原来的RDD创建一个元祖或列表的RDD。

### 2.用StructType 创建一个和步骤一中创建的RDD中元祖或列表的结构相匹配的Schema。

### 3.通过SQLContext提供的createDataFrame方法将schema 应用到RDD上。

##三、实战

### 1.使用Java实战RDD与DataFrame转换
简单介绍：
动态构造有时候有些麻烦：spark开发了一个API就是DataSet ，DataSet可以基于RDD，RDD里面有类型。他可以基于这种类型。
sparkSQL+DataFrame+DataSet:三者都相当重要，在2.0的时候编码会使用大量使用DataSet。
DataSet上可以直接查询。Spark的核心RDD+DataFrame+DataSet:最终会形成三足鼎立。RDD实际是服务SparkSQL的。DataSet是想要用所有的子框架都用DataSet进行计算。DataSet的底层是无私计划。这就让天然的性能优势体现出来。
官方建议使用hiveContext，在功能上比SQLContext的更好更高级的功能。

```
 public class RDD2DataFrameByProgrammatically {
	public static void main(String[] args) {
		SparkConf conf = new SparkConf().setMaster("local").setAppName("RDD2DataFrameByProgrammatically");
		JavaSparkContext sc = new JavaSparkContext(conf);
		SQLContext sqlContext = new SQLContext(sc);
		JavaRDD<String> lines = sc.textFile("E://test.txt");	
		/** * 第一步：在RDD的基础上创建类型为Row的RDD */
		JavaRDD<Row> personsRDD = lines.map(new Function<String, Row>() {
			@Override
			public Row call(String line) throws Exception {
				String[] splited = line.split(",");
				return RowFactory.create(Integer.valueOf(splited[0]), splited[1],Integer.valueOf(splited[2]));
			}
		});
		/*** 第二步：动态构造DataFrame的元数据，一般而言，有多少列以及每列的具体类型可能来自于JSON文件，也可能来自于DB */
		List<StructField> structFields = new ArrayList<StructField>();
		structFields.add(DataTypes.createStructField("id", DataTypes.IntegerType, true));
		structFields.add(DataTypes.createStructField("name", DataTypes.StringType, true));
		structFields.add(DataTypes.createStructField("age", DataTypes.IntegerType, true));
		//构建StructType，用于最后DataFrame元数据的描述
		StructType structType =DataTypes.createStructType(structFields);
		
		/*** 第三步：基于以后的MetaData以及RDD<Row>来构造DataFrame*/
		DataFrame personsDF = sqlContext.createDataFrame(personsRDD, structType);
		/** 第四步：注册成为临时表以供后续的SQL查询操作*/
		personsDF.registerTempTable("persons");
		/** 第五步，进行数据的多维度分析*/
		DataFrame result = sqlContext.sql("select * from persons where age >20");
		/**第六步：对结果进行处理，包括由DataFrame转换成为RDD<Row>，以及结构持久化*/
		List<Row> listRow = result.javaRDD().collect();
		for(Row row : listRow){
			System.out.println(row);
		}
	}
}
```


### 2.使用Scala实战RDD与DataFrame转换

```
import org.apache.spark.{SparkContext, SparkConf}
import org.apache.spark.sql.SQLContext


class RDD2DataFrameByProgrammaticallyScala {
  def main(args:Array[String]): Unit = {
    val conf = new SparkConf()
    conf.setAppName("RDD2DataFrameByProgrammaticallyScala") //设置应用程序的名称，在程序运行的监控界面可以看到名称
    conf.setMaster("local")
    val sc = new SparkContext(conf)
    val sqlContext = new SQLContext(sc)
    val people = sc.textFile("C://Users//DS01//Desktop//persons.txt")
    val schemaString = "name age"
    import org.apache.spark.sql.Row;
    import org.apache.spark.sql.types.{StructType,StructField,StringType};
    val schema = StructType(schemaString.split(" ").map(fieldName=>StructField(fieldName,StringType,true)))
    val rowRDD = people.map(_.split(",")).map(p=>Row(p(0),p(1).trim))
    val peopleDataFrame = sqlContext.createDataFrame(rowRDD,schema)
    peopleDataFrame.registerTempTable("people")
    val results = sqlContext.sql("select name from people")
    results.map(t=>"Name: "+t(0)).collect().foreach(println)
  }
}
```

