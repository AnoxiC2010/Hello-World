# MyBatis notes

## 1. 简介

[MyBatis](https://mybatis.org/mybatis-3/zh/index.html) 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

**Mybatis就是一个ORM框架**，什么是ORM呢？ Object  Relationship Mapping （对象关系映射）, 通俗的来说，其实就是把我们的Java对象和关系型数据库中的记录去做一个映射，我们使用Mybatis可以很方便的去把Java对象转化为关系型数据库表中的记录，也可以很方便的把关系型数据库表中的记录转化为Java对象。



为什么不使用JDBC呢？

1. JDBC步骤很繁琐，不利于提高开发效率

2. 在JDBC里面，我们的SQL语句和业务代码是严重耦合起来的，这样其实就很不合理，我们去修改SQL语句的时候会很麻烦，不利于后期代码的维护

3. JDBC自动封装参数和自动解析结果的功能很繁琐，即使我们使用了DBUtils也还是不够强大，不太能够符合企业中开发的需要

## 2. 入门案例

第一个Mybatis的入门案例

### 2.1 导包

```xml
<!-- 数据库驱动-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
</dependency>

<!-- Mybatis 使用Mybatis 3.5以上的版本-->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.5</version>
</dependency>

<!-- 测试-->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>
```

### 2.2 配置

我们需要配置一个Mybatis的主配置文件

![image-20210427100205200](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427100205200.png)





我们还需要配置一个Mapper.xml文件，这个Mapper.xml文件是用来集中管理SQL语句的

![image-20210427100252056](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427100252056.png)



### 2.3 使用

```java
public static void main(String[] args) throws IOException {

    String resource = "mybatis-config.xml";

    // 获取核心配置文件的输入流
    InputStream inputStream = Resources.getResourceAsStream(resource);

    // 获取sqlSessionFactory，这是一个工厂，这个工厂是用来生产SqlSession
    // 这个sqlSession就是用来操作Mybatis的一个主要的类，这个类给我们提供了很多查询的方法，插入的方法
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

    // 获取sqlSession
    SqlSession sqlSession = sqlSessionFactory.openSession();

    // 使用SqlSession对象
    // 这个参数是你sql语句的坐标 ，而不是sql语句 这个坐标是 namespace + id
    List<Account> accountList = sqlSession.selectList("a.selectAll");

    // 输出
    System.out.println(accountList);

    // 关闭sqlSession
    sqlSession.close();


}
```

### 2.4 Mybatis的动态代理

使用Mybatis的动态代理有几个规则必须遵守

1. 接口的全限定名称必须和 mapper文件的namespace对应起来

   ![image-20210427110945692](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427110945692.png)

   

2. 接口的方法名必须和 mapper.xml配置文件里面<select> <update> <delete> <insert> 标签的id值对应起来

   ![image-20210427111023239](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427111023239.png)

   

3. 方法的返回值类型必须和mapper.xml里面的resultType对应起来

![image-20210427111113220](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427111113220.png)

4. 参数的类型和名字必须和mapper.xml里面取值的类型对应起来

![image-20210427111623589](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427111623589.png)



第一点和第二点对应起来是为了帮助我们去定位到对应给的sql语句，然后执行

第三点是为了告诉我们的Mybatis底层是采用sqlSession. selectList，或者是SelectOne，或者是Update等方法

第四点是为了方便我们去传值



### 2.5 增删改查

- 增

  mapper

  ```java
  // 插入
  int insertAccount(Account account);
  ```

  mapper.xml

  ```xml
  <insert id="insertAccount" parameterType="com.cskaoyan.vo.Account">
      insert into account values (#{id},#{name},#{money},#{role})
  </insert>
  ```

- 删除

  mapper

  ```java
  // 删除
  int deleteAccountById(Integer id);
  ```

  mapper.xml

  ```xml
  <delete id="deleteAccountById" parameterType="int">
      delete from account where id = #{id}
  </delete>
  ```

- 改

  mapper

  ```java
  // 修改
  int updateAccountById(Account account);
  ```

  mapper.xml

  ```xml
  <update id="updateAccountById" parameterType="com.cskaoyan.vo.Account">
      update account set name = #{name} where id = #{id}
  </update>
  ```

- 查

  mapper

  ```java
  Account selectById(Integer id);
  ```

  mapper.xml

  ```xml
  <select id="selectById" parameterType="int" resultType="com.cskaoyan.vo.Account">
      select * from account where id = #{id}
  </select>
  ```



## 3. 配置

```xml
<configuration>

    <!--环境配置-->
    <environments default="development">

        <!--环境配置-->
        <environment id="development">
            <!-- 这个是事务管理器，默认是JDBC，这里开放出来是为了方便大家后续使用别的容器去管理我们的事务
            例如，tomcat, Spring -->
            <transactionManager type="JDBC"/>

            <!-- 数据源  有三个配置 UNPOOLED|POOLED|JNDI POOLED表示池化的数据源-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/30sql?characterEncoding=utf8&amp;useSSL=false&amp;serverTimezone=Asia/Shanghai"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>

    </environments>


    <!-- 表示我们的Mybatis需要加载的mapper.xml 所有的都需要加载-->
    <mappers>

        <!--第一种配置方式 直接配置文件 -->
        <!--<mapper resource="com/cskaoyan/mapper/UserMapper.xml"/>-->

        <!--第二种配置方式 配置到包，表示扫描这个包下的所有内容-->
        <package name="com.cskaoyan.mapper"/>

    </mappers>
</configuration>
```



### 3.1 maven识别Java里面配置文件

```xml
<!-- 编译的配置 我们可以指定我们的maven工程在编译的时候去哪个路径下找资源文件--><build>    <resources>        <resource>            <directory>src/main/resources</directory>            <includes>                <!-- 两个* 表示目录的通配 -->                <include>**</include>            </includes>        </resource>        <resource>            <directory>src/main/java</directory>            <includes>                <include>**/*.xml</include>            </includes>        </resource>    </resources></build>
```



### 3.2 properties

我们可以利用Mybatis去读取properties配置文件，并且赋值

![image-20210427150643474](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427150643474.png)



![image-20210427150707094](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427150707094.png)



### 3.3 别名

其实就是我们可以给我们自己定义的类去起名字，并且让mybatis知道。

我们发现别名不区分大小写。 起了别名，代码的可读性就会变差，所以有的人喜欢用别名，觉得省事，有的人不喜欢用别名。

```xml
<!--别名配置--><typeAliases>    <typeAlias type="com.cskaoyan.vo.Student" alias="stu"/></typeAliases>
```



其实Mybatis有很多内置的别名

![image-20210427151454201](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427151454201.png)

![image-20210427151509013](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427151509013.png)



### 3.4 setting 配置

里面有很多配置，这些配置可以改变Mybatis运行的方式，例如缓存，懒加载这些后续会详细讲，目前而言，需要大家配置一个日志相关的配置

```xml
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

配置日志可以让我们更加方便的去查找BUG



## 4. 输入映射

输入映射其实就是我们的Mybatis如何去传参，如果把参数传入到我们的SQL语句里面

> 注意：
>
> 	<SELECT> 这个标签必须得有resultType，也就是必须得有返回值类型，
> 	    	 当我们传入简单参数的时候，可以没有parameterType
> 		
> 	<insert> <update> <delete> 这些标签因为标签里面都是增删改语句，增删改语句的返回值都是影响的行数，所以这些标签不用写resultType,但是得写parameterType,得告诉Mybatis我们传入的参数的类型

### 4.1 一个简单参数

简单参数：其实就是指Java里面基本类型以及String

![image-20210427155926104](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427155926104.png)



### 4.2 传入多个简单参数

![image-20210427160543129](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427160543129.png)

### 4.3 传入对象

![image-20210427162256751](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427162256751.png)



### 4.4 使用Map传值

![image-20210427163050506](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427163050506.png)

我们可以发现，使用Map传值不够好，为什么不够好呢？

map传值的话，我们作为使用者就不知道我们如何去往这个map里面塞值，不够透明，假如我们作为借口的开发者的话，那么我们在SQL语句里面就不知道如何去取值，因为你不知道前面调用的人是如何传值的



使用Map 传值不推荐大家使用

### 4.5 使用位置传值

![image-20210427164302306](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427164302306.png)



第一种方式：

arg0：表示第一个参数

arg1：表示第二个参数

arg2：表示第三个参数

第二种方式：

param1：表示第一个参数

param2：表示第二个参数

param3：表示第三个参数



使用位置传值比较容易出错，因为是和位置相关的，那么假如传入的顺序反了的话，底层的sql的代码也要修改，不容易发现，难以维护。还有一点是使用位置传值的代码可读性差



### 4.6 #和$的区别（面试）

我们可以通过#{} 来取值，我们也可以通过${}来取值，这是两种取值的方式，那么他们有什么区别呢？

![image-20210427171625756](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427171625756.png)



我们通过日志可以发现，#采用的是预编译的方式。$采用的是字符串拼接的方式，那么使用$符号去取值的话，就可能会有数据库注入的风险。



那么是不是说我们的$没有用武之地呢？

我们在以后开发中，以下场景需要使用 $去取值，不然会报错

1. 传入表名，列名，需要使用$取值
2. order by，group by 后面跟的列名都需要使用$符号去取值



- 传入表名

  mapper

  ```java
  // 把表名传入sqlStudent selectStudentByTableNameAndId(@Param("tableName") String tableName,                                      @Param("id") Integer id);
  ```

  mapper.xml

  ```xml
  <select id="selectStudentByTableNameAndId" resultType="stu">
      select * from ${tableName} where id = #{id}
  </select>
  ```

我们需要通过${}去获取表名，并且给他拼接到sql语句里面，这样我们sql预编译的时候才能通过。

通常和数据库分表搭配起来使用，也就是说，当我们某一个表的数据量很大的时候，我们需要对表进行拆分，把这些数据存到多个结构相同的表里面去，然后我们在查询的时候，我们就可以通过一个路由规则确定到某一个表，然后再把表名传到sql里面去。



- 传入列名

  通常传入列名的使用场景就是 order by、group by 

  ![image-20210427173858557](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210427173858557.png)

  

当我们需要在sql里面通过$ 符号去取值的时候，因为我们使用$符号去取值有数据库注入的问题，所以${取值} ,大括号里面的值不能交给用户自己输入，也就不会产生数据库注入的风险。

## 5. 插件

### 5.1 Mybatis的插件

![image-20210428094708885](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210428094708885.png)



### 5.2 Lombok

这个插件是用来生成getter、setter、toString、hashcode等方法的。

如何使用呢？

- 导包

  ```xml
  <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>1.18.12</version>
  </dependency>
  ```



- 使用

  可以通过注解去给Java对象去生成 getter方法，settter方法、toString方法等等。**lombok是在编译的时候生效**

  

  ![image-20210428100944653](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210428100944653.png)



@Getter 给对应的类去生成getter方法

@Setter 生成setter方法

@ToString 生成toString方法

@NoArgsConstructor 生成无参构造

@AllArgsConstructor 生成全参构造



@Data 生成getter、setter、hashcode、equals、toString这些方法



## 6. 输出映射

输出映射其实就是Mybatis获取到了结果集，然后把结果集映射到Java对象里面去

### 6.1 简单类型

mapper

```java
// 获取单个简单参数
String selectNameById(@Param("id") Integer id);

// 获取多个简单参数
List<String> selectNameList();

// 获取多个简单参数
String[] selectNameArray();
```

mapper.xml

```xml
<select id="selectNameById" resultType="string">
    select name from student where id = #{id}
</select>

<select id="selectNameList" resultType="java.lang.String">
    select name from student
</select>

<select id="selectNameArray" resultType="java.lang.String">
    select name from student
</select>
```



测试

```java
// 获取一个简单参数
@Test
public void testSelectNameById() {

    String name = studentMapper.selectNameById(1);

    System.out.println("name:" + name);

}

@Test
public void testSelectAllName() {

    List<String> nameList = studentMapper.selectNameList();

    System.out.println("nameList:" + nameList);

}

@Test
public void testSelectAllName2() {

    String[] nameList = studentMapper.selectNameArray();

    for (String s : nameList) {
        System.out.println(s);
    }

}
```

我们发现，获取单个简单类型的时候，不管接口的返回值是什么类型的，我们的SQL只需要和返回值类型里面的单个bean对应起来即可

![image-20210428102954281](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210428102954281.png)



### 6.2 Java对象

mapper

```java
// 获取单个Java对象Student selectStudentById(@Param("id") Integer id);// 获取一个Java对象的集合List<Student> selectStudentList();// 查一个对象的数组Student[] selectStudentArray();// 问题 需要列名和成员变量的名字对应 假如没有对应会怎样呢？// 获取单个Java对象Student2 selectStudentById2(@Param("id") Integer id);
```

mapper.xml

```xml
    <select id="selectStudentById" resultType="com.cskaoyan.vo.Student">        select * from student where id =#{id}    </select>    <select id="selectStudentList" resultType="com.cskaoyan.vo.Student">    select * from student</select>    <select id="selectStudentArray" resultType="com.cskaoyan.vo.Student">        select * from student    </select>    <select id="selectStudentById2" resultType="com.cskaoyan.vo.Student2">        select id as id, name as username, age as age, gender as gender,birthday as birthday from student where id =#{id}    </select>
```

测试

```java
@Testpublic void testSelectById(){    Student student = studentMapper.selectStudentById(1);    System.out.println(student);}@Testpublic void testSelectStudentList(){    List<Student> students = studentMapper.selectStudentList();    students.stream().forEach(student -> System.out.println(student));    students.forEach(student -> System.out.println(student));}@Testpublic void testSelectStudentArray(){    Student[] students = studentMapper.selectStudentArray();    for (Student student : students) {        System.out.println(student);    }}@Testpublic void testSelectById2(){    Student2 student = studentMapper.selectStudentById2(1);    System.out.println(student);}
```



假如我们的列名和 对应的JavaBean里面的成员变量的名字对应不上，那么Mybatis在进行封装的时候就封装不进去，导致得到的JavaBean里面对应不上的成员变量的值为null，如何解决这个问题呢？

1. 可以通过起别名的方式来解决这个问题
2. 可以通过resultMap的方式解决这个问题

### 6.3 ResultMap

mapper

```java
// ResultMap解决问题Student2 selectStudentWithResultMap(@Param("id") Integer id);
```

mapper.xml

```xml
<!-- 映射的map  --><resultMap id="studentMap" type="com.cskaoyan.vo.Student2">    <!--主键使用id标签，普通字段使用result标签-->    <!--column 是数据库中的列名  property:是java对象中的成员变量名-->    <!-- jdbcType是表中列的数据类型  javaType是成员变量的类型 这两个可以省略-->    <id column="id" property="id"/>    <result column="name" property="username" javaType="java.lang.String" jdbcType="VARCHAR"/>    <result column="age" property="age"/>    <result column="gender" property="gender"/>    <result column="birthday" property="birthday"/></resultMap><!-- 插入的入口 --><select id="selectStudentWithResultMap" resultMap="studentMap">    select id,name,age,gender,birthday from student where id = #{id}</select>
```

![image-20210428112428449](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210428112428449.png)



在我们目前看来，我们的resultMap去做映射的时候，其实是不如别名来的方便， 但是他有自己的用途。

在我们使用Mybatis进行多表查询的时候，这个resultMap就很有用了。

## 7. 动态SQL

Mybatis的动态SQL是指Mybatis可以根据输入的参数动态去改变SQL，然后得到不同的返回结果

mybatis的动态SQL依赖于Mybatis提供的标签库

### 7.1 where

- 给我们去拼接一个where关键字

mapper

```java
//    where标签Student selectStudentByIdWithWhere(@Param("id") Integer id);
```

mapper.xml

```xml
<select id="selectStudentByIdWithWhere" resultType="com.cskaoyan.vo.Student">    select * from student    <where>        id = #{id}    </where></select>
```



![image-20210428114635538](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210428114635538.png)



- 作用二

  可以帮助我们去除最近的and或者是or关键字

  ```xml
  <select id="selectStudentByIdWithWhere" resultType="com.cskaoyan.vo.Student">    select * from student    <where>        <if test="id != null">            and id = #{id}        </if>        <if test="id == null">            and id = 2        </if>    </where></select>
  ```

  sql:

  ![image-20210428115212717](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210428115212717.png)

  

### 7.2 if

这个标签可以帮助我们去动态的改变sql

mapper

```java
//    根据条件去查询StudentList<Student> selectStudentBySelective(@Param("student") Student student);
```

mapper.xml

```xml
<select id="selectStudentBySelective" resultType="com.cskaoyan.vo.Student">
    select * from student
    <where>
        <if test="student.id != null">
           and id = #{student.id}
        </if>
        <if test="student.name != null">
           and name = #{student.name}
        </if>
        <if test="student.age != null">
           and  age > #{student.age}
        </if>
        <if test="student.gender != null">
           and  gender = #{student.gender}
        </if>
    </where>
</select>
```

测试：

```java
@Test
public void testSelectStudentBySelective(){


    Student student = new Student();

    student.setAge(25);
    student.setGender("male");

    List<Student> students = studentMapper2.selectStudentBySelective(student);

    System.out.println(students);
}
```



<if test="这里是一个OGNL表达式"></if>

OGNL其实是之前用的一种语法格式，这个单独的语法最后的结果是一个boolean类型的值，但是大于，小于有特殊的写法

在xml文件里面， 我们常见的转义字符有哪些？

```xml
&			&amp;>			&gt;<			&lt;>=			&gt;=<=			&lt;=
```

在ognl表达式里面，也有不同的写法

```xml
>			gt<			lt>=			gte<=			lte
```

不为空： != null

为空		== null 			is null



### 7.3 choose when otherwise

相当于我们Java里面的if else

```xml
<select id="selectStudentBySelective3" resultType="com.cskaoyan.vo.Student">
    select * from student
    <where>
        <choose>
            <when test="student.age gte 25">
                and age &gt;= 25
            </when>
            <otherwise>
                and age &lt; 25
            </otherwise>
        </choose>
    </where>
</select>
```



### 7.4  sql-include

这个标签是可以帮助我们提取一些公共的sql片段，提取出来了以后这些SQL片段就能够反复使用

![image-20210429101754684](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210429101754684.png)



我们一般利用sql片段提取表的列名，这样就可以反复使用



### 7.5 trim

这个trim标签是修剪的意思，其实就是可以通过这个标签给我们的sql语句去增加一个字段或者是减少一些字段

```xml
<update id="updateStudentByIdSelective" parameterType="stu">

    update student
    
    <!--
	prefix: 增加指定的前缀
	suffix: 增加指定的后缀
	
	prefixOverrides: 删除指定的前缀
	suffixOrverrides: 删除指定的后缀
	-->
    
    <trim prefix="set" suffixOverrides=",">
        <if test="student.name != null">
            name = #{student.name},
        </if>
        <if test="student.age != null">
            age = #{student.age},
        </if>
        <if test="student.gender != null">
            gender = #{student.gender},
        </if>
    </trim>
    <where>
        id = #{student.id}
    </where>
</update>
```

### 7.6 set

​    <trim prefix="set" suffixOverrides=","> 这个就相当于<set>标签

```xml
<update id="updateStudentByIdSelective2" parameterType="stu">    update student    <set>        <if test="student.name != null">            name = #{student.name},        </if>        <if test="student.age != null">            age = #{student.age},        </if>        <if test="student.gender != null">            gender = #{student.gender},        </if>    </set>    <where>        id = #{student.id}    </where></update>
```



### 7.7 foreach

循环的意思，有两个使用场景

- 插入一个List 或者是数组

  ![image-20210429112625457](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210429112625457.png)

  数组

  ![image-20210429112733989](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210429112733989.png)

- in查询

  mapper

  ```java
  // 根据一个id的list去查询学生List<Student> selectStduentListByIds(@Param("ids") List<Integer> ids);
  ```

  mapper.xml

  ```xml
  <select id="selectStduentListByIds" resultType="com.cskaoyan.vo.Student">    select <include refid="base_column"/>    from student    where id in    <!--    collection: 代表集合的名字，        如果参数有@param注解，那么就是注解的值        如果没有注解，那么就是collection或者array            item: 表示集合中的每个元素    open: 表示给循环前面增加指定的字符    close: 给循环后面增加指定的字符    separator: 集合中的每一个元素循环以后以什么符号分隔开来    index: 元素的下标    -->    <foreach collection="ids" item="id" open="(" close=")" separator=",">        #{id}    </foreach></select>
  ```



### 7.8 selectKey

这个标签可以帮助我们在执行sql语句的前后去额外的执行一条sql语句

通常的场景是可以帮助我们获取插入到数据库里面自增的id值

mapper

```java
// 插入一个学生Integer insertStudent(@Param("student") Student student);
```

mapper.xml

```xml
<insert id="insertStudent" parameterType="stu">    insert into student values (null,#{student.name},#{student.age},#{student.gender},#{student.birthday})    <!--	order: 表示执行的顺序	resultType: 表示执行结果返回的类型	keyProperty: 表示返回的结果封装到外面哪个字段里面去	-->    <selectKey order="AFTER" resultType="integer" keyProperty="student.id">        select LAST_INSERT_ID()    </selectKey></insert>
```

测试

如何取值呢？

```
@Testpublic void testInsertStudent(){    Student student = new Student();    student.setName("兰钊");    student.setAge(40);    student.setGender("male");    student.setBirthday(new Date());    Integer affectedRows = studentMapper2.insertStudent(student);    System.out.println("affectedRows:" + affectedRows);		// 取值方式    Integer id = student.getId();    System.out.println("自增的id:" + id);}
```

### 7.9 useGeneratedKeys 

这个是一个配置，这个也是用来帮助我们获取主键的

```xml
<insert id="insertStudentUseGeneratedKeys" parameterType="stu" useGeneratedKeys="true" keyProperty="student.id">    insert into student values (null,#{student.name},#{student.age},#{student.gender},#{student.birthday})</insert>
```

如何获取值：

![image-20210429143854146](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210429143854146.png)



## 8. 多表查询（难点）

### 8.1 一对一

表设计

![image-20210429155501426](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210429155501426.png)



- 分次查询

  其实就是采用多次sql，把结果查询出来

  

  ```xml
  <resultMap id="studentMap2" type="com.cskaoyan.vo.Student">
      <id column="id" property="id"/>
      <result column="username" property="username"/>
      <result column="password" property="password"/>
  
      <!--接下来要去封装userDetail这个参数-->
      <!--
  		association标签，去封装单个bean
  		property: Bean里面的成员变量的名字
  		javaType: 成员变量的类型
  		select: 指第二次查询的sql语句
  		column: 给第二次查询传递的列名
  	-->
      <association property="studentDetail"
                   javaType="com.cskaoyan.vo.StudentDetail"
                   select="selectStudentDetailByStudentId"
                   column="id"/>
  </resultMap>
  
  <!-- 分次查询 查询的入口-->
  <select id="selectStudentByIdUseMultiCount" parameterType="int" resultMap="studentMap2">
      select id,username,password from student where id = #{id}
  </select>
  
  <!--第二次查询 就是通过 studentId去查询 studentDetail-->
  <select id="selectStudentDetailByStudentId" parameterType="int" resultType="com.cskaoyan.vo.StudentDetail">
      select id,student_id as studentId,height,weight,age,comment from student_detail where student_id = #{id}
  </select>
  ```

- 连接查询

  连接查询其实就是通过写连接查询语句把所有的字段都查询出来，然后封装到bean里面去

  ```xml
  <!-- 一对一连接查询 -->
  
  <resultMap id="studentMap3" type="com.cskaoyan.vo.Student">
      <id column="id" property="id"/>
      <result column="username" property="username"/>
      <result column="password" property="password"/>
  	
    	<!--
  	封装查询的结果到对应的成员变量
  	-->  
      <association property="studentDetail" javaType="com.cskaoyan.vo.StudentDetail">
          <id column="sid" property="id"/>
          <result column="student_id" property="studentId"/>
          <result column="height" property="height"/>
          <result column="weight" property="weight"/>
          <result column="age" property="age"/>
          <result column="comment" property="comment"/>
      </association>
  
  </resultMap>
  
  <!-- 连接查询-->
  <select id="selectStudentByIdUseCrossJoin" parameterType="int" resultMap="studentMap3">
      SELECT
              *,
              sd.id AS sid
      FROM
              student AS s,
              student_detail AS sd
      WHERE
              s.id = sd.student_id
              and
              s.id = #{id}
  </select>
  ```



#### 8.1.2 建立模板

![image-20210429161533082](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210429161533082.png)



### 8.2 一对多

建立模型

![image-20210429163830141](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210429163830141.png)

JavaBean映射关系

![image-20210429164028631](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210429164028631.png)



- 分次查询

  mapper

  ```java
  //    一对多的分次查询
      Clazz selectClazzWithNewStudentByClazzId(@Param("id") Integer id);
  ```

  mapper.xml

  ```xml
      <resultMap id="clazzMap" type="com.cskaoyan.vo.Clazz">
  
          <id column="id" property="id"/>
          <result column="name" property="name"/>
  
          <collection property="studentList"
                      ofType="com.cskaoyan.vo.NewStudent"
                      select="selectNewStudentByClazzId"
                      column="id"/>
      </resultMap>
  
      <!-- 分次查询 -->
      <select id="selectClazzWithNewStudentByClazzId" resultMap="clazzMap">
          select * from clazz where id = #{id}
      </select>
  
      <!-- 分次查询第二次查询-->
      <select id="selectNewStudentByClazzId" resultType="com.cskaoyan.vo.NewStudent">
          select id,name,clazz_id as clazzId from student where clazz_id = #{id}
      </select>
  ```

- 连接查询

  mapper

  ```java
  //    一对多的连接查询    Clazz selectClazzWithNewStudentByClazzIdAndCrossJoin(@Param("id") Integer id);
  ```

  mapper.xml

  ```xml
  <resultMap id="clazzMap2" type="com.cskaoyan.vo.Clazz">
      <id column="id" property="id"/>
      <result column="name" property="name"/>
  
      <collection property="studentList" ofType="com.cskaoyan.vo.NewStudent">
          <id column="sid" property="id"/>
          <result column="sname" property="name"/>
          <result column="clazzId" property="clazzId"/>
      </collection>
  
  </resultMap>
  <!-- 一对多的连接查询 -->
  <select id="selectClazzWithNewStudentByClazzIdAndCrossJoin" resultMap="clazzMap2">
      SELECT
              c.id AS id,
              c.NAME AS name,
              s.id AS sid,
              s.NAME AS sname,
              s.clazz_id AS clazzId
      FROM
              clazz AS c
                      LEFT JOIN student AS s ON c.id = s.clazz_id
      WHERE
              c.id = #{id}
  </select>
  ```

### 8.3 多对多

其实和一对多是一样的，只是连接查的时候SQL语句有点区别，一对多连接两张表，多对多连接三张表。

建立模型

学生和选课

![image-20210429171331999](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210429171331999.png)



- 分次查询

  mapper

  ```java
  //    多对多的分次查询NewStudent selectStudentByIdWithCourses(@Param("id") Integer id);
  ```

  mapper.xml

  ```xml
  <resultMap id="studentCourseMap" type="com.cskaoyan.vo.NewStudent">    <id column="id" property="id"/>    <result column="name" property="name"/>    <result column="clazz_id" property="clazzId"/>    <collection property="courseList"                ofType="com.cskaoyan.vo.Course"                select="selectCourseByStudentId"                column="id"/></resultMap><select id="selectStudentByIdWithCourses" resultMap="studentCourseMap">    select * from student where id = #{id}</select><select id="selectCourseByStudentId" resultType="com.cskaoyan.vo.Course">    select id,name from course where id in (select cid  from s_c where sid = #{id})</select>
  ```

- 连接查询

  mapper

  ```java
  // 连接查询NewStudent selectStudentByIdWithCoursesAndCrossJoin(@Param("id") Integer id);
  ```

  mapper.xml

  ```xml
  <resultMap id="studentCourseMap2" type="com.cskaoyan.vo.NewStudent">    <id column="sid" property="id"/>    <result column="sname" property="name"/>    <result column="clazz_id" property="clazzId"/>    <collection property="courseList" ofType="com.cskaoyan.vo.Course">        <id column="cid" property="id"/>        <result column="cname" property="name"/>    </collection></resultMap><!-- 连接查询 --><select id="selectStudentByIdWithCoursesAndCrossJoin" resultMap="studentCourseMap2">    SELECT            s.id AS sid,            s.NAME AS sname,            s.clazz_id AS clazz_id,            c.id AS cid,            c.NAME AS cname    FROM            student AS s                    LEFT JOIN s_c AS sc ON s.id = sc.sid                    LEFT JOIN course AS c ON sc.cid = c.id    WHERE            s.id = #{id}</select>
  ```

## 9. 懒加载

这个是Mybatis优化相关的内容

- 全局配置

  ![image-20210430094541952](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210430094541952.png)



- 局部配置

  当我们不需要全局开启懒加载的时候，我们可以采用局部配置，表示只对对应的SQL语句开启懒加载

  ![image-20210430095409018](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210430095409018.png)



懒加载是什么意思呢？

懒加载只针对分次查询，

当我们开启懒加载的时候，分次查询的第二次查询结果，会在我们需要用到的时候才会去执行sql语句，进行加载。

如果我们关闭了懒加载，那么分次查询的第二次查询，会在执行主查询的时候直接执行。



那我们在以后的工作中要不要使用懒加载呢？

一般来说，可以使用懒加载。



## 10. 缓存

缓存是什么呢？

![image-20210430100634118](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210430100634118.png)



缓存其实就是利用内存访问快的特点，在内存里面去保存一份临时数据，加快我们请求的访问速度。



Mybatis分为一级缓存和二级缓存

### 10.1 一级缓存

一级缓存是默认打开的，是SQLsession级别的缓存（会话级别）

```java
// 同一个mapper
@Test
public void testSelectStu1(){


    NewStudent newStudent = newStudentMapper.selectStudentById(1);  //查询数据库
    NewStudent newStudent2 = newStudentMapper.selectStudentById(1); // 查询缓存
    NewStudent newStudent3 = newStudentMapper.selectStudentById(1); // 查询缓存
    NewStudent newStudent4 = newStudentMapper.selectStudentById(1); // 查询缓存
    NewStudent newStudent5 = newStudentMapper.selectStudentById(1); // 查询缓存

    NewStudent newStudent6 = newStudentMapper.selectStudentById(2); // 查询数据库

}

// 不同的mapper,同一个sqlSession
@Test
public void testSelectStu2(){

    NewStudentMapper mapper1 = sqlSession.getMapper(NewStudentMapper.class);
    NewStudentMapper mapper2 = sqlSession.getMapper(NewStudentMapper.class);
    NewStudentMapper mapper3 = sqlSession.getMapper(NewStudentMapper.class);
    NewStudentMapper mapper4 = sqlSession.getMapper(NewStudentMapper.class);

    NewStudent newStudent1 = mapper1.selectStudentById(1);
    NewStudent newStudent2 = mapper2.selectStudentById(1);
    NewStudent newStudent3 = mapper3.selectStudentById(1);
    NewStudent newStudent4 = mapper4.selectStudentById(1);

}

// 不同的sqlSession
@Test
public void testSelectStu3(){

    SqlSession sqlSession1 = sqlSessionFactory.openSession();
    SqlSession sqlSession2 = sqlSessionFactory.openSession();
    SqlSession sqlSession3 = sqlSessionFactory.openSession();
    SqlSession sqlSession4 = sqlSessionFactory.openSession();


    NewStudentMapper mapper1 = sqlSession1.getMapper(NewStudentMapper.class);
    NewStudentMapper mapper2 = sqlSession2.getMapper(NewStudentMapper.class);
    NewStudentMapper mapper3 = sqlSession3.getMapper(NewStudentMapper.class);
    NewStudentMapper mapper4 = sqlSession4.getMapper(NewStudentMapper.class);

    NewStudent newStudent1 = mapper1.selectStudentById(1);
    NewStudent newStudent2 = mapper2.selectStudentById(1);
    NewStudent newStudent3 = mapper3.selectStudentById(1);
    NewStudent newStudent4 = mapper4.selectStudentById(1);

}
```



总结一下：一级缓存什么时候生效呢？

- 同一个SqlSession
- 同样的SQL语句与参数
- 之前已经在这个SqlSession里面查询过了



什么时候会失效呢？

1. 当我们执行增删改的时候，数据库的数据会改变，那么缓存会失效
2. 当我们执行sqlSession.commit或者是sqlSession.close的时候，缓存会失效

```java
    public void testSelectStu1(){


        NewStudent newStudent = newStudentMapper.selectStudentById(1);  //查询数据库
        NewStudent newStudent2 = newStudentMapper.selectStudentById(1); // 查询缓存
        NewStudent newStudent3 = newStudentMapper.selectStudentById(1); // 查询缓存

        sqlSession.commit(); // 清空缓存
        
        NewStudent newStudent4 = newStudentMapper.selectStudentById(1); // 查数据库
        NewStudent newStudent5 = newStudentMapper.selectStudentById(1); // 查询缓存

//        NewStudent newStudent6 = newStudentMapper.selectStudentById(2); // 查询数据库

    }
```



我们能不能自己实现一个一级缓存呢？如果能，用什么数据结构来实现呢？

可以自己实现一个一级缓存

Map<Key,value>

key: sql语句以及参数

value: 查询的结果



### 10.2 二级缓存



二级缓存的概念：

![image-20210430110236906](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210430110236906.png)



Mybatis 二级缓存和上述有一点小区别

但是Mybatis是先查二级缓存，再查一级缓存

**Mybatis的二级缓存是Namespace级别的缓存**

也就是说，只要我们查询的是同一个命名空间下的sql语句，就会走二级缓存

如何开启Mybatis的二级缓存呢？

- 修改配置，开启二级缓存

  ```xml
  <!-- 开启全局二级缓存-->
  <setting name="cacheEnabled" value="true"/>
  ```

- 给我们对应的Bean实现序列化接口

  开启自动生成UID

  ![image-20210430111123143](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210430111123143.png)



​		实现序列化接口

```
  @Data
  public class NewStudent implements Serializable {
  
      // 这个UID是在你进行反序列化的时候验证
      private static final long serialVersionUID = 8240573104558202820L;
      
      private Integer id;
      private String name;
      private Integer clazzId;
  
      private List<Course> courseList;
  }
```



- 第三步，我们需要去对应的Namespace里面加上`<cache/>`标签，表示开启二级缓存

  ![image-20210430111339355](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210430111339355.png)



使用二级缓存：

我们发现正常情况下二级缓存里面没有数据，也就是命中不了二级缓存

我们需要使用`sqlSession.commit()`或者是`sqlSeesion.close()`去把查询出来的数据放入二级缓存中

![image-20210430112040897](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210430112040897.png)



Mybatis缓存作用的流程：



![image-20210430112938546](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210430112938546.png)

我们使用了Mybatis的二级缓存后，我们可能会产生我们二级缓存区的数据和数据库的数据不一致的问题，这个问题就叫做脏数据的问题。这个问题怎么解决呢？

其实我们在使用缓存的时候，就需要考虑到这样的问题，也就是缓存和数据库不一样的问题，如何去解决这个数据不一致的问题呢？

做法：不解决



因为假如你的数据要求必须缓存里面数据和数据库的最新的数据保持实时一致的话，那么这个时候就没必要用缓存。

如果们要求查询速度一般，但是数据的准确性比较高，这个时候我们应该使用数据库存储数据

如果我们对查询的速度要求很高，但是对数据的准确性要求不高，这个时候我们可以使用数据库+缓存的方式

如果我们对查询你的速度要求很高，对数据的准确性要求也很高，那么这个时候就应该只存缓存（会有一个问题：缓存里面的数据是没有持久化到磁盘的，假如这个时候应用重启或者是服务器down机，缓存里面的数据就会丢失）



如果你要求查询速度快，数据的准确性要求也高，数据还不能丢失，这个问题是各个大公司研究的重点。

Memcache （之前使用的缓存中间件） 不支持持久化到磁盘，理解是一个大号的map

后续针对这个问题，推出了Redis，支持把数据持久化到磁盘

后续针对以上的问题，有的公司又搞出了 ES、Solr这样的搜索引擎来提高查询的效率



结论：我们一般不使用Mybatis的二级缓存，因为他会产生脏数据的问题。



简单带大家看一下缓存的源码：其实也是使用Map来实现的

![image-20210430144459042](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\MyBatis-notes.assets\image-20210430144459042.png)

