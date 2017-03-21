```
package com.core

import org.apache.spark.{SparkConf, SparkContext}


/**
  * Created by lxh on 2016/3/14.
  * 查看源码快捷键：CTRL + N
  *
  */
object WordCount {
  def main(args: Array[String]) {

    
    val conf = new SparkConf() //创建spark conf 对象
    conf.setAppName("WordCount app")
    conf.setMaster("local")
    val sc = new SparkContext(conf)
    sc.setLogLevel("WARN")

    val filepath = "D://testdata//helloSpark.txt"
    /**
      * 读取filepath的文件。分片数为1.
      */
    val lines = sc.textFile(filepath, 1)

    /**
      * 将行根据空格切割成词。
      */
    val words = lines.flatMap { lines => lines.split(" ") }

    /**
      * 将词转化为元组。出现一次计数为1.
      */
    val pairs = words.map { word => (word, 1) }


    /**
      * 将出现次数 累加。
      * 并且 转换（词，次数） 为（次数，词） 根据key——次数 进行排序。 然后重新转换成（词，次数）
      */
    val wordCountsOdered = pairs.reduceByKey(_ + _).map(pair => (pair._2, pair._1)).sortByKey(false).map(pair => (pair._2, pair._1))

    /**
      * 对每个元素进行打印输出。
      */
    wordCountsOdered.foreach(wordNumberPair => println(wordNumberPair._1 + "" + wordNumberPair._2))
    /* val wordCount = pairs.reduceByKey(_+_)
     wordCount.foreach(wordNumberPair => println(wordNumberPair._1+""+wordNumberPair._2 ))*/
    while (true){
      //循环，以便在web 控制台观察。
    }
    sc.stop()
  }
}

```