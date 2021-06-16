# MapStruct notes

依赖

```xml
<!--MapStruct-->
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct-jdk8</artifactId>
    <version>1.3.0.Final</version>
</dependency>
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct-processor</artifactId>
    <version>1.3.0.Final</version>
</dependency>
```

DO

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Table(name = "user_1")
public class User {
   @Id
   private Integer id;

   @Column(name = "username")
   private String name;

   @Column(name = "password")
   private String passwd;

   @Column(name = "age")
   private Integer userage;
   //成员的注解是用于tkmybatis的，不影响mapStruct。
    //mapStruct做的是映射两个BEAN比如例子中的DO和DTO的成员属性，不是这里@Column对应的数据库字段
}
```

DTO

```java
@Data
public class UserDTO {
   private Integer id;
   private String email;
   private String pass;
   private String howOld;
}
```

Bean转换接口

```java
import com.cskaoyan.bean.User;
import org.mapstruct.Mapper;
import org.mapstruct.Mapping;
import org.mapstruct.Mappings;

import java.util.List;

@Mapper(componentModel = "spring")//这个注解配置会在自动生成的实现类UserConverterImpl上加上Spring的@Component注解来把自动生成的实现类注册组件到spring容器
//编译后观察target目录下此接口的包下会自动生成UserConverterImpl.class文件，并加了@Component注解注册为Spring组件以供使用
public interface UserConverter {
    @Mappings({
            @Mapping(source = "name", target = "email"),//source是源的属性名，target是转化目标的对应属性名
            @Mapping(source = "passwd", target = "pass"),
            @Mapping(source = "userage", target = "howOld")
    })
    UserDTO userDOToUserDTO(User user);

    List<UserDTO> userDOsToUserDTOs(List<User> users);
}
```

使用

```java
@SpringBootTest
@RunWith(SpringRunner.class)
public class SkillsTest01ApplicationTests {
    @Autowired
    UserMapper userMapper;
    @Autowired
    UserConverter userConverter;
    @Test
    public void testSelect() {

        User user = userMapper.selectByPrimaryKey(1);
        System.out.println(user);
        UserDTO userDTO = userConverter.userDOToUserDTO(user);
        System.out.println(userDTO);

        Example example = new Example(User.class);
        example.createCriteria()
            .andGreaterThan("userage", 0)
            .andLike("name", "%北%");
        List<User> users = userMapper.selectByExample(example);
        System.out.println(users);

        List<UserDTO> userDTOS = userConverter.userDOsToUserDTOs(users);
        System.out.println(userDTOS);

    }
}
```

