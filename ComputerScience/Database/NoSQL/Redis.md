# 概述
---





## 简介
---

## 需求
---

## 对比
---

# 开始
---

## 安装
---

## 使用
---

# 概念
---

# 配置
---

# 附录
---
> [!website]
    > https://redis.io/

> [!code]
    > https://github.com/redis/redis

> [!video]
    > Content

> [!doc]
    > Content




# Redis入门篇

学习Redis的常见命 <u>令和客户端使用</u>

## 1.初识Redis

**Redis 是一种键值型的 NoSql 数据库**，这里有两个关键字：

- 键值型
- NoSql

其中键值型，是指 Redis 中存储的数据都是以 key、value 对的形式存储，而 value 的形式多种多样，可以是字符串、数值、甚至 json：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666512309591-3593b43b-a75c-45e3-a958-d54c4c7580a3.png)

  
而 NoSql 则是相对于传统关系型数据库而言，有很大差异的一种数据库。

### 1.1 认识NoSQL

NoSql 可以翻译做 Not Only Sql（不仅仅是SQL），或者是 No Sql（非Sql的）数据库。是相对于传统关系型数据库而言，有很大差异的一种特殊的数据库，因此也称之为非关系型数据库。

#### 1.1.1结构化与非结构化

传统关系型数据库是结构化数据，每一张表都有严格的约束信息：字段名、字段数据类型、字段约束等等信息，插入的数据必须遵守这些约束：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666512453960-7a0324e7-e37a-4fed-a3d0-1c409b7a47cf.png)

而 NoSql 则对数据库格式没有严格约束，往往形式松散，自由。

可以是键值型：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666512475008-0d04464c-21ed-4be0-96d9-c1140d887e21.png)

也可以是文档型：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666512490614-8d102b08-a486-46ee-9674-e6ca27f6aba8.png)

甚至可以是图格式：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666512504530-c0dcac3e-1808-419f-b84a-75de58d7c721.png)

#### 1.1.2 关联和非关联

传统数据库的表与表之间往往存在关联，例如外键：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666512554538-dc2f42fc-b252-4fda-acd4-579e8e3dac02.png)

而非关系型数据库不存在关联关系，要维护关系要么靠代码中的**业务逻辑**，要么靠**数据之间的耦合**：

```
{
  id: 1,
  name: "张三",
  orders: [
    {
       id: 1,
       item: {
	              id: 10, 
      					title: "荣耀6", 
      					price: 4999
       }
    },
    {
       id: 2,
       item: {
	 							id: 20, 
      					title: "小米11", 
      					price: 3999
       }
    }
  ]
}
```

此处要维护“张三”的订单与商品“荣耀”和“小米11”的关系，不得不冗余的将这两个商品保存在张三的订单文档中，不够优雅。还是建议用业务来维护关联关系。

#### 1.1.3 查询方式

传统关系型数据库会基于 Sql 语句做查询，语法有统一标准；

而不同的非关系数据库查询语法差异极大，五花八门各种各样。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666512661806-6b970e83-663a-4984-93b9-13c070401b81.png)

#### 1.1.4 事务

传统关系型数据库能满足事务ACID的原则。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666512709688-50515eff-e556-436e-a68d-73557e90e00b.png)

而非关系型数据库往往不支持事务，或者不能严格保证ACID的特性，只能实现基本的一致性。

#### 1.1.5 总结

除了上述四点以外，在存储方式、扩展性、查询性能上关系型与非关系型也都有着显著差异，总结如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666512746538-72448100-75d1-4452-b224-c2609a925e1f.png)

- 存储方式

- 关系型数据库基于磁盘进行存储，会有大量的磁盘IO，对性能有一定影响
- 非关系型数据库，他们的操作更多的是依赖于内存来操作，内存的读写速度会非常快，性能自然会好一些

- 扩展性

- 关系型数据库集群模式一般是主从，主从数据一致，起到数据备份的作用，称为垂直扩展。
- 非关系型数据库可以将数据拆分，存储在不同机器上，可以保存海量数据，解决内存大小有限的问题。称为水平扩展。
- 关系型数据库因为表之间存在关联关系，如果做水平扩展会给数据查询带来很多麻烦

### 1.2 认识Redis

Redis诞生于2009年全称是Remote Dictionary Server 远程词典服务器，是一个基于内存的键值型NoSQL数据库。

特征：

- 键值（key-value）型，value支持多种不同数据结构，功能丰富
- 单线程，每个命令具备原子性
- 低延迟，速度快（基于内存、IO多路复用（NIO）、良好的编码）。
- 支持数据持久化
- 支持主从集群、分片集群
- 支持多语言客户端

作者：Antirez

Redis的官方网站地址：[Redis](https://redis.io/)

### 1.3 安装Redis

Redis 没有提供Windows版本，Linux 版本使用CentOS7。

#### 1.3.1 依赖库

Redis是基于C语言编写的，因此首先需要安装 Redis 所需要的 gcc 依赖：

```
yum install -y gcc tcl
```

#### 1.3.2 安装Redis

将文件上传到 /usr/local/src 目录，解压缩：

```
tar -xzf redis-6.2.6.tar.gz
```

解压后，进入redis目录：

```
cd redis-6.2.6
```

运行编译安装：

```
make && make install
```

如果没有出错，应该就安装成功了。

默认的安装路径是在 `/usr/local/bin`目录下：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1680172425064-565ba13c-61dd-4b06-8cfb-add756f6570a.png)

该目录已经默认配置到环境变量，因此可以在任意目录下运行这些命令。

- `redis-cli`：是 redis 提供的命令行客户端
- `redis-server`：是 redis 的服务端启动脚本
- `redis-sentinel`：是 redis 的哨兵启动脚本

#### 1.3.3 启动

redis 的启动方式有很多种，例如：

1. 默认启动

安装完成后，在任意目录输入`redis-server`命令即可启动Redis：

```
redis-server
```

这种启动属于前台启动，会阻塞整个会话窗口，窗口关闭或者按下 `CTRL + C` 则 Redis 停止。不推荐使用。

2. 指定配置启动

如果要让 Redis 以后台方式启动，则必须修改 Redis 配置文件，就在我们之前解压的 redis 安装包下（/usr/local/src/redis-6.2.6），名字叫 redis.conf：

我们先将这个配置文件备份一份：

```
cp redis.conf redis.conf.bck
```

然后修改redis.conf文件中的一些配置：

```
# 监听的地址，默认是127.0.0.1，会导致只能在本地访问。修改为0.0.0.0则可以在任意IP访问，生产环境不要设置为0.0.0.0
bind 0.0.0.0
# 守护进程，修改为yes后即可后台运行
daemonize yes 
# 密码，设置后访问Redis必须输入密码
requirepass root
```

Redis的其它常见配置：

```
# 监听的端口
port 6379
# 工作目录，默认是当前目录，也就是运行redis-server时的命令，日志、持久化等文件会保存在这个目录
dir .
# 数据库数量，设置为1，代表只使用1个库，默认有16个库，编号0~15
databases 1
# 设置redis能够使用的最大内存
maxmemory 512mb
# 日志文件，默认为空，不记录日志，可以指定日志文件名
logfile "redis.log"
```

启动Redis：

```
# 进入redis安装目录 
cd /usr/local/src/redis-6.2.6
# 启动并指定配置文件
redis-server redis.conf
```

停止服务：

```
# 利用redis-cli来执行 shutdown 命令，即可停止 Redis 服务，
# 因为之前配置了密码，因此需要通过 -u 来指定密码
redis-cli -u root shutdown

# 或者通过 kill -9 直接杀死进程
```

3. 开机自启

我们也可以通过配置来实现开机自启。

首先，新建一个系统服务文件：

```
vi /etc/systemd/system/redis.service
```

内容如下：

```
[Unit]
Description=redis-server
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/bin/redis-server /usr/local/src/redis-6.2.6/redis.conf
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

然后重载系统服务：

```
systemctl daemon-reload
```

现在，我们可以用下面这组命令来操作redis了：

```
# 启动
systemctl start redis
# 停止
systemctl stop redis
# 重启
systemctl restart redis
# 查看状态
systemctl status redis
```

执行下面的命令，可以让redis开机自启：

```
systemctl enable redis
```

### 1.4 Redis桌面客户端

安装完成Redis，我们就可以操作Redis，实现数据的CRUD了。这需要用到Redis客户端，包括：

命令行客户端

Redis安装完成后就自带了命令行客户端：redis-cli，使用方式如下：

```
redis-cli [options] [commonds]
```

其中常见的options有：

- -h 127.0.0.1：指定要连接的redis节点的IP地址，默认是127.0.0.1
- -p 6379：指定要连接的redis节点的端口，默认是6379
- -a 123321：指定redis的访问密码

其中的 commonds 就是Redis的操作命令，例如：

- ping：与 redis 服务端做心跳测试，服务端正常会返回 pong

不指定密码时，会进入 redis-cli 的交互控制台，可以通过 auth 命令指定密码，与 redis 服务器正确连接

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1680506604412-43f41582-c79b-4348-b6f2-0c9f496330c3.png)

图形化桌面客户端

[https://github.com/qishibo/AnotherRedisDesktopManager](https://github.com/qishibo/AnotherRedisDesktopManager)

Redis默认有16个仓库，编号从0至15. 通过配置文件可以设置仓库数量，但是不超过16，并且不能自定义仓库名称。

如果是基于redis-cli连接Redis服务，可以通过select命令来选择数据库：

```
# 选择 0号库
select 0
```

编程客户端

我们使用 Redis 的Java 客户端

## 2.Redis常用命令

Redis是典型的key-value数据库，key一般是字符串，而value包含很多不同的数据类型：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666513823462-96eedd9d-30db-4b21-8658-d12a4debe753.png)

Redis为了方便我们学习，将操作不同数据类型的命令也做了分组，在官网（ [https://redis.io/commands](https://redis.io/commands)）可以查看到不同的命令：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1680507615616-e8940bbb-9814-4114-91e2-1c62f9ddadcd.png)

不同类型的命令称为一个 group，我们也可以通过 help 命令来查看各种不同 group 的命令：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1680507807490-e11991cd-22d2-4e10-bf86-0881415d2717.png)

接下来，我们就学习常见的五种基本数据类型的相关命令。

### 2.1 Redis通用命令

通用指令是不分数据类型的，都可以使用的指令，常见的有：

- `KEYS`：查看符合模板的所有 key
- `DEL`：删除一个指定的 key
- `EXISTS`：判断 key 是否存在
- `EXPIRE`：给一个 key 设置有效期，有效期到期时该 key 会被自动删除
- `TTL`：查看一个KEY的剩余有效期

通过help [command] 可以查看一个命令的具体用法，例如：

```
# 查看keys命令的帮助信息：
127.0.0.1:6379> help keys

KEYS pattern
summary: Find all keys matching the given pattern
since: 1.0.0
group: generic
```

### 2.2 String类型

String类型，也就是字符串类型，是Redis中最简单的存储类型。

其value是字符串，不过根据字符串的格式不同，又可以分为3类：

- `string`：普通字符串
- `int`：整数类型，可以做自增、自减操作
- `float`：浮点类型，可以做自增、自减操作

不管是哪种格式，底层都是字节数组形式存储，只不过是编码方式不同。字符串类型的最大空间不能超过512m.

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666514093069-5b789ed6-b4d0-459a-bb64-5c84b3041fd7.png)

#### 2.2.1 String常用命令

- `SET`：添加或者修改已经存在的一个String类型的键值对
- `GET`：根据key获取String类型的value
- `MSET`：批量添加多个String类型的键值对
- `MGET`：根据多个key获取多个String类型的value
- `INCR`：让一个整型的key自增1
- `INCRBY`:让一个整型的key自增并指定步长，例如：incrby num 2 让num值自增2
- `INCRBYFLOAT`：让一个浮点类型的数字自增并指定步长
- `SETNX`：添加一个String类型的键值对，前提是这个key不存在，否则不执行
- `SETEX`：添加一个String类型的键值对，并且指定有效期

#### 2.2.2 Key结构

Redis 没有类似 MySQL 中的 Table 的概念，我们该如何区分不同类型的 key 呢？

例如，需要存储用户、商品信息到redis，有一个用户id是1，有一个商品id恰好也是1，此时如果使用id作为key，那就会冲突了，该怎么办？

我们可以通过给key添加前缀加以区分，不过这个前缀不是随便加的，有一定的规范：

Redis的key允许有多个单词形成层级结构，多个单词之间用':'隔开，格式如下：

```
项目名:业务名:类型:id
```

这个格式并非固定，也可以根据自己的需求来删除或添加词条。这样以来，我们就可以把不同类型的数据区分开了。从而避免了key的冲突问题。

例如我们的项目名称叫 heima，有user和product两种不同类型的数据，我们可以这样定义key：

- user相关的key：heima:user:1
- product相关的key：heima:product:1

如果Value是一个Java对象，例如一个User对象，则可以将对象序列化为JSON字符串后存储：

|   |   |
|---|---|
|KEY|VALUE|
|heima:user:1|{"id":1, "name": "Jack", "age": 21}|
|heima:product:1|{"id":1, "name": "小米11", "price": 4999}|

并且，在Redis的桌面客户端中，还会以相同前缀作为层级结构，让数据看起来层次分明，关系清晰：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666514350216-7cefed5d-f437-42b4-8d70-ea92f4317592.png)

### 2.3 Hash类型

Hash类型，也叫散列，其value是一个无序字典，类似于Java中的HashMap结构。

String结构是将对象序列化为JSON字符串后存储，当需要修改对象某个字段时很不方便：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666514384151-ace194ed-ac78-4fa5-82c8-e210169e3b76.png)

Hash结构可以将对象中的每个字段独立存储，可以针对单个字段做CRUD：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666514404337-8f1fd844-af05-46f0-ac53-0ab7cb68fc29.png)

Hash的常见命令有（在String类型命令前加H）：

- `HSET key field value`：添加或者修改hash类型key的field的值
- `HGET key field`：获取一个hash类型key的field的值
- `HMSET`：批量添加多个hash类型key的field的值
- `HMGET`：批量获取多个hash类型key的field的值
- `HGETALL`：获取一个hash类型的key中的所有的field和value
- `HKEYS`：获取一个hash类型的key中的所有的field
- `HINCRBY`:让一个hash类型key的字段值自增并指定步长
- `HSETNX`：添加一个hash类型的key的field值，前提是这个field不存在，否则不执行

### 2.4 List类型

Redis中的List类型与Java中的LinkedList类似，可以看做是一个双向链表结构。既可以支持正向检索和也可以支持反向检索。

特征也与LinkedList类似：

- 有序
- 元素可以重复
- 插入和删除快
- 查询速度一般

常用来存储一个有序数据，例如：朋友圈点赞列表，评论列表等。

List的常见命令有：

- `LPUSH key element ...` ：向列表左侧插入一个或多个元素
- `LPOP key`：移除并返回列表左侧的第一个元素，没有则返回nil
- `RPUSH key element ...` ：向列表右侧插入一个或多个元素
- `RPOP key`：移除并返回列表右侧的第一个元素
- `LRANGE key star end`：返回一段角标范围内的所有元素
- `BLPOP`和`BRPOP`：与LPOP和RPOP类似，只不过在没有元素时等待指定时间，而不是直接返回nil

### 2.5 Set类型

Redis的Set结构与Java中的HashSet类似，可以看做是一个value为null的HashMap。因为也是一个hash表，因此具备与HashSet类似的特征：

- 无序
- 元素不可重复
- 查找快
- 支持交集、并集、差集等功能

Set的常见命令有：

- `SADD key member ...` ：向set中添加一个或多个元素
- `SREM key member ...` : 移除set中的指定元素
- `SCARD key`： 返回set中元素的个数
- `SISMEMBER key member`：判断一个元素是否存在于set中
- `SMEMBERS`：获取set中的所有元素
- `SINTER key1 key2 ...` ：求key1与key2的交集

例如两个集合：s1和s2:

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666514600711-5c831205-0136-4af6-9bb0-05a46d4cbf49.png)

求交集：SINTER s1 s2

求s1与s2的不同：SDIFF s1 s2

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666514617025-94fb5b32-f117-45cc-bcef-3fbf76b49c69.png)

练习：

1. 将下列数据用Redis的Set集合来存储：

- 张三的好友有：李四、王五、赵六
- 李四的好友有：王五、麻子、二狗

1. 利用Set的命令实现下列功能：

- 计算张三的好友有几人
- 计算张三和李四有哪些共同好友
- 查询哪些人是张三的好友却不是李四的好友
- 查询张三和李四的好友总共有哪些人
- 判断李四是否是张三的好友
- 判断张三是否是李四的好友
- 将李四从张三的好友列表中移除

### 2.6 SortedSet

Redis的SortedSet是一个可排序的set集合，与Java中的TreeSet有些类似，但底层数据结构却差别很大。SortedSet中的每一个元素都带有一个score属性，可以基于score属性对元素排序，底层的实现是一个跳表（SkipList）加 hash表。

SortedSet具备下列特性：

- 可排序
- 元素不重复
- 查询速度快

因为SortedSet的可排序特性，经常被用来实现排行榜这样的功能。

SortedSet的常见命令有：

- `ZADD key score member`：添加一个或多个元素到sorted set ，如果已经存在则更新其score值
- `ZREM key member`：删除sorted set中的一个指定元素
- `ZSCORE key member` : 获取sorted set中的指定元素的score值
- `ZRANK key member`：获取sorted set 中的指定元素的排名
- `ZCARD key`：获取sorted set中的元素个数
- `ZCOUNT key min max`：统计score值在给定范围内的所有元素的个数
- `ZINCRBY key increment member`：让sorted set中的指定元素自增，步长为指定的increment值
- `ZRANGE key min max`：按照score排序后，获取指定排名范围内的元素
- `ZRANGEBYSCORE key min max`：按照score排序后，获取指定score范围内的元素
- `ZDIFF、ZINTER、ZUNION`：求差集、交集、并集

注意：所有的排名默认都是升序，如果要降序则在命令的Z后面添加REV即可，例如：

- 升序获取sorted set 中的指定元素的排名：ZRANK key member
- 降序获取sorted set 中的指定元素的排名：ZREVRANK key memeber

练习题：

将班级的下列学生得分存入Redis的SortedSet中：

Jack 85, Lucy 89, Rose 82, Tom 95, Jerry 78, Amy 92, Miles 76

并实现下列功能：

- 删除Tom同学
- 获取Amy同学的分数
- 获取Rose同学的排名
- 查询80分以下有几个学生
- 给Amy同学加2分
- 查出成绩前3名的同学
- 查出成绩80分以下的所有同学

## 3.Redis的Java客户端

使用 Java 语言来操作 Redis，官网：[Get started using Redis clients](https://redis.io/docs/clients/)

Redis 可以各种语言操作，而 Redis 的 Java 客户端也有很多个，主要的有 `Jedis` 、`Lettuce` 和 `Redisson`。

- Jedis 和 Lettuce：这两个主要是提供了 Redis 命令对应的 API，方便我们操作 Redis，而 SpringDataRedis 又对这两种做了抽象和封装，因此后期会直接以 SpringDataRedis 来学习。

- Redisson：是在 Redis 基础上实现了分布式的可伸缩的 java 数据结构，例如 Map、Queue 等，而且支持跨进程的同步机制：Lock、Semaphore等待，比较适合用来实现特殊的功能需求。

### 3.1 Jedis客户端

Jedis的官网地址： [GitHub - redis/jedis: Redis Java client designed for performance and ease of use.](https://github.com/redis/jedis)

#### 3.1.1 快速入门

1. 引入依赖

```
<!--jedis-->
<dependency>
  <groupId>redis.clients</groupId>
  <artifactId>jedis</artifactId>
  <version>3.7.0</version>
</dependency>
<!--单元测试 junit5-->
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter</artifactId>
  <version>5.7.0</version>
  <scope>test</scope>
</dependency>
```

2. 建立连接

```
private Jedis jedis;

@BeforeEach
void setUp() {
    // 1.建立连接
    // jedis = new Jedis("192.168.150.101", 6379);
    jedis = JedisConnectionFactory.getJedis();
    // 2.设置密码
    jedis.auth("123321");
    // 3.选择库
    jedis.select(0);
    
}
```

3. 测试

```
@Test
void testString() {
    // 存入数据
    String result = jedis.set("name", "虎哥");
    System.out.println("result = " + result);
    // 获取数据
    String name = jedis.get("name");
    System.out.println("name = " + name);
}

@Test
void testHash() {
    // 插入hash数据
    jedis.hset("user:1", "name", "Jack");
    jedis.hset("user:1", "age", "21");

    // 获取
    Map<String, String> map = jedis.hgetAll("user:1");
    System.out.println(map);
}
```

4. 释放资源

```
@AfterEach
void tearDown() {
    if (jedis != null) {
        jedis.close();
    }
}
```

#### 3.1.2 连接池

Jedis 本身是线程不安全的，并且频繁的创建和销毁连接会有性能损耗，因此使用 Jedis 连接池代替 Jedis 的直连方式。

```
package com.heima.jedis.util;

import redis.clients.jedis.*;

public class JedisConnectionFactory {

    private static JedisPool jedisPool;

    static {
        // 配置连接池
        JedisPoolConfig poolConfig = new JedisPoolConfig();
        poolConfig.setMaxTotal(8);
        poolConfig.setMaxIdle(8);
        poolConfig.setMinIdle(0);
        poolConfig.setMaxWaitMillis(1000);
        // 创建连接池对象，参数：连接池配置、服务端ip、服务端端口、超时时间、密码
        jedisPool = new JedisPool(poolConfig, "192.168.150.101", 6379, 1000, "123321");
    }

    public static Jedis getJedis(){
        return jedisPool.getResource();
    }
}
```

### 3.2 SpringDataRedis客户端

SpringData 是 Spring 中数据操作的模块，包含对各种数据库的集成，对 Redis 集成的模块叫做 SpringDataRedis，官网地址：[Spring Data Redis](https://spring.io/projects/spring-data-redis)

- 提供了对不同 Redis 客户端的整合（Lettuce和Jedis）
- 使用模板设计模式，提供了 RedisTemplate 统一 API 来操作 Redis
- 支持 Redis 的发布订阅模型
- 支持 Redis 哨兵和 Redis 集群
- 支持基于 Lettuce 的响应式编程
- 支持基于 JDK、JSON、字符串、Spring 对象的数据序列化及反序列化
- 支持基于 Redis 的 JDKCollection 实现

SpringDataRedis 中提供了 RedisTemplate 工具类，其中封装了各种对 Redis 的操作。并且将不同数据类型的操作 API 封装到了不同的类型中：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666515304246-a2c8a646-795c-4549-976b-1ca6907576cb.png)

#### 3.2.1 快速入门

SpringBoot 已经提供了对 SpringDataRedis 的支持，使用非常简单。

1. 引入依赖

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.7</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.heima</groupId>
    <artifactId>redis-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>redis-demo</name>
    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <!--redis依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <!--common-pool-->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
        </dependency>
        <!--Jackson依赖-->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

2. 配置 Redis

```
spring:
  redis:
    host: 192.168.108.111
    port: 6379
    password: root
    lettuce:
      pool:
        max-active: 8
        max-idle: 8
        min-idle: 0
        max-wait: 100ms
```

3. 注入 RedisTemplate

```
@SpringBootTest
class RedisStringTests {

    @Autowired
    private RedisTemplate redisTemplate;
}
```

4. 测试

```
@SpringBootTest
class RedisStringTests {

    @Autowired
    private RedisTemplate edisTemplate;

    @Test
    void testString() {
        // 写入一条String数据
        redisTemplate.opsForValue().set("name", "虎哥");
        // 获取string数据
        Object name = stringRedisTemplate.opsForValue().get("name");
        System.out.println("name = " + name);
    }
}
```

#### 3.2.2 自定义序列化

RedisTemplate 可以接收任意 Object 作为值写入 Redis：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666515564704-62501059-658c-496c-9c3e-aacb51f465f9.png)

只不过写入前会把 Object 序列化为字节形式，默认是采用 JDK 序列化，得到的结果是这样的：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666515585530-118a8b43-3235-4dc7-9265-4277e4c1a6aa.png)

缺点：

- 可读性差
- 内存占用较大

我们可以自定义 RedisTemplate 的序列化方式，代码如下：

```
@Configuration
public class RedisConfig {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory connectionFactory){
        // 创建RedisTemplate对象
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        // 设置连接工厂
        template.setConnectionFactory(connectionFactory);
        // 创建JSON序列化工具
        GenericJackson2JsonRedisSerializer jsonRedisSerializer = new GenericJackson2JsonRedisSerializer();
        // 设置Key的序列化
        template.setKeySerializer(RedisSerializer.string());
        template.setHashKeySerializer(RedisSerializer.string());
        // 设置Value的序列化
        template.setValueSerializer(jsonRedisSerializer);
        template.setHashValueSerializer(jsonRedisSerializer);
        // 返回
        return template;
    }
}
```

这里采用了JSON序列化来代替默认的JDK序列化方式。最终结果如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666515762915-614396f6-633e-4588-9c34-1848fc3c8f6e.png)

整体可读性有了很大提升，并且能将 Java 对象自动的序列化为 JSON 字符串，并且查询时能自动把 JSON 反序列化为 Java 对象。不过，其中记录了序列化时对应的 class 名称，目的是为了查询时实现自动反序列化。这会带来额外的内存开销。

#### 3.2.3 StringRedisTemplate

为了节省内存空间，我们可以不使用 JSON 序列化器来处理 value，而是统一使用 String 序列化器，要求只能存储 String 类型的 key 和 value。当需要存储 Java 对象时，手动完成对象的序列化和反序列化。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666515839148-238af68f-05ec-46c8-8e0f-9c346ea750a4.png)

因为存入和读取时的序列化及反序列化都是我们自己实现的，SpringDataRedis 就不会将 class 信息写入 Redis 了。

这种用法比较普遍，因此 SpringDataRedis 就提供了 RedisTemplate 的子类：StringRedisTemplate，它的 key 和 value 的序列化方式默认就是 String方式。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666515919420-58bf89cb-93f1-4cf2-a390-11b4f30020a6.png)

省去了我们自定义 RedisTemplate 的序列化方式的步骤，而是直接使用：

```
@Autowired
private StringRedisTemplate stringRedisTemplate;
// JSON序列化工具
private static final ObjectMapper mapper = new ObjectMapper();

@Test
void testSaveUser() throws JsonProcessingException {
    // 创建对象
    User user = new User("虎哥", 21);
    // 手动序列化
    String json = mapper.writeValueAsString(user);
    // 写入数据
    stringRedisTemplate.opsForValue().set("user:200", json);

    // 获取数据
    String jsonUser = stringRedisTemplate.opsForValue().get("user:200");
    // 手动反序列化
    User user1 = mapper.readValue(jsonUser, User.class);
    System.out.println("user1 = " + user1);
}
```

# Redis高级篇之分布式缓存

基于Redis 集群解决单机 Redis 存在的问题

单机的Redis存在四大问题：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666555788517-003a35d4-1ff3-440c-9287-63de64a77131.png)

## 1.Redis持久化

Redis 有两种持久化方案

### 1.1 RDB持久化

RDB 全称 `Redis Database Backup file`（Redis数据备份文件），也被叫做 Redis 数据快照。简单来说就是把内存中的所有数据都记录到磁盘中。当Redis 实例故障重启后，从磁盘读取快照文件，恢复数据。快照文件称为 RDB 文件，默认是保存在当前运行目录。

#### 1.1.1 执行时机

1. 执行 save 命令时

执行 save 命令，可以立即执行一次RDB：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666555904690-23e19d94-9761-4ee4-94dc-49634ebcfd42.png)

save 命令会导致主进程执行 RDB，这个过程中其它所有命令都会被阻塞。只有在数据迁移时可能用到。

2. 执行 bgsave 命令时

执行 bgsave 命令，可以异步执行 RDB：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666555929828-e28c44af-f4ab-46c4-9161-985bcf1f5245.png)

bgsave 命令执行后会开启独立进程完成 RDB，主进程可以持续处理用户请求，不受影响。

3. 停机时

Redis 停机时会执行一次 save 命令，实现 RDB 持久化。

4. 触发 RDB 条件时

Redis 内部有触发 RDB 的机制，可以在 redis.conf 文件中找到，格式如下：

```
# 900秒内，如果至少有1个key被修改，则执行bgsave ， 如果是save "" 则表示禁用RDB
save 900 1  
save 300 10  
save 60 10000 
```

RDB的其它配置也可以在redis.conf文件中设置：

```
# 是否压缩 ,建议不开启，压缩也会消耗cpu，磁盘的话不值钱
rdbcompression yes

# RDB文件名称
dbfilename dump.rdb  

# 文件保存的路径目录
dir ./ 
``` 

#### 1.1.2 RDB原理

bgsave 开始时会 fork 主进程得到子进程，子进程共享主进程的内存数据。完成 fork 后读取内存数据并写入 RDB 文件。

fork 采用的是 `copy-on-write` 技术：

- 当主进程执行读操作时，访问共享内存；
- 当主进程执行写操作时，则会拷贝一份数据，执行写操作。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666556017195-1f08681b-3ea1-44f8-80d0-426fe4627358.png)

RDB的缺点

- RDB执行间隔时间长，两次RDB之间写入数据有丢失的风险
- fork子进程、压缩、写出RDB文件都比较耗时

### 1.2 AOF持久化

#### 1.2.1 AOF原理

AOF 全称为`Append Only File`（追加文件）。Redis 处理的每一个写命令都会记录在 AOF 文件，可以看做是命令日志文件。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666556085705-2280a15e-15f5-4615-9c47-803f8107b166.png)

#### 1.2.2 AOF配置

AOF 默认是关闭的，需要修改 redis.conf 配置文件来开启AOF：

```
# 是否开启 AOF 功能，默认是no
appendonly yes
# AOF文件的名称
appendfilename "appendonly.aof"
```

AOF 的命令记录的频率也可以通过 redis.conf 文件来配置：

```
# 表示每执行一次写命令，立即记录到AOF文件
appendfsync always 
# 写命令执行完先放入AOF缓冲区，然后表示每隔1秒将缓冲区数据写到AOF文件，是默认方案
appendfsync everysec 
# 写命令执行完先放入AOF缓冲区，由操作系统决定何时将缓冲区内容写回磁盘
appendfsync no
```

  
三种策略对比：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666556161312-36598dc3-ee66-4926-a093-041cc101403d.png)

#### 1.2.3 AOF文件重写

因为是记录命令，AOF 文件会比 RDB 文件大的多。而且 AOF 会记录对同一个 key 的多次写操作，但只有最后一次写操作才有意义。通过执行bgrewriteaof 命令，可以让 AOF 文件执行重写功能，用最少的命令达到相同效果。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666556195371-34333ece-ac86-46ee-9355-aa981bd9540b.png)

Redis也会在触发阈值时自动去重写AOF文件。阈值也可以在redis.conf中配置：

```
# AOF文件比上次文件 增长超过多少百分比则触发重写
auto-aof-rewrite-percentage 100
# AOF文件体积最小多大以上才触发重写 
auto-aof-rewrite-min-size 64mb 
```

### 1.3 RDB和AOF的对比

RDB 和 AOF 各有自己的优缺点，如果对数据安全性要求较高，在实际开发中往往会结合两者来使用。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666556246655-714155c2-6b31-4750-a85d-999c249fd13a.png)

## 2.Redis主从

### 2.1 搭建主从架构

单节点 Redis 的并发能力是有上限的，要进一步提高 Redis 的并发能力，就需要搭建主从集群，实现读写分离。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666556296722-32e9f656-e522-4345-a9bf-0fc9bf2eb773.png)

#### 2.1.1 集群结构

我们搭建的主从集群结构如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666556479379-5ffde90f-7d2e-46c9-8196-d8c17cf49740.png)

共包含三个节点，一个主节点，两个从节点。

这里我们会在同一台虚拟机中开启3个 redis 实例，模拟主从集群，信息如下：

|   |   |   |
|---|---|---|
|IP|PORT|角色|
|192.168.150.101|7001|master|
|192.168.150.101|7002|slave|
|192.168.150.101|7003|slave|

#### 2.1.2 准备实例和配置

要在同一台虚拟机开启3个实例，必须准备三份不同的配置文件和目录，配置文件所在目录也就是工作目录。

1. 创建目录

我们创建三个文件夹，名字分别叫7001、7002、7003：

```
# 进入/tmp目录
cd /tmp
# 创建目录
mkdir 7001 7002 7003
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666556539879-81c2d97d-a4bd-475c-a93e-d4cb1b356a27.png)

2. 恢复原始配置

修改redis-6.2.4/redis.conf文件，将其中的持久化模式改为默认的RDB模式，AOF保持关闭状态。

```
# 开启RDB
# save ""
save 3600 1
save 300 100
save 60 10000

# 关闭AOF
appendonly no
```

3. 拷贝配置文件到每个实例目录

然后将redis-6.2.4/redis.conf文件拷贝到三个目录中（在/tmp目录执行下列命令）：

```
# 方式一：逐个拷贝
cp redis-6.2.4/redis.conf 7001
cp redis-6.2.4/redis.conf 7002
cp redis-6.2.4/redis.conf 7003

# 方式二：管道组合命令，一键拷贝
echo 7001 7002 7003 | xargs -t -n 1 cp redis-6.2.4/redis.conf
```

4. 修改每个实例的端口和工作目录

修改每个文件夹内的配置文件，将端口分别修改为7001、7002、7003，将rdb文件保存位置都修改为自己所在目录（在/tmp目录执行下列命令）：

```
sed -i -e 's/6379/7001/g' -e 's/dir .\//dir \/tmp\/7001\//g' 7001/redis.conf
sed -i -e 's/6379/7002/g' -e 's/dir .\//dir \/tmp\/7002\//g' 7002/redis.conf
sed -i -e 's/6379/7003/g' -e 's/dir .\//dir \/tmp\/7003\//g' 7003/redis.conf
```

5. 修改每个实例的声明IP

虚拟机本身有多个IP，为了避免将来混乱，我们需要在redis.conf文件中指定每一个实例的绑定ip信息，格式如下：

```
# redis实例的声明 IP
replica-announce-ip 192.168.150.101
```

每个目录都要改，我们一键完成修改（在/tmp目录执行下列命令）：

```
# 逐一执行
sed -i '1a replica-announce-ip 192.168.150.101' 7001/redis.conf
sed -i '1a replica-announce-ip 192.168.150.101' 7002/redis.conf
sed -i '1a replica-announce-ip 192.168.150.101' 7003/redis.conf

# 或者一键修改
printf '%s\n' 7001 7002 7003 | xargs -I{} -t sed -i '1a replica-announce-ip 192.168.150.101' {}/redis.conf
```

#### 2.1.3 启动

为了方便查看日志，我们打开3个ssh窗口，分别启动3个redis实例，启动命令：

```
# 第1个
redis-server 7001/redis.conf
# 第2个
redis-server 7002/redis.conf
# 第3个
redis-server 7003/redis.conf
```

启动后：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666556762948-eb626719-e34b-49c1-93a8-36fc8e1ffa49.png)

如果要一键停止，可以运行下面命令：

```
printf '%s\n' 7001 7002 7003 | xargs -I{} -t redis-cli -p {} shutdown
```

#### 2.1.4 开启主从关系

现在三个实例还没有任何关系，要配置主从可以使用 `replicaof` 或者 `slaveof`（5.0以前）命令。给 slave Redis配置 master Redis 的ip和端口。

有临时和永久两种模式：

- 修改配置文件（永久生效）

- 在redis.conf中添加一行配置：slaveof <masterip> <masterport>

- 使用redis-cli客户端连接到redis服务，执行 slaveof 命令（重启后失效）：

```
slaveof <masterip> <masterport>
```

注意：在5.0以后新增命令 replicaof，与 salveof 效果一致。

这里我们为了演示方便，使用方式二。

通过redis-cli命令连接7002，执行下面命令：

```
# 连接 7002
redis-cli -p 7002
# 执行slaveof
slaveof 192.168.150.101 7001
```

通过redis-cli命令连接7003，执行下面命令：

```
# 连接 7003
redis-cli -p 7003
# 执行slaveof
slaveof 192.168.150.101 7001
```

然后连接 7001节点，查看集群状态：

```
# 连接 7001
redis-cli -p 7001
# 查看状态
info replication
```

结果：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666556877998-b3c8cb9f-6bf7-4a1e-a3bc-c4876d23725a.png)

#### 2.1.5 测试

执行下列操作以测试：

- 利用 redis-cli 连接7001，执行 set num 123
- 利用 redis-cli 连接7002，执行 get num，再执行 set num 666
- 利用 redis-cli 连接7003，执行 get num，再执行 set num 888

可以发现，只有在7001这个master节点上可以执行写操作，7002和7003这两个slave节点只能执行读操作。

### 2.2 主从数据同步原理

#### 2.2.1 全量同步

主从第一次建立连接时，会执行全量同步，将 master 节点的所有数据都拷贝给 slave 节点，流程：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666602488056-4206aa5f-a5e2-444f-acdf-a6eebdb1a0a8.png)

这里有一个问题：master 如何得知 slave 是第一次来连接呢？

有几个概念，可以作为判断依据：

- Replication Id：简称 replid，是数据集的标记，id一致则说明是同一数据集。每一个master都有唯一的replid，slave则会继承master节点的replid
- offset：偏移量，随着记录在repl_baklog中的数据增多而逐渐增大。slave完成同步时也会记录当前同步的offset。如果slave的offset小于master的offset，说明slave数据落后于master，需要更新。

因此slave做数据同步，必须向master声明自己的replication id 和offset，master才可以判断到底需要同步哪些数据。

因为slave原本也是一个master，有自己的replid和offset，当第一次变成slave，与master建立连接时，发送的replid和offset是自己的replid和offset。

master判断发现slave发送来的replid与自己的不一致，说明这是一个全新的slave，就知道要做全量同步了。

master会将自己的replid和offset都发送给这个slave，slave保存这些信息。以后slave的replid就与master一致了。

因此，master判断一个节点是否是第一次同步的依据，就是看replid是否一致。

如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666602512237-06a6c775-490e-4350-b255-4e02c20a6812.png)

  
完整流程描述：

- slave节点请求增量同步
- master节点判断replid，发现不一致，拒绝增量同步
- master将完整内存数据生成RDB，发送RDB到slave
- slave清空本地数据，加载master的RDB
- master将RDB期间的命令记录在repl_baklog，并持续将log中的命令发送给slave
- slave执行接收到的命令，保持与master之间的同步

#### 2.2.2 增量同步

全量同步需要先做RDB，然后将RDB文件通过网络传输个slave，成本太高了。因此除了第一次做全量同步，其它大多数时候slave与master都是做增量同步

什么是增量同步？就是只更新slave与master存在差异的部分数据。如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666602566996-1f7289f0-6a8f-4b2f-9b86-52859e4cf845.png)

那么master怎么知道slave与自己的数据差异在哪里呢?

#### 2.2.3 repl_backlog原理

master怎么知道slave与自己的数据差异在哪里呢?

这就要说到全量同步时的 repl_baklog 文件了。

这个文件是一个固定大小的数组，只不过数组是环形，也就是说角标到达数组末尾后，会再次从0开始读写，这样数组头部的数据就会被覆盖。

repl_baklog 中会记录 Redis 处理过的命令日志及 offset，包括 master 当前的 offset，和 slave 已经拷贝到的 offset：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666602640241-39ed308a-bb0c-4385-ad94-4b8ced880724.png)

slave与master的offset之间的差异，就是salve需要增量拷贝的数据了。

随着不断有数据写入，master的offset逐渐变大，slave也不断的拷贝，追赶master的offset：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666602657258-7ad898bc-849d-48ca-b4ca-7b62b764c1c7.png)

直到数组被填满：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666602671310-f9789bdd-4df9-497f-966e-c65e0f964334.png)

此时，如果有新的数据写入，就会覆盖数组中的旧数据。不过，旧的数据只要是绿色的，说明是已经被同步到slave的数据，即便被覆盖了也没什么影响。因为未同步的仅仅是红色部分。

但是，如果slave出现网络阻塞，导致master的offset远远超过了slave的offset：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666602687617-30126d97-8947-4b86-8926-a770dce8ddfd.png)

如果master继续写入新数据，其offset就会覆盖旧的数据，直到将slave现在的offset也覆盖：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666602704522-e273c563-508b-4057-b8e6-665b45280d63.png)

棕色框中的红色部分，就是尚未同步，但是却已经被覆盖的数据。此时如果slave恢复，需要同步，却发现自己的offset都没有了，无法完成增量同步了。只能做全量同步。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666602722945-5b6db3f2-107e-40ad-ae6f-e40c4b758fd4.png)

### 2.3 主从同步优化

主从同步可以保证主从数据的一致性，非常重要。

可以从以下几个方面来优化Redis主从集群：

- 在master中配置repl-diskless-sync yes启用无磁盘复制，避免全量同步时的磁盘IO。
- Redis单节点上的内存占用不要太大，减少RDB导致的过多磁盘IO
- 适当提高repl_baklog的大小，发现slave宕机时尽快实现故障恢复，尽可能避免全量同步
- 限制一个master上的slave节点数量，如果实在是太多slave，则可以采用主-从-从链式结构，减少master压力

主从从架构图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666602778530-ae9523f9-7cc5-466c-9b02-5696fa699104.png)

## 3.Redis哨兵

Redis提供了哨兵（Sentinel）机制来实现主从集群的自动故障恢复。

### 3.1 哨兵原理

#### 3.1.1 集群结构和作用

哨兵的结构如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666602864152-c6abe477-e891-4a96-aa15-327d3eb22bfb.png)

哨兵的作用如下：

- 监控：Sentinel 会不断检查 master 和 slave 是否按预期工作
- 自动故障恢复：如果 master 故障，Sentinel 会将一个 slave 提升为 master。当故障实例恢复后也以新的 master 为主
- 通知：Sentinel 充当 Redis 客户端的服务发现来源，当集群发生故障转移时，会将最新信息推送给 Redis 的客户端

#### 3.1.2 集群监控原理

Sentinel 基于心跳机制监测服务状态，每隔1秒向集群的每个实例发送 ping 命令：

- 主观下线：如果某 sentinel 节点发现某实例未在规定时间响应，则认为该实例主观下线。
- 客观下线：若超过指定数量（quorum）的 sentinel 都认为该实例主观下线，则该实例客观下线。quorum 值最好超过 Sentinel 实例数量的一半。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666602910052-56d7ce4e-dc39-4381-a4ae-4fb96048ebdc.png)

#### 3.1.3 集群故障恢复原理

一旦发现 master 故障，sentinel 需要在 salve 中选择一个作为新的 master，选择依据是这样的：

- 首先会判断 slave 节点与 master 节点断开时间长短，如果超过指定值（down-after-milliseconds * 10）则会排除该 slave 节点
- 然后判断 slave 节点的 slave-priority 值，越小优先级越高，如果是0则永不参与选举
- 如果 slave-prority 一样，则判断 slave 节点的 offset 值，越大说明数据越新，优先级越高
- 最后是判断 slave 节点的运行 id 大小，越小优先级越高。

当选出一个新的 master 后，该如何实现切换呢？

流程如下：

- sentinel 给备选的 slave 节点发送 slaveof no one 命令，让该节点成为 master
- sentinel 给所有其它 slave 发送 slaveof 192.168.150.101 7002 命令，让这些 slave 成为新 master 的从节点，开始从新的 master 上同步数据。
- 最后，sentinel 将故障节点标记为 slave，当故障节点恢复后会自动成为新的 master 的 slave 节点

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666602963332-cd8c5de7-7d2f-4ff5-8e0f-15796bc4e750.png)

#### 3.1.4 小结

Sentinel的三个作用是什么？

- 监控
- 故障转移
- 通知

Sentinel如何判断一个redis实例是否健康？

- 每隔1秒发送一次ping命令，如果超过一定时间没有相向则认为是主观下线
- 如果大多数sentinel都认为实例主观下线，则判定服务下线

故障转移步骤有哪些？

- 首先选定一个slave作为新的master，执行slaveof no one
- 然后让所有节点都执行slaveof 新master
- 修改故障节点配置，添加slaveof 新master

### 3.2 搭建哨兵集群

#### 3.2.1 集群结构

这里我们搭建一个三节点形成的 Sentinel 集群，来监管之前的 Redis 主从集群。如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666603077333-aabf0949-7389-4034-8021-521dc5607beb.png)

三个 sentinel 实例信息如下：

|   |   |   |
|---|---|---|
|节点|IP|PORT|
|s1|192.168.150.101|27001|
|s2|192.168.150.101|27002|
|s3|192.168.150.101|27003|

#### 3.2.2 准备实例和配置

要在同一台虚拟机开启3个实例，必须准备三份不同的配置文件和目录，配置文件所在目录也就是工作目录。

我们创建三个文件夹，名字分别叫s1、s2、s3：

```
# 进入/tmp目录
cd /tmp
# 创建目录
mkdir s1 s2 s3
```

如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666603157412-ffa8ebb5-134c-4e8a-a6c7-5cfae1749013.png)

然后我们在s1目录创建一个sentinel.conf文件，添加下面的内容：

```
port 27001
sentinel announce-ip 192.168.150.101
sentinel monitor mymaster 192.168.150.101 7001 2
sentinel down-after-milliseconds mymaster 5000
sentinel failover-timeout mymaster 60000
dir "/tmp/s1"
```

解读：

- port 27001：是当前sentinel实例的端口
- sentinel monitor mymaster 192.168.150.101 7001 2：指定主节点信息

- mymaster：主节点名称，自定义，任意写
- 192.168.150.101 7001：主节点的ip和端口
- 2：选举master时的quorum值

  
然后将s1/sentinel.conf文件拷贝到s2、s3两个目录中（在/tmp目录执行下列命令）：

```
# 方式一：逐个拷贝
cp s1/sentinel.conf s2
cp s1/sentinel.conf s3
# 方式二：管道组合命令，一键拷贝
echo s2 s3 | xargs -t -n 1 cp s1/sentinel.conf
```

修改s2、s3两个文件夹内的配置文件，将端口分别修改为27002、27003：

```
sed -i -e 's/27001/27002/g' -e 's/s1/s2/g' s2/sentinel.conf
sed -i -e 's/27001/27003/g' -e 's/s1/s3/g' s3/sentinel.conf
```

#### 3.2.3 启动

为了方便查看日志，我们打开3个ssh窗口，分别启动3个redis实例，启动命令：

```
# 第1个
redis-sentinel s1/sentinel.conf
# 第2个
redis-sentinel s2/sentinel.conf
# 第3个
redis-sentinel s3/sentinel.conf
```

启动后：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666603270586-cdf96756-28e5-46c6-89f5-c0bbb2cd1860.png)

#### 3.2.4 测试

尝试让master节点7001宕机，查看sentinel日志：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666603310040-93f6afde-7e14-4f95-a636-f21c89ab3ad0.png)

查看7003的日志：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666603316099-47de6bb1-aa08-4268-89e8-3a522e009716.png)

查看7002的日志：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666603337922-0d2ea069-8b32-457e-bab4-d7cfc19c265d.png)

### 3.3 RedisTemplate

在Sentinel集群监管下的Redis主从集群，其节点会因为自动故障转移而发生变化，Redis的客户端必须感知这种变化，及时更新连接信息。Spring的RedisTemplate底层利用lettuce实现了节点的感知和自动切换。

下面，我们通过一个测试来实现RedisTemplate集成哨兵机制。

#### 3.3.1 导入Demo工程

首先，我们引入课前资料提供的Demo工程：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666603423776-fc5eb6b7-d097-4d67-b510-47da8fa2534f.png)

#### 3.3.2 引入依赖

在项目的pom文件中引入依赖：

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

#### 3.3.3 配置Redis地址

然后在配置文件application.yml中指定redis的sentinel相关信息：

```
spring:
  redis:
    sentinel:
      master: mymaster
      nodes:
        - 192.168.150.101:27001
        - 192.168.150.101:27002
        - 192.168.150.101:27003
```

#### 3.3.4 配置读写分离

在项目的启动类中，添加一个新的bean：

```
@Bean
public LettuceClientConfigurationBuilderCustomizer clientConfigurationBuilderCustomizer(){
    return clientConfigurationBuilder -> clientConfigurationBuilder.readFrom(ReadFrom.REPLICA_PREFERRED);
}
```

这个bean中配置的就是读写策略，包括四种：

- MASTER：从主节点读取
- MASTER_PREFERRED：优先从master节点读取，master不可用才读取replica
- REPLICA：从slave（replica）节点读取
- REPLICA _PREFERRED：优先从slave（replica）节点读取，所有的slave都不可用才读取master

## 4.Redis分片集群

### 4.1 搭建分片集群

主从和哨兵可以解决高可用、高并发读的问题。但是依然有两个问题没有解决：

- 海量数据存储问题
- 高并发写的问题

使用分片集群可以解决上述问题，如图:

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666603587447-5601a2f7-b047-4c34-a3d1-ba4196bff368.png)

  
分片集群特征：

- 集群中有多个master，每个master保存不同数据
- 每个master都可以有多个slave节点
- master之间通过ping监测彼此健康状态
- 客户端请求可以访问集群任意节点，最终都会被转发到正确节点

#### 4.1.1 集群结构

分片集群需要的节点数量较多，这里我们搭建一个最小的分片集群，包含3个master节点，每个master包含一个slave节点，结构如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666603706790-fe2ee5b5-c9ae-4f47-bd5e-f32e99df432b.png)

  
这里我们会在同一台虚拟机中开启6个redis实例，模拟分片集群，信息如下：

|   |   |   |
|---|---|---|
|IP|PORT|角色|
|192.168.150.101|7001|master|
|192.168.150.101|7002|master|
|192.168.150.101|7003|master|
|192.168.150.101|8001|slave|
|192.168.150.101|8002|slave|
|192.168.150.101|8003|slave|

#### 4.1.2 准备实例和配置

删除之前的7001、7002、7003这几个目录，重新创建出7001、7002、7003、8001、8002、8003目录：

```
# 进入/tmp目录
cd /tmp
# 删除旧的，避免配置干扰
rm -rf 7001 7002 7003
# 创建目录
mkdir 7001 7002 7003 8001 8002 8003
```

在/tmp下准备一个新的redis.conf文件，内容如下：

```
port 6379
# 开启集群功能
cluster-enabled yes
# 集群的配置文件名称，不需要我们创建，由redis自己维护
cluster-config-file /tmp/6379/nodes.conf
# 节点心跳失败的超时时间
cluster-node-timeout 5000
# 持久化文件存放目录
dir /tmp/6379
# 绑定地址
bind 0.0.0.0
# 让redis后台运行
daemonize yes
# 注册的实例ip
replica-announce-ip 192.168.150.101
# 保护模式
protected-mode no
# 数据库数量
databases 1
# 日志
logfile /tmp/6379/run.log
```

将这个文件拷贝到每个目录下：

```
# 进入/tmp目录
cd /tmp
# 执行拷贝
echo 7001 7002 7003 8001 8002 8003 | xargs -t -n 1 cp redis.conf
```

#### 4.1.3 启动

因为已经配置了后台启动模式，所以可以直接启动服务：

```
# 进入/tmp目录
cd /tmp
# 一键启动所有服务
printf '%s\n' 7001 7002 7003 8001 8002 8003 | xargs -I{} -t redis-server {}/redis.conf
```

通过ps查看状态：

```
ps -ef | grep redis
```

发现服务都已经正常启动：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666603901830-df81c37f-d9cf-4fc1-b528-5b2789fbe04e.png)

如果要关闭所有进程，可以执行命令：

```
ps -ef | grep redis | awk '{print $2}' | xargs kill
```

或者（推荐这种方式）：

```
printf '%s\n' 7001 7002 7003 8001 8002 8003 | xargs -I{} -t redis-cli -p {} shutdown
```

#### 4.1.4 创建集群

虽然服务启动了，但是目前每个服务之间都是独立的，没有任何关联。

我们需要执行命令来创建集群，在Redis5.0之前创建集群比较麻烦，5.0之后集群管理命令都集成到了redis-cli中。

1）Redis5.0之前

Redis5.0之前集群命令都是用redis安装包下的src/redis-trib.rb来实现的。因为redis-trib.rb是有ruby语言编写的所以需要安装ruby环境。

```
# 安装依赖
yum -y install zlib ruby rubygems
gem install redis
```

然后通过命令来管理集群：

```
# 进入redis的src目录
cd /tmp/redis-6.2.4/src
# 创建集群
./redis-trib.rb create --replicas 1 192.168.150.101:7001 192.168.150.101:7002 192.168.150.101:7003 192.168.150.101:8001 192.168.150.101:8002 192.168.150.101:8003
```

2）Redis5.0以后

我们使用的是Redis6.2.4版本，集群管理以及集成到了redis-cli中，格式如下：

```
redis-cli --cluster create --cluster-replicas 1 192.168.150.101:7001 192.168.150.101:7002 192.168.150.101:7003 192.168.150.101:8001 192.168.150.101:8002 192.168.150.101:8003
```

命令说明：

- redis-cli --cluster或者./redis-trib.rb：代表集群操作命令
- create：代表是创建集群
- --replicas 1或者--cluster-replicas 1 ：指定集群中每个master的副本个数为1，此时节点总数 ÷ (replicas + 1) 得到的就是master的数量。因此节点列表中的前n个就是master，其它节点都是slave节点，随机分配到不同master

运行后的样子：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666604135552-e989a637-cad9-4190-800e-c22da11825d2.png)

这里输入yes，则集群开始创建：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666604153583-b0fa2ebb-6f44-4d4b-b736-69edef93f9f4.png)

通过命令可以查看集群状态：

```
redis-cli -p 7001 cluster nodes
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666604187347-0c28a55b-cfd6-4485-9457-a320425d4a8b.png)

#### 4.1.5 测试

尝试连接7001节点，存储一个数据：

```
# 连接
redis-cli -p 7001
# 存储数据
set num 123
# 读取数据
get num
# 再次存储
set a 1
```

结果悲剧了：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666604236792-bc2fbe01-c36a-4e47-a515-a36b4b28874e.png)

集群操作时，需要给redis-cli加上-c参数才可以：

```
redis-cli -c -p 7001
```

这次可以了：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666604273539-99aa18c9-fcf9-4606-b1a6-13a25f3d6159.png)

## 4.2 散列插槽

### 4.2.1 插槽原理

Redis会把每一个master节点映射到0~16383共16384个插槽（hash slot）上，查看集群信息时就能看到：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739215460-13bd34f3-c1c7-41fb-a709-876f3916b28c.png)

数据key不是与节点绑定，而是与插槽绑定。redis会根据key的有效部分计算插槽值，分两种情况：

- key中包含"{}"，且“{}”中至少包含1个字符，“{}”中的部分是有效部分
- key中不包含“{}”，整个key都是有效部分

例如：key是num，那么就根据num计算，如果是{itcast}num，则根据itcast计算。计算方式是利用CRC16算法得到一个hash值，然后对16384取余，得到的结果就是slot值。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739265456-109f9045-f3fb-4950-9044-a84bea913e5f.png)

如图，在7001这个节点执行set a 1时，对a做hash运算，对16384取余，得到的结果是15495，因此要存储到103节点。

到了7003后，执行get num时，对num做hash运算，对16384取余，得到的结果是2765，因此需要切换到7001节点

### 4.2.1.小结

Redis如何判断某个key应该在哪个实例？

- 将16384个插槽分配到不同的实例
- 根据key的有效部分计算哈希值，对16384取余
- 余数作为插槽，寻找插槽所在实例即可

如何将同一类数据固定的保存在同一个Redis实例？

- 这一类数据使用相同的有效部分，例如key都以{typeId}为前缀

## 4.3.集群伸缩

redis-cli --cluster提供了很多操作集群的命令，可以通过下面方式查看：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739330956-21b8e424-2743-43c4-b925-f6eea126d83b.png)

比如，添加节点的命令：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739342897-c2ce80c5-b73b-4227-b25e-7ebf4789d888.png)

### 4.3.1.需求分析

需求：向集群中添加一个新的master节点，并向其中存储 num = 10

- 启动一个新的redis实例，端口为7004
- 添加7004到之前的集群，并作为一个master节点
- 给7004节点分配插槽，使得num这个key可以存储到7004实例

这里需要两个新的功能：

- 添加一个节点到集群中
- 将部分插槽分配到新插槽

### 4.3.2.创建新的redis实例

创建一个文件夹：

```
mkdir 7004
```

拷贝配置文件：

```
cp redis.conf /7004
```

修改配置文件：

```
sed /s/6379/7004/g 7004/redis.conf
```

启动

```
redis-server 7004/redis.conf
```

### 4.3.3.添加新节点到redis

添加节点的语法如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739366795-ba848805-2923-41ad-ad2d-644f51c71c3e.png)

执行命令：

```
redis-cli --cluster add-node  192.168.150.101:7004 192.168.150.101:7001
```

通过命令查看集群状态：

```
redis-cli -p 7001 cluster nodes
```

如图，7004加入了集群，并且默认是一个master节点：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739385247-7501254b-524f-4973-80a2-a0881b5f680c.png)

但是，可以看到7004节点的插槽数量为0，因此没有任何数据可以存储到7004上

### 4.3.4.转移插槽

我们要将num存储到7004节点，因此需要先看看num的插槽是多少：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739394885-0f8fab4e-a36b-4440-add2-aae037f8deea.png)

如上图所示，num的插槽为2765.

我们可以将0~3000的插槽从7001转移到7004，命令格式如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739408301-260ad914-fca0-470d-b0bb-cfb51e99a5ed.png)

具体命令如下：

建立连接：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739420159-b4a850fd-9357-4996-b133-5922b26c3c6a.png)

得到下面的反馈：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739431999-66a671e3-7790-4d58-95f1-3d2f18a55285.png)

询问要移动多少个插槽，我们计划是3000个：

新的问题来了：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739444998-a86f669c-6e74-42b2-84dd-abd6757a89e6.png)

那个node来接收这些插槽？？

显然是7004，那么7004节点的id是多少呢？

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739461029-7af427c8-3b8e-4c83-a10b-0cb625f9782c.png)

复制这个id，然后拷贝到刚才的控制台后：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739472397-b4d4f673-361e-4239-a7a2-a5421f66a501.png)

这里询问，你的插槽是从哪里移动过来的？

- all：代表全部，也就是三个节点各转移一部分
- 具体的id：目标节点的id
- done：没有了

这里我们要从7001获取，因此填写7001的id：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739485031-bb74d1f5-173f-4e72-8366-c778943cfad0.png)

填完后，点击done，这样插槽转移就准备好了：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739497541-c4f772d6-a57a-4e11-8f38-3779fc1bbbbb.png)

确认要转移吗？输入yes：

然后，通过命令查看结果：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739513151-870d1863-dd2c-4ae5-aed1-c7c9185337fb.png)

可以看到：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739527573-59373d1c-b733-4969-8455-57f72d99eae2.png)

目的达成。

## 4.4.故障转移

集群初识状态是这样的：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739576750-6b0a9818-9738-483d-8a29-d54bd89a630f.png)

其中7001、7002、7003都是master，我们计划让7002宕机。

### 4.4.1.自动故障转移

当集群中有一个master宕机会发生什么呢？

直接停止一个redis实例，例如7002：

```
redis-cli -p 7002 shutdown
```

1）首先是该实例与其它实例失去连接

2）然后是疑似宕机：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739620127-5ea91c3c-46ad-4360-a1ae-8fe284ff7399.png)

3）最后是确定下线，自动提升一个slave为新的master：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739631374-2cab576e-0c65-42e4-b5ed-a1756a340e94.png)

4）当7002再次启动，就会变为一个slave节点了：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739643015-4f3baed2-7cb9-4217-80eb-e9147bd30a00.png)

### 4.4.2.手动故障转移

利用cluster failover命令可以手动让集群中的某个master宕机，切换到执行cluster failover命令的这个slave节点，实现无感知的数据迁移。其流程如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739657834-c6ed42ef-27f0-4ea3-b785-61d1891d82ff.png)

这种failover命令可以指定三种模式：

- 缺省：默认的流程，如图1~6歩
- force：省略了对offset的一致性校验
- takeover：直接执行第5歩，忽略数据一致性、忽略master状态和其它master的意见

案例需求：在7002这个slave节点执行手动故障转移，重新夺回master地位

步骤如下：

1）利用redis-cli连接7002这个节点

2）执行cluster failover命令

如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739675614-033f9ea1-04fb-433c-a369-2e2433b7d42d.png)

效果：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739685656-d5200065-8e06-4b0a-a471-d19f54066cf7.png)

## 4.5.RedisTemplate访问分片集群

RedisTemplate底层同样基于lettuce实现了分片集群的支持，而使用的步骤与哨兵模式基本一致：

1）引入redis的starter依赖

2）配置分片集群地址

3）配置读写分离

与哨兵模式相比，其中只有分片集群的配置方式略有差异，如下：

```
spring:
  redis:
    cluster:
      nodes:
        - 192.168.150.101:7001
        - 192.168.150.101:7002
        - 192.168.150.101:7003
        - 192.168.150.101:8001
        - 192.168.150.101:8002
        - 192.168.150.101:8003
```

# Redis高级篇之多级缓存

# 1.什么是多级缓存

传统的缓存策略一般是请求到达Tomcat后，先查询Redis，如果未命中则查询数据库，如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739855638-8a8f2959-ff09-4060-829f-266b0c420a25.png)

存在下面的问题：

•请求要经过Tomcat处理，Tomcat的性能成为整个系统的瓶颈

•Redis缓存失效时，会对数据库产生冲击

多级缓存就是充分利用请求处理的每个环节，分别添加缓存，减轻Tomcat压力，提升服务性能：

- 浏览器访问静态资源时，优先读取浏览器本地缓存
- 访问非静态资源（ajax查询数据）时，访问服务端
- 请求到达Nginx后，优先读取Nginx本地缓存
- 如果Nginx本地缓存未命中，则去直接查询Redis（不经过Tomcat）
- 如果Redis查询未命中，则查询Tomcat
- 请求进入Tomcat后，优先查询JVM进程缓存
- 如果JVM进程缓存未命中，则查询数据库

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739870032-cc3bca94-18e6-45e2-b04a-6e606d5d5ecf.png)

在多级缓存架构中，Nginx内部需要编写本地缓存查询、Redis查询、Tomcat查询的业务逻辑，因此这样的nginx服务不再是一个反向代理服务器，而是一个编写业务的Web服务器了。

因此这样的业务Nginx服务也需要搭建集群来提高并发，再有专门的nginx服务来做反向代理，如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739883450-c2b95085-2e73-4836-8572-1ff9f4f19a59.png)

另外，我们的Tomcat服务将来也会部署为集群模式：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739894906-00c7ad9b-064d-4701-bae8-5cfdbf0714ab.png)

可见，多级缓存的关键有两个：

- 一个是在nginx中编写业务，实现nginx本地缓存、Redis、Tomcat的查询
- 另一个就是在Tomcat中实现JVM进程缓存

其中Nginx编程则会用到OpenResty框架结合Lua这样的语言。

这也是今天课程的难点和重点。

# 2.JVM进程缓存

为了演示多级缓存的案例，我们先准备一个商品查询的业务。

## 2.1.导入案例

### 2.1.1.安装MySQL

后期做数据同步需要用到MySQL的主从功能，所以需要大家在虚拟机中，利用Docker来运行一个MySQL容器。

1）准备目录

为了方便后期配置MySQL，我们先准备两个目录，用于挂载容器的数据和配置文件目录：

```
# 进入/tmp目录
cd /tmp
# 创建文件夹
mkdir mysql
# 进入mysql目录
cd mysql
```

2）运行命令

进入mysql目录后，执行下面的Docker命令：

```
docker run \
 -p 3306:3306 \
 --name mysql \
 -v $PWD/conf:/etc/mysql/conf.d \
 -v $PWD/logs:/logs \
 -v $PWD/data:/var/lib/mysql \
 -e MYSQL_ROOT_PASSWORD=123 \
 --privileged \
 -d \
 mysql:5.7.25
```

3）修改配置

在/tmp/mysql/conf目录添加一个my.cnf文件，作为mysql的配置文件：

```
# 创建文件
touch /tmp/mysql/conf/my.cnf
```

文件的内容如下：

```
[mysqld]
skip-name-resolve
character_set_server=utf8
datadir=/var/lib/mysql
server-id=1000
```

4）重启

配置修改后，必须重启容器：

```
docker restart mysql
```

### 2.1.2.导入SQL

接下来，利用Navicat客户端连接MySQL，然后导入课前资料提供的sql文件：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667782829616-3fe9e56b-53ab-4ddd-b9ac-709630e64f7b.png)

其中包含两张表：

- tb_item：商品表，包含商品的基本信息
- tb_item_stock：商品库存表，包含商品的库存信息

之所以将库存分离出来，是因为库存是更新比较频繁的信息，写操作较多。而其他信息修改的频率非常低。

### 2.1.3.导入Demo工程

下面导入课前资料提供的工程：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667782847180-1c279772-6e88-44f6-946b-085823ab4ab0.png)

项目结构如图所示：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667782877344-4a59af82-f12c-4e18-8a0b-9739ab7edc4e.png)

其中的业务包括：

- 分页查询商品
- 新增商品
- 修改商品
- 修改库存
- 删除商品
- 根据id查询商品
- 根据id查询库存

业务全部使用mybatis-plus来实现，如有需要请自行修改业务逻辑。

1）分页查询商品

在`com.heima.item.web`包的`ItemController`中可以看到接口定义：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667782933724-0fb76a7c-2beb-4a44-9f4f-4cb6411f41e6.png)

2）新增商品

在`com.heima.item.web`包的`ItemController`中可以看到接口定义：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667782945131-09eba21c-1cd4-4222-80a7-e8c49698ee26.png)

3）修改商品

在`com.heima.item.web`包的`ItemController`中可以看到接口定义：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667782984079-8b735ae7-5e16-4c10-ba58-56290cdebbd1.png)

4）修改库存

在`com.heima.item.web`包的`ItemController`中可以看到接口定义：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667782994680-ae2e1503-bf02-4421-a4e5-1ddee04bbba3.png)

5）删除商品

在`com.heima.item.web`包的`ItemController`中可以看到接口定义：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667783005746-0b4da763-5c84-45c7-8341-d643a855ed76.png)

这里是采用了逻辑删除，将商品状态修改为3

6）根据id查询商品

在`com.heima.item.web`包的`ItemController`中可以看到接口定义：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667783043947-601b534d-eec8-4a77-9d92-eaa0ba7ee2d7.png)

这里只返回了商品信息，不包含库存

7）根据id查询库存

在`com.heima.item.web`包的`ItemController`中可以看到接口定义：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667783061304-979cc68a-897a-43bb-acd5-f81c65ef63de.png)

8）启动

注意修改application.yml文件中配置的mysql地址信息：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667783074172-9f2becd6-dd03-4bcc-8ae6-c0fd8b485dad.png)

需要修改为自己的虚拟机地址信息、还有账号和密码。

修改后，启动服务，访问：http://localhost:8081/item/10001即可查询数据

### 2.1.4 导入商品查询页面

商品查询是购物页面，与商品管理的页面是分离的。

部署方式如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667783106025-06afc815-93df-4414-9943-0878b0e259fb.png)

我们需要准备一个反向代理的nginx服务器，如上图红框所示，将静态的商品页面放到nginx目录中。

页面需要的数据通过ajax向服务端（nginx业务集群）查询。

1）运行nginx服务

这里我已经给大家准备好了nginx反向代理服务器和静态资源。

我们找到课前资料的nginx目录：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667783134357-8516d3e3-1581-4d96-ad03-473b718706ff.png)

将其拷贝到一个非中文目录下，运行这个nginx服务。

运行命令：

```
start nginx.exe
```

然后访问 http://localhost/item.html?id=10001即可：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667783148611-fc3593c2-adb7-4d4d-93c4-242be34f4293.png)

2）反向代理

现在，页面是假数据展示的。我们需要向服务器发送ajax请求，查询商品数据。

打开控制台，可以看到页面有发起ajax查询数据：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667783175458-d95599e2-98a2-4504-aaf2-417f6fe845a9.png)

而这个请求地址同样是80端口，所以被当前的nginx反向代理了。

查看nginx的conf目录下的nginx.conf文件：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667783192071-6afb848d-e62d-496c-b229-02ee7b1c5d37.png)

其中的关键配置如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667783221576-28dbd249-501f-4e17-bb7f-c4c86a70d895.png)

其中的192.168.150.101是我的虚拟机IP，也就是我的Nginx业务集群要部署的地方：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667783233738-2f72c49b-3976-4f5f-939c-c85b18e2ddc5.png)

完整内容如下：

```
#user  nobody;
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    #tcp_nopush     on;
    keepalive_timeout  65;

    upstream nginx-cluster{
        server 192.168.150.101:8081;
    }
    server {
        listen       80;
        server_name  localhost;

	location /api {
            proxy_pass http://nginx-cluster;
        }

        location / {
            root   html;
            index  index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

## 2.2.初识Caffeine

缓存在日常开发中启动至关重要的作用，由于是存储在内存中，数据的读取速度是非常快的，能大量减少对数据库的访问，减少数据库的压力。我们把缓存分为两类：

- 分布式缓存，例如Redis：

- 优点：存储容量更大、可靠性更好、可以在集群间共享
- 缺点：访问缓存有网络开销
- 场景：缓存数据量较大、可靠性要求较高、需要在集群间共享

- 进程本地缓存，例如HashMap、GuavaCache：

- 优点：读取本地内存，没有网络开销，速度更快
- 缺点：存储容量有限、可靠性较低、无法共享
- 场景：性能要求较高，缓存数据量较小

我们今天会利用Caffeine框架来实现JVM进程缓存。

Caffeine是一个基于Java8开发的，提供了近乎最佳命中率的高性能的本地缓存库。目前Spring内部的缓存使用的就是Caffeine。GitHub地址：[https://github.com/ben-manes/caffeine](https://github.com/ben-manes/caffeine)

Caffeine的性能非常好，下图是官方给出的性能对比：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739933404-f1eec4c6-e1d2-4232-8343-92364285d850.png)

可以看到Caffeine的性能遥遥领先！

缓存使用的基本API：

```
@Test
void testBasicOps() {
    // 构建cache对象
    Cache<String, String> cache = Caffeine.newBuilder().build();

    // 存数据
    cache.put("gf", "迪丽热巴");

    // 取数据
    String gf = cache.getIfPresent("gf");
    System.out.println("gf = " + gf);

    // 取数据，包含两个参数：
    // 参数一：缓存的key
    // 参数二：Lambda表达式，表达式参数就是缓存的key，方法体是查询数据库的逻辑
    // 优先根据key查询JVM缓存，如果未命中，则执行参数二的Lambda表达式
    String defaultGF = cache.get("defaultGF", key -> {
        // 根据key去数据库查询数据
        return "柳岩";
    });
    System.out.println("defaultGF = " + defaultGF);
}
```

Caffeine既然是缓存的一种，肯定需要有缓存的清除策略，不然的话内存总会有耗尽的时候。

Caffeine提供了三种缓存驱逐策略：

- 基于容量：设置缓存的数量上限

```
// 创建缓存对象
Cache<String, String> cache = Caffeine.newBuilder()
    .maximumSize(1) // 设置缓存大小上限为 1
    .build();
```

- 基于时间：设置缓存的有效时间

```
// 创建缓存对象
Cache<String, String> cache = Caffeine.newBuilder()
    // 设置缓存有效期为 10 秒，从最后一次写入开始计时 
    .expireAfterWrite(Duration.ofSeconds(10)) 
    .build();
```

- 基于引用：设置缓存为软引用或弱引用，利用GC来回收缓存数据。性能较差，不建议使用。

注意：在默认情况下，当一个缓存元素过期的时候，Caffeine不会自动立即将其清理和驱逐。而是在一次读或写操作后，或者在空闲时间完成对失效数据的驱逐。

## 2.3.实现JVM进程缓存

### 2.3.1.需求

利用Caffeine实现下列需求：

- 给根据id查询商品的业务添加缓存，缓存未命中时查询数据库
- 给根据id查询商品库存的业务添加缓存，缓存未命中时查询数据库
- 缓存初始大小为100
- 缓存上限为10000

### 2.3.2.实现

首先，我们需要定义两个Caffeine的缓存对象，分别保存商品、库存的缓存数据。

在item-service的`com.heima.item.config`包下定义`CaffeineConfig`类：

```
package com.heima.item.config;

import com.github.benmanes.caffeine.cache.Cache;
import com.github.benmanes.caffeine.cache.Caffeine;
import com.heima.item.pojo.Item;
import com.heima.item.pojo.ItemStock;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class CaffeineConfig {

    @Bean
    public Cache<Long, Item> itemCache(){
        return Caffeine.newBuilder()
                .initialCapacity(100)
                .maximumSize(10_000)
                .build();
    }

    @Bean
    public Cache<Long, ItemStock> stockCache(){
        return Caffeine.newBuilder()
                .initialCapacity(100)
                .maximumSize(10_000)
                .build();
    }
}
```

然后，修改item-service中的`com.heima.item.web`包下的ItemController类，添加缓存逻辑：

```
@RestController
@RequestMapping("item")
public class ItemController {

    @Autowired
    private IItemService itemService;
    @Autowired
    private IItemStockService stockService;

    @Autowired
    private Cache<Long, Item> itemCache;
    @Autowired
    private Cache<Long, ItemStock> stockCache;
    
    // ...其它略
    
    @GetMapping("/{id}")
    public Item findById(@PathVariable("id") Long id) {
        return itemCache.get(id, key -> itemService.query()
                .ne("status", 3).eq("id", key)
                .one()
        );
    }

    @GetMapping("/stock/{id}")
    public ItemStock findStockById(@PathVariable("id") Long id) {
        return stockCache.get(id, key -> stockService.getById(key));
    }
}
```

# 3.Lua语法入门

Nginx编程需要用到Lua语言，因此我们必须先入门Lua的基本语法。

## 3.1.初识Lua

Lua 是一种轻量小巧的脚本语言，用标准C语言编写并以源代码形式开放， 其设计目的是为了嵌入应用程序中，从而为应用程序提供灵活的扩展和定制功能。官网：[https://www.lua.org/](https://www.lua.org/)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739962797-1d75a037-a128-4324-968f-47a04dbf19ed.png)

Lua经常嵌入到C语言开发的程序中，例如游戏开发、游戏插件等。

Nginx本身也是C语言开发，因此也允许基于Lua做拓展。

## 3.1.HelloWorld

CentOS7默认已经安装了Lua语言环境，所以可以直接运行Lua代码。

1）在Linux虚拟机的任意目录下，新建一个hello.lua文件

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739977974-d38144c4-21d8-49c1-af1b-c0e672f1fc91.png)

2）添加下面的内容

```
print("Hello World!")
```

3）运行

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667739988958-ee094e7a-c46a-40e8-8d9e-9c541fab5476.png)

## 3.2.变量和循环

学习任何语言必然离不开变量，而变量的声明必须先知道数据的类型。

### 3.2.1.Lua的数据类型

Lua中支持的常见数据类型包括：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740004134-6026ec2e-2d23-40d3-b73c-1b379543aa2a.png)

另外，Lua提供了type()函数来判断一个变量的数据类型：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740013125-1423bd30-5cfe-4480-9175-a38e2591d1aa.png)

### 3.2.2.声明变量

Lua声明变量的时候无需指定数据类型，而是用local来声明变量为局部变量：

```
-- 声明字符串，可以用单引号或双引号，
local str = 'hello'
-- 字符串拼接可以使用 ..
local str2 = 'hello' .. 'world'
-- 声明数字
local num = 21
-- 声明布尔类型
local flag = true
```

Lua中的table类型既可以作为数组，又可以作为Java中的map来使用。数组就是特殊的table，key是数组角标而已：

```
-- 声明数组 ，key为角标的 table
local arr = {'java', 'python', 'lua'}
-- 声明table，类似java的map
local map =  {name='Jack', age=21}
```

Lua中的数组角标是从1开始，访问的时候与Java中类似：

```
-- 访问数组，lua数组的角标从1开始
print(arr[1])
```

Lua中的table可以用key来访问：

```
-- 访问table
print(map['name'])
print(map.name)
```

### 3.2.3.循环

对于table，我们可以利用for循环来遍历。不过数组和普通table遍历略有差异。

遍历数组：

```
-- 声明数组 key为索引的 table
local arr = {'java', 'python', 'lua'}
-- 遍历数组
for index,value in ipairs(arr) do
    print(index, value) 
end
```

遍历普通table

```
-- 声明map，也就是table
local map = {name='Jack', age=21}
-- 遍历table
for key,value in pairs(map) do
   print(key, value) 
end
```

## 3.3.条件控制、函数

Lua中的条件控制和函数声明与Java类似。

### 3.3.1.函数

定义函数的语法：

```
function 函数名( argument1, argument2..., argumentn)
    -- 函数体
    return 返回值
end
```

例如，定义一个函数，用来打印数组：

```
function printArr(arr)
    for index, value in ipairs(arr) do
        print(value)
    end
end
```

### 3.3.2.条件控制

类似Java的条件控制，例如if、else语法：

```
if(布尔表达式)
then
   --[ 布尔表达式为 true 时执行该语句块 --]
else
   --[ 布尔表达式为 false 时执行该语句块 --]
end
```

与java不同，布尔表达式中的逻辑运算是基于英文单词：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740044216-58e3d9f4-7cfc-4542-9aad-f0895b71d476.png)

### 3.3.3.案例

需求：自定义一个函数，可以打印table，当参数为nil时，打印错误信息

```
function printArr(arr)
    if not arr then
        print('数组不能为空！')
    end
    for index, value in ipairs(arr) do
        print(value)
    end
end
```

# 4.实现多级缓存

多级缓存的实现离不开Nginx编程，而Nginx编程又离不开OpenResty。

## 4.1.安装OpenResty

OpenResty® 是一个基于 Nginx的高性能 Web 平台，用于方便地搭建能够处理超高并发、扩展性极高的动态 Web 应用、Web 服务和动态网关。具备下列特点：

- 具备Nginx的完整功能
- 基于Lua语言进行扩展，集成了大量精良的 Lua 库、第三方模块
- 允许使用Lua自定义业务逻辑、自定义库

官方网站： [https://openresty.org/cn/](https://openresty.org/cn/)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740058966-a1b265b4-88d7-4a52-a00d-df776265b256.png)

### 4.1.1 安装

首先你的Linux虚拟机必须联网

1）安装开发库

首先要安装OpenResty的依赖开发库，执行命令：

```
yum install -y pcre-devel openssl-devel gcc --skip-broken
```

2）安装OpenResty仓库

你可以在你的 CentOS 系统中添加 `openresty` 仓库，这样就可以便于未来安装或更新我们的软件包（通过 `yum check-update` 命令）。运行下面的命令就可以添加我们的仓库：

```
yum-config-manager --add-repo https://openresty.org/package/centos/openresty.repo
```

如果提示说命令不存在，则运行：

```
yum install -y yum-utils
```

然后再重复上面的命令

3）安装OpenResty

然后就可以像下面这样安装软件包，比如 `openresty`：

```
yum install -y openresty
```

4）安装opm工具

opm是OpenResty的一个管理工具，可以帮助我们安装一个第三方的Lua模块。

如果你想安装命令行工具 `opm`，那么可以像下面这样安装 `openresty-opm` 包：

```
yum install -y openresty-opm
```

5）目录结构

默认情况下，OpenResty安装的目录是：/usr/local/openresty

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667783422358-4b11af57-e63d-461a-9976-e3cf4b577ae9.png)

看到里面的nginx目录了吗，OpenResty就是在Nginx基础上集成了一些Lua模块。

6）配置nginx的环境变量

打开配置文件：

```
vi /etc/profile
```

在最下面加入两行：

```
export NGINX_HOME=/usr/local/openresty/nginx
export PATH=${NGINX_HOME}/sbin:$PATH
```

NGINX_HOME：后面是OpenResty安装目录下的nginx的目录

然后让配置生效：

```
source /etc/profile
```

### 4.1.2 启动和运行

OpenResty底层是基于Nginx的，查看OpenResty目录的nginx目录，结构与windows中安装的nginx基本一致：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667783436656-fbfc4e61-8345-40e5-85f1-1dfe50cec7d4.png)

所以运行方式与nginx基本一致：

```
# 启动nginx
nginx
# 重新加载配置
nginx -s reload
# 停止
nginx -s stop
```

nginx的默认配置文件注释太多，影响后续我们的编辑，这里将nginx.conf中的注释部分删除，保留有效部分。

修改`/usr/local/openresty/nginx/conf/nginx.conf`文件，内容如下：

```
#user  nobody;
worker_processes  1;
error_log  logs/error.log;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       8081;
        server_name  localhost;
        location / {
            root   html;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

在Linux的控制台输入命令以启动nginx：

```
nginx
```

然后访问页面：http://192.168.150.101:8081，注意ip地址替换为你自己的虚拟机IP：

### 4.1.3 备注

加载OpenResty的lua模块：

```
#lua 模块
lua_package_path "/usr/local/openresty/lualib/?.lua;;";
#c模块     
lua_package_cpath "/usr/local/openresty/lualib/?.so;;";
```

common.lua

```
-- 封装函数，发送http请求，并解析响应
local function read_http(path, params)
    local resp = ngx.location.capture(path,{
        method = ngx.HTTP_GET,
        args = params,
    })
    if not resp then
        -- 记录错误信息，返回404
        ngx.log(ngx.ERR, "http not found, path: ", path , ", args: ", args)
        ngx.exit(404)
    end
    return resp.body
end
-- 将方法导出
local _M = {  
    read_http = read_http
}  
return _M
```

释放Redis连接API：

```
-- 关闭redis连接的工具方法，其实是放入连接池
local function close_redis(red)
    local pool_max_idle_time = 10000 -- 连接的空闲时间，单位是毫秒
    local pool_size = 100 --连接池大小
    local ok, err = red:set_keepalive(pool_max_idle_time, pool_size)
    if not ok then
        ngx.log(ngx.ERR, "放入redis连接池失败: ", err)
    end
end
```

读取Redis数据的API：

```
-- 查询redis的方法 ip和port是redis地址，key是查询的key
local function read_redis(ip, port, key)
    -- 获取一个连接
    local ok, err = red:connect(ip, port)
    if not ok then
        ngx.log(ngx.ERR, "连接redis失败 : ", err)
        return nil
    end
    -- 查询redis
    local resp, err = red:get(key)
    -- 查询失败处理
    if not resp then
        ngx.log(ngx.ERR, "查询Redis失败: ", err, ", key = " , key)
    end
    --得到的数据为空处理
    if resp == ngx.null then
        resp = nil
        ngx.log(ngx.ERR, "查询Redis数据为空, key = ", key)
    end
    close_redis(red)
    return resp
end
```

开启共享词典：

```
# 共享字典，也就是本地缓存，名称叫做：item_cache，大小150m
lua_shared_dict item_cache 150m;
```

## 4.2.OpenResty快速入门

我们希望达到的多级缓存架构如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740074084-4fef4b0a-16b9-4c3e-bb92-15daa4e22bf7.png)

其中：

- windows上的nginx用来做反向代理服务，将前端的查询商品的ajax请求代理到OpenResty集群
- OpenResty集群用来编写多级缓存业务

### 4.2.1.反向代理流程

现在，商品详情页使用的是假的商品数据。不过在浏览器中，可以看到页面有发起ajax请求查询真实商品数据。

这个请求如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740084536-dd732f71-272e-4803-ab22-050295c7713d.png)

请求地址是localhost，端口是80，就被windows上安装的Nginx服务给接收到了。然后代理给了OpenResty集群：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740095399-524ac1bc-e457-4bbe-ba80-416fe2888da3.png)

我们需要在OpenResty中编写业务，查询商品数据并返回到浏览器。

但是这次，我们先在OpenResty接收请求，返回假的商品数据。

### 4.2.2.OpenResty监听请求

OpenResty的很多功能都依赖于其目录下的Lua库，需要在nginx.conf中指定依赖库的目录，并导入依赖：

1）添加对OpenResty的Lua模块的加载

修改`/usr/local/openresty/nginx/conf/nginx.conf`文件，在其中的http下面，添加下面代码：

```
#lua 模块
lua_package_path "/usr/local/openresty/lualib/?.lua;;";
#c模块     
lua_package_cpath "/usr/local/openresty/lualib/?.so;;";
```

2）监听/api/item路径

修改`/usr/local/openresty/nginx/conf/nginx.conf`文件，在nginx.conf的server下面，添加对/api/item这个路径的监听：

```
location  /api/item {
    # 默认的响应类型
    default_type application/json;
    # 响应结果由lua/item.lua文件来决定
    content_by_lua_file lua/item.lua;
}
```

这个监听，就类似于SpringMVC中的`@GetMapping("/api/item")`做路径映射。

而`content_by_lua_file lua/item.lua`则相当于调用item.lua这个文件，执行其中的业务，把结果返回给用户。相当于java中调用service。

### 4.2.3.编写item.lua

1）在`/usr/loca/openresty/nginx`目录创建文件夹：lua

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740108541-f49db36d-d57a-4048-861f-6240e41fb56a.png)

2）在`/usr/loca/openresty/nginx/lua`文件夹下，新建文件：item.lua

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740118970-149dc1fe-6dc9-400b-be21-737fe6bd4d37.png)

3）编写item.lua，返回假数据

item.lua中，利用ngx.say()函数返回数据到Response中

```
ngx.say('{"id":10001,"name":"SALSA AIR","title":"RIMOWA 21寸托运箱拉杆箱 SALSA AIR系列果绿色 820.70.36.4","price":17900,"image":"https://m.360buyimg.com/mobilecms/s720x720_jfs/t6934/364/1195375010/84676/e9f2c55f/597ece38N0ddcbc77.jpg!q70.jpg.webp","category":"拉杆箱","brand":"RIMOWA","spec":"","status":1,"createTime":"2019-04-30T16:00:00.000+00:00","updateTime":"2019-04-30T16:00:00.000+00:00","stock":2999,"sold":31290}')
```

4）重新加载配置

```
nginx -s reload
```

刷新商品页面：http://localhost/item.html?id=1001，即可看到效果：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740151261-e53ff810-45a2-4aa7-bbd1-1bffebcd0e45.png)

## 4.3.请求参数处理

上一节中，我们在OpenResty接收前端请求，但是返回的是假数据。

要返回真实数据，必须根据前端传递来的商品id，查询商品信息才可以。

那么如何获取前端传递的商品参数呢？

### 4.3.1.获取参数的API

OpenResty中提供了一些API用来获取不同类型的前端请求参数：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740165843-a23e3392-4bd8-4749-bedc-942be2fc0b57.png)

### 4.3.2.获取参数并返回

在前端发起的ajax请求如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740175757-3e21ae83-b257-4485-b6c6-5fe2cbcf1374.png)

可以看到商品id是以路径占位符方式传递的，因此可以利用正则表达式匹配的方式来获取ID

1）获取商品id

修改`/usr/loca/openresty/nginx/nginx.conf`文件中监听/api/item的代码，利用正则表达式获取ID：

```
location ~ /api/item/(\d+) {
    # 默认的响应类型
    default_type application/json;
    # 响应结果由lua/item.lua文件来决定
    content_by_lua_file lua/item.lua;
}
```

2）拼接ID并返回

修改`/usr/loca/openresty/nginx/lua/item.lua`文件，获取id并拼接到结果中返回：

```
-- 获取商品id
local id = ngx.var[1]
-- 拼接并返回
ngx.say('{"id":' .. id .. ',"name":"SALSA AIR","title":"RIMOWA 21寸托运箱拉杆箱 SALSA AIR系列果绿色 820.70.36.4","price":17900,"image":"https://m.360buyimg.com/mobilecms/s720x720_jfs/t6934/364/1195375010/84676/e9f2c55f/597ece38N0ddcbc77.jpg!q70.jpg.webp","category":"拉杆箱","brand":"RIMOWA","spec":"","status":1,"createTime":"2019-04-30T16:00:00.000+00:00","updateTime":"2019-04-30T16:00:00.000+00:00","stock":2999,"sold":31290}')
```

3）重新加载并测试

运行命令以重新加载OpenResty配置：

```
nginx -s reload
```

刷新页面可以看到结果中已经带上了ID：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740191912-309ffd2f-5e6c-4265-a660-b64ee6c0b1a0.png)

## 4.4.查询Tomcat

拿到商品ID后，本应去缓存中查询商品信息，不过目前我们还未建立nginx、redis缓存。因此，这里我们先根据商品id去tomcat查询商品信息。我们实现如图部分：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740203519-d3e2d1da-8a35-41ff-8e9d-585d903e2125.png)

需要注意的是，我们的OpenResty是在虚拟机，Tomcat是在Windows电脑上。两者IP一定不要搞错了。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740216111-48d59174-6b15-4623-954f-7a357f5053e3.png)

### 4.4.1.发送http请求的API

nginx提供了内部API用以发送http请求：

```
local resp = ngx.location.capture("/path",{
    method = ngx.HTTP_GET,   -- 请求方式
    args = {a=1,b=2},  -- get方式传参数
})
```

返回的响应内容包括：

- resp.status：响应状态码
- resp.header：响应头，是一个table
- resp.body：响应体，就是响应数据

注意：这里的path是路径，并不包含IP和端口。这个请求会被nginx内部的server监听并处理。

但是我们希望这个请求发送到Tomcat服务器，所以还需要编写一个server来对这个路径做反向代理：

```
 location /path {
     # 这里是windows电脑的ip和Java服务端口，需要确保windows防火墙处于关闭状态
     proxy_pass http://192.168.150.1:8081; 
 }
```

原理如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740230097-cf7626ba-ae7c-4b4b-9368-4d29badc7415.png)

### 4.4.2.封装http工具

下面，我们封装一个发送Http请求的工具，基于ngx.location.capture来实现查询tomcat。

1）添加反向代理，到windows的Java服务

因为item-service中的接口都是/item开头，所以我们监听/item路径，代理到windows上的tomcat服务。

修改 `/usr/local/openresty/nginx/conf/nginx.conf`文件，添加一个location：

```
location /item {
    proxy_pass http://192.168.150.1:8081;
}
```

以后，只要我们调用`ngx.location.capture("/item")`，就一定能发送请求到windows的tomcat服务。

2）封装工具类

之前我们说过，OpenResty启动时会加载以下两个目录中的工具文件：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740242061-502dd0e5-51a5-41ba-97fe-96ff73fb29a2.png)

所以，自定义的http工具也需要放到这个目录下。

在`/usr/local/openresty/lualib`目录下，新建一个common.lua文件：

```
vi /usr/local/openresty/lualib/common.lua
```

内容如下:

```
-- 封装函数，发送http请求，并解析响应
local function read_http(path, params)
    local resp = ngx.location.capture(path,{
        method = ngx.HTTP_GET,
        args = params,
    })
    if not resp then
        -- 记录错误信息，返回404
        ngx.log(ngx.ERR, "http请求查询失败, path: ", path , ", args: ", args)
        ngx.exit(404)
    end
    return resp.body
end
-- 将方法导出
local _M = {  
    read_http = read_http
}  
return _M
```

这个工具将read_http函数封装到_M这个table类型的变量中，并且返回，这类似于导出。

使用的时候，可以利用`require('common')`来导入该函数库，这里的common是函数库的文件名。

3）实现商品查询

最后，我们修改`/usr/local/openresty/lua/item.lua`文件，利用刚刚封装的函数库实现对tomcat的查询：

```
-- 引入自定义common工具模块，返回值是common中返回的 _M
local common = require("common")
-- 从 common中获取read_http这个函数
local read_http = common.read_http
-- 获取路径参数
local id = ngx.var[1]
-- 根据id查询商品
local itemJSON = read_http("/item/".. id, nil)
-- 根据id查询商品库存
local itemStockJSON = read_http("/item/stock/".. id, nil)
```

这里查询到的结果是json字符串，并且包含商品、库存两个json字符串，页面最终需要的是把两个json拼接为一个json：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740257231-7b4073e0-9c7f-493a-8524-f0a3dfbb4ffa.png)

这就需要我们先把JSON变为lua的table，完成数据整合后，再转为JSON。

### 4.4.3.CJSON工具类

OpenResty提供了一个cjson的模块用来处理JSON的序列化和反序列化。

官方地址： [https://github.com/openresty/lua-cjson/](https://github.com/openresty/lua-cjson/)

1）引入cjson模块：

```
local cjson = require "cjson"
```

2）序列化：

```
local obj = {
    name = 'jack',
    age = 21
}
-- 把 table 序列化为 json
local json = cjson.encode(obj)
```

3）反序列化：

```
local json = '{"name": "jack", "age": 21}'
-- 反序列化 json为 table
local obj = cjson.decode(json);
print(obj.name)
```

### 4.4.4.实现Tomcat查询

下面，我们修改之前的item.lua中的业务，添加json处理功能：

```
-- 导入common函数库
local common = require('common')
local read_http = common.read_http
-- 导入cjson库
local cjson = require('cjson')

-- 获取路径参数
local id = ngx.var[1]
-- 根据id查询商品
local itemJSON = read_http("/item/".. id, nil)
-- 根据id查询商品库存
local itemStockJSON = read_http("/item/stock/".. id, nil)

-- JSON转化为lua的table
local item = cjson.decode(itemJSON)
local stock = cjson.decode(stockJSON)

-- 组合数据
item.stock = stock.stock
item.sold = stock.sold

-- 把item序列化为json 返回结果
ngx.say(cjson.encode(item))
```

### 4.4.5.基于ID负载均衡

刚才的代码中，我们的tomcat是单机部署。而实际开发中，tomcat一定是集群模式：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740275640-facca9ba-2b83-4948-8604-0a7bb270d56e.png)

因此，OpenResty需要对tomcat集群做负载均衡。

而默认的负载均衡规则是轮询模式，当我们查询/item/10001时：

- 第一次会访问8081端口的tomcat服务，在该服务内部就形成了JVM进程缓存
- 第二次会访问8082端口的tomcat服务，该服务内部没有JVM缓存（因为JVM缓存无法共享），会查询数据库
- ...

你看，因为轮询的原因，第一次查询8081形成的JVM缓存并未生效，直到下一次再次访问到8081时才可以生效，缓存命中率太低了。

怎么办？

如果能让同一个商品，每次查询时都访问同一个tomcat服务，那么JVM缓存就一定能生效了。

也就是说，我们需要根据商品id做负载均衡，而不是轮询。

#### 1）原理

nginx提供了基于请求路径做负载均衡的算法：

nginx根据请求路径做hash运算，把得到的数值对tomcat服务的数量取余，余数是几，就访问第几个服务，实现负载均衡。

例如：

- 我们的请求路径是 /item/10001
- tomcat总数为2台（8081、8082）
- 对请求路径/item/1001做hash运算求余的结果为1
- 则访问第一个tomcat服务，也就是8081

只要id不变，每次hash运算结果也不会变，那就可以保证同一个商品，一直访问同一个tomcat服务，确保JVM缓存生效。

#### 2）实现

修改`/usr/local/openresty/nginx/conf/nginx.conf`文件，实现基于ID做负载均衡。

首先，定义tomcat集群，并设置基于路径做负载均衡：

```
upstream tomcat-cluster {
    hash $request_uri;
    server 192.168.150.1:8081;
    server 192.168.150.1:8082;
}
```

然后，修改对tomcat服务的反向代理，目标指向tomcat集群：

```
location /item {
    proxy_pass http://tomcat-cluster;
}
```

重新加载OpenResty

```
nginx -s reload
```

#### 3）测试

启动两台tomcat服务：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740294326-27286f1a-ca55-4430-801a-308672db063e.png)

同时启动：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740301703-a7cbb2b8-31f7-4015-b6ba-8476b496a655.png)

清空日志后，再次访问页面，可以看到不同id的商品，访问到了不同的tomcat服务：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740452058-a0e5735a-819c-4c0c-b614-3d4afdbd5224.png)

## 4.5.Redis缓存预热

Redis缓存会面临冷启动问题：

冷启动：服务刚刚启动时，Redis中并没有缓存，如果所有商品数据都在第一次查询时添加缓存，可能会给数据库带来较大压力。

缓存预热：在实际开发中，我们可以利用大数据统计用户访问的热点数据，在项目启动时将这些热点数据提前查询并保存到Redis中。

我们数据量较少，并且没有数据统计相关功能，目前可以在启动时将所有数据都放入缓存中。

1）利用Docker安装Redis

```
docker run --name redis -p 6379:6379 -d redis redis-server --appendonly yes
```

2）在item-service服务中引入Redis依赖

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

3）配置Redis地址

```
spring:
  redis:
    host: 192.168.150.101
```

4）编写初始化类

缓存预热需要在项目启动时完成，并且必须是拿到RedisTemplate之后。

这里我们利用InitializingBean接口来实现，因为InitializingBean可以在对象被Spring创建并且成员变量全部注入后执行。

```
package com.heima.item.config;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.heima.item.pojo.Item;
import com.heima.item.pojo.ItemStock;
import com.heima.item.service.IItemService;
import com.heima.item.service.IItemStockService;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.stereotype.Component;

import java.util.List;

@Component
public class RedisHandler implements InitializingBean {

    @Autowired
    private StringRedisTemplate redisTemplate;

    @Autowired
    private IItemService itemService;
    @Autowired
    private IItemStockService stockService;

    private static final ObjectMapper MAPPER = new ObjectMapper();

    @Override
    public void afterPropertiesSet() throws Exception {
        // 初始化缓存
        // 1.查询商品信息
        List<Item> itemList = itemService.list();
        // 2.放入缓存
        for (Item item : itemList) {
            // 2.1.item序列化为JSON
            String json = MAPPER.writeValueAsString(item);
            // 2.2.存入redis
            redisTemplate.opsForValue().set("item:id:" + item.getId(), json);
        }

        // 3.查询商品库存信息
        List<ItemStock> stockList = stockService.list();
        // 4.放入缓存
        for (ItemStock stock : stockList) {
            // 2.1.item序列化为JSON
            String json = MAPPER.writeValueAsString(stock);
            // 2.2.存入redis
            redisTemplate.opsForValue().set("item:stock:id:" + stock.getId(), json);
        }
    }
}
```

## 4.6.查询Redis缓存

现在，Redis缓存已经准备就绪，我们可以再OpenResty中实现查询Redis的逻辑了。如下图红框所示：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740470094-56f04a6a-42b5-4054-b504-9907e3088f60.png)

当请求进入OpenResty之后：

- 优先查询Redis缓存
- 如果Redis缓存未命中，再查询Tomcat

### 4.6.1.封装Redis工具

OpenResty提供了操作Redis的模块，我们只要引入该模块就能直接使用。但是为了方便，我们将Redis操作封装到之前的common.lua工具库中。

修改`/usr/local/openresty/lualib/common.lua`文件：

1）引入Redis模块，并初始化Redis对象

```
-- 导入redis
local redis = require('resty.redis')
-- 初始化redis
local red = redis:new()
red:set_timeouts(1000, 1000, 1000)
```

2）封装函数，用来释放Redis连接，其实是放入连接池

```
-- 关闭redis连接的工具方法，其实是放入连接池
local function close_redis(red)
    local pool_max_idle_time = 10000 -- 连接的空闲时间，单位是毫秒
    local pool_size = 100 --连接池大小
    local ok, err = red:set_keepalive(pool_max_idle_time, pool_size)
    if not ok then
        ngx.log(ngx.ERR, "放入redis连接池失败: ", err)
    end
end
```

3）封装函数，根据key查询Redis数据

```
-- 查询redis的方法 ip和port是redis地址，key是查询的key
local function read_redis(ip, port, key)
    -- 获取一个连接
    local ok, err = red:connect(ip, port)
    if not ok then
        ngx.log(ngx.ERR, "连接redis失败 : ", err)
        return nil
    end
    -- 查询redis
    local resp, err = red:get(key)
    -- 查询失败处理
    if not resp then
        ngx.log(ngx.ERR, "查询Redis失败: ", err, ", key = " , key)
    end
    --得到的数据为空处理
    if resp == ngx.null then
        resp = nil
        ngx.log(ngx.ERR, "查询Redis数据为空, key = ", key)
    end
    close_redis(red)
    return resp
end
```

4）导出

```
-- 将方法导出
local _M = {  
    read_http = read_http,
    read_redis = read_redis
}  
return _M
```

完整的common.lua：

```
-- 导入redis
local redis = require('resty.redis')
-- 初始化redis
local red = redis:new()
red:set_timeouts(1000, 1000, 1000)

-- 关闭redis连接的工具方法，其实是放入连接池
local function close_redis(red)
    local pool_max_idle_time = 10000 -- 连接的空闲时间，单位是毫秒
    local pool_size = 100 --连接池大小
    local ok, err = red:set_keepalive(pool_max_idle_time, pool_size)
    if not ok then
        ngx.log(ngx.ERR, "放入redis连接池失败: ", err)
    end
end

-- 查询redis的方法 ip和port是redis地址，key是查询的key
local function read_redis(ip, port, key)
    -- 获取一个连接
    local ok, err = red:connect(ip, port)
    if not ok then
        ngx.log(ngx.ERR, "连接redis失败 : ", err)
        return nil
    end
    -- 查询redis
    local resp, err = red:get(key)
    -- 查询失败处理
    if not resp then
        ngx.log(ngx.ERR, "查询Redis失败: ", err, ", key = " , key)
    end
    --得到的数据为空处理
    if resp == ngx.null then
        resp = nil
        ngx.log(ngx.ERR, "查询Redis数据为空, key = ", key)
    end
    close_redis(red)
    return resp
end

-- 封装函数，发送http请求，并解析响应
local function read_http(path, params)
    local resp = ngx.location.capture(path,{
        method = ngx.HTTP_GET,
        args = params,
    })
    if not resp then
        -- 记录错误信息，返回404
        ngx.log(ngx.ERR, "http查询失败, path: ", path , ", args: ", args)
        ngx.exit(404)
    end
    return resp.body
end
-- 将方法导出
local _M = {  
    read_http = read_http,
    read_redis = read_redis
}  
return _M
```

### 4.6.2.实现Redis查询

接下来，我们就可以去修改item.lua文件，实现对Redis的查询了。

查询逻辑是：

- 根据id查询Redis
- 如果查询失败则继续查询Tomcat
- 将查询结果返回

1）修改`/usr/local/openresty/lua/item.lua`文件，添加一个查询函数：

```
-- 导入common函数库
local common = require('common')
local read_http = common.read_http
local read_redis = common.read_redis
-- 封装查询函数
function read_data(key, path, params)
    -- 查询本地缓存
    local val = read_redis("127.0.0.1", 6379, key)
    -- 判断查询结果
    if not val then
        ngx.log(ngx.ERR, "redis查询失败，尝试查询http， key: ", key)
        -- redis查询失败，去查询http
        val = read_http(path, params)
    end
    -- 返回数据
    return val
end
```

2）而后修改商品查询、库存查询的业务：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740493834-87eb769b-5eea-42b1-9d0f-f7b53cb098f5.png)

3）完整的item.lua代码：

```
-- 导入common函数库
local common = require('common')
local read_http = common.read_http
local read_redis = common.read_redis
-- 导入cjson库
local cjson = require('cjson')

-- 封装查询函数
function read_data(key, path, params)
    -- 查询本地缓存
    local val = read_redis("127.0.0.1", 6379, key)
    -- 判断查询结果
    if not val then
        ngx.log(ngx.ERR, "redis查询失败，尝试查询http， key: ", key)
        -- redis查询失败，去查询http
        val = read_http(path, params)
    end
    -- 返回数据
    return val
end

-- 获取路径参数
local id = ngx.var[1]

-- 查询商品信息
local itemJSON = read_data("item:id:" .. id,  "/item/" .. id, nil)
-- 查询库存信息
local stockJSON = read_data("item:stock:id:" .. id, "/item/stock/" .. id, nil)

-- JSON转化为lua的table
local item = cjson.decode(itemJSON)
local stock = cjson.decode(stockJSON)
-- 组合数据
item.stock = stock.stock
item.sold = stock.sold

-- 把item序列化为json 返回结果
ngx.say(cjson.encode(item))
```

## 4.7.Nginx本地缓存

现在，整个多级缓存中只差最后一环，也就是nginx的本地缓存了。如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740508798-737895ee-d0f9-4d7f-9b8a-3cb8466530f6.png)

### 4.7.1.本地缓存API

OpenResty为Nginx提供了shard dict的功能，可以在nginx的多个worker之间共享数据，实现缓存功能。

1）开启共享字典，在nginx.conf的http下添加配置：

```
 # 共享字典，也就是本地缓存，名称叫做：item_cache，大小150m
 lua_shared_dict item_cache 150m;
```

2）操作共享字典：

```
-- 获取本地缓存对象
local item_cache = ngx.shared.item_cache
-- 存储, 指定key、value、过期时间，单位s，默认为0代表永不过期
item_cache:set('key', 'value', 1000)
-- 读取
local val = item_cache:get('key')
```

### 4.7.2.实现本地缓存查询

1）修改`/usr/local/openresty/lua/item.lua`文件，修改read_data查询函数，添加本地缓存逻辑：

```
-- 导入共享词典，本地缓存
local item_cache = ngx.shared.item_cache

-- 封装查询函数
function read_data(key, expire, path, params)
    -- 查询本地缓存
    local val = item_cache:get(key)
    if not val then
        ngx.log(ngx.ERR, "本地缓存查询失败，尝试查询Redis， key: ", key)
        -- 查询redis
        val = read_redis("127.0.0.1", 6379, key)
        -- 判断查询结果
        if not val then
            ngx.log(ngx.ERR, "redis查询失败，尝试查询http， key: ", key)
            -- redis查询失败，去查询http
            val = read_http(path, params)
        end
    end
    -- 查询成功，把数据写入本地缓存
    item_cache:set(key, val, expire)
    -- 返回数据
    return val
end
```

2）修改item.lua中查询商品和库存的业务，实现最新的read_data函数：

其实就是多了缓存时间参数，过期后nginx缓存会自动删除，下次访问即可更新缓存。

这里给商品基本信息设置超时时间为30分钟，库存为1分钟。

因为库存更新频率较高，如果缓存时间过长，可能与数据库差异较大。

3）完整的item.lua文件：

```
-- 导入common函数库
local common = require('common')
local read_http = common.read_http
local read_redis = common.read_redis
-- 导入cjson库
local cjson = require('cjson')
-- 导入共享词典，本地缓存
local item_cache = ngx.shared.item_cache

-- 封装查询函数
function read_data(key, expire, path, params)
    -- 查询本地缓存
    local val = item_cache:get(key)
    if not val then
        ngx.log(ngx.ERR, "本地缓存查询失败，尝试查询Redis， key: ", key)
        -- 查询redis
        val = read_redis("127.0.0.1", 6379, key)
        -- 判断查询结果
        if not val then
            ngx.log(ngx.ERR, "redis查询失败，尝试查询http， key: ", key)
            -- redis查询失败，去查询http
            val = read_http(path, params)
        end
    end
    -- 查询成功，把数据写入本地缓存
    item_cache:set(key, val, expire)
    -- 返回数据
    return val
end

-- 获取路径参数
local id = ngx.var[1]

-- 查询商品信息
local itemJSON = read_data("item:id:" .. id, 1800,  "/item/" .. id, nil)
-- 查询库存信息
local stockJSON = read_data("item:stock:id:" .. id, 60, "/item/stock/" .. id, nil)

-- JSON转化为lua的table
local item = cjson.decode(itemJSON)
local stock = cjson.decode(stockJSON)
-- 组合数据
item.stock = stock.stock
item.sold = stock.sold

-- 把item序列化为json 返回结果
ngx.say(cjson.encode(item))
```

# 5.缓存同步

大多数情况下，浏览器查询到的都是缓存数据，如果缓存数据与数据库数据存在较大差异，可能会产生比较严重的后果。

所以我们必须保证数据库数据、缓存数据的一致性，这就是缓存与数据库的同步。

## 5.1.数据同步策略

缓存数据同步的常见方式有三种：

设置有效期：给缓存设置有效期，到期后自动删除。再次查询时更新

- 优势：简单、方便
- 缺点：时效性差，缓存过期之前可能不一致
- 场景：更新频率较低，时效性要求低的业务

同步双写：在修改数据库的同时，直接修改缓存

- 优势：时效性强，缓存与数据库强一致
- 缺点：有代码侵入，耦合度高；
- 场景：对一致性、时效性要求较高的缓存数据

异步通知：修改数据库时发送事件通知，相关服务监听到通知后修改缓存数据

- 优势：低耦合，可以同时通知多个缓存服务
- 缺点：时效性一般，可能存在中间不一致状态
- 场景：时效性要求一般，有多个服务需要同步

而异步实现又可以基于MQ或者Canal来实现：

1）基于MQ的异步通知：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740532632-2363981f-2cbd-4d45-b35c-3655bc2f0637.png)

解读：

- 商品服务完成对数据的修改后，只需要发送一条消息到MQ中。
- 缓存服务监听MQ消息，然后完成对缓存的更新

依然有少量的代码侵入。

2）基于Canal的通知

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740548504-6d9e31e3-3933-458f-97a2-ec96f644a97b.png)

解读：

- 商品服务完成商品修改后，业务直接结束，没有任何代码侵入
- Canal监听MySQL变化，当发现变化后，立即通知缓存服务
- 缓存服务接收到canal通知，更新缓存

代码零侵入

## 5.2.安装Canal

### 5.2.1.认识Canal

Canal [kə'næl]，译意为水道/管道/沟渠，canal是阿里巴巴旗下的一款开源项目，基于Java开发。基于数据库增量日志解析，提供增量数据订阅&消费。GitHub的地址：[https://github.com/alibaba/canal](https://github.com/alibaba/canal)

Canal是基于mysql的主从同步来实现的，MySQL主从同步的原理如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740568518-2427599a-f963-4805-b60e-b2a0498917ed.png)

- 1）MySQL master 将数据变更写入二进制日志( binary log），其中记录的数据叫做binary log events
- 2）MySQL slave 将 master 的 binary log events拷贝到它的中继日志(relay log)
- 3）MySQL slave 重放 relay log 中事件，将数据变更反映它自己的数据

而Canal就是把自己伪装成MySQL的一个slave节点，从而监听master的binary log变化。再把得到的变化信息通知给Canal的客户端，进而完成对其它数据库的同步。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740579544-03e89a0c-4d9d-4f8e-a5cf-d14db6ab2fdf.png)

### 5.2.2.安装Canal

安装和配置Canal参考课前资料文档：

1）开启MySQL主从

Canal是基于MySQL的主从同步功能，因此必须先开启MySQL的主从功能才可以。

这里以之前用Docker运行的mysql为例：

①开启binlog

打开mysql容器挂载的日志文件，我的在`/tmp/mysql/conf`目录:

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667783762229-f5da6ba5-21b5-48d1-9e2e-1e0ad93482e1.png)

修改文件：

```
vi /tmp/mysql/conf/my.cnf
```

添加内容：

```
log-bin=/var/lib/mysql/mysql-bin
binlog-do-db=heima
```

配置解读：

- `log-bin=/var/lib/mysql/mysql-bin`：设置binary log文件的存放地址和文件名，叫做mysql-bin
- `binlog-do-db=heima`：指定对哪个database记录binary log events，这里记录heima这个库

最终效果：

```
[mysqld]
skip-name-resolve
character_set_server=utf8
datadir=/var/lib/mysql
server-id=1000
log-bin=/var/lib/mysql/mysql-bin
binlog-do-db=heima
```

②设置用户权限

接下来添加一个仅用于数据同步的账户，出于安全考虑，这里仅提供对heima这个库的操作权限。

```
create user canal@'%' IDENTIFIED by 'canal';
GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT,SUPER ON *.* TO 'canal'@'%' identified by 'canal';
FLUSH PRIVILEGES;
```

重启mysql容器即可

```
docker restart mysql
```

测试设置是否成功：在mysql控制台，或者Navicat中，输入命令：

```
show master status;
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667783777118-74005829-e31b-402c-af29-4d8a70a1916a.png)

2）安装Canal

①创建网络

我们需要创建一个网络，将MySQL、Canal、MQ放到同一个Docker网络中：

```
docker network create heima
```

让mysql加入这个网络：

```
docker network connect heima mysql
```

②安装Canal

课前资料中提供了canal的镜像压缩包:

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667783790545-bd0934b0-e0cd-42f0-9fda-83d2b83571cc.png)

大家可以上传到虚拟机，然后通过命令导入：

```
docker load -i canal.tar
```

然后运行命令创建Canal容器：

```
docker run -p 11111:11111 --name canal \
-e canal.destinations=heima \
-e canal.instance.master.address=mysql:3306  \
-e canal.instance.dbUsername=canal  \
-e canal.instance.dbPassword=canal  \
-e canal.instance.connectionCharset=UTF-8 \
-e canal.instance.tsdb.enable=true \
-e canal.instance.gtidon=false  \
-e canal.instance.filter.regex=heima\\..* \
--network heima \
-d canal/canal-server:v1.1.5
```

说明:

- `-p 11111:11111`：这是canal的默认监听端口
- `-e canal.instance.master.address=mysql:3306`：数据库地址和端口，如果不知道mysql容器地址，可以通过`docker inspect 容器id`来查看
- `-e canal.instance.dbUsername=canal`：数据库用户名
- `-e canal.instance.dbPassword=canal` ：数据库密码
- `-e canal.instance.filter.regex=`：要监听的表名称

表名称监听支持的语法：

```
mysql 数据解析关注的表，Perl正则表达式.
多个正则之间以逗号(,)分隔，转义符需要双斜杠(\\) 
常见例子：
1.  所有表：.*   or  .*\\..*
2.  canal schema下所有表： canal\\..*
3.  canal下的以canal打头的表：canal\\.canal.*
4.  canal schema下的一张表：canal.test1
5.  多个规则组合使用然后以逗号隔开：canal\\..*,mysql.test1,mysql.test2
```

## 5.3.监听Canal

Canal提供了各种语言的客户端，当Canal监听到binlog变化时，会通知Canal的客户端。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740596209-dea27afe-6b20-4eee-9a2f-15a5faa43fd3.png)

我们可以利用Canal提供的Java客户端，监听Canal通知消息。当收到变化的消息时，完成对缓存的更新。

不过这里我们会使用GitHub上的第三方开源的canal-starter客户端。地址：[https://github.com/NormanGyllenhaal/canal-client](https://github.com/NormanGyllenhaal/canal-client)

与SpringBoot完美整合，自动装配，比官方客户端要简单好用很多。

### 5.3.1.引入依赖：

```
<dependency>
    <groupId>top.javatool</groupId>
    <artifactId>canal-spring-boot-starter</artifactId>
    <version>1.2.1-RELEASE</version>
</dependency>
```

### 5.3.2.编写配置：

```
canal:
  destination: heima # canal的集群名字，要与安装canal时设置的名称一致
  server: 192.168.150.101:11111 # canal服务地址
```

### 5.3.3.修改Item实体类

通过@Id、@Column、等注解完成Item与数据库表字段的映射：

```
package com.heima.item.pojo;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableField;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import lombok.Data;
import org.springframework.data.annotation.Id;
import org.springframework.data.annotation.Transient;

import javax.persistence.Column;
import java.util.Date;

@Data
@TableName("tb_item")
public class Item {
    @TableId(type = IdType.AUTO)
    @Id
    private Long id;//商品id
    @Column(name = "name")
    private String name;//商品名称
    private String title;//商品标题
    private Long price;//价格（分）
    private String image;//商品图片
    private String category;//分类名称
    private String brand;//品牌名称
    private String spec;//规格
    private Integer status;//商品状态 1-正常，2-下架
    private Date createTime;//创建时间
    private Date updateTime;//更新时间
    @TableField(exist = false)
    @Transient
    private Integer stock;
    @TableField(exist = false)
    @Transient
    private Integer sold;
}
```

### 5.3.4.编写监听器

通过实现`EntryHandler<T>`接口编写监听器，监听Canal消息。注意两点：

- 实现类通过`@CanalTable("tb_item")`指定监听的表信息
- EntryHandler的泛型是与表对应的实体类

```
package com.heima.item.canal;

import com.github.benmanes.caffeine.cache.Cache;
import com.heima.item.config.RedisHandler;
import com.heima.item.pojo.Item;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import top.javatool.canal.client.annotation.CanalTable;
import top.javatool.canal.client.handler.EntryHandler;

@CanalTable("tb_item")
@Component
public class ItemHandler implements EntryHandler<Item> {

    @Autowired
    private RedisHandler redisHandler;
    @Autowired
    private Cache<Long, Item> itemCache;

    @Override
    public void insert(Item item) {
        // 写数据到JVM进程缓存
        itemCache.put(item.getId(), item);
        // 写数据到redis
        redisHandler.saveItem(item);
    }

    @Override
    public void update(Item before, Item after) {
        // 写数据到JVM进程缓存
        itemCache.put(after.getId(), after);
        // 写数据到redis
        redisHandler.saveItem(after);
    }

    @Override
    public void delete(Item item) {
        // 删除数据到JVM进程缓存
        itemCache.invalidate(item.getId());
        // 删除数据到redis
        redisHandler.deleteItemById(item.getId());
    }
}
```

在这里对Redis的操作都封装到了RedisHandler这个对象中，是我们之前做缓存预热时编写的一个类，内容如下：

```
package com.heima.item.config;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.heima.item.pojo.Item;
import com.heima.item.pojo.ItemStock;
import com.heima.item.service.IItemService;
import com.heima.item.service.IItemStockService;
import org.springframework.beans.factory.InitializingBean;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.stereotype.Component;

import java.util.List;

@Component
public class RedisHandler implements InitializingBean {

    @Autowired
    private StringRedisTemplate redisTemplate;

    @Autowired
    private IItemService itemService;
    @Autowired
    private IItemStockService stockService;

    private static final ObjectMapper MAPPER = new ObjectMapper();

    @Override
    public void afterPropertiesSet() throws Exception {
        // 初始化缓存
        // 1.查询商品信息
        List<Item> itemList = itemService.list();
        // 2.放入缓存
        for (Item item : itemList) {
            // 2.1.item序列化为JSON
            String json = MAPPER.writeValueAsString(item);
            // 2.2.存入redis
            redisTemplate.opsForValue().set("item:id:" + item.getId(), json);
        }

        // 3.查询商品库存信息
        List<ItemStock> stockList = stockService.list();
        // 4.放入缓存
        for (ItemStock stock : stockList) {
            // 2.1.item序列化为JSON
            String json = MAPPER.writeValueAsString(stock);
            // 2.2.存入redis
            redisTemplate.opsForValue().set("item:stock:id:" + stock.getId(), json);
        }
    }

    public void saveItem(Item item) {
        try {
            String json = MAPPER.writeValueAsString(item);
            redisTemplate.opsForValue().set("item:id:" + item.getId(), json);
        } catch (JsonProcessingException e) {
            throw new RuntimeException(e);
        }
    }

    public void deleteItemById(Long id) {
        redisTemplate.delete("item:id:" + id);
    }
}
```

# Redis高级篇之最佳实践

今日内容**

- Redis键值设计
- 批处理优化
- 服务端优化
- 集群最佳实践

## 1、Redis键值设计

### 1.1、优雅的key结构

Redis的Key虽然可以自定义，但最好遵循下面的几个最佳实践约定：

- 遵循基本格式：[业务名称]:[数据名]:[id]
- 长度不超过44字节
- 不包含特殊字符

例如：我们的登录业务，保存用户信息，其key可以设计成如下格式：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740701176-19f2712f-9c44-4ea5-a65c-84670e61bf96.png)

这样设计的好处：

- 可读性强
- 避免key冲突
- 方便管理
- 更节省内存： key是string类型，底层编码包含int、embstr和raw三种。embstr在小于44字节使用，采用连续内存空间，内存占用更小。当字节数大于44字节时，会转为raw模式存储，在raw模式下，内存空间不是连续的，而是采用一个指针指向了另外一段内存空间，在这段空间里存储SDS内容，这样空间不连续，访问的时候性能也就会收到影响，还有可能产生内存碎片

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740713539-af76a4fd-643f-470d-8a8c-eae39fb6b7df.png)

### 1.2、拒绝BigKey

BigKey通常以Key的大小和Key中成员的数量来综合判定，例如：

- Key本身的数据量过大：一个String类型的Key，它的值为5 MB
- Key中的成员数过多：一个ZSET类型的Key，它的成员数量为10,000个
- Key中成员的数据量过大：一个Hash类型的Key，它的成员数量虽然只有1,000个但这些成员的Value（值）总大小为100 MB

那么如何判断元素的大小呢？redis也给我们提供了命令

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740725592-e024d460-ce6e-4d0b-94ac-e82e9960f51b.png)

推荐值：

- 单个key的value小于10KB
- 对于集合类型的key，建议元素数量小于1000

#### 1.2.1、BigKey的危害

- 网络阻塞

- 对BigKey执行读请求时，少量的QPS就可能导致带宽使用率被占满，导致Redis实例，乃至所在物理机变慢

- 数据倾斜

- BigKey所在的Redis实例内存使用率远超其他实例，无法使数据分片的内存资源达到均衡

- Redis阻塞

- 对元素较多的hash、list、zset等做运算会耗时较旧，使主线程被阻塞

- CPU压力

- 对BigKey的数据序列化和反序列化会导致CPU的使用率飙升，影响Redis实例和本机其它应用

#### 1.2.2、如何发现BigKey

##### ①redis-cli --bigkeys

利用redis-cli提供的--bigkeys参数，可以遍历分析所有key，并返回Key的整体统计信息与每个数据的Top1的big key

命令：`redis-cli -a 密码 --bigkeys`

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740738659-7c54b11a-41a5-4302-951f-8ca712520774.png)

##### ②scan扫描

自己编程，利用scan扫描Redis中的所有key，利用strlen、hlen等命令判断key的长度（此处不建议使用MEMORY USAGE）

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740754778-5077dd11-304a-40d0-b1df-c5782672abf8.png)

scan 命令调用完后每次会返回2个元素，第一个是下一次迭代的光标，第一次光标会设置为0，当最后一次scan 返回的光标等于0时，表示整个scan遍历结束了，第二个返回的是List，一个匹配的key的数组

```
import com.heima.jedis.util.JedisConnectionFactory;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.ScanResult;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class JedisTest {
    private Jedis jedis;

    @BeforeEach
    void setUp() {
        // 1.建立连接
        // jedis = new Jedis("192.168.150.101", 6379);
        jedis = JedisConnectionFactory.getJedis();
        // 2.设置密码
        jedis.auth("123321");
        // 3.选择库
        jedis.select(0);
    }

    final static int STR_MAX_LEN = 10 * 1024;
    final static int HASH_MAX_LEN = 500;

    @Test
    void testScan() {
        int maxLen = 0;
        long len = 0;

        String cursor = "0";
        do {
            // 扫描并获取一部分key
            ScanResult<String> result = jedis.scan(cursor);
            // 记录cursor
            cursor = result.getCursor();
            List<String> list = result.getResult();
            if (list == null || list.isEmpty()) {
                break;
            }
            // 遍历
            for (String key : list) {
                // 判断key的类型
                String type = jedis.type(key);
                switch (type) {
                    case "string":
                        len = jedis.strlen(key);
                        maxLen = STR_MAX_LEN;
                        break;
                    case "hash":
                        len = jedis.hlen(key);
                        maxLen = HASH_MAX_LEN;
                        break;
                    case "list":
                        len = jedis.llen(key);
                        maxLen = HASH_MAX_LEN;
                        break;
                    case "set":
                        len = jedis.scard(key);
                        maxLen = HASH_MAX_LEN;
                        break;
                    case "zset":
                        len = jedis.zcard(key);
                        maxLen = HASH_MAX_LEN;
                        break;
                    default:
                        break;
                }
                if (len >= maxLen) {
                    System.out.printf("Found big key : %s, type: %s, length or size: %d %n", key, type, len);
                }
            }
        } while (!cursor.equals("0"));
    }
    
    @AfterEach
    void tearDown() {
        if (jedis != null) {
            jedis.close();
        }
    }

}
```

##### ③第三方工具

- 利用第三方工具，如 Redis-Rdb-Tools 分析RDB快照文件，全面分析内存使用情况
- [https://github.com/sripathikrishnan/redis-rdb-tools](https://github.com/sripathikrishnan/redis-rdb-tools)

##### ④网络监控

- 自定义工具，监控进出Redis的网络数据，超出预警值时主动告警
- 一般阿里云搭建的云服务器就有相关监控页面

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740770286-20001824-b145-49be-909a-783a18c18780.png)

#### 1.2.3、如何删除BigKey

BigKey内存占用较多，即便时删除这样的key也需要耗费很长时间，导致Redis主线程阻塞，引发一系列问题。

- redis 3.0 及以下版本

- 如果是集合类型，则遍历BigKey的元素，先逐个删除子元素，最后删除BigKey

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740783671-2cb345a8-c7b5-4eff-8b59-fcf2c853ca8b.png)

- Redis 4.0以后

- Redis在4.0后提供了异步删除的命令：unlink

### 1.3、恰当的数据类型

#### 例1：比如存储一个User对象，我们有三种存储方式：

##### ①方式一：json字符串

|   |   |
|---|---|
|user:1|{"name": "Jack", "age": 21}|

优点：实现简单粗暴

缺点：数据耦合，不够灵活

##### ②方式二：字段打散

|   |   |
|---|---|
|user:1:name|Jack|
|user:1:age|21|

优点：可以灵活访问对象任意字段

缺点：占用空间大、没办法做统一控制

##### ③方式三：hash（推荐）

|   |   |   |
|---|---|---|
|user:1|name|jack|
|age|21|

优点：底层使用ziplist，空间占用小，可以灵活访问对象的任意字段

缺点：代码相对复杂

#### 例2：假如有hash类型的key，其中有100万对field和value，field是自增id，这个key存在什么问题？如何优化？

|   |   |   |
|---|---|---|
|key|field|value|
|someKey|id:0|value0|
|.....|.....|
|id:999999|value999999|

存在的问题：

- hash的entry数量超过500时，会使用哈希表而不是ZipList，内存占用较多

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740929334-1bbeda59-8883-48df-9fdf-6938dbfe5a8c.png)

- 可以通过hash-max-ziplist-entries配置entry上限。但是如果entry过多就会导致BigKey问题

##### 方案一

拆分为string类型

|   |   |
|---|---|
|key|value|
|id:0|value0|
|.....|.....|
|id:999999|value999999|

存在的问题：

- string结构底层没有太多内存优化，内存占用较多

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740953200-c5059a1e-3073-4bbd-a365-df65e21edbf9.png)

- 想要批量获取这些数据比较麻烦

##### 方案二

拆分为小的hash，将 id / 100 作为key， 将id % 100 作为field，这样每100个元素为一个Hash

|   |   |   |
|---|---|---|
|key|field|value|
|key:0|id:00|value0|
|.....|.....|
|id:99|value99|
|key:1|id:00|value100|
|.....|.....|
|id:99|value199|
|....|   |   |
|key:9999|id:00|value999900|
|.....|.....|
|id:99|value999999|

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740968737-f51e2833-cf73-41e7-8f65-088192ca3346.png)

```
package com.heima.test;

import com.heima.jedis.util.JedisConnectionFactory;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.Pipeline;
import redis.clients.jedis.ScanResult;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class JedisTest {
    private Jedis jedis;

    @BeforeEach
    void setUp() {
        // 1.建立连接
        // jedis = new Jedis("192.168.150.101", 6379);
        jedis = JedisConnectionFactory.getJedis();
        // 2.设置密码
        jedis.auth("123321");
        // 3.选择库
        jedis.select(0);
    }

    @Test
    void testSetBigKey() {
        Map<String, String> map = new HashMap<>();
        for (int i = 1; i <= 650; i++) {
            map.put("hello_" + i, "world!");
        }
        jedis.hmset("m2", map);
    }

    @Test
    void testBigHash() {
        Map<String, String> map = new HashMap<>();
        for (int i = 1; i <= 100000; i++) {
            map.put("key_" + i, "value_" + i);
        }
        jedis.hmset("test:big:hash", map);
    }

    @Test
    void testBigString() {
        for (int i = 1; i <= 100000; i++) {
            jedis.set("test:str:key_" + i, "value_" + i);
        }
    }

    @Test
    void testSmallHash() {
        int hashSize = 100;
        Map<String, String> map = new HashMap<>(hashSize);
        for (int i = 1; i <= 100000; i++) {
            int k = (i - 1) / hashSize;
            int v = i % hashSize;
            map.put("key_" + v, "value_" + v);
            if (v == 0) {
                jedis.hmset("test:small:hash_" + k, map);
            }
        }
    }

    @AfterEach
    void tearDown() {
        if (jedis != null) {
            jedis.close();
        }
    }
}
```

### 1.4、总结

- Key的最佳实践

- 固定格式：[业务名]:[数据名]:[id]
- 足够简短：不超过44字节
- 不包含特殊字符

- Value的最佳实践：

- 合理的拆分数据，拒绝BigKey
- 选择合适数据结构
- Hash结构的entry数量不要超过1000
- 设置合理的超时时间

## 2、批处理优化

### 2.1、Pipeline

#### 2.1.1、我们的客户端与redis服务器是这样交互的

单个命令的执行流程

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740982734-fb49bd78-69ee-46de-8bff-55741e73fbdc.png)

N条命令的执行流程

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667740993536-994a426a-370b-412a-9a3c-0475e3f5f346.png)

redis处理指令是很快的，主要花费的时候在于网络传输。于是乎很容易想到将多条指令批量的传输给redis

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741003987-729a30e9-fcb2-4c83-8531-0e0dd0fd9bc3.png)

#### 2.1.2、MSet

Redis提供了很多Mxxx这样的命令，可以实现批量插入数据，例如：

- mset
- hmset

利用mset批量插入10万条数据

```
@Test
void testMxx() {
    String[] arr = new String[2000];
    int j;
    long b = System.currentTimeMillis();
    for (int i = 1; i <= 100000; i++) {
        j = (i % 1000) << 1;
        arr[j] = "test:key_" + i;
        arr[j + 1] = "value_" + i;
        if (j == 0) {
            jedis.mset(arr);
        }
    }
    long e = System.currentTimeMillis();
    System.out.println("time: " + (e - b));
}
```

#### 2.1.3、Pipeline

MSET虽然可以批处理，但是却只能操作部分数据类型，因此如果有对复杂数据类型的批处理需要，建议使用Pipeline

```
@Test
void testPipeline() {
    // 创建管道
    Pipeline pipeline = jedis.pipelined();
    long b = System.currentTimeMillis();
    for (int i = 1; i <= 100000; i++) {
        // 放入命令到管道
        pipeline.set("test:key_" + i, "value_" + i);
        if (i % 1000 == 0) {
            // 每放入1000条命令，批量执行
            pipeline.sync();
        }
    }
    long e = System.currentTimeMillis();
    System.out.println("time: " + (e - b));
}
```

### 2.2、集群下的批处理

如MSET或Pipeline这样的批处理需要在一次请求中携带多条命令，而此时如果Redis是一个集群，那批处理命令的多个key必须落在一个插槽中，否则就会导致执行失败。大家可以想一想这样的要求其实很难实现，因为我们在批处理时，可能一次要插入很多条数据，这些数据很有可能不会都落在相同的节点上，这就会导致报错了

这个时候，我们可以找到4种解决方案

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741031113-e3315c0c-7398-48c9-a4b0-4a014b2a8c6d.png)

第一种方案：串行执行，所以这种方式没有什么意义，当然，执行起来就很简单了，缺点就是耗时过久。

第二种方案：串行slot，简单来说，就是执行前，客户端先计算一下对应的key的slot，一样slot的key就放到一个组里边，不同的，就放到不同的组里边，然后对每个组执行pipeline的批处理，他就能串行执行各个组的命令，这种做法比第一种方法耗时要少，但是缺点呢，相对来说复杂一点，所以这种方案还需要优化一下

第三种方案：并行slot，相较于第二种方案，在分组完成后串行执行，第三种方案，就变成了并行执行各个命令，所以他的耗时就非常短，但是实现呢，也更加复杂。

第四种：hash_tag，redis计算key的slot的时候，其实是根据key的有效部分来计算的，通过这种方式就能一次处理所有的key，这种方式耗时最短，实现也简单，但是如果通过操作key的有效部分，那么就会导致所有的key都落在一个节点上，产生数据倾斜的问题，所以我们推荐使用第三种方式。

#### 2.2.1 串行化执行代码实践

```
public class JedisClusterTest {

    private JedisCluster jedisCluster;

    @BeforeEach
    void setUp() {
        // 配置连接池
        JedisPoolConfig poolConfig = new JedisPoolConfig();
        poolConfig.setMaxTotal(8);
        poolConfig.setMaxIdle(8);
        poolConfig.setMinIdle(0);
        poolConfig.setMaxWaitMillis(1000);
        HashSet<HostAndPort> nodes = new HashSet<>();
        nodes.add(new HostAndPort("192.168.150.101", 7001));
        nodes.add(new HostAndPort("192.168.150.101", 7002));
        nodes.add(new HostAndPort("192.168.150.101", 7003));
        nodes.add(new HostAndPort("192.168.150.101", 8001));
        nodes.add(new HostAndPort("192.168.150.101", 8002));
        nodes.add(new HostAndPort("192.168.150.101", 8003));
        jedisCluster = new JedisCluster(nodes, poolConfig);
    }

    @Test
    void testMSet() {
        jedisCluster.mset("name", "Jack", "age", "21", "sex", "male");

    }

    @Test
    void testMSet2() {
        Map<String, String> map = new HashMap<>(3);
        map.put("name", "Jack");
        map.put("age", "21");
        map.put("sex", "Male");
        //对Map数据进行分组。根据相同的slot放在一个分组
        //key就是slot，value就是一个组
        Map<Integer, List<Map.Entry<String, String>>> result = map.entrySet()
                .stream()
                .collect(Collectors.groupingBy(
                        entry -> ClusterSlotHashUtil.calculateSlot(entry.getKey()))
                );
        //串行的去执行mset的逻辑
        for (List<Map.Entry<String, String>> list : result.values()) {
            String[] arr = new String[list.size() * 2];
            int j = 0;
            for (int i = 0; i < list.size(); i++) {
                j = i<<2;
                Map.Entry<String, String> e = list.get(0);
                arr[j] = e.getKey();
                arr[j + 1] = e.getValue();
            }
            jedisCluster.mset(arr);
        }
    }

    @AfterEach
    void tearDown() {
        if (jedisCluster != null) {
            jedisCluster.close();
        }
    }
}
```

2.2.2 Spring集群环境下批处理代码

```
   @Test
    void testMSetInCluster() {
        Map<String, String> map = new HashMap<>(3);
        map.put("name", "Rose");
        map.put("age", "21");
        map.put("sex", "Female");
        stringRedisTemplate.opsForValue().multiSet(map);


        List<String> strings = stringRedisTemplate.opsForValue().multiGet(Arrays.asList("name", "age", "sex"));
        strings.forEach(System.out::println);

    }
```

原理分析

在RedisAdvancedClusterAsyncCommandsImpl 类中

首先根据slotHash算出来一个partitioned的map，map中的key就是slot，而他的value就是对应的对应相同slot的key对应的数据

通过 RedisFuture mset = super.mset(op);进行异步的消息发送

```
@Override
public RedisFuture<String> mset(Map<K, V> map) {

    Map<Integer, List<K>> partitioned = SlotHash.partition(codec, map.keySet());

    if (partitioned.size() < 2) {
        return super.mset(map);
    }

    Map<Integer, RedisFuture<String>> executions = new HashMap<>();

    for (Map.Entry<Integer, List<K>> entry : partitioned.entrySet()) {

        Map<K, V> op = new HashMap<>();
        entry.getValue().forEach(k -> op.put(k, map.get(k)));

        RedisFuture<String> mset = super.mset(op);
        executions.put(entry.getKey(), mset);
    }

    return MultiNodeExecution.firstOfAsync(executions);
}
```

## 3、服务器端优化-持久化配置

Redis的持久化虽然可以保证数据安全，但也会带来很多额外的开销，因此持久化请遵循下列建议：

- 用来做缓存的Redis实例尽量不要开启持久化功能
- 建议关闭RDB持久化功能，使用AOF持久化
- 利用脚本定期在slave节点做RDB，实现数据备份
- 设置合理的rewrite阈值，避免频繁的bgrewrite
- 配置no-appendfsync-on-rewrite = yes，禁止在rewrite期间做aof，避免因AOF引起的阻塞
- 部署有关建议：

- Redis实例的物理机要预留足够内存，应对fork和rewrite
- 单个Redis实例内存上限不要太大，例如4G或8G。可以加快fork的速度、减少主从同步、数据迁移压力
- 不要与CPU密集型应用部署在一起
- 不要与高硬盘负载应用一起部署。例如：数据库、消息队列

## 4、服务器端优化-慢查询优化

### 4.1 什么是慢查询

并不是很慢的查询才是慢查询，而是：在Redis执行时耗时超过某个阈值的命令，称为慢查询。

慢查询的危害：由于Redis是单线程的，所以当客户端发出指令后，他们都会进入到redis底层的queue来执行，如果此时有一些慢查询的数据，就会导致大量请求阻塞，从而引起报错，所以我们需要解决慢查询问题。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741053351-a01dee65-1b1e-470b-830d-bc0e391a7278.png)

慢查询的阈值可以通过配置指定：

slowlog-log-slower-than：慢查询阈值，单位是微秒。默认是10000，建议1000

慢查询会被放入慢查询日志中，日志的长度有上限，可以通过配置指定：

slowlog-max-len：慢查询日志（本质是一个队列）的长度。默认是128，建议1000

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741095200-60c6aef8-3da0-408a-b34c-c0991f52e52e.png)

修改这两个配置可以使用：config set命令：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741075048-5133e29d-a450-44ab-9ea5-39ed910b7f85.png)

### 4.2 如何查看慢查询

知道了以上内容之后，那么咱们如何去查看慢查询日志列表呢：

- slowlog len：查询慢查询日志长度
- slowlog get [n]：读取n条慢查询日志
- slowlog reset：清空慢查询列表

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741109481-d343f2bb-de8b-4cab-8c1d-dc97776277b7.png)

## 5、服务器端优化-命令及安全配置

安全可以说是服务器端一个非常重要的话题，如果安全出现了问题，那么一旦这个漏洞被一些坏人知道了之后，并且进行攻击，那么这就会给咱们的系统带来很多的损失，所以我们这节课就来解决这个问题。

Redis会绑定在0.0.0.0:6379，这样将会将Redis服务暴露到公网上，而Redis如果没有做身份认证，会出现严重的安全漏洞.  
漏洞重现方式：[https://cloud.tencent.com/developer/article/1039000](https://cloud.tencent.com/developer/article/1039000)

为什么会出现不需要密码也能够登录呢，主要是Redis考虑到每次登录都比较麻烦，所以Redis就有一种ssh免秘钥登录的方式，生成一对公钥和私钥，私钥放在本地，公钥放在redis端，当我们登录时服务器，再登录时候，他会去解析公钥和私钥，如果没有问题，则不需要利用redis的登录也能访问，这种做法本身也很常见，但是这里有一个前提，前提就是公钥必须保存在服务器上，才行，但是Redis的漏洞在于在不登录的情况下，也能把秘钥送到Linux服务器，从而产生漏洞

漏洞出现的核心的原因有以下几点：

- Redis未设置密码
- 利用了Redis的config set命令动态修改Redis配置
- 使用了Root账号权限启动Redis

所以：如何解决呢？我们可以采用如下几种方案

为了避免这样的漏洞，这里给出一些建议：

- Redis一定要设置密码
- 禁止线上使用下面命令：keys、flushall、flushdb、config set等命令。可以利用rename-command禁用。
- bind：限制网卡，禁止外网网卡访问
- 开启防火墙
- 不要使用Root账户启动Redis
- 尽量不是有默认的端口

## 6、服务器端优化-Redis内存划分和内存配置

当Redis内存不足时，可能导致Key频繁被删除、响应时间变长、QPS不稳定等问题。当内存使用率达到90%以上时就需要我们警惕，并快速定位到内存占用的原因。

有关碎片问题分析

Redis底层分配并不是这个key有多大，他就会分配多大，而是有他自己的分配策略，比如8,16,20等等，假定当前key只需要10个字节，此时分配8肯定不够，那么他就会分配16个字节，多出来的6个字节就不能被使用，这就是我们常说的 碎片问题

进程内存问题分析：

这片内存，通常我们都可以忽略不计

缓冲区内存问题分析：

一般包括客户端缓冲区、AOF缓冲区、复制缓冲区等。客户端缓冲区又包括输入缓冲区和输出缓冲区两种。这部分内存占用波动较大，所以这片内存也是我们需要重点分析的内存问题。

|   |   |
|---|---|
|内存占用|说明|
|数据内存|是Redis最主要的部分，存储Redis的键值信息。主要问题是BigKey问题、内存碎片问题|
|进程内存|Redis主进程本身运⾏肯定需要占⽤内存，如代码、常量池等等；这部分内存⼤约⼏兆，在⼤多数⽣产环境中与Redis数据占⽤的内存相⽐可以忽略。|
|缓冲区内存|一般包括客户端缓冲区、AOF缓冲区、复制缓冲区等。客户端缓冲区又包括输入缓冲区和输出缓冲区两种。这部分内存占用波动较大，不当使用BigKey，可能导致内存溢出。|

于是我们就需要通过一些命令，可以查看到Redis目前的内存分配状态：

- info memory：查看内存分配的情况

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741133136-1bc3f026-bdb1-4627-a9f2-2d659cf6e4c3.png)

- memory xxx：查看key的主要占用情况

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741146304-ef989495-ad19-4e74-b3cc-daa8892104da.png)

接下来我们看到了这些配置，最关键的缓存区内存如何定位和解决呢？

内存缓冲区常见的有三种：

- 复制缓冲区：主从复制的repl_backlog_buf，如果太小可能导致频繁的全量复制，影响性能。通过replbacklog-size来设置，默认1mb
- AOF缓冲区：AOF刷盘之前的缓存区域，AOF执行rewrite的缓冲区。无法设置容量上限
- 客户端缓冲区：分为输入缓冲区和输出缓冲区，输入缓冲区最大1G且不能设置。输出缓冲区可以设置

以上复制缓冲区和AOF缓冲区 不会有问题，最关键就是客户端缓冲区的问题

客户端缓冲区：指的就是我们发送命令时，客户端用来缓存命令的一个缓冲区，也就是我们向redis输入数据的输入端缓冲区和redis向客户端返回数据的响应缓存区，输入缓冲区最大1G且不能设置，所以这一块我们根本不用担心，如果超过了这个空间，redis会直接断开，因为本来此时此刻就代表着redis处理不过来了，我们需要担心的就是输出端缓冲区

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741162501-eb549eef-7158-40ac-b917-fcf1ed46a6c1.png)

我们在使用redis过程中，处理大量的big value，那么会导致我们的输出结果过多，如果输出缓存区过大，会导致redis直接断开，而默认配置的情况下， 其实他是没有大小的，这就比较坑了，内存可能一下子被占满，会直接导致咱们的redis断开，所以解决方案有两个

1、设置一个大小

2、增加我们带宽的大小，避免我们出现大量数据从而直接超过了redis的承受能力

## 7、服务器端集群优化-集群还是主从

集群虽然具备高可用特性，能实现自动故障恢复，但是如果使用不当，也会存在一些问题：

- 集群完整性问题
- 集群带宽问题
- 数据倾斜问题
- 客户端性能问题
- 命令的集群兼容性问题
- lua和事务问题

问题1、在Redis的默认配置中，如果发现任意一个插槽不可用，则整个集群都会停止对外服务：

大家可以设想一下，如果有几个slot不能使用，那么此时整个集群都不能用了，我们在开发中，其实最重要的是可用性，所以需要把如下配置修改成no，即有slot不能使用时，我们的redis集群还是可以对外提供服务

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741177755-93eecc05-9e42-4c42-b60e-dfee98bf2d9c.png)

问题2、集群带宽问题

集群节点之间会不断的互相Ping来确定集群中其它节点的状态。每次Ping携带的信息至少包括：

- 插槽信息
- 集群状态信息

集群中节点越多，集群状态信息数据量也越大，10个节点的相关信息可能达到1kb，此时每次集群互通需要的带宽会非常高，这样会导致集群中大量的带宽都会被ping信息所占用，这是一个非常可怕的问题，所以我们需要去解决这样的问题

解决途径：

- 避免大集群，集群节点数不要太多，最好少于1000，如果业务庞大，则建立多个集群。
- 避免在单个物理机中运行太多Redis实例
- 配置合适的cluster-node-timeout值

问题3、命令的集群兼容性问题

有关这个问题咱们已经探讨过了，当我们使用批处理的命令时，redis要求我们的key必须落在相同的slot上，然后大量的key同时操作时，是无法完成的，所以客户端必须要对这样的数据进行处理，这些方案我们之前已经探讨过了，所以不再这个地方赘述了。

问题4、lua和事务的问题

lua和事务都是要保证原子性问题，如果你的key不在一个节点，那么是无法保证lua的执行和事务的特性的，所以在集群模式是没有办法执行lua和事务的

那我们到底是集群还是主从

单体Redis（主从Redis）已经能达到万级别的QPS，并且也具备很强的高可用特性。如果主从能满足业务需求的情况下，所以如果不是在万不得已的情况下，尽量不搭建Redis集群

## 8、结束语

亲爱的小伙帮们辛苦啦，咱们有关redis的最佳实践到这里就讲解完毕了，期待小伙们学业有成~~~~

# Redis原理篇

## 1、原理篇-Redis数据结构

### 1.1 Redis数据结构-动态字符串

我们都知道Redis中保存的Key是字符串，value往往是字符串或者字符串的集合。可见字符串是Redis中最常用的一种数据结构。

不过Redis没有直接使用C语言中的字符串，因为C语言字符串存在很多问题：  
获取字符串长度的需要通过运算  
非二进制安全  
不可修改  
Redis构建了一种新的字符串结构，称为简单动态字符串（Simple Dynamic String），简称SDS。  
例如，我们执行命令：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741285650-c065b9ea-4d24-4a34-9adc-51de12ecce3c.png)

那么Redis将在底层创建两个SDS，其中一个是包含“name”的SDS，另一个是包含“虎哥”的SDS。

Redis是C语言实现的，其中SDS是一个结构体，源码如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741297742-693aff08-3870-4e2a-834c-cbdcac5726a4.png)

例如，一个包含字符串“name”的sds结构如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741308577-1851601b-4f3b-425b-8aa6-7b9e46dda226.png)

SDS之所以叫做动态字符串，是因为它具备动态扩容的能力，例如一个内容为“hi”的SDS：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741317749-26079c17-0bdc-44a7-87b3-7e2cdb149ad9.png)

假如我们要给SDS追加一段字符串“,Amy”，这里首先会申请新内存空间：

如果新字符串小于1M，则新空间为扩展后字符串长度的两倍+1；

如果新字符串大于1M，则新空间为扩展后字符串长度+1M+1。称为内存预分配。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741329644-f216cdeb-7fd1-445a-aa08-592980781b02.png)

### 1.2 Redis数据结构-intset

IntSet是Redis中set集合的一种实现方式，基于整数数组来实现，并且具备长度可变、有序等特征。  
结构如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741346455-10d93019-db7e-471e-84f0-2c1fb78eb23f.png)

其中的encoding包含三种模式，表示存储的整数大小不同：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741356125-1cacca36-1280-4d54-a0a8-2c5f9bc63b9a.png)

为了方便查找，Redis会将intset中所有的整数按照升序依次保存在contents数组中，结构如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741368755-6c967c4b-8450-4afd-af17-ea1be25c8b76.png)

现在，数组中每个数字都在int16_t的范围内，因此采用的编码方式是INTSET_ENC_INT16，每部分占用的字节大小为：  
encoding：4字节  
length：4字节  
contents：2字节 * 3  = 6字节

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741380946-037f4490-3ca4-41a1-bd90-6f85901a713f.png)

我们向该其中添加一个数字：50000，这个数字超出了int16_t的范围，intset会自动升级编码方式到合适的大小。  
以当前案例来说流程如下：

- 升级编码为INTSET_ENC_INT32, 每个整数占4字节，并按照新的编码方式及元素个数扩容数组
- 倒序依次将数组中的元素拷贝到扩容后的正确位置
- 将待添加的元素放入数组末尾
- 最后，将inset的encoding属性改为INTSET_ENC_INT32，将length属性改为4

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741390902-c698fe5b-dc03-4464-86d9-4edaf97b1ff8.png)

源码如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741415343-642a093b-a868-4ed3-a105-b5e1530e40f0.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741428691-ca047022-605c-4680-b7fd-887cf7a7a69f.png)

小总结：

Intset可以看做是特殊的整数数组，具备一些特点：

- Redis会确保Intset中的元素唯一、有序
- 具备类型升级机制，可以节省内存空间
- 底层采用二分查找方式来查询

### 1.3 Redis数据结构-Dict

我们知道Redis是一个键值型（Key-Value Pair）的数据库，我们可以根据键实现快速的增删改查。而键与值的映射关系正是通过Dict来实现的。  
Dict由三部分组成，分别是：哈希表（DictHashTable）、哈希节点（DictEntry）、字典（Dict）

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741443132-0ed7ab97-5a72-46d4-85d6-737966a73869.png)

当我们向Dict添加键值对时，Redis首先根据key计算出hash值（h），然后利用 h & sizemask来计算元素应该存储到数组中的哪个索引位置。我们存储k1=v1，假设k1的哈希值h =1，则1&3 =1，因此k1=v1要存储到数组角标1位置。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741456149-b8f2e51e-985c-4653-b559-bc0dd667da2f.png)

Dict由三部分组成，分别是：哈希表（DictHashTable）、哈希节点（DictEntry）、字典（Dict）

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741473900-b8894ab5-544f-49bd-934a-ac8996e4ecb0.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741487112-665ef0a9-8538-4649-91f3-15e8d87c1f40.png)

Dict的扩容

Dict中的HashTable就是数组结合单向链表的实现，当集合中元素较多时，必然导致哈希冲突增多，链表过长，则查询效率会大大降低。  
Dict在每次新增键值对时都会检查负载因子（LoadFactor = used/size） ，满足以下两种情况时会触发哈希表扩容：  
哈希表的 LoadFactor >= 1，并且服务器没有执行 BGSAVE 或者 BGREWRITEAOF 等后台进程；  
哈希表的 LoadFactor > 5 ；

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741500916-6a5bc806-bd33-4962-9deb-97fedcec3cb8.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741512999-7ef9a22d-e0d3-46f3-afc6-a0794cc3e305.png)

Dict的rehash

不管是扩容还是收缩，必定会创建新的哈希表，导致哈希表的size和sizemask变化，而key的查询与sizemask有关。因此必须对哈希表中的每一个key重新计算索引，插入新的哈希表，这个过程称为rehash。过程是这样的：

- 计算新hash表的realeSize，值取决于当前要做的是扩容还是收缩：

- 如果是扩容，则新size为第一个大于等于dict.ht[0].used + 1的2^n
- 如果是收缩，则新size为第一个大于等于dict.ht[0].used的2^n （不得小于4）

- 按照新的realeSize申请内存空间，创建dictht，并赋值给dict.ht[1]
- 设置dict.rehashidx = 0，标示开始rehash
- 将dict.ht[0]中的每一个dictEntry都rehash到dict.ht[1]
- 将dict.ht[1]赋值给dict.ht[0]，给dict.ht[1]初始化为空哈希表，释放原来的dict.ht[0]的内存
- 将rehashidx赋值为-1，代表rehash结束
- 在rehash过程中，新增操作，则直接写入ht[1]，查询、修改和删除则会在dict.ht[0]和dict.ht[1]依次查找并执行。这样可以确保ht[0]的数据只减不增，随着rehash最终为空

整个过程可以描述成：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741530187-d8400391-fff4-488c-b3b9-a3c91f810cbb.png)

小总结：

Dict的结构：

- 类似java的HashTable，底层是数组加链表来解决哈希冲突
- Dict包含两个哈希表，ht[0]平常用，ht[1]用来rehash

Dict的伸缩：

- 当LoadFactor大于5或者LoadFactor大于1并且没有子进程任务时，Dict扩容
- 当LoadFactor小于0.1时，Dict收缩
- 扩容大小为第一个大于等于used + 1的2^n
- 收缩大小为第一个大于等于used 的2^n
- Dict采用渐进式rehash，每次访问Dict时执行一次rehash
- rehash时ht[0]只减不增，新增操作只在ht[1]执行，其它操作在两个哈希表

### 1.4 Redis数据结构-ZipList

ZipList 是一种特殊的“双端链表” ，由一系列特殊编码的连续内存块组成。可以在任意一端进行压入/弹出操作, 并且该操作的时间复杂度为 O(1)。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741545379-79db9aa4-3f3c-4365-b970-a54f245320a4.png)

|   |   |   |   |
|---|---|---|---|
|属性|类型|长度|用途|
|zlbytes|uint32_t|4 字节|记录整个压缩列表占用的内存字节数|
|zltail|uint32_t|4 字节|记录压缩列表表尾节点距离压缩列表的起始地址有多少字节，通过这个偏移量，可以确定表尾节点的地址。|
|zllen|uint16_t|2 字节|记录了压缩列表包含的节点数量。 最大值为UINT16_MAX （65534），如果超过这个值，此处会记录为65535，但节点的真实数量需要遍历整个压缩列表才能计算得出。|
|entry|列表节点|不定|压缩列表包含的各个节点，节点的长度由节点保存的内容决定。|
|zlend|uint8_t|1 字节|特殊值 0xFF （十进制 255 ），用于标记压缩列表的末端。|

ZipListEntry

ZipList 中的Entry并不像普通链表那样记录前后节点的指针，因为记录两个指针要占用16个字节，浪费内存。而是采用了下面的结构：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741562641-25c2e533-e010-4205-b0c2-ddf2ee58ef0e.png)

- previous_entry_length：前一节点的长度，占1个或5个字节。

- 如果前一节点的长度小于254字节，则采用1个字节来保存这个长度值
- 如果前一节点的长度大于254字节，则采用5个字节来保存这个长度值，第一个字节为0xfe，后四个字节才是真实长度数据

- encoding：编码属性，记录content的数据类型（字符串还是整数）以及长度，占用1个、2个或5个字节
- contents：负责保存节点的数据，可以是字符串或整数

ZipList中所有存储长度的数值均采用小端字节序，即低位字节在前，高位字节在后。例如：数值0x1234，采用小端字节序后实际存储值为：0x3412

Encoding编码

ZipListEntry中的encoding编码分为字符串和整数两种：  
字符串：如果encoding是以“00”、“01”或者“10”开头，则证明content是字符串

|   |   |   |
|---|---|---|
|编码|编码长度|字符串大小|
|\|00pppppp\||1 bytes|<= 63 bytes|
|\|01pppppp\|qqqqqqqq\||2 bytes|<= 16383 bytes|
|\|10000000\|qqqqqqqq\|rrrrrrrr\|ssssssss\|tttttttt\||5 bytes|<= 4294967295 bytes|

例如，我们要保存字符串：“ab”和 “bc”

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741582551-40639082-9707-4c09-8369-1a23e63dc790.png)

ZipListEntry中的encoding编码分为字符串和整数两种：

- 整数：如果encoding是以“11”开始，则证明content是整数，且encoding固定只占用1个字节

|   |   |   |
|---|---|---|
|编码|编码长度|整数类型|
|11000000|1|int16_t（2 bytes）|
|11010000|1|int32_t（4 bytes）|
|11100000|1|int64_t（8 bytes）|
|11110000|1|24位有符整数(3 bytes)|
|11111110|1|8位有符整数(1 bytes)|
|1111xxxx|1|直接在xxxx位置保存数值，范围从0001~1101，减1后结果为实际值|

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741597323-5862a69b-c085-4e84-94f5-f584c480a06b.png)

### 1.5 Redis数据结构-ZipList的连锁更新问题

ZipList的每个Entry都包含previous_entry_length来记录上一个节点的大小，长度是1个或5个字节：  
如果前一节点的长度小于254字节，则采用1个字节来保存这个长度值  
如果前一节点的长度大于等于254字节，则采用5个字节来保存这个长度值，第一个字节为0xfe，后四个字节才是真实长度数据  
现在，假设我们有N个连续的、长度为250~253字节之间的entry，因此entry的previous_entry_length属性用1个字节即可表示，如图所示：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741609276-0c385070-d0c7-4bfb-aa64-4cf5feae4f40.png)

ZipList这种特殊情况下产生的连续多次空间扩展操作称之为连锁更新（Cascade Update）。新增、删除都可能导致连锁更新的发生。

小总结：

ZipList特性：

- 压缩列表的可以看做一种连续内存空间的"双向链表"
- 列表的节点之间不是通过指针连接，而是记录上一节点和本节点长度来寻址，内存占用较低
- 如果列表数据过多，导致链表过长，可能影响查询性能
- 增或删较大数据时有可能发生连续更新问题

### 1.6 Redis数据结构-QuickList

问题1：ZipList虽然节省内存，但申请内存必须是连续空间，如果内存占用较多，申请内存效率很低。怎么办？

答：为了缓解这个问题，我们必须限制ZipList的长度和entry大小。

问题2：但是我们要存储大量数据，超出了ZipList最佳的上限该怎么办？

答：我们可以创建多个ZipList来分片存储数据。

问题3：数据拆分后比较分散，不方便管理和查找，这多个ZipList如何建立联系？

答：Redis在3.2版本引入了新的数据结构QuickList，它是一个双端链表，只不过链表中的每个节点都是一个ZipList。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741629544-e84248f5-d45d-4db7-8d0d-2986665f7a8e.png)

为了避免QuickList中的每个ZipList中entry过多，Redis提供了一个配置项：list-max-ziplist-size来限制。  
如果值为正，则代表ZipList的允许的entry个数的最大值  
如果值为负，则代表ZipList的最大内存大小，分5种情况：

- -1：每个ZipList的内存占用不能超过4kb
- -2：每个ZipList的内存占用不能超过8kb
- -3：每个ZipList的内存占用不能超过16kb
- -4：每个ZipList的内存占用不能超过32kb
- -5：每个ZipList的内存占用不能超过64kb

其默认值为 -2：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741641713-1d960d7e-bde0-4a25-bc5c-ef4837bf4908.png)

以下是QuickList的和QuickListNode的结构源码：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741657524-3dd7c166-3e26-4538-a1a5-a3ac3b9bd933.png)

我们接下来用一段流程图来描述当前的这个结构

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741668008-21e670a6-8740-41f4-9bdb-37a608b54e06.png)

总结：

QuickList的特点：

- 是一个节点为ZipList的双端链表
- 节点采用ZipList，解决了传统链表的内存占用问题
- 控制了ZipList大小，解决连续内存空间申请效率问题
- 中间节点可以压缩，进一步节省了内存

1.7 Redis数据结构-SkipList

SkipList（跳表）首先是链表，但与传统链表相比有几点差异：  
元素按照升序排列存储  
节点可能包含多个指针，指针跨度不同。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741682222-b2e1dbf8-bd0c-4713-bf61-737ad1084f78.png)

SkipList（跳表）首先是链表，但与传统链表相比有几点差异：  
元素按照升序排列存储  
节点可能包含多个指针，指针跨度不同。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741692703-0a65feb7-d645-4e57-ada4-31ad26fc769a.png)

SkipList（跳表）首先是链表，但与传统链表相比有几点差异：  
元素按照升序排列存储  
节点可能包含多个指针，指针跨度不同。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741704401-b3ceb784-9c43-46f1-9363-6ae9268e8b07.png)

小总结：

SkipList的特点：

- 跳跃表是一个双向链表，每个节点都包含score和ele值
- 节点按照score值排序，score值一样则按照ele字典排序
- 每个节点都可以包含多层指针，层数是1到32之间的随机数
- 不同层指针到下一个节点的跨度不同，层级越高，跨度越大
- 增删改查效率与红黑树基本一致，实现却更简单

### 1.7 Redis数据结构-RedisObject

Redis中的任意数据类型的键和值都会被封装为一个RedisObject，也叫做Redis对象，源码如下：

1、什么是redisObject：  
从Redis的使用者的角度来看，⼀个Redis节点包含多个database（非cluster模式下默认是16个，cluster模式下只能是1个），而一个database维护了从key space到object space的映射关系。这个映射关系的key是string类型，⽽value可以是多种数据类型，比如：  
string, list, hash、set、sorted set等。我们可以看到，key的类型固定是string，而value可能的类型是多个。  
⽽从Redis内部实现的⾓度来看，database内的这个映射关系是用⼀个dict来维护的。dict的key固定用⼀种数据结构来表达就够了，这就是动态字符串sds。而value则比较复杂，为了在同⼀个dict内能够存储不同类型的value，这就需要⼀个通⽤的数据结构，这个通用的数据结构就是robj，全名是redisObject。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741723306-8e5a07da-876d-49ab-a7cd-cb0043b01b53.png)

Redis的编码方式

Redis中会根据存储的数据类型不同，选择不同的编码方式，共包含11种不同类型：

|   |   |   |
|---|---|---|
|编号|编码方式|说明|
|0|OBJ_ENCODING_RAW|raw编码动态字符串|
|1|OBJ_ENCODING_INT|long类型的整数的字符串|
|2|OBJ_ENCODING_HT|hash表（字典dict）|
|3|OBJ_ENCODING_ZIPMAP|已废弃|
|4|OBJ_ENCODING_LINKEDLIST|双端链表|
|5|OBJ_ENCODING_ZIPLIST|压缩列表|
|6|OBJ_ENCODING_INTSET|整数集合|
|7|OBJ_ENCODING_SKIPLIST|跳表|
|8|OBJ_ENCODING_EMBSTR|embstr的动态字符串|
|9|OBJ_ENCODING_QUICKLIST|快速列表|
|10|OBJ_ENCODING_STREAM|Stream流|

五种数据结构

Redis中会根据存储的数据类型不同，选择不同的编码方式。每种数据类型的使用的编码方式如下：

|   |   |
|---|---|
|数据类型|编码方式|
|OBJ_STRING|int、embstr、raw|
|OBJ_LIST|LinkedList和ZipList(3.2以前)、QuickList（3.2以后）|
|OBJ_SET|intset、HT|
|OBJ_ZSET|ZipList、HT、SkipList|
|OBJ_HASH|ZipList、HT|

### 1.8 Redis数据结构-String

String是Redis中最常见的数据存储类型：

其基本编码方式是RAW，基于简单动态字符串（SDS）实现，存储上限为512mb。

如果存储的SDS长度小于44字节，则会采用EMBSTR编码，此时object head与SDS是一段连续空间。申请内存时

只需要调用一次内存分配函数，效率更高。

（1）底层实现⽅式：动态字符串sds 或者 long  
String的内部存储结构⼀般是sds（Simple Dynamic String，可以动态扩展内存），但是如果⼀个String类型的value的值是数字，那么Redis内部会把它转成long类型来存储，从⽽减少内存的使用。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741739560-bc0e43a5-0dca-4a7b-9e79-ea98ddf8ec1d.png)

如果存储的字符串是整数值，并且大小在LONG_MAX范围内，则会采用INT编码：直接将数据保存在RedisObject的ptr指针位置（刚好8字节），不再需要SDS了。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741753949-cbfef3a1-3966-4c49-a60a-e83e18c1873e.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741766107-797ba75e-a72b-4a55-80e4-162b16123505.png)

确切地说，String在Redis中是⽤⼀个robj来表示的。

用来表示String的robj可能编码成3种内部表⽰：OBJ_ENCODING_RAW，OBJ_ENCODING_EMBSTR，OBJ_ENCODING_INT。  
其中前两种编码使⽤的是sds来存储，最后⼀种OBJ_ENCODING_INT编码直接把string存成了long型。  
在对string进行incr, decr等操作的时候，如果它内部是OBJ_ENCODING_INT编码，那么可以直接行加减操作；如果它内部是OBJ_ENCODING_RAW或OBJ_ENCODING_EMBSTR编码，那么Redis会先试图把sds存储的字符串转成long型，如果能转成功，再进行加减操作。对⼀个内部表示成long型的string执行append, setbit, getrange这些命令，针对的仍然是string的值（即⼗进制表示的字符串），而不是针对内部表⽰的long型进⾏操作。比如字符串”32”，如果按照字符数组来解释，它包含两个字符，它们的ASCII码分别是0x33和0x32。当我们执行命令setbit key 7 0的时候，相当于把字符0x33变成了0x32，这样字符串的值就变成了”22”。⽽如果将字符串”32”按照内部的64位long型来解释，那么它是0x0000000000000020，在这个基础上执⾏setbit位操作，结果就完全不对了。因此，在这些命令的实现中，会把long型先转成字符串再进行相应的操作。

### 1.9 Redis数据结构-List

Redis的List类型可以从首、尾操作列表中的元素：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741779454-1a71453c-0288-4efe-aa53-03031d753c31.png)

哪一个数据结构能满足上述特征？

- LinkedList ：普通链表，可以从双端访问，内存占用较高，内存碎片较多
- ZipList ：压缩列表，可以从双端访问，内存占用低，存储上限低
- QuickList：LinkedList + ZipList，可以从双端访问，内存占用较低，包含多个ZipList，存储上限高

Redis的List结构类似一个双端链表，可以从首、尾操作列表中的元素：

在3.2版本之前，Redis采用ZipList和LinkedList来实现List，当元素数量小于512并且元素大小小于64字节时采用ZipList编码，超过则采用LinkedList编码。

在3.2版本之后，Redis统一采用QuickList来实现List：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741790211-c22eaab3-855a-4bd5-8744-fa0e8ae3aa59.png)

### 2.0 Redis数据结构-Set结构

Set是Redis中的单列集合，满足下列特点：

- 不保证有序性
- 保证元素唯一
- 求交集、并集、差集

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741803553-298ce24c-a4ca-4669-9846-132d9e56fc7b.png)

可以看出，Set对查询元素的效率要求非常高，思考一下，什么样的数据结构可以满足？  
HashTable，也就是Redis中的Dict，不过Dict是双列集合（可以存键、值对）

Set是Redis中的集合，不一定确保元素有序，可以满足元素唯一、查询效率要求极高。  
为了查询效率和唯一性，set采用HT编码（Dict）。Dict中的key用来存储元素，value统一为null。  
当存储的所有数据都是整数，并且元素数量不超过set-max-intset-entries时，Set会采用IntSet编码，以节省内存

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741827249-bab25a1b-1e82-49e8-81ce-51feb6ec086e.png)

结构如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741835844-b22742e8-fda1-4f36-877c-122c1e05e4cc.png)

### 2.1、Redis数据结构-ZSET

ZSet也就是SortedSet，其中每一个元素都需要指定一个score值和member值：

- 可以根据score值排序后
- member必须唯一
- 可以根据member查询分数

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741845385-abb2045c-a1c7-4cf3-a665-be22e32c43f7.png)

因此，zset底层数据结构必须满足键值存储、键必须唯一、可排序这几个需求。之前学习的哪种编码结构可以满足？

- SkipList：可以排序，并且可以同时存储score和ele值（member）
- HT（Dict）：可以键值存储，并且可以根据key找value

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741878188-925259f1-8c8b-4fb4-8af4-3ffd43a1fdc2.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741890012-dce3e19c-ef90-4cbd-b641-54cdba1ff618.png)

当元素数量不多时，HT和SkipList的优势不明显，而且更耗内存。因此zset还会采用ZipList结构来节省内存，不过需要同时满足两个条件：

- 元素数量小于zset_max_ziplist_entries，默认值128
- 每个元素都小于zset_max_ziplist_value字节，默认值64

ziplist本身没有排序功能，而且没有键值对的概念，因此需要有zset通过编码实现：

- ZipList是连续内存，因此score和element是紧挨在一起的两个entry， element在前，score在后
- score越小越接近队首，score越大越接近队尾，按照score值升序排列

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741917172-2c4fc19d-d7e0-4978-ae99-ddb79dad65db.png)

### 2.2 、Redis数据结构-Hash

Hash结构与Redis中的Zset非常类似：

- 都是键值存储
- 都需求根据键获取值
- 键必须唯一

区别如下：

- zset的键是member，值是score；hash的键和值都是任意值
- zset要根据score排序；hash则无需排序

（1）底层实现方式：压缩列表ziplist 或者 字典dict  
当Hash中数据项比较少的情况下，Hash底层才⽤压缩列表ziplist进⾏存储数据，随着数据的增加，底层的ziplist就可能会转成dict，具体配置如下：

hash-max-ziplist-entries 512

hash-max-ziplist-value 64

当满足上面两个条件其中之⼀的时候，Redis就使⽤dict字典来实现hash。  
Redis的hash之所以这样设计，是因为当ziplist变得很⼤的时候，它有如下几个缺点：

- 每次插⼊或修改引发的realloc操作会有更⼤的概率造成内存拷贝，从而降低性能。
- ⼀旦发生内存拷贝，内存拷贝的成本也相应增加，因为要拷贝更⼤的⼀块数据。
- 当ziplist数据项过多的时候，在它上⾯查找指定的数据项就会性能变得很低，因为ziplist上的查找需要进行遍历。

总之，ziplist本来就设计为各个数据项挨在⼀起组成连续的内存空间，这种结构并不擅长做修改操作。⼀旦数据发⽣改动，就会引发内存realloc，可能导致内存拷贝。

hash结构如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741926953-46a100c8-757c-48b2-aed9-6cd6ce33cd6b.png)

zset集合如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741936380-07e74b37-61b3-4b2d-b99d-5a93d7173b83.png)

因此，Hash底层采用的编码与Zset也基本一致，只需要把排序有关的SkipList去掉即可：

Hash结构默认采用ZipList编码，用以节省内存。 ZipList中相邻的两个entry 分别保存field和value

当数据量较大时，Hash结构会转为HT编码，也就是Dict，触发条件有两个：

- ZipList中的元素数量超过了hash-max-ziplist-entries（默认512）
- ZipList中的任意entry大小超过了hash-max-ziplist-value（默认64字节）

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741948107-2ab0722b-93a6-4ae1-9154-28dd39302d03.png)

## 2、原理篇-Redis网络模型

### 2.1 用户空间和内核态空间

服务器大多都采用Linux系统，这里我们以Linux为例来讲解:

ubuntu和Centos 都是Linux的发行版，发行版可以看成对linux包了一层壳，任何Linux发行版，其系统内核都是Linux。我们的应用都需要通过Linux内核与硬件交互

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741966896-b431c22a-a383-4c13-8072-8924e39048e6.png)

用户的应用，比如redis，mysql等其实是没有办法去执行访问我们操作系统的硬件的，所以我们可以通过发行版的这个壳子去访问内核，再通过内核去访问计算机硬件

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741978744-bc0ae732-8f53-492e-bd7e-0db66aa42c7d.png)

计算机硬件包括，如cpu，内存，网卡等等，内核（通过寻址空间）可以操作硬件的，但是内核需要不同设备的驱动，有了这些驱动之后，内核就可以去对计算机硬件去进行 内存管理，文件系统的管理，进程的管理等等

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667741991210-da0c88ce-a8cc-461b-8df4-ac29a69259ff.png)

我们想要用户的应用来访问，计算机就必须要通过对外暴露的一些接口，才能访问到，从而简介的实现对内核的操控，但是内核本身上来说也是一个应用，所以他本身也需要一些内存，cpu等设备资源，用户应用本身也在消耗这些资源，如果不加任何限制，用户去操作随意的去操作我们的资源，就有可能导致一些冲突，甚至有可能导致我们的系统出现无法运行的问题，因此我们需要把用户和内核隔离开

进程的寻址空间划分成两部分：内核空间、用户空间

什么是寻址空间呢？我们的应用程序也好，还是内核空间也好，都是没有办法直接去物理内存的，而是通过分配一些虚拟内存映射到物理内存中，我们的内核和应用程序去访问虚拟内存的时候，就需要一个虚拟地址，这个地址是一个无符号的整数，比如一个32位的操作系统，他的带宽就是32，他的虚拟地址就是2的32次方，也就是说他寻址的范围就是0~2的32次方， 这片寻址空间对应的就是2的32个字节，就是4GB，这个4GB，会有3个GB分给用户空间，会有1GB给内核系统

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742001695-aa121643-0bf9-480e-9058-9910a17d405c.png)

在linux中，他们权限分成两个等级，0和3，用户空间只能执行受限的命令（Ring3），而且不能直接调用系统资源，必须通过内核提供的接口来访问内核空间可以执行特权命令（Ring0），调用一切系统资源，所以一般情况下，用户的操作是运行在用户空间，而内核运行的数据是在内核空间的，而有的情况下，一个应用程序需要去调用一些特权资源，去调用一些内核空间的操作，所以此时他俩需要在用户态和内核态之间进行切换。

比如：

Linux系统为了提高IO效率，会在用户空间和内核空间都加入缓冲区：

写数据时，要把用户缓冲数据拷贝到内核缓冲区，然后写入设备

读数据时，要从设备读取数据到内核缓冲区，然后拷贝到用户缓冲区

针对这个操作：我们的用户在写读数据时，会去向内核态申请，想要读取内核的数据，而内核数据要去等待驱动程序从硬件上读取数据，当从磁盘上加载到数据之后，内核会将数据写入到内核的缓冲区中，然后再将数据拷贝到用户态的buffer中，然后再返回给应用程序，整体而言，速度慢，就是这个原因，为了加速，我们希望read也好，还是wait for data也最好都不要等待，或者时间尽量的短。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742016897-b4cd7001-b041-473c-ac7a-93ae44744d78.png)

### 2.2.网络模型-阻塞IO

在《UNIX网络编程》一书中，总结归纳了5种IO模型：

- 阻塞IO（Blocking IO）
- 非阻塞IO（Nonblocking IO）
- IO多路复用（IO Multiplexing）
- 信号驱动IO（Signal Driven IO）
- 异步IO（Asynchronous IO）

应用程序想要去读取数据，他是无法直接去读取磁盘数据的，他需要先到内核里边去等待内核操作硬件拿到数据，这个过程就是1，是需要等待的，等到内核从磁盘上把数据加载出来之后，再把这个数据写给用户的缓存区，这个过程是2，如果是阻塞IO，那么整个过程中，用户从发起读请求开始，一直到读取到数据，都是一个阻塞状态。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742029846-55b86fa1-79f3-4e1a-8bc6-3c2e6334418d.png)

具体流程如下图：

用户去读取数据时，会去先发起recvform一个命令，去尝试从内核上加载数据，如果内核没有数据，那么用户就会等待，此时内核会去从硬件上读取数据，内核读取数据之后，会把数据拷贝到用户态，并且返回ok，整个过程，都是阻塞等待的，这就是阻塞IO

总结如下：

顾名思义，阻塞IO就是两个阶段都必须阻塞等待：

阶段一：

- 用户进程尝试读取数据（比如网卡数据）
- 此时数据尚未到达，内核需要等待数据
- 此时用户进程也处于阻塞状态

阶段二：

- 数据到达并拷贝到内核缓冲区，代表已就绪
- 将内核数据拷贝到用户缓冲区
- 拷贝过程中，用户进程依然阻塞等待
- 拷贝完成，用户进程解除阻塞，处理数据

可以看到，阻塞IO模型中，用户进程在两个阶段都是阻塞状态。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742044313-ad4a87ef-910c-486e-9b5a-7a636d86b644.png)

### 2.3 网络模型-非阻塞IO

顾名思义，非阻塞IO的recvfrom操作会立即返回结果而不是阻塞用户进程。

阶段一：

- 用户进程尝试读取数据（比如网卡数据）
- 此时数据尚未到达，内核需要等待数据
- 返回异常给用户进程
- 用户进程拿到error后，再次尝试读取
- 循环往复，直到数据就绪

阶段二：

- 将内核数据拷贝到用户缓冲区
- 拷贝过程中，用户进程依然阻塞等待
- 拷贝完成，用户进程解除阻塞，处理数据
- 可以看到，非阻塞IO模型中，用户进程在第一个阶段是非阻塞，第二个阶段是阻塞状态。虽然是非阻塞，但性能并没有得到提高。而且忙等机制会导致CPU空转，CPU使用率暴增。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742058061-bbc20b2e-2235-4810-af97-852c1eec84bf.png)

### 2.4 网络模型-IO多路复用

无论是阻塞IO还是非阻塞IO，用户应用在一阶段都需要调用recvfrom来获取数据，差别在于无数据时的处理方案：

如果调用recvfrom时，恰好没有数据，阻塞IO会使CPU阻塞，非阻塞IO使CPU空转，都不能充分发挥CPU的作用。  
如果调用recvfrom时，恰好有数据，则用户进程可以直接进入第二阶段，读取并处理数据

所以怎么看起来以上两种方式性能都不好

而在单线程情况下，只能依次处理IO事件，如果正在处理的IO事件恰好未就绪（数据不可读或不可写），线程就会被阻塞，所有IO事件都必须等待，性能自然会很差。

就比如服务员给顾客点餐，分两步：

- 顾客思考要吃什么（等待数据就绪）
- 顾客想好了，开始点餐（读取数据）

要提高效率有几种办法？

方案一：增加更多服务员（多线程）  
方案二：不排队，谁想好了吃什么（数据就绪了），服务员就给谁点餐（用户应用就去读取数据）

那么问题来了：用户进程如何知道内核中数据是否就绪呢？

所以接下来就需要详细的来解决多路复用模型是如何知道到底怎么知道内核数据是否就绪的问题了

这个问题的解决依赖于提出的

文件描述符（File Descriptor）：简称FD，是一个从0 开始的无符号整数，用来关联Linux中的一个文件。在Linux中，一切皆文件，例如常规文件、视频、硬件设备等，当然也包括网络套接字（Socket）。

通过FD，我们的网络模型可以利用一个线程监听多个FD，并在某个FD可读、可写时得到通知，从而避免无效的等待，充分利用CPU资源。

阶段一：

- 用户进程调用select，指定要监听的FD集合
- 核监听FD对应的多个socket
- 任意一个或多个socket数据就绪则返回readable
- 此过程中用户进程阻塞

阶段二：

- 用户进程找到就绪的socket
- 依次调用recvfrom读取数据
- 内核将数据拷贝到用户空间
- 用户进程处理数据

当用户去读取数据的时候，不再去直接调用recvfrom了，而是调用select的函数，select函数会将需要监听的数据交给内核，由内核去检查这些数据是否就绪了，如果说这个数据就绪了，就会通知应用程序数据就绪，然后来读取数据，再从内核中把数据拷贝给用户态，完成数据处理，如果N多个FD一个都没处理完，此时就进行等待。

用IO复用模式，可以确保去读数据的时候，数据是一定存在的，他的效率比原来的阻塞IO和非阻塞IO性能都要高

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742084234-92f9c714-1af7-455d-a01d-ce0fc80a28a1.png)

IO多路复用是利用单个线程来同时监听多个FD，并在某个FD可读、可写时得到通知，从而避免无效的等待，充分利用CPU资源。不过监听FD的方式、通知的方式又有多种实现，常见的有：

- select
- poll
- epoll

其中select和pool相当于是当被监听的数据准备好之后，他会把你监听的FD整个数据都发给你，你需要到整个FD中去找，哪些是处理好了的，需要通过遍历的方式，所以性能也并不是那么好

而epoll，则相当于内核准备好了之后，他会把准备好的数据，直接发给你，咱们就省去了遍历的动作。

### 2.5 网络模型-IO多路复用-select方式

select是Linux最早是由的I/O多路复用技术：

简单说，就是我们把需要处理的数据封装成FD，然后在用户态时创建一个fd的集合（这个集合的大小是要监听的那个FD的最大值+1，但是大小整体是有限制的 ），这个集合的长度大小是有限制的，同时在这个集合中，标明出来我们要控制哪些数据，

比如要监听的数据，是1,2,5三个数据，此时会执行select函数，然后将整个fd发给内核态，内核态会去遍历用户态传递过来的数据，如果发现这里边都数据都没有就绪，就休眠，直到有数据准备好时，就会被唤醒，唤醒之后，再次遍历一遍，看看谁准备好了，然后再将处理掉没有准备好的数据，最后再将这个FD集合写回到用户态中去，此时用户态就知道了，奥，有人准备好了，但是对于用户态而言，并不知道谁处理好了，所以用户态也需要去进行遍历，然后找到对应准备好数据的节点，再去发起读请求，我们会发现，这种模式下他虽然比阻塞IO和非阻塞IO好，但是依然有些麻烦的事情， 比如说频繁的传递fd集合，频繁的去遍历FD等问题

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742104992-f46e40b8-852f-4f18-ade4-b4414f8dfb9f.png)

### 2.6 网络模型-IO多路复用模型-poll模式

poll模式对select模式做了简单改进，但性能提升不明显，部分关键代码如下：

IO流程：

- 创建pollfd数组，向其中添加关注的fd信息，数组大小自定义
- 调用poll函数，将pollfd数组拷贝到内核空间，转链表存储，无上限
- 内核遍历fd，判断是否就绪
- 数据就绪或超时后，拷贝pollfd数组到用户空间，返回就绪fd数量n
- 用户进程判断n是否大于0,大于0则遍历pollfd数组，找到就绪的fd

与select对比：

- select模式中的fd_set大小固定为1024，而pollfd在内核中采用链表，理论上无上限
- 监听FD越多，每次遍历消耗时间也越久，性能反而会下降

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742124974-5e7b7824-aeec-48d5-a251-d6dc6ee18774.png)

### 2.7 网络模型-IO多路复用模型-epoll函数

epoll模式是对select和poll的改进，它提供了三个函数：

第一个是：eventpoll的函数，他内部包含两个东西

一个是：

1、红黑树-> 记录的事要监听的FD

2、一个是链表->一个链表，记录的是就绪的FD

紧接着调用epoll_ctl操作，将要监听的数据添加到红黑树上去，并且给每个fd设置一个监听函数，这个函数会在fd数据就绪时触发，就是准备好了，现在就把fd把数据添加到list_head中去

3、调用epoll_wait函数

就去等待，在用户态创建一个空的events数组，当就绪之后，我们的回调函数会把数据添加到list_head中去，当调用这个函数的时候，会去检查list_head，当然这个过程需要参考配置的等待时间，可以等一定时间，也可以一直等， 如果在此过程中，检查到了list_head中有数据会将数据添加到链表中，此时将数据放入到events数组中，并且返回对应的操作的数量，用户态的此时收到响应后，从events中拿到对应准备好的数据的节点，再去调用方法去拿数据。

小总结：

select模式存在的三个问题：

- 能监听的FD最大不超过1024
- 每次select都需要把所有要监听的FD都拷贝到内核空间
- 每次都要遍历所有FD来判断就绪状态

poll模式的问题：

- poll利用链表解决了select中监听FD上限的问题，但依然要遍历所有FD，如果监听较多，性能会下降

epoll模式中如何解决这些问题的？

- 基于epoll实例中的红黑树保存要监听的FD，理论上无上限，而且增删改查效率都非常高
- 每个FD只需要执行一次epoll_ctl添加到红黑树，以后每次epol_wait无需传递任何参数，无需重复拷贝FD到内核空间
- 利用ep_poll_callback机制来监听FD状态，无需遍历所有FD，因此性能不会随监听的FD数量增多而下降

### 2.8、网络模型-epoll中的ET和LT

当FD有数据可读时，我们调用epoll_wait（或者select、poll）可以得到通知。但是事件通知的模式有两种：

- LevelTriggered：简称LT，也叫做水平触发。只要某个FD中有数据可读，每次调用epoll_wait都会得到通知。
- EdgeTriggered：简称ET，也叫做边沿触发。只有在某个FD有状态变化时，调用epoll_wait才会被通知。

举个栗子：

- 假设一个客户端socket对应的FD已经注册到了epoll实例中
- 客户端socket发送了2kb的数据
- 服务端调用epoll_wait，得到通知说FD就绪
- 服务端从FD读取了1kb数据回到步骤3（再次调用epoll_wait，形成循环）

结论

如果我们采用LT模式，因为FD中仍有1kb数据，则第⑤步依然会返回结果，并且得到通知  
如果我们采用ET模式，因为第③步已经消费了FD可读事件，第⑤步FD状态没有变化，因此epoll_wait不会返回，数据无法读取，客户端响应超时。

### 2.9 网络模型-基于epoll的服务器端流程

我们来梳理一下这张图

服务器启动以后，服务端会去调用epoll_create，创建一个epoll实例，epoll实例中包含两个数据

1、红黑树（为空）：rb_root 用来去记录需要被监听的FD

2、链表（为空）：list_head，用来存放已经就绪的FD

创建好了之后，会去调用epoll_ctl函数，此函数会会将需要监听的数据添加到rb_root中去，并且对当前这些存在于红黑树的节点设置回调函数，当这些被监听的数据一旦准备完成，就会被调用，而调用的结果就是将红黑树的fd添加到list_head中去(但是此时并没有完成)

3、当第二步完成后，就会调用epoll_wait函数，这个函数会去校验是否有数据准备完毕（因为数据一旦准备就绪，就会被回调函数添加到list_head中），在等待了一段时间后(可以进行配置)，如果等够了超时时间，则返回没有数据，如果有，则进一步判断当前是什么事件，如果是建立连接时间，则调用accept() 接受客户端socket，拿到建立连接的socket，然后建立起来连接，如果是其他事件，则把数据进行写出

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742144792-341f190a-f509-47b0-8b37-ce12e99bce03.png)

### 3.0 、网络模型-信号驱动

信号驱动IO是与内核建立SIGIO的信号关联并设置回调，当内核有FD就绪时，会发出SIGIO信号通知用户，期间用户应用可以执行其它业务，无需阻塞等待。

阶段一：

- 用户进程调用sigaction，注册信号处理函数
- 内核返回成功，开始监听FD
- 用户进程不阻塞等待，可以执行其它业务
- 当内核数据就绪后，回调用户进程的SIGIO处理函数

阶段二：

- 收到SIGIO回调信号
- 调用recvfrom，读取
- 内核将数据拷贝到用户空间
- 用户进程处理数据

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742158730-ad48accb-ca7a-4fbe-a936-e772547c346b.png)

当有大量IO操作时，信号较多，SIGIO处理函数不能及时处理可能导致信号队列溢出，而且内核空间与用户空间的频繁信号交互性能也较低。

#### 3.0.1 异步IO

这种方式，不仅仅是用户态在试图读取数据后，不阻塞，而且当内核的数据准备完成后，也不会阻塞

他会由内核将所有数据处理完成后，由内核将数据写入到用户态中，然后才算完成，所以性能极高，不会有任何阻塞，全部都由内核完成，可以看到，异步IO模型中，用户进程在两个阶段都是非阻塞状态。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742170680-0dd1f8e2-096c-4afc-b81b-b44f1b1d41d5.png)

#### 3.0.2 对比

最后用一幅图，来说明他们之间的区别

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742184309-e5b1205a-08eb-4ecf-95f2-1b4dbc904622.png)

### 3.1 、网络模型-Redis是单线程的吗？为什么使用单线程

Redis到底是单线程还是多线程？

- 如果仅仅聊Redis的核心业务部分（命令处理），答案是单线程
- 如果是聊整个Redis，那么答案就是多线程

在Redis版本迭代过程中，在两个重要的时间节点上引入了多线程的支持：

- Redis v4.0：引入多线程异步处理一些耗时较旧的任务，例如异步删除命令unlink
- Redis v6.0：在核心网络模型中引入 多线程，进一步提高对于多核CPU的利用率

因此，对于Redis的核心网络模型，在Redis 6.0之前确实都是单线程。是利用epoll（Linux系统）这样的IO多路复用技术在事件循环中不断处理客户端情况。

为什么Redis要选择单线程？

- 抛开持久化不谈，Redis是纯  内存操作，执行速度非常快，它的性能瓶颈是网络延迟而不是执行速度，因此多线程并不会带来巨大的性能提升。
- 多线程会导致过多的上下文切换，带来不必要的开销
- 引入多线程会面临线程安全问题，必然要引入线程锁这样的安全手段，实现复杂度增高，而且性能也会大打折扣

### 3.2 、Redis的单线程模型-Redis单线程和多线程网络模型变更

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742202762-ad41a227-9e0f-45c2-afa1-66990a700a7e.png)

当我们的客户端想要去连接我们服务器，会去先到IO多路复用模型去进行排队，会有一个连接应答处理器，他会去接受读请求，然后又把读请求注册到具体模型中去，此时这些建立起来的连接，如果是客户端请求处理器去进行执行命令时，他会去把数据读取出来，然后把数据放入到client中， clinet去解析当前的命令转化为redis认识的命令，接下来就开始处理这些命令，从redis中的command中找到这些命令，然后就真正的去操作对应的数据了，当数据操作完成后，会去找到命令回复处理器，再由他将数据写出。

## 3、Redis通信协议-RESP协议

Redis是一个CS架构的软件，通信一般分两步（不包括pipeline和PubSub）：

客户端（client）向服务端（server）发送一条命令

服务端解析并执行命令，返回响应结果给客户端

因此客户端发送命令的格式、服务端响应结果的格式必须有一个规范，这个规范就是通信协议。

而在Redis中采用的是RESP（Redis Serialization Protocol）协议：

Redis 1.2版本引入了RESP协议

Redis 2.0版本中成为与Redis服务端通信的标准，称为RESP2

Redis 6.0版本中，从RESP2升级到了RESP3协议，增加了更多数据类型并且支持6.0的新特性--客户端缓存

但目前，默认使用的依然是RESP2协议，也是我们要学习的协议版本（以下简称RESP）。

在RESP中，通过首字节的字符来区分不同数据类型，常用的数据类型包括5种：

单行字符串：首字节是 ‘+’ ，后面跟上单行字符串，以CRLF（ "\r\n" ）结尾。例如返回"OK"： "+OK\r\n"

错误（Errors）：首字节是 ‘-’ ，与单行字符串格式一样，只是字符串是异常信息，例如："-Error message\r\n"

数值：首字节是 ‘:’ ，后面跟上数字格式的字符串，以CRLF结尾。例如：":10\r\n"

多行字符串：首字节是 ‘$’ ，表示二进制安全的字符串，最大支持512MB：

如果大小为0，则代表空字符串："$0\r\n\r\n"

如果大小为-1，则代表不存在："$-1\r\n"

数组：首字节是 ‘*’，后面跟上数组元素个数，再跟上元素，元素数据类型不限:

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742217222-8e066a5c-e835-4e22-ace4-a372dab2a058.png)

### 3.1、Redis通信协议-基于Socket自定义Redis的客户端

Redis支持TCP通信，因此我们可以使用Socket来模拟客户端，与Redis服务端建立连接：

```
public class Main {

    static Socket s;
    static PrintWriter writer;
    static BufferedReader reader;

    public static void main(String[] args) {
        try {
            // 1.建立连接
            String host = "192.168.150.101";
            int port = 6379;
            s = new Socket(host, port);
            // 2.获取输出流、输入流
            writer = new PrintWriter(new OutputStreamWriter(s.getOutputStream(), StandardCharsets.UTF_8));
            reader = new BufferedReader(new InputStreamReader(s.getInputStream(), StandardCharsets.UTF_8));

            // 3.发出请求
            // 3.1.获取授权 auth 123321
            sendRequest("auth", "123321");
            Object obj = handleResponse();
            System.out.println("obj = " + obj);

            // 3.2.set name 虎哥
            sendRequest("set", "name", "虎哥");
            // 4.解析响应
            obj = handleResponse();
            System.out.println("obj = " + obj);

            // 3.2.set name 虎哥
            sendRequest("get", "name");
            // 4.解析响应
            obj = handleResponse();
            System.out.println("obj = " + obj);

            // 3.2.set name 虎哥
            sendRequest("mget", "name", "num", "msg");
            // 4.解析响应
            obj = handleResponse();
            System.out.println("obj = " + obj);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 5.释放连接
            try {
                if (reader != null) reader.close();
                if (writer != null) writer.close();
                if (s != null) s.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    private static Object handleResponse() throws IOException {
        // 读取首字节
        int prefix = reader.read();
        // 判断数据类型标示
        switch (prefix) {
            case '+': // 单行字符串，直接读一行
                return reader.readLine();
            case '-': // 异常，也读一行
                throw new RuntimeException(reader.readLine());
            case ':': // 数字
                return Long.parseLong(reader.readLine());
            case '$': // 多行字符串
                // 先读长度
                int len = Integer.parseInt(reader.readLine());
                if (len == -1) {
                    return null;
                }
                if (len == 0) {
                    return "";
                }
                // 再读数据,读len个字节。我们假设没有特殊字符，所以读一行（简化）
                return reader.readLine();
            case '*':
                return readBulkString();
            default:
                throw new RuntimeException("错误的数据格式！");
        }
    }

    private static Object readBulkString() throws IOException {
        // 获取数组大小
        int len = Integer.parseInt(reader.readLine());
        if (len <= 0) {
            return null;
        }
        // 定义集合，接收多个元素
        List<Object> list = new ArrayList<>(len);
        // 遍历，依次读取每个元素
        for (int i = 0; i < len; i++) {
            list.add(handleResponse());
        }
        return list;
    }

    // set name 虎哥
    private static void sendRequest(String ... args) {
        writer.println("*" + args.length);
        for (String arg : args) {
            writer.println("$" + arg.getBytes(StandardCharsets.UTF_8).length);
            writer.println(arg);
        }
        writer.flush();
    }
}
```

### 3.2、Redis内存回收-过期key处理

Redis之所以性能强，最主要的原因就是基于内存存储。然而单节点的Redis其内存大小不宜过大，会影响持久化或主从同步性能。  
我们可以通过修改配置文件来设置Redis的最大内存：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742235794-e1a90916-003a-4cc8-b641-ef344038341d.png)

当内存使用达到上限时，就无法存储更多数据了。为了解决这个问题，Redis提供了一些策略实现内存回收：

内存过期策略

在学习Redis缓存的时候我们说过，可以通过expire命令给Redis的key设置TTL（存活时间）：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742245773-df6e7a2d-0ec1-4c95-84f9-52317c3352e4.png)

可以发现，当key的TTL到期以后，再次访问name返回的是nil，说明这个key已经不存在了，对应的内存也得到释放。从而起到内存回收的目的。

Redis本身是一个典型的key-value内存存储数据库，因此所有的key、value都保存在之前学习过的Dict结构中。不过在其database结构体中，有两个Dict：一个用来记录key-value；另一个用来记录key-TTL。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742255442-3475bd8b-2a3f-4094-83db-94bd92569a1f.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742267446-357038b5-356b-4317-80f7-eb95700536a7.png)

这里有两个问题需要我们思考：  
Redis是如何知道一个key是否过期呢？

利用两个Dict分别记录key-value对及key-ttl对

是不是TTL到期就立即删除了呢？

惰性删除

惰性删除：顾明思议并不是在TTL到期后就立刻删除，而是在访问一个key的时候，检查该key的存活时间，如果已经过期才执行删除。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742278588-499b680d-f48d-41aa-bbff-5ac4743b7850.png)

周期删除

周期删除：顾明思议是通过一个定时任务，周期性的抽样部分过期的key，然后执行删除。执行周期有两种：  
Redis服务初始化函数initServer()中设置定时任务，按照server.hz的频率来执行过期key清理，模式为SLOW  
Redis的每个事件循环前会调用beforeSleep()函数，执行过期key清理，模式为FAST

周期删除：顾明思议是通过一个定时任务，周期性的抽样部分过期的key，然后执行删除。执行周期有两种：  
Redis服务初始化函数initServer()中设置定时任务，按照server.hz的频率来执行过期key清理，模式为SLOW  
Redis的每个事件循环前会调用beforeSleep()函数，执行过期key清理，模式为FAST

SLOW模式规则：

- 执行频率受server.hz影响，默认为10，即每秒执行10次，每个执行周期100ms。
- 执行清理耗时不超过一次执行周期的25%.默认slow模式耗时不超过25ms
- 逐个遍历db，逐个遍历db中的bucket，抽取20个key判断是否过期
- 如果没达到时间上限（25ms）并且过期key比例大于10%，再进行一次抽样，否则结束
- FAST模式规则（过期key比例小于10%不执行 ）：
- 执行频率受beforeSleep()调用频率影响，但两次FAST模式间隔不低于2ms
- 执行清理耗时不超过1ms
- 逐个遍历db，逐个遍历db中的bucket，抽取20个key判断是否过期  
    如果没达到时间上限（1ms）并且过期key比例大于10%，再进行一次抽样，否则结束

小总结：

RedisKey的TTL记录方式：

在RedisDB中通过一个Dict记录每个Key的TTL时间

过期key的删除策略：

惰性清理：每次查找key时判断是否过期，如果过期则删除

定期清理：定期抽样部分key，判断是否过期，如果过期则删除。  
定期清理的两种模式：

SLOW模式执行频率默认为10，每次不超过25ms

FAST模式执行频率不固定，但两次间隔不低于2ms，每次耗时不超过1ms

### 3.3 Redis内存回收-内存淘汰策略

内存淘汰：就是当Redis内存使用达到设置的上限时，主动挑选部分key删除以释放更多内存的流程。Redis会在处理客户端命令的方法processCommand()中尝试做内存淘汰：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742290278-0692b752-602c-4f04-aef6-73b1c92c3f29.png)

淘汰策略

Redis支持8种不同策略来选择要删除的key：

- noeviction： 不淘汰任何key，但是内存满时不允许写入新数据，默认就是这种策略。
- volatile-ttl： 对设置了TTL的key，比较key的剩余TTL值，TTL越小越先被淘汰
- allkeys-random：对全体key ，随机进行淘汰。也就是直接从db->dict中随机挑选
- volatile-random：对设置了TTL的key ，随机进行淘汰。也就是从db->expires中随机挑选。
- allkeys-lru： 对全体key，基于LRU算法进行淘汰
- volatile-lru： 对设置了TTL的key，基于LRU算法进行淘汰
- allkeys-lfu： 对全体key，基于LFU算法进行淘汰
- volatile-lfu： 对设置了TTL的key，基于LFI算法进行淘汰  
    比较容易混淆的有两个：

- LRU（Least Recently Used），最少最近使用。用当前时间减去最后一次访问时间，这个值越大则淘汰优先级越高。
- LFU（Least Frequently Used），最少频率使用。会统计每个key的访问频率，值越小淘汰优先级越高。

Redis的数据都会被封装为RedisObject结构：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742301009-64f73c16-1099-45eb-9d9d-69147495a8be.png)

LFU的访问次数之所以叫做逻辑访问次数，是因为并不是每次key被访问都计数，而是通过运算：

- 生成0~1之间的随机数R
- 计算 (旧次数 * lfu_log_factor + 1)，记录为P
- 如果 R < P ，则计数器 + 1，且最大不超过255
- 访问次数会随时间衰减，距离上一次访问时间每隔 lfu_decay_time 分钟，计数器 -1

最后用一副图来描述当前的这个流程吧

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667742316059-e8931078-0f4a-4b13-98ed-b4de630b1a41.png)

## 4、结束语

亲爱的小伙伴们，我们的redis到这里就结束了，希望小伙伴们好好学习，找到一份满意的工作！传智为你加油。

[^1]: 

[^2]: - [ ] 
