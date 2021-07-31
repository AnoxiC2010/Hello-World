# Spring Cloud notes

使用版本

cloud	Hoxton.SR1

boot	2.2.2.RELEASE

cloud alibaba	2.1.0.RELEASE

Java	8

Maven	3.5及以上

MySQL	5.7及以上

![image-20210731122624691](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-Cloud-notes.assets\image-20210731122624691.png)



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

     官网及使用

   - config配置类 - ApplicationContextConfig

   - controller

# 热部署Devtools

如果需要，仅用于开发阶段

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



