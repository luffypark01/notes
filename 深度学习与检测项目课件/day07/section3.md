#     7.3 Docker使用介绍

### 学习目标

- 目标
  - 无
- 应用
  - 无

## 7.3.1 Docker安装

* 1、获取Docker安装包

```
wget -qO- https://get.docker.com/ | sh
```

* 2、启动Docker后台服务

```
sudo service docker start
```

## 7.3.2 Docker命令

* docker ps   # 查看正在运行的docker
* docker ps --all  # 查看所有运行的docker，包括停止和未启动异常退出的
* docker stop 容器ID(CONTAINER ID)或容器名称(NAMES) # 停止运行中的docker，CONTAINER ID可以只输入前几位，能区分开不同的容器即可
* docker start 容器ID(CONTAINER ID)或容器名称（NAMES） # 启动停止的docker
* docker exec: 在运行的容器中执行命令
* **docker pull :** 从镜像仓库中拉取或者更新指定镜像
* **docker push :** 将本地的镜像上传到镜像仓库,要先登陆到镜像仓库





