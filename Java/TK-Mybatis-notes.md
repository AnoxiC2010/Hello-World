# tk-mybatis notes

[github主页]([abel533/Mapper: Mybatis Common Mapper - Easy to use (github.com)](https://github.com/abel533/Mapper))

依赖

```xml
<!--TK-MKybatis-->
<!--Mysql连接-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.17</version>
</dependency> 
<!--Mybatis-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.0</version>
</dependency> 
<!--durid-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.16</version>
</dependency> 
<!--tk-Mybatis-->
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper-spring-boot-starter</artifactId>
    <version>2.1.5</version>
</dependency>
```

启动类配置包扫描

```java
import tk.mybatis.spring.annotation.MapperScan;
@MapperScan(basePackages = "com.cskaoyan.mapper")//这是tk的包扫描，要扫描到mapper，无需扫描到bean类
@ComponentScan(basePackages = "com.cskaoyan")//springboot默认扫描启动类目录，代码写在其他地方需要配置包扫描
@SpringBootApplication
public class SkillsTest01Application {
    public static void main(String[] args) {
        SpringApplication.run(SkillsTest01Application.class, args);
    }

}
```

数据源配置

```yaml
# application.yml
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    username: root
    password: 123456
    # 配置serverTimezone时区，否则timybatis可能会报没有设置时区的错误
    # useOldAliasMetadataBehavior=true是支持别名 好像之前不加也可以支持别名...
    url: jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf8&useOldAliasMetadataBehavior=true&serverTimezone=Asia/Shanghai
```

Bean

```java
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import javax.persistence.Column;
import javax.persistence.Id;
import javax.persistence.Table;

@Data
@AllArgsConstructor
@NoArgsConstructor
@Table(name = "user_1")//数据库表名
public class User {
   @Id//主键
   private Integer id;

   @Column(name = "username")//数据库列名
   private String name;

   @Column(name = "password")
   private String passwd;

   @Column(name = "age")
   private Integer userage;
}
```

使用

```java
@SpringBootTest
@RunWith(SpringRunner.class)//springboot 2.1.6的注解配置
public class SkillsTest01ApplicationTests {
    @Autowired
    UserMapper userMapper;
    @Test
    public void testInsert() {
        User user = new User(null,"有鱼", "123456", 18);
        int i = userMapper.insert(user);
        System.out.println(i);//1
    }
        @Test
    public void testSelect() {
        Example example = new Example(User.class);
        example.createCriteria()
                .andGreaterThan("userage", 0)//第一个参数是DO的属性，不是@Column注解即数据库的字段
                .andLike("name", "%北%");//like这里要有%
        List<User> users = userMapper.selectByExample(example);
        System.out.println(users);
    }
}
```







