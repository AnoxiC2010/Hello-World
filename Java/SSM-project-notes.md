

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



# BeanUtils转换bean

查查询的pojo了和前端需要的参数不一致

org.springframework.beans.BeanUtils

转换需要属性名和类型对应

```java
BeanUtils.copyProperties(bo, admin);
```



# bug

public int deleteAdmin(DeleteAdminBO bo) {

两个update语句存在时

 Cause: com.mysql.jdbc.exceptions.jdbc4.MySQLTransactionRollbackException: Lock wait timeout exceeded; try restarting transaction