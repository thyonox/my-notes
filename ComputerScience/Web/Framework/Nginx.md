# 

|标签|Java,中间件|
|---|---|
|创建者|Maktub|
|创建时间|2024/05/17 23:46|
|状态||

# 1. Nginx简介

## 1.1 Nginx概述

Nginx是一款高性能的Web服务器和反向代理服务器。 它可以在Linux、Unix、Windows等操作系统上运行，并可以作为HTTP、HTTPS、SMTP、POP3、IMAP等多种协议的服务器或代理服务器使用。 Nginx的优点包括高并发、低内存消耗、良好的稳定性和可靠性等。 虽然 Nginx 和 Tomcat 都可以作为 Web 服务器，但是它们的功能、性能和使用场景有所不同：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679954452169-b4ad8195-f1c8-462d-bdfa-eb84bf995277.png#averageHue=%23fcfdf7&clientId=u93245514-f929-4&from=paste&height=121&id=u07b3a458&originHeight=498&originWidth=1200&originalType=url&ratio=1.100000023841858&rotation=0&showTitle=false&size=237678&status=done&style=none&taskId=u251dac68-31be-4cec-a4cc-f399d1b5e36&title=&width=292.3067932128906)

image.png

1. 功能不同：Nginx 是一个高性能的 Web 服务器和反向代理服务器，它主要用于处理**静态内容**和反向代理请求。Tomcat 是一个 Java Servlet 容器，用于运行 Java 应用程序和 Web 应用程序。
2. 性能不同：Nginx 具有出色的性能和高并发处理能力，可以快速处理大量的并发请求，因此在处理静态内容和反向代理请求时非常有效。Tomcat 的性能也很好，但通常用于处理**动态内容**和 Java Servlet 等应用程序。
3. 部署方式不同：Nginx 可以作为独立的 Web 服务器部署，也可以与其他 Web 服务器一起使用。Tomcat 则通常以 WAR 包或 EAR 包的形式部署在应用服务器中。
4. 支持语言不同：Nginx 主要用于处理 HTTP 请求，支持多种编程语言的 Web 应用程序。Tomcat 则主要用于运行 Java 应用程序，支持 Java Servlet和 Java Server Pages（JSP）等技术。

## 1.2 下载

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679955791800-dfbaf422-acbc-4b85-80e3-3e6a5ece436d.png#averageHue=%23fdfdfd&clientId=u93245514-f929-4&from=paste&height=224&id=u8d246af4&originHeight=304&originWidth=961&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=6652&status=done&style=none&taskId=u786a58b3-820d-4b57-8c30-733a606a641&title=&width=707.6363525390625)

image.png

## 1.3 安装

1. 安装依赖包由于nginx是基于c语言开发的，所以需要安装c语言的编译环境，及正则表达式库等第三方依赖库。

```
yum -y install gcc pcre-devel zlib-devel openssl openssl-devel
```

1. 解压 nginx 压缩包```shell tar -zxvf nginx-1.22.1.tar.gz

```
3. 配置 nginx 编译环境```shell
cd nginx-1.22.1
./configure --prefix=/usr/local/nginx
```

在Linux中，configure命令通常用于为软件包的编译和安装过程进行配置。configure命令是GNU自动化工具中的一个脚本，其主要作用是自动检测系统环境和依赖项，生成Makefile文件，并设置编译选项和安装路径。 --prefix：指定安装路径。 --enable-xxx：启用某些功能，如–enable-shared表示编译共享库。 --disable-xxx：禁用某些功能。 --with-xxx：指定依赖库的路径。 4. 编译、安装```shell make & make install

```
## 1.4 目录结构
![image.png](<https://cdn.nlark.com/yuque/0/2023/png/29046015/1679984071444-8e1f3940-e6c9-4dfa-a531-99a4f0c8049f.png#averageHue=%23011f28&clientId=u572ca807-a640-4&from=paste&height=375&id=u9d9da72b&originHeight=412&originWidth=789&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=8217&status=done&style=none&taskId=u873c568d-6b65-4c1b-81cf-c7bb9543dd7&title=&width=717.2727117262599>)
重点目录和文件如下：

| **目录/文件** | **说明** | **备注** |
| --- | --- | --- |
| conf | 配置文件的存放目录 |
 |
| conf/nginx.conf | Nginx的核心配置文件 | conf下有很多nginx的配置文件，我们主要操作这个核心配置文件 |
| html | 存放静态资源(html, css, ) | 部署到Nginx的静态资源都可以放在html目录中 |
| logs | 存放nginx日志(访问日志、错误日志等) |
 |
| sbin/nginx | 二进制文件，用于启动、停止Nginx服务 |
 |

# 2. Nginx命令
> Nginx中，所有二进制可执行文件都存放在sbin目录下，我们可以在sbin目录下通过nginx命令，搭配不同参数达到不同功能

## 2.1 常用命令
1. 查看版本```shell
./nginx -v
```

1. 检查配置文件```shell ./nginx -t

```
修改了nginx.conf核心配置文件之后，在启动Nginx服务之前，可以先检查一下conf/nginx.conf文件配置的是否有错误。
3. 启动```shell
./nginx
```

启动之后，我们可以通过 `ps -ef|grep nginx` 指令来查看nginx的进程是否存在。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679985291877-839681f3-3988-4e78-8ddb-d49c334c5990.png#averageHue=%2307242d&clientId=ud25be90e-5862-4&from=paste&height=64&id=u749af763&originHeight=70&originWidth=790&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=3759&status=done&style=none&taskId=u7e75ad43-7f27-48a7-b562-cd34f6189c3&title=&width=718.1818026156468)

image.png

浏览器访问 Nginx 的80端口，进入 Nginx 默认页。 关闭防火墙：

```
systemctl stop firewalld
```

开发80端口：

```
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --reload
```

1. 停止```shell ./nginx -s stop

```
5. 重新加载```shell
./nginx -s reload
```

当修改了Nginx配置文件后，需要重新加载才能生效。

## 2.2 环境变量配置

通过vim编辑器，打开 `/etc/profile` 文件, 在 PATH 环境变量中增加 nginx 的 sbin 目录：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679989539772-541039af-ce95-41d6-95f7-ae69d236da32.png#averageHue=%2304222b&clientId=ud25be90e-5862-4&from=paste&height=51&id=uc09cd31c&originHeight=56&originWidth=796&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=2274&status=done&style=none&taskId=uc1fa4051-0984-4372-99fc-556509528ea&title=&width=723.6363479519682)

image.png

修改完配置文件之后，需要执行 `source /etc/profile` 使文件生效：

# 3. 配置文件结构

nginx 的配置文件 (conf/nginx.conf) 整体上分为三部分：全局块、events 块、http 块。

|​**区域**|​**职责**|
|---|---|
|全局块|配置和nginx运行相关的全局配置|
|events块|配置和网络连接相关的配置|
|http块|配置代理、缓存、日志记录、虚拟主机等配置|

```
#全局块
#user  nobody;
worker_processes  1;

#events块
events {
    worker_connections  1024;
}

#http块
http {
    #http全局块
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    #server块
    server {
        #server全局块
        listen       8000;
        server_name  localhost;
        #location块
        location / {
            root   html;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
    #这边可以有多个server块
    server {
      ...
    }
}

```

在 http 块中可以包含多个 server 块，每个 server 块可以配置多个 location 块。

> 相关阅读： Nginx配置文件详解 - 程序员自由之路 - 博客园

# 4. 静态资源部署

Nginx 的主要作用之一就是部署静态资源（html、css、js、图片、视频等）。 将静态资源部署到Nginx非常简单，只需要将文件复制到Nginx安装目录下的html目录中即可。

```
server {
    listen 80;              #监听端口
    server_name localhost;  #服务器名称
    location / {            #匹配客户端请求url
        root html;          #指定静态资源根目录
        index index.html;   #指定默认首页
    }
}
```

# 5. 反向代理

正向代理服务器是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679998880708-2fd82f96-144f-4b02-9e2a-636d10c2cf87.png#averageHue=%23eeebe3&clientId=u6218006d-8165-4&from=paste&height=248&id=ude8aafd5&originHeight=273&originWidth=790&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=114071&status=done&style=none&taskId=ufc402b73-2091-490f-9ddf-710b35cbc9f&title=&width=718.1818026156468)

image.png

这也是翻墙的原理。 反向代理服务器位于用户与目标服务器之间，但是对于用户而言，反向代理服务器就相当于目标服务器，即用户直接访问反向代理服务器就可以获得目标服务器的资源，反向代理服务器负责将请求转发给目标服务器。用户不需要知道目标服务器的地址，也无须在用户端作任何设定，对于用户来说，访问反向代理服务器是完全无感知的。

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679998963929-927cc4b7-41e9-431b-be67-1ddd7bacc409.png#averageHue=%23fefefe&clientId=u6218006d-8165-4&from=paste&height=324&id=u8374f31b&originHeight=356&originWidth=957&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=29982&status=done&style=none&taskId=u14449406-c30b-409d-8b2e-57b412d9713&title=&width=869.9999811432582)

image.png

其实正向代理和反向代理有点近似于API和SPI的关系。

- 正向代理：代理的出站请求， 客户端能感知到代理程序，架构上距离客户端更近。
- 反向代理：代理的是入站请求，客户端认为代理程序就是服务器，客户端感知不到代理逻辑，架构上距离服务端更近。

在nginx中，我们可以在nginx.conf中配置反向代理：

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679999069865-b54396c1-1243-41e1-ac69-be44a1f3535c.png#averageHue=%23fdfdfd&clientId=u6218006d-8165-4&from=paste&height=354&id=udbaaa49e&originHeight=389&originWidth=875&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=126174&status=done&style=none&taskId=u1070685c-b136-4460-8e54-bde9708cdb4&title=&width=795.4545282135329)

image.png

```
server {
    listen 82;
    server_name localhost;
    location / {
        proxy_pass <http://192.168.200.201:8080>;     #反向代理配置，将请求转发到指定服务
    }
}
```

上述配置的含义为: 当我们访问nginx的82端口时，根据反向代理配置，会将请求转发到 `http://192.168.200.201:8080` 对应的服务上。

# 6. 负载均衡

早期的网站流量和业务功能都比较简单，单台服务器就可以满足基本需求，但是随着互联网的发展，业务流量越来越大并且业务逻辑也越来越复杂，单台服务器的性能及单点故障问题就凸显出来了，因此需要多台服务器组成应用集群，进行性能的水平扩展以及避免单点故障出现。 应用集群：将同一应用部署到多台机器上，组成应用集群，接收负载均衡器分发的请求，进行业务处理并返回响应数据 负载均衡器：将用户请求根据对应的负载均衡算法分发到应用集群中的一台服务器进行处理

![](https://cdn.nlark.com/yuque/0/2023/png/29046015/1679999255091-03c02051-a6b6-4f54-9b31-5c15b10b4048.png#averageHue=%23f0f5e6&clientId=u6218006d-8165-4&from=paste&height=235&id=u7c072184&originHeight=259&originWidth=840&originalType=binary&ratio=1.100000023841858&rotation=0&showTitle=false&size=45014&status=done&style=none&taskId=uc554e08c-dd86-4d08-a65a-718ce8f2790&title=&width=763.6363470849916)

image.png

Nginx的负载均衡是基于反向代理的 在Nginx 的配置文件中增加负载均衡配置：

```
#upstream指令可以定义一组服务器
upstream targetserver{
    server 192.168.200.201:8080;
    server 192.168.200.201:8081;
}

server {
    listen       8080;
    server_name  localhost;
    location / {
        proxy_pass <http://targetserver>;
    }
}
```

以上使用 Nginx 默认的的负载均衡策略，在 Nginx 中还提供了其他的负载均衡策略：

|​**名称**|​**说明**|特点|
|---|---|---|
|轮询|默认方式||
|weight|权重方式|根据权重分发请求,权重大的分配到请求的概率大|
|ip_hash|依据ip分配方式|根据客户端请求的IP地址计算hash值， 根据hash值来分发请求, 同一个IP发起的请求, 会发转发到同一个服务器上|
|least_conn|依据最少连接方式|哪个服务器当前处理的连接少, 请求优先转发到这台服务器|
|url_hash|依据url分配方式|根据客户端请求url的hash值，来分发请求, 同一个url请求, 会发转发到同一个服务器上|
|fair|依据响应时间方式|优先把请求分发给处理请求时间短的服务器|

比如以权重方式配置：

```
#upstream指令可以定义一组服务器
upstream targetserver{
    server 192.168.200.201:8080 weight=10;
    server 192.168.200.201:8081 weight=5;
}
```