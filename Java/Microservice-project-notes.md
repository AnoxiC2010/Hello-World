# 微服务项目

```
com.mysql.cj.jdbc.Driver
```



日志

lombok的

@Sl4j注解提供log属性

线程debug模式

swagger注解

select for update

CollectionUtils springframeword的

全局id生成器 （发号器 实现生成唯一id的工具）

JWT 生成与取出

图片验证码

防止跨域问题的配置

redis集群 

项目微服务集群

消息中间件集群

数据库分表

## java发送邮件



sun公司给我们提供了Java发送邮件的解决⽅案，并且提供了jar包

```xml
<dependency>
    <groupId>javax.mail</groupId>
    <artifactId>mail</artifactId>
</dependency>
```



⽽Springboot项⽬则给我们提供了封装了上⾯mail包的接⼝，让我们在Springboot项⽬中能够更⽅便的 实现邮件发送

- 导包

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-mail</artifactId>
  </dependency>
  ```

- 配置

  ```properties
  #邮件服务器
  spring.mail.host=smtp.163.com
  #端⼝
  spring.mail.port=25
  #发送邮件邮箱⽤户名
  spring.mail.username=ciggarnot@163.com
  #发送邮件邮箱密码（这个密码是上⾯的授权码，不是邮箱登录的密码）
  spring.mail.password=SNTGFTIEAJUMRHCT
  #设置邮箱认证打开
  spring.mail.properties.mail.smtp.auth=true
  ```

  

- 代码编写

  ```java
  @Autowired
  private JavaMailSender javaMailSender;
  @Test
  public void sendMail(){
      SimpleMailMessage message = new SimpleMailMessage();
      message.setFrom("ciggarnot@163.com");
      message.setTo("291136733@qq.com");
      message.setSubject("csmall商城激活2");
      message.setText("http://localhost:8080/user/verify?uid=akjsdnh39udklnw");
      javaMailSender.send(message);
  /*
  javaMailSender.send(MimeMessage mimeMessage)发送有附件的邮件
  javaMailSender.send(SimpleMailMessage simpleMailMessage)发送简单的邮件对象
  */
  }
  ```




## 下单

买家留言暂不实现

评价暂不实现

unique key唯一键 代替订单id暴漏给外部



商品库存售卖id 不同区域有不同的库存

![image-20210619102039646](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Microservice-project-notes.assets\image-20210619102039646.png)





![image-20210619112009978](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Microservice-project-notes.assets\image-20210619112009978.png)

pipeline采用工厂方法来创建

![image-20210619112152271](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Microservice-project-notes.assets\image-20210619112152271.png)

这个工厂方法

![image-20210619114159615](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Microservice-project-notes.assets\image-20210619114159615.png)

![image-20210619144623741](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Microservice-project-notes.assets\image-20210619144623741.png)

# Pipeline设计模式

- 简介

  Pipeline模式⼜称为流⽔线模式，pipeline⼜称为管道，是⼀种在计算机普遍使⽤的技术。举个最普遍的例⼦，如下图所示cpu流⽔线，⼀个流⽔线分为4部分，每个部分可以独⽴⼯作，于是可以处理多个数据流。linux 管道也是⼀个常⽤的管道技术，其字符处理功能⼗分强⼤，在⾯试过程中常会被问到。在分布式处理领域，由于管道模式是数据驱动，⽽⽬前流⾏的Spark分布式处理平台也是数据驱动的，两者⾮常合拍，于是在spark的新的api⾥⾯pipeline模式得到了⼴泛的应⽤。还有java web中的struct的filter、netty的pipeline，⽆处不⻅pipeline模式。
  Netty：NIO框架，Dubbo底层调⽤的时候就是⽤的netty框架（Mina）

- 解决的问题：

  有时⼀些线程的步骤⽐较冗⻓，⽽且由于每个阶段的结果与下阶段的执⾏有关系，⼜不能分开

- 解决思路

  可以将任务的处理分解为若⼲个处理阶段，上⼀个阶段任务的结果交给下⼀个阶段来处理，这样每个线程的处理是并⾏的，可以充分利⽤资源提⾼计算效率。



### 概念

管道模型包含两个部分：pipeline 管道、valve 阀⻔（也称为handler）、Context（返回结果的上下⽂）。

pipeline 管道，可以⽐作⻋间⽣产线，在这⾥可认为是容器的逻辑处理总线。
valve 阀⻔，可以⽐作⽣产线上的⼯⼈，负责完成各⾃的部分⼯作。 阀⻔也可以叫做Handler 处理者

![image-20210620221703975](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Microservice-project-notes.assets\image-20210620221703975.png)



### 接⼝代码建模

Handler接⼝

```JAVA
public interface Handler {
    Boolean handle(MyContext context);
}
```

操作节点单元

```JAVA
@Data
public class HandlerNode {

    private Handler handler;

    private HandlerNode next;

    public void exec(MyContext context){
        Boolean ret = handler.handle(context);
        if (ret) {
            if (next != null) {
                next.exec(context);
            }
        }
    }
}
```

上下⽂环境

```JAVA
@Data
public class PipelineContext {
 String orderId;
}
```

管道接⼝

```java
/**
* 管道
* 其中的"PipelineContext"俗称“上下⽂”，
* 代表从开始贯穿到结尾的⼀个执⾏环境，⽤于在各个Pipeline之间传递信息；
* ⼀个“Pipeline”包含两个⽅法，
* process代表当前的处理流程，
* forward⽅法代表将处理消息转发给下游的流程：上游可以控制消息是否转发给下游。
*/
public interface Pipeline {
 /**
 * 启动流程
 */
 void start();
 /**
 * 终⽌流程
 */
 void shutdown();
 /**
 * 获取返回值
 */
 PipelineContext getContext();
 /**
 * 添加到头部
 * @param handlers
 */
 void addFirst(Handler ... handlers);
 /**
 * 添加到尾部
 */
 void addLast(Handler ... handlers);
}
```

管道对象实现

```java
public class OrderPipeline implements Pipeline {
    //尾部节点
    private OrderHandlerNode tail;
    //头部节点
    private OrderHandlerNode head = new OrderHandlerNode();
    // 上下⽂环境
    private PipelineContext context = null;
    public OrderPipeline(PipelineContext context) {
        this.context = context;
    }
    /**
 * 添加到队⾸
 * @param handlers
 */
    public void addFirst(Handler... handlers) {
        OrderHandlerNode pre = head.getNext();
        for (Handler handler : handlers) {
            if (handler == null) {
                continue;
            }
            OrderHandlerNode orderHandlerNode = new OrderHandlerNode();
            orderHandlerNode.setHandler(handler);
            orderHandlerNode.setNext(pre);
            pre = orderHandlerNode;
        }
        head.setNext(pre);
    }
    /**
 * 添加到队尾
 * @param handlers
 */
    public void addLast(Handler... handlers) {
        OrderHandlerNode next = tail;
        for (Handler handler : handlers) {
            if (handler == null) {
                continue;
            }
            OrderHandlerNode orderHandlerNode = new OrderHandlerNode();
            orderHandlerNode.setHandler(handler);
            next.setNext(orderHandlerNode);
            next = orderHandlerNode;
        }
        tail = next;
    }
    /**
 * 启动流程
 */
    public void start() {
        head.getNext().exec(getContext());
    }
    /**
 * 获取context
 * @param
 * @return
 */
    public PipelineContext getContext() {
        return context;
    }
}
```

注意事项

1. 阀⻔的实现分两种，即普通阀⻔和尾阀⻔。普通阀⻔在处理完⾃⼰的事情之后，必须调⽤
getNext.invoke(s)⽅法，也就是交给下⼀个阀⻔处理；⽽尾阀⻔不⽤调⽤这个⽅法，因为它是结束的那个阀⻔。
2. 流⽔线实现类的主要逻辑在addFirst和addLast，它持有⼀个头阀⻔和⼀个尾阀⻔，它按照添加阀⻔顺序的⽅式构造阀⻔链表，按照队列的形式，决定调⽤的先后顺序
3. Pipeline的深度:Pipeline中handler的个数被称作Pipeline的深度。所以我们在⽤Pipeline的深度与JVM宿主机的CPU个数间的关系。如果Pipeline实例所处的任务多属于CPU密集⾏，那么深度最好不超过Ncpu。如果Pipeline所处理的任务多属于I/O密集型，那么Pipeline的深度最好不要超过2*Ncpu。



优点

总结⼀下Pipeline模式的优点，如下：
1、降低耦合度。它将请求的发送者和接收者解耦。
2、简化了Handler处理器。使得处理器不需要不需要知道链的结构。也就是Handler处理器可以是⽆状态的。与责任链（流⽔线）相关的状态，交给了Context去维护。
3、增强给对象指派职责的灵活性。通过改变链内的成员或者调动它们的次序，允许动态地新增或者删除责任。
4、 增加新的请求处理器很⽅便。





并发编程 Doug Lee

AbstractQueuedSynchronizer(java.util.concurrent.locks)  AQS

抽象的队列同步器

链表把头部设置为空节点，方便插入，链表里的基本的思想

抽象的队列同步器作为模板方法提供了很多实现类

实现类

公平锁、非公平锁、可重入锁、计数器、信号量、栅栏...

![image-20210619104329049](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Microservice-project-notes.assets\image-20210619104329049.png)





链表把头部设置为空节点，方便插入，链表里的基本的思想

有了头部空节点，插入方便，空节点永远是头部

![image-20210619105206968](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Microservice-project-notes.assets\image-20210619105206968.png)

![image-20210619105605278](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Microservice-project-notes.assets\image-20210619105605278.png)



# ![image-20210619110146640](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Microservice-project-notes.assets\image-20210619110146640.png)



## 管道模式应用



### 创建订单接⼝业务分析

创建订单接⼝需要做什么事情呢？
1. 参数校验
2. 向订单表中插⼊⼀条数据
3. 向订单商品关联表中插⼊N条数据
4. 扣减库存
5. 向订单邮寄表中插⼊⼀条数据
6. 清空购物⻋
7. 发送订单超时取消的消息（暂时不做）

我们发现创建⼀个订单的流程很⻓，步骤⽐较冗⻓，⽽且每⼀步的执⾏结果都会影响到下⼀步的操作，所以我们这⾥采取pipeline模式来设计整体架构



### 订单服务表分析

订单表

```sql
CREATE TABLE `tb_order` (
  `order_id` varchar(50) COLLATE utf8_bin NOT NULL DEFAULT '' COMMENT '订单id',
  `payment` decimal(10,2) DEFAULT NULL COMMENT '实付金额',
  `payment_type` int(1) DEFAULT NULL COMMENT '支付类型 1在线支付 2货到付款',
  `post_fee` decimal(10,2) DEFAULT NULL COMMENT '邮费',
  `status` int(1) DEFAULT NULL COMMENT '状态 0未付款 1已付款 2未发货 3已发货 4交易成功 5交易关闭 6交易失败 7-已退款',
  `create_time` datetime DEFAULT NULL COMMENT '订单创建时间',
  `update_time` datetime DEFAULT NULL COMMENT '订单更新时间',
  `payment_time` datetime DEFAULT NULL COMMENT '付款时间',
  `consign_time` datetime DEFAULT NULL COMMENT '发货时间',
  `end_time` datetime DEFAULT NULL COMMENT '交易完成时间',
  `close_time` datetime DEFAULT NULL COMMENT '交易关闭时间',
  `shipping_name` varchar(20) COLLATE utf8_bin DEFAULT NULL COMMENT '物流名称',
  `shipping_code` varchar(20) COLLATE utf8_bin DEFAULT NULL COMMENT '物流单号',
  `user_id` bigint(20) DEFAULT NULL COMMENT '用户id',
  `buyer_message` varchar(100) COLLATE utf8_bin DEFAULT NULL COMMENT '买家留言',
  `buyer_nick` varchar(50) COLLATE utf8_bin DEFAULT NULL COMMENT '买家昵称',
  `buyer_comment` tinyint(1) DEFAULT NULL COMMENT '买家是否已经评价',
  `unique_key` varchar(50) COLLATE utf8_bin DEFAULT NULL COMMENT '唯一键',
  PRIMARY KEY (`order_id`),
  KEY `create_time` (`create_time`),
  KEY `buyer_nick` (`buyer_nick`),
  KEY `status` (`status`),
  KEY `payment_type` (`payment_type`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin;
```

订单商品关联表

```sql
CREATE TABLE `tb_order_item` (
  `id` varchar(50) COLLATE utf8_bin NOT NULL,
  `item_id` bigint(20) NOT NULL COMMENT '商品id',
  `order_id` varchar(50) COLLATE utf8_bin NOT NULL COMMENT '订单id',
  `num` int(10) DEFAULT NULL COMMENT '商品购买数量',
  `title` varchar(200) COLLATE utf8_bin DEFAULT NULL COMMENT '商品标题',
  `price` decimal(10,2) DEFAULT NULL COMMENT '商品单价',
  `total_fee` decimal(10,2) DEFAULT NULL COMMENT '商品总金额',
  `pic_path` varchar(1000) COLLATE utf8_bin DEFAULT NULL COMMENT '商品图片地址',
  `status` int(4) DEFAULT NULL COMMENT '1库存已锁定 2库存已释放 3-库存减扣成功',
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `oder_item_id` (`order_id`,`item_id`) USING BTREE COMMENT '订单商品唯一索引',
  KEY `item_id` (`item_id`),
  KEY `order_id` (`order_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin;
```

商品库存表

```java
CREATE TABLE `tb_stock` (
  `item_id` bigint(20) NOT NULL DEFAULT '0' COMMENT '商品id',
  `stock_count` bigint(20) NOT NULL DEFAULT '0' COMMENT '库存数量',
  `lock_count` int(11) NOT NULL DEFAULT '0' COMMENT '冻结库存数量',
  `restrict_count` int(3) DEFAULT '5' COMMENT '限购数量',
  `sell_id` int(6) DEFAULT NULL COMMENT '售卖id',
  PRIMARY KEY (`item_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='库存表';
```

物流信息表

```sql
CREATE TABLE `tb_order_shipping` (
  `order_id` varchar(50) NOT NULL COMMENT '订单ID',
  `receiver_name` varchar(20) DEFAULT NULL COMMENT '收货人全名',
  `receiver_phone` varchar(20) DEFAULT NULL COMMENT '固定电话',
  `receiver_mobile` varchar(30) DEFAULT NULL COMMENT '移动电话',
  `receiver_state` varchar(10) DEFAULT NULL COMMENT '省份',
  `receiver_city` varchar(10) DEFAULT NULL COMMENT '城市',
  `receiver_district` varchar(20) DEFAULT NULL COMMENT '区/县',
  `receiver_address` varchar(200) DEFAULT NULL COMMENT '收货地址，如：xx路xx号',
  `receiver_zip` varchar(6) DEFAULT NULL COMMENT '邮政编码,如：310001',
  `created` datetime DEFAULT NULL,
  `updated` datetime DEFAULT NULL,
  PRIMARY KEY (`order_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```



# 其他



spring配置注入

或spi动态发现



核心 Handler方法的添加和执行

# NIO

传统io是阻塞io  ; new io多路复用模型效率更高



# 能优化的部分



密码散列

通知+注解 比 在各方法体中分别使用散列工具类风格和功能更好。

Md5有各种静态工具类，但是在每个方法分别使用工具类，不如配置 通知+注解 ，这样各个方法体只专注于自己的业务即可。

```
 /**
     * 用户激活
     */
    @Anoymous
    @GetMapping("verify")
    public ResponseData verify(String uid, String username) {
        UserVerifyRequest userVerifyRequest = new UserVerifyRequest();
        userVerifyRequest.setUserName(username);
        userVerifyRequest.setUuid(uid);
        UserVerifyResponse response = iVerifyService.userVerify(userVerifyRequest);

        if (response.getCode().equals(SysRetCodeConstants.SUCCESS.getCode())) {
            return new ResponseUtil<>().setData(null);
        }
        return new ResponseUtil().setErrorMsg(response.getCode());
    }
```



# BUG

## 网关层启动不了，没使用数据源，却提示没配置DataSource url属性

问题:

网关层一开始没问题，后来某次git pull后启动不了，错误如下。找不到原因，毕竟网关层并没有使用到数据源。

```
***************************
APPLICATION FAILED TO START
***************************

Description:

Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.

Reason: Failed to determine a suitable driver class


Action:

Consider the following:
	If you want an embedded database (H2, HSQL or Derby), please put it on the classpath.
	If you have database settings to be loaded from a particular profile you may need to activate it (no profiles are currently active).
```



解决：

项目结构：

```
分布式微服务 dubbo sprigboot Project下分不同module
gateway #都是controller
mall-commons
	commons-core
	commons-lock
	commons-mq
	commons-tool
mall-parent
order-service
	order-api
	order-provider
shopping-service
	shopping-api
	shopping-provider
user-service
	user-api
	user-provider
	user-sdk
```

由于网关层不需要依赖数据源，导入的依赖一般来自

mall-parent、mall-commons中的、各个service模块层的api

查遍所有pom.xml之后在队友已经无耐先给网关配置数据源用于启动之后，回头在查看网关层的pom.xml发现引入了一个provider层，就是这个provider层的依赖激活了网关层的数据源自动配置要求。

注释掉之后正常启动。

