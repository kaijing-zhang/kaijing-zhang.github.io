# 安装FTP

## 1. 安装vsftpd

	yum install -y vsftpd

## 2. 设置FTP服务开机自启动

	systemctl enable vsftpd.service

## 3. 启动FTP服务

	systemctl start vsftpd.service

## 4. 查看FTP监听窗口

	netstat -antup | grep ftp

## 5. 创建一个供FTP服务使用的文件目录

	mkdir /var/ftp/test

## 6. 创建测试文件
	
	touch /var/ftp/test/testfile.txt

## 7. 为用户user1设置权限

	chown -R user1:user1 /var/ftp/test

## 8. 修改vsftpd.conf配置文件

	vim /etc/vsftpd/vsftpd.conf

文件设置  

	#除下面提及的参数，其他参数保持默认值即可。

	#修改下列参数的值：
	#禁止匿名登录FTP服务器。
	anonymous_enable=NO
	#允许本地用户登录FTP服务器。
	local_enable=YES
	#监听IPv4 sockets。
	listen=YES

	#在行首添加#注释掉以下参数：
	#关闭监听IPv6 sockets。
	#listen_ipv6=YES

	#在配置文件的末尾添加下列参数：
	#设置本地用户登录后所在目录。
	local_root=/var/ftp/test
	#全部用户被限制在主目录。
	chroot_local_user=YES
	#启用例外用户名单。
	chroot_list_enable=YES
	#指定例外用户列表文件，列表中用户不被锁定在主目录。
	chroot_list_file=/etc/vsftpd/chroot_list
	#开启被动模式。
	pasv_enable=YES
	allow_writeable_chroot=YES
	#本教程中为Linux实例的公网IP。
	pasv_address=<FTP服务器公网IP地址>
	#设置被动模式下，建立数据传输可使用的端口范围的最小值。
	#建议您把端口范围设置在一段比较高的范围内，例如50000~50010，有助于提高访问FTP服务器的安全性。
	pasv_min_port=<port number>
	#设置被动模式下，建立数据传输可使用的端口范围的最大值。
	pasv_max_port=<port number>

## 9. 重启vsftpd服务

	systemctl restart vsftpd.service

## 10. 设置安全组

![FTP安全组](https://github.com/kaijing-zhang/kaijing-zhang.github.io/blob/main/img/ftp%20%E5%AE%89%E5%85%A8%E7%BB%84.png)
