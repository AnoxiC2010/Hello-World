# dobbo notes

# dubbo - SpringMVC

新建父子工程

三个子工程：common-api 、 provider 、 consumer

在provider、consumer中分别导入common-api为依赖

## api集

定义接口

```java
public interface DemoService {
    String sayHello(String name);
}
```

## provider

依赖

```xml
<parent>
    <artifactId>dubbo_spring</artifactId>
    <groupId>com.cskaoyan</groupId>
    <version>1.0-SNAPSHOT</version>
</parent>
<modelVersion>4.0.0</modelVersion>

<groupId>com.cskaoyan.dubbo</groupId>
<artifactId>provider</artifactId>

<dependencies>
    <!--自定义api集-->
    <dependency>
        <groupId>com.cskaoyan</groupId>
        <artifactId>common-api</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
    <!--注册中心相关-->
    <dependency>
        <groupId>com.101tec</groupId>
        <artifactId>zkclient</artifactId>
        <version>0.9</version>
    </dependency>
    <dependency>
        <groupId>org.apache.zookeeper</groupId>
        <artifactId>zookeeper</artifactId>
        <version>3.4.9</version>
        <type>pom</type>
    </dependency>
    <!--dubbo-->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>dubbo</artifactId>
        <version>2.5.3</version>
    </dependency>
    <dependency>
        <groupId>io.netty</groupId>
        <artifactId>netty-all</artifactId>
        <version>4.1.6.Final</version>
    </dependency>
    <!--日志-->
    <dependency>
        <groupId>commons-logging</groupId>
        <artifactId>commons-logging</artifactId>
        <version>1.2</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
    <!--字节码增强-->
    <dependency>
        <groupId>org.javassist</groupId>
        <artifactId>javassist</artifactId>
        <version>3.21.0-GA</version>
    </dependency>
    <!-- spring相关jar -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>4.3.3.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>4.3.3.RELEASE</version>
    </dependency>
    <!--junit-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

api集中接口实现

```java
public class DemoServiceImpl implements DemoService {
    @Override
    public String sayHello(String name) {
        return "hello, " + name;
    }
}
```

spring配置文件

```xml
<!--application-context.xml-->
<!--定义服务提供者的名称-->
<dubbo:application name="my-provider"/>

<!--定义服务-->
<!--registry=N/A属性说不注册到注册中心，先使用点对点直连测试时注释掉注册中心组件-->
<!--<dubbo:service interface="com.cskaoyan.api.DemoService" ref="demoService" registry="N/A"/>-->
<dubbo:service interface="com.cskaoyan.api.DemoService" ref="demoService"/>

<!--服务暴露的协议-->
<dubbo:protocol name="dubbo" port="20880"/>

<!--注册中心-->
<dubbo:registry  address="zookeeper://localhost:2181" />

<!--实例化真正提供服务的类的对象，并交给spring容器-->
<bean id="demoService" class="com.cskaoyan.provider.DemoServiceImpl"/>
```

提供main方法，启动并阻塞防止进程结束

```java
public class Main {

    public static void main(String[] args) {

        // 创建spring容器
        ClassPathXmlApplicationContext
                context = new ClassPathXmlApplicationContext("application-context.xml");

        context.start();

        //  阻塞
        try {
            System.in.read();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```

## consumer

依赖和provider相同

spring配置文件

```xml
<!--application-context.xml-->
<!--定义服务消费者的名称-->
<dubbo:application name="my-consumer"/>

<!--引用服务-->
<!--url属性点对点直连，使用一下测试 测试时注释掉注册中心组件-->
<!--<dubbo:reference id="demoService" interface="com.cskaoyan.api.DemoService" url="dubbo://localhost:20880"/>-->
<dubbo:reference interface="com.cskaoyan.api.DemoService" id="demoService"/>

<!--服务注册中心-->
<dubbo:registry address="zookeeper://localhost:2181"/>
```

在main方法中测试消费

```java
public class Main {

    public static void main(String[] args) {

        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("application-context.xml");

        // 如果我有DemoService的引用
        DemoService demoService = (DemoService) context.getBean("demoService");

        // 在这里执行远程方法调用
        String hello = demoService.sayHello("liushifu");
        System.out.println(hello);

    }
}
```





# dubbo - SpringBoot

三个子工程：common-api 、 provider 、 consumer

在provider、consumer中分别导入common-api为依赖

### api集

不需要是springboot工程普通maven工程即可，定义api接口

```java
package com.springboot.api;
public interface DemoService {
    String sayHello(String name);
}
```

### provider多个

依赖

```xml
<dependencies>
    <!--自定义api集的依赖-->
    <dependency>
        <groupId>com.cskaoyan</groupId>
        <artifactId>commen-api</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </dependency>
	<!--springboot依赖-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

    <!--dubbo 2.7 apache-->
    <dependency>
        <groupId>org.apache.dubbo</groupId>
        <artifactId>dubbo-spring-boot-starter</artifactId>
        <version>2.7.3</version>
    </dependency>

    <!--zookeeper-->
    <dependency>
        <groupId>org.apache.zookeeper</groupId>
        <artifactId>zookeeper</artifactId>
        <version>3.4.13</version>
    </dependency>
    <!--zookeeper 底层依赖curator-->
    <dependency>
        <groupId>org.apache.curator</groupId>
        <artifactId>curator-framework</artifactId>
        <version>2.10.0</version>
    </dependency>
    <dependency>
        <groupId>org.apache.curator</groupId>
        <artifactId>curator-recipes</artifactId>
        <version>2.10.0</version>
    </dependency>
</dependencies>

```

实现api集定义的接口

```java
import com.springboot.api.DemoService;
import org.apache.dubbo.config.annotation.Service;

@Service//这个@Service注解不是Spring的，是dubbo的，作用是注册为容器组件和dubbo服务
public class DemoServiceImpl implements DemoService {
    @Override
    public String sayHello(String name) {
        return name + "你好1";
    }
}
```

启动类配置

```java
import org.apache.dubbo.config.spring.context.annotation.DubboComponentScan;
@SpringBootApplication
@DubboComponentScan(basePackages = "com.cskaoyan.service")//这是dubbo的包扫描注解，不是spring的@ComponentScan
public class SpringbootProviderApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringbootProviderApplication.class, args);
    }

}
```

springboot配置文件

```yaml
# application.yml
dubbo:
  application:
    name: springboot-provider

  protocol:
    name: dubbo
    port: 20880

  registry:
    address: zookeeper://localhost:2181
```

可以多建几个provider工程测试负载均衡, springboot的配置文件的dubbo.protocol.port需要分别指定以防端口冲突。

其他provider返回值以数字2 3 ...结尾，以供consumer负载均衡测试



注意如果是webmvc工程多个provider工程要区别tomcat的端口号

### consumer



测试组件

```java
import com.springboot.api.DemoService;
import org.apache.dubbo.config.annotation.Reference;
import org.springframework.stereotype.Component;

@Component
public class ThirdService {
    //random随机   roundrobin轮询  leastactive最少活跃调用数     consistenthash一致性哈希
//dubobo注解，interfaceClass属性可以不设置，默认是声明的接口类型，loadbalance是负载均衡设置
    @Reference(interfaceClass = DemoService.class, loadbalance = "consistenthash")
    DemoService demoService;

    public String sayHello(String name) {
        return demoService.sayHello(name);
    }
}
```



启动类

```java
import org.springframework.context.annotation.ComponentScan;
@SpringBootApplication
@ComponentScan(basePackages = "com.cskaoyan.service")//这是spring的包扫描注解
public class SpringbootConsumerApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext context
                = SpringApplication.run(SpringbootConsumerApplication.class, args);

        context.start();

        ThirdService thirdService = context.getBean(ThirdService.class);
//        String lanlaoshi = thirdService.sayHello("lanlaoshi");
//        System.out.println(lanlaoshi);

//下面通过修改@Reference注解的loadbalance属性改变负载均衡设置，查看效果
        int firstCount = 0;
        int secondCount = 0;
        int thirdCount = 0;
        for (int i = 0; i < 30; i++) {
            String zhangsan = thirdService.sayHello("zhangsan");
            System.out.println(zhangsan);

            if (zhangsan.endsWith("1")) {
                firstCount++;
            } else if (zhangsan.endsWith("2")) {
                secondCount++;
            } else {
                thirdCount++;
            }
        }

        System.out.println("first count:" + firstCount);
        System.out.println("second count:" + secondCount);
        System.out.println("third count:" + thirdCount);
    }

}
```





# zookeeper

无需安装，直接将文件夹复制到

![image-20210615211912821](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\dubbo-notes.assets\image-20210615211912821.png)