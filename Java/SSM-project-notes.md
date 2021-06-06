

# TypeHandler

```java
@Data
public class Admin {
	    private Integer[] roleIds;//配置类型转换器
}
```

```java
package com.cskaoyan.typehandler;

import com.fasterxml.jackson.databind.ObjectMapper;
import lombok.SneakyThrows;
import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedJdbcTypes;
import org.apache.ibatis.type.MappedTypes;
import org.apache.ibatis.type.TypeHandler;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

@MappedTypes(Integer[].class)
@MappedJdbcTypes(JdbcType.VARCHAR)
public class IntegerArrayTypeHandler implements TypeHandler<Integer[]> {

    //Json
    ObjectMapper objectMapper = new ObjectMapper();

    //输入映射 Integer[] → String
    //index:输入映射过程中，预编译 占位符的顺序
    @SneakyThrows
    @Override
    public void setParameter(PreparedStatement preparedStatement, int index, Integer[] integers, JdbcType jdbcType) throws SQLException {
        String value = objectMapper.writeValueAsString(integers);
        preparedStatement.setString(index,value);
    }
    //输出映射 String → Integer[]
    @Override
    public Integer[] getResult(ResultSet resultSet, String columnName) throws SQLException {
        String result = resultSet.getString(columnName);
        return transfer(result);
    }

    @Override
    public Integer[] getResult(ResultSet resultSet, int index) throws SQLException {
        String result = resultSet.getString(index);
        return transfer(result);
    }

    @Override
    public Integer[] getResult(CallableStatement callableStatement, int index) throws SQLException {
        String result = callableStatement.getString(index);
        return transfer(result);
    }

    @SneakyThrows
    private Integer[] transfer(String result) {
        if (result == null || "".equals(result)) {
            return new Integer[0];
        }
        Integer[] integers = objectMapper.readValue(result, Integer[].class);
        return integers;
    }
}

```

```yaml
# application.yml
# jdbc类型转换器
mybatis:
  type-handlers-package: com.cskaoyan.typehandler
```



```java
@MappedTypes(String[].class)
@MappedJdbcTypes(JdbcType.VARCHAR)
public class StringArrayTypeHandler implements TypeHandler<String[]> {

    //Json
    ObjectMapper objectMapper = new ObjectMapper();

    //输入映射 String[] → String
    //index:输入映射过程中，预编译 占位符的顺序
    @SneakyThrows
    @Override
    public void setParameter(PreparedStatement preparedStatement, int index, String[] strings, JdbcType jdbcType) throws SQLException {
        String value = objectMapper.writeValueAsString(strings);
        preparedStatement.setString(index,value);
    }
    //输出映射 String → String[]
    @Override
    public String[] getResult(ResultSet resultSet, String columnName) throws SQLException {
        String result = resultSet.getString(columnName);
        return transfer(result);
    }

    @Override
    public String[] getResult(ResultSet resultSet, int index) throws SQLException {
        String result = resultSet.getString(index);
        return transfer(result);
    }

    @Override
    public String[] getResult(CallableStatement callableStatement, int index) throws SQLException {
        String result = callableStatement.getString(index);
        return transfer(result);
    }

    @SneakyThrows
    private String[] transfer(String result) {
        if (result == null || "".equals(result)) {
            return new String[0];
        }
        String[] strings = objectMapper.readValue(result, String[].class);
        return strings;
    }
}
```



# BeanUtils转换bean

查查询的pojo了和前端需要的参数不一致

org.springframework.beans.BeanUtils

转换需要属性名和类型对应

```java
BeanUtils.copyProperties(bo, admin);
```



# 文件上传

[Spring Boot应用上传文件时报错 - nuccch - 博客园 (cnblogs.com)](https://www.cnblogs.com/nuccch/p/11546494.html)

```yaml
# application.yml
# 文件上传配置
  servlet:
    multipart:
      # 启用
      enabled: true
      max-file-size: 1MB
      # 上传是产生的临时文件目录 位于 例如/tmp/tomcat.6574404581312272268.18333/work/Tomcat/localhost/ROOT + /temp
      location: /temp
# 静态资源映射
  web:
    resources:
      # 映射文件夹
      static-locations: file:${mall.storage-base-path}
  mvc:
    # 映射url
    static-path-pattern: ${mall.static-path-pattern-prefix}*
# 文件本地存储路径 个人可按照自己需要修改(注释人只有C盘)
mall:
  # 域名
  domain: http://localhost:8083
  # 图片url映射前缀
  static-path-pattern-prefix: /wx/storage/fetch/
  # 图片本地存储路径
  storage-base-path: c:/D/mall/img
```



# 密码散列

```java
/**
 * 注解
 * 添加到需要散列密码的方法上
 * 检查参数里的psssword属性
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface HashPasswordAnno {

}
```



```java
@Aspect
@Component
public class HashPasswordAspect {
    //切点
    @Pointcut("@annotation(com.cskaoyan.anno.HashPasswordAnno)")
    public void hashPasswordPointCut() {
    }

    /**
     * 在执行Handler方法之前，检查参数中名为password的属性，并散列
     * 在需要散列密码的Handler方法上加 注解 @HashPasswordAnno 即可
     * 散列方法位于util.StringUtils.sha250(string raw)
     * 可以按需要修改散列方法的内部实现
     * @param joinPoint
     */
    @Before("hashPasswordPointCut()")
    public void hashPasswordBeforeController(JoinPoint joinPoint) {
        //获得方法参数
        Object[] args = joinPoint.getArgs();
        //检查password（如果有的话）属性并散列
        try {
            for (Object arg : args) {
                Class<?> aClass = arg.getClass();
                Field passwordField = aClass.getDeclaredField("password");
                if (passwordField != null) {
                    passwordField.setAccessible(true);
                    String rawPassword = (String) passwordField.get(arg);
                    String hashPassword = StringUtils.sha250(rawPassword);
                    passwordField.set(arg, hashPassword);
                    break;
                }
            }
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        }

    }
}
```



# 操作日志记录



# bug

public int deleteAdmin(DeleteAdminBO bo) {

两个update语句存在时

 Cause: com.mysql.jdbc.exceptions.jdbc4.MySQLTransactionRollbackException: Lock wait timeout exceeded; try restarting transaction