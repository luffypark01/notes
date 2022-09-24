# **Linux常用命令**

### 创建普通用户

命令如下

```linux
useradd 用户名
```

然后可以用 cat	/etc/passwd   来查看已创建的用户(位于最底下)

设置用户密码：passwd 用户名

如果直接用passwd则是修改当前用户的密码（root密码也可修改）

### 其它常用命令

mkidr -p 递归创建文件、文件夹

su  切换用户     su - user完全切换用户，会加载对应用户的环境变量

touch  创建文件

cat  查看文本内容

ll  相当于ls -l

ps -ef   查看进程

top   linux任务管理器

find   查找文件

grep   过滤字符串信息

mv  移动，重命名文件

yum  下载软件

head   从文本前*行开始看，默认前10行

tail   从文本后10行看，  tail -f 文件名   实时监听文件内容

more 翻页显示文件内容

less  翻页显示文件内容

echo 相当于print打印

alias   linux的别名命令

chattr + a 文件名    加锁

chattr -a 文件名     减锁

lsattr 文件名   显示文件是否上锁



### 查看用户

id user  查看用户ID号

uid 用户id,  gid用户组id,groups 组id号码

root创建的用户，id默认是从1000开始的

sudo使普通用户提权执行命令，但是需要vim /etc/sudoers  添加对应的用户：username	ALL=(ALL)      ALL

在普通用户的情况下，sudo	你想执行的命令



### 删除用户

userdel	用户名

userdel -r	username 同时删除用户和用户的目录



### 用户权限

ll  查看文件详情信息

-rw-r--r-- 1 root root 0 Feb 16 16:00 new.txt

-：代表文件

d：代表文件夹

l：代表快捷方式

后面的是创建时间和文件名

chmod u-r,u-w,u-x	new.txt  减少权限（r读，w写，x可执行）

chmod u+r,u+w,u+x	new.txt  加上权限（r读，w写，x可执行）

u属主，g用户组，o其他人

权限分为

r	4

w	2

x	1

权限最高计算是7,最低是0

chmod 777 new,txt	赋予属主，组内成员，其他人所有权限

chown	root	new.txt	chown命令修改属主，

chgrp	修改文件属组

但是对于root用户，所有权限都有



### 软连接语法

创建快捷方式

ln 	-s	目标文件	快捷方式绝对路径

### linux打包、压缩文件

tar命令

-c	打包

-x	解压缩

-z	调用gzip命令去压缩文件，节省磁盘空间

-v	显示打包过程

-f	指定压缩后文件的名字（或要解压的文件名）

ll -h可显示文件的大小(以M为单位)

**du -h也可以显示文件大小**

#### 打包文件(节省空间)

tar -zcvf	压缩文件名字.tar.gz	./*

#### 解压缩文件

tar	-zxvf	压缩文件名字.tar.gz



### Linux命令提示符

PS1变量，用$符号取变量的值

可以直接赋值来改变命令提示符



### netstat命令

netstat命令用来打印Linux网络系统的状态信息，可让你得知整个Linux系统的网络情况

常用命令:netstat	-tunlp



### 查看磁盘空间

**df 	-h	命令，显示磁盘信息**



### dns域名解析系统

dns就是域名解析到ip的一个过程

解析命令：nslookup	www.baidu.com

vim	/etc/resolv.conf可以看到dns服务器

##### linux解析域名的命令：

nslookup	域名

/etc/hosts	本地强制dns的文件

### linux定时任务

分	时	日	月	周

\*	 \*	   \*	 \*	  \*	命令的绝对路径

分隔用逗号，范围用句号，频率用/

eg:3,15	*	*	*	*	每小时3分钟和15分钟时执行的命令

crontab	-l 可以显示计划任务的列表

crontab	-e	可以编辑计划任务



### linux软件包

linux的软件包格式为rpm

用systemctl	start	要启动的程序

![](E:\学习笔记\Linux\img\01.png)



![](E:\学习笔记\Linux\img\02.png)

![](E:\学习笔记\Linux\img\03.png)

### Python虚拟环境

#### 步骤1

1.python的虚拟环境，用于解决python环境依赖冲突的问题，仅仅是多个解释器的分身，多个解释器的复制，和换

4 2.python虚拟环境的工具有很多，有virtualenv， pipenv，pyenv

5 3.virtualenv可以在系统中建立多个不同并且相互不干扰的虚拟环境。

7 virtualenv的学习安装使用

10 注意：确保python3成功的安装，并且PATH配置在第一条路径

13 1.下载安装

14 pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple virtualenv

16 2.安装完毕，就可以使用命令，创建虚拟环境去使用了

17 virtualenv --no-site-packages  --python=python3 venv #得到独立第三方包的环境，并且指定解释器是pyt

这个命令在哪敲就会在哪里生成venv文件夹

18 #参数解释

19 ——no—site—packages #这个参数用于构建，干净的环境，没有任何的第三方包(virtualenv版本20以上的默认)

20——python=python3 #指定虚拟环境的本体，是python的哪一个版本

21 venv就是一个虚拟环境的文件夹，是虚拟python解释器

####  步骤2：这2个虚拟环境，都得单独的激活去使用

44 source venvl/bin/activate #激活虚拟环境

45 pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple django==2.0.1 #下载django模块

46 deactivate #退出虚拟环境

49 source venv2/bin/activate

50 pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple django==1.11.20 #T载django

51 deactivate #退出虚拟环境

### 保证python环境的一直性

pip freeze > new.txt  可以到处python项目所需要的第三方库

然后用pip3	install	-r	new.txt来装	-r参数指定一个文件，-i参数指定一个源

#### virtualenvwrapper的学习使用 

1．安装

pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple virtualenvwrapper 101

2．配置环境变量，每次开机都加载virtualenvwrapper这个工具，注意配置的是个人环境变量配置文件103

vim ~/.bash_profile

＃打开文件 

＃写入如下环境变量 export也是一个读取指令，让变量生效的

export WORKON_HOME=~/Envs	＃设置virtualenv的统一管理目录 

export VIRTUALENVWRAPPER_VIRTUALENV_ARGS='--no-site-packages'	＃添加virtualenvwrapper的参数，生成干净隔绝的环境 

export VIRTUALENVWRAPPER_PYTHON=/opt/python3/bin/python3	＃指定python解释器 

source	opt/python3/bin/virtualenvwrapper.sh 	＃执行virtualenvwrapper安装脚本

读取文件，使得生效，此时已经可以使用virtalenvwrapper

**相关操作**：

mkvirtualenv	虚拟环境的名字

workon 	进入虚拟环境

rmvirtualenv	删除虚拟环境

lsvirtualenv	列出所有虚拟环境

cdvirtualenv	进入虚拟环境的目录

cdsitepackages	进入虚拟环境的第三方包

deactivate #退出虚拟环境

# nginx

#### 源代码编译安装

选择这个，支持自定义的第三方功能扩展，比如自定义安装路径，支持https，支持gzip资源压缩

8．注意点，删除之前yum安装的nginx

yum remove nginx -y ＃卸载yum安装的nginx 

选择源码编译安装 

1．下载淘宝nginx的源代码包

wget http://tengine.taobao.org/download/tengine-2.3.0.tar.gz 

2．解压缩源码包 

tar -zxvf tengine-2.3.0.tar.gz 

3．进入源码目录开始编译安装

编译安装三部曲 

./configure --prefix=/opt/tngx/230/ 

make && make install 

4．配置淘宝nginx的环境变量，写入／etc／profile 

如下内容 

PATH="/opt/python362/bin/:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin:/opt/tngx230/sbin" 

读取／etc／profile 

source/etc/profile 

5．启动nginx 

直接输入nginx 指令，默认代表启动，不得再执行第二次

nginx	-s 	reload   ＃平滑重启nginx，重新读取nginx配置文件

nginx	-s 	stop ＃停止nginx进程 

nginx	-t	检测nginx.conf的语法

### nginx的目录配置文件信息

conf 存放nginx配置文件的

html 存放前端文件目录T

logs 存放nginx运行日志，错误日志的

sbin 存放nginx执行脚本的

### nginx配置文件学习

![](img\04.png)

![](img\05.png)

![](img\06.png)

[https://www.bilibili.com/video/BV1Uq4y1w7SA?p=24&amp;t=1092.2]: 

### nginx的状态信息功能，检测当前有多少个链接数

![](img\07.png)

在nginx.conf下打开一个参数即可

\#当你的请求来自于192.168.16.37/status，就进入如下的代码块

location /status {

​		\#开启nginx状态功能

​		stub status on;

}

使用linux的压测命令，给nginx发送大量的请求ab命令

安装方式

yum -y install httpd-tools

—n requests #执行的请求数，即一共发起多少请求。

—c concurrency #请求并发数

—k #启用HTTP KeepAlive功能，即在一个HTTP会话中执行多个请求。

url必须格式如下

ab -kc 1000 -n 100000 http://192.168.16.37/

### 多虚拟主机

（在一台服务器上，运行读个网站页面）

基于多域名的虚拟主机实现，其实就是多个server标签环境准备，1个1inux服务器，192.168.16.37

2个域名，我们是写入hosts文件，强制解析的假域名

www.s19dnf.com

www.s19riju.com

 安装好nginx软件，配置方式如下

1．服务器准备好了，nginx也安装好了2．在windows中写入hosts假的域名

找到如下文件，编辑写入域名对应关系C:\windows\System32\drivers\etc\hosts 

192.168.16.37	www.s19dnf.com 

192.168.16.37	www.s19riju.com 

### location

<img src="img\08.png" style="margin-left:0;" />

### 负载均衡

需要有至少三台服务器!

***对负责调度的服务进行如下操作：***

192.168.16.241 负载均衡的配置
nginx.conf修改为如下的配置

1	.添加负载均衡池，写入web服务器的地址 

upstream mydjango {				#mydjango这个名字是自定义的
		#负载均衡的方式，默认是轮训，1s一次
		#还有其他的负载均衡的规则

​		ip_hash;	  #写在这里(上面)

​		server 192.168.16.37	weight=1 ;		#这是两台资源服务器的ip地址
​		server 192.168.16.140 	weight=4;  	#weight:权重
}

2.再修改location

location / {

​		proxy_pass	http://mydjango

}

负载均衡的规则
调度算法					概述
轮询							按时间顺序逐一分配到不同的后端服务器（默认）
weight						加权轮询,weight值越大，分配到的访问几率越高
ip_hash					  每个请求按访问工P的hasli结果分配，这样来自同一工P的固定访问一个后端服务器	
url_hash					按照访问URL的hash结果来分配请求，是每个URL定向到同一个后端服务器
least conn				 最少链接数,那个机器链接数少就分发
复制代码
1 .轮询（不做配置，默认轮询）
2	.weight权重（优先级）
3.ip_hash配置，根据客户端ip哈希分配，不能和weight一起用





