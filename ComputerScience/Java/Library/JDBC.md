## 基本概念

JDBC（Java Database Connectivity）是一个用于执行 SQL 语句的 Java API，能够与各种数据库进行交互。

## 1. JDBC概述

### 1.1 什么是JDBC？

**JDBC 是使用 Java 语言操作关系型数据库的一套 API**。这套 **API** 是交由不同的数据库厂商实现的。我们利用 JDBC 编写操作数据库的代码，真正执行的是各个数据库的实现类（驱动）。像这种给定一种规范，交由不同实现方实现的机制，也被称为 SPI 机制。 全称：（Java DataBase Connectivity）Java 数据库连接。

![[JDBC--1.png]]

### 1.2 JDBC的好处

- 面向接口编程，屏蔽实现上的差异。
- 一套 Java 代码操作不同数据库。

## 2. 使用JDBC

### 2.1 JDBC流程

![[JDBC--2.png]]


1. 编写 Java 代码
2. Java 代码将 SQL发送到 MySQL服务器
3. MySQL 服务器接收到 SQL，并执行该 SQL 语句
4. 将 SQL 语句的执行结果返回给Java 代码

### 2.2 环境配置

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.29</version>
</dependency>
```

### 2.3 编码

#### 2.3.1 步骤

1. 引入驱动并注册
2. 获取连接
3. 定义SQL
4. 获取执行SQL对象
5. 执行SQL
6. 处理返回结果
7. 释放资源

#### 2.3.2 代码实现

```java
public static void demo(){    
Connection conn = null;    
Statement state = null;    
try {        // 1.注册驱动        
Class.forName("com.mysql.cj.jdbc.Driver");        // 2.获取连接        
conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/test", "root", "root");        // 3.定义SQL        
String sql = "select * from user";        // 4.获取执行 SQL 的对象 statement        
state = conn.createStatement();        // 5.执行 SQL，因为是查询，所以是Query，并且返回结果        
ResultSet resultSet = state.executeQuery(sql);        // 6.处理返回结果        
System.out.println(resultSet);    } 
catch (ClassNotFoundException | SQLException e) {        e.printStackTrace();    } finally {        // 7.关闭资源       
 if (state != null) {            try {                state.close();            } catch (SQLException e) {                e.printStackTrace();            }        }        if (conn != null) {            try {                conn.close();            } catch (SQLException e) {                e.printStackTrace();            }        }    }}
```

#### 2.3.3 注意事项

- 最晚获得的资源最先关闭。
- **Mysql8** 之前的驱动类全类名为 `com.mysql.jdbc.Driver`，**Mysql8**之后则为 `com.mysql.cj.jdbc.Driver`。

## 3. JDBC API

### 3.1 DriverManager

#### 3.1.1 作用

- 注册驱动。因为 JDBC 并不知道连接的是哪个数据库，所以需要注册驱动
- 获取数据库服务器的连接

这两个作用对应着两个方法：

|方法|作用|
|---|---|
|`getConnection(String url, String user, String password)`|通过给定的URL、用户名和密码与数据库服务器建立连接，并返回连接对象|
|`registerDriver(Driver driver)`|注册给定的驱动 Driver|

Mysql 驱动底层通过静态代码块调用了 DriverManager 的 `registerDriver`方法，所以我们不必去显式的进行驱动的注册，它已经帮我们注册好了。

```java
public class Driver extends NonRegisteringDriver implements java.sql.Driver {    public Driver() throws SQLException {    }    static {        try {            DriverManager.registerDriver(new Driver());        } catch (SQLException var1) {            throw new RuntimeException("Can't register driver!");        }    }}
```

#### 3.1.2 注意事项

如果导入 **MySQL5 **之后的驱动 jar 包，可以不用写 `Class.forName("com.mysql.cj.jdbc.Driver");`，DriverManager 会通过上面的方法进而读取 `java.sql.Driver`文件中的驱动类全限定名，进行驱动的注册。

![[JDBC--3.png]]

image.png

### 3.2 Connection

#### 3.2.1 作用

- 获取执行 SQl 的对象
- 管理事务

#### 3.2.2 执行SQL的对象

- `Statement` 普通的执行对象，将获得的字段拼接在 SQl 语句上，然后再执行编译，有 SQl 注入攻击的风险。
- `PerparedStatement` 对 SQL 进行**预编译**的执行对象，先对 SQl 语句进行编译，再用字段将`？`替换掉。
- `CallableStatement` 执行存储过程的对象。

#### 3.2.3 事务管理

|方法|说明|
|---|---|
|`setAutoCommit(boolean)`|开启事务。true：自动提交事务，false：开启手动提交事务，默认开启自动提交|
|`commit()`|提交事务|
|`rollback()`|回滚事务|

```java
try {    // 开启事务    conn.setAutoCommit(false);    // 执行操作一    int i = state.executeUpdate(sql1);    System.out.println(i);    // 制造异常    int j = 1/0;    // 执行操作二    int i1 = state.executeUpdate(sql2);    System.out.println(i1);    // 提交事务    conn.commit();} catch (Exception e) {    // 事务回滚    conn.rollback();    e.printStackTrace();}
```

sql1 执行成功，数据库中并没有增加数据，回滚成功

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678639230262-0a44f21b-27a2-4922-8391-8396b33c8ab9.png#averageHue=%23282a37&clientId=u04fda965-0338-4&from=paste&height=203&id=ud34f4891&originHeight=223&originWidth=761&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=21001&status=done&style=none&taskId=u606c2220-e29e-473e-b677-c993ec57ca0&title=&width=691.8181668234268)

image.png

### 3.3 Statement

#### 3.3.1 作用

执行 sql 语句。

#### 3.3.2 方法

|方法|说明|
|---|---|
|`int executeUpdate(String sql)`|执行给定的SQL语句，这可能是 `INSERT` ， `UPDATE`，或 `DELETE`语句，或者不返回任何内容，如SQL DDL语句的SQL语句。并返回受影响的行数。|
|`ResultSet executeQuery(String sql)`|执行给定的SQL语句，该语句返回单个 `ResultSet`对象。|

### 3.4 ResultSet

#### 3.4.1 作用

封装了查询语句的执行结果，`Statement` 或者 `PreparedStatement` 通过执行 `executeQuery` 方法返回 ResultSet 对象。

#### 3.4.2 方法

| 方法         | 说明                                |
| ---------- | --------------------------------- |
| `getXxx()` | 获取每行中各个数据，可以传入 `列号`（从1开始） 或者 `列名` |
| `next()`   | 向下移动指针，判断该行是否有数据。                 |

![[JDBC--4.png]]

```java
// 6.处理返回结果while (resultSet.next()) {    String name = resultSet.getString("name");    System.out.println(name);    String password = resultSet.getString(3);    System.out.println(password);}
```

### 3.5 PerparedStatement

#### 3.5.1 作用

执行 sql 语句，但是它会对 SQl 语句进行`预编译`，可以防止 `SQl 注入`攻击。并且 `PreparedStatement` 继承自 `Statement` 。

#### 3.5.2 SQl注入

**在编译之前利用 SQl 语句的拼接，输入特殊字符串修改定义好的 SQL 语句，改变 SQL 语句的语义。** 一个简单的 SQL 注入演示： (比如是有一个用户登录的场景，在 DAO 层中进行数据库操作)

```java
String sql = "select * from user where username='"+admin+"' and password='"+'or'1'='1+"'";
```

用户密码一看就不正经，但是可以看一下拼接好的 SQL 语句是怎样的：

```sql
select * from user where username='admin' and password=''or'1'='1'
```

password 变成了空字符串，在 SQL 语句的尾部 拼接了 `or '1'='1'` 。username 和 password 查询为false，但是接着又 or 一个**恒等式**，整体结果就变成了 true，将会查询出所有结果。

#### 3.5.3 预编译

`PrepareStatement` 的使用步骤：

```java
// 3.定义SQLString sql = "select * from user where name=? and password=?";// 4.获取PreparedStatement对象PreparedStatement statement = conn.prepareStatement(sql);// 4.5替换占位符statement.setString(1,"黑夫");statement.setString(2,"123");// 5.执行SQLResultSet resultSet = statement.executeQuery();
```

在定义 SQL 语句时，SQL 中未知字段使用 `？` 进行占位。 可以看到在获取 `PreparedStatement` 对象的同时，就需要将 SQL 传入，这时会将 SQL 语句发送给 mysql 服务器，进行检查语法、编译等。 为什么能达到防止 SQL 注入的效果？ SQL 语句在执行之前已经进行了编译，它的结构已经被固定下来，当运行时动态地把参数传给 PrepraredStatement 时，即使参数里有敏感字符如 or ’1=1’数据库会将它作为一个参数一个字段的**属性值**来处理而不会作为一个 SQL 指令。而且 PreparedStatement 在用具体字段替换占位符时，会对特殊字符进行**转义**，比如单引号 `'` 会转义为 `\\'`。如此，可以有效的防止 SQL 注入。 可以在 `url` 后面加上命令开启预编译：`useServerPrepStmts=true`。不写这句，仍会对特殊字符进行转义。

```java
jdbc:mysql://localhost:3306/test？useSSL=false&useServerPrepStmts=true
```

`useSSL=false` 表示不进行安全连接。

#### 3.5.4 预编译的好处

- 防止 SQL 注入
- **一次编译、多次运行，省去了解析优化等过程** 在执行 SQL 时，很多 SQL 语句都是结构相同的，只是个别字段不同，如果对每一条 SQL 进行编译，那么运行速度将大大降低。预编译时会将编译过后的SQL语句进行缓存，当有结构相同的 SQL 语句需要执行时，只需用属性值替换掉占位符即可，不需要重新**词法语义解析**、**语句优化**、**制定执行计划**、**编译**等，从而提高效率。

关于更多 PreparedStatement 的了解，看这篇博文： [预编译语句(Prepared Statements)介绍，以MySQL为例](https://www.cnblogs.com/micrari/p/7112781.html)

## 4. 数据库连接池

### 4.1 简介

数据库连接池是负责**分配、管理和释放数据库连接**的容器，它允许应用程序**重复使用**一个现有的数据库连接，而不是再重新建立一个。 对于多用户的应用，如果为每一个用户都创建一个数据库连接，毫无疑问是对资源的浪费，同时也浪费时间。我们应当在系统加载的时候就将数据库资源创建好，不推荐运行时再去开启资源。做到对资源的统一管理，从而避免资源浪费，提升响应速度。

### 4.2 使用

**SUN** 公司提供了数据库连接池的标准接口 `DataSource` 并由第三方组织进行实现。 推荐使用 **Druid** 数据库连接池技术。

#### 4.2.1 环境搭建

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.11</version>
</dependency>
```

在`resources`目录下新建`druid.properties`配置文件

```
driverClassName = com.mysql.cj.jdbc.Driver
url = jdbc:mysql://localhost:3306/test
username = root
password = root
# 初始化连接数
initialSize = 5
# 最大连接数
maxActive = 10
# 最大等待时间
maxWait = 3000
```

`maxWait` 表示如果连接池中没有空闲连接的最大等待时间，超过时间则会抛出异常。 更多配置参考：[https://druid.apache.org/docs/latest/configuration/index.html](https://druid.apache.org/docs/latest/configuration/index.html)

#### 4.2.2 获取连接

```java
// 1.加载配置文件Properties properties = new Properties();properties.load(ClassLoader.getSystemResourceAsStream("druid.properties"));// 2.获取连接DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);conn = dataSource.getConnection();
```