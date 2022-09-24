## Web服务基础知识

```applescript
我们每天都会用web客户端上网，浏览器就是一个web客户端，例如谷歌浏览器，以及火狐浏览器等。
当我们输入www.oldboyedu.com/时候，很快就能看到老男孩教育的官网了，这一切看起来很平淡无奇，背后又是什么道理呢？
普通人可以不知道，但是咱们作为it开发人员，必须得掌握清楚背后的技术。
```

下面超哥为你揭晓用户访问网站的基本流程

1. 老男孩教育某python总监，讲了一天课感觉很累，下了班躺床上打开他的macbook pro，双击浏览器，输入www.pythonav.com网址后，系统首先会查找本地的DNS缓存以及hosts文件信息，确定是否存在www.pythonav.com域名对应的ip解析记录，如果有就直接获取ip进行访问服务器，第一次请求时，dns缓存是没有解析记录的，hosts文件多数是开发临时测试用
2. 如果本地dns缓存和hosts文件都没有域名解析记录，系统就会把某python总监访问的网址解析请求发送给客户端设置的DNS服务器去解析，也叫做Local DNS，如果LDNS服务器的本地缓存有对应的解析记录就会直接返回给客户端IP地址，如果没有LDNS就会继续请求其他的DNS服务器
3. LDNS继续从DNS系统的”.”(根)开始请求www.pythonav.com域名的解析，并且根据每个层级的DNS服务器系统进行系列的查找，最终在DNS网络上找到www.pythonav.com域名对应的授权DNS服务器。这个授权DNS服务器就是企业（个人）购买域名时用于管理域名解析的服务器，服务器上有对应的域名（IP）解析。
4. 此时授权的DNS服务器就会把www.pythonav.com对应的IP解析记录，例如（1.1.1.1）发送给LDNS
5. 此时LDNS会把解析记录发给浏览器，并且缓存域名和IP的解析记录，便于下一次更快的返回请求
6. 浏览器获得ip，请求对应的服务器，网站服务器接收到客户端的请求开始响应处理，将内容返回给浏览器，然后某python总监拿出了一盒清风牌抽纸。。。。
   ![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/_book/2.png)

图解dns

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/_book/3.png)

1.DNS域名解析，获取域名对应服务器IP地址

```avrasm
windows查看dns本地缓存
c:\>ipconfig /displaydns  显示缓存内容
c:\>ipconfig /flushdns      清除DNS缓存
c:\Windows\system32\drivers\etc\hosts    hosts解析文件
```

2.根据ip地址访问网站服务，TCP三次握手

3.用户向网站服务请求信息，HTTP请求报文

4.网站对用户请求进行相应，HTTP相应过程

5.断开网络链接，TCP四次挥手

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/_book/16.png)

## HTTP协议

```apache
Http协议，全称是HyperText Tansfer Protocol,中文叫超文本传输协议，是互联网最常见的协议。Http最重要的是www(World Wide Web)服务，也叫web服务器，中文叫“万维网”。
web服务端口默认是80，另外一个加密的www服务应用https默认端口是443，主要用于支付，网银相关业务
```

*1.什么是超文本?*

*包含有超链接(Link)和各种多媒体元素标记的文本。这些超文本文件彼此链接，形成网状(Web)，因此又被称为网页(Web Page)。这些链接使用URL表示。最常见的超文本格式是超文本标记语言HTML。*

> html文件->包含各种各样的元素（URL链接）->形成web page简称web页面。

版本

```apache
http协议诞生以来有若干个版本，主要是http/1.0 http/1.1
http/1.0规定浏览器和服务器只能保持短暂的连接，浏览器的每次请求都需要和服务器建立一个TCP连接，服务器完成请求后即断开TCP连接，服务器不跟踪每个链接，也不记录请求
http/1.1是对HTTP的缺陷进行重点修复，从可扩展性，缓存，带宽优化，持久连接，host头，错误通知等访问改进。
http/1.1支持长连接，增加了更多的请求头和响应头信息，例如配置请求头的Connection的值为keep-alive，表示请求结果返回后保持连接
```

Http请求方法

```undefined
在HTTP通信中，每个请求报文都包含一个方法，以告诉web服务器端需要执行哪些操作，这些动作被称为HTTP的请求方法
1    GET    请求指定的页面信息，并返回实体主体。
2    HEAD    类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头
3    POST    向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。
4    PUT    从客户端向服务器传送的数据取代指定的文档的内容。
5    DELETE    请求服务器删除指定的页面。
6    CONNECT    HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。
7    OPTIONS    允许客户端查看服务器的性能。
8    TRACE    回显服务器收到的请求，主要用于测试或诊断。
```

HTTP状态码

```markdown
HTTp状态码表示web服务器响应http请求状态的数字代码
常见状态码以及作用是
1**    信息，服务器收到请求，需要请求者继续执行操作
2**    成功，操作被成功接收并处理
3**    重定向，需要进一步的操作以完成请求
4**    客户端错误，请求包含语法错误或无法完成请求
5**    服务器错误，服务器在处理请求的过程中发生了错误
```

HTTP状态码的命令查看

```yaml
curl -I www.oldboyedu.com
Server: OES
Date: Sun, 12 Aug 2018 04:18:24 GMT
Content-Type: text/html
Content-Length: 152
Connection: keep-alive
Location: https://www.oldboyedu.com/
```

# HTTP报文

## 什么是HTTP请求报文

```undefined
HTTP请求由请求行，请求头部，空行，请求报文主体几个部分组成
```

**HTTP报文：**它是HTTP应用程序之间发送的数据块。这些数据块以一些文本形式的元信息开头，这些信息描述了报文的内容及含义，后面跟着可选的数据部分。这些报文都是在客户端、服务器和代理之间流动。

```oxygene
请求报文的格式：
起始行： <method> <request-URL> <version>
头部：   <headers>
主体：   <entity-body>
```

GET请求报文示例：

```apache
GET /books/?sex=man&name=Professional HTTP/1.1
 Host: www.example.com　　主机名
 User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)Gecko/20050225 Firefox/1.0.1　　客户端类型
 Accept-Encoding:gzip,deflate　　　　支持压缩
 Accept-Language:zh-cn　　支持语言类型
 Connection: Keep-Alive　　长链接
```

请求行

```qml
请求报文第一行，表示客户端想要什么
由请求方法 url    协议版本 　　组成
```

请求头部

```ada
请求头由    
关键字    :    值    组成
通过客户端把请求相关信息告诉服务器
```

空行

```undefined
请求头信息之后是一个空行，发送回车和换行符，通知web服务器以下没有请求头信息了
```

请求报文主体

```apache
 POST / HTTP/1.1
 Host: www.example.com
 User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
 Gecko/20050225 Firefox/1.0.1
 Content-Type: application/x-www-form-urlencoded
 Content-Length: 40
 Connection: Keep-Alive
 sex=man&name=Professional
请求体中包含了要发送给web服务器的数据信息，请问报文主体不用于get方法，而是用于post方法。
post方法适用于客户端填写表单的场合。
```

## HTTP响应报文

HTTP 响应与 HTTP 请求相似，HTTP响应也由3个部分构成，分别是：

- 状态行
- 响应头(Response Header)
- 响应正文

状态行由协议版本、数字形式的状态代码、及相应的状态描述，各元素之间以空格分隔。

```undefined
状态行，用来说明服务器响应客户端的状况，一般分为协议版本号，数字状态码，状态情况
响应头部
常见响应头信息
Connection: Keep-Alive
Content-Encoding: gzip
Content-Type: text/html;charset=utf-8
Date: Mon, 13 Aug 2018 06:06:54 GMT
Expires: Mon, 13 Aug 2018 06:06:54 GMT
空白行
通知客户端空行以下没有头部信息了
响应报文主体
主要包含了返回给客户端的数据，可以是文本，二进制（图片，视频）
```

HTTP响应例子

```makefile
HTTP/1.1 200 OK
Server:Apache Tomcat/5.0.12
Date:Mon,6Oct2003 13:23:42 GMT
Content-Length:112
<html>...
```

# URl介绍

url中文叫“统一资源标识符”，是一个用于标识某一互联网资源名称的字符串，在世界范围内标识定位某一个唯一信息资源。

那什么是URL，URL简称统一资源定位符。那URL的组成部分是由协议, 域名:端口, 路径和文件名

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/_book/4.jpg)

例如

```awk
#访问一张郭达的照片
http://www.pythonav.cn/man.jpg
```

url主要用在各种www客户端和服务器程序上，url可以用一种统一的格式来描述各种信息资源，包括文件，服务器地址和目录等

### url组成

1. 协议
2. 主机ip或域名
3. 主机资源具体地址

第一部分用”://“隔开，第二部分用”/“符号隔开

## 静态网页资源

在网页设计中，纯HTMl格式的网页（包含图片，视频，JS，CSS等样式）通常被称作“静态网页”。

静态网页是相对于动态网页而言的，是指没有后台数据库，不包含程序，不可交互的网页。

```undefined
静态网页的特点
开发人员写了什么，显示就是什么，一旦编写完成，就不会有任何改变。静态网页一般适用于更新较少的展示型网页，例如（酒水，家具，水果等宣传页），是很多中小网站的展示方式。
```

静态网页资源对应文件扩展名为

- 纯文本文件，如.htm .html .xml .js .css
- 图片或数据文档，如 .jpg .gif .bmp .txt .ppt
- 视频类文件 .mp4 .avi .flv 等

静态网页重要特性

- 每个页面有一个固定的url地址，url地址不含有问号”?”或”&”等符号
- 网页一经发布到服务器，网页内容是保存在服务器文件系统上的，每个网页都是独立的一个文件
- 网页内容固定不变，容易被搜索引擎收录（优点）
- 网页没有数据库支撑，在网站制作和维护上工作量很大（缺点）
- 网页的交互性很差，缺少程序的功能实现（缺点）
- 客户端解析网址时，由于不需要读取数据库，因此服务器端可以接受更高的并发访问。请求到来时，直接从磁盘上返回数据。（优点）

举例（吃火锅，现成的蔬菜）

### 有关高并发架构思想

```undefined
在高并发，高访问量的场景下做架构优化时，比较关键的就是把动态网页转化成静态网页，而不是直接请求数据库和动态服务器，并且可以吧静态内容推到缓存中，这样就提升用户体验，节约服务器压力成本。
```

## 动态网页资源

```jboss-cli
动态网页是和静态网页相对而言的，动态网页的url后缀一般是.asp  .aspx  .php .js .cgi 
并且动态网页都有标志性的符号"? &"，后端都有数据库的支持。
```

动态网页地址

```awk
添加新随笔
https://i.cnblogs.com/EditPosts.aspx?opt=1
```

动态网页资源特点

1. 网页以数据库技术为支撑，大大降低网站维护的工作量
2. 动态网页技术的网站可以实现更多的功能，如用户注册，用户登录，投票，用户管理，博客管理等
3. 动态网页不是独立存在服务器上的网页文件，用户请求动态程序时，服务器解析程序并且可能读取数据库返回一个完整的网页内容
4. 搜索引擎（爬虫）一般不会抓取网址中的“？”后面的内容，因此企业都会做伪静态技术页面

举例（饭店炒菜，现做）

## 网站流量术语

**网站统计一般以数值较大的IP,PV统计，比较好看**

#### IP

```armasm
IP即Internet Protocol，这里是指独立ip数，不同的ip地址的计算机访问网站时被计算的总次数。
独立ip数是网站流量的一个重要指标。
一般相同ip地址的客户端访问网站页面一天内只会被计算一次。
这里的ip指的是是固定的公网ip。
比如咱们整个班级的学生，都是通过路由器NAT出口的，公网捕捉到的IP地址都来自于同一个。
```

#### PV

```excel
pv（Page View）即是页面浏览量，不管客户端是不是相同，也不管ip是否相同，用户只要访问网站页面就会被计算PV，一次计算一个PV。
pv的度量方法就是客户端从浏览器发出一个web请求（request），服务器接收请求返回一个页面给客户端，这样就产生一个pv。
页面刷新一下，就是一个PV。
```

举例

```excel
例如武佩奇去访问百合网想找一个知心朋友，你觉得他能产生多少PV？
答案可能是十几个到几十个。一个用户的访问PV量和网站的业务成正比的，武佩奇可能点击18岁左右的女性，28岁左右的女性，也可能点击18岁的小伙子。。。。因此他访问的页面会很多，自然pv也会增多
```

#### UV

```armasm
UV即unique visitor，同一个客户端（pc或移动端）访问网站被计算为一个访客。
一天内相同的客户端访问同一个网站只计一次uv，uv是以cookie等技术为统计依据，实际统计存在误差。
一台计算机可能有多人使用，因此uv也不是最准确的。
```

#### 并发数

```undefined
并发数指系统同时能处理的请求数量，也反应了系统的负载能力
```

#### 响应时间

```undefined
响应时间是指执行一个请求从开始到最后收到响应数据所花费的总体时间。
```

QPS

```sql
Query Per Second
每秒查询数
服务器在一秒内处理了多少个请求，显然数字越大代表服务器的负载越高，处理能力越强。
```

*Http相关术语pv、ip、uv 假设公司有一座大厦，大厦有100人，每个人有一台电脑和一部手机，上网都是通过nat转换出口，每个人点击网站2次（发2次请求）, 请问对应的pv,uv,ip分别是多少*

> PV：页面浏览量, 400 100人 2个设备 访问2次 =400数
>
> uv：独立的客户, 200 100人2个设备=200数
>
> ip：独立IP, 1个 同一个NAT出口，独立IP为1