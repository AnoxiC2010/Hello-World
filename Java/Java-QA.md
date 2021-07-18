# HashMap

## 为什么HashMap线程不安全

[为什么HashMap线程不安全 - 简书 (jianshu.com)](https://www.jianshu.com/p/e2f75c8cce01)

put方法源码





# 保证Redis和数据库数据的一致性





# 多线程



## 线程池的参数

[(2条消息) Java线程池七个参数详解_IT小跟班-CSDN博客_线程池的七个参数](https://blog.csdn.net/ye17186/article/details/89467919)

一、corePoolSize 线程池核心线程大小

二、maximumPoolSize 线程池最大线程数量

三、keepAliveTime 空闲线程存活时间

四、unit 空闲线程存活时间单位

五、workQueue 工作队列

新任务被提交后，会先进入到此工作队列中，任务调度时再从队列中取出任务。jdk中提供了四种工作队列：

①ArrayBlockingQueue

②LinkedBlockingQuene

③SynchronousQuene

④PriorityBlockingQueue

六、threadFactory 线程工厂

七、handler 拒绝策略

当工作队列中的任务已到达最大限制，并且线程池中的线程数量也达到最大限制，这时如果有新任务提交进来，该如何处理呢。这里的拒绝策略，就是解决这个问题的，jdk中提供了4中拒绝策略：

①CallerRunsPolicy

该策略下，在调用者线程中直接执行被拒绝任务的run方法，除非线程池已经shutdown，则直接抛弃任务。

②AbortPolicy

该策略下，直接丢弃任务，并抛出RejectedExecutionException异常。

③DiscardPolicy

该策略下，直接丢弃任务，什么都不做。

④DiscardOldestPolicy

该策略下，抛弃进入队列最早的那个任务，然后尝试把这次拒绝的任务放入队列

## ReentrantLock

[ReentrantLock(重入锁)功能详解和应用演示 - takumiCX - 博客园 (cnblogs.com)](https://www.cnblogs.com/takumicx/p/9338983.html)

ReentrantLock是可重入的独占锁。比起synchronized功能更加丰富，支持公平锁实现，支持中断响应以及限时等待等等。可以配合一个或多个Condition条件方便的实现等待通知机制。



# 设计模式

## 单例

双重校验锁实现单例模式

```java
public class Singleton {
    private volatile static Singleton uniqueInstance;
    private Singleton() {
    }
    public static Singleton getUniqueInstance() {
        //先判断对象是否已经实例过，没有实例化过才进⼊加锁代码
        if (uniqueInstance == null) {
            //类对象加锁
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```

# IO

nio	bio	多路复用器	epoll	c10k→资源&性能

int 0x80 中断指令

linux命令 `strace`、`jps`、`cd /proc/`

`strace`命令追踪字节码文件执行过程中对内核的调用

文件描述符（整数）

socket()是2类系统调用

java中net Thread调用了内核的clone，产生了轻量级的进程，克隆的过程中有些文件和内存、堆是共享的

bio模型

![image-20210717233904479](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-QA.assets\image-20210717233904479.png)

fcntl系统调用设置非阻塞

![image-20210718103649666](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-QA.assets\image-20210718103649666.png)

![image-20210718105558643](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-QA.assets\image-20210718105558643.png)

![image-20210718111302642](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-QA.assets\image-20210718111302642.png)

![image-20210718113845996](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-QA.assets\image-20210718113845996.png)

![image-20210718162912131](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-QA.assets\image-20210718162912131.png)

![image-20210718162513026](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-QA.assets\image-20210718162513026.png)

从上面，尤其是看代码，能发现nio其实也是同步的，one by one， 遍历

多路复用器 select poll epoll

select、poll有程序告诉内核需要看哪些路的状态-每次都是，epoll需要在内核开辟看空间-多一个往里add一个

epoll实际上也只是告诉app所关注的事件的状态，需要应用自己取拉去数据，只要需要自己拉取数据，就是同步io模型

上面过程是linux c级别的，但是java netty的多路复用器是靠java的selector实现的

java中的selector是对linux系统的select poll epoll的封装或unix系统的kqueue的封装。

java使用的多路复用器其实是kernel提供的，种类很多，java的selector是如何包装的

java的Selector.open()时有先选择epoll，可以设置-D参数修正

java的Selector.select()>0循环，调用就是调用了内核的select,如果时epoll实现的话就调用的时epoll的wait，这个调用过程完成了内核到jvm空间的事件的拷贝，Selector.selectedKeys()返回`Set<SelectionKey>`把有事件的集合拷贝到java程序员手里，边迭代边remove（内核里的读取完了，下次会追加到JVM的集合，所以要把老的迭代处理的同时remove）

用到epoll的：redis，nginx，java netty

主题：性能、资源、（隔离 为云准备）

windows的iocp是真正的异步io，程序留出空间，内核执行完把数据拷过去



自己的理解，我们为什么要用分布式，由单机发展到多机，不是说机器越多越好，更重要的是为了充分利用单机的性能，这也是为什么要学习计组、操作系统、算法... 所以可以说多机是为单机服务的，或者保障单机的安全性、或者让单机充分发挥潜能

## NIO

Java面试常考的 BIO，NIO，AIO 总结

https://blog.csdn.net/m0_38109046/article/details/89449305



epoll



# Spring

循环依赖

https://www.zhihu.com/question/438247718/answer/1730527725



# MySQL

## MVCC

https://www.jianshu.com/p/8845ddca3b23



MySQL InnoDB的锁和MyISAM的锁有什么区别

MyISAM只支持表锁，InnoDB支持表锁还支持颗粒度更细的行锁，仅对相关记录上锁即可，对于写入操作InnoDB的性能更高。



MySQL相关

https://zhuanlan.zhihu.com/p/388691518

表锁和行锁分两类

shared(S) locks 和 exclusive(X) locks

S锁，共享锁，事务读取记录时获取S锁，允许多个事务同时获取S锁，互相之间不会冲突。

X锁，独占锁，事务在修改记录的时候获取 X 锁，且只允许一个事务获取 X 锁，其它事务需要阻塞等待。

所以 S 锁之间不冲突，X 锁则为独占锁，所以 X 之间会冲突， X 和 S 也会冲突。



## 什么是乐观锁，什么是悲观锁

https://www.jianshu.com/p/d2ac26ca6525



## CAS的ABA问题详解

[CAS的ABA问题详解 - 技术-刘腾飞 - 博客园 (cnblogs.com)](https://www.cnblogs.com/frankltf/p/10554965.html)

ABA问题
在多线程场景下CAS会出现ABA问题，关于ABA问题这里简单科普下，例如有2个线程同时对同一个值(初始值为A)进行CAS操作，这三个线程如下
1.线程1，期望值为A，欲更新的值为B
2.线程2，期望值为A，欲更新的值为B
线程1抢先获得CPU时间片，而线程2因为其他原因阻塞了，线程1取值与期望的A值比较，发现相等然后将值更新为B，然后这个时候出现了线程3，期望值为B，欲更新的值为A，线程3取值与期望的值B比较，发现相等则将值更新为A，此时线程2从阻塞中恢复，并且获得了CPU时间片，这时候线程2取值与期望的值A比较，发现相等则将值更新为B，虽然线程2也完成了操作，但是线程2并不知道值已经经过了A->B->A的变化过程。
ABA问题带来的危害：
小明在提款机，提取了50元，因为提款机问题，有两个线程，同时把余额从100变为50
线程1（提款机）：获取当前值100，期望更新为50，
线程2（提款机）：获取当前值100，期望更新为50，
线程1成功执行，线程2某种原因block了，这时，某人给小明汇款50
线程3（默认）：获取当前值50，期望更新为100，
这时候线程3成功执行，余额变为100，
线程2从Block中恢复，获取到的也是100，compare之后，继续更新余额为50！！！
此时可以看到，实际余额应该为100（100-50+50），但是实际上变为了50（100-50+50-50）这就是ABA问题带来的成功提交。
解决方法： 在变量前面加上版本号，每次变量更新的时候变量的版本号都+1，即A->B->A就变成了1A->2B->3A。



在数据库中，表中加入version字段 或者 时间戳也可以，相比使用其他已有字段能消除aba问题



## 分库分表

看看

分库：使用中间件 mycat sharing-jdbc

分表：单表业务量过大，可以采用按业务分表或者是按时间分表等手段来切分



## 如果 sql 查询很慢，如果排查并优化？



# 强引用、软引用、弱引用、幻象引用

引用、软引用

https://baijiahao.baidu.com/s?id=1629253892215446066&wfr=spider&for=pc



# RocketMQ

## RocketMQ 死信队列 | 消费者出现异常如何处理？

[RocketMQ 死信队列 | 消费者出现异常如何处理？ - HelloJava菜鸟社区](http://www.hellojava.com/a/91820.html)

重试队列
这个时候，消息会进入到 RocketMQ 的重试队列中。

比如说消费者所属的消息组名称为AAAConsumerGroup
其重试队列名称就叫做%RETRY%AAAConsumerGroup
重试队列中的消息过一段时间会再次发送给消费者，如果还是无法正常执行会再次进入重试队列
默认重试16次，还是无法执行，消息就会从重试队列进入到死信队列


死信队列
重试队列中的消息重试16次任然无法执行，将会进入到死信队列
死信队列的名字是 %DLQ%AAAConsumerGroup
死信队列中的消息可以后台开一个线程，订阅%DLQ%AAAConsumerGroup，并不停重试


总结


本文从消费者的业务代码出现异常讲起，介绍了 RocketMQ 的重试队列和死信队列：

代码正常执行返回消息状态为CONSUME_SUCCESS，执行异常返回RECONSUME_LATER
状态为RECONSUME_LATER的消息会进入到重试队列，重试队列的名称为 %RETRY% + ConsumerGroupName；
重试16次消息任然没有处理成功，消息就会进入到死信队列%DLQ% + ConsumerGroupName;



[(2条消息) rocketmq死信队列问题。_leadseczgw01的博客-CSDN博客_rocketmq死信队列](https://blog.csdn.net/leadseczgw01/article/details/107021724)

1.多次消费消息未成功，未进入重试队列。

  检查消息监听器类型，如果类型是MessageListenerOrderly，重复消费并不会进入重试队列，只有类型为MessageListenerConcurrently时才会。

2.无法查询死信队列消息：Can not find Message Queue for this topic, %DLQ%hello-consumer↵See http://rocketmq.apache.org/docs/faq/ for further details。

解决方法：修改死信队列的perm，将死信队列的perm值由2修改为6。



## RocketMQ篇.Topic配置中perm的含义

作用: 设置该 Topic 的读写模式。

```
6：同时支持读写
4：禁写
2：禁读
```

一般情况设置为: 6



# Redis

 Redis 的数据类型有哪几种？

## redis常见应用场景



https://www.jianshu.com/p/40dbc78711c8

```
redis应用场景总结redis平时我们用到的地方蛮多的，下面就了解的应用场景做个总结：

1、热点数据的缓存
由于redis访问速度块、支持的数据类型比较丰富，所以redis很适合用来存储热点数据，另外结合expire，我们可以设置过期时间然后再进行缓存更新操作，这个功能最为常见，我们几乎所有的项目都有所运用。

2、限时业务的运用
redis中可以使用expire命令设置一个键的生存时间，到时间后redis会删除它。利用这一特性可以运用在限时的优惠活动信息、手机验证码等业务场景。

3、计数器相关问题
redis由于incrby命令可以实现原子性的递增，所以可以运用于高并发的秒杀活动、分布式序列号的生成、具体业务还体现在比如限制一个手机号发多少条短信、一个接口一分钟限制多少请求、一个接口一天限制调用多少次等等。

4、排行榜相关问题
关系型数据库在排行榜方面查询速度普遍偏慢，所以可以借助redis的SortedSet进行热点数据的排序。

在奶茶活动中，我们需要展示各个部门的点赞排行榜， 所以我针对每个部门做了一个SortedSet,然后以用户的openid作为上面的username,以用户的点赞数作为上面的score, 然后针对每个用户做一个hash,通过zrangebyscore就可以按照点赞数获取排行榜，然后再根据username获取用户的hash信息，这个当时在实际运用中性能体验也蛮不错的。

5、分布式锁
这个主要利用redis的setnx命令进行，setnx："set if not exists"就是如果不存在则成功设置缓存同时返回1，否则返回0 ，这个特性在俞你奔远方的后台中有所运用，因为我们服务器是集群的，定时任务可能在两台机器上都会运行，所以在定时任务中首先 通过setnx设置一个lock，如果成功设置则执行，如果没有成功设置，则表明该定时任务已执行。 当然结合具体业务，我们可以给这个lock加一个过期时间，比如说30分钟执行一次的定时任务，那么这个过期时间设置为小于30分钟的一个时间 就可以，这个与定时任务的周期以及定时任务执行消耗时间相关。

当然我们可以将这个特性运用于其他需要分布式锁的场景中，结合过期时间主要是防止死锁的出现。

6、延时操作
这个目前我做过相关测试，但是还没有运用到我们的实际项目中，下面我举个该特性的应用场景。 比如在订单生产后我们占用了库存，10分钟后去检验用户是够真正购买，如果没有购买将该单据设置无效，同时还原库存。 由于redis自2.8.0之后版本提供Keyspace Notifications功能，允许客户订阅Pub/Sub频道，以便以某种方式接收影响Redis数据集的事件。 所以我们对于上面的需求就可以用以下解决方案，我们在订单生产时，设置一个key，同时设置10分钟后过期， 我们在后台实现一个监听器，监听key的实效，监听到key失效时将后续逻辑加上。 当然我们也可以利用rabbitmq、activemq等消息中间件的延迟队列服务实现该需求。

7、分页、模糊搜索
redis的set集合中提供了一个zrangebylex方法，语法如下：

ZRANGEBYLEX key min max [LIMIT offset count]

通过ZRANGEBYLEX zset - + LIMIT 0 10 可以进行分页数据查询，其中- +表示获取全部数据

zrangebylex key min max 这个就可以返回字典区间的数据，利用这个特性可以进行模糊查询功能，这个也是目前我在redis中发现的唯一一个支持对存储内容进行模糊查询的特性。

前几天我通过这个特性，对学校数据进行了模拟测试，学校数据60万左右，响应时间在700ms左右，比mysql的like查询稍微快一点，但是由于它可以避免大量的数据库io操作，所以总体还是比直接mysql查询更利于系统的性能保障。


8、点赞、好友等相互关系的存储
Redis set对外提供的功能与list类似是一个列表的功能，特殊之处在于set是可以自动排重的，当你需要存储一个列表数据，又不希望出现重复数据时，set是一个很好的选择，并且set提供了判断某个成员是否在一个set集合内的重要接口，这个也是list所不能提供的。 又或者在微博应用中，每个用户关注的人存在一个集合中，就很容易实现求两个人的共同好友功能。

这个在奶茶活动中有运用，就是利用set存储用户之间的点赞关联的，另外在点赞前判断是否点赞过就利用了sismember方法，当时这个接口的响应时间控制在10毫秒内，十分高效。

9、队列
由于redis有list push和list pop这样的命令，所以能够很方便的执行队列操作。
```



## Redission客户端已经实现的分布式锁

```java
@Autowired
private RedissonClient redissonClient;
...
RLock rLock = redissonClient.getLock(topic + ":" + psId + productId);
rlock.lock();
rlock.unlock();
```



## Redission分布式锁底层实现



## redis自己实现分布式锁

原理 setnx上锁 del解锁



谈redis分布式锁可以聊聊下面的简单实现，然后了了Redission的分布式锁底层使用lua脚本的巧妙之处，最后zookeeper也能实现分布式锁，数据库加锁也可以，拓展描述下分布式锁的这几中实现的优劣

https://blog.csdn.net/jf991804033/article/details/109382683

RedisTemplate分布式锁-加锁/解锁的实现

实现逻辑
通过for循环自旋的方式，判断redis中是否存在锁的缓存，存在则放回true，否则判断获取锁的时间是否超时，超时则返回false。
自旋的判断时间是很快的，设置的超时时间如果太长会占用cpu的时间片处理。

```java
/**
* 获取锁的超时时间
*/
private static final long timeout = 300;

/**
 * 加锁，无阻塞
 * @param key
 * @param expireTime
 * @return
 */
public Boolean lock(String key, long expireTime) {
    String requestId = UUID.randomUUID().toString();
    Long start = System.currentTimeMillis();
    //自旋，在一定时间内获取锁，超时则返回错误
    for (;;){
        //Set命令返回OK，则证明获取锁成功
        Boolean ret = redisTemplate.opsForValue().setIfAbsent( key, requestId, expireTime,
                TimeUnit.SECONDS);
        if (ret) {
            return true;
        }
        //否则循环等待，在timeout时间内仍未获取到锁，则获取失败
        long end = System.currentTimeMillis() - start;
        if (end >= timeout){//少时时间和自旋次数都行作为超时返回的标志
            return false;
        }
    }
}
```

解锁实现
实现逻辑
直接删除锁的缓存即可

```
/**
 * 删除缓存
 *
 * @param key 可以传一个值 或多个
 */
public void del(String... key) {
    if (key != null && key.length > 0) {
        if (key.length == 1) {
            redisTemplate.delete(key[0]);
        } else {
            redisTemplate.delete(CollectionUtils.arrayToList(key));
        }
    }
}
```



## 全局发号器

lua脚本 雪花id



# git

秒杀的这个项目，一般是有几个分支

两个master-dev分支 / 多个 master-dev 以及每个开发人员一个



# Zookeeper

## zookeeper集群奇偶数节点问题

容灾数量

集群模式“ 过半存活即可用 ”的特性：

https://blog.csdn.net/csdn3993023/article/details/100384921

1.成功选举Leader必须要备节点过半,2n和2n-1（n>1）的容错数是一样的都是 n-1  。

 2.集群服务偶数节点也是可以的，偶数容错数和奇数一样，所以没必要浪费一个节点资源。



## 理解zookeeper选举机制

https://www.cnblogs.com/shuaiandjun/p/9383655.html



# JWT

JWT 的实现原理？如何做到安全？

如何做到安全？思路：
1， 生产 JWT 的加密算法和密钥（假如有的话）不要泄漏
2， 使用 HTTPS 传输，防止传输数据被拦截



# tps qps

性能测试：TPS和QPS的区别

https://www.cnblogs.com/uncleyong/p/11059556.html

TPS
TPS：Transactions Per Second，意思是每秒事务数，具体事务的定义，都是人为的，可以一个接口、多个接口、一个业务流程等等。一个事务是指事务内第一个请求发送到接收到最后一个请求的响应的过程，以此来计算使用的时间和完成的事务个数。

以单接口定义为事务为例，每个事务包括了如下3个过程：

　　a.向服务器发请求

　　b.服务器自己的内部处理（包含应用服务器、数据库服务器等）

　　c.服务器返回结果给客户端

　　如果每秒能够完成N次这三个过程，tps就是N；

如果多个接口定义为一个事务，那么，会重复执行abc，完成一次这几个请求，算做一个tps。


QPS
QPS：Queries Per Second，意思是每秒查询率，是一台服务器每秒能够响应的查询次数（数据库中的每秒执行查询sql的次数），显然，这个不够全面，不能描述增删改，所以，不建议用qps来作为系统性能指标。


区别
如果是对一个查询接口（单场景）压测，且这个接口内部不会再去请求其它接口，那么tps=qps，否则，tps≠qps

如果是容量场景，假设n个接口都是查询接口，且这个接口内部不会再去请求其它接口，qps=n*tps

 

jmeter聚合报告中，Throughput是用来衡量请求的吞吐量，也就是tps，tps=样本数/运行时间；我们定义的是tps，不是qps

如果没有定义事务，会把每个请求作为一个事务




个人建议
QPS是Query Per Second，是数据库中的概念，每秒执行条数（查询），被引申到压测中来了，但是不包括插入、更新、删除操作，所以不建议用qps来描述系统整体的性能；

建议用tps，这个t，你可以随意的定义，可以是一个接口，也可以是一个业务流程等等。



# 项目

## 单点登录

这个不够好

![image-20210717163811072](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Java-QA.assets\image-20210717163811072.png)



我自己想的：

基于JWT

网关登录API→用户模块核对数据库用户信息→一致则登录成功→把username和id放到JWT的Payloader中返回给网关→网关把JWT以名为access_token的cookie发给客户端



access_token的过期和续期策略

1.不使用redis和数据库，只用JWT。登录本来只放了username和id，在Payloader中增加两个字段loginTime和expireTime。

2.请求经过拦截器会取出{username, id, loginTime, expireTime}, 到期时间= 登录时间+有效期（validPeriod）

2.1如果 currentTime- expireTime > 0，说明超时没有活动（比如半小时都没点击过）JWT过期，拒绝访问需要登陆的资源 + 把名为access_token的cookie的MaxAge设为0让客户端删除cookie。

2.2如果expireTime  - currentTime <= timeout(超时无活动时间比如30分钟)，说明用户在临界的30分钟内点击了，则重新发放JWT（还是以cookie形式），这个JWT里的信息loginTime不变，expireTime = currentTime + validPeriod，相当于用户在超时JWT的token失效边缘点了下鼠标就算重新登录, 时间会正好延长一个我们设定的 有效期validPeriod



这样即使在临界时间（比如30分钟内）即使最后一秒用户点击了网页，有效期会延长一个新的周期。而且只依靠JWT本身，逻辑上只加了一个当前时间和expireTime的算数运算，效率很高。比如有效期validPeriod有n小时，更新JWT token的间隔为 （n小时-30分钟）-n小时，频率很低。30分钟或我们设定的多少分钟没动过鼠标点击就让他失效即可



这种方式还避免了用户用多个客户端登录的问题，如果放一份登录信息登录时间等放在redis里，那么多个客户端登录会导致信息覆盖。

而且不用查询redis



## 秒杀接口

### 限流

使用令牌桶算法，通过限制令牌的生成速率，进行限流。

线程池，队列泄洪。

redis缓存空库存标记，拦截无效的数据库压力。

### 防刷

- 秒杀开始前使用公式验证码进行验证，可用技术：scriptEngine

- 限制秒杀请求次数

  **使用自定义注解+拦截器实现接口限流防刷**

  [(2条消息) 【商城秒杀项目】-- 接口限流防刷_今日相乐，皆当喜欢的博客-CSDN博客_接口防刷限流](https://blog.csdn.net/weixin_42687829/article/details/104516751)

  接口限流防刷的目的

  限制同一个用户一段时间之内只能访问固定次数，对系统做一层保护

  实现思路

  利用缓存实现，当用户每次点击访问接口的时候，在缓存中生成一个计数器，第一次请求的时候将这个计数器计数为1后存入缓存，并给其设定有效期，比如一分钟，如果一分钟之内再访问，那么数值加一；一分钟之内访问次数超过限定数值，就直接返回“访问过于频繁”；等到下一个一分钟，数据又重新从0开始计算，因为给缓存设定了一个有效期，所以一分钟之后会自动失效

  获取访问路径
  拼接用户的id作为一个记录该用户访问次数的key
  从缓存里面取得该key做判断（以指定时间之内只能访问5次为例），如果在缓存里面没有取到，代表是第一次访问，然后给缓存设置该key，并设置初始value值为1；如果从缓存里面取得值并且小于5，那么直接将该key对应的value值+1；如果从缓存里面取的值>=5，那么代表在限制时间内访问次数达到限制

  

- 隐藏秒杀接口，动态获取秒杀路径：

  路径可以使用localhost:8081/miaosha/{path}/doMiaosha的方式，{path}则通过动态方式生成，生成逻辑：UUID+盐值 然后使用MD5加密，同时保存至redis中一份：key使用用户id+商品id。

  

  ## Redis 和 Mysql 数据库数据如何保持一致性

  延时双删

  https://www.jianshu.com/p/2edbb48604bd

  读取缓存步骤一般没有什么问题，但是一旦涉及到数据更新：数据库和缓存更新，就容易出现缓存和数据库间的数据一致性问题。不管是先写数据库，再删除缓存；还是先删除缓存，再写库，都有可能出现数据不一致的情况。举个例子：

          1.如果删除了缓存Redis，还没有来得及写库MySQL，另一个线程就来读取，发现缓存为空，则去数据库中读取数据写入缓存，此时缓存中为脏数据。
      
          2.如果先写了库，在删除缓存前，写库的线程宕机了，没有删除掉缓存，则也会出现数据不一致情况。
      
          因为写和读是并发的，没法保证顺序,就会出现缓存和数据库的数据不一致的问题。如何解决？这里给出两个解决方案，先易后难，结合业务和技术代价选择使用。

  一、 延时双删策略
          在写库前后都进行redis.del(key)操作，并且设定合理的超时时间。具体步骤是：

          1）先删除缓存
      
          2）再写数据库
      
          3）休眠500毫秒（根据具体的业务时间来定）
      
          4）再次删除缓存。
      
          那么，这个500毫秒怎么确定的，具体该休眠多久呢？
      
          需要评估自己的项目的读数据业务逻辑的耗时。这么做的目的，就是确保读请求结束，写请求可以删除读请求造成的缓存脏数据。
      
          当然，这种策略还要考虑 redis 和数据库主从同步的耗时。最后的写数据的休眠时间：则在读数据业务逻辑的耗时的基础上，加上几百ms即可。比如：休眠1秒。

  二、设置缓存的过期时间
          从理论上来说，给缓存设置过期时间，是保证最终一致性的解决方案。所有的写操作以数据库为准，只要到达缓存过期时间，则后面的读请求自然会从数据库中读取新值然后回填缓存

          结合双删策略+缓存超时设置，这样最差的情况就是在超时时间内数据存在不一致，而且又增加了写请求的耗时。

  三、如何写完数据库后，再次删除缓存成功？
          上述的方案有一个缺点，那就是操作完数据库后，由于种种原因删除缓存失败，这时，可能就会出现数据不一致的情况。这里，我们需要提供一个保障重试的方案。

  1、方案一具体流程

          （1）更新数据库数据； 
          
          （2）缓存因为种种问题删除失败；
      
          （3）将需要删除的key发送至消息队列；
      
          （4）自己消费消息，获得需要删除的key；
      
          （5）继续重试删除操作，直到成功。
      
          然而，该方案有一个缺点，对业务线代码造成大量的侵入。于是有了方案二，在方案二中，启动一个订阅程序去订阅数据库的binlog，获得需要操作的数据。在应用程序中，另起一段程序，获得这个订阅程序传来的信息，进行删除缓存操作。 

  2、方案二具体流程
          （1）更新数据库数据；

          （2）数据库会将操作信息写入binlog日志当中；
      
          （3）订阅程序提取出所需要的数据以及key； 
      
          （4）另起一段非业务代码，获得该信息；
      
          （5）尝试删除缓存操作，发现删除失败； 
      
          （6）将这些信息发送至消息队列；
      
          （7）重新从消息队列中获得该数据，重试操作。

  
