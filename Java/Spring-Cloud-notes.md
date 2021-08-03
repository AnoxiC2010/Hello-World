Spring Cloud notes

使用版本

cloud	Hoxton.SR1

boot	2.2.2.RELEASE

cloud alibaba	2.1.0.RELEASE

Java	8

Maven	3.5及以上

MySQL	5.7及以上

![image-20210731122624691](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210731122624691.png)



有关技术栈：

- RestTemplate实现REST风格的地址调用

# 创建父工程

创建maven工程

create from archetype → org.apache.maven.archetypes:maven-archetype-site

字符集设置

idea → settings → File Encoding 都改为utf-8

开启注解支持

idea → Annotation Processors 勾选 Enable annotation processing

删除父工程 的src

maven父工程的dependencyManagement标签统一管理依赖版本

maven跳过单元测试

父工程创建完成执行mvn:install将父工程发布到仓库方便子工程继承

```xml
<modelVersion>4.0.0</modelVersion>

<groupId>com.huawei.springcloud</groupId>
<artifactId>cloud2020</artifactId>
<version>1.0-SNAPSHOT</version>
<!--pom表示总的父工程-->
<packaging>pom</packaging>

<modules>
    <module>cloud-provider-payment8001</module>
</modules>

<!--统一管理jar包版本-->
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <junit.version>4.12</junit.version>
    <log4j.version>1.2.17</log4j.version>
    <lombok.version>1.16.18</lombok.version>
    <mysql.version>5.1.47</mysql.version>
    <druid.version>1.1.16</druid.version>
    <mybatis.spring.boot.version>1.3.0</mybatis.spring.boot.version>
</properties>

<!--子模块继承之后，提供作用：锁定版本 + 子module不用写groupId和version-->
<dependencyManagement>
    <dependencies>
        <!--spring boot 2.2.2-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>2.2.2.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <!--spring cloud Hoxton.SR1-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>Hoxton.SR1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <!--spring cloud alibaba 2.1.0.RELEASE-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2.1.0.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <!--MySQL driver-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>${mysql.version}</version>
        </dependency>
        <!--Data Source-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>${druid.version}</version>
        </dependency>
        <!--ORM-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>${mybatis.spring.boot.version}</version>
        </dependency>
        <!--junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
        </dependency>
        <!--log4j-->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>${log4j.version}</version>
        </dependency>
        <!--lombok-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>${lombok.version}</version>
            <optional>true</optional>
        </dependency>
    </dependencies>
</dependencyManagement>

<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <fork>true</fork>
                <addResources>true</addResources>
            </configuration>
        </plugin>
    </plugins>
</build>
```



# 构建微服务模块

建module → 改POM → 写YML → 主启动 → 业务类

## payment模块

![image-20210731142221057](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210731142221057.png)

1. 建module

   ![image-20210731143145160](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210731143145160.png)

   创建完成后父工程的pom.xml多了如下部分

   ```xml
   <modules>
       <module>cloud-provider-payment8001</module>
   </modules>
   ```

2. 改POM

   ```xml
   <parent>
       <artifactId>cloud2020</artifactId>
       <groupId>com.huawei.springcloud</groupId>
       <version>1.0-SNAPSHOT</version>
   </parent>
   <modelVersion>4.0.0</modelVersion>
   
   <artifactId>cloud-provider-payment8001</artifactId>
   
   <dependencies>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-actuator</artifactId>
       </dependency>
       <dependency>
           <groupId>org.mybatis.spring.boot</groupId>
           <artifactId>mybatis-spring-boot-starter</artifactId>
       </dependency>
       <dependency>
           <groupId>com.alibaba</groupId>
           <artifactId>druid-spring-boot-starter</artifactId>
           <version>1.1.10</version>
       </dependency>
       <!--mysql-connector-java-->
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
       </dependency>
       <!--jdbc-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-jdbc</artifactId>
       </dependency>
       <!--热部署-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
           <scope>runtime</scope>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
   </dependencies>
   ```

   

3. 写YML

   resource报价新建application.yml

   凡是微服务记得写端口和和服务名称

   ```yaml
   server:
     port: 8001
   
   spring:
     application:
       name: cloud-payment-service
     datasource:
       # 当前数据源操作类型
       type: com.alibaba.druid.pool.DruidDataSource
       # mysql驱动包
       driver-class-name: org.gjt.mm.mysql.Driver
       url: jdbc:mysql://localhost:3306/cloud2020?useUnicode=true&characterEncoding=utf-8&useSSL=false
       username: root
       password: 123456
   
   mybatis:
     mapper-locations: classpath:mapper/*.xml
     # 所有entity别名类所在的包
     type-aliases-package: com.huawei.springcloud.entities
   ```

   

4. 主启动

   创建主启动类

   ```java
   package com.huawei.springcloud;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   @SpringBootApplication
   public class PaymentMain8001 {
       public static void main(String[] args) {
           SpringApplication.run(PaymentMain8001.class, args);
       }
   }
   ```

5. 业务类

   - 建表

     ```sql
     CREATE TABLE `payment`(
     `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'ID',
     `serial` varchar(200) DEFAULT '',
     PRIMARY KEY(`id`)
     ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
     ```

   - entities

     ```java
     //实体类
     @Data
     @AllArgsConstructor
     @NoArgsConstructor
     public class Payment implements Serializable {
         private Long id;
         private String serial;
     }
     ```

     ```java
     //通用返回结果集 json封装体
     @Data
     @NoArgsConstructor
     @AllArgsConstructor
     public class CommenResult<T> {
         private Integer code;
         private String message;
         private T data;
     
         public CommenResult(Integer code, String message) {
             this(code, message, null);
         }
     }
     ```

     

   - dao

     ```java
     @Mapper
     public interface PaymentDao {
         int create(Payment payment);
         Payment getPaymentById(@Param("id") Long id);
     }
     ```

     mybatis映射文件

     ```XML
     <mapper namespace="com.huawei.springcloud.dao.PaymentDao">
     
         <insert id="create" parameterType="com.huawei.springcloud.entities.Payment" useGeneratedKeys="true" keyProperty="id">
             insert into payment(serial) values (#{serial});
         </insert>
         <resultMap id="BaseResultMap" type="com.huawei.springcloud.entities.Payment">
             <id property="id" column="id" jdbcType="BIGINT"/>
             <result property="serial" column="serial" jdbcType="VARCHAR"/>
         </resultMap>
         <select id="getPaymentById" parameterType="Long" resultMap="BaseResultMap">
             select * from payment where id = #{id};
         </select>
     </mapper>
     ```

     

   - service

     ```java
     public interface PaymentService {
         int create(Payment payment);
         Payment getPaymentById(Long id);
     }
     ```

     ```java
     @Service
     public class PaymentServiceImpl implements PaymentService {
         @Resource
         private PaymentDao paymentDao;
         @Override
         public int create(Payment payment) {
             return paymentDao.create(payment);
         }
     
         @Override
         public Payment getPaymentById(Long id) {
             return paymentDao.getPaymentById(id);
         }
     }
     ```

     

   - controller

     ```java
     @Slf4j
     @RestController
     public class PayentController {
         @Resource
         private PaymentService paymentService;
     	/*
     	注意：
     	这里入参前没加@RequestBody注解，但是如果由restTemplate远程调用的话，
     	需要加@RequestBody注解，restTemplate的postForObject会把参数以json形式传递
     	*/
         @PostMapping("payment/create")//不太符合restful不要在意
         public CommenResult create(Payment payment) {
             int affectedRows = paymentService.create(payment);
             log.info("******插入支付数据：" + payment);
             if (affectedRows > 0) {
                 return new CommenResult<Payment>(200, "插入数据库成功");
             }
             return new CommenResult(444, "插入数据库失败");
         }
         @GetMapping("payment/get/{id}")
         public CommenResult getPaymentById(@PathVariable("id") Long id) {
             Payment payment = paymentService.getPaymentById(id);
             log.info("******查询支付记录 id=" + id);
             if (payment != null) {
                 return new CommenResult<Payment>(200, "查询成功", payment);
             }
             return new CommenResult(444, "查询失败，不存在该记录，id=", id);
         }
     }
     ```



## order模块

1. 建module![image-20210731230044210](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210731230044210.png)

2. 改POM

   ```xml
   <parent>
       <artifactId>cloud2020</artifactId>
       <groupId>com.huawei.springcloud</groupId>
       <version>1.0-SNAPSHOT</version>
   </parent>
   <modelVersion>4.0.0</modelVersion>
   
   <artifactId>cloud-consumer-order80</artifactId>
   
   <dependencies>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-actuator</artifactId>
       </dependency>
       <!--开启热部署-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
           <scope>runtime</scope>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
   </dependencies>
   
   ```

3. 写YML

   ```yml
   server:
     port: 80
   ```

4. 主启动

   ```java
   package com.huawei.springboot;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   @SpringBootApplication
   public class OrderMain80 {
       public static void main(String[] args) {
           SpringApplication.run(OrderMain80.class, args);
       }
   }
   ```

5. 业务类

   - entities

     主实体payment和Json封装体 复制payment模块的（应该抽取为公共模块）

   - RestTemplate

     是什么：RestTemplate提供了多种便捷访问远程Http服务的方法，是一种简单便捷的访问restful服务模板类，是Spring提供的用于访问Rest服务的客户端模板工具集

     官网及使用：[RestTemplate (Spring Framework 5.2.2.RELEASE API)](https://docs.spring.io/spring-framework/docs/5.2.2.RELEASE/javadoc-api/)

     ```
     org.springframework.web.client
     Class RestTemplate
     ```

     > 和httpClient异曲同工

     使用：使用restTemplate访问restful接口非常简单，(url, requestMap, ResponseBeans.class)这三个参数分别单表REST请求地址、请求参数、HTTP响应转换被转换成的对象类型。

   - config配置类 - ApplicationContextConfig

     ```java
     package com.huawei.springboot.config;
     
     import org.springframework.context.annotation.Bean;
     import org.springframework.context.annotation.Configuration;
     import org.springframework.web.client.RestTemplate;
     
     @Configuration
     public class ApplicationContextConfig {
     
         @Bean
         public RestTemplate getRestTemplate() {
             return new RestTemplate();
         }
     }
     ```

   - controller

     ```java
     @Slf4j
     @RestController
     public class OrderController {
         private static final String PAYMENT_URL = "http://localhost:8001";
         @Resource
         private RestTemplate restTemplate;
     	/*
     	注意：
     	restTemplate的postForObject把参数payment以json的形式请求服务提供者，
     	所以服务提供者的入参需要增加@RequestBody注解才能接收到参数的属性值
     	*/
         @GetMapping("/consumer/payment/create")//这里是客户端发送用get
         public CommenResult<Payment> creat(Payment payment) {
             log.info("OrderController.creat payment=" + payment);
             return restTemplate.postForObject(PAYMENT_URL + "/payment/create", payment, CommenResult.class);
         }
     
         @GetMapping("/consumer/payment/get/{id}")
         public CommenResult getPaymentById(@PathVariable("id") Long id) {
             log.info("OrderController.getPaymentById id=" + id);
             return consumer/payment/get(PAYMENT_URL + "/payment/get/" + id, CommenResult.class);
         }
     }
     ```

6. 测试

   注意上面提到的@RequestBody问题，不加注解不报错，但是数据库插入值为null

# 工程重构

- 问题

  系统中由重复部分，需要重构：order模块直接复制的payment模块的entities包

- 建module

  ![image-20210801121438314](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801121438314.png)

- POM

  ```xml
  <parent>
      <artifactId>cloud2020</artifactId>
      <groupId>com.huawei.springcloud</groupId>
      <version>1.0-SNAPSHOT</version>
  </parent>
  <modelVersion>4.0.0</modelVersion>
  
  <artifactId>cloud-api-commons</artifactId>
  
  <dependencies>
      <!--开启热部署-->
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-devtools</artifactId>
          <scope>runtime</scope>
          <optional>true</optional>
      </dependency>
      <dependency>
          <groupId>org.projectlombok</groupId>
          <artifactId>lombok</artifactId>
          <optional>true</optional>
      </dependency>
      <!--糊涂工具包，方便之后有用-->
      <dependency>
          <groupId>cn.hutool</groupId>
          <artifactId>hutool-all</artifactId>
          <version>5.1.0</version>
      </dependency>
  </dependencies>
  ```

- entities

  把payment和order模块里的Payment实体类和CommonResult通用封装类复制到这个新建的公共模块

- maven命令celan install

  把新建的公共模块打包上传到公用的本地库供其他另外两个工程调用

- 订单80和支付8001分别改造

  - 删除各自原先有过的entities文件夹

  - 各自粘贴POM内容

    80

    8001

    ```xml
    <!--引入自定义的api通用包，可以使用Payment支付Entity和CommonResult通用返回封装类-->
    <dependency>
        <groupId>com.huawei.springcloud</groupId>
        <artifactId>cloud-api-commons</artifactId>
        <version>${project.version}</version>
    </dependency>
    ```

    

# Eureka服务注册与发现

虽然Eureka已经停更，但是很多Spring Cloud老项目还在使用

[Home · Netflix/eureka Wiki (github.com)](https://github.com/Netflix/eureka/wiki)

![image-20210801144809422](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801144809422.png)

## Eureka基础知识

[Spring Cloud服务注册-Eureka介绍和部署 – Heart.Think.Do (heartthinkdo.com)](http://www.heartthinkdo.com/?p=1933)

- 什么是服务治理

  Spring Cloud封装了Netflix公司开发的Eureka模块来实现服务治理

  在传统的rpc远程调用框架中，管理每个服务于服务之间的依赖关系比较复杂，管理比较复杂，所以需要使用服务治理，管理服务与服务之间的依赖关系，可以实现服务调用、负载均衡、容错等，实现服务发现与注册。

- 什么是服务注册

  ![image-20210801150624038](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801150624038.png)

  ![image-20210801150658056](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801150658056.png)

- Eurake两个组件

  ![image-20210801151137026](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801151137026.png)

## 单机Eurake构建步骤

用例端口号

Eureka Server	7001

Service Consumer 80

Service Provider 8001

### IDEA生成EurekaServer端服务注册中心

- 建module 

  ![image-20210801152559701](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801152559701.png)

- 改POM 

  ![image-20210801152815402](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801152815402.png)

  ```xml
  <parent>
      <artifactId>cloud2020</artifactId>
      <groupId>com.huawei.springcloud</groupId>
      <version>1.0-SNAPSHOT</version>
  </parent>
  <modelVersion>4.0.0</modelVersion>
  
  <artifactId>cloud-eureka-server7001</artifactId>
  
  <dependencies>
      <!--eureka-server-->
      <dependency>
          <groupId>org.springframework.cloud</groupId>
          <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
      </dependency>
      <!--引入自己定义的api通用包-->
      <dependency>
          <groupId>com.huawei.springcloud</groupId>
          <artifactId>cloud-api-commons</artifactId>
          <version>${project.version}</version>
      </dependency>
      <!--boot web actuator-->
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-web</artifactId>
      </dependency>
      <dependency><!--图形监控-->
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-actuator</artifactId>
      </dependency>
      <!--一般为通用配置-->
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-devtools</artifactId>
          <scope>runtime</scope>
          <optional>true</optional>
      </dependency>
      <dependency>
          <groupId>org.projectlombok</groupId>
          <artifactId>lombok</artifactId>
          <optional>true</optional>
      </dependency>
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-test</artifactId>
          <scope>test</scope>
      </dependency>
  </dependencies>
  ```

- 写YML 

  ```yaml
  server:
    port: 7001
  
  eureka:
    instance:
      # eureka服务端的实例名称
      hostname: localhost
    client:
      # false表示不向注册中心注册自己
      register-with-eureka: false
      # false表示自己端就是注册中心，我的职责就是维护服务实例
      fetch-registry: false
      service-url:
        # 设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址
        defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
  ```

- 主启动 

  添加@EnableEurekaServer注解

  ```java
  @SpringBootApplication
  @EnableEurekaServer//表示本服务模块是Eureka注册中心
  public class EurekaMain7001 {
      public static void main(String[] args) {
          SpringApplication.run(EurekaMain7001.class, args);
      }
  }
  ```

- 业务类

  该模块用做Eureka注册中心，不需要

- 测试

  访问localhost:7001显示Eureka主页

  ![image-20210801160827413](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801160827413.png)

  ![image-20210801161041964](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801161041964.png)

  No instance available显示还没由实例入驻

### provider注册

EurekaClient端cloud-provider-payment-8001

将注册进EurekaServer成为服务提供者provider

- 改POM

  ![image-20210801161556687](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801161556687.png)

  加入

  ```xml
  <!--eureka-client-->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
  </dependency>
  ```

- 写YML

  application.yml添加

  ```yaml
  eureka:
    client:
      # 表示是否将自己注册进EurekaServer默认为true
      register-with-eureka: true
      # 是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
      fetch-registry: true
      service-url:
        defaultZone: http://localhost:7001/eureka
  ```

- 主启动

  添加@EnableEurekaClient

  ```java
  @SpringBootApplication
  @EnableEurekaClient
  public class PaymentMain8001 {
      public static void main(String[] args) {
          SpringApplication.run(PaymentMain8001.class, args);
      }
  }
  ```

- 测试

  - 先要启动EurekaServerr

  - 在启动provider

  - http://localhost:7001

    ![image-20210801163602776](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801163602776.png)

    发现服务实例

  - 微服务注册名配置说明

    如上可知：服务的实例名称 = yml文件配置的spring.application.name

- 自我保护机制

### consumer注册

EurekaClient端cloud-consumer-order80

将注册进EurekaServer成为服务消费者consumer

- POM

  添加

  ```xml
  <!--eureka-client-->
  <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
  </dependency>
  ```

- YML

  添加

  ```yaml
  spring:
    application:
      name: cloud-order-service
  
  eureka:
    client:
      # 表示是否将自己注册进EurekaServer默认为true
      register-with-eureka: true
      # 是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
      fetch-registry: true
      service-url:
        defaultZone: http://localhost:7001/eureka
  ```

- 主启动

  添加@EnableEurekaClient

  ```java
  @SpringBootApplication
  @EnableEurekaClient
  public class OrderMain80 {
      public static void main(String[] args) {
          SpringApplication.run(OrderMain80.class, args);
      }
  }
  ```

- 测试

  - 先启动EurekaServer， 7001服务

  - 在启动服务提供者provider， 8001服务

  - eureka服务器

    ![image-20210801165836557](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801165836557.png)

  - http://localhost/consumer/payment/get31

    ![image-20210801165958550](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801165958550.png)

    测试时代码里是restTemplate的url写死的应该和eureka没什么关系

### bug

yml文件缩进和空格不正确的小bug注意

### Eureka自我保护

![image-20210801165750705](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801165750705.png)

说是Eureka的自我保护机制，

启动payment服务入驻时我没由遇到这个问题，再启动order模块入驻时遇到了



## 集群Eureka构建步骤

### Eureka集群原理说明

<img src="C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801173401545.png" alt="image-20210801173401545"  />

![image-20210801173857482](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801173857482.png)

原理：互相注册，相互守望，对外暴漏出一个整体，多个同理

![image-20210801174313166](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801174313166.png)

### EurekaServer集群环境构建步骤

- 参考cloud-eureka-server7001

- 新建cloud-eureka-server7002

- 改POM

- 修改映射配置

  win10修改C:\Windows\System32\drivers\etc下的hosts文件

  添加映射配置

  ```
  127.0.0.1   eureka7001.com
  127.0.0.1   eureka7002.com
  ```

  因为需要再yml给eureka的server配置不同的hostname但是测试环境只有一台物理机笔记本电脑，以此模拟多台机器

- 写YML(以前单机)

  ```yaml
  server:
    port: 7001
  
  eureka:
    instance:
      # eureka服务端的实例名称
      hostname: eureka7001.com
    client:
      # false表示不向注册中心注册自己
      register-with-eureka: false
      # false表示自己端就是注册中心，我的职责就是维护服务实例
      fetch-registry: false
      service-url:
        # defaultZone: http://eureka7001.com:7001/eureka/ # 单机时就是自己
        # 设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址
        defaultZone: http://eureka7002.com:7002/eureka/
  ```

  ```yaml
  server:
    port: 7002
  
  eureka:
    instance:
      # eureka服务端的实例名称
      hostname: eureka7002.com
    client:
      # false表示不向注册中心注册自己
      register-with-eureka: false
      # false表示自己端就是注册中心，我的职责就是维护服务实例
      fetch-registry: false
      service-url:
        # defaultZone: http://eureka7002.com:7002/eureka/ # 单机时就是自己
        # 设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址
        defaultZone: http://eureka7001:7001/eureka/
  ```

  

- 主启动

  ```java
  @SpringBootApplication
  @EnableEurekaServer
  public class EurekaMain7002 {
      public static void main(String[] args) {
          SpringApplication.run(EurekaMain7002.class, args);
      }
  }
  ```

- 测试

  ![image-20210801204344512](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801204344512.png)

  ![image-20210801204426415](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801204426415.png)

  集群搭建成功，用配置的hostname和端口访问呢，也可以用localhost和端口访问，发现互相指向

### 将支付服务801微服务发布到上面2台Eureka集群配置中

- YML

  修改

  ```yaml
  eureka:
    client:
      register-with-eureka: true
      fetch-registry: true
      service-url:
  #      defaultZone: http://localhost:7001/eureka  # 单机版
        defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka # 集群版
  ```

### 将订单服务80微服务发布到上面2台Eureka集群配置中

- YML

  同上

### 测试01

- 先启动EurekaServer，7001/7002服务
- 再启动服务提供者provider， 8001
- 在启动消费者， 80
- http://localhost/consumer/payment/get/31

![image-20210801210813082](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801210813082.png)

eureka7001.com:7001同理。只是controller层url写死的还测不出集群效果



### 支付服务提供者8001集群环境构建

- 参考cloud-provider-payment8001

- 新建cloud-provider-payment8002

- 改POM

  copy

- 写YML

  只改端口号

- 主启动

  copy 改类名

- 业务类

  copy

- 修改8001/8002的Controller

  ```java
  @Slf4j
  @RestController
  public class PayentController {
      @Resource
      private PaymentService paymentService;
      @Value("${server.port}")//添加端口打印，用于测试
      private String serverPort;
  
      @PostMapping("payment/create")
      public CommenResult<Payment> create(@RequestBody Payment payment) {
          int affectedRows = paymentService.create(payment);
          log.info("******插入支付数据：{}, 端口:{}", payment, serverPort);
          if (affectedRows > 0) {
              return new CommenResult<Payment>(200, "插入数据库成功,serverPort=" + serverPort);
          }
          return new CommenResult(444, "插入数据库失败,serverPort=" + serverPort);
      }
      @GetMapping("payment/get/{id}")
      public CommenResult getPaymentById(@PathVariable("id") Long id) {
          Payment payment = paymentService.getPaymentById(id);
          log.info("******查询支付记录 id={}, 端口：{}", id, serverPort);
          if (payment != null) {
              return new CommenResult<Payment>(200, "查询成功,serverPort=" + serverPort, payment);
          }
          return new CommenResult(444, "查询失败，不存在该记录，id=" + id + ", serverPort=" + serverPort, null);
      }
  }
  ```

配置好集群之后, eureka7002.com:7002同

![image-20210801214418120](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801214418120.png)

### 负载均衡

以上由于再order服务里把调用服务的url写死了，所以测试指挥访问8001端口的payment服务

- 订单服务访问地址不能写死

  修改order模块controller

  ```java
  @Slf4j
  @RestController
  public class OrderController {
  //    private static final String PAYMENT_URL = "http://localhost:8001";
      //通过再eureka上注册过的微服务名称调用
      private static final String PAYMENT_URL = "http://CLOUD-PAYMENT-SERVICE";
  
      @Resource
      private RestTemplate restTemplate;
      ...
  }
  ```

  修改完调用服务报错：Caused by: java.net.UnknownHostException: CLOUD-PAYMENT-SERVICE

  原因：没有加@LoadBalance注解的RestTemplate只能通过ip+端口调用，通过服务名称调用需要添加@LoadBanlance注解即使只有一个服务

- 使用@LoadBalanced注解赋予RestTemplate负载均衡的能力

  ```java
  @Configuration
  public class ApplicationContextConfig {
  
      @Bean
      @LoadBalanced//赋予RestTemplate负载均衡的能力，默认轮询
      public RestTemplate getRestTemplate() {
          return new RestTemplate();
      }
  }
  ```

  这样RestTemplate才能通过服务名远程调用

- ApplicationContextBean

  ribbon的负载均衡功能

### 测试02

- 先启动EurekaServer， 7001/7002服务
- 在启动服务提供者provider, 8001/8002服务
- http://localhost/consumet/payment/get/1
- 结果 → 负载均衡效果达到，8001/8002端口的服务交替被调用
- Ribbon和Eureka整合后Consumer可以直接掉用服务而不用再关心地址和端口号，且该服务还有负载均衡功能

## actuator微服务信息完善

### 主机名称：服务名称修改

#### 当前问题

![image-20210801224653200](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801224653200.png)

含有主机名称

鼠标放在UP绿字链接上左下角没有显示ip

#### 修改cloud-provider-payment8001

YML

- 修改部分

  添加

  ```yaml
  eureka:
      instance:
      # 访问路径显示实例id
      instance-id: payment8001
      # 访问路径可以显示IP地址
      prefer-ip-address: true
  ```

  

- 完整

  ```yaml
  eureka:
    client:
      # 表示是否将自己注册进EurekaServer默认为true
      register-with-eureka: true
      # 是否从EurekaServer抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
      fetch-registry: true
      service-url:
  #      defaultZone: http://localhost:7001/eureka  # 单机版
        defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka # 集群版
    instance:
      # 访问路径显示实例id
      instance-id: payment8001
      # 访问路径可以显示IP地址
      prefer-ip-address: true
  ```

  

#### 修改之后

![image-20210801230919307](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801230919307.png)

8001/8002修改后不含有主机名

显示YML中配置的实例id，鼠标指向实例链接左下角会显示ip+端口号

健康检查：

![image-20210801230206727](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801230206727.png)

### 访问信息有IP信息提示

如上

通过POM如下和YML如上

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

完成主机名服务名称的修改和访问信息有ip地址的提示

## 服务发现Discovery

- 对于注册进eureka里面的微服务，可以通过服务发现来获得该服务的信息

- 修改cloud-provider-payment8001的Controller

  ```java
  @Slf4j
  @RestController
  public class PayentController {
      @Resource
      private PaymentService paymentService;
      @Value("${server.port}")//添加端口打印，用于测试
      private String serverPort;
      @Resource
      private DiscoveryClient discoveryClient;
  	...
      @GetMapping("payment/discovery")
      public Object discovery() {
          List<String> services = discoveryClient.getServices();
          for (String service : services) {
              log.info("*****service : " + service);
          }
          List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
          for (ServiceInstance instance : instances) {
              log.info(instance.getServiceId() + "\t" + instance.getHost() + "\t" + instance.getPort() + "\t" + instance.getUri());
          }
          return discoveryClient;
      }
  }
  ```

  

- 8001主启动类

  添加@EnableDiscoveryClient

  ```java
  @SpringBootApplication
  @EnableEurekaClient//只是eureka
  @EnableDiscoveryClient//不只是eureka可以用
  public class PaymentMain8001 {
      public static void main(String[] args) {
          SpringApplication.run(PaymentMain8001.class, args);
      }
  }
  ```

- 自测

  先启动EurekaServer

  在启动8001主启动类

  http://localhost:8001/payment/discovery

  ![image-20210801234537075](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801234537075.png)

  ![image-20210801234603168](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801234603168.png)



## Eureka自我保护

![image-20210801235114882](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801235114882.png)

- 故障现象

- 导致原因

  - 一句话：某时刻某一个微服务不可用了，Eureka不会立刻清理，依旧会对改为服务的信息进行保存

  - 属于CAP里面的AP分支

    ![image-20210801235806765](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801235806765.png)

    ![image-20210802000003747](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210802000003747.png)

    ![image-20210802000153035](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210802000153035.png)

    

- 怎么禁止自我保护

  - 注册中心eurekaServer端7001

    出厂默认，自我保护机制时开启的 → eureka.server.enable-self-preservation = true

    使用eureka.server.enable-self-preservation = false可以禁用自我保护模式

    ```yaml
    eureka:
      server:
        # 关闭自我保护机制，保证不可用服务被及时剔除
        enable-self-preservation: false
        # 修改默认清理无效节点时间间隔60s → 2s， 测试关闭自我保护效果
        eviction-interval-timer-in-ms: 2000
    ```

    关闭效果

    在eurekaServer端7001处设置关闭自我保护机制

    ![image-20210802110856064](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210802110856064.png)

  - 生产者客户端eurekaClient端8001

    默认

    ```
    eureka.instance.lease-renewal-interval-in-seconds=30 # 单位秒，默认30秒
    eureka.instance.lease-expiration-duration-in-seconds=90 # 单位秒，默认90秒
    ```

    配置

    ```yaml
    eureka:
      instance:
        # Eureka客户端向服务端发送心跳的时间间隔，单位为秒（默认30秒）
        lease-renewal-interval-in-seconds: 1
        # Eureka服务端在收到最后一次心跳后等待时间上线，单位为秒（默认时90秒），超时将剔除服务
        lease-expiration-duration-in-seconds: 2
    ```

    测试

    7001和8001都配置完成

    先启动7001在启动8001

    先关闭8001 → 马上被删除了



# Spring Cloud整合Zookeeper代替Eureka

## 注册中心Zookeeper

​	zookeeper是一个分布式协调工具，可以实现注册中心功能

​	关闭Linux服务器防火墙后启动zookeeper服务器

​	zookeeper服务器取代Eureka服务器，zk作为服务注册中心

![image-20210802120627570](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210802120627570.png)

```
cd /myzookeeper/zookeeper-3.4.9/bin
systemctl stop firewalld → 关闭防火墙
systemctl status firewalld
ifconfig → 查看zookeeper的ip
ping一下win的ip，保证win和linux户型ping通
```



## 服务提供者

- 新建cloud-provider-payment8004

  ![image-20210802161815184](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210802161815184.png)

- POM

  ```xml
  <parent>
      <artifactId>cloud2020</artifactId>
      <groupId>com.huawei.springcloud</groupId>
      <version>1.0-SNAPSHOT</version>
  </parent>
  <modelVersion>4.0.0</modelVersion>
  
  <artifactId>cloud-provider-payment8004</artifactId>
  
  <dependencies>
      <!--SpringBoot整合web组件-->
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-web</artifactId>
      </dependency>
      <!--引入自定义的api通用包，可以使用Payment支付Entity和CommonResult通用返回封装类-->
      <dependency>
          <groupId>com.huawei.springcloud</groupId>
          <artifactId>cloud-api-commons</artifactId>
          <version>${project.version}</version>
      </dependency>
      <!--SpringBoot整合zookeeper客户端-->
      <dependency>
          <groupId>org.springframework.cloud</groupId>
          <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
      </dependency>
      <!--开启热部署-->
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-devtools</artifactId>
          <scope>runtime</scope>
          <optional>true</optional>
      </dependency>
      <dependency>
          <groupId>org.projectlombok</groupId>
          <artifactId>lombok</artifactId>
          <optional>true</optional>
      </dependency>
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-test</artifactId>
          <scope>test</scope>
      </dependency>
  </dependencies>
  ```

- YML

  ```yaml
  # 8004表示注册到zookeeper服务器的支付服务提供者端口号
  server:
    port: 8004
  
  spring:
    application:
      # 服务别名----注册zookeeper到注册中心名称
      name: cloud-provider-payment
    cloud:
      zookeeper:
        # 1个zookeeper机器的ip+端口号
        connect-string: 192.168.88.130:2181
  ```

- 主启动类

  ```java
  //不是Eureka了，不需要要@EnableEurekaClient或server的注解了
  @SpringBootApplication
  @EnableDiscoveryClient//该注解用于向使用consul或这zookeeper作为注册中心时注册服务
  public class PaymentMain8004 {
      public static void main(String[] args) {
          SpringApplication.run(PaymentMain8004.class, args);
      }
  }
  ```

- Controller

  ```java
  @RestController
  @Slf4j
  public class PaymentController {
      @Value("${server.port}")
      private String serverPort;
  
      @RequestMapping(value = "payment/zk")
      public String paymentZk() {
          //返回端口号和流水号
          return "SpringCloud with zookeeper:" + serverPort + "\t" + UUID.randomUUID().toString();
      }
  }
  ```

- 启动8004注册进zookeeper

- 验证测试

- 验证测试2

- 思考


## 服务消费者

# 热部署Devtools

如果需要，仅用于开发阶段

启用后dashboard会显示devtools

![image-20210801163503201](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210801163503201.png)

>  Idea有重新加载的功能，其实也没必要使用热部署

1. Adding devtools to your project

   ```xml
   <!--添加到微服务模块的pom.xml中-->
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-devtools</artifactId>
       <scope>runtime</scope>
       <optional>true</optional>
   </dependency>
   ```

2. Adding plugin to your pom.xml

   ```xml
   <!--添加到聚合父类总工程的pom.xml里-->
   <build>
       <finalName>你自己的工程名字</finalName><!--这行不是必要的-->
       <plugins>
         <plugin>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-maven-plugin</artifactId>
           <configuration>
             <fork>true</fork>
             <addResources>true</addResources>
           </configuration>
         </plugin>
       </plugins>
     </build>
   ```

   

3. Enabling automatic build

   settings → Compiler

   勾选

   - automatically show first error in editor (默认已勾选)
   - Display notification on build completion (默认已勾选)
   - Build project automatically
   - Compile independent modules in parallel

   ![image-20210731215430800](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210731215430800.png)

4. Update the value of Registry

   快捷建ctrl + shift + alt + / 选择Registry

   Registry窗口勾选

   - compiler.automake.allow.when.app.running
   - actionSystem.assertFocusAccessFromEdt (默认已勾选)

   ![image-20210731220056209](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210731220056209.png)

5. Restart IDEA



