##Redis介绍：

redis是一个key-value存储系统。和Memcached类似，它支持存储的value类型相对更多，包括string(字符串)、list(链表)、set(集合)、zset(sorted set --有序集合)和hash（哈希类型）。这些数据类型都支持push/pop、add/remove及取交集并集和差集及更丰富的操作，而且这些操作都是原子性的。在此基础上，redis支持各种不同方式的排序。与memcached一样，为了保证效率，数据都是缓存在内存中。区别的是redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave(主从)同步。
Redis 是一个高性能的key-value数据库。 redis的出现，很大程度补偿了memcached这类key/value存储的不足，在部 分场合可以对关系数据库起到很好的补充作用。它提供了Java，C/C++，C#，PHP，JavaScript，Perl，Object-C，Python，Ruby，Erlang等客户端，使用很方便。[1] 
Redis支持主从同步。数据可以从主服务器向任意数量的从服务器上同步，从服务器可以是关联其他从服务器的主服务器。这使得Redis可执行单层树复制。存盘可以有意无意的对数据进行写操作。由于完全实现了发布/订阅机制，使得从数据库在任何地方同步树时，可订阅一个频道并接收主服务器完整的消息发布记录。同步对读取操作的可扩展性和数据冗余很有帮助。

##Redis安装：

在linux执行以下命令
```
sudo apt-get install redis-server
```

启动 Redis：

```
redis-server
```

连接redis：

```
redis-cli
```

输入：

```
redis 127.0.0.1:6379> ping
```
显示：

```
PONG
```
说明你已经成功地安装Redis在您的机器上。


## Java操作Redis（Jedis）

>首先要修改/etc/redis的redis配置。讲ip绑定为127.0.0.1注释掉。 否则无法在另外一台电脑上远程连接redis！（例如在windows里远程连接虚拟机linux里的redis）

### 导入jar包

```
jedis-2.7.2.jar
commons-pool2-2.4.2.jar
```

### 建立测试类

```
public class RedisJava {

	public static void main(String[] args) {
	
		// Connecting to Redis server on localhost
		Jedis jedis = new Jedis("10.137.12.12", 6379);

		System.out.println("Connection to server sucessfully");
		
		// check whether server is running or not
		System.out.println("Server is running: " + jedis.ping());//成功会输出PONG

		jedis.set("tch", "sb");
		// Get the stored data and print it
		System.out.println("tch:: " + jedis.get("tch"));
	}
}
```

附录：



```
package com.java.cache.redis;

import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Map;

import org.junit.Before;
import org.junit.Test;

import redis.clients.jedis.Jedis;

public class TestRedis {
    private Jedis jedis; 
    
    @Before
    public void setup() {
        //连接redis服务器，192.168.0.100:6379
        jedis = new Jedis("10.137.12.12", 6379);
        //权限认证
       // jedis.auth("admin");  
    }
    
    /**
     * jedis存储字符串
     */
    @Test
    public void testString() {
        //-----添加数据----------  
        jedis.set("name","xinxin");//向key-->name中放入了value-->xinxin  
        System.out.println(jedis.get("name"));//执行结果：xinxin  
        
        jedis.append("name", " is my lover"); //拼接
        System.out.println(jedis.get("name")); 
        
        jedis.del("name");  //删除某个键
        System.out.println(jedis.get("name"));
        //设置多个键值对
        jedis.mset("name","liuling","age","23","qq","476777XXX");
        jedis.incr("age"); //进行加1操作
        System.out.println(jedis.get("name") + "-" + jedis.get("age") + "-" + jedis.get("qq"));
        
    }
    
    /**
     * jedis操作Map
     */
    @Test
    public void testMap() {
        //-----添加数据----------  
        Map<String, String> map = new HashMap<String, String>();
        map.put("name", "spark");
        map.put("age", "4");
        map.put("qq", "123456");
        jedis.hmset("user",map);
        //取出user中的name，执行结果:[minxr]-->注意结果是一个泛型的List  
        //第一个参数是存入redis中map对象的key，后面跟的是放入map中的对象的key，后面的key可以跟多个，是可变参数  
        List<String> rsmap = jedis.hmget("user", "name", "age", "qq");
        System.out.println(rsmap);  
  
        //删除map中的某个键值  
        jedis.hdel("user","age");
        System.out.println(jedis.hmget("user", "age")); //因为删除了，所以返回的是null  
        System.out.println(jedis.hlen("user")); //返回key为user的键中存放的值的个数2 
        System.out.println(jedis.exists("user"));//是否存在key为user的记录 返回true  
        System.out.println(jedis.hkeys("user"));//返回map对象中的所有key  
        System.out.println(jedis.hvals("user"));//返回map对象中的所有value 
  
        Iterator<String> iter=jedis.hkeys("user").iterator();  
        while (iter.hasNext()){  
            String key = iter.next();  
            System.out.println(key+":"+jedis.hmget("user",key));  
        }  
    }
    
    /** 
     * jedis操作List 
     */  
    @Test  
    public void testList(){  
        //开始前，先移除所有的内容  
        jedis.del("big data");  
        System.out.println(jedis.lrange("big data",0,-1));  
        //先向key java framework中存放三条数据  
        jedis.lpush("big data","hadoop");  
        jedis.lpush("big data","flink");  
        jedis.lpush("big data","spark");  
        //再取出所有数据jedis.lrange是按范围取出,
        //第一个是key,第二个是起始位置,第三个是结束位置,jedis.llen获取长度 -1表示取得所有  
        System.out.println(jedis.lrange("java framework",0,-1));  
        
        jedis.del("big data");
        jedis.rpush("big data","spark");  
        jedis.rpush("big data","hadoop");  
        jedis.rpush("big data","scala"); 
        System.out.println(jedis.lrange("big data",0,-1));
    }  
    
    /** 
     * jedis操作Set 
     */  
    @Test  
    public void testSet(){  
        //测试之前先删除
    	jedis.del("data");
        jedis.sadd("data","spark");  
        jedis.sadd("data","scala");  
        jedis.sadd("data","hadoop");  
        jedis.sadd("data","kafka");
        jedis.sadd("data","Zookeeper");  
        //移除noname  
        jedis.srem("data","kafka");  
        System.out.println("获取所有加入的value		"+jedis.smembers("data"));//获取所有加入的value  
        System.out.println("判断 hadoop 是否是data集合的元素		"+jedis.sismember("data", "hadoop"));//判断 hadoop 是否是data集合的元素  
        System.out.println("随机获取一个元素		"+jedis.srandmember("data"));  
        System.out.println("返回集合的元素个数		"+jedis.scard("data"));//返回集合的元素个数  
    }  
  
    @Test  
    public void testSort() throws InterruptedException {  
        //jedis 排序  
        //注意，此处的rpush和lpush是List的操作。是一个双向链表（但从表现来看的）  
        jedis.del("a");//先清除数据，再加入数据进行测试  
        jedis.rpush("a", "1");  
        jedis.lpush("a","6");  
        jedis.lpush("a","3");  
        jedis.lpush("a","9");  
        System.out.println(jedis.lrange("a",0,-1));// [9, 3, 6, 1]  
        System.out.println("输出排序后结果 "+jedis.sort("a")); //[1, 3, 6, 9]  
        System.out.println(jedis.lrange("a",0,-1));  
    }  
    
    @Test
    public void testRedisPool() {
        RedisUtil.getJedis().set("newname", "中文测试");
        System.out.println(RedisUtil.getJedis().get("newname"));
    }
}
```


```
package com.java.cache.redis;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;

public class RedisUtil {
	 //Redis服务器IP
    private static String ADDR = "10.137.12.12";
    
    //Redis的端口号
    private static int PORT = 6379;
    
    //访问密码
    private static String AUTH = "admin";
    
    //可用连接实例的最大数目，默认值为8；
    //如果赋值为-1，则表示不限制；如果pool已经分配了maxActive个jedis实例，则此时pool的状态为exhausted(耗尽)。
    private static int MAX_ACTIVE = 1024;
    
    //控制一个pool最多有多少个状态为idle(空闲的)的jedis实例，默认值也是8。
    private static int MAX_IDLE = 200;
    
    //等待可用连接的最大时间，单位毫秒，默认值为-1，表示永不超时。如果超过等待时间，则直接抛出JedisConnectionException；
    private static int MAX_WAIT = 10000;
    
    private static int TIMEOUT = 10000;
    
    //在borrow一个jedis实例时，是否提前进行validate操作；如果为true，则得到的jedis实例均是可用的；
    private static boolean TEST_ON_BORROW = true;
    
    private static JedisPool jedisPool = null;
    
    /**
     * 初始化Redis连接池
     */
    static {
        try {
            JedisPoolConfig config = new JedisPoolConfig();
            config.setMaxIdle(MAX_ACTIVE);
            config.setMaxIdle(MAX_IDLE);
            config.setMaxWaitMillis(MAX_WAIT);
            config.setTestOnBorrow(TEST_ON_BORROW);
            jedisPool = new JedisPool(config, ADDR, PORT, TIMEOUT);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    /**
     * 获取Jedis实例
     * @return
     */
    public synchronized static Jedis getJedis() {
        try {
            if (jedisPool != null) {
                Jedis resource = jedisPool.getResource();
                return resource;
            } else {
                return null;
            }
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }
    
    /**
     * 释放jedis资源
     * @param jedis
     */
    @SuppressWarnings("deprecation")
	public static void returnResource(final Jedis jedis) {
        if (jedis != null) {
            jedisPool.returnResource(jedis);
        }
    }
}

```


