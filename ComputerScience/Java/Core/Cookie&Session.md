## 一、简介

### 1.什么是Cookie

是服务器发送到用户浏览器并保存在本地的键值对数据。并在下次访问同一服务器时被浏览器携带并发送到服务器。这样服务器就可以通过分析Cookie，做到种种业务操作。

### 2.Cookie的作用

Cookie主要用于一下三个方面：

1. 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）。
2. 个性化设置（如用户自定义设置、主题等）。
3. 浏览器行为跟踪（如跟踪分析用户行为等）。

### 2.什么是Session

Session代表着服务器和浏览器的一次会话过程。Session 对象存储特定用户会话所需的属性及配置信息。这样，当用户在应用程序的 Web 页之间跳转时，存储在 Session 对象中的变量将不会丢失，而是在整个用户会话中一直存在下去。当客户端关闭会话，或者 Session 超时失效时会话结束。

### 3.Cookie和Session的不同

- 作用范围不同，Cookie 保存在客户端（浏览器），Session 保存在服务器端。
- 存取方式的不同，Cookie 只能保存 ASCII，Session 可以存任意数据类型，一般情况下我们可以在 Session 中保持一些常用变量信息，比如说 UserId 等。
- 有效期不同，Cookie 可设置为长时间保持，比如我们经常使用的默认登录功能，Session 一般失效时间较短，客户端关闭或者 Session 超时都会失效。
- 隐私策略不同，Cookie 存储在客户端，比较容易遭到不法获取，早期有人将用户的登录名和密码存储在 Cookie 中导致信息被窃取；Session 存储在服务端，安全性相对 Cookie 要好一些。
- 存储大小不同， 单个 Cookie 保存的数据不能超过 4K，Session 可存储数据远高于 Cookie。

### 4.Cookie和Session的关联

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668696180034-5d275319-f079-4091-a2eb-ef4d2c8bafa5.png#averageHue=%23f7f9ee&clientId=u0047bd19-4f84-4&from=paste&height=672&id=u7e4b1881&originHeight=672&originWidth=1404&originalType=binary&ratio=1&rotation=0&showTitle=false&size=900369&status=done&style=none&taskId=u19a13c72-b54c-4eb3-9b5d-29e43670c3b&title=&width=1404)

image.png

## 二、使用

### 1.Cookie

### 创建Cookie

```java
// 创建CookieCookie cookie = new Cookie("name1", "value");// 将Cookie添加到响应中resp.addCookie(cookie);
```

### 获取Cookie

```java
Cookie[] cookies = req.getCookies();for (Cookie cookie : cookies) {    System.out.println(cookie);    System.out.println(cookie.getName()+":"+cookie.getValue());}
```

### 修改Cookie值

```java
// 为同一个key重新赋值Cookie cookie = new Cookie("name1","value1")// 提交resp.addCookie(cookie)
```

### 浏览器查看Cookie

### 第一次访问

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668696193408-84815f8a-4afb-40b3-bef2-256f9b88ef0d.png#averageHue=%23272b33&clientId=u0047bd19-4f84-4&from=paste&height=817&id=ue79474cb&originHeight=817&originWidth=1116&originalType=binary&ratio=1&rotation=0&showTitle=false&size=134540&status=done&style=none&taskId=u81f83f1f-dfc3-4e23-8e4c-8baefd72da7&title=&width=1116)

image.png

可以看到第一次访问服务器在Http请求报文中并没有看到Cookie。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668696273032-53037ccd-4c0c-4c80-96cb-a84469fc9e21.png#averageHue=%23252931&clientId=u0047bd19-4f84-4&from=paste&height=817&id=u34966ddb&originHeight=817&originWidth=1115&originalType=binary&ratio=1&rotation=0&showTitle=false&size=115088&status=done&style=none&taskId=u645f3691-5310-4404-a692-659a8728dea&title=&width=1115)

image.png

在浏览器Cookie中已经添加上了服务器发送来的Cookie。

### 第二次访问

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668696287646-dc606849-2132-4097-9b58-2d715245192a.png#averageHue=%23272c34&clientId=u0047bd19-4f84-4&from=paste&height=816&id=u538a3a70&originHeight=816&originWidth=1115&originalType=binary&ratio=1&rotation=0&showTitle=false&size=133715&status=done&style=none&taskId=u2d658658-9516-41d7-833c-825532f5291&title=&width=1115)

image.png

可以看到在第二次请求访问中，Http请求报文已经携带上了Cookie。

### 删除Cookie

Cookie是服务器发送到浏览器，让浏览器保存的一段数据，所有要删除Cookie，需要在浏览器上删除。

![](https://cdn.nlark.com/yuque/0/2022/png/29046015/1668696297818-57db26ee-3cd4-4b98-a646-2ed118c3bcb3.png#averageHue=%23252b33&clientId=u0047bd19-4f84-4&from=paste&height=817&id=udfb68ccd&originHeight=817&originWidth=1115&originalType=binary&ratio=1&rotation=0&showTitle=false&size=119210&status=done&style=none&taskId=u297b61d6-1716-41dc-a9d1-9c3d8920b89&title=&width=1115)

image.png

### Cookie的生命控制

```java
cookie.setMaxAge()// 正数：表示在指定时间后过期，单位为秒// 负数：表示关闭浏览器立即删除Cookie(默认值：-1)// 零：表示马上删除Cookie
```

Cookie的生命控制写在提交之前，或者修改之后，必须再次提交。

如果想要删除服务器中的Cookie对象，可以将目标Cookie对象的该方法值设置为0。

### Cookie的Path设置

可以设置Cookie的访问路径，也就是说不是所有的访问都会携带Cookie，只有在访问Cookie自身设置的路径时，才会携带此Cookie。

```java
cookie.setPath(req.getContextPath + "/service")// 只有在访问工程路径/service目录下的资源时，会携带Cookie。
```

### 免用户名登录

Servlet

```java
@Overrideprotected void doPost(HttpServletRequest req, HttpServletResponse resp) throws IOException {    resp.setContentType("text/html; charset=UTF-8");    String username = req.getParameter("username");    String password = req.getParameter("password");    if (username != null && password != null){        if (username.equals("张三") && password.equals("111")){            resp.getWriter().print("登陆成功！");            Cookie cookie = new Cookie("username", username);            resp.addCookie(cookie);        }    }}
```

JSP

```
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false"%>
<html>
<head>
    <title>login</title>
</head>
<body>
<div>
    <form action="/myHttpServlet" method="get">
        用户名：<input type="text" name="username" value="${cookie.username.value}"><br>
        密码：<input type="password" name="password"> <br>
        <input type="submit" value="登录">
    </form>
</div>
</body>
</html>
```

### 2.Session

### 创建（获取）Session

```java
Session session = req.getSession()
```

创建和获取Session的方法是一样的，因为只有一个Session对象。第一次调用该方法时，创建Session对象，之后调用该方法表示获取Session对象。

想要判断是否是新创建的Session可以调用 `isNew()` 方法。

```java
session.isNew()
```

### Session的生命控制

```java
session.setMaxInactiveInterval() // 设置超时时间// 正数：Session的超时时间，单位为秒// 负数：永不超时session.getMaxInactiveInterval() // 获取超时时间session.invalidate() // Session立马超时无效
```

Session的默认超时时间为30分钟。在Tomcat的web.xml中设置。

```xml
<session-config>
    <session-timeout>30</session-timeout>
</session-config>
```


