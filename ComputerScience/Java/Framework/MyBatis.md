![[MyBatis Doc.png|1190x445]]
# 概述

## 简介
- MyBatis 是一个支持普通 SQL 查询、存储过程及高级映射的持久层框架。
- 半自动化的 ORM（对象关系映射）框架，介于 JDBC 和全自动 ORM（如 Hibernate）之间。
- 前身是 iBatis，Apache 基金会项目，后迁移至 Google Code 并更名为 MyBatis
- 特点：
	- 灵活性高，支持动态 SQL 和复杂查询
	- 提供 XML 和注解两种配置方式
	- 支持一对一、一对多、多对多复杂关系映射
	- 支持延迟加载和缓存机制，提升性能
	- 与 Spring 框架无缝集成
## 需求
- JDBC的局限性
	- 问题：
		- **代码冗余**：JDBC 需要手动编写大量重复的代码（如连接管理、SQL 执行、结果集处理）。
		- **SQL 与代码耦合**：SQL 语句硬编码在 Java 代码中，难以维护和修改。
		- **参数和结果处理繁琐**：手动设置参数、处理结果集，容易出错且效率低。
		- **资源管理复杂**：需要手动管理数据库连接、关闭资源，容易引发资源泄漏。
	- MyBatis 的解决方案：
		- **SQL 与代码分离**：通过 XML 或注解配置 SQL，降低耦合，便于维护。
		- **自动参数映射**：支持多种参数类型（基本类型、POJO、Map 等），简化参数绑定。
		- **结果集自动映射**：通过 resultMap 或注解将查询结果映射为 Java 对象，减少手动处理。
		- **连接管理**：通过数据源和 SqlSession 管理数据库连接，自动处理资源释放。
- 全自动化 ORM 框架的不足
	- 问题：
		- **灵活性不足**：如 Hibernate 的 HQL 语言限制了复杂 SQL 的编写，难以优化。
		- **性能开销**：全自动化 ORM 常自动生成 SQL，可能导致性能问题，尤其在复杂查询场景。
		- **学习曲线陡峭**：Hibernate 等框架配置复杂，调试困难。
		- **黑盒操作**：开发者对底层 SQL 控制较少，难以应对特殊需求。
	- MyBatis 的解决方案：
		- **SQL 控制权**：开发者完全掌控 SQL 语句，易于编写高效、优化的查询。
		- **轻量级设计**：半自动化机制，减少不必要的性能开销。
		- **简单易用**：提供 XML 和注解两种配置方式，学习成本较低。
		- **动态 SQL 支持**：通过 if、foreach、choose 等标签，灵活应对复杂业务逻辑。
## 对比

|   |   |   |   |   |
|---|---|---|---|---|
|特性|MyBatis|Hibernate|Spring JDBC|JPA|
|**ORM 自动化程度**|半自动化，需手动编写 SQL|全自动化，自动生成 SQL|无 ORM，手动处理映射|全自动化，自动生成 SQL|
|**SQL 控制**|高度控制，SQL 完全自定义|有限控制，依赖 HQL 或自动 SQL|完全控制，SQL 直接嵌入代码|有限控制，依赖 JPQL 或自动 SQL|
|**动态 SQL**|支持（if、foreach、choose 等）|有限（HQL 或 Criteria API）|不支持，需手动拼接|有限（JPQL 或 Criteria API）|
|**结果映射**|灵活，支持复杂映射（resultMap）|自动映射，支持复杂关系|手动映射（RowMapper）|自动映射，支持复杂关系|
|**缓存机制**|一级/二级缓存，需手动配置|一级/二级缓存，默认支持|无内置缓存，依赖外部实现|一级/二级缓存，依赖实现框架|
|**延迟加载**|支持，需配置|默认支持|不支持|默认支持|
|**事务管理**|集成 Spring 事务，简单配置|集成 Spring 事务，复杂配置|集成 Spring 事务，简单配置|集成 Spring 事务，复杂配置|
|**学习曲线**|适中，需熟悉动态 SQL 和 XML|较陡峭，需掌握 HQL 和配置|简单，接近原生 JDBC|适中，需掌握 JPQL 和注解|
|**生态集成**|与 Spring/Spring Boot 无缝集成|与 Spring/Spring Boot 集成良好|原生 Spring 组件，集成最佳|与 Spring 集成良好，依赖实现框架|

# 开始

## 安装
- Maven
	- 引入依赖：
		```xml
		<dependency>
		    <groupId>mysql</groupId>
		    <artifactId>mysql-connector-java</artifactId>
		    <version>8.0.33</version>
		</dependency>
		<dependency>
		    <groupId>org.mybatis</groupId>
		    <artifactId>mybatis</artifactId>
		    <version>3.5.7</version>
		</dependency>
		```
## 使用
1. **创建 MyBatis 主配置文件**
	- 在项目资源目录（`src/main/resources`）下创建 `mybatis-config.xml` 文件，连接数据库和全局配置。
	- 说明：
		- `settings`：配置全局参数，如驼峰命名转换、延迟加载等。
		- `typeAliases`：为实体类设置别名，简化 XML 中的类名引用。
		- `environments`：配置数据库连接信息，`POOLED` 表示使用连接池。
		- `mappers`：指定 Mapper XML 文件或接口。
	- 示例：
		```xml
		<?xml version="1.0" encoding="UTF-8" ?>
		<!DOCTYPE configuration
		        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
		        "http://mybatis.org/dtd/mybatis-3-config.dtd">
		<configuration>
		    <!-- 全局设置 -->
		    <settings>
		        <setting name="mapUnderscoreToCamelCase" value="true"/>
		        <setting name="lazyLoadingEnabled" value="true"/>
		    </settings>
		
		    <!-- 类型别名 -->
		    <typeAliases>
		        <package name="com.thyonox.domain"/>
		    </typeAliases>
		
		    <!-- 数据库环境 -->
		    <environments default="development">
		        <environment id="development">
		            <transactionManager type="JDBC"/>
		            <dataSource type="POOLED">
		                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
		                <property name="url" value="jdbc:mysql://localhost:3306/test?useSSL=false&amp;serverTimezone=UTC"/>
		                <property name="username" value="root"/>
		                <property name="password" value="root"/>
		            </dataSource>
		        </environment>
		    </environments>
		
		    <!-- Mapper 文件 -->
		    <mappers>
		        <mapper resource="mapper/UserMapper.xml"/>
		    </mappers>
		</configuration>
		
		```
2. **创建 Mapper 文件**
	- 在 `resources` 目录下的 `mapper` 包里创建 Mapper XML 文件，命名格式为：`实体类名 + Mapper.xml`。
	- 说明：
		- `namespace`：绑定 Mapper 接口的包路径。
		- `select`：定义查询语句，`id` 对应接口方法名，`resultType` 指定返回类型。
	- 示例：
		```xml
		<?xml version="1.0" encoding="UTF-8" ?>
		<!DOCTYPE mapper
		        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
		        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
		<mapper namespace="com.thyonox.mapper.UserMapper">
		    <select id="selectUserById" resultType="com.thyonox.domain.User">
		        SELECT * FROM mb_user WHERE user_id = #{id}
		    </select>
		</mapper>
		```
3. **创建 Mapper 接口**
	- 在 `java` 目录下的 `mapper` 包里创建 Mapper 接口，命名格式为：`实体类名 + Mapper.class`。
	- 说明：
		- 接口方法名需与 Mapper XML 中的 `id` 一致。
		- 返回类型和参数需与 XML 中的 `resultType` 和 `parameterType` 匹配。
		- 相当于 JDBC 的 Dao 层，只是不需要手动创建实现类，而是通过接口的代理对象执行方法。
	- 示例：
		```java
		public interface UserMapper {
		    User selectUserById(Long id);
		}
		```
4. **创建实体类**
	- 在实体类包下创建对应数据结构的实体类，对应数据库表结构。
	- 示例：
		```java
		public class User {
		    private Long userId;
		
		    private String userName;
		
		    private String email;
		
		    private String password;
		
			...
		}
		```
5. **调用 Mybatis API**
	- 加载 Mybatis 配置文件，创建 Mapper 接口代理对象，执行方法。
	- 说明：
		- `SqlSessionFactory.openSession(true);`：表示自动提交事务，如果不设置自动提交事务，需要在方法调用结束后，手动提交事务 `sqlSession.commit()`。
		- `SqlSessionFactory` 是单例模式，应用启动时创建。
		- `SqlSession` 非线程安全，每次操作需新建。
		- 使用 `try-with-resources` 确保 `SqlSession` 正确关闭。
	- 示例：
		```java
		public class MBApplicationTest {
		
		    @Test
		    public void test() throws IOException {
		        InputStream input = Resources.getResourceAsStream("mybatis-config.xml");
		        SqlSessionFactory build = new SqlSessionFactoryBuilder().build(input);
		
		        try (SqlSession sqlSession = build.openSession(true)) {
		            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
		            User user = mapper.selectUserById(1L);
		            System.out.println(user);
		        }
		    }
		}
		
		```
# 概念
## 核心组件






## Mapper XML文件







## 动态SQL
### 动态SQL简介



### 动态SQL标签




### OGNL表达式














# 配置
## Mybatis 配置文件
### 文件结构
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties />
    <settings />
    <typeAliases />
    <typeHandlers />
    <objectFactory />
    <plugins />
    <environments>
        <environment>
            <transactionManager />
            <dataSource />
        </environment>
    </environments>
    <databaseIdProvider />
    <mappers />
</configuration>
```
### 元素详解
- `<properties>`
	- **作用**：定义属性键值对，供配置文件其他部分引用，增强配置灵活性。
	- **用法**：可通过外部文件（如 `db.properties`）或直接定义属性，然后在内部通过 `${属性名}` 获取属性值。
	- **示例**：
		- `mybatis-config.xml`：
			```xml
			<properties resource="db.properties">
			    <property name="jdbc.url" value="jdbc:mysql://localhost:3306/test"/>
			</properties>
			<environments default="development">
			    <environment id="development">
			        <transactionManager type="JDBC"/>
			        <dataSource type="POOLED">
			            <property name="driver" value="${jdbc.driver}"/>
			            <property name="url" value="${jdbc.url}"/>
			            <property name="username" value="${jdbc.username}"/>
			            <property name="password" value="${jdbc.password}"/>
			        </dataSource>
			    </environment>
			</environments>
			```
		- 外部 `db.properties` 文件：
			```
			jdbc.driver=com.mysql.cj.jdbc.Driver
			jdbc.url=jdbc:mysql://localhost:3306/test?useSSL=false&serverTimezone=UTC
			jdbc.username=root
			jdbc.password=root
			```
- `<settings>`
	- **作用**：配置 MyBatis 的运行时行为，影响全局功能。
	- **常用配置项**：
		- `cacheEnabled`：全局缓存开关（默认 true）。
		- `lazyLoadingEnabled`：延迟加载开关（默认 false）。
		- `mapUnderscoreToCamelCase`：下划线转驼峰命名（默认 false）。
		- `logImpl`：日志实现（如 SLF4J、STDOUT_LOGGING）。
	- **示例**：
		```xml
		<settings>
		    <setting name="cacheEnabled" value="true"/>
		    <setting name="lazyLoadingEnabled" value="true"/>
		    <setting name="aggressiveLazyLoading" value="false"/>
		    <setting name="mapUnderscoreToCamelCase" value="true"/>
		    <setting name="logImpl" value="STDOUT_LOGGING"/>
		</settings>
		```
- `<typeAliases>`
	- **作用**：为 Java 类定义别名，简化 XML 或注解中的类名引用。
	- **用法**：
		- 单个类定义：`<typeAlias alias="User" type="com.example.entity.User"/>`
		- 包扫描：`<package name="com.example.entity"/>`
	- **说明**：包扫描会为类生成默认别名（类名首字母小写，如 User -> user）。
	- **示例**：
		```xml
		<typeAliases>
		    <typeAlias alias="User" type="com.example.entity.User"/>
		    <package name="com.example.entity"/>
		</typeAliases>
		```
- `<typeHandlers>`
	- **作用**：自定义类型处理器，处理 Java 类型与 JDBC 类型之间的转换。
	- **用法**：默认提供常见类型处理器，可自定义处理复杂类型（如 JSON、枚举）。
	- **示例**：
		```xml
		<typeHandlers>
		    <typeHandler javaType="java.time.LocalDateTime" jdbcType="TIMESTAMP" handler="org.apache.ibatis.type.LocalDateTimeTypeHandler"/>
		</typeHandlers>
		```
- `<objectFactory>`
	- **作用**：自定义对象创建方式，默认用于实例化结果对象。
	- **用法**：一般使用默认实现，特殊场景可自定义。
	- **示例**：
		```xml
		<objectFactory type="com.example.CustomObjectFactory">
		    <property name="someProperty" value="value"/>
		</objectFactory>
		```
- `<plugins>`
	- **作用**：配置 MyBatis 插件（如分页插件、SQL 拦截器）。
	- **用法**：拦截 Executor、StatementHandler 等核心组件。
	- **示例**：
		```xml
		<plugins>
		    <plugin interceptor="com.example.MyInterceptor">
		        <property name="someProperty" value="value"/>
		    </plugin>
		</plugins>
		```
- `<environments>`
	- **作用**：定义数据库环境，支持多环境切换（如开发、测试、生产）通过 `default` 属性切换。
	- **子元素**：
		- `<transactionManager>`：事务管理类型（JDBC、MANAGED）。
		- `<dataSource>`：数据源类型（POOLED、UNPOOLED、JNDI）。
			- `POOLED`：使用连接池，适合生产环境。
			- `UNPOOLED`：每次创建新连接，适合简单应用。
			- `JNDI`：使用容器提供的数据源（如 Tomcat）。
	- **示例**：
		```xml
		<environments default="development">
		    <environment id="development">
		        <transactionManager type="JDBC"/>
		        <dataSource type="POOLED">
		            <property name="driver" value="${jdbc.driver}"/>
		            <property name="url" value="${jdbc.url}"/>
		            <property name="username" value="${jdbc.username}"/>
		            <property name="password" value="${jdbc.password}"/>
		        </dataSource>
		    </environment>
		</environments>
		```
- `<databaseIdProvider>`
	- **作用**：支持多数据库 SQL 适配，动态选择 SQL 语句。
	- **用法**：通过 `databaseId` 区分数据库类型（如 MySQL、Oracle）。
	- **示例**：
		```xml
		<databaseIdProvider type="DB_VENDOR">
		    <property name="MySQL" value="mysql"/>
		    <property name="Oracle" value="oracle"/>
		</databaseIdProvider>
		```
- `<mappers>`
	- **作用**：指定 Mapper 文件或接口，定义 SQL 映射。
	- **用法**：
		- 单个文件：`<mapper resource="com/example/mapper/UserMapper.xml"/>`
		- 包扫描：`<package name="com.example.mapper"/>`
	- **说明**：
		- 要使用包路径扫描，需要在 `resources` 目录下定义和 `java` 包中相同的目录结构，让打包后的 Mapper 接口和 Mapper 文件在同一目录。
	- **示例**：
		```xml
		<mappers>
		    <mapper resource="com/example/mapper/UserMapper.xml"/>
		    <package name="com.example.mapper"/>
		</mappers>
		```













# 思考




# 附录
- 官方文档：[MyBatis 3 | 简介 – mybatis](https://mybatis.org/mybatis-3/zh_CN/index.html)
- 









# 4. MyBatis的增删改查

## 4.1 添加

`parameterType`属性表示参数类型，DAO 接口中传入方法的参数类型。在增删查改中都可以添加该属性。和 @Param类似，根据参数类型进行匹配。

```xml
<!--int insertUser(id(自增)、name、password) -->
<insert id="insertUser" parameterType="user">
    insert into user values (null,'大白','12345')
</insert>
```

## 4.2 删除

```xml
<!-- int deleteUser()-->
<delete id="deleteUser">
    delete from user where id=1
</delete>
```

## 4.3 查询实体

查询语句需要指明结果映射（`resultType`），意思是**查询出的结果映射为什么结构**。可以在核心配置文件中的`<typeAliases>`标签中设置别名。

```xml
<!--User selectUser()-->
<select id="selectUser" resultType="com.dong.entity.User">
    select * from user where id=10
</select>
```

## 4.4 查询集合

```xml
<!--List<User> selectUsers()-->
<select id="selectUsers" resultType="com.dong.entity.User">
    select * from user
</select>
```

## 4.5 修改

```xml
<!--int updataUser()-->
<updata id="updataUser">
    updata user set name='张三' where id=5
</updata>
```

## 4.6 细节

- select 标签必须设置属性 `resultType` 或者 `resultMap`，用于设置实体类和数据库表的映射关系。
    - `resultType`: 自动映射，用于**属性名与表中字段一致**的情况。
    - `resultMap`: 自定义映射，用于**一对多或多对一或属性名与表中字段不一致**的情况。
- **当查询的数据为多条的时候，不能使用实体类作为返回值**，只能使用集合，否则抛出异常 `TooManyResultsException`。

# 5. MyBatis获取参数的两种方式

获取参数表示在调用 Mapper 接口中的方法时，传入的参数。

## 5.1#

- 是 sql 的参数占位符。
- MyBatis 在处理 `#{}` 的时候,会把 #{} 替换为 `?` ,调用 `PerparedStatement` 的 `setXxx()` 方法`按序`来赋值。
- 使用 `#{}` 可以有效防止 SQL注入。
- 自动添加单引号。

## 5.2 $

- 是 properties 文件中的变量占位符。
- 它可以用于标签属性值和 sql 内部，属于静态文本替换。
- ${driver}替换成 `com.mysql.cj.jdbc.Driver`。
- 若为字符串或日期类型的字段进行赋值时，需手动添加单引号。

# 6. 参数类型

## 6.1 单个字面量参数

若 mapper 接口中方法的参数是单个参数，可以使用`#{}` 或 `${}`以**任意参数名**名称获取

```xml
<!--int deleteUser(int userId);-->
<delete id="deleteUser">
    delete from user where id=#{userId}
</delete>
```

## 6.2 多个字面量参数

- 若 mapper 接口中方法的参数是多个参数，MyBatis 会自动的将这些参数放入`map`集合中。
    1. 以 arg0、arg1、…为键，以参数为值。
    2. 以 param1、param2、…为键，以参数为值。
- 以 #{} 或 ${} 访问 map 集合中的键，就可以拿到值。
- arg 是以 `arg0开始`，param 以 `param1开始`。

```xml
<!--int updateUser(String name,int userId);-->
<update id="updateUser">
    update user set name = #{arg0} where id=#{arg1}
</update>
```

## 6.3 map集合类型参数

若 mapper 接口的方法参数为多个时，可以手动创建一个 map 集合，将参数放入 map 集合中，使用 #{} 或 ${} 访问 map 集合对应的键就可以拿到相应的值。

```xml
<!--int insertUserMap(Map<String,Object> map);-->
<insert id="insertUserMap">
    insert into user values (null,#{name},#{password})
</insert>
```

## 6.4 实体类型参数

若 mapper 接口方法参数为实体类型时,使用 #{} 或 ${} 访问实体对象的属性名拿到属性值。

```xml
<!--int insertUserUser(User user);-->
<insert id="insertUserUser">
    insert into user values (null,#{name},#{password})
</insert>
```

## 6.5 @Param

- 可以通过 `@Param` 注解标识 mapper 接口中的方法参数，会将这些参数放入 map （`ParamNameResolver`进行封装）集合中
    - 以 @Param 注解的 value 属性值为键，以参数为值
    - 以 param1，param2，…为键，以参数为值
- 只需通过 #{} 或 ${} 访问集合中的键，就能拿到相应的参数值

```xml
<!--User selectUserAN(@Param("name") String name, @Param("password") String password);-->
<select id="selectUserAN" resultType="com.dong.entity.User">
    select * from user where name=#{name} and password=#{password}
</select>
```

# 7. MyBatis的查询功能

## 7.1 返回单个实体对象

```xml
<!--User selectUser(int userId);-->
<select id="selectUser" resultType="com.dong.entity.User">
    select * from user where id=#{userId}
</select>
```

## 7.2 返回多个对象进Lis集合

```xml
<!-- List<User> selectUsers();-->
<select id="selectUsers" resultType="com.dong.entity.User">
    select * from user
</select>
```

## 7.3 返回单个对象进Map集合

注意将 resultType 设置为 map。

```xml
<!--Map<String,Object> selectMap();-->
<select id="selectMap" resultType="map">
    select *from user where id=#{userId}
</select>
```

## 7.4 返回多个数据进Map集合

- 方法一

```xml
<!--List<Map<String,Object>> selectUserListMap();-->
<select id="selectUserListMap" resultType="map">
    select *from user
</select>
```

- 方法二 使用 `@MapKey("")`设置 Map 的键，值是每条数据所对应的 map 集合。

```xml
<!--@MayKey("id")    
		Map<String,Object> selectUserListMap();-->
<select id="selectUserListMap" resultType="map">
    select *from user
</select>
```

# 8.特殊SQL的执行

## 8.1 模糊查询

```xml
<!--List<User> selectUserByLike(@Param("name")String name);-->
<select id="selectUserByLike" resultType="com.dong.pojo.User">
    <!--select * from t_user where username like '%${name}%'-->
    <!--select * from t_user where username like concat('%',#{name},'%')-->    
    select * from mybatis_demo where name like "%"#{name}"%"
</select>
```

## 8.2 批量删除

只能使用 `${}` ，如果使用 `#{}` 会将解析后 `delete from user where id in ('1,2,3')`中的 `'1,2,3'`看做一个整体，也就是只有 id 为’1,2,3’的数据才会被删除。 这是因为 `#{}`会自动添加单引号。

```xml
<!--int deleteMore(@Param("userId") String userId);--><delete id="deleteMore">
    delete from user where id in (${userId})
</delete>
/*
    调用
*/
mapper.deleteMore("1,2,3");
```

## 8.3 动态设置表名

只能使用 `${}`，因为表名不能添加单引号。

```xml
<!--List<User> selectBytable(@Param("table") String table);-->
<select id="selectBytable" resultType="com.dong.entity.User">
    select * from ${table}
</select>
```

## 8.4 获取自增的主键

给表中添加记录的同时，也会将该记录所在的自增主键值赋值给实体类属性。 比如： 普通插入对象时，创建的实体类的 id 为空（为了保持表中主键自增）:

```xml
<!--int insertUser(String name,String password);--><insert id="insertUserUser">
    insert into user values (null,#{name},#{password})
</insert>
/*
    调用
*/
User user = new User(null,"奥特曼","1009");
userMapper.insertUserUser(user);
System.out.println(user);
/*
    输出
*/
User{id=null, name='奥特曼', password='1009'}
```

获取自增主键，并赋值给实体类属性:

- `useGeneratedKeys="true"`：设置使用自增的主键
- `keyProperty="id"`：自增的主键放在映射实体类中的 id 属性上

```xml
<!--int insertUser(String name,String password);-->
<insert id="insertUserUser" useGeneratedKeys="true" keyProperty="id">
    insert into user values (null,#{name},#{password})
</insert>
/*
    调用
*/
User user = new User(null,"奥特曼","1009");
userMapper.insertUserUser(user);
System.out.println(user);
/*
    输出
*/
User{id=17, name='奥特曼', password='1009'}
```

# 9. 自定义resultMap

> resultMap: 自定义映射，用于一对多或多对一或属性名与表中字段不一致的情况。

## 9.1 字段与属性名不一致

使用 ResultMap 进行映射，~~需要列出全部属性，即使属性名和字段一致。~~

- 属性：
    - id：表示自定义映射的唯一标识，不能重复。
    - type：查询的数据要映射的实体类的类型。
- 子标签
    - id：设置主键的映射关系。
    - result：设置普通字段的映射关系。
        - column：表中字段名。
        - property：实体类属性名。

```xml
<!--设置自定义映射-->
<resultMap id="result" type="com.dong.pojo.Result">
    <id column="result_id" property="id"/>
    <result column="result_name" property="name"/>
    <result column="result_password" property="password"/>
</resultMap>
<select id="select" resultMap="result">
    select * from mybatis_demo where id = 1
</select>
```

如果字段符合数据库规则（用`_`），而属性名使用驼峰命名法，还有两种方法可以保证字段与属性的一致：

- 可以在 SQL 语句中通过给字段设置别名（属性名）。
    
- 可以在 MyBatis 配置文件中的 `<settings>` 标签中设置，让字段`_`自动装换为驼峰。
    
    ```xml
    <configuration>
        <settings>
            <setting name="mapUnderscoreToCamelCase" value="true"/>
        </settings>
    </configuration>
    ```
    

还可以通过results注解

```java
public interface UserMapper {
    @Select("SELECT id, username AS name, user_age AS age FROM users")
    @Results({
        @Result(property = "name", column = "username"),
        @Result(property = "age", column = "user_age")
    })
    List<User> getAllUsers();
}
```

配置文件

```yaml
mybatis:
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.maktub.userservice.entity
  configuration:
    map-underscore-to-camel-case: on
```

## 9.2 多对一映射

```xml
public class Emp {
    private Integer eid;
    private String empName;
    private Integer age;
    private String sex;
    private String email;
    private Dept dept;
    //...构造器、get、set方法等
}
```

### 9.2.3 级联方式处理映射

```xml
<resultMap id="empAndDeptResultMapOne" type="Emp">
    <id property="eid" column="eid"></id>
    <result property="empName" column="emp_name"></result>
    <result property="age" column="age"></result>
    <result property="sex" column="sex"></result>
    <result property="email" column="email"></result>
    <result property="dept.did" column="did"></result>
    <result property="dept.deptName" column="dept_name"></result>
</resultMap>
<!--Emp getEmpAndDept(@Param("eid")Integer eid);-->
<select id="getEmpAndDept" resultMap="empAndDeptResultMapOne">
    select * from t_emp left join t_dept on t_emp.eid = t_dept.did where t_emp.eid = #{eid}
</select>
```

### 9.2.4 association处理映射

- association：处理多对一的映射关系
- property：需要处理多对一的映射关系的属性名
- javaType：该属性的类型

```xml
<resultMap id="empAndDeptResultMapTwo" type="Emp">
    <id property="eid" column="eid"></id>
    <result property="empName" column="emp_name"></result>
    <result property="age" column="age"></result>
    <result property="sex" column="sex"></result>
    <result property="email" column="email"></result>
    <association property="dept" javaType="Dept">
        <id property="did" column="did"></id>
        <result property="deptName" column="dept_name"></result>
    </association>
</resultMap>
<!--Emp getEmpAndDept(@Param("eid")Integer eid);--><select id="getEmpAndDept" resultMap="empAndDeptResultMapTwo">
    select * from t_emp left join t_dept on t_emp.eid = t_dept.did where t_emp.eid = #{eid}
</select>
```

也可以将子字段的映射写成一个单独的隐射关系，然后在association标签中使用`resultMap` 指向该映射

### 9.2.5 分步查询处理映射

1. 查询员工信息

- select：设置分布查询的sql的唯一标识（namespace.SQLId或mapper接口的全类名.方法名）
- column：设置分步查询的条件

```xml
<resultMap id="empAndDeptByStepResultMap" type="Emp">
    <id property="eid" column="eid"></id>
    <result property="empName" column="emp_name"></result>
    <result property="age" column="age"></result>
    <result property="sex" column="sex"></result>
    <result property="email" column="email"></result>
    <association property="dept"                 select="com.atguigu.mybatis.mapper.DeptMapper.getEmpAndDeptByStepTwo"                 column="did"></association>
</resultMap>
<!--Emp getEmpAndDeptByStepOne(@Param("eid") Integer eid);--><select id="getEmpAndDeptByStepOne" resultMap="empAndDeptByStepResultMap">
    select * from t_emp where eid = #{eid}
</select>
```

1. 查询部门信息

```xml
<!--此处的resultMap仅是处理字段和属性的映射关系--><resultMap id="EmpAndDeptByStepTwoResultMap" type="Dept">
    <id property="did" column="did"></id>
    <result property="deptName" column="dept_name"></result>
</resultMap>
<!--Dept getEmpAndDeptByStepTwo(@Param("did") Integer did);--><select id="getEmpAndDeptByStepTwo" resultMap="EmpAndDeptByStepTwoResultMap">
    select * from t_dept where did = #{did}
</select>
```

## 9.3 一对多映射

```xml
public class Dept {
    private Integer did;
    private String deptName;
    private List<Emp> emps;
    //...构造器、get、set方法等
}
```

### 9.3.1 Collection处理映射

- collection：用来处理一对多的映射关系
- ofType：表示该属性对饮的集合中存储的数据的类型

```xml
<resultMap id="DeptAndEmpResultMap" type="Dept">
    <id property="did" column="did"></id>
    <result property="deptName" column="dept_name"></result>
    <collection property="emps" ofType="Emp">
        <id property="eid" column="eid"></id>
        <result property="empName" column="emp_name"></result>
        <result property="age" column="age"></result>
        <result property="sex" column="sex"></result>
        <result property="email" column="email"></result>
    </collection>
</resultMap>
<!--Dept getDeptAndEmp(@Param("did") Integer did);--><select id="getDeptAndEmp" resultMap="DeptAndEmpResultMap">
    select * from t_dept left join t_emp on t_dept.did = t_emp.did where t_dept.did = #{did}
</select>
```

### 9.3.2 分步查询处理映射

1. 查询部门信息

```xml
<resultMap id="DeptAndEmpByStepOneResultMap" type="Dept">
    <id property="did" column="did"></id>
    <result property="deptName" column="dept_name"></result>
    <collection property="emps"                select="com.atguigu.mybatis.mapper.EmpMapper.getDeptAndEmpByStepTwo"                column="did"></collection>
</resultMap>
<!--Dept getDeptAndEmpByStepOne(@Param("did") Integer did);--><select id="getDeptAndEmpByStepOne" resultMap="DeptAndEmpByStepOneResultMap">
    select * from t_dept where did = #{did}
</select>
```

1. 根据部门 id 查询部门的所有员工

```xml
<!--List<Emp> getDeptAndEmpByStepTwo(@Param("did") Integer did);--><select id="getDeptAndEmpByStepTwo" resultType="Emp">
    select * from t_emp where did = #{did}
</select>
```

## 9.4 延迟加载

- 分步查询的优点：可以实现延迟加载，但是必须在核心配置文件中设置全局配置信息：
    - lazyLoadingEnabled：延迟加载的全局开关。当开启时，所有关联对象都会延迟加载
    - aggressiveLazyLoading：当开启时，任何方法的调用都会加载该对象的所有属性。 否则，每个属性会按需加载
- 此时就可以实现按需加载，获取的数据是什么，就只会执行相应的sql。此时可通过association和collection中的fetchType属性设置当前的分步查询是否使用延迟加载，fetchType=“lazy(延迟加载)|eager(立即加载)”

```xml
<settings>
    <!--开启延迟加载-->    <setting name="lazyLoadingEnabled" value="true"/>
</settings>
```

```xml
@Test
public void getEmpAndDeptByStepOne() {
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
    Emp emp = mapper.getEmpAndDeptByStepOne(1);
    System.out.println(emp.getEmpName());
}
```

- 关闭延迟加载，两条SQL语句都运行了

开启延迟加载，只运行获取emp的SQL语句

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679136579079-169e5589-81d1-4c87-9499-03b0b10def52.png#averageHue=%232d323c&clientId=uaf803194-5e42-4&from=paste&height=128&id=uac44f24b&originHeight=141&originWidth=744&originalType=binary&ratio=1&rotation=0&showTitle=false&size=63855&status=done&style=none&taskId=udff383eb-64b5-44e9-a253-20a5f5d5f17&title=&width=676.3636217038496)

image.png

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679136604929-84b202c5-8971-49ab-beab-d72c2e0e3205.png#averageHue=%232d323b&clientId=uaf803194-5e42-4&from=paste&height=72&id=uaa8037a1&originHeight=79&originWidth=716&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30687&status=done&style=none&taskId=u8995c19f-cbf9-47ed-b781-ede0bab43da&title=&width=650.9090768010166)

image.png

```xml
@Test
public void getEmpAndDeptByStepOne() {
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
    Emp emp = mapper.getEmpAndDeptByStepOne(1);
    System.out.println(emp.getEmpName());
    System.out.println("----------------");
    System.out.println(emp.getDept());
}
```

- 开启后，需要用到查询dept的时候才会调用相应的SQL语句

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679136632990-455aa717-9baf-43f5-adef-c42c5d8089a8.png#averageHue=%232c313b&clientId=uaf803194-5e42-4&from=paste&height=168&id=uc5e68b16&originHeight=185&originWidth=741&originalType=binary&ratio=1&rotation=0&showTitle=false&size=70048&status=done&style=none&taskId=u9c787763-f9cd-4e25-80b2-b19a441edd5&title=&width=673.636349035689)

image.png

- fetchType：当开启了全局的延迟加载之后，可以通过该属性手动控制延迟加载的效果，fetchType=“lazy(延迟加载)|eager(立即加载)”

```xml
<resultMap id="empAndDeptByStepResultMap" type="Emp">
    <id property="eid" column="eid"></id>
    <result property="empName" column="emp_name"></result>
    <result property="age" column="age"></result>
    <result property="sex" column="sex"></result>
    <result property="email" column="email"></result>
    <association property="dept"                 select="com.atguigu.mybatis.mapper.DeptMapper.getEmpAndDeptByStepTwo"                 column="did"                 fetchType="lazy"></association>
</resultMap>
```

# 10. 动态SQL

动态 SQL 是MyBatis 的一个特色，是一种根据特定条件动态拼装 SQL 语句的功能。

`parameterType`

## 10.1 if

根据条件判断子句是否执行。太爽了！ `if` 标签通过 `test` 属性的表达式进行判断，如果表达式为 `true` ，标签内容将被执行，否则不执行。 `1=1`是一个恒等式，不影响结果，有了 `1=1` 可以在 `where` 后面拼接 `and`。

```xml
<!--List<User> selectUsers(@Param("name") String name,@Param("password") String password);-->
<select id="selectUsers" resultType="com.dong.entity.User">
    select * from user where 1=1
    <if test="name!=null and name!=''">
        and name=#{name}
    </if>
    <if test="password!=null and password!=''">
        and password=#{password}
    </if>
</select>
```

如果 name 和 password 表达式都为 false，则两个标签中 SQL 语句都不会执行，将会查询所有记录。

## 10.2 where

- 若 where 标签中的 if 条件都不满足，则 where 标签没有任何功能，即不会添加 where 关键字。
- 若 where 标签中的 if 条件满足，则 where 标签会自动添加 where 关键字，并将条件最前方多余的 and/or 去掉。

效果和上面代码的效果一致。

```xml
<!--List<User> selectUsers(@Param("name") String name,@Param("password") String password);--><select id="selectUsers" resultType="com.dong.entity.User">
    select * from user
    <where>
        <if test="name!=null and name!=''">
            and name=#{name}
        </if>
        <if test="password!=null and password!=''">
            and password=#{password}
        </if>
    </where>
</select>
```

注意：where标签不能去掉条件后多余的and/or。

## 10.3 trim

trim 用于去掉或者添加表标签中的内容。 属性：

- prefix：执行之前添加
- suffix：执行之后添加
- prefixOverrides：去掉最前面的相应内容
- suffixOverrides：去掉最后面的响应内容

效果和上面代码一致。

```xml
<!--List<User> selectUsers(@Param("name") String name,@Param("password") String password);--><select id="selectUsers" resultType="com.dong.entity.User">
    select * from user
    <trim prefix="where" suffixOverrides="and|or" >
        <if test="name!=null and name!=''">
             name=#{name} and
        </if>
        <if test="password!=null and password!=''">
            and password=#{password} or
        </if>
    </trim>
</select>
```

## 10.4 choose、when、otherwise

when 至少要有一个，otherwise 至多只有一个。 相当于 `if` a `else if` b `else if` c `else` d

```xml
<!--List<User> selectUsers(@Param("name") String name,@Param("password") String password);--><select id="selectUsers" resultType="com.dong.entity.User">
    select * from user
        <where>
            <choose>
                <when test="name!=null and name!=''">
                    name=#{name}
                </when>
                <when test="password!=null and password!=''">
                    password=#{password}
                </when>
            </choose>
        </where>
</select>
```

## 10.5 foreach

属性：

- collection：设置要循环的数组或集合
- item：表示集合或数组中的每一个数据
- separator：设置循环体之间的分隔符，分隔符前后默认有一个空格，如`,`
- open：设置 foreach 标签中的内容的开始符
- close：设置 foreach 标签中的内容的结束符

```xml
<!--int deleteFor(@Param("args") Integer[] args);--><delete id="deleteFor">
    delete
    from user
    where id in
    <foreach collection="args" item="arg" separator="," open="(" close=")">
        #{arg}
    </foreach>
</delete>
```

是对Integer数组参数的遍历。相当于：

```sql
delete from user where id in(15,16,17)
```

## 10.6 SQL片段

- `<sql>`标签：声明标签
- `<include>`标签：引入标签

```xml
// 截取sql片段
<sql id="user">name,password</sql>
// 引入sql片段
<!--List<User> selectBytable(@Param("table") String table);--><select id="selectBytable" resultType="com.dong.entity.User">
    select <include refid="user"></sql> from user
</select>
```

# 11. MyBatis的缓存

## 11.1 MyBatis一级缓存

- 一级缓存是SqlSession级别的，通过同一个SqlSession查询的数据会被缓存，下次通过SqlSession查询相同的数据，就会从缓存中直接获取，不会从数据库重新访问
- 条件:同一个SqlSession连续两次查询,并且查询条件相同,第二次查询才会启用缓存,如果两次查询之间使用增,将会清空一级缓存.
- 使一级缓存失效的四种情况：
    1. 不同的SqlSession对应不同的一级缓存
    2. 同一个SqlSession但是查询条件不同
    3. 同一个SqlSession两次查询期间执行了任何一次增删改操作（缓存只是提高查询速度的，不能影响查询结果） 不管有没有影响缓存中的数据，只要增删改，就会把一级缓存清空
    4. 同一个SqlSession两次查询期间手动清空了缓存

## 11.2 MyBatis二级缓存

- 二级缓存是SqlSessionFactory级别，通过同一个SqlSessionFactory创建的SqlSession查询的结果会被缓存；此后若再次执行相同的查询语句，结果就会从缓存中获取
- 二级缓存开启的条件
    1. 在核心配置文件中，设置全局配置属性`cacheEnabled="true"`，默认为true，不需要设置
    2. 在映射文件中设置标签
    3. 二级缓存必须在SqlSession关闭或提交之后有效
    4. 查询的数据所转换的实体类类型必须实现序列化的接口
- 使二级缓存失效的情况：两次查询之间执行了任意的增删改，会使一级和二级缓存同时失效

## 11.3 二级缓存相关配置

- 在mapper配置文件中添加的cache标签可以设置一些属性
- eviction属性：缓存回收策略
    - LRU（Least Recently Used） – 最近最少使用的：移除最长时间不被使用的对象。
    - FIFO（First in First out） – 先进先出：按对象进入缓存的顺序来移除它们。
    - SOFT – 软引用：移除基于垃圾回收器状态和软引用规则的对象。
    - WEAK – 弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象。
    - 默认的是 LRU
- flushInterval属性：刷新间隔，单位毫秒
    - 默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句（增删改）时刷新
- size属性：引用数目，正整数
    - 代表缓存最多可以存储多少个对象，太大容易导致内存溢出
- readOnly属性：只读，true/false
    - true：只读缓存；会给所有调用者返回缓存对象的相同实例。因此这些对象不能被修改。这提供了很重要的性能优势。
    - false：读写缓存；会返回缓存对象的拷贝（通过序列化）。这会慢一些，但是安全，因此默认是false

## 11.4 MyBatis缓存查询的顺序

- 先查询二级缓存，因为二级缓存中可能会有其他程序已经查出来的数据，可以拿来直接使用
- 如果二级缓存没有命中，再查询一级缓存
- 如果一级缓存也没有命中，则查询数据库
- SqlSession关闭之后，一级缓存中的数据会写入二级缓存

## 11.5 整合第三方缓存

### 环境搭建

```xml
<!-- Mybatis EHCache整合包 --><dependency>
    <groupId>org.mybatis.caches</groupId>
    <artifactId>mybatis-ehcache</artifactId>
    <version>1.2.1</version>
</dependency>
<!-- slf4j日志门面的一个具体实现 --><dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.3</version>
</dependency>
```

# 12. MyBatis逆向工程

# 13. 分页插件






# 1. MyBatisPlus简介

MyBatis-Plus（简称 MP）是一个 MyBatis的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679331024168-8766d8f0-92d7-4191-bbce-9d300a2c43d3.png#averageHue=%23cfcac9&clientId=u0fdce6d3-f0ed-4&from=paste&height=303&id=u98d987ed&originHeight=333&originWidth=872&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=96636&status=done&style=none&taskId=ubf5c59a6-c7fb-4107-b6ce-16fdd0f0ea1&title=&width=792.7272555453721)

image.png

## 1.1 特性

- 无侵入：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- 损耗小：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
- 强大的 CRUD 操作：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
- 支持 Lambda 形式调用：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- 支持主键自动生成：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- 支持 ActiveRecord 模式：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- 支持自定义全局通用操作：支持全局通用方法注入（ Write once, use anywhere ）
- 内置代码生成器：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- 内置分页插件：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- 分页插件支持多种数据库：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- 内置性能分析插件：可输出 SQL 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- 内置全局拦截插件：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

## 1.2 框架结构

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679331229876-d8a9828b-fea2-45b6-b3da-b44c555982fb.png#averageHue=%23d5cfab&clientId=u0fdce6d3-f0ed-4&from=paste&height=468&id=ud1623c43&originHeight=568&originWidth=898&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=262085&status=done&style=none&taskId=u50937a18-f996-4c8f-9583-97fd9c9a715&title=&width=739.3635864257812)

image.png

# 2. 入门案例

1. 创建新项目服务器 url可以选择：

- [](https://start.spring.io/)[https://start.spring.io](https://start.spring.io)
- [](https://start.aliyun.com/)[https://start.aliyun.com](https://start.aliyun.com)

SpringBoot 3.x 需要JDK17以上 选择依赖时，只选择 SQL Driver 2. 添加 MyBatisPlus的starter以及连接池依赖```xml <dependency><groupId>com.baomidou</groupId><artifactId>mybatis-plus-boot-starter</artifactId><version>3.4.1</version></dependency><dependency><groupId>com.alibaba</groupId><artifactId>druid</artifactId><version>1.1.16</version></dependency>

```
3. 制作实体类与表结构```sql
CREATE TABLE  mybatisPlus_demo(
            id bigint(20) primary key auto_increment,
            name varchar(32) not null,
            password  varchar(32) not null,
            age int(3) not null ,
            tel varchar(32) not null
);
insert into mybatisPlus_demo values(null,'tom','123456',12,'12345678910');
insert into mybatisPlus_demo values(null,'jack','123456',8,'12345678910');
insert into mybatisPlus_demo values(null,'jerry','123456',15,'12345678910');
insert into mybatisPlus_demo values(null,'tom','123456',9,'12345678910');
insert into mybatisPlus_demo values(null,'snake','123456',28,'12345678910');
insert into mybatisPlus_demo values(null,'张益达','123456',22,'12345678910');
insert into mybatisPlus_demo values(null,'张大炮','123456',16,'12345678910');
```

```java
@Data@AllArgsConstructor@NoArgsConstructor@TableName("mybatisplus_demo")public class User {    // id    private Long id;    // 姓名    private String name;    // 密码    private String password;    // 姓名    private Integer age;    // 电话    private String tel;}
```

因为表名与实体类名不一致，所以使用@TableName指定表名。 4. 配置 jdbc 参数```yaml spring: datasource: type: com.alibaba.druid.pool.DruidDataSource driver-class-name: com.mysql.cj.jdbc.Driver url: jdbc:mysql://localhost:3306/demo?ssl=false&serverTimeZone=UTC username: root password: root 19190046969

```
5. 定义数据层接口，继承BaseMapper```java
@Mapper
public interface UserDao extends BaseMapper<User> {
}
```

泛型为需要操作的实体类。 因为没有设置Mapper接口所在的包，所以需要加注解@Mapper，才能将Mapper接口扫描到。 6. 测试```java @SpringBootTest class MybatisPlusDemoApplicationTests {

```
@Autowired
private UserDao userDao;

/*
    测试目的：UserMapper API
 */
@Test
public void test(){
    userDao.insert(new User(null,"王二","333",33,"1999763552"));
}
```

}

```
# 3. 数据层开发
## 3.1 CURD 操作
![image.png](<https://cdn.nlark.com/yuque/0/2022/png/29046015/1666690803757-3e4a0621-2ca1-485a-8285-fd78c84b8a99.png#averageHue=%23dccccb&clientId=uf1b0ef3a-92aa-4&from=paste&height=562&id=HfavB&originHeight=562&originWidth=1404&originalType=binary&ratio=1&rotation=0&showTitle=false&size=140206&status=done&style=none&taskId=u8aef8cfd-1f75-4d72-922d-3192e426cf1&title=&width=1404>)
## 3.2 Lombok
简化POJO开发，在编译期间动态生成get、set等方法。

- @Getter：生成getter方法
- @Setter：生成setter方法
- @ToString：生成toString方法
- @EqualsAndHashCode：生成equals和hashcode方法
- @Data：包含上面所有功能
- @NoArgsConstructor：生成空参构造
- @AllArgsConstructor：生成全参构造
- @Builder

一般加 @Data 注解就够了，但如果使用全参构造，需要全参无参两个注解一起加。
## 3.3 分页功能
1. 添加分页拦截器```java
@Configuration
public class MybatisPlusConfig {

    /**
     * 分页拦截器
     * @return
     */
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        //1 创建MybatisPlusInterceptor拦截器对象
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        //2 添加分页拦截器
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return interceptor;
    }
}
```

1. 执行分页查询```java /* 测试目的：分页查询 */ @Test public void test2(){ //1 创建IPage分页对象,设置分页参数（当前页码值，每页数据条数） Page<User> page = new Page<>(1, 3); //2 执行分页查询 userDao.selectPage(page,null); //3 获取分页结果 System.out.println(“当前页码值：”+page.getCurrent()); System.out.println(“每页显示数：”+page.getSize()); System.out.println(“总页数：”+page.getPages()); System.out.println(“总条数：”+page.getTotal()); System.out.println(“当前页数据：”+page.getRecords()); }

```
## 3.4 日志打印
1. 开启MybatisPlus的日志功能```yaml
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl #开启日志，输出在控制台
```

1. 简化打印
2. 取消初始化spring日志打印

在resources下新建一个logback.xml文件，名称固定，内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?><configuration>
</configuration>
```

1. 取消SpringBoot的banner图标

```yaml
spring:  main:    banner-mode: off # 关闭SpringBoot启动图标(banner)
```

1. 取消MybatisPlus启动banner图标

```yaml
# mybatis-plus日志控制台输出mybatis-plus:  configuration:    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl  global-config:    banner: off # 关闭mybatisplus启动图标
```

# 4. DQL编程

## 4.1 条件查询

MyBatisPlus 将书写复杂的 SQL 查询条件封装进了 Wrapper，使用编程的形式完成查询条件的组合

### 4.1.1 基本条件

方式一```java @Test public void test(){ QueryWrapper<User> wrapper = new QueryWrapper<>(); [wrapper.lt](http://wrapper.lt)(“age”,18); List<User> users = userDao.selectList(wrapper); users.forEach(System.out::println); }

```
方式二```java
@Test
public void test4(){
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.lambda().lt(User::getAge,18);
    List<User> users = userDao.selectList(wrapper);
    users.forEach(System.out::println);
}
```

方式三```java @Test public void test(){ LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>(); [wrapper.lt](http://wrapper.lt)(User::getAge,18); List<User> users = userDao.selectList(wrapper); users.forEach(System.out::println); }

```
### 4.1.2 组合条件
与```java
@Test
public void test6(){
    LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
    //并且关系：10到30岁之间
    wrapper.lt(User::getAge,30).gt(User::getAge,18);
    List<User> users = userDao.selectList(wrapper);
    users.forEach(System.out::println);
}
```

或```java @Test public void test(){ LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>(); //或者关系：小于10岁或者大于30岁 [wrapper.lt](http://wrapper.lt)(User::getAge,10).or().gt(User::getAge,30); List<User> users = userDao.selectList(wrapper); users.forEach(System.out::println); }

```
### 4.1.3 NULL值处理
在一些搜索场景下，多条件查询中，有些筛选条件的值可能为空。
if语句```java
@Test
public void test8(){
    Integer minAge = 10;//将来有用户传递进来,此处简化成直接定义变量了
    Integer maxAge = null;//将来有用户传递进来,此处简化成直接定义变量了
    LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
    if (minAge != null)
        wrapper.gt(User::getAge,minAge);
    if (maxAge != null)
        wrapper.lt(User::getAge,maxAge);
    List<User> users = userDao.selectList(wrapper);
    users.forEach(System.out::println);
}
```

条件参数控制```java @Test public void test9(){ Integer minAge = 10;//将来有用户传递进来,此处简化成直接定义变量了 Integer maxAge = null;//将来有用户传递进来,此处简化成直接定义变量了 LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>(); //参数1：如果表达式为true，那么查询才使用该条件 [wrapper.gt](http://wrapper.gt)(minAge!=null,User::getAge,minAge); [wrapper.lt](http://wrapper.lt)(maxAge!=null,User::getAge,maxAge); List<User> users = userDao.selectList(wrapper); users.forEach(System.out::println); }

```
如果参数一表达式为true，那么查询才使用该条件
链式编程```java
@Test
public void test10(){
    Integer minAge = 10;//将来有用户传递进来,此处简化成直接定义变量了
    Integer maxAge = null;//将来有用户传递进来,此处简化成直接定义变量了
    LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
    //参数1：如果表达式为true，那么查询才使用该条件
    wrapper.gt(minAge!=null,User::getAge,minAge)
            .lt(maxAge!=null,User::getAge,maxAge);
    List<User> users = userDao.selectList(wrapper);
    users.forEach(System.out::println);
}
```

## 4.2 投影查询

让结果集仅包含指定列，称为投影查询。 查询结果包含实体类的部分属性```java @Test public void test11(){ LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>(); // password、tel将会是null wrapper.select(User::getId,User::getAge,User::getName); List<User> users = userDao.selectList(wrapper); users.forEach(System.out::println); }

```
查询结构包含实体类未定义属性```java
@Test
public void test12(){
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.select("count(*) as count,tel");
    wrapper.groupBy("tel");
    List<Map<String, Object>> maps = userDao.selectMaps(wrapper);
    maps.forEach(System.out::println);
}
```

结果：

```java
{count=7, tel=12345678910}{count=1, tel=1999763552}
```

## 4.3 查询条件

- 范围匹配（> 、 = 、between）
- 模糊匹配（like）
- 空判定（null）
- 包含性匹配（in）
- 分组（group）
- 排序（order）
- …… eq匹配```java LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>(); //等同于= lqw.eq(User::getName, “Jerry”).eq(User::getPassword, “jerry”); User loginUser = userDao.selectOne(lqw); System.out.println(loginUser);

```
le ge匹配 或 between匹配```java
LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
//范围查询 lt le gt ge eq between
lqw.between(User::getAge, 10, 30);
List<User> userList = userDao.selectList(lqw);
System.out.println(userList);
```

like匹配```java LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>(); //模糊匹配 like lqw.likeLeft(User::getName, “J”); List<User> userList = userDao.selectList(lqw); System.out.println(userList);

```
分组查询聚合函数```java
QueryWrapper<User> qw = new QueryWrapper<User>();
qw.select("gender","count(*) as nums");
qw.groupBy("gender");
List<Map<String, Object>> maps = userDao.selectMaps(qw);
System.out.println(maps);
```

更多条件查看官方文档：[条件构造器 | MyBatis-Plus](https://baomidou.com/pages/10c804/#abstractwrapper)

# 5. 映射

1. 字段映射表中字段与实体类属性名不一致。 在实体类属性名上方使用 @TableField 注解的 value 属性映射字段名和实体类属性名。
    
    ![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666690914956-2e4e8a37-4c60-454b-a1fe-a25ab146da79.png#averageHue=%23f9f8e4&clientId=uf1b0ef3a-92aa-4&from=paste&height=325&id=HiEI7&originHeight=325&originWidth=1169&originalType=binary&ratio=1&rotation=0&showTitle=false&size=44868&status=done&style=none&taskId=u39e2f608-7d10-4a70-af72-94ad674a319&title=&width=1169)
    
    image.png
    
2. 表中未定义字段实体类中有的属性，在表中并没有定义对应字段。 在实体类属性名上方使用 @TableField 注解的 exist 属性设置实体类属性是否存在表中字段对应。
    
    ![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666690926165-8cc6a41f-2194-4a33-ab2b-68b366fc1ee9.png#averageHue=%23fcfbe0&clientId=uf1b0ef3a-92aa-4&from=paste&height=387&id=vmcTZ&originHeight=387&originWidth=1151&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36423&status=done&style=none&taskId=u41e1f5ba-e60a-4dfe-bdc7-cc68444f7d1&title=&width=1151)
    
    image.png
    
3. 字段查询权限实体类中的一些字段可能不希望参与查询，比如密码。 在实体类属性名上方使用 @TableField 注解的 select 属性设置该实体类属性对应的表中字段是否参与查询。
    
    ![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666690939425-ea2b1985-8811-47af-8aec-26af69c636f1.png#averageHue=%23f9f8e4&clientId=uf1b0ef3a-92aa-4&from=paste&height=362&id=jdQHr&originHeight=453&originWidth=1143&originalType=binary&ratio=1&rotation=0&showTitle=false&size=42631&status=done&style=none&taskId=u7534ca49-1ba9-41a7-8538-a5423dd5d5c&title=&width=913.6676025390625)
    
    image.png
    
4. 表名与实体类名不一致在实体类上方使用 @TableName 注解的 value 属性设置该实体类对应的表名。
    
    ![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666690951737-61b74aac-b185-4e79-ad91-3ed44dc2f872.png#averageHue=%23faf9e6&clientId=uf1b0ef3a-92aa-4&from=paste&height=415&id=Q2qAb&originHeight=415&originWidth=1221&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47419&status=done&style=none&taskId=u84a77731-0202-44b1-b3b0-286b38b3e8e&title=&width=1221)
    
    image.png
    

# 6. DML编程

## 6.1 主键生成策略

不同的表应用不同的 id 生成策略，比如日志只需要主键自增即可，但是订单表就要使用某种特殊规则。 使用 @TableId 注解加在表中主键对应实体类的属性上，使用它的 type 属性选择主键生成策略。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666690968067-d13efc97-e09a-43a3-b5a3-335ca9672a7e.png#averageHue=%23f0efef&clientId=uf1b0ef3a-92aa-4&from=paste&height=215&id=a0ytc&originHeight=215&originWidth=1118&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41759&status=done&style=none&taskId=ub25f15f0-820c-4ba6-bb0d-d1c87166e52&title=&width=1118)

image.png

## 6.2 全局配置策略

```yaml
mybatis-plus:  global-config:    db-config:      id-type: assign_id      table-prefix: tbl_
```

主键生成策略将全部使用雪花算法，表名将全部是 tb_ 加上首字母小写的类名。

## 6.3 批处理操作

根据主键批处理删除```java //删除指定多条数据 List<Long> list = new ArrayList<>(); list.add(1402551342481838081L); list.add(1402553134049501186L); list.add(1402553619611430913L);

userDao.deleteBatchIds(list);

```
根据主键批处理查询```java
//查询指定多条数据
List<Long> list = new ArrayList<>();
list.add(1L);
list.add(3L);
list.add(4L);
userDao.selectBatchIds(list);
```

## 6.4 逻辑删除

在实际开发中删除一条数据不是真的删除，一旦删除业务数据将会丢失。 所以一般是设置一个表示是否可用的状态字段，执行删除时，设置状态字段为不可用状态，数据保留在数据库中。

1. 数据库中添加逻辑删除标记字段
    
    ![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666691029183-798879df-20b2-4962-a080-ab11c56737dd.png#averageHue=%23f9f8f7&clientId=uf1b0ef3a-92aa-4&from=paste&height=209&id=MzIxf&originHeight=209&originWidth=758&originalType=binary&ratio=1&rotation=0&showTitle=false&size=39662&status=done&style=none&taskId=u44b6bb43-2e54-489a-ae1e-06541641e4d&title=&width=758)
    
    image.png
    
2. 实体类中添加对应属性，并使用 @TableLogic 注解设置属性为逻辑删除标记字段```java @Data public class User { private Long id; //逻辑删除字段，标记当前记录是否被删除 @TableLogic private Integer deleted;
    

}

```
3. 配置逻辑删除字面量```yaml
mybatis-plus:
  global-config:
    db-config:
      table-prefix: tbl_
      # 逻辑删除字段名
      logic-delete-field: deleted
      # 逻辑删除字面值：未删除为0
      logic-not-delete-value: 0
      # 逻辑删除字面值：删除为1
      logic-delete-value: 1
```

## 6.5 乐观锁

乐观锁顾名思义就是在操作时很乐观，认为操作不会产生并发问题(不会有其他线程对数据进行修改)，因此不会上锁。 但是在更新时会判断其他线程在这之前有没有对数据进行修改。一旦修改导致更新失败，事务回滚。 乐观锁的实现机制有：版本号机制和CAS算法，MybatisPlus中使用的是版本号机制。

- 取出记录时，获取当前version
- 更新时，带上这个version
- 执行更新时， set version = newVersion where version = oldVersion
- 如果version不对，就更新失败

1. 数据库表中添加乐观锁标记字段
    
    ![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666691063527-960820fc-380c-41c7-9256-0606c49ad753.png#averageHue=%23f9f9f8&clientId=uf1b0ef3a-92aa-4&from=paste&height=222&id=luHK8&originHeight=222&originWidth=746&originalType=binary&ratio=1&rotation=0&showTitle=false&size=42617&status=done&style=none&taskId=u99c0e632-ad6b-4a0f-974d-656d5c1bcf8&title=&width=746)
    
    image.png
    
2. 实体类中添加对应属性，并使用 @Version 注解设置属性为乐观锁标记字段```java @Data public class User { private Long id; @Version private Integer version; }
    

```
3. 配置乐观锁拦截器，实现SQL的拼装（version的更新和比较是否相等）```java
@Configuration
public class MpConfig {
    @Bean
    public MybatisPlusInterceptor mpInterceptor() {
        //1.定义Mp拦截器
        MybatisPlusInterceptor mpInterceptor = new MybatisPlusInterceptor();

        //2.添加乐观锁拦截器
        mpInterceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());

        return mpInterceptor;
    }
}
```

1. 修改前必须知道对应数据的version才可以正常进行```java @Test public void testUpdate() { /_User user = new User(); user.setId(3L); user.setName(“Jock666”); user.setVersion(1); userDao.updateById(user);_/ [//1.先通过要修改的数据id将当前数据查询出来](//1.xn--id-613c74cszaho557cfhgfkkfa651c4saga031e22b246i8pzdu7i2ssoqb) //User user = userDao.selectById(3L); [//2.将要修改的属性逐一设置进去](//2.xn--4gqv0cp9et6kvmar1qt8j559a50qx6wjufclouta) [//user.setName](//user.setName)(“Jock888”); [//userDao.updateById](//userDao.updateById)(user); [//1.先通过要修改的数据id将当前数据查询出来](//1.xn--id-613c74cszaho557cfhgfkkfa651c4saga031e22b246i8pzdu7i2ssoqb) User user = userDao.selectById(3L); //version=3 User user2 = userDao.selectById(3L); //version=3 user2.setName(“Jock aaa”); userDao.updateById(user2); //version=>4 user.setName(“Jock bbb”); userDao.updateById(user); //verion=3?条件还成立吗？ }

```
![image.png](<https://cdn.nlark.com/yuque/0/2022/png/29046015/1666691079268-74c37d59-869c-47e1-8d98-bd59463ebd5b.png#averageHue=%23efeeee&clientId=uf1b0ef3a-92aa-4&from=paste&height=212&id=y3FQu&originHeight=212&originWidth=1191&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30811&status=done&style=none&taskId=u54bb165f-1f55-4620-a8ee-6515777279a&title=&width=1191>)
# 7.代码生成器
mybatis-plus-generator 3.5.1 代码生成器更新，不兼容历史版本
AutoGenerator 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、Mapper、Mapper XML、Service、Controller 等各个模块的代码，极大的提升了开发效率。
## 7.1 代码生成器（旧）
> 适用版本：mybatis-plus-generator 3.5.1 以下版本

1. 添加依赖```xml
<!--代码生成器-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.4.1</version>
</dependency>

<!--velocity模板引擎-->
<dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity-engine-core</artifactId>
    <version>2.3</version>
</dependency>
```

1. 编写代码生成器类```java public class Generator { public static void main(String[] args) { //1. 创建代码生成器对象，执行生成代码操作 AutoGenerator autoGenerator = new AutoGenerator();
    
    ```
     //2. 数据源相关配置：读取数据库中的信息，根据数据库表结构生成代码
     DataSourceConfig dataSource = new DataSourceConfig();
     dataSource.setDriverName("com.mysql.cj.jdbc.Driver");
     dataSource.setUrl("jdbc:mysql://localhost:3306/mybatisplus_db?serverTimezone=UTC");
     dataSource.setUsername("root");
     dataSource.setPassword("root");
     autoGenerator.setDataSource(dataSource);
    
      //3. 执行生成操作
     autoGenerator.execute();
    ```
    
    } }
    

````
3. 自定义设置全局配置
```java
//设置全局配置
GlobalConfig globalConfig = new GlobalConfig();
globalConfig.setOutputDir(System.getProperty("user.dir")+"/mybatisplus_04_generator/src/main/java");    //设置代码生成位置
globalConfig.setOpen(false);    //设置生成完毕后是否打开生成代码所在的目录
globalConfig.setAuthor("黑马程序员");    //设置作者
globalConfig.setFileOverride(true);     //设置是否覆盖原始生成的文件
globalConfig.setMapperName("%sDao");    //设置数据层接口名，%s为占位符，指代模块名称
globalConfig.setIdType(IdType.ASSIGN_ID);   //设置Id生成策略
autoGenerator.setGlobalConfig(globalConfig);
````

包相关配置

```java
//设置包名相关配置PackageConfig packageInfo = new PackageConfig();packageInfo.setParent("com.aaa");   //设置生成的包名，与代码所在位置不冲突，二者叠加组成完整路径packageInfo.setEntity("domain");    //设置实体类包名packageInfo.setMapper("dao");   //设置数据层包名autoGenerator.setPackageInfo(packageInfo);
```

策略相关配置

```java
//策略设置StrategyConfig strategyConfig = new StrategyConfig();strategyConfig.setInclude("tbl_user");  //设置当前参与生成的表名，参数为可变参数strategyConfig.setTablePrefix("tbl_");  //设置数据库表的前缀名称，模块名 = 数据库表名 - 前缀名  例如： User = tbl_user - tbl_strategyConfig.setRestControllerStyle(true);    //设置是否启用Rest风格strategyConfig.setVersionFieldName("version");  //设置乐观锁字段名strategyConfig.setLogicDeleteFieldName("deleted");  //设置逻辑删除字段名strategyConfig.setEntityLombokModel(true);  //设置是否启用lombokautoGenerator.setStrategy(strategyConfig);
```

更多配置查看官方文档：[代码生成器配置旧 | MyBatis-Plus](https://baomidou.com/pages/061573/#%E5%9F%BA%E6%9C%AC%E9%85%8D%E7%BD%AE)

## 7.2 代码生成器（新）

> 适用版本：mybatis-plus-generator 3.5.1 及以上版本