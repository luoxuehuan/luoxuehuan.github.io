---
layout: post
title: Hibernate实现不带条件的简单分页查询
date: 2015-08-19
categories: blog
tags: [Hibernate]
description: Hibernate实现不带条件的简单分页查询
---

# Hibernate实现不带条件的简单分页查询。 #

----------

> **主要用到：**

	Query query = getSessionDao().createQuery(hql1);

> **及两个分页条件：**

	query.setFirstResult(firstPage*pageSize);//设置查询起始数
	
	query.setMaxResults(pageSize);//设置查询最大结果数


> **代码示例：**

```
    /**
    * 不带条件的简单分页查询
    * @author lxh
    * @version 1.0
    *
    */
    publicclassQueryListByPageextendsHibernateDaoImpl{
    /**
    * * 简单的分页的查询数据列表
    *
    * @param modelName
    * 实体类名
    * @param firstPage
    * 查询第几页
    * @param pageSize
    * 每页显示几条数据
    * @return 一页的数据列表，和总页数
    */
    @SuppressWarnings("unchecked")
    publicQueryResult findAllList(String modelName,int firstPage,int pageSize){
    	String hql1="from "+modelName;
	    String hql2="select count(*) from "+modelName;
	    Query query = getSessionDao().createQuery(hql1);
	    query.setFirstResult(firstPage*pageSize);
	    query.setMaxResults(pageSize);
	    Long count =(Long) getSessionDao().createQuery(hql2).uniqueResult();
	    int totalPageNum=count.intValue()/pageSize+1;//总页数
	    List list = query.list();
	    // 返回结果
	    returnnewQueryResult(totalPageNum, list);
	    /**
	    *
	    * Map map = new HashMap();
	    * map.put("xx",list);
	    * map.put("xxx",totalPageNum);
	    * JSONObject jobj = JSONTUtil.toObject(map);
	    *
	    */
    }
   
    
    
    import java.util.List;
    publicclassQueryResult{
	    privateint totalPageNum;// 总记录数
	    privateList list;// 一页的数据
	    publicQueryResult(int totalPageNum,List list){
		    this.totalPageNum = totalPageNum;
		    this.list = list;
	    }
	    publicint getTotalPageNum(){
	    	return totalPageNum;
	    }
	    publicvoid setTotalPageNum(int totalPageNum）{
	    	this.totalPageNum = totalPageNum;
	    }
	    publicList getList(){
	    	return list;
	    }
	    publicvoid setList(List list){
	   		this.list = list;
	    }
    }
```