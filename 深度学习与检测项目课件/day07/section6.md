# Web Server开启

### 学习目标

- 目标
  - 无
- 应用
  - 应用TensorFlow Serving Client完成模型服务调用预测
  - 应用Docker完成Web服务的运行

## 5.8.2 Docker管理运行Web部分

此部份的docker镜像需要自己通过Dockerfile来制作

1. 创建名称的`Dockerfile`的文本文件（没有后缀名），

   ```dockerfile
   # FROM表示从哪个镜像基础上进行构建新的镜像
   # 从Python官方提供的3.6.7的镜像基础上进行构建
   FROM python:3.6.7-stretch
   
   # 表示在镜像系统中的 /tmp目录下进行操作
   WORKDIR /tmp
   # 将主机当前目录下的requirements.txt文件复制到镜像系统中/tmp目录下
   COPY ./requirements.txt .
   
   # RUN表示运行命令，安装python环境依赖包
   RUN pip install -r requirements.txt
   
   # 表示镜像对外暴露5000端口，即通过镜像运行的容器程序可以让外部访问5000端口
   EXPOSE 5000
   
   # 将工作目录切换到/app下
   WORKDIR /app
   # 将主机当前目录下的start.sh文件复制到镜像系统中的/home目录下
   COPY ./start.sh /home/start.sh
   # 表示镜像在被容器启动时（即此虚拟机在被开启时）执行的命令，意思是此虚拟机开启后立即执行start.sh的脚本程序
   CMD ["/home/start.sh"]
   ```

   requirements.txt

   ```python
   grpcio==1.12.0
   matplotlib==2.2.2
   numpy==1.14.2
   pandas==0.20.3
   Pillow==4.3.0
   tensorflow==1.12.0
   flask==1.0.2
   gunicorn==19.7.1
   tensorflow-serving-api==1.10.1
   keras==1.2.2
   ```

   - flask 与gunicorn 为flask运行依赖的包

   start.sh

   ```shell
   #!/bin/bash
   gunicorn -w 2 -b 0.0.0.0:5000 main:app
   ```

   gunicorn为启动flask服务的命令，`-w`表示以2个进程运行，`-b`表示绑定的ip地址和端口，`main:app`表示运行的文件main.py和其中定义的flask app对象

2. 生成镜像

   将上述三个文件放到一个目录中，然后**在此目录中**执行（执行下面命令所在的目录中一定要包含上述三个文件）

   ```shell
   docker build -t tf-serving-web .
   ```

   - -t表示给生成的镜像起名（打标签tag）

   此命令执行要花费很长时间，请耐心等待！！！

3. 镜像中并未包含flask程序代码，只提供了flask运行的环境，方便更新flask代码时，环境不用变化。

   flask程序代码如下

   ```python
   from flask import Flask
   from flask import request, abort, send_file, render_template, jsonify
   import base64
   from urllib import parse
   
   from prediction import make_prediction,make_prediction_v2
   
   app = Flask(__name__)
   @app.route("/api/v1/prediction/commodity", methods=['POST'])
   def commodity_predict():
       """
       商品图片预测接口,供web页面ajax访问
       """
       # 获取用户上传图片
       image = request.files.get('image')
       if not image:
           # 如果没有上传图片，返回400状态码，表示请求错误
           abort(400)
   
       # 预测标记
       result_img = make_prediction(image.read())
       data = result_img.read()
       result_img.close()
   
       # 返回预测结果被标记的图片，200状态码，
       return data, 200, {'Content-Type': 'image/png'}
   
   @app.route("/")
   def index():
       """
       呈现Web页面
       """
       return render_template('index.html')
   
   if __name__ == "__main__":
       app.run(debug=True, host='0.0.0.0')
   ```

4. 运行web 程序

   ```shell
   docker run -t -p 80:5000 -v /Users/huxinghui/workspace/ml/detection/ssd_detection/web_code:/app --name=web tf-serving-web
   ```

   - `-t` 表示创建一个伪终端，供flask程序运行
   - `-p 80:5000`表示将主机的80端口映射到docker容器程序（flask程序）的5000端口（上述start.sh脚本程序让flask运行在5000端口），即访问主机的80端口时，就是访问的flask docker程序的5000端口
   - `-v /home/ubuntu/web_code:/app` 表示文件映射，将主机中存放flask程序的目录（/home/ubuntu/web_code，此目录依自己存放flask程序的目录而定）映射到docker 容器中的/app目录中（因为docker启动后运行在/app目录中）
   - `--name=web`给启动的docker容器程序命名，起名为web，方便启动停止管理容器时使用
   - `tf-serving-web`表示启动的镜像

5. 现在可以通过访问主机ip地址（如127.0.0.1）来加载页面进行测试了

> 注： run只需要开启一遍即可，不需要重复run，否则会遇到名字存在（--name=web），容器不能重复。
>
> 一般启动用docker start即可

```python
docker stop web2  # 停止调试的docker容器
docker rm web2  # 删除调试的docker容器环境
docker start web  # 启动正常的flask docker容器
docker restart web
```

## 5.8.3调试web程序（flask程序）的方法（了解）

1. 首先需要停止运行中的flask docker程序

   ```shell
   docker ps  # 查看运行中flask docker容器的名称
   docker stop web # 停止flask docker容器（前述中起名为web）
   ```

2. 创建一个用于调试flask docker容器

   ```shell
   docker run -dti -p 80:5000 -v /home/ubuntu/web_code:/app --name=web2 tf-serving-web /bin/bash
   ```

   - `-dti`  表示后台独立运行（d）、分配一个伪终端（t）、可以输入与容器程序进行交互（i）
   - `--name=web2`表示这个容器起名为web2
   - `/bin/bash`表示容器启动后 运行这个指定的命令即bash

3. 进入到flask docker容器环境中（理解为虚拟机中），进行调试

   ```shell
   docker exec -it web2 /bin/bash
   ```

   - `-it` 表示分配伪终端并且可以进行输入交互
   - `web2`表示要进入的容器名称
   - `/bin/bash`表示容器环境（虚拟环境）后执行的命令，即进入后得到bash终端

4. 手动启动flask程序，进行调试

   ```python
   cd /app  # 进入app目录
   python main.py  # 运行flask程序
   ```

5. 在docker容器外部重新修改flask程序后，在容器内重新执行`python main.py`即可

6. 从容器环境中退出执行`exit`命令

7. 调试完成后，清理调试环境，再重启flask docker容器程序

   ```shell
   docker stop web2  # 停止调试的docker容器
   docker rm web2  # 删除调试的docker容器环境
   docker start web  # 启动正常的flask docker容器
   ```

