# 概述

## 基本概念

## 数据类型

在上述的建表语句中，我们在指定字段的数据类型时，用到了int ，varchar，那么在MySQL中除了 以上的数据类型，还有哪些常见的数据类型呢？ 接下来,我们就来详细介绍一下MySQL的数据类型。 MySQL中的数据类型有很多，主要分为三类：数值类型、字符串类型、日期时间类型。 1.数值类型

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678608908355-6b753cf6-ac0e-4432-8499-caa3d8f13036.png#averageHue=%23fcfcfc&clientId=u3e165ace-3fb1-4&from=paste&height=787&id=u08860ae0&originHeight=866&originWidth=638&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=111375&status=done&style=none&taskId=u9272ffcc-c212-43f2-991a-1bd010e1394&title=&width=579.9999874288388)

image.png

2.字符串类型

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678608938576-4addb0d2-04d4-4583-837b-de9e619d10f2.png#averageHue=%23fcfcfc&clientId=u3e165ace-3fb1-4&from=paste&height=590&id=u5c582a89&originHeight=649&originWidth=846&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=129079&status=done&style=none&taskId=u59d13e08-9e89-40bc-9cde-868341b95ba&title=&width=769.0908924213129)

image.png

char 与 varchar 都可以描述字符串，char是定长字符串，指定长度多长，就占用多少个字符，和字段值的长度无关（剩余长度使用空格补齐） 。而varchar是变长字符串，指定的长度为最大占用长度 。相对来说，char的性能会更高些。

```sql
1). 用户名 username ------> 长度不定, 最长不会超过50username varchar(50)
2). 性别 gender ---------> 存储值, 不是男,就是女gender char(1)
3). 手机号 phone --------> 固定长度为11phone char(11)
```

3.日期时间类型

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678609165386-a24e4a0a-1d34-46bc-94cc-ad2f568935ab.png#averageHue=%23fcfcfc&clientId=u3e165ace-3fb1-4&from=paste&height=583&id=uf9a35aad&originHeight=641&originWidth=1053&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=107233&status=done&style=none&taskId=u817f9d5a-c337-4cb3-a902-5a2751734df&title=&width=957.2727065244002)

image.png

## 安装配置

- **下载解压**
    
    [https://downloads.mysql.com/archives/community/](https://downloads.mysql.com/archives/community/)
    
    下载解压版，能有更多自定义设置。解压到非中文目录。
    
- **环境变量**
    
    cmd管理员运行`mysql`命令出现不能连接到IP端口的提示，表示配置成功。
    
    如果没有任何反应是缺失了动态链接库
    
    [https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170)
    
- **配置文件**
    
    在MySQL根目录下创建一个名为`my.ini`的配置文件，用于配置MySQL的基本参数。参考如下：
    
```bash
    [mysql]
    # 设置mysql客户端默认字符集
    default-character-set = utf8 
    # MySQL 服务端的设置
    [mysqld]
    # 设置 MySQL 安装根目录路径
    basedir=C:/mysql
    # 设置 MySQL 数据文件存放目录
    datadir=C:/mysql/data
    # 服务器的端口号
    port=3306
    # 使用的字符集
    character-set-server=utf8mb4
    # 允许最大连接数
    max_connections=150
    # 启用慢查询日志
    slow_query_log=1
    slow_query_log_file=C:/mysql/logs/slow_query.log
    long_query_time=2
    # 错误日志文件
    log_error=C:/mysql/logs/mysql_error.log
    # 默认存储引擎
    default-storage-engine=INNODB
    # 缓存大小设置
    innodb_buffer_pool_size=1G
    innodb_log_file_size=128M
```
    
- **数据目录**
    
    cmd**管理员**运行初始化命令，初始化数据目录，并会在控制台打印一个随机密码，记下密码，后面登录需要使用。
    
    ```bash
    mysqld --initialize --console
    ```
    
    或者使用如下命令，它不会生成随机密码，而是使用不安全的默认密码（1234）或者不使用密码。
    
    ```bash
    mysqld --initialize -insecure
    ```
    
- **安装服务**
    
    ```bash
    mysqld --install MySQL --defaults-file="D:\\Development\\MySQL\\mysql-9.0.1-winx64\\my.ini"
    ```
    
    - `-install MySQL`：表示安装一个名为 `MySQL` 的服务。
    - `-defaults-file`：指定使用的配置文件路径。
- **启动服务**
    
    ```bash
    net start MySQL
    ```
    
    如果要停止服务使用命令：
    
    ```bash
    net stop MySQL
    ```
    
- **修改密码**
    
    首先需要登录MySQL，输入控制台打印的密码：WkrkynpfK3#=
    
    ```bash
    mysql -u root -p
    ```
    
    修改密码：
    
    ```bash
    ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
    ```
    
- **卸载删除**
    
    首先需要停止服务：
    
    ```bash
    net stop mysql
    ```
    
    删除服务：
    
    ```bash
    mysqld -remove mysql
    ```
    
    最后删除相关目录和环境变量。
    

# SQL

SQL（Structured Query Language，结构化查询语言）是一种专门用于管理和操作关系型数据库的标准编程语言。

## 通用语法

- SQL的一条语句以分号作为结尾。
- 可以通过换行、空格、缩进来增强阅读性。
- SQL不区分大小写，关键字推荐使用大写。
- 注释格式：
    - 单行注释：`--` 或者`#`
    - 多行注释：`/**/`

## DDL

DDL（Data Definition Language，数据定义语言）用于定义和管理数据库对象，如数据库、表、视图和索引等。DDL不直接操作数据，而是用于创建、修改和删除数据库结构。

- **`CREATE`**：用于创建数据库、表、视图、索引等数据库对象。
    
    - 创建数据库
        
        ```sql
        CREATE DATABASE db_name;
        ```
        
    - 创建表
        
        ```sql
        CREATE TABLE table_name (
          column1 datatype [constraint],
          column2 datatype [constraint],
          ...
        );
        ```
        
    - 创建视图
        
        ```sql
        CREATE VIEW view_name AS
        SELECT column1, column2
        FROM table_name
        WHERE condition;
        ```
        
    - 创建索引
        
        ```sql
        CREATE INDEX index_name
        ON table_name (column_name);
        ```
        
- **`ALTER`**：用于修改现有的数据库对象，如表的结构，字段的添加、删除或修改等。
    
    - 添加列
        
        ```sql
        ALTER TABLE table_name
        ADD column_name datatype [constraint];
        ```
        
    - 修改列的数据结构
        
        ```sql
        ALTER TABLE table_name
        MODIFY column_name new_datatype;
        ```
        
    - 删除列
        
        ```sql
        ALTER TABLE table_name
        DROP COLUMN column_name;
        ```
        
    - 重命名列
        
        ```sql
        ALTER TABLE table_name
        RENAME COLUMN old_column_name TO new_column_name;
        ```
        
    - 重命名表
        
        ```sql
        ALTER TABLE old_table_name
        RENAME TO new_table_name;
        ```
        
- **`DROP`**：用于删除数据库对象，如表、数据库、视图、索引等。操作不可逆。
    
    - 删除数据库
        
        ```sql
        DROP DATABASE db_name;
        ```
        
    - 删除表
        
        ```sql
        DROP TABLE table_name;
        ```
        
    - 删除视图
        
        ```sql
        DROP VIEW view_name;
        ```
        
    - 删除索引
        
        ```sql
        DROP INDEX index_name ON table_name;
        ```
        
- **`TRUNCATE`**：用于清空表中的数据，但不删除表的结构。比 `DELETE` 语句更快，并且不记录单行删除的日志，因此不能通过事务回滚恢复。
    
    ```sql
    TRUNCATE TABLE table_name;
    ```
    
- **`RENAME`**：语句用于重命名数据库或表。
    
    - 重命名表
        
        ```sql
        RENAME TABLE old_table_name TO new_table_name;
        ```
        
    - 重命名多个表
        
        ```sql
        RENAME TABLE old_table_name1 TO new_table_name1,
                     old_table_name2 TO new_table_name2;
        ```
        
- **`SHOW`**：语句用于查看数据库或表的结构信息。
    
    - 查看数据库列表
        
        ```sql
        SHOW DATABASES;
        ```
        
    - 查看表结构
        
        ```sql
        DESCRIBE table_name;
        --或者
        SHOW COLUMNS FROM table_name;
        ```
        
    - 查看表的索引
        
        ```sql
        SHOW INDEX FROM table_name;
        ```
        
    - 查看建表语句
        
        ```sql
        SHOW CREATE TABLE table_name;
        ```
        
- **`USE`**：用于选择数据库，并将后续的操作限定在该数据库内。
    
    ```sql
    USE database_name;
    ```
    
- 通过`if exists`或者`if not exists`限定在同名数据库或表存在或不存在时进行删除和创建。
    
- DDL语句在执行后会自动提交，意味着DDL操作不能被回滚。
    

## DML

DML（Data Manipulation Language，数据操作语言）用于对数据库中的数据进行增、删、改操作。

- **`INSERT`**：用于向表中插入新行。可以插入指定列的值，也可以插入所有列的值。
    
    - 插入指定列的值
        
        ```sql
        INSERT INTO table_name (column1, column2, ...)
        VALUES (value1, value2, ...);
        ```
        
    - 插入所有列的值
        
        ```sql
        INSERT INTO table_name
        VALUES (value1, value2, ...);
        ```
        
    - 插入多个所有列的值
        
        ```sql
        INSERT INTO table_name
        VALUES (value1, value2, ...),(value1, value2, ...),...;
        ```
        
- **`UPDATE`**：用于修改表中的现有数据。通过 `WHERE` 子句指定更新条件。
    
    ```sql
    UPDATE table_name
    SET column1 = value1, column2 = value2, ...
    WHERE condition;
    ```
    
- **`DELETE`** ：用于删除表中的一行或多行数据。通过 `WHERE` 子句指定删除条件。
    
    ```sql
    DELETE FROM table_name
    WHERE condition;
    ```
    
- 字符串和日期值要包含在引号内。
    
- DML操作通常和事务结合使用。
    

## DQL

DQL（Data Query Language，数据查询语言）用于从数据库中查询数据。

语法结构如下：

```jsx
SELECT [DISTINCT] column1, column2, ...
FROM table_name
[WHERE condition]
[GROUP BY column1, column2, ...]
[HAVING condition]
[ORDER BY column1, column2, ... ASC|DESC]
[LIMIT index,number];
```

- 基础查询
    
    ```jsx
    SELECT column1, column2 FROM table_name;
    ```
    
    - `*`通配所有字段。
    - 使用`DISTINCT`关键字去除重复记录。
    - 使用`as`关键字为字段设置别名，也可以不用`as`，直接在字段后跟别名。
- 条件查询
    
    ```jsx
    SELECT column1, column2 FROM table_name WHERE condition;
    ```
    
    条件运算符：
    
    - `=`：等于
    - `<>`或`!=`：不等于
    - `>`、`<`、`>=`、`<=`：比较运算
    - `BETWEEN...AND…`：在范围内，包含边界
    - `LIKE`：模糊匹配，`_`匹配单个字符，`%`匹配多个字符。
    - `IN`：在指定列表中
    - `is null`，`is not null`：字段是否为空
    
    逻辑运算符：
    
    - `AND`，`&&`：且
    - `OR`，`||`：或
    - `NOT`，`!`：非
- 聚合函数
    
    ```jsx
    SELECT AVG(salary) FROM employees;
    ```
    
    对列数据进行计算，`NULL`值不参与聚合运算。聚合函数有：
    
    - `COUNT()`: 计算行数，统计总行数使用`count(*)`或`count(1)`
    - `SUM()`: 求和
    - `AVG()`: 平均值
    - `MAX()`: 最大值
    - `MIN()`: 最小值
- 分组查询
    
    ```jsx
    SELECT column1, COUNT(*) 
    FROM table_name 
    [WHERE condition] 
    GROUP BY column1 
    [HAVING condition];
    
    --查询年龄小于45的员工 , 并根据工作地址分组 , 获取员工数量大于等于3的工作地址
    SELECT workaddress,COUNT(*) number FROM emp WHERE age<45 GROUP BY workaddress HAVING number>3;
    ```
    
    - 通常和聚合函数一起使用。
    - `HAVING`对分组之后的结果进行过滤，因为在分组的时候会计算出每个组的聚合值，所以`HAVING`可以对聚合值进行过滤。
- 排序查询
    
    ```jsx
    SELECT column1, column2 FROM table_name ORDER BY column1 ASC|DESC;
    ```
    
    - `ASC`，`DESC`：升序（默认），降序
    - 多字段排序时，当第一个字段相等时，才会进行第二个字段的排序。
- 分页查询
    
    ```jsx
    SELECT column1, column2 FROM table_name LIMIT index,number;
    SELECT column1, column2 FROM table_name LIMIT number OFFSET index;
    ```
    
    - `index`：起始索引
    - `number`：查询的记录数
    
    分页开发中，需要分页的当前页码：
    
    ```jsx
    SELECT column1, column2 FROM table_name LIMIT (page_number - 1) * page_size,page_size;
    SELECT column1, column2 FROM table_name LIMIT page_size OFFSET (page_number - 1) * page_size;
    ```
    
    - `page_size`：每页的记录数。
    - `page_number`：当前页码（从1开始）。

DQL语句的执行顺序为：

1. **`FROM`** ：从表中提取数据
2. **`WHERE`** ：保留符合条件的记录
3. **`GROUP BY`**：根据指定列进行分组
4. **`HAVING`** ：对分组之后的结果进行过滤
5. **`SELECT`** ：从结果集中选出需要的列
6. **`ORDER BY`** ：对结果集进行排序
7. **`LIMIT`** ：限制返回的记录数

在`FROM`之后，子句中如果有函数，将在子句中执行函数。

- 测试数据
    
    ```sql
    drop table if exists employee;
    create table emp(
      id int comment '编号',
      workno varchar(10) comment '工号',
      name varchar(10) comment '姓名',
      gender char(1) comment '性别',
      age tinyint unsigned comment '年龄',
      idcard char(18) comment '身份证号',
      workaddress varchar(50) comment '工作地址',
      entrydate date comment '入职时间')comment '员工表';
    INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
    VALUES (1, '00001', '柳岩666', '女', 20, '123456789012345678', '北京', '2000-01-01');
    INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
    VALUES (2, '00002', '张无忌', '男', 18, '123456789012345670', '北京', '2005-09-01');
    INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
    VALUES (3, '00003', '韦一笑', '男', 38, '123456789712345670', '上海', '2005-08-01');
    INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
    VALUES (4, '00004', '赵敏', '女', 18, '123456757123845670', '北京', '2009-12-01');
    INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
    VALUES (5, '00005', '小昭', '女', 16, '123456769012345678', '上海', '2007-07-01');
    INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
    VALUES (6, '00006', '杨逍', '男', 28, '12345678931234567X', '北京', '2006-01-01');
    INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
    VALUES (7, '00007', '范瑶', '男', 40, '123456789212345670', '北京', '2005-05-01');
    INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
    VALUES (8, '00008', '黛绮丝', '女', 38, '123456157123645670', '天津', '2015-05-01');
    INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
    VALUES (9, '00009', '范凉凉', '女', 45, '123156789012345678', '北京', '2010-04-01');
    INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
    VALUES (10, '00010', '陈友谅', '男', 53, '123456789012345670', '上海', '2011-01-01');
    INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
    VALUES (11, '00011', '张士诚', '男', 55, '123567897123465670', '江苏', '2015-05-01');
    INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
    VALUES (12, '00012', '常遇春', '男', 32, '123446757152345670', '北京', '2004-02-01');
    INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
    VALUES (13, '00013', '张三丰', '男', 88, '123656789012345678', '江苏', '2020-11-01');
    INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
    VALUES (14, '00014', '灭绝', '女', 65, '123456719012345670', '西安', '2019-05-01');
    INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
    VALUES (15, '00015', '胡青牛', '男', 70, '12345674971234567X', '西安', '2018-04-01');
    INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
    VALUES (16, '00016', '周芷若', '女', 18, null, '北京', '2012-06-01');
    ```
    

## DCL

DCL（Data control Language，数据控制语言）用于控制对数据库中数据的访问权限。

- 用户控制
    
    MYSQL中使用`user_name@host_name`的形式标识用户。主机名可以通过`%`进行通配。
    
    - 查询用户
        
        ```sql
        select * from mysql.user;
        ```
        
    - 创建用户
        
        ```sql
        CREATE USER 'user_name'@'host_name' IDENTIFIED BY 'password';
        ```
        
    - 修改用户密码
        
        ```sql
        ALTER USER 'user_name'@'user_name' IDENTIFIED WITH mysql_native_password BY 'new_password' ;
        ```
        
    - 删除用户
        
        ```sql
        DROP USER 'user_name'@'user_name' ;
        ```
        
- 权限控制
    
    常用的权限列表：
    
    ```sql
    ALL.ALL PRIVILEGES --所有权限
    SELECT             --查询数据
    INSERT             --插入数据
    DELETE             --删除数据
    UPDATE             --修改数据
    ALTER              --修改表
    DROP               --删除数据库、表等
    CREATE             --创建数据库、表等
    ```
    
    数据库名和表明可以用`*`进行通配。
    
    - 查询权限
        
        ```sql
        SHOW GRANTS FOR '用户名'@'主机名' ;
        ```
        
    - 授予权限
        
        ```sql
        GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';
        ```
        
    - 撤销权限
        
        ```sql
        REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
        ```
        

# 函数

MYSQL中函数是预定义操作的代码块。可以直接调用MYSQL提供的内置函数，也可以自己创建自定义函数。

## 内置函数

- 字符串函数
    
    - `LENGTH(str)`：返回字符串的长度
    - `CONCAT(s1,s2,…)`：连接两个或多个字符串
    - `UPPER(str) 和 LOWER(str)`：将字符串转换为大写或小写
    - `SUBSTRING(str,start,len)`：返回字符串`str`冲`start`开始的`len`个长度的子字符串
    - `LPAD(str,n,pad)和RPAD(str,n,pad)`：对字符串`str`用`pad`向左或向右填充到`n`个长度
    - `TRIM(str)`：去除字符串头尾空格
    
    ```sql
    SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM employees;
    ```
    
- 数值函数
    
    - `CEIL(x)`：向上取整
    - `FLOOR(x)`：向下取整
    - `MOD(x,y)`：返回x/y的模
    - `PAND()`：返回0~1内的随机数
    - `ROUND(x,y)`：对数字x四舍五入，保留y位小数
    
    ```sql
    SELECT MOD(age,id) FROM emp;
    ```
    
- 日期函数
    

## 自定义函数

# 约束
select  
            v.video_id, v.code, v.title, v.cover_url, v.embed_url, v.release_date, v.category, v.del_flag, v.remark, v.create_by, v.create_time, v.update_by, v.update_time,  
            a.author_id as author_id, a.author_name as author_name, a.original_name as author_original_name, a.avatar_url as author_avatar_url,  
            t.tag_id as tag_id, t.tag_name as tag_name  
        from  
            video v  
        left join video_author va on v.video_id = va.video_id  
        left join author a on va.author_id = a.author_id  
        left join video_tag vt on v.video_id = vt.video_id  
        left join tag t on vt.tag_id = t.tag_id  
        where del_flag = '0'  
        limit 0 , 25;  
  
  
        SELECT m.video_id,  
               m.code,  
               m.title,  
               m.cover_url,  
               m.embed_url,  
               m.release_date,  
               a.author_id            AS author_id,  
               a.author_name          AS author_name,  
               a.original_name AS author_original_name,  
               a.avatar_url    AS author_avatar_url,  
               t.tag_id            AS tag_id,  
               t.tag_name          AS tag_name  
  
        FROM (SELECT video_id, code, title, cover_url, embed_url, release_date  
              FROM video  
              ORDER BY video_id  
              LIMIT 25 OFFSET 0) m  
                 LEFT JOIN video_author ma ON m.video_id = ma.video_id  
                 LEFT JOIN author a ON ma.author_id = a.author_id  
                 LEFT JOIN video_tag mt ON m.video_id = mt.video_id  
                 LEFT JOIN tag t ON mt.tag_id = t.tag_id  
        ORDER BY m.video_id, a.author_id, t.tag_id;  
  
        SELECT video_id, code, title, cover_url, embed_url, release_date  
              FROM video  
              ORDER BY video_id  
              LIMIT 25 OFFSET 0;
# 多表查询

# 事务

# 存储引擎

# 索引

# SQL优化

# 视图

# 存储过程

# 触发器

# 锁

# 1. MySQL概述

## 1.1 数据库相关概念

- 数据库（DB）：有组织存储数据的仓库。
- 数据库管理系统（DBMS）：操纵和管理数据库的大型软件。
- SQL：操作关系型数据库的一套通用标准语言。

关系型数据库中比较有名的有：Oracle、MySQL。

## 1.2 MySQL数据库

### 1.2.1 版本

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667067494592-1d00534b-8948-439d-a9eb-19a545d9ffa8.png)

官方： **https://www.mysql.com/**

MySQL官方提供了两种不同的版本：

- 社区版本（MySQL Community Server）免费， MySQL不提供任何技术支持
- 商业版本（MySQL Enterprise Edition）收费，可以使用30天，官方提供技术支持

本课程采用的是MySQL最新的社区版-MySQL Community Server 8.0.26

### 1.2.2 下载

下载地址：**https://downloads.mysql.com/archives/installer/**

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667067637915-14db1147-5c46-49ba-b217-25c4cb1cb5e8.png)

### 1.2.3 安装

要想使用MySQL，我们首先先得将MySQL安装好，我们可以根据下面的步骤，一步一步的完成MySQL的安装。

1). 双击官方下来的安装包文件

2). 根据安装提示进行安装

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667067697244-bbbdad96-01bc-4e30-b367-f306ba591c32.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667067709614-f0ac2dfe-261a-421d-b1ce-c3a2f73827d5.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667067722774-348197c3-edaf-491d-92ed-edd05468c86d.png)

安装MySQL的相关组件，这个过程可能需要耗时几分钟，耐心等待。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667067740275-f2f788e7-001b-4f30-b432-bb2083a794ee.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667067749970-13923985-a47e-480c-9e73-fe927d449ce9.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667067760769-f9001797-f34f-4500-88fc-f115bd600074.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667067772113-97eecfe7-d6ad-4a65-86d2-40d5ed7c8990.png)

**输入MySQL中root用户的密码,一定记得记住该密码**

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667067798023-26424ce1-41af-4f56-828b-fd44dc4d0686.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667067809697-d2928f30-9f44-4d4c-b156-9366ba87497e.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667067819376-a6a3f848-0d00-4846-b751-7817e18e1270.png)

3). 配置

安装好MySQL之后，还需要配置环境变量，这样才可以在任何目录下连接MySQL。

A. 在此电脑上，右键选择属性

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667067851538-d514dc84-8c6c-4aea-af18-e744407b4860.png)

B. 点击左侧的 "高级系统设置"，选择环境变量

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667067867959-09ca12c8-0ce0-4f7d-afd0-350ad35f845d.png)

C. 找到 Path 系统变量, 点击 "编辑"

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667067897180-9779e58d-55b1-4217-b00a-f34918cd170f.png)

D. 选择 "新建" , 将MySQL Server的安装目录下的bin目录添加到环境变量

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667067917450-33fbc40e-6ed7-4ef0-b004-84a8d81a41c5.png)

### 1.2.4 启动停止

MySQL安装完成之后，在系统启动时，会自动启动MySQL服务，我们无需手动启动了。

当然，也可以手动的通过指令启动停止，以管理员身份运行cmd，进入命令行执行如下指令：

```
net start mysql80
net stop mysql80
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667068001991-b130a29e-bfaf-4b3d-9829-bc82babbdac6.png)

注意 ： 上述的 mysql80 是我们在安装MySQL时，默认指定的mysql的系统服务名，不是固定的，如果未改动，默认就是mysql80。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667068031895-0cd5ada3-fdec-4c33-9411-d751ea744fdc.png)

### 1.2.5 客户端连接

1). 方式一：使用MySQL提供的客户端命令行工具

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667068065991-b3e1a8db-1185-43c7-9988-61617e42bfe9.png)

2). 方式二：使用系统自带的命令行工具执行指令

```
mysql [-h 127.0.0.1] [-P 3306] -u root -p

参数：
-h : MySQL服务所在的主机IP
-P : MySQL服务端口号， 默认3306
-u : MySQL数据库用户名
-p ： MySQL数据库用户名对应的密码
```

[]内为可选参数，如果需要连接远程的MySQL，需要加上这两个参数来指定远程主机IP、端口，如果连接本地的MySQL，则无需指定这两个参数。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667068161097-4cba6867-1584-4240-93d6-9187949349e1.png)

注意： 使用这种方式进行连接时，需要安装完毕后配置PATH环境变量。

### 1.2.6 数据模型

1). 关系型数据库（RDBMS）

概念：建立在关系模型基础上，由多张相互连接的二维表组成的数据库。而所谓二维表，指的是由行和列组成的表，如下图（就类似于Excel表格数据，有表头、有列、有行，还可以通过一列关联另外一个表格中的某一列数据）。我们之前提到的MySQL、Oracle、DB2、SQLServer这些都是属于关系型数据库，里面都是基于二维表存储数据的。简单说，基于二维表存储数据的数据库就成为关系型数据库，不是基于二维表存储数据的数据库，就是非关系型数据库。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678605275645-60a80d58-bbb0-495d-9d38-7e649309ecc1.png)

特点：

A. 使用表存储数据，格式统一，便于维护。

B. 使用SQL语言操作，标准统一，使用方便。

2). 数据模型

MySQL是关系型数据库，是基于二维表进行数据存储的，具体的结构图下:

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667068292282-f887b329-d334-4034-9761-cd06ecc5a01f.png)

- 我们可以通过MySQL客户端连接数据库管理系统DBMS，然后通过DBMS创建和操作数据库。
- 可以使用SQL语句，通过数据库管理系统操作数据库，以及操作数据库中的表结构及数据。
- 一个数据库服务器中可以创建多个数据库，一个数据库中也可以包含多张表，而一张表中又可以包含多行记录。

### 2.1 MySQL安装

**安装环境:Win10 64位****软件版本:MySQL 5.7.24 解压版**

#### 2.1.1 下载

[https://downloads.mysql.com/archives/community/](https://downloads.mysql.com/archives/community/)

点开上面的链接就能看到如下界面：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604624185-e7ef8551-164b-438a-b08d-d0771ea71f43.png)

选择选择和自己**系统位数**相对应的版本点击右边的**Download**，此时会进到另一个页面，同样在接近页面底部的地方找到如下图所示的位置：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604564227-faa0f19a-4519-4282-9e28-2e8a25693cb8.png)

不用理会上面的登录和注册按钮，直接点击 **No thanks, just start my download.** 就可以下载。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604653647-862b330f-273e-4f18-a8bc-143bd56dedf8.png)

#### 2.1.2 安装(解压)

下载完成后我们得到的是一个压缩包，将其解压，我们就可以得到MySQL 5.7.24的软件本体了(就是一个文件夹)，我们可以把它放在你想安装的位置。

---

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604675235-2e25d410-d234-442d-bd5a-fab118d115e1.png)

### 2.2 MySQL卸载

如果你想卸载MySQL，也很简单。

右键开始菜单，选择**命令提示符(管理员)**，打开黑框。

1. 敲入**net stop mysql**，回车。

 net stop mysql

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604786109-1f432fe4-9689-4b5e-a274-c804ae459a55.png)

1. 再敲入**mysqld -remove mysql**，回车。

 mysqld -remove mysql

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604800012-3175052c-1b87-4ceb-b5e5-c96ba450ba37.png)

1. 最后删除MySQL目录及相关的环境变量。

**至此，MySQL卸载完成！**

### 2.3 MySQL配置

#### 2.3.1 添加环境变量

环境变量里面有很多选项，这里我们只用到**Path**这个参数。为什么在初始化的开始要添加环境变量呢？在黑框(即CMD)中输入一个可执行程序的名字，Windows会先在环境变量中的**Path**所指的路径中寻找一遍，如果找到了就直接执行，没找到就在当前工作目录找，如果还没找到，就报错。我们添加环境变量的目的就是能够在任意一个黑框直接调用MySQL中的相关程序而不用总是修改工作目录，大大简化了操作。

右键**此电脑**→**属性**，点击**高级系统设置**

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604813189-b5526043-23fb-4b3d-83e4-ac0993b84584.png)

点击**环境变量**

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604822253-4498c017-f1f5-40d5-8206-6861b485d689.png)

在**系统变量**中新建MYSQL_HOME

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604834011-52286621-ad61-4bfb-91d7-ba67f58a32e9.png)

在**系统变量**中找到并**双击****Path**

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604843475-d35719ea-b5e6-4844-b5c0-a2757868a7b2.png)

点击**新建**

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604853600-51be419a-0fd6-410a-a6e9-72d7f3e2a25e.png)

最后点击确定。

**如何验证是否添加成功？**

右键开始菜单(就是屏幕左下角)，选择**命令提示符(管理员)**，打开黑框，敲入**mysql**，回车。如果提示**Can't connect to MySQL server on 'localhost'**则证明添加成功；如果提示**mysql不是内部或外部命令，也不是可运行的程序或批处理文件**则表示添加添加失败，请重新检查步骤并重试。

#### 2.3.2 新建配置文件

新建一个文本文件，内容如下：

```
[mysql]
 default-character-set=utf8
 
 [mysqld]
 character-set-server=utf8
 default-storage-engine=INNODB
 sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
```

把上面的文本文件另存为，在保存类型里选**所有文件 (*.*)**，文件名叫**my.ini**，存放的路径为MySQL的**根目录**(例如我的是**D:\software\mysql-5.7.24-winx64**,根据自己的MySQL目录位置修改)。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604895496-f17715ec-7d34-438d-9572-19432f931b0e.png)

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604905142-6ff97ee2-a125-4f9d-9ef9-6806fee0ad34.png)

上面代码意思就是配置数据库的默认编码集为utf-8和默认存储引擎为INNODB。

#### 2.3.3 初始化MySQL

在刚才的黑框中敲入**mysqld --initialize-insecure**，回车，稍微等待一会，如果出现没有出现报错信息(如下图)则证明data目录初始化没有问题，此时再查看MySQL目录下已经有data目录生成。

 mysqld --initialize-insecure

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604917099-fb82b8b9-05b3-481b-b936-02a0e93744ac.png)

tips：如果出现如下错误

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604930774-2882ad4e-20c0-4ad6-bbb3-e9a130c42f17.png)

是由于权限不足导致的，去**C:\Windows\System32** 下以管理员方式运行 cmd.exe

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604948405-8e5e0e58-f075-4b3e-8394-934692a95c27.png)

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604954611-03beda57-3295-4071-878e-16e9395dd57d.png)

#### 2.3.4 注册MySQL服务

在黑框里敲入**mysqld -install**，回车。

 mysqld -install

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604967771-01ccac3c-d14f-4350-9685-8f247ae81c44.png)

现在你的计算机上已经安装好了MySQL服务了。

MySQL服务器

#### 2.3.5 启动MySQL服务

在黑框里敲入**net start mysql**，回车。

```
net start mysql  // 启动mysql服务
     
 net stop mysql  // 停止mysql服务
```

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604981140-12bcad50-11c6-495a-aa81-15eba874c76c.png)

#### 2.3.6 修改默认账户密码

在黑框里敲入**mysqladmin -u root password 1234**，这里的**1234**就是指默认管理员(即root账户)的密码，可以自行修改成你喜欢的。

 mysqladmin -u root password 1234

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678604996464-ad59d1c2-2da2-4736-9273-03470f76e822.png)

**至此，MySQL 5.7 解压版安装完毕！**

### 2.4 MySQL登陆和退出

#### 2.4.1 登陆

右键开始菜单，选择**命令提示符**，打开黑框。在黑框中输入，**mysql -uroot -p1234**，回车，出现下图且左下角为**mysql>**，则登录成功。

 mysql -uroot -p1234

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678605013855-944c8a74-e4db-4141-8d00-9f74f2285cdf.png)

**到这里你就可以开始你的MySQL之旅了！**

登陆参数：

 mysql -u用户名 -p密码 -h要连接的mysql服务器的ip地址(默认127.0.0.1) -P端口号(默认3306)

#### 2.4.2 退出

退出mysql：

```
exit
 quit
```

  

# 2.SQL

全称 **Structured Query Language**，结构化查询语言。

操作关系型数据库的编程语言，**定义了一套操作关系型数据库统一标准** 。当然对于同一种需求，每一种数据库，操作的方式可能存在不一样的情况，称之为**方言**。

## 2.1 SQL通用语法

在学习具体的SQL语句之前，先来了解一下SQL语言的统一语法。

- SQL语句可以单行或多行书写，以**分号结尾**。
- SQL语句可以使用空格、缩进来增强语句的可读性。
- MySQL数据库的SQL语句**不区分大小写**，关键字建议使用大写。
- 注释：

- 单行注释：-- 注释内容 或 # 注释内容
- 多行注释：/* 注释内容 */

## 2.2 SQL分类

SQL语句，根据其功能，主要分为四类：DDL、DML、DQL、DCL。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667068493398-1641e43a-dcff-4a57-a486-845afd80bf71.png)

## 2.3 DDL

`Data Definition Language`，数据定义语言，用来定义数据库对象(数据库，表，字段) 。

### 2.3.1 数据库操作

1. **查询所有数据库**

```
show databases ;
```

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678607765669-9bd15035-5ee7-4f31-80b0-cb861c882be7.png)

2. **查询当前数据库**

```
select database() ;
```

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678608150405-9acce45f-36af-4515-9e16-2e3834566ae6.png)

**注意：不要忘记加括号！**

3. **创建数据库**

```
create database [ if not exists ] 数据库名 [ default charset 字符集 ] [ collate 排序规则 ] ;
```

eg1：创建一个test11数据库, 使用数据库默认的字符集。

```
create database test11;
```

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678608268313-f35dfd04-7a56-4295-9638-a9ba8b17c58b.png)

在同一个数据库服务器中，不能创建两个名称相同的数据库，否则将会报错。

可以通过`if not exists` 参数来解决这个问题，数据库不存在, 则创建该数据库，如果存在，则不创建。

```
create database if not extists test11;
```

eg2：创建一个itheima数据库，并且指定字符集

```
create database itheima default charset utf8mb4;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667068782849-dba0acf9-f134-41c2-826a-b94075a658ff.png)

**4). 删除数据库**

```
drop database [ if exists ] 数据库名 ;
```

如果删除一个不存在的数据库，将会报错。此时，可以加上参数 if exists ，如果数据库存在，再执行删除，否则不执行删除。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678607249273-94eaf890-4dea-4c2d-b01a-eb235e00100a.png)

5）切换数据库

```
use 数据库名 ;
```

我们要操作某一个数据库下的表时，就需要通过该指令，切换到对应的数据库下，否则是不能操作的。 比如，切换到itcast数据，执行如下SQL：

```
use itcast;
```

### 2.3.2 表操作

#### 2.3.2.1 查询创建

1）查询当前数据库所有表

```
show tables;
```

比如,我们可以切换到sys这个系统数据库,并查看系统数据库中的所有表结构。

```
use sys; 
show tables;
```

2）查看指定表结构

```
desc 表名 ;
```

通过这条指令，我们可以查看到指定表的字段，字段的类型、是否可以为NULL，是否存在默认值等信 息。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678608576908-fbeee717-ee0d-46eb-95b7-dc49ffdb8dda.png)

3. 查询指定表的建表语句

```
show create table 表名 ;
```

通过这条指令，主要是用来查看建表语句的，而有部分参数我们在创建表的时候，并未指定也会查询 到，因为这部分是数据库的默认值，如：存储引擎、字符集等。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678608667208-6e5c96f2-4800-4523-b433-2c8b5f5267e4.png)

4.创建表结构

```
CREATE TABLE 表名( 
	字段1 字段1类型 [ COMMENT 字段1注释 ], 
  字段2 字段2类型 [COMMENT 字段2注释 ], 
  字段3 字段3类型 [COMMENT 字段3注释 ], 
  ...... 
  字段n 字段n类型 [COMMENT 字段n注释 ]
) [ COMMENT 表注释 ] ;
```

注意: [...] 内为可选参数，最后一个字段后面没有逗号

比如，我们创建一张表 tb_user ，对应的结构如下，那么建表语句为：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678608791785-067ac3aa-568d-4538-a696-7b8f7b57f7d7.png)

```
create table tb_user( 
  id int comment '编号', 
  name varchar(50) comment '姓名', 
  age int comment '年龄', 
  gender varchar(1) comment '性别'
) comment '用户表';
```

#### 2.3.2.2 数据类型

在上述的建表语句中，我们在指定字段的数据类型时，用到了int ，varchar，那么在MySQL中除了 以上的数据类型，还有哪些常见的数据类型呢？ 接下来,我们就来详细介绍一下MySQL的数据类型。

MySQL中的数据类型有很多，主要分为三类：数值类型、字符串类型、日期时间类型。

1.数值类型

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678608908355-6b753cf6-ac0e-4432-8499-caa3d8f13036.png)

2.字符串类型

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678608938576-4addb0d2-04d4-4583-837b-de9e619d10f2.png)

char 与 varchar 都可以描述字符串，char是定长字符串，指定长度多长，就占用多少个字符，和字段值的长度无关（剩余长度使用空格补齐） 。而varchar是变长字符串，指定的长度为最大占用长度 。相对来说，char的性能会更高些。

```
1). 用户名 username ------> 长度不定, 最长不会超过50 
username varchar(50)
2). 性别 gender ---------> 存储值, 不是男,就是女 
gender char(1)
3). 手机号 phone --------> 固定长度为11
phone char(11)
```

3.日期时间类型

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678609165386-a24e4a0a-1d34-46bc-94cc-ad2f568935ab.png)

```
1). 生日字段 birthday 
birthday date
2). 创建时间 createtime
createtime datetime
```

#### 2.3.2.3 案例

设计一张员工信息表，要求如下：

1. 编号（纯数字）

2. 员工工号 (字符串类型，长度不超过10位)

3. 员工姓名（字符串类型，长度不超过10位）

4. 性别（男/女，存储一个汉字）

5. 年龄（正常人年龄，不可能存储负数）

6. 身份证号（二代身份证号均为18位，身份证中有X这样的字符）

7. 入职时间（取值年月日即可）

对应的建表语句如下:

```
create table emp(
id int comment '编号',
workno varchar(10) comment '工号',
name varchar(10) comment '姓名',
gender char(1) comment '性别',
age tinyint unsigned comment '年龄',
idcard char(18) comment '身份证号',
entrydate date comment '入职时间'
) comment '员工表';
```

SQL语句编写完毕之后，就可以在MySQL的命令行中执行SQL，然后也可以通过 desc 指令查询表结构信息：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678609356940-1a8305c6-b51e-404d-9c81-6ecba58b22aa.png)

表结构创建好了，里面的name字段是varchar类型，最大长度为10，也就意味着如果超过10将会报错，如果我们想修改这个字段的类型 或 修改字段的长度该如何操作呢？接下来再来讲解DDL语句中，如何操作表字段。

#### 2.3.2.4 修改

1.添加字段

```
ALTER TABLE 表名 ADD 字段名 类型 (长度) [ COMMENT 注释 ] [ 约束 ];
```

eg：为emp表增加一个新的字段”昵称”为nickname，类型为varchar(20)

```
ALTER TABLE emp ADD nickname varchar(20) COMMENT '昵称'; 
```

2.修改数据类型

```
ALTER TABLE 表名 MODIFY 字段名 新数据类型 (长度);
```

3.修改字段名和字段类型

```
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型 (长度) [ COMMENT 注释 ] [ 约束 ];
```

eg：将emp表的nickname字段修改为username，类型为varchar(30)

```
ALTER TABLE emp CHANGE nickname username varchar(30) COMMENT '昵称';
```

4.删除字段

```
ALTER TABLE 表名 DROP 字段名;
```

eg：将emp表的字段username删除

```
ALTER TABLE emp DROP username;
```

5.修改表名

```
ALTER TABLE 表名 RENAME TO 新表名;
```

eg：将emp表的表名修改为 employee

```
ALTER TABLE emp RENAME TO employee;
```

#### 2.3.2.5 删除

1.删除表

```
DROP TABLE [ IF EXISTS ] 表名;
```

可选项 IF EXISTS 代表，只有表名存在时才会删除该表，表名不存在，则不执行删除操作(如果不加该参数项，删除一张不存在的表，执行将会报错)。

eg：如果tb_user表存在，则删除tb_user表

```
DROP TABLE IF EXISTS tb_user;
```

2.删除指定表，并重新创建表

```
TRUNCATE TABLE 表名;
```

注意: 在删除表的时候，表中的全部数据也都会被删除

## 2.4 图形化界面工具

上述，我们已经讲解了通过DDL语句，如何操作数据库、操作表、操作表中的字段，而通过DDL语句执

行在命令进行操作，主要存在以下两点问题：

1).会影响开发效率 ;

2). 使用起来，并不直观，并不方便 ；

所以呢，我们在日常的开发中，会借助于MySQL的图形化界面，来简化开发，提高开发效率。而目前

mysql主流的图形化界面工具，有以下几种：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678609793489-6c0ea727-0340-4219-a36e-a93edec92b35.png)

而本次课程中，选择最后一种DataGrip，这种图形化界面工具，功能更加强大，界面提示更加友好，

是我们使用MySQL的不二之选。接下来，我们来介绍一下DataGrip该如何安装、使用。

## 2.5 DML

DML英文全称是Data Manipulation Language(数据操作语言)，用来对数据库中表的数据记录进行增、删、改操作。

添加数据（INSERT）

修改数据（UPDATE）

删除数据（DELETE）

### 2.5.1 添加数据

1.给指定字段添加数据

```
INSERT INTO 表名 (字段名1, 字段名2, ...) VALUES (值1, 值2, ...);
```

eg：给employee表所有的字段添加数据 ；

```
insert into employee(id,workno,name,gender,age,idcard,entrydate)
values(1,'1','Itcast','男',10,'123456789012345678','2000-01-01');
```

插入数据完成之后，我们有两种方式，查询数据库的数据：

A. 方式一

在左侧的表名上双击，就可以查看这张表的数据。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678609975887-17ee0cee-be5f-451c-b404-2f158129543f.png)

B. 方式二

可以直接一条查询数据的SQL语句, 语句如下

```
select * from employee; 
```

eg：给employee表所有的字段添加数据

执行如下SQL，添加的年龄字段值为-1

```
insert into employee(id,workno,name,gender,age,idcard,entrydate)
values(1,'1','Itcast','男',-1,'123456789012345678','2000-01-01');
```

执行上述的SQL语句时，报错了，具体的错误信息如下：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678610041960-2d74ae63-c5a9-44a3-8775-407d91518e72.png)

因为 employee 表的age字段类型为 tinyint，而且还是无符号的 unsigned ，所以取值只能在0-255 之间。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678610072702-5de9c7fa-dd2e-4ada-a532-409ba0122f8d.png)

2.给全部字段添加信息

```
INSERT INTO 表名 VALUES (值1, 值2, ...);
```

3.批量添加数据

```
INSERT INTO 表名 (字段名1, 字段名2, ...) VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...) ;
```

```
INSERT INTO 表名 VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...) ;
```

注意事项：

插入数据时，指定的字段顺序需要与值的顺序是一一对应的。

字符串和日期型数据应该包含在引号中。

插入的数据大小，应该在字段的规定范围内

### 2.5.2 修改数据

修改数据的具体语法为:

```
UPDATE 表名 SET 字段名1 = 值1 , 字段名2 = 值2 , .... [ WHERE 条件 ] ;
```

eg：

A. 修改id为1的数据，将name修改为itheima

```
update employee set name = 'itheima' where id = 1;
```

B. 修改id为1的数据, 将name修改为小昭, gender修改为 女

```
update employee set name = '小昭' , gender = '女' where id = 1;
```

C. 将所有的员工入职日期修改为 2008-01-01

```
update employee set entrydate = '2008-01-01';
```

注意事项：

修改语句的条件可以有，也可以没有，如果没有条件，则会修改整张表的所有数据。

### 2.5.3 删除数据

删除数据的具体语法为：

```
DELETE FROM 表名 [ WHERE 条件 ] ;
```

eg：

A. 删除gender为女的员工

```
delete from employee where gender = '女';
```

B. 删除所有员工

```
delete from employee;
```

注意事项：

• DELETE 语句的条件可以有，也可以没有，如果没有条件，则会删除整张表的所有数据。

• DELETE 语句不能删除某一个字段的值(可以使用UPDATE，将该字段值置为NULL即可)。

• 当进行删除全部数据操作时，datagrip会提示我们，询问是否确认删除，我们直接点击Execute即可。

## 2.6 DQL

DQL英文全称是Data Query Language(数据查询语言)，数据查询语言，用来查询数据库中表的记录。

查询关键字: SELECT

在一个正常的业务系统中，查询操作的频次是要远高于增删改的，当我们去访问企业官网、电商网站，在这些网站中我们所看到的数据，实际都是需要从数据库中查询并展示的。而且在查询的过程中，可能还会涉及到条件、排序、分页等操作。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678610525316-25570cea-24e5-4d2b-81c3-569af7cae9cc.png)

那么，本小节我们主要学习的就是如何进行数据的查询操作。 我们先来完成如下数据准备工作:

```
drop table if exists employee;
create table emp(
	id int comment '编号',
	workno varchar(10) comment '工号',
	name varchar(10) comment '姓名',
	gender char(1) comment '性别',
	age tinyint unsigned comment '年龄',
	idcard char(18) comment '身份证号',
	workaddress varchar(50) comment '工作地址',
	entrydate date comment '入职时间'
)comment '员工表';

INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (1, '00001', '柳岩666', '女', 20, '123456789012345678', '北京', '2000-01-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (2, '00002', '张无忌', '男', 18, '123456789012345670', '北京', '2005-09-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (3, '00003', '韦一笑', '男', 38, '123456789712345670', '上海', '2005-08-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (4, '00004', '赵敏', '女', 18, '123456757123845670', '北京', '2009-12-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (5, '00005', '小昭', '女', 16, '123456769012345678', '上海', '2007-07-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (6, '00006', '杨逍', '男', 28, '12345678931234567X', '北京', '2006-01-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (7, '00007', '范瑶', '男', 40, '123456789212345670', '北京', '2005-05-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (8, '00008', '黛绮丝', '女', 38, '123456157123645670', '天津', '2015-05-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (9, '00009', '范凉凉', '女', 45, '123156789012345678', '北京', '2010-04-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (10, '00010', '陈友谅', '男', 53, '123456789012345670', '上海', '2011-01-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (11, '00011', '张士诚', '男', 55, '123567897123465670', '江苏', '2015-05-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (12, '00012', '常遇春', '男', 32, '123446757152345670', '北京', '2004-02-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (13, '00013', '张三丰', '男', 88, '123656789012345678', '江苏', '2020-11-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (14, '00014', '灭绝', '女', 65, '123456719012345670', '西安', '2019-05-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (15, '00015', '胡青牛', '男', 70, '12345674971234567X', '西安', '2018-04-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (16, '00016', '周芷若', '女', 18, null, '北京', '2012-06-01');
```

准备完毕后，我们就可以看到emp表中准备的16条数据。接下来，我们再来完成DQL语法的学习。

### 2.6.1 基本语法

DQL 查询语句，语法结构如下：

```
SELECT
	字段列表
FROM
	表名列表
WHERE
	条件列表
GROUP BY
	分组字段列表
HAVING
	分组后条件列表
ORDER BY
	排序字段列表
LIMIT
	分页参数
```

我们在讲解这部分内容的时候，会将上面的完整语法进行拆分，分为以下几个部分：

基本查询（不带任何条件）

条件查询（WHERE）

聚合函数（count、max、min、avg、sum）

分组查询（group by）

排序查询（order by）

分页查询（limit）

### 2.6.2 基础查询

在基本查询的DQL语句中，不带任何的查询条件，查询的语法如下：

1.查询多个字段

```
SELECT 字段1, 字段2, 字段3 ... FROM 表名 ;
```

```
SELECT * FROM 表名 ;
```

* 号代表查询所有字段，在实际开发中尽量少用（不直观、影响效率）。

2.字段设置别名

```
SELECT 字段1 [ AS 别名1 ] , 字段2 [ AS 别名2 ] ... FROM 表名;
```

```
SELECT 字段1 [ 别名1 ] , 字段2 [ 别名2 ] ... FROM 表名; 
```

3.去除重复记录

```
SELECT DISTINCT 字段列表 FROM 表名;
```

eg：

A. 查询指定字段 name, workno, age并返回

```
select name,workno,age from emp;
```

B. 查询返回所有字段

```
select id ,workno,name,gender,age,idcard,workaddress,entrydate from emp;
```

```
select * from emp;
```

C. 查询所有员工的工作地址,起别名

```
select workaddress as '工作地址' from emp;
```

```
-- as可以省略
select workaddress '工作地址' from emp;
```

D. 查询公司员工的上班地址有哪些(不要重复)

```
select distinct workaddress '工作地址' from emp;
```

### 2.6.3 条件查询

1.语法

```
SELECT 字段列表 FROM 表名 WHERE 条件列表 ;
```

2.条件

常用的比较运算符如下:

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678611075498-6e6c70ae-acb8-4659-b949-ffc8fc494284.png)

常用的逻辑运算符如下:

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678611095545-fbd8cb6a-fedf-479f-8a70-d34d5ae7c4d5.png)

eg：

A. 查询年龄等于 88 的员工

```
select * from emp where age = 88;
```

B. 查询年龄小于 20 的员工信息

```
select * from emp where age < 20;
```

C. 查询年龄小于等于 20 的员工信息

```
select * from emp where age <= 20;
```

D. 查询没有身份证号的员工信息

```
select * from emp where idcard is null;
```

E. 查询有身份证号的员工信息

```
select * from emp where idcard is not null;
```

F. 查询年龄不等于 88 的员工信息

```
select * from emp where age != 88;
select * from emp where age <> 88;
```

G. 查询年龄在15岁(包含) 到 20岁(包含)之间的员工信息

```
select * from emp where age >= 15 && age <= 20;
select * from emp where age >= 15 and age <= 20;
select * from emp where age between 15 and 20;
```

H. 查询性别为 女 且年龄小于 25岁的员工信息

```
select * from emp where gender = '女' and age < 25;
```

I. 查询年龄等于18 或 20 或 40 的员工信息

```
select * from emp where age = 18 or age = 20 or age =40;
select * from emp where age in(18,20,40);
```

J. 查询姓名为两个字的员工信息 _ %

```
select * from emp where name like '__';
```

K. 查询身份证号最后一位是X的员工信息

```
select * from emp where idcard like '%X';
select * from emp where idcard like '_________________X';
```

### 2.6.4 聚合函数

1.介绍

将一列数据作为一个整体，进行纵向计算 。

2.常见的聚合函数

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678611510571-5d6333a7-2a44-4877-a51c-75f871b81af4.png)

3.语法

```
SELECT 聚合函数(字段列表) FROM 表名 ;
```

NULL值是不参与所有聚合函数运算的。

eg：

A. 统计该企业员工数量

```
select count(*) from emp; -- 统计的是总记录数
select count(idcard) from emp; -- 统计的是idcard字段不为null的记录数
```

对于count聚合函数，统计符合条件的总记录数，还可以通过 count(数字/字符串)的形式进行统计查询，比如：

```
select count(1) from emp;
```

对于count(*) 、count(字段)、 count(1) 的具体原理，我们在进阶篇中SQL优化部分会详

细讲解，此处大家只需要知道如何使用即可。

B. 统计该企业员工的平均年龄

```
select avg(age) from emp;
```

C. 统计该企业员工的最大年龄

```
select max(age) from emp;
```

D. 统计该企业员工的最小年龄

```
select min(age) from emp;
```

E. 统计西安地区员工的年龄之和

```
select sum(age) from emp where workaddress = '西安';
```

### 2.6.5 分组查询

1.语法

```
SELECT 字段列表 FROM 表名 [ WHERE 条件 ] GROUP BY 分组字段名 [ HAVING 分组后过滤条件 ];
```

2.where和having的区别

执行时机不同：where是分组之前进行过滤，不满足where条件，不参与分组；而having是分组之后对结果进行过滤。

判断条件不同：where不能对聚合函数进行判断，而having可以。

注意事项：

• 分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义。

• 执行顺序: where > 聚合函数 > having 。

• 支持多字段分组, 具体语法为 : group by columnA,columnB

eg：

A. 根据性别分组 , 统计男性员工 和 女性员工的数量

```
select gender, count(*) from emp group by gender ;
```

B. 根据性别分组 , 统计男性员工 和 女性员工的平均年龄

```
select gender, avg(age) from emp group by gender ;
```

C. 查询年龄小于45的员工 , 并根据工作地址分组 , 获取员工数量大于等于3的工作地址

```
select workaddress, count(*) address_count from emp where age < 45 group by workaddress having address_count >= 3;
```

D. 统计各个工作地址上班的男性及女性员工的数量

```
select workaddress, gender, count(*) '数量' from emp group by gender , workaddress;
```

### 2.6.6 排序查询

排序在日常开发中是非常常见的一个操作，有升序排序，也有降序排序。

1.语法

```
SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1 , 字段2 排序方式2 ;
```

2.排序方式

ASC : 升序(默认值)

DESC: 降序

• 如果是升序, 可以不指定排序方式ASC ;

• 如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序 ;

eg：

A. 根据年龄对公司的员工进行升序排序

```
select * from emp order by age asc;
select * from emp order by age;
```

B. 根据入职时间, 对员工进行降序排序

```
select * from emp order by entrydate desc;
```

C. 根据年龄对公司的员工进行升序排序 , 年龄相同 , 再按照入职时间进行降序排序

```
select * from emp order by age asc , entrydate desc;
```

### 2.6.7 分页查询

分页操作在业务系统开发时，也是非常常见的一个功能，我们在网站中看到的各种各样的分页条，后台都需要借助于数据库的分页操作。

1.语法

```
SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询记录数 ;
```

注意事项：

• 起始索引从0开始，起始索引 = （查询页码 - 1）* 每页显示记录数。

• 分页查询是数据库的方言，不同的数据库有不同的实现，MySQL中是LIMIT。

• 如果查询的是第一页数据，起始索引可以省略，直接简写为 limit 10。

eg：

A. 查询第1页员工数据, 每页展示10条记录

```
select * from emp limit 0,10;
select * from emp limit 10;
```

B. 查询第2页员工数据, 每页展示10条记录 --------> (页码-1)*页展示记录数

```
select * from emp limit 10,10;
```

### 2.6.8 案例

1). 查询年龄为20,21,22,23岁的员工信息。

```
select * from emp where gender = '女' and age in(20,21,22,23);
```

2). 查询性别为 男 ，并且年龄在 20-40 岁(含)以内的姓名为三个字的员工。

```
select * from emp where gender = '男' and ( age between 20 and 40 ) and name like'___';
```

3). 统计员工表中, 年龄小于60岁的 , 男性员工和女性员工的人数。

```
select gender, count(*) from emp where age < 60 group by gender;
```

4). 查询所有年龄小于等于35岁员工的姓名和年龄，并对查询结果按年龄升序排序，如果年龄相同按入职时间降序排序。

```
select name , age from emp where age <= 35 order by age asc , entrydate desc;
```

5). 查询性别为男，且年龄在20-40 岁(含)以内的前5个员工信息，对查询的结果按年龄升序排序，年龄相同按入职时间升序排序。

```
select * from emp where gender = '男' and age between 20 and 40 order by age asc ,entrydate asc limit 5 ;
```

### 2.6.9 执行顺序

在讲解DQL语句的具体语法之前，我们已经讲解了DQL语句的完整语法，及编写顺序，接下来，我们要来说明的是DQL语句在执行时的执行顺序，也就是先执行那一部分，后执行那一部分。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678617398312-4585b047-c703-4abe-a207-342f0ece09a2.png)

验证：

查询年龄大于15的员工姓名、年龄，并根据年龄进行升序排序。

```
select name , age from emp where age > 15 order by age asc;
```

在查询时，我们给emp表起一个别名 e，然后在select 及 where中使用该别名。

```
select e.name , e.age from emp e where e.age > 15 order by age asc;
```

执行上述SQL语句后，我们看到依然可以正常的查询到结果，此时就说明： from 先执行, 然后where 和 select 执行。那 where 和 select 到底哪个先执行呢?

此时，此时我们可以给select后面的字段起别名，然后在 where 中使用这个别名，然后看看是否可以执行成功

执行上述SQL报错了:

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678617589762-208c141b-3e7b-4ba2-b0d5-2beb7cfcbba5.png)

由此我们可以得出结论: from 先执行，然后执行 where ， 再执行select 。

接下来，我们再执行如下SQL语句，查看执行效果：

```
select e.name ename , e.age eage from emp e where e.age > 15 order by eage asc;
```

结果执行成功。 那么也就验证了: order by 是在select 语句之后执行的。

综上所述，我们可以看到DQL语句的执行顺序为： from ... where ... group by ...having ... select ... order by ... limit ...

## 2.7 DCL

DCL英文全称是**Data Control Language**(数据控制语言)，用来管理数据库用户、控制数据库的访问权限。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678617651791-3a001fc3-7a9b-46ec-af3e-710fb76f818f.png)

### 2.7.1 管理用户

1.查询用户

```
select * from mysql.user;
```

查询的结果如下:

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678617700255-48a16155-9cae-44c8-a12d-be5c8a343ae0.png)

其中 Host代表当前用户访问的主机, 如果为localhost, 仅代表只能够在当前本机访问，是不可以远程访问的。 User代表的是访问该数据库的用户名。在MySQL中需要通过Host和User来唯一标识一个用户。

2.创建用户

```
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
```

3.修改用户密码

```
ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码' ;
```

4.删除用户

```
DROP USER '用户名'@'主机名' ;
```

注意事项：

• 在MySQL中需要通过用户名@主机名的方式，来唯一标识一个用户。

• 主机名可以使用 % 通配。

• 这类SQL开发人员操作的比较少，主要是DBA（Database Administrator 数据库管理员）使用。

eg：

A. 创建用户itcast, 只能够在当前主机localhost访问, 密码123456;

```
create user 'itcast'@'localhost' identified by '123456';
```

B. 创建用户heima, 可以在任意主机访问该数据库, 密码123456;

```
create user 'heima'@'%' identified by '123456';
```

C. 修改用户heima的访问密码为1234;

```
alter user 'heima'@'%' identified with mysql_native_password by '1234';
```

D. 删除 itcast@localhost 用户

```
drop user 'itcast'@'localhost';
```

### 2.7.2 权限控制

MySQL中定义了很多种权限，但是常用的就以下几种：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678617914801-f24927a6-458b-473b-bfa7-1c23104f0c19.png)

上述只是简单罗列了常见的几种权限描述，其他权限描述及含义，可以直接参考**官方文档**

1.查询权限

```
SHOW GRANTS FOR '用户名'@'主机名' ;
```

2.授予权限

```
GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';
```

3.撤销权限

```
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
```

注意事项：

• 多个权限之间，使用逗号分隔

• 授权时， 数据库名和表名可以使用 * 进行通配，代表所有。

eg：

A. 查询 'heima'@'%' 用户的权限

```
show grants for 'heima'@'%';
```

B. 授予 'heima'@'%' 用户itcast数据库所有表的所有操作权限

```
grant all on itcast.* to 'heima'@'%';
```

C. 撤销 'heima'@'%' 用户的itcast数据库的所有权限

```
revoke all on itcast.* from 'heima'@'%';
```

# 3.函数

函数 是指一段可以直接被另一段程序调用的程序或代码。 也就意味着，这一段程序或代码在MySQL中已经给我们提供了，我们要做的就是在合适的业务场景调用对应的函数完成对应的业务需求即可。 那么，函数到底在哪儿使用呢？

我们先来看两个场景：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678618089996-03708cd5-3e92-401c-8592-b056b5970f81.png)

1). 在企业的OA或其他的人力系统中，经常会提供的有这样一个功能，每一个员工登录上来之后都能够看到当前员工入职的天数。 而在数据库中，存储的都是入职日期，如 2000-11-12，那如果快速计算出天数呢？

2). 在做报表这类的业务需求中,我们要展示出学员的分数等级分布。而在数据库中，存储的是学生的分数值，如98/75，如何快速判定分数的等级呢？

其实，上述的这一类的需求呢，我们通过MySQL中的函数都可以很方便的实现 。MySQL中的函数主要分为以下四类： 字符串函数、数值函数、日期函数、流程函数。

## 3.1 字符串函数

MySQL中内置了很多字符串函数，常用的几个如下

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678618200371-a5b1a453-8c58-40c1-b690-b695d187d5ae.png)

演示如下：

A. concat : 字符串拼接

```
select concat('Hello' , ' MySQL');
```

B. lower : 全部转小写

```
select lower('Hello');
```

C. upper : 全部转大写

```
select upper('Hello');
```

D. lpad : 左填充

```
select lpad('01', 5, '-');
```

E. rpad : 右填充

```
select rpad('01', 5, '-');
```

F. trim : 去除空格

```
select trim(' Hello MySQL ');
```

G. substring : 截取子字符串

```
select substring('Hello MySQL',1,5);
```

案例：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678618487909-e27216b5-52c8-4d43-86e2-879572771504.png)

由于业务需求变更，企业员工的工号，统一为5位数，目前不足5位数的全部在前面补0。比如： 1号员工的工号应该为00001。

```
update emp set workno = lpad(workno, 5, '0');
```

处理完毕后, 具体的数据为:

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678618511346-651d08c1-7bbd-4693-907c-a2044e8854a3.png)

## 3.2 数值函数

常见的数值函数如下：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678618541878-115b7587-e5df-4766-95dc-eb3d95f7b98a.png)

演示如下：

A. ceil：向上取整

```
select ceil(1.1);
```

B. floor：向下取整

```
select floor(1.9);
```

C. mod：取模

```
select mod(7,4);
```

D. rand：获取随机数

```
select rand();
```

E. round：四舍五入

```
select round(2.344,2);
```

案例：

通过数据库的函数，生成一个六位数的随机验证码。

思路： 获取随机数可以通过rand()函数，但是获取出来的随机数是在0-1之间的，所以可以在其基础上乘以1000000，然后舍弃小数部分，如果长度不足6位，补0

```
select lpad(round(rand()*1000000 , 0), 6, '0');
```

## 3.3 日期函数

常见的日期函数如下：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678618666333-09c091b0-4d8f-4f20-b61b-eeab8e46036f.png)

演示如下：

A. curdate：当前日期

```
select curdate();
```

B. curtime：当前时间

```
select curtime();
```

C. now：当前日期和时间

```
select now();
```

D. YEAR , MONTH , DAY：当前年、月、日

```
select YEAR(now());
select MONTH(now());
select DAY(now());
```

E. date_add：增加指定的时间间隔

```
select date_add(now(), INTERVAL 70 YEAR );
```

F. datediff：获取两个日期相差的天数

```
select datediff('2021-10-01', '2021-12-01');
```

案例：

查询所有员工的入职天数，并根据入职天数倒序排序。

思路： 入职天数，就是通过当前日期 - 入职日期，所以需要使用datediff函数来完成。

```
select name, datediff(curdate(), entrydate) as 'entrydays' from emp order by entrydays desc;
```

## 3.4 流程函数

流程函数也是很常用的一类函数，可以在SQL语句中实现条件筛选，从而提高语句的效率。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678618922666-0ea81654-343f-476a-a5dd-3500496d3bcc.png)

演示如下：

A. if

```
select if(false, 'Ok', 'Error');
```

B. ifnull

```
select ifnull('Ok','Default');
select ifnull('','Default');
select ifnull(null,'Default');
```

C. case when then else end

需求: 查询emp表的员工姓名和工作地址 (北京/上海 ----> 一线城市 , 其他 ----> 二线城市)

```
select
	name,
	( case workaddress when '北京' then '一线城市' when '上海' then '一线城市' else
'二线城市' end ) as '工作地址'
from emp;
```

案例:

```
create table score(
	id int comment 'ID',
	name varchar(20) comment '姓名',
	math int comment '数学',
	english int comment '英语',
	chinese int comment '语文'
) comment '学员成绩表';
insert into score(id, name, math, english, chinese) VALUES (1, 'Tom', 67, 88, 95
), (2, 'Rose' , 23, 66, 90),(3, 'Jack', 56, 98, 76);
```

具体的SQL语句如下:

```
select
	id,
	name,
	(case when math >= 85 then '优秀' when math >=60 then '及格' else '不及格' end )'数学',
	(case when english >= 85 then '优秀' when english >=60 then '及格' else '不及格'end ) '英语',
	(case when chinese >= 85 then '优秀' when chinese >=60 then '及格' else '不及格'end ) '语文'
from score;
```

MySQL的常见函数我们学习完了，那接下来，我们就来分析一下，在前面讲到的两个函数的案例场景，

思考一下需要用到什么样的函数来实现?

1). 数据库中，存储的是入职日期，如 2000-01-01，如何快速计算出入职天数呢？ -------->

答案: datediff

2). 数据库中，存储的是学生的分数值，如98、75，如何快速判定分数的等级呢？ ---------->

答案: case ... when ...

# 4.约束

4.1 概述

概念：约束是作用于表中字段上的规则，用于限制存储在表中的数据。

目的：保证数据库中数据的正确、有效性和完整性。

分类:

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678619245139-ee90c357-1621-4f7a-a78f-a9f0f5975717.png)

注意：约束是作用于表中字段上的，可以在创建表/修改表的时候添加约束。

## 4.2 约束演示

上面我们介绍了数据库中常见的约束，以及约束涉及到的关键字，那这些约束我们到底如何在创建表、修改表的时候来指定呢，接下来我们就通过一个案例，来演示一下。

案例需求： 根据需求，完成表结构的创建。需求如下：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678619294468-c4218c2a-e740-4a94-9e08-fcac7ba67e60.png)

对应的建表语句为：

```
CREATE TABLE tb_user(
	id int AUTO_INCREMENT PRIMARY KEY COMMENT 'ID唯一标识',
	name varchar(10) NOT NULL UNIQUE COMMENT '姓名' ,
	age int check (age > 0 && age <= 120) COMMENT '年龄' ,
	status char(1) default '1' COMMENT '状态',
	gender char(1) COMMENT '性别'
);
```

在为字段添加约束时，我们只需要在字段之后加上约束的关键字即可，需要关注其语法。我们执行上面的SQL把表结构创建完成，然后接下来，就可以通过一组数据进行测试，从而验证一下，约束是否可以生效。

```
insert into tb_user(name,age,status,gender) values ('Tom1',19,'1','男'),('Tom2',25,'0','男');
insert into tb_user(name,age,status,gender) values ('Tom3',19,'1','男');
insert into tb_user(name,age,status,gender) values (null,19,'1','男');
insert into tb_user(name,age,status,gender) values ('Tom3',19,'1','男');
insert into tb_user(name,age,status,gender) values ('Tom4',80,'1','男');
insert into tb_user(name,age,status,gender) values ('Tom5',-1,'1','男');
insert into tb_user(name,age,status,gender) values ('Tom5',121,'1','男');
insert into tb_user(name,age,gender) values ('Tom5',120,'男');
```

上面，我们是通过编写SQL语句的形式来完成约束的指定，那加入我们是通过图形化界面来创建表结构时，又该如何来指定约束呢？ 只需要在创建表的时候，根据我们的需要选择对应的约束即可。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678619378577-1127c8b3-ca26-44ee-87fb-89cb1782ac2c.png)

## 4.3 外键约束

### 4.3.1 介绍

外键：用来让两张表的数据之间建立连接，从而保证数据的一致性和完整性。

我们来看一个例子：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678619440694-2268887e-238e-4e6a-963a-da292ffb72e0.png)

左侧的emp表是员工表，里面存储员工的基本信息，包含员工的ID、姓名、年龄、职位、薪资、入职日期、上级主管ID、部门ID，在员工的信息中存储的是部门的ID dept_id，而这个部门的ID是关联的部门表dept的主键id，那emp表的dept_id就是外键,关联的是另一张表的主键。

注意：目前上述两张表，只是在逻辑上存在这样一层关系；在数据库层面，并未建立外键关联，所以是无法保证数据的一致性和完整性的。

没有数据库外键关联的情况下，能够保证一致性和完整性呢，我们来测试一下。

准备数据

```
create table dept(
	id int auto_increment comment 'ID' primary key,
	name varchar(50) not null comment '部门名称'
)comment '部门表';

INSERT INTO dept (id, name) VALUES (1, '研发部'), (2, '市场部'),(3, '财务部'), (4,'销售部'), (5, '总经办');

create table emp(
	id int auto_increment comment 'ID' primary key,
	name varchar(50) not null comment '姓名',
	age int comment '年龄',
	job varchar(20) comment '职位',
	salary int comment '薪资',
	entrydate date comment '入职时间',
	managerid int comment '直属领导ID',
	dept_id int comment '部门ID'
)comment '员工表';

INSERT INTO emp (id, name, age, job,salary, entrydate, managerid, dept_id)
VALUES
(1, '金庸', 66, '总裁',20000, '2000-01-01', null,5),
(2, '张无忌', 20,'项目经理',12500, '2005-12-05', 1,1),
(3, '杨逍', 33, '开发', 8400,'2000-11-03', 2,1),
(4, '韦一笑', 48, '开发',11000, '2002-02-05', 2,1),
(5, '常遇春', 43, '开发',10500, '2004-09-07', 3,1),(6, '小昭', 19, '程序员鼓励师',6600, '2004-10-12', 2,1);
```

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678619633167-690d85cb-f5ef-49c0-b2fc-1c07a7b56a4b.png)

接下来，我们可以做一个测试，删除id为1的部门信息。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678619654846-c3322d53-9363-470b-82a6-5bbb20bffe36.png)

结果，我们看到删除成功，而删除成功之后，部门表不存在id为1的部门，而在emp表中还有很多的员工，关联的为id为1的部门，此时就出现了数据的不完整性。 而要想解决这个问题就得通过数据库的外键约束。

### 4.3.2 语法

1.添加外键

```
CREATE TABLE 表名(
	字段名 数据类型,
	...
	[CONSTRAINT] [外键名称] FOREIGN KEY (外键字段名) REFERENCES 主表 (主表列名)
);
```

```
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表 (主表列名) ;
```

案例:

为emp表的dept_id字段添加外键约束,关联dept表的主键id。

```
alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references dept(id);
```

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678619773320-2f8f77d7-c286-4aaf-aa4e-9c8a094c3470.png)

添加了外键约束之后，我们再到dept表(父表)删除id为1的记录，然后看一下会发生什么现象。 此时将会报错，不能删除或更新父表记录，因为存在外键约束。

2.删除外键

```
ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;
```

案例：

删除emp表的外键fk_emp_dept_id。

```
alter table emp drop foreign key fk_emp_dept_id;
```

### 4.3.3 删除、更新行为

添加了外键之后，再删除父表数据时产生的约束行为，我们就称为删除/更新行为。具体的删除/更新行为有以下几种:

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678619869395-26fbc19e-4508-4bf0-b720-17bd4f4fa919.png)

具体语法为:

```
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段) REFERENCES 主表名 (主表字段名) ON UPDATE CASCADE ON DELETE CASCADE;
```

演示如下：

由于NO ACTION 是默认行为，我们前面语法演示的时候，已经测试过了，就不再演示了，这里我们再演示其他的两种行为：CASCADE、SET NULL。

1). CASCADE

```
alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references dept(id) on update cascade on delete cascade ;
```

A. 修改父表id为1的记录，将id修改为6

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678620106119-0838d724-0415-47c6-af97-19c8c68af596.png)

我们发现，原来在子表中dept_id值为1的记录，现在也变为6了，这就是cascade级联的效果。

在一般的业务系统中，不会修改一张表的主键值。

B. 删除父表id为6的记录

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678620138573-e0019a1d-44e5-4cd4-87a6-c27400deaff5.png)

我们发现，父表的数据删除成功了，但是子表中关联的记录也被级联删除了。

2). SET NULL

在进行测试之前，我们先需要删除上面建立的外键 fk_emp_dept_id。然后再通过数据脚本，将emp、dept表的数据恢复了。

```
alter table emp add constraint fk_emp_dept_id foreign key (dept_id) reference dept(id) on update set null on delete set null ;
```

接下来，我们删除id为1的数据，看看会发生什么样的现象。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678620196892-a448cd3e-e8fc-41bd-84a0-0875eaa90a38.png)

我们发现父表的记录是可以正常的删除的，父表的数据删除之后，再打开子表 emp，我们发现子表emp的dept_id字段，原来dept_id为1的数据，现在都被置为NULL了。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678620219128-dbd754df-684a-47e5-92d2-c536ba718183.png)

这就是SET NULL这种删除/更新行为的效果。

# 5.多表查询

我们之前在讲解SQL语句的时候，讲解了DQL语句，也就是数据查询语句，但是之前讲解的查询都是单表查询，而本章节我们要学习的则是多表查询操作，主要从以下几个方面进行讲解。

## 5.1 多表关系

项目开发中，在进行数据库表结构设计时，会根据业务需求及业务模块之间的关系，分析并设计表结

构，由于业务之间相互关联，所以各个表结构之间也存在着各种联系，基本上分为三种：

一对多(多对一)

多对多

一对一

### 5.1.1 一对多

案例: 部门 与 员工的关系

关系: 一个部门对应多个员工，一个员工对应一个部门

实现: 在多的一方建立外键，指向一的一方的主键

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678620297375-d33ca4c7-3879-4e86-9e90-bb54ec82fd89.png)

### 5.1.2 多对多

案例: 学生 与 课程的关系

关系: 一个学生可以选修多门课程，一门课程也可以供多个学生选择

实现: 建立第三张中间表，中间表至少包含两个外键，分别关联两方主键

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678620332388-a4a749a5-4ead-428e-813b-4eb134524b00.png)

对应的SQL脚本:

```
create table student(
	id int auto_increment primary key comment '主键ID',
	name varchar(10) comment '姓名',
	no varchar(10) comment '学号'
) comment '学生表';

insert into student values (null, '黛绮丝', '2000100101'),(null, '谢逊','2000100102'),(null, '殷天正', '2000100103'),(null, '韦一笑', '2000100104');

create table course(
	id int auto_increment primary key comment '主键ID',
	name varchar(10) comment '课程名称'
) comment '课程表';

insert into course values (null, 'Java'), (null, 'PHP'), (null , 'MySQL') ,(null, 'Hadoop');

create table student_course(
  id int auto_increment comment '主键' primary key,
	studentid int not null comment '学生ID',
	courseid int not null comment '课程ID',
	constraint fk_courseid foreign key (courseid) references course (id),
	constraint fk_studentid foreign key (studentid) references student (id)
)comment '学生课程中间表';

insert into student_course values (null,1,1),(null,1,2),(null,1,3),(null,2,2),(null,2,3),(null,3,4);
```

### 5.1.3 一对一

案例: 用户 与 用户详情的关系

关系: 一对一关系，多用于单表拆分，将一张表的基础字段放在一张表中，其他详情字段放在另

一张表中，以提升操作效率

实现: 在任意一方加入外键，关联另外一方的主键，并且设置外键为唯一的(UNIQUE)

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678620446232-c499dcb6-42a8-4db5-af28-cd722f3beb54.png)

对应的SQL脚本:

```
create table tb_user(
	id int auto_increment primary key comment '主键ID',
	name varchar(10) comment '姓名',
	age int comment '年龄',
	gender char(1) comment '1: 男 , 2: 女',
	phone char(11) comment '手机号'
) comment '用户基本信息表';

create table tb_user_edu(
	id int auto_increment primary key comment '主键ID',
	degree varchar(20) comment '学历',
	major varchar(50) comment '专业',
	primaryschool varchar(50) comment '小学',
	middleschool varchar(50) comment '中学',
  university varchar(50) comment '大学',
	userid int unique comment '用户ID',
	constraint fk_userid foreign key (userid) references tb_user(id)
) comment '用户教育信息表';

insert into tb_user(id, name, age, gender, phone) values
(null,'黄渤',45,'1','18800001111'),
(null,'冰冰',35,'2','18800002222'),
(null,'码云',55,'1','18800008888'),
(null,'李彦宏',50,'1','18800009999');
insert into tb_user_edu(id, degree, major, primaryschool, middleschool,
university, userid) values
(null,'本科','舞蹈','静安区第一小学','静安区第一中学','北京舞蹈学院',1),
(null,'硕士','表演','朝阳区第一小学','朝阳区第一中学','北京电影学院',2),
(null,'本科','英语','杭州市第一小学','杭州市第一中学','杭州师范大学',3),
(null,'本科','应用数学','阳泉第一小学','阳泉区第一中学','清华大学',4);
```

## 5.2 多表查询概述

### 5.2.1 数据准备

1). 删除之前 emp, dept表的测试数据

2). 执行如下脚本，创建emp表与dept表并插入测试数据

```
-- 创建dept表，并插入数据
create table dept(
	id int auto_increment comment 'ID' primary key,
	name varchar(50) not null comment '部门名称'
)comment '部门表';

INSERT INTO dept (id, name) VALUES (1, '研发部'), (2, '市场部'),(3, '财务部'), (4,'销售部'), (5, '总经办'), (6, '人事部');
-- 创建emp表，并插入数据
create table emp(
	id int auto_increment comment 'ID' primary key,
  name varchar(50) not null comment '姓名',
	age int comment '年龄',
	job varchar(20) comment '职位',
	salary int comment '薪资',
	entrydate date comment '入职时间',
	managerid int comment '直属领导ID',
	dept_id int comment '部门ID'
)comment '员工表';
-- 添加外键
alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references dept(id);
INSERT INTO emp (id, name, age, job,salary, entrydate, managerid, dept_id)
VALUES
(1, '金庸', 66, '总裁',20000, '2000-01-01', null,5),
(2, '张无忌', 20, '项目经理',12500, '2005-12-05', 1,1),
(3, '杨逍', 33, '开发', 8400,'2000-11-03', 2,1),
(4, '韦一笑', 48, '开发',11000, '2002-02-05', 2,1),
(5, '常遇春', 43, '开发',10500, '2004-09-07', 3,1),
(6, '小昭', 19, '程序员鼓励师',6600, '2004-10-12', 2,1),
(7, '灭绝', 60, '财务总监',8500, '2002-09-12', 1,3),
(8, '周芷若', 19, '会计',48000, '2006-06-02', 7,3),
(9, '丁敏君', 23, '出纳',5250, '2009-05-13', 7,3),
(10, '赵敏', 20, '市场部总监',12500, '2004-10-12', 1,2),
(11, '鹿杖客', 56, '职员',3750, '2006-10-03', 10,2),
(12, '鹤笔翁', 19, '职员',3750, '2007-05-09', 10,2),
(13, '方东白', 19, '职员',5500, '2009-02-12', 10,2),
(14, '张三丰', 88, '销售总监',14000, '2004-10-12', 1,4),
(15, '俞莲舟', 38, '销售',4600, '2004-10-12', 14,4),
(16, '宋远桥', 40, '销售',4600, '2004-10-12', 14,4),
(17, '陈友谅', 42, null,2000, '2011-10-12', 1,null);
```

dept表共6条记录，emp表共17条记录。

### 5.2.2 概述

多表查询就是指从多张表中查询数据。

原来查询单表数据，执行的SQL形式为：select * from emp;

那么我们要执行多表查询，就只需要使用逗号分隔多张表即可，如： select * from emp , dept ;

具体的执行结果如下:

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678620670178-0aee83cb-65a3-4642-8523-534be1af17b5.png)

此时,我们看到查询结果中包含了大量的结果集，总共102条记录，而这其实就是员工表emp所有的记录 (17) 与 部门表dept所有记录(6) 的所有组合情况，这种现象称之为笛卡尔积。接下来，就来简单介下笛卡尔积。

笛卡尔积: 笛卡尔乘积是指在数学中，两个集合A集合 和 B集合的所有组合情况。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678620706427-b25c6711-41b9-40ea-9d6b-ccc611c83e0c.png)

而在多表查询中，我们是需要消除无效的笛卡尔积的，只保留两张表关联部分的数据。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678620720809-11abf572-2466-41fb-9991-3c8665c15fd5.png)

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678620734457-62afd8aa-e839-48d4-b671-d77b316a1498.png)

在SQL语句中，如何来去除无效的笛卡尔积呢？ 我们可以给多表查询加上连接查询的条件即可。

```
select * from emp , dept where emp.dept_id = dept.id;
```

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678620763352-6d3a03ba-a1e5-4e1a-b2f7-275ec54afba9.png)

而由于id为17的员工，没有dept_id字段值，所以在多表查询时，根据连接查询的条件并没有查询到。

### 5.2.3 分类

- 连接查询

- 内连接：相当于查询A、B交集部分数据
- 外连接：
- 左外连接：查询左表所有数据，以及两张表交集部分数据
- 右外连接：查询右表所有数据，以及两张表交集部分数据
- 自连接：当前表与自身的连接查询，自连接必须使用表别名

- 子查询

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678620830169-417acad6-cf5d-42e0-9654-5c0a548cd169.png)

## 5.3 内连接

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678620864481-58509dff-647c-4cfc-9da0-531a57f3ee36.png)

内连接查询的是两张表交集部分的数据。(也就是绿色部分的数据)

内连接的语法分为两种: 隐式内连接、显式内连接。先来学习一下具体的语法结构。

1). 隐式内连接

```
SELECT 字段列表 FROM 表1 , 表2 WHERE 条件 ... ;
```

2). 显式内连接

```
SELECT 字段列表 FROM 表1 [ INNER ] JOIN 表2 ON 连接条件 ... ;
```

案例:

A. 查询每一个员工的姓名 , 及关联的部门的名称 (隐式内连接实现)

表结构: emp , dept

连接条件: emp.dept_id = dept.id

```
select emp.name , dept.name from emp , dept where emp.dept_id = dept.id ;
-- 为每一张表起别名,简化SQL编写
select e.name,d.name from emp e , dept d where e.dept_id = d.id;
```

B. 查询每一个员工的姓名 , 及关联的部门的名称 (显式内连接实现) --- INNER JOIN ...ON ...

表结构: emp , dept

连接条件: emp.dept_id = dept.id

```
select e.name, d.name from emp e inner join dept d on e.dept_id = d.id;
-- 为每一张表起别名,简化SQL编写
select e.name, d.name from emp e join dept d on e.dept_id = d.id;
```

表的别名:

①. tablea as 别名1 , tableb as 别名2 ;

②. tablea 别名1 , tableb 别名2 ;

注意事项：

一旦为表起了别名，就不能再使用表名来指定对应的字段了，此时只能够使用别名来指定字段。

## 5.4 外连接

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678621007374-67736792-fc4b-4ac0-9a60-a46c8706e3a3.png)

外连接分为两种，分别是：左外连接 和 右外连接。具体的语法结构为：

左边为 join 语句 左边的表数据，

1). 左外连接

```
SELECT 字段列表 FROM 表1 LEFT [ OUTER ] JOIN 表2 ON 条件 ... ;
```

左外连接相当于查询表1(左表)的所有数据，当然也包含表1和表2交集部分的数据。

2). 右外连接

```
SELECT 字段列表 FROM 表1 RIGHT [ OUTER ] JOIN 表2 ON 条件 ... ;
```

右外连接相当于查询表2(右表)的所有数据，当然也包含表1和表2交集部分的数据。

案例:

A. 查询emp表的所有数据, 和对应的部门信息

由于需求中提到，要查询emp的所有数据，所以是不能内连接查询的，需要考虑使用外连接查询。

表结构: emp, dept

连接条件: emp.dept_id = dept.id

```
select e.*, d.name from emp e left outer join dept d on e.dept_id = d.id;
select e.*, d.name from emp e left join dept d on e.dept_id = d.id;
```

B. 查询dept表的所有数据, 和对应的员工信息(右外连接)

由于需求中提到，要查询dept表的所有数据，所以是不能内连接查询的，需要考虑使用外连接查询。

表结构: emp, dept

连接条件: emp.dept_id = dept.id

```
select d.*, e.* from emp e right outer join dept d on e.dept_id = d.id;
select d.*, e.* from dept d left outer join emp e on e.dept_id = d.id;
```

注意事项：

左外连接和右外连接是可以相互替换的，只需要调整在连接查询时SQL中，表结构的先后顺序就可以了。而我们在日常开发使用时，更偏向于左外连接。

## 5.5 自连接

### 5.5.1 自连接查询

自连接查询，顾名思义，就是自己连接自己，也就是把一张表连接查询多次。我们先来学习一下自连接的查询语法：

```
SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件 ... ;
```

而对于自连接查询，可以是内连接查询，也可以是外连接查询。

案例：

A. 查询员工 及其 所属领导的名字

表结构: emp

```
select a.name , b.name from emp a , emp b where a.managerid = b.id;
```

B. 查询所有员工 emp 及其领导的名字 emp , 如果员工没有领导, 也需要查询出来表结构: emp a , emp b

```
select a.name '员工', b.name '领导' from emp a left join emp b on a.managerid =b.id;
```

注意事项:

在自连接查询中，必须要为表起别名，要不然我们不清楚所指定的条件、返回的字段，到底是哪一张表的字段。

### 5.5.2 联合查询

对于union查询，就是把多次查询的结果合并起来，形成一个新的查询结果集。

```
SELECT 字段列表 FROM 表A ...
UNION [ ALL ]
SELECT 字段列表 FROM 表B ....;
```

对于联合查询的多张表的列数必须保持一致，字段类型也需要保持一致。

union all 会将全部的数据直接合并在一起，union 会对合并之后的数据去重

案例:

A. 将薪资低于 5000 的员工 , 和 年龄大于 50 岁的员工全部查询出来.

当前对于这个需求，我们可以直接使用多条件查询，使用逻辑运算符 or 连接即可。 那这里呢，我们

也可以通过union/union all来联合查询.

```
select * from emp where salary < 5000
union all
select * from emp where age > 50;
```

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678621269702-311a4403-e8e4-43b2-878c-7437ffe08bf8.png)

union all查询出来的结果，仅仅进行简单的合并，并未去重。

```
select * from emp where salary < 5000
union
select * from emp where age > 50;
```

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678621295152-32cdeeab-5718-4478-a5c2-effb10ce1875.png)

union 联合查询，会对查询出来的结果进行去重处理。

**注意：**

如果多条查询语句查询出来的结果，字段数量不一致，在进行union/union all联合查询时，将会报

错。如：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678621315790-648e27fa-a118-4cb9-81be-7befab265d9b.png)

## 5.6 子查询

### 5.6.1 概念

1). 概念

SQL语句中嵌套SELECT语句，称为嵌套查询，又称子查询。

```
SELECT * FROM t1 WHERE column1 = ( SELECT column1 FROM t2 );
```

子查询外部的语句可以是INSERT / UPDATE / DELETE / SELECT 的任何一个。

2). 分类

根据子查询结果不同，分为：

A. 标量子查询（子查询结果为单个值）

B. 列子查询(子查询结果为一列)

C. 行子查询(子查询结果为一行)

D. 表子查询(子查询结果为多行多列)

根据子查询位置，分为：

A. WHERE之后

B. FROM之后

C. SELECT之后

### 5.6.2 标量子查询

子查询返回的结果是单个值（数字、字符串、日期等），最简单的形式，这种子查询称为标量子查询。

常用的操作符：= <> > >= < <=

案例:

A. 查询 "销售部" 的所有员工信息

完成这个需求时，我们可以将需求分解为两步：

①. 查询 "销售部" 部门ID

```
select id from dept where name = '销售部';
```

②. 根据 "销售部" 部门ID, 查询员工信息

```
select * from emp where dept_id = (select id from dept where name = '销售部');
```

B. 查询在 "方东白" 入职之后的员工信息

完成这个需求时，我们可以将需求分解为两步：

①. 查询 方东白 的入职日期

```
select entrydate from emp where name = '方东白';
```

②. 查询指定入职日期之后入职的员工信息

```
select * from emp where entrydate > (select entrydate from emp where name = '方东白');
```

### 5.6.3 列子查询

子查询返回的结果是一列（可以是多行），这种子查询称为列子查询。

常用的操作符：IN 、NOT IN 、 ANY 、SOME 、 ALL

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678621499680-edd4325b-a1e0-49ca-859b-26047f59aced.png)

案例:

A. 查询 "销售部" 和 "市场部" 的所有员工信息

分解为以下两步:

①. 查询 "销售部" 和 "市场部" 的部门ID

```
select id from dept where name = '销售部' or name = '市场部';
```

②. 根据部门ID, 查询员工信息

```
select * from emp where dept_id in (select id from dept where name = '销售部' or name = '市场部');
```

B. 查询比 财务部 所有人工资都高的员工信息

分解为以下两步:

①. 查询所有 财务部 人员工资

```
select id from dept where name = '财务部';
select salary from emp where dept_id = (select id from dept where name = '财务部');
```

②. 比 财务部 所有人工资都高的员工信息

```
select * from emp where salary > all ( select salary from emp where dept_id =(select id from dept where name = '财务部') );
```

C. 查询比研发部其中任意一人工资高的员工信息

分解为以下两步:

①. 查询研发部所有人工资

```
select salary from emp where dept_id = (select id from dept where name = '研发部');
```

②. 比研发部其中任意一人工资高的员工信息

```
select * from emp where salary > any ( select salary from emp where dept_id =(select id from dept where name = '研发部') );
```

### 5.6.4 行子查询

子查询返回的结果是一行（可以是多列），这种子查询称为行子查询。

常用的操作符：= 、<> 、IN 、NOT IN

案例:

A. 查询与 "张无忌" 的薪资及直属领导相同的员工信息 ;

这个需求同样可以拆解为两步进行:

①. 查询 "张无忌" 的薪资及直属领导

```
select salary, managerid from emp where name = '张无忌'; 1
```

②. 查询与 "张无忌" 的薪资及直属领导相同的员工信息 ;

```
select * from emp where (salary,managerid) = (select salary, managerid from emp where name = '张无忌');
```

### 5.6.5 表子查询

子查询返回的结果是多行多列，这种子查询称为表子查询。

常用的操作符：IN

案例:

A. 查询与 "鹿杖客" , "宋远桥" 的职位和薪资相同的员工信息

分解为两步执行:

①. 查询 "鹿杖客" , "宋远桥" 的职位和薪资

```
select job, salary from emp where name = '鹿杖客' or name = '宋远桥';
```

②. 查询与 "鹿杖客" , "宋远桥" 的职位和薪资相同的员工信息

```
select * from emp where (job,salary) in ( select job, salary from emp where name ='鹿杖客' or name = '宋远桥' );
```

B. 查询入职日期是 "2006-01-01" 之后的员工信息 , 及其部门信息

分解为两步执行:

①. 入职日期是 "2006-01-01" 之后的员工信息

```
select * from emp where entrydate > '2006-01-01';
```

②. 查询这部分员工, 对应的部门信息;

```
select e.*, d.* from (select * from emp where entrydate > '2006-01-01') e left join dept d on e.dept_id = d.id ;
```

## 5.7 多表查询案例

数据环境准备:

```
create table salgrade(
	grade int,
	losal int,
	hisal int
) comment '薪资等级表';

insert into salgrade values (1,0,3000);
insert into salgrade values (2,3001,5000);
insert into salgrade values (3,5001,8000);
insert into salgrade values (4,8001,10000);
insert into salgrade values (5,10001,15000);
insert into salgrade values (6,15001,20000);
insert into salgrade values (7,20001,25000);
insert into salgrade values (8,25001,30000);
```

在这个案例中，我们主要运用上面所讲解的多表查询的语法，完成以下的12个需求即可，而这里主要涉

及到的表就三张：emp员工表、dept部门表、salgrade薪资等级表 。

1). 查询员工的姓名、年龄、职位、部门信息 （隐式内连接）

表: emp , dept

连接条件: emp.dept_id = dept.id

```
select e.name , e.age , e.job , d.name from emp e , dept d where e.dept_id = d.id;
```

2). 查询年龄小于30岁的员工的姓名、年龄、职位、部门信息（显式内连接）

表: emp , dept

连接条件: emp.dept_id = dept.id

```
select e.name , e.age , e.job , d.name from emp e inner join dept d on e.dept_id = d.id where e.age < 30;
```

3). 查询拥有员工的部门ID、部门名称

表: emp , dept

连接条件: emp.dept_id = dept.id

```
select distinct d.id , d.name from emp e , dept d where e.dept_id = d.id;
```

4). 查询所有年龄大于40岁的员工, 及其归属的部门名称; 如果员工没有分配部门, 也需要展示出

来(外连接)

表: emp , dept

连接条件: emp.dept_id = dept.id

```
select e.*, d.name from emp e left join dept d on e.dept_id = d.id where e.age > 40 ;
```

5). 查询所有员工的工资等级

表: emp , salgrade

连接条件 : emp.salary >= salgrade.losal and emp.salary <= salgrade.hisal

```
-- 方式一
select e.* , s.grade , s.losal, s.hisal from emp e , salgrade s where e.salary >= s.losal and e.salary <= s.hisal;
-- 方式二
select e.* , s.grade , s.losal, s.hisal from emp e , salgrade s where e.salary between s.losal and s.hisal;
```

6). 查询 "研发部" 所有员工的信息及 工资等级

表: emp , salgrade , dept

连接条件 : emp.salary between salgrade.losal and salgrade.hisal ,

emp.dept_id = dept.id

查询条件 : dept.name = '研发部'

```
select e.* , s.grade from emp e , dept d , salgrade s where e.dept_id = d.id and (
e.salary between s.losal and s.hisal ) and d.name = '研发部';
```

7). 查询 "研发部" 员工的平均工资

表: emp , dept

连接条件 : emp.dept_id = dept.id

```
select avg(e.salary) from emp e, dept d where e.dept_id = d.id and d.name = '研发
部';
```

8). 查询工资比 "灭绝" 高的员工信息。

①. 查询 "灭绝" 的薪资

```
select salary from emp where name = '灭绝';
```

②. 查询比她工资高的员工数据

```
select * from emp where salary > ( select salary from emp where name = '灭绝' );
```

9). 查询比平均薪资高的员工信息

①. 查询员工的平均薪资

```
select avg(salary) from emp;
```

②. 查询比平均薪资高的员工信息

```
select * from emp where salary > ( select avg(salary) from emp );
```

10). 查询低于本部门平均工资的员工信息

①. 查询指定部门平均薪资

```
select avg(e1.salary) from emp e1 where e1.dept_id = 1;
select avg(e1.salary) from emp e1 where e1.dept_id = 2;
```

②. 查询低于本部门平均工资的员工信息

```
select * from emp e2 where e2.salary < ( select avg(e1.salary) from emp e1 where
e1.dept_id = e2.dept_id );
```

11). 查询所有的部门信息, 并统计部门的员工人数

```
select d.id, d.name , ( select count(*) from emp e where e.dept_id = d.id ) '人数'
from dept d;
```

12). 查询所有学生的选课情况, 展示出学生名称, 学号, 课程名称

表: student , course , student_course

连接条件: student.id = student_course.studentid , course.id =

student_course.courseid

```
select s.name , s.no , c.name from student s , student_course sc , course c where
s.id = sc.studentid and sc.courseid = c.id ;
```

# 6.事务

## 6.1 事务简述

**事务是一组操作的集合，不可分割，事务中的操作要么全部成功，要么全部失败**。

举个栗子：

经典的转账场景，张三向李四转账1000元，整个的操作逻辑是：①查询张三账户余额，②张三余额减1000，③李四余额加1000。这三步是通过三条SQL来执行的，假如在执行第三步时发生异常，就会导致1000元不翼而飞。

为了解决这一隐患，于是将三步操作对应的三条SQL加入同一个事务，只有SQL全部执行成功，事务才会提交（commit），否则将回滚事务（rollback），撤销操作。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668880744626-9f3570fc-820d-4919-9b2b-9298809ef6a7.png)

📢 注意：MySQL的事务默认自动提交（隐式）

## 6.2 事务操作

因为MySQL事务自动提交，每条SQL都是单独的事务，但是如果是多条SQL，就需要手动开启事务，提交事务。

**查看事务的提交方式：**

```
SELECT @@autocommit ;
```

（1 为自动提交；0 为手动提交）

**设置事务的提交方式：**

```
SET @@autocommit = 0 ;
```

**提交事务：**

```
COMMIT;
```

**回滚事务：**

```
ROLLBACK;
```

**开启事务：**

```
START TRANSACTION 或 BEGIN ;
```

不管是手动提交还是自动提交，都可以利用 BEGIN......COMMIT 将多条DML合成一个事务。

**相关阅读：**

[https://www.jianshu.com/p/64385ac0febb](https://www.jianshu.com/p/64385ac0febb)

## 6.3 事务的四大特性

1. 原子性（Atomicity）：事务是不可分割的最小操作单元，要么全部成功，要么全部失败
2. 一致性（Consistency）：事务执行前后，数据保持一致
3. 隔离性（Isolation）：保证事务不受外部并发操作的影响
4. 持久性（Durability）：事务一旦提交或回滚，它对数据库中数据的改变将是永久的

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668880908445-2a8d64c2-b9de-4b03-ae5f-955e796f9799.png)

📢 **注意：只有保证了事务的原子性、隔离性、持久性之后，一致性才能得到保障，也就是说 A、I、D 是手段，C 是目的。**

## 6.4 并发事务问题

因为事务四大特性之一的隔离性的隔离级别不同，并发事务会造成如下问题：

- 脏读（Dirty read）

一个事务对数据进行了修改，这个修改对其他事物来说是可见的，即使这个事务没有提交。这时另一个事务读取到了这个事务还没有提交的 数据， 但是前一个事务突然回滚，导致数据并没有提交到数据库，后一个事务读取到的数据就是脏数据。

- 不可重复读（Unrepeatable read）

一个事务内多次读取同一数据，但由于在读取间隙，另一个事务修改了该数据，导致读取到的数据不一致。

- 幻读（Phantom read）

一个事务内多次对数据进行查询、统计操作，但由于在操作间隙，另一个事务新增了记录，导致在前一个事务看来，多了一些原本不存在的 记录，如同幻觉。

- 修改丢失（Lost to modify）：

两个事务在提交之前，对同一数据进行了修改，导致前一个事务的修改结果丢失。

**不可重复读着重数据的修改、记录的减少；幻读着重记录的新增。**

## 6.5 事务的隔离级别

通过设置不同的事务隔离级别，解决不同的事务并发问题。

- READ-UNCOMMITTED(读取未提交)：最低的隔离级别，允许读取事务尚未提交的数据
- READ-COMMITTED(读取已提交)：允许读取事务已经已经提交的数据
- REPEATABLE-READ(可重复读) ：对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改
- SERIALIZABLE(可串行化)：最高的隔离级别，完全服从 ACID 的隔离级别，所有的事务依次逐个执行

|   |   |   |   |
|---|---|---|---|
|**隔离级别**|**脏读**|**不可重复读**|**幻读**|
|READ-UNCOMMITTED|√|√|√|
|READ-COMMITTED|×|√|√|
|REPEATABLE-READ|×|×|√|
|SERIALIZABLE|×|×|×|

事务的隔离级别是基于锁以及MVCC机制共同实现的，后面事务原理一章有详细解释。

**查看事务的隔离级别：**

```
SELECT @@TRANSACTION_ISOLATION;
```

**设置事务的隔离级别：**

```
SET [ SESSION | GLOBAL ] TRANSACTION ISOLATION LEVEL { READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE }
```

📢 注意：隔离级别越高，数据越安全，但是性能越低，MySQL 的默认隔离级别为 REPEATABLE-READ（可重复读）。

# 存储引擎
---
## 存储引擎概述
---
- MySQL 的存储引擎是其数据库核心组件之一，负责数据的**存储**、**检索**和**更新**等底层操作。
- MySQL 存储引擎使用**插件式架构**，不同的存储引擎针对不同场景优化，提供了灵活性和性能选择。
- 常用的存储引擎包括 InnoDB、MyISAM、Memory、CSV、Archive 等。
- 自 MySQL 5.5 之后 InnoDB 称为默认的存储引擎。

|特性|InnoDB|MyISAM|Memory|CSV|Archive|
|---|---|---|---|---|---|
|**事务支持**|是|否|否|否|否|
|**锁粒度**|行级锁|表级锁|表级锁|表级锁|表级锁|
|**外键支持**|是|否|否|否|否|
|**全文索引**|是（5.6+）|是|否|否|否|
|**崩溃恢复**|支持|不支持|不支持|不支持|不支持|
|**数据持久化**|是|是|否|是|是|
|**存储空间**|较大|较小|内存|较小|最小|

```sql
-- 查看 MySQL 支持的存储引擎
SHOW ENGINES;
-- 查看指定表的详细信息，包括存储引擎
SHOW TABLE STATUS WHERE Name = 'table_name';
-- 为表指定存储引擎
CREATE TABLE my_table (...) ENGINE=InnoDB;
-- 修改已有表的存储引擎
ALTER TABLE my_table ENGINE=MyISAM;
```
## MySQL体系结构

![[MySQL高级.png]]

- **连接层（Connection Layer）**
	- 负责用户连接管理、权限验证和安全控制。
	- 包括连接池（Connection Pool），用于管理客户端连接，支持多用户并发访问。
	- 提供身份认证（如用户名、密码验证）、权限检查（读写权限等）以及SSL/TLS加密等功能。
	- 客户端通过 TCP/IP、Unix Socket 或命名管道等方式与 MySQL 建立连接。
- **服务层（Service Layer）**
	- 核心功能层，处理 SQL 语句的解析、优化和执行。
	- 主要组件包括：
	    - **SQL接口**：接收客户端发送的 SQL 语句，支持 DML（如 `SELECT`、`INSERT`）、DDL（`如CREATE`、`ALTER`）等。
	    - **解析器（Parser）**：对 SQL 语句进行词法和语法分析，生成解析树。
	    - **查询优化器（Optimizer）**：根据统计信息和规则，生成最优执行计划（如选择索引、决定表连接顺序）。
	    - **查询执行引擎**：根据执行计划调用存储引擎 API，完成查询操作。
	    - **缓存与缓冲区**：如查询缓存（MySQL 8.0 已移除）或表缓存，用于提高查询效率。
	- 提供视图、触发器、存储过程、事务管理等高级功能。
- **存储引擎层（Storage Engine Layer）**
	- 负责数据的存储和读取，MySQL支持多种存储引擎（如 InnoDB、MyISAM、Memory），用户可根据需求选择。
	- 特点：
	    - **可插拔架构**：不同表可使用不同存储引擎，灵活性高。
	    - **InnoDB**：默认引擎，支持事务、行级锁、外键，适合高并发和事务密集型应用。
	    - **MyISAM**：不支持事务，表级锁，适合读多写少的场景。
	    - **Memory**：数据存储在内存中，速度快但断电丢失，适合临时表。
	- 存储引擎通过统一 API 与服务层交互，屏蔽底层实现差异。
- **物理存储层（Physical Storage Layer）**
	- 管理实际的磁盘文件，包括数据文件、日志文件（如二进制日志、错误日志、慢查询日志）和配置文件。
	- 数据以表空间、索引文件等形式存储在文件系统中。
	- 涉及操作系统的文件 I/O 操作，性能受硬件和文件系统影响。
## InnoDB逻辑存储
---
![[MySQL高级-1.png]]

- **表空间（Tablespace）**
	- **定义**：表空间是 InnoDB 存储的最高级别逻辑单位，用于存储所有数据和索引。每个表空间由一个或多个物理文件组成。
	- **类型**：
	    - **系统表空间（System Tablespace）**：存储 InnoDB 的元数据（如数据字典）、双写缓冲区（Doublewrite Buffer）、更改缓冲区（Change Buffer）等。通常由 ibdataX 文件表示。
	    - **独立表空间（File-per-table Tablespace）**：每个表（包括其索引）独占一个表空间，文件以 `.ibd` 结尾。默认开启（`innodb_file_per_table=ON`）。
	    - **通用表空间（General Tablespace）**：用户自定义的多表共享表空间。
	    - **临时表空间（Temporary Tablespace）**：存储临时表数据。
	- **特点**：
	    - 表空间是逻辑概念，实际数据存储在磁盘上的物理文件中。
	    - 独立表空间便于管理单个表的数据和索引，支持表空间迁移。
- **段（Segment）**
	- **定义**：段是表空间内的逻辑单元，用于管理特定类型的对象（如数据、索引或回滚段）。
	- **类型**：
	    - **数据段**：存储表的数据（B+ 树叶子节点）。
	    - **索引段**：存储索引数据（B+ 树非叶子节点）。
	    - **回滚段**：存储事务的撤销日志（Undo Log）。
	- **特点**：
	    - 每个索引（包括主键和二级索引）对应一个段。
	    - 段不直接存储数据，而是通过区（Extent）来组织。
- **区（Extent）**
	- **定义**：区是段内的连续页面集合，默认大小为 **1MB**（包含 64 个页面，每个页面 16KB）。
	- **作用**：
	    - 提高大块数据分配效率，减少碎片。
	    - 区是 InnoDB 分配磁盘空间的基本单位。
	- **特点**：
	    - 新建表时，InnoDB 按区分配空间，而不是单个页面。
	    - 区内的页面可能属于不同的对象（如数据页、索引页）。
- **页面（Page）**
	- **定义**：页面是 InnoDB 存储的最小逻辑单位，默认大小为 **16KB**。
	- **类型**：
	    - **数据页（Data Page）**：存储实际的表行数据。
	    - **索引页（Index Page）**：存储 B+ 树索引结构。
	    - **Undo 页**：存储撤销日志。
	    - **系统页**：存储表空间头部信息等。
	    - **BLOB 页**：存储大对象数据（如 `TEXT`、`BLOB`）。
	- **结构**：
	    - 页面包含头部（Page Header）、页面目录（Page Directory）、数据行（Rows）以及校验信息等。
	    - 页面目录用于快速定位页面内的行记录。
	- **特点**：
	    - 页面是 InnoDB 读写磁盘的基本单位。
	    - 页面内存储的行记录按主键顺序排列，便于范围查询。
- **行（Row）**
	- **定义**：行是实际存储用户数据的单元，位于数据页内。
	- **存储格式**：
	    - InnoDB 支持多种行格式：`REDUNDANT`、`COMPACT`（默认）、`DYNAMIC` 和 `COMPRESSED`。
	    - **COMPACT 格式**：节省空间，包含行头（记录元数据）、变长字段长度列表、非 `NULL` 字段值等。
	    - **DYNAMIC 格式**：适合存储大对象，`BLOB/TEXT` 数据可溢出到单独的页面。
	- **特点**：
	    - 每行记录包含隐藏字段（如行 ID、事务 ID、回滚指针）以支持事务和 MVCC（多版本并发控制）。
	    - 行记录按主键或聚簇索引顺序存储在数据页中。
# 索引
---
## 索引简述
---
### 索引的定义
---
索引是存储引擎用于**快速定位和访问数据库表中数据的特殊数据结构**，类似于书的目录。通过索引，MySQL 可以避免全表扫描，显著提高查询效率。

**演示：**
![[MySQL高级-2.png]]
对于这张表需要执行语句 `select * from user where age = 45;`
在没有索引的情况下，需要全表扫描，效率不高。
现在对表中的 age 列建立索引：
![[MySQL高级-3.png]]
建立索引后只需要扫描三次就可以找到目标行。
### 索引的作用
---
- **提高查询速度**：通过索引，MySQL 可以快速定位到符合条件的行，减少扫描的数据量。
- **优化排序和分组**：索引可以加速 `ORDER BY` 和 `GROUP BY` 操作。
- **支持唯一性约束**：唯一索引确保列或列组合的值不重复。
- **加速连接查询**：在多表连接时，索引可以减少匹配时间。
### 索引类型
---
MySQL 支持多种索引类型，常见的有：
- **主键索引（Primary Key）**：唯一标识表中每一行，自动创建且不能包含 `NULL` 值。
- **唯一索引（Unique Key）**：确保索引列或列组合的值唯一，但允许 `NULL` 值。
- **普通索引（Index/Key）**：最基本的索引类型，用于加速查询，无唯一性约束。
- **全文索引（Full-Text Index）**：用于全文搜索，适用于文本字段（如 `VARCHAR` 或 `TEXT`）。
- **空间索引（Spatial Index）**：用于地理数据类型（如 `GEOMETRY`）。
- **前缀索引**：对长字符串列（如 `VARCHAR`）的前 N 个字符创建索引，节省存储空间。
- **复合索引（Multi-Column Index）**：基于多个列创建的索引，适合多条件查询。
### 索引的存储结构
---
MySQL 常用存储引擎（如 InnoDB 和 MyISAM）主要使用以下数据结构：
- **B+树索引**：最常见的索引结构，适用于范围查询、精确查找和排序。InnoDB 的默认索引类型。
- **哈希索引**：适用于精确查找，但不支持范围查询或排序，MyISAM 支持，InnoDB 不支持（有自适应哈希索引）。
- **R树索引**：用于空间数据类型的索引。
- **倒排索引**：用于全文索引，基于词条映射。
### 索引的创建于管理
---
#### 创建普通索引
---
- 普通索引用于加速查询，适用于频繁出现在 `WHERE`、`JOIN` 或 `ORDER BY` 中的列。
- 语法：
	```sql
	CREATE INDEX index_name ON table_name (column_name);	
	```
- 示例：
	```sql
	CREATE INDEX idx_name ON employees (last_name);
	```
	- 创建一个名为`idx_name`的索引，基于`employees`表的`last_name`列。
	- 适合查询如：`SELECT * FROM employees WHERE last_name = 'Smith';`
#### 创建唯一索引
---
- 唯一索引确保索引列或列组合的值不重复（允许`NULL`值）。
- 语法：
	```sql
	CREATE UNIQUE INDEX index_name ON table_name (column_name);
	```
- 示例：
	```sql
	CREATE UNIQUE INDEX idx_email ON users (email);
	```
	- 在`users`表的`email`列上创建唯一索引，防止重复邮箱。
	- 适合需要唯一性约束的场景，如用户名或邮箱。
#### 创建复合索引（多列索引）
---
- 复合索引基于多个列，适合多条件查询或排序。
- 语法：
	```sql
	CREATE INDEX index_name ON table_name (column1, column2, ...);
	```
- 示例：
	```sql
	CREATE INDEX idx_name_dept ON employees (last_name, department_id);
	```
	- 为`last_name`和`department_id`创建复合索引。
	- 适合查询如：`SELECT * FROM employees WHERE last_name = 'Smith' AND department_id = 10;`
	- 注意：列的顺序影响索引效率，常用条件应放在前面。
#### 创建主键索引
---
- 主键索引是特殊的唯一索引，不允许`NULL`值，通常在建表时定义。
- 语法：
	- 建表时：
		```sql
		CREATE TABLE table_name (
		    column_name datatype PRIMARY KEY,
		    ...
		);
		```
	- 添加已有表：
		```sql
		ALTER TABLE table_name ADD PRIMARY KEY (column_name);
		```
- 示例：
	```sql
	ALTER TABLE employees ADD PRIMARY KEY (employee_id);
	```
	- 为`employees`表的`employee_id`列添加主键索引。
	- InnoDB 会自动创建聚簇索引。
#### 创建全文索引
---
- 全文索引用于文本搜索，适用于`VARCHAR`或`TEXT`列，支持`MATCH...AGAINST`查询。
- 语法：
	```sql
	CREATE FULLTEXT INDEX index_name ON table_name (column_name);
	```
- 示例：
	```sql
	CREATE FULLTEXT INDEX idx_content ON articles (content);
	```
	- 在`articles`表的`content`列上创建全文索引。
	- 适合查询如：`SELECT * FROM articles WHERE MATCH(content) AGAINST('keyword');`
	- 注意：MyISAM 和 InnoDB（5.6+）支持，需确保存储引擎兼容。
#### 创建前缀索引
---
- 前缀索引用于长字符串列（如`VARCHAR`），只索引列值的前`N`个字符，节省空间。
- 语法：
	```sql
	CREATE INDEX index_name ON table_name (column_name(length));
	```
- 示例：
	```sql
	CREATE INDEX idx_title ON products (product_name(10));
	```
	- 为`product_name`列的前10个字符创建索引。
	- 适合长字符串列，需选择合适的前缀长度以保证选择性。
#### 创建空间索引
---
- 空间索引用于地理数据类型（如`GEOMETRY`），适用于空间查询。
- 语法：
	```sql
	CREATE SPATIAL INDEX index_name ON table_name (column_name);
	```
- 示例：
	```sql
	CREATE SPATIAL INDEX idx_location ON places (coordinates);
	```
	- 为`places`表的`coordinates`列（空间类型）创建空间索引。
	- 注意：需确保列是空间数据类型，MyISAM 和 InnoDB支持。
#### 通过ALTER TABLE创建索引
---
- 除了`CREATE INDEX`，还可以通过`ALTER TABLE`添加索引，功能类似。
- 语法：
	```sql
	ALTER TABLE table_name ADD INDEX index_name (column_name);
	ALTER TABLE table_name ADD UNIQUE INDEX index_name (column_name);
	ALTER TABLE table_name ADD FULLTEXT INDEX index_name (column_name);
	ALTER TABLE table_name ADD SPATIAL INDEX index_name (column_name);
	```
- 示例：
	```sql
	ALTER TABLE employees ADD INDEX idx_salary (salary);
	```
	- 为`employees`表的`salary`列添加普通索引。
#### 在建表时定义索引
---
- 可以在创建表时直接定义索引，适用于新表设计。
- 语法：
	```sql
	CREATE TABLE table_name (
	    column_name datatype,
	    [INDEX | KEY] index_name (column_name),
	    [UNIQUE INDEX | UNIQUE KEY] index_name (column_name),
	    [FULLTEXT INDEX | FULLTEXT KEY] index_name (column_name),
	    [SPATIAL INDEX | SPATIAL KEY] index_name (column_name),
	    ...
	);
	```
- 示例：
	```sql
	CREATE TABLE customers (
	    id INT PRIMARY KEY,
	    name VARCHAR(50),
	    email VARCHAR(100),
	    INDEX idx_name (name),
	    UNIQUE INDEX idx_email (email)
	);
	```
	- 定义主键索引（`id`）和普通索引（`name`）、唯一索引（`email`）。
#### 注意事项
---
- **索引命名**：索引名通常以`idx_`开头，清晰描述用途（如`idx_name_email`）。
- **存储引擎**：
    - InnoDB 支持聚簇索引、唯一索引、普通索引、全文索引（5.6+）、空间索引。
    - MyISAM 支持普通索引、唯一索引、全文索引、空间索引。
    - Memory 支持哈希索引和 B+树索引。
- **选择性**：优先为高选择性列（值分布广泛）创建索引，避免低选择性列（如性别）。
- **性能权衡**：
    - 索引加速查询，但插入、更新、删除会增加维护开销。
    - 过多索引可能导致优化器选择不当，降低性能。
- **检查索引**：使用`SHOW INDEX FROM table_name;`查看表上的索引。
- **分析查询**：通过`EXPLAIN SELECT ...`检查索引是否被使用。
#### 优化建议
---
- **选择合适的列**：为常用于过滤、排序或连接的列创建索引。
- **复合索引顺序**：将最常用的条件列放在索引定义的前面。
- **覆盖索引**：设计索引使查询只需访问索引，无需回表。
- **避免冗余**：检查是否已有类似索引，避免重复创建。
- **监控性能**：定期使用`ANALYZE TABLE`更新表统计信息，确保优化器选择最优索引。
#### 索引管理
---
- 删除索引
	```sql
	DROP INDEX index_name ON table_name;
	```
- 查看索引
	```sql
	SHOW INDEX FROM table_name;
	```
### 索引的优缺点
---
- **优点**：
    - 显著提升查询性能，尤其是对 WHERE、JOIN、ORDER BY 等操作。
    - 减少数据扫描量，降低 I/O 开销。
- **缺点**：
    - **存储空间**：索引占用额外磁盘空间。
    - **写性能下降**：插入、更新、删除操作需要同步更新索引，增加开销。
    - **维护成本**：索引过多可能导致优化器选择不当，降低查询效率。
### 索引的使用场景
---
- 适合场景：
    - 频繁查询的列（如 WHERE 条件、JOIN 字段）。
    - 排序或分组操作的列。
    - 唯一性约束的列。
- 不适合场景：
    - 数据量少的表（全表扫描可能更快）。
    - 频繁更新的列（维护索引开销大）。
    - 低选择性列（如布尔值或性别字段，重复值多）。
### 索引的优化建议
---
- **选择合适的列**：优先为高选择性（区分度高）的列创建索引。
- **使用复合索引**：针对多条件查询，合理设计列顺序（常用条件放前面）。
- **覆盖索引**：设计索引使查询只需访问索引而无需回表。
- **定期维护**：使用 ANALYZE TABLE 更新统计信息，优化查询计划。
- **避免冗余索引**：删除重复或不必要的索引，降低维护成本。
- **监控查询性能**：通过 EXPLAIN 分析查询计划，检查索引是否被有效使用。
### 注意事项
---
- **索引不是越多越好**：过多索引会增加存储和维护开销，可能导致优化器选择错误。
- **存储引擎差异**：InnoDB 和 MyISAM 的索引实现不同，需根据引擎特性优化。
- **查询优化器**：MySQL优化器可能不总是选择最优索引，可通过 FORCE INDEX或USE INDEX干预。
## 索引的存储结构
---


















### 2.2.3 B-Tree

B-Tree、B树是一种多叉路平衡查找树，相比于二叉树，B-Tree的每个节点都可以有多个分支。

以最大度数为5（5阶）的BTree为例，每个节点可以存储4个key、5个指针：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667630218796-0536f830-1f77-4459-8527-a8c1d5bcaa1c.png)

可以在以下这个网站中在线插入数据，观察数据结构的变化：

[https://www.cs.usfca.edu/~galles/visualization/BTree.html](https://www.cs.usfca.edu/~galles/visualization/BTree.html)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667630654728-74fe9571-1676-4f32-aa63-03d0cb26741e.png)

Max.Degree（最大度数）：每个节点的子节点个数

可以总结B-Tree具有以下特点：

- n阶的B树，具有 n-1 个key，n个指针
- n阶的B树，当存储的key达到Max.Degree时，会产生裂变，中间元素向上分裂，当n为偶数时，分裂后，右子树节点比左子树节点多一
- 在B树中，叶子结点和非叶子节点都会存储数据

### 2.2.4 B+Tree

B+tree是B树的变种，以最大度数（Max.Degree）为4（4阶）的B+Tree为例，结构：

![|700x206](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667631482101-654aa83c-1aff-49e4-803d-de78ff2890a1.png)

可以看到叶子结点和非叶子结点的数据是重复的，其实非叶子结点是不存储数据的，只是起到索引的作用。

可以在以下这个网站中在线插入数据，观察数据结构的变化：

[https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html](https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667631753589-17c9cd57-e42d-4db6-90ef-57ab2b8c6b4b.png)

对比B-Tree，B+Tree具有以下特点：

- 所有数据存储在叶子节点，非叶子节点起索引作用
- 叶子结点形成了一个单向链表
- n阶B+Tree树，n为奇数，分列时，中间数据为父节点，右子树节点数比左子树大一；n为偶数时，对半分，右子树第一个节点为父节点

MySQL索引使用的B+Tree对传统的B+Tree进行了优化，将叶子结点的单向链表变为双向循环链表，提高区间访问性能，利于排序。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667632467575-bfd93926-7965-47f6-a7fe-a90c0eb66b85.png)

### 2.2.5 Hash

**1）结构**

Hash索引就是将键值根据特定的Hash算法，计算出Hash值，然后将Hash值与数据长度进行某种计算，将Hash值映射到数组中，如此，就可以将数据存储到哈希表中。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667632910225-961d0a4b-e83d-41fb-8cc2-61493d1bc1fb.png)

既然使用Hash，肯定会存在哈希冲突（哈希碰撞），这时可以使用链表，采用拉链法解决哈希冲突。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667633015846-32f66b1f-0136-4124-b0ee-aabc5fd8f807.png)

**2）特点**

- 哈希索引只能用于对等比较（=、in），不支持范围查询（between、<、>）
- 无法利用哈希索引完成排序
- 查询效率高

**3）存储引擎支持**

在MySQL中支持哈希索引的是Memory。
InnoDB存储引擎虽然不支持哈希索引，但具有自适应哈希索引，哈希索引是InnoDB根据B+Tree在指定条件下自动生成的。

自适应哈希索引：使用B+Tree索引，查找数据需要查找根节点、多个非叶子节点之后，才能找到目标叶子结点，而现在一些叶子结点的数据经常被查找，于是InnoDB存储引擎通过监控表上索引页的查找模式，自动根据查找模式对“热点数据”创建哈希索引。如此，下一次查找这些“热点数据”就可以直接通过哈希索引来查找，哈希索引的查询效率是要比B+Tree高的。

## 2.3 索引分类

### 2.3.1 索引分类

根据索引使用的场景与位置不同，分为以下几类：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667634674208-d1478568-77ef-4fc1-87aa-ccd0c559ef12.png)

### 2.3.2 聚焦索引&二级索引

在InnoDB存储引擎中，根据索引的存储形式，又可以分为以下两种：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667634773774-ee5eb250-a32b-42c4-b655-e0873299bd29.png)

聚焦索引选取规则：

- 如果存在主键，主键索引就是聚焦索引
- 如果不存在主键，将使用第一个唯一索引（UNIQUE）作为聚焦索引
- 如果上述两种都没有，InnoDB将自动生成一个rowid作为隐藏的聚焦索引

聚焦索引和二级索引结构：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667635378385-7da81903-c20a-4dfc-9044-eb3754b3d012.png)

- 聚焦索引非叶子结点存储聚焦索引值，叶子结点挂这一行数据
- 二级索引非叶子节点存储特定字段值，叶子结点挂该字段对应的聚焦索引值（一般是主键值，遵循聚焦索引选取规则）

分析执行以下SQL，具体的查找过程：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667635702117-afe9a8ac-4aae-4a49-985d-e4550f508845.png)

因为查找的是 * ，也就是这一行的数据，需要先根据name字段值，在二级索引中找到对应的主键值，再去聚焦索引中根据主键值，查找行数据。这种查找方式也称为回表查询。

### 2.3.3 B+Tree高度与数据量的计算

以高度为2为例：

单个叶子节点的记录数：16K/1K = 16

说明：16K是InnoDB页的大小；1K表示一行数据大小

根节点的一页中指针数量：16384 / 14 = 1170

说明：指针数量是指叶子结点的个数，因为只有两层，非叶子节点就是根节点；16384（16K）InnoDB一页的大小；14表示聚焦索引值大小，以BigInt为例（8字节）加指针6个字节（如果是B-Tree，需要将数据所占字节数加上）

总数据量：1170 * 16 = 18720条

以高度为3为例：

1170 * 1170 * 16 = 21902400条

说明：根节点指针数 * 二级非叶子节点指针数 * 数据条数

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667701314458-48f292d7-43b6-4bfc-8477-81c6e012cfbb.png)

## 2.4 索引语法

**1）创建索引**

```
CREATE [ UNIQUE | FULLTEXT ] INDEX index_name ON table_name (
index_col_name,... ) ;
```

**2）查看索引**

```
SHOW INDEX FROM table_name ;
```

**3）删除索引**

```
DROP INDEX index_name ON table_name ;
```

演示：

创建tb_user表，以作测试使用：

```sql
create table tb_user(
	id int primary key auto_increment comment '主键',
	name varchar(50) not null comment '用户名',
	phone varchar(11) not null comment '手机号',
	email varchar(100) comment '邮箱',
	profession varchar(11) comment '专业',
	age tinyint unsigned comment '年龄',
	gender char(1) comment '性别 , 1: 男, 2: 女',
	status char(1) comment '状态',
	createtime datetime comment '创建时间'
) comment '系统用户表';
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('吕布', '17799990000', 'lvbu666@163.com', '软件工程', 23, '1','6', '2001-02-02 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('曹操', '17799990001', 'caocao666@qq.com', '通讯工程', 33,'1', '0', '2001-03-05 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('赵云', '17799990002', '17799990@139.com', '英语', 34, '1','2', '2002-03-02 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('孙悟空', '17799990003', '17799990@sina.com', '工程造价', 54,'1', '0', '2001-07-02 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('花木兰', '17799990004', '19980729@sina.com', '软件工程', 23,'2', '1', '2001-04-22 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('大乔', '17799990005', 'daqiao666@sina.com', '舞蹈', 22, '2','0', '2001-02-07 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('露娜', '17799990006', 'luna_love@sina.com', '应用数学', 24,'2', '0', '2001-02-08 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('程咬金', '17799990007', 'chengyaojin@163.com', '化工', 38,'1', '5', '2001-05-23 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('项羽', '17799990008', 'xiaoyu666@qq.com', '金属材料', 43,'1', '0', '2001-09-18 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('白起', '17799990009', 'baiqi666@sina.com', '机械工程及其自动化', 27, '1', '2', '2001-08-16 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('韩信', '17799990010', 'hanxin520@163.com', '无机非金属材料工程', 27, '1', '0', '2001-06-12 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('荆轲', '17799990011', 'jingke123@163.com', '会计', 29, '1','0', '2001-05-11 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('兰陵王', '17799990012', 'lanlinwang666@126.com', '工程造价',44, '1', '1', '2001-04-09 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('狂铁', '17799990013', 'kuangtie@sina.com', '应用数学', 43,'1', '2', '2001-04-10 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('貂蝉', '17799990014', '84958948374@qq.com', '软件工程', 40,'2', '3', '2001-02-12 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('妲己', '17799990015', '2783238293@qq.com', '软件工程', 31,'2', '0', '2001-01-30 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('芈月', '17799990016', 'xiaomin2001@sina.com', '工业经济', 35,'2', '0', '2000-05-03 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('嬴政', '17799990017', '8839434342@qq.com', '化工', 38, '1','1', '2001-08-08 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('狄仁杰', '17799990018', 'jujiamlm8166@163.com', '国际贸易',30, '1', '0', '2007-03-12 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('安琪拉', '17799990019', 'jdodm1h@126.com', '城市规划', 51,'2', '0', '2001-08-15 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('典韦', '17799990020', 'ycaunanjian@163.com', '城市规划', 52,'1', '2', '2000-04-12 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('廉颇', '17799990021', 'lianpo321@126.com', '土木工程', 19,'1', '3', '2002-07-18 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('后羿', '17799990022', 'altycj2000@139.com', '城市园林', 20,'1', '0', '2002-03-10 00:00:00');
INSERT INTO tb_user (name, phone, email, profession, age, gender, status,createtime) 
    VALUES ('姜子牙', '17799990023', '37483844@qq.com', '工程造价', 29,'1', '4', '2003-05-26 00:00:00');
```

①为name字段创建索引

```
CREATE INDEX idx_user_name ON tb_user(name);
```

②phone非空且唯一，创建唯一索引

```
CREATE UNIQUE INDEX idx_user_phone ON tb_user(phone);
```

③为profession、age、status创建联合索引

```
CREATE INDEX idx_user_pro_age_sta ON tb_user(profession,age,status);
```

④查看表中所有索引数据

```
show index from tb_user;
```

## 2.5 SQL性能优化

### 2.5.1 SQL执行频率

提高SQL的查询性能，可以通过创建索引来完成，而创建索引之前，我们需要明确指定数据库是以查询为主，还是以增删改为主，不同的需求场景优化手段并不相同，而判断数据库是以查询为主还是以增删改为主，可以通过SQL的执行频率来进行分析。

以下命令用来查看insert、select、delete、update的执行频次：

```
// session 是查看当前会话 ;
// global 是查询全局数据 ;
SHOW GLOBAL STATUS LIKE 'Com_______';
```

- Com_delete：删除次数
- Com_insert：插入次数
- Com_select：查询次数
- Com_update：更新次数

### 2.5.2 慢查询日志

根据SQL执行频率我们知道了数据库是以查询为主，还是以增删改为主，那我们又该针对那些SQl进行优化呢？这时需要借助慢查询日志，通过慢查询日志可以筛选出效率低下的SQL，可以让我们进行针对性的优化。

**1）查看是否开启慢查询日志**

```
show Variables like 'slow_query_log';
```

**2）开启慢查询日志**

开启慢查询日志，需要在MySQl配置文件（/etc/my.cnf或者my.ini）中[mysqld]下添加：

```
# 开启MySQL慢日志查询开关
slow_query_log=1
# 设置慢日志的时间为2秒，SQL语句执行时间超过2秒，就会视为慢查询，记录慢查询日志
long_query_time=2
```

**3）重启服务**

- Windows：打开服务面板，找到MYSQL，点击重新启动
- Linux：执行命令 systemctl restart mysqld

**4）查看慢查询日志**

- Windows：/data/用户名-slow.log
- Linux：/var/lib/mysql/localhost-slow.log。

### 2.5.3 profile

现在我们找到了效率低下的SQL，需要优化我们需要找到到底是哪个阶段导致的效率低下，这时需要借助profile来查看（客户端不互通）

**1）查看当前MySQL是否支持profile操作**

```
SELECT @@have_profiling ;
```

**2）查看MySQL是否开启profile操作**

```
SELECT @@profiling;
```

**3）开启profile操作**

```
// session当前会话
// global全局
SET profiling = 1;
```

**4）查看每一条SQL的耗时情况**

```
show profiles;
```

**5）查看指定query_id的SQL语句各个阶段的耗时情况**

```
show profile for query query_id;
```

**6）查看指定query_id的SQL语句的CPU使用情况**

```
show profile cpu for query query_id;
```

### 2.5.4 explain

explain或者desc命令获取select语句执行过程中的详细信息，包括表如何连接和连接的顺序

```
explain select语句;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667645088412-a255a077-5b97-43a9-9bcb-c0c8cfa6362f.png)

个字段含义：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667645135540-520b4e71-dfba-4742-8414-fbfd4e204e4b.png)

## 2.6 使用索引

### 2.6.1 最左前缀法则

如果索引了多列（联合索引），需要遵循最左前缀法则。

最左前缀法则：查询从索引的最左列开始，并且不跳过索引中的列，因为一旦跳过某个索引列，将导致后面的索引失效。

在上面的学习中，在tb_user表中将profession、age、status这三个字段建立了联合索引：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667646633877-63c6a6c1-1ba7-4e8b-9ef4-783a07aa108d.png)

下面以这三个索引字段进行演示：

```
explain select * from tb_user where profession = '软件工程' and age = 31 and status = '0';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667646863181-b0c90c01-36a8-4aaa-a19b-69dcbc890ee7.png)

```
explain select * from tb_user where profession = '软件工程' and age = 31;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667646916725-46030d72-13a5-48d3-a304-8d24c9ffef80.png)

```
explain select * from tb_user where profession = '软件工程';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667646954138-9618d4ec-b53a-43dc-be04-da31fde14c49.png)

我们发现只要SQL中profession字段存在索引就会生效，只是索引长度不同，从上面的测试中可以推断出profession索引长36，age索引长2，status索引长4。

继续测试：

```
explain select * from tb_user where age = 31 and status = '0';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667647257288-44a0d0da-45d6-4723-af75-cf5f8900b4c6.png)

```
explain select * from tb_user where status = '0';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667647277673-4880d2dc-f1ff-470e-b330-2c6ac1923d02.png)

我们发现SQL中没有了profession字段，索引就会失效，不满足最左前缀法则。

继续测试：

```
explain select * from tb_user where profession = '软件工程' and status = '0';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667647423639-a30b9bd9-a59f-436c-bd30-a8c22bc098ac.png)

我们发现索引长度为36而不是40，可见status索引失效的。

需要注意一点：上面所说的最左前缀是指联合索引的第一个列，而不是我们所写的SQL的最左，即使先查询age或者status再查询profession，三个字段的索引都会生效！

```
explain select * from tb_user where age = 31 and status = '0' and profession = '软件工程';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667647783554-73186cd4-ad87-48c3-9f49-0a1ae59d457c.png)

### 2.6.2 范围查询

联合索引中，出现范围查询（>、<），范围查询右边的列索引失效。

测试：

```
explain select * from tb_user where profession = '软件工程' and age > 30 and status = '0';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667648086048-15618593-d5a8-448d-a73a-611cbf5dd02a.png)

索引长度为38（profession：36 + age：2），可见范围查询右边的status列索引失效。

```
explain select * from tb_user where profession = '软件工程' and age >= 30 and status = '0';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667648228968-2913d623-ee2d-4fcd-9304-1f21545dff19.png)

但是如果使用>=或者<=，我们惊奇的发现索引是生效的。所以我们在业务允许的情况下，尽量使用>=或者<=，而不是>或者<。

### 2.6.3 索引失效情况

**1）索引列运算**

不要在索引列上进行运算操作，否则将导致索引失效。

测试：

```
explain select * from tb_user where phone = '17799990015';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667648698362-99adf9cd-56a5-47be-8447-24aa4fdc38e1.png)

```
explain select * from tb_user where substring(phone,10,2) = '15';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667648748639-9328d9c7-8be0-4d41-a0d9-89a53031efcb.png)

我们发现一旦phone索引列进行运算，会导致索引失效。

**2）字符串不加引号**

使用字符串类型字段时，不加引号，将导致索引失效。

测试：

```
explain select * from tb_user where phone = '17799990015';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667649108170-b4bed6fe-a7b2-4bbc-805e-069c2ca334e2.png)

```
explain select * from tb_user where phone = 17799990015;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667649123395-bf490cda-289f-4b12-aa2d-d94efbec31e5.png)

我们发现字符串类型数据不加引号并不影响查询结果，但会使索引失效。

**3）模糊查询**

模糊查询时，如果只是尾部模糊匹配，并不影响索引，但如果是头部模糊匹配，将导致索引失效。

测试：

```
explain select * from tb_user where profession like '软件%';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667649373109-5d3f1e09-ca79-476a-affa-4b4f570f223d.png)

```
explain select * from tb_user where profession like '%工程';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667649403369-9ca3159c-096b-444b-9cac-2cf2f8fd29c7.png)

```
explain select * from tb_user where profession like '%工%';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667649439055-443670e1-7afa-404b-b984-67e98d376dbf.png)

我们发现%加载关键字之后，并不影响索引，%加载关键字之前，索引失效。

**4）or连接条件**

用or分割的条件，只有or前后的列所有索引时，索引才会生效，其中一方没有索引，都将导致索引全部失效。

测试：

```
explain select * from tb_user where id = 10 or age = 23;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667649948028-eed146ad-e127-41c5-9d62-5e3a3fe654ac.png)

```
explain select * from tb_user where  age = 23 or id = 10;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667649976769-6fda144a-2609-42ac-b2db-a92ae0e07c30.png)

```
explain select * from tb_user where id = 10 or profession = '英语';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667650150610-1318928d-b721-4744-ad4f-d3e507a703d0.png)

**5）数据分布影响**

如果MySQL评估使用索引比直接全表还慢，就不会使用索引。

测试：

```
explain select * from tb_user where phone >= '17799990005';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667650343471-a6eee254-5c89-4ed8-b7e3-37c708c4c9e5.png)

### 2.6.4 SQL提示

SQL提示是优化数据库的重要手段，就是在SQL中添加一些人为的提示已达到操作优化的目的。

上面我们知道profession有一个联合索引，我们现在再给它创建一个索引：

```
create index idx_user_pro on tb_user(profession);
```

执行查询，观察MySQL选择那个索引：

```
explain select * from tb_user where profession = '软件工程';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667650954782-a8b89692-cfd6-482e-aa70-1edb302e5617.png)

接下来，我们使用SQL提示来完成索引的改变，SQL提示有以下几种：

- user index ：建议MySQL使用指定索引
- ignore index：忽略指定索引
- force index：强制使用指定索引

示例：

```
explain select * from tb_user force index(idx_user_pro) where profession = '软件工程';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667652270885-03738bcb-cbab-4d88-90aa-82d58e61ce27.png)

### 2.6.5 覆盖索引

尽量使用覆盖索引，减少使用select * 。

覆盖索引是指，查询列使用了索引，并且需要返回的列在该索引中全部能找到。目的还为了不回表查询，只在二级索引就可以找到数据并返回。

测试：

```
explain select id,profession,age,status from tb_user where profession='软件工程';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667655972864-d65fa37a-e87b-44e5-a213-8652e5c50cb4.png)

profession、age、status是联合查询，而id就在叶子结点下挂着，所有只使用二级索引就可以查询到全部返回列。

如果使用联合查询以及独立索引，将不能使用覆盖索引，将name作为条件业并没有意义

```
explain select id,profession,age,status,name from tb_user where age = 31 and name = '妲己';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667662238257-2c5bcaf2-4984-4445-809e-cda6441cb26e.png)

在使用联合索引，即使不使用最左节点，也可以使用二级索引完成查询

```
 explain select id,profession,age,status from tb_user where age = 31;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667662442528-78ca53e2-d299-42f5-ab89-b94c7e0c40d0.png)

在上述中主要观察Extra的值，只要出现Using index就是使用覆盖索引，Using where和使用索引覆盖没有关系

### 2.6.6 前缀索引

当索引为字符串时，有时需要索引很长一段字符串，这会让索引变得很长，浪费磁盘IO，影响查询效率。

前缀索引就是只索引字符串的一部分前缀，如此可以节省索引空间，提高查询效率。

1）语法

```
create index idx_xxxx on table_name(column(n)) ;
```

示例：

为tb_user表的email字段，建立长度为5的前缀索引。

```
create index idx_email_5 on tb_user(email(5));
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667696730293-cb2448bf-0844-4a3d-917a-497e6133f80d.png)

2）前缀长度

可以根据索引的选择性来决定。

索引的选择性是指不重复的索引值和数据表的记录总数的比值，这个选择性越高，查询效率越高。唯一索引的选择性为1，这是最好的索引选择性，同时性能最好。

3）前缀索引的查询流程

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667696082322-955046cf-fdce-456f-b63a-a54d8c62f3d5.png)

### 2.6.7 单列索引和联合索引

单列索引：一个索引只包含一个列

联合索引：一个索引包含多个列

前面所得索引覆盖是对联合索引来讲的，如果我们使用多个单列索引，是否能覆盖索引，不进行回表查询呢？

```
explain 4select id,phone,name from tb_user where phone='17799990000' and name = '吕布';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667696978133-522c556e-e0ce-4e7e-b5f5-8c153cad25b6.png)

可以看到name、phone都是索引列，但是最后只是选取了其中一个索引，还是要进行回表查询。

如果在业务中phone和name经常需要一起查询，可以将这两个列建立联合索引，这样就不会回表查询，提高查询效率。

联合索引结构：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667697328545-4b286b55-6e75-45b0-8e03-4721c1cc4295.png)

## 2.7 索引设计原则

1）针对于数据量较大，且查询比较频繁的表建立索引。

2）针对于常作为查询条件（where）、排序（order by）、分组（group by）操作的字段建立索引。

3）尽量选择区分度高的列作为索引，尽量建立唯一索引，区分度越高，使用索引的效率越高。

4）如果是字符串类型的字段，字段的长度较长，可以针对于字段的特点，建立前缀索引。

5）尽量使用联合索引，减少单列索引，查询时，联合索引很多时候可以覆盖索引，节省存储空间，避免回表，提高查询效率。

6）要控制索引的数量，索引并不是多多益善，索引越多，维护索引结构的代价也就越大，会影响增删改的效率。

7）如果索引列不能存储NULL值，请在创建表时使用NOT NULL约束它。当优化器知道每列是否包含NULL值时，它可以更好地确定哪个索引最有效地用于查询。

# 3.SQL优化

## 3.1 插入数据

### 3.1.1 insert

如果一需要次性往表中插入多条数据，可以从以下方面进行优化：

```
insert into tb_test values(1,'tom');
insert into tb_test values(2,'cat');
insert into tb_test values(3,'jerry');
.....
```

1）优化方案一

批量插入数据

```
Insert into tb_test values(1,'Tom'),(2,'Cat'),(3,'Jerry')...;
```

2）优化方案二

手动控制事务

```
start transaction;
insert into tb_test values(1,'Tom'),(2,'Cat'),(3,'Jerry');
insert into tb_test values(4,'Tom'),(5,'Cat'),(6,'Jerry');
insert into tb_test values(7,'Tom'),(8,'Cat'),(9,'Jerry');
...
commit;
```

3）优化方案三

主键顺序插入，性能要高于乱序插入

```
主键乱序插入 : 8 1 9 21 88 2 4 15 89 5 7 3
主键顺序插入 : 1 2 3 4 5 7 8 9 15 21 88 89
```

### 3.1.2 大批量插入数据

如果需要一次性插入大批量的数据（几百万、几千万数据量），使用insert插入性能比较低，可以使用MySQL提供的load指令进行插入

1）客户端连接服务端时，添加参数 --local-infile

```
mysql –-local-infile -u root -p
```

2）开启从本地加载文件导入数据的开关

```
set global local_infile = 1;
```

3）将准备好的数据加载到表结构中

```
load data local infile '/root/sql1.log' into table tb_user fields terminated by ',' lines terminated by '\n' ;
```

## 3.2 主键优化

为什么顺序插入要优于乱序插入呢？下面详细说明

1）数据组织方式

在InnoDB存储引擎中，表数据都是**根据主键顺序组织存放**的。使用这种存储方式的表也称为**索引组织表**。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667701628297-c9f73a12-69ad-48c4-b26e-1e79580b2572.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667701639506-06454801-80ce-4e67-81df-4bb09d9a1f8d.png)

InnoDB存储引擎中，数据行存储在Page中，Page大小为16K，数据行大小不确定，一般平均下来估算为1K，也就是说，Page中存储的数据行是有限的，如果当前Page存储不下，会存储到下一个Page，Page之间使用指针连接

2）页分裂

每个页存储2-n行数据（数据行过大，会溢出），根据主键排列。

主键顺序插入效果：

①从磁盘中申请页，根据主键顺序插入数据行

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667703189957-dba6f8f8-8f2e-47bf-8c21-11b8b0582bca.png)

②第一页没有满，继续往第一页插入数据

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667703201207-431639fd-ef02-4cba-8f9e-295744befb66.png)

③当第一页写满，再写入第二页，页之间使用指针连接

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667703212459-efe75b91-364d-4bde-8679-5615ba518fff.png)

④当第二页也写满了，继续往第三页写入

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667703224313-f07c0239-08fc-4bc7-96f0-3525eb8d8741.png)

主键乱序插入效果：

①假如1#、2#页都写满了，存放如图所示数据

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667703366090-f2a86937-fc79-494a-b339-06199c1c4841.png)

②此时再插入id为50的记录

因为两个页都满了，此时会吧id为50的数据存储到第三页吗？

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667703428064-739dcb92-6d2a-4b1e-9cbd-19cb1bc1eeb2.png)

因为聚焦索引的叶子节点是按主键顺序排列的，所以该数据应该插入到id为47的后面，那应该怎么做呢？

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667703534421-cbb398de-2011-4421-888c-8a855cf00518.png)

这时，会开辟新的页3#

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667703599421-4ef13d00-96f5-4b63-addb-dc08a029eea0.png)

但并不是直接将50存入3#，而是将1#的后一半数据移动到3#，然后再在3#后插入50

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667703686970-c7c79be9-698a-4ac2-b700-4950599ccb66.png)

这时虽然将50存入了页中，但是发现3#并没有指针连接，如果我们使用1,2,3的顺序使用指针连接页的话，会出现顺序问题，1#的后面应该是3#才对，事实上就是如此，1#的指针会连接到3#上，3#的指针再连接到2#上，形成新的顺序

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667703874765-03596b0e-1d80-4ef7-8121-122f3c6abb22.png)

上面这种情况就是页分裂，是比较浪费性能的

3）页合并

现在具有以下叶子结点结构

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667704147157-d740c27d-3283-4e72-9d18-f3e2a2bc8ffd.png)

当我们对已有数据进行删除时，具体效果如下：

当删除一行记录时，这行记录并没有被物理删除，而是被标记（flaged）为删除，并且它的空间变得可以被其他记录声明使用

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667704366226-c087b526-217f-4b37-a44f-2df7b11daf35.png)

继续删除2#的记录

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667704389632-bb8bf843-dd1c-4c47-a3fd-955b768b595d.png)

当页中删除的记录达到MERGE_THRESHOLD（默认为页的50%，可以自定义设置），InnoDB会寻找最靠近的页（前或后）看是否能结合两个页从而对页空间进行优化，提高查找效率

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667704531512-33d390b3-2b56-42fe-9988-6c5f1e298eff.png)

删除数据，并将页合并之后，再插入数据21，会直接插入到3#

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667704633638-4c58b7df-76d1-44f5-82c8-2f5f489accbe.png)

上面这种现象就称为页合并

4）索引设计原则

- 满足业务需求的情况下，尽量缩短主键的长度
- 插入数据时，尽量选择顺序插入，最好选择AUTO_INCREMENT主键自增
- 尽量不要使用UUID作为主键或者其他自然主键
- 业务操作时，尽量避免对主键的修改

## 3.3 order by优化

MySQL的排序有两种情况：

- Using filesort：通过表的索引或者全表扫描，获得满足条件的数据行，然后在排序缓冲区（sort buffer）完成排序操作。所有不是通过索引直接返回排序结果的排序都是Using filesort
- Using index：通过有序索引顺序扫描直接返回有序数据，不需要额外的排序，操作效率高

下面通过测试，寻找规律：

删除掉name、phone的索引

```
drop index idx_user_phone on tb_user;
drop index idx_user_phone_name on tb_user;
drop index idx_user_name on tb_user;
```

执行排序

```
explain select id,age,phone from tb_user order by age ;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667706636453-efbd2cae-37ff-4370-9a0d-ac42792b498c.png)

```
explain select id,age,phone from tb_user order by age, phone ;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667706695159-26277582-b7b2-49a8-8c5c-a2b1f2d2f4ad.png)

因为phone和name都没有索引，所以排序时使用Using filesort，效率低下

下面创建联合索引

```
create index idx_user_age_phone_aa on tb_user(age,phone);
```

再次执行排序

```
explain select id,age,phone from tb_user order by age ;
explain select id,age,phone from tb_user order by age , phone;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667706891677-2e56852f-7392-4be5-8eb2-91f30f3744e4.png)

发现Using index，查找有序索引直接返回

上面都是升序排序，下面测试降序排序

```
explain select id,age,phone from tb_user order by age desc , phone desc ;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667707014924-c2c72a17-9efe-4746-8cda-cccb7f4d557b.png)

Backward index表示反向扫描索引，默认索引的叶子结点是从小到大排序的，降序排序的话，从大到小搜索，同样使用到了索引

mySQL8支持降序索引，也可以创建降序索引

下面交换name和phone的位置进行测试：

```
explain select id,age,phone from tb_user order by phone , age;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667707232218-4717d30e-3e20-4f1e-bddb-356a78629254.png)

发现既有Using index也有Using filesort，可见排序也是要遵循最左前缀法则

下面使age和phone一个降序一个升序，继续测试：

```
explain select id,age,phone from tb_user order by age asc , phone desc ;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667707494927-76e31f22-4a6e-4bc8-b9e9-8e442dc30cbb.png)

因为创建索引时，如果没有指定默认是按照升序排序的

解决这种情况，**可以创建索引时，指定索引列的排序方式**。

```
create index idx_user_age_phone_ad on tb_user(age asc ,phone desc);
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667707760427-14cd9bac-4dff-4a93-8732-6fafc21d0f35.png)

这是再次进行排序：

```
explain select id,age,phone from tb_user order by age asc , phone desc ;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667707821625-b8a1d2fa-6437-4d49-9a7c-6bb90da448d5.png)

通过创建索引时指定排序方式，我们可以通过直接使用索引拿到一个升序一个降序的排排序结果

升降序联合索引结果图示：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667707946124-83fe3900-467f-4f55-824a-596542b8847a.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667707952266-3900aa3a-4748-4c30-9b3e-6c8976e6136f.png)

对于name相同的的记录，phone使用降序排列

由上面的测试，总结出order by的优化结果：

- 根据排序字段建立合适的索引，多字段排序时，遵循最左前缀法则
- 尽量使用索引覆盖
- 多字段排序，一个升序一个降序，注意创建联合索引时的索引排序规则
- 如果不可避免的出现Using filesort ，大数据量排序时，可以适当增大排序缓冲区 sort_buffer_size（默认大小256K）

## 3.4 group by优化

对于group by优化有以下建议：

- 利用索引排序时，同样遵循最左前缀法则
- 分组操作，可以通过索引提高效率

## 3.5 limit优化

通过覆盖查询加子查询进行优化

```
explain select * from tb_sku t , (select id from tb_sku order by id limit 2000000,10) a where t.id = a.id;
```

## 3.6 count优化

数据量很大时，执行count是非常耗费时间的，对于count操作，MyISAM和InnoDB底层不同

- MyISAM总是把总行数记录在磁盘上，执行count（*）时直接返回结果即可，效率非常高，但是一旦是带条件的count，MyISAM也很慢
- InnoDB执行count时，是把数据一行行从InnoDB中读出来，然后在累计计数

主要的优化思路是自己计数，不使用MySQL进行统计（可以使用Redis），但是如果携带条件的count还是很麻烦。

count（）是一个聚合函数，对于满足条件的结果集，一行行的判断，如果count函数的参数不是NULL就加1，否则不加，最后返回累计值

count的用法主要有：

- count(*)
- count(主键)
- count(数字)
- count(字段)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667717464247-33e28282-d8be-47b1-be05-e92b8c1d34dd.png)

按照效率排序的话，count(字段) < count(主键 id) < count(1) ≈ count(*)，所以尽量使用 count(*)。

## 3.7 update优化

update本质上是先删除老数据，然后插入新数据，为保证更新的正常执行，需要开启事务。

如果where后跟的是索引列，InnoDB为行锁，否则为表锁。而且一旦行锁失效，会从行锁升级为表锁。

# 4.视图

## 4.1 视图

### 4.1.1 介绍

视图（View）是一种虚拟存在的表。视图中的数据并不在数据库中实际存在，行和列数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的。

通俗的讲，视图只保存了查询的SQL逻辑，不保存查询结果。所以我们在创建视图的时候，主要的工作就落在创建这条SQL查询语句上。

# 5.存储过程

# 6.触发器

# 锁
---
MySQL 的锁机制可以按照粒度划分为**全局锁**、**表级锁** 和 **行级锁** 三大类，其他锁（如意向锁、间隙锁、Next-Key Lock 等）通常是这些锁的子类或衍生形式，依赖于具体的存储引擎（主要是InnoDB）和使用场景。
MySQL 的锁机制是数据库并发控制的核心，用于保证数据一致性和事务隔离性。
## 全局锁
---
- **定义**：
	全局锁锁定整个数据库实例，通常用于全局只读或备份。
- **使用场景**：
	- 逻辑备份（如 `mysqldump`）。
	- 设置全局只读状态。
- **实现机制**：
	- 通过 `FLUSH TABLES WITH READ LOCK` 实现。
	- 所有表加读锁，禁止写操作。
	- 配合 `SET GLOBAL read_only=1` 可增强只读（普通用户只读，不限制 `SUPER` 权限的账户，常用于主从复制场景中，主库写操作，从库读操作）。
	- 与存储引擎无关，由 MySQL Server 层实现。
```sql
FLUSH TABLES WITH READ LOCK;
-- 执行备份
UNLOCK TABLES;
```
## 表级锁
---
- **定义**：
	表级锁是锁定整个表的一种锁机制，适用于 MyISAM、MEMORY 等非事务存储引擎，也在InnoDB 中某些场景下使用（如 DDL 操作）。
- **类型**：
	- 读锁：
		- 允许多个事务同时持有读锁，读取表数据。
		- 禁止其他事务对表加写锁或执行写操作。
	- 写锁
		- 仅允许一个事务持有写锁，独占表进行写操作。
		- 禁止其他事务对表加任何锁或执行读写操作。
- **使用场景**：
	- MyISAM 引擎的读写操作（如 `SELECT`, `INSERT`, `UPDATE`）。
	- 某些 InnoDB DDL 操作（如 `ALTER TABLE`）会隐式使用表级锁。
	- 全局备份或只读场景（如 `LOCK TABLES`）。
- **实现机制**：
	- 表级锁由 MySQL Server 层管理，非存储引擎层。
	- 锁信息存储在内存中，维护简单。
	- 读锁和写锁的兼容性：
	    - 读锁与读锁兼容。
	    - 写锁与任何锁不兼容。
- **子类**：
	- 意向锁
	- 元数据锁
- **优缺点**：
	- 优点：
		- 实现简单，加锁/释放开销低。
		- 适合读多写少的场景（如报表查询）。
	- 缺点：
		- 锁粒度大，并发性差。
		- 不支持事务，易导致锁冲突。
- **注意事项**：
	- MyISAM 表级锁优先级：写锁 > 读锁，可能导致读操作饥饿。
	- 避免在高并发场景下使用 MyISAM，推荐 InnoDB。
```sql
-- MyISAM表加读锁
LOCK TABLES my_table READ;
SELECT * FROM my_table;
UNLOCK TABLES;

-- MyISAM表加写锁
LOCK TABLES my_table WRITE;
UPDATE my_table SET col = 1;
UNLOCK TABLES;
```
### 意向锁
---
- **定义**
	意向锁是 InnoDB 特有的表级锁，用于**协调表级锁和行级锁之间的关系**，表示事务后续**将**对表中**某些行加锁**。
- **类型**
	- 意向共享锁（Intention Shared Lock, **IS** Lock）：
		- 表示事务将对某些行加 S 锁。
	- 意向排他锁（Intention Exclusive Lock, **IX** Lock）：
		- 表示事务将对某些行加 X 锁。
- **需求**：
	- 提高锁冲突检测效率：
		- 当事务需要加表级锁（如 ALTER TABLE 需要表级 X 锁）时，InnoDB 只需检查表上是否存在意向锁，而无需扫描每一行。
		- 如果表上有 IX 锁，说明有事务持有行级 X 锁，表级 X 锁无法立即获取。
		- 如果表上无意向锁，说明表中没有行级锁，表级锁可以直接加。
	- 支持多粒度锁的兼容性：
		- InnoDB 支持多粒度锁（表级和行级），需要一种机制来协调两者之间的冲突。
		- 意向锁作为“桥梁”，通过表级锁的状态反映行级锁的存在，简化锁兼容性检查。
	- 保证事务一致性：
		- 意向锁防止表级操作（如 DDL）与行级操作（如 DML）冲突，确保事务隔离性和数据一致性。
		- 例如，事务 A 正在更新表中某行（持有行级 X 锁），事务 B 尝试执行 ALTER TABLE（需要表级 X 锁）。意向锁（IX）会通知事务 B 等待，直到事务 A 完成。
	- 支持高并发场景：
		- 意向锁是轻量级的表级锁，不会阻塞行级锁操作，仅在表级锁请求时起作用。
		- 这允许高并发的事务操作（如多个事务同时加行级锁），同时保证表级操作的安全性。
- **使用场景**
	- 事务在加行级锁前，自动为表加意向锁。
	- 防止表级锁（如 DDL 操作）与行级锁冲突。
- **实现机制**
	1. 事务加行级锁前：
		- 如果事务需要对某行加 S 锁，InnoDB 自动为表加 IS 锁。
		- 如果事务需要对某行加 X 锁，InnoDB 自动为表加 IX 锁。
	2. 表级锁请求：
		- 当事务请求表级锁（如表级 S 锁或 X 锁），InnoDB 检查表上的意向锁状态。
		- 如果存在冲突的意向锁（如 IX 锁与表级 S 锁冲突），表级锁请求进入等待。
	3. 锁释放：
		- 事务提交或回滚时，行级锁和对应的意向锁同时释放。
		- 表级锁请求可继续处理。
	- 意向锁由 InnoDB 自动管理，无需用户显式加锁。
	- 锁兼容性：
	    - IS 锁与 IS/IX 锁兼容，与表级 X 锁不兼容。
	    - IX 锁与 IX/IS 锁兼容，与表级 S/X 锁不兼容。
	- 作用：快速判断表是否可加表级锁，避免逐行检查。
```sql
-- 事务A：更新一行，隐式加行级X锁和表级IX锁
START TRANSACTION;
UPDATE my_table SET col = 1 WHERE id = 10;

-- 事务B：尝试修改表结构，请求表级X锁
ALTER TABLE my_table ADD COLUMN new_col INT;
-- 事务B因表上的IX锁而等待，直到事务A提交
```
### 元数据锁
---
- **定义**
	元数据锁用于保护表结构（如表名、列定义、索引等），防止 DDL 操作（如 ALTER TABLE）与 DML 操作冲突。
- **类型**：
	- **MDL_SHARED**：读表结构或数据操作（如 SELECT, INSERT）。
	- **MDL_SHARED_WRITE**：写数据操作（如 UPDATE, DELETE）。
	- **MDL_EXCLUSIVE**：修改表结构（如 ALTER, DROP）。
- **需求**
	- 防止表结构与数据操作冲突：
		- 当事务访问表（如 SELECT, INSERT）时，自动为表加 MDL 读锁（MDL_SHARED），允许其他事务读表但禁止修改表结构。
		- 当执行 DDL 操作（如 ALTER TABLE）时，加 MDL 写锁（MDL_EXCLUSIVE），禁止任何事务访问表。 
	- 保证事务一致性：
		- 确保事务在执行期间，表结构保持不变。例如，事务中的 SELECT 不会因表结构变更而失败。
	- 协调并发 DDL 操作：
		- 防止多个 DDL 操作同时修改同一表，避免元数据损坏。
- **使用场景**
	- DDL 操作（如 CREATE, ALTER, DROP）。
	- 事务访问表时，自动加 MDL 锁。
- **实现机制**
	- 由 MySQL Server 层管理，自动加锁/释放。
	- DDL 操作需等待所有事务释放 MDL 锁。
	- 兼容性：
		- MDL_SHARED 与 MDL_SHARED 兼容，允许多个事务读表。
		- MDL_EXCLUSIVE 与任何 MDL 锁不兼容，独占表。
```sql
-- 隐式加MDL锁
ALTER TABLE my_table ADD COLUMN new_col INT;
```
## 行级锁
---
- **定义**：
	行级锁是锁定表中特定行的一种锁机制，仅 InnoDB 支持，是事务性存储引擎的核心并发控制手段。
- **类型**：
	- 共享锁（Shared Lock, S Lock）：
		- 允许事务读取行数据，其他事务可加 S 锁但不能加 X 锁。
	- 排他锁（Exclusive Lock, X Lock）：
		- 允许事务修改行数据，禁止其他事务对该行加任何锁。
- **使用场景**：
	- 事务性操作，如 INSERT, UPDATE, DELETE。
	- 显式锁定，如 `SELECT ... FOR UPDATE`（加X锁）或 `SELECT ... LOCK IN SHARE MODE`（加 S 锁）。
- **实现机制**：
	- 锁绑定到索引记录上，依赖 InnoDB 的 B+ 树索引。
	- 如果查询无索引，可能退化为表锁（全表扫描）。
	- 锁信息存储在内存中，记录每个被锁定的行。
	- 锁冲突通过锁兼容性矩阵检测：
	    - S 锁与 S 锁兼容。
	    - X 锁与任何锁不兼容。
- **子类**：
	- 记录锁
	- 间隙锁
	- Next-Key Lock
- **优缺点**：
	- 优点：
		- 锁粒度小，并发性高，适合高并发事务场景。
		- 支持事务隔离，配合 MVCC 保证一致性。
	- 缺点：
		- 加锁/释放开销较大。
		- 可能因索引缺失导致锁范围扩大。
- **注意事项**：
	- 确保查询使用索引，避免锁表。
	- 行锁可能升级为表锁（如无主键或唯一索引）。
	- 事务过长可能导致锁持有时间长，影响性能。

```sql
-- 加共享锁
SELECT * FROM my_table WHERE id = 1 LOCK IN SHARE MODE;

-- 加排他锁
SELECT * FROM my_table WHERE id = 1 FOR UPDATE;

-- 写操作隐式加 X 锁
UPDATE my_table SET col = 2 WHERE id = 1;
```
### 记录锁
---
- **定义**
	记录锁是针对单行索引记录的锁，精确锁定某条记录，防止其他事务读写。
- **需求**
		记录锁通过锁定单行记录，确保同一时间只有一个事务可以修改该行（排他锁，X Lock），或者多个事务可以读取但不能修改（共享锁，S Lock），从而防止脏写和丢失更新。
- **使用场景**
	- 精确查询（如WHERE id = 10）且命中索引。
	- 配合FOR UPDATE或LOCK IN SHARE MODE。
- **实现机制**
	- 锁绑定到具体索引记录（主键或唯一索引）。
	- 如果查询无索引，可能退化为表锁。
	- 锁类型为S锁或X锁。
```sql
-- 锁定id=10的记录（X锁）
SELECT * FROM my_table WHERE id = 10 FOR UPDATE;

-- 锁定id=10的记录（S锁）
SELECT * FROM my_table WHERE id = 10 LOCK IN SHARE MODE;
```
### 间隙锁
---
- **定义**
	间隙锁是 InnoDB 在可重复读（RR）隔离级别下使用的锁，锁定索引记录之间的“间隙”，防止新数据插入到间隙中，从而避免幻读。
- **需求**
- **使用场景**
	- 范围查询（如WHERE id BETWEEN 10 AND 20）。
	- 防止幻读（Phantom Read），确保事务的可重复读。
- **实现机制**
	- 锁住索引记录之间的空隙（如(10, 20)），不包括记录本身。
	- 常与记录锁结合，形成Next-Key Lock。
	- 仅在RR隔离级别启用，读已提交（RC）级别禁用。
```sql
-- 假设id有记录10, 20
SELECT * FROM my_table WHERE id BETWEEN 10 AND 20 FOR UPDATE;
-- 锁定间隙(10, 20)，禁止插入id为15的记录
```
### Next-Key Lock
---
- **定义**
	Next-Key Lock 是 InnoDB 在 RR 隔离级别下使用的锁，结合记录锁和间隙锁，锁定一个范围（包括记录本身和前面的间隙）。
- **需求**
- **使用场景**
	- 范围查询或防止幻读。
	- 默认在RR隔离级别启用。
- **实现机制**
	- 锁定范围为(a, b]，包括记录b和间隙(a, b)。
	- 由记录锁（锁定b）和间隙锁（锁定(a, b)）组成。
	- 如果查询命中唯一索引，退化为记录锁。
```sql
-- 假设id有记录10, 20
SELECT * FROM my_table WHERE id > 10 AND id < 20 FOR UPDATE;
-- 锁定(10, 20]，包括记录20和间隙(10, 20)
```
## 7.1 全局锁

**全局锁是对整个MySQL数据库加锁，用于维护一个数据库的逻辑一致性。**全局锁一般用于逻辑备份，用于保证数据的一致性，但在分库场景下，对于从表的备份，会导致从库不能执行从主库同步来的二进制日志，导致**主从延迟**，所以在InnoDB引擎中，可以通过在备份语句中添加参数完成不加锁的一致性备份。

**开启全局锁：**

```sql
flush tables with read lock ;
```

**释放锁：**

```
unlock tables ;
```

相关阅读：

[https://www.cnblogs.com/keme/p/11065025.html](https://www.cnblogs.com/keme/p/11065025.html)

## 7.2 表级锁

表级锁分为以下三类：表锁、元数据锁、意向锁。

**表锁是在整张表上添加锁，其中共享锁不影响其它线程读操作，会阻塞写操作；排他锁会阻塞其他线程的读和写操作。**

**元数据锁**（meta data lock）简称 MDL，MDL 是系统自动控制，无需显示调用，在访问一张表的时候自动添加MDL，事务提交，自动释放锁，**作用是维护元数据数据的一致性**，在表上有活动事务的时候，不能对元数据进行写入操作。**防止DML与MDL冲突，保证读写的正确性**。

ps：不可能一个线程在给表中name字段赋值，另一个线程转手就把name字段删除了吧！

**意向锁是为了避免DML执行期间的加的行锁与表锁冲突，在InnoDB中引入了意向锁概念，使得表锁不用检查每行数据是否加锁，使用意向锁减少表锁的检查次数。**

举个栗子：

首先客户端一，对表中数据进行DML，对涉及行添加行锁。这时候客户端二来了，想要对整张表添加表锁，会检查表中是否有行锁，如果没有，则添加表锁，所以会对整张表进行检查，从第一行到最后一行，效率低。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668388053697-2c6ddbaa-2452-4fde-a782-c74aa49fd92d.png)

有了意向锁之后，客户端一在进行DML操作时，不但会对涉及行添加行锁，还会对表添加意向锁，这时客户端二就不用逐行检查是否有行锁了，只需要查看表上意向锁的情况，就可以知道自己添加表锁是否能成功了。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668388273680-823d3f51-07b1-4d17-8ca5-ea8a853d8209.png)

## 7.3 行级锁

行级锁锁住对应行数据，力度最小，发生冲突概率最低，并发最高，**应用在InnoDB引擎中（数据基于索引组织）**，**行锁是通过对索引上的索引项加锁实现的，而不是对记录加的锁**。

行级锁分为以下三类：

- 记录锁（Record Lock）：锁定单个行记录的锁，防止其他事务对此行进行update和delete。在 RC、RR隔离级别下都支持。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668390666649-aae7e9ca-6e55-428e-b396-1eb8dff55286.png)

- 间隙锁（Gap Lock）：锁定索引记录间隙（不含该记录），确保索引记录间隙不变，防止其他事 务在这个间隙进行insert，产生幻读。在RR隔离级别下都支持

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668390686644-4d43d134-434f-4961-83af-09419570eed6.png)

- 临键锁（Next-Key Lock）：行锁和间隙锁组合，同时锁住数据，并锁住数据前面的间隙Gap。 在RR隔离级别下支持。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668390707288-f7555f87-74be-4f0c-b313-42883572cbf2.png)

常见的SQL语句，在执行时，所加的行锁如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668391028723-601e4874-ab9b-445e-9539-e1ee64b54e32.png)

默认情况下，InnoDB在 REPEATABLE READ事务隔离级别运行，InnoDB使用 next-key 锁进行搜索和索引扫描，以防止幻读。

- 针对唯一索引进行检索时，对已存在的记录进行等值匹配时，将会自动优化为行锁。
- InnoDB的行锁是针对于索引加的锁，不通过索引条件检索数据，那么InnoDB将对表中的所有记录加锁，此时 就会升级为表锁。

默认情况下，InnoDB在 REPEATABLE READ事务隔离级别运行，InnoDB使用 next-key 锁进行搜 索和索引扫描，以防止幻读。

- 索引上的等值查询(唯一索引)，给不存在的记录加锁时, 优化为间隙锁 。
- 索引上的等值查询(非唯一普通索引)，向右遍历时最后一个值不满足查询需求时，next-key lock 退化为间隙锁。
- 索引上的范围查询(唯一索引)--会访问到不满足条件的第一个值为止。

注意：间隙锁唯一目的是防止其他事务插入间隙。间隙锁可以共存，一个事务采用的间隙锁不会 阻止另一个事务在同一间隙上采用间隙锁。

## 7.4 共享锁和排他锁

**共享锁和排他锁并不是真正存在的锁，它是表级锁和行级锁共有的特性。**

- **共享锁（Share Lock，S 锁）**：又称读锁，事务在读取记录的时候获取共享锁，允许多个事务同时获取（锁兼容）。
- **排他锁（Exclusive Lock，X 锁）**：又称写锁/独占锁，事务在修改记录的时候获取排他锁，不允许多个事务同时获取（锁不兼容）

|   |   |   |
|---|---|---|
||**S 锁**|**X 锁**|
|S 锁|不冲突|冲突|
|X 锁|冲突|冲突|

意向锁稍有不同，它也分为两类：

- **意向共享锁（Intention Shared Lock，IS 锁）**
- **意向排他锁（Intention Exclusive Lock，IX 锁）**







|   |   |   |
|---|---|---|
||**IS 锁**|**IX 锁**|
|IS 锁|兼容|兼容|
|IX 锁|兼容|兼容|

意向锁和表级锁的共享锁和排他锁的兼容情况（**意向锁不会与行级的共享锁和排他锁互斥**）：

|   |   |   |
|---|---|---|
||**IS 锁**|**IX 锁**|
|S 锁|兼容|互斥|
|X 锁|互斥|互斥|

# 8.InnoDB引擎

## 8.1 逻辑存储结构

## 8.2 架构

## 8.3 事务原理

### 8.3.1 事务基础

事务 是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系 统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。

事务特性：

- 原子性（Atomicity）：事务是不可分割的最小操作单元，要么全部成功，要么全部失败。
- 一致性（Consistency）：事务完成时，必须使所有的数据都保持一致状态。
- 隔离性（Isolation）：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环 境下运行。
- 持久性（Durability）：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。

那实际上，我们研究事务的原理，就是研究MySQL的InnoDB引擎是如何保证事务的这四大特性的。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668395143803-98a1ffeb-569e-4242-9772-3f6bb7edc48e.png)

而对于这四大特性，实际上分为两个部分。 其中的原子性、一致性、持久化，实际上是由InnoDB中的 两份日志来保证的，一份是redo log日志，一份是undo log日志。 而持久性是通过数据库的锁，加上MVCC来保证的。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668395170357-12d00cdf-1036-4ab7-ae00-4560f434292b.png)

我们在讲解事务原理的时候，主要就是来研究一下redolog，undolog以及MVCC。

### 8.3.2 redo log

重做日志，记录的是事务提交时数据页的物理修改，是用来实现事务的持久性。

该日志文件由两部分组成：重做日志缓冲（redo log buffer）以及重做日志文件（redo log file）,前者是在内存中，后者在磁盘中。当事务提交之后会把所有修改信息都存到该日志文件中, 用于在刷新脏页到磁盘,发生错误时, 进行数据恢复使用。

如果没有redolog，可能会存在什么问题的？ 我们一起来分析一下。

我们知道，在InnoDB引擎中的内存结构中，主要的内存区域就是缓冲池，在缓冲池中缓存了很多的数据页。 当我们在一个事务中，执行多个增删改的操作时，InnoDB引擎会先操作缓冲池中的数据，如果缓冲区没有对应的数据，会通过后台线程将磁盘中的数据加载出来，存放在缓冲区中，然后将缓冲池中的数据修改，修改后的数据页我们称为脏页。 而脏页则会在一定的时机，通过后台线程刷新到磁盘中，从而保证缓冲区与磁盘的数据一致。 而缓冲区的脏页数据并不是实时刷新的，而是一段时间之后将缓冲区的数据刷新到磁盘中，假如刷新到磁盘的过程出错了，而提示给用户事务提交成功，而数据却没有持久化下来，这就出现问题了，没有保证事务的持久性。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668395412792-3b392d0c-870b-48e8-914b-f74a3244bd29.png)

那么，如何解决上述的问题呢？ 在InnoDB中提供了一份日志 redo log，接下来我们再来分析一 下，通过redolog如何解决这个问题。

有了redolog之后，当对缓冲区的数据进行增删改之后，会首先将操作的数据页的变化，记录在redo log buffer中。在事务提交时，会将redo log buffer中的数据刷新到redo log磁盘文件中。 过一段时间之后，如果刷新缓冲区的脏页到磁盘时，发生错误，此时就可以借助于redo log进行数据恢复，这样就保证了事务的持久性。 而如果脏页成功刷新到磁盘或者涉及到的数据已经落盘，此时redolog就没有作用了，就可以删除了，所以存在的两个redolog文件是循环写的。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668395464090-901824df-af12-4675-82e2-36a77d6fb919.png)

那为什么每一次提交事务，要刷新redo log 到磁盘中呢，而不是直接将buffer pool中的脏页刷新到磁盘呢 ?

因为在业务操作中，我们操作数据一般都是随机读写磁盘的，而不是顺序读写磁盘。 而redo log在往磁盘文件中写入数据，由于是日志文件，所以都是顺序写的。顺序写的效率，要远大于随机写。 这种先写日志的方式，称之为 WAL（Write-Ahead Logging）。

### 8.3.3 undo log

回滚日志，用于记录数据被修改前的信息 , 作用包含两个 : 提供回滚(保证事务的原子性) 和 MVCC(多版本并发控制) 。

undo log和redo log记录物理日志不一样，它是逻辑日志。可以认为当delete一条记录时，undo log中会记录一条对应的insert记录，反之亦然，当update一条记录时，它记录一条对应相反的update记录。当执行rollback时，就可以从undo log中的逻辑记录读取到相应的内容并进行回滚。

Undo log销毁：undo log在事务执行时产生，事务提交时，并不会立即删除undo log，因为这些日志可能还用于MVCC。

Undo log存储：undo log采用段的方式进行管理和记录，存放在前面介绍的 rollback segment回滚段中，内部包含1024个undo log segment。

## 8.4 MVCC

### 8.4.1 基本概念

1）当前读

读取的是记录的最新版本，读取时还要保证其他并发事务不能修改当前记录，会对读取的记录进行加锁。对于我们日常的操作，如：select ... lock in share mode(共享锁)，select ...for update、update、insert、delete(排他锁)都是一种当前读

测试：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668396528399-d9f9772e-38e7-404b-9961-b650400fa880.png)

在测试中我们可以看到，即使是在默认的RR隔离级别下，事务A中依然可以读取到事务B最新提交的内容，因为在查询语句后面加上了 lock in share mode 共享锁，此时是当前读操作。当然，当我们加排他锁的时候，也是当前读操作

2）快照读

简单的select（不加锁）就是快照读，快照读，读取的是记录数据的可见版本，有可能是历史数据， 不加锁，是非阻塞读。

- Read Committed：每次select，都生成一个快照读。
- Repeatable Read：开启事务后第一个select语句才是快照读的地方。
- Serializable：快照读会退化为当前读。

测试：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668396637095-65163b0d-c532-4c89-ad66-822045266f26.png)

在测试中,我们看到即使事务B提交了数据,事务A中也查询不到。 原因就是因为普通的select是快照读，而在当前默认的RR隔离级别下，开启事务后第一个select语句才是快照读的地方，后面执行相同的select语句都是从快照中获取数据，可能不是当前的最新数据，这样也就保证了可重复读

3）MVCC

全称 Multi-Version Concurrency Control，多版本并发控制。指维护一个数据的多个版本， 使得读写操作没有冲突，快照读为MySQL实现MVCC提供了一个非阻塞读功能。MVCC的具体实现，还需要依赖于数据库记录中的三个隐式字段、undo log日志、readView

接下来，我们再来介绍一下InnoDB引擎的表中涉及到的隐藏字段 、undolog 以及 readview，从 而来介绍一下MVCC的原理

### 8.4.2 隐藏字段

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668396803229-548ea67a-ee04-4f50-8dac-147c5ff6b81f.png)

当我们创建了上面的这张表，我们在查看表结构的时候，就可以显式的看到这三个字段。 实际上除了这三个字段以外，InnoDB还会自动的给我们添加三个隐藏字段及其含义分别是：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668396827033-3db01ae8-a03a-4ee9-8c72-a798de37c689.png)

而上述的前两个字段是肯定会添加的， 是否添加最后一个字段DB_ROW_ID，得看当前表有没有主键， 如果有主键，则不会添加该隐藏字段。

### 8.4.3 undolog

回滚日志，在insert、update、delete的时候产生的便于数据回滚的日志。 当insert的时候，产生的undo log日志只在回滚时需要，在事务提交后，可被立即删除。 而update、delete的时候，产生的undo log日志不仅在回滚时需要，在快照读时也需要，不会立即被删除。

有一张表原始数据为：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668397138093-d1b349da-4a3d-4d36-948f-3acfef27a0c6.png)

DB_TRX_ID : 代表最近修改事务ID，记录插入这条记录或最后一次修改该记录的事务ID，是自增的。

DB_ROLL_PTR ： 由于这条数据是才插入的，没有被更新过，所以该字段值为null。

然后，有四个并发事务同时在访问这张表。

A. 第一步

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668397214050-34153be5-160d-4447-ac9b-15595071883d.png)

当事务2执行第一条修改语句时，会记录undo log日志，记录数据变更之前的样子; 然后更新记录， 并且记录本次操作的事务ID，回滚指针，回滚指针用来指定如果发生回滚，回滚到哪一个版本。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668397278211-2ab8dae6-4910-42de-89b3-2a463f5036d9.png)

B.第二步

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668397309491-69ae2e30-5b92-4b3d-9ede-db32cb3e4b59.png)

当事务3执行第一条修改语句时，也会记录undo log日志，记录数据变更之前的样子; 然后更新记 录，并且记录本次操作的事务ID，回滚指针，回滚指针用来指定如果发生回滚，回滚到哪一个版本。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668397327121-52d9eabc-b335-4454-9296-3eea760be085.png)

C. 第三步

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668397346107-d37b3ef6-f210-46ef-979a-5e17d99f8c40.png)

当事务4执行第一条修改语句时，也会记录undo log日志，记录数据变更之前的样子; 然后更新记 录，并且记录本次操作的事务ID，回滚指针，回滚指针用来指定如果发生回滚，回滚到哪一个版本

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668397404277-fcb920fa-15b1-4933-b8ca-573dd8a89ed1.png)

最终我们发现，不同事务或相同事务对同一条记录进行修改，会导致该记录的undolog生成一条 记录版本链表，链表的头部是最新的旧记录，链表尾部是最早的旧记录

### 8.4.4 readview

ReadView（读视图）是快照读 SQL执行时MVCC提取数据的依据，记录并维护系统当前活跃的事务 （未提交的）id。

ReadView中包含了四个核心字段：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668397482247-c46a3421-bff8-4564-b8dd-902ec69ac9e4.png)

而在readview中就规定了版本链数据的访问规则： trx_id 代表当前undolog版本链对应事务ID。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668397553612-6c060951-07c8-406e-856a-50de3a5947b2.png)

不同的隔离级别，生成ReadView的时机不同：

- READ COMMITTED ：在事务中每一次执行快照读时生成ReadView。
- REPEATABLE READ：仅在事务中第一次执行快照读时生成ReadView，后续复用该ReadView。

### 8.4.5 原理分析

1）RC隔离级别

RC隔离级别下，在事务中每一次执行快照读时生成ReadView。

我们就来分析事务5中，两次快照读读取数据，是如何获取数据的?

在事务5中，查询了两次id为30的记录，由于隔离级别为Read Committed，所以每一次进行快照读都会生成一个ReadView，那么两次生成的ReadView如下。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668397756736-221a8039-64c1-4149-8c34-7e0365b0f16b.png)

那么这两次快照读在获取数据时，就需要根据所生成的ReadView以及ReadView的版本链访问规则， 到undolog版本链中匹配数据，最终决定此次快照读返回的数据。

A. 先来看第一次快照读具体的读取过程：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668397816417-e394bf3a-3465-4c93-86ba-d25d9763de4f.png)

在进行匹配时，会从undo log的版本链，从上到下进行挨个匹配：

- 先匹配OX00003这条记录，这条记录对应的trx_id为4，也就是将4带入右侧的匹配规则中。 ①不满足 ②不满足 ③不满足 ④也不满足 ，都不满足，则继续匹配undo log版本链的下一条。
- 再匹配第二条OX00002 ，这条记录对应的trx_id为3，也就是将3带入右侧的匹配规则中。①不满足 ②不满足 ③不满足 ④也不满足 ，都不满足，则继续匹配undo log版本链的下一条。
- 再匹配第三条 OX00001，这条记录对应的trx_id为2，也就是将2带入右侧的匹配规则中。①不满足 ②满足 终止匹配，此次快照读，返回的数据就是版本链中记录的这条数据。

B. 再来看第二次快照读具体的读取过程:

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668397968882-0a5053c7-7ba9-4394-9371-5112bfd81d88.png)

在进行匹配时，会从undo log的版本链，从上到下进行挨个匹配：

- 先匹配0X00003这条记录，这条记录对应的trx_id为4，也就是将4带入右侧的匹配规则中。 ①不满足 ②不满足 ③不满足 ④也不满足 ，都不满足，则继续匹配undo log版本链的下一条。
- 再匹配第二条 0X00002，这条记录对应的trx_id为3，也就是将3带入右侧的匹配规则中。①不满足 ②满足 。终止匹配，此次快照读，返回的数据就是版本链中记录的这条数据。

2）RR隔离级别

RR隔离级别下，仅在事务中第一次执行快照读时生成ReadView，后续复用该ReadView。 而RR 是可重复读，在一个事务中，执行两次相同的select语句，查询到的结果是一样的。

那MySQL是如何做到可重复读的呢? 我们简单分析一下就知道了

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668398143296-282a5560-6fe2-4a16-8842-a1e2ebe23eb1.png)

我们看到，在RR隔离级别下，只是在事务中第一次快照读时生成ReadView，后续都是复用该 ReadView，那么既然ReadView都一样， ReadView的版本链匹配规则也一样， 那么最终快照读返回的结果也是一样的。

所以呢，MVCC的实现原理就是通过 InnoDB表的隐藏字段、UndoLog 版本链、ReadView来实现的。 而MVCC + 锁，则实现了事务的隔离性。 而一致性则是由redolog 与 undolog保证。![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668398195598-df953b48-1436-475d-a1a8-2ec8d5eb3cf2.png)

# 9.MySQL管理

## 9.1 系统数据库

Mysql数据库安装完成后，自带了一下四个数据库，具体作用如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668398329587-0e08c83a-77c7-4b55-ac74-71abc119c4dd.png)

## 9.2 常用工具

### 9.2.1 mysql

该mysql不是指mysql服务，而是指mysql的客户端工具。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668398509564-487c2ee5-c259-4bee-afcf-a79840e17f1f.png)

-e选项可以在Mysql客户端执行SQL语句，而不用连接到MySQL数据库再执行，对于一些批处理脚本， 这种方式尤其方便。

示例：

```
mysql -uroot –p123456 db01 -e "select * from stu";
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668407379237-cd1fdb51-4edd-4d4d-8663-83eca1f63ad7.png)

### 9.2.2 mysqladmin

mysqladmin 是一个执行管理操作的客户端程序。可以用它来检查服务器的配置和当前状态、创建并删除数据库等

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668407535005-66c64047-64e3-4949-bbc1-e24be8d549f7.png)

示例：

```
mysqladmin -uroot –p1234 drop 'test01';
mysqladmin -uroot –p1234 version;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668407609378-755638b0-5ce3-4271-97cb-aebec5dcdb0e.png)

### 9.2.3 mysqlbinlog

由于服务器生成的二进制日志文件以二进制格式保存，所以如果想要检查这些文本的文本格式，就会使 用到mysqlbinlog 日志管理工具。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668407658503-1085bda0-ac9e-49f4-b801-fef9573862e1.png)

示例：

查看 binlog.000008这个二进制文件中的数据信息

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668407720655-dfccb5dd-6c99-4cba-b6f0-dbebecf30fd1.png)

上述查看到的二进制日志文件数据信息量太多了，不方便查询。 我们可以加上一个参数 -s 来显示简 单格式。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668407743084-78b1b61b-a7c5-47fa-9841-bad58c8cfecb.png)

### 9.2.4 mysqlshow

mysqlshow 客户端对象查找工具，用来很快地查找存在哪些数据库、数据库中的表、表中的列或者索引。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668407785300-a7c53e89-f687-48ac-8025-993ea018d40a.png)

示例：

查询每个数据库的表的数量及表中记录的数量

```
mysqlshow -uroot -p1234 --count
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668407825375-d9101f44-e977-4f25-bb57-a009fe87e981.png)

查看数据库db01中的course表的id字段的信息

```
mysqlshow -uroot -p1234 db01 course id --count
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668407917226-d2642c0a-3e26-49c4-a54f-44ff870db221.png)

### 7.2.5 mysqldump

mysqldump 客户端工具用来备份数据库或在不同数据库之间进行数据迁移。备份内容包含创建表，及 插入表的SQL语句。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668407961977-2871b305-0602-4290-9946-a063ce3b1afa.png)

示例：

备份db01数据库

```
mysqldump -uroot -p1234 db01 > db01.sql
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668408007668-e9ce190a-dd7a-402e-8004-932202121e58.png)

### 7.2.6 mysqlimport/source

mysqlimport 是客户端数据导入工具，用来导入mysqldump 加 -T 参数后导出的文本文件。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668408159698-be8cced0-ab88-41e3-a2c8-20325ac17ff7.png)

如果需要导入sql文件,可以使用mysql中的source 指令 :

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668408194070-30d0391c-0f50-44e6-96d7-310738864a27.png)







# 1.日志

## 1.1 错误日志

错误日志是 MySQL 中最重要的日志之一，它记录了当 mysqld 启动和停止时，以及服务器在运行过

程中发生任何严重错误时的相关信息。当数据库出现任何故障导致无法正常使用时，建议首先查看此日

志。

该日志是默认开启的，默认存放目录 /var/log/，默认的日志文件名为 mysqld.log 。查看日志

位置：

```
show variables like '%log_error%';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668339361136-22021348-5217-43e0-91cf-94a552bce2d1.png)

## 1.2 二进制日志

### 1.2.1 介绍

二进制日志（BINLOG）记录了所有的 DDL（数据定义语言）语句和 DML（数据操纵语言）语句，但

不包括数据查询（SELECT、SHOW）语句。

作用：①. 灾难时的数据恢复；②. MySQL的主从复制。在MySQL8版本中，默认二进制日志是开启着

的，涉及到的参数如下：

```
show variables like '%log_bin%';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668339416928-63f7c49f-eae3-4c59-9cf4-22c34670dcf0.png)

参数说明：

log_bin_basename：当前数据库服务器的binlog日志的基础名称(前缀)，具体的binlog文

件名需要再该basename的基础上加上编号(编号从000001开始)。

log_bin_index：binlog的索引文件，里面记录了当前服务器关联的binlog文件有哪些。

### 1.2.2 格式

MySQL服务器中提供了多种格式来记录二进制日志，具体格式及特点如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668339457203-750b56bc-6c1d-4d18-a2a0-ff901be8c963.png)

```
show variables like '%binlog_format%';
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668339473263-9a4e6d2a-54d6-4eed-8405-777de3220194.png)

如果我们需要配置二进制日志的格式，只需要在 /etc/my.cnf 中配置 binlog_format 参数即

可。

### 1.2.3 查看

由于日志是以二进制方式存储的，不能直接读取，需要通过二进制日志查询工具 mysqlbinlog 来查

看，具体语法：

```
mysqlbinlog [ 参数选项 ] logfilename
参数选项：
-d 指定数据库名称，只列出指定的数据库相关操作。
-o 忽略掉日志中的前n行命令。
-v 将行事件(数据变更)重构为SQL语句
-vv 将行事件(数据变更)重构为SQL语句，并输出注释信息
```

### 1.2.4 删除

对于比较繁忙的业务系统，每天生成的binlog数据巨大，如果长时间不清除，将会占用大量磁盘空

间。可以通过以下几种方式清理日志：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668339543280-882e71f5-1eab-407d-8cc0-d57cbcf2aa7b.png)

也可以在mysql的配置文件中配置二进制日志的过期时间，设置了之后，二进制日志过期会自动删除。

```
show variables like '%binlog_expire_logs_seconds%'; 
```

## 1.3 查询日志

查询日志中记录了客户端的所有操作语句，而二进制日志不包含查询数据的SQL语句。默认情况下，

查询日志是未开启的。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668339620094-7bd94158-85aa-44ef-822b-b1df3369031a.png)

如果需要开启查询日志，可以修改MySQL的配置文件 /etc/my.cnf 文件，添加如下内容：

```
#该选项用来开启查询日志 ， 可选值 ： 0 或者 1 ； 0 代表关闭， 1 代表开启
general_log=1
#设置日志的文件名 ， 如果没有指定， 默认的文件名为 host_name.log
general_log_file=mysql_query.log
```

开启了查询日志之后，在MySQL的数据存放目录，也就是 /var/lib/mysql/ 目录下就会出现

mysql_query.log 文件。之后所有的客户端的增删改查操作都会记录在该日志文件之中，长时间运

行后，该日志文件将会非常大。

## 1.4 慢查询日志

慢查询日志记录了所有执行时间超过参数 long_query_time 设置值并且扫描记录数不小于

min_examined_row_limit 的所有的SQL语句的日志，默认未开启。long_query_time 默认为

10 秒，最小为 0， 精度可以到微秒。

如果需要开启慢查询日志，需要在MySQL的配置文件 /etc/my.cnf 中配置如下参数：

```
#慢查询日志
slow_query_log=1
#执行时间参数
long_query_time=2
```

默认情况下，不会记录管理语句，也不会记录不使用索引进行查找的查询。可以使用

log_slow_admin_statements和 更改此行为 log_queries_not_using_indexes，如下所

述。

```
#记录执行较慢的管理语句
log_slow_admin_statements =1
#记录执行较慢的未使用索引的语句
log_queries_not_using_indexes = 1
```

上述所有的参数配置完成之后，都需要重新启动MySQL服务器才可以生效。

# 2.主从复制

## 2.1 概述

主从复制是指将主数据库的 DDL 和 DML 操作通过二进制日志传到从库服务器中，然后在从库上对这

些日志重新执行（也叫重做），从而使得从库和主库的数据保持同步。

MySQL支持一台主库同时向多台从库进行复制， 从库同时也可以作为其他从服务器的主库，实现链状

复制

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668339752708-9c137918-11ef-43ef-b5a4-ae9d2a2cb9af.png)

MySQL 复制的优点主要包含以下三个方面：

- 主库出现问题，可以快速切换到从库提供服务。
- 实现读写分离，降低主库的访问压力。
- 可以在从库中执行备份，以避免备份期间影响主库服务。

## 2.2 原理

MySQL主从复制的核心就是 二进制日志，具体的过程如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668339784210-18a7b64b-3318-475a-bcba-93a3917dd40f.png)

从上图来看，复制分成三步：

1. Master 主库在事务提交时，会把数据变更记录在二进制日志文件 Binlog 中。

2. 从库读取主库的二进制日志文件 Binlog ，写入到从库的中继日志 Relay Log 。

3. slave重做中继日志中的事件，将改变反映它自己的数据

## 2.3 搭建

### 2.3.1 准备

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668339820059-04bf7276-eee8-4744-98fc-3db5bbe3ed8f.png)

准备好两台服务器之后，在上述的两台服务器中分别安装好MySQL，并完成基础的初始化准备(安装、

密码配置等操作)工作。 其中：

- 192.168.200.200 作为主服务器master
- 192.168.200.201 作为从服务器slave

### 2.3.2 主库配置

1.修改配置文件 /etc/my.cnf

```
#mysql 服务ID，保证整个集群环境中唯一，取值范围：1 – 232-1，默认为1
server-id=1
#是否只读,1 代表只读, 0 代表读写
read-only=0
#忽略的数据, 指不需要同步的数据库
#binlog-ignore-db=mysql
#指定同步的数据库
#binlog-do-db=db01
```

2. 重启MySQL服务器

```
systemctl restart mysqld
```

3. 登录mysql，创建远程连接的账号，并授予主从复制权限

```
#创建itcast用户，并设置密码，该用户可在任意主机连接该MySQL服务
CREATE USER 'itcast'@'%' IDENTIFIED WITH mysql_native_password BY 'Root@123456';
#为 'itcast'@'%' 用户分配主从复制权限
GRANT REPLICATION SLAVE ON *.* TO 'itcast'@'%';
```

4. 通过指令，查看二进制日志坐标

```
show master status ;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668339979346-2b93a8cf-edaa-4c12-af82-80b3a2564536.png)

字段含义说明：

file : 从哪个日志文件开始推送日志文件

position ： 从哪个位置开始推送日志

binlog_ignore_db : 指定不需要同步的数据库

### 2.3.3 从库配置

1. 修改配置文件 /etc/my.cnf

```
#mysql 服务ID，保证整个集群环境中唯一，取值范围：1 – 2^32-1，和主库不一样即可
server-id=2
#是否只读,1 代表只读, 0 代表读写
read-only=1
```

2. 重新启动MySQL服务

```
systemctl restart mysqld
```

3. 登录mysql，设置主库配置

```
CHANGE REPLICATION SOURCE TO SOURCE_HOST='192.168.200.200', SOURCE_USER='itcast',
SOURCE_PASSWORD='Root@123456', SOURCE_LOG_FILE='binlog.000004',
SOURCE_LOG_POS=663;
```

上述是8.0.23中的语法。如果mysql是 8.0.23 之前的版本，执行如下SQL：

```
CHANGE MASTER TO MASTER_HOST='192.168.200.200', MASTER_USER='itcast',
MASTER_PASSWORD='Root@123456', MASTER_LOG_FILE='binlog.000004',
MASTER_LOG_POS=663;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668340101469-5aa958c9-23bc-48cd-9b03-2e86f03343d1.png)

4. 开启同步操作

```
start replica ; #8.0.22之后
start slave ; #8.0.22之前
```

5. 查看主从同步状态

```
show replica status ; #8.0.22之后
show slave status ; #8.0.22之前
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668340143714-1b394d5a-2c82-46da-bdd0-b23435fde2c9.png)

### 2.3.4 测试

1. 在主库 192.168.200.200 上创建数据库、表，并插入数据

```
create database db01;
use db01;
create table tb_user(
id int(11) primary key not null auto_increment,
name varchar(50) not null,
sex varchar(1)
)engine=innodb default charset=utf8mb4;
insert into tb_user(id,name,sex) values(null,'Tom', '1'),(null,'Trigger','0'),
(null,'Dawn','1');
```

2. 在从库 192.168.200.201 中查询数据，验证主从是否同步

# 3.分库分表

## 3.1 介绍

### 3.1.1 问题分析

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668340293139-09df6b7a-02e9-4eb3-b669-6381a345cba3.png)

随着互联网及移动互联网的发展，应用系统的数据量也是成指数式增长，若采用单数据库进行数据存

储，存在以下性能瓶颈：

1. IO瓶颈：热点数据太多，数据库缓存不足，产生大量磁盘IO，效率较低。 请求数据太多，带宽

不够，网络IO瓶颈。

2. CPU瓶颈：排序、分组、连接查询、聚合统计等SQL会耗费大量的CPU资源，请求数太多，CPU出

现瓶颈

为了解决上述问题，我们需要对数据库进行分库分表处理。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668340326309-6f4102a3-15ef-4404-a556-b28033a95a7c.png)

分库分表的中心思想都是将数据分散存储，使得单一数据库/表的数据量变小来缓解单一数据库的性能

问题，从而达到提升数据库性能的目的。

### 3.1.2 拆分策略

分库分表的形式，主要是两种：垂直拆分和水平拆分。而拆分的粒度，一般又分为分库和分表，所以组

成的拆分策略最终如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668340363345-31391687-5739-4f07-86df-7623110bfcbf.png)

### 3.1.3 垂直拆分

1. 垂直分库

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668340403181-2dfe1a27-33e9-45e5-9d7b-53158d46c082.png)

垂直分库：以表为依据，根据业务将不同表拆分到不同库中。

特点：

每个库的表结构都不一样。

每个库的数据也不一样。

所有库的并集是全量数据。

2. 垂直分表

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668340439397-478687f6-530a-42ee-8ff6-0fc4196f1ef7.png)

垂直分表：以字段为依据，根据字段属性将不同字段拆分到不同表中。

特点：

每个表的结构都不一样。

每个表的数据也不一样，一般通过一列（主键/外键）关联。

所有表的并集是全量数据

### 3.1.4 水平拆分

1. 水平分库

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668340509907-618458f8-4216-43f7-8351-5e9171a28d89.png)

水平分库：以字段为依据，按照一定策略，将一个库的数据拆分到多个库中。

特点：

每个库的表结构都一样。

每个库的数据都不一样。

所有库的并集是全量数据。

2. 水平分表

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668340544155-f858482d-2b71-4d21-9956-6cdddea51ce8.png)

水平分表：以字段为依据，按照一定策略，将一个表的数据拆分到多个表中。

特点：

每个表的表结构都一样。

每个表的数据都不一样。

所有表的并集是全量数据。

### 3.1.5 实现技术

shardingJDBC：基于AOP原理，在应用程序中对本地执行的SQL进行拦截，解析、改写、路由处

理。需要自行编码配置实现，只支持java语言，性能较高。

MyCat：数据库分库分表中间件，不用调整代码即可实现分库分表，支持多种语言，性能不及前

者。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668340636111-c2d28a51-3e7d-41cb-81a0-355d753561e9.png)

本次课程，我们选择了是MyCat数据库中间件，通过MyCat中间件来完成分库分表操作。

## 3.2 MyCat

### 3.2.1 介绍

Mycat是开源的、活跃的、基于Java语言编写的MySQL数据库中间件。可以像使用mysql一样来使用

mycat，对于开发人员来说根本感觉不到mycat的存在。

开发人员只需要连接MyCat即可，而具体底层用到几台数据库，每一台数据库服务器里面存储了什么数

据，都无需关心。 具体的分库分表的策略，只需要在MyCat中配置即可。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668340694786-bdfd57ac-d9b1-4684-b6d6-5a06c58b385a.png)

优势：

性能可靠稳定

强大的技术团队

体系完善

社区活跃

### 3.2.2 下载安装

下载地址：**http://dl.mycat.org.cn/**

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668340740554-1240709d-3b01-47e7-92d5-2e55c0c9331a.png)

Mycat是采用java语言开发的开源的数据库中间件，支持Windows和Linux运行环境，下面介绍

MyCat的Linux中的环境搭建。我们需要在准备好的服务器中安装如下软件。

MySQL

JDK

Mycat

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668340780735-c168a526-3e4b-48dc-98b6-488ad48faff6.png)





在 MySQL 中，EXPLAIN 是一个强大的工具，用于分析查询的执行计划，帮助开发者理解 MySQL 优化器如何执行 SELECT 查询，从而优化查询性能和索引使用。以下是关于 EXPLAIN SELECT ... 的详细使用说明。

---

### 1. **什么是 EXPLAIN**

EXPLAIN 是一个 SQL 语句前缀，用于展示 MySQL 如何执行一个查询，包括：

- 访问表的顺序
- 使用哪些索引
- 扫描的行数
- 查询的执行方式（如全表扫描、索引扫描等）
- 优化的可能性

通过分析 EXPLAIN 的输出，可以判断查询是否高效，是否正确使用了索引，以及如何改进。

---

### 2. **基本语法**

sql

Copy

`EXPLAIN SELECT ... FROM ... WHERE ... [other clauses];`

- 在 SELECT 语句前添加 EXPLAIN，MySQL 会返回查询的执行计划，而不实际执行查询。
- 适用于 SELECT、DELETE、INSERT、UPDATE 和 REPLACE 语句，但最常用于 SELECT。

**示例**：

sql

Copy

`EXPLAIN SELECT * FROM employees WHERE department_id = 10;`

---

### 3. **EXPLAIN 输出格式**

MySQL 提供以下几种输出格式：

- **默认格式（表格格式）**：MySQL 5.7 及以上默认输出为表格，适合人类阅读。
- **JSON 格式**：提供更详细的信息，使用 EXPLAIN FORMAT=JSON。
- **TREE 格式**：MySQL 8.0+ 支持，显示树形执行计划，使用 EXPLAIN FORMAT=TREE。
- **传统格式**：早期版本的表格输出，结构类似默认格式。

**选择格式**：

sql

Copy

`EXPLAIN FORMAT=JSON SELECT * FROM employees WHERE department_id = 10; EXPLAIN FORMAT=TREE SELECT * FROM employees WHERE department_id = 10;`

---

### 4. **EXPLAIN 输出字段（表格格式）**

EXPLAIN 的输出包含多个列，常用字段说明如下：

|字段名|含义|
|---|---|
|id|查询的标识符，标识子查询或联接的执行顺序。值越大，优先级越高。|
|select_type|查询类型，如 SIMPLE（简单查询）、SUBQUERY（子查询）、JOIN 等。|
|table|查询涉及的表名。|
|partitions|涉及的分区（如果表分区）。|
|type|访问类型，反映查询效率（如 ALL、INDEX、RANGE、REF 等）。|
|possible_keys|优化器可能使用的索引列表。|
|key|实际使用的索引（若为 NULL，则未使用索引）。|
|key_len|使用索引的长度（字节），用于判断复合索引的使用情况。|
|ref|与索引比较的列或常量。|
|rows|预计扫描的行数（越少越好）。|
|filtered|过滤后的行百分比（100% 表示无过滤）。|
|Extra|附加信息，如是否使用临时表、文件排序等。|

---

### 5. **常用访问类型（type 字段）**

type 字段反映查询的访问方式，从高效到低效排序如下：

- **system**：表只有一行数据（最高效，常见于系统表）。
- **const**：通过主键或唯一索引查找单行数据。
- **eq_ref**：在联接中，通过主键或唯一索引精确匹配一行。
- **ref**：通过非唯一索引查找，匹配多行。
- **range**：通过索引进行范围查询（如 >、<、BETWEEN）。
- **index**：扫描整个索引树（比全表扫描稍好）。
- **ALL**：全表扫描（最低效，需优化）。

---

### 6. **使用步骤**

1. **编写查询**：确定需要优化的 SELECT 查询。
2. **运行 EXPLAIN**：在查询前加 EXPLAIN 或 EXPLAIN FORMAT=...。
3. **分析输出**：
    - 检查 type 是否为 ALL 或 INDEX，表示可能需要添加索引。
    - 查看 key 和 possible_keys，确认是否使用了预期索引。
    - 关注 rows 和 filtered，评估扫描行数和过滤效率。
    - 检查 Extra 字段，是否有 Using filesort（文件排序）或 Using temporary（临时表），这些通常表示性能问题。
4. **优化查询**：
    - 添加或调整索引。
    - 重写查询（如简化条件、避免函数操作）。
    - 使用 FORCE INDEX 或 USE INDEX 干预优化器选择。

---

### 7. **示例分析**

假设有以下表结构和查询：

sql

Copy

`CREATE TABLE employees ( id INT PRIMARY KEY, last_name VARCHAR(50), department_id INT, salary DECIMAL(10,2), INDEX idx_dept (department_id), INDEX idx_name_dept (last_name, department_id) ); SELECT * FROM employees WHERE department_id = 10 AND last_name = 'Smith' ORDER BY salary;`

**运行 EXPLAIN**：

sql

Copy

`EXPLAIN SELECT * FROM employees WHERE department_id = 10 AND last_name = 'Smith' ORDER BY salary;`

**假设输出**（表格格式）：

|id|select_type|table|type|possible_keys|key|key_len|ref|rows|Extra|
|---|---|---|---|---|---|---|---|---|---|
|1|SIMPLE|employees|ref|idx_dept, idx_name_dept|idx_name_dept|152|const|10|Using where; Using filesort|

**分析**：

- **type: ref**：使用了非唯一索引，效率较高。
- **possible_keys: idx_dept, idx_name_dept**：优化器考虑了两个索引。
- **key: idx_name_dept**：实际使用了复合索引 idx_name_dept，因为查询同时涉及 last_name 和 department_id。
- **key_len: 152**：表示使用了复合索引的全部字段（假设 last_name 是 VARCHAR(50)，UTF8 编码，50*3+2+4=152 字节）。
- **rows: 10**：预计扫描 10 行，数量较少，效率较高。
- **Extra: Using filesort**：表示 ORDER BY salary 触发了文件排序，因为 salary 不在索引中。

**优化建议**：

- 为支持 ORDER BY salary，可创建新复合索引：
    
    sql
    
    Copy
    
    `CREATE INDEX idx_name_dept_salary ON employees (last_name, department_id, salary);`
    
- 重新运行 EXPLAIN，检查 Extra 是否移除 Using filesort。

---

### 8. **高级用法**

- **JSON 格式**：
    
    sql
    
    Copy
    
    `EXPLAIN FORMAT=JSON SELECT * FROM employees WHERE department_id = 10;`
    
    - 提供更详细的执行计划，包括成本估算（cost_info）和子查询信息。
    - 示例输出片段：
        
        json
        
        Copy
        
        `{ "query_block": { "select_id": 1, "cost_info": { "query_cost": "10.00" }, "table": { "table_name": "employees", ... } } }`
        
- **TREE 格式（MySQL 8.0+）**：
    
    sql
    
    Copy
    
    `EXPLAIN FORMAT=TREE SELECT * FROM employees WHERE department_id = 10;`
    
    - 显示树形结构，便于理解复杂查询的执行顺序。
- **分析多表联接**：
    
    sql
    
    Copy
    
    `EXPLAIN SELECT e.last_name, d.department_name FROM employees e JOIN departments d ON e.department_id = d.department_id WHERE e.salary > 50000;`
    
    - 检查联接顺序（id 和 table 字段）、索引使用和行数。
- **强制索引**： 如果优化器未选择预期索引，可使用：
    
    sql
    
    Copy
    
    `EXPLAIN SELECT * FROM employees FORCE INDEX (idx_dept) WHERE department_id = 10;`
    
    - FORCE INDEX 强制使用指定索引，USE INDEX 建议使用。

---

### 9. **注意事项**

- **存储引擎差异**：
    - InnoDB 和 MyISAM 的执行计划可能不同，需结合引擎特性分析。
    - InnoDB 的聚簇索引可能影响 key_len 计算。
- **统计信息**：
    - 确保表统计信息最新，使用 ANALYZE TABLE table_name; 更新。
    - 过时的统计信息可能导致优化器选择错误的索引。
- **限制**：
    - EXPLAIN 不执行查询，成本估算可能与实际性能略有偏差。
    - 某些复杂查询（如存储过程）可能无法完整分析。
- **性能影响**：
    - 运行 EXPLAIN 本身开销低，但频繁分析复杂查询可能影响开发效率。
- **版本差异**：
    - MySQL 8.0+ 支持 TREE 格式和更详细的成本分析。
    - 老版本（如 5.5）输出较简单，缺少 filtered 等字段。

---

### 10. **优化建议**

- **避免全表扫描**：如果 type 是 ALL，尝试添加索引或优化 WHERE 条件。
- **减少扫描行数**：关注 rows 字段，尽量降低扫描行数。
- **消除文件排序**：如果 Extra 包含 Using filesort，调整索引包含排序列。
- **覆盖索引**：设计索引使查询只需访问索引（如选择 key 覆盖所有查询列）。
- **定期维护**：更新表统计信息，检查索引是否冗余或失效。
- **验证效果**：优化后重新运行 EXPLAIN，对比 rows 和 type 变化。

---

### 11. **综合示例**

**场景**：优化以下查询：

sql

Copy

`SELECT last_name, department_id FROM employees WHERE last_name LIKE 'S%' AND salary > 50000 ORDER BY department_id;`

**步骤**：

1. **初始 EXPLAIN**：
    
    sql
    
    Copy
    
    `EXPLAIN SELECT last_name, department_id FROM employees WHERE last_name LIKE 'S%' AND salary > 50000 ORDER BY department_id;`
    
    假设输出显示 type: ALL，Extra: Using where; Using filesort。
2. **优化**：
    - 创建复合索引：
        
        sql
        
        Copy
        
        `CREATE INDEX idx_name_salary_dept ON employees (last_name, salary, department_id);`
        
    - 索引覆盖 last_name（支持 LIKE 'S%'）、salary（支持范围查询）和 department_id（支持排序）。
3. **再次 EXPLAIN**：
    
    sql
    
    Copy
    
    `EXPLAIN SELECT last_name, department_id FROM employees WHERE last_name LIKE 'S%' AND salary > 50000 ORDER BY department_id;`
    
    检查是否变为 type: range，key: idx_name_salary_dept，Extra 无 Using filesort。