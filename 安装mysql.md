# 安装mysql

## 使用rpm来安装MySQL

因为CentOS 7默认安装的数据库是Mariadb,所以使用YUM命令是无法安装MySQL的，只会更新Mariadb。使用rpm来进行安装。可以在mysql的repo源仓库右键复制指定版本的数据库。  

	wget http://mirrors.tuna.tsinghua.edu.cn/mysql/yum/mysql80-community-el7/mysql80-community-release-el7-1.noarch.rpm

安装mysql80-community-release-el7-1.noarch.rpm包  

	rpm -ivh mysql80-community-release-el7-1.noarch.rpm
	
安装完成后会在 /etc/yum.repos.d文件夹里面获得两个文件：mysql-community.repo && mysql-community-source.repo && mysql-community-debuginfo.repo  
	
	c  /etc/yum.repos.d
	ls -lrt
	mysql-community-source.repo
	mysql-community.repo
	
使用yum安装mysql服务  

	yum install mysql-server

如果显示以下内容说明安装成功：  

	Complete!

## 检查是否已经设置为开机启动MySQL服务

	systemctl list-unit-files|grep mysqld

## 开机启动

	systemctl enable mysqld.service

## 启动MySQL

	systemctl start mysqld.service

初始化  

	mysqld --initialize

查看默认密码  

	grep 'temporary password' /var/log/mysqld.log

localhost后面的最后的那一大串字符，就是密码，复制下来。  

## 登录

	mysql -uroot -p

输入刚刚的密码。  

## 重置root密码

	alter user 'root'@'localhost' identified by '12345678';

如果设置密码时候出现提示：  

	ERROR 1819 (HY000): Your password does not satisfy the current policy requirements

执行  
	
	set global validate_password.policy=0;

## 新建用户

建议不要使用root用户作为日常开发，新建一个用户使用：
	
	create user 'user1'@'%' identified by '123456';

授予全部权限：  

	GRANT ALL PRIVILEGES ON *.* TO 'user1'@'%' WITH GRANT OPTION;

刷新  

	flush privileges;

## 开通远程连接权限

	use mysql;

	show tables;
	select * from user \G

	select host, user from user \G
	update user set host="%" where Host='localhost' and user = "root";
	flush privileges;

## 开通防火墙（mysql默认3306）
	
	firewall-cmd --add-port=3306/tcp
	firewall-cmd --add-port=3306/tcp --permanent
