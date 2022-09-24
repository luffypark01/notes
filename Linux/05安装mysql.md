# MYSQL(mariadb)

Red Hat Enterprise Linux/CentOS 7.0 发行版已将默认的数据库从 MySQL 切换到 MariaDB

```undefined
MariaDB数据库管理系统是MySQL的一个分支，主要由开源社区在维护，采用GPL授权许可。
开发这个分支的原因之一是：甲骨文公司收购了MySQL后，有将MySQL闭源的潜在风险，因此社区采用分支的方式来避开这个风险。
MariaDB的目的是完全兼容MySQL，包括API和命令行，使之能轻松成为MySQL的代替品。
```

## yum安装mariadb

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter7/pic/m11.png)

### 添加yum源安装Mariadb

```awk
1、首先在 RHEL/CentOS 和 Fedora 操作系统中添加 MariaDB 的 YUM 配置文件 MariaDB.repo 文件。
#编辑创建mariadb.repo仓库文件
vi /etc/yum.repos.d/MariaDB.repo
2、添加repo仓库配置
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.1/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```

## yum一键安装mariadb

```axapta
当 MariaDB 仓库地址添加好后，你可以通过下面的一行命令轻松安装 MariaDB。
yum install MariaDB-server MariaDB-client -y
mariadb数据库的相关命令是：
systemctl start mariadb  #启动MariaDB
systemctl stop mariadb  #停止MariaDB
systemctl restart mariadb  #重启MariaDB
systemctl enable mariadb  #设置开机启动
```

### 初始化mysql

```xquery
在确认 MariaDB 数据库软件程序安装完毕并成功启动后请不要立即使用。为了确保数据 库的安全性和正常运转，需要先对数据库程序进行初始化操作。这个初始化操作涉及下面 5 个 步骤。
➢ 设置 root 管理员在数据库中的密码值(注意，该密码并非 root 管理员在系统中的密 码，这里的密码值默认应该为空，可直接按回车键)。
➢ 设置 root 管理员在数据库中的专有密码。
➢ 随后删除匿名账户，并使用 root 管理员从远程登录数据库，以确保数据库上运行的业
务的安全性。
➢ 删除默认的测试数据库，取消测试数据库的一系列访问权限。
➢ 刷新授权列表，让初始化的设定立即生效。
```

确保mariadb服务器启动后，执行命令初始化

```ebnf
mysql_secure_installation
```

### 创建mysql普通用户

生产环境里不会死磕root用户，为了数据库的安全以及和其他用户协同管理数据库，就需要创建其他数据库账户，然后分配权限，满足工作需求。

```pgsql
MariaDB [(none)]> create user username@'%' identified by 'password';
MariaDB [(none)]> use mysql;
MariaDB [mysql]> select host,user,password from user where user='username';
```

切换普通用户yuchao，查看数据库信息，发现无法看到完整的数据库列表

```crmsh
[root@master ~]# mysql -uusername -p -h 127.0.0.1
MariaDB [(none)]> show databases;
```

### mysql授权sql

mysql使用grant命令对账户进行授权，grant命令常见格式如下

```pgsql
grant 权限 on 数据库.表名 to 账户@主机名            对特定数据库中的特定表授权
grant 权限 on 数据库.* to 账户@主机名            　　对特定数据库中的所有表给与授权
grant 权限1,权限2,权限3 on *.* to 账户@主机名   　　 对所有库中的所有表给与多个授权
grant all privileges on *.* to 账户@主机名   　　 对所有库和所有表授权所有权限
```

退出数据库，使用root登录，开始权限设置

```pgsql
[root@master ~]# mysql -uroot -p
MariaDB [(none)]> use mysql;
##授予远程登录的权限
MariaDB [(none)]> grant all privileges on *.* to username@"%" identified by 'password';
MariaDB [mysql]> show grants for username@"%";
```

移除权限

```pgsql
MariaDB [(none)]> revoke all privileges on *.* from username@"%";
```

## mysql备份与恢复

mysqldump命令用于备份数据库数据

```crmsh
[root@master ~]# mysqldump -u root -p --all-databases > /tmp/db.dump
```

进入mariadb数据库，删除一个db

```crmsh
[root@master ~]# mysql -uroot -p
MariaDB [(none)]> drop database s11;
```

进行数据恢复，吧刚才重定向备份的数据库文件导入到mysql中

```crmsh
[root@master ~]# mysql -uroot -p < /tmp/db.dump
```

# 7.1 mysql主从复制

MySQL数据库的主从复制方案，是其自带的功能，并且主从复制并不是复制磁盘上的数据库文件，而是通过binlog日志复制到需要同步的从服务器上。

MySQL数据库支持单向、双向、链式级联，等不同业务场景的复制。在复制的过程中，一台服务器充当主服务器（Master），接收来自用户的内容更新，而一个或多个其他的服务器充当从服务器（slave），接收来自Master上binlog文件的日志内容，解析出SQL，重新更新到Slave，使得主从服务器数据达到一致。

主从复制的逻辑有以下几种

一主一从，单向主从同步模式，只能在Master端写入数据

一主多从

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter7/pic/m2.png)

应用场景

```gauss
利用复制功能当Master服务器出现问题时，我们可以人工的切换到从服务器继续提供服务，此时服务器的数据和宕机时的数据几乎完全一致。
复制功能也可用作数据备份，但是如果人为的执行drop,delete等语句删除，那么从库的备份功能也就失效了.
```

## 主从复制原理图

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter7/pic/m3.png)

```autoit
(1) master将改变记录到二进制日志(binary log)中（这些记录叫做二进制日志事件，binary log events）； 
(2) slave将master的binary log events拷贝到它的中继日志(relay log)； 
(3) slave重做中继日志中的事件，将改变反映它自己的数据。
```

### 步骤1 配置master主库

```routeros
#查看数据库状态
systemctl status mariadb
#停mariadb
systemctl stop mariadb
#修改配置文件
vim /etc/my.cnf
#修改内容
#解释：server-id服务的唯一标识（主从之间都必须不同）；log-bin启动二进制日志名称为mysql-bin 
　　[mysqld]
　　server-id=1
　　log-bin=mysql-bin
#重启mariadb
systemctl start mariadb
```

## 步骤2 在master主库添加进行同步的用户

```pgsql
1.新建用于主从同步的用户username,允许登录的从库是'%'
create user 'username'@'%' identified by 'password';
2.#题外话：如果提示密码太简单不复合策略加在前面加这句
mysql> set global validate_password_policy=0;
3.给从库账号授权,说明给username从库复制的权限，在"%"机器上复制
grant replication slave on *.* to 'username'@'%';
#检查主库创建的复制账号
select user,host from mysql.user;
#检查授权账号的权限
show grants for username@'%';
实现对主数据库锁表只读，防止数据写入，数据复制失败
flush table with read lock;
4.检查主库的状态
MariaDB [(none)]> show master status
-> ;
+------------------+----------+--------------+------------------+
| File | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+------------------+----------+--------------+------------------+
| mysql-bin.000001 | 575 | | |
+------------------+----------+--------------+------------------+
1 row in set (0.00 sec)
File是二进制日志文件名，Position 是日志开始的位置。后面从库会用到 后面从库会用到 后面从库会用到！！！！！！
5.锁表后，一定要单独再打开一个SSH窗口，导出数据库的所有数据，
[root@oldboy_python ~ 19:32:45]#mysqldump -uroot -p --all-databases > /data/all.sql 
6.确保数据导出后，没有数据插入，完毕再查看主库状态
show master status;
7.导出数据完毕后，解锁主库，恢复可写；
unlock tables;
8.将备份导出的数据scp至Slave数据库
scp /data/all.sql root@ip:/data/
```

## 步骤3 配置salve从库机器

```pgsql
1.设置server-id值并关闭binlog功能参数
数据库的server-id在主从复制体系内是唯一的，Slave的server-id要与主库和其他从库不同，并且注释掉Slave的binlog参数。
2.因此修改Slave的/etc/my.cnf，写入
[mysqld]
server-id=3
3.重启数据库
systemctl restart mariadb
4.检查Slava从数据库的各项参数
show variables like 'log_bin';
show variables like 'server_id';
5.恢复主库Master的数据导入到Slave库
导入数据（注意sql文件的路径）
mysql>source /data/all.sql;
方法二：
#mysql -uroot -p  < abc.sql 
6.配置复制的参数，Slave从库连接Master主库的配置
mysql > change master to master_host='x.x.x.x',
master_user='username',
master_password='password',
master_log_file='mysql-bin.000001',
master_log_pos=575;
7.启动从库的同步开关，测试主从复制的情况
start slave;
8.查看复制状态
show slave status\G;
```

## 步骤4 检测主从复制状态关键点

```yaml
MariaDB [(none)]> show slave status\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 192.168.119.10
                  Master_User: chaoge
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000001
          Read_Master_Log_Pos: 1039
               Relay_Log_File: slave-relay-bin.000002
                Relay_Log_Pos: 537
        Relay_Master_Log_File: mysql-bin.000001
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
```

Tip:

注意此处还未配置从库的只读模式，只需在slave服务器上配置/etc/my.cnf，加上以下配置，并且在slave上创建普通用户，使用普通用户主从同步即可达到只读的效果

如果用root用户，无法达到readonly，这是一个坑

```ini
[mysqld]
character-set-server=utf8
collation-server=utf8_general_ci
log-error=/var/log/mysqld.log
server-id=3
read-only=true
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
```