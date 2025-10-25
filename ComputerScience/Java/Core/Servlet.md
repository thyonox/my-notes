## 一、Servlet简介（）

### 1.Servlet架构图

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668695972840-a40b6114-7a87-4af0-9dcb-e06a6f8d668c.png#averageHue=%2334be97&clientId=uac1cf05b-78de-4&from=paste&height=707&id=u53a97b42&originHeight=707&originWidth=1223&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1193579&status=done&style=none&taskId=u29abac9d-b949-494f-bcf2-d0af5b6dfe7&title=&width=1223)

image.png

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668695982294-a413ede7-ab92-472c-852c-96bc8a38bc3b.png#averageHue=%23e8a391&clientId=uac1cf05b-78de-4&from=paste&height=523&id=u9a07ed0d&originHeight=523&originWidth=1153&originalType=binary&ratio=1&rotation=0&showTitle=false&size=275273&status=done&style=none&taskId=u2557ca60-264b-4c10-875a-ef439bc852d&title=&width=1153)

image.png

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668695991000-9f8ea517-5339-4de4-a050-2e70b551a0e5.png#averageHue=%23fcfbf9&clientId=uac1cf05b-78de-4&from=paste&height=748&id=u9b5ce1a8&originHeight=748&originWidth=908&originalType=binary&ratio=1&rotation=0&showTitle=false&size=110499&status=done&style=none&taskId=ud8d8d5ab-4919-41be-ae9e-56de9683753&title=&width=908)

image.png

### 2.什么是Servlet

Servlet 是 JavaEE 的规范接口之一（Servlet、Filter、Listener…），**用于接受客户端发送的请求，并且响应数据给客户端**。 Servlet 没有主方法，所以 Servlet 要放在 Servlet 容器（Servlet引擎）中。

### 3.Servlet容器

# 2. Servlet使用

## 2.1 概述

Servlet 的创建方式有三种：

1. 实现Servlet接口
2. 继承GenericServlet类
3. 继承HttpServlet类

第三种方式最为常用。接下来以第一种和第三种方式为例演示Servlet的使用。

## 2.2 实现Servlet

**环境搭建**

```xml
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>4.0.1</version>
  <scope>provided</scope><!--servlet不参与运行，如果集成本地Tomcat则可以不用写--></dependency>
```

**步骤**

1. 自定义类实现 Servlet。
2. 实现 service 方法，在service方法中，处理请求，响应数据。
3. 在 web.xml 中配置 Servlet 的访问地址（`Servlet3.0`之后，可以使用注解 `@WebServlet` 进行配置）

**编码**

```java
import javax.servlet.*;import javax.servlet.annotation.WebServlet;import java.io.IOException;
@WebServlet("/myServlet")
public class MyServlet implements Servlet {  
@Override  
public void init(ServletConfig servletConfig){      
System.out.println("执行初始化...");  
}  
@Override  
public ServletConfig getServletConfig() {      
return null;  
}  
@Override  
public void service(ServletRequest servletRequest, ServletResponse servletResponse){      
System.out.println("执行service方法...");  }  
@Override  public String getServletInfo() {      
return null;  }  
@Override  public void destroy() {      
System.out.println("执行销毁方法...");  }}
```

> 注意事项： 如果重写父类或实现类的方法后，没有自动添加 @Override 检查编译器版本： [https://blog.csdn.net/qq_34246778/article/details/103442654](https://blog.csdn.net/qq_34246778/article/details/103442654)

**路由配置** 上面使用了注解 @WebServlet 进行路径配置（**还可以在一个Servlet中使用urlPattern配置多个访问路径**），这种方式也是最简单、最为推荐的，当然我们也需要了解了使用 `web.xml` 进行配置，掌握原理。

```xml
<servlet>
  <!--Servlet程序的别名-->  <servlet-name>MyServlet</servlet-name>
  <!--Servlet程序的全类名-->  <servlet-class>com.dong.MyServlet</servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>MyServlet</servlet-name>
  <!--url访问路劲-->  <url-pattern>/myServlet</url-pattern>
</servlet-mapping>
```

**测试** 现在一个 Servlet 就已经创建好了，可以和浏览器进行请求和响应的交互，我们来测试一下。 后台启动 Tomcat，在浏览器输入地址，然后进入控制台查看打印结果。

```markdown
localhost:8080/项目名/myServlet
```

当然，我们也可以获取浏览器发送的数据。 在 service 方法中，添加以下代码，获取浏览器的传来的参数：

```java
@Overridepublic void service(ServletRequest servletRequest, ServletResponse servletResponse) {    System.out.println("进入service方法...");    String name = servletRequest.getParameter("name");    String password = servletRequest.getParameter("password");    System.out.println(name);    System.out.println(password);}
```

然后直接在浏览器地址栏中发送 GET 请求，将参数写在请求行中。在控制台查看输出结果。

```markdown
<http://localhost:8080/项目名/myServlet?name=IU&password=123123>
```

路径与参数用 `?` 分割，多个参数之间使用 `&` 分割。

# 注意Tomcat和Servlet的兼容性问题

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/6d055ff5-7126-4d31-91c0-66bc04da41a6/image.png)

## 2.3 继承HttpServlet

是一种**接口适配器**。 我们很明显感觉第一种创建 Servlet 的方式很繁琐，好多方法都用不上，但因为是抽象方法，却又不得不重写，容易造成代码冗余，也不利于代码的维护。 所以经过层层简化，在 `GenericServlet` 中将除 service 方法之外的方法全部**重写**，在 `HttpServlet` 中则是对 service 方法进行细分，针对不同的请求转发到不同方法中。 **步骤**

1. 自定义类**继承** `HttpServlet`。
2. 根据业务需要重写 `doGet`和 `doPost`方法（这两种方法最为常用）。
3. 配置路径（和实现 Servlet接口创建 Servlet 一样）。

**编码**

```java
@WebServlet("/myHttpServlet")public class MyHttpServlet extends HttpServlet {  @Override  protected void doGet(HttpServletRequest req, HttpServletResponse resp){      System.out.println("执行doGet方法！");  }  @Override  protected void doPost(HttpServletRequest req, HttpServletResponse resp){      System.out.println("执行doPOST方法！");  }}
```

**测试** 分别使用浏览器以及接口测试工具分别发送 Get 请求和 Post 请求，查看控制台打印信息。

# 3. Servlet类的继承体系

## 3.1 继承体系图

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668696076908-6addf06e-75dd-455f-865f-e5590d2b7f37.png#averageHue=%2360836c&clientId=uac1cf05b-78de-4&from=drop&id=u2d3eab46&originHeight=2062&originWidth=1888&originalType=binary&ratio=1&rotation=0&showTitle=false&size=309615&status=done&style=none&taskId=ua72fa392-80a4-4b6f-a406-747c5628b2f&title=)

MyServlet.png

## 3.2 层次解释

- `Servlet`：狭义的 Servlet 是实现 Servlet 的类，广义的 Servlet 是符合 Servlet 规约的类。
- `GenericServlet`：实现了 Servlet 接口中除了 service 方法外的其他四个方法，这几个方法我们就可以不用去重写了。
- `HttpServlet`：实现了 Service 方法，通过 service 方法将处理不同的 HTTP 请求方法，并交由相应的处理方法去处理，如doGET、doPOST等。
- 自定义 Servlet：重写处理方法，完成业务。

## 3.3 GP分发处理

发生在 HttpServlet 中，下面是请求方法的分发处理的简易实现。

```java
@Overridepublic void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {    
System.out.println("Hello Servlet！");    //将ServletRequest转换为HttpServletRequest    
HttpServletRequest request = (HttpServletRequest) servletRequest;    
HttpServletResponse Response = (HttpServletResponse) ServletResponse；
    //获取请求方法    
    String method = request.getMethod();   
     if ("GET".equals(method)){        
     doGET(request,response);    }
     else if("POST".equals(method)){        
     doPOST(requestm,response);    }}
```

# 4. Servlet的生命周期

## 4.1 Servlet 源码

```java
package javax.servlet;import java.io.IOException;
public interface Servlet {    // 初始化方法 加载资源    
void init(ServletConfig var1) throws ServletException;    // Servlet的配置类    
ServletConfig getServletConfig();    // 执行业务核心方法    
void service(ServletRequest var1, ServletResponse var2) throws ServletException, IOException;    // 覆写该方法以产生有意义的信息。（如：版本号、版权、作者等）    
String getServletInfo();    // 销毁方法 释放资源    void destroy();}
```

## 4.2 生命周期

Servlet 中的三个方法对应着 Servlet 的生命周期。 **初始化** 默认情况下，当 Web 客户**第一次**请求访问某个 Servlet 时，Web 容器会创建这个 Servlet 的实例（查找 web.xml 的配置找到对应的 Servlet，或者通过扫描注解，创建对象，执行`init`方法）。 **每一个用户请求都会产生一个新的线程**，`init()`方法简单的创建或加载一些资源，这些资源将会被用在 Servlet 的整个生命周期。 也可以通过注解，或者在 web.xml 配置文件中手动设置 Servlet 的加载优先级： 注解：

```java
@WebServlet(value = "/myHttpServlet",loadOnStartup = 2)
```

web.xml

```xml
<servlet>
  <!--Servlet程序的别名-->  <servlet-name>MyServlet</servlet-name>
  <!--Servlet程序的全类名-->  <servlet-class>com.dong.MyServlet</servlet-class>
  <load-on-startup>2</load-on-startup>
</servlet>
```

**负整数**：第一次访问 Servlet 时创建 Servlet **零和正整数**：Web 工程创建的时候创建，数字越小，优先级越高。 **运行** 执行接受请求，处理响应的业务。每次请求都会执行。 **销毁** 在关闭 Web 容器时调用 `destroy` 方法，之后 Servlet 被标记为垃圾回收。

# 5. Servlet API

## 5.1 ServletConfig

**简介**

1. `ServletConfig` 是一个接口，代表 Servlet 程序的配置信息。
2. Servlet 程序是第一次访问的时候创建的，ServletConfig 也**随之创建**，并将 ServletConfig 对象作用参数传入 Servlet的`init`方法中。
3. Servlet 和 ServletConfig 对象都是由 Tomcat 负责创建。

**作用**

1. 可以获得 Servlet 程序的别名（web.xml中 servlet-name 的值）。
2. 获取初始化参数 `init-param` 。
3. 获取ServletContext对象。

**示例** web.xml

```xml
<servlet>
  <servlet-name>MyServlet_03</servlet-name>
  <servlet-class>com.dong.servlet.MyServlet_03</servlet-class>
  <!--初始化参数-->  <init-param>
    <param-name>name</param-name>
    <param-value>张三</param-value>
  </init-param>
</servlet>
<servlet-mapping>
  <servlet-name>MyServlet_03</servlet-name>
  <url-pattern>/test3</url-pattern>
</servlet-mapping>
```

Servlet

```java
@Overrideprotected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {    
System.out.println("进入方法！");    //获取Servlet的别名    
System.out.println(getServletConfig().getServletName());    //获取初始化参数的值    
System.out.println(getServletConfig().getInitParameter("name"));    //获取ServletContext对象    
System.out.println(getServletConfig().getServletContext());}
```

**细节** 如果使用实现 Servlet 接口的方式来创建 Servlet，需要通过 ServletConfig（`getServletConfig()`） 来获取 ServletContext 对象。但是如果使用继承 HttpServlet 的方式来创建 Servlet，那么获取 ServletContext 时，可以直接调用`getServletContext()`方法，因为此方法由 GenericServlet 类提供。

```java
public ServletContext getServletContext() {    
ServletConfig sc = this.getServletConfig();    
if (sc == null) {        
throw new IllegalStateException(lStrings.getString("err.servlet_config_not_initialized"));    
} else {        
return sc.getServletContext();    }}
```

**相关方法**

```java
// 获取 Servlet 别名String getServletName();// 获取ServletContext 对象ServletContext getServletContext();// 获取初始化参数String getInitParameter(String var1);// 获取初始化参数数组Enumeration<String> getInitParameterNames();
```

## 5.2 ServletContext

**简介**

- ServletContext 是一个接口，表示 **Servlet 上下文对象**。
- 一个 web 工程，**只有一个 **ServletContext 对象实例。
- ServletContext 对象是一个域对象。
- ServletContext 是在 web 工程部署启动的时候创建，在 web 工程停止的时候销毁。

**域对象** 域对象有两重含义：一是可以像 Map 集合一样存取数据的对象；二是代表作用域，不同的域对象有着不同的作用域。ServletContext 的作用域是整个web 工程。

|操作|存数据|取数据|删除|
|---|---|---|---|
|Map|put()|get()|remove()|
|域对象|setAttribute()|getAttribute()|removeAttribute()|

**作用**

1. 获取 web.xml 中配置的上下文参数 context-param。
2. 获取当前的工程路径，格式：/工程路径。
3. 获取工程部署后在服务器硬盘上的绝对路径。
4. 像 Map 一样存取数据，在 Web 工程中传递数据

**示例** web.xml

```xml
<context-param>
  <!--上下文参数，属于整个web工程-->  <param-name>username</param-name>
  <!--参数值-->  <param-value>context</param-value>
</context-param>
<context-param>
  <param-name>password</param-name>
  <param-value>1111</param-value>
</context-param>
```

Servlet

```java
@Overrideprotected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {    //1、获取上下文参数    
System.out.println(getServletContext().getInitParameter("username"));    
System.out.println(getServletContext().getInitParameter("password"));    //2、获取当前的工程路径    
System.out.println(getServletContext().getContextPath());    //3、获取工程部署后在服务器硬盘上的绝对路径    
System.out.println("工程部署路径："+getServletContext().getRealPath("/"));    //工程下servlet目录下的MyServlet_04文件路径    
System.out.println(getServletContext().getRealPath("/servlet/MyServlet_04"));    //4、像Map一样存取数据    
getServletContext().setAttribute("ServletContext","你好ServletContext！");    
System.out.println(getServletContext().getAttribute("ServletContext"));}
```

**相关方法**

```java
Object getAttribute(String name);// 返回给定名的属性值
Enumeration getAttributeNames();// 返回所有可用属性名的枚举
void setAttribute(String name,Object obj);// 设定属性的属性值
void removeAttribute(String name);// 删除一属性及其属性值
String getServerInfo();// 返回JSP(SERVLET)引擎名及版本号
String getRealPath(String path);// 返回一虚拟路径的真实路径
ServletContext getContext(String uripath);// 返回指定WebApplication的application对象
int getMajorVersion();// 返回服务器支持的Servlet API的最大版本号
int getMinorVersion();// 返回服务器支持的Servlet API的最大版本号
String getMimeType(String file);// 返回指定文件的MIME类型URL
 getResource(String path);// 返回指定资源(文件及目录)的URL路径
 InputStream getResourceAsStream(String path);// 返回指定资源的输入流
 RequestDispatcher getRequestDispatcher(String uripath);// 返回指定资源的RequestDispatcher对象
 Servlet getServlet(String name);// 返回指定名的
 ServletEnumeration getServlets();// 返回所有Servlet的枚举
 Enumeration getServletNames();// 返回所有Servlet名的枚举
 void log(String msg);// 把指定消息写入Servlet的日志文件
 void log(Exception exception,String msg);// 把指定异常的栈轨迹及错误消息写入Servlet的日志文件
 void log(String msg,Throwable throwable);// 把栈轨迹及给出的Throwable异常的说明信息 写入Servlet的日志文件
```

## 5.3 HttpServletRequest

**作用** 每次只要有请求进入Tomcat服务器，Tomcat 服务器就会把请求过来的 HTTP 协议信息解析好封装到 Request 对象中。然后传递到 service 方法（doGET和doPOST方法）中给我们使用。我们可以通过 HttpServletRequest 对象，获取到所有请求的信息。 **相关方法**

```java
Object getAttribute(String name);// 返回指定属性的属性值
void setAttribute(String key, Object value);// 设置属性的属性值
Enumeration getAttributeNames();// 返回所有可以用属性名的枚举
String getParameter(String name);// 返回指定name的参数值
Enumeration getParameterNames();// 返回可用参数名的枚举
String[] getParameterValues(String name);// 返回包含参数name的所有制的数组
ServletInputStream geetInputStream();// 得到请求体中一行的二进制流
BufferedReader getReader();// 返回解码过了的请求体
String getServerName();// 返回接收请求的服务器主机名
int getServerPort();// 返回服务器接收此请求所用的端口号
String getRemoteAddr();// 返回发送请求的客户端的IP地址
String getRemoteHost();// 返回发送请求的客户端主机名
String getRealPath();// 返回一个虚拟路径的真实路径
String getCharacterEncoding();// 返回字符编码方式
int geContentLength();// 返回请求体的长度 ( 字节数 )
String getContentType();// 返回请求体的MIME类型
String getProtocol();// 返回请求用的协议类型以及版本号
String getScheme();// 返回请求用的协议名称( 例如 : http  https  ftp )
```

**请求的中文乱码问题** 如果不对请求进行处理，获取的表单数据中的中文在控制台上输出，就会乱码。 有两种解决方法：

1. 方法一（不推荐）

```java
//先获取请求参数，将参数以iso8859-1进行编码，再以utf-8进行解码
@Overrideprotected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {    //获取请求参数    
String username = req.getParameter("username");    //1、先用iso8859-1进行编码    //2、再用utf-8进行解码    
String name = new String(username.getBytes(StandardCharsets.ISO_8859_1), StandardCharsets.UTF_8);}
```

1. 方法二（推荐）

```java
//直接设置请求体的字符集为utf-8
@Overrideprotected 
void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {    
req.setCharacterEncoding("UTF-8");}
```

**请求转发** 请求转发表示服务器接受到请求之后，从一个资源（Servlet）跳到另一个资源（Servlet）的操作。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668696106552-f22d05fc-3612-4253-913b-530e95ed13b9.png#averageHue=%23a9c48a&clientId=uac1cf05b-78de-4&from=paste&height=578&id=u4a912b0d&originHeight=578&originWidth=1050&originalType=binary&ratio=1&rotation=0&showTitle=false&size=271797&status=done&style=none&taskId=u40d92155-1bbf-4f89-8940-ed5bd892598&title=&width=1050)

image.png

请求转发的几个特点：

1. 浏览器地址栏没有变化
2. 它们属于一次请求
3. 共享 Request 域中的数据
4. 可以转发到 WEB-INF 目录下
5. 不可以访问工程以外的资源

编码实现： 在当前 Servlet 中转发到 url 为`"/myServlet-2"`的资源上，一般是转发到另一个 Servlet 中。

```java
req.getRequestDispatcher("/myServlet-2").forward(req,resp);
```

**请求重定向** 请求重定向表示服务器发送请求后，服务器给了客户端一个新地址，然后客户端去访问新地址。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668696116136-95abf7ce-44a0-473d-8afb-be07eca87387.png#averageHue=%23aac58c&clientId=uac1cf05b-78de-4&from=paste&height=569&id=uf3bccead&originHeight=569&originWidth=1018&originalType=binary&ratio=1&rotation=0&showTitle=false&size=271371&status=done&style=none&taskId=u1093d90c-f8a7-4abf-ad6d-f3553fdae55&title=&width=1018)

image.png

请求重定向有一下几个特点：

1. 浏览器地址栏变化。
2. 是两次请求。
3. 不能共享 Request 域中数据。
4. 不能访问 WEB-INF 下的资源。
5. 可以访问工程外的资源。

对于重定向的编码实现有两种方式：

1. 方法一 （浏览器处理）

```java
//设置响应状态码，表示重定向
resp.setStatus(302);//设置响应头，说明新的地址在哪里
resp.setHeader("Location","<http://localhost:8080>");
```

1. 方法二 （后端处理）

```java
resp.sendRedirect("<http://localhost:8080>");
```

## 5.4 HttpServletResponse

**作用** 每次请求进入，Tomcat 服务器都会创建一个 Response 对象传递给 Servlet 程序去使用，表示所有响应的信息，我们需要设置返回客户端的信息，可以通过这个类的对象来设置。 **相关方法**

```java
String getCharacterEncoding();// 返回响应用的是哪种字符编码ServletOutputStream getOutputStream();// 返回响应的一个二进制输出流PrintWriter getWriter();// 返回可以向客户端输出字符的一个对象void setContentLength(int len);// 设置响应头长度void setContentType(String type);// 设置响应的MIME类型void sendRedirect(String location);// 重新定向客户端的请求
```

**往客户端回传数据**

- 显式的往客户端回传数据

```java
//先获取输出流对象，利用输出流对象将数据传回客户端resp.getWriter().println(req.getAttribute("key1"));
```

- out.writer和out.print的区别

```markdown
1. out.writer
   只能输出字符串
2. out.print
   可以输出对象
```

**响应的中文乱码问题** 如果不进行设置，在浏览器上显示的中文将会乱码。

- 方法1

```java
//设置响应体字符集为UTF-8，通过响应头，设置浏览器也使用UTF-8字符集resp.setCharacterEncoding("UTF-8");
resp.setHeader("Content-Type", "text/html; charset=UTF-8");
```

- 方法2

```java
//同时设置服务器和客户端都使用UTF-8字符集，还设置了响应头.此方法在获取流对象之前调用才有效resp.setContentType("text/html; charset=UTF-8");
```



