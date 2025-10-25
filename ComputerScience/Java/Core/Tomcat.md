# 1. Web概述

## 1.1 Web和JavaWeb的概念

**Web是全球广域网，也称为万维网(www)**，能够通过浏览器访问的网站。在我们日常的生活中，经常会使用浏览器去访问百度、京东、传智官网等这些网站，这些网站统称为Web网站。如下就是通过浏览器访问传智官网的界面:

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678694395180-622213c6-36a4-4e41-8fc9-f4efb95c4c51.png#averageHue=%23d8d8d2&clientId=uaf724859-4e16-4&from=paste&height=231&id=u2401cd85&originHeight=254&originWidth=1409&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=106252&status=done&style=none&taskId=u18b42f2b-c1ac-49c8-9fb2-7d00514e7ba&title=&width=1280.9090631461347)

image.png

我们知道了什么是Web，那么JavaWeb又是什么呢？顾名思义JavaWeb就是**用Java技术来解决相关web互联网领域的技术栈**。 等学习完JavaWeb之后，同学们就可以使用Java语言开发我们上述所说的网站。而国内很多大型网站公司也是首选Java语言来解决web互联网相关的问题。那都有哪些公司的系统是使用Java语言的呢?

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678694433319-b8a646fc-9edf-4bac-959b-cf3906d366fc.png#averageHue=%23f5ad85&clientId=uaf724859-4e16-4&from=paste&height=406&id=ud27019ba&originHeight=447&originWidth=825&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=215562&status=done&style=none&taskId=u4513035f-b5ef-4473-ac5d-b24ea088d66&title=&width=749.9999837441882)

image.png

使用Java语言开发互联网系统是有很多技术栈需要大家了解，具体都有哪些呢?

## 1.2 JavaWeb技术栈

了解JavaWeb技术栈之前，有一个很重要的概念要介绍。

### 1.2.1 B/S架构

什么是B/S架构? B/S 架构：Browser/Server，浏览器/服务器 架构模式，它的特点是，客户端只需要浏览器，应用程序的逻辑和数据都存储在服务器端。浏览器只需要请求服务器，获取Web资源，服务器把Web资源发送给浏览器即可。大家可以通过下面这张图来回想下我们平常的上网过程:

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678694486213-dbe15111-9c38-45b7-a64a-3d9107cd257b.png#averageHue=%23eae3df&clientId=uaf724859-4e16-4&from=paste&height=229&id=uc82f9fea&originHeight=252&originWidth=957&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=96554&status=done&style=none&taskId=ua6105b4f-a995-49d3-9779-5d624f40a6b&title=&width=869.9999811432582)

image.png

- 打开浏览器访问百度首页，输入要搜索的内容，点击回车或百度一下，就可以获取和搜索相关的内容
- 思考下搜索的内容并不在我们自己的点上，那么这些内容从何而来？答案很明显是从百度服务器返回给我们的
- 日常百度的小细节，逢年过节百度的logo会更换不同的图片，服务端发生变化，客户端不需做任务事情就能获取最新内容
- 所以说B/S架构的好处:易于维护升级：服务器端升级后，客户端无需任何部署就可以使用到新的版本。了解了什么是B/S架构后，作为后台开发工程师的我们将来主要关注的是服务端的开发和维护工作。在服务端将来会放很多资源,都有哪些资源呢?

### 1.2.2 静态资源

- 静态资源主要包含HTML、CSS、JavaScript、图片等，主要负责页面的展示。
- 我们之前已经学过前端网页制作三剑客(HTML+CSS+JavaScript),使用这些技术我们就可以制作出效果比较丰富的网页，将来展现给用户。但是由于做出来的这些内容都是静态的，这就会导致所有的人看到的内容将是一模一样。
- 在日常上网的过程中，我们除了看到这些好看的页面以外，还会碰到很多动态内容，比如我们常见的百度登录效果:

张三登录以后在网页的右上角看到的是 张三，而李四登录以后看到的则是李四。所以不同的用户访问相同的资源看到的内容大多数是不一样的，要想实现这样的效果，光靠静态资源是无法实现的。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678694550981-c6f9c2fd-e055-4723-911e-f3a91e5b1046.png#averageHue=%23fefdfd&clientId=uaf724859-4e16-4&from=paste&height=319&id=u9f9f1107&originHeight=351&originWidth=1353&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=58343&status=done&style=none&taskId=u18de4786-63cc-456d-8b31-0cf99903083&title=&width=1229.9999733404686)

image.png

### 1.2.3 动态资源

- 动态资源主要包含Servlet、JSP等，主要用来负责逻辑处理。
- 动态资源处理完逻辑后会把得到的结果交给静态资源来进行展示，动态资源和静态资源要结合一起使用。
- 动态资源虽然可以处理逻辑，但是当用户来登录百度的时候，就需要输入用户名和密码,这个时候我们就又需要解决的一个问题是，用户在注册的时候填入的用户名和密码、以及我们经常会访问到一些数据列表的内容展示(如下图所示)，这些数据都存储在哪里?我们需要的时候又是从哪里来取呢?

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678694705763-2dfdf9d9-d629-47f2-9795-942b2d9a1a33.png#averageHue=%23e8e8e8&clientId=uaf724859-4e16-4&from=paste&height=415&id=uf3d21327&originHeight=456&originWidth=1375&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=152555&status=done&style=none&taskId=u33a216d7-0451-4fd2-ab49-cb76e284fde&title=&width=1249.9999729069802)

image.png

### 1.2.4 数据库

- 数据库主要负责存储数据。
- 整个Web的访问过程就如下图所示:

(1)浏览器发送一个请求到服务端，去请求所需要的相关资源; (2)资源分为动态资源和静态资源,动态资源可以是使用Java代码按照Servlet和JSP的规范编写的内容; (3)在Java代码可以进行业务处理也可以从数据库中读取数据; (4)拿到数据后，把数据交给HTML页面进行展示,再结合CSS和JavaScript使展示效果更好; (5)服务端将静态资源响应给浏览器; (6)浏览器将这些资源进行解析; (7)解析后将效果展示在浏览器，用户就可以看到最终的结果。在整个Web的访问过程中，会涉及到很多技术，这些技术有已经学习过的，也有还未涉及到的内容，都有哪些还没有涉及到呢?

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678694752708-f881c25f-ddc9-4ebd-94ba-7c2d6a18b357.png#averageHue=%23cc9669&clientId=uaf724859-4e16-4&from=paste&height=212&id=u11809afe&originHeight=233&originWidth=1134&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=140604&status=done&style=none&taskId=ub55c5a47-7129-43f0-8cb0-80e7299803f&title=&width=1030.9090685647386)

image.png

### 1.2.5 HTTP协议

- HTTP协议:主要定义通信规则
- 浏览器发送请求给服务器，服务器响应数据给浏览器，这整个过程都需要遵守一定的规则，之前大家学习过TCP、UDP，这些都属于规则，这里我们需要使用的是HTTP协议，这也是一种规则。

### 1.2.6 Web服务器

- Web服务器:负责解析 HTTP 协议，解析请求数据，并发送响应数据
- 浏览器按照HTTP协议发送请求和数据，后台就需要一个Web服务器软件来根据HTTP协议解析请求和数据，然后把处理结果再按照HTTP协议发送给浏览器
- Web服务器软件有很多，我们课程中将学习的是目前最为常用的**Tomcat**服务器

到这为止，关于JavaWeb中用到的技术栈我们就介绍完了，这里面就只有HTTP协议、Servlet、JSP以及Tomcat这些知识是没有学习过的，所以整个Web核心主要就是来学习这些技术。

## 1.3 Web核心课程安排

整个Web核心，我们总共有六天的学习内容，分别是:

- 第一天：HTTP、Tomcat、Servlet
- 第二天：Request(请求)、Response(响应)
- 第三天：JSP、会话技术(Cookie、Session)
- 第四天：Filter(过滤器)、Listener(监听器)
- 第五天：Ajax、Vue、ElementUI
- 第六天：综合案例

(1)Request是从客户端向服务端发出的请求对象， (2)Response是从服务端响应给客户端的结果对象， (3)JSP是动态网页技术, (4)会话技术是用来存储客户端和服务端交互所产生的数据， (5)过滤器是用来拦截客户端的请求, (6)监听器是用来监听特定事件, (7)Ajax、Vue、ElementUI都是属于前端技术 这些技术都该如何来使用，我们后面会一个个进行详细的讲解。接下来我们来学习下HTTP、Tomcat和Servlet。

# 2. HTTP

## 2.1 简介

**HTTP概念** HyperText Transfer Protocol，超文本传输协议，规定了浏览器和服务器之间数据传输的规则。

- 数据传输的规则指的是请求数据和响应数据需要按照指定的格式进行传输。
- 如果想知道具体的格式，可以打开浏览器，点击F12打开开发者工具，点击Network来查看某一次请求的请求数据和响应数据具体的格式内容，如下图所示:

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678695024839-cd86da8e-172b-402d-add1-4414e2bc07af.png#averageHue=%23ece9e0&clientId=uaf724859-4e16-4&from=paste&height=622&id=uec0fc5bc&originHeight=684&originWidth=1414&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=327242&status=done&style=none&taskId=ua3c316cb-1a46-4afb-b85f-575ef939ea0&title=&width=1285.454517593069)

image.png

> 注意:在浏览器中如果看不到上述内容，需要清除浏览器的浏览数据。chrome浏览器可以使用ctrl+shift+Del进行清除。

所以学习HTTP主要就是学习请求和响应数据的具体格式内容。 **HTTP协议特点** HTTP协议有它自己的一些特点，分别是:

- 基于TCP协议: 面向连接，安全TCP是一种面向连接的(建立连接之前是需要经过三次握手)、可靠的、基于字节流的传输层通信协议，在数据传输方面更安全。
- 基于请求-响应模型的:一次请求对应一次响应请求和响应是一一对应关系
- HTTP协议是无状态协议:对于事物处理没有记忆能力。每次请求-响应都是独立的

无状态指的是客户端发送HTTP请求给服务端之后，服务端根据请求响应数据，响应完后，不会记录任何信息。这种特性有优点也有缺点，

- 缺点:多次请求间不能共享数据
- 优点:速度快

请求之间无法共享数据会引发的问题，如:

- 京东购物，加入购物车和去购物车结算是两次请求，
- HTTP协议的无状态特性，加入购物车请求响应结束后，并未记录加入购物车是何商品
- 发起去购物车结算的请求后，因为无法获取哪些商品加入了购物车，会导致此次请求无法正确展示数据

具体使用的时候，我们发现京东是可以正常展示数据的，原因是Java早已考虑到这个问题，并提出了使用会话技术(Cookie、Session)来解决这个问题。具体如何来做，我们后面会详细讲到。刚才提到HTTP协议是规定了请求和响应数据的格式，那具体的格式是什么呢?

## 2.2 请求数据格式

### 2.2.1 格式介绍

请求数据总共分为三部分内容，分别是**请求行**、**请求头**、**请求体**

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678695285659-bb920b51-06e3-443c-b06f-91df958ae724.png#averageHue=%23f1efd6&clientId=uaf724859-4e16-4&from=paste&height=147&id=uf25513e7&originHeight=162&originWidth=508&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=54072&status=done&style=none&taskId=u88c89420-b43c-4fb8-bcf6-9758172125c&title=&width=461.8181718085425)

image.png

- 请求行: HTTP请求中的第一行数据，请求行包含三块内容，分别是 GET[请求方式] /[请求URL路径] HTTP/1.1[HTTP协议及版本]

请求方式有七种,最常用的是GET和POST

- 请求头: 第二行开始，格式为key: value形式请求头中会包含若干个属性，常见的HTTP请求头有:

```
Host: 表示请求的主机名
User-Agent: 浏览器版本,例如Chrome浏览器的标识类似Mozilla/5.0 ...Chrome/79，IE浏览器的标识类似Mozilla/5.0 (Windows NT ...)like Gecko；
Accept：表示浏览器能接收的资源类型，如text/*，image/*或者*/*表示所有；
Accept-Language：表示浏览器偏好的语言，服务器可以据此返回不同语言的网页；
Accept-Encoding：表示浏览器可以支持的压缩类型，例如gzip, deflate等。
```

**这些数据有什么用处?** 举例说明:服务端可以根据请求头中的内容来获取客户端的相关信息，有了这些信息服务端就可以处理不同的业务需求，比如:

- 不同浏览器解析HTML和CSS标签的结果会有不一致，所以就会导致相同的代码在不同的浏览器会出现不同的效果
- 服务端根据客户端请求头中的数据获取到客户端的浏览器类型，就可以根据不同的浏览器设置不同的代码来达到一致的效果
- 这就是我们常说的浏览器兼容问题
- 请求体: POST请求的最后一部分，存储请求参数

如上图红线框的内容就是请求体的内容，请求体和请求头之间是有一个空行隔开。此时浏览器发送的是POST请求，为什么不能使用GET呢?这时就需要回顾GET和POST两个请求之间的区别了:

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678695511011-08a19efd-d197-4892-9c0c-131c327b81ab.png#averageHue=%23efedd4&clientId=uaf724859-4e16-4&from=paste&height=166&id=u17f1c446&originHeight=183&originWidth=506&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=71904&status=done&style=none&taskId=u748cae7a-b512-44ec-9965-6b0815a4525&title=&width=459.99999002976875)

image.png

- GET请求请求参数在请求行中，没有请求体，POST请求请求参数在请求体中
- GET请求请求参数大小有限制，POST没有

### 2.2.2 实例演示

把 代码`\\http` 拷贝到IDEA的工作目录中，比如`D:\\workspace\\web`目录，

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678695591089-ef9eb147-efa4-444f-b483-d2ee7b0dd608.png#averageHue=%23fefefd&clientId=uaf724859-4e16-4&from=paste&height=173&id=u4f1c8475&originHeight=190&originWidth=797&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=7751&status=done&style=none&taskId=u65112d5f-f267-4b37-82ab-ff2c8961c7d&title=&width=724.5454388413551)

image.png

使用IDEA打开

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678695608259-1f1cf88a-f2e1-4074-a535-738fb384a5c9.png#averageHue=%23f6f5f5&clientId=uaf724859-4e16-4&from=paste&height=445&id=u6930280a&originHeight=490&originWidth=1266&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=42961&status=done&style=none&taskId=u35a4c497-930a-42dc-9a31-7841176cee6&title=&width=1150.9090659638086)

image.png

打开后，可以点击项目中的`html\\19-表单验证.html`，使用浏览器打开，通过修改页面中form表单的method属性来测试GET请求和POST请求的参数携带方式。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678695650516-4c10a50e-1e5e-4cb5-8a19-d4c532a6bf1a.png#averageHue=%23fcfcfb&clientId=uaf724859-4e16-4&from=paste&height=302&id=u3eca903d&originHeight=332&originWidth=370&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=9848&status=done&style=none&taskId=u013359d2-1920-4cc4-a540-2c3c4c9552c&title=&width=336.36362907315106)

image.png

小结:

1. 请求数据中包含三部分内容，分别是请求行、请求头和请求体
2. POST请求数据在请求体中，GET请求数据在请求行上

## 2.3 响应数据格式

### 2.3.1 格式介绍

响应数据总共分为三部分内容，分别是**响应行**、**响应头**、**响应体**

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678695709946-2a52eb79-af83-4a60-ba95-cc61c9ab09c9.png#averageHue=%23f8f5dc&clientId=uaf724859-4e16-4&from=paste&height=252&id=u6e3982e7&originHeight=277&originWidth=506&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=53299&status=done&style=none&taskId=u4ccf1438-d4fc-4dc6-83c5-7308a242fec&title=&width=459.99999002976875)

image.png

- 响应行：响应数据的第一行,响应行包含三块内容，分别是 HTTP/1.1[HTTP协议及版本] 200[响应状态码] ok[状态码的描述]
- 响应头：第二行开始，格式为key：value形式

响应头中会包含若干个属性，常见的HTTP响应头有:

```
Content-Type：表示该响应内容的类型，例如text/html，image/jpeg；
Content-Length：表示该响应内容的长度（字节数）；
Content-Encoding：表示该响应压缩算法，例如gzip；
Cache-Control：指示客户端应如何缓存，例如max-age=300表示可以最多缓存300秒
```

- 响应体： 最后一部分。存放响应数据，上图中<html>…</html>这部分内容就是响应体，它和响应头之间有一个空行隔开。

### 2.3.2 响应状态码

参考: 资料/1.HTTP/《[响应状态码.md](http://xn--vvr80uk5ak57bv7f.md)》 关于响应状态码，我们先主要认识三个状态码，其余的等后期用到了再去掌握:

- 200 ok 客户端请求成功
- 404 Not Found 请求资源不存在
- 500 Internal Server Error 服务端发生不可预期的错误

### 2.3.3 自定义服务器

在前面我们导入到IDEA中的http项目中，有一个Server.java类，这里面就是自定义的一个服务器代码，主要使用到的是ServerSocket和Socket

```java
package com.itheima;import sun.misc.IOUtils;import java.io.*;import java.net.ServerSocket;import java.net.Socket;import java.nio.charset.StandardCharsets;import java.nio.file.Files;/*    自定义服务器 */public class Server {    public static void main(String[] args) throws IOException {        ServerSocket ss = new ServerSocket(8080); // 监听指定端口        System.out.println("server is running...");        while (true){            Socket sock = ss.accept();            System.out.println("connected from " + sock.getRemoteSocketAddress());            Thread t = new Handler(sock);            t.start();        }    }}class Handler extends Thread {    Socket sock;    public Handler(Socket sock) {        this.sock = sock;    }    public void run() {        try (InputStream input = this.sock.getInputStream()) {            try (OutputStream output = this.sock.getOutputStream()) {                handle(input, output);            }        } catch (Exception e) {            try {                this.sock.close();            } catch (IOException ioe) {            }            System.out.println("client disconnected.");        }    }    private void handle(InputStream input, OutputStream output) throws IOException {        BufferedReader reader = new BufferedReader(new InputStreamReader(input, StandardCharsets.UTF_8));        BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(output, StandardCharsets.UTF_8));        // 读取HTTP请求:        boolean requestOk = false;        String first = reader.readLine();        if (first.startsWith("GET / HTTP/1.")) {            requestOk = true;        }        for (;;) {            String header = reader.readLine();            if (header.isEmpty()) { // 读取到空行时, HTTP Header读取完毕                break;            }            System.out.println(header);        }        System.out.println(requestOk ? "Response OK" : "Response Error");        if (!requestOk) {            // 发送错误响应:            writer.write("HTTP/1.0 404 Not Found\\r\\n");            writer.write("Content-Length: 0\\r\\n");            writer.write("\\r\\n");            writer.flush();        } else {            // 发送成功响应:            //读取html文件，转换为字符串            BufferedReader br = new BufferedReader(new FileReader("http/html/a.html"));            StringBuilder data = new StringBuilder();            String line = null;            while ((line = br.readLine()) != null){                data.append(line);            }            br.close();            int length = data.toString().getBytes(StandardCharsets.UTF_8).length;            writer.write("HTTP/1.1 200 OK\\r\\n");            writer.write("Connection: keep-alive\\r\\n");            writer.write("Content-Type: text/html\\r\\n");            writer.write("Content-Length: " + length + "\\r\\n");            writer.write("\\r\\n"); // 空行标识Header和Body的分隔            writer.write(data.toString());            writer.flush();        }    }}
```

上面代码，大家不需要自己写，主要通过上述代码，只需要大家了解到服务器可以使用java完成编写，是可以接受页面发送的请求和响应数据给前端浏览器的，真正用到的Web服务器，我们不会自己写，都是使用目前比较流行的web服务器，比如Tomcat 小结

1. 响应数据中包含三部分内容，分别是响应行、响应头和响应体
2. 掌握200，404，500这三个响应状态码所代表含义，分布是成功、所访问资源不存在和服务的错误

# 3. Tomcat

## 3.1 简介

### 3.1.1 什么是Web服务器

Web服务器是一个应该程序（**软件**），对HTTP协议的操作进行封装，使得程序员不必直接对协议进行操作，让Web开发更加便捷。主要功能是”提供网上信息浏览服务”。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678695886815-cbf29e01-ef9e-4eff-8a18-ac2101c49b47.png#averageHue=%23e7e2dd&clientId=uaf724859-4e16-4&from=paste&height=208&id=uc8ebafc0&originHeight=229&originWidth=846&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=89914&status=done&style=none&taskId=u3213264f-40cb-4cb7-96bb-c4c08b7015a&title=&width=769.0908924213129)

image.png

Web服务器是安装在服务器端的一款软件，将来我们把自己写的Web项目部署到Web Tomcat服务器软件中，当Web服务器软件启动后，部署在Web服务器软件中的页面就可以直接通过浏览器来访问了。 Web服务器软件使用步骤

- 准备静态资源
- 下载安装Web服务器软件
- 将静态资源部署到Web服务器上
- 启动Web服务器使用浏览器访问对应的资源

上述内容在演示的时候，使用的是Apache下的Tomcat软件，至于Tomcat软件如何使用，后面会详细的讲到。而对于Web服务器来说，实现的方案有很多，Tomcat只是其中的一种，而除了Tomcat以外，还有很多优秀的Web服务器，比如:

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678695919814-87961e9f-b951-449e-be5c-8a7f365d2f17.png#averageHue=%23e7ceab&clientId=uaf724859-4e16-4&from=paste&height=195&id=u01b179fe&originHeight=214&originWidth=1125&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=124362&status=done&style=none&taskId=u2a4126ff-04a5-4f3f-93a5-69bf9c32e71&title=&width=1022.7272505602566)

image.png

Tomcat就是一款软件，我们主要是以学习如何去使用为主。具体我们会从以下这些方向去学习:

1. 简介: 初步认识下Tomcat
2. 基本使用: 安装、卸载、启动、关闭、配置和项目部署，这些都是对Tomcat的基本操作
3. IDEA中如何创建Maven Web项目
4. IDEA中如何使用Tomcat,后面这两个都是我们以后开发经常会用到的方式

首选我们来认识下Tomcat。 **Tomcat** Tomcat的相关概念:

- Tomcat是Apache软件基金会一个核心项目，是一个开源免费的轻量级Web服务器，支持Servlet/JSP少量JavaEE规范。
- 概念中提到了JavaEE规范，那什么又是JavaEE规范呢?JavaEE: Java Enterprise Edition,Java企业版。指Java企业级开发的技术规范总和。包含13项技术规范:JDBC、JNDI、EJB、RMI、JSP、Servlet、XML、JMS、Java IDL、JTS、JTA、JavaMail、JAF。
- 因为Tomcat支持Servlet/JSP规范，所以Tomcat也被称为Web容器、Servlet容器。Servlet需要依赖Tomcat才能运行。
- Tomcat的官网: [https://tomcat.apache.org/](https://tomcat.apache.org/) 从官网上可以下载对应的版本进行使用。

Tomcat的LOGO

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678695965969-86a9bd28-2540-43e1-baa0-90438be994cd.png#averageHue=%23f4f2ef&clientId=uaf724859-4e16-4&from=paste&height=131&id=u59aa186b&originHeight=144&originWidth=331&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=18943&status=done&style=none&taskId=u96e3d7f6-3c82-4921-9c6e-f3b5f950032&title=&width=300.90908438706214)

image.png

小结 通过这一节的学习，我们需要掌握以下内容:

1. Web服务器的作用

> 封装HTTP协议操作，简化开发 可以将Web项目部署到服务器中，对外提供网上浏览服务

2. Tomcat是一个轻量级的Web服务器，支持Servlet/JSP少量JavaEE规范，也称为Web容器，Servlet容器。

## 3.2 基本使用

Tomcat总共分两部分学习，先来学习Tomcat的基本使用，包括Tomcat的下载、安装、卸载、启动和关闭。

### 3.2.1 下载

直接从官网下载

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696006028-107d84ab-34d7-4453-a2b0-79ff0d405017.png#averageHue=%23eee8de&clientId=uaf724859-4e16-4&from=paste&height=516&id=u86df121d&originHeight=568&originWidth=657&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=183388&status=done&style=none&taskId=ue0574461-43c8-4d42-af93-96c3c7b111f&title=&width=597.2727143271899)

image.png

大家可以自行下载，也可以直接使用资料中已经下载好的资源， Tomcat的软件程序 资料/2. Tomcat/apache-tomcat-8.5.68-windows-x64.zip Tomcat的源码 资料/2. Tomcat/tomcat源码/apache-tomcat-8.5.68-src.zip

### 3.2.2 安装

Tomcat是绿色版,直接解压即可

- 在D盘的software目录下，将apache-tomcat-8.5.68-windows-x64.zip进行解压缩，会得到一个apache-tomcat-8.5.68的目录，Tomcat就已经安装成功。注意，Tomcat在解压缩的时候，解压所在的目录可以任意，但最好解压到一个不包含中文和空格的目录，因为后期在部署项目的时候，如果路径有中文或者空格可能会导致程序部署失败。
- 打开apache-tomcat-8.5.68目录就能看到如下目录结构，每个目录中包含的内容需要认识下

bin:目录下有两类文件，一种是以.bat结尾的，是Windows系统的可执行文件，一种是以.sh结尾的，是Linux系统的可执行文件。webapps:就是以后项目部署的目录到此，Tomcat的安装就已经完成。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696062680-43ac4989-7be4-4fca-92c5-e4116eb74c48.png#averageHue=%23f8f3d7&clientId=uaf724859-4e16-4&from=paste&height=200&id=u807d3d9f&originHeight=220&originWidth=236&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=51068&status=done&style=none&taskId=u4558ab46-2f9a-4762-879b-6d8e65fe919&title=&width=214.54544989530714)

image.png

### 3.2.3 卸载

卸载比较简单，可以直接删除目录即可

### 3.2.4 启动

双击: bin.bat

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696100974-a366fdaf-b830-4dab-acbc-4faf93f202a4.png#averageHue=%23fbfcf7&clientId=uaf724859-4e16-4&from=paste&height=42&id=uf3749dfe&originHeight=46&originWidth=627&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=14857&status=done&style=none&taskId=u0eb69cc3-0433-49d9-9650-e07b25fd33e&title=&width=569.999987645583)

image.png

启动后，通过浏览器访问 http://localhost:8080能看到Apache Tomcat的内容就说明Tomcat已经启动成功。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696120314-debe98ba-9aeb-4f5e-b5d3-029e1f3179c6.png#averageHue=%23ddedc7&clientId=uaf724859-4e16-4&from=paste&height=584&id=u93f3d9af&originHeight=642&originWidth=1411&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=262570&status=done&style=none&taskId=u8188aadc-faad-4ed2-87c5-acd06c41dd0&title=&width=1282.7272449249085)

image.png

**注意**: 启动的过程中，控制台有中文乱码，需要修改conf/logging.prooperties

### 3.2.5 关闭

关闭有三种方式

- 直接x掉运行窗口:强制关闭[不建议]
- bin.bat：正常关闭
- ctrl+c： 正常关闭

### 3.2.6 配置

**修改端口**

- Tomcat默认的端口是8080，要想修改Tomcat启动的端口号，需要修改 conf/server.xml

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696157482-d5944a10-f541-4dc1-a42b-5e17ff2d4b6e.png#averageHue=%23373825&clientId=uaf724859-4e16-4&from=paste&height=102&id=ua2fb491b&originHeight=112&originWidth=688&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=67803&status=done&style=none&taskId=u5ba935f4-53dc-4059-9097-e5c837db44a&title=&width=625.4545318981835)

image.png

> 注: HTTP协议默认端口号为80，如果将Tomcat端口号改为80，则将来访问Tomcat时，将不用输入端口号。

**启动时可能出现的错误**

- Tomcat的端口号取值范围是0-65535之间任意未被占用的端口，如果设置的端口号被占用，启动的时候就会包如下的错误

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696183524-306b518d-e697-4ae7-b146-ba9e07549fea.png#averageHue=%23202722&clientId=uaf724859-4e16-4&from=paste&height=81&id=u51fc5d3f&originHeight=89&originWidth=635&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=95513&status=done&style=none&taskId=u89e72626-88f1-48a1-9e22-0947daef858&title=&width=577.2727147606781)

image.png

- Tomcat启动的时候，启动窗口一闪而过: 需要检查JAVA_HOME环境变量是否正确配置

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696198831-e0c66a45-2dfd-4491-be99-74b1045d876a.png#averageHue=%23f8f8f7&clientId=uaf724859-4e16-4&from=paste&height=527&id=uabe3f3fc&originHeight=580&originWidth=1407&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=336818&status=done&style=none&taskId=ud10706aa-889f-4d1f-a4cc-8792c982ddd&title=&width=1279.090881367361)

image.png

### 3.2.7 部署

- Tomcat部署项目： 将项目放置到webapps目录下，即部署完成。
    - 将 资料/2. Tomcat/hello 目录拷贝到Tomcat的webapps目录下
    - 通过浏览器访问[](http://localhost/hello/a.html%EF%BC%8C%E8%83%BD%E7%9C%8B%E5%88%B0%E4%B8%8B%E9%9D%A2%E7%9A%84%E5%86%85%E5%AE%B9%E5%B0%B1%E8%AF%B4%E6%98%8E%E9%A1%B9%E7%9B%AE%E5%B7%B2%E7%BB%8F%E9%83%A8%E7%BD%B2%E6%88%90%E5%8A%9F%E3%80%82)[http://localhost/hello/a.html，能看到下面的内容就说明项目已经部署成功。](http://localhost/hello/a.html%EF%BC%8C%E8%83%BD%E7%9C%8B%E5%88%B0%E4%B8%8B%E9%9D%A2%E7%9A%84%E5%86%85%E5%AE%B9%E5%B0%B1%E8%AF%B4%E6%98%8E%E9%A1%B9%E7%9B%AE%E5%B7%B2%E7%BB%8F%E9%83%A8%E7%BD%B2%E6%88%90%E5%8A%9F%E3%80%82)

但是呢随着项目的增大，项目中的资源也会越来越多，项目在拷贝的过程中也会越来越费时间，该如何解决呢?

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696234181-fa6f87c4-b6ca-4294-98b3-3940cbd2e221.png#averageHue=%2378756b&clientId=uaf724859-4e16-4&from=paste&height=383&id=u4a7e05e1&originHeight=421&originWidth=1356&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=312609&status=done&style=none&taskId=u4576e4d2-d3e9-44c3-b738-469cc731f71&title=&width=1232.7272460086292)

image.png

- 一般JavaWeb项目会被打包称war包，然后将war包放到Webapps目录下，Tomcat会自动解压缩war文件
    - 将 资料/2. Tomcat/haha.war目录拷贝到Tomcat的webapps目录下
    - Tomcat检测到war包后会自动完成解压缩，在webapps目录下就会多一个haha目录
    - 通过浏览器访问[](http://localhost/haha/a.html%EF%BC%8C%E8%83%BD%E7%9C%8B%E5%88%B0%E4%B8%8B%E9%9D%A2%E7%9A%84%E5%86%85%E5%AE%B9%E5%B0%B1%E8%AF%B4%E6%98%8E%E9%A1%B9%E7%9B%AE%E5%B7%B2%E7%BB%8F%E9%83%A8%E7%BD%B2%E6%88%90%E5%8A%9F%E3%80%82)[http://localhost/haha/a.html，能看到下面的内容就说明项目已经部署成功。](http://localhost/haha/a.html%EF%BC%8C%E8%83%BD%E7%9C%8B%E5%88%B0%E4%B8%8B%E9%9D%A2%E7%9A%84%E5%86%85%E5%AE%B9%E5%B0%B1%E8%AF%B4%E6%98%8E%E9%A1%B9%E7%9B%AE%E5%B7%B2%E7%BB%8F%E9%83%A8%E7%BD%B2%E6%88%90%E5%8A%9F%E3%80%82)

至此，Tomcat的部署就已经完成了，至于如何获得项目对应的war包，后期我们会借助于IDEA工具来生成。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696278010-4658ab8d-4f16-4892-8d8a-7d7c41e94848.png#averageHue=%2373736c&clientId=uaf724859-4e16-4&from=paste&height=413&id=u461bd497&originHeight=454&originWidth=1353&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=342756&status=done&style=none&taskId=uf2c514e2-2f9a-4038-9502-3fd57cbe82d&title=&width=1229.9999733404686)

image.png

## 3.3 Maven创建Web项目

介绍完Tomcat的基本使用后，我们来学习在IDEA中如何创建Maven Web项目，学习这种方式的原因是以后Tomcat中运行的绝大多数都是Web项目，而使用Maven工具能更加简单快捷的把Web项目给创建出来，所以Maven的Web项目具体如何来构建呢? 在真正创建Maven Web项目之前，我们先要知道Web项目长什么样子，具体的结构是什么?

### 3.3.1 Web项目结构

Web项目的结构分为:开发中的项目和开发完可以部署的Web项目,这两种项目的结构是不一样的，我们一个个来介绍下:

- Maven Web项目结构: 开发中的项目

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696325668-489fb5c9-b754-4984-9201-10b821dcd41a.png#averageHue=%23f6f3f2&clientId=uaf724859-4e16-4&from=paste&height=262&id=uda27a8de&originHeight=288&originWidth=552&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=97970&status=done&style=none&taskId=u0f44f8de-9749-4efd-be8b-c871f94813e&title=&width=501.8181709415659)

image.png

- 开发完成部署的Web项目

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696374499-c4803529-b168-49d1-8bb3-7c8ae39eafd6.png#averageHue=%23eeeadb&clientId=uaf724859-4e16-4&from=paste&height=165&id=uba61addf&originHeight=182&originWidth=544&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=74579&status=done&style=none&taskId=u79ca6ced-1eb9-4b97-bc6e-42c5d593274&title=&width=494.5454438264707)

image.png

- 开发项目通过执行Maven打包命令package,可以获取到部署的Web项目目录
- 编译后的Java字节码文件和resources的资源文件，会被放到WEB-INF下的classes目录下
- pom.xml中依赖坐标对应的jar包，会被放入WEB-INF下的lib目录下

### 3.3.2 创建Maven Web项目

介绍完Maven Web的项目结构后，接下来使用Maven来创建Web项目，创建方式有两种:使用骨架和不使用骨架 **使用骨架** 具体的步骤包含:

> 1.创建Maven项目 2.选择使用Web项目骨架 3.输入Maven项目坐标创建项目 4.确认Maven相关的配置信息后，完成项目创建 5.删除pom.xml中多余内容 6.补齐Maven Web项目缺失的目录结构

1. 创建Maven项目

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696439255-d3d303cd-286a-4926-baaf-e7e8f986425d.png#averageHue=%23e5dfd3&clientId=uaf724859-4e16-4&from=paste&height=711&id=ubfb3ce1b&originHeight=782&originWidth=1215&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=308555&status=done&style=none&taskId=u6af9f6ac-5f9f-4fa9-bd49-3894b4c1dc6&title=&width=1104.5454306050772)

image.png

1. 选择使用Web项目骨架

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696453937-ca8c774b-d7d9-4e13-a01e-12473e768351.png#averageHue=%23eeeee6&clientId=uaf724859-4e16-4&from=paste&height=716&id=u44161521&originHeight=788&originWidth=996&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=479846&status=done&style=none&taskId=ucc43fa10-d709-4c2b-bd3f-69be276b0d9&title=&width=905.4545258293472)

image.png

1. 输入Maven项目坐标创建项目

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696472693-73058fe8-6b3b-4709-952e-21cde7564065.png#averageHue=%23dfd5c9&clientId=uaf724859-4e16-4&from=paste&height=704&id=uf6abe22d&originHeight=774&originWidth=985&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=148157&status=done&style=none&taskId=uaecae913-6697-4df7-8bbe-613d409e7a1&title=&width=895.4545260460912)

image.png

1. 确认Maven相关的配置信息后，完成项目创建

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696496474-fd925c6d-b40a-417e-a841-db5a13bcdb9c.png#averageHue=%23ded3c4&clientId=uaf724859-4e16-4&from=paste&height=891&id=u4554127f&originHeight=980&originWidth=1129&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=223631&status=done&style=none&taskId=u395c6e3d-1080-487c-a372-3bd33e6a369&title=&width=1026.3636141178042)

image.png

1. 删除pom.xml中多余内容，只留下面的这些内容，注意打包方式 jar和war的区别

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696518411-d4e4bb65-9f81-484a-ae8d-59cd7fa29fb4.png#averageHue=%23fdfdfb&clientId=uaf724859-4e16-4&from=paste&height=505&id=uc1f61676&originHeight=556&originWidth=1264&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=213584&status=done&style=none&taskId=ua5e71902-89e6-45de-91b3-29d8afbdd86&title=&width=1149.0908841850348)

image.png

1. 补齐Maven Web项目缺失的目录结构，默认没有java和resources目录，需要手动完成创建补齐，最终的目录结果如下

**不使用骨架**

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696534706-ebf6b723-6f40-403d-8c42-de1c0c6746cb.png#averageHue=%23fdfcfb&clientId=uaf724859-4e16-4&from=paste&height=252&id=u5f607705&originHeight=277&originWidth=425&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=44232&status=done&style=none&taskId=u674d8bae-654b-411b-881e-95a632fccba&title=&width=386.36362798943026)

image.png

> 具体的步骤包含: 1.创建Maven项目 2.选择不使用Web项目骨架 3.输入Maven项目坐标创建项目 4.在pom.xml设置打包方式为war 5.补齐Maven Web项目缺失webapp的目录结构 6.补齐Maven Web项目缺失WEB-INF/web.xml的目录结构

1. 创建Maven项目

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696566840-f55ef5db-11d7-4360-a89b-b5ff40168bf9.png#averageHue=%23e6e0d5&clientId=uaf724859-4e16-4&from=paste&height=699&id=u9922c834&originHeight=769&originWidth=1382&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=348538&status=done&style=none&taskId=uc241e239-0952-41f7-af38-aa622e8e5d6&title=&width=1256.3636091326885)

image.png

1. 选择不使用Web项目骨架

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696580489-ce9d2128-d5a4-408c-a30c-72c3f7674152.png#averageHue=%23e3dcce&clientId=uaf724859-4e16-4&from=paste&height=873&id=u30e0367c&originHeight=960&originWidth=798&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=526523&status=done&style=none&taskId=u3cbf3f37-56f6-468f-9e08-da2fee5e637&title=&width=725.454529730742)

image.png

1. 输入Maven项目坐标创建项目

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696596418-4ca92582-272a-4824-a477-ec59dbe5ffbd.png#averageHue=%23dfd5c8&clientId=uaf724859-4e16-4&from=paste&height=825&id=u83189227&originHeight=908&originWidth=1086&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=138279&status=done&style=none&taskId=u9390e9c3-208e-4570-af94-46556c730be&title=&width=987.2727058741676)

image.png

1. 在pom.xml设置打包方式为war,默认是不写代表打包方式为jar

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696611847-104be345-8bde-4eeb-9ca6-a72c2b6c0edd.png#averageHue=%23f3f1e3&clientId=uaf724859-4e16-4&from=paste&height=535&id=u80f50d31&originHeight=588&originWidth=933&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=245167&status=done&style=none&taskId=ud911086a-4178-4e70-b68f-9752b8e7cb0&title=&width=848.1817997979728)

image.png

1. 补齐Maven Web项目缺失webapp的目录结构

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696631736-a722cac1-fed3-4d17-8a6f-c9790be3e3be.png#averageHue=%23e7dcce&clientId=uaf724859-4e16-4&from=paste&height=835&id=u043fdf76&originHeight=919&originWidth=1184&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=177985&status=done&style=none&taskId=u00874346-c080-4eb1-a238-16f1a09e184&title=&width=1076.3636130340833)

image.png

1. 补齐Maven Web项目缺失WEB-INF/web.xml的目录结构

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696650624-f78a3995-4445-477d-b510-99f85d8aa019.png#averageHue=%23dfd3c0&clientId=uaf724859-4e16-4&from=paste&height=708&id=u2a56d74e&originHeight=779&originWidth=1062&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=153536&status=done&style=none&taskId=u2c195846-1c81-452b-a75f-c515641dfce&title=&width=965.4545245288822)

image.png

1. 补充完后，最终的项目结构如下:

上述两种方式，创建的web项目，都不是很全，需要手动补充内容，至于最终采用哪种方式来创建Maven Web项目，都是可以的，根据各自的喜好来选择使用即可。 小结 1.掌握Maven Web项目的目录结构 2.掌握使用骨架的方式创建Maven Web项目

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696663256-5008cc47-fc36-478c-8307-bf955a0c5231.png#averageHue=%23faf9f8&clientId=uaf724859-4e16-4&from=paste&height=241&id=ud3d16d27&originHeight=265&originWidth=485&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=38281&status=done&style=none&taskId=u8933738f-9c25-49e1-8a2e-db0ff670b1a&title=&width=440.9090813526439)

image.png

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696686705-d159a28e-c7de-4cf5-a951-7870fe4f5058.png#averageHue=%23faf9f9&clientId=uaf724859-4e16-4&from=paste&height=457&id=u383387be&originHeight=503&originWidth=1426&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=298223&status=done&style=none&taskId=u90d130ee-2bfc-4a6b-9fcd-e88570d1c41&title=&width=1296.363608265712)

image.png

3.掌握不使用骨架的方式创建Maven Web项目

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696704993-6f6c852c-76cc-4060-956b-0f19ab5da86c.png#averageHue=%23f7f7f6&clientId=uaf724859-4e16-4&from=paste&height=412&id=u91a5f9cf&originHeight=453&originWidth=1430&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=304438&status=done&style=none&taskId=u115a037b-de3e-4753-9b87-dd635326ac9&title=&width=1299.9999718232596)

image.png

## 3.4 IDEA使用Tomcat

- Maven Web项目创建成功后，通过Maven的package命令可以将项目打包成war包，将war文件拷贝到Tomcat的webapps目录下，启动Tomcat就可以将项目部署成功，然后通过浏览器进行访问即可。
- 然而我们在开发的过程中，项目中的内容会经常发生变化，如果按照上面这种方式来部署测试，是非常不方便的
- 如何在IDEA中能快速使用Tomcat呢?

在IDEA中集成使用Tomcat有两种方式，分别是**集成本地Tomcat**和**Tomcat Maven**插件

### 3.4.1 集成本地Tomcat

目标: 将刚才本地安装好的Tomcat8集成到IDEA中，完成项目部署，具体的实现步骤

1. 打开添加本地Tomcat的面板

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696747100-834b6d7f-e0ed-41bc-81bd-43cb46017e32.png#averageHue=%23f3f3f3&clientId=uaf724859-4e16-4&from=paste&height=621&id=u350b37b5&originHeight=683&originWidth=1119&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=135327&status=done&style=none&taskId=u49cdb2a1-dad4-45ba-a3ba-d3d5a9ecda8&title=&width=1017.2727052239352)

image.png

1. 指定本地Tomcat的具体路径

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696759221-1fe14291-eaad-4334-b3b5-8093b7ab4e80.png#averageHue=%23f5f4f4&clientId=uaf724859-4e16-4&from=paste&height=792&id=ude9a1d23&originHeight=871&originWidth=1353&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=326094&status=done&style=none&taskId=u527b8b24-cb4c-4567-bebd-6149acae1cc&title=&width=1229.9999733404686)

image.png

1. 修改Tomcat的名称，此步骤可以不改，只是让名字看起来更有意义，HTTP port中的端口也可以进行修改，比如把8080改成80

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696774492-530ffb93-2038-47b0-907d-df55d253d16c.png#averageHue=%23f5f5f4&clientId=uaf724859-4e16-4&from=paste&height=657&id=u4862f9c8&originHeight=723&originWidth=1084&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=165208&status=done&style=none&taskId=uf67ce509-3b52-4497-9c35-08a0a2efa73&title=&width=985.4545240953938)

image.png

1. 将开发项目部署项目到Tomcat中

扩展内容： `xxx.war`和 `xxx.war exploded`这两种部署项目模式的区别?

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696803273-141dee5b-c068-47fc-8772-7ab70e420b3a.png#averageHue=%23f5f4f4&clientId=uaf724859-4e16-4&from=paste&height=708&id=ue4c146a1&originHeight=779&originWidth=878&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=147804&status=done&style=none&taskId=ub3357079-4a65-4e18-a21d-b3a4ce33a9e&title=&width=798.1818008816936)

image.png

- war模式是将WEB工程打成war包，把war包发布到Tomcat服务器上
- war exploded模式是将WEB工程以当前文件夹的位置关系发布到Tomcat服务器上
- war模式部署成功后，Tomcat的webapps目录下会有部署的项目内容
- war exploded模式部署成功后，Tomcat的webapps目录下没有，而使用的是项目的target目录下的内容进行部署
- 建议大家都选war模式进行部署，更符合项目部署的实际情况

1. 部署成功后，就可以启动项目，为了能更好的看到启动的效果，可以在webapp目录下添加a.html页面

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696867712-f9b0c5df-e0c8-4ec7-a6b2-9119b5733e2d.png#averageHue=%23f9f9f8&clientId=uaf724859-4e16-4&from=paste&height=555&id=ud0077ba0&originHeight=610&originWidth=1374&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=201791&status=done&style=none&taskId=u77030f6a-2b20-4bf8-b3ee-63681a4a153&title=&width=1249.0908820175932)

image.png

1. 启动成功后，可以通过浏览器进行访问测试

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696882980-f3c65e5f-cad7-4057-8fed-30106bd69d10.png#averageHue=%23e8e1d2&clientId=uaf724859-4e16-4&from=paste&height=255&id=u4f593ae2&originHeight=280&originWidth=1306&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=38632&status=done&style=none&taskId=u6f14a21a-5103-438f-91f3-20b8f74fd7d&title=&width=1187.2727015392845)

image.png

1. 最终的注意事项

至此，IDEA中集成本地Tomcat进行项目部署的内容我们就介绍完了，整体步骤如下，大家需要按照流程进行部署操作练习。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696905387-3d6c2191-d223-4c6b-9033-43c6e54574be.png#averageHue=%23f3f3f2&clientId=uaf724859-4e16-4&from=paste&height=832&id=u892e943c&originHeight=915&originWidth=978&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=213659&status=done&style=none&taskId=u18a87209-72c4-4a9e-bfe4-b43e2b341ef&title=&width=889.090889820383)

image.png

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696935989-8e67d457-8df3-4f43-b18e-03c6768f6425.png#averageHue=%23f7f7f6&clientId=uaf724859-4e16-4&from=paste&height=515&id=u49fdcdaf&originHeight=567&originWidth=1411&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=293966&status=done&style=none&taskId=uf3d6def8-a433-4f60-b704-ff3598bfcfc&title=&width=1282.7272449249085)

image.png

### 3.4.2 Tomcat Maven插件

在IDEA中使用本地Tomcat进行项目部署，相对来说步骤比较繁琐，所以我们需要一种更简便的方式来替换它，那就是直接使用Maven中的Tomcat插件来部署项目，具体的实现步骤，只需要两步，分别是:

1. 在pom.xml中添加Tomcat插件

```xml
<build>
    <plugins>
        <!--Tomcat插件 -->        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.2</version>
        </plugin>
    </plugins>
</build>
```

1. 使用Maven Helper插件快速启动项目，选中项目，右键–>Run Maven –> tomcat7:run

**浏览器输入：**`**localhost:8080/项目名/页面**`**注意:**

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678696993249-112b63ad-3324-4a17-b73a-bcbdc36adc1f.png#averageHue=%23d8d0c6&clientId=uaf724859-4e16-4&from=paste&height=295&id=u9e855996&originHeight=325&originWidth=1290&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=248580&status=done&style=none&taskId=u17213d37-2f28-4765-b2ef-cddb624ac49&title=&width=1172.727247309094)

image.png

- 如果选中项目并右键点击后，看不到Run Maven和Debug Maven，这个时候就需要在IDEA中下载Maven Helper插件，具体的操作方式为: File –> Settings –> Plugins –> Maven Helper —> Install,安装完后按照提示重启IDEA，就可以看到了。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1678697028692-4d8ee625-dcef-4b11-b886-ac3335f2f798.png#averageHue=%23d2bc88&clientId=uaf724859-4e16-4&from=paste&height=571&id=ub99f30d2&originHeight=628&originWidth=904&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=51782&status=done&style=none&taskId=uccba71b6-19e9-49a6-a117-c0e9746c757&title=&width=821.8181640057528)

image.png

- Maven Tomcat插件目前只有Tomcat7版本，没有更高的版本可以使用
- 使用Maven Tomcat插件，要想修改Tomcat的端口和访问路径，可以直接修改pom.xml

```xml
<build>
    <plugins>
        <!--Tomcat插件 -->        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.2</version>
            <configuration>
                <port>80</port><!--访问端口号 -->                <!--项目访问路径                                        未配置访问路径: <http://localhost:80/tomcat-demo2/a.html>                                        配置/后访问路径: <http://localhost:80/a.html>                                        如果配置成 /hello,访问路径会变成什么?                                        答案: <http://localhost:80/hello/a.html>                                -->                <path>/</path>
            </configuration>
        </plugin>
    </plugins>
</build>
```

小结 通过这一节的学习，大家要掌握在IDEA中使用Tomcat的两种方式，集成本地Tomcat和使用Maven的Tomcat插件。后者更简单，推荐大家使用，但是如果对于Tomcat的版本有比较高的要求，要在Tomcat7以上，这个时候就只能用前者了。