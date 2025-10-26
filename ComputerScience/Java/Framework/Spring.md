![[Spring.png]]
# 1. Spring 概述

Spring 是一个开源的、轻量级的 JavaEE 开发框架。

Spring 整合了众多优秀的设计模式，提供了一系列底层容器和基础设施，可以和大量常用开发框架无缝集成。

通常所说的 Spring 指的是 SpringFramework，它是 Spring 家族最为底层也是最为重要框架。

**Spring4.x**

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678970467036-79098586-551c-4625-9307-4810ef8260a7.png)

**Spring5.x**

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678970485025-8aacc5ff-0bdd-4c78-8e87-18b8ac2ed296.png)

# 2. IOC

  

## 2.1 IOC 核心概念

IOC（Inversion of Control）控制反转

以前在实例化对象时，是通过 new 关键字创建的对象，如果是在多态情况下，更换不同实现类时，就需要重新编码、编译、部署。

而原因就在于实例化对象是硬编码在了程序中。

IOC 的思想就在于，将对象创建的控制权交由程序外部，我们不管这个对象是怎么创建的，我们只需要拿到这个对象用就行，如此实现解耦。

Spring 中管理对象的容器称为 IOC 容器，被管理的对象称为 Bean。

## 2.2 IOC入门

以用户登录为例：控制层接收请求-->调用业务方法处理-->调用持久层对数据库进行读写。

1. 导入 Spring 坐标

```
<dependencies>
    <!--导入spring的坐标spring-context，对应版本是5.2.10.RELEASE-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.2.10.RELEASE</version>
    </dependency>
</dependencies>
```

2. 定义 Spring 管理的类或者接口

```
// 接口
public interface UserService {
    public void login(String name,String password);
}
// 实现类
public class UserServiceImpl implements UserService {
    private UserDao userDao = new UserDaoImpl();
    @Override
    public void login(String name, String password) {
        userDao.select(name,password);
    }
}
```

```
// 接口
public interface UserDao {
    public void select(String name,String password);
}
// 实现类
public class UserDaoImpl implements UserDao {
    @Override
    public void select(String name, String password) {
        System.out.println("开始查询用户");
    }
}
```

3. 创建 Spring 配置文件，定义类

右键项目-->新建-->XML配置文件-->Spring配置。

在 beans 标签中，添加 bean 标签，定义类。

```
<bean id="userService" class="com.dong.service.impl.UserServiceImpl"/>
```

4. 初始化 IOC 容器，管理类

```
@Test
public void test(){
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("application.xml");
    UserService userService = context.getBean("userService", UserService.class);
    userService.login("张三","123");
}
```

可以发现程序中并没有创建 UserService 对象，我们只是从 IOC 容器中拿到的。

## 2.3 IOC 底层原理

工厂设计模式+反射+配置文件

## 2.4 IOC 容器

Spring 的核心 API 就是 `ApplicationContext`，称为 `ApplicationContext工厂`或者`IOC容器`，用于`对象的创建`。但是它是`接口`，主要实现类是：

- `ClassPathXmlApplicationContext`（**非Web环境,XML配置文件**）
- `AnnotationApplicationContext`(**非Web环境配置类**)
- `XmlWebApplicationContext`（**Web环境,XML配置文件**）
- `WebApplicationContext`(**Web环境配置类**)

使用接口的好处就是可以**屏蔽实现上的差异**。

需要注意的是：ApplicationContext 是一个`重量级资源`，ApplicationContext 工厂的对象会占用`大量内存`，`所以一个应用只会创建一个工厂对象`。

只能创建一个对象，也就是说可能会存在`并发访问`的情况，所以 ApplicationContext 工厂一定是`线程安全`的。

IOC 容器的顶级接口是 BeanFactory，只提供了最基本的功能，ApplicationContext 对 BeanFactory 进行了拓展，ApplicationContext 是BeanFactory 的完整**超集**

# 3. DI

## 3.1 DI 核心概念

DI（Dependency Injection）依赖注入。

有了 IOC 容器之后，我们再进行编码的时候，需要从 IOC 容器中拿到对应对象即可。如此也为 DI 打下了基础。

例如：A 依赖于 B，我们不再需要在 A 中 new 一个B，只需要在 A 中定义B作为属性，然后由 IOC 容器创建 B对象，并将 B 对象赋值给 A 中的 B 属性即可。

## 3.1 DI 入门

还是以用户登录为例，在 Service 层中依赖 Dao 层，我们是通过 new 的方式创建的 Dao 对象。

我们也不需要在 Service 层中，创建 IOC 容器对象，获得 Dao 对象，再赋值给 Dao 属性，因为 IOC 容器一般一个项目只会创建一个，是由 Spring 进行管理，而我们创建的对象一般来说也是单例对象，在初始化 IOC 容器时就会将对象全部创建。

我们只需要在 Spring 配置文件中定义好，哪个类依赖哪个类，这样 IOC 容器就会自动将被依赖的类创建，通过 Set 或者构造器将该对象注入给依赖类。

在 SpringBoot 中，我们甚至不需要去配置被依赖的到底是哪个类，SpringBoot 会进行扫描，按照类型或者名称，在 IOC 容器中找到那个类，然后通过反射，将该对象赋值给依赖类的属性。

1. 在 UserServiceImpl 中取消通过 new 创建 UserDao

```
public class UserServiceImpl implements UserService {
    private UserDao userDao;

    @Override
    public void login(String name, String password) {
        userDao.select(name,password);
    }
}
```

2. 提供 UserDao 的 Setter 方法

```
public class UserServiceImpl implements UserService {
    private UserDao userDao;

    @Override
    public void login(String name, String password) {
        userDao.select(name,password);
    }

    public void setUserDao(UserDaoImpl userDao) {
        this.userDao = userDao;
    }
}
```

3. 在 Spring 配置文件中定义依赖关系

```
<bean id="userDao" class="com.dong.dao.impl.UserDaoImpl"/>
<bean id="userService" class="com.dong.service.impl.UserServiceImpl">
    <property name="userDao" ref="userDao"/>
</bean>
```

property 标签的 name 属性值为依赖类中定义的名称。

## Set 注入

**Spring 底层通过 Set 方法为成员赋值的方式称为 Set 注入。**

针对不同类型的成员变量，在property 标签中，需要嵌套其它标签。

JDK内置类型

**String+8种基本类型**

```
<value>xxx</value>
```

```
<bean id="user" class="com.dong.entity.User">
    <property name="age">
        <value>20</value>
    </property>
    <property name="name">
        <value>John</value>
    </property>
</bean>
```

**数组和list集合**

```
<property name="list">
    <list>
        <value>123@</value>
        <value>456@</value>
        <value>789@</value>
    </list>
</property>
```

**Set集合**

**注意Set集合的泛型, 根据泛型的不同, Set标签内部嵌套不同标签**

```
<!--泛型为String-->
<property name="set">
    <set>
        <value>111</value>
        <value>222</value>
        <value>333</value>
    </set>
</property>
```

**Map集合**

**一个entry标签代表一个键值对, key有key标签, value没有特殊标签, 因为key-value都是字符串, 所以使用value标签**

```
<property name="map">
    <map>
        <entry>
            <key><value>qqq</value></key>
            <value>www</value>
        </entry>
        <entry>
            <key><value>eeee</value></key>
            <value>dddd</value>
        </entry>
    </map>
</property>
```

**Properties**

**key-value都是字符串, 一个prop标签代表一个键值对**

```
<property name="pro">
    <props>
        <prop key="key1">value1</prop>
        <prop key="key2">value2</prop>
    </props>
</property>
```

引用类型

**第一种方式**

1. 为成员变量提供get set方法
2. 配置文件中进行注入(赋值)

```
<bean id="userService" class="com.dong.UserServiceImpl">
    <property name="userDao">
        <bean class="com.dong.UserDaoImpl"/>
    </property>
</bean>
```

**第二种方式**

第一种方式存在的问题

- 配置文件代码冗余
- 被注入的对象,多次创建,浪费内存(JVM)资源

1. 为成员变量提供get set方法
2. 配置文件进行注入

```
<bean id="userDao" class="com.dong.UserDaoImpl"/>
<bean id="userService" class="com.dong.UserServiceImpl">
    <property name="userDao">
      <ref bean="userDao"/>  
    </property>
</bean>
```

set注入的简化

**基于属性简化**

```
JDK内置类型
<property name="username">
    <value>777</value>
</property>
<property name="password">
    <value>777</value>
</property>

简化:
<property name="username" value="777"/>
<property name="password" value="777"/>

自定义类型
<property name="userDao">
  <ref bean="userDao"/>
</property>

简化:
<property name="userDao" ref="userDao"/>
```

**基于p命名空间简化**

在beans标签引入命名空间：

```
xmlns:p="http://www.springframework.org/schema/p"
```

```
JDK内置类型
<bean id="user" class="com.dong.User">
    <property name="username" value="777"/>
    <property name="password" value="777"/>
</bean>

简化:
<bean id="user" class="com.dong.User" p:username="777" p:password="777"/>

自定义类型
<bean id="userService" class="com.dong.UserServiceImpl">
    <property name="userDao" ref="userDao"/>
</bean>

简化:
<bean id="userService" class="com.dong.UserServiceImpl" p:userDao-ref="userDao"/>
```

## 构造注入

**Spring 底层调用构造方法，通过配置文件，为成员变量赋值。**

开发步骤

1. 提供有参构造方法

```
public class Customer {
    private String name;
    private int age;
    public Customer(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

2. Spring的配置文件的配置  
    **一个constructor-arg标签代表一个参数**

```
<bean id="customer" class = "com.dong.constructer.Customer">
    <constructor-arg>
        <value>张三</value>
    </constructor-arg>
    <constructor-arg>
        <value>111</value>
    </constructor-arg>
</bean>
```

构造方法的重载

**参数个数不同时**

**通过控制 constructor-arg 标签的数量，来指定调用，不同参数个数的构造方法。**

**参数个数相同时**

**通过在标签引入 type 属性，进行类型的区分 。**

# 4. Bean的配置

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679025124873-5d1f8c73-85c7-44b0-b0b0-efd73aa7470c.png)

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679025152819-6ab48e1d-3e88-4f1e-b80d-f66558b1be34.png)

# 5. Bean的实例化

Bean 的实例化共有四种方法，构造方法用于创建简单对象，也是最常用的，剩下的三种用于创建复杂对象，也就是不能直接通过 new 创建的对象。

## 5.1 构造方法

一般我们创建的 Bean 都是通过构造方法进行实例化，这类的特点就是可以直接通过 new 创建对象。

## 5.2 FactoryBean接口

1. 实现 FactoryBean 接口，重写方法。

```
//FactoryBean接口实现的方法

public Object getObject(){
    //用于书写创建复杂对象的代码,并把复杂对象作为返回值返回
}
public Class getObjectType(){
    //返回所创建的复杂对象的Class对象
}
public Boolean isSingleton(){
    //创建一次 return true
    //每次调用都要创建 return false
}
```

2. 配置文件配置

```
<bean id="conn" class="com.dong.FactoryBean.ConnectionFactoryBean"/>
```

IOC 容器中获取实现 FactoryBean 接口的类的对象，是 getObject 方法中返回的对象。

isSingleto 方法中如果对象能被共用，就创建一次，返回 true，不能共用则返回 false，创建多次。

FactoryBean 是 Spring 原生提供，Spring 整合其他框架时，大量使用了 FactoryBean。

## 5.3 实例工厂

为了避免 Spring 的侵入，也因为历史遗留问题可能在开发过程中已经有了获取复杂对象的方法。

在 Spring 配置文件中配置，让 Spring 通过实例工厂获取复杂对象。

```
<!--com.dong.factorybean.ConnectionFactory是已经有的创建复杂对象的类，getConnection是获取Connection对象的方法-->
<bean id="connFactory" class="com.dong.factorybean.ConnectionFactory"/>
<bean id="conn" factory-bean="connFactory" factory-method="getConnection"/>
```

## 5.4 静态工厂

静态工厂和实例工厂的唯一区别在于获取复杂对象的方法是静态的。

因为是静态的，所有 Spring 不需要创建对象，直接通过类名调用静态方法。

```
<bean id="conn" class="com.dong.factorybean.StaticConnectionFactory" factory-method="getConnection"/>
```

# 6.Bean的生命周期

对象的创建到销毁过程，现在使用 Spring 对 Bean 进行管理，所以对象的生命周期由 Spring 控制，可能在创建对象或者销毁对象的同时，需要对一些资源进行管理分配，Spring 提供方法供我们使用。

## 6.1 创建阶段

通过使用 scope 属性对 Bean 的创建时机进行控制。

- `singleton`

Spring 创建容器的同时，创建对象。

- `prototype`

IOC 容器在获取对象时，创建对象。

如果 scope 设置为 singleto，但想在获取对象的同时，创建对象可以使用懒加载，在 bean 标签中设置 lazy-int为true。

## 6.2 初始化阶段

Spring 工厂在创建完对象后，调用对象的初始化方法，完成初始化操作。

实现 `InitializingBean` 接口，重写方法

```
//将初始化需求写在方法里面,完成初始化操作
public void afterPropertiesSet(){}
```

在对象中提供一个普通的初始化方法

```
//名字随便起
public void myInit(){}
//配置文件中进行配置
<bean id="product" class="xxx" init-method="myInit"/>
```

## 6.3 销毁阶段

Spring 销毁对象之前，会调用对象的销毁方法，完成销毁操作。Spring 会在关闭 IOC 容器时，销毁对象。

# 自定义类型转换器

## 类型转换器

作用：Spring 通过类型转换器把配置文件中**字符串类型**的数据，转换成对象中成员变量**对应类型**的数据，从而完成注入。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667785733875-79964744-757a-4af2-8a7c-2e227bf7c60e.png?x-oss-process=image%2Fresize%2Cw_1410%2Climit_0)

## 自定义类型转换器

当 Spring 内部没有提供特定的类型转换器时，而程序员在应用过程中还需要使用，那么就需要程序员自己定义类型转换器。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667785745507-367121ce-33ac-436c-88a8-bae5ef91f9ac.png?x-oss-process=image%2Fresize%2Cw_1407%2Climit_0)

1. 实现 `Converter<s,t>` 接口

```
public class MyDateConverter implements Converter<String, Date> {

    /*
        convert:将String类型转换为Date
        param:source 代表配置文件中的日期字符串
        return:Date 返回Date类型,让Spring拿到转换后的数据
     */
    @Override
    public Date convert(String source) {
        Date date = null;
        try {
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
            date = sdf.parse(source);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return date;
    }
}
```

2. 在 Spring 配置文件中进行配置

- 将 MyDateConverter 对象创建出来

```
<bean id="myDateConverter" class="com.dong.converter.MyDateConverter"/>
```

- 类型转换器的注册  
    目的：告知 Spring 框架，MyDateConverter 是一个类型转换器(将自定义类型转换器注册进类型转换器集合converts中)。

```
<!--将MyDateConverter注入conversionService-->
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <ref bean="myDateConverter"/>
        </set>
    </property>
</bean>
```

MyDateConverter 中的日期格式, 可以通过依赖注入的方式, 由配置文件完成赋值

```
public class MyDateConverter implements Converter<String, Date> {
    private String pattern;

    public String getPattern() {
        return pattern;
    }

    public void setPattern(String pattern) {
        this.pattern = pattern;
    }

    /*
            convert:将String类型转换为Date
            param:source 代表配置文件中的日期字符串
            return:Date 返回Date类型,让Spring拿到转换后的数据
         */
    @Override
    public Date convert(String source) {
        Date date = null;
        try {
            SimpleDateFormat sdf = new SimpleDateFormat(pattern);
            date = sdf.parse(source);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return date;
    }
}
```

- `ConversionServiceFactoryBean` 定义的 id 属性值必须是 `**conversionService**`
- Spring 框架内置了日期类型装换器, 但格式有要求  
    日期格式: 2022/04/16 (不支持: 2022-4-16)。

# 后置处理器

通过 BeanPostProcessor 接口中提供的方法，对 Spring 工厂所创建的对象，进行再加工。

没有加工时：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667785762988-583d5b93-7ade-4197-a8a4-18d22be0ee30.png)

加工后：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667785777102-caac8366-e948-495c-abf5-518ab6b24895.png?x-oss-process=image%2Fresize%2Cw_1421%2Climit_0)

实现 `BeanPostProcessor` 接口，重写规定的方法(注入之后,初始化之前)：

- `Object postProcessBeforeInitialization(Object bean,String beanName)`  
    作用: Spring创建完对象，并进行注入后，可以运行 `Before` 方法进行加工。通过方法参数获得 Spring 创建好的对象；通过返回值将加工好的对象交给 Spring 工厂。
- `Object postProcessAfterInitialization(Object bean,String beanName)`  
    作用: Spring执行完对象的初始化操作后，可以运行 `After` 方法进行加工。通过方法参数获得 Spring 创建好的对象；通过返回值将加工好的对象交给 Spring 工厂。

实际开发中，很少处理 Spring 的初始化操作，这种情况下，没必要区 分Before、After，只需实现 `After` 即可。

`Before` 方法可以什么都不用写，但`必须将 bean 返回`，才能把对象传递给 `After` 方法。

1. 用一个类实现 `BeanPostProcessor` 接口

```
public class MyBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        /*
        if (bean instanceof Category category) {
             category.setName("SWJ");
         }
        */
        if(bean instanceof Category){
            Category category = (Category) bean;
            category.setName("SWJ");
        }        
        return bean;
    }
}
```

2. 在 Spring 配置文中进行配置

```
<bean id="myBeanPostProcessor" class="com.dong.beanpost.MyBeanPostProcessor"/>
```

**细节**：

`BeanPostProcessor` 会对 Spring 工厂中`所有`创建的对象进行加工。

# 第三方资源配置管理

## DataSource

使用 druid 做为 DataSource 的实现。

1. 添加 druid 依赖

```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.11</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.25</version>
</dependency>
```

2. Spring 配置文件中配置 DataSource 对象

```
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/demo"/>
    <property name="username" value="root"/>
    <property name="password" value="root"/>
</bean>
```

## Properties

在 Spring 配置文件中可能有一些需要经常修改的信息，这时我们可以将这些信息抽取出来，放入到一个小配置文件中。

以上面的数据库连接池对象为例，将参数抽取出来。

1. 编写小配置文件

```
jdbc.driver = com.mysql.cj.jdbc.Driver
jdbc.url = jdbc:mysql://localhost:3306/demo
jdbc.username = root
jdbc.password = root
```

2. Spring 配置文件与小配置文件整合

```
<!--引入小配置文件-->
<context:property-placeholder location="classpath:/db.properties"/>
<!--需要添加命名空间-->
xmlns:context="http://www.springframework.org/schema/context"
<!--在xsi:schemaLocation属性中,添加解析方法-->
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
```

3. 在Spring配置文件中通过${key}获取小配置文件的值

```
<bean class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="${jdbc.driver}"/>
    <property name="url" value="${jdbc.url}"/>
    <property name="username" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
</bean>
```

注意：

在小配置文件中如果数据库用户名的 key 使用 username，那么获取到的值不是 root，而是计算机的名称。

所以要么不使用 username 作为 key，要么在context标签中添加 `system-properties-mode="NEVER"`，表示不加载系统属性。

有多个配置文件时，location 属性值用逗号分隔，并且支持通配符。

# AOP 编程

## 静态代理模式

代理模式是为了实现一个类中核心功能和额外功能的解耦，或者为了实现在不改变原有设计的基础上进行功能增强。

代理模式是实现 AOP 编程的基础。  
**代理类 = 目标类 + 额外功能 + 与目标类实现相同的接口(方法一致)**

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667785849535-0acd7757-3664-48d4-9e50-130f9606817e.png)

**工厂获取代理类的Bean，代理类与原始类实现相同接口，实现相同方法，代理类就可以将额外功能书写在自己的实现方法中，然后再调用原始类的该方法，就可以做到将额外功能与核心功能分离。**

但静态代理也存在问题：

- **有一个原始类就要有一个代理类**，导致静态代理类文件过多，不利于项目管理。
- 维护性差，当给多个原始类添加相同的额外功能的时候，如果这个额外功能有变化，就需要更改所有的代理类。

## 动态代理开发

所需依赖：

```
<!--spring核心依赖，会将spring-aop传递进来-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.2.10.RELEASE</version>
</dependency>
<!--切入点表达式依赖，目的是找到切入点方法，也就是找到要增强的方法-->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```

1. 创建原始对象

```
public class UserServiceImpl implements UserService{
    @Override
    public void register(User user) {
        System.out.println("UserServiceImpl.register");
    }
    @Override
    public boolean login(String username, String password) {
        System.out.println("UserServiceImpl.login");
        return true;
    }
}
```

2. 定义额外功能

创建类实现 `MethodBeforeAdvice` 接口。额外功能书写在接口的实现中，在原始方法`执行之前`运行额外功能。

```
public class Before implements MethodBeforeAdvice {
    @Override
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println("-----log:MethodBeforeAdvice-------");
    }
}
```

3. 定义切入点

**切入点**: 额外功能加入的位置(哪个方法)。  
需要加入额外功能的方法称为切入点，可以加入额外功能的方法称为连接点，连接点是切入点的完整超集。  
**目的**: 由程序员根据自己需要，决定额外功能加入那个原始方法。

```
<aop:config><!--AOP配置-->
  <!--为所有的方法加上额外功能,切入点为所有方法-->
  <aop:pointcut id="bef" expression="execution(* *(..))"/>
</aop:config>
```

4. 组装切面

整合二三步。

```
<aop:config>
    <!--为所有的方法加上额外功能,切入点为所有方法-->
    <aop:pointcut id="bef" expression="execution(* *(..))"/>
    <!--将额外功能和切入点进行组装-->
    <aop:advisor advice-ref="before" pointcut-ref="bef"/>
</aop:config>
```

5. 调用

**目的**: 获得 Spring 工厂创建的动态代理对象，并进行调用。  
**注意**：

1. Spring 工厂通过原始对象的 id 值获得的是代理对象。  
    `context.getBean("userService")`
2. 获得代理对象后，通过声明接口类型，进行存储(**代理对象和原始对象实现相同接口**)。  
    `UserService userService = context.getBean("userService")`

Spring 框架在运行时，通过动态字节码技术，在 JVM 中创建动态代理类，运行在 JVM 内部。等程序结束后，会和 JVM 一起消失，所以不会和静态代理一样，类文件过多，影响项目管理。

动态字节码技术是通过第三方字节码框架，在JVM中创建对应类的字节码，进而创建对象，当虚拟机结束，动态字节码跟着消失。

在额外功能不改变的前提下，创建其他原始类的代理对象时，只要通过切入点表达式加入新原始类的新方法即可。

在程序开发中遵循一个原则: `打开拓展,关闭修改` 。当我们对额外功能不满意时，我们不必去修改额外功能代码，只需新建一个额外功能，然后在配置文件中，修改标签即可。

**以所有类作为模板，以这些类中方法作为切入点（锚点），面向这些方法进行编程，灵活的选择这些方法（切入点）并且灵活的进行额外功能的添加与更换。**

## 动态代理详解

### 额外功能详解

- **MethodBeforeAdvice分析**

对于before方法虽然方法参数我们并没有使用，但是应当知道参数代表着什么：

- **method**：增加额外功能的那个原始方法。
- **args**：增加额外功能的那个原始方法的参数。
- **target**：增加额外功能的原始方法所在的原始对象。

这些参数根据需要选用。

这个类也就是使用注解开发AOP时的@Before注解的底层实现。

- **MethodInterceptor(方法拦截器)**

除了实现 `MethodBeforeAdvice` 接口外，还可以实现 `MethodInterceptor` 接口。

这两者的区别：

- `MethodBeforeAdvice`：只能用于原始方式执行**之前**添加额外功能。
- `MethodInterceptor`：可以在原始方法运行**之前、之后、抛出异常**时，都可以添加额外功能。

这个类也是大部分AOP注解开发中使用的注解的底层实现，如@Around  
**MethodInterceptor 影响原始方法的返回值**：  
原始方法的返回值`(invocation.proceed())`，直接作为 `invoke` 方法的返回值返回，即 `MethodInterceptor` 不会影响原始方法的返回值。`invoke` 方法中，如果对方法返回值进行修改，将直接导致原始方法的返回值发生改变。

### 切入点讲解

切入点决定额外功能的加入位置（哪些方法需要加额外功能）

- `execution()`：切入点函数
- `* *(..)`：切入点表达式

```
<aop:pointcut id="before" expression="execution(* *(..))"/>
```

切入点表达式

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667785919341-3dad1af8-a58c-42f9-83d4-a34cb9e3c557.png)

- `*`：通配符，必须有一个
- `..`：任意
- `+`：专用匹配子类类型`* *..dong.Service+.*`

### 方法切入点表达式

- 定义 login 方法作为切入点

```
* login(..)
```

- 定义 login 方法, 且 login 方法有两个字符串类型的参数, 作为切入点

```
* login(String,String)
```

- 定义 login 方法, 且login方法有一个 User 类型的参数, 作为切入点

```
* login(com.dong.entity.User)
```

- 定义 login 方法, 且 login 方法至少有一个String类型的参数, 作为切入点

```
* login(String,..)
```

只写方法名这种的切入点表达式不精准，会受到**不同类中具有相同方法名**的方法的干扰

精准的切入点表达式应当具有完整的包名、类名、方法名

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667785933532-b1315de6-7b3f-453d-8dd7-49c95cbadbcd.png)

### 类切入点表达式

指定类作为额外功能的切入点，这个类中**所有方法**都会添加额外功能。

- 语法一

```
* com.baizhiedu.a.UserServiceImpl.*(..)
// UserServiceImpl类中所有方法加入额外功能
```

- 语法二

```
// 1. 类只存在一级包中 com.UserServiceImpl
* *.UserServiceImpl.*(..)

// 2. 类在多级包 com.baizhiedu.proxy.UserServiceImpl
* *..UserServiceImpl.*(..)
// 忽略包,给不同包中的同名类中的所有方法添加额外功能
```

### 包切入点表达式

指定包作为额外功能的切入点，**包中所有类所的有方法**，都将加入额外功能。

- 语法一

```
* com.baizhiedu.proxy.*.*(..)
// 注意: 切入点包中的所有类,必须在proxy包中,不能在proxy包的子包中
```

- 语法二

```
* com.baizhiedu.proxy..*.*(..)
// 让当前包的子包生效
```

切入点函数

用于执行切入点表达式，有四种常用切入点函数

1. execution

```
最为重要的切入点函数,功能最全
可以执行 方法切入点表达式、类切入点表达式、包切入点函数

弊端: execution执行切入点表达式,书写麻烦

注意: 其他切入点函数 简化的是execution书写复杂度,功能一致
```

2. args

```
作用: 主要用于方法参数的匹配

切入点: 必须是两个字符串类型的参数

execution(* *(String,String))
args(String,String)
```

3. within

```
作用: 主要用于类\包切入点表达式的匹配

切入点: UserServiceImpl这个类

execution(* *..UserServiceImpl.*(..))
within(*..UserServiceImpl)

切入点: com.baizhiedu.proxy这个包

execution(* com.baizhiedu.proxy..*.*)
within(com.baizhiedu.proxy..)
```

4. @annotation

```
作用: 为具有特殊注解的方法加入额外功能

切入点: 加有com.baizhiedu.Log这个注解的方法

@annotation(com.baizhiedu.Log)
```

切入点函数的逻辑操作

切入点函数的逻辑运算，指的是 **整合多个切入点函数**一起配合工作,进而完成更为复杂的需求：

- and与操作

```
案例: 方法名为login,同时有两个字符串参数

1. execution(* login(String,String))

2. execution(* login(..)) and args(String,String)

注意: 与操作不能用于同种类型的切入点函数
```

- or或操作

```
案例: register方法和login方法作为切入点

execution(* register(..)) or execution(* login(..))
```

## AOP编程

`AOP(Aspect Oriented Programing)`  
本质就是 Spring 的动态代理开发，通过代理类为原始类增加额外功能。  
以切面为基本单位的程序开发，通过切面间的相互协同，相互调用，完成程序的构建。

### 基于注解的AOP编程

XML开发 AOP 的步骤：

1. 原始对象
2. 额外功能
3. 切入点
4. 组装切面

现在将 2、3、4 步放入切面类中。

- 定义切面类：@Aspect
- 定义额外功能：@Around 修饰的方法
- 原始方法的运行：ProceedingJoinPoint.proceed()
- 定义切入点：@Around(* login(..))

```
package com.dong.aspect;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;

@Aspect
public class MyAspect {
    @Around("execution(* login(..))")
    public Object around(ProceedingJoinPoint point) throws Throwable {

        System.out.println("----Aspect log--------");
        Object ret = point.proceed();

        return ret;
    }
}
```

配置文件定义：

```
<!--原始对象-->
<bean id="userService" class="com.dong.aspect.UserServiceImpl"/>
<!--
    切面
        1.额外功能
        2.切入点
        3.组装切面
-->
<!--切面类-->
<bean id="around" class="com.dong.aspect.MyAspect"/>
<!--告知 Spring 基于注解进行AOP编程-->
<aop:aspectj-autoproxy/>
```

切入点的复用：

```
//在切面类中定义一个函数,加上注解@Pointcut 将切入点函数提取出来,利于切入点的复用

@Aspect
public class MyAspect {

    @Pointcut("execution(* login(..))")
    public void myPointcut() {
    }

    @Around(value = "myPointcut()")
    public Object around(ProceedingJoinPoint point) throws Throwable {
        System.out.println("----Aspect log--------");
        Object ret = point.proceed();

        return ret;
    }

    public Object around2(ProceedingJoinPoint point) throws Throwable {
        System.out.println("----new Aspect log------");
        Object ret = point.proceed();
        
        return ret;
    }
}
```

动态代理的切换方式：

```
AOP的底层实现 动态代理的两种创建方式
1. JDK
2. CGlib

默认情况下,AOP编程,底层应用JDK动态代理创建方式
如果切换CGlib
		1. 基于注解的AOP开发
			<aop:aspectj-autoproxy proxy-target-class="true"/>
			false是默认情况,使用JDK创建代理对象
		2. 传统的AOP开发
			<aop:config proxy-target-class="true">
```

### 纯注解的AOP编程

基于注解开发AOP的回顾：

1. 原始类
2. 切面类配置（额外功能、定义切入点、组装切面）
3. Spring配置文件的配置

```
// 1. 原始类
public class UserServiceImpl implements UserService{
  
}

// 2. 切面类
@Aspect
public class MyAspect {

  @Pointcut("execution(* com.dong.service.UserService.login(..))")
  public void pointcut1(){}

  @Around("pointcut1()")
  public Object around(ProceedingJoinPoint point) throws Throwable {
    
    // 额外功能
    Object ret = point.proceed();// 原始方法运行
	// 额外功能
    return ret;
  }
}

// 3.配置文件的配置
<!--原始对象-->
<bean id="userService" class="com.dong.aspect.UserServiceImpl"/>
<!--
    切面
        1.额外功能
        2.切入点
        3.组装切面
-->
<bean id="myAspect" class="com.dong.aspect.MyAspect"/>
<!--告知Spring基于注解进行AOP编程-->
<aop:aspectj-autoproxy/>
```

我们使用纯注解的AOP开发，就是为了替换Spring的配置文件，所以只需要把配置文件中的配置转移出来，即可完成AOP的纯注解开发

- 使用@Component注解以及衍生注解将原始类和切面类注册进Spring工厂。
- 在配置类中，设置注解扫描，设置`@EableAspectJAutoProxy`注解，替换`<aop:aspectj-autoproxy />`标签

```
// 1. 原始类
@Service
public class USerServiceImpl implements UserService{
}

// 2.切面类
@Component
@Aspect
public class MyAspect {

  @Pointcut("execution(* com.dong.service.UserService.login(..))")
  public void pointcut1(){}

  @Around("pointcut1()")
  public Object around(ProceedingJoinPoint point) throws Throwable {
    
    // 额外功能
    Object ret = point.proceed();// 原始方法运行
		// 额外功能
    return ret;
  }
}

// 3.配置Bean配置
@Configuration
@ComponentScan("com.dong")
@EableAspectJAutoProxy
public class AopConfig{
}
```

代理方式的切换

```
@EnableAspectJAutoProxy(proxyTargetClass = false) // 默认是false，使用JDK代理
```

SpringBoot中AOP开发  
在SpringBoot中@EnableAspectJAutoProxy不用我们去显示的调用，Spring会帮我们实现。Spring中默认代理使用JDK，SpringBoot中默认代理使用Cglib  
额外功能也称之为通知，额外功能所在的类也称之为通知类，根据**额外功能所添加的位置**，而划分为不同的通知类型，不同类型可以使用不同的注解

- 前置：@Before
- 后置：@After
- 环绕：@Around
- 后置：@AfterReturning
- 抛出异常时：@AfterThrowing

获取原始方法的信息  
可以在切面中获取原始方法的返回值、参数等信息。可以对这些数据进行操作。

- 获取原始方法参数(适用于于@Before、@After、[@Around)](/Around\))

```
@Before("point2()")
public void before(JoinPoint point){
    Object[] args = point.getArgs();
    System.out.println("MyAspect.before");
}
```

- 获取原始方法返回值（适用于@Around、@AfterReturning）

```
@AfterReturning(value = "point1()",returning = "obj")
public void afterReturning(JoinPoint point,Object obj){
    System.out.println(obj);
}
```

- 获取原始方法抛出异常对象（适用于@Around、@AfterThrowing）

```
@AfterThrowing(value = "point4()",throwing = "t")
public void afterThrowing(Throwable t){
    System.out.println(t);
}
```

### AOP开发过程中的坑

```
//在同一个业务类中,一个业务方法调用另一个业务方法
//问题: login方法添加有额外功能
//	    register方法没有添加额外功能
public class UserServiceImpl implements UserService {
    @Override
    public void login(String username, String password) {
        System.out.println("UserServiceImpl.login");
        this.register(new User());
    }

    @Override
    public void register(User user) {
        System.out.println("UserServiceImpl.register");
    }
}
```

```
调用login方法的,是代理对象,而调用register方法的,是原始对象,所以没有加上额外功能

能不能在login方法中再获取一个代理对象,从而使代理对象调用register方法呢?

不能,我们知道Spring工厂是一个重量级资源,一个应用应当创建一个Spring工厂
```

```
使业务类再实现ApplicationContextAware接口
实现接口的setApplicationContext方法,这个方法的作用是获取创建的Spring工厂对象
```

```
public class UserServiceImpl implements UserService, ApplicationContextAware {
    private ApplicationContext context;
    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.context = applicationContext;
    }

    @Override
    public void login(String username, String password) {
        System.out.println("UserServiceImpl.login");
        UserService userService = (UserService) context.getBean("userService");
        userService.register(new User());
    }

    @Override
    public void register(User user) {
        System.out.println("UserServiceImpl.register");
    }
}
```

```
IVoucherOrderService proxy = (IVoucherOrderService) AopContext.currentProxy();
            return proxy.createVoucherOrder(voucherId);
```

# AOP底层原理

1. AOP 如何创建动态代理类
2. Spring 工厂如何加工创建代理对象（通过原始对象的id值,获得的是代理对象）

## JDK动态代理

通过方法 `Proxy.newProxyInstance(ClassLoader,interfaces,InvocationHandler)` 创建代理对象，并返回代理对象。

- `InvocationHandler`

```
将额外功能写在InvocationHandler接口的invoke方法中,额外功能可以执行在原始方法执行之前\之后\前后\抛出异常

Object invoke(Object proxy,Method method,Object[] args)

返回值: 原始方法的返回值

参数: proxy  -->代表代理对象,将Proxy.newProxyInstance创建好的代理对象传入invoke方法,现在基本不用,可以忽略
     method  -->额外功能所增加给的那个原始方法
     args  -->原始方法的参数
     
原始方法的运行: method.invoke(userService,args),将运行结果作为原始方法的返回值返回
```

- `interfaces`

```
原始对象实现的接口
作用: 代理类和原始类实现相同接口

userService.getClass().getInterfaces()
```

- `ClassLoader`

```
类加载器的作用: 

1. 通过类加载器将对应类的字节码文件加载进JVM
2. 通过类加载器创建类的Class对象,进而创建这个类的对象

过程: 编写User.java文件,编译为User.class文件,由类加载器将User.class文件加载进JVM,再由类加载器创建类的Class对象,才能创建这个类的对象

获取类加载器: JVM为每一个类的.class文件,自动分配与之对应的ClassLoader

在动态代理类的开发中,需要动态代理类,才能创建代理对象,但是,动态代理类是由动态字节码技术,也就是Proxy.newProxyInstance(interfaces,InvocationHandler),将创建的字节码直接写入JVM,没有.java文件,也没有.class文件,所以JVM不能给动态代理类分配ClassLoader,没有ClassLoader就无法创建类的Class对象,也就没办法创建类的对象,所以Proxy.newProxyInstance方法需要第三个参数,那就是类加载器

解决办法: 借用一个ClassLoader,可以是任何一个类类加载器
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667785974057-ef058828-bef9-4332-ae1a-9d8ccc06a231.png)

```
public class TestJDKProxy {
    public static void main(String[] args) {
        //1.创建原始对象
        UserService userService = new UserServiceImpl();
        //2.JDK创建动态代理
        //以内部类的方式实现InvocationHandler接口
        InvocationHandler handler = new InvocationHandler() {

            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                //额外方法
                System.out.println("----proxy log------");

                //原始方法运行
                Object ret = method.invoke(userService, args);
                return ret;
            }
        };
        UserService userServiceProxy = (UserService) Proxy.newProxyInstance(TestJDKProxy.class.getClassLoader(), userService.getClass().getInterfaces(), handler);
        userServiceProxy.register(new User());
        userServiceProxy.login("rad","123");
    }
}
```

## CGlib动态代理

CGlib创建动态代理的原理: 父子继承关系创建代理对象，原始类作为父类，代理类作为子类，这样既可以保证两者方法一致，同时也可以在代理类中提供新的实现。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667785987685-32b491c4-eb3c-4811-9ab9-33f936c727bf.png)

```
public class TestCGlib {
    public static void main(String[] args) {
        //1.创建原始对象
        UserService userService = new UserService();
        /*
            2.通过CGlib的方式创建动态代理对象
            和JDK创建动态代理高度一致,将实现接口变成继承父类
            Proxy.newProxyInstance(ClassLoader,interfaces,InvocationHandler)

            Enhancer.setClassLoader()
            Enhancer.setSuperClass()
            Enhancer.setCallback()  -->需要实现MethodInterceptor -->实现intercept方法(和invoke方法一致)
            enhancer.create()  --> 创建代理对象

         */
        Enhancer enhancer = new Enhancer();
        enhancer.setClassLoader(TestCGlib.class.getClassLoader());
        enhancer.setSuperclass(userService.getClass());

        MethodInterceptor interceptor = new MethodInterceptor(){

            @Override
            public Object intercept(Object o, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
                System.out.println("----log----");
                Object ret = method.invoke(userService,args);
                return ret;
            }
        };

        enhancer.setCallback(interceptor);
        UserService serviceProxy = (UserService) enhancer.create();
        serviceProxy.login("args","333");
        serviceProxy.register(new User());
    }
}
```

## 总结

1. JDK 创建代理对象 `Proxy.newProxyInstance` 通过接口实现来创建代理类
2. CGlib 创建动态代理 `enhancer` 通过继承父类来创建代理类

Spring工厂如何加工原始对象成为代理对象？

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786015335-32fa27bf-0d6b-4c03-b686-d7922a6df245.png)

```
public class ProxyBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {

        InvocationHandler handler = new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

                System.out.println("-----new log-----");
                Object ret = method.invoke(bean, args);
                return ret;
            }
        };
        return Proxy.newProxyInstance(ProxyBeanPostProcessor.class.getClassLoader(),bean.getClass().getInterfaces(),handler);
    }
}
```

```
<bean id="proxyBeanPostProcessor" class="com.dong.factory.ProxyBeanPostProcessor"/>
```

# 注解开发

## 为什么使用注解

- 替换XML这种配置形式，简化配置。
- 替换接口，实现调用双方的**契约性**。

什么是调用双方的契约性？  
功能调用者要调用功能提供者的m1方法，但是功能调用者又怎么知道功能提供者的m1方法呢？所以需要给两者立一个契约。最常使用的就是利用接口保证双方契约性。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786287441-7c3f000a-1b0a-4798-b6d0-2390e85c5ecc.png)

功能提供者实现接口，重写方法，在方法里面书写功能。而功能调用者知道有这么个接口，他只需要利用接口调用接口中的方法即可，如此便可以实现调用双方的契约性。  
![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786298442-924605fc-07d4-47b2-9069-d9626ef1898e.png)

比如tomcat和程序员通过Servlet接口达成契约关系。但是在Servlet中最重要的是service方法，其他方法我们不太关注，但实现Servlet的时候，我们还是不得不重写所有方法，于是我们使用HttpServlet充当适配器，通过继承HttpServlet我们只需要重写有关业务逻辑的方法即可。

但是使用适配器，还是比较繁琐，比较生硬。于是可以使用注解，将注解加在需要调用的方法上，功能调用者反射扫描注解，获得方法信息，从而实现方法的调用。这样我们不必实现特定的接口，就能把功能提供者的方法精准的交由功能调用者调用。实现调用双方的契约性。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786322870-47f5ad87-87a2-4d1e-a3f7-b60754b30155.png)

在前面的开发过程中，其实我们已经使用过这种方式。  
![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786336446-13148070-a6dc-43ef-b7d9-357910cd9af9.png)  
总结：通过注解的方式，在功能调用者和功能提供者之间达成约定，进而进行功能的调用。因为注解应用更为方便灵活，所以在现在的开发中，更推荐通过注解的形式实现调用双方的契约性。

## 基础注解（2.x）

这个阶段的注解，仅仅是简化了XML的配置，并不能完全替代XML。

使用基础注解开发，创建对象放入 IOC 容器将摆脱 Spring 配置文件的 bean 标签，在配置文件中加入注解扫描即可。

### 创建对象相关注解

1. 在 Spring 配置文件中开启注解包扫描

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
	 <!--扫描com.dong包及其子包下的类中注解-->
    <context:component-scan base-package="com.dong"/>
</beans>
```

需要设置命名空间及xsi。

2. 在类上使用 @Component 注解定义 Bean

```
@Component("bookDao")
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ...");
    }
}
```

可以使用 @Component 注解的属性值定义该类在 IOC 容器中的 id 值，如果不设置，默认使用首字母小写的类名作为 id 值。

**@Component注解**

- 替换 Spring 原有配置文件中的 bean 标签，创建对象使用。
- 原有 bean 标签中的 calss 属性，@Component 通过反射获取
- 原有 bean 标签的 id 属性，@Component 默认使用类名的首单词首字母小写形式。
- 可以使用属性值显示指定对象在 IOC 容器中的 id 值

**@Component衍生注解**

- @Repository：DAO 层
- @Service：Service 层
- @Controller：Controller 层

作用与 @Component 注解一致，都被 @Component 注解修饰，只是符合分层思想。

**@Scope注解**

- 控制简单对象的创建次数
- @Scope("singleton | prototype")
- 如果不设置 @Scope 注解属性值，默认为 singleton（单实例）。

**@Lazy注解**

- 懒加载
- 添加 @lazy 注解表示将创建单实例对象的时机延迟到获取对象的时候来创建。

**@PostConstruct 和 @PreDestroy**

- 生命周期相关注解
- @PostConstruct 初始化注解
- @PreDestroy 销毁注解
- JDK 中提供的注解，JDK9 移除了相关包，要使用需要引入额外依赖 `javax.annotation-api`

### 注入相关注解

用户自定义类型

1. @Autowired

```
@Service
public class UserService {
    private UserDAO userDAO;

    public UserDAO getUserDAO() {
        return userDAO;
    }

    @Autowired
    public void setUserDAO(UserDAO userDAO) {
        this.userDAO = userDAO;
    }

    public void register(User user) {
        userDAO.save(user);
    }
}
```

- @Autowired 基于类型注入，注入的对象与成员变量类型相同（子类或者实现类）。
- @Autowired 放置在成员变量的 set 方法上，Spring 调用该 set 方法为成员变量赋值。
- @Autowired 放置在成员变量上，Spring 通过反射直接为成员变量赋值。

2. @Autowired + @Qualifier

- 基于名字注入
- 需要注入的对象类 id 值必须和 @Qualifier 注解设置的值一致。
- @Qualifier 注解无法单独使用，必须配合 @Autowired 注解使用。

3. @Resource

- JavaEE规范提供。
- 等同于 @Autowired + @Qualifier
- 如果使用 @Resource 注解没有设置 name 属性值，名字匹配失败，也可以通过类型匹配，进行注入

4. @Inject

- JavaEE规范提供。
- 与 @Autowired 注解完全一致。主要应用于 EJB3.0
- 需要引入依赖 `javax.inject`

JDK类型

使用步骤：

1. 设置xx.properties配置文件
2. Spring工厂读取这个配置文件 `<context:property-placeholder location=""/>`
3. 成员变量上设置注解`@Value("${key}")`读取配置文件的值

注意：

- 使用注解 `@PropertySource` 替换 Spring 配置文件中的 `<context:property-placeholder>`标签。在注解中添加 location 属性值。该注解添加在需要注入的类上。
- @PropertySource() 中加载多文件时使用数组格式配置，不允许使用通配符。
- @Value注解不能加载静态成员变量上。会导致注入失败。
- @Value+properties这种方式，不能注入集合类型（多个数值），Spring在后续中推荐使用 YAML 配置形式，可以配置更为复杂的数据类型。

### 注解扫描详解

Spring 为我们提供了两种方式可以让我们更加方便、灵活的进行包扫描。

排除方式

表示排除我们所选的包或者类，不进行扫描

```
<context:component-scan base-package="com.dong">
    <context:exclude-filter type="assignable" expression="com.dong.bean.User"/>
</context:component-scan>
```

type：

- _assignable_：排除描述特定的类型： `<context:exclude-filter type="assignable" expression="com.dong.bean.User"/>`
- _annotation_：排除特定的注解：`<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Service"/>`
- _aspectj_：排除切入点表达式表示的包或者类：``<context:exclude-filter type="aspectj" expression="com.dong.bean.."/>`
- _regex_：使用正则表达式进行排除。
- _custom_：自定义排除策略，用于底层框架开发。

注意：排除策略可以叠加使用。

包含方式

表示只对我们所选包或类进行扫描

```
<context:component-scan base-package="com.dong" use-default-filters="false">
    <context:include-filter type="assignable" expression="com.dong.entity.User"/>
</context:component-scan>
```

与排除方式的区别：

1. `use-default-filters="false"` 表示使Spring默认的扫描方式失效。
2. 使用`<context:include-filter`标签。
3. 语义和排除方式相反，表示只扫描我们所选的包或者类。

### 注解开发的思考

- 注解和配置文件是互通的。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786497666-207721a6-209c-40cd-b585-f7a8630c9c6b.png)

- 基础注解（Spring2.x）适用于程序员开发类型的配置。对于不是我们自定义的类，比如：SqlSessionFactoryBean、MapperScannerConfigure，在现价段，我们只能使用配置文件进行配置。

## 高级注解（3.x）

这个阶段 Spring 引入了 配置类，使 Spring 完全可以脱离 Xml 配置文件。

从此 Spring 的配置文件将不再只是 XML 配置文件，还有配置类。

### 配置类

使用 @Configuration 注解定义当前类为配置类，配置类和XML配置文件作用一致。

- 创建工厂

new AnnotationConfigApplicationContext()。

- 配置类的加载

- 指定配置类的class对象

AnnotationConfigApplicationContext("AppConfig.class")

- 配置配置类所在的路径

AnnotationConfigApplicationContext("com.dong.bean")

- @Configuration本质

本质也是@Component的衍生注解，可以被扫描到。

### 管理第三方Bean

Spring2.x 时我们只能将自己写的类从 Spring 配置文件中脱离出来，对于第三方Bean还是要在配置文件中进行定义，而在Spring3.x中，第三方Bean同样可以脱离配置文件，使用 @Bean 标签进行定义。

```
public class JdbcConfig {
    //@Bean：表示当前方法的返回值是一个bean对象，添加到IOC容器中
    @Bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://localhost:3306/spring_db");
        ds.setUsername("root");
        ds.setPassword("root");
        return ds;
    }
}
```

- 只要是被@Component注解修饰过的注解里面都可以定义@Bean，并且能加入Spring容器中。
- 如果已经有了创建复杂对象的方法，@Bean方法里面可以直接调用方法，将复杂对象加入 Spring 容器中。
- 可以通过@Bean的属性值，自定义对象在Spring容器中id值。
- 在@Bean修饰的方法上加上 @Scope 控制对象创建次数。

### 为第三方Bean注入依赖

引用类型依赖注入

通过方法的形参体现出用户自定义类型的注入，容器会根据类型自动装配对象。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786577409-ca207ba4-5df6-48e7-8215-f518e8d6e9df.png)

JDK内置类型

使用前面的配置文件转义参数的方法，将参数值转移到配置文件中，然后再通过@Value注解将参数值读入。

```
@Configuration
    @PropertySource("/person.properties")
    public class AppConfig {
        @Value("${name}")
        private String name;
        @Value("${gender}")
        private String gender;
        ...
        @Bean
        public Person person3(){
            Person person = new Person();
            person.setName(name);
            person.setGender(gender);
            return person;
        }    
    }
```

### @ComponentScan注解

@ComponentScan注解在配置类中使用，等同于Xml配置文件中`<context:component-scan>`标签。

排除方式

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786624789-bcbdfbc8-c652-46cd-8086-c3b740c165b6.png?x-oss-process=image%2Fresize%2Cw_1374%2Climit_0)

同样使用注解也可以使用排除策略的叠加

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786636736-c8561cb2-d621-40b3-95e2-2d1232d1b868.png?x-oss-process=image%2Fresize%2Cw_1366%2Climit_0)

需要注意的是type为 aspectj 或者 regex 时使用pattern，其他类型使用value。

包含方式

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786647144-5166cc41-b8ad-4fa5-a68f-f3aba60b363d.png?x-oss-process=image%2Fresize%2Cw_1338%2Climit_0)

### 配置优先级

配置一个类的方式是多种多样的，而这些配置方式有一个优先级，在 id 相同的情况下，优先级高的可以覆盖优先级低的。

@Component及其衍生注解 < @Bean < 配置文件bean标签

使用注解开发，实现了耦合，而利用配置的优先级可以起到一个解耦的作用：先使用低优先级的配置，如果不满意了，可以高优先级的配置进行覆盖，而不用去修改代码。

当然可能原本的配置类中并没有引入配置文件，这时候我们可以创建一个新的配置类，在配置Bean中引入配置文件，并在获取Spring工厂对象时，让其加载多个配置类即可。

### 整合多个配置信息

拆分多个配置bean的开发，是一种模块化开发的形式，也体现了面向对象的设计思想。

多个配置类的整合

有两种整合方式：

- 基于base-package进行多个配置类的整合

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786693986-00efe88c-6a2d-4e84-8033-34e1c7e2e9ac.png?x-oss-process=image%2Fresize%2Cw_1339%2Climit_0)

或者创建工厂时，指定多个配置类的 class 信息

 ```
 ApplicationContext context = new AnnotationConfigApplicationContext(Config1.class,Config2.class,Config3.class);
```

- 基于@Import注解进行多个配置类的整合

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786713069-6f34a1e3-1543-4499-a607-a019fd6cf66f.png?x-oss-process=image%2Fresize%2Cw_1342%2Climit_0)

跨配置进行注入

在应用配置类的过程中，不管使 用那种方式进行配置信息的汇总，其操作方式都是通过成员变量加@Autowired注解完成的。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786726394-eca708fc-1a12-4c31-aa3f-a2d320cf8234.png)

配置类和@Component相关注解的整合

在配置Bean中使用注解@ComponentScan扫描需要整合的类所在的包。

如果需要进行注入，可以使用上面的方法，将需要注入的类作为成员变量，其上加上@Autowired注解，并且在@Bean中，使用set方法进行注入。

```
@Repository
public class PersonDAO {
    public void save(Person person){
        System.out.println("PersonDAO.save");
    }
}

@Configuration
@ComponentScan("com.dong.config")
public class Config6 {
    @Bean
    public PersonService personService(){
        return new PersonService();
    }
}
```

配置类和配置文件的整合

要想配置类整合配置文件，我们只需在配置类中使用@ImportResource注解引入配置文件即可。

如果需要进行注入，还是之前的方式，提供成员变量，在上面添加注解@Autowired，在Bean中使用set方式进注入。

```
@Configuration
@ImportResource("/applicationContext.xml")
public class Config6 {
    @Bean
    public PersonService personService(){
        return new PersonService();
    }
}
```

### 四维一体的开发方式

Spring开发一个功能的四种形式，虽然开发方式不同，但最终效果一致。

1. 基于schema

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786780315-5059b729-12b8-4221-ab94-b69764aa0bc1.png)

2. 基于特定功能注解

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786791731-36e4d323-2e21-4f91-b877-989f8ea58e4b.png)

3. 基于原始bean标签

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786803001-651fd5ba-1513-47d9-acdc-ab3392d79552.png)

4. 基于@Bean注解

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786812864-3788b67b-60ee-4ec7-a8b4-5fa12a7005de.png)

### 配置类的底层实现原理

在使用@Bean 注解时，每次获取对象就会调用@Bean修饰的方法，但事实上只会创建一个对象。

所以是Spring控制了对象的创建次数。创建对象是核心功能，控制对象的创建次数是额外功能，那底层是调用JDK代理还是CGlib呢，我们知道使用JDK代理需要代理类和原始类实现相同的接口，明显不是，所以Spring底层是使用CGlib来进行代理开发。

### 注解开发总结

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679222439175-454ba09b-8854-4ea3-93d6-8f33b12e3c9e.png)

### Spring中使用YAML

YML（YAML）是一种新形势的配置文件，比XML更简单，比properties更强大。

1. properties表达过于繁琐，无法表达数据的内在联系
2. properties无法表达对象、集合类型

yaml语法简介：

```
1.定义yaml文件
	xxx.yml  xxx.yaml
2.语法
	1.基本语法
		namE: 张三
		password: 123
	2.对象
		cat:
			id: 1
			namE: 小白
	3.List集合
		list:
			- 111
			- 222
```

Spring默认不支持YAML，所以需要我们自己配置。

在使用properties配置的时候，读取配置文件以及输出数据靠的是PropertySourcePlaceholderConfigurer对象，是将properties文件中数据读入properties集合集合中，只要我们可以将YAML配置文件的数据读入properties集合，就可以使用PropertySourcePlaceholderConfigurer进行注入。这个中间对象就是YamlPropertiesFactoryBean。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786854526-58600c35-bf21-4a1d-8603-afdfd7c07084.png?x-oss-process=image%2Fresize%2Cw_1408%2Climit_0)

所需依赖：

```
<dependency>
    <groupId>org.yaml</groupId>
    <artifactId>snakeyaml</artifactId>
    <version>1.30</version>
</dependency>
```

最低版本1.18

1. 准备YAML文件
2. 在配置Bean中，完成YAML的读取与PropertySourcePlaceholderConfigurer的创建
3. 在特定类上加上@Value注解

我们使用YAML是通过YamlPropertiesFactoryBean进行中转实现的。

但是需要注意的是YamlPropertiesFactoryBean没办法解析集合类型的

只能使用SpringEL表达式解决

```
@Value("#{'${list}'.split(',')}")
```

而且我们在使用时，通过@Value注解注解也比较繁琐

所以在SpringBoot中，SpringBoot为我们提供了专门的注解@ConfigurationProperties进行对YAML文件的解析。

# Spring整合第三方技术

## 整合 MyBatis

### 整合思路

Spring 整合 MyBatis 主要是对调用 MyBatis API 时的四行代码进行封装。

```
@Test
public void test10() throws IOException {
    InputStream input = Resources.getResourceAsStream("mybatis-config.xml");
    SqlSessionFactory build = new SqlSessionFactoryBuilder().build(input);
    
    SqlSession sqlSession = build.openSession(true);
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    
    mapper.insertUser("陀罗","123");
}
```

`SqlSessionFactoryBean` 封装前两行代码，将 SqlSessionFactory 注入容器。

`MapperScannerConfigurer` 封装后两行代码，将 Mapper 接口的代理对象注入容器。

再将配置文件的内容注入相应类中，从而摆脱掉 MyBatis 的配置文件。

因为设置了 `basepackage` 属性，确定了 Mapper 接口的位置，所以并不需要使用 `@MapperScan` 进行扫描了。

为什么在 Spring 整合 Mubatis 中没有开启事务的代码，但是数据能插入成功呢？

事务由Connection对象控制（Connection.setAutoCommmit()），谁控制了Connection对象，谁就控制了事务。

MyBatis中Connection 对象由连接池对象（JDBC、Druid（封装了JDBC创建连接对象的功能））进行控制，创建Connection，设置了自动提交事务（Connection.setAutoCommmit(true)）

在实际开发中，需要手动提交事务（多条sql一起成功，一起失败），Spring 通过事务控制接管了事务之后，我们就可以手动提交事务了。

所需的依赖：

```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.29</version>
</dependency>
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.6</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.22</version>
</dependency>
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.6</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.8</version>
</dependency>
```

### Xml开发

配置文件中主要依赖有：

- 实体别名
- 数据源
- mapper 文件的注册

`SqlSessionFactoryBean` 对上述三个依赖分配了三个属性：

- `typeAliasesPackage`：指定实体类所在包，为实体类自动创建别名，别名就是首字母小写的类名。
- `dataSource`：用户自定义类型，需要引入连接池对象。
- `mapperLocations`：通配设置，只要以 Mapper.xml 结尾都将被注册。

`MapperScannerConfigure` 需要两个依赖：

- SqlSessionFactory
- Mapper 接口的 class 对象

`MapperScannerConfigure` 对象上述两个依赖分配了两个属性：

- `SqlSessionFactoryBeanName`：通过 value 属性和 SqlSessionFactoryBean 关联起来。
- `basePackage`：设置 Mapper 接口所在的包，mapper 接口的代理对象在 Spring 容器中的 id 值为默认值（首字母小写的接口名）。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--创建连接池-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/test" />
        <property name="username" value="root"/>
        <property name="password" value="root" />
    </bean>

    <!--创建SqlSessionFactory 依赖于SqlSessionFactoryBean来创建-->
    <bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--引入连接池-->
        <property name="dataSource" ref="dataSource"/>
        <!--实体别名-->
        <property name="typeAliasesPackage" value="com.dong.entity"/>
        <!--注册mapper-->
        <property name="mapperLocations">
            <list>
                <value>classpath:com.dong.mapper/*Mapper.xml</value>
            </list>
        </property>
    </bean>

    <!--创建DAO对象-->
    <bean id="scanner" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--引入SqlSessionFactory-->
        <property name="sqlSessionFactoryBeanName" ref="sqlSessionFactoryBean"/>
        <!--mapper接口的包路径-->
        <property name="basePackage" value="com.dong.dao"/>
    </bean>
</beans>
```

注意：可以将数据源的字符串信息转移到小配置文件中。

### 注解开发

1. 配置数据源

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/spring_db?useSSL=false
jdbc.username=root
jdbc.password=root
```

```
public class JdbcConfig {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String userName;
    @Value("${jdbc.password}")
    private String password;

    @Bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(userName);
        ds.setPassword(password);
        return ds;
    }
}
```

2. 创建 MyBatisConfig 整合MyBatis

```
public class MybatisConfig {
    //定义bean，SqlSessionFactoryBean，用于产生SqlSessionFactory对象
    @Bean
    public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource){
        SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();
        ssfb.setTypeAliasesPackage("com.dong.pojo");
        ssfb.setDataSource(dataSource);
        PathMatchingResourcePatternResolver resolver = new PathMatchingResourcePatternResolver();
        ssfb.setMapperLocations(resolver.getResources("classpath:/mapper/*Mapper.xml"));
        return ssfb;
    }
    //定义bean，返回MapperScannerConfigurer对象
    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
        MapperScannerConfigurer msc = new MapperScannerConfigurer();
        msc.setBasePackage("com.dong.mapper");
        return msc;
    }
}
```

3. 创建 Spring 的主配置类进行包扫描以及加载其他配置类

```
@Configuration
@ComponentScan("com.itheima")
//@PropertySource：加载类路径jdbc.properties文件
@PropertySource("classpath:jdbc.properties")
@Import({JdbcConfig.class,MybatisConfig.class})
public class SpringConfig {
}
```

注意：Spring 整合 Mybatis 不能使用 @Component 或者 @Repository 修饰。

## 整合 Junit

1. 导入依赖

```
<!--junit-->
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.12</version>
</dependency>
<!--spring整合junit-->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-test</artifactId>
  <version>5.1.9.RELEASE</version>
</dependency>
```

2. 使用 Spring 整合 Junit 专用的类加载器
3. 加载配置文件或配置类

```
//【第二步】使用Spring整合Junit专用的类加载器
@RunWith(SpringJUnit4ClassRunner.class)
//【第三步】加载配置文件或者配置类
@ContextConfiguration(classes = {SpringConfiguration.class}) //加载配置类
//@ContextConfiguration(locations={"classpath:applicationContext.xml"})//加载配置文件
public class AccountServiceTest {
    //支持自动装配注入bean
    @Autowired
    private AccountService accountService;

    @Test
    public void testFindById(){
        System.out.println(accountService.findById(1));
    }
}
```

注意：

junit 的版本至少是 4.12 否则就会报错。

# Spring事务

## 事务开发思路

如何控制事务：

- DBC  
    开启事务：Connection.setAutoCommit(false：手动、默认)  
    提交事务：Connection.commit()  
    回滚事务：Connection.rollback()
- MyBatis  
    开启事务：MyBatis自动开启事务  
    提交事务：sqlSession.commit()  
    回滚事务：sqlSession.rollback()  
    sqlSession中封装了Connection对象。

控制事务的底层，都是Connection对象完成的。

**事务处理是一种额外功能，所以通过AOP的方式进行开发。**

- 原始对象

- 原始对象提供的原始方法，只关注核心功能（业务处理+DAO调用）
- Service 依赖于 DAO ，所以将DAO作为Service的成员变量，通过注入的方式进行赋值。

- 额外功能  
    我们可以将事务的开启放在原始方法运行之前，将事务的提交放在原始方法运行之后。然后将整个代码用try代码块包裹起来，一旦出现异常，进行捕获，然后进行事务的回滚。  
    Spring将上述代码封装在了`DataSourceTransactionManager`对象中，但是对事务控制需要`Connection`对象，而`Connection`对象封装在连接池对象中，所以需要将连接池对象注入给Spring控制事务对象。
- 切入点  
    Spring提供了一个注解供我们使用`@Transactional`

- 加在类上：类中所有方法都将会添加事务
- 加在方法上：这个方法会添加事务
- 通常加在Service层的接口上

- 组装切面  
    通过标签`<tx:annotation-driven transaction-manager="">`来完成。

1. 切入点：会扫描`@Transactional`来得知切入点
2. 额外功能：在属性`transaction-manager`添加额外功能对象。

依赖：

```
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>5.1.4.RELEASE</version>
</dependency>
```

```
// 1.创建原始对象
@Transactional
public interface UserService{
  
  private UserDAO userDAO;
  
  public UserDAO getUserDAO() {
    return userDAO;
  }
  public void setUserDAO(UserDAO userDAO) {
    this.userDAO = userDAO;
  }
  
  @Override
  public void register(User user) {
    userDAO.save(user);
  }
}
```

- 细节

- 原始对象中的UserDAO是通过MapperScannerConfigure获取的，只要DAO接口的首单词首字母小写即可。
- tx标签的schema以tx结尾。
- 在`<tx:annotation-driven>`标签中存在`proxy-target-class`属性，默认属性值为false，表示使用JDK代理，属性值t为rue，表示使用Cglib代理，足见底层就是用的AOP

```
<tx:annotation-driven transaction-manager="dataSourceTransactionManager" proxy-target-class="false"/>
```

## 事务属性

什么是事务属性：

描述事务特征的一系列值

  

- 隔离属性
- 传播属性
- 只读属性
- 超时属性
- 异常属性

如何添加书屋属性：

在@Transactional注解中添加属性

1. 隔离属性：@Transactional(isolation=)
2. 传播属性：@Transactional(propagation=)
3. 只读属性：@Transactional(readOnly=)
4. 超时属性：@Transactional(timeout=)
5. 异常属性：@Transactional(rollbackFor=,noRollbackFor=)

  

可以设置多个属性，属性之前使用逗号隔开。

#### 1.隔离属性

  

隔离属性的概念：描述了事务解决并发问题的特征

  

什么是并发：多个事务（用户）同一时间（有细微时间的前后差异），访问操作相同的数据。

  

并发产生的问题以及解决办法：

  

1. 脏读  
    一个事务读取了另一个事务中更新但没有提交的数据。因为可能会事务回滚，所以造成了数据不一致的问题。  
    举例：用户A和用户B同时访问相同的数据。用户A对数据进行了更新但没有提交，用户B读到了更新后的数据，随后用户A进行了事务的回滚，导致用户B读到了脏数据。  
    解决：@Transactional(isolation=Isolation.READ_COMMITTED)，修改时加排它锁、读取时加共享锁（其他事务只允许读取）
2. 不可重复读  
    **一个事务**中多次读取相同的数据，但是读取的结果不一样。因为可能同一时间段内另一个事务更新了数据，所以造成数据不一致的问题。  
    举例：用户A和用户B同时访问相同的数据，用户A先查询了一次，同时，用户B对数据进行了更新并且提交了，用户A查询之后又做了一些操作，然后再次查询，发现两次查询数据不一致  
    解决：@Transactional(isolation=Isolation.REPEATABLE_READ)。本质上是在表中一行读数据的过程中添加了**行锁**。别的事务只能等待。
3. 幻读  
    一个事务中多次对整表进行统计查询，但是结果不一样。因为可能同一时间段内另一个事务插入或者删除了数据，所以造成数据不一致的问题。  
    举例：用户A和用户B同时访问相同的数据，用户A先统计了一下表，用户A随后插入了数据并且提交了，用户A查询之后又做了一些操作，然后再次进行了数据的统计，发现数据总量不一致  
    解决：@Transactional(isolation=Isolation.SERIALIZABLE)。本质上是在表中统计数据的过程中添加了**表锁**。别的事务只能等待。

  

总结：

  

- 并发安全：SERIALIZABLE>REPEATABLE_READ>READ_COMMITTED
- 运行效率：READ_COMMITTED>REPEATABLE_READ>SERIALIZABLE

  

不是所有数据库对隔离属性的值都是支持的：

|   |   |   |
|---|---|---|
|隔离属性的值|MySQL|Oracle|
|ISOLATION_READ_COMMITTED|✔|✔|
|ISOLATION_REPEATABLE_READ|✔|❌|
|ISOLATION_SERIALIZABLE|✔|✔|

  

Oracle通过多版本比对的方式，解决不同重复读的问题。

  

如果不设置隔离属性的值，Spring会设置默认隔离属性（ISOLATION_DEFAULT）并根据不同的数据有不同的实现：

  

- MySQL：REPEATABLE_READ（使用命令 select @@tx_isolation查看）
- Oracle：READ_COMMITTED

  

隔离属性在实战中的建议：

  

其实在实战中，并发情况非常罕见（需要超级用户群体），所以使用Spring默认指定的隔离属性即可。

  

如果真遇见了并发问题，可以使用乐观锁（在Mybatis中通过拦截器自定义开发）。

  

#### 2.传播属性

  

- 传播属性的概念  
    概念：描述了事务解决嵌套问题的特征。  
    事务的嵌套：一个大事务中包含若干个小事务。  
    问题：大事务中融入了很多小事务，它们彼此影响，最终导致外部的大事务，丧失了事务的原子性。  
    举例：事务A中包含事务B事务C，在执行事务A的过程中，先执行事务B，并且提交了，在执行事务C的时候发生了异常，为保证事务的原子性，事务A必须回滚，但是这时候事务B因为提交回滚不了
- 传播属性的值及其用法

|   |   |   |   |   |
|---|---|---|---|---|
|传播属性的值|外部不存在事务|外部存在事务|用法|备注|
|REQUIRED|开启新的事务|融合到外部事务中|[@Transactional(propagation](/Transactional\(propagation)<br><br>= Propagation.REQUIRED)|增删改方法|
|SUPPORTS|不开启新的事务|融合到外部事务中|[@Transactional(propagation](/Transactional\(propagation)<br><br>= Propagation.SUPPORTS)|查找方法|
|REQUIRES_NEW|开启新的事务|挂起外部事物，创建新的事务|[@Transactional(propagation](/Transactional\(propagation)<br><br>= Propagation.REQUIRES_NEW)|日志记录方法|
|NOT_SUPPORTED|不开启新的事务|挂起外部事物|[@Transactional(propagation](/Transactional\(propagation)<br><br>= Propagation.NOT_SUPPORTED)|不常用|
|NEVER|不开启新的事务|抛出异常|[@Transactional(propagation](/Transactional\(propagation)<br><br>= Propagation.NEVER)|不常用|
|MANDATORY|抛出异常|融合到外部事务中|[@Transactional(propagation](/Transactional\(propagation)<br><br>= Propagation.MANDATORY)|不常用|

- 默认的传播属性  
    REQUIRED是传播属性默认的值。
- 传播属性的使用方式

- 在增删改方法中，可以不用指定传播属性的值。
- 在查找方法中，需要显示指定传播属性的值为SUPPORTS

  

#### 3.只读属性

  

针对只进行查询操作的业务方法，可以加入只读属性，提高运行效率。

  

默认值：false

  

#### 4.超时属性

  

指定了事务等待的最长时间。

  

为什么事务进行等待？

  

当前事务访问数据时，可能访问的数据被别的事务进行了加锁处理，当前事务只能等待。

  

单位：秒 Transactional(timeout=2)

  

超时属性的默认值：-1(超时时间由不同数据库来指定)

  

#### 5.异常属性

  

Spring 处理事务过程中，默认情况下：

  

- 对于RuntimeException及其子类，采用的是回滚策略。
- 对于Error和Exception以及子类，采用的是提交策略。

  

如果想让Spring对RuntimeException采用提交策略：@Transactional(noRollback={java.lang.RuntimeException.class，xxx})。

  

如果想让Spring对Exception采用回滚策略：@Transactional(Rollback={java.lang.Exception.class，xxx})。

  

但是在实际开发中，并不对异常属性进行重新指定策略。使用默认策略。

### 配置总结

1. 隔离属性 默认值
2. 传播属性    Required(默认值)增删改      Supports 查询操作
3. 只读属性    readOnly  false增删改      true查询操作
4. 超时属性    默认值-1
5. 异常属性    默认值

  

总结：

  

- 对于增删改操作   [@Transactional](/Transactional)
- 对于查询操作     @Transactional(propagation=Propagation.SUPPORTS，readOnly=true)

## 事务开发

### XML开发

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786133895-fbbc0948-8665-4ad3-9182-2e4b9e09f5ed.png?x-oss-process=image%2Fresize%2Cw_1143%2Climit_0)

### 注解开发

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667786148981-a2476fb7-ff47-4f3e-907d-cf216aedc0a7.png?x-oss-process=image%2Fresize%2Cw_1143%2Climit_0)

好是将@Transactional注解加在业务层的接口上

对于使用配置Bean设置事务可以使用它的接口PlateformTransactionaManager

  

# 验证、数据绑定和类型装换




# SpringMVC
## ![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678709170400-209cc7cb-8520-466c-a859-e8c00900d87e.png)

  

  

  

  

## 一、引言

  

### 1.什么是SpringMVC

  

概念：SpringMVC 是基于 Spring Framework 衍生而来的一个 MVC 框架。主要解决了原有MVC框架开发过程中，控制器（controller）的问题。

  

#### MVC架构

  

SpringMVC 是一种 MVC 架构，MVC 是一种架构思想，在 JavaEE 开发中用于多 Web 开发。

  

应用 MVC 框架，会把项目划分为三个层次：

  

- View：JSP
- Model：Service+DAO
- Controller：Servlet

  

MVC分层开发，体现了面向对象、各司其职的设计思想，有利于后续项目的维护。

  

#### 基于SpringFramework

  

- 通过工厂（容器）创建对象，解耦合（IOC、DI）
- 通过 AOP 方式，为原始类添加额外功能
- 方便与第三方框架继承

- Mybatis
- JPA
- MQ
- ES
- Redis
- ...

  

SpringMVC 基于 Spring 就可以将 SpringFramework 的优点继承下来。

  

#### 原有MVC开发中控制器的问题

  

回答这个问题，首先我们应该明确两个问题的答案：

  

1. **原有MVC开发中控制器使用哪些技术实现？**

- Servlet（基于Java Model2模式）
- Struts2 中的 Action

2. **这些技术在实现过程中存在哪些问题？**

  

- 控制器的核心作用

- 接受用户请求
- 调用业务功能（Service）
- 并根据处理结果控制程序的运行流程

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789459267-c60d6233-cfb3-40f6-b31b-f9dfb67f214c.png)

- **控制器的核心代码**

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789477193-38612f61-a122-404f-b170-2d5019d03cd4.png)

- **现有控制器存在的问题**

- **接受client请求参数过程中存在的问题**

```
1. 代码冗余

2. 只能接受字符串类型数据，其它类型需要手工进行转换

3. 无法自动封装对象
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789493906-c0093d96-2b5d-4ab0-83a3-5f11eb86a8cd.png)

- **调用业务对象（Service）过程中存的问题**  
    通过new的方式创建对象，会存在耦合

```
  UserService userService = new UserServiceImpl;// 存在耦合
	UserService.login("name","password")
```

- **流程跳转（页面跳转）过程中存在的问题**

```
1. 代码冗余

2. 跳转路径存在耦合

3. 与视图层技术耦合
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789546453-89bae894-38a2-4783-9376-3824b16b8f38.png)

  

### 2.SpringMVC的学习要点

  

#### 2.1SpringMVC的三种开发模式

  

- **传统视图开发**

```
1. 通过作用域（request、session）进行数据传输

2. 通过视图层技术进行数据展示（JSP、Freemarker、Thymeleaf）
```

- **前后端分离开发**

```
1. 多种新的请求发送发送

2. Restful的访问

3. 通过HttpMassageConverter进行数据响应
```

- **Spring5 WebFlux开发**

```
1. 替换传统Web开发的一种新的Web开发方式

2. 通过NettyServer，进行Web通信
```

  

#### 2.2控制器开发

  

对于控制器的开发也就是主要解决原有控制器存在的三个问题

  

```
1. 接受client用户请求

2. 调用业务对象

3. 流程跳转
```

  

## 二、第一个SpringMVC程序的开发

  

### 1.版本控制

  

```
1. JDK 1.8
2. Maven 3.6
3. Idea 2021+
4. Spring Framework 5.1.4
5. Tomcat 8.5.29
6. Mysql 5.7.18
```

  

### 2.开发步骤

  

1. 导入依赖
2. 初始化DispatcherServlet
3. 配置文件进行配置（开启映射器和适配器,包扫描）
4. Controller的开发

  

### 3.环境搭建

  

#### 3.1Idea设置

  

1. 创建多模块项目，设置Maven（创建新项目会启用idea默认的idea）、JDK
2. 使用父子工程结构，创建父工程管理依赖、管理项目结构，删除父项目src文件夹
3. 创建子项目，选择父项目
4. 关联父子项目，在父项目中存在`<modules>`标签，代表子项目，在子项目中存在`<parent>`标签，代表父项目。
5. 补全子项目项目结构（点击src文件夹新建目录会有提示）
6. 修复web.xml（默认Servlet2.0），我们需要更高版本，删除web.xml，在项目结构中找到fact，找到当前项目，找到部署描述符，先删除原有的描述符，然后新建（注意路径）。
7. 设置子项目的pom文件，设置JDK版本，子项目的依赖和插件统一交由父项目完成，所以子项目中的可以删除
8. 添加Tomcat，在部署中选择普通目录（exploded），便于调试，添加虚拟机参数避免控制台启动时乱码`-Dfile.encoding=UTF-8` ，**选择重新部署**和**更新类和资源**。

  

#### 3.2添加依赖

  

```
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.2</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.1.4.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.1.4.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.24</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>5.1.4.RELEASE</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.29</version>
</dependency>
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.6</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.1.4.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.6</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.8</version>
</dependency>
<dependency>
    <groupId>org.yaml</groupId>
    <artifactId>snakeyaml</artifactId>
    <version>1.30</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.1.4.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.1.4.RELEASE</version>
</dependency>
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope><!--仅编译-->
</dependency>
<dependency>
    <groupId>jstl</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>javax.servlet.jsp-api</artifactId>
    <version>2.3.3</version>
    <scope>provided</scope>
</dependency>
```

  

#### 3.3配置文件初始化

  

- 初始化Dispatcher(web.xml)注意version=4.0

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    
    <servlet>
        <servlet-name>servlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--指定SpringMVC配置文件的路径-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:dispatcher.xml</param-value>
        </init-param>
        <!--在Tomcat启动时创建-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>servlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

- **dispatcher.xml**

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context-4.2.xsd
                           http://www.springframework.org/schema/mvc
                           http://www.springframework.org/schema/mvc/spring-mvc.xsd
                           http://www.springframework.org/schema/tx
                           http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--设置注解扫描的路径-->
    <context:component-scan base-package="com.dong"/>

    <!--引入SpringMVC的核心功能-->
    <mvc:annotation-driven/>
</beans>
```

  

#### 3.4配置文件初始化细节

  

- **DispatcherServlet**

```
1. DispatcherServlet称为前端控制器(中央控制器)

2. DispatcherServlet的核心作用：
	1.用于创建Spring工厂(容器)
			ApplicationContext context = ClassPathXmlApplicationContext("/applicationContext.xml") // 所以我们要在web.xml中设置配置文件的路径。
			因为DispatcherServlet封装的Spring工厂(容器)只能读取xml，所以无法迁移到纯注解编程
  3. 控制SpringMVC内部的运行流程
```

- **SpringMVC的配置文件(dispatcher.xml)**

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789579969-bea9461c-7847-446a-9ffb-871be24ca6e6.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789596173-bd0acb1f-000f-451b-a605-c50d419f21e1.png)

### .编码开发

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789610422-88fa7668-ea0a-4c7d-b468-0da7d0f65e92.png)

  

  

**开发流程**：

  

1. 定义一个类，加上@Controller注解。
2. 提供一个控制方法，参数为HttpServletRequest、HttpServletResponse，返回值为String。在方法上加上@RequestMapping注解，指定访问路径。
3. 把对应JSP页面的路径作为返回值返回。

  

### 4.多服务方法

  

- Servlet作为控制器，一个类中只能提供一个服务方法
- SpringMVC作为控制器，一个类中可以提供多个服务方法

  

### 5.注意

  

SpringMVC开发中的Controller也称为Handler（SpringMVC内部叫法）

  

### 6.细节分析

  

#### 6.1一种类型的SpringMVC控制器被创建几次？

  

- Servlet被创建的次数  
    一种类型的Servlet，只会被Tomcat创建一次，所以Servlet是单实例的。
- Servlet是单实例并不是单例设计模式（单例设计模式构造函数私有）。
- SpringMVC的控制器被Spring工厂创建的次数  
    可以创建一次也可以创建多次，由Scope注解(默认singleton)

```
@Controller
@Scope(singleton | prototype)
public class FirstController{
  //...
}
```

- 默认情况下SpringMVC的控制器只会被创建一次, 会存在线程安全问题

  

#### 6.2[@RequestMapping](/RequestMapping)

  

作用: 为控制器方法提供外部URL访问路径

  

支持: 由`<mvc:annotation-driven/>`引入的RequestMappingHandlerMapping进行支持.

  

细节:

  

- 路径分割符可以忽略

```
@RequestMapping("first")

// 忽略第一个路径分隔符
@RequestMapping("first/second")
```

- 在一个方法上映射多个访问路径

```
@RequestMapping(value={"first","second"})
```

- 在Controller类上加@RequestMapping注解

```
localhost:8989/01/user/first

@RequestMapping("/user")
@Controller
public class Controller{
  
  @RequestMapping("/first")
  public String First(HttpServletRequest req,HttpServletResponse resp){
    //...
  }
}
```

  
设计目的:  
按照功能, 进行不同模块的区分, 有利于项目的管理

- RequestMapping限定用户的请求方式

- 请求方式  
    前端向后端发送数据的方式(get | post)  
    两种请求方式的区别:

```
1. GET请求:通过请求行(地址栏)提交数据(QueryString),明文提交数据,不安全,提交数据量小( < 2048字节)

		https:localhost:8989/01/user/first?username=张三&password=123
		
2. POST请求:通过请求体提交数据,密文提交(非加密,只是用户看不见),相对安全,数据量大(理论上没有限制)
```

- 两种请求发起方式的区别

```
1. GET请求

	浏览器地址栏:     localhost:8989/basic/firstController/first
	
	超链接:          <a href="${pageContext.request.contextPath}/firstController/first">超链接</a>
	
	表单:            <from action="${pageCOntext.request.contextPath}/firstController/first"></from>   
	
	JS:						 location.href=;
									
	ajax:					 $.ajax({url:url,typE:\Pictures\Typora\"get",...})	
	
	专属工具或者库:	POSTMAN | POSTWOMAN | RestfulToolKits | RestTemplate | HttpClient | OKHttp | NsMutableURLRequest
	
2. POST请求

	表单:          <from action="${pageCntext.request.contextPath}/firstController/first" method="post"></from>
	
	ajax:         $.ajax({url:url,typE:"post",...})	
	
	专属工具库:    POSTMAN | POSTWOMAN | RestfulToolKits | RestTemplate | HttpClient | OKHttp | NsMutableURLRequest
```

- @RequestMapping如何限定请求方式

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789669886-f570715a-67fe-4196-9faf-c3b85b39636e.png)

- Http协议中的其它请求方式

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789684431-50172ff0-7c3d-4a2a-bf81-af258be22f71.png)

- @RequestMapping限定方法参数  
    服务方法可以任意选取域对象参数

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789701825-3e467a00-81ff-4934-8215-407d12eec342.png)

- 那种方式更为常用

```
1. 前四种因为与ServletAPI耦合，不推荐使用

2. 第五种，不能接受用户参数，不推荐使用
```

  

### 7.视图解析器

  

```
1. 视图解析器用于页面跳转
	
	@Controller
	@RequestMapping("/user")
	public class FirstController{
	@RequestMapping("/first")
		public String m1(){
			// 1. 接受用户参数
						...
			   2. 调用业务方法
			   		...
			   3. 视图跳转
			   return "/index.jsp";
		}
	}
```

  

目前页面跳转存在的问题：

  

跳转路径与实际路径存在耦合

  

- 视图解析器(ViewResolver)  
    通过视图解析器解决跳转路径与实际路径耦合的问题  
    思路分析：  
    将原来页面跳转中变化的提取出来，只留下不变的东西。  
    ![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789730070-797b8733-d2eb-4332-824e-0f9b63d77725.png)  
    通过视图解析器ViewResolver进行跳转路径的拼接。

```
<bean id="resolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  <!--前缀-->
	<property name="refix" value=""/>
  <!--后缀-->
  <property name="suffix" value=""/>
</bean>
```

- 注解开发

```
@Configuration
public class AppConfig {
    // 视图解析器在配置Bean中的配置
    @Bean
    public InternalResourceViewResolver resolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/");
        resolver.setSuffix(".jsp");
        return resolver;
    }
}
```

  

### 8.SpringMVC配置文件的默认设置

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789744469-e92997c2-f498-4259-8acc-78cdb6588923.png)

  

  

## 三、SpringMVC控制器开发详解

  

### 1.核心要点

  

1. 接受Client请求参数
2. 调用业务方法
3. 页面跳转

  

### 2.接收Client请求参数详解

  

#### 2.1回顾

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789755915-14e126c7-2633-47f5-a85b-e680628a992f.png)

  

  

#### 2.2基于ServletAPI接受用户参数

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789769128-87774481-774e-4770-86e0-c034f157a59c.png)

  

  

#### 2.3基于简单变量接收Client参数

  

简单变量：8种基本数据类型+String类型的变量，把这些类型的变量作为控制器方法的形参，用于接受client提交的数据

  

思路分析：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789781165-e3693e07-2b25-4aee-8c9f-0ebc6ec6a56f.png)

  

  

发送数据的name属性值要与参数的名字一致。

  

- 细节分析

- SpringMVC支持常见类型（8中基本数据类型及包装器以及String类型）的自动转换

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789815867-3090d759-e937-401f-aeb2-6bdd68e17166.png)

- 基本数据类型尽量使用包装器

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789829711-7a2543b6-fcf2-46fe-af32-c6353aecd8d3.png)

#### 2.4基于POJO类型接受Client参数

  

- 什么是POJO

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789841726-ebb1a0e3-a569-49d9-ae78-cf7cb59e2cde.png)

- 使用场景

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789857056-ebbf9ee7-bc43-413a-9cc7-7ef82944f043.png)

- 注意1

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789870133-2aef635a-d3f2-4de7-a0fd-d13764d0b3eb.png)

- 注意点2

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789880355-27941ca0-fa89-4d62-bc52-c71d7dc44d2c.png)

#### 2.5基于一组简单变量接受Client参数

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789896536-09c3718d-f2ee-4858-a1f5-5bbd89bad19c.png)

  

  

- 细节  
    能不能用集合接受一组参数

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789909842-5be2db76-16c5-4a9c-9179-2c913b17c072.png)

#### 2.6接受一组POJO类型的对象的请求参数

  

通过一个包装类进行封装

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789923632-f38c4edb-5ead-4fc3-a922-a5d0a7188dfb.png)

  

  

#### 2.7SpringMVC接受Client请求参数的总结

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789936938-4bcff2ef-e914-4cd2-bf74-9774b0e9b86b.png)

  

  

#### 2.8接受JSON类型数据

  

开启SpringMVC识别JSON，在配置类加上

  

```
@EnableWebmvc
```

  

### 3.@RequestParam注解

  

作用：用于修饰控制器方法的形参

  

```
@RequestMapping("/param1")
public String param1(@RequestParam String name,@RequestParam String password){
  
}
```

  

#### 3.1@RequestParam注解详解

  

- 解决请求参数和方法形参名字不一致的问题

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789956727-ed8a9b9d-ec34-4dc1-a74a-fbd35d035624.png)

- 注意事项

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789970312-4e02334d-f791-42a3-8652-8ababcc62d82.png)

- @RequestParam的required属性

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789981980-7b16b661-d678-4d36-8912-b9ab40543b1e.png)

- RequestParam的defaultValue属性

- 客户端没有提交数据的时候，给对应的形参设置默认值

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667789995771-3699e297-e77f-4d35-bee3-88c85eef57e5.png)

- 解决控制器方法形参，使用包装类的问题  
    使用了defaultValue属性之后，就算控制器没有提交数据，使用int接受也不会报错，因为我们可以给它的默认值设置为0
- 未来开发中的使用场景

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790012849-35465761-90ea-4955-8a19-f2d7216eb4c3.png)

### 4.中文请求参数乱码的问题

  

#### 4.1回顾JavaWeb开发中中文乱码解决方案

  

- GET请求乱码解决方案

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790031283-60adf0f5-466f-428e-adda-52564914a22f.png)

- POST请求乱码解决方案

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790042725-7941d686-314d-44d3-bc44-a1c5c00db557.png)

#### 4.2SpringMVC解决中文字符集乱码

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790054711-4ff4d440-7bbb-481b-9c11-f5dc2adddf06.png)

  

  

web.xml

  

```
<filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

  

### 5.SpringMVC的类型转换器

  

#### 5.1内置类型转换器

  

SpringMVC提供了内置类型转换器，把客户端提交的字符串类型参数，转换为控制器方法需要的数据类型

  

SpringMVC只提供了常见类型的转换器：8中基本数据类型+常见集合等

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790078874-2dcb3a3b-a50e-4658-a75a-434b2df5514f.png)

  

  

- 原理分析

  

#### 5.2自定义类型转换器

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790097693-801526d0-d4dd-4a28-a2ee-4cd881309cdd.png)

  

  

  

  

### 6.接收其它请求参数

  

#### 6.1动态参数收集

  

- 分析

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790110495-704f35a4-d49e-4f02-a018-ed4a87ea90d6.png)

- 单值动态参数收集

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790124619-6a3c18ea-8ad6-4b17-a43f-cba9c4e22623.png)

- 多值动态参数收集  
      
    MultiValueMap的key为参数名，value是一个list集合，存储多个参数值
- 应用场景

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790139843-83756480-265a-47b7-9b2b-b2b70453572b.png)

#### 6.2接受Cookie数据

  

- Servlet中获取方式

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790150677-97a01aab-e332-4bbb-ab89-7a30c91d29e0.png)

- SpringMVC中获取方式

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790162154-32334828-16dd-4e31-aace-905b43cddc33.png)

#### 6.3接受请求头数据

  

- 什么是请求头

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790174630-11ec6e89-5efc-4268-8c6b-5019617f608f.png)

- 获取请求头方式

- Servlet

```
String value = request.getHeader("key");
```

- SpringMVC

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790187627-0aea53d6-928b-4c94-ac69-3309d27ee9cc.png)

## 四、SpringMVC控制器开发详解二

  

### 1.核心要点

  

前面学习了SpringMVC接受请求参数

  

这章将学习SpringMVC调用业务对象

  

### 2.SpringMVC调用业务对象（SSM整合）

  

#### 2.1思路分析

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790208948-03acb435-eece-4e1c-a4ae-ced8ba269777.png)

  

  

  

  

#### 2.2编码

  

// TODO

  

#### 2.3父子工厂（父子容器）拆分

  

- 现有SSM整合存在的问题

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790226171-08faa018-9d6d-49a5-b5b8-6e5781c71f90.png)

- 解决方案  
    DispatcherServlet中封装着Spring工厂对象（webApplicationContext）  
    对MVC和非MVC的配置文件进行拆分

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790247212-c89ffe75-08f3-4b20-b944-daa8b122f5a0.png)  
Controller由MVC创建，Service由Spring创建  
一个原则：子工厂没有的东西可以去父工工厂获取，但是父工厂没有的动词不能去子工厂获取，所以Controller可以获取Service

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790279221-6bb64543-5b08-4f3a-9c35-255a3f344308.png)

- 编码  
    //TODO

  

#### 2.4父子容器问题

  

- 问题  
    在现有的父子容器开发中，没有设置上事务
- 原因  
    设置了相同的包扫描路径，都会创建Controller和Service，Client会先去子容器中寻找。而子容器中并没有设置事务。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790304110-32957f20-1de5-4ce9-885b-65b34d1b1aef.png)

- 解决方案

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790315728-b2e4b67c-22e0-407a-bba5-c7ebcfc6754b.png)

### 3.SpringMVC控制器调用业务对象总结（SSM）

  

#### 3.1完整编码总结

  

- web.xml

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790331038-a369acd6-1c4c-4d46-bdf9-8c72c949369b.png)

- dispatcher.xml

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790342177-9c7bc672-ef36-492d-ae88-25d7fa658ed6.png)

- applicationContext.xml

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790359681-5ffa8b00-dc49-4307-bb01-4aff17d6b063.png)

- DAO

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790376739-e8c6cd7b-df32-44b8-ab09-4bb296a5df1e.png)

- Service

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790384930-a775a781-277b-47f7-834a-eeadcb425515.png)

- Controller

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790396817-7e47e4a0-145a-4a53-85e9-2112207205fd.png)

## 五、SpringMVC控制器开发详解三

  

### 1.核心要点

  

前面学习了SpringMVC作用中的接收用户请求参数和调用业务对象，这章将学习流程跳转\发出响应

  

### 2.JavaWeb中流程跳转的回顾

  

#### 2.1JavaWeb流程跳转的核心代码

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790422165-70d5765d-8c1a-44aa-9d6c-53c771856447.png)

  

  

#### 2.2JavaWeb页面跳转方式的回顾

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790434475-035fcc6c-a6bd-4870-a939-96026944433f.png)

  

  

Session：状态、购物车、验证码...

  

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790453932-0270b274-9754-4ed4-9e0b-0183867539b8.png)

  

如果传输的是对象类型，需要把对象中的数据取出，将这些数据拼接

  

#### 2.3SpringMVC的四种跳转方式

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790466036-ab6817cf-f681-4d74-9ab4-a989ac634bb0.png)

  

  

#### 2.4控制器forward页面

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790479100-95cd6a06-65e9-4d8f-a12f-fe4d61ba5c23.png)

  

  

#### 2.5控制器redirect页面

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790488895-67b8f277-0c29-4dc5-8781-94a9c7b5b79a.png)

  

  

#### 2.6控制器forward控制器

  

- 应用场景

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790505691-32a6723d-a4c1-4e6f-a268-bce62db84107.png)

- 编码

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790517365-32b2b687-0bdd-4148-939f-cec5992782f3.png)

#### 2.7控制器redirect控制器

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790531335-9c2b5bf8-b3da-4b2b-b287-ff834a7d3a8b.png)

  

  

在JavaWeb中需要在路径中添加应用名，而SpringMVC中不用添加

  

### 3.Web作用域处理

  

#### 3.1JavaWeb中作用域回顾

  

- 三种作用域及其使用场景

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790549004-8130c056-60ff-4e42-bce5-d2e83e077f38.png)

#### 3.2SpringMVC中作用域处理

  

- 基本使用方式及其存在的问题

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790570856-911e960c-3b94-42aa-868a-a4aaab1fd649.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790585929-1fdb1d14-8dfa-4317-b328-cb0b392f3c5d.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790600180-dba68e93-9481-4e9c-9c3e-c868d3140c10.png)

- SpringMVC中request作用域的处理

- 代码

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790614353-de5363b8-5c20-46ec-ac12-2c235643f0b0.png)

- Model、ModelMap的相关细节

```
1. 通过Model、ModelMap进行作用域的处理，就可以解决视图模板耦合的问题
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790639114-403b455a-fda6-4ac7-a322-a72ae78c68f7.png)  
  
SpringMVC通过视图解析器识别不同的视图模板

```
2. SpringMVC中提供的Model和ModelMap这两种方式处理request作用域，它们的区别是什么
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790735423-5b9a2031-3b82-4da3-ade1-81b9e3386888.png)

```
3. 为什么不直接使用BindingAwareModelMap？
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790757908-74f841c7-d50f-4cb4-8999-c2c3cea9bdb0.png)

```
4. SpringMVC开发中为什么会提供两种方式？Model和ModelMap那种方式更为推荐？
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790783266-fde6479c-d6ed-453f-bb14-f46e244dd590.png)

```
5. 如果redirect跳转，数据该如何传递？
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790797816-c0c115eb-2eca-4a35-9e50-91829889cc75.png)

  

- SpringMVC中Session作用域的处理

- 基本使用及其存在的问题

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790837622-ac39e69b-94de-47b3-96ab-e03117bd0c7f.png)

- @SessionAttributes注解  
    可以在控制器类上加入该注解，选择性的为Session中存储数据

- 存储数据

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790856626-ce1ba1c5-24b8-499a-b19b-c78d2dfbd8e1.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790869264-fd157207-cd17-4966-9e32-672bf0aa82c8.png)

- 注意

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790881754-3906b2f4-2847-474a-9ec5-491f9ffc63b0.png)

- 删除数据

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790895452-ab87085e-2033-441a-837a-3b4a7a17b8cc.png)

- SpringMVC中application作用域的处理

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790910118-8aea212a-48a3-4946-99a9-33fcf16cfe82.png)

- 为什么SpringMVC没有提供application作用域

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790920032-7095a38e-cb10-430b-a860-6d4b1f3f3f8b.png)

- 特殊操作

- @ModelAttribute注解

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790960202-45fb341f-0a7d-44d2-863c-54a8aeb3af1d.png)

- 使用场景

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790977367-67b8cb6c-caa6-44fa-a699-f2e994b722eb.png)

- 注意细节

- 细节一

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667790993548-2174840d-20fe-42b7-89ea-d64dc98d8eb8.png)

- 细节二

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667791008856-4e4adc35-04c1-435c-a2c2-8659a85b2bf6.png)

- ModelAndView技术

- 什么是ModelAndView【了解】

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667791026025-10a92e1a-b6e0-419e-b332-cafb8a10dbc3.png)

- 总结目前控制器方法的返回值

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667791036207-aa08e9e5-079c-4c67-91a2-556d7c6999ce.png)

### 4.视图控制器

  

#### 4.1什么是视图控制器

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667791046690-8f3b3302-242d-4423-8bcd-2264c93afab3.png)

  

  

#### 4.2受保护的视图模板该如何访问

  

所有视图模板只能通过控制器的forward进行访问

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667791059046-f83a750a-f64b-4851-a0a9-1597adbe72c2.png)

  

  

使用试图控制器，简单的创建空的只用于跳转到JSP的控制器方法，在标签中设置

  

注意：视图控制器的path属性值不能和@RequestMapping设置的路径冲突

  

#### 3.视图控制器的redirect跳转

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667791071597-accc2665-c63d-46be-8630-b7a39c20f58f.png)

  

  

### 5.静态资源处理

  

#### 5.1什么是静态资源

  

项目中非java代码部分的内容，图片、js、css

  

在这章学习之前，我们无法访问静态资源

  

#### 5.2原因

  

用户请求被DispatcherServlet接收之后，DispatcherServlet会去查找是否存在同名的控制器方法，找到之后，实例化对象，调用控制器方法，但是现在很明显是找不到，所以会报404错误

  

#### 5.3解决方法

  

- 激活Tomcat提供的DefaultServlet，在请求进入DispatcherServlet之前，先由DefaultServlet将对于静态资源的访问URL拿走
- `<mvc:resources>`，url转移，将对于requestMapping的请求转移到ResurceHttpRequestHandler中
- `<mvc:default-servlet-handler>`，将requestMapping的请求转移到DefaultServletRHttpRequestHandler中

  

### restful

  

  

  

## Thymeleaf

  

# 统一异常处理

  

# 拦截器

  

S pring工厂是一个重量级资源，创建一次

  

所以需要将可能初始化的类都要注册进容器，初始化容器的时候，也会将这些组件初始化

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667791374830-bcce6c31-3bc0-41c6-ab0e-30094e0ca3b3.png)







  

# 1. SpringMVC概述

Spring MVC 是 Spring 提供的一个基于 MVC 设计模式的轻量级 Web 开发框架。

本质相当于 Servlet ，在 SpringFramework5.x中，取消了 Servlet，改为 Spring MVC。

SpringMVC 是基于 Spring Framework 衍生而来的一个 MVC 框架。主要解决了原有MVC框架开发过程中，控制器（controller）的问题。

# 2. SpringMVC入门

## 入门案例

1. 导入依赖

```
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.10.RELEASE</version>
    </dependency>
```

2. 定义 Controller 类

```
//定义表现层控制器bean
@Controller
public class UserController {
    //设置映射路径为/save，即外部访问路径
    @RequestMapping("/save")
    //设置当前操作返回结果为指定json数据（本质上是一个字符串信息）
    @ResponseBody
    public String save(){
        System.out.println("user save ...");
        return "{'info':'springmvc'}";
    }
}
```

3. 编写 SpringMVC 的配置类，加载处理请求的 Bean

```
//springmvc配置类，本质上还是一个spring配置类
@Configuration
@ComponentScan("com.itheima.controller")
public class SpringMvcConfig {
}
```

4. 加载配置类，初始化容器，设置拦截请求路径

```
//web容器配置类
public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer {
    //加载springmvc配置类，产生springmvc容器（本质还是spring容器）
    protected WebApplicationContext createServletApplicationContext() {
        //初始化WebApplicationContext对象
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        //加载指定配置类
        ctx.register(SpringMvcConfig.class);
        return ctx;
    }

    //设置由springmvc控制器处理的请求映射路径
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    //加载spring配置类
    protected WebApplicationContext createRootApplicationContext() {
        return null;
    }
}
```

## 案例注解和类解析

@Controller注解

作用在控制器类上，负责定义控制器类

@RequestMapping注解

作用在控制器类或者控制器方法上，负责设置当前控制器类或者方法的请求路径

@ResponseBody注解

作用在控制器类或者控制器方法上，负责设置当前控制器类或者控制器方法响应内容为当前返回值（默认返回要跳转的页面）

AbstractDispatcherServletInitializer类

是SpringMVC提供的快速初始化Web3.0容器的抽象类。提供三个接口方法供用户实现。

- createServletApplicationContext()：创建Servlet容器时，加载SpringMVC对应的bean并放入WebApplicationContext对象范围中
- getServletMappings()：设定SpringMVC对应的请求映射路径，设置为/表示拦截所有请求，任意请求都将转入到SpringMVC进行处理
- createRootApplicationContext()：如果创建Servlet容器时需要加载非SpringMVC对应的bean，使用当前方法

## 工作流程分析

1. 服务器启动，执行ServletContainersInitConfig类，初始化web容器
2. 执行createServletApplicationContext方法，创建了WebApplicationContext对象
3. 加载SpringMvcConfig配置类
4. 执行@ComponentScan加载对应的bean
5. 加载UserController，每个@RequestMapping的名称对应一个具体的方法
6. 执行getServletMappings方法，定义所有的请求都通过SpringMVC

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679032705732-50156505-7f3c-484c-995b-ec3efb06ef4f.png)

## 父子容器加载Bean

事实上有两个容器，Spring 容器及 Spring MVC 容器，如果加载错误的 Bean 将会导致事务等情况出错。

Spring 容器：表现层以外的 Bean

SpringMVC：表现层的 Bean

加载控制：

```
@Configuration
@ComponentScan(value = "com.itheima",
               excludeFilters = @ComponentScan.Filter(
                   type = FilterType.ANNOTATION,
                   classes = Controller.class
               )
              )
public class SpringConfig {
}
```

属性

1. excludeFilters：排除扫描路径中加载的bean，需要指定类别（type）与具体项（classes）
2. includeFilters：加载指定的bean，需要指定类别（type）与具体项（classes）

Bean 的加载格式：

```
public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer { 
    protected WebApplicationContext createServletApplicationContext() { 
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        ctx.register(SpringMvcConfig.class);
        return ctx;  
    }   
    protected WebApplicationContext createRootApplicationContext() {  
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();      
        ctx.register(SpringConfig.class);        
        return ctx;  
    }   
    protected String[] getServletMappings() { 
        return new String[]{"/"}; 
    }
}
```

简化：

```
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer{
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class}
    };
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }
}
```

# 3. 请求与响应

## 3.1 请求的中文乱码问题

如果浏览器发送的请求参数是中文，可能接收到数据后会出现乱码问题，对于中文乱码 GET 和 POST 有不同的解决方案。

对于GET请求，Tomca 在8.5 之后就不会再出现中文乱码问题了，低于 8.5 可以在Tomcat中进行设置。

对于 POST 请求，在加载SpringMVC配置的配置类中指定字符过滤器。

```
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
    protected Class<?>[] getRootConfigClasses() {
        return new Class[0];
    }

    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }

    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    //乱码处理
    @Override
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        filter.setEncoding("UTF-8");
        return new Filter[]{filter};
    }
}
```

## 3.2 请求

1. 普通参数

请求参数与形参名称对应即可完成参数传递

请求参数名与形参名不同时，使用@RequestParam注解关联请求参数名称与形参名称之间的关系

@RequestParam有两个参数：

- required：是否为必传参数
- defaultValue：参数默认值

2. POJO参数

请求参数名与形参对象属性名相同，定义POJO类型形参即可接收参数

3. 嵌套 POJO 参数

请求参数名与形参对象属性名相同，按照对象层次结构关系即可接收嵌套POJO属性参数

4. 数组参数

请求参数名与形参对象属性名相同且请求参数为多个，定义数组类型即可接收参数

5. 集合参数

请求参数名与形参集合对象名相同且请求参数为多个，@RequestParam绑定参数关系

6. json 数组参数

7. 添加json数据转换相关坐标

```
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.0</version>
</dependency>
```

2. 开启自动转换json数据的支持

```
@Configuration
@ComponentScan("com.itheima.controller")
//开启json数据类型自动转换
@EnableWebMvc
public class SpringMvcConfig {
}
```

```
//集合参数：json格式
//1.开启json数据格式的自动转换，在配置类中开启@EnableWebMvc
//2.使用@RequestBody注解将外部传递的json数组数据映射到形参的集合对象中作为数据
@RequestMapping("/listParamForJson")
@ResponseBody
public String listParamForJson(@RequestBody List<String> likes){
    System.out.println("list common(json)参数传递 list ==> "+likes);
    return "{'module':'list common for json param'}";
}
```

- @EnableWebMvc：开启SpringMVC多项辅助功能
- @RequestBody：将请求中请求体所包含的数据传递给请求参数，此注解一个处理器方法只能使用一次

7. json 对象

json数据与形参对象属性名相同，定义POJO类型形参即可接收参数

```
//POJO参数：json格式
//1.开启json数据格式的自动转换，在配置类中开启@EnableWebMvc
//2.使用@RequestBody注解将外部传递的json数据映射到形参的实体类对象中，要求属性名称一一对应
@RequestMapping("/pojoParamForJson")
@ResponseBody
public String pojoParamForJson(@RequestBody User user){
    System.out.println("pojo(json)参数传递 user ==> "+user);
    return "{'module':'pojo for json param'}";
}
```

8. json 数组对象

json数组数据与集合泛型属性名相同，定义List类型形参即可接收参数

```
//集合参数：json格式
//1.开启json数据格式的自动转换，在配置类中开启@EnableWebMvc
//2.使用@RequestBody注解将外部传递的json数组数据映射到形参的保存实体类对象的集合对象中，要求属性名称一一对应
@RequestMapping("/listPojoParamForJson")
@ResponseBody
public String listPojoParamForJson(@RequestBody List<User> list){
    System.out.println("list pojo(json)参数传递 list ==> "+list);
    return "{'module':'list pojo for json param'}";
}
```

9. 日期类型

根据不同的日期格式设置不同的接收方式

```
//日期参数 http://localhost:80/dataParam?date=2088/08/08&date1=2088-08-18&date2=2088/08/28 8:08:08
//使用@DateTimeFormat注解设置日期类型数据格式，默认格式yyyy/MM/dd
@RequestMapping("/dataParam")
@ResponseBody
public String dataParam(Date date,
                  @DateTimeFormat(pattern="yyyy-MM-dd") Date date1,
                  @DateTimeFormat(pattern="yyyy/MM/dd HH:mm:ss") Date date2){
    System.out.println("参数传递 date ==> "+date);
    System.out.println("参数传递 date1(yyyy-MM-dd) ==> "+date1);
    System.out.println("参数传递 date2(yyyy/MM/dd HH:mm:ss) ==> "+date2);
    return "{'module':'data param'}";
}
```

@DataTimeFormat：设定日期时间型数据格式，属性pattern用来指定日期时间格式字符串

内部依赖Converter接口

传递日期类型参数必须在配置类上使用@EnableWebMvc注解。其功能之一：根据类型匹配对应的类型转换器。

## 3.3 响应

1. 响应页面

```
@Controller
public class UserController {

    //响应页面/跳转页面
    //返回值为String类型，设置返回值为页面名称，即可实现页面跳转
    @RequestMapping("/toJumpPage")
    public String toJumpPage(){
        System.out.println("跳转页面");
        return "page.jsp";
    }
}
```

2. 响应文本

```
//响应文本数据
//返回值为String类型，设置返回值为任意字符串信息，即可实现返回指定字符串信息，需要依赖@ResponseBody注解
@RequestMapping("/toText")
@ResponseBody
public String toText(){
    System.out.println("返回纯文本数据");
    return "response text";
}
```

3. 响应 json

```
//响应POJO对象
//返回值为实体类对象，设置返回值为实体类类型，即可实现返回对应对象的json数据，需要依赖@ResponseBody注解和@EnableWebMvc注解
@RequestMapping("/toJsonPOJO")
@ResponseBody
public User toJsonPOJO(){
    System.out.println("返回json对象数据");
    User user = new User();
    user.setName("itcast");
    user.setAge(15);
    return user;
}
```

需要添加jackson-databind依赖以及在SpringMvcConfig配置类上添加@EnableWebMvc注解

4. 响应 json 集合

```
//响应POJO集合对象
//返回值为集合对象，设置返回值为集合类型，即可实现返回对应集合的json数组数据，需要依赖@ResponseBody注解和@EnableWebMvc注解
@RequestMapping("/toJsonList")
@ResponseBody
public List<User> toJsonList(){
    System.out.println("返回json集合数据");
    User user1 = new User();
    user1.setName("传智播客");
    user1.setAge(15);

    User user2 = new User();
    user2.setName("黑马程序员");
    user2.setAge(12);

    List<User> userList = new ArrayList<User>();
    userList.add(user1);
    userList.add(user2);

    return userList;
}
```

# 4.Rest风格

## 4.1 Restful 简介

REST 是 REpresentational State Transfer 的缩写。这个词组的翻译过来就是“表现层状态转化”。

实际上 REST 的全称是 Resource Representational State Transfer ，直白地翻译过来就是 “资源”在网络传输中以某种“表现形式”进行“状态转换”

意思是通过浏览器发送的动作以及url，实现服务端对象数据等资源，向具体表现形式如json、txt、xml等状态进行转换。

这也是为什么前面使用@ResponseBody以及@RequestBody在对象和json之间进行转换的原因。

```
GET    /classes：列出所有班级
POST   /classes：新建一个班级
GET    /classes/{classId}：获取某个指定班级的信息
PUT    /classes/{classId}：更新某个指定班级的信息（一般倾向整体更新）
PATCH  /classes/{classId}：更新某个指定班级的信息（一般倾向部分更新）
DELETE /classes/{classId}：删除某个班级
GET    /classes/{classId}/teachers：列出某个指定班级的所有老师的信息
GET    /classes/{classId}/students：列出某个指定班级的所有学生的信息
DELETE /classes/{classId}/teachers/{ID}：删除某个指定班级下的指定的老师的信息
```

## 4.2 入门案例

```
@RestController   //使用@RestController注解替换@Controller与@ResponseBody注解，简化书写
@RequestMapping("/books")
public class BookController {

	// @RequestMapping( method = RequestMethod.POST)
    @PostMapping//使用@PostMapping简化Post请求方法对应的映射配置
    public String save(@RequestBody Book book){
        System.out.println("book save..." + book);
        return "{'module':'book save'}";
    }

	// @RequestMapping(value = "/{id}" ,method = RequestMethod.DELETE)
    @DeleteMapping("/{id}")  //使用@DeleteMapping简化DELETE请求方法对应的映射配置
    public String delete(@PathVariable Integer id){
        System.out.println("book delete..." + id);
        return "{'module':'book delete'}";
    }

	// @RequestMapping(method = RequestMethod.PUT)
    @PutMapping   //使用@PutMapping简化Put请求方法对应的映射配置
    public String update(@RequestBody Book book){
        System.out.println("book update..."+book);
        return "{'module':'book update'}";
    }

	// @RequestMapping(value = "/{id}" ,method = RequestMethod.GET)
    @GetMapping("/{id}")    //使用@GetMapping简化GET请求方法对应的映射配置
    public String getById(@PathVariable Integer id){
        System.out.println("book getById..."+id);
        return "{'module':'book getById'}";
    }

	// @RequestMapping(method = RequestMethod.GET)
    @GetMapping      //使用@GetMapping简化GET请求方法对应的映射配置
    public String getAll(){
        System.out.println("book getAll...");
        return "{'module':'book getAll'}";
    }
}
```

`@PathVariable`：绑定路径参数与处理器方法形参间的关系，要求路径参数名与形参名一一对应，如果不对应，可以在注解上注定名称。

# 5. SSM整合

如何保证工厂的唯一以及共用？

  

我们知道Spring工厂是一个重量级资源，一个工程最好创建一次，所以就涉及到工厂对象的共用问题。

  

- 共用：我们可以把工厂对象放入Web的域对象中，在request、Session、ServletContext(application)三个域对象中，因为application代表着工程，整个工程都有效，所以我们将工厂对象放入application域中。（ServletContext.setAttribute("xxx"，context)）
- 唯一：在一个工程中，ServletContext对象只会创建一次，所以我们可以将ServletContext和Spring工厂关联起来，将spring工厂的创建放入监听ServletContext的监听器对象ServletContextListener中。只要ServletContext一创建，ServletContextListener就会调用Spring工厂的创建方法。

  

Spring将执行上述思路的代码封装进了ContextLoaderListener中。

  

ContextLoaderListener的配置：

  

```
<listener>
	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
<!--设置配置文件-->
<context-param>
	<param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
</context-param>
```

## 5.1 依赖

```
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.10.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.10.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>5.2.10.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.6</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>1.3.0</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.47</version>
    </dependency>

    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.1.16</version>
    </dependency>

    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>

    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>

    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.9.0</version>
    </dependency>
```

## 5.2 Spring 整合 Mybatis

1. jdbc.properties属性文件

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm_db
jdbc.username=root
jdbc.password=root
```

2. JdbcConfig配置类

```
public class JdbcConfig {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;
	//配置连接池
    @Bean
    public DataSource dataSource(){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName(driver);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        return dataSource;
    }
	//Spring事务管理需要的平台事务管理器对象
    @Bean
    public PlatformTransactionManager transactionManager(DataSource dataSource){
        DataSourceTransactionManager ds = new DataSourceTransactionManager();
        ds.setDataSource(dataSource);
        return ds;
    }
}
```

3. MyBatis配置类

```
public class MyBatisConfig {
    @Bean
    public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource){
        SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
        factoryBean.setDataSource(dataSource);
        factoryBean.setTypeAliasesPackage("com.itheima.domain");
        return factoryBean;
    }

    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
        MapperScannerConfigurer msc = new MapperScannerConfigurer();
        msc.setBasePackage("com.itheima.dao");
        return msc;
    }
}
```

4. SpringConfig配置类

```
@Configuration
@ComponentScan({"com.itheima.service"})
@PropertySource("classpath:jdbc.properties")
@Import({JdbcConfig.class,MyBatisConfig.class})
@EnableTransactionManagement //开启Spring事务管理
public class SpringConfig {
}
```

## 5.3 Spring整合SpringMVC

1. SpringMVCConfig配置类

```
@Configuration
@ComponentScan("com.itheima.controller")
@EnableWebMvc
public class SpringMvcConfig {
}
```

2. ServletConfig配置类，加载SpringMvcConfig和SpringConfig配置类

```
public class ServletConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }

    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }

    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```

# 7. 异常处理器

## 7.1 异常处理器简介

后端项目在运行中可能出现各种情况的异常，框架、数据层、业务层、表现层、工具类等等都可能抛出异常。

一旦前端发出请求，在执行逻辑过程中发生异常，我们不可能将这些异常直接返回给前端，这样用户也不知道哪里错了，影响用户体验。

所以我们将这些各种各样的异常全部往上层抛出，并使用异常处理器将这些异常捕获，并做出相应逻辑处理，统一对这些异常进行管理。

## 7.2 入门案例

```
@RestControllerAdvice  //用于标识当前类为REST风格对应的异常处理器
public class ProjectExceptionAdvice {

    //统一处理所有的Exception异常
    @ExceptionHandler(Exception.class)
    public Result doOtherException(Exception ex){
        return new Result(666,null);
    }
}
```

- @RestControllerAdvice：为Rest风格开发的控制器类做增强，自带@ResponseBody注解与@Component注解
- @ExceptionHandler：设置指定异常的处理方案，功能等同于控制器方法，出现异常后终止原始控制器执行，并转入当前方法执行

# 8. 拦截器

## 8.1 拦截器简介

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679319807280-d6e1959e-0ff1-42d6-af4a-7eb1a74ecdb1.png)

拦截器（Interceptor）是一种动态拦截方法调用的机制，在 SpringMVC 中动态拦截控制器方法的执行

在指定的方法调用前后执行预先设定的代码，或者阻止原始方法的运行。

底层实现机制是利用了 AOP 思想。

## 8.2 拦截器和过滤器

Spring 代码运行在 Servlet 引擎中，用户请求首先要经过 Web 容器然后才能到 SpringMVC。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679320121854-89fd3629-b581-45ba-bc3e-f25364eb9939.png)

- Filter 属于 Servlet 技术，Interceptor 属于 SpringMVC 技术
- Filter 对所有访问进行增强，Interceptor 仅针对 SpringMVC 的访问进行增强

## 8.3 入门案例

### 8.3.1 实现步骤

1. 定义拦截器

```
@Component //注意当前类必须受Spring容器控制
//定义拦截器类，实现HandlerInterceptor接口
public class ProjectInterceptor implements HandlerInterceptor {
    @Override
    //原始方法调用前执行的内容
    //返回值类型可以拦截控制器的执行，true放行，false终止
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle..."+contentType);
        return true;
    }

    @Override
    //原始方法调用后执行的内容
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle...");
    }

    @Override
    //原始方法调用完成后执行的内容
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion...");
    }
}
```

2. 加载拦截器

方法一：

```
@Configuration
public class SpringMvcSupport extends WebMvcConfigurationSupport {
    @Autowired
    private ProjectInterceptor projectInterceptor;

    @Override
    protected void addInterceptors(InterceptorRegistry registry) {
        //配置拦截器
        registry.addInterceptor(projectInterceptor)
            .addPathPatterns("/books","/books/*");
    }
}
```

方法二：

```
@Configuration
@ComponentScan({"com.itheima.controller"})
@EnableWebMvc
//实现WebMvcConfigurer接口可以简化开发，但具有一定的侵入性
public class SpringMvcConfig implements WebMvcConfigurer {
    @Autowired
    private ProjectInterceptor projectInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //配置多拦截器
        registry.addInterceptor(projectInterceptor)
            .addPathPatterns("/books","/books/*");
    }
}
```

### 8.3.2 流程分析

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679320708889-3beaec78-00c5-466f-b5f1-6dcd7e652b9c.png)

## 8.4 拦截器参数

### 8.4.1 前置处理

```
//原始方法调用前执行的内容
//返回值类型可以拦截控制的执行，true放行，false终止
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    System.out.println("preHandle..."+contentType);
    return true;
}
```

- request：请求对象
- response：响应对象
- handler：被调用的控制器方法，本质上是一个方法对象，对反射技术中的Method对象进行了再包装

### 8.4.2 后置处理

```
//原始方法调用后执行的内容
public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
    System.out.println("postHandle...");
}
```

- modelAndView：如果处理器执行完成具有返回结果，可以读取到对应数据与页面信息，并进行跳转

### 8.4.3 完成后处理

```
//原始方法调用完成后执行的内容
public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
    System.out.println("afterCompletion...");
}
```

- ex:如果处理器执行过程中出现异常对象，可以针对异常情况进行单独处理

注意：无论处理器方法内部是否出现异常，该方法都会执行。

## 8.5 拦截器链配置

### 8.5.1 实现步骤

1. 定义第二个拦截器

```
@Component
public class ProjectInterceptor2 implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle...222");
        return false;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle...222");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion...222");
    }
}
```

2. 配置第二个拦截器

```
@Configuration
@ComponentScan({"com.itheima.controller"})
@EnableWebMvc
//实现WebMvcConfigurer接口可以简化开发，但具有一定的侵入性
public class SpringMvcConfig implements WebMvcConfigurer {
    @Autowired
    private ProjectInterceptor projectInterceptor;
    @Autowired
    private ProjectInterceptor2 projectInterceptor2;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //配置多拦截器
        registry.addInterceptor(projectInterceptor)
            .addPathPatterns("/books","/books/*");
        registry.addInterceptor(projectInterceptor2)
            .addPathPatterns("/books","/books/*");
    }
}
```

### 8.5.2 流程分析

- 当配置多个拦截器时，形成拦截器链
- 拦截器链的运行顺序参照拦截器添加顺序为准
- 当拦截器中出现对原始处理器的拦截，后面的拦截器均终止运行
- 当拦截器运行中断，仅运行配置在前面的拦截器的afterCompletion操作

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679321367239-736bbb91-cdf4-4f4b-9625-bc9dfed30353.png)

# 9. 静态资源放行

```
@Configuration
public class SpringMvcSupport extends WebMvcConfigurationSupport {
    //设置静态资源访问过滤，当前类需要设置为配置类，并被扫描加载
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        //当访问/pages/????时候，从/pages目录下查找内容
        registry.addResourceHandler("/pages/**")
            .addResourceLocations("/pages/");
        registry.addResourceHandler("/js/**")
            .addResourceLocations("/js/");        		
        registry.addResourceHandler("/css/**")
            .addResourceLocations("/css/");       
        registry.addResourceHandler("/plugins/**")
            .addResourceLocations("/plugins/");
    }
}
```

# 10. 文件上传与下载

## 10.1 文件上传

1. 前端准备

文件上传对前端 form 表单有要求

```
<form method="post" action="/common/upload" enctype="multipart/form-data">
    <input name="myFile" type="file"  />
    <input type="submit" value="提交" /> 
</form>
```

- method：选择使用 Post 方式进行提交
- enctype：采用 multipart 格式上传文件
- type：使用input的file控件上传

现在一些前端组件库使用的文件上传组件，底层还是基于 form 表单的提交

2. 后端准备

服务端要接收客户端页面上传的文件，通常都会使用 Apache 的两个组件：

- commons-fileupload
- commons-io

spring-web包对文件上传功能进行了封装，只需要在 Controller 控制器方法上声明一个 MultipartFile 类型的形参即可接收上传的文件。

```
/**
 * 文件上传
 * @param file
 * @return
 */
@PostMapping("/upload")
public R<String> upload(MultipartFile file){
    System.out.println(file);
    return R.success(fileName);
}
```

## 10.2 文件下载

文件下载分为两种格式：

1. 以附件形式下载，将文件保存在指定磁盘中
2. 直接在浏览器中打开（上传后需要展示的图片等）






![[SpringBoot.png]]
# 概述
---
## 简介
---
- SpringBoot 是基于 Spring 框架的开源 Java 开发框架，旨在简化企业级应用的开发与部署。
- 它通过“约定优于配置”的理念，减少繁琐的配置工作，帮助开发者快速构建生产就绪的应用程序。
- 核心特性：
	- **自动配置**：根据项目依赖自动配置 Spring 组件，减少手动设置。
	- **起步依赖**：提供 spring-boot-starter 模块，简化依赖管理。
	- **嵌入式服务器**：内置 Tomcat、Jetty 等服务器，可直接运行 JAR 包。
	- **生产就绪**：通过 Actuator 提供监控、健康检查等功能。
	- **开发效率**：支持 DevTools 热重启、CLI 快速原型开发。
- 主要用途：
	- Spring Boot 广泛用于构建 RESTful API、微服务、Web 应用和批处理任务，适用于从小型项目到大型分布式系统。
- 优势：
	- 快速开发：减少样板代码，专注业务逻辑。
	- 易于部署：支持容器化（如 Docker）和云平台。
	- 生态丰富：无缝集成 Spring Security、Spring Data、Spring Cloud 等。
## 需求
---
Spring Boot 的出现是为了解决传统 Spring 框架开发中遇到的复杂性和繁琐配置问题，同时满足现代应用开发对快速、简洁和生产就绪的需求。
- **简化 Spring 配置**
	传统 Spring 框架需要大量的 XML 或 Java 配置（如 Bean 定义、MVC 设置、数据源配置），配置复杂且容易出错。Spring Boot 通过自动配置（Auto-Configuration）根据项目依赖智能设置默认配置，极大减少手动配置工作。
- **加速开发效率**
	开发企业级应用时，开发者常需手动整合各种库（如 Hibernate、Spring MVC）和依赖版本。Spring Boot 提供起步依赖（Starter Dependencies），如 spring-boot-starter-web，打包了常用库，简化依赖管理，开发者可专注于业务逻辑。
- **简化部署**
	传统 Java 应用通常需要部署到外部应用服务器（如 Tomcat、WebSphere），配置复杂且不利于快速迭代。Spring Boot 内置嵌入式服务器（如 Tomcat、Jetty），支持将应用打包为可执行 JAR 文件，运行简单，适合现代微服务和容器化部署（如 Docker）。
- **生产就绪特性**
	现代应用需要监控、健康检查和指标收集等功能。Spring Boot 的 Actuator 模块提供开箱即用的生产级功能（如 `/actuator/health`、`/actuator/metrics`），无需额外开发。
- **适应微服务架构**
	微服务架构要求应用轻量、独立且易于扩展。Spring Boot 通过简化配置、支持云原生（Spring Cloud 集成）和快速启动特性，成为微服务开发的理想选择。
## 对比
---

| **特性**    | **Spring Boot**    | **Quarkus**          | **Micronaut**        | **Play Framework** |
| --------- | ------------------ | -------------------- | -------------------- | ------------------ |
| **启动时间**  | 中等（秒级）             | 极快（毫秒级，Native 模式）    | 很快（接近 Quarkus）       | 中等                 |
| **内存占用**  | 较高（100MB+）         | 低（原生镜像几十 MB）         | 低（优于 Spring Boot）    | 中等                 |
| **生态系统**  | 非常成熟，Spring 生态丰富   | 快速增长，兼容部分 Spring API | 较新，扩展中               | 成熟但偏 Web 开发        |
| **云原生支持** | 优秀（Spring Cloud）   | 极佳（Kubernetes 原生）    | 优秀（内置服务发现等）          | 良好（响应式设计）          |
| **开发体验**  | 自动配置、DevTools 提升效率 | 热重载、开发者模式友好          | 简洁 API、热重载           | 热重载、响应式开发          |
| **学习曲线**  | 中等，Spring 基础易上手    | 中等，GraalVM 需学习       | 较低，API 直观            | 中等，响应式编程有门槛        |
| **适用场景**  | 企业级应用、微服务、传统项目     | 云原生、serverless、微服务   | 微服务、serverless、低资源环境 | Web 应用、响应式系统       |
| **社区支持**  | 非常强大，文档丰富          | 快速增长，文档完善            | 较新，文档完善但社区较小         | 成熟但社区活跃度低于 Spring  |

# 开始
---
## 安装
---
1. **使用 Spring Initializr**
	Spring Initializr 是一个在线工具，用于快速生成 Spring Boot 项目结构，适合大多数场景。
	打开 [start.spring.io](https:start.spring.io)，配置项目，生成一个压缩包，将解压后的文件夹导入 IDE。
2. **使用 Spring Boot CLI**
	Spring Boot CLI 是一个命令行工具，它允许开发者使用Groovy 脚本快速创建、运行和测试Spring Boot 应用程序，无需大量样板代码。
	1. 从官网下载 ZIP 文件。
	2. 将解压后的 bin 目录添加到环境变量。
	3. 验证安装。
		```bash
		spring --version
		```
	4. 创建一个 Groovy 脚本。
		```groovy
		@RestController
		class App {
		    @GetMapping("/")
		    String home() {
		        "Hello, Spring Boot!"
		    }
		}
		```
	5. 运行，访问 http://localhost:8080 查看结果。
		```bash
		spring run app.groovy
		```
3. **手动创建项目**
	1. 创建 Maven 项目。
	2. 添加 SpringBoot 父依赖，如果是微服务项目的顶级父模块，可以添加 SpringBoot 依赖管理模块。
	3. 创建主类，在项目目录外层创建主类：
		```java
		package com.example.demo;
		import org.springframework.boot.SpringApplication;
		import org.springframework.boot.autoconfigure.SpringBootApplication;
		@SpringBootApplication
		public class DemoApplication {
		    public static void main(String[] args) {
		        SpringApplication.run(DemoApplication.class, args);
		    }
		}
		```
4. **使用 IDE 插件**
	- **IntelliJ IDEA**：
	    - 选择 `File > New > Project > Spring Initializr`。
	    - 配置项目元数据和依赖，直接生成项目。
	- **Eclipse/STS（Spring Tool Suite）**：
	    - 安装 STS 插件或直接使用 Spring Tool Suite。
	    - 选择 `File > New > Spring Starter Project`，配置依赖和元数据。
	- **VS Code**：
	    - 安装 `Spring Boot Extension Pack` 插件。
	    - 使用 `Spring Initializr: Create a Maven/Gradle Project` 命令生成项目。
## 使用
---
```text
my-spring-boot-project/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── demo/
│   │   │               ├── DemoApplication.java        # 应用入口类
│   │   │               ├── controller/                 # 控制器层（REST API）
│   │   │               │   └── HelloController.java
│   │   │               ├── service/                    # 业务逻辑层
│   │   │               │   └── HelloService.java
│   │   │               ├── repository/                 # 数据访问层
│   │   │               │   └── UserRepository.java
│   │   │               ├── model/                      # 实体类/数据模型
│   │   │               │   └── User.java
│   │   │               ├── config/                     # 配置类
│   │   │               │   └── AppConfig.java
│   │   │               └── exception/                  # 自定义异常
│   │   │                   └── GlobalExceptionHandler.java
│   │   └── resources/
│   │       ├── application.properties                  # 主配置文件
│   │       ├── application-dev.yml                     # 开发环境配置文件
│   │       ├── application-prod.yml                    # 生产环境配置文件
│   │       ├── static/                                 # 静态资源（CSS、JS、图片）
│   │       │   └── css/
│   │       │       └── style.css
│   │       └── templates/                              # 模板文件（Thymeleaf、Freemarker）
│   │           └── index.html
│   └── test/
│       ├── java/
│       │   └── com/
│       │       └── example/
│       │           └── demo/
│       │               ├── DemoApplicationTests.java   # 单元/集成测试
│       │               └── controller/
│       │                   └── HelloControllerTest.java
│       └── resources/
│           └── application-test.properties             # 测试环境配置文件
├── pom.xml                                             # Maven 配置文件（或 build.gradle）
├── Dockerfile                                          # Docker 配置文件（可选）
├── README.md                                           # 项目说明
└── .gitignore                                          # Git 忽略文件
```
# 概念
---
## YAML
---
- YAML（YAML Ain’t Markup Language）是一种简洁、人类可读的数据序列化格式，常用于配置文件、数据交换等场景
### 基本特点
---
- **人类可读**：语法简洁，易于阅读和编写。
- **跨语言支持**：广泛用于多种编程语言（如Python、Java、Go等）。
- **用途**：常用于配置文件（如Docker、Kubernetes、GitHub Actions）、数据存储和交换。
- **基于缩进**：使用空格（通常2个）缩进表示层级，**禁止使用Tab**。
### 基本语法
---
1. **基本数据类型**
	- 标量（Scalar）：字符串、数字、布尔值、null等。
		```yaml
		name: Alice
		age: 25
		is_student: true
		score: 3.14
		nothing: null
		```
	- 字符串：
		- 默认不需引号，含特殊字符（如`:`、`#`）时需用单引号`'`或双引号`"`。
		- 多行字符串：
			- `|`：保留换行符。
			- `>`：折叠换行符为单空格。
				```yaml
				description: |
				  Line 1
				  Line 2
				folded: >
				  This is
				  one line
				```
2. **集合类型**
	- 列表（Sequence/List）：
		- 使用短横线`-`表示，同一层级对齐。
			```yaml
			fruits:
			  - apple
			  - banana
			  - orange
			```
	- 映射（Mapping/Dictionary）：
		- 键值对用`key: value`表示。
			```yaml
			person:
			  name: Bob
			  age: 30
			```
3. **嵌套结构**
	- 列表和映射可以任意嵌套。
		```yaml
		students:
		  - name: Alice
		    age: 20
		  - name: Bob
		    age: 22		
		```
### 高级特性
---
1. **锚点（Anchor）与引用（Alias）**
	- 用`&`定义锚点，`*`引用复用数据。
		```yaml
		defaults: &defaults
		  adapter: postgres
		  host: localhost
		
		development:
		  base: *defaults
		  database: dev_db
		```
2. **合并（Merge）**
	- 使用`<<:`合并映射。
		```yaml
		base: &base
		  a: 1
		  b: 2
		extended:
		  <<: *base
		  c: 3
		```
3. **标签（Tag）**
	- 显式指定数据类型（如`!!str`、`!!int`）。
		```yaml
		number: !!str 123  # 强制作为字符串
		```
4. **注释**
	- 使用#添加注释，仅单行有效。
		```yaml
		name: Alice # This is a comment
		```
### SpringBoot特定特性
---
- Profile 特定配置：Spring Boot 支持在单个 YAML 文件中使用 `---` 分隔符定义不同 Profile 的配置。
	```yaml
	spring:
	  profiles: default
	server:
	  port: 8080
	---
	spring:
	  profiles: prod
	server:
	  port: 9000
	```
- 占位符和属性引用：支持 `${}` 引用其他配置属性或环境变量。
	```yaml
	app:
	  name: my-app
	  description: Welcome to ${app.name}
	```
- 对于引用、合并，SnakeYAML 不会追踪动态关系，而是会展开成为静态数据。
### 注意事项
---
- **缩进敏感**：必须使用空格（通常2个），不能用Tab。
- **冒号空格**：键值对的`:`后需跟一个空格。
- **特殊字符**：如`:`、`#`、`-`等在字符串中可能需引号包裹。
- **引用合并**：引用直接复用整个锚点，合并是将锚点内容合并进来。合并只适用于映射，不能用于列表。
- **覆盖优先级**：后合并的会覆盖前合并的同名字段。本地定义的键值对（如`dev.name`、`prod.debug`）会覆盖合并进来的同名键。
- **文件扩展名**：通常为`.yaml`或`.yml`。
- **解析器兼容性**：不同解析器可能对某些高级特性支持不一致。
### 综合案例
---
```yaml
# 定义锚点
base_settings: &base_settings
  debug: true
  log_level: info
  timeout: 30s

# 锚父映射
parent: &parent
  name: localhost
  url: 127.0.0.1
  port: 8080

# 配置文件示例
app:
  name: MyApp
  version: 1.0.0
  environments:
    dev:
	  <<: *parent # 合并parent锚点
	  <<: *base_settings # 合并base_settings锚点
      name: dev-server # 覆盖parent中的name
    prod:
	  <<: *parent # 合并parent锚点
	  <<: *base_settings # 合并base_settings锚点
      debug: false # 覆盖base_settings中的debug
      url: https://api.myapp.com # 覆盖parent中的url
  databases:
    - type: postgres
      host: localhost
      port: 5432
    - type: redis
      host: localhost
      port: 6379
  description: >
    This is a sample
    configuration file.
```
## 自动配置
---
### 自动配置是什么
---
- **定义**：自动配置（Auto-Configuration）是 Spring Boot 的一种机制，通过扫描类路径中的依赖（通常来自 Starter）和运行时环境，自动注册所需的 Bean、配置属性和服务，无需开发者手动编写 XML 或 Java 配置。
- **目标**：
    - 简化配置：提供合理的默认配置，减少样板代码。
    - 条件化配置：仅在特定条件（如存在某类或属性）满足时应用配置。
    - 可定制化：允许开发者通过配置文件或自定义 Bean 覆盖默认配置。
- **典型场景**：
    - 添加 `spring-boot-starter-web` 后，Spring Boot 自动配置嵌入式 Tomcat 服务器、Spring MVC 和 Jackson（JSON 序列化）。
    - 添加 `spring-boot-starter-data-jpa` 后，自动配置数据源、EntityManager 和 JpaRepository。
### 自动配置工作原理
---
自动配置基于 Spring Framework 的条件化配置机制，主要依赖以下组件：
- **类路径扫描**：Spring Boot 扫描项目依赖（`pom.xml` 或 `build.gradle`）中的库，识别需要配置的组件。
- **条件注解**：使用 `@Conditional` 注解（如 `@ConditionalOnClass`、`@ConditionalOnProperty`）决定是否应用配置。
- **spring.factories**：位于 META-INF/spring.factories 文件中，指定自动配置类的列表。
- **Spring Boot Starter**：提供依赖和自动配置的入口。
### 工作流程
---
1. **应用启动并触发自动配置**
	- 触发点
		- Spring Boot 应用的入口是一个主类，标注 @SpringBootApplication 注解：
			```java
			import org.springframework.boot.SpringApplication;
			import org.springframework.boot.autoconfigure.SpringBootApplication;
			
			@SpringBootApplication
			public class DemoApplication {
			    public static void main(String[] args) {
			        SpringApplication.run(DemoApplication.class, args);
			    }
			}		
			```
		- `@SpringBootApplication` 是复合注解，包含：
			- `@EnableAutoConfiguration`：启用自动配置。
			- `@ComponentScan`：扫描组件（如 `@Controller`、`@Service`）。
			- `@SpringBootConfiguration`：标记为配置类（等价于 `@Configuration`）。
	- 执行过程
		- `SpringApplication.run()` 方法启动 Spring 容器，初始化 `ApplicationContext`。
		- `@EnableAutoConfiguration` 触发 `AutoConfigurationImportSelector` 类，负责加载自动配置的元数据。
	- 关键组件
		- `AutoConfigurationImportSelector`：读取 `spring.factories` 中的配置类列表。
		- `SpringApplication`：管理应用启动生命周期。
2. **加载 spring.factories**
	- **作用**：
	    - `spring.factories` 文件定义了所有自动配置类的全限定名，Spring Boot 在启动时加载这些类。
	- **文件位置**：
	    - 位于 `spring-boot-autoconfigure` 模块的 `META-INF/spring.factories`，以及自定义 Starter 的相同路径。
	    - 示例内容：
			```
			org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
			org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration,\
			org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration,\
			org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration
			```
	- **执行过程**：
	    - `AutoConfigurationImportSelector` 的 `selectImports()` 方法调用 `SpringFactoriesLoader`，从类路径中的所有 JAR 文件加载 `spring.factories`。
	    - 加载顺序：
	        1. 加载 `spring-boot-autoconfigure` 的内置配置类。
	        2. 加载项目依赖（包括自定义 Starter）的 `spring.factories`。
	    - 结果是一个配置类全限定名的列表（如 `WebMvcAutoConfiguration`）。
	- **过滤机制**：
	    - 使用 `@EnableAutoConfiguration` 的 `exclude` 或 `excludeName` 属性过滤不需要的配置：
			```java
			@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
			```
	    - 通过 `spring.autoconfigure.exclude` 属性排除：
			```java
			spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
			```
	- **关键组件**：
		- `SpringFactoriesLoader`：Spring 的工具类，加载 `spring.factories`。
		- `META-INF/spring.factories`：元数据文件。
3. **条件评估**
	- **作用**：
	    - 每个自动配置类使用条件注解（如 `@ConditionalOnClass`、`@ConditionalOnProperty`）决定是否生效。
	- **条件注解**：
	    - 常见注解及其作用：
	        - `@ConditionalOnClass`：类路径中存在指定类（如 Tomcat.class）。
	        - `@ConditionalOnMissingClass`：类路径中缺少指定类。
	        - `@ConditionalOnBean / @ConditionalOnMissingBean`：容器中存在/缺少指定 Bean。
	        - `@ConditionalOnProperty`：配置文件中存在指定属性且值匹配。
	        - `@ConditionalOnWebApplication`：当前是 Web 应用。
	    - 示例（WebMvcAutoConfiguration）：
			```java
			@Configuration
			@ConditionalOnWebApplication(type = ConditionalOnWebApplication.Type.SERVLET)
			@ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
			@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
			@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
			@AutoConfigureAfter({ DispatcherServletAutoConfiguration.class })
			public class WebMvcAutoConfiguration {
			    // 配置代码
			}
			```
	- **执行过程**：
	    - Spring Boot 遍历 `spring.factories` 中的配置类，逐一评估条件。
	    - 使用 `ConditionEvaluator` 检查注解条件：
	        - 检查类路径（通过 `Class.forName()`）。
	        - 检查容器中的 Bean（通过 `BeanFactory`）。
	        - 检查属性（通过 `Environment`）。
	    - 条件满足的配置类被加入 Spring 容器，条件不满足的被跳过。
	- **优化**：
	    - Spring Boot 使用延迟加载（Lazy Loading），部分配置在实际使用时才加载。
	    - `@AutoConfigureBefore` / `@AutoConfigureAfter` 控制配置顺序，确保依赖关系正确。
	- **关键组件**：
	    - `ConditionEvaluator`：评估条件。
	    - `ConfigurationClassParser`：解析配置类。
4. **注册 Bean 和应用配置**
	- **作用**：
	    - 条件满足的配置类通过 `@Bean` 方法注册 Spring 容器管理的对象。
	- **执行过程**：
	    - 配置类被解析为 `ConfigurationClass`，其 `@Bean` 方法被调用。
	    - 示例（`DataSourceAutoConfiguration`）：
			```java
			@Configuration
			@ConditionalOnClass({ DataSource.class })
			@ConditionalOnMissingBean(type = "javax.sql.DataSource")
			public class DataSourceAutoConfiguration {
			    @Bean
			    public DataSource dataSource() {
			        return DataSourceBuilder.create().build();
			    }
			}
			```
	    - 如果类路径中有 `DataSource` 类，且容器中无自定义 `DataSource`，则注册默认数据源。
	- **配置来源**：
		- 默认值：配置类提供合理的默认设置（如 Tomcat 默认端口 8080）。
		- 外部属性：从 `application.properties` 或 `application.yml` 读取配置：
			```
			server.port=8081
			spring.datasource.url=jdbc:mysql://localhost:3306/demo
			```
		- 使用 `@ConfigurationProperties` 绑定属性：
			```java
			@ConfigurationProperties(prefix = "spring.datasource")
			public class DataSourceProperties {
			    private String url;
			    private String username;
			    // getters and setters
			}
			```
	- **关键组件**：
		- `BeanDefinitionRegistry`：注册 Bean 定义。
		- `ConfigurationPropertiesBindingPostProcessor`：绑定配置属性。
5. **合并用户配置**
	- **作用**：
	    - 允许开发者通过自定义 Bean 或配置文件覆盖自动配置。
	- **覆盖方式**：
	    - **自定义 Bean**：
	        - 定义同类型或同名的 Bean，覆盖自动配置的默认 Bean。
	        - 示例：自定义 `ObjectMapper`：
				```java
				@Bean
				public ObjectMapper objectMapper() {
				    return new ObjectMapper().setSerializationInclusion(JsonInclude.Include.NON_NULL);
				}
				```
	        - 自动配置中的 `@ConditionalOnMissingBean` 确保用户 Bean 优先。
	    - **配置文件**：
			- 通过 `application.properties` 修改默认设置：
				```
				spring.jpa.hibernate.ddl-auto=update
				```
		- **禁用自动配置**：
			- 使用 `exclude` 或 `spring.autoconfigure.exclude` 禁用特定配置。
	- **执行过程**：
	    - Spring 容器在加载自动配置后，加载用户定义的 `@Configuration` 和 `@Bean`。
	    - 用户 Bean 的优先级高于自动配置 Bean。
	- **关键组件**：
	    - `BeanFactoryPostProcessor`：处理用户配置。
	    - `ApplicationContext`：管理 Bean 生命周期。
6. **完成容器初始化**
	- **作用**：
	    - Spring 容器完成所有 Bean 的初始化，应用启动。
	- **执行过程**：
	    - 容器刷新（`refresh()`）后，触发 `ContextRefreshedEvent`。
	    - 自动配置的组件（如嵌入式服务器）启动，例如 Tomcat 监听 8080 端口。
	    - 应用进入运行状态，处理请求或执行任务。
	- **关键组件**：
	    - `AbstractApplicationContext`：管理容器生命周期。
	    - `EmbeddedWebServerFactory`：启动嵌入式服务器。





## Starter
---
### Starter 是什么
---
- **定义**：Starter 是 Spring Boot 提供的一组便捷依赖描述符，封装了特定功能的依赖及其默认配置，统一版本管理，减少手动配置和依赖冲突。
- **核心理念**：通过引入一个 Starter 依赖，开发者可以快速集成相关技术栈（如 Web、数据库、消息队列等），无需手动添加多个相关库或配置版本。
- **命名规范**：Starter 通常以 `spring-boot-starter-*` 命名，例如 `spring-boot-starter-web`、`spring-boot-starter-data-jpa`。
### Starter 的作用
---
- **简化依赖管理**：
    - 每个 Starter 包含一组相关依赖（如框架、库、工具），并由 Spring Boot 统一管理版本，确保兼容性。
    - 开发者只需添加一个 Starter 依赖，无需逐一指定底层库（如 Spring MVC、Hibernate）的版本。
- **自动配置**：
    - Starter 配合 Spring Boot 的自动配置机制，根据类路径中的依赖自动配置 Bean 和默认设置。
    - 例如，spring-boot-starter-web 自动配置 Tomcat 服务器和 Spring MVC。
- **减少配置**：
    - 提供合理的默认配置（如端口、日志级别），开发者只需在 application.properties 或 application.yml 中覆盖必要设置。
- **一致性**：
    - 通过 `spring-boot-starter-parent` 或 Spring Boot 的 BOM（Bill of Materials），确保所有依赖版本一致，避免冲突。
### Starter 的工作原理
---
- **依赖传递**：Starter 是一个聚合依赖，包含特定功能的多个库。例如，`spring-boot-starter-web` 包括 Spring MVC、Jackson、Tomcat 等。
- **自动配置**：
    - Spring Boot 使用 `@Conditional` 注解（如 `@ConditionalOnClass`），根据类路径中是否存在特定类启用配置。
    - 例如，检测到 `spring-boot-starter-web` 中的 `Servlet` 类后，自动配置嵌入式 Tomcat。
- **配置文件**：Starter 通常提供默认配置，开发者可通过 `application.properties` 自定义（如 `server.port=8081`）。
- **版本管理**：Starter 依赖 `spring-boot-starter-parent` 或 `spring-boot-dependencies` BOM，统一管理版本。
### 常见的 Starter
---

| **Starter**                         | **功能**                                              |
| ----------------------------------- | --------------------------------------------------- |
| `spring-boot-starter`               | 核心 Starter，包含自动配置、日志、YAML 支持等基础功能                   |
| `spring-boot-starter-web`           | 用于 Web 开发，包含 Spring MVC、Tomcat、Jackson（JSON 处理）     |
| `spring-boot-starter-data-jpa`      | 用于数据库操作，包含 Hibernate、Spring Data JPA、JDBC           |
| `spring-boot-starter-jdbc`          | 提供 JDBC 支持，包含 JdbcTemplate 和数据源配置                   |
| `spring-boot-starter-security`      | 提供 Spring Security，支持认证和授权（如 JWT、OAuth2）            |
| `spring-boot-starter-test`          | 测试支持，包含 JUnit、Mockito、Spring Test                   |
| `spring-boot-starter-actuator`      | 生产就绪功能，提供监控端点（如 /actuator/health、/actuator/metrics） |
| `spring-boot-starter-data-redis`    | Redis 集成，支持缓存和键值存储                                  |
| `spring-boot-starter-amqp`          | RabbitMQ 集成，支持消息队列                                  |
| `spring-boot-starter-kafka`         | Kafka 集成，支持分布式消息系统                                  |
| `spring-boot-starter-websocket`     | WebSocket 支持，用于实时通信                                 |
| `spring-boot-starter-thymeleaf`     | Thymeleaf 模板引擎，用于服务端渲染                              |
| `spring-boot-starter-batch`         | Spring Batch 支持，用于批量处理                              |
| `spring-boot-starter-mail`          | 邮件发送支持，集成 JavaMail                                  |
| `spring-boot-starter-oauth2-client` | OAuth2 客户端支持，用于 SSO 和第三方认证                          |
### 使用 Starter
---
- 添加依赖：
	- 在 Maven 的 `pom.xml` 中添加 Starter：
		```xml
		<dependency>
		    <groupId>org.springframework.boot</groupId>
		    <artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		```
	- 或在 Gradle 的 `build.gradle` 中：
		```gradle
		implementation 'org.springframework.boot:spring-boot-starter-web'
		```
- 版本管理：
	- 使用 `spring-boot-starter-parent` 自动管理版本：
		```xml
		<parent>
		    <groupId>org.springframework.boot</groupId>
		    <artifactId>spring-boot-starter-parent</artifactId>
		    <version>3.3.4</version>
		</parent>
		```
	- 或使用 BOM：
		```xml
		<dependencyManagement>
		    <dependencies>
		        <dependency>
		            <groupId>org.springframework.boot</groupId>
		            <artifactId>spring-boot-dependencies</artifactId>
		            <version>3.3.4</version>
		            <type>pom</type>
		            <scope>import</scope>
		        </dependency>
		    </dependencies>
		</dependencyManagement>
		```
### 自定义 Starter
---
#### 结构
---
一个完整的自定义 Starter 通常包含以下部分：
- **依赖模块**：定义 Starter 依赖，聚合所需库。
- **自动配置模块**：实现自动配置逻辑，包含配置类和条件注解。
- **元数据**：配置 spring.factories 文件，注册自动配置类。
- **可选文档**：提供使用说明和配置文件。
通常，Starter 项目分为两个模块：
- **autoconfigure 模块**：包含自动配置逻辑。
- **starter 模块**：定义依赖，依赖 autoconfigure 模块。
#### 步骤
---
1. **创建项目结构**
	创建一个 Maven 多模块项目，结构如下：
	```text
	my-custom-starter/
	├── my-custom-autoconfigure/
	│   ├── src/
	│   │   ├── main/
	│   │   │   ├── java/
	│   │   │   │   └── com/
	│   │   │   │       └── example/
	│   │   │   │           └── custom/
	│   │   │   │               ├── MyCustomService.java            # 服务类
	│   │   │   │               ├── MyCustomProperties.java         # 配置属性类
	│   │   │   │               └── MyCustomAutoConfiguration.java  # 自动配置类
	│   │   │   └── resources/
	│   │   │       └── META-INF/
	│   │   │           └── spring/
	│   │   │               └── spring.factories                    # 自动配置注册
	│   │   └── pom.xml                                             # autoconfigure 模块的 POM
	├── my-custom-starter/
	│   ├── src/
	│   │   ├── main/
	│   │   │   └── resources/
	│   │   └── pom.xml                                             # starter 模块的 POM
	├── pom.xml                                                     # 父 POM 文件
	└── README.md                                                   # 使用说明	
	```
2. **配置父 POM 文件**
	在根目录创建 pom.xml，定义多模块项目并继承 Spring Boot：
	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<project xmlns="http://maven.apache.org/POM/4.0.0"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	    <modelVersion>4.0.0</modelVersion>
	    <groupId>com.example</groupId>
	    <artifactId>my-custom-starter</artifactId>
	    <version>1.0.0-SNAPSHOT</version>
	    <packaging>pom</packaging>
	    <name>My Custom Starter</name>
	    <description>Parent POM for custom Spring Boot starter</description>
	
	    <modules>
	        <module>my-custom-autoconfigure</module>
	        <module>my-custom-starter</module>
	    </modules>
	
	    <properties>
	        <java.version>17</java.version>
	        <spring-boot.version>3.3.4</spring-boot.version>
	    </properties>
	
	    <dependencyManagement>
	        <dependencies>
	            <dependency>
	                <groupId>org.springframework.boot</groupId>
	                <artifactId>spring-boot-dependencies</artifactId>
	                <version>${spring-boot.version}</version>
	                <type>pom</type>
	                <scope>import</scope>
	            </dependency>
	        </dependencies>
	    </dependencyManagement>
	
	    <build>
	        <plugins>           
	            <plugin>
	                <groupId>org.apache.maven.plugins</groupId>
	                <artifactId>maven-compiler-plugin</artifactId>
	                <version>3.8.1</version>
	                <configuration>
	                    <source>${java.version}</source>
	                    <target>${java.version}</target>
	                </configuration>
	            </plugin>
	        </plugins>
	    </build>
	</project>	
	```
3. **创建 autoconfigure 模块**
	此模块包含自动配置逻辑、服务类和配置属性。
	1. 创建配置属性类：
		定义可通过 `application.properties` 配置的属性。
		```java
		package com.example.custom;
		
		import org.springframework.boot.context.properties.ConfigurationProperties;
		
		@ConfigurationProperties(prefix = "my.custom")
		public class MyCustomProperties {
		    private String message = "Default Message";
		
		    public String getMessage() {
		        return message;
		    }
		
		    public void setMessage(String message) {
		        this.message = message;
		    }
		}
		```
	2. 创建服务类：
		定义一个简单的服务类，供应用使用。
		```java
		package com.example.custom;
		
		public class MyCustomService {
		    private final MyCustomProperties properties;
		
		    public MyCustomService(MyCustomProperties properties) {
		        this.properties = properties;
		    }
		
		    public String getMessage() {
		        return properties.getMessage();
		    }
		}		
		```
	3. 创建自动配置类：
		使用 `@Configuration` 和条件注解定义自动配置。
		```java
		package com.example.custom;
		
		import org.springframework.boot.autoconfigure.condition.ConditionalOnClass;
		import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
		import org.springframework.boot.context.properties.EnableConfigurationProperties;
		import org.springframework.context.annotation.Bean;
		import org.springframework.context.annotation.Configuration;
		
		@Configuration
		@ConditionalOnClass(MyCustomService.class)
		@EnableConfigurationProperties(MyCustomProperties.class)
		public class MyCustomAutoConfiguration {
		    @Bean
		    @ConditionalOnMissingBean
		    public MyCustomService myCustomService(MyCustomProperties properties) {
		        return new MyCustomService(properties);
		    }
		}
		```
	4. 注册自动配置：
		在 `my-custom-autoconfigure/src/main/resources/META-INF/spring/spring.factories` 中注册配置类：
		```
		org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
		com.example.custom.MyCustomAutoConfiguration
		```
	5. 配置 autoconfigure 模块的 POM：
		在 `my-custom-autoconfigure/pom.xml` 中添加依赖：
		```xml
		<?xml version="1.0" encoding="UTF-8"?>
		<project xmlns="http://maven.apache.org/POM/4.0.0"
				 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
				 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
			<modelVersion>4.0.0</modelVersion>
			<parent>
				<groupId>com.example</groupId>
				<artifactId>my-custom-starter</artifactId>
				<version>1.0.0-SNAPSHOT</version>
			</parent>
			<artifactId>my-custom-autoconfigure</artifactId>
			<name>My Custom Autoconfigure</name>
		
			<dependencies>
				<dependency>
					Ascending
					<groupId>org.springframework.boot</groupId>
					<artifactId>spring-boot-starter</artifactId>
				</dependency>
			</dependencies>
		</project>		
		```
4. **创建 starter 模块**
	此模块仅定义依赖，依赖 autoconfigure 模块。
	在 `my-custom-starter/pom.xml` 中配置：
	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<project xmlns="http://maven.apache.org/POM/4.0.0"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	    <modelVersion>4.0.0</modelVersion>
	    <parent>
	        <groupId>com.example</groupId>
	        <artifactId>my-custom-starter</artifactId>
	        <version>1.0.0-SNAPSHOT</version>
	    </parent>
	    <artifactId>my-custom-starter</artifactId>
	    <name>My Custom Starter</name>
	
	    <dependencies>
	        <dependency>
	            <groupId>com.example</groupId>
	            <artifactId>my-custom-autoconfigure</artifactId>
	            <version>1.0.0-SNAPSHOT</version>
	        </dependency>
	    </dependencies>
	</project>	
	```
5. **打包和发布**
	- 在根目录运行：
		```bash
		mvn clean install
		```
	- 发布到 Maven 仓库（本地或远程）：
		```bash
		mvn deploy
		```
6. **使用自定义 Starter**
	在其他 Spring Boot 项目中引入 Starter：
	```xml
	<dependency>
	    <groupId>com.example</groupId>
	    <artifactId>my-custom-starter</artifactId>
	    <version>1.0.0-SNAPSHOT</version>
	</dependency>	
	```
	在 application.properties 中配置属性：
	```
	my.custom.message=Hello from Custom Starter!	
	```
	创建测试控制器：
	```java
	package com.example.demo;
	
	import com.example.custom.MyCustomService;
	import org.springframework.web.bind.annotation.GetMapping;
	import org.springframework.web.bind.annotation.RestController;
	
	@RestController
	public class TestController {
	    private final MyCustomService myCustomService;
	
	    public TestController(MyCustomService myCustomService) {
	        this.myCustomService = myCustomService;
	    }
	
	    @GetMapping("/test")
	    public String test() {
	        return myCustomService.getMessage();
	    }
	}	
	```


# 配置
---
## 配置文件
---
### 优先级
---
SpringBoot 的配置包括配置文件、配置类、命令行参数和环境变量，它们的优先级顺序是：
1. **命令行参数**：通过`--key=value`的方式传递给 Spring Boot 应用的命令行参数。
2. **Spring `@Configuration` 类**：在`@Configuration`类中的 @Bean 配置或者`@PropertySource`注解中引用的外部配置文件。
3. **`application.properties` / `application.yml` 文件**：
	- 默认的配置文件位置：`src/main/resources/application.properties` 或 `src/main/resources/application.yml`。
	- 如果有多个`application.properties`或`application.yml`文件，Spring Boot会按以下顺序加载：
	    - `application.properties` 在 `src/main/resources` 目录下的主配置文件。
	    - 各种配置文件的位置：`config/application.properties` 或 `config/application.yml`（在`classes`目录下）。
	    - Profile-specific properties 文件，如：`application-dev.properties`、`application-prod.properties` 等。
	    - 配置文件也可以通过`spring.config.location`或`spring.config.name`来覆盖。
4. **默认的配置值**：Spring Boot自动提供的默认配置。
配置文件中的值会在配置类定义之前被使用，但如果配置类中显式地定义了相同的配置项，则配置类的值会优先。


### 多环境配置


## 集成外部框架
### 集成Mybatis
1. **添加依赖**
	- 在`pom.xml`中添加 Spring Boot 和 MyBatis 相关依赖。
	- 示例：
		```xml
		<dependencies>
		    <!-- Spring Boot Starter Web -->
		    <dependency>
		        <groupId>org.springframework.boot</groupId>
		        <artifactId>spring-boot-starter-web</artifactId>
		    </dependency>
		    
		    <!-- MyBatis Spring Boot Starter -->
		    <dependency>
		        <groupId>org.mybatis.spring.boot</groupId>
		        <artifactId>mybatis-spring-boot-starter</artifactId>
		        <version>3.0.3</version>
		    </dependency>
		    
		    <!-- 数据库驱动，以MySQL为例 -->
		    <dependency>
		        <groupId>mysql</groupId>
		        <artifactId>mysql-connector-java</artifactId>
		        <scope>runtime</scope>
		    </dependency>
		</dependencies>
		```
2. **配置数据源**
	- 在`application.yml`或`application.properties`中配置数据库连接信息。
	- 示例：
		```yaml
		spring:
		  datasource:
		    url: jdbc:mysql://localhost:3306/your_database?useSSL=false&serverTimezone=UTC
		    username: your_username
		    password: your_password
		    driver-class-name: com.mysql.cj.jdbc.Driver
		
		mybatis:
		  # 指定mapper xml文件位置
		  mapper-locations: classpath:mapper/*.xml
		  # 指定实体类包路径
		  type-aliases-package: com.example.entity
		  configuration:
		    # 开启驼峰命名转换
		    map-underscore-to-camel-case: true
		    # 打印SQL日志
		    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
		    # 开启二级缓存
		    cache-enabled: true
		    # 延迟加载
		    lazy-loading-enabled: true
		```
3. **创建实体类**
	- 创建实体类，对应数据库表。
	- 示例：
		```java
		public class User {
		    private Long id;
		    private String username;
		    private String email;
		    private Date createTime;
		    
		    // getter和setter方法...
		}
		```
4. **创建Mapper接口**
	- 定义MyBatis的Mapper接口，使用注解或XML映射。
	- 示例：
		```java
		@Mapper
		public interface UserMapper {
		    
		    @Select("SELECT * FROM user WHERE id = #{id}")
		    User findById(Long id);
		    
		    @Insert("INSERT INTO user(username, email, create_time) VALUES(#{username}, #{email}, #{createTime})")
		    @Options(useGeneratedKeys = true, keyProperty = "id")
		    int insert(User user);
		    
		    @Update("UPDATE user SET username = #{username}, email = #{email} WHERE id = #{id}")
		    int update(User user);
		    
		    @Delete("DELETE FROM user WHERE id = #{id}")
		    int deleteById(Long id);
		}
		```
5. **创建XML映射文件**
	- 复杂的查询方式，使用XML映射文件，在`src/main/resources/mapper/*Mapper.xml`中定义SQL。
	- 示例：
		```xml
		<?xml version="1.0" encoding="UTF-8"?>
		<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
		    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
		
		<mapper namespace="com.example.mapper.UserMapper">
		    
		    <select id="findById" parameterType="long" resultType="User">
		        SELECT * FROM user WHERE id = #{id}
		    </select>
		    
		    <select id="findAll" resultType="User">
		        SELECT * FROM user
		    </select>
		    
		    <insert id="insert" parameterType="User" useGeneratedKeys="true" keyProperty="id">
		        INSERT INTO user(username, email, create_time) 
		        VALUES(#{username}, #{email}, #{createTime})
		    </insert>
		    
		    <update id="update" parameterType="User">
		        UPDATE user SET username = #{username}, email = #{email} 
		        WHERE id = #{id}
		    </update>
		    
		    <delete id="deleteById" parameterType="long">
		        DELETE FROM user WHERE id = #{id}
		    </delete>
		    
		</mapper>
		```
6. **在启动类上添加扫描注解**
	- 在Spring Boot主类上添加`@MapperScan`注解，指定Mapper接口的包路径。
	- 注意：
		- 在 Mapper 接口上使用 `@Mapper` 注解后，会被 SpringBoot 扫描并注册，可以不使用 `@MapperScan` 注解。
		- 大型项目中推荐使用 `@MapperScan` 提高配置的灵活性和可维护性。
	- 示例：
		```java
		@SpringBootApplication
		@MapperScan("com.example.mapper") // 扫描Mapper接口
		public class Application {
		    public static void main(String[] args) {
		        SpringApplication.run(Application.class, args);
		    }
		}
		```




# 思考




# 附录








# 2. 入门案例

1. 创建 SpringBoot 项目

有三种创建方式

- [https://start.spring.io](https://start.spring.io)
- [https://start.aliyun.com](https://start.aliyun.com)
- [https://start.springboot.io/](https://start.springboot.io/)
- 先创建 Maven 项目，导入 Boot 依赖（parent、打包插件），编写主启动类

三种创建方式创建出来的项目中 Pom 文件有所区别：

![|920x440](https://cdn.nlark.com/yuque/0/2022/png/29046015/1670577612981-a3413946-98a0-4b78-86ce-494a61eafd4c.png)

需要注意的是 SpringBoot 3.x 需要 JDK17以上，否则运行项目时会报错

2. 编写控制器类

```
@RestController
@RequestMapping("/controller")
public class TestController {
    @GetMapping("/hello")
    public String hello(){
        return "Hello";
    }
}
```

3. 测试

启动主启动类之后，浏览器输入 localhost:8080/controller/hello 将在浏览器上输出 hello

因为 SpringBoot 集成了 Tomcat ，所以不再需要将项目打成 war 包，在 Tomcat 中运行，而是只需要启动主启动类即可运行项目。

如果在 Linux 系统，只需要通过 SpringBoot 的打包插件，将项目打成 jar 包，执行启动指令即可。

打包插件：

```
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

启动指令：

```
java -jar springboot_01_quickstart.jar	# 项目的名称根据实际情况修改
```

# 3. 配置文件

## 3.1 配置文件优先级

SpringBoot 通过使用不同格式的配置文件以及配置文件的加载位置，从而调整优先级，高优先级配置内容会覆盖低优先级配置内容。

- 格式优先级

application.properties > application.yml > application.yaml

- 位置优先级


file:./config/ > file:./ > classpath:/config/ > classpath:/





yaml 数据读取：

- 使用@Value读取单个数据，属性名引用方式：${一级属性名.二级属性名……}

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667788541514-7d8f8708-f3fe-49df-8967-99f8a9810889.png)

- 封装全部数据到Environment对象

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667788554803-f17a35d4-0652-49c1-9008-c806c582e5fc.png)

- 自定义对象封装指定数据

```
public class Enterprise {
    private String name;
    private Integer age;
    private String tel;
    private String[] subject;
    //自行添加getter、setter、toString()等方法
}
```

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667788569514-c8acc353-c592-4f20-b0d2-52d9646b773e.png)

- 自定义对象封装数据警告解决方案

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667788582584-b7ea5859-6877-44ea-9f91-0c79885cc57f.png)

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

## 3.3 多环境配置

### 3.3.1 yaml 多环境配置

在开发环境、测试环境和生产环境中使用不同的配置文件，并且能在不同环境中灵活切换。

主要使用的是 yaml 中的`---`来分隔不同的配置环境。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667788615149-e2d0c9ad-49c5-42e7-ba15-a9c52392a8c0.png)

### 3.3.2 properties 多环境配置

```
#主启动配置文件 application.properties
spring.profiles.active=pro
```

```
#环境分类配置文件 application-pro.properties
server.port=80
```

```
#环境分类配置文件 application-dev.properties
server.port=81
```

```
#环境分类配置文件application-test.properties
server.port=82
```

### 3.3.3 命令行加载配置

如果没有在默认配置文件中设置 spring.profiles.active 属性值，也可以通过命令行启动项目时指定参数。

```
java –jar springboot.jar --spring.profiles.active=test
java –jar springboot.jar --server.port=88
java –jar springboot.jar --server.port=88 --spring.profiles.active=test
```

设置参数时所使用的连续的两个减号 `--` 就是对其属性配置文件 application.properties 或者 application.yml 中的属性值进行赋值的标识。

### 3.3.4 参数加载顺序

1. 首先加载启动命令中传入的参数;
2. 加载SPRING_APPLICATION_JSON中的属性。SPRING_APPLICATION_JSON是以JSON格式配置在系统环境变量中的内容;
3. 加载java:comp/dev中的JNDI属性;
4. 加载Java的系统属性，可以通过System.getProperties()获取到的内容;
5. 加载操作系统的环境变量;
6. 加载random.*配置的随机属性;
7. 加载位于当前应用jar包之外，针对不同{profile}环境的配置文件内容，比如application-{profile}.properties或者YAML定义的配置文件;
8. 加载位于当前应用jar包之内，针对不同{profile}环境的配置文件内容，比如application-{profile}.properties或者YAML定义的配置文件;
9. 加载位于当前应用jar包之外的application.properties和YAML配置内容;
10. 加载位于当前应用jar包之内的application.properties和YAML配置内容;
11. 加载含有@Configuration注解的类，通过@PropertySource注解定义的属性;
12. 最后加载应用的默认属性，使用SpringApplication.setDefaultProperties定义的内容。

### 3.3.5 兼容Maven环境

因为在 Maven 中也可以进行多环境配置，所以可能存在 Maven 与 SpringBoot 多环境冲突。

解决方式：

1. Maven 中设置 profile 属性

```
<profiles>
    <profile>
        <id>dev_env</id>
        <properties>
            <profile.active>dev</profile.active>
        </properties>
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
    </profile>
    <profile>
        <id>pro_env</id>
        <properties>
            <profile.active>pro</profile.active>
        </properties>
    </profile>
    <profile>
        <id>test_env</id>
        <properties>
            <profile.active>test</profile.active>
        </properties>
    </profile>
</profiles>
```

2. SpringBoot 中引用 Maven 属性

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667788642748-c5cf9d8b-c26b-45eb-bea9-9d2979ebd00f.png)

3. 执行 Maven 打包命令

Maven指令执行完毕后，生成了对应的包，其中类参与编译，但是配置文件并没有编译，而是复制到包中。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667788652485-ddcc5991-3438-4243-a106-854d1ac96cbe.png)

对于源码中非java类的操作要求加载Maven对应的属性，解析${}占位符，并且对资源文件开启对默认占位符的解析

```
<build>
    <plugins>
        <plugin>
            <artifactId>maven-resources-plugin</artifactId>
            <configuration>
                <encoding>utf-8</encoding>
                <useDefaultDelimiters>true</useDefaultDelimiters>
            </configuration>
        </plugin>
    </plugins>
</build>
```

这样 Maven 打包，配置文件中即可加载到属性，打包通过

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667788662928-ce50f9b2-d102-4faf-9133-1a2b9aedeb00.png)

## 3.4 starter

### 3.4.1 starter 简介

starter 提供了在特定场景下启动Spring Boot应用程序所需的一组默认依赖项。比如 添加 `spring-boot-starter-web` starter就可以自动添加所必须的依赖项，如 SpringMVC、Tomcat等。Starter使得开发者可以轻松地集成第三方库、框架和其他组件，而不需要手动处理大量的配置和依赖关系。

### 3.4.2 自定义starter

## 3.5 parent

所有 SpringBoot 项目都要继承的项目，定义了若干个坐标版本号（依赖管理，而非依赖），以达到减少依赖冲突的目的。

以后在 SpringBoot 中添加依赖就可以不用添加 version 属性，同时如果需要自定义依赖版本，也可以显示的书写 version 用来覆盖默认版本。

## 3.6 切换为 Jetty

```
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <!--web起步依赖环境中，排除Tomcat起步依赖-->
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <!--添加Jetty起步依赖，版本由SpringBoot的starter控制-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jetty</artifactId>
    </dependency>
</dependencies>
```

# 4. 整合第三方技术

## 4.1 整合 Junit

### 4.1.1 Spring整合Junit

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1667788688736-ababb011-a142-4f20-937c-ed811bc7ea62.png)

### 4.1.2 SpringBoot整合Junit

1. 添加Junit的starter

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

2. 编写测试类（默认自动生成一个）

```
@SpringBootTest
class Springboot07JunitApplicationTests {
    @Autowired
    private BookService bookService;

    @Test
    public void testSave() {
        bookService.save();
    }
}
```

## 4.2 整合 MyBatis

1. 创建SpringBoot依赖，导入mybatis、mysql依赖

  

2. 配置数据源

```
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    username: root
    password: root
    type: com.alibaba.druid.pool.DruidDataSource
```

SpringBoot版本低于2.4.3(不含)，Mysql驱动版本大于8.0时，需要在url连接串中配置时区，或在MySQL数据库端配置时区解决此问题

```
jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
```

3. 定义数据层接口和映射配置（配置文件）

```
@Mapper
public interface UserDao {
    @Select("select * from tbl_book where id=#{id}")
    Book getById(Integer id);
}
```

# 5.SpringBoot所做的优化

## 三、整合第三方技术

  

### 1. 整合JUnit

  

#### 问题导入

  

回忆一下Spring整合JUnit的步骤？

  

#### 1.1 Spring整合JUnit（复习）

  

  

  

#### 1.2 SpringBoot整合JUnit

  

【第一步】添加整合junit起步依赖(可以直接勾选)

  

  

【第二步】编写测试类，默认自动生成了一个

  

  

### 2. 基于SpringBoot实现SSM整合

  

#### 问题导入

  

回忆一下Spring整合MyBatis的核心思想？

  

#### 2.1 Spring整合MyBatis（复习）

  

- SpringConfig

- 导入JdbcConfig
- 导入MyBatisConfig

  

```
@Configuration
@ComponentScan("com.itheima")
@PropertySource("classpath:jdbc.properties")
@Import({JdbcConfig.class, MyBatisConfig.class})
public class SpringConfig {

}
```

  

- JDBCConfig

- 定义数据源（加载properties配置项：driver、url、username、password）

  

```
#jdbc.properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/spring_db
jdbc.username=root
jdbc.password=itheima
```

  

```
public class JdbcConfig {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String userName;
    @Value("${jdbc.password}")
    private String password;

    @Bean
    public DataSource getDataSource() {
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(userName);
        ds.setPassword(password);
        return ds;
    }
}
```

  

- MyBatisConfig

- 定义SqlSessionFactoryBean
- 定义映射配置

  

```
@Bean
public SqlSessionFactoryBean getSqlSessionFactoryBean(DataSource dataSource) {
    SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();
    ssfb.setTypeAliasesPackage("com.itheima.domain");
    ssfb.setDataSource(dataSource);
    return ssfb;
}
```

  

```
@Bean
public MapperScannerConfigurer getMapperScannerConfigurer() {
    MapperScannerConfigurer msc = new MapperScannerConfigurer();
    msc.setBasePackage("com.itheima.dao");
    return msc;
}
```

  

#### 2.2 SpringBoot整合MyBatis

  

  

  

④：定义数据层接口与映射配置

  

  

⑤：测试类中注入dao接口，测试功能

  

```
@SpringBootTest
class Springboot08MybatisApplicationTests {
    @Autowired
    private BookDao bookDao;

    @Test
    public void testGetById() {
        Book book = bookDao.getById(1);
        System.out.println(book);
    }
}
```

  

#### 2.3 案例-SpringBoot实现ssm整合

  

**【第一步】创建SpringBoot工程，添加druid依赖**

  

```
<!-- todo 1 添加druid连接池依赖-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.6</version>
</dependency>
```

  

**【第二步】复制springmvc_11_page工程各种资源(主java类、页面、测试类)**

  

**【第三步】删除config包中的所有配置，在BookDao接口上加@Mapper注解**

  

```
//todo 3 在BookDao接口上加@Mapper注解，让SpringBoot给接口创建代理对象
@Mapper
public interface BookDao {
    //...
}
```

  

**【第四步】将application.properties修改成application.yml，配置端口号和连接参数**

  

```
server:
  port: 80
# todo 4 配置数据库连接参数
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db
    username: root
    password: root
    type: com.alibaba.druid.pool.DruidDataSource
```

  

**【第五步】修改BookServiceTest配置类，进行配置**

  

```
// todo 5 修改单元测试类，添加@SpringBootTest主键，修复@Test注解导包
@SpringBootTest
public class BookServiceTest {

    @Autowired
    private BookService bookService;

    @Test
    public void testGetById(){
        Book book = bookService.getById(2); //传递参数1会抛出异常
        System.out.println(book);
    }
    @Test
    public void testGetAll(){
        List<Book> all = bookService.getAll();
        System.out.println(all);
    }
}
```

  

**【第六步】在static目录中提供index.html页面，跳转到"pages/books.html"**

  

```
<script>
    location.href="pages/books.html"
</script>
```

  

**最后：运行引导类即可访问**

# 4.自定义starter





# SpringTask
![[Spring Task.png|700x344]]
# 概述
---
## 简介
---
Spring Task 是 Spring 框架提供的一个用于任务调度的功能模块，通常用于在 SpringBoot 项目中执行定时任务和管理异步任务。它基于 **`TaskScheduler`** 和 **`TaskExecutor`**，提供了一个简化的方式基于注解的方式来处理后台任务的调度和异步执行。
## 需求
---
在项目开发中，可能需要在特定时间、周期执行任务，最简单的做法是使用 Java 自带的 **`Timer`** 和 **`ScheduledExecutorService`** 来实现定时任务，但它们有很大的局限性，无法满足复杂的业务需求：
- **无法灵活控制任务的执行时间**（比如，每月 1 号 12:00 执行）。
- **应用重启后，任务会丢失**（如服务器重启，定时任务需要手动重新启动）。
- **无法动态增删任务**（比如，想在运行时修改任务时间，只能重启应用）。
- **不支持分布式调度**（多个节点可能会重复执行任务）。
相较于 Quartz 的复杂，Spring Task 提供了一种简化方式来进行任务调度管理，适合于简单的单机的任务调度，优点有：
- 通过 @**`Scheduled`** 和 **`@Async`** 注解，开发者可以非常简单地定义定时任务和异步任务，无需显式地管理任务的调度和执行。
- 支持 Cron 表达式、固定间隔、固定延迟等多种调度方式，极大简化了定时任务的配置。
- 自动集成到 Spring 的上下文中，确保任务在应用启动时自动运行，方便维护和扩展。
## 对比
---
参照 Quartz 的任务调度框架的对比：[[Quartz#对比]]
# 开始
---
## 安装
---
如果使用的是 SpringBoot 项目，它已经包含了 Spring Task 的所有必要依赖，只需要在 Pom 文件中确保引入了 Spring Boot Starter 依赖即可（一般作为基础依赖被引入）。
```xml
<!-- Spring Boot Starter Web（如果有需要）-->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter</artifactId>
</dependency>
```
如果是传统的 Spring 项目，则需要手动添加依赖 **`spring-context`** 和 **`spring-task`**。
```xml
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-context</artifactId>
	<version>5.3.x</version>
</dependency>
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-task</artifactId>
	<version>5.3.x</version>
</dependency>
```
## 使用
---
1. **启用定时任务**：需要在 Spring 配置类上使用 **`@EnableScheduling`** 注解。这会让 Spring 自动扫描和执行标记为 **`@Scheduled`** 的定时任务。
	```java
	@Configuration
	@EnableScheduling  // 启用定时任务
	public class SchedulingConfig {
	    // 可以在此配置更多的定时任务调度策略
	}
	```
2. **启用异步任务**：需要在 Spring 配置类上使用 **`@EnableAsync`** 注解。这会让 Spring 自动扫描和执行标记为 **`@Async`** 的异步任务。这两个注解也可以放在一个类上。
	```java
	@Configuration
	@EnableAsync  // 启用异步任务
	public class AsyncConfig {
	    // 可以配置自定义的线程池
	}
	```
3. **定义定时任务**：使用 **`@Scheduled`** 注解来定义任务。
	```java
	@Component
	public class ScheduledTasksService {

	    // 每5秒执行一次任务
	    @Scheduled(fixedRate = 5000)
	    public void runTask() {
	        System.out.println("任务开始执行...");
	    }
	
	    // 上一个任务结束后5秒再执行
	    @Scheduled(fixedDelay = 5000)
	    public void runDelayedTask() {
	        System.out.println("延时任务执行...");
	    }
	}
	```
4. **定义异步任务**：使用 **`@Async`** 注解来定义异步任务，该注解允许方法在独立的线程中执行，避免阻塞主线程，默认使用 **`SimpleAsyncTaskExecutor`**，但是通常会自定义线程池来提高效率和控制并发。
	```java
	@Component
	public class AsyncTaskService {

	    // 异步执行的任务
	    @Async
	    public void asyncTask() {
	        System.out.println("异步任务开始执行...");
	        try {
	            Thread.sleep(5000);  // 模拟耗时任务
	        } catch (InterruptedException e) {
	            e.printStackTrace();
	        }
	        System.out.println("异步任务执行完毕");
	    }
	}
	```
5. **执行**：定时任务会由 **`TaskScheduler`** 进行调度执行，而异步任务则需要手动调用执行。
# 概念
---
## 核心组件
---
### TaskScheduler
---
**`TaskScheduler`** 接口提供了调度任务的方法，用于定时任务的调度。它提供了任务调度的基本功能，可以通过它来安排任务的执行。常见的实现类有：
- **`ThreadPoolTaskScheduler`**：一个基于线程池的调度器，适合在多线程环境下调度任务，支持定时任务、延时任务和 Cron 表达式调度。默认使用。
- **`ConcurrentTaskScheduler`**：通过 Java 标准的 **`ScheduledExecutorService`** 实现的调度器，功能类似 **`ThreadPoolTaskScheduler`**。
### TaskExecutor
---
**`TaskExecutor`** 接口定义了任务执行的方法，用于异步任务的执行。它的作用是把任务交给不同的线程池执行。常见的实现类有：
- **`SimpleAsyncTaskExecutor`**：一个简单的任务执行器，每次执行任务时都会创建一个新的线程。适合任务数量不多且任务执行时间较短的场景。默认使用。
- **`ThreadPoolTaskExecutor`**：基于线程池的任务执行器，适合大规模并发任务的场景，能够复用线程，减少线程创建的开销。
### @Scheduled 
---
**`@Scheduled`** 注解是 Spring Task 中最常用的注解之一，主要用于定义定时任务。通过使用这个注解，我们可以非常简便地在方法上配置**定时任务**的执行时间和周期，而不需要显式地使用 **`TaskScheduler`**，这些任务之间相互隔离，互不干扰。**`@Scheduled`** 注解支持多种定时调度方式，包括：
- **固定间隔（`fixedRate`）**
- **固定延迟（`fixedDelay`）**
- **延迟启动（`initialDelay`）**
- **Cron 表达式（`cron`）**
```java
@Component
public class ScheduledTask {

    // 每5秒执行一次任务
    @Scheduled(fixedRate = 5000)
    public void fixedRateTask() {
        System.out.println("定时任务：每5秒执行一次...");
    }

    // 上一次任务完成后，延迟10秒执行下一次
    @Scheduled(fixedDelay = 10000)
    public void fixedDelayTask() {
        System.out.println("延迟任务：上次任务完成后延迟10秒...");
    }

    // 使用 Cron 表达式每天中午12点执行
    @Scheduled(cron = "0 0 12 * * ?")
    public void cronTask() {
        System.out.println("Cron任务：每天中午12点执行...");
    }
}
```
### @Async
---
**`@Async`** 注解是 Spring 中用于标记**异步方法**的注解，使得方法的执行不再阻塞主线程。Spring 会通过 **`TaskExecutor`** 自动异步执行被 **`@Async`** 标记的方法。通常，**`@Async`** 用于执行那些不需要等待执行结果的长时间任务，或者希望在后台执行的任务。**`@Async`** 默认使用 **`SimpleAsyncTaskExecutor`**，如果需要使用基于线程池的任务执行器，需要在 **`@Async`** 中指定 **Bean** 的名称。
```java
@Component
public class AsyncTask {

    // 异步执行任务
    @Async("asyncTaskExecutor")
    public void asyncTask() {
        System.out.println("异步任务开始执行...");
        try {
            Thread.sleep(5000);  // 模拟耗时任务
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("异步任务执行完毕");
    }
}
```
## 定时任务和异步任务
---
### 定时任务
---
定时任务的特点是按**预定时间**执行，它可以是定时触发的，也可以是固定间隔或延迟执行的。定时任务默认由**主线程**执行，但是如果使用 **`@Async`** 注解或者手动将定时任务交给 **`TaskExecutor`** 执行，则任务将会变为异步执行。
### 异步任务
---
异步任务是指在执行时不会阻塞调用线程的任务，通常它会在**后台线程池**中执行。异步任务没有调度策略，它们的执行与时间无关，而是由事件触发的，它会在调用时立即执行，任务会被交给 **`TaskExecutor`** 管理的线程池来执行。而如果想让异步任务定时执行，则需要同时使用 **`@Scheduled`** 和 **`@Async`** 两个注解：
```java
@Component
public class ScheduledAsyncTask {

	@Async
	@Scheduled(fixedRate = 5000)  // 每5秒执行一次
	public void executeAsyncTask() {
		// 异步执行的逻辑
		System.out.println("异步任务开始执行：" + Thread.currentThread().getName());
		try {
			// 模拟耗时操作
			Thread.sleep(2000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println("异步任务执行完毕：" + Thread.currentThread().getName());
	}
}
```
或者手动将定时任务提交给 **`TaskExecutor`**：
```java
@Component
public class ScheduledAsyncTask {

	@Autowired
	private AsyncTask asyncTask;

	@Scheduled(fixedRate = 5000)  // 每5秒执行一次
	public void task() {
		asyncTask.executeAsyncTask();  // 执行异步任务
	}
}
```
## Cron表达式
---
Cron （Crontab）表达式是一种用于定义定时任务的时间安排的字符串，它源自 Unix 系统的 cron 服务，Cron 表达式提供了一种简洁且强大的方式来表示重复执行的任务安排。
### 表达式格式
---
Cron 表达式通常由 6 个或 7 个字段组成，每个字段代表不同的时间单位。标准的 Cron 表达式格式如下：
```
秒（Seconds） 分钟（Minutes） 小时（Hours） 日（Day of month） 月（Month） 星期（Day of week） 年（Year）（可选）
```
例如：
```
0 15 10 * * ?
```
解释：
- **`0`**：秒（0-59）
- **`15`**：分钟（0-59）
- **`10`**：小时（0-23）
- **`*`**：日（1-31）
- **`*`**：月（1-12）
- **`?`**：星期（0-6：0 为星期日，6 为星期六）
- **`年`**：年份（optional：可选，如 2025）
### 特殊字符
---
- **`*`**（星号）：表示所有值。例如，在分钟字段中，`*` 表示每分钟都执行一次。
- **`,`**（逗号）：表示多个值的集合。例如，`1,2,3` 表示在1、2和3分钟执行。
- **`-`**（连字符）：表示一个范围。例如，`10-12` 表示从10到12分钟每分钟都执行。
- **`/`**（斜杠）：表示增量。例如，`*/5` 在分钟字段中表示每5分钟执行一次。
- **`?`**（问号）：表示“无特定值”，仅用于日和星期字段。通常是与另一个字段的具体值配合使用，以避免二者的冲突。例如，如果你设置了具体的日字段，你可以在星期字段使用 `?`。
- **`L`**：表示“最后”，通常用于日和星期字段。例如，在星期字段中，`L` 表示每个月的最后一天；在日字段中，`L` 表示每个月的最后一个星期几。
- **`W`**：表示“最接近的工作日”，例如，`15W` 表示最接近15号的工作日。
- **`#`**：用于指定某个月的第几个星期几。例如，`5#2` 表示5月的第二个星期五。
# 配置
---
## 线程池
---
对于线程池这种重量级资源，最好是一个项目设置一个线程池，如果项目中存在线程池，就可以使用共享线程池来替代 Spring 默认线程池。
### Spring 线程池
---
只需要在定时任务配置类或者异步任务配置类中定义一次，会被注册为 Bean，就能被定时任务和异步任务共同使用。默认使用 Spring 自带的 **`TreadPoolTaskExecutor`** 和 **`ThreadPoolTaskScheduler`**。**`ThreadPoolTaskExecutor`** 内部封装了 **`ThreadPoolExecutor`**。
配置类示例：
```java
@Configuration  
@EnableAsync  
public class AsyncConfig {  
  
    @Bean  
    public TaskExecutor asyncTaskExecutor() {  
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();  
        executor.setCorePoolSize(5);  // 设置核心线程数
        executor.setMaxPoolSize(10);  // 设置最大线程数
        executor.setQueueCapacity(100);  // 设置队列容量
        executor.setThreadNamePrefix("ThreadPoolTaskExecutor - "); // 设置线程前缀  
        executor.initialize();  // 初始化线程池
        return executor;  
    }  
  
    @Bean  
    public TaskScheduler taskScheduler() {  
        ThreadPoolTaskScheduler scheduler = new ThreadPoolTaskScheduler();  
        scheduler.setPoolSize(5);  // 设置线程数量
        scheduler.setThreadNamePrefix("ThreadPoolTaskScheduler - "); // 设置线程前缀
        return scheduler;  // 会被自动初始化，直接返回
    }  
}
```
配置文件示例：
```yaml
spring:  
  task:  
    execution: 
      pool:  
        core-size: 5  
        max-size: 10  
        queue-capacity: 100  
      thread-name-prefix: ThreadPoolTaskExecutor -  
    scheduling:  
      pool:  
        size: 5  
      thread-name-prefix: ThreadPoolTaskScheduler -
```
然后在 **`@Async`** 注解中指定 Bean 名称，如果不使用名称，则默认使用 **`SimpleAsyncTaskExecutor`**。同样也可以在 **`@Async`** 中指定外部线程池的 Bean 的名称。
# 思考
---
1. 如何在使用外部线程池的情况下，让任务仍被 TaskExecutor 管理？
# 附录
---
> [!website]
    > [Spring Task官网](https://spring.io/guides/gs/scheduling-tasks)

> [!code]
    > [Spring Task 指导代码](https://github.com/spring-guides/gs-scheduling-tasks)

> [!video]
    > 

> [!doc]
    > [Spring Task中文文档](https://springdoc.cn/spring/integration.html#scheduling)




# Spring Security
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

## JWT过滤器





## SecurityFilterChain

---

Spring Security 5.7 以后，推荐使用 `SecurityFilterChain` 配置安全策略，主要包括：

- **请求匹配规则**
    
    - 方法：`.securityMatcher("/路径/**")`
    - 作用：指定当前 `SecurityFilterChain` 适用于哪些请求
- **身份授权**
    
    - 方法：`.authorizeHttpRequests()`
    - 作用：配置请求的权限规则（如 `permitAll()`、`hasRole()`）
- **身份认证方式**
    
    - 方法：`.formLogin()`、`.httpBasic()`、`.oauth2Login()`
    - 作用：选择认证方式（表单登录、Basic 认证、OAuth2）
- **回话管理**
    
    - 方法：`.sessionManagement()`
    - 作用：控制 `Session` 策略（如 `STATELESS`、`IF_REQUIRED`）
        - 代码示例
            
            ```java
            .sessionManagement(session -> session
					                        .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            ```
            
            `sessionCreationPolicy()` 允许控制 **Session 的创建和使用策略：**
            
            - **SessionCreationPolicy.ALWAYS**
                
                每次请求都创建 Session
                
            - **SessionCreationPolicy.IF_REQUIRED**
                
                Spring Security 需要时才创建 Session（默认值）
                
            - **SessionCreationPolicy.NEVER**
                
                Spring Security 不创建 Session，但如果存在就使用
                
            - **SessionCreationPolicy.STATELESS**
                
                完全不使用 Session（推荐 Token 认证）
                
- **跨域处理**
    
    - 方法：`.cors()`
    - 作用：是否允许跨域请求
        - 代码示例
            
            ```java
            // 关闭CSRF
            .csrf(csrf -> csrf.disable())
            ```
            
- **CSRF保护**
    
    - 方法：`.csrf().disable()`
    - 作用：关闭或启用 CSRF 防护
- **异常处理**
    
    - 方法：`.exceptionHandling()`
    - 作用：配置自定义的异常处理（如未授权处理）
        - 代码示例
            
            ```java
            .exceptionHandling(exception -> exception
            				  // 处理未认证异常
                     .authenticationEntryPoint(new CustomAuthenticationEntryPoint()) 
                      // 处理权限不足异常 
                     .accessDeniedHandler(new CustomAccessDeniedHandler())  
            )
            ```
            
            自定义的异常处理类，需要实现 AuthenticationEntryPoint 接口，重写 commence 方法
            
- **过滤器管理**
    
    - 方法：`.addFilterBefore()`、`addFilterAfter()`、`addFilter()`
    - 作用：在 Spring Security 过滤器链中添加自定义过滤器
- **其它**
    
    - 方法：`.logout()`
    - 作用：自定义退出逻辑

# 附录

---

<aside> 🌐

**官网地址：**

</aside>

<aside> 💾

**代码地址：**

</aside>

<aside> 📓

**其它文档：**

</aside>

# 概述

## 简介

SpringSecurity 是一个基于过滤器链的功能强大、高度自定义的认证和访问控制框架

为应用程序提供了诸如身份验证、授权、保护等安全功能

SpringSecurity 要求有 Java8 或更高的运行环境

## 核心组件

### SecurityContext

- **`SecurityContext`**：
    - 用于存储当前认证用户的安全信息（例如：`Authentication` 对象）。
    - 在请求处理的各个阶段中，`SecurityContext` 用来共享和传递安全信息。
- **`SecurityContextHolder`**：
    - 是 `SecurityContext` 的持有者，提供静态方法来访问当前线程的安全上下文。
    - 可以通过 `SecurityContextHolder.getContext()` 获取当前 `SecurityContext`。
- **应用场景**：
    - 获取当前用户的身份信息：`SecurityContextHolder.getContext().getAuthentication()`。

### Authentication

- **概念**：
    - 表示用户认证信息的核心接口，封装了当前用户的身份和权限。
- **主要内容**：
    - `Principal`：用户的身份信息，通常是用户名。
    - `Credentials`：认证凭据（如密码）。
    - `Authorities`：用户被授予的权限集合。
- **实现类**：
    - 常见的实现类有 `UsernamePasswordAuthenticationToken`，它主要用于用户名密码登录认证。

### GrantedAuthority

- **概念**：
    - 表示授予用户的权限，用于权限检查。
- **作用**：
    - 是 `Authentication` 中权限集合的基本单元。
    - 一般用字符串表示权限（如 `"ROLE_USER"`）。

### UserDetails

- **`UserDetails`**：
    - 定义了用户的核心属性（如用户名、密码、权限、账号是否过期等）。
    - 开发者需要提供自己的 `UserDetails` 实现以适配业务需求。
- **`UserDetailsService`**：
    - 是一个接口，用于加载用户的详细信息。封装进 `UserDetails` 里面。
        
    - 其核心方法：
        
        ```java
        UserDetails loadUserByUsername(String username) throws UsernameNotFoundException;
        ```
        
    - 用于从数据库或其他存储中加载用户信息。
        

### AuthenticationManager

- **`AuthenticationManager`**：
    - 认证管理器，负责协调完成认证逻辑。定义了认证 `Authentication` 的方法 `authenticate`
    - 是认证流程的入口，其常见实现类是 `ProviderManager`。
- **`AuthenticationProvider`**：
    - 提供实际的认证逻辑。
    - 通过 `supports()` 方法决定是否支持某种类型的认证。
    - 多个 `AuthenticationProvider` 可以注册到 `AuthenticationManager` 中。

### Filter

Spring Security 的过滤器链是整个安全逻辑的核心机制之一。过滤器链中包含多种类型的过滤器，处理从请求到响应的不同阶段的安全逻辑。

- **`UsernamePasswordAuthenticationFilter`**：
    - 处理基于用户名和密码的登录认证请求。
    - 默认 URL：`/login`。
- **`BasicAuthenticationFilter`**：
    - 处理 HTTP Basic 认证。
- **`BearerTokenAuthenticationFilter`**：
    - 处理基于 Bearer Token 的认证。
- **`SecurityContextPersistenceFilter`**：
    - 负责加载和保存 `SecurityContext`。
- **`ExceptionTranslationFilter`**：
    - 处理认证或授权失败的异常。
- **`FilterSecurityInterceptor`**：
    - 执行最终的权限校验。
- **`CorsFilter`**：
    - 处理跨域请求。

### AccessDecisionManager

- **`AccessDecisionManager`**：
    - 负责决定用户是否拥有访问资源的权限。
    - 通常与 `AccessDecisionVoter` 配合使用。
- **`AccessDecisionVoter`**：
    - 是一个投票器，根据具体逻辑对用户权限投票。
    - 有多个实现类，例如：
        - `RoleVoter`：基于角色的投票器。
        - `AuthenticatedVoter`：基于是否已认证的投票器。

### SecurityFilterChain

- **作用**：
    
    - 定义一组过滤器的配置，用于保护特定的 URL 模式。
    - 可以通过 Java 配置或 XML 配置来设置。
- **Java 配置示例**：
    
    ```java
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
            .antMatchers("/admin/**").hasRole("ADMIN")
            .antMatchers("/user/**").authenticated()
            .and()
            .formLogin();
        return http.build();
    }
    ```
    

### PasswordEncoder

- **作用**：
    
    - 用于对用户密码进行加密和匹配验证。
    - 推荐使用 `BCryptPasswordEncoder` 进行密码加密。
- **示例**：
    
    ```java
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
    ```
    

### HttpSecurity

- **概念**：
    - 是 Spring Security 配置的核心入口，通过它定义安全规则。
- **常见配置**：
    - URL 授权规则。
    - 登录和注销行为。
    - CSRF、CORS 等防护。

## 基本架构

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/0612efd8-9813-461c-a6a4-a8a5d37aa52a/image.png)

## 认证流程

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/a126ed36-ca8f-4070-98a1-ef7e9925dc5e/image.png)

# 入门案例

## 引入依赖

SpringBoot 提供了一个 SpringSecurity 的 starter：`spring-boot-starter-security`

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

## 默认认证

只需要在 Web 项目中引入 SpringSecurity 的依赖，不需要任何配置，当使用浏览器访问该项目任何 url 时，会自动跳转到 `/login` 登录界面，用户名是 user，密码会打印到控制台

# 认证

认证就是确认请求是谁发来的

## 自定义认证逻辑

默认情况下，如果配置文件中没有提供用户名和密码，则默认用户名为 user，密码打印到控制台

模式是基于sessionID实现用户认证

现在需要通过在数据库中查询用户信息，然后根据查询到的信息确定用户信息是否有效

做法是实现 `UserDetailsService` 接口，重写 `loadUserByUsername()` 方法

返回一个封装的 `UserDetails` 对象

- UserDetailsServiceImpl
    
    ```java
    @Service
    public class UserDetailsServiceImpl implements UserDetailsService {
    
        private static final Logger log = LoggerFactory.getLogger(UserDetailsServiceImpl.class);
        @Autowired
        private UserMapper userMapper;
    
        @Override
        public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
            // 根据用户名查询用户
            LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
            wrapper.eq(User::getUserName,username);
            User user = userMapper.selectOne(wrapper);
            if (Objects.isNull(user)){
                log.error("用户登录：{} 不存在.",username);
                throw new RuntimeException("用户名不存在！");
            }
            else if(user.getDelFlag().equals(1)){
                log.error("用户登录：{} 已被删除.",username);
                throw new RuntimeException("用户已被删除！");
            }
            else if(user.getStatus().equals("1")){
                log.error("用户登录：{} 已被停用",username);
                throw new RuntimeException("用户已被停用！");
            }
            return new LoginUser(user);
        }
    }
    ```
    
- LoginUser
    
    ```java
    public class LoginUser implements UserDetails {
    
        private User user;
    
        public LoginUser() {
        }
    
        public LoginUser(User user) {
            this.user = user;
        }
    
        @Override
        public Collection<? extends GrantedAuthority> getAuthorities() {
            return null;
        }
    
        @Override
        public String getPassword() {
            return user.getPassword();
        }
    
        @Override
        public String getUsername() {
            return user.getUserName();
        }
    
        @Override
        public boolean isAccountNonExpired() {
            return true;
        }
    
        @Override
        public boolean isAccountNonLocked() {
            return true;
        }
    
        @Override
        public boolean isCredentialsNonExpired() {
            return true;
        }
    
        @Override
        public boolean isEnabled() {
            return true;
        }
    }
    ```
    

现在就可以使用自定义的用户以及验证逻辑

需要注意的是，密码铭文存储时需要在前面加上`{noop}`

## 密码加密器

默认的 `PasswordEncoder`要求数据库中的密码为明文，格式为：`{id}password`，然后根据 `id` 去选择密码加密方式。

一般使用  `bcrypt` 算法对密码进行散列。对应的密码加密器为：`BCryptPasswordEncoder`

- SecurityConfig
    
    写在 Security 配置类中，可以是继承 `WebSecurityConfigurerAdapter` 的方式
    
    在 5.7.0 版本中 `WebSecurityConfigurerAdapter` 被废弃了，所以使用这种方式
    
    直接将 PasswordEncoder 注入容器即可
    
    ```java
    @Configuration
    @EnableWebSecurity
    public class SecurityConfig {
        /**
         * 密码编码器
         *
         * @return {@code PasswordEncoder }
         */
        @Bean
        public PasswordEncoder passwordEncoder() {
            return new BCryptPasswordEncoder();
        }
    }
    ```
    

## 登录功能实现

现在 `SpringSecutity` 会对所有请求进行拦截，我们希望将登录、注册、验证码和静态资源的请求放行，进入自定义的逻辑。

在登陆时不需要手动调用数据库查询，只需要调用 `AuthenticationManager` 的`authenticate` 方法进行校验就可以拿到`Authentication` 对象，再调用`getPrincipal` 方法就能拿到 `LoginUser` 对象，将 `LoginUser` 存入`Redis`，并生成 `Token` 返回前端

- SecurityConfig
    
    ```java
    @Configuration
    @EnableWebSecurity
    public class SecurityConfig {
    
        @Autowired
        private UserDetailsService userDetailsService;
    
        /**
         * 过滤器链配置
         *
         * @param http http
         * @return {@code SecurityFilterChain }
         * @throws Exception 例外
         */
        @Bean
        public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
            return http    //关闭csrf
                    .csrf(AbstractHttpConfigurer::disable)
                    //不通过Session获取SecurityContext
                    .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
                    .authorizeRequests(request -> {
                        request.antMatchers("/login","/register","/captchaImage").permitAll()
                                .antMatchers(HttpMethod.GET,"/","*.html","/**/*.css").permitAll()
                                .anyRequest()
                                .authenticated();
    
                    })
                    .build();
        }
    
        /**
         * 密码编码器
         *
         * @return {@code PasswordEncoder }
         */
        @Bean
        public PasswordEncoder passwordEncoder() {
            return new BCryptPasswordEncoder();
        }
    
        /**
         * 认证管理器实现
         * 可以不配置，默认使用了 UserDetailsService
         *
         * @return {@code AuthenticationManager }
         */
        @Bean
        public AuthenticationManager authenticationManager(){
            DaoAuthenticationProvider provider = new DaoAuthenticationProvider();
            provider.setUserDetailsService(userDetailsService);
            provider.setPasswordEncoder(passwordEncoder());
            return new ProviderManager(provider);
        }
    }
    ```
    
- LoginserviceImpl
    
    ```java
    @Service
    public class LoginServiceImpl implements LoginServcie {
     
        @Autowired
        private AuthenticationManager authenticationManager;
        @Autowired
        private RedisCache redisCache;
     
        @Override
        public ResponseResult login(User user) {
            UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(user.getUserName(),user.getPassword());
            Authentication authenticate = authenticationManager.authenticate(authenticationToken);
            //使用userid生成token
            LoginUser loginUser = (LoginUser) authenticate.getPrincipal();
            String userId = loginUser.getUser().getId().toString();
            String jwt = JwtUtil.createJWT(userId);
            //authenticate存入redis
            redisCache.setCacheObject("login:"+userId,loginUser);
            //把token响应给前端
            HashMap<String,String> map = new HashMap<>();
            map.put("token",jwt);
            return new ResponseResult(200,"登陆成功",map);
        }
    }
    ```
    

## 认证过滤器

以后前端每次请求都将携带 Token，认证过滤器的作用是根据 Token 获取 LoginUser 对象

## 登出功能实现

# 授权

授权就是确认该用户具有的权限，某些功能只有有权限的用户才能使用

### 自定义授权逻辑

授权逻辑有三种实现方式：

- 在 SecurityConfig 中通过 `url` 确定权限，然后在 `UserDetailsService` 中将权限封装到`authorities`
- 通过注解方式确定方法的权限，然后在 `UserDetailsService` 中将权限封装到`authorities`
- 通过注解方式确定方法的权限，需要将`UserDetailsService` 中将权限封装到`authorities` ，而是通过自定义的方式，校验权限，参照 Ruoyi 的方式

第二种方式较为常用，下面实现第二种方式的权限校验

首先需要再 SecurityConfig 类上加上注解 `*@EnableMethodSecurity*(prePostEnabled = true, securedEnabled = true)` 以开启方法注解

然后在 UserDetailsService 将权限传入封装成 `GrantedAuthority` 的List集合

- UserDetailsServiceImpl
    

# 配置

## 自定义过滤器链

一旦在 springsecurity 配置类中创建了过滤器链对象，则整个默认的过滤器链都将失效

## **SecurityFilterChain 主要配置项**

在 `HttpSecurity` API 中，你可以使用以下方法进行自定义配置：

### **1. 认证（Authentication）**

- **`.formLogin()`**：开启基于表单的身份认证。
- **`.httpBasic()`**：开启 HTTP Basic 认证（适用于 API）。
- **`.oauth2Login()`**：启用 OAuth2 登录（如 GitHub、Google 登录）。
- **`.authenticationManager()`**：自定义认证管理器。

### **2. 授权（Authorization）**

- **`.authorizeHttpRequests()`**：控制 URL 访问权限。
- **`.requestMatchers()`**：匹配特定 URL 进行权限控制。
- **`.hasRole("ROLE") / hasAuthority("AUTHORITY")`**：指定访问角色或权限。

### **3. 会话管理（Session Management）**

- **`.sessionManagement()`**：配置会话策略（如无状态、每用户单会话）。
- **`.maximumSessions(1).maxSessionsPreventsLogin(true)`**：限制单用户登录。

### **4. 记住我（Remember Me）**

- **`.rememberMe()`**：启用“记住我”功能。

### **5. CSRF 保护**

- **`.csrf().disable()`**：关闭 CSRF 保护（REST API 通常需要）。
- **`.csrf().ignoringRequestMatchers("/api/**")`**：对特定 API 关闭 CSRF 保护。

### **6. CORS 处理**

- **`.cors()`**：启用 CORS 机制，允许跨域访问。

### **7. 退出登录**

- **`.logout()`**：自定义登出逻辑，如跳转 URL、删除 Cookie 等。

---

## **示例：完整的 SecurityFilterChain 配置**

```java
java
复制编辑
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http
            // 1. 授权管理：不同URL的访问权限
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/", "/home", "/login").permitAll()  // 允许所有人访问
                .requestMatchers("/admin/**").hasRole("ADMIN")        // 仅管理员访问
                .requestMatchers("/user/**").hasAnyRole("USER", "ADMIN") // 用户或管理员
                .anyRequest().authenticated()  // 其他请求需要登录
            )
            // 2. 认证方式：基于表单 & HTTP Basic 认证
            .formLogin(form -> form
                .loginPage("/login")      // 自定义登录页
                .defaultSuccessUrl("/dashboard") // 登录成功跳转
                .permitAll()
            )
            .httpBasic()  // 启用 HTTP Basic 认证
            // 3. CSRF 保护（API 关闭，其他启用）
            .csrf(csrf -> csrf.ignoringRequestMatchers("/api/**"))
            // 4. 记住我功能
            .rememberMe(remember -> remember
                .tokenValiditySeconds(86400) // 记住 1 天
                .key("uniqueAndSecret")
            )
            // 5. 退出登录
            .logout(logout -> logout
                .logoutUrl("/logout")
                .logoutSuccessUrl("/home")
                .permitAll()
            )
            // 6. Session 管理：仅允许单用户登录
            .sessionManagement(session -> session
                .maximumSessions(1).maxSessionsPreventsLogin(true)
            )
            .build();
    }
}

```

---

## **SecurityFilterChain 重要配置项**

|配置项|作用|
|---|---|
|`.authorizeHttpRequests()`|访问权限控制|
|`.formLogin()`|表单登录|
|`.httpBasic()`|HTTP Basic 认证|
|`.oauth2Login()`|OAuth2 登录（如 Google）|
|`.csrf()`|CSRF 保护|
|`.cors()`|CORS 处理|
|`.rememberMe()`|记住我|
|`.logout()`|退出登录|
|`.sessionManagement()`|会话管理|

---

## **总结**

- `SecurityFilterChain` 允许我们自定义 Spring Security 过滤器链。
- **关键配置项**：身份认证（登录）、访问授权（权限管理）、CSRF 保护、CORS 处理、会话管理等。
- **Spring Security 5.7+ 推荐使用 `SecurityFilterChain` 而不是 `WebSecurityConfigurerAdapter`**。

## **Spring Security 内置过滤器**

Spring Security 具有一组预定义的 **过滤器链**，按照特定顺序执行（Spring Security 6 版本中部分过滤器名称有所调整）：

|**顺序**|**过滤器类**|**功能**|
|---|---|---|
|1|`WebAsyncManagerIntegrationFilter`|处理异步请求的安全上下文|
|2|`SecurityContextPersistenceFilter`|维护 SecurityContext，管理认证信息|
|3|`HeaderWriterFilter`|添加安全响应头，如 `X-Frame-Options`|
|4|`CsrfFilter`|处理 CSRF 保护|
|5|`LogoutFilter`|处理用户登出逻辑|
|6|`OAuth2AuthorizationRequestRedirectFilter`|处理 OAuth2 认证请求重定向|
|7|`Saml2WebSsoAuthenticationFilter`|处理 SAML2 认证|
|8|`UsernamePasswordAuthenticationFilter`|处理用户名密码认证|
|9|`DefaultLoginPageGeneratingFilter`|生成默认登录页面|
|10|`DefaultLogoutPageGeneratingFilter`|生成默认登出页面|
|11|`BasicAuthenticationFilter`|处理 HTTP Basic 认证|
|12|`BearerTokenAuthenticationFilter`|处理 Bearer Token 认证（JWT）|
|13|`RequestCacheAwareFilter`|处理请求缓存|
|14|`SecurityContextHolderAwareRequestFilter`|包装 `HttpServletRequest`，提供安全上下文|
|15|`AnonymousAuthenticationFilter`|处理匿名用户认证|
|16|`SessionManagementFilter`|处理会话管理（防止并发登录等）|
|17|`ExceptionTranslationFilter`|处理 Spring Security 异常（如 `AccessDeniedException`）|
|18|`FilterSecurityInterceptor`|处理访问控制（权限判断）|

### **常用核心过滤器**

- **`UsernamePasswordAuthenticationFilter`**：处理表单登录认证（默认 `/login`）。
- **`BasicAuthenticationFilter`**：处理 HTTP Basic 认证（`Authorization: Basic xxx`）。
- **`BearerTokenAuthenticationFilter`**：处理基于 JWT 的 `Bearer` 令牌认证。
- **`CsrfFilter`**：防御 CSRF 攻击。
- **`FilterSecurityInterceptor`**：执行最终的权限访问控制。
- [AuthorizationFilter](https://springdoc.cn/spring-security/servlet/authorization/authorize-http-requests.html):

# 拓展

## JWT

## RBAC

# 资源

文档参照资源：

- [https://springdoc.cn/spring-security/index.html](https://springdoc.cn/spring-security/index.html)
- [https://github.com/shuhongfan/SpringSecurity_Demo04?tab=readme-ov-file#springsecurity从入门到精通](https://github.com/shuhongfan/SpringSecurity_Demo04?tab=readme-ov-file#springsecurity%E4%BB%8E%E5%85%A5%E9%97%A8%E5%88%B0%E7%B2%BE%E9%80%9A)
- [https://github.com/yangzongzhuan/RuoYi-Vue-fast](https://github.com/yangzongzhuan/RuoYi-Vue-fast)
- [https://github.com/spring-projects/spring-security](https://github.com/spring-projects/spring-security)



# SpringCloud


# 概述
## 简介
---
SpringCloud 是基于 SpringBoot 的一系列框架的集合，专门用于构建**分布式系统**和**微服务架构**。它提供了一整套解决方案，帮助开发者轻松构建和管理微服务，包括**服务注册与发现**、**配置管理**、**负载均衡**、**熔断机制**、**网关路由**、**消息驱动**等功能。
## 需求
---
在传统的单体架构中，应用部署在一个 War 包或者 Jar 包里，功能模块紧密耦合。

![[SpringCloud-1.png]]

随着业务的发展，系统越来越复杂，单体应用难以承载**高并发**、**高可用**、**快速迭代**的需求，于是提出了微服务架构：
- 每个模块拆分为**独立的服务**。
- 服务之间通过**网络通信**协作（如 REST、消息队列等）。
- 每个服务**独立开发**、**部署**、**拓展**、**维护**。

![[SpringCloud-2.png]]

微服务虽然带来了灵活性，但也引发了很多新问题，而 SpringCloud 提供了一系列框架用于解决这些问题：
- **服务注册与发现**
	- 问题：微服务数量多，地址（IP + 端口）经常变化，如何管理和发现这些服务？
	- 解决：使用 **Eureka**（或 **Nacos**） 作为**注册中心**，所有服务启动时自动注册，消费者通过服务名访问即可。
- **服务间调用**
	- 问题：每次写 HTTP 请求麻烦、冗余，代码可读性差，异常处理繁琐。
	- 解决：**OpenFeign** 提供声明式调用方式，像调用本地接口一样远程调用。
- **负载均衡**
	- 问题：一个服务部署了多个实例，如何将请求均匀地分配给它们？
	- 解决：整合 **Ribbon / Spring Cloud LoadBalancer**，自动实现客户端负载均衡。
- **配置中心**
	- 问题：多个服务有各自的配置文件，如何统一管理、动态刷新？
	- 解决：**Spring Cloud Config**（或 **Nacos**）提供集中式配置管理，支持从 Git、Nacos 读取配置，还可以通过 **Spring Cloud Bus** 实现配置的**动态刷新**。
- **服务容错（熔断/限流）**
	- 问题：某个服务挂了，调用方也卡住，造成雪崩效应。
	- 解决：使用 **Resilience4j（或 Hystrix）** 实现**熔断**、**限流**和**自动降级**。
- **网关统一入口**
	- 问题：外部客户端访问多个服务太混乱，如何统一入口、统一鉴权、限流、日志？
	- 解决：**Spring Cloud Gateway** 提供一个高性能的反应式网关，支持**路由转发**、**权限校验**、**请求限流**、**请求头过滤**、**日志打印**等功能。
- **链路追踪**
	- 问题：一个请求穿越多个服务，出了问题不知道是哪一步出错了，怎么排查？
	- 解决：使用 **Spring Cloud Sleuth + Zipkin** 提供统一的链路追踪，每个请求自动打上 TraceId 和 SpanId，便于排查问题。
- **消息驱动（异步通信）**
	- 问题：服务之间需要解耦，异步通信、事件驱动更灵活。
	- 解决：**Spring Cloud Stream** 统一整合 Kafka、RabbitMQ，简化消息通信开发。
- **服务治理与监控**
	- 问题：微服务太多，如何统一查看服务状态、健康状况、路由情况等？
	- 解决：集成 **Spring Boot Actuator**、**Admin Dashboard**、**Prometheus + Grafana**、**Zipkin**，实现监控可视化。
## 对比
---
就如同 Spring 是 Java 杀手级应用一样，对于 Java 微服务开发，SpringCloud 已经成为了事实上的标准，只不过是在解决具体问题上会更换不同的框架，例如使用 Dubbo 替代 OpenFeign等，这是视具体场景而选择的。
# 开始
---
## 安装
---

自Springboot 3.2开始，mysql驱动名称变为了mysql-connector-j，这是因为 Oracle 变更了驱动程序


## 使用
---





# 概念
---
## 分布式和微服务
---

## 远程调用
---
> [!warning]
> 远程调用时，服务名会编程域名（DNS名）的一部分，而域名只能由字母、数字和连字符组成！
### HTTP
---

#### RestTemplate
---
RestTemplate 是 Spring 提供的一个同步 HTTP 请求工具，底层基于 Java 的 **HttpURLConnection** 或 **Apache HttpClient**，用于发送 HTTP 请求，接收响应。

| 方法                                      | 作用                       | 常用场景         |
| --------------------------------------- | ------------------------ | ------------ |
| `getForObject(url, class)`              | 发GET请求，返回对象              | 获取数据         |
| `getForEntity(url, class)`              | 发GET请求，返回ResponseEntity  | 获取数据和响应头     |
| `postForObject(url, request, class)`    | 发POST请求，提交数据，返回对象        | 提交表单、JSON数据  |
| `postForEntity(url, request, class)`    | 发POST请求，返回ResponseEntity | 提交数据获取更多响应信息 |
| `exchange(url, method, request, class)` | 万能方法，支持所有请求方式            | 自定义复杂请求      |
| `delete(url)`                           | 发DELETE请求                | 删除资源         |

**使用方法：**
1. 配置 RestTemplate Bean
	```java
	@Configuration
	public class RestTemplateConfig {
	
	    @Bean
	    public RestTemplate restTemplate() {
	        return new RestTemplate();
	    }
	}
	```
2. 使用 @Autowired 注入
	```java
	@Autowired
	private RestTemplate restTemplate;
	```



### RPC
---



## 服务注册与发现
---
### Eureka
---
#### 简介
---
在微服务项目开发中，为了实现高可用，会对一个服务启动多个实例，每个实例 Ip 和端口都可能发生变化，如果硬编码在代码中，需要频繁更改代码，而使用 Eureka 则可以自动管理这些服务，并在调用时进行负载均衡。

![[SpringCloud-3.png]]

#### 角色
---
Eureka 具有两种角色：
- **Eureka Server**：注册中心（类似通讯录）
	```xml
	<dependency>  
	    <groupId>org.springframework.cloud</groupId>  
	    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId> 
	</dependency>
	```
- **Eureka Client**：具体服务（注册、发现、健康检测）
	```xml
	<dependency>
	    <groupId>org.springframework.cloud</groupId>
	    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
	</dependency>
	```

#### 工作机制
---
1. **服务上线**：自动将自己的信息注册到 Eureka Server，保存服务名称和服务实例地址列表的映射关系。
2. **服务调用**：根据服务名称拉取服务地址列表，并根据负载均衡算法（Ribbon（早期）或LoadBalancer）选择一个实例地址。
3. **服务宕机**：服务实例每隔一段时间（默认30秒）向 Eureka Server 发起请求，报告自己的状态，当服务宕机，超过一定时间没有发起请求时，Eureka Server 会将实例从服务列表中剔出。

#### Eureka Server 使用
---
1. 引入 Eureka Server 依赖
2. 在配置类或者启动类中加上 **`@EnableEurekaServer`** 注解。
3. 编写 Eureka Server 配置文件
	```yaml
	server:  
	  port: 8761  
	spring:  
	  application:  
	    name: eureka_server  
	eureka:  
	  client:  
	    register-with-eureka: false    # 自己是服务器，不注册  
	    fetch-registry: false          # 不去注册中心拉取服务  
	  server:  
	    enable-self-preservation: true # 开启自我保护机制
	```
4. 在具体服务中引入 Eureka Client 依赖
5. 自 Spring Boot 2.x 起，不加 **`@EnableEurekaClient`** 也能自动识别。
6. 编写 Eureka Client 配置文件
	```yaml
	eureka:  
	  client:  
	    service-url:  
	      defaultZone: http://127.0.0.1:8761/eureka/
	```

#### 负载均衡
---




### Nacos
---


## 配置中心
---
### Nacos
---



## 负载均衡
---




# 配置
---






# 附录
---
> [!website]
    > 

> [!code]
    > 

> [!video]
    > 

> [!doc]
    > [单体架构、分布式架构、微服务架构、Serverless架构](https://crimsonromance.github.io/2019/03/23/%E5%9B%9B%E7%A7%8D%E8%BD%AF%E4%BB%B6%E6%9E%B6%E6%9E%84%EF%BC%9A%E5%8D%95%E4%BD%93%E6%9E%B6%E6%9E%84%E3%80%81%E5%88%86%E5%B8%83%E5%BC%8F%E6%9E%B6%E6%9E%84%E3%80%81%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9E%B6%E6%9E%84%E3%80%81Serverless%E6%9E%B6%E6%9E%84/)














# 4.Ribbon负载均衡

上一节中，我们添加了@LoadBalanced注解，即可实现负载均衡功能，这是什么原理呢？

# 4.1 负载均衡原理

SpringCloud底层其实是利用了一个名为Ribbon的组件，来实现负载均衡功能的。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666338752023-b80d73a8-84e4-4766-bd39-7b677a6f8a2a.png)

那么我们发出的请求明明是http://userservice/user/1，怎么变成了[](http://localhost:8081/)[http://localhost:8081](http://localhost:8081)的呢？

# 4.2 源码跟踪

为什么我们只输入了service名称就可以访问了呢？之前还要获取ip和端口。

显然有人帮我们根据service名称，获取到了服务实例的ip和端口。它就是**LoadBalancerInterceptor**，这个类会在对RestTemplate的请求进行拦截，然后从Eureka根据服务id获取服务列表，随后利用负载均衡算法得到真实的服务地址信息，替换服务id。

我们进行源码跟踪：

### 4.2.1 LoadBalancerInterceptor

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666338984080-650d089c-e817-4ff0-ab8f-987d7b7b2105.png)

可以看到这里的intercept方法，拦截了用户的HttpRequest请求，然后做了几件事：

- **request.getURI()**：获取请求uri，本例中就是 [http://user-service/user/8](http://user-service/user/8)
- **originalUri.getHost()**：获取uri路径的主机名，其实就是服务id，**user-service**
- **this.loadBalancer.execute()**：处理服务id，和用户请求。

这里的**this.loadBalancer**是**LoadBalancerClient**类型，我们继续跟入。

### 4.2.2 LoadBalancerClient

继续跟入execute方法：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666339077330-59669aff-e591-4493-985f-db14ed194e50.png)

代码是这样的：

- getLoadBalancer(serviceId)：根据服务id获取ILoadBalancer，而ILoadBalancer会拿着服务id去eureka中获取服务列表并保存起来。
- getServer(loadBalancer)：利用内置的负载均衡算法，从服务列表中选择一个。本例中，可以看到获取了8082端口的服务

放行后，再次访问并跟踪，发现获取的是8081：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666339125836-f20e4067-f503-48bf-a5ed-d59805f6fcc9.png)

果然实现了负载均衡。

### 4.2.3 负载均衡策略IRule

在刚才的代码中，可以看到获取服务使通过一个**getServer**方法来做负载均衡:

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666339221443-3ecb9a1c-501f-4fc5-86dc-b7b353f5e721.png)

我们继续跟入：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666339240048-7d78ab62-ce1a-4a0e-98a3-6e0d8466e0c8.png)

继续跟踪源码chooseServer方法，发现这么一段代码：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666339257952-924848ba-6388-4b4c-bc73-a3fea7e78d32.png)

我们看看这个rule是谁：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666339283109-7321872f-e932-493a-b76b-1f1bf833a099.png)

这里的rule默认值是一个**RoundRobinRule**，看类的介绍：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666339298744-0c842082-e8f2-4fa7-84f0-7310e30b822f.png)

这不就是轮询的意思嘛。

到这里，整个负载均衡的流程我们就清楚了。

### 4.2.4 总结

SpringCloudRibbon的底层采用了一个拦截器，拦截了RestTemplate发出的请求，对地址做了修改。用一幅图来总结一下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666339346784-1054afe4-30e2-4398-ab26-7e8e0ede4f95.png)

基本流程如下：

- 拦截我们的RestTemplate请求http://userservice/user/1
- RibbonLoadBalancerClient会从请求url中获取服务名称，也就是user-service
- DynamicServerListLoadBalancer根据user-service到eureka拉取服务列表
- eureka返回列表，localhost:8081、localhost:8082
- IRule利用内置负载均衡规则，从列表中选择一个，例如localhost:8081
- RibbonLoadBalancerClient修改请求地址，用localhost:8081替代userservice，得到http://localhost:8081/user/1，发起真实请求

# 4.3 负载均衡策略

### 4.3.1 负载均衡策略

负载均衡的规则都定义在IRule接口中，而IRule有很多不同的实现类：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666339398598-b5ff7dc5-2a61-424e-8452-191bba68b663.png)

不同规则的含义如下：

|**内置负载均衡规则类**|**规则描述**|
|---|---|
|RoundRobinRule|简单轮询服务列表来选择服务器。它是Ribbon默认的负载均衡规则。|
|AvailabilityFilteringRule|对以下两种服务器进行忽略： （1）在默认情况下，这台服务器如果3次连接失败，这台服务器就会被设置为“短路”状态。短路状态将持续30秒，如果再次连接失败，短路的持续时间就会几何级地增加。 （2）并发数过高的服务器。如果一个服务器的并发连接数过高，配置了AvailabilityFilteringRule规则的客户端也会将其忽略。并发连接数的上限，可以由客户端的<clientName>.<clientConfigNameSpace>.ActiveConnectionsLimit属性进行配置。|
|WeightedResponseTimeRule|为每一个服务器赋予一个权重值。服务器响应时间越长，这个服务器的权重就越小。这个规则会随机选择服务器，这个权重值会影响服务器的选择。|
|**ZoneAvoidanceRule**|以区域可用的服务器为基础进行服务器的选择。使用Zone对服务器进行分类，这个Zone可以理解为一个机房、一个机架等。而后再对Zone内的多个服务做轮询。|
|BestAvailableRule|忽略那些短路的服务器，并选择并发数较低的服务器。|
|RandomRule|随机选择一个可用的服务器。|
|RetryRule|重试机制的选择逻辑|

默认的实现就是ZoneAvoidanceRule，是一种轮询方案

### 4.3.2自定义负载均衡策略

通过定义IRule实现可以修改负载均衡规则，有两种方式：

①代码方式：在order-service中的OrderApplication类中，定义一个新的IRule：

```java
@Bean
public IRule randomRule(){
    return new RandomRule();
}
```

②配置文件方式：在order-service的application.yml文件中，添加新的配置也可以修改规则：

```yaml
userservice: # 给某个微服务配置负载均衡规则，这里是userservice服务
  ribbon:
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule # 负载均衡规则
```

**注意**，一般用默认的负载均衡规则，不做修改。

# 4.4 饥饿加载

Ribbon默认是采用懒加载，即第一次访问时才会去创建LoadBalanceClient，请求时间会很长。

而饥饿加载则会在项目启动时创建，降低第一次访问的耗时，通过下面配置开启饥饿加载：

```yaml
ribbon:
  eager-load:
    enabled: true
    clients: userservice
```

# 5.Nacos注册中心

国内公司一般都推崇阿里巴巴的技术，比如注册中心，SpringCloudAlibaba也推出了一个名为Nacos的注册中心。

# 5.1 认识安装Nacos

[Nacos](https://nacos.io/)是阿里巴巴的产品，现在是[SpringCloud](https://spring.io/projects/spring-cloud)中的一个组件。相比[Eureka](https://github.com/Netflix/eureka)功能更加丰富，在国内受欢迎程度较高。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666339773418-bb38ebaf-2bdc-4090-8ecb-5cd5a6058f62.png)

### 5.1.1 Windows安装

开发阶段采用单机安装即可。

**1）下载安装包**

在Nacos的GitHub页面，提供有下载链接，可以下载编译好的Nacos服务端或者源代码：

GitHub主页：[https://github.com/alibaba/nacos](https://github.com/alibaba/nacos)

GitHub的Release下载页：[https://github.com/alibaba/nacos/releases](https://github.com/alibaba/nacos/releases)

如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666339903244-a58c1738-d73b-413b-a587-84a3c27aa274.png)

windows版本使用**nacos-server-1.4.1.zip**包即可。

**2）解压**

将这个包解压到任意非中文目录下，如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666339961127-d5859883-171a-4086-96c8-0bcccb6fef45.png)

目录说明：

- bin：启动脚本
- conf：配置文件

**3）端口设置**

Nacos的默认端口是8848，如果你电脑上的其它进程占用了8848端口，请先尝试关闭该进程。

**如果无法关闭占用8848端口的进程**，也可以进入nacos的conf目录，修改配置文件中的端口：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666340040850-c8b688cc-584a-4735-bb17-733ae123f636.png)

修改其中的内容：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666340055726-819396f5-0cf2-4bc5-b777-d3ebe944875f.png)

**4）启动**

启动非常简单，进入bin目录，结构如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666340099412-4da4625e-bd20-42b3-881f-69bd775711fe.png)

然后执行命令即可：

windows命令：

```powershell
startup.cmd -m standalone
```

执行后的效果如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666340162288-8adb890b-5347-4004-b421-444142d990e0.png)

**5）访问**

在浏览器输入地址：[http://127.0.0.1:8848/nacos即可：](http://127.0.0.1:8848/nacos%E5%8D%B3%E5%8F%AF%EF%BC%9A)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666340202657-20342e44-6918-41c9-838a-de53014e7a7c.png)

默认的账号和密码都是nacos，进入后：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666340219868-2f0e756c-c56b-4200-be9a-1fce111d6bdf.png)

### 5.2.2 Linux安装

Linux或者Mac安装方式与Windows类似。

**1）安装JDK**

Nacos依赖于JDK运行，索引Linux上也需要安装JDK才行。

将JDK安装包上传到某个目录，例如：**/usr/local/**

然后解压缩：

```
tar -xvf jdk-8u144-linux-x64.tar.gz
```

配置环境变量：

```
export JAVA_HOME=/usr/local/java
export PATH=$PATH:$JAVA_HOME/bin
```

重新加载配置：

```
source /etc/profile
```

**2）上传安装包**

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666340553331-879a87de-2134-4026-a893-c455ce79182d.png)

上传到Linux服务器的某个目录，例如**/usr/local/src**目录下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666340577417-892f3a42-080e-4ccb-89e9-895e479c7cad.png)

**3）解压**

命令解压缩安装包：

```
tar -xvf nacos-server-1.4.1.tar.gz
```

然后删除安装包：

```
rm -rf nacos-server-1.4.1.tar.gz
```

目录中最终样式：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666340652712-215307fd-5ed4-4a75-b5b2-998930b01c7b.png)

目录内部：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666340666165-62a9f26b-7303-48b3-9161-270c9ac19451.png)

**4）端口设置**

与windows中类似

**5）启动**

在nacos/bin目录中，输入命令启动Nacos：

```
sh startup.sh -m standalone
```

# 5.2 服务注册到nacos

Nacos是SpringCloudAlibaba的组件，而SpringCloudAlibaba也遵循SpringCloud中定义的服务注册、服务发现规范。因此使用Nacos和使用Eureka对于微服务来说，并没有太大区别。

主要差异在于：

- 依赖不同
- 服务地址不同

### 5.2.1 引入依赖

在cloud-demo父工程的pom文件中的`<dependencyManagement>`中引入SpringCloudAlibaba的依赖：

``` xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-alibaba-dependencies</artifactId>
    <version>2.2.6.RELEASE</version>
    <type>pom</type>
    <scope>import</scope>
</dependency>
```

然后在user-service和order-service中的pom文件中引入nacos-discovery依赖：

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

**注意：不要忘了注释掉eureka的依赖。

### 5.2.2 配置nacos地址

在user-service和order-service的application.yml中添加nacos地址：

```yaml
spring:
  cloud:
    nacos:
      server-addr: localhost:8848
```

**注意**：不要忘了注释掉eureka的地址

### 5.2.3 重启

重启微服务后，登录nacos管理页面，可以看到微服务信息：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666341047121-b8078735-08aa-4ed2-abf2-1d1130da842c.png)

# 5.3 服务分级存储模型

一个**服务**可以有多个**实例**，例如我们的user-service，可以有:

- 127.0.0.1:8081
- 127.0.0.1:8082
- 127.0.0.1:8083

假如这些实例分布于全国各地的不同机房，例如：

- 127.0.0.1:8081，在上海机房
- 127.0.0.1:8082，在上海机房
- 127.0.0.1:8083，在杭州机房

Nacos就将同一机房内的实例 划分为一个**集群**。

也就是说，user-service是服务，一个服务可以包含多个集群，如杭州、上海，每个集群下可以有多个实例，形成分级模型，如图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666341095036-64937e21-6a7a-4852-b559-96cb7b01843f.png)

微服务互相访问时，应该尽可能访问同集群实例，因为本地访问速度更快。当本集群内不可用时，才访问其它集群。例如：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666341117260-a5411b07-816c-425c-ace0-0c85f0ad75ce.png)

杭州机房内的order-service应该优先访问同机房的user-service。

### 5.3.1 给user-service配置集群

修改user-service的application.yml文件，添加集群配置：

```yaml
spring:
  cloud:
    nacos:
      server-addr: localhost:8848
      discovery:
        cluster-name: HZ # 集群名称
```

重启两个user-service实例后，我们可以在nacos控制台看到下面结果：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666343612761-ed89e052-f31e-4521-aa56-d956b60be3c7.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666343560076-980f6b91-b450-4074-b83f-a6a3e6aebadd.png)

我们再次复制一个user-service启动配置，添加属性：

```powershell
-Dserver.port=8083 -Dspring.cloud.nacos.discovery.cluster-name=SH
```

配置如图所示：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666341265982-acd2f868-850b-42d2-8fc9-dc9dd870d6de.png)

启动UserApplication3后再次查看nacos控制台：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666341307882-e699edde-f611-4a56-94f1-52e7094b4808.png)

### 5.3.2 同集群优先的负载均衡

默认的**ZoneAvoidanceRule**并不能实现根据同集群优先来实现负载均衡。

因此Nacos中提供了一个**NacosRule**的实现，可以优先从同集群中挑选实例。

1）给order-service配置集群信息

修改order-service的application.yml文件，添加集群配置：

```yaml
spring:
  cloud:
    nacos:
      server-addr: localhost:8848
      discovery:
        cluster-name: HZ # 集群名称
```

2）修改负载均衡规则

修改order-service的application.yml文件，修改负载均衡规则：

```yaml
userservice:
  ribbon:
    NFLoadBalancerRuleClassName: com.alibaba.cloud.nacos.ribbon.NacosRule # 负载均衡规则
```

# 5.4 权重配置

实际部署中会出现这样的场景：

服务器设备性能有差异，部分实例所在机器性能较好，另一些较差，我们希望性能好的机器承担更多的用户请求。

但默认情况下NacosRule是同集群内随机挑选，不会考虑机器的性能问题。

因此，Nacos提供了权重配置来控制访问频率，权重越大则访问频率越高。

在nacos控制台，找到user-service的实例列表，点击编辑，即可修改权重：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666341461498-9e800905-9cf9-4081-a119-87b8aab02758.png)

在弹出的编辑窗口，修改权重：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666341481499-5997fe4f-ddfe-482c-ad99-62c168e2f607.png)

**注意**：如果权重修改为0，则该实例永远不会被访问

# 5.5 环境隔离

Nacos提供了namespace来实现环境隔离功能。

- nacos中可以有多个namespace
- namespace下可以有group、service等
- 不同namespace之间相互隔离，例如不同namespace的服务互相不可见

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666341530556-e5bb0efe-d90f-4dab-b56a-44118bf0b234.png)

### 5.5.1 创建namespace

默认情况下，所有service、data、group都在同一个namespace，名为public：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666341570772-927f67ec-2efe-470c-8781-75c7389c7127.png)

我们可以点击页面新增按钮，添加一个namespace：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666341585992-81c35555-33c3-4bc1-a1db-1f1f23b3cf90.png)

然后，填写表单：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666341600906-ac586fe2-4c90-4c35-a48a-85661363b7f8.png)

就能在页面看到一个新的namespace：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666341615669-04a7723d-3ded-46eb-ad5b-b87c514e7566.png)

### 5.5.2 给微服务配置namespace

给微服务配置namespace只能通过修改配置来实现。

例如，修改order-service的application.yml文件：

```yaml
spring:
  cloud:
    nacos:
      server-addr: localhost:8848
      discovery:
        cluster-name: HZ
        namespace: 492a7d5d-237b-46a1-a99a-fa8e98e4b0f9 # 命名空间，填ID
```

重启order-service后，访问控制台，可以看到下面的结果：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666341676220-6f1444d4-7d93-4080-a95e-2f529ca0fa29.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666341684437-aa8c956f-34b8-42db-aa42-c92a629ee41b.png)

此时访问order-service，因为namespace不同，会导致找不到userservice，控制台会报错：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666341706135-2e5a5ce9-7964-49c0-abb9-69844f618279.png)

# 5.6 Nacos与Eureka区别

Nacos的服务实例分为两种l类型：

- 临时实例：如果实例宕机超过一定时间，会从服务列表剔除，默认的类型。
- 非临时实例：如果实例宕机，不会从服务列表剔除，也可以叫永久实例。

配置一个服务实例为永久实例：

```yaml
spring:
  cloud:
    nacos:
      discovery:
        ephemeral: false # 设置为非临时实例
```

Nacos和Eureka整体结构类似，服务注册、服务拉取、心跳等待，但是也存在一些差异：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666341912716-89d76ed6-511d-42b6-832b-774fe0ca628d.png)

- Nacos与eureka的共同点
- 都支持服务注册和服务拉取
- 都支持服务提供者心跳方式做健康检测
- Nacos与Eureka的区别
- Nacos支持服务端主动检测提供者状态：临时实例采用心跳模式，非临时实例采用主动检测模式
- 临时实例心跳不正常会被剔除，非临时实例则不会被剔除
- Nacos支持服务列表变更的消息推送模式，服务列表更新更及时
- Nacos集群默认采用AP方式，当集群中存在非临时实例时，采用CP模式；Eureka采用AP方式

# 6.Nacos配置管理

Nacos除了可以做注册中心，同样可以做配置管理来使用。

# 6.1 统一配置管理

当微服务部署的实例越来越多，达到数十、数百时，逐个修改微服务配置就会让人抓狂，而且很容易出错。我们需要一种统一配置管理方案，可以集中管理所有实例的配置。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666345341541-25ccb287-156e-48e3-b8b7-83c81c5a31fc.png)

Nacos一方面可以将配置集中管理，另一方可以在配置变更时，及时通知微服务，实现配置的热更新。

### 6.1.1 在nacos中添加配置文件

如何在nacos中管理配置呢？

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666345732266-4c6e3762-cb69-42c2-84ca-a12659a8e86a.png)

然后在弹出的表单中，填写配置信息：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666345752195-d3ba10c6-0bff-4504-9a4e-54b565a672b4.png)

**注意**：项目的核心配置，需要热更新的配置才有放到nacos管理的必要。基本不会变更的一些配置还是保存在微服务本地比较好。

**注意**：注意环境隔离，小心配置文件加入的namespace

### 6.1.2 从微服务拉取配置

微服务要拉取nacos中管理的配置，并且与本地的application.yml配置合并，才能完成项目启动。

但如果尚未读取application.yml，又如何得知nacos地址呢？

因此spring引入了一种新的配置文件：bootstrap.yaml文件，会在application.yml之前被读取，流程如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666345953140-9f138fe3-98f0-4dfc-ba99-f1ba548a392a.png)

1）引入nacos-config依赖

首先，在user-service服务中，引入nacos-config的客户端依赖：

```xml
<!--nacos配置管理依赖-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```

2）添加bootstrap.yaml

然后，在user-service中添加一个bootstrap.yaml文件，内容如下：

```yaml
spring:
  application:
    name: userservice # 服务名称
  profiles:
    active: dev #开发环境，这里是dev
  cloud:
    nacos:
      server-addr: localhost:8848 # Nacos地址
      config:
        file-extension: yaml # 文件后缀名
```

这里会根据spring.cloud.nacos.server-addr获取nacos地址，再根据

**${[spring.application.name](http://spring.application.name)}-${spring.profiles.active}.${spring.cloud.nacos.config.file-extension}**作为文件id，来读取配置。

本例中，就是去读取**userservice-dev.yaml**：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666346036023-83f1c7d2-38d0-4923-a477-88470a22f858.png)

3）读取nacos配置

在user-service中的UserController中添加业务逻辑，读取pattern.dateformat配置：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666346055395-a0433568-26a4-448c-a59b-7d1c9a1219ee.png)

完整代码：

```java
package cn.itcast.user.web;

import cn.itcast.user.pojo.User;
import cn.itcast.user.service.UserService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.*;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

@Slf4j
@RestController
@RequestMapping("/user")
public class UserController {

    @Autowired
    private UserService userService;

    @Value("${pattern.dateformat}")
    private String dateformat;

    @GetMapping("now")
    public String now(){
        return LocalDateTime.now().format(DateTimeFormatter.ofPattern(dateformat));
    }
    // ...略
}
```

在页面访问，可以看到效果：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666346108897-7c0d1c2a-4e4e-4676-a5de-43cc291669a6.png)

# 6.2 配置热更新

我们最终的目的，是修改nacos中的配置后，微服务中无需重启即可让配置生效，也就是**配置热更新**。

要实现配置热更新，可以使用两种方式：

### 6.2.1 方法一

在@Value注入的变量所在类上添加注解@RefreshScope：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666346162651-85ef3d59-2b02-4bf0-a8cc-140e5df2d634.png)

### 6.2.2 方式二

使用@ConfigurationProperties注解代替@Value注解。

在user-service服务中，添加一个类，读取patterrn.dateformat属性：

```java
package cn.itcast.user.config;

import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@Data
@ConfigurationProperties(prefix = "pattern")
public class PatternProperties {
    private String dateformat;
}
```

在UserController中使用这个类代替@Value：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666346205680-4bddee89-b989-4906-929a-aa4518430892.png)

完整代码：

```java
package cn.itcast.user.web;

import cn.itcast.user.config.PatternProperties;
import cn.itcast.user.pojo.User;
import cn.itcast.user.service.UserService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

@Slf4j
@RestController
@RequestMapping("/user")
public class UserController {

    @Autowired
    private UserService userService;

    @Autowired
    private PatternProperties patternProperties;

    @GetMapping("now")
    public String now(){
        return LocalDateTime.now().format(DateTimeFormatter.ofPattern(patternProperties.getDateformat()));
    }

    // 略
}
```

# 6.3 配置共享

其实微服务启动时，会去nacos读取多个配置文件，例如：

- **[[spring.application.name](http://spring.application.name)]-[spring.profiles.active].yaml**，例如：userservice-dev.yaml
- **[[spring.application.name](http://spring.application.name)].yaml**，例如：userservice.yaml

而**[[spring.application.name](http://spring.application.name)].yaml**不包含环境，因此可以被多个环境共享。

**注意**：注意环境隔离

下面我们通过案例来测试配置共享

**1）添加环境共享配置**

我们在nacos中添加一个userservice.yaml文件：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666346303499-62d65541-488b-4462-be54-f13d5b5fbb9a.png)

**2） 在user-service中读取共享配置**

在user-service服务中，修改PatternProperties类，读取新添加的属性：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666346366242-76b57493-a3e8-4a98-aacf-d08f141dc6bc.png)

在user-service服务中，修改UserController，添加一个方法：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666346381468-d14ef6f3-289d-4023-9c2c-0825d316a22c.png)

**3）运行两个UserApplication，使用不同的profile**

修改UserApplication2这个启动项，改变其profile值：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666346499520-0014bdc9-4b02-4f64-a535-4950db4f4fd1.png)

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666346549674-fb5af169-8a30-49b9-8fd6-b8d66bb20b63.png)

这样，UserApplication(8081)使用的profile是dev，UserApplication2(8082)使用的profile是test。

启动UserApplication和UserApplication2

访问http://localhost:8081/user/prop，结果：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666346571251-6d284f7b-9664-4af5-a276-45f720c73809.png)

访问http://localhost:8082/user/prop，结果：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666346585583-67b13c57-97f4-447a-bcc1-171fd56c00d3.png)

可以看出来，不管是dev，还是test环境，都读取到了envSharedValue这个属性的值。

**4）配置共享的优先级**

当nacos、服务本地同时出现相同属性时，优先级有高低之分：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666346633268-65983168-e9b4-4b73-b305-759d58b9d0e4.png)

# 6.4 搭建nacos集群

### 6.4.1 集群结构图

官方给出的Nacos集群图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666422793636-4fd30938-fb15-4626-9c9e-5e7e820b7a79.png)

其中包含3个nacos节点，然后一个负载均衡器代理3个Nacos。这里负载均衡器可以使用nginx。

我们计划的集群结构：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666422861963-4a67e55e-ff3b-40e7-8f0b-3cd989f1d59e.png)

三个nacos节点的地址：

|节点|ip|port|
|---|---|---|
|nacos1|192.168.150.1|8845|
|nacos2|192.168.150.1|8846|
|nacos3|192.168.150.1|8847|

### 6.4.2 搭建集群

搭建集群的基本步骤：

- 搭建数据库，初始化数据库表结构
- 下载nacos安装包
- 配置nacos
- 启动nacos集群
- nginx反向代理

**1）初始化数据库**

Nacos默认数据存储在内嵌数据库Derby中，不属于生产可用的数据库。

官方推荐的最佳实践是使用带有主从的高可用数据库集群，主从模式的高可用数据库可以参考**传智教育**的后续高手课程。

这里我们以单点的数据库为例来讲解。

首先新建一个数据库，命名为nacos，而后导入下面的SQL：

```sql
CREATE TABLE `config_info` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(255) DEFAULT NULL,
  `content` longtext NOT NULL COMMENT 'content',
  `md5` varchar(32) DEFAULT NULL COMMENT 'md5',
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '修改时间',
  `src_user` text COMMENT 'source user',
  `src_ip` varchar(50) DEFAULT NULL COMMENT 'source ip',
  `app_name` varchar(128) DEFAULT NULL,
  `tenant_id` varchar(128) DEFAULT '' COMMENT '租户字段',
  `c_desc` varchar(256) DEFAULT NULL,
  `c_use` varchar(64) DEFAULT NULL,
  `effect` varchar(64) DEFAULT NULL,
  `type` varchar(64) DEFAULT NULL,
  `c_schema` text,
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_configinfo_datagrouptenant` (`data_id`,`group_id`,`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_info';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_info_aggr   */
/******************************************/
CREATE TABLE `config_info_aggr` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(255) NOT NULL COMMENT 'group_id',
  `datum_id` varchar(255) NOT NULL COMMENT 'datum_id',
  `content` longtext NOT NULL COMMENT '内容',
  `gmt_modified` datetime NOT NULL COMMENT '修改时间',
  `app_name` varchar(128) DEFAULT NULL,
  `tenant_id` varchar(128) DEFAULT '' COMMENT '租户字段',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_configinfoaggr_datagrouptenantdatum` (`data_id`,`group_id`,`tenant_id`,`datum_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='增加租户字段';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_info_beta   */
/******************************************/
CREATE TABLE `config_info_beta` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(128) NOT NULL COMMENT 'group_id',
  `app_name` varchar(128) DEFAULT NULL COMMENT 'app_name',
  `content` longtext NOT NULL COMMENT 'content',
  `beta_ips` varchar(1024) DEFAULT NULL COMMENT 'betaIps',
  `md5` varchar(32) DEFAULT NULL COMMENT 'md5',
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '修改时间',
  `src_user` text COMMENT 'source user',
  `src_ip` varchar(50) DEFAULT NULL COMMENT 'source ip',
  `tenant_id` varchar(128) DEFAULT '' COMMENT '租户字段',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_configinfobeta_datagrouptenant` (`data_id`,`group_id`,`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_info_beta';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_info_tag   */
/******************************************/
CREATE TABLE `config_info_tag` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(128) NOT NULL COMMENT 'group_id',
  `tenant_id` varchar(128) DEFAULT '' COMMENT 'tenant_id',
  `tag_id` varchar(128) NOT NULL COMMENT 'tag_id',
  `app_name` varchar(128) DEFAULT NULL COMMENT 'app_name',
  `content` longtext NOT NULL COMMENT 'content',
  `md5` varchar(32) DEFAULT NULL COMMENT 'md5',
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '修改时间',
  `src_user` text COMMENT 'source user',
  `src_ip` varchar(50) DEFAULT NULL COMMENT 'source ip',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_configinfotag_datagrouptenanttag` (`data_id`,`group_id`,`tenant_id`,`tag_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_info_tag';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_tags_relation   */
/******************************************/
CREATE TABLE `config_tags_relation` (
  `id` bigint(20) NOT NULL COMMENT 'id',
  `tag_name` varchar(128) NOT NULL COMMENT 'tag_name',
  `tag_type` varchar(64) DEFAULT NULL COMMENT 'tag_type',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(128) NOT NULL COMMENT 'group_id',
  `tenant_id` varchar(128) DEFAULT '' COMMENT 'tenant_id',
  `nid` bigint(20) NOT NULL AUTO_INCREMENT,
  PRIMARY KEY (`nid`),
  UNIQUE KEY `uk_configtagrelation_configidtag` (`id`,`tag_name`,`tag_type`),
  KEY `idx_tenant_id` (`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_tag_relation';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = group_capacity   */
/******************************************/
CREATE TABLE `group_capacity` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `group_id` varchar(128) NOT NULL DEFAULT '' COMMENT 'Group ID，空字符表示整个集群',
  `quota` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '配额，0表示使用默认值',
  `usage` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '使用量',
  `max_size` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '单个配置大小上限，单位为字节，0表示使用默认值',
  `max_aggr_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '聚合子配置最大个数，，0表示使用默认值',
  `max_aggr_size` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '单个聚合数据的子配置大小上限，单位为字节，0表示使用默认值',
  `max_history_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '最大变更历史数量',
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_group_id` (`group_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='集群、各Group容量信息表';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = his_config_info   */
/******************************************/
CREATE TABLE `his_config_info` (
  `id` bigint(64) unsigned NOT NULL,
  `nid` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `data_id` varchar(255) NOT NULL,
  `group_id` varchar(128) NOT NULL,
  `app_name` varchar(128) DEFAULT NULL COMMENT 'app_name',
  `content` longtext NOT NULL,
  `md5` varchar(32) DEFAULT NULL,
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `src_user` text,
  `src_ip` varchar(50) DEFAULT NULL,
  `op_type` char(10) DEFAULT NULL,
  `tenant_id` varchar(128) DEFAULT '' COMMENT '租户字段',
  PRIMARY KEY (`nid`),
  KEY `idx_gmt_create` (`gmt_create`),
  KEY `idx_gmt_modified` (`gmt_modified`),
  KEY `idx_did` (`data_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='多租户改造';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = tenant_capacity   */
/******************************************/
CREATE TABLE `tenant_capacity` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `tenant_id` varchar(128) NOT NULL DEFAULT '' COMMENT 'Tenant ID',
  `quota` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '配额，0表示使用默认值',
  `usage` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '使用量',
  `max_size` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '单个配置大小上限，单位为字节，0表示使用默认值',
  `max_aggr_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '聚合子配置最大个数',
  `max_aggr_size` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '单个聚合数据的子配置大小上限，单位为字节，0表示使用默认值',
  `max_history_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '最大变更历史数量',
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_tenant_id` (`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='租户容量信息表';

CREATE TABLE `tenant_info` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `kp` varchar(128) NOT NULL COMMENT 'kp',
  `tenant_id` varchar(128) default '' COMMENT 'tenant_id',
  `tenant_name` varchar(128) default '' COMMENT 'tenant_name',
  `tenant_desc` varchar(256) DEFAULT NULL COMMENT 'tenant_desc',
  `create_source` varchar(32) DEFAULT NULL COMMENT 'create_source',
  `gmt_create` bigint(20) NOT NULL COMMENT '创建时间',
  `gmt_modified` bigint(20) NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_tenant_info_kptenantid` (`kp`,`tenant_id`),
  KEY `idx_tenant_id` (`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='tenant_info';

CREATE TABLE `users` (
	`username` varchar(50) NOT NULL PRIMARY KEY,
	`password` varchar(500) NOT NULL,
	`enabled` boolean NOT NULL
);

CREATE TABLE `roles` (
	`username` varchar(50) NOT NULL,
	`role` varchar(50) NOT NULL,
	UNIQUE INDEX `idx_user_role` (`username` ASC, `role` ASC) USING BTREE
);

CREATE TABLE `permissions` (
    `role` varchar(50) NOT NULL,
    `resource` varchar(255) NOT NULL,
    `action` varchar(8) NOT NULL,
    UNIQUE INDEX `uk_role_permission` (`role`,`resource`,`action`) USING BTREE
);

INSERT INTO users (username, password, enabled) VALUES ('nacos', '$2a$10$EuWPZHzz32dJN7jexM34MOeYirDdFAZm2kuWj7VEOJhhZkDrxfvUu', TRUE);

INSERT INTO roles (username, role) VALUES ('nacos', 'ROLE_ADMIN');
```

**2）配置nacos**

进入nacos的conf目录，修改配置文件cluster.conf.example，重命名为cluster.conf：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666423055280-f2702c89-5081-4cde-a3c3-fcf6f76cdfc4.png)

然后添加内容：

```sql
127.0.0.1:8845
127.0.0.1.8846
127.0.0.1.8847
```

然后修改application.properties文件，添加数据库配置

```
spring.datasource.platform=mysql

db.num=1

db.url.0=jdbc:mysql://127.0.0.1:3306/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useUnicode=true&useSSL=false&serverTimezone=UTC
db.user.0=root
db.password.0=123
```

**3）启动**

将nacos文件夹复制三份，分别命名为：nacos1、nacos2、nacos3

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666423148716-87fe8e01-83ff-457e-a631-64f77cdb6549.png)

然后分别修改三个文件夹中的application.properties，

nacos1:

```
server.port=8845
```

nacos2:

```
server.port=8846
```

nacos3:

```
server.port=8847
```

然后分别启动三个nacos节点：

```
startup.cmd
```

**4）Nginx反向代理**

nginx安装包解压到任意非中文目录下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666423320700-09de654e-af77-44ac-a455-af6a9c23d0d1.png)

修改conf/nginx.conf文件，配置如下：

```
upstream nacos-cluster {
    server 127.0.0.1:8845;
	server 127.0.0.1:8846;
	server 127.0.0.1:8847;
}

server {
    listen       80;
    server_name  localhost;

    location /nacos {
        proxy_pass <http://nacos-cluster>;
    }
}
```

而后在浏览器访问：[http://localhost/nacos即可。](http://localhost/nacos%E5%8D%B3%E5%8F%AF%E3%80%82)

代码中application.yml文件配置如下：

```yaml
spring:
  cloud:
    nacos:
      server-addr: localhost:80 # Nacos地址
```

**5）优化**

- 实际部署时，需要给做反向代理的nginx服务器设置一个域名，这样后续如果有服务器迁移nacos的客户端也无需更改配置.
- Nacos的各个节点应该部署到多个不同服务器，做好容灾和隔离

# 7.Feign远程调用

先来看我们以前利用 RestTemplate 发起远程调用的代码：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666346780080-3e20f2e5-f29e-462e-8daa-e0b03a046b6b.png)

存在下面的问题：

- 代码可读性差，编程体验不统一
- 参数复杂URL难以维护

对于远程调用有很多实现办法，如：Web接口、RestTemplate+OKHttp、Feign、RPC调用（Dubbo、Socket编程）、WebService

在微服务技术架构中使用最多的是 **Feign** 和 **Dubbo**，Feign相对来说更加简便，如果追求灵活、高并发和更多功能可以使用Dubbo

Feign 是一个**声明式的 http 客户端**，[官方地址](https://www.notion.so/GitHub%20-%20OpenFeign/feign:%20Feign%20makes%20writing%20java%20http%20clients%20easier)。

其作用就是帮助我们优雅的实现 http 请求的发送，解决上面提到的问题。

# 7.1 Feign替代RestTemplate

Fegin的使用步骤如下：

### 7.1.1 引入依赖

我们在order-service服务（**调用发起者**）的pom文件中引入feign的依赖：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

### 7.1.2 添加注解

在order-service的启动类添加注解开启Feign的功能：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666346937212-ec673e25-816e-4a04-ad4e-97246dcbe357.png)

### 7.1.3 编写Feign客户端接口

在order-service中新建一个**接口**，内容如下：

```java
package cn.itcast.order.client;

import cn.itcast.order.pojo.User;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@FeignClient("userservice")
public interface UserClient {
    @GetMapping("/user/{id}")
    User findById(@PathVariable("id") Long id);
}
```

nacos很好的兼容了feign，可以从nacos拉取到注册服务列表，接口中申明了调用方法

这个客户端主要是基于SpringMVC的注解来声明远程调用的信息，比如：

- **服务名称**：userservice
- **请求方式**：GET
- **请求路径**：/user/{id}
- **请求参数**：Long id
- **返回值类型**：User

这样，Feign就可以帮助我们发送http请求，无需自己使用RestTemplate来发送了。

### 7.1.4 使用接口方法

修改order-service中的OrderService类中的queryOrderById方法，使用我们刚才自定义的feign客户端代替RestTemplate：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666347009980-2f5de34e-16c0-45c2-a724-4216ac560e81.png)

是不是看起来优雅多了。

# 7.2 自定义配置

Feign可以支持很多的自定义配置，如下表所示：

|类型|作用|说明|
|---|---|---|
|`feign.Logger.Level`|修改日志级别|包含四种不同的级别：NONE、BASIC、HEADERS、FULL|
|`feign.codec.Decoder`|响应结果的解析器|http远程调用的结果做解析，例如解析json字符串为java对象|
|`feign.codec.Encoder`|请求参数编码|将请求参数编码，便于通过http请求发送|
|`feign. Contract`|支持的注解格式|默认是SpringMVC的注解|
|`feign. Retryer`|失败重试机制|请求失败的重试机制，默认是没有，不过会使用Ribbon的重试|

一般情况下，默认值就能满足我们使用，如果要自定义时，只需要创建自定义的@Bean覆盖默认Bean即可。

下面以日志为例来演示如何自定义配置。

### 7.2.1 配置文件方式

基于配置文件修改feign的日志级别可以针对单个服务：

```yaml
feign:
  client:
    config:
      userservice: # 针对某个微服务的配置
        loggerLevel: FULL #  日志级别
```

也可以针对所有服务：

```yaml
feign:
  client:
    config:
      default: # 这里用default就是全局配置，如果是写服务名称，则是针对某个微服务的配置
        loggerLevel: FULL #  日志级别
```

而日志的级别分为四种：

- NONE：不记录任何日志信息，这是默认值。
- BASIC：仅记录请求的方法，URL以及响应状态码和执行时间
- HEADERS：在BASIC的基础上，额外记录了请求和响应的头信息
- FULL：记录所有请求和响应的明细，包括头信息、请求体、元数据。

### 7.2.2 Java代码方式

也可以基于Java代码来修改日志级别，先声明一个类，然后声明一个Logger.Level的对象：

```java
public class DefaultFeignConfiguration  {
    @Bean
    public Logger.Level feignLogLevel(){
        return Logger.Level.BASIC; // 日志级别为BASIC
    }
}
```

如果要**全局生效**，将其放到启动类的@EnableFeignClients这个注解中：

```java
@EnableFeignClients(defaultConfiguration = DefaultFeignConfiguration .class)
```

如果是**局部生效**，则把它放到对应的@FeignClient这个注解中：

```java
@FeignClient(value = "userservice", configuration = DefaultFeignConfiguration .class)
```

# 7.3 Feign配置连接池

Feign底层发起http请求，依赖于其它的框架。其底层客户端实现包括：

- `URLConnection`：默认实现，不支持连接池
- `Apache HttpClient` ：支持连接池
- `OKHttp`：支持连接池

因此提高Feign的性能主要手段就是使用**连接池**代替默认的 URLConnection。

这里我们用 Apache 的 HttpClient 来演示。

### 7.3.1 引入依赖

在 order-service 的 pom 文件中引入 Apache 的 HttpClient 依赖：

```xml
<!--httpClient的依赖 -->
<dependency>
    <groupId>io.github.openfeign</groupId>
    <artifactId>feign-httpclient</artifactId>
</dependency>
```

### 7.3.2 配置连接池

在 order-service 的 application.yml 中添加配置：

```yaml
feign:
  client:
    config:
      default: # default全局的配置
        loggerLevel: BASIC # 日志级别，BASIC就是基本的请求和响应信息
  httpclient:
    enabled: true # 开启feign对HttpClient的支持
    max-connections: 200 # 最大的连接数
    max-connections-per-route: 50 # 每个路径的最大连接数
```

接下来，在 FeignClientFactoryBean 中的 loadBalance 方法中打断点：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666347413674-271019a8-c75f-49eb-9c62-b23e2d906427.png)

Debug 方式启动 order-service 服务，可以看到这里的 client，底层就是 Apache HttpClient：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666347428107-c517f02b-924a-4bf5-aea2-e5dc163f7d70.png)

总结，Feign的优化：

1.日志级别尽量用 basic

2.使用 HttpClient 或 OKHttp 代替 URLConnection

① 引入 feign-httpClient 依赖

② 配置文件开启 httpClient 功能，设置连接池参数

# 7.4 最佳实践

所谓最近实践，就是使用过程中总结的经验，最好的一种使用方式。

自习观察可以发现，Feign的客户端与服务提供者的controller代码非常相似：

feign客户端：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666347464709-fe633f3c-e0ca-47b1-af46-120d294dd8f7.png)

UserController：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666347480381-71f2be6e-f966-458a-8331-38eeb66757d3.png)

有没有一种办法简化这种重复的代码编写呢？

### 7.4.1 继承方式

一样的代码可以通过继承来共享：

1）定义一个 API 接口，利用定义方法，并基于 SpringMVC 注解做声明。

2）Feign 客户端和 Controller 都继承该接口

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666347536891-4caa531a-9b9c-4e49-ac0d-5f0e9d261ed2.png)

优点：

- 简单
- 实现了代码共享

缺点：

- 服务提供方、服务消费方紧耦合
- 参数列表中的注解映射（@PathVariable）并不会继承，因此Controller中必须再次声明方法、参数列表、注解

### 7.4.2 抽取方式

将 Feign 的 Client 抽取为独立模块，并且把接口有关的 POJO、默认的 Feign 配置都放到这个模块中，提供给所有消费者使用。

例如，将 UserClient、User、Feign 的默认配置都抽取到一个 feign-api 包中，所有微服务引用该依赖包，即可直接使用。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666347575229-d8908dd4-ca63-4e31-991b-e064ded54cc2.png)

### 7.4.3 实现基于抽取的最佳实践

**1）抽取**

首先创建一个module，命名为feign-api：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666347625859-37a23f34-eafb-4a66-b287-3beab49dcc2f.png)

项目结构：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666347638281-eb642d17-8fe7-4679-94b5-d93fb6db61ca.png)

在feign-api中然后引入feign的starter依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

然后，order-service中编写的UserClient、User、DefaultFeignConfiguration都复制到feign-api项目中

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666347676740-28ed0457-80e3-4527-b207-394b987d3f0b.png)

**2）在order-service中使用feign-api**

首先，删除order-service中的UserClient、User、DefaultFeignConfiguration等类或接口。

在order-service的pom文件中中引入feign-api的依赖：

```xml
<dependency>
    <groupId>cn.itcast.demo</groupId>
    <artifactId>feign-api</artifactId>
    <version>1.0</version>
</dependency>
```

修改order-service中的所有与上述三个组件有关的导包部分，改成导入feign-api中的包

**3）重启测试**

重启后，发现服务报错了：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666347767938-f3187daa-6209-484f-b167-dc7a3a238ce6.png)

这是因为UserClient现在在cn.itcast.feign.clients包下，

而order-service的@EnableFeignClients注解是在cn.itcast.order包下，不在同一个包，无法扫描到UserClient**。**

**4）解决扫描包的问题**

方式一：

指定Feign应该扫描的包：

```java
@EnableFeignClients(basePackages = "cn.itcast.feign.clients")
```

方式二：

指定需要加载的Client接口：

```java
@EnableFeignClients(clients = {UserClient.class})
```

# 8.Gateway服务网关

Spring Cloud Gateway 是 Spring Cloud 的一个全新项目，该项目是基于 Spring 5.0，Spring Boot 2.0 和 Project Reactor 等响应式编程和事件流技术开发的网关，它旨在为微服务架构提供一种简单有效的统一的 API 路由管理方式。

# 8.1 为什么需要网关

Gateway网关是我们服务的守门神，所有微服务的统一入口。

网关的核心功能特性：

- 请求路由
- 权限控制
- 限流

架构图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666347907320-2c280792-5bb8-43ac-8c28-64f5f8a6fa3e.png)

**权限控制**：网关作为微服务入口，需要校验用户是否有请求资格，如果没有则进行拦截。

**路由和负载均衡**：一切请求都必须先经过gateway，但网关不处理业务，而是根据某种规则，把请求转发到某个微服务，这个过程叫做路由。当然路由的目标服务有多个时，还需要做负载均衡。

**限流**：当请求流量过高时，在网关中按照下流的微服务能够接受的速度来放行请求，避免服务压力过大。

在SpringCloud中网关的实现包括两种：

- gateway
- zuul

Zuul是基于Servlet的实现，属于阻塞式编程。而 SpringCloud Gateway 则是基于Spring5中提供的WebFlux，属于响应式编程的实现，具备更好的性能。

# 8.2 Gateway快速入门

下面，我们就演示下网关的基本路由功能。基本步骤如下：

- 1. 创建SpringBoot工程gateway，引入网关依赖
    
    创建服务：
    
    ![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666348022075-3ba9fcaf-6c5a-4981-a06f-f3d4bdbd9889.png)
    
    引入依赖：
    
    ```xml
    <!--网关-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-gateway</artifactId>
    </dependency>
    <!--nacos服务发现依赖-->
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
    </dependency>
    ```
    
- 2. 编写启动类
    
    ```java
    package cn.itcast.gateway;
    
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    
    @SpringBootApplication
        public class GatewayApplication {
    
            public static void main(String[] args) {
                SpringApplication.run(GatewayApplication.class, args);
            }
        }
    ```
    
- 3. 编写基础配置和路由规则
    
    创建application.yml文件，内容如下：
    
    ```yaml
    server:
      port: 10010 # 网关端口
    spring:
      application:
        name: gateway # 服务名称
      cloud:
        nacos:
          server-addr: localhost:8848 # nacos地址
        gateway:
          routes: # 网关路由配置
            - id: user-service # 路由id，自定义，只要唯一即可
              # uri: <http://127.0.0.1:8081> # 路由的目标地址 http就是固定地址
              uri: lb://userservice # 路由的目标地址 lb就是负载均衡，后面跟服务名称
              predicates: # 路由断言，也就是判断请求是否符合路由规则的条件
                - Path=/user/** # 这个是按照路径匹配，只要以/user/开头就符合要求
    ```
    
    我们将符合 **Path** 规则的一切请求，都代理到 **uri** 参数指定的地址。
    
    本例中，我们将 `/user/**`开头的请求，代理到 `lb://userservice`，lb 是负载均衡，根据服务名拉取服务列表，实现负载均衡。
    
    "P"一定要大写！！！
    
- 4. 启动网关服务进行测试
    
    重启网关，访问 `http://localhost:10010/user/1` 时，符合 `/user/**` ****规则，请求转发到`uri：<http://userservice/user/1`，得到了结果：>
    
    ![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666348212527-f014df84-cfb5-45bc-bf4e-1cb7e2028771.png)
    

整个访问的流程如下：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666348252353-c1c433fa-77f7-49b0-b6bc-18223780be08.png)

总结：

网关搭建步骤：

1. 创建项目，引入nacos服务发现和gateway依赖
2. 配置application.yml，包括服务基本信息、nacos地址、路由

路由配置包括：

1. 路由id：路由的唯一标示
2. 路由目标（uri）：路由的目标地址，http代表固定地址，lb代表根据服务名负载均衡
3. 路由断言（predicates）：判断路由的规则，
4. 路由过滤器（filters）：对请求或响应做处理

# 8.3 断言工厂

我们在配置文件中写的断言规则只是字符串，这些字符串会被 Predicate Factory 读取并处理，转变为路由判断的条件

例如 `Path=/user/**` 是按照路径匹配，这个规则是由`PathRoutePredicateFactory`类来处理的，像这样的断言工厂在 SpringCloudGateway 还有十几个:

|**名称**|**说明**|**示例**|
|---|---|---|
|After|是某个时间点后的请求|- After=2037-01-20T17:42:47.789-07:00[America/Denver]|
|Before|是某个时间点之前的请求|- Before=2031-04-13T15:14:47.433+08:00[Asia/Shanghai]|
|Between|是某两个时间点之前的请求|- Between=2037-01-20T17:42:47.789-07:00[America/Denver], 2037-01-21T17:42:47.789-07:00[America/Denver]|
|Cookie|请求必须包含某些cookie|- Cookie=chocolate, ch.p|
|Header|请求必须包含某些header|- Header=X-Request-Id, \d+|
|Host|请求必须是访问某个host（域名）|- Host=**.somehost.org,**.anotherhost.org|
|Method|请求方式必须是指定方式|- Method=GET,POST|
|Path|请求路径必须符合指定规则|- Path=/red/{segment},/blue/**|
|Query|请求参数必须包含指定参数|- Query=name, Jack或者- Query=name|
|RemoteAddr|请求者的ip必须是指定范围|- RemoteAddr=192.168.1.1/24|
|Weight|权重处理||

我们只需要掌握Path这种路由工程就可以了。

# 8.4 过滤器工厂

GatewayFilter 是网关中提供的一种过滤器，可以对进入网关的请求和微服务返回的响应做处理：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666348377105-c1e5fddf-7fc9-475c-b863-a96d9c58c219.png)

### 8.4.1 路由过滤器的种类

Spring 提供了**31**种不同的路由过滤器工厂。例如：

|**名称**|**说明**|
|---|---|
|`AddRequestHeader`|给当前请求添加一个请求头|
|`RemoveRequestHeader`|移除请求中的一个请求头|
|`AddResponseHeader`|给响应结果中添加一个响应头|
|`RemoveResponseHeader`|从响应结果中移除有一个响应头|
|`RequestRateLimiter`|限制请求的流量|

### 8.4.2 请求头过滤器

下面我们以 AddRequestHeader 为例来讲解。

**需求**：给所有进入userservice的请求添加一个请求头：Truth=itcast is freaking awesome!

只需要修改 gateway 服务的 application.yml 文件，添加路由过滤即可：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: user-service
        uri: lb://userservice
        predicates:
        - Path=/user/**
        filters: # 过滤器
        - AddRequestHeader=Truth, Itcast is freaking awesome! # 添加请求头
```

当前过滤器写在 userservice 路由下，因此仅仅对访问 userservice 的请求有效。

### 8.4.3 默认过滤器

如果要对所有的路由都生效，则可以将过滤器工厂写到default下。格式如下：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: user-service
        uri: lb://userservice
        predicates:
        - Path=/user/**
      default-filters: # 默认过滤项
      - AddRequestHeader=Truth, Itcast is freaking awesome!
```

### 8.4.4 总结

过滤器的作用是什么？

① 对路由的请求或响应做加工处理，比如添加请求头

② 配置在路由下的过滤器只对当前路由的请求生效

defaultFilters的作用是什么？

① 对所有路由都生效的过滤器

### 8.4.5 自定义过滤器

有两种方法：

- 实现 GatewayFilterFactory 接口
- 继承 AbstractGatewayFilterFactory 类

因为 `GatewayFilterFactory` 接口的默认方法非常多，所以 `AbstractGatewayFilterFactory` 抽象类对接口进行了适配，比较常用。

[Spring Cloud Gateway-自定义GatewayFilter - 腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/1650119)

自定义过滤器针对路由，需要在配置文件中定义，而全局过滤器是针对所有路由，不需要在配置文件中定义。

# 8.5 全局过滤器

上一节学习的过滤器，网关提供了31种，但每一种过滤器的作用都是固定的。如果我们希望拦截请求，做自己的业务逻辑则没办法实现。

### 8.5.1 全局过滤器作用

全局过滤器的作用也是处理一切进入网关的请求和微服务响应，与 GatewayFilter 的作用一样。区别在于GatewayFilter 通过配置定义，处理逻辑是固定的；而 GlobalFilter 的逻辑需要自己写代码实现。

定义方式是实现 GlobalFilter 接口。

```java
public interface GlobalFilter {
    /**
     *  处理当前请求，有必要的话通过{@link GatewayFilterChain}将请求交给下一个过滤器处理
     *
     * @param exchange 请求上下文，里面可以获取Request、Response等信息
     * @param chain 用来把请求委托给下一个过滤器
     * @return {@code Mono<Void>} 返回标示当前过滤器业务结束
     */
    Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain);
}
```

在 filter 中编写自定义逻辑，可以实现下列功能：

- 登录状态判断
- 权限校验
- 请求限流等

### 8.5.2 自定义全局过滤器

需求：定义全局过滤器，拦截请求，判断请求的参数是否满足下面条件：

- 参数中是否有 `authorization`
- authorization 参数值是否为 admin

如果同时满足则放行，否则拦截

实现：

在 gateway 中定义一个过滤器：

```java
package cn.itcast.gateway.filters;

import org.springframework.cloud.gateway.filter.GatewayFilterChain;
import org.springframework.cloud.gateway.filter.GlobalFilter;
import org.springframework.core.annotation.Order;
import org.springframework.http.HttpStatus;
import org.springframework.stereotype.Component;
import org.springframework.web.server.ServerWebExchange;
import reactor.core.publisher.Mono;

@Order(-1)
@Component
public class AuthorizeFilter implements GlobalFilter {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        // 1.获取请求参数
        MultiValueMap<String, String> params = exchange.getRequest().getQueryParams();
        // 2.获取authorization参数
        String auth = params.getFirst("authorization");
        // 3.校验
        if ("admin".equals(auth)) {
            // 放行
            return chain.filter(exchange);
        }
        // 4.拦截
        // 4.1.禁止访问，设置状态码
        exchange.getResponse().setStatusCode(HttpStatus.FORBIDDEN);
        // 4.2.结束处理
        return exchange.getResponse().setComplete();
    }
}
```

### 8.5.3 过滤器执行顺序

请求进入网关会碰到三类过滤器：当前路由的过滤器、DefaultFilter、GlobalFilter

请求路由后，会将当前路由过滤器和DefaultFilter、GlobalFilter，合并到一个过滤器链（集合）中，排序后依次执行每个过滤器：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666348829665-17264eae-8580-44e6-821e-516bc7aa6371.png)

排序的规则是什么呢？

- 每一个过滤器都必须指定一个int类型的order值，**order值越小，优先级越高，执行顺序越靠前**。
- GlobalFilter通过实现Ordered接口，或者添加@Order注解来指定order值，由我们自己指定
- 路由过滤器和defaultFilter的order由Spring指定，默认是按照声明顺序从1递增。
- 当过滤器的order值一样时，会按照 defaultFilter > 路由过滤器 > GlobalFilter的顺序执行。

详细内容，可以查看源码：

**org.springframework.cloud.gateway.route.RouteDefinitionRouteLocator#getFilters()**方法是先加载defaultFilters，然后再加载某个route的filters，然后合并。

**org.springframework.cloud.gateway.handler.FilteringWebHandler#handle()**方法会加载全局过滤器，与前面的过滤器合并后根据order排序，组织过滤器链

1. 全局过滤器与其他2类过滤器相比，永远是最后执行的；它的优先级只对其他全局过滤器起作用
2. 当默认过滤器与自定义过滤器的优先级一样时，优先出发默认过滤器，然后才是自定义过滤器；同类型的过滤器，出发顺序与他们在配置文件中声明的顺序一致
3. 默认过滤器与自定义过滤器使用同样的order顺序空间，即他们会按照各自的顺序来进行排序

# 8.6 跨域问题

### 8.6.1 什么是跨域问题

跨域：域名不一致就是跨域，主要包括：

- 域名不同： [www.taobao.com](https://www.taobao.com/) 和 [www.taobao.org](https://www.taobao.org/) 和 [www.jd.com](https://www.jd.com/) 和 [miaosha.jd.com](http://miaosha.jd.com)
- 域名相同，端口不同：localhost:8080和localhost8081

跨域问题：浏览器禁止请求的发起者与服务端发生跨域ajax请求，请求被浏览器拦截的问题

解决方案：CORS，这个以前应该学习过，这里不再赘述了。不知道的小伙伴可以查看https://www.ruanyifeng.com/blog/2016/04/cors.html

### 8.6.2 模拟跨域问题

找到课前资料的页面文件：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666348978403-a3b40f8d-f50f-4051-b9b0-6ab9a7644474.png)

放入tomcat或者nginx这样的web服务器中，启动并访问。

可以在浏览器控制台看到下面的错误：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1666348992582-bddbaaaf-5077-4372-accf-05e502e4bc0f.png)

从localhost:8090访问localhost:10010，端口不同，显然是跨域的请求。

### 8.6.3 解决跨域问题

在gateway服务的application.yml文件中，添加下面的配置：

```yaml
spring:
  cloud:
    gateway:
      # 。。。
      globalcors: # 全局的跨域处理
        add-to-simple-url-handler-mapping: true # 解决options请求被拦截问题
        corsConfigurations:
          '[/**]':
            allowedOrigins: # 允许哪些网站的跨域请求
              - "<http://localhost:8090>"
            allowedMethods: # 允许的跨域ajax的请求方式
              - "GET"
              - "POST"
              - "DELETE"
              - "PUT"
              - "OPTIONS"
            allowedHeaders: "*" # 允许在请求中携带的头信息
            allowCredentials: true # 是否允许携带cookie
            maxAge: 360000 # 这次跨域检测的有效期
```

# 9.Dubbo

# 10.Zookeeper



微服务中的熔断是指在系统遇到错误或延迟时，通过监测服务的健康状况，及时阻止对不健康服务的请求，以防止问题的蔓延，从而保护系统的整体稳定性。熔断器的概念最初源自电气工程，指的是一种防止电路过载的装置。在微服务架构中，熔断器通过拦截请求并快速响应来减少对失败服务的依赖。

### 熔断的基本概念

1. **状态**：
    - **闭合状态**（Closed）：正常工作，所有请求都被允许通过。
    - **打开状态**（Open）：服务不可用，所有请求都会被拒绝。
    - **半开状态**（Half-Open）：允许一部分请求通过，以监测服务是否恢复。
2. **工作原理**：
    - 当请求失败次数超过设定阈值时，熔断器会切换到打开状态，停止对该服务的请求。
    - 一段时间后，熔断器会切换到半开状态，尝试发送少量请求以判断服务是否恢复。
    - 如果请求成功，则切换回闭合状态；如果仍然失败，则继续保持打开状态。

### 降级的概念

降级是指当某个服务无法正常工作时，系统选择以较低的性能或功能响应请求。降级可以是为了提高系统的可用性，避免服务完全不可用的情况。

- **举例**：
    - 在一个电商网站中，如果订单服务不可用，可以返回一个静态的“服务繁忙，请稍后再试”的页面，而不是让用户看到错误信息。
    - 或者可以返回缓存的数据，而不是实时数据，以保证系统的基本功能。

### 熔断与降级的关系

1. **相辅相成**：
    - **熔断**用于监测和控制对外部服务的请求流，保护系统不被瞬时的服务故障拖垮。
    - **降级**是在服务故障时，通过提供简化或替代的响应，以维持系统的可用性。
2. **使用场景**：
    - 当一个服务的调用失败率达到设定阈值时，熔断器触发，将请求拦截并切换到打开状态。在这种情况下，可以启动降级逻辑，返回一个备选响应，避免用户体验不佳。

### 总结

熔断和降级是微服务架构中处理服务不稳定和故障的重要机制。熔断保护系统不受到错误服务的影响，而降级则保证在服务不可用的情况下，仍能提供基本的用户体验。通过这两者的结合，可以提高系统的健壮性和可用性。希望这些信息能帮助你更好地理解熔断和降级的概念！如果有其他问题，欢迎随时询问。

# 降级

在微服务架构中，降级（Fallback）是指在服务调用失败或响应时间过长时，采取的应急措施，以保证系统的可用性和用户体验。具体来说，当某个服务不可用或超时，系统不会直接报错，而是会调用预先定义的降级逻辑，以提供一个替代的响应或处理方式。这样可以有效防止故障蔓延，确保系统整体的稳定性。

### 降级的场景

1. **服务不可用**：当一个微服务因为网络故障、服务器宕机等原因无法访问时。
2. **超时**：当调用外部服务的响应时间超过预设的时间限制时。
3. **流量限制**：在高负载情况下，为了保护后端服务的稳定性，可以选择临时关闭某些功能或返回默认数据。

### 降级的实现方式

1. **返回默认值**：在服务不可用时，返回一个默认的响应。例如，查询用户信息时，如果用户服务不可用，可以返回一个空的用户对象。
2. **返回静态内容**：使用静态数据代替动态数据。例如，当商品详情服务不可用时，可以返回一些静态的产品信息。
3. **日志记录**：记录错误信息或异常，以便后续分析和监控。
4. **用户提示**：在前端提示用户当前功能暂时不可用，提升用户体验。