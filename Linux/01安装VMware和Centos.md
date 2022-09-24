## 安装方式

想学Linux，如何安装才是正确的方式呢.

```vim
pc可以选择
    -纯系统 Linux或者windows
    -双系统    Windows+Linux
    -虚拟化技术    Windows+vmware workstation/ESXI + Linux
```

使用虚拟机来安装Linux，除了安装方便简单，也不怕讲电脑系统搞坏，无论你怎么玩，也不怕Linux坏掉，因为vmware可以快照！可以重新安装，多玩坏几次你就是装机大神了！

## 下载centos镜像文件

要安装centos系统，就必须得有centos系统软件安装程序，可以通过浏览器访问centos官网[http://www.centos.org，然后找到Downloads](http://www.centos.xn--org%2Cdownloads-682vi1tvz8dekyc/) - > mirrors链接，点击后进入下载，但是由于这是国外的网址，下载速度肯定受限。

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/p1.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/p2.png)

## 国内镜像站

因此可以使用国内的镜像源

```awk
https://opsx.alibaba.com/mirror#阿里云官方镜像站
iso下载地址（此DVD映像包含可以使用该软件安装的所有软件包安装程序。这是大多数用户的推荐图像。）：https://mirrors.aliyun.com/centos/7/isos/x86_64/CentOS-7-x86_64-DVD-1804.iso
```

## 安装VMware虚拟机

```awk
vmware是什么？
有了这个软件，大家就不需要为了学习linux特意再去买一台电脑了，虚拟机能让用户在一台机器上模拟出多个操作系统的软件，一般的机器配置足够胜任虚拟机的任务。
虚拟机不但可以虚拟出硬件资源，把实验环境与真机文件分离保证数据安全，更nb的是当你手残删掉系统核心配置时，还能有”快照“的功能，立即恢复到出错前的状态，省去装机的超长时间。。。。
(Windows用户)VMware Workstation是一款功能强大的桌面虚拟计算机软件，提供用户可在单一的桌面上同时运行不同的操作系统，和进行开发、测试 、部署新的应用程序的最佳解决方案。
下载激活地址：http://www.zdfans.com/html/5928.html
(Mac用户) VMware fusion
简单的说，虚拟机（virtual Machine）软件就是一套特殊的软件，同时可以用“多个操作系统”
虚拟出硬件+操作系统==服务器+OS
误区：学Linux不需要再物理机上安装，费时费力，采用虚拟机是最合适的方式
```

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/p3.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/p4.png)

## 安装VMware过程图解

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v1.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v2.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v3.png)

## VMware安装Centos

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v4.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v5.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v6.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v7.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v8.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v9.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v10.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v11.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v12.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v13.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v114.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v15.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v16.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v17.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v18.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v19.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v20.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v21.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v22.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/v23.png)

## 安装完成后重启Linux

此时进入这个黑乎乎的界面，输入root账号与密码，成功进入linux系统

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/l1.png)

## 确保你的电脑支持虚拟化

安装 RHEL 7 或 CentOS 7 系统时，大家的电脑的 CPU 需要支持 VT(Virtualization Technology，虚拟化技术)。所谓 VT，指的是让单台计算机能够分割出多个独立资源区， 并让每个资源区按照需要模拟出系统的一项技术，其本质就是通过中间层实现计算机 资源的管理和再分配，让系统资源的利用率最大化。其实只要您的电脑不是五六年前 买的，价格不低于三千元，它的 CPU 就肯定会支持 VT 的。如果开启虚拟机后依然提 示“CPU 不支持 VT 技术”等报错信息，请重启电脑并进入到 BIOS 中把 VT 虚拟化功 能开启即可。

# 忘记root登录密码

针对好多好多同学经常忘记root密码。。。超哥这里给你整理怎么重置root密码！！

重启 Linux 系统主机并出现引导界面时，按下键盘上的 e 键进入内核编辑界面

## 重置root密码图解

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/h1.png)

在 linux16 参数这行的最后面追加“rd.break”参数，然后按下 Ctrl + X 组合键来运行修 改过的内核程序

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/h2.png)

大约 30 秒过后，进入到系统的紧急求援模式，

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/h33.png)

依次输入以下命令，等待系统重启操作完毕，然后就可以使用新密码来登录Linux 系统了

```awk
 mount -o remount,rw /sysroot
    chroot /sysroot
    passwd
    touch /.autorelabel
exit reboot
```

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic/h4.png)

# 3.2 远程登录Linux

## 为什么要远程Linux

在实际的工作场景中，虚拟机界面或者物理服务器本地的终端都是很少接触的，因为服务器装完系统之后，都要拉倒IDC机房托管，如果是购买的云主机，那更碰不到服务器本体了，只能通过远程连接的方式管理自己的Linux系统。

因此在装好Linux系统之后，使用的第一步应该是配置好客户端软件（ssh软件进行连接）连接Linux系统。

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic2/s1.png)

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic2/s2.png)

## 远程连接工具

Xshell和SecureCRT
![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic2/s3.png)![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic2/s4.png)

## 远程连接与IP地址

在Linux上输入命令 ，查看ip地址，然后测试远程连接

```armasm
ip addr show
```

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter3/pic2/s5.png)

## 虚拟机网络连接方式

仅主机模式， host-only ，也是单机模式

NAT模式 (推荐使用)

桥接模式

## 远程连接Linux命令

```nginx
ssh 用户名@ip
如：
ssh root@192.168.1.110
默认ssh端口是22
确保Linux服务器，开启了sshd服务，且防火墙允许访问
```