####mvn 配置
```
 <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>2.8.0</version>
        </dependency>
```
String
```
        Jedis jedis = new Jedis("127.0.0.1",6379);
        jedis.set("hello","redis");
        System.out.println(jedis.get("hello"));
        jedis.incr("counter");
        System.out.println(jedis.get("counter"));
```
hash
```
        jedis.hset("myhash","f1","v1");
        jedis.hset("myhash","f2","v2");
        System.out.println(jedis.hgetAll("myhash"));
```
list
```
        Jedis jedis = new Jedis("127.0.0.1",6379);
        jedis.rpush("mylist","1");
        jedis.rpush("mylist","2");
        jedis.rpush("mylist","3");
        List<String> list = jedis.lrange("mylist",0,-1);
        System.out.println(list);
```
set
```
        Jedis jedis = new Jedis("127.0.0.1", 6379);
        jedis.sadd("myset", "q");
        jedis.sadd("myset", "w");
        jedis.sadd("myset", "q");
        Set<String> myset = jedis.smembers("myset");
        System.out.println(myset);
```
zset
```
        Jedis jedis = new Jedis("127.0.0.1", 6379);
        jedis.zadd("myzset", 1,"xiaohong");
        jedis.zadd("myzset", 2,"xiaowang");
        jedis.zadd("myzset", 3,"xiaoming");
        Set<String> myset = jedis.zrange("myzset",0,-1);
        System.out.println(myset);
        System.out.println(jedis.zrangeByScore("myzset",1,2));
```
连接池
客户端连接redis 使用的是TCP协议，直连的方式每次需要建立TCP连接，而连接池的方式可以预先初始化好jedis连接，所以每次只需要从Jedis连接池借用即可，小于新建TCP连接的开销，直接连接的方式无限限制Jedis对象的个数，在极端情况下可能造成连接泄漏。
```
        GenericObjectPoolConfig poolConfig = new GenericObjectPoolConfig();
        JedisPool jedisPool = new JedisPool(poolConfig, "127.0.0.1", 6379);
        Jedis jedis = jedisPool.getResource();
        System.out.println(jedis.get("hello"));
        jedis.close();
```
####Pipeline
pipeline.sync();才会真正执行数据操作
```
  GenericObjectPoolConfig poolConfig = new GenericObjectPoolConfig();
        JedisPool jedisPool = new JedisPool(poolConfig, "127.0.0.1", 6379);
        Jedis jedis = jedisPool.getResource();
        Pipeline pipeline = jedis.pipelined();
        pipeline.set("test","pipeline");
        pipeline.set("hi","redis");
        pipeline.sync();
        System.out.println(jedis.get("test"));
        System.out.println(jedis.get("hi"));
```
