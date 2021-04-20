# MySQL notes

# 数据库基础

定义：

Database：A database is an organized collection of data,
 stored and accessed electronically. [wikipedia]

数据库：数据库是按照数据结构来组织、存储和管理数据
的仓库。——百度百科



分类：

- 关系型数据库
  不仅存储数据本身，还存储数据之间的关系，比如说用户信息和订单信息。

  关系型数据库模型把复杂的数据结构归结为简单的二维表（关系表）。

- 非关系型数据库

  非关系型数据库也被称为NoSQL数据库，NoSQL的本意是指 
  “Not Olnly SQL”。NoSQL 的产生并不是要否定关系型数据库，
  而是作为关系型数据库的一个有效补充。

随着互联网 web2.0 网站的兴起，传统的关系数据库在应付 web2.0 网站，特别是超大规模和高并发的 SNS 类型的 web2.0 纯动态网站时，已经显得力不从心。NoSQL 数据库就是在这样的情景下诞生，并迅速发展的。 

NoSQL 的应用场景：

1. 数据模型比较简单；
2. 需要灵活性更强的IT系统；
3. 对数据库性能要求较高；
4. 不需要高度的数据一致性；
5. 对于给定key，比较容易映射复杂值的环境。



常用的关系型数据库（系统）

<span style="color:red">Oracle---TikTok()   apple  云上贵州</span>  
<span style="color:red">MySQL</span>
<span style="color:red">MariaDB</span>
<span style="color:red">SQL Server</span>
Access (office)
DB2(IBM)
Informix
Sybase （power design）
SQLite等 （Android）
OceanBase

> 1970 E.F.Codd 提出                      
>
> Larry Ellison



常用的非关系型数据库（系统）

<span style="color:red">Memcached</span>
<span style="color:red">Redis (String, Set, Zset, List, Hash)</span>
<span style="color:red">MongoDB (ORM, Herbinate & Mybaitis)</span>
Cassandra（Facebook）
Hbase （Apache, Hadoop database）
MemcachedDB（Sina）



数据库管理系统、数据库服务、数据库和表的关系

![image-20210419231911803](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Database\MySQL-notes.assets\image-20210419231911803.png)



数据在表中的形式

![image-20210419231937521](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Database\MySQL-notes.assets\image-20210419231937521.png)

```
对象与行对应，属性与列对应
ORM 框架：Hibernate & MyBatis
```



易混淆术语：

数据库系统：是指在计算机系统中引入数据库后的系统，一般由数据库、
数据库管理系统、应用系统、数据库管理员（DBA）构成。 

数据库管理系统（DBMS）：是一种操纵和管理数据库的大型软件，
用于建立、使用和维护数据库（如：MySQL）。 

数据库：数据库是按照数据结构来组织、存储和管理数据的仓库。

注意：我们通常用数据库这个术语来代表 DBMS，严格来说，这是不正确的，容易产生混淆。



MySQL 8 安装教程

```
MySQL 8 安装教程

1.	下载
MySQL 8 for window下载地址DOWNLOAD，进入页面后可以不登录, 后点击底部“No thanks, just start my download.”即可开始下载。
2.	安装
2.1 解压
将zip 包解压到你要安装的目录。
2.2 配置环境变量：
 
2.3	初始化
以管理员身份运行cmd。在MySQL安装目录的 bin 目录下执行命令：
mysqld --initialize –console
执行完成后，会打印 root 用户的初始默认密码，比如：
 
rI5rvf5x5G,E”就是初始密码（不含首位空格）。在没有更改密码前，需要记住这个密码，后续登录需要用到。
2.4	安装服务
在MySQL安装目录的 bin 目录下执行命令：
mysqld --install [服务名]
	后面的服务名可以不写，默认的名字为 mysql。当然，如果你的电脑上需要安装多个MySQL服务，就可以用不同的名字区分了，比如 mysql5 和 mysql8。
2.5	启动服务
安装完成之后，就可以通过命令net start mysql启动MySQL的服务了。通过命令net stop mysql停止服务。
```



登录 MySQL Server

在启动 MySQL 服务后，输入以下格式的命令：

		mysql -h 主机名 -u 用户名 –p

-h：该参数用于指定客户端的主机名（host），即哪台机器要登录 MySQL Server，如果是当前机器该参数可以省略;
-u：用户名（user）。
-p：密码（password），如果密码为空，该参数可以省略。

```
刚装的 MySQL，它有一个管理员账号 root，密码默认为空。所以我们可以用下面命令登录：

mysql -u root
```



# SQL简介

SQL是结构化查询语言（Structured Query Language）的缩写。
它是一种专门用来与关系型数据库沟通的语言。 



它主要有如下的优点： 
SQL 是一种通用语言，几乎所有的关系型数据库都支持 SQL。
SQL 简单易学。它的语句是由一些有很强描述性的关键词组织而成，
而且这些关键词并不多。
SQL 虽然简单，但它是一种强有力的语言，灵活地使用 SQL，
可以进行非常复杂的数据库操作。 

```
说明：SQL 的扩展

标准 SQL 是由 ANSI 标准委员会管理的，从而称为 ANSI SQL。许多 DBMS 厂商通过增加语句或指令，对 SQL 进行了扩展，目的是提供一些特定的操作，或者是简化某些操作。 虽然这种扩展很有必要，但同时也给 SQL 代码的移植带来了麻烦。 

即使 DBMS 有自己的扩展，但它们都支持 ANSI SQL。

注意：请正确认识 “SQL 不区分大小写“

虽然 SQL 不区分大小写，但是表名、列名和值可能区分！（这依赖具体的 DBMS 及其配置）。
```

<span style="color:red">SQL 不区分大小写！！！</span>
<span style="color:red">关键字大写， 表名，列名，值最好是以它定义时值</span>



组成：

<span style="color:red">DDL：数据定义语言</span>
<span style="color:red">DML：数据操作语言</span>
<span style="color:red">DQL： 数据查询语言</span>
DCL： 数据控制语言
TPL： 事务处理语言



# 数据定义语言（DDL）

DDL：Data Definition Language
作用：创建 & 管理数据库和表的结构。
常用关键字：
<span style="color:red">CREATE   ALTER   DROP</span>



## 创建数据库

```
CREATE DATABASE [IF NOT EXISTS] db_name
    [create_specification [ create_specification] ...];
 
create_specification:
    [DEFAULT] CHARACTER SET charset_name //指定字符集
  | [DEFAULT] COLLATE collation_name//指定数据库字符集的比较方式
```

```
https://dev.mysql.com/doc/refman/8.0/en/charset.html   utf8_general_ci（不区分大小写）, utf8_bin（区分大小写）
```



## 查看、删除数据库

```
显示数据库语句：
SHOW DATABASES

显示数据库创建语句：
SHOW CREATE DATABASE db_name
 
数据库删除语句：
DROP DATABASE  [IF EXISTS]  db_name 
```



## 修改数据库

```
ALTER  DATABASE   db_name    
	[alter_specification [, alter_specification] ...] 

alter_specification:    

    [DEFAULT] CHARACTER SET charset_name  
|   [DEFAULT] COLLATE collation_name
```

```
修改数据库名称

很遗憾，官方并没有提供直接修改数据库名称的命令。但网上有提供各种修改数据库名称的脚本，感兴趣可以自己去研究
```



## 创建表

```
CREATE TABLE table_name
(
	field1  datatype,
	field2  datatype,
	field3  datatype
)[CHARACTER SET 字符集 COLLATE 校对规则]
field：指定列名　datatype：指定列类型

//注意：创建表前，要先使用USE db_name语句使用库。
```

注意：创建表时，要根据需保存的数据创建相应的列，并根据数据的类型定义相应的列类型。如：user对象

```
| id | name | password | birthday |
int id                              
String name
String password
Date birthday
```



## 数据类型

<span style="color:red">注意：这里以MySQL为例，不同的DBMS的都支持数值类型，字符串类型以及日期类型，但他们的实现可能不一样。</span>

整数类型

| **数据类型** | **占用字节** | **说明**       |
| ------------ | ------------ | -------------- |
| TINYINT      | 1            | 很小的整数     |
| SMALLINT     | 2            | 小的整数       |
| MEDIUMINT    | 3            | 中等大小的整数 |
| INT          | 4            | 普通大小的整数 |
| BIGINT       | 8            | 大整数         |

```
CREATE TABLE `t1` (    `year` year(4) DEFAULT NULL,    `month` int(2) unsigned zerofill DEFAULT NULL,    `day` int(2) unsigned zerofill DEFAULT NULL  ) 
```



浮点数类型和定点数类型

| **类型名称** | **占用字节** | **说明**     |
| ------------ | ------------ | ------------ |
| FLOAT(M,D)   | 4            | 单精度浮点数 |
| DOUBLE(M,D)  | 8            | 双精度浮点数 |
| DECIMAL(M,D) | M+2          | 定点数       |

其中 M 称为精度，表示总共的位数; 
         D 称为标度，表示小数的位数。
DECIMAL 类型不同于 FLOAT & DOUBLE，DECIMAL 实际是以字符串存放的，它的存储空间并不固定，而是由精度 M 决定的。

![image-20210419233404512](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Database\MySQL-notes.assets\image-20210419233404512.png)



日期与时间类型

| **类型名称** | **日期格式**            | **占用字节** |
| ------------ | ----------------------- | ------------ |
| YEAR         | YYYY      (2018)        | 1            |
| TIME         | HH:MM:SS  (10:20:00)    | 3            |
| DATE         | YYYY-MM-DD  (2018-7-23) | 3            |
| DATETIME     | YYYY-MM-DD  HH:MM:SS    | 8            |
| TIMESTAMP    | YYYY-MM-DD  HH:MM:SS    | 4            |

DATETIME 和 TIMESTAMP 虽然显示的格式是一样的，但是它们有很大的区别：
DATETIME 的系统默认值是 NULL, 而 TIMESTAMP 的系统默认值是 当前时间 NOW();
DATETIME 存储的时间与时区无关，而 TIMESTAMP 与时区有关。

（修改系统失去会影响DATETIME，但TIMESTAMP只和服务器所在地的时区有关）

```
set time_zone=‘+10:00’; 
create table time1(t1 year, t2 time, t3 date, t4 datetime, t5 timestamp);
insert into time1 values (now(),now(),now(),now(),now());
```



字符串类型

| 类型名称   | 占用字节                                         | 说明                 |
| ---------- | ------------------------------------------------ | -------------------- |
| CHAR(M)    | M,  1 <= M <= 255                                | 固定长度字符串       |
| VARCHAR(M) | L+1,  L  <=M, 1 <=M <=255                        | 变长字符串           |
| TINYTEXT   | L+1,  L < 2^8                                    | 非常小的文本字符串   |
| TEXT       | L+2,  L < 2^16                                   | 小的文本字符串       |
| MEDIUMTEXT | L+3,  L < 2^24                                   | 中等大小的文本字符串 |
| LONGTEXT   | L+4,  L < 2^32                                   | 大的文本字符串       |
| ENUM       | 1 或者  2个字节，取决于枚举的数目，最大  65535个 | 枚举类型             |
| SET        | 1,2,3,4或8个字节                                 | 集合类型             |

ENUM 类型总有一个默认值，当ENUM 列声明为NULL，则默认值为NULL。如果 ENUM 列被声明为 NOT NULL，则其默认值为列表的第一个元素。

```
CREATE TABLE t_enum(enm ENUM(‘first’,’second’,’third’));
CREATE TABLE t_set(st SET(‘a’,’b’,’c’,’d’));
```



二进制类型

| **类型名称**  | **占用字节**     | **说明**               |
| ------------- | ---------------- | ---------------------- |
| BIT(M)        | [(m+7)/8]        | 位字节类型             |
| BINARY(M)     | M                | 固定长度的二进制字符串 |
| VARBINARY(M)  | M+1              | 可变长度的二进制字符串 |
| TINYBLOB(M)   | L+1,  L < 2^8    | 非常小的  BLOB         |
| BLOB(M)       | L+2,  L < 2^16   | 小的  BLOB             |
| MEDIUMBLOB(M) | L+3,  L < 2^24   | 中等大小的BLOB         |
| LONGBLOB(M)   | L+4,  L  <  2^32 | 非常大的BLOB           |

字符串类型存储的是非二进制字符串（字符字符串），

二进制类型存储的是二进制字符串（字节字符串）。



## 查询表

简单描述表结构

DESC 表名
    或者
DESCRIBE 表名

查看生成表的 DDL 语句
SHOW CREATE TABLE 表名



## 修改表

使用ALTER TABLE语句追加,修改,或删除列的语法。

```
ALTER TABLE table_name
ADD		   (column datatype [DEFAULT expr]
		   [,ADD column datatype]...);
```

```
ALTER TABLE table_name
change	   col_name new_col_name datatype [DEFAULT expr]
[,change col_name new_col_name datatype [DEFAULT ...] ]...;
```

```
ALTER TABLE table_name
MODIFY column datatype [DEFAULT expr]
		   [,MODIFY column datatype]...;
```

```
ALTER TABLE table_name
DROP	      (column);
```

修改表的名称：RENAME TABLE 表名 TO 新表名
修改表的字符集：alter table student character set utf8;

```
RENAME TABLE 语句的另一个用法是移动该表到另一个数据库

语法为：
RENAME TABLE 旧数据库名.旧表名 TO 新数据库名.新表名

提示：我们可以把 RENAME TABLE 的这两种用法很好地统一起来，如果我们把 “重命名” 理解为 “在同一数据库里的移动”。甚至我们可以省略数据库名，如果你恰好正在使用该数据库。
```



## 删除表

DROP TALBE 表名



# DML数据操纵语言结合DQL

DML:Data Manipulation Language
作用：用于向数据库表中插入、删除、修改数据。
常用关键字：
INSERT UPDATE DELETE



## Insert语句

使用 INSERT 语句向表中插入数据。

```
INSERT INTO	table [(column [, column...])]
VALUES		(value [, value...]);
```

插入的数据应与字段的数据类型相同。
数据的大小应在列的规定范围内，例如：不能将一个长度为80的字符串加入到长度为40的列中。
在values中列出的数据 。
<span style="color:red">字符串和日期型数据应包含在单引号中。</span>
插入空值 insert into table value(null)



## Update语句

使用 update语句修改表中数据。

```
UPDATE 	tbl_name    
	SET col_name1=expr1 [, col_name2=expr2 ...]    
	[WHERE where_definition]    
```

UPDATE语法可以用新值更新原有表行中的各列。
SET子句指示要修改哪些列和要给予哪些值。
WHERE子句指定应更新哪些行。如没有WHERE子句，则更新所有的行。



## Delete语句

使用 delete语句删除表中数据。

```
delete from  table_name       
	[WHERE where_definition]    
```

如果不使用where子句，将删除表中所有数据。
Delete语句不能删除某一列的值（可使用update）<span style="color:red">删除的单位是行(行)</span>
使用delete语句仅删除记录，不删除表本身。如要删除表，使用drop  table语句。
同insert和update一样，从一个表中删除记录将引起其它表的<span style="color:red">参照完整性问题</span>，在修改数据库数据时，头脑中应该始终不要忘记这个潜在的问题。

```
删除表中数据也可使用TRUNCATE TABLE 语句，它和delete有所不同 
delete 和drop的区别
```



# DQL数据查询语言（简单查询）

DQL：Data Query Language
作用：查询表中的数据。
关键字：
SELECT



## 测试计算

虽然 SELECT 语句通常用于从表中检索数据，但我们也可以用它计算表达式和函数的值。如：

SELECT 32; 			-- 计算表达式32的值 
SELECT NOW(); 		-- 查看当前的时间 
SELECT TRIM(' ab cd ‘); 	-- 修剪字符串' ab cd '左右两边的空白
SELECT CONCAT('ab','cd’); 	-- 拼接字符串'ab'和'cd'



## 查询单列

```
SELECT column_name FROM table_name;
```



## 查询多列

```
SELECT col_name1, col_name2, … , col_namen 
FROM table_name;
```



## 查询所有列

```
SELECT * FROM table_name;
```

```
注意：除非确实需要表中的所有数据，否则最好不要使用 * 通配符。因为，查询不必要的数据会降低查询和应用程序的效率。

提示：当我们不知道列的名称时，可以使用 * 通配符获取它们的数据。
```



## 使用 WHERE 子句过滤记录

```
SELECT * | {column_names}
FROM table_name
WHERE <filter_condition>;
```

filter_condition 是一个逻辑表达式，即表达式的结果是布尔类型。

```
注意：在 MySQL 中，0 表示 false，非 0 表示 true，故：SELECT * FROM t_students WHERE 1; 也是合法的。 
```



## 常见运算符介绍

算术运算符

`+    -    *    /     %`
比较运算符

| **运算符**   | **作用**       | **运算符**   | **作用**     |
| ------------ | -------------- | ------------ | ------------ |
| =            | 等于           | <=>          | 安全的等于   |
| <>  (!=)     | 不等于         | <=           | 小于等于     |
| >=           | 大于等于       | >            | 大于         |
| IS  NULL     | 是否为NULL     | IS  NOT NULL | 是否不为NULL |
| BETWEEN  AND | 是否在闭区间内 | IN           | 是否在列表内 |
| NOT  IN      | 是否不在列表内 | LIKE         | 通配符匹配   |

逻辑运算符
`NOT(!)       AND(&&)      OR(||) `    
位操作运算符
`&     |     ~     ^     <<     >>`

```
LIKE 通配符规则：
‘%’ 匹配任何数目的字符，甚至包括零字符
‘_’ 只能匹配一个字符
```



## DISTINCT 过滤相同的记录

```
SELECT DISTINCT {column_names}
FROM table_name; 
```

```
注意：DISTINCT 操作的基本单位是行（记录），只有当两行数据一模一样的情况下，才会删除另一行。
```



## 限制结果

SELECT 语句返回所有匹配的记录。但是如果我们只想返回第一行或者一定数量的行，这该怎么办呢？

在 MySQL 中，我们可以用 LIMIT 关键字实现这一要求。

```
LIMIT offset, nums;     -- LIMIT 3, 4;
--OR
LIMIT nums OFFSET offset;
```

```
注意：offset是指偏移量，当我们从第1行查起，偏移量自然为0。此时，可以写成 LIMIT nums;

扩展：我们可以使用 LIMIT 关键字实现分页查询。 LIMIT (page_num – 1) * page_size, page_size;
```



## 对查询结果进行排序

我们可以使用 ORDER BY 子句对查询结果进行排序。

对单列排序
     如：对数学单科成绩从低到高进行排序。
对多列排序 
     如：对语数外三科成绩进行排序。
指定排序方向（ASC, DESC）
     如：对语文降序，数学升序，外语降序排序。

```
提示：其实，检索出的数据并不是随机显示的。如果不排序，数据一般将以它在底层表中出现的顺序显示，这有可能是数据最初添加到表中的顺序。 但是，如果数据随后进行过更新或删除，那么这个顺序将会受到DBMS重用回收存储空间的方式的影响。因此，如果不明确控制的话，则最终的结果不能（也不应该）依赖该排序顺序。关系数据库设计理论认为，如果不明确规定排列顺序，则不应该假定检索出的数据的顺序有任何意义。 	
```



## 计算字段

我们想查询同学的总成绩信息，该怎么办？这时候计算字段就可以派上用场了。计算字段并不实际存在于数据库表中。计算字段是运行时在SELECT语句内创建的。
SELECT name, chinese + math + english  FROM t_students;

别名

当然，我们可以给 Chinese + math + english 起一个更简单直接一点的名字。AS 关键字就可以做到这一点。

SELECT name, chinese + math + english AS total FROM t_students;

```
注意：在很多DBMS中，AS关键字是可选的，不过最好使用它，这被视为一条最佳实践。

提示：AS 关键字不仅能给列起别名，还可以给表起别名。在多表查询中，我们会这样使用它。
```



## 聚合函数

COUNT()
    COUNT(*) 计算表中的总行数;
    COUNT(column_name) 计算指定列下的总行数，计算时将忽略值为NULL的行。
SUM():返回指定列的所有值之和
    计算时将忽略值为NULL的行;
AVG():返回指定列的平均值
    计算时将忽略值为NULL的行;
MAX():返回指定列的最大值
    计算时将忽略值为NULL的行;
MIN():返回指定列的最小值
    计算时将忽略值为NULL的行;



## 分组查询

比如：我们想统计各班学生的人数，该怎么办？这时候我们就得用到分组的概念。在 SQL 中，我们使用 GROUP BY 关键字对数据进行分组。

```
[GROUP BY {column_names}] [HAVING <filter_condition>]
```

使用 HAVING 过滤分组

如：查询人数大于2的班级

    提示：GROUP BY 关键字通常是和集合函数一起使用的。



多字段分组

GROUP BY 关键字后可以接多个列名，其意思是从左到右，按层次分组。即先按第一个字段分组，然后在第一个字段值相同的记录中，再根据第2个字段的值进行分组…（多个字段看成一个整体）

 如：对学生按语文和数学成绩分组。[GROUP_CONCAT() 函数]



分组和排序

我们经常发现，用 GROUP BY 分组的数据确实是以分组顺序输出的。此外，即使特定的 DBMS 总是按给出的 GROUP BY 子句排序数据，用户也可能会要求以不同的顺序排序。应该提供明确的 ORDER BY 子句，即使其效果等同于GROUP BY子句。

如：查询人数少于10的班级，并按人数降序排序。

```
group by多字段，新增gender字段，group by class,gender
group_concat()按照性别进行分组，每组有哪些人
 select group_concat(name)as people,gender from t_students group by gender;
```



## SELECT语句的执行顺序 

```
(5) SELECT column_name, ... 
(1) FROM table_name, ...
(2) [WHERE ...] 
(3) [GROUP BY ...] 
(4) [HAVING ...] 
(6) [ORDER BY ...]; 
```



## 数据完整性

数据完整性是为了保证插入到数据库中的数据是正确的，它防止了用户可能的输入错误。
数据完整性主要分为以下三类：
实体完整性 （唯一性）
规定表的一行（即每一条记录）在表中是唯一的实体。实体完整性通过表的主键来实现。
域完整性：
指数据库表的列（即字段）必须符合某种特定的数据类型或<span style="color:red">约束</span>。比如NOT NULL。
<span style="background:yellow">参照完整性：</span>
<span style="background:yellow">保证一个表的外键和另一个表的主键对应。</span>

```
数据库主键主键：表中经常有一个列或列的组合，其值能唯一地标识表中的每一行。这样的一列或多列称为表的主键，通过它可强制表的实体完整性。当创建或更改表时可通过定义 PRIMARY KEY 约束来创建主键。一个表只能有一个 PRIMARY KEY 约束，而且 PRIMARY KEY 约束中的列不能接受空值。由于 PRIMARY KEY 约束确保唯一数据，所以经常用来定义标识列。 作用 :
1）保证实体的完整性; 
2）加快数据库的操作速度
 3） 在表中添加新记录时，会自动检查新记录的主键值，不允许该值与其他记录的主键值重复。 
4) 自动按主键值的顺序显示表中的记录。如果没有定义主键，则按输入记录的顺序显示表中的记录。
```



## 定义表的约束

```
定义主键约束
  primary key:不允许为空，不允许重复
     (可以区分两条记录的唯一性)
删除主键：alter table tablename drop primary key ;
定义主键自动增长
  auto_increment
定义唯一约束
  unique
定义非空约束
  not null
定义外键约束
constraint constraint_FK_name foreign key(ordersid) references orders(id),
```



## 多表设计(两个表)

一对多 （省份和城市、分类和商品、班级和学生）
多对多（学生和课程、用户和商品）
一对一（学生和学号、人和身份证号码）

避免数据的冗余



一对多

province

| ID(Primary  key) | province  =char(50) |
| ---------------- | ------------------- |
| 1                | shandong            |
| 2                | sichuan             |
| 3                | guangdong           |
| 4                | hunan               |

student

| ID(PK) | Name | Proviince_ID  tiny int |
| ------ | ---- | ---------------------- |
| 1      |      | 1                      |
| 2      |      | 1                      |
| 3      |      | 1                      |
| 4      |      | 1                      |

```
Create table province (
    id int primary key,
    province varchar(9) not null  
   )
insert into province values(1,'shandong');

Create table students(
    id int primary key,
    name varchar(5) not null,
    province_id int not null
)

Alter table student add constraint FK_student_Provinceid     foreign key (province_id)  references provices(id);

alter table employee add constraint FK_employee_departmentid foreign key (departmentid) references department(id);
```



多对多

| ID(Primary  key) | teacher |
| ---------------- | ------- |
| 1                | chinese |
| 2                | math    |
| 3                | english |
| 4                | c       |

| ID(PK) | Name |
| ------ | ---- |
| 1      |      |
| 2      |      |
| 3      |      |
| 4      |      |

| T_ID | S_ID |
| ---- | ---- |
| 1    | 1    |
| 1    | 2    |
| 2    | 1    |
| 2    | 2    |
| 3    | 1    |

```
create table TEACHER(
ID int primary key,
NAME varchar(100)
);
insert into teacher values(1,'math teacher');
insert into teacher values(2,'chinese teacher');
insert into teacher values(3,'english teacher');

create table STUDENT3(
ID int primary key,
NAME varchar(100)
);

insert into student3 values(1,"lily");
insert into student3 values(2,"lucy");
insert into student3 values(3,"lilei");
insert into student3 values(4,"lilyzhou");

insert into TEACHER_STUDENT(1,1);
create table TEACHER_STUDENT(
T_ID int,
S_ID int,
primary key(T_ID,S_ID),
constraint T_ID_FK foreign key(T_ID) references TEACHER(ID),
constraint S_ID_FK foreign key(S_ID) references STUDENT3(ID));
```



一对一

| ID   | name | province |
| ---- | ---- | -------- |
| 1    | 张三 | 湖北     |
| 2    | 李四 | 广东     |

| id   | computer     | Persion_id  (外键+unique) |
| ---- | ------------ | ------------------------- |
| 1    | 联想T420     | 1                         |
| 2    | 苹果mac  pro | 2                         |

```
方法1：外键关联+唯一
create table PERSON(
ID int primary key,
NAME varchar(100)
);
create table computer(
ID int primary key,
brand varchar(20),
PERSON_ID int unique,
constraint PERSON_ID_FK foreign key(PERSON_ID) references PERSON(ID)
);

方法2：主键关联

constraint PERSON_ID_FK foreign key(ID) references PERSON(ID)
```



## 数据库三大范式

第一范式（1NF）：（每列保持原子性）
      如果数据库中的所有字段都是不可分割的原子值，则说明该数据库满足第一范式，用户的收货地址

第二范式（2NF）：（记录的唯一性）
      要求记录有唯一标识，不存在部分依赖。表：学号、课程号、姓名、学分;
       学分依赖课程，姓名依赖学号

第三范式（3NF）：（字段不要冗余）
         表: 学号, 姓名, 年龄,       学院名称, 学院电话，不要存在传递依赖



![image-20210419235640503](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Database\MySQL-notes.assets\image-20210419235640503.png)

![image-20210419235645384](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Database\MySQL-notes.assets\image-20210419235645384.png)

![image-20210419235649390](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Database\MySQL-notes.assets\image-20210419235649390.png)

设计一个小型的电商网站数据库

需要首先理清有哪些模块：
	用户模块：用户名，密码，电话，邮箱，身份证号，地址，姓名，昵称
	商品模块：编号，名称，描述，分类，价格、图片
	分类模块：分类编号，分类名称
	订单模块：订单号，用户编号，收件人姓名，收件人电话，收货地址，价格，订单状态，支付状态、商品id、商品名称等信息
	购物车模块：用户名，商品编号，商品名称，商品价格，加入时间，商品数量

出发点：从页面出发。展示什么，那么表里面就应该有什么。其次分析各个表之间的相对关系，一对多、一对一、多对多。进而就可以知道应该在哪一方保存相对关系。
	