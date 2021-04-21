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

url编码问题：

```
SQL语句中还有中文，JDBC查无结果，但数据库查有结果
SQL语句过滤条件中含中文

因为数据库编码为utf-8，但jdbc的url地址没有指定编码格式

在url最后添加?useUnicode=true&characterEncoding=UTF-8
```



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



## Tip：PreparedStatement

仔细分析一下，数据库注入成功的根本原因是，我们把sql语句中的参数(用户的输入)和sql命令拼接成了一个sql语句，因为一个sql语句中既可以有sql的命令又可以有参数，因此，<span style="color:red">用户的输入也可以被当做sql的语句来解析执行</span>。
那么，既然知道了sql注入成功的原因，我们就反其道而行之，不让用户输入的参数被当做sql命令解析，而是只把它当做普通字符串来解析。
由此，java中引入了prepareStatement，利用preparestatement来防止sql注入的核心思想就是，<span style="color:red">不把用户的输入当做sql命令来解析和执行</span>。



<span style="color:red">PreparedStatement继承自Statement</span>，可以通过Connection的prepareStatement方法得到。
prepareStatement很明显的将sql<span style="color:red">命令语句与参数分开处理</span>，其执行过程是，首先在sql语句真正执行之前，先把sql命令送到数据库中进行<span style="color:red">预编译</span>，生成相应 的<span style="color:red">数据库命令</span>，然后在获取sql中的参数，然后真正执行该sql语句。
这样一来，用户输入的参数，只被当做参数<span style="color:red">而非命令</span>来解析，就可以避免数据库注入这样的问题发生。
不过，这样一来，<span style="color:red">单次执行</span>PreparedStatement需要与数据库<span style="color:red">通信两次</span>，效率，比之于单词执行Statement要低。



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





