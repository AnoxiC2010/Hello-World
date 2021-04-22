# JDBC notes

# 数据库的访问过程

根据之前我们所学过的知识，其实不难知道，数据库的访问过程如下：

- <span style="color:red">客户端与Mysql服务器之间建立连接</span>
- <span style="color:red">客户端向Mysql服务器发送数据库请求</span>
- <span style="color:red">Mysql服务器处理客户端请求，并返回结果给客户端</span>
- <span style="color:red">客户端接受Mysql服务器的响应，并按照自己的业务逻辑做响应处理。</span>
- <span style="color:red">释放相关资源。</span>

```
１、注册驱动
ＳＵＮ公司定义了ＪＤＢＣ只是一些接口，用户要想操作数据库，需要先把数据库的驱动，也就是ＪＤＢＣ的实现类拿到程序里来，这个操作称之为注册驱动。Sun公司定义了DriveManager类用于完成驱动程序的注册，示例：
DriverManager.registerDrive(new com.jdbc.mysql.Driver());

2、驱动注册后，就可以创建与数据库的连接了。
Connection connection = DriverManager.getConnection();

3、获得连接后，就可以在此连接上创建一条ＳＱＬ语句，并发送给数据库
Statement st = connection.createStatement();
ResultSet set = St.excute(“select * from user”);
```



# JDBC简介 

**JDBC是JAVA语言定义好的一套访问数据库的接口。**

其实就是Java帮助我们去访问数据库的这么一个客户端。

- 数据库驱动
- SUN公司为了简化、统一对数据库的操作，定义了一套Java操作数据库的<span style="color:red">规范</span>，称之为JDBC。

![image-20210421184208794](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\JDBC-notes.assets\image-20210421184208794.png)

Sun公司为简化数据库开发，定义了一套JDBC接口，这套接口由数据库厂商去实现，这样，开发人员只需要学习JDBC接口，并通过JDBC加载具体的驱动，就可以操作数据库。

![image-20210421184342335](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\JDBC-notes.assets\image-20210421184342335.png)



JDBC全称为：Java Data Base Connectivity（java数据库连接），它主要由接口组成。
组成JDBC的２个包：

- java.sql
- javax.sql

<span style="color:red">开发JDBC应用需要以上2个包的支持外，还需要导入相应JDBC的数据库实现</span>(即数据库驱动)。

```
java.sql.*;  是核心包里的
java.sql.*是jdbc2.0之前的东西
javax.sql.*; 是扩展包中的
javax.sql.*包括了jdbc3.0的特性
```



# 第一个JDBC程序

## 导包

jar包有一个下载地址：[下载地址](https://mvnrepository.com/)

下载完了以后，会得到一个jar包，这个jar包是什么东西呢？其实就是一种压缩格式。这个压缩包里面存放的都是`.class`文件。

- 新建一个工程
- 新建一个文件夹 directory
- 把我们下载的jar丢到对应的目录下去
- 右键文件夹，添加为library



## 编写一个程序

这个程序从user表中读取数据，并打印在命令行窗口中。

1. 搭建实验环境 ：

   - 在mysql中创建一个库，并创建user表和插入表的数据。
   - 新建一个Java工程，并导入数据驱动。

2. 编写程序，在程序中加载数据库驱动
   `DriverManager. registerDriver(Driver driver) `

3. 建立连接(Connection)

   ```java
   Connection conn = DriverManager.getConnection(url,user,pass); 
   ```

4. 创建用于向数据库发送SQL的Statement对象，并发送sql

   ```java
   Statement st = conn.createStatement();
   ResultSet rs = st.executeQuery(sql);
   ```

5. 从代表结果集的ResultSet中取出数据，打印到命令行窗口

6. 断开与数据库的连接，并释放相关资源



不好的写法：

```java
public static void main(String[] args) {

        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;
        try {
            // 注册驱动
            DriverManager.registerDriver(new Driver());
            // 建立连接
            connection = DriverManager.getConnection(url, username, password);
            // 获取statement对象
            // 这个statement 对象是什么呢？ 这个对象是用来执行sql语句并且给你返回结果的
            statement = connection.createStatement();
            // 执行sql语句 获取结果集
            resultSet = statement.executeQuery("select * from teacher");

            // 解析结果集 ret= true表示获取到了数据
            boolean ret = resultSet.next();
            if (!ret) {
                System.out.println("执行sql没有获取到结果");
            }else {

                int id = resultSet.getInt("id");
                String name = resultSet.getString("name");
                String major = resultSet.getString("major");

                System.out.println("id:" + id + ", name:" + name + ", major:" + major);
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            // 关闭资源
            // 要从下往上关闭
            try {
                if (resultSet != null) {
                    resultSet.close();
                }
                if (statement != null){
                    statement.close();

                }
                if (connection != null) {
                    connection.close();
                }
            }catch (SQLException ex){
                ex.printStackTrace();
            }

        }
    }
```

优化的写法：

```java
/**
 * 优化：
 *  1. 解析全部的结果集
 *  2. 配置文件
 *  3. 加载驱动优化
 */
public class JDBCDemo2 {

    static String url = null;
    static String username = null;
    static String password = null;
    static String driverClassName = null;

    static {


        try {
            // 加载配置文件
            Properties properties = new Properties();
            properties.load(new FileInputStream("jdbc.properties"));
            // 赋值
            url = properties.getProperty("url");
            username = properties.getProperty("username");
            password = properties.getProperty("password");
            driverClassName = properties.getProperty("driverClassName");

        } catch (IOException e) {
            e.printStackTrace();
        }


    }



    public static void main(String[] args) {

        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;

        ArrayList<Teacher> teachers = new ArrayList<>();
        try {
            // 注册驱动
//            DriverManager.registerDriver(new Driver());
            // 通过反射注册驱动,驱动类的静态代码块里会注册驱动
            Class<?> aClass = Class.forName(driverClassName);
            // 建立连接
            connection = DriverManager.getConnection(url, username, password);
            // 获取statement对象
            // 这个statement 对象是什么呢？ 这个对象是用来执行sql语句并且给你返回结果的
            statement = connection.createStatement();
            // 执行sql语句 获取结果集
            resultSet = statement.executeQuery("select * from teacher");

            // 解析结果集 ret= true表示获取到了数据
            while (resultSet.next()) {

                int id = resultSet.getInt("id");
                String name = resultSet.getString("name");
                String major = resultSet.getString("major");
                System.out.println("id:" + id + ", name:" + name + ", major:" + major);

                Teacher teacher = new Teacher();
                teacher.setId(id);
                teacher.setName(name);
                teacher.setMajor(major);

                teachers.add(teacher);
            }


            System.out.println("teachers:" + teachers);

        } catch (SQLException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            // 关闭资源
            // 要从下往上关闭
            try {
                if (resultSet != null) {
                    resultSet.close();
                }
                if (statement != null){
                    statement.close();

                }
                if (connection != null) {
                    connection.close();
                }
            }catch (SQLException ex){
                ex.printStackTrace();
            }

        }
    }
}

/*配置文件 jdbc.properties
url=jdbc:mysql://localhost:3306/30sql
username=root
password=123456
driverClassName=com.mysql.jdbc.Driver
*/
```

## 构建JDBC工具类

```java
public class JDBCUtils {


    static String url = null;
    static String username = null;
    static String password = null;
    static String driverClassName = null;

    static {


        try {
            // 加载配置文件
            Properties properties = new Properties();
            properties.load(new FileInputStream("jdbc.properties"));
            // 赋值
            url = properties.getProperty("url");
            username = properties.getProperty("username");
            password = properties.getProperty("password");
            driverClassName = properties.getProperty("driverClassName");

        } catch (IOException e) {
            e.printStackTrace();
        }


    }
    
    
    // 提供一个方法，和数据库之间建立连接
    public static Connection getConnection(){

        Connection connection = null;

        ArrayList<Teacher> teachers = new ArrayList<>();
        try {
            // 注册驱动
//            DriverManager.registerDriver(new Driver());
            // 通过反射注册驱动
            Class<?> aClass = Class.forName(driverClassName);
            // 建立连接
            connection = DriverManager.getConnection(url, username, password);
        }catch (SQLException ex) {
            ex.printStackTrace();

        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        
        return connection;
    }
    
    //  提供一个方法，关闭资源
    public static void releaseSource(Connection connection,Statement statement,ResultSet resultSet) {

        // 关闭资源
        // 要从下往上关闭
        try {
            if (resultSet != null) {
                resultSet.close();
            }
            if (statement != null){
                statement.close();

            }
            if (connection != null) {
                connection.close();
            }
        }catch (SQLException ex){
            ex.printStackTrace();
        }

    } 
}
```



## 程序详解—DriverManager

JDBC程序中的DriverManager用于加载驱动，并创建与数据库的链接，这个API的常用方法：

`DriverManager.registerDriver(new Driver())`
`DriverManager.getConnection(url, user, password)`



## 数据库URL

URL用于标识数据库的位置，程序员通过URL地址告诉JDBC程序连接哪个数据库，URL的写法为：
jdbc:mysql:［］//localhost:3306/test <span style="color:red">?参数名=参数值</span>

协议  子协议             主机:端口     数据库

常用数据库URL地址的写法：

- Oracle写法：`jdbc:oracle:thin:@localhost:1521:dbname`
- SqlServer—`jdbc:microsoft:sqlserver://localhost:1433;DatabaseName=dbname`
- MySql—`jdbc:mysql://localhost:3306/dbname`

Mysql的url地址的简写形式： `jdbc:mysql:///sid`

如果你的主机地址默认是localhost  端口是3306

```
serverTimezone=GMT+8
```

### url编码问题

```
SQL语句中还有中文，JDBC查无结果，但数据库查有结果
SQL语句过滤条件中含中文

因为数据库编码为utf-8，但jdbc的url地址没有指定编码格式

在url最后添加?useUnicode=true&characterEncoding=UTF-8
```

### url配置需要注意一下

```properties
url=jdbc:mysql://localhost:3306/30sql?characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai
```

- characterEncoding=utf8 这个是字符集编码的设置
- useSSL=false 我们去连接Mysql数据库的时候默认使用的是SSL安全认证协议，需要安全证书，但是我们没有，所以配置为false让他关闭安全认证，去除警告日志。

- serverTimezone=Asia/Shanghai 这个是时区的设置

## 程序详解—Connection

JDBC程序中的Connection，它用于代表数据库的连接（桥梁）， Connection是数据库编程中最重要的一个对象，客户端与数据库所有交互都是通过connection对象完成的，这个对象的常用方法：

- createStatement()：创建向数据库发送sql的statement对象。
- prepareStatement(sql) ：创建向数据库发送预编译sql的PrepareSatement对象。
- <span style="color:blue">setAutoCommit(boolean autoCommit)：设置事务是否自动提交。 </span>
- <span style="color:blue">secommit() ：在链接上提交事务。</span>
- <span style="color:blue">serollback() ：在此链接上回滚事务。</span>



## 程序详解—Statement

JDBC程序中的Statement对象用于向数据库发送SQL语句， Statement对象常用方法：

- executeQuery(String sql) ：用于向数据发送查询语句。
- executeUpdate(String sql)：用于向数据库发送insert、update或delete语句
- execute(String sql)：用于向数据库发送任意sql语句



## 程序详解—ResultSet

Jdbc程序中的ResultSet用于代表Sql语句的执行结果。Resultset封装执行结果时，采用的类似于表格的方式。ResultSet 对象维护了一个指向表格数据行的<span style="color:red">游标</span>，<span style="color:red">初始的时候，游标在第一行之前</span>，调用<span style="color:red">ResultSet.next() </span>方法，可以使游标指向具体的数据行，进行调用方法获取该行的数据。
ResultSet既然用于封装执行结果的，所以该对象提供的都是用于获取数据的get方法：

- 获取任意类型的数据
  <span style="color:red">getObject(int index)</span>
  getObject(string columnName)
- 获取指定类型的数据，(封装数据时方便)例如：
  <span style="color:red">getString(int index)</span>
  getString(String columnName)

思考：数据库中列的类型是varchar，获取该列的数据调用什么方法？Int类型呢？bigInt类型呢？Boolean类型？



## 常用数据类型转换表

| SQL类型  数据库           | Jdbc对应方法（resultSet的get...方法） | 返回类型  Java     |
| ------------------------- | ------------------------------------- | ------------------ |
| TINYINT                   | getByte()                             | Byte               |
| SMALLINT                  | getShort()                            | Short              |
| Int                       | getInt()                              | Int                |
| BIGINT                    | getLong()                             | Long               |
| CHAR,VARCHAR,LONG VARCHAR | getString()                           | String             |
| Text(clob) Blob           | getClob  getBlob()                    | Clob Blob          |
| DATE                      | getDate()                             | java.sql.Date      |
| TIME                      | getTime()                             | java.sql.Time      |
| TIMESTAMP                 | getTimestamp()                        | java.sql.Timestamp |



## 程序详解—ResultSet

ResultSet还提供了对结果集的游标进行滚动的方法：

- next()：移动到下一行
- previous()：移动到前一行
- absolute(int row)：移动到指定行
- beforeFirst()：移动resultSet的最前面。
- afterLast() ：移动到resultSet的最后一行之后。
  

## 程序详解—释放资源

Jdbc程序运行完后，切记要释放程序在运行过程中，创建的那些与数据库进行交互的对象，这些对象通常是ResultSet, Statement和Connection对象。

特别是<span style="color:red">Connection对象</span>，它是非常稀有的资源，用完后必须马上释放，如果Connection不能及时、正确的关闭，极易导致系统宕机。Connection的使用原则是<span style="color:red">尽量晚创建，尽量早的释放</span>。

为确保资源释放代码能运行，资源释放代码也一定要放在finally语句中。



## 使用JDBC对数据库进行CRUD

JDBC中的statement对象用于向数据库发送SQL语句，想完成对数据库的增删改查，只需要通过这个对象向数据库发送增删改查语句即可。

Statement对象的executeUpdate方法，用于向数据库发送增、删、改的sql语句，executeUpdate执行完后，将会返回一个整数(即增删改语句导致了数据库几行数据发生了变化)。

Statement.executeQuery方法用于向数据库发送查询语句，executeQuery方法返回代表查询结果的ResultSet对象。

练习：编写程序对User表进行增删改查操作。
练习： 异常处理



### CRUD操作-create

使用executeUpdate(String sql)方法完成数据添加操作，示例操作：

```JAVA
Statement st = conn.createStatement();

String sql = "insert into user(….) values(…..) "; 

int num = st.executeUpdate(sql);

if(num>0){
	System.out.println("插入成功！！！");
}
```

增

```java
public static void main(String[] args) throws SQLException {


    // 获取连接
    Connection connection = JDBCUtils.getConnection();
    // 获取statement对象
    Statement statement = connection.createStatement();
    // 执行sql语句
    int effectedRows = statement.executeUpdate("insert  into teacher values (7,'长风','开车')");

    if (effectedRows < 1) {
        System.out.println("插入数据失败！！！");
    }
    System.out.println("插入成功！！！");
    // 关闭资源
    JDBCUtils.releaseSource(connection,statement,null);

}
```



### CRUD操作-update

使用executeUpdate(String sql)方法完成数据修改操作，示例操作：

```JAVA
Statement st = conn.createStatement();

String sql = “update user set name=‘’ where name=‘’"; 

int num = st.executeUpdate(sql);

if(num>0){
	System.out.println(“修改成功！！！");
}
```

改

```java
public static void main(String[] args) throws SQLException {

    // 获取连接
    Connection connection = JDBCUtils.getConnection();
    // 获取statement对象
    Statement statement = connection.createStatement();
    // 执行sql语句
    int effectedRows = statement.executeUpdate("update teacher set major = '开飞机'");

    if (effectedRows < 1) {
        System.out.println("修改数据失败！！！");
    }
    System.out.println("影响的行数：" + effectedRows);
    // 关闭资源
    JDBCUtils.releaseSource(connection,statement,null);

}
```

### 

### CRUD操作-delete

使用executeUpdate(String sql)方法完成数据删除操作，示例操作：

```JAVA
Statement st = conn.createStatement();

String sql = “delete from user where id=1; 

int num = st.executeUpdate(sql);

if(num>0){
	System.out.println(“删除成功！！！");
}
```

删

```java
public static void main(String[] args) throws SQLException {

    // 获取连接
    Connection connection = JDBCUtils.getConnection();
    // 获取statement对象
    Statement statement = connection.createStatement();
    // 执行sql语句
    int effectedRows = statement.executeUpdate("delete from teacher where id = 7");

    if (effectedRows < 1) {
        System.out.println("删除数据失败！！！");
    }
    System.out.println("删除长风成功！！！");
    // 关闭资源
    JDBCUtils.releaseSource(connection,statement,null);

}
```

### 

### CRUD操作-read

使用executeQuery(String sql)方法完成数据查询操作，示例操作：

```JAVA
Statement st = conn.createStatement();

String sql = “select * from user where id=1; 

ResultSet rs = st.executeQuery(sql);

while(rs.next()){
    //根据获取列的数据类型，分别调用rs的相应方法
    //映射到java对象中
}
```

查

```java
public static void main(String[] args) throws SQLException {

    // 获取连接
    Connection connection = JDBCUtils.getConnection();
    // 获取statement对象
    Statement statement = connection.createStatement();
    // 执行sql语句
    ResultSet resultSet = statement.executeQuery("select * from teacher");

    while (resultSet.next()) {

        String name = resultSet.getString("name");
        System.out.println(name);
    }
    // 关闭资源
    JDBCUtils.releaseSource(connection,statement,resultSet);
}
```

# 数据库注入问题

对于数据库注入，我们来看一个登陆的例子：
假设有一个登陆界面，让我们输入用户名和密码进行登陆，我们这样输入用户名和密码：
用户名：name = "xxxx' (用户名随便输入)OR 0==0 --";
密码：密码=“xxxx”（随便输入一串）
此时我们可以试一下，我们是否可以登陆成功。
让我们看一下，在我们的代码中，最终拼接而成的，发送给数据库执行的sql语句

```JAVA
String sql = "select * from t_user where name ='"+ name +"' AND PASSWORD = '"+ password +"'";
```

问题:如何规避这种情况呢？

这个问题产生的原因：

其实就是因为我们使用JDBC的时候，假如需要用户输入的数据来作为SQL的参数的话，那么就需要去进行SQL的字符串拼接。

一旦进行SQL字符串拼接，那么就可能会导致你的SQL语句的结构发生变化，产生安全隐患。

```java
 @Test
    public void testLogin(){

        login("张三","这个程序真sb' or '1=1");

    }


    public Boolean login(String username, String password) {

        try {
            String sql  = "select * from user where username = '"+ username+"' and password = '"+password+"'";
            System.out.println(sql);
             resultSet = statement.executeQuery(sql);
             if (resultSet.next()) {
                 System.out.println("登录成功。。。");
                 return true;
             }else {
                 return false;
             }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return false;

    }
```

## PrepareStatement解决数据库注入问题

仔细分析一下，数据库注入成功的根本原因是，我们把sql语句中的参数(用户的输入)和sql命令拼接成了一个sql语句，因为一个sql语句中既可以有sql的命令又可以有参数，因此，<span style="color:red">用户的输入也可以被当做sql的语句来解析执行</span>。
那么，既然知道了sql注入成功的原因，我们就反其道而行之，不让用户输入的参数被当做sql命令解析，而是只把它当做普通字符串来解析。
由此，java中引入了prepareStatement，利用preparestatement来防止sql注入的核心思想就是，<span style="color:red">不把用户的输入当做sql命令来解析和执行</span>。



<span style="color:red">PreparedStatement继承自Statement</span>，可以通过Connection的prepareStatement方法得到。
prepareStatement很明显的将sql<span style="color:red">命令语句与参数分开处理</span>，其执行过程是，首先在sql语句真正执行之前，先把sql命令送到数据库中进行<span style="color:red">预编译</span>，生成相应 的<span style="color:red">数据库命令</span>，然后在获取sql中的参数，然后真正执行该sql语句。
这样一来，用户输入的参数，只被当做参数<span style="color:red">而非命令</span>来解析，就可以避免数据库注入这样的问题发生。
不过，这样一来，<span style="color:red">单次执行</span>PreparedStatement需要与数据库<span style="color:red">通信两次</span>，效率，比之于单词执行Statement要低。



```java
public Boolean login2(String username, String password){

    try {
        // ? 是占位用的
        String sql = "select * from user where username = ? and password = ?";
        // 获取一个prepareStatement 预编译的statement
        PreparedStatement preparedStatement = connection.prepareStatement(sql);

        // 设置参数 index 从 1 开始
        preparedStatement.setString(1, username);
        preparedStatement.setString(2, password);

        // 执行sql
        resultSet = preparedStatement.executeQuery();

        if (resultSet.next()) {
            System.out.println("登录成功。。。");
            return true;
        } else {
            System.out.println("登录失败...");
            return false;
        }
    }catch (SQLException ex) {
        ex.printStackTrace();
    }
    return false;
}
```

我们使用PrepareStatement进行操作的时候，其实与数据库通信了两次，效率比之前使用statement的时候要低。

# 使用JDBC进行批处理

业务场景：当需要向数据库发送一批SQL语句执行时，应避免向数据库一条条的发送执行，而应采用JDBC的批处理机制，以提升执行效率。
<span style="color:red">默认情况下Mysql的批处理仍然是一条一条执行，需要在url后面添加rewriteBatchedStatements=true参数</span>
实现批处理有两种方式



## 实现批处理的第一种方式：

Statement.addBatch(sql)  
执行批处理SQL语句
executeBatch()方法：执行批处理命令
clearBatch()方法：清除批处理命令

```
逐条插入时间：167007
statement所用时间：20282
prepareStatement所用时间：219
```

```java
Connection conn = null;
Statement st = null;
ResultSet rs = null;
try {
    conn = JdbcUtil.getConnection();
    String sql1 = "insert into user(name,password,email,birthday) 
        values('kkk','123','abc@sina.com','1978-08-08')";
    String sql2 = "update user set password='123456' where id=3";
    st = conn.createStatement();
    st.addBatch(sql1);  //把SQL语句加入到批命令中
    st.addBatch(sql2);  //把SQL语句加入到批命令中
    st.executeBatch();
} finally{
	JdbcUtil.free(conn, st, rs);
}
```



采用Statement.addBatch(sql)方式实现批处理：
<span style="background:yellow">优点</span>：可以向数据库发送多条不同的ＳＱＬ语句。
<span style="background:yellow">缺点</span>：

- SQL语句没有预编译。

- 当向数据库发送多条语句相同，但仅参数不同的SQL语句时，
  需重复写上很多条SQL语句。例如：

  ```sql
  Insert into user(name,password) values(‘aa’,’111’);
  	Insert into user(name,password) values(‘bb’,’222’);
  	Insert into user(name,password) values(‘cc’,’333’);
  	Insert into user(name,password) values(‘dd’,’444’);
  ```



## 实现批处理的第二种方式：

PreparedStatement.addBatch()

```java
conn = JdbcUtil.getConnection();
String sql = "insert into user(name,password,email,birthday) values(?,?,?,?)";
st = conn.prepareStatement(sql);
for(int i=0;i<50000;i++){
    st.setString(1, "aaa" + i);
    st.setString(2, "123" + i);
    st.setString(3, "aaa" + i + "@sina.com");
    st.setDate(4,new Date(1980, 10, 10));

    st.addBatch();
    if(i%1000==0){
        st.executeBatch();
        st.clearBatch();
    }
}
st.executeBatch();
```



采用PreparedStatement.addBatch()实现批处理
<span style="background:yellow">优点</span>：与数据库通信次数在批量操作时，<span style="color:red">PreparedStatment的通信次数远少于Statment</span>。
<span style="background:yellow">缺点</span>：<span style="color:red">只能应用在SQL语句相同，但参数不同的批处理中</span>。因此此种形式的批处理经常用于在同一个表中批量插入数据，或批量更新表的数据。

## 对比

- 从代码的便捷性上来说，我们的statement看起来更好，prepareStatement有点繁琐

- 从效率上来说，我们的prepareStatement的效率比statement要高（这个是在Mysql开启了批处理的情况下）

  如何开启呢？

  在url的后面增加一个参数，`rewriteBatchedStatements=true`

- 从使用场景上来说，我们的statement批处理里面可以添加各种不同格式的SQL语句，但是我们的prepareStatement在使用批处理的时候，我们SQL语句的格式必须是一样的，只是参数不一样。如果格式也不一样，那就是不同的prepareStatement了。



```
测试表结构
account | CREATE TABLE `account` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(20) DEFAULT NULL,
  `balance` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=40001 DEFAULT CHARSET=utf8
```

采用两种不同的批处理方式插入10000条数据，比较速度差别

开启rewriteBatchedStatements=true前

| Statement批处理：        | 18247 毫秒 |
| ------------------------ | ---------- |
| PrepareStatement批处理： | 17333 毫秒 |

开启rewriteBatchedStatements=true后

| Statement批处理：        | 16637 毫秒毫秒 |
| ------------------------ | -------------- |
| PrepareStatement批处理： | 215 毫秒       |



然后去根据主键查询和根据普通字段查询一条数据，比较查询速度差别

查询第20000个数据

| 主键查询：   | 10 毫秒 |
| ------------ | ------- |
| 普通字段查询 | 34 毫秒 |

# JDBC 事务Transaction

- 事务的概念

  [事务理解参考资料](https://developer.aliyun.com/article/743691#:~:text=MySQL%E7%9A%84%E4%BA%8B%E5%8A%A1,%E8%AF%BB%E4%BB%A5%E5%8F%8A%E5%8F%AF%E4%B8%B2%E8%A1%8C%E5%8C%96%E3%80%82)

  事务指逻辑上的一组操作，组成这组操作的各个单元，要么全部成功，要么全部不成功。
  例如: A——B转帐100，对应于如下两条sql语句

  ```sql
  update account set money=money-100 where name=‘a’;
  update account set money=money+100 where name=‘b’;
  ```

- 数据库开启事务命令
  start transaction  开启事务DTL
  rollback  回滚事务
  commit   提交事务

  ```
  Start transaction
  ….
  ….
  commit
  ```



首先，我们来看一个例子，假设现在用户A要给数据B转账100元，这个过程反映在数据上，就是两个步骤：

- 首先，A用户的账户数据首先减去100元
- 然后，B用户的账户数据在增加100元

当这两个步骤都完成的时候，转账的这个过程才算完成
但是，假设有这样一个场景，当用户A的账户减少100元之后，因为某些突发故障，即你给B用户转入失败。此时，我们再来看用户A和用户B的账户上的数据，你会发现，A账户少了100，B账户的余额未变，也就是说用户A莫名其妙的少了100元。



使用事务

当JDBC程序向数据库获得一个Connection对象时，默认情况下这个Connection对象会自动向数据库提交commit在它上面发送的SQL语句。若想关闭这种默认提交方式，<span style="color:blue">让多条SQL在一个事务中执行</span>，可使用下列语句：
JDBC控制事务语句

- Connection.setAutoCommit(false);        start transaction
- Connection.rollback();  		rollback
- Connection.commit();  		commit

```
# 命令行查看自动提交开启状态：
show variables like ‘%commit%’; # on/off
# 或
select @@autocommit; # 1/0
# 修改自动提交
set autocommit = off
```



演示银行转帐案例

在JDBC代码中使如下转帐操作在同一事务中执行。

```sql
update from account set money=money-100 where name=‘a’;
update from account set money=money+100 where name=‘b’;
```

<span style="color:red">设置事务回滚点</span>

- Savepoint sp = conn.setSavepoint();
- Conn.rollback(sp);
- Conn.commit();   //<span style="color:red">回滚后必须要提交</span>



## JDBC事务的使用示例

通过一个转账的案例，来连续JDBC事务操作的使用

```java
public Boolean transfer(String fromName, String toName, Integer salary) {
        
        try {
            // 开启事务
            connection.setAutoCommit(false);

            // 第一步 给 fromName 账户扣钱
            String sql = "update account set money = money - ? where name = ?";
            PreparedStatement preparedStatement = connection.prepareStatement(sql);

            preparedStatement.setInt(1, salary);
            preparedStatement.setString(2, fromName);

            int effectedRows = preparedStatement.executeUpdate();

            if (effectedRows <= 0) {
                System.out.println("扣钱失败");
                return  false;
            }

//            int i = 1/0;
            // 第二步 给 toName 账户加钱
            preparedStatement.clearParameters();

            preparedStatement.setInt(1, -salary);
            preparedStatement.setString(2, toName);
            int effectedRows2 = preparedStatement.executeUpdate();

            if (effectedRows2 > 0) {

                // 提交事务
                connection.commit();

                System.out.println("加钱成功");
                return true;
            }

            return false;
        }catch (Exception ex) {
            ex.printStackTrace();

            // 回滚事务
            try {
                connection.rollback();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        return false;

    }
```

事务相关的API

```java
// 开启事务 
// 其实这个说法是不够严谨的，其实这个地方的意思是把事务的自动提交关闭
// 在Mysql中，每一条SQL语句的执行都是一个事务，这个事务是自动提交的，在SQL语句执行后还未提交前，其实这个SQL对数据库产生的影响还未持久化，那么提交以后相当于对数据库的改变就是永久性的了,设置autoCommit=false其实就是不让这个SQL语句自动提交，我们手动的来控制这个sql语句什么时候提交，什么时候回滚(回滚其实就是让之前的还未提交的操作失效)
connection.setAutoCommit(false);

// 回滚
connection.rollback();

// 提交
connection.commit();
```

我们在使用事务的时候，需要注意我们必须使用同一个Connection对象去操作数据库，如果不是同一个connection对象的话，那么这个事务就会失效。

## 事务的特性(ACID)

- <span style="color:red">原子性（Atomicity）</span>

  原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。 

- <span style="color:red">一致性（Consistency）</span>：

  事务必须使数据库从一个一致性状态变换到另外一个一致性状态。

- <span style="background:yellow;color:red">隔离性（Isolation）</span>

  事务的隔离性是多个<span style="color:red">用户并发访问数据库时</span>，数据库为每一个用户开启的事务，不能被其他事务的操作数据所干扰，多个并发事务之间要相互隔离。

- <span style="color:red">持久性（Durability）</span>

  持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来即使数据库发生故障(软件故障)也不应该对其有任何影响。



## 事务的隔离级别

数据库中的数据是共享资源，当一个数据库允许多个用户同时访问的时候，数据库系统就必须支持并发控制。

在数据库中，事务是并发控制的基本单位，而对数据库数据并发访问时，事务的隔离性，保证了并发访问数据库时，<span style="color:red">数据的正确性和一致性</span>。

如果不考虑事务的隔离性，就会发生数据不一致的情况，主要有如下3种情况：

- 脏读（dirty reads） 
  一个事务读取了另一个未提交的并行事务写的数据。 

- 不可重复读（non-repeatable reads） 
  一个事务重新读取前面读取过的数据， 发现该数据已经被另一个已提交的事务修改过。 

- 幻读（phantom read） 
  一个事务重新执行一个查询，返回一套符合查询条件的行， 发现这些行因为其他最近提交的事务而发生了改变。

  在一个事务中，读取到了其他事务新增的记录

## 事务隔离性的设置语句

数据库共定义了四种隔离级别：

- Serializable：可避免脏读、不可重复读、虚幻读情况的发生。（串行化）
- Repeatable read：可避免脏读、不可重复读情况的发生。（无法避免虚读）
- Read committed：可避免脏读情况发生。（无法避免不可重复读）
- Read uncommitted：最低级别，以上情况均无法保证。(读未提交)

```
# MySQL命令行客户端查询数据库隔离级别
select @@transaction_isolation;# 或 select @@tx_isolation
# MySQL命令行客户端修改数据库隔离级别
# session会话，global全局
set session/global transaction isolation level read uncommitted.
# 注意：我们修改了之后要重新去连接数据库
```

我们发现Mysql默认的数据库的隔离级别是：**Repeatable read**

按照可重复读的官方定义来说，可重复读这种隔离级别并不能解决虚幻读的问题。

但是在Mysql中，我们Mysql的存储引擎（InnoDB）帮助我们解决了再可重复读这种隔离级别下的虚幻读问题。

```
Innodb的重复读（repeatable read）不允许脏读，不允许非重复读（即可以重复读，Innodb使用多版本一致性读来实现）和不允许幻象读（这点和ANSI/ISO SQL标准定义的有所区别）。
Mysql数据库的隔离级别 设定为repeatable read，就已经可以防止 脏读，重复读，虚读等问题。

Mysql8 现在更名为 transaction_isolation
```

- 串行化

  **Serializable**： 其实就是表示我们Mysql在应对并发的问题的时候，是一个一个sql语句串行起来执行的。

  没有并发安全性相关的问题，但是效率很差。

  ![image-20210422174551987](C:\Users\AnoxiC2010\Desktop\wdJava30th\SE\DB\Day04_JDBC&Transaction\note\事务.assets\image-20210422174551987.png)

  

## 隔离级别总结

|          | 脏读 | 不可重复读 | 虚幻读                           |
| -------- | ---- | ---------- | -------------------------------- |
| 读未提交 | √    | √          | √                                |
| 读已提交 | ×    | √          | √                                |
| 可重复读 | ×    | ×          | √（Mysql中InnoDB解决了这个问题） |
| 串行化   | ×    | ×          | ×                                |



## 事务的隔离性 如果做的不好 会有：

### 脏读

指一个事务读取了另外一个事务未提交的数据。
这是非常危险的，假设Ａ向Ｂ转帐１００元，对应sql语句如下所示

```sql
1.update account set money=money+100 while name=‘b’
2.update account set money=money-100 while name=‘a’;
```

如果此时银行正在作做报表统计，那么此时，所读取用户
余额总值明显多出了100元，之后再做统计会发现“少了100
元，而这100元去了哪里，就说不清楚了。



### 不可重复读

**（针对一条记录的，同一条记录前后不一样）**

**在<span style="color:red">一个事务内</span>读取表中的某一行数据，多次读取结果不同。**

- 例如银行想查询A帐户余额，第一次查询A帐户为200元，此时A向帐户内存了100元并提交了，银行接着又进行了一次查询，此时A帐户为300元了。银行两次查询不一致，可能就会很困惑，不知道哪次查询是准的。

和脏读的区别是，<span style="color:red">脏读是读取前一事务未提交的脏数据</span>，<span style="color:blue">不可重复读是重新读取了前一事务已提交的数据</span>。

很多人认为这种情况就对了，无须困惑，当然是后面的为准。
我们可以考虑这样一种情况，比如银行程序需要将查询结果分别输出到电脑屏幕和写到文件中，结果在一个事务中针对输出的目的地，进行的两次查询不一致，导致文件和屏幕中的结果不一致，银行工作人员就不知道以哪个为准了。



### 虚读

**(幻读，同一张表前后不一样记录数)**
**是指在一个事务内读取到了别的事务插入的数据，导致前后读取不一致。**

- 如丙存款100元未提交，这时银行做报表统计account表中所有用户的总额为500元，然后丙提交了，这时银行再统计发现帐户为600元了，造成虚读同样会使银行不知所措，到底以哪个为准。

```
mysql的repeatable read隔离级别可以避免虚读（幻读）。
```



# 测试工具JUNIT

Junit这个测试工具可以帮助我们执行一个类里面的任意一个方法，或者是批量执行类里面的方法。而不是像main方法一样，一个类里面只能有一个。

这个测试工具不光是在我们的数据库里面可以用上，在以后的学习中也有大的作用。因为他可以帮助我们测试自己写的接口或者方法是否功能正常。

### 导包

Junit

![image-20210422110219928](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\JDBC-notes.assets\image-20210422110219928.png)



### 加注解

@Test注解

注意：

  1. @Test修饰的方法必须是public
  2. @Test修饰的方法不能是static
  3. @Test修饰的方法不能有返回值
  4. @Test修饰的方法不能传参
  5. 软规则（主要是为了给别的工具去做适配的-Maven）
     - 我们测试类的类名建议是 XXXTest
     - 我们测试的方法名建议写成 TestXXX

### 执行

![image-20210422110323191](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\JDBC-notes.assets\image-20210422110323191.png)

### 其他注解

- @Before

  在测试方法@Test 执行之前执行

- @After

  在测试方法@Test执行之后执行

- @BeforeClass

  在测试工具类启动的时候执行

- @AfterClass

  在测试完成，类销毁之前执行

  ![image-20210422111139913](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\JDBC-notes.assets\image-20210422111139913.png)



## 