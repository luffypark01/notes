# 9.0.1 HTTPS

## HTTPS安全概述

使用http网站时，可能会遭遇劫持和篡改，那么是很不安全的，如果用https协议，数据再传输时都是加密的，黑客无法窃取或篡改数据报文，也避免网站数据泄露。

## Openssl

Netscape网景公司创建的第一代浏览器，且为了提高浏览器访问网站的安全性，在TCP/IP层的传输层与应用层之间创建了一个SSL(Secure Sockets Layer)，安全套接字层。

SSL是一个库，为了让`应用层`讲数据传递给`传输层`之前，调用ssl层的功能，对数据进行`加密`。

但是SSL(v2 v3)是网景公司定义的，协议不够开放，因此TSL（传输层安全协议）出现了，流行的版本是(TSL v1== SSL v3)。

```gcode
在OSI七层模型中，应用层是HTTP协议，在应用层之下就是表示层，也就是TLS协议发挥作用的一层。
TLS协议通过(握手、交换秘钥、告警、加密)等方式做到数据的安全加密。
```

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/_book/ssl.png)

那么在数据`加密`与`解密`的过程中，如何确定，信任对方身份呢？这时候必须有一个`权威的机构`来验证双方的身份。

这个权威机构就是CA机构。

```stata
CA中心又称CA机构，即证书授权中心(Certificate Authority )，或称证书授权机构，作为电子商务交易中受信任的第三方，承担公钥体系中公钥的合法性检验的责任。
```

那么CA机构如何办法证书呢？流程如下

1.我们首先要`申请证书`，然后进行`登记`（我是谁，我是什么组织，我想干啥），发给`登记机构`后，通过`CSR`发给`CA`，当CA中心通过后，CA中心生成一对`公钥`和`私钥`，`公钥会在CA`证书链中保存，公钥和私钥证书发给订阅人后，将其部署在WEB服务器（nginx）上。

2.当浏览器访问https网站时，它会请求我们的证书

3.Nginx这样的web服务器会发送公钥证书给浏览器

4.浏览器进行证书验证，是否合法有效

5.CA机构会将过期的证书放在CRL服务器。由于CRL服务验证效率低下，CA还推出了OCSP在线验证证书是否过期。

6.Nginx有一个OCSP的开关，开启后，Nginx主动上OCSP查询证书是否有效。

```fortran
    证书吊销列表（Certificate Revocation List，CRL）是在网络中使用公钥结构存取服务器的两种常用方法中的一个，另外一个，比较新的，在将来要代替CRL的方法，是在线证书状态查询协议（Online Certificate Status Protocol，OCSP）。　　
    CRL就像它的名字描述的：一个用户数字验证状态的列表。此列表列出废除的验证以及废除的原因，证书的发放日期和发放的实体，同样包含在列表中。另外，梅尔过列表包含一个下次发布列表的可能时间，当一个用户常数进入一台服务器，服务器将使用CRL列表来决定是否允许或拒绝其进入。　　
    CRL的缺点就是必须不停的下载和更新列表，以保持列表是最新的，OCSP克服了这种限制，它会实时检测证书状态。
```

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/_book/ssl2.png)

## HTTPS证书

```stylus
Https不支持续费,证书到期需重新申请新并进行替换.
Https不支持三级域名解析, 如test.m.oldboy.com
Https显示绿色, 说明整个网站的url都是https的。
Https显示黄色, 因为网站代码中包含http的不安全连接。
Https显示红色, 要么证书是假的，要么证书过期。
```

## Nginx搭建自建HTTPS

1.服务器nginx环境准备，确保安装了opensll，且nginx支持ssl模块

创建存放证书的目录

```awk
提示：编译安装nginx时候，
yum install openssl openssl-devel //确保有ssl模块支持
./configure --prefix=/opt/nginx1-12/ --with-http_ssl_module   //编译时候开启nginx支持ssl模块
```

检查nginx模块状态

```awk
[root@yugo ~]# nginx  -V
Tengine version: Tengine/2.2.3 (nginx/1.8.1)
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-36) (GCC)
TLS SNI support enabled
configure arguments: --prefix=/opt/tengine223/
nginx: loaded modules:
...
nginx:     ngx_http_ssl_module (static)  //此服务器nginx 已有ssl模块
```

执行命令

```awk
//执行这里的命令
mkdir -p /opt/tengine223/ssl_key
cd /opt/tengine223/ssl_key/
```

2.使用openssl命令充当CA机构，颁发自建证书，学习使用，生成server.key

```axapta
[root@yugo ssl_key]# openssl genrsa -idea -out server.key 2048
Generating RSA private key, 2048 bit long modulus
................................+++
.........+++
e is 65537 (0x10001)
//我这里密码都是1234
Enter pass phrase for server.key:
Verifying - Enter pass phrase for server.key:
```

3.生成自签证书server.crt，同时去掉私钥的密码

```delphi
[root@yugo ssl_key]# openssl req -days 36500 -x509 -sha256 -nodes -newkey rsa:2048 -keyout server.key -out server.crt
Generating a 2048 bit RSA private key
...........................+++
......................................................................+++
writing new private key to 'server.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:CN            //国家名称（2个字母代码
State or Province Name (full name) []:BJ        //省/市/自治区名称（全名）
Locality Name (eg, city) [Default City]:BJ        //地点名称（例如，城市）[默认城市]
Organization Name (eg, company) [Default Company Ltd]:EDU    //组织名称（如公司）【默认公司】
Organizational Unit Name (eg, section) []:PY    //组织单位名称（例如，部门）[]
Common Name (eg, your name or your server's hostname) []:YU    //公用名（您的名称或服务器主机名）
Email Address []:yc_uuu@163.com                    //电子邮件地址
//参数解释
# req  -->用于创建新的证书
# new  -->表示创建的是新证书
# x509 -->表示定义证书的格式为标准格式
# key  -->表示调用的私钥文件信息
# out  -->表示输出证书文件信息
# days -->表示证书的有效期
```

4.配置nginx支持https

```nginx
    server {
        listen       443 ssl;
        server_name  pythonav.cn;
        ssl on;
        #证书文件
        ssl_certificate      /opt/tengine223/ssl_key/server.crt;
        #私钥文件
        ssl_certificate_key  /opt/tengine223/ssl_key/server.key;
        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;
        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;
        location / {
            root    /opt/_book;
            index  index.html index.htm;
        }
    }
}
```

5.此时重启nginx，加载https服务

```ebnf
nginx -s reload
```

6.浏览器访问https://pythonav.cn/由于这个证书我们自己签发的，浏览器肯定不认同，因此是红色告警。

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/_book/txssl/3.png)

## 使用腾讯云搭建https网站

腾讯云SSL是一个新的证书颁发机构（CA），它提供了一种获取和安装免费TLS /SSL证书的简便方法，从而在Web服务器上启用加密的HTTPS。您可以在腾讯云Web页面轻松获取免费的SSL证书，无论您选择哪种Web服务器软件。

1.获取ssl证书，要先注册腾讯云服务器

新用户请单击腾讯云官网顶部的【注册】按钮，进入注册页面。

https://console.qcloud.com/

2.注册完成后可以进入ssl证书控制台申请证书
https://console.qcloud.com/ssl

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/_book/txssl/1.png)

第二步，填写申请域名 www.pythonav.cn

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/_book/txssl/2.1.png)

第三步，自动dns解析，确保你的服务器域名已经备案

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/_book/txssl/2.png)

第四步，证书确认通过后，会提供证书下载地址

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/_book/txssl/4.png)

第五步，下载腾讯颁发的证书，会得到一个压缩文件www.pythonav.cn.zip

```stata
[root@yugo txssl]# ls
Apache  IIS  Nginx  Tomcat  www.pythonav.cn.csr  www.pythonav.cn.zip
```

第六步，解压缩证书，我们用的是Nginx的配置

第七步，配置nginx，结合腾讯云颁发的证书，这个是受信任的

```nginx
    server {
        listen       443 ssl;
        server_name  pythonav.cn;
        ssl on;
        #证书文件
        ssl_certificate      /opt/tengine223/txssl/Nginx/1_www.pythonav.cn_bundle.crt;
        #私钥文件
        ssl_certificate_key  /opt/tengine223/txssl/Nginx/2_www.pythonav.cn.key;
        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;
        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;
        location / {
            root    /opt/_book;
            index  index.html index.htm;
        }
    }
```

第八步，确保服务的防火墙打开443端口，重启nginx

```ebnf
nginx -s reload
```

第九步，访问服务器

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/_book/txssl/5.png)