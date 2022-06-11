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

## 4. 重新加载VNC守护程序

	systemctl daemon-reload
	systemctl start vncserver@:1
	systemctl status vncserver@:1
	
## 5. 设置开机启动

	systemctl enable vncserver@:1

在尝试连接到服务器之前，您可以执行的另一个测试是使用ss命令查看活动网络套接字：如果一切工作正常，您应该会看到 VNC 服务器工作正常并使用 TCP 端口 5901。执行命令：  

	ss -tulpn| grep vnc

## 6. 防火墙设置

由于我们的 VNC 服务正在 TCP 端口 5901 上侦听，所以防火墙必须放行。不建议直接简单粗暴的关闭防火墙。  

	systemctl start firewalld.service     # 启动防火墙
	systemctl enable firewalld.service    # 防火墙开机自动启动
	firewall-cmd --add-port=5901/tcp
	firewall-cmd --add-port=5901/tcp --permanent

## 7.  修改配置文件，使开启VNC就进入桌面

修改配置文件`~/.vnc/xstartup`

	 vim ~/.vnc/xstartup

因为我用的是xfce桌面，所以要从`/etc/X11/xinit/xinitrc`修改为`startxfce4`  

	#!/bin/sh

	unset SESSION_MANAGER
	unset DBUS_SESSION_BUS_ADDRESS
	startxfce4
	vncserver -kill $DISPLAY
	
## 8. 重启一下VNC服务

	systemctl stop vncserver@:1
	systemctl start vncserver@:1


## 9. 添加vnc用户
	
	adduser user1
	passwd user1
	su - user1
	vncpasswd
	su -
	cp /etc/systemd/system/vncserver@\:1.service /etc/systemd/system/vncserver@\:2.service
	vim /etc/systemd/system/vncserver@\:2.service

![user1 的VNC 配置](https://github.com/kaijing-zhang/kaijing-zhang.github.io/blob/main/img/user1%20vnc%20%E9%85%8D%E7%BD%AE.png)  

user1的vcn配置如下：  

	
	[Unit]
	Description=Remote desktop service (VNC)
	After=syslog.target network.target

	[Service]
	Type=simple

	# Clean any existing files in /tmp/.X11-unix environment
	ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'
	ExecStart=/usr/bin/vncserver_wrapper user1 %i
	PIDFile=/home/user1/.vnc/%H%i.pid
	ExecStop=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'

	[Install]
	WantedBy=multi-user.target
## 10. 重新加载VNC守护程序
	systemctl daemon-reload
	systemctl start vncserver@:2
	systemctl status vncserver@:2

## 11. 设置开机启动
	systemctl enable vncserver@:2
	ss -tulpn| grep vnc

## 12. 修改user1配置文件，使开启VNC就进入桌面

	su - user1
	vim ~/.vnc/xstartup
	
从`/etc/X11/xinit/xinitrc`修改为`startxfce4`  
	
	#!/bin/sh

	unset SESSION_MANAGER
	unset DBUS_SESSION_BUS_ADDRESS
	startxfce4
	vncserver -kill $DISPLAY

## 13. 重启VNC服务
	systemctl stop vncserver@:1
	systemctl start vncserver@:1




