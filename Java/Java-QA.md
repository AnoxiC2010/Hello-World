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



# NIO

Java面试常考的 BIO，NIO，AIO 总结

https://blog.csdn.net/m0_38109046/article/details/89449305



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
