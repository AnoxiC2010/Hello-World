# Redis notes

## 1. 简介

Redis是什么？

Redis是完全开源免费的，遵守BSD协议，是⼀个⾼性能（NOSQL）的key-value数据库。Redis是⼀个开源的使⽤ANSI C语⾔编写、⽀持⽹络、可基于内存亦可持久化的⽇志型、Key-Value数据库，并提供多种语⾔的API。



Redis是一个内存数据库，也就是Redis是把数据存到内存里面的。这样访问速度就会很快。

[redis中文官网](http://redis.cn/)

[redis英文官网](https://redis.io/)

[开源地址](https://github.com/redis/redis)

## 2. 安装

Redis如何安装呢？

在服务器上有软件安装包

![image-20210430162442886](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Database\Redis-notes.assets\image-20210430162442886.png)



直接下载到本地，解压即可

解压需要注意：解压路径不要带中文，不要带空格





解压出来的文件列表：

![image-20210430163017339](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Database\Redis-notes.assets\image-20210430163017339.png)

启动：

​	进入对应的目录，打开cmd，执行指令

`redis-server.exe redis.windows.conf`



使用客户端去连接Redis

```cmd
redis-cli.exe -h 192.168.2.100 -p 3306
```

![image-20210430163908189](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Database\Redis-notes.assets\image-20210430163908189.png)



连接成功了以后，就可以去操作了

简单使用

![image-20210430164156795](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Database\Redis-notes.assets\image-20210430164156795.png)



## 3. 配置

接下来去介绍Redis的配置文件

### 3.1 常规配置

```properties
# 1. Redis 默认不是以守护进程的⽅式运⾏，可以通过该配置项修改，使⽤yes启动守护进程
 daemonize no
#. 当客户端闲置多⻓时间后关闭连接(单位是秒)
 timeout 300
# *3. 指定Redis监听端⼝，默认端⼝为6379，作者在⼀⽚博⽂中解释了为什么选⽤6379作为默认端⼝，因
# 为6379在⼿机按键上MERZ对应的号码，⽽MERZ取⾃意⼤利歌⼿Alessia Merz的名字
port 6379

#*4. 绑定的主机地址 127.0.0.1 意味着这个Redis服务器只有127.0.0.1这个ip才能访问（本机才能访问），如果你的Redis需要被别的远程机器访问，那么应该改成 0.0.0.0 (意味着说有的IP都能访问)
bind 127.0.0.1 
#5. 指定⽇志记录级别，Redis共⽀持四个级别：debug、verbose、notice、warning
 loglevel verbose
#6. 数据库数量(单机环境下)，默认数据库为0，可以使⽤select <dbid>命令在连接上指定数据库id
databases 16

# *10. 设置Redis连接密码，如果配置了连接密码，客户端在连接Redis的时候需要通过 AUTH
# <password> 命令提供密码，默认关闭
requirepass foobared
```



### 3.2 持久化配置(面试会问)

Redis给我们提供了两种持久化的方式，一种叫做RDB，一种叫做AOF

#### 3.2.1 RDB

RDB是指我们可以通过给内存快照的方式，来保存内存中的数据，这样去做持久化。

其实就是在某一个时刻，去保存内存里面的全部数据，然后写到磁盘，通过这种方式去做持久化。

配置：

```properties
#save <seconds> <changes>
# Redis默认配置⽂件提供了三个条件
 save 900 1
 save 300 10
 save 60 10000
#持久化⽂件名
 dbfilename dump.rdb
 
# 持久化文件保存路径
dir D:\soft\Redis-x64-3.2.100\tmp
```

RDB这种持久化方式会在快照的时候，保存内存里面所有数据，其实有的时候没有必要保存所有的，我们只需要保存新增的变化即可。

问题：RDB这种持久化方式有没有可能丢失数据？

有可能丢失数据，可能会丢失上一次持久化之后所有写入的数据



#### 3.2.2 AOF

Append only file，其实就是通过追加日志文件的方式来持久化。什么意思呢？其实就是通过把我们输入的命令保存到日志文件中，然后在进行数据恢复的时候，就是通过执行日志文件里面的命令来进行恢复整个数据库的内容。

```properties
# 默认是关闭的，改为yes之后表示开启AOF
appendonly yes

# 文件的名字，保存文件的路径和RDB是一致的
# The name of the append only file (default: "appendonly.aof")
appendfilename "appendonly.aof"

# appendfsync always
appendfsync everysec
# appendfsync no
```

问题：AOF会不会丢失数据呢？

在配置 `appendfsync always` 的前提下，AOF可以做到不丢失数据。但是这种配置方式推不推荐大家使用呢？

因为每一次（update、insert、delete） 操作都会同步持久化到磁盘，速度是比较慢的，甚至和Mysql是一样的了，也就是每一条命令都要持久化到日志文件，都要经过磁盘IO，速度比较慢，所以一般不采用该方式。



我们在以后的工作中，应该是采用AOF还是采用RDB呢？

在公司里面，早期的时候是采用RDB的，但是经过Redis对AOF这种持久化的方式优化了以后，AOF被越来越多的公司所采用，目前而言，一般是两个都用，AOF配置 `appendfsync everysec`



## 4. 命令介绍

Redis支持五种数据结构。哪五种呢？

 ### 4.1 String 字符串

字符串其实就是类似于Map，在Redis里面可以存储Key-value，这个value是单个值的时候，其实就是字符串。

什么叫单个值呢？例如zhangsan、3、3.3 ，其实数字，小数这些都是以字符串的方式来存储的

![image-20210504102229481](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Database\Redis-notes.assets\image-20210504102229481.png)

- SET key value

- GET key

- INCR 可以对应的key的数值（整型的数值）加⼀( 原⼦操作)

- INCRBY 给数值加上⼀个步⻓

- SETEX expire 过期

- SETNX not exist key不存在的时候再去赋值  不覆盖原来的值
- MSET 设置多个值
- MGET 获取多个值

使用场景：可以利用INCR命令，来统计网站的访问数，也可以统计在线人数。当然String这种数据结构，有很多使用场景，他是我们使用Redis最多的一种数据结构。

### 4.2 List

![image-20210504112107121](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Database\Redis-notes.assets\image-20210504112107121.png)

List这种数据结构其实有点类似于我们之前学过的 `栈`，特点是**有序，可重复**

- LPUSH 后面的元素放在栈顶

- LPOP 返回第⼀个元素，并且在列表上删除该元素 （栈顶）

- LLEN 返回当前的list列表的⻓度
- LINDEX 返回当前的list的指定index下标的元素。没有返回nil，0表示栈顶的元素

- LINSERT 插⼊的位置是按照index的顺序，Before的话得注意 index的值

- LPUSHX 如果list存在，再去push

- LRANGE 可以⽅便的查看某个index范围内的list的值。输⼊的index是从0开始，显示的标号是从1

开始的。

- LREM 删除list⾥的指定的前⼏个（指定value的）元素

  删除指定位置的元素：没有

- LSET 设置指定的位置的元素的值 （修改） 输⼊的index是从0开始，显示的标号是从1开始的。

使用场景：消息队列。消息的最新排行榜

### 4.3 Hash

其实这个是一个二维表

![image-20210504112051612](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Database\Redis-notes.assets\image-20210504112051612.png)



- **HSET key field value**

  将哈希表 key 中的域 field 的值设为 value 。

  如果 key 不存在，⼀个新的哈希表被创建并进⾏ HSET 操作。

  如果域 field 已经存在于哈希表中，旧值将被覆盖。

- **HGET** 返回哈希表 key 中给定域 field 的值。如果不存在，返回nil

- **HEXISTS key field**

  查看哈希表 key 中，给定域 field 是否存在。

- **HGETALL key**

  返回哈希表 key 中，所有的域和值。

  在返回值⾥，紧跟每个域名(field name)之后是域的值(value)，所以返回值的⻓度是哈希表⼤⼩的两倍。

- **HKEYS key**

  返回哈希表 key 中的所有值。

- HLEN

  返回哈希表 key 中值的数量。

- **HVALS key**

  返回哈希表 key 中所有域的值。HINCRBY

- **HINCRBY key field increment**

  为哈希表 key 中的域 field 的值加上增量 increment 。

  增量也可以为负数，相当于对给定域进⾏减法操作。

  如果 key 不存在，⼀个新的哈希表被创建并执⾏ HINCRBY 命令。

  如果域 field 不存在，那么在执⾏命令前，域的值被初始化为 0 。

  对⼀个储存字符串值的域 field 执⾏ HINCRBY 命令将造成⼀个错误。

  本操作的值被限制在 64 位(bit)有符号数字表示之内。

- **HMGET key field [field ...]**

  返回哈希表 key 中，⼀个或多个给定域的值。

  如果给定的域不存在于哈希表，那么返回⼀个 nil 值。

  因为不存在的 key 被当作⼀个空哈希表来处理，所以对⼀个不存在的 key 进⾏ HMGET 操作将

  返回⼀个只带有 nil 值的表。

- **HMSET key field value [field value ...]**

  同时将多个 field-value (域-值)对设置到哈希表 key 中。

  此命令会覆盖哈希表中已存在的域。

  如果 key 不存在，⼀个空哈希表被创建并执⾏ HMSET 操作。

- **HSETNX key field value**

  将哈希表 key 中的域 field 的值设置为 value ，当且仅当域 field 不存在。

  若域 field 已经存在，该操作⽆效。

  如果 key 不存在，⼀个新哈希表被创建并执⾏ HSETNX 命令

  其实也就是不覆盖原来的操作

使用场景：

​	二维表可以用来存储我们的Java对象，存对象的时候key：对象的名字，field：对象成员变量的名字，value：成员变量的值



### 4.4 SET

这个是一个无序的集合，特点是：**无序、不可重复**

![image-20210504112543612](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Database\Redis-notes.assets\image-20210504112543612.png)



- **SADD key member [member ...]**

  将⼀个或多个 member 元素加⼊到集合 key 当中，已经存在于集合的 member 元素将被忽略。

  假如 key 不存在，则创建⼀个只包含 member 元素作成员的集合。

  当 key 不是集合类型时，返回⼀个错误。

- **SMEMBERS key**返回集合 key 中的所有成员。

  不存在的 key 被视为空集合。

- **SISMEMBER key member**

判断 member 元素是否集合 key 的成员。

- **SCARD key**

  返回集合 key 的基数(集合中元素的数量)。

- **SPOP key**

  移除并返回集合中的⼀个随机元素。

  如果只想获取⼀个随机元素，但不想该元素从集合中被移除的话，可以使⽤ *SRANDMEMBER*。

- **SRANDMEMBER key [count]**

  如果命令执⾏时，只提供了 key 参数，那么返回集合中的⼀个随机元素。

​	随机取出 count个元素（不删除）

- **SINTER key [key ...]**

  返回⼀个集合的全部成员，该集合是所有给定集合的交集。

  不存在的 key 被视为空集。

- **SINTERSTORE destination key [key ...]**

  这个命令类似于 *SINTER* 命令，但它将结果保存到 destination 集合，⽽不是简单地返回结果集。

  如果 destination 集合已经存在，则将其覆盖。

- **SUNION key [key ...]**

  返回⼀个集合的全部成员，该集合是所有给定集合的并集。

  不存在的 key 被视为空集。

- **SUNIONSTORE destination key [key ...]**

  这个命令类似于 *SUNION* 命令，但它将结果保存到 destination 集合，⽽不是简单地返回结果集。

  如果 destination 已经存在，则将其覆盖。

- **SDIFF key [key ...]**

  返回⼀个集合的全部成员，该集合是所有给定集合之间的差集。

  不存在的 key 被视为空集。

- **SDIFFSTORE destination key [key ...]**

  这个命令的作⽤和 *SDIFF* 类似，但它将结果保存到 destination 集合，⽽不是简单地返回结果集。

  如果 destination 集合已经存在，则将其覆盖。 destination 可以是 key 本身。

- **SMOVE source destination member**

  将 member 元素从 source 集合移动到 destination 集合。SMOVE 是原⼦性操作。

- **SREM key member [member ...]**

  移除集合 key 中的⼀个或多个 member 元素，不存在的 member 元素会被忽略。

  当 key 不是集合类型，返回⼀个错误。

使用场景：

1. 求共同好友
2. 好友推荐

3. 统计网站的独立IP

### 4.5 SortSET

有序集合。其实和二维表比较类似

![image-20210504143531369](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Database\Redis-notes.assets\image-20210504143531369.png)



- **ZADD key score member [[score member] [score member] ...]**

  将⼀个或多个 member 元素及其 score 值加⼊到有序集 key 当中。

- **ZCARD key**

  返回有序集 key 的基数。

- **ZSCORE key member**

  返回有序集 key 中，成员 member 的 score 值。

  如果 member 元素不是有序集 key 的成员，或 key 不存在，返回 nil 。

- **ZCOUNT key min max**

  返回有序集 key 中， score 值在 min 和 max 之间(默认包括 score 值等于 min 或 max )的

  成员的数量。关于参数 min 和 max 的详细使⽤⽅法，请参考 *ZRANGEBYSCORE* 命令。

- **ZINCRBY key increment member**

  为有序集 key 的成员 member 的 score 值加上增量 increment 。

- **ZRANGE key start stop [WITHSCORES]**

  返回有序集 key 中，指定区间内的成员。

  其中成员的位置按 score 值递增(从⼩到⼤)来排序。

  具有相同 score 值的成员按字典序(lexicographical order )来排列。

- **ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]**

  返回有序集 key 中，所有 score 值介于 min 和 max 之间(包括等于 min 或 max )的成员。有

  序集成员按 score 值递增(从⼩到⼤)次序排列。

  根据指定的分值范围去查找

- ZRANK （排名从0开始）

  **ZRANK key member**

  返回有序集 key 中成员 member 的排名。其中有序集成员按 score 值递增(从⼩到⼤)顺序排列。

- ZREVRANGE

  **ZREVRANGE key start stop [WITHSCORES]**

​	返回有序集 key 中，指定区间内的成员。

​	其中成员的位置按 score 值递减(从⼤到⼩)来排列。

​	具有相同 score 值的成员按字典序的逆序(reverse lexicographical order)排列。

- ZREVRANGEBYSCORE

  **ZREVRANGE key start stop [WITHSCORES]**

  返回有序集 key 中，指定区间内的成员。

- ZREVRANK

  **ZREVRANK key member**

  返回有序集 key 中成员 member 的排名。其中有序集成员按 score 值递减(从⼤到⼩)排序。

  排名以 0 为底，也就是说， score 值最⼤的成员排名为 0 。

  使⽤ *ZRANK* 命令可以获得成员按 score 值递增(从⼩到⼤)排列的排名。

- ZREM

  **ZREM key member [member ...]**

  移除有序集 key 中的⼀个或多个成员，不存在的成员将被忽略。

  当 key 存在但不是有序集类型时，返回⼀个错误。ZREMRANGEBYRANK

- **ZREMRANGEBYRANK key start stop**

  移除有序集 key 中，指定排名(rank)区间内的所有成员。

  区间分别以下标参数 start 和 stop 指出，包含 start 和 stop 在内。

- ZREMRANGEBYSCORE 根据分数区间去删除

  **ZREMRANGEBYSCORE key min max**



使用场景：

1. 可以很方便的去统计排名（实时积分排行榜）
2. 主播排行榜

## 5. Java客户端

Jedis客户端

如何使用Jedis客户端去操作Redis呢？

- 导包

  ```xml
  <dependency>
      <groupId>redis.clients</groupId>
      <artifactId>jedis</artifactId>
      <version>2.9.0</version>
  </dependency>
  ```

- 配置

- 使用

  ```java
  @Test
  public void test01(){
  
      // 配置
      Jedis jedis = new Jedis("127.0.0.1",6379);
  
      // 密码
      jedis.auth("password");
  
  
      jedis.set("zhangfei","张飞");
  
  }
  ```



我们发现，这个Java客户端，基本上API的名字和我们使用Redis命令行操作的命令是一致的

## 6. 图形化界面客户端

[下载地址](https://gitee.com/qishibo/AnotherRedisDesktopManager/releases)

![image-20210504160141648](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Database\Redis-notes.assets\image-20210504160141648.png)



## 7. 内存淘汰策略

我们的Redis是把数据存储在内存中，但是内存资源是有限的，所以当内存不足的时候，我们Redis需要去存储新的数据的时候该怎么办呢？

这就得需要知道不同的Redis的内存淘汰策略

Redis默认给我们提供了8中不同的内存淘汰策略（5.0这个版本以后）

- volatile-lru：从已设置过期时间的数据集中挑选最近最少使⽤的数据淘汰

- lru：least recently used 最近最少使用

- volatile-lfu：从已设置过期的Keys中，删除⼀段时间内使⽤次数最少使用的key

- volatile-ttl：从已设置过期时间的数据集中挑选最近将要过期的数据进⾏淘汰

- volatile-random：从已设置过期时间的数据集中随机选择数据淘汰

- allkeys-lru：从数据集中挑选最近最少使⽤的数据淘汰

- allkeys-lfu：从所有的keys中，删除⼀段时间内使⽤次数最少的key

- allkeys-random：从数据集中随机选择数据淘汰

- no-enviction（驱逐）：禁⽌驱逐数据（不采⽤任何淘汰策略。默认即此配置），内存不⾜时，针对写操作，返回错误信息

  

建议：了解了Redis的淘汰策略之后，在平时使⽤时应尽量主动设置/更新key的expire时间，主动剔除

不活跃的旧数据，有助于提升查询性能

## 8. 常见面试题





