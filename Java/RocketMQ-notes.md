# RocketMq notes

# 简介

消息中间件是什么？

中间件：顾名思义 介于两者之间的⼀个技术

A-------------中间-------------B

消息中间件：消息中间件利⽤⾼效可靠的消息传递机制进⾏平台⽆关的数据交流，并基于数据通信来进⾏分布式系统的集成。



RocketMQ是什么？

RocketMQ是阿⾥巴巴开源的⼀个消息中间件，是⼀个队列模型的消息中间件，具有⾼性能、⾼可靠、⾼实时、分布式特点。⽬前已贡献给apache



参考阅读链接：[RocketMQ的前世今⽣](https://yq.aliyun.com/articles/66129)



# 功能

## 异步化

将⼀些可以进⾏异步化的操作通过发送消息来进⾏异步化，提⾼效率

具体场景：⽤户为了使⽤某个应⽤，进⾏注册，系统需要发送注册邮件并验证短信。对这两个操作的处理⽅式有两种：串⾏及并⾏。

1. 串⾏⽅式：新注册信息⽣成后，先发送注册邮件，再发送验证短信

   ![image-20210621230907185](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\RocketMQ-notes.assets\image-20210621230907185.png)

   在这种⽅式下，需要最终发送验证短信后再返回给客户端。

2. 并⾏处理：新注册信息写⼊后，发短信和发邮件并⾏处理

   ![image-20210621231007849](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\RocketMQ-notes.assets\image-20210621231007849.png)

   在这种⽅式下，发短信和发邮件 需处理完成后再返回给客户端。
   假设以上三个⼦系统处理的时间均为50ms，且不考虑⽹络延迟，则总的处理时间：
   串⾏：50 + 50 + 50 = 150ms 并⾏：50 + 50 = 100ms

3. 使⽤消息队列

   ![image-20210621231100584](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\RocketMQ-notes.assets\image-20210621231100584.png)

   并在写⼊消息队列后⽴即返回成功给客户端，则总的响应时间依赖于写⼊消息队列的时间，⽽写⼊消息队列的时间本身是可以很快的，基本可以忽略不计，因此总的处理时间相⽐串⾏提⾼了2倍，相⽐并⾏提⾼了⼀倍；



## 限流削峰

在⾼并发场景下把请求存⼊消息队列，利⽤排队思想降低系统瞬间峰值
具体场景：购物⽹站开展秒杀活动，⼀般由于瞬时访问量过⼤，服务器接收过⼤，会导致流量暴增，相关系统⽆法处理请求甚⾄崩溃。⽽加⼊消息队列后，系统可以从消息队列中取数据，相当于消息队列做了⼀次缓冲。

![image-20210621231232743](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\RocketMQ-notes.assets\image-20210621231232743.png)

优点：
1. 请求先⼊消息队列，⽽不是由业务处理系统直接处理，做了⼀次缓冲,极⼤地减少了业务处理系统的压⼒；
2. 队列⻓度可以做限制，事实上，秒杀时，后⼊队列的⽤户⽆法秒杀到商品，这些请求可以直接被抛弃，返回活动已结束或商品已售完信息；



对⽐

消息中间件不仅仅只有RocketMQ，市⾯上还有很多其他的消息中间件，这⾥列举⼏个常⻅的和
RocketMQ作为⼀个对⽐
ActiveMQ：ActiveMQ 是Apache出品，最流⾏的，能⼒强劲的开源消息总线。ActiveMQ 是⼀个完全
⽀持JMS1.1和J2EE 1.4规范的 JMS Provider实现。

```
JMS: 全称是Java Message Service,即消息服务应⽤程序接⼝，是⼀个Java⾯向消息中间件平台的API，⽤于在两个应⽤程序之间，或分布式系统中发送消息，进⾏异步通信
```

备注：最早的，可能会丢消息，社区不活跃

RabbitMQ：AMQP协议的领导实现，⽀持多种场景。淘宝的MySQL集群内部有使⽤它进⾏通讯，OpenStack开源云平台的通信组件，最先在⾦融⾏业得到运⽤。

```
AMQP: 即Advanced Message Queuing Protocol，⼀个提供统⼀消息服务的应⽤层标准⾼级消息队列协议，是应⽤层协议的⼀个开放标准，为⾯向消息的中间件设计
```

备注：社区活跃，控制界面友好，运维友好

Kafka: Kafka是最初由Linkedin公司开发，是⼀个分布式、⽀持分区的（partition）、多副本的（replica），基于zookeeper协调的分布式消息系统，它的最⼤的特性就是可以实时的处理⼤量数据以满⾜各种需求场景：⽐如基于hadoop的批处理系统、低延迟的实时系统、storm/Spark流式处理引擎，web/nginx⽇志、访问⽇志，消息服务等等，⽤scala语⾔编写，Linkedin于2010年贡献给了Apache基⾦会并成为顶级开源 项⽬。

备注：大数据标杆，但比较RocketMQ功能单一

![image-20210621233043331](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\RocketMQ-notes.assets\image-20210621233043331.png)



# 模型

## 相关概念

- Producer： 消息⽣产者，负责消息的产⽣，由业务系统负责产⽣
- Consumer：消息消费者，负责消息消费，由后台业务系统负责异步消费
- Topic：消息的逻辑管理单位

这三者是RocketMq中最最基本的概念。Producer是消息的⽣产者。Consumer是消息的消费者。
消息通过Topic进⾏传递。Topic存放的是消息的逻辑地址。

![image-20210621233400622](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\RocketMQ-notes.assets\image-20210621233400622.png)

具体来说是Producer将消息发往具体的Topic。Consumer订阅Topic，主动拉取或被动接受消
息，如果Consumer消费消息失败则默认会重试16次

```
重连每次时间不一样，越来越长，16次要4个多小时
```



## 概念模型

![image-20210621233603883](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\RocketMQ-notes.assets\image-20210621233603883.png)

- Broker: 消息的中转⻆⾊，负责存储消息，转发消息，⼀般也称为server，可以理解为⼀个存放消
  息的服务，⾥⾯可以有多个Topic
- MessageQueue: 消息的物理管理单位，⼀个Topic下有多个Queue，默认⼀个Topic创建时会创建
  四个MessageQueue
- ConsumerGroup: 具有同样消费逻辑消费同样消息的Consumer，可以归并为⼀个group
- ProducerGroup: 具有同样属性的⼀些Producer可以归并为同⼀个Group
  同样属性是指：发送同样Topic类型的消息
- Nameserver 注册中⼼
  作⽤：
  每个Broker启动的时候会向namesrv注册
  Producer发送消息的时候根据Topic获取路由到Broker⾥⾯Topic的信息
  Consumer根据Topic到Namesrv 获取topic的路由到Broker的信息



## 部署模型

![image-20210621234436696](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\RocketMQ-notes.assets\image-20210621234436696.png)

1. 注册中⼼Nameserver启动
2. 消息中转服务Broker启动

  - 启动的时候会去创建Topic并创建对应的MessageQueue

  - 启动的时候会去注册中⼼注册，把⾃⼰的地址以及负责的Topic告诉注册中⼼

  - Broker和Nameserver之间通过⼼跳机制来检测对⽅是否存活

    ```
    连接: 单个broker和所有nameserver保持⻓连接
    ⼼跳:
     ⼼跳间隔：每隔30秒（此时间⽆法更改）向所有nameserver发送⼼跳，⼼跳包含了⾃身的
    topic配置信息。
     ⼼跳超时：nameserver每隔10秒钟（此时间⽆法更改），扫描所有还存活的broker连接，
    若某个连接2分钟内（当前时间与最后更新时间差值超过2分钟，此时间⽆法更改）没有发送⼼跳
    数据，则断开连接。
    ```

3. 消息⽣产者Produer启动
   启动的时候会去注册中⼼注册

   注册那些内容呢？
   1. 把⾃⼰的IP地址告诉注册中⼼
   2. 把⾃⼰⽣产的Topic告诉注册中⼼

   运⾏时：

   - 单个⽣产者者和⼀台nameserver保持⻓连接，定时查询topic配置信息，如果该
     nameserver挂掉，⽣产者会⾃动连接下⼀个nameserver，直到有可⽤连接为⽌，并能
     ⾃动重连。
   - 单个⽣产者和该⽣产者关联的所有broker保持⻓连接。
   - 默认情况下，⽣产者每隔30秒从nameserver获取所有topic的最新队列情况，这意味着
     某个broker如果宕机，⽣产者最多要30秒才能感知，在此期间，发往该broker的消息发
     送失败。该时间可⼿动配置
   - 默认情况下，⽣产者每隔30秒向所有broker发送⼼跳，该时间由DefaultMQProducer
     的heartbeatBrokerInterval参数决定，可⼿动配置。broker每隔10秒钟（此时间⽆法更
     改），扫描所有还存活的连接，若某个连接2分钟内（当前时间与最后更新时间差值超
     过2分钟，此时间⽆法更改）没有发送⼼跳数据，则关闭连接

4. 消息消费者Consumer启动
   启动的时候会去注册中⼼注册
   注册哪些内容呢？

   1. 把⾃⼰的IP地址告诉注册中⼼

   2. 把⾃⼰可以消费的Topic告诉注册中⼼

   运⾏时：

   - 单个消费者和⼀台nameserver保持⻓连接，定时查询topic配置信息，如果该
     nameserver挂掉，消费者会⾃动连接下⼀个nameserver，直到有可⽤连接为⽌，并能
     ⾃动重连。
   - 单个消费者和该消费者关联的所有broker保持⻓连接。
   - 默认情况下，消费者每隔30秒从nameserver获取所有topic的最新队列情况，这意味着
     某个broker如果宕机，客户端最多要30秒才能感知。该时间由
     DefaultMQPushConsumer的pollNameServerInteval参数决定，可⼿动配置。
   - 默认情况下，消费者每隔30秒向所有broker发送⼼跳，该时间由
     DefaultMQPushConsumer的heartbeatBrokerInterval参数决定，可⼿动配置。
     broker每隔10秒钟（此时间⽆法更改），扫描所有还存活的连接，若某个连接2分钟内
     （当前时间与最后更新时间差值超过2分钟，此时间⽆法更改）没有发送⼼跳数据，则
     关闭连接，并向该消费者分组的所有消费者发出通知，分组内消费者重新分配队列继续
     消费

## 注意事项

- 同步刷盘与异步刷盘：
  RocketMQ的消息是存储到磁盘上的，这样既能保证断电后恢复，⼜可以让存储的消息量超出内存的限制。
  RocketMQ为了提⾼性能，会尽可能地保证磁盘的顺序写。消息在通过Producer写⼊RocketMQ的时候，有两种写磁盘⽅式：

  - 异步刷盘：在返回写成功状态时，消息可能只是被写⼊了内存中，写操作的返回快

    吞吐量⼤；当内存⾥的消息量积累到⼀定程度时，统⼀触发写磁盘操作，快速写⼊

  - 同步刷盘：在返回写成功状态时，消息已经被写⼊磁盘。具体流程是，消息写⼊内存后，⽴刻通知刷盘线程刷盘，然后等待刷盘完成，刷盘线程执⾏完成后唤醒等待的线程，返回消息写成功的状态。

  同步刷盘还是异步刷盘，是通过Broker配置⽂件⾥的flushDiskType参数设置的，这个参数被设置成SYNC_FLUSH、ASYNC_FLUSH中的⼀个

- 同步复制与异步复制
  如果⼀个broker组有Master和Slave，消息需要从Master复制到Slave上，有同步和异步两种复制⽅式。
  同步复制是等Master和Slave均写成功后才反馈给客户端写成功状态；异步复制⽅式是只要Master写成功即可反馈给客户端写成功状态
  同步复制和异步复制是通过Broker配置⽂件⾥的brokerRole参数进⾏设置的，这个参数可以被设置成ASYNC_MASTER、SYNC_MASTER中的⼀个。

刷盘策略，钱有关的要 ；主从复制策略，钱有关的。。。

## 安装使⽤

下载

官⽹地址：http://rocketmq.apache.org
下载地址：http://rocketmq.apache.org/release_notes/release-notes-4.4.0/

![image-20210622000055295](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\RocketMQ-notes.assets\image-20210622000055295.png)

 安装

1. 按照上述步骤，下载，解压
2. 注意事项：
a) RocketMQ 是基于Java开发的，所以需要配置相应的jdk环境变量才能运⾏
b) RocketMQ也需要配置环境变量才可在windows上运⾏



系统环境变量

ROCKETMQ_HOME 

C:\Programming\rocketmq-all-4.5.1-bin-release

PATH 

%ROCKETMQ_HOME %\bin



ROCKETMQ_HOME 

准备⼯作
⽬的：为了防⽌出现内存不⾜的情况
1. 修改配置：进⼊bin⽬录

   runbroker.com和runserver.com默认设置为

   -Xms2g -Xmx2g -Xmn1g

   笔记本可能启动不了，服务器会设置比较大的内存

   ![image-20210622001043903](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\RocketMQ-notes.assets\image-20210622001043903.png)

   修改NameServer和Broker配置：（windows/Linux）
   （如果是Linux环境则对应的是 runbroker.sh 和runserver.sh)
   编辑runbroker.cmd⽂件，修改如下：

   -Xms512m -Xmx512m -Xmn256m 即可

   ![image-20210622083934214](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\RocketMQ-notes.assets\image-20210622083934214.png)

   ```
   参数解释：
    - Xms: 是指程序启动时初始内存⼤⼩（此值可以设置成与-Xmx相同，以避免每次GC完成后 JVM
   内存重新分配）
    - Xmx: 指程序运⾏时最⼤可⽤内存⼤⼩，程序运⾏中内存⼤于这个值会 OutOfMemory
    - Xmn: 年轻代⼤⼩（整个JVM内存⼤⼩ = 年轻代 + 年⽼代 + 永久代）
   ```

   编辑runserver.cmd⽂件，修改如上图⼀样



启动

1. ⾸先启动注册中⼼nameserver ，默认启动在9876端⼝，打开cmd命令窗⼝，进⼊bin⽬录，执⾏
  命令：

  ```cmd
  start mqnamesrv.cmd
  ```

  Linux则执⾏

  ```bash
  sh ./mqnamesrv
  ```

  出现以下⽇志表示启动成功

  ![image-20210622084148534](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\RocketMQ-notes.assets\image-20210622084148534.png)

2. 启动RocketMQ服务，也就是broker
  进⼊bin⽬录，执⾏命令：

  Windows

  ```cmd
  start mqbroker.cmd -n 127.0.0.1:9876 autoCreateTopicEnable=true
  ```

  启动参数加 `-c ../conf/broker.conf`使用配置文件启动

  Linux

  ```bash
  sh ./mqbroker -n 127.0.0.1:9876 autoCreateTopicEnable=true
  ```

  补充命令

  ```cmd
  jps # 查看Java进程，是JDK给我们提供的⼀个⼯具命令
  jps -l # 详细
  jps -v # 含启动参数信息
  ```

  

# 整合

## 导包

```xml
<dependency>
    <groupId>org.apache.rocketmq</groupId>
    <artifactId>rocketmq-client</artifactId>
    <version>4.4.0</version>
</dependency>
```

## 消息⽣产者实现

```java
public static void main(String[] args){
    // 新增消息⽣产者
    DefaultMQProucer producer = new DefaultMQProucer("producer_group");
    // 配置注册中⼼
    producer.setNamesrvAddr("localhost:9876");
    // 启动
    producer.start();
    // 新建消息对象
    Message message = new
        Message("topicA","message".context.getBytes(Charset.forName("utf-8")));
    // 发送消息
    producer.send(message);
}

@Component//可以使用Spring容器管理
public class DeylayOrderProducer {
    private DefaultMQProducer producer;
    //可以使用Spring配置文件注入属性
	private String namesrvAddr;
    private String producerGroup;
    private String topic;
    private int delayTimeLevel;
    //可以让spring生成实例的时候初始化并启动，参数来自配置文件
    @PostConstruct
    public voit init() {
        // 创建Producer对象
        producer = new DefaultMQProducer("delay_producer");

        // 设置连接的nameserver地址
        producer.setNamesrvAddr("127.0.0.1:9876");

        try {
            producer.start();
        } catch (MQClientException e) {
            e.printStackTrace();
        }
    }
    //外部Autowired之后可以直接调用的方法
    public voit sendDelayOrderMessage(Stirng orderId) {
         // 发送消息
        Message message = new Message();
        message.setTopic("delay_order");
        byte[] body = orderId.getBytes(Charset.forName("utf-8"));
        message.setBody(body);
        // 发送延迟消息, 消息被延迟消费的时间(只能指定时间延迟的级别)
        message.setDelayTimeLevel(2);

        // 为了验证延迟消息消费时间，测试
        message.putUserProperty("startTime", System.currentTimeMillis() +  "");

        SendResult sendResult = null;
        Exception ex = null;
        try {
            sendResult = producer.send(message);
        } catch (MQClientException e) {
            e.printStackTrace();
        } catch (RemotingException e) {
            e.printStackTrace();
        } catch (MQBrokerException e) {
            e.printStackTrace();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        if (sendResult != null && SendStatus.SEND_OK.equals(sendResult.getSendStatus())) {
            log.info("消息发送成功： msgId = " + sendResult.getMsgId());
            return;
        }

        log.info("消息发送失败");
    }
}

```

延迟消息级别

```
private String messageDelayLevel = "1s 5s 10s 30s 1m 2m 3m 4m 5m 6m 7m 8m 9m 10m 20m 30m 1h 2h";
                                    1  2   3   4                                      16
```

## 消息消费者实现

```java
public static void main(String[] args) throws MQClientException {
    DefaultMQPushConsumer mqConsumer = new
        DefaultMQPushConsumer("consumer_group");
    mqConsumer.setNamesrvAddr("localhost:9876");
    mqConsumer.subscribe("topicA", "*");
    // 设置消息监听器
    mqConsumer.registerMessageListener(new MessageListenerConcurrently() {
        @Override
        public ConsumeConcurrentlyStatus consumeMessage(List<MessageExt>
                                                        msgs, ConsumeConcurrentlyContext context) {
            MessageExt message = msgs.get(0);
            //获取消息内容
            byte[] body = message.getBody();
        }
    });
    mqConsumer.start();
}
/*
      消费订单延迟消息的消费者：
      1. 从获取到的消息中，拿出订单编号
      2. 查询tb_order表中，对应的订单的状态， 如果订单状态是已支付，就什么都不做
      3. 如果订单的状态是init，这个时候说明订单还没有被支付，我们的就需要取消订单
         a. 修改订单状态
         b. 还原商品的冻结库存
         c. 修改订单条目的库存状态为 库存已释放
 */
@Slf4j
@Component//可以用spring容器管理
public class DelayConsumer {
	private DefaultMQPushConsumer consumer;
    //可以使用Spring配置文件注入属性
	private String namesrvAddr;
    private String consumerGroup;
    private String topic;

    @PostConstruct//让spring创建实例时初始化
    public void init() {
        // 创建一个用来接收消息的Consumer对象
        consumer = new DefaultMQPushConsumer("delay_consumer");

        // 设置Consumer连接的nameserver的地址
        consumer.setNamesrvAddr("127.0.0.1:9876");

        // 消费者Consumer订阅某个主题的消息
        consumer.subscribe("test_delay", "*");//*表示全部，不过滤

        // 定义消费者，获取到消息之后的消费逻辑
        //这里是 取消订单的业务实现
        consumer.setMessageListener(new MessageListenerConcurrently() {
            @Override
            public ConsumeConcurrentlyStatus consumeMessage(List<MessageExt> msgs, ConsumeConcurrentlyContext context) {
                // 在该方法执行过程中如果出现了异常
                try {
                    // 实现消费逻辑

                    // 因为我们知道，我们一次只发送一个消息，一次只取一个消息
                    MessageExt message = msgs.get(0);
					//时间级别的测试，结果基本符合设定
                    String startTimeStr = message.getUserProperty("startTime");
                    long startTime = Long.parseLong(startTimeStr);
                    long endTime = System.currentTimeMillis();


                    // 获取消息体中，封装的数据
                    byte[] body = message.getBody();

                    // 按照消费逻辑处理获取到的数据
                    String s = new String(body, 0, body.length, Charset.forName("utf-8"));
                    System.out.println("接收到了消息：" + message.getMsgId() + ":" + s + "耗时：" + (endTime - startTime));
					log.("...")
                    return ConsumeConcurrentlyStatus.CONSUME_SUCCESS;
                } catch (Exception e) {
                    e.printStackTrace();
                    log.("...")
                  return ConsumeConcurrentlyStatus.RECONSUME_LATER;
                }
            }
        });

		
        // 启动消息消费者
        consumer.start();//有编译异常可以全部捕获下
    }
}

```





## 补充：

如果写完代码之后抛出以下错误

```
org.apache.rocketmq.client.exception.MQClientException: No route info of this
topic, xxx
```

那么表示你的rocketMQ中没有创建这个topic,表示开启autoCreateTopicEnable失效了，这个时候需要
⼿动创建topic，还是进⼊bin⽬录，执⾏命令：

```bash
sh ./mqadmin updateTopic -n localhost:9876 -b localhost:10911 -t topicName
```

然后重新启动⼀下程序即可。