想必我们大多数人都是通过访问网站而开始接触互联网的吧。我们平时访问的网站服务 就是 Web 网络服务，一般是指允许用户通过浏览器访问到互联网中各种资源的服务。

Web 网络服务是一种被动访问的服务程序，即只有接收到互联网中其他主机发出的 请求后才会响应，最终用于提供服务程序的 Web 服务器会通过 HTTP(超文本传输协议)或 HTTPS(安全超文本传输协议)把请求的内容传送给用户。

目前能够提供 Web 网络服务的程序有 IIS、Nginx 和 Apache 等。其中，IIS(Internet Information Services，互联网信息服务)是 Windows 系统中默认的 Web 服务程序

2004 年 10 月 4 日，为俄罗斯知名门户站点而开发的 Web 服务程序 Nginx 横空出世。 Nginx 程序作为一款轻量级的网站服务软件，因其稳定性和丰富的功能而快速占领服务器市 场，但 Nginx 最被认可的还当是系统资源消耗低且并发能力强，因此得到了国内诸如新浪、 网易、腾讯等门户站的青睐。

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/p1.png)

```bash
web服务器（nginx）：接收HTTP请求（例如www.pythonav.cn/xiaocang.jpg）并返回数据
web框架（django，flask）：开发web应用程序，处理接收到的数据
```

## nginx介绍

```bash
nginx是一个开源的，支持高性能，高并发的www服务和代理服务软件。它是一个俄罗斯人lgor sysoev开发的，作者将源代码开源出来供全球使用。
nginx比它大哥apache性能改进许多，nginx占用的系统资源更少，支持更高的并发连接，有更高的访问效率。
nginx不但是一个优秀的web服务软件，还可以作为反向代理，负载均衡，以及缓存服务使用。
安装更为简单，方便，灵活。
支持高并发，能支持几万并发连接
资源消耗少，在3万并发连接下开启10个nginx线程消耗的内存不到200M
可以做http反向代理和负载均衡
支持异步网络i/o事件模型epoll
```

## Tengine

Tengine是由淘宝网发起的Web服务器项目。它在Nginx的基础上，针对大访问量网站的需求，添加了很多高级功能和特性。Tengine的性能和稳定性已经在大型的网站如淘宝网，天猫商城等得到了很好的检验。它的最终目标是打造一个高效、稳定、安全、易用的Web平台。

## 安装配置nginx

#### 安装nginx前的依赖环境解决

```nsis
#执行第一条语句即可
yum install gcc patch libffi-devel python-devel  zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel openssl openssl-devel -y
#依赖简单介绍
一. gcc 安装
安装 nginx 需要先将官网下载的源码进行编译，编译依赖 gcc 环境，如果没有 gcc 环境，则需要安装：
yum install gcc-c++
二. PCRE pcre-devel 安装
PCRE(Perl Compatible Regular Expressions) 是一个Perl库，包括 perl 兼容的正则表达式库。nginx 的 http 模块使用 pcre 来解析正则表达式，所以需要在 linux 上安装 pcre 库，pcre-devel 是使用 pcre 开发的一个二次开发库。nginx也需要此库。命令：
yum install -y pcre pcre-devel
三. zlib 安装
zlib 库提供了很多种压缩和解压缩的方式， nginx 使用 zlib 对 http 包的内容进行 gzip ，所以需要在 Centos 上安装 zlib 库。
yum install -y zlib zlib-devel
四. OpenSSL 安装
OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及 SSL 协议，并提供丰富的应用程序供测试或其它目的使用。
nginx 不仅支持 http 协议，还支持 https（即在ssl协议上传输http），所以需要在 Centos 安装 OpenSSL 库。
```

#### 编译安装nginx

```awk
1.下载源码包
wget -c https://nginx.org/download/nginx-1.12.0.tar.gz
2.解压缩源码
tar -zxvf nginx-1.12.0.tar.gz
3.配置，编译安装  开启nginx状态监测功能
./configure --prefix=/opt/nginx112/ 
make && make install 
4.启动nginx，进入sbin目录,找到nginx启动命令
cd sbin
./nginx #启动
./nginx -s stop #关闭
./nginx -s reload #重新加载
5.修改PATH
PATH=$PATH:/opt/nginx112/
```

## nginx软件目录

```autoit
[root@oldboy_python /opt/nginx1-12 11:44:02]#ls
client_body_temp  conf  fastcgi_temp  html  logs  proxy_temp  sbin  scgi_temp  static  uwsgi_temp
```

- conf 存放nginx所有配置文件的目录,主要nginx.conf
- html 存放nginx默认站点的目录，如index.html、error.html等
- logs 存放nginx默认日志的目录，如error.log access.log
- sbin 存放nginx主命令的目录,sbin/nginx

Nginx主配置文件`/etc/nginx/nginx.conf`是一个纯文本类型的文件，整个配置文件是以区块的形式组织的。一般，每个区块以一对大括号`{}`来表示开始与结束。

```applescript
CoreModule核心模块
user www;                       #Nginx进程所使用的用户
worker_processes 1;             #Nginx运行的work进程数量(建议与CPU数量一致或auto)
error_log /log/nginx/error.log  #Nginx错误日志存放路径
pid /var/run/nginx.pid          #Nginx服务运行后产生的pid进程号
events事件模块
events {            
    worker_connections  //每个worker进程支持的最大连接数
    use epool;          //事件驱动模型, epoll默认
}
http内核模块
//公共的配置定义在http{}
http {  //http层开始
...    
    //使用Server配置网站, 每个Server{}代表一个网站(简称虚拟主机)
    'server' {
        listen       80;        //监听端口, 默认80
        server_name  localhost; //提供服务的域名或主机名
        access_log host.access.log  //访问日志
        //控制网站访问路径
        'location' / {
            root   /usr/share/nginx/html;   //存放网站代码路径
            index  index.html index.htm;    //服务器返回的默认页面文件
        }
        //指定错误代码, 统一定义错误页面, 错误代码重定向到新的Locaiton
        error_page   500 502 503 504  /50x.html;
    }
    ...
    //第二个虚拟主机配置
    'server' {
    ...
    }
    include /etc/nginx/conf.d/*.conf;  //包含/etc/nginx/conf.d/目录下所有以.conf结尾的文件
}   //http层结束
```

### 部署nginx站点

nginx默认站点是Nginx目录下的html文件夹，这里可以从nginx.conf中查到

```glsl
 location /{
            root   html;  #这里是默认的站点html文件夹，也就是              /opt/nginx1-12/html/文件夹下的内容
            index  index.html index.htm; #站点首页文件名是index.html
        }
```

如果要部署网站业务数据，只需要把开发好的程序全放到html目录下即可

```stylus
[root@oldboy_python /tmp 11:34:52]#ls /opt/nginx1-12/html/
index.html  jssts.jpeg  lhy.mp4  man.jpg  wget-log
```

因此只需要通过域名/资源，即可访问

```awk
http://www.pyyuc.cn/man.jpg
```

## Nginx多个虚拟主机

如果每台linux服务器只运行了一个小网站，那么人气低，流量小的草根站长需要承担高额的服务器租赁费，也造成了硬件资源浪费。

虚拟主机就是将一台服务器分割成多个“虚拟服务器”，每个站点使用各自的硬盘空间，由于省资源，省钱，众多网站都使用虚拟主机来部署网站。

```axapta
虚拟主机的概念就是在web服务里的一个独立的网站站点，这个站点对应独立的域名（IP），具有独立的程序和资源目录，可以独立的对外提供服务。
这个独立的站点配置是在nginx.conf中使用server{}代码块标签来表示一个虚拟主机。
Nginx支持多个server{}标签，即支持多个虚拟主机站点。
```

虚拟主机类型

```armasm
基于域名的虚拟主机
通过不同的域名区分不同的虚拟主机，是企业应用最广的虚拟主机。
基于端口的虚拟主机
通过不同的端口来区分不同的虚拟主机，一般用作企业内部网站，不对外直接提供服务的后台，例如www.pythonav.cn:9000
基于IP的虚拟主机
通过不同的IP区分不同的虚拟主机，此类比较少见，一般业务需要多IP的常见都会在负载均衡中绑定VIP
```

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/p2.png)

nginx可以自动识别用户请求的域名，根据不同的域名请求服务器传输不同的内容，只需要保证服务器上有一个可用的ip地址，配置好dns解析服务。

/etc/hosts是linux系统中本地dns解析的配置文件，同样可以达到域名访问效果

修改nginx.conf

```nginx
egrep -v '#|^$' /opt/nginx1-12/conf/nginx.conf
#配置文件内容如下
worker_processes  1;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  logs/access.log  main;
    sendfile        on;
    keepalive_timeout  65;
    #虚拟主机1
    server {
        listen       80;
        server_name  www.pyyuc.cn;
        location /{
            root   html/pyyuc;
            index  index.html index.htm;
        }
}
    #虚拟主机2
    server {
        listen       80;
        server_name  www.pythonav.cn;
        location /{
            root   html/pythonav;
            index  index.html index.htm;
        }
}
    }
[root@oldboy_python /opt/nginx1-12 14:52:12]#curl www.pythonav.cn
<meta charset=utf8>我是pythonav，未成年禁止入内
[root@oldboy_python /opt/nginx1-12 14:52:40]#curl www.pyyuc.cn
<meta charset=utf8>我是pyyuc站点
```

## Nginx访问日志功能

日志功能对每个用户访问网站的日志信息记录到指定的日志文件里，开发运维人员可以分析用户的浏览器行为，此功能由ngx_http_log_module模块负责，官网地址是：

http://nginx.org/en/docs/http/ngx_http_log_module.html

控制日志的参数

```asciidoc
log_format    记录日志的格式，可定义多种格式
accsss_log    指定日志文件的路径以及格式
```

log_format main ‘$remote_addr - $remote_user [$time_local] “$request” ‘

‘$status $body_bytes_sent “$http_referer” ‘

‘“$http_user_agent” “$http_x_forwarded_for”‘;

对应参数解析

```gams
$remote_addr    记录客户端ip
$remote_user    远程用户，没有就是 “-”
$time_local 　　 对应[14/Aug/2018:18:46:52 +0800]
$request　　　 　对应请求信息"GET /favicon.ico HTTP/1.1"
$status　　　  　状态码
$body_bytes_sent　　571字节 请求体的大小
$http_referer　　对应“-”　　由于是直接输入浏览器就是 -
$http_user_agent　　客户端身份信息
$http_x_forwarded_for　　记录客户端的来源真实ip 97.64.34.118
```

日志效果如下

```accesslog
66.102.6.6 - - [14/Aug/2018:18:46:52 +0800] "GET /favicon.ico HTTP/1.1" 404 571 "-"
 "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/49.0.2623.75 Safari/537.36 Google Favicon" "97.64.34.118"
```

nginx.conf默认配置

```nginx
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  logs/access.log  main;
```

日志格式配置定义

```css
log_format是日志关键字参数，不能变
main是日志格式指定的标签，记录日志时通过main标签选择指定的格式。
```

## Nginx404页面优化

在网站运行过程中，可能因为页面不存在等原因，导致网站无法正常响应请求，此时web服务会返回系统的错误码，但是默认的错误页面很不友好。

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/p3.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/p4.png)

因此我们可以将404，403等页面的错误信息重定向到网站首页或者其他指定的页面，提升用户访问体验。

```nginx
server {
        listen       80;
        server_name  www.pythonav.cn;
        root html/pythonav;
        location /{
            index  index.html index.htm;
        }
　　　　　　#在pythonav路径下的40x.html错误页面
        error_page 400 403 404 405 /40x.html;
        }
```

40x.html

```awk
<img style='width:100%;height:100%;' src=https://pic1.zhimg.com/80/v2-77a9281a2bebc7a2ea5e02577af266a8_hd.png>
```

此时访问www.pythonav.cn/asdasd错误页面已经优化了

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/p5.png)

## Nginx反向代理功能

nginx实现负载均衡的组件

```nginx
ngx_http_proxy_module    proxy代理模块，用于把请求抛给服务器节点或者upstream服务器池
```

机器准备，两台服务器

```crmsh
master  192.168.11.63　　主负载
slave   192.168.11.64　　web1
```

反向代理配置文件nginx.conf

```nginx
worker_processes  1;
error_log  logs/error.log;
pid        logs/nginx.pid;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  logs/access.log  main;
    sendfile        on;
    keepalive_timeout  65;
    server {
        listen       80;
        server_name  192.168.11.63;
        location / {
        proxy_pass  http://192.168.11.64;
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

此时访问master的服务器192.168.11.63:80地址，已经会将请求转发给slave的80端口

除了页面效果的展示以外，还可以通过log(access.log)查看代理效果

63端日志

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/p6.png)

64端日志

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/p7.png)

# 9.1 nginx负载均衡

## 集群概念

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/j1.png)

## 集群优点

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/j2.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/j3.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/j4.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/j5.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/j6.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/j7.png)

## 负载均衡介绍

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/f1.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/f2.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/f3.png)

# 9.2 nginx实现负载均衡

```undefined
Web服务器，直接面向用户，往往要承载大量并发请求，单台服务器难以负荷，我使用多台WEB服务器组成集群，前端使用Nginx负载均衡，将请求分散的打到我们的后端服务器集群中，
实现负载的分发。那么会大大提升系统的吞吐率、请求性能、高容灾
```

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/fz1.png)

Nginx要实现负载均衡需要用到proxy_pass代理模块配置

Nginx负载均衡与Nginx代理不同地方在于

Nginx代理仅代理一台服务器，而Nginx负载均衡则是将客户端请求代理转发至一组upstream虚拟服务池

Nginx可以配置代理多台服务器，当一台服务器宕机之后，仍能保持系统可用。

## upstream配置

在nginx.conf > http 区域中

```nginx
upstream django {
       server 10.0.0.10:8000;
       server 10.0.0.11:9000;
}
```

在nginx.conf > http 区域 > server区域 > location配置中

添加proxy_pass

```glsl
location / {
            root   html;
            index  index.html index.htm;
            proxy_pass http://django;
}
```

此时初步负载均衡已经完成，upstream默认按照轮训方式负载，每个请求按时间顺序逐一分配到后端节点。

## upstream分配策略

```arcade
调度算法   　　 概述
轮询    　　　　按时间顺序逐一分配到不同的后端服务器(默认)
weight  　　   加权轮询,weight值越大,分配到的访问几率越高
ip_hash   　　 每个请求按访问IP的hash结果分配,这样来自同一IP的固定访问一个后端服务器
url_hash   　  按照访问URL的hash结果来分配请求,是每个URL定向到同一个后端服务器
least_conn    最少链接数,那个机器链接数少就分发
```

1.轮询(不做配置，默认轮询)

2.weight权重(优先级)

3.ip_hash配置，根据客户端ip哈希分配，不能和weight一起用weight 权重

```nginx
upstream django {
       server 10.0.0.10:8000 weight=5;
       server 10.0.0.11:9000 weight=10;#这个节点访问比率是大于8000的
}
```

ip_hash

```roboconf
每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器
upstream django {
　　　　ip_hash;
       server 10.0.0.10:8000;
       server 10.0.0.11:9000;
}
```

**backup**

**在非backup机器繁忙或者宕机时，请求backup机器，因此机器默认压力最小**

```nginx
upstream django {
       server 10.0.0.10:8000 weight=5;
       server 10.0.0.11:9000;
       server node.oldboy.com:8080 backup;
}
```

## Nginx实验负载均衡

```dns
角色            ip                    主机名
lb01        192.168.119.10        lb01    
web01        192.168.119.11        web01
web02        192.168.119.12        web02
```

## 关闭防火墙

```routeros
iptables -F
sed  -i 's/enforcing/disabled/' /etc/selinux/config
systemctl stop firewalld
systemctl disable firewalld
```

## 一、web01服务器配置nginx，创建index.html

```crmsh
server {
        listen       80;
        server_name  192.168.119.11;
        location / {
        root /node;
            index  index.html index.htm;
        }
}
mkdir /node
echo 'i am web01' > /node/index.html
#启动NGINX
./sbgin/nginx
```

## 二、web01服务器配置nginx,创建index.html

```crmsh
server {
    listen       80;
    server_name  192.168.119.12;
    location / {
        root /node;
        index  index.html index.htm;
}
mkdir /node
echo 'i am web02...' > /node/index.html
#启动nginx
./sbing/nginx
```

## 三、配置lb01服务器的nginx负载均衡

1.检查lb01的 nginx.conf

```nginx
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    upstream node {
    　　server 192.168.119.11:80;
    　　server 192.168.119.12:80;
}
    server {
        listen       80;
        server_name 192.168.119.10;
        location / {
        　　proxy_pass http://node;
        　　include proxy_params;  #需要手动创建
        }
    }
}
```

2.手动创建proxy_params文件，文件中存放代理的请求头相关参数

```autoit
[root@lb01 conf]# cat /opt/nginx/conf/proxy_params
proxy_set_header Host $http_host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_connect_timeout 30;
proxy_send_timeout 60;
proxy_read_timeout 60;
proxy_buffering on;
proxy_buffer_size 32k;
proxy_buffers 4 128k;
启动lb01负载均衡nginx服务
./sbin/nginx
```

## 四、访问lb01节点nginx，反复刷新

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/fz2.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/fz3.png)

## 五、nginx动静分离

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/dj1.png)

## 环境准备

```stylus
系统                 服务                软件                ip地址
centos7(lb01)                负载均衡            nginx proxy        192.168.119.10
centos7(web01)                静态资源            nginx静态资源        192.168.119.11
centos7(web02)                动态资源            django            192.168.119.12
```

## 一、在web01机器上，配置静态资源，图片等

```awk
cat nginx.conf
server {
        listen       80;
        server_name  192.168.119.11;
        #定义网页根目录
         root /code;
        #定义了静态资源
        index index.html;
#域名匹配，所有的png、jpg、gif请求资源，都去/root/code/images底下找
         location ~* .*\.(png|jpg|gif)$ {
                root /code/images;
        }    
#重启nginx
./sbin/nginx
#创建目录
mkdir -p /code/images
#准备首页文件
[root@web01  /code]$cat index.html
static files...
#准备静态文件，图片
[root@web01  /code/images]$wget http://pythonav.cn/av/girlone.jpg^C
[root@web01  /code/images]$ls
girlone.jpg
```

## 二、在web02配置动态请求，准备一个flask程序和静态资源转发

```nginx
cat  nginx.conf
#静态资源地址
upstream static {
server 192.168.119.11:80;
}
#flask动态请求
upstream flask {
server 192.168.119.12:8080;
}
    server {
        listen       80;
        server_name  192.168.119.12;
　　　　　　#当请求到达192.168.119.12:80/时，转发给flask的8080应用
        location / {
            proxy_pass http://flask;
            include proxy_params;
        }
　　　　　　#当判断资源请求是 192.168.119.12/girl.jpg时候，转发请求给static地址池的服务器192.168.119.11/
        location ~ .*\.(png|jpg|gif)$ {
　　　　　　　　proxy_pass http://static;
include proxy_params;
}
```

准备flask应用,flask.py

```routeros
from flask import Flask
app=Flask(__name__)
@app.route('/')
def hello():
    return "i am flask....from nginx"
if __name__=="__main__":
    app.run(host='0.0.0.0',port=8080)
```

后台运行flask程序

```vim
python flask-web.py &
```

## 三、在负载均衡服务器lb01上测试访问192.168.119.10

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/dj2.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/dj3.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter9/pic/dj4.png)