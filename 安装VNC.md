# 安装VNC

VNC（Virtual Network Computing），为一种使用RFB协议的屏幕画面分享及远程操作软件。此软件借由网络，可发送键盘与鼠标的动作及即时的屏幕画面。  

## 1. 安装TigerVNC

	yum install tigervnc-server

## 2. 设置VNC密码

	vncpasswd

![密码](https://github.com/kaijing-zhang/kaijing-zhang.github.io/blob/main/img/%E8%AE%BE%E7%BD%AEVNC%E5%AF%86%E7%A0%81.png)

## 3. 为root创建一个VNC配置文件

这样做的最快方法就是复制位于/lib/systemd/system/文件夹中的共享 VNC模板文件，然后更改它：  

	cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service

这里新文件中的数字1将用与此服务特定实例的显示编号，这一点很重要。因为它还将确定我们的 VNC 服务器将使用的 TCP 端口，等于 5900 + 显示编号。第一个是5901，然后是5902，等等。  

	vim /etc/systemd/system/vncserver@:1.service

![root vnc 配置](https://github.com/kaijing-zhang/kaijing-zhang.github.io/blob/main/img/root%20vnc%20%E9%85%8D%E7%BD%AE.png)  

配置如下：  
	
	[Unit]
	Description=Remote desktop service (VNC)
	After=syslog.target network.target

	[Service]
	Type=simple

	# Clean any existing files in /tmp/.X11-unix environment
	ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'
	ExecStart=/usr/bin/vncserver_wrapper root %i
	PIDFile=/root/.vnc/%H%i.pid
	ExecStop=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'

	[Install]
	WantedBy=multi-user.target

