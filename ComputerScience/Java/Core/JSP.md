
# 1. JSP简介

## 1.1 什么是JSP

- jsp 的全称是 Java 的服务器页面（`java server pages`）。
- jsp 的主要作用是代替 Servlet 程序回传 html 页面的数据，作为视图层。
- Servlet 程序回传 html 页面数据是一件非常繁锁的事情，开发成本和维护成本都极高，所以**使用 JSP 进行简化**，使程序员**专注于业务逻辑**。
- 相当于将 Servlet 封装了一层，客户端第一次访问 JSP 时，**JSP 会解析成 Servlet**，编译为 .class文件运行。在这过程中，JSP 中的 HTML 代码会被存入 `out`缓冲区中，等 JSP 代码执行完毕，会将 out 缓区中的数据追加写入 Response 缓冲区中，由 response 进行响应。

下面演示使用 Servlet 和 JSP 回传 HTML 的区别： **Servlet回传html页面数据 **

```java
@Overrideprotected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {    resp.setContentType("text/html;charset=UTF-8");    PrintWriter out = resp.getWriter();    out.write("<!DOCTYPE html>\\r\\n");    out.write(" <html lang=\\"en\\">\\r\\n");    out.write(" <head>\\r\\n");    out.write(" <meta charset=\\"UTF-8\\">\\r\\n");    out.write(" <title>Title</title>\\r\\n");    out.write(" </head>\\r\\n");    out.write(" <body>\\r\\n");    out.write(" 这是 html 页面数据 \\r\\n");    out.write(" </body>\\r\\n");    out.write("</html>\\r\\n");    out.write("\\r\\n");}
```

> 注意：这个 out 不是 JSP 中的 out，是 Servlet 中的 response 缓冲区。

- *JSP回传html页面 **

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
    <head>
        <title>Title</title>    </head>
    <body>
        这是 html 页面数据
    </body></html>
```

## 1.2 JSP的本质

- JSP 就是 Servlet 程序
- 当我们第一次访问 jsp 页面的时候。Tomcat 服务器会帮我们把 jsp 页面翻译成为一个 java 源文件。并且对它进行编译成 为 `.class` 字节码程序，jsp 类继承于 HttpJspBase，而 HttpJspBase 直接继承于 HttpServlet，所以 JSP 间接继承了 HttpServlet
- 相当于是将回传的 html 数据那部分抽离出来，使我们专注于业务，减少繁琐、重复的工作
- JSP 是从 Servlet 中剥离出来的用于处理 HTML 的特殊 Servlet

# 2. JSP基础语法

## 2.1 JSP模板元素

JSP 页面中的 HTML 内容称之为 JSP 模版元素。JSP 模版元素定义了网页的基本骨架，即定义了页面的结构和外观。

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %><html><head>    <title>Title</title></head><body>
```

### 2.JSP声明

JSP 页面中编写的所有代码，默认会被编译到 Servlet 的`_jspService`方法中，而 Jsp 声明中的 java 代码会被翻译到 `_jspService`方法的外面。用于定义静态代码块、成员变量和方法。

```java
<%!static {
    System.out.println("静态代码块");
}private String name = "张三";public void TestFun(){    System.out.println("成员方法！");}%><%    TestFun();    out.println("name:" + name);%>
```

### 3.JSP表达式

用于将程序数据输出到客户端。JSP表达式会被翻译成为 `out.print()`输出到页面上，不能以分号结尾。

```java
<%=2022%><%=22.22%><%="这是个字符串"%><%=map%><%=request.getParameter("username")%>
```

### 4.JSP脚本片段

只能书写Java代码，不能定义方法。会将代码脚本中的Java代码原封不动放入`_jspService()`方法中。

```java
<%    for (int i=0;i<3;i++){%>        <h1>第<%=i+1%>行</h1><%    }%><%--jspService方法中内容都可以写--%><%    System.out.println(request.getParameter("username"));%>
```

### 5.JSP注释

JSP有三种注释，其中html注释可以在浏览器查看源代码时可见，而JSP注释可以注释掉JSP页面中的所有代码。

- html注释

```java
<!-- 这是html注释 -->
```

- java注释

```java
// 单行java注释/* 多行java注释 */
```

- jsp注释

```java
<%-- 这是jsp注释 --%>
```

## 三、JSP指令

### 1.指令语法格式

```
<%@ 指令名  属性1 = "属性1的值" 属性2 = "属性2的值" ....%>
```

指令名：指定指令类型，在 JSP 中包含 `page`、`include`、`taglib` 这3种指令。

属性：指定指令属性，不同的指令包含不同的属性，在同一个指令中可以设置多个属性，各个属性之间用逗号或者空格隔开。

### 2.Page指令

jsp 的 page 指令可以修改 jsp 页面中一些重要的属性，或者行为。

|属性|说明|
|---|---|
|language|表示 jsp 翻译后是什么语言文件。暂时只支持 java|
|contentType|表示 jsp 返回的数据类型是什么|
|pageEncoding|表示当前 jsp 页面文件本身的字符集|
|import|跟 java 源代码中一样。用于导包，导类|
|autoFlush|设置当 out 输出流缓冲区满了之后，是否自动刷新缓冲级区。默认值是 true|
|buffer|设置 out 缓冲区的大小。默认是 8kb|
|errorPage|设置当 jsp 页面运行时出错，自动跳转去的错误页面路径|
|isErrorPage|设置当前 jsp 页面是否是错误信息页面。默认是 false。如果是 true 可以|
|isELIgnored|忽略（true）JSP 2.0 表达式语言（EL），还是进行正常的求值（false）|
|session|设置访问当前 jsp 页面，是否会创建 HttpSession 对象。默认是 true|
|extends|设置 jsp 翻译出来的 java 类默认继承谁|

### 3.include指令

include 指令用于引入其它 JSP 页面。如果使用 include 指令引入了其它 JSP 页面，那么 JSP 引擎将把这两个 JSP 翻译成一个 servlet。所以 include 指令引入通常也称之为**静态引入**。

语法格式：

```
<%@ include file="relativeURL" %>
```

file属性用于指定被引入文件的路径。路径以”/“开头，表示代表当前web应用。

细节：

1. 被引入的文件必须遵循JSP语法。
2. 被引入的文件可以使用任意的扩展名，即使其扩展名是html，JSP引擎也会按照处理jsp页面的方式处理它里面的内容，为了见明知意，JSP规范建议使用.jspf（JSP fragments(片段)）作为静态引入文件的扩展名。
3. 由于使用include指令将会涉及到2个JSP页面，并会把2个JSP翻译成一个servlet，所以这2个JSP页面的指令不能冲突（除了pageEncoding和导包除外）。
4. 静态引入不会翻译被包含的jsp页面
5. 静态引入其实是把被包含的jsp页面的代码拷贝到包含的位置执行输出

## 四、JSP标签

### 1.标签分类

JSP标签分为三种：

1. 内置标签（动作标签）：不需要在JSP页面导入标签
2. jstl标签：需要引入依赖，需要在JSP页面导入标签
3. 自定义标签：自定义标签，需要在JSP页面导入标签

### 2.内置标签

### `<jsp:include>`

`<jsp:include>`标签表示**动态引入**。`<jsp:include>`标签涉及到的2个JSP页面会被翻译成2个servlet，这2个servlet的内容在执行时进行合并。

语法格式：

```
<jsp:include page=""></jsp:include>
```

细节：

1. 动态包含会把包含的jsp页面也翻译成为java代码。
2. 动态包含底层代码使用如下代码去调用被包含的jsp页面执行输出。

```java
JspRuntimeLibrary.include(request, response, "/include/footer.jsp", out, false);
```

1. 原理图：

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668696357825-48c04219-ceee-42a7-b27a-0a16b7b42084.png#averageHue=%23fcfbfb&clientId=u946b39c0-4311-4&from=paste&height=692&id=ua191badb&originHeight=692&originWidth=1375&originalType=binary&ratio=1&rotation=0&showTitle=false&size=417933&status=done&style=none&taskId=ua2a0ecf5-36f1-4b4a-966a-fd6b9616434&title=&width=1375)

image.png

### `<jsp:forward>`

`<jsp:forward>`标签用于把请求转发给另外一个资源（服务器跳转，地址不变）。

语法格式：

```
<jsp:forward page=""></jsp:forward>
```

### `<jsp:param>`

可以设置键值对数据，作为`<jsp:include>`或者`<jsp:forward>`标签的参数进行传递。

语法格式：

```
<jsp:forward page="/index2.jsp" >
    <jsp:param value="10086" name="num"/>
    <jsp:param value="10010" name="num2"/>
</jsp:forward>
```

### 3.自定义标签

// TODO

## 五、JSP九大内置对象

> jsp 中的内置对象，是指Tomcat 在翻译 jsp 页面成为 Servlet 源代码时，内部已经为我们提供好了九个对象，叫内置对象，所以我们可以直接使用。

### 1.request

**作用：** 表示客户端的请求，HttpServletRequest的实例，作用域为响应生成之前。

**方法：**

```java
Object getAttribute(String name);// 返回指定属性的属性值void setAttribute(String key, Object value);// 设置属性的属性值Enumeration getAttributeNames();// 返回所有可以用属性名的枚举String getParameter(String name);// 返回指定name的参数值Enumeration getParameterNames();// 返回可用参数名的枚举String[] getParameterValues(String name);// 返回包含参数name的所有制的数组ServletInputStream geetInputStream();// 得到请求体中一行的二进制流BufferedReader getReader();// 返回解码过了的请求体String getServerName();// 返回接收请求的服务器主机名int getServerPort();// 返回服务器接收此请求所用的端口号String getRemoteAddr();// 返回发送请求的客户端的IP地址String getRemoteHost();// 返回发送请求的客户端主机名String getRealPath();// 返回一个虚拟路径的真实路径String getCharacterEncoding();// 返回字符编码方式int geContentLength();// 返回请求体的长度 ( 字节数 )String getContentType();// 返回请求体的MIME类型String getProtocol();// 返回请求用的协议类型以及版本号String getScheme();// 返回请求用的协议名称( 例如 : http  https  ftp )
```

### 2.response

**作用：** 表示服务器的响应，HttpServletResponse的实例，作用域为页面执行期。

**方法：**

```java
String getCharacterEncoding();// 返回响应用的是哪种字符编码ServletOutputStream getOutputStream();// 返回响应的一个二进制输出流PrintWriter getWriter();// 返回可以向客户端输出字符的一个对象void setContentLength(int len);// 设置响应头长度void setContentType(String type);// 设置响应的MIME类型void sendRedirect(String location);// 重新定向客户端的请求
```

### 3.session

**作用：** 表示客户端和服务器的一次会话，HttpSession的实例，作用域为会话期。

**方法：**

```java
long getCreationTime();// 返回SESSION创建时间public String getId();// 返回SESSION创建时JSP引擎为它设的惟一ID号long getLastAccessedTime();// 返回此SESSION里客户端最近一次请求时间int getMaxInactiveInterval();// 返回两次请求间隔多长时间此SESSION被取消(ms)String[] getValueNames();// 返回一个包含此SESSION中所有可用属性的数组void invalidate();// 取消SESSION，使SESSION不可用
```

### 4.out

**作用：** 用于向客户端输出内容，JspWriter的实例，作用域为页面执行期。

**方法：**

```java
void clear();// 清除缓冲区的内容void clearBuffer();// 清除缓冲区的当前内容void flush();// 清空流int getBufferSize();// 返回缓冲区以字节数的大小，如不设缓冲区则为0int getRemaining();// 返回缓冲区还剩余多少可用boolean isAutoFlush();// 返回缓冲区满时，是自动清空还是抛出异常void close();// 关闭输出流
```

### 5.page

**作用：** 表示当前JSP页面，类似this，Object的实例，作用域为页面执行期。

**方法：**

```java
class getClass();// 返回此Object的类int hashCode();// 返回此Object的hash码boolean equals(Object obj);// 判断此Object是否与指定的Object对象相等void copy(Object obj);// 把此Object拷贝到指定的Object对象中Object clone();// 克隆此Object对象String toString();// 把此Object对象转换成String类的对象void notify();// 唤醒一个等待的线程void notifyAll();// 唤醒所有等待的线程void wait(int timeout);// 使一个线程处于等待直到timeout结束或被唤醒void wait();// 使一个线程处于等待直到被唤醒void enterMonitor();// 对Object加锁void exitMonitor();// 对Object开锁
```

### 6.application

**作用：** 表示整个Web工程，ServletContext的实例，作用域为全局。

**方法：**

```java
Object getAttribute(String name);// 返回给定名的属性值Enumeration getAttributeNames();// 返回所有可用属性名的枚举void setAttribute(String name,Object obj);// 设定属性的属性值void removeAttribute(String name);// 删除一属性及其属性值String getServerInfo();// 返回JSP(SERVLET)引擎名及版本号String getRealPath(String path);// 返回一虚拟路径的真实路径ServletContext getContext(String uripath);// 返回指定WebApplication的application对象int getMajorVersion();// 返回服务器支持的Servlet API的最大版本号int getMinorVersion();// 返回服务器支持的Servlet API的最大版本号String getMimeType(String file);// 返回指定文件的MIME类型URL getResource(String path);// 返回指定资源(文件及目录)的URL路径InputStream getResourceAsStream(String path);// 返回指定资源的输入流RequestDispatcher getRequestDispatcher(String uripath);// 返回指定资源的RequestDispatcher对象Servlet getServlet(String name);// 返回指定名的ServletEnumeration getServlets();// 返回所有Servlet的枚举Enumeration getServletNames();// 返回所有Servlet名的枚举void log(String msg);// 把指定消息写入Servlet的日志文件void log(Exception exception,String msg);// 把指定异常的栈轨迹及错误消息写入Servlet的日志文件void log(String msg,Throwable throwable);// 把栈轨迹及给出的Throwable异常的说明信息 写入Servlet的日志文件
```

### 7.pageContext

**作用：** 表示当前JSP页面的上下文对象，不但包含本页面内容，还包含部分工程属性，PageContext的实例，作用域为页面执行期。

**方法：**

```java
JspWriter getOut();// 返回当前客户端响应被使用的JspWriter流(out)HttpSession getSession();// 返回当前页中的HttpSession对象(session)Object getPage();// 返回当前页的Object对象(page)ServletRequest getRequest();// 返回当前页的ServletRequest对象(request)ServletResponse getResponse();// 返回当前页的ServletResponse对象(response)Exception getException();// 返回当前页的Exception对象(exception)ServletConfig getServletConfig();// 返回当前页的ServletConfig对象(config)ServletContext getServletContext();// 返回当前页的ServletContext对象(application)void setAttribute(String name,Object attribute);// 设置属性及属性值void setAttribute(String name,Object obj,int scope);// 在指定范围内设置属性及属性值public Object getAttribute(String name);// 取属性的值Object getAttribute(String name,int scope);// 在指定范围内取属性的值public Object findAttribute(String name);// 寻找一属性,返回起属性值或NULLvoid removeAttribute(String name);// 删除某属性void removeAttribute(String name,int scope);// 在指定范围删除某属性int getAttributeScope(String name);// 返回某属性的作用范围Enumeration getAttributeNamesInScope(int scope);// 返回指定范围内可用的属性名枚举void release();// 释放pageContext所占用的资源void forward(String relativeUrlPath);// 使当前页面重导到另一页面void include(String relativeUrlPath);// 在当前位置包含另一文件
```

### 8.config

**作用：** 表示Servlet的初始化配置，ServletConfig的实例，作用域为页面执行期。

**方法：**

```java
ServletContext getServletContext();// 返回含有服务器相关信息的ServletContext对象String getInitParameter(String name);// 返回初始化参数的值Enumeration getInitParameterNames();// 返回Servlet初始化所需所有参数的枚举
```

### 9.exception

**作用：** 表示页面中发生异常，Throwable的实例，作用域为页面执行期。

**方法：**

```java
String getMessage();// 返回描述异常的消息String toString();// 返回关于异常的简短描述消息void printStackTrace();// 显示异常及其栈轨迹Throwable FillInStackTrace();// 重写异常的执行栈轨迹
```

### 10.总结

|对象|说明|类型|作用域|
|---|---|---|---|
|request|请求对象|javax.servlet.ServletRequest|Request|
|Response|响应对象|javax.servlet.SrvletResponse|Page|
|pageContext|页面上下文对象|javax.servlet.jsp.PageContext|Page|
|session|会话对象|javax.servlet.http.HttpSession|Session|
|applocation|应用程序对象|javax.servlet.ServletContext|Application|
|out|输出对象|javax.servlet.jsp.JspWriter|Page|
|config|配置对象|javax.servlet.ServletConfig|Page|
|page|页面对象|javax.lang.Object|Page|
|exception|异常对象|javax.lang.Throwable|Page|

### 11.细节

out和Response.getWriter()都是在浏览器输出内容，区别是什么？

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668696376623-b4b64f07-e774-446e-8b82-c33e51173321.png#averageHue=%23fcfcfb&clientId=u946b39c0-4311-4&from=paste&height=454&id=ueafc4593&originHeight=454&originWidth=1325&originalType=binary&ratio=1&rotation=0&showTitle=false&size=273925&status=done&style=none&taskId=u5e6c96c0-4fa1-4d43-be4f-ee965f4fe31&title=&width=1325)

image.png

结论：一般统一使用out输出，两种输出混合使用的话，out输出会追加到response输出的后面，会打乱顺序

## 六、四大域对象

> 域对象，表示可以存取数据的对象，作用为数据的传输

### 1.对象作用域

1. pageContext：一个属性只能在一个页面中存取，跳转其他页面无法获取。
2. Request：一个页面中设置的属性，只要经过了**请求重定向**之后的页面可以继续取得。
3. session：一个用户设置的内容，只要是与此用户相关的页面都可以访问。
4. application：在整个服务器上设置的属性，所有人都可以访问。

### 2.作用域大小顺序

四个域在使用上存在优先顺序，按域范围的从小到大排序：

pageContext——>request——>session——>application

## 七、JSP应用示例

### 1.浏览器页面显示数据

Servlet

```java
@Overrideprotected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {    ArrayList<User> usersList = new ArrayList<>();    //模拟数据    for (int i=0;i<10;i++){        usersList.add(new User(i,"name"+i,12+i,"phone"+i));    }    req.setAttribute("user",usersList);    req.getRequestDispatcher("jsp/e.jsp").forward(req,resp);}
```

jsp

```
<body>
<%
    List<User> users = (List<User>) request.getAttribute("user");
%>
<table>
    <tr>
        <td>编号</td>
        <td>姓名</td>
        <td>年龄</td>
        <td>电话</td>
        <td>操作</td>
    </tr>
    <%for (User user : users) {%>
    <tr>
        <td><%=user.getId()%></td>
        <td><%=user.getName()%></td>
        <td><%=user.getAge()%></td>
        <td><%=user.getPhone()%></td>
        <td>添加、删除</td>
    </tr>
    <%}%>
</table>
</body>
```

# 八。EL

## 一、EL表达式

### 1.什么是EL表达式

- EL（Expression Language）中文名为表达式语言，通常称为EL表达式。
- El表达式的作用是**替代JSP表达式**。好处是使输出语句变得简洁。
- 注意：对于 JSP2.0，需要在 Page 指令中将 **isELIgnored** 属性设置为 **false**，表示正常执行 EL表达式。

### 2.EL表达式的使用

为了更直观的感受EL表达式的简介，可以和JSP表达式进行一个对比： JSP表达式：

```
application中的值：<%= application.getAttribute("name") %> <br>
 session中的值：<%= session.getAttribute("name") %> <br>
 request中的值：<%= request.getAttribute("name") %> <br>
 pageContext中的值：<%= pageContext.getAttribute("name") %> <br>
```

EL表达式：

```
application中的值：${applicationScope.name} <br>
 session中的值：${sessionScope.name} <br>
 request中的值：${requestScope.name} <br>
 pageContext中的值：${pageScope.name} <br>
```

### 3.域对象的映射

从上面代码中我们可以看到，似乎JSP表达式和EL表达式的域对象不一致，这是因为EL表达式只能获取域对象中的数据，而且主要作用也是为了输出数据，所以对JSP域对象及从域对象中获取数据的方法进行封装，使EL表达式变得更加简介。其他方面与JSP表达式一致。 JSP域对象和EL域对象的对比：

|JSP|EL|
|---|---|
|pageContext|pageScope|
|request|requestScope|
|session|sessionScope|
|application|applicationScope|

### 4.EL表达式的缺陷

虽然EL使用方便，但也有缺点：

1. 只能读取域对象中的值，不能写入。
2. 不支持if判断和控制语句。

### 5.细节

**EL 表达式在输出 null 值的时候，输出的是空串。jsp 表达式脚本输出 null 值的时候，输出的是 “null” 字符串**。

## 二、JSTL

### 1.什么是JSTL？

- JSTL（JSP Standrad Tag Lib）中文名为**JSP标准标签库**。
- JSTL 作用是**代替JSP脚本**。好处是使 JSP 页面更加简介，在 JSP 页面中尽量不出现 java 代码，达到**解耦**的效果。

### 2.JSTL的组成

JSTL 由五个不同功能的标签库组成，其中核心标签库最为重要：

|功能范围|URI|前缀|
|---|---|---|
|核心标签库|[http://java.sun.com/jsp/jstl/core](http://java.sun.com/jsp/jstl/core)|c|
|格式化|[http://java.sun.com/jsp/jstl/fmt](http://java.sun.com/jsp/jstl/fmt)|fmt|
|函数|[http://java.sun.com/jsp/jstl/functions](http://java.sun.com/jsp/jstl/functions)|fn|
|数据库|[http://java.sun.com/jsp/jstl/sql](http://java.sun.com/jsp/jstl/sql)|sql|
|XML|[http://java.sun.com/jsp/jstl/xml](http://java.sun.com/jsp/jstl/xml)|x|

### 3.JSTL的环境搭建

1. 添加依赖
2. 使用 Taglib 指令引入标签库 <%@taglib prefix=“c” uri=“[http://java.sun.com/jsp/jstl/core”](http://java.sun.com/jsp/jstl/core%E2%80%9D) %>❗**注意**：如果是 Servlet 2.0 以上，需要在 Page 指令中设置 isELIgnored 属性为 false，表示正常执行 EL表达式。 <%@ page contentType=“text/html;charset=UTF-8” language=“java” isELIgnored=“false” %>

```
<dependency>
   <groupId>jstl</groupId>
   <artifactId>jstl</artifactId>
   <version>1.2</version>
 </dependency>
```

### 4.core 标签库的使用

### （1）**<c:set/>**

**用于设置域对象中的共享数据**

```
<body>
 <!--在page域中存入数据-->
 <c:set scope="page" var="name" value="张三"/>
 <!--使用EL表达式输出数据-->
 ${pageScope.name}
 </body>
```

### （2）**<c:if/>**

**用于做 if 判断**

```
<body>
 <!--设置域对象-->
 <c:set scope="page" var="age" value="20"/>
 <!--判断，true执行，false不执行-->
 <c:if test="${age>=18}">
     <h1>欢迎光临</h1>
 </c:if>
 <c:if test="${age<18}">
     <h1>未成年人禁止入内！</h1>
 </c:if>
 </body>
```

### （3）**<c:choose>**

**用于多分支判断，类似 switch**

```
<body>
 <!--设置域对象-->
 <c:set scope="page" var="height" value="200"/>
 <!--判断-->
 <c:choose>
     <c:when test="${height<=160}">
         <h1>有点矮</h1>
     </c:when>
     <c:when test="${height<=170}">
         <h1>可以接受</h1>
     </c:when>
     <c:when test="${height<=180}">
         <h1>一般水平</h1>
     </c:when>
     <c:when test="${height<=200}">
         <h1>有点意思</h1>
     </c:when>
     <c:otherwise>
         <h1>您去NBA吧！</h1>
     </c:otherwise>
 </c:choose>
 </body>
```

注意：

1. 只会有一个被选中。
2. choose 标签可以和 otherwise 标签相互嵌套。

### （4）**<c:forEach/>**

**用于遍历** 标签属性：

- begin：开始的索引
- end：结束的索引
- var：循环变量（遍历到的数据）
- items：遍历的数据源
- step：递增或者递减（默认递增1）

```
// controller
 @Override
 protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
     req.setAttribute("students",new HashMap<String,Person>(){{
         put("s1",new Person("张三",22,"男"));
         put("s2",new Person("翠花",18,"女"));
     }});
     req.getRequestDispatcher("/jstl.jsp").forward(req, resp);
 }

 // jsp
 <body>
 <c:forEach items="${students}" var="student">
     ${student.key}
     ${student.value.name}
     ${student.value.age}
     ${student.value.gender}
 </c:forEach>
 </body>
```

## 5，EL 表达式

### 5.1 概述

EL（全称Expression Language ）表达式语言，用于简化 JSP 页面内的 Java 代码。 EL 表达式的主要作用是 **获取数据**。其实就是从域对象中获取数据，然后将数据展示在页面上。 而 EL 表达式的语法也比较简单，**_expression_ *  * 。_例如_：{brands} 就是获取域中存储的 key 为 brands 的数据。

### 5.2 代码演示

- 定义servlet，在 servlet 中封装一些数据并存储到 request 域对象中并转发到 **el-demo.jsp** 页面。
- 在 **el-demo.jsp** 中通过 EL表达式 获取数据
- 在浏览器的地址栏输入 [**](http://localhost:8080/jsp-demo/demo1)[http://localhost:8080/jsp-demo/demo1**](http://localhost:8080/jsp-demo/demo1**) ，页面效果如下：

```
@WebServlet("/demo1")
 public class ServletDemo1 extends HttpServlet {
     @Override
     protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
         //1. 准备数据
         List<Brand> brands = new ArrayList<Brand>();
         brands.add(new Brand(1,"三只松鼠","三只松鼠",100,"三只松鼠，好吃不上火",1));
         brands.add(new Brand(2,"优衣库","优衣库",200,"优衣库，服适人生",0));
         brands.add(new Brand(3,"小米","小米科技有限公司",1000,"为发烧而生",1));

         //2. 存储到request域中
         request.setAttribute("brands",brands);

         //3. 转发到 el-demo.jsp
         request.getRequestDispatcher("/el-demo.jsp").forward(request,response);
     }

     @Override
     protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
         this.doGet(request, response);
     }
 }
```

**注意：** 此处需要用转发，因为转发才可以使用 request 对象作为域对象进行数据共享

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
 <html>
 <head>
     <title>Title</title>
 </head>
 <body>
     ${brands}
 </body>
 </html>
```

### 5.3 域对象

JavaWeb中有四大域对象，分别是：

- page：当前页面有效
- request：当前请求有效
- session：当前会话有效
- application：当前应用有效

el 表达式获取数据，会依次从这4个域中寻找，直到找到为止。而这四个域对象的作用范围如下图所示 例如： ${brands}，el 表达式获取数据，会先从page域对象中获取数据，如果没有再到 requet 域对象中获取数据，如果再没有再到 session 域对象中获取，如果还没有才会到 application 中获取数据。

## 6，JSTL标签

### 6.1 概述

JSP标准标签库(Jsp Standarded Tag Library) ，使用标签取代JSP页面上的Java代码。如下代码就是JSTL标签

```
<c:if test="${flag == 1}">
     男
 </c:if>
 <c:if test="${flag == 2}">
     女
 </c:if>
```

上面代码看起来是不是比 JSP 中嵌套 Java 代码看起来舒服好了。而且前端工程师对标签是特别敏感的，他们看到这段代码是能看懂的。 JSTL 提供了很多标签，如下图 我们只对两个最常用的标签进行讲解，**<c:forEach>** 标签和 **<c:if>** 标签。 JSTL 使用也是比较简单的，分为如下步骤：

- 导入坐标
- 在JSP页面上引入JSTL标签库 <%@ taglib prefix=“c” uri=“[http://java.sun.com/jsp/jstl/core”](http://java.sun.com/jsp/jstl/core%E2%80%9D) %>
- 使用标签

```
<dependency>
     <groupId>jstl</groupId>
     <artifactId>jstl</artifactId>
     <version>1.2</version>
 </dependency>
 <dependency>
     <groupId>taglibs</groupId>
     <artifactId>standard</artifactId>
     <version>1.1.2</version>
 </dependency>
```

### 6.2 if 标签

**<c:if>**：相当于 if 判断

- 属性：test，用于定义条件表达式

```
<c:if test="${flag == 1}">
     男
 </c:if>
 <c:if test="${flag == 2}">
     女
 </c:if>
```

**代码演示：**

- 定义一个 **servlet** ，在该 **servlet** 中向 request 域对象中添加 键是 **status** ，值为 **1** 的数据
- 定义 **jstl-if.jsp** 页面，在该页面使用 **<c:if>** 标签

```
@WebServlet("/demo2")
 public class ServletDemo2 extends HttpServlet {
     @Override
     protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
         //1. 存储数据到request域中
         request.setAttribute("status",1);

         //2. 转发到 jstl-if.jsp
         数据request.getRequestDispatcher("/jstl-if.jsp").forward(request,response);
     }

     @Override
     protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
         this.doGet(request, response);
     }
 }
```

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
 <%@ taglib prefix="c" uri="<http://java.sun.com/jsp/jstl/core>" %>
 <html>
 <head>
     <title>Title</title>
 </head>
 <body>
     <%--
         c:if：来完成逻辑判断，替换java  if else
     --%>
     <c:if test="${status ==1}">
         启用
     </c:if>

     <c:if test="${status ==0}">
         禁用
     </c:if>
 </body>
 </html>
```

**注意：** 在该页面已经要引入 JSTL核心标签库 **<%@ taglib prefix=“c” uri=“[http://java.sun.com/jsp/jstl/core”](http://java.sun.com/jsp/jstl/core%E2%80%9D) %>**

### 6.3 forEach 标签

**<c:forEach>**：相当于 for 循环。java中有增强for循环和普通for循环，JSTL 中的 **<c:forEach>** 也有两种用法

### 6.3.1 用法一

类似于 Java 中的增强for循环。涉及到的 **<c:forEach>** 中的属性如下

- items：被遍历的容器
- var：遍历产生的临时变量
- varStatus：遍历状态对象

如下代码，是从域对象中获取名为 brands 数据，该数据是一个集合；遍历遍历，并给该集合中的每一个元素起名为 **brand**，是 Brand对象。在循环里面使用 EL表达式获取每一个Brand对象的属性值

```
<c:forEach items="${brands}" var="brand">
     <tr align="center">
         <td>${brand.id}</td>
         <td>${brand.brandName}</td>
         <td>${brand.companyName}</td>
         <td>${brand.description}</td>
     </tr>
 </c:forEach>
```

**代码演示：**

- **servlet** 还是使用之前的名为 **ServletDemo1** 。
- 定义名为 **jstl-foreach.jsp** 页面，内容如下：

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
 <%@ taglib prefix="c" uri="<http://java.sun.com/jsp/jstl/core>" %>

 <!DOCTYPE html>
 <html lang="en">
 <head>
     <meta charset="UTF-8">
     <title>Title</title>
 </head>
 <body>
 <input type="button" value="新增"><br>
 <hr>
 <table border="1" cellspacing="0" width="800">
     <tr>
         <th>序号</th>
         <th>品牌名称</th>
         <th>企业名称</th>
         <th>排序</th>
         <th>品牌介绍</th>
         <th>状态</th>
         <th>操作</th>
     </tr>

     <c:forEach items="${brands}" var="brand" varStatus="status">
         <tr align="center">
             <%--<td>${brand.id}</td>--%>
             <td>${status.count}</td>
             <td>${brand.brandName}</td>
             <td>${brand.companyName}</td>
             <td>${brand.ordered}</td>
             <td>${brand.description}</td>
             <c:if test="${brand.status == 1}">
                 <td>启用</td>
             </c:if>
             <c:if test="${brand.status != 1}">
                 <td>禁用</td>
             </c:if>
             <td><a href="#">修改</a> <a href="#">删除</a></td>
         </tr>
     </c:forEach>
 </table>
 </body>
 </html>
```

### 6.3.2 用法二

类似于 Java 中的普通for循环。涉及到的 **<c:forEach>** 中的属性如下

- begin：开始数
- end：结束数
- step：步长

实例代码： 从0循环到10，变量名是 **i** ，每次自增1

```
<c:forEach begin="0" end="10" step="1" var="i">
     ${i}
 </c:forEach>
```

