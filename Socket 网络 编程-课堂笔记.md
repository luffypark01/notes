Socket 网络 编程

  	什么是socket ? 
  		socket是基于tcp/ip协议 封装出来的一套编程接口。 把tcp/ip协议的各种细节给你隐藏了，
    
    socket 起源
    	70年代 ， 伯克利分校开发的东西，unix 系统 ， 
    	进行间通讯， 

    	地址族
	    	面向文件 的 socket 通信 
	    		本机进程间通讯

	    	面向网络协议 的socket 通信 
	    		ip v4  socket.AF_INET 
	    		ip v6  socket.AF_INET6
	    socket 协议 类型
	    	tcp socket.SOCK_STREAM 
	    	udp socket.SOCK_DGRAM
	    	raw ip层  socket.SOCK_RAW


SOCKET工作流程
	
	server 1345555
		1. 买 个手机		# 初始化一个socket 实例 ， 类 s =  socket.socket()
		2. 买 个电话卡，号码， 卡绑定到手机上 # 绑定ip+端口 s.bind(ip+port)
		3. 开机		# 监听端口  s.listen()
		4. 等电话     # 等待连接 s.accept()

	client 
		1. 有个手机 
		3. 绑定电话号
		3. 开机
		4. 拨号 1345555

socket 模块编程各参数介绍 

基于tcp 协议 的socket 开发

开发简单的ssh 远程命令执行工具
	
	1. 客户端给服务端发指令 ， 服务端收到指令后执行，并且把结果返回 给客户端
	2. 客户端收到结果，并打印

	df -h 查看硬盘
	ls 查看 文件 
	pwd查看 当前目录 
	cat filename 
	top 查看系统状态 

	alex_bigking

服务器间实现文件传送
	客户端循环收服务器端发来的数据 ，
	得让客户端知道 服务器发过来的数据的长度


粘包问题解决
	缓冲区
	io_wait 
	缓冲区清空有2种情况
		1. 缓冲区满了(<10k)
		2. 超时(ms)

	解决办法：
		1. 让缓冲区超时
		2. 通过一个recv让2条消息强行隔开
		3. 约定一个固定的长度的消息头，消息头里包含里命令结果的长度 
			3.1 服务器端收到指令，执行，拿到结果 ，
			3.2 生成一个消息头，固定长度 ，消息头里，写上命令结果的长度
				100，  1000，40000

传送文件 
	server 
		1. 服务器端收到 客户端指令， 解析 ， 判断get开头，代表下载文件 。 
		2. 提取文件名，判断文件是否在服务器上存在，如果存在就下载 
		3. 拿到文件的大小，发给客户端
		4. 打开文件 ，发送文件 内容 

	client 
		1. 发送下载 文件 的指令
			get filename 
		2. 接收固定长度的消息头，解析
		3. 拿到文件的大小， 
		4. 在本地创建一个文件 ，文件名.download
		5. 循环接收数据



像迅雷下载一样带下载进度条功能 
  100% 
  received_size / total_size 

  能否实现在同一行不断的打印？

  能否实现当前行的数据修改
  	清空当前行数据，再写新内容 

  把进度条封装成单独的函数
  	生成器的作用， 实现函数的中断， 


基于udp 协议 的socket 开发
	特点： 	
		无连接，面向数据报文的协议 ， 
		不保证数据交付，也没有数据重传机制、验证机制 
		延迟小

	应用场景：
		直播、聊天软件 ， qq 


	tcp vs udp socket 
		server: 
			不需要listen , accept 
		client;
			不需要connect建立 连接 ，直接 发消息就可以

		发消息 : 
			tcp :send(msg)
			udp : send_to(msg,addr)

		收消息： 
			tcp : recv(1024)
			udp : recv_from(1024)



批量给很多服务器发指令 
	ansible 

基于udp 开发一个聊天室原理介绍 

