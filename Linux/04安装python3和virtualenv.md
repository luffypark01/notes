centos7默认是装有python的，咱们先看一下

```1c
#检查python版本
[root@oldboy_python ~ 17:23:54]#python -V
Python 2.7.5
```

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter6/pic/p1.png)

《震惊，python2.7不再维护！》

![img](https://pythonav.com/media/uploads/2019/02/22/Gobook/Chapter6/pic/p2.png)

## 源码编译安装python3

### 安装python前的库环境，非常重要

如果要正确安装python3，且使用python3的功能，需提前解决如下的环境依赖的问题

```mipsasm
yum install gcc patch libffi-devel python-devel  zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel -y
```

### 下载python3源码包

**网址：https://www.python.org/downloads/release/python-362/**

**下载地址：https://www.python.org/ftp/python/3.6.2/Python-3.6.2.tgz**

### 解压缩源码包

```apache
wget https://www.python.org/ftp/python/3.6.7/Python-3.6.7.tar.xz
xz -d Python-3.6.7.tar.xz
tar -xf Python-3.6.7.tar
```

### 编译且安装

进入python源码包目录，编译且安装

```awk
./configure --prefix=/opt/python3/
make && make install
```

#### configure

这一步一般用来生成 Makefile，为下一步的编译做准备，你可以通过在 configure 后加上参数来对安装进行控制，比如代码:

```awk
./configure --prefix=/usr
```

上面的意思是将该软件安装在 /usr 下面，执行文件就会安装在 /usr/bin （而不是默认的 /usr/local/bin)，资源文件就会安装在 /usr/share（而不是默认的/usr/local/share）。

同时一些软件的配置文件你可以通过指定 —sys-config= 参数进行设定。有一些软件还可以加上 —with、—enable、—without、—disable 等等参数对编译加以控制，你可以通过允许 ./configure —help 察看详细的说明帮助。

#### make

这一步就是编译，大多数的源代码包都经过这一步进行编译（当然有些perl或python编写的软件需要调用perl或python来进行编译）。

如果 在 make 过程中出现 error ，你就要记下错误代码（注意不仅仅是最后一行），然后你可以向开发者提交 bugreport（一般在 INSTALL 里有提交地址），或者你的系统少了一些依赖库等，这些需要自己仔细研究错误代码。

make 的作用是开始进行源代码编译，以及一些功能的提供，这些功能由他的 Makefile 设置文件提供相关的功能，比如 make install 一般表示进行安装，make uninstall 是卸载，不加参数就是默认的进行源代码编译。

make 是 [Linux](http://www.icultivator.com/tag/linux) 开发套件里面自动化编译的一个控制程序，他通过借助 Makefile 里面编写的编译规范进行自动化的调用 [gcc](http://www.icultivator.com/tag/gcc) 、ld 以及运行某些需要的程序进行编译的程序。一般情况下，他所使用的 Makefile 控制代码，由 configure 这个设置脚本根据给定的参数和系统环境生成。

#### make install

这条命令来进行安装（当然有些软件需要先运行 make check 或 make test来进行一些测试），这一步一般需要你有 root 权限（因为要向系统写入文件）

## 配置python3环境变量

```awk
在/etc/profile最后一行添加
export PATH=/opt/python3/bin:$PATH
然后
source /etc/profile
```

# 6.1 virtualenv讲解

**在使用 Python 开发的过程中，工程一多，难免会碰到不同的工程依赖不同版本的库的问题；**

**亦或者是在开发过程中不想让物理环境里充斥各种各样的库，引发未来的依赖灾难。**

**此时，我们需要对于不同的工程使用不同的虚拟环境来保持开发环境以及宿主环境的清洁。**

**这里，就要隆重介绍 virtualenv，一个可以帮助我们管理不同 Python 环境的绝好工具。**

**virtualenv 可以在系统中建立多个不同并且相互不干扰的虚拟环境。**

## 安装配置virtualenv

```awk
#指定清华源下载pip的包
pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple virtualenv
#升级pip工具
pip3 install --upgrade pip
1.安装virtualenv
pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple virtualenv
2.创建目录
mkdir Myproject
cd Myproject
3.创建独立运行环境-命名
virtualenv --no-site-packages --python=python3  venv#得到独立第三方包的环境，并且指定解释器是python3
4.进入虚拟环境
source venv/bin/activate#此时进入虚拟环境(venv)Myproject
5.安装第三方包
(venv)Myproject: pip3 install django==1.9.8
#此时pip的包都会安装到venv环境下，venv是针对Myproject创建的
6.退出venv环境
deactivate命令
7.
virtualenv是如何创建“独立”的Python运行环境的呢？原理很简单，就是把系统Python复制一份到virtualenv的环境，用命令source venv/bin/activate进入一个virtualenv环境时，virtualenv会修改相关环境变量，让命令python和pip均指向当前的virtualenv环境。
```

## 确保开发环境的一致性

```mel
1.假设我们在本地开发环境，准备好了项目+依赖包环境
2.现在需要将项目上传至服务器，上线发布
3.那么就要保证服务器的python环境一致性
解决方案：
1.通过命令保证环境的一致性，导出当前python环境的包
pip3 freeze > requirements.txt   
这将会创建一个 requirements.txt 文件，其中包含了当前环境中所有包及 各自的版本的简单列表。
可以使用 “pip list”在不产生requirements文件的情况下， 查看已安装包的列表。
2.上传至服务器后，在服务器下创建virtualenv，在venv中导入项目所需的模块依赖
pip3 install -r requirements.txt
```

# 6.2 virtualenvwrapper讲解

**virtualenv 的一个最大的缺点就是：**

**每次开启虚拟环境之前要去虚拟环境所在目录下的 bin 目录下 source 一下 activate，这就需要我们记住每个虚拟环境所在的目录。**

**并且还有可能你忘记了虚拟环境放在哪。。。**

- 一种可行的解决方案是，将所有的虚拟环境目录全都集中起来，例如/opt/all_venv/，并且针对不同的目录做不同的事。
- 使用virtualenvwrapper管理你的虚拟环境（virtualenv），其实他就是统一管理虚拟环境的目录，并且省去了source的步骤。

## 安装virtualenvwrapper

```awk
#安装
pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple virtualenvwrapper
#设置环境变量，每次开机加载virtualevnwrapper
export WORKON_HOME=~/Envs   #设置virtualenv的统一管理目录
export VIRTUALENVWRAPPER_VIRTUALENV_ARGS='--no-site-packages'   #添加virtualenvwrapper的参数，生成干净隔绝的环境
export VIRTUALENVWRAPPER_PYTHON=/opt/python3/bin/python3     #指定python解释器
source /opt/python34/bin/virtualenvwrapper.sh #执行virtualenvwrapper安装脚本
读取文件，使得生效，此时已经可以使用virtalenvwrapper
```

## 管理虚拟环境的命令

virtualenvwrapper 提供环境名字的tab补全功能。
当有很多环境， 并且很难记住它们的名字时，这就显得很有用。

```asciidoc
创建一个虚拟环境：
mkvirtualenv my_django115
这会在 ~/Envs 中创建 my_django115 文件夹。
激活虚拟环境my_django115
workon my_django115
也可以手动停止虚拟环境
deactivate
删除虚拟环境，需要先退出虚拟环境
rmvirtualenv my_django115
lsvirtualenv
列举所有的环境。
cdvirtualenv
导航到当前激活的虚拟环境的目录中，比如说这样您就能够浏览它的 site-packages 。
cdsitepackages
和上面的类似，但是是直接进入到 site-packages 目录中。
lssitepackages
显示 site-packages 目录中的内容。
```