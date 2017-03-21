---
layout: post
title: Java类共同的父类Object
date: 2015-04-02
categories: blog
tags: [Java基础]
description: 简单的介绍java
---




## 一、Java类共同的父类Object

	1、hashCode()1方法

格式：public int hashCode()

功能：返回当前对象哈希码数值

 

	2、toString()方法

功能：以字符串形式返回当前对象的有关信息。

注意：一般要重写toString.

 

	3、equals()方法。

功能：比较当前对象和方法参数obj所引用的对象两者的等价性，比较的是变量的值（内容）。

而“==”比较的是内存地址。

 

	4、finalize()方法。

java运行时环境中的垃圾收集器在销毁一个对象之前，会自动调用该对象的finalize()方法，然后才释放对象的内存空间。修饰符为protected。

重写后：public。

 

	5、clone()方法。

通过复制一个现有的对象，得到一个新对象，与现有对象封装完全相同的信息（属性值）

目的：为了此后两者互不相干，修改其中的一个对象不会影响到另一个。