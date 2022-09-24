### redis

```nginx
Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件
```

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/p1.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/p2.png)

## 安装redis

```markdown
安装方式：
1. yum安装
2.源码编译安装
```

大家用过yum，是相当省事好用吧，为什么还要学习源码安装？

有人说编译安装性能好？错

编译安装的优势是：

- 编译安装时可以指定扩展的module（模块），php、apache、nginx都是一样有很多第三方扩展模块，如mysql，编译安装时候，如果需要就定制存储引擎（innodb，还是MyIASM）
- 编译安装可以统一安装路径，linux软件约定安装目录在/opt/下面
- 软件仓库版本一般比较低，编译源码安装可以根据需求，安装最新的版本

## 编译安装redis步骤

```apache
1.下载redis源码
wget http://download.redis.io/releases/redis-4.0.10.tar.gz
2.解压缩
tar -zxf redis-4.0.10.tar.gz
3.切换redis源码目录
cd redis-4.0.10.tar.gz
4.编译源文件
make 
5.编译好后，src/目录下有编译好的redis指令
6.make install 安装到指定目录，默认在/usr/local/bin
```

redis可执行文件

```jboss-cli
./redis-benchmark //用于进行redis性能测试的工具
./redis-check-dump //用于修复出问题的dump.rdb文件
./redis-cli //redis的客户端
./redis-server //redis的服务端
./redis-check-aof //用于修复出问题的AOF文件
./redis-sentinel //用于集群管理
```

启动redis服务端

```axapta
启动redis非常简单，直接./redis-server就可以启动服务端了，还可以用下面的方法指定要加载的配置文件：
redis-server redis.conf
默认情况下，redis-server会以非daemon的方式来运行，且默认服务端口为6379。
```

使用redis客户端

```accesslog
#执行客户端命令即可进入
./redis-cli  
#测试是否连接上redis
127.0.0.1:6379 > ping
返回pong代表连接上了
//用set来设置key、value
127.0.0.1:6379 > set name "chaoge"
OK
//get获取name的值
127.0.0.1:6379 > get name
"chaoge"
```

# 8.1 redis发布订阅

redis提供了发布（publish）订阅（subbscribe）功能

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/p4.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/p5.png)

## 发布订阅的命令

发布订阅的命令

```haxe
PUBLISH channel msg
    将信息 message 发送到指定的频道 channel
SUBSCRIBE channel [channel ...]
    订阅频道，可以同时订阅多个频道
UNSUBSCRIBE [channel ...]
    取消订阅指定的频道, 如果不指定频道，则会取消订阅所有频道
PSUBSCRIBE pattern [pattern ...]
    订阅一个或多个符合给定模式的频道，每个模式以 * 作为匹配符，比如 it* 匹配所    有以 it 开头的频道( it.news 、 it.blog 、 it.tweets 等等)， news.* 匹配所有    以 news. 开头的频道( news.it 、 news.global.today 等等)，诸如此类
PUNSUBSCRIBE [pattern [pattern ...]]
    退订指定的规则, 如果没有参数则会退订所有规则
PUBSUB subcommand [argument [argument ...]]
    查看订阅与发布系统状态
注意：使用发布订阅模式实现的消息队列，当有客户端订阅channel后只能收到后续发布到该频道的消息，之前发送的不会缓存，必须Provider和Consumer同时在线。
```

### 发布订阅

**窗口1，启动两个redis-cli窗口，均订阅diantai 频道（channel）**

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/r1.png)

**窗口2，启动发布者向频道 diantai发送消息**

```accesslog
[root@web02 ~]# redis-cli
127.0.0.1:6379> PUBLISH diantai 'jinyewugenglaiwojia'
(integer) 2
```

**窗口3，查看订阅者的消息状态**

### 订阅一个或者多个符合模式的频道

**窗口1，启动两个redis-cli窗口，均订阅 wang\*频道（channel）**

```apache
127.0.0.1:6379> PSUBSCRIBE wang*
Reading messages... (press Ctrl-C to quit)
1) "psubscribe"
2) "wang*"
3) (integer) 1
1) "pmessage"
2) "wang*"
3) "wangbaoqiang"
4) "jintian zhennanshou "
```

窗口2，启动redis-cli窗口，均订阅wang*频道

```apache
127.0.0.1:6379> PSUBSCRIBE wang*
Reading messages... (press Ctrl-C to quit)
1) "psubscribe"
2) "wang*"
3) (integer) 1
1) "pmessage"
2) "wang*"
3) "wangbaoqiang"
4) "jintian zhennanshou "
```

窗口3，发布者消息

```accesslog
[root@web02 ~]# redis-cli
127.0.0.1:6379> PUBLISH wangbaoqiang "jintian zhennanshou "
(integer) 2
```

# 8.2 redis持久化rdb与aof

`Redis`是一种内存型数据库，一旦服务器进程退出，数据库的数据就会丢失，为了解决这个问题，`Redis`提供了两种持久化的方案，将内存中的数据保存到磁盘中，避免数据的丢失。

## RDB持久化

`redis`提供了`RDB持久化`的功能，这个功能可以将`redis`在内存中的的状态保存到硬盘中，它可以**手动执行。**

也可以再`redis.conf`中配置，**定期执行**。

RDB持久化产生的RDB文件是一个**经过压缩**的**二进制文件**，这个文件被保存在硬盘中，redis可以通过这个文件还原数据库当时的状态。

```fortran
RDB(持久化)
内存数据保存到磁盘
在指定的时间间隔内生成数据集的时间点快照（point-in-time snapshot）
优点：速度快，适合做备份，主从复制就是基于RDB持久化功能实现
rdb通过再redis中使用save命令触发 rdb
rdb配置参数：
dir /data/6379/
dbfilename  dbmp.rdb
每过900秒 有1个操作就进行持久化
save 900秒  1个修改类的操作
save 300秒  10个操作
save 60秒  10000个操作
save  900 1
save 300 10
save 60  10000
```

# redis持久化之RDB实践

1.启动redis服务端,准备配置文件

```apache
daemonize yes
port 6379
logfile /data/6379/redis.log
dir /data/6379              #定义持久化文件存储位置
dbfilename  dbmp.rdb        #rdb持久化文件
bind 10.0.0.10  127.0.0.1    #redis绑定地址
requirepass redhat            #redis登录密码
save 900 1                    #rdb机制 每900秒 有1个修改记录
save 300 10                    #每300秒        10个修改记录
save 60  10000                #每60秒内        10000修改记录
```

2.启动redis服务端

3.登录redis设置一个key

```avrasm
redis-cli -a redhat
```

4.此时检查目录，/data/6379底下没有dbmp.rdb文件

5.通过save触发持久化，将数据写入RDB文件

```accesslog
127.0.0.1:6379> set age 18
OK
127.0.0.1:6379> save
OK
```

# redis持久化之AOF

AOF（append-only log file）

记录服务器执行的所有变更操作命令（例如set del等），并在服务器启动时，通过重新执行这些命令来还原数据集

AOF 文件中的命令全部以redis协议的格式保存，新命令追加到文件末尾。

优点：最大程序保证数据不丢

缺点：日志记录非常大

```axapta
redis-client   写入数据  >  redis-server   同步命令   >  AOF文件
```

配置参数

```coffeescript
AOF持久化配置，两条参数
appendonly yes
appendfsync  always    总是修改类的操作
             everysec   每秒做一次持久化
             no     依赖于系统自带的缓存大小机制
```

1.准备aof配置文件 redis.conf

```apache
daemonize yes
port 6379
logfile /data/6379/redis.log
dir /data/6379
dbfilename  dbmp.rdb
requirepass redhat
save 900 1
save 300 10
save 60  10000
appendonly yes
appendfsync everysec
```

2.启动redis服务

```awk
redis-server /etc/redis.conf
```

3.检查redis数据目录/data/6379/是否产生了aof文件

```autoit
[root@web02 6379]# ls
appendonly.aof  dbmp.rdb  redis.log
```

4.登录redis-cli，写入数据，实时检查aof文件信息

```autoit
[root@web02 6379]# tail -f appendonly.aof
```

5.设置新key，检查aof信息，然后关闭redis，检查数据是否持久化

```stata
redis-cli -a redhat shutdown
redis-server /etc/redis.conf
redis-cli -a redhat
```

**redis 持久化方式有哪些？有什么区别？**

**rdb：基于快照的持久化，速度更快，一般用作备份，主从复制也是依赖于rdb持久化功能**

**aof：以追加的方式记录redis操作日志的文件。可以最大程度的保证redis数据安全，类似于mysql的binlog**

## RDB持久化切换AOF持久化

# 确保redis版本在2.2以上

```routeros
[root@pyyuc /data 22:23:30]#redis-server -v
Redis server v=4.0.10 sha=00000000:0 malloc=jemalloc-4.0.3 bits=64 build=64cb6afcf41664c
```

本文在redis4.0中，通过config set命令，达到不重启redis服务，从RDB持久化切换为AOF

# 实验环境准备

redis.conf服务端配置文件

```apache
daemonize yes
port 6379
logfile /data/6379/redis.log
dir /data/6379
dbfilename  dbmp.rdb
save 900 1                    #rdb机制 每900秒 有1个修改记录
save 300 10                    #每300秒        10个修改记录
save 60  10000                #每60秒内        10000修改记录
```

启动redis服务端

```axapta
redis-server redis.conf
```

登录redis-cli插入数据，手动持久化

```accesslog
127.0.0.1:6379> set name chaoge
OK
127.0.0.1:6379> set age 18
OK
127.0.0.1:6379> set addr shahe
OK
127.0.0.1:6379> save
OK
```

检查RDB文件

```autoit
[root@pyyuc /data 22:34:16]#ls 6379/
dbmp.rdb  redis.log
```

# 备份这个rdb文件，保证数据安全

```gradle
[root@pyyuc /data/6379 22:35:38]#cp dbmp.rdb /opt/
```

# 执行命令，开启AOF持久化

```accesslog
127.0.0.1:6379> CONFIG set appendonly yes   #开启AOF功能
OK
127.0.0.1:6379> CONFIG SET save ""  #关闭RDB功能
OK
```

# 确保数据库的key数量正确

```apache
127.0.0.1:6379> keys *
1) "addr"
2) "age"
3) "name"
```

# 确保插入新的key，AOF文件会记录

```accesslog
127.0.0.1:6379> set title golang
OK
```

**此时RDB已经正确切换AOF，注意还得修改redis.conf添加AOF设置，不然重启后，通过config set的配置将丢失**

# 8.3 redis主从同步

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/r2.png)

原理：

1. 从服务器向主服务器发送 SYNC 命令。
2. 接到 SYNC 命令的主服务器会调用BGSAVE 命令，创建一个 RDB 文件，并使用缓冲区记录接下来执行的所有写命令。
3. 当主服务器执行完 BGSAVE 命令时，它会向从服务器发送 RDB 文件，而从服务器则会接收并载入这个文件。
4. 主服务器将缓冲区储存的所有写命令发送给从服务器执行。
   -——————
   1、在开启主从复制的时候，使用的是RDB方式的，同步主从数据的
   2、同步开始之后，通过主库命令传播的方式，主动的复制方式实现
   3、2.8以后实现PSYNC的机制，实现断线重连

## redis主从同步配置

### 步骤一、准备一主两从的数据库实例

主节点：6380

从节点：6381、6382

6380.conf

```bash
1、环境：
准备两个或两个以上redis实例
mkdir /data/638{0..2}  #创建6380 6381 6382文件夹
配置文件示例：
vim   /data/6380/redis.conf
port 6380
daemonize yes
pidfile /data/6380/redis.pid
loglevel notice
logfile "/data/6380/redis.log"
dbfilename dump.rdb
dir /data/6380
protected-mode no
```

6381.conf

```gradle
vim   /data/6381/redis.conf
port 6381
daemonize yes
pidfile /data/6381/redis.pid
loglevel notice
logfile "/data/6381/redis.log"
dbfilename dump.rdb
dir /data/6381
protected-mode no
```

6382.conf

```gradle
port 6382
daemonize yes
pidfile /data/6382/redis.pid
loglevel notice
logfile "/data/6382/redis.log"
dbfilename dump.rdb
dir /data/6382
protected-mode no
```

启动三个redis实例

```awk
redis-server /data/6380/redis.conf
redis-server /data/6381/redis.conf
redis-server /data/6382/redis.conf
```

### 步骤二、配置主从同步

6381/6382命令行

redis-cli -p 6381
SLAVEOF 127.0.0.1 6380 #指明主的地址

redis-cli -p 6382
SLAVEOF 127.0.0.1 6380 #指明主的地址

检查主从状态

从库：

```accesslog
127.0.0.1:6382> info replication
127.0.0.1:6381> info replication
```

主库：

```accesslog
127.0.0.1:6380> info replication
```

## 测试写入数据，主库写入数据，检查从库数据

```accesslog
主
127.0.0.1:6380> set name chaoge
从
127.0.0.1:6381>get name
```

## 手动进行主从复制故障切换

```apache
#关闭主库6380
redis-cli -p 6380
shutdown
```

检查从库主从信息，此时master_link_status:down

```pgsql
redis-cli -p 6381
info replication
redis-cli -p 6382
info replication
```

**既然主库挂了，我想要在6381 6382之间选一个新的主库**

**1.关闭6381的从库身份**

```pgsql
redis-cli -p 6381
info replication
slaveof no one
```

**2.将6382设为6381的从库**

```accesslog
6382连接到6381：
[root@db03 ~]# redis-cli -p 6382
127.0.0.1:6382> SLAVEOF no one
127.0.0.1:6382> SLAVEOF 127.0.0.1 6381
```

3.检查6382，6381的主从信息

# 8.4 redis哨兵

## redis-Sentinel

```crmsh
Redis-Sentinel是redis官方推荐的高可用性解决方案，
当用redis作master-slave的高可用时，如果master本身宕机，redis本身或者客户端都没有实现主从切换的功能。
而redis-sentinel就是一个独立运行的进程，用于监控多个master-slave集群，
自动发现master宕机，进行自动切换slave > master。
```

sentinel主要功能如下：

- 不时的监控redis是否良好运行，如果节点不可达就会对节点进行下线标识
- 如果被标识的是主节点，sentinel就会和其他的sentinel节点“协商”，如果其他节点也人为主节点不可达，就会选举一个sentinel节点来完成自动故障转义
- 在master-slave进行切换后，master_redis.conf、slave_redis.conf和sentinel.conf的内容都会发生改变，即master_redis.conf中会多一行slaveof的配置，sentinel.conf的监控目标会随之调换

## Redis-sentinel工作机制

```crmsh
每个Sentinel以每秒钟一次的频率向它所知的Master，Slave以及其他 Sentinel 实例发送一个 PING 命令
如果一个实例（instance）距离最后一次有效回复 PING 命令的时间超过 down-after-milliseconds 选项所指定的值， 则这个实例会被 Sentinel 标记为主观下线。
如果一个Master被标记为主观下线，则正在监视这个Master的所有 Sentinel 要以每秒一次的频率确认Master的确进入了主观下线状态。
当有足够数量的 Sentinel（大于等于配置文件指定的值）在指定的时间范围内确认Master的确进入了主观下线状态， 则Master会被标记为客观下线
在一般情况下， 每个 Sentinel 会以每 10 秒一次的频率向它已知的所有Master，Slave发送 INFO 命令
当Master被 Sentinel 标记为客观下线时，Sentinel 向下线的 Master 的所有 Slave 发送 INFO 命令的频率会从 10 秒一次改为每秒一次
若没有足够数量的 Sentinel 同意 Master 已经下线， Master 的客观下线状态就会被移除。
若 Master 重新向 Sentinel 的 PING 命令返回有效回复， Master 的主观下线状态就会被移除。
主观下线和客观下线
主观下线：Subjectively Down，简称 SDOWN，指的是当前 Sentinel 实例对某个redis服务器做出的下线判断。
客观下线：Objectively Down， 简称 ODOWN，指的是多个 Sentinel 实例在对Master Server做出 SDOWN 判断，并且通过 SENTINEL is-master-down-by-addr 命令互相交流之后，得出的Master Server下线判断，然后开启failover.
SDOWN适合于Master和Slave，只要一个 Sentinel 发现Master进入了ODOWN， 这个 Sentinel 就可能会被其他 Sentinel 推选出， 并对下线的主服务器执行自动故障迁移操作。
ODOWN只适用于Master，对于Slave的 Redis 实例，Sentinel 在将它们判断为下线前不需要进行协商， 所以Slave的 Sentinel 永远不会达到ODOWN。
```

## redis主从复制背景问题

`Redis`主从复制可将主节点数据同步给从节点，从节点此时有两个作用：

- 一旦主节点宕机，从节点作为主节点的备份可以随时顶上来。
- 扩展主节点的读能力，分担主节点读压力。

但是问题是：

- 一旦主节点宕机，从节点上位，那么需要人为修改所有应用方的主节点地址（改为新的master地址），还需要命令所有从节点复制新的主节点

那么这个问题，redis-sentinel就可以解决了

## 主从复制架构图

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/r3.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/r4.png)

## redis sentinel架构图

redis的一个进程，但是不存储数据，只是监控redis

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/s1.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/s2.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/s3.png)

## 安装配置redis哨兵

**本实验是在测试环境下，考虑到学生机器较弱，因此只准备一台linux服务器用作环境！！**

服务器环境，一台即可完成操作

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/s4.png)

### 主节点master的redis-6379.conf

```bash
port 6379
daemonize yes
logfile "6379.log"
dbfilename "dump-6379.rdb"
dir "/var/redis/data/"
```

### 从节点slave的redis-6380.conf

```apache
port 6380
daemonize yes
logfile "6380.log"
dbfilename "dump-6380.rdb"
dir "/var/redis/data/" 
slaveof 127.0.0.1 6379      // 从属主节点
```

### 从节点slave的redis-6381.conf

```apache
port 6381
daemonize yes
logfile "6380.log"
dbfilename "dump-6380.rdb"
dir "/var/redis/data/" 
slaveof 127.0.0.1 6379      // 从属主节点
```

### 启动redis主节点

```awk
redis-server /etc/redis-6379.conf
```

### 测试redis主节点是否通信

```avrasm
redis-cli  ping
```

## 启动两slave节点

还记得上面超哥的截图吗？总体redis配置文件如下，6379为master，6380和6381为slave

```tap
-rw-r--r-- 1 root root 145 Nov  7 17:44 /etc/redis-6379.conf      #这个为主，port是6379
-rw-r--r-- 1 root root  93 Nov  7 17:42 /etc/redis-6380.conf　　　 # 这个是从，port6380，并且得加上新的参数slaveof
-rw-r--r-- 1 root root 115 Nov  7 17:42 /etc/redis-6381.conf      # 这个是从，port6381，并且得加上新的参数slaveof
redis-server /etc/redis-6380.conf
redis-server /etc/redis-6381.conf
```

### 验证从节点的redis服务

```autoit
[root@master  ~]$redis-cli   -p 6380 ping
PONG
[root@master  ~]$redis-cli   -p 6381 ping
PONG
```

## 检测主从关系

在主节点上查看主从通信关系

```pf
[root@master ~]# redis-cli  -p 6379 info replication
# Replication
role:master
connected_slaves:2
slave0:ip=192.168.119.10,port=6380,state=online,offset=407,lag=0
slave1:ip=192.168.119.10,port=6381,state=online,offset=407,lag=0
master_repl_offset:407
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:2
repl_backlog_histlen:406
```

在从节点上查看主从关系（6380、6379）

```avrasm
[root@slave 192.168.119.11 ~]$redis-cli  -p 6380 info replication
# Replication
role:slave
master_host:192.168.119.10
master_port:6379
master_link_status:up
master_last_io_seconds_ago:3
master_sync_in_progress:0
slave_repl_offset:505
slave_priority:100
slave_read_only:1
connected_slaves:0
master_repl_offset:0
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
```

**此时可以在master上写入数据，在slave上查看数据，此时主从复制配置完**

## 配置redis sentinel环境

实验的环境是单独一台linux

```tap
[root@master tmp]# ll /etc/redis-*
-rw-r--r-- 1 root root 145 Nov  7 17:44 /etc/redis-6379.conf
-rw-r--r-- 1 root root  93 Nov  7 17:42 /etc/redis-6380.conf
-rw-r--r-- 1 root root 115 Nov  7 17:42 /etc/redis-6381.conf
-rw-r--r-- 1 root root 556 Nov  7 17:42 /etc/redis-sentinel-26379.conf
-rw-r--r-- 1 root root 556 Nov  7 17:42 /etc/redis-sentinel-26380.conf
-rw-r--r-- 1 root root 556 Nov  7 17:42 /etc/redis-sentinel-26381.conf
```

redis-sentinel-26379.conf配置文件写入如下信息

```awk
// Sentinel节点的端口
port 26379  
dir /var/redis/data/
logfile "26379.log"
// 当前Sentinel节点监控 192.168.119.10:6379 这个主节点
// 2代表判断主节点失败至少需要2个Sentinel节点节点同意
// mymaster是主节点的别名
sentinel monitor mymaster 192.168.119.10 6379 2
//每个Sentinel节点都要定期PING命令来判断Redis数据节点和其余Sentinel节点是否可达，如果超过30000毫秒30s且没有回复，则判定不可达
sentinel down-after-milliseconds mymaster 30000
//当Sentinel节点集合对主节点故障判定达成一致时，Sentinel领导者节点会做故障转移操作，选出新的主节点，
原来的从节点会向新的主节点发起复制操作，限制每次向新的主节点发起复制操作的从节点个数为1
sentinel parallel-syncs mymaster 1
//故障转移超时时间为180000毫秒
sentinel failover-timeout mymaster 180000
redis-sentinel-26380.conf
redis-sentinel-26381.conf的配置仅仅差异是port(端口)的不同。
```

然后启动三个sentinel哨兵

```awk
redis-sentinel /etc/redis-sentinel-26379.conf
redis-sentinel /etc/redis-sentinel-26380.conf
redis-sentinel /etc/redis-sentinel-26381.conf
```

此时查看哨兵是否成功通信

```avrasm
[root@master ~]# redis-cli -p 26379  info sentinel
# Sentinel
sentinel_masters:1
sentinel_tilt:0
sentinel_running_scripts:0
sentinel_scripts_queue_length:0
sentinel_simulate_failure_flags:0
master0:name=mymaster,status=ok,address=192.168.119.10:6379,slaves=2,sentinels=3
#看到最后一条信息正确即成功了哨兵，哨兵主节点名字叫做mymaster，状态ok，监控地址是192.168.119.10:6379，有两个从节点，3个哨兵
```

# redis高可用故障实验

大致思路

- 杀掉主节点的redis进程6379端口，观察从节点是否会进行新的master选举，进行切换

- 重新恢复旧的“master”节点，查看此时的redis身份

  首先查看三个redis的进程状态

```vim
ps -ef|grep redis
```

检查三个节点的复制身份状态

第一个

```crmsh
[root@master tmp]# redis-cli -p 6381 info replication
# Replication
role:slave
master_host:127.0.0.1
master_port:6380
```

第二个

```pf
[root@master tmp]# redis-cli -p 6380 info replication
# Replication
role:master
connected_slaves:2
slave0:ip=127.0.0.1,port=6381,state=online,offset=54386,lag=0
slave1:ip=127.0.0.1,port=6379,state=online,offset=54253,lag=0
```

第三个

```crmsh
[root@master tmp]# redis-cli -p 6379 info replication
# Replication
role:slave
master_host:127.0.0.1
master_port:6380
```

**此时，干掉master！！！然后等待其他两个节点是否能自动被哨兵sentienl，切换为master节点**

```perl
ps -ef|grep 6380   #干掉master进程
```

此时查看两个slave的状态

**精髓就是查看一个参数**

```apache
master_link_down_since_seconds:13
```

**稍等片刻之后，发现slave节点成为master节点！！**

```crmsh
[root@master tmp]# redis-cli -p 6379 info replication
# Replication
role:master
connected_slaves:1
slave0:ip=127.0.0.1,port=6381,state=online,offset=41814,lag=1
```

**大功告成！！**

# 8.5 redis-cluster

为什么要用redis-cluster

## 并发问题

```undefined
redis官方生成可以达到 10万/每秒,每秒执行10万条命令
假如业务需要每秒100万的命令执行呢？
```

## 数据量太大

一台服务器内存正常是16~256G，假如你的业务需要500G内存，你怎么办？

新浪微博作为世界上最大的redis存储，就超过1TB的数据，去哪买这么大的内存条？

解决方案如下

1. 配置一个超级牛逼的计算机，超大内存，超强cpu，但是问题是。。。。
   ![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/s5.png)

2.正确的应该是考虑分布式，加机器，把数据分到不同的位置，分摊集中式的压力，**一堆机器做一件事**

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/s6.png)

## 客户端分片

```gauss
redis实例集群主要思想是将redis数据的key进行散列，通过hash函数特定的key会映射到指定的redis节点上
```

## 数据分布原理图

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/s8.png)

分布式数据库首要解决把整个数据集按照分区规则映射到多个节点的问题，即把数据集划分到多个节点上，每个节点负责整个数据的一个子集。

常见的分区规则有哈希分区和顺序分区。`Redis Cluster`采用哈希分区规则，因此接下来会讨论哈希分区规则。

- 节点取余分区
- 一致性哈希分区
- **虚拟槽分区(redis-cluster采用的方式)**

### 顺序分区

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/t1.png)

### 哈希分区

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/t2.png)

例如按照节点取余的方式，分三个节点

1~100的数据对3取余，可以分为三类

- 余数为0
- 余数为1
- 余数为2

那么同样的分4个节点就是hash(key)%4

节点取余的优点是简单，客户端分片直接是哈希+取余

### 虚拟槽分区

`Redis Cluster`采用虚拟槽分区

虚拟槽分区巧妙地使用了哈希空间，使用分散度良好的哈希函数把所有的数据映射到一个固定范围内的整数集合，整数定义为槽（slot）。

**Redis Cluster槽的范围是0 ～ 16383。**

**槽是集群内数据管理和迁移的基本单位。**采用大范围的槽的主要目的是为了方便数据的拆分和集群的扩展，

**每个节点负责一定数量的槽。**

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter8/pic/t3.png)

## 如何搭建redis-cluster

搭建集群分为几部

- 准备节点（几匹马儿）
- 节点通信（几匹马儿分配主从）
- 分配槽位给节点（slot分配给马儿）

redis-cluster集群架构

```vim
多个服务端，负责读写，彼此通信，redis指定了16384个槽。
多匹马儿，负责运输数据，马儿分配16384个槽位，管理数据。
ruby的脚本自动就把分配槽位这事做了
```

### 安装方式

官方提供通过ruby语言的脚本一键安装

### 步骤一、通过配置，redis.conf开启redis-cluster

```stata
port 7000
daemonize yes
dir "/opt/redis/data"
logfile "7000.log"
dbfilename "dump-7000.rdb"
cluster-enabled yes   #开启集群模式
cluster-config-file nodes-7000.conf　　#集群内部的配置文件
cluster-require-full-coverage no　　#redis cluster需要16384个slot都正常的时候才能对外提供服务，换句话说，只要任何一个slot异常那么整个cluster不对外提供服务。 因此生产环境一般为no
```

redis支持多实例的功能，我们在单机演示集群搭建，需要6个实例，三个是主节点，三个是从节点，数量为6个节点才能保证高可用的集群。

**每个节点仅仅是端口运行的不同！**

```stylus
[root@yugo /opt/redis/config 17:12:30]#ls
redis-7000.conf  redis-7002.conf  redis-7004.conf
redis-7001.conf  redis-7003.conf  redis-7005.conf
#确保每个配置文件中的端口修改！！
```

### 步骤二、运行redis实例

创建6个节点的redis实例

```apache
 1855  2018-10-24 15:46:01 redis-server redis-7000.conf
 1856  2018-10-24 15:46:13 redis-server redis-7001.conf
 1857  2018-10-24 15:46:16 redis-server redis-7002.conf
 1858  2018-10-24 15:46:18 redis-server redis-7003.conf
 1859  2018-10-24 15:46:20 redis-server redis-7004.conf
 1860  2018-10-24 15:46:23 redis-server redis-7005.conf
redis-server redis-7000.conf
redis-server redis-7001.conf
redis-server redis-7002.conf
redis-server redis-7003.conf
redis-server redis-7004.conf
redis-server redis-7005.conf
```

### 步骤3、开启redis-cluster

1. 下载、编译、安装Ruby
2. 安装rubygem redis
3. 安装redis-trib.rb命令

第一步、安装ruby

```awk
#下载ruby源码包
wget https://cache.ruby-lang.org/pub/ruby/2.3/ruby-2.3.1.tar.gz
#解压缩ruby源码包
tar -zxvf ruby-2.3.1.tar.gz
#编译且安装
./configure --prefix=/opt/ruby/
make && make install
```

第二步、配置ruby环境变量

```elixir
vim /etc/profile
写入如下配置
PATH=$PATH:/opt/ruby/bin
```

第三步、安装ruby操作redis包

```apache
wget http://rubygems.org/downloads/redis-3.3.0.gem
gem install -l redis-3.3.0.gem
```

第四部、安装redis-trib.rb命令

```awk
[root@yugo /opt/redis/src 18:38:13]#cp /opt/redis/src/redis-trib.rb /usr/local/bin/
```

### 一键开启redis-cluster集群

```dns
#每个主节点，有一个从节点，代表--replicas 1
redis-trib.rb create --replicas 1 127.0.0.1:7000 127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005
#集群自动分配主从关系  7000、7001、7002为 7003、7004、7005 主动关系
```

### 查看集群状态

```stata
redis-cli -p 7000 cluster info  
redis-cli -p 7000 cluster nodes  #等同于查看nodes-7000.conf文件节点信息
集群主节点状态
redis-cli -p 7000 cluster nodes | grep master
集群从节点状态
redis-cli -p 7000 cluster nodes | grep slave
```

### 写入redis-cluster集群数据

安装完毕后，检查集群状态

```dts
[root@yugo /opt/redis/src 18:42:14]#redis-cli -p 7000 cluster info
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6
cluster_size:3
cluster_current_epoch:6
cluster_my_epoch:1
cluster_stats_messages_ping_sent:10468
cluster_stats_messages_pong_sent:10558
cluster_stats_messages_sent:21026
cluster_stats_messages_ping_received:10553
cluster_stats_messages_pong_received:10468
cluster_stats_messages_meet_received:5
cluster_stats_messages_received:21026
```

**测试写入集群数据，登录集群必须使用redis-cli -c -p 7000必须加上-c参数**

```accesslog
127.0.0.1:7000> set name chao     
-> Redirected to slot [5798] located at 127.0.0.1:7001       
OK
127.0.0.1:7001> exit
[root@yugo /opt/redis/src 18:46:07]#redis-cli -c -p 7000
127.0.0.1:7000> ping
PONG
127.0.0.1:7000> keys *
(empty list or set)
127.0.0.1:7000> get name
-> Redirected to slot [5798] located at 127.0.0.1:7001
"chao"
```

# 8.6 redis-python api

```python
1、对redis的单实例进行连接操作
python3
>>>import redis
>>>r = redis.StrictRedis(host='localhost', port=6379, db=0,password='root')
>>>r.set('lufei', 'guojialei')
True
>>>r.get('lufei')
'bar'
--------------------
2、sentinel集群连接并操作
[root@db01 ~]# redis-server /data/6380/redis.conf
[root@db01 ~]# redis-server /data/6381/redis.conf
[root@db01 ~]# redis-server /data/6382/redis.conf 
[root@db01 ~]# redis-sentinel /data/26380/sentinel.conf &
--------------------------------
## 导入redis sentinel包
>>> from redis.sentinel import Sentinel  
##指定sentinel的地址和端口号
>>> sentinel = Sentinel([('localhost', 26380)], socket_timeout=0.1)  
##测试，获取以下主库和从库的信息
>>> sentinel.discover_master('mymaster')  
>>> sentinel.discover_slaves('mymaster')  
##配置读写分离
#写节点
>>> master = sentinel.master_for('mymaster', socket_timeout=0.1)  
#读节点
>>> slave = sentinel.slave_for('mymaster', socket_timeout=0.1)  
###读写分离测试   key     
>>> master.set('oldboy', '123')  
>>> slave.get('oldboy')  
'123'
----------------------
redis cluster的连接并操作（python2.7.2以上版本才支持redis cluster，我们选择的是3.5）
https://github.com/Grokzen/redis-py-cluster
3、python连接rediscluster集群测试
使用
python3
>>> from rediscluster import StrictRedisCluster  
>>> startup_nodes = [{"host": "127.0.0.1", "port": "7000"}]  
### Note: decode_responses must be set to True when used with python3  
>>> rc = StrictRedisCluster(startup_nodes=startup_nodes, decode_responses=True)  
>>> rc.set("foo", "bar")  
True  
>>>   
'bar'
```