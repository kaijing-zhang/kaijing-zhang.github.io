# 安装mysql

## 使用rpm来安装MySQL

因为CentOS 7默认安装的数据库是Mariadb,所以使用YUM命令是无法安装MySQL的，只会更新Mariadb。使用rpm来进行安装。可以在mysql的repo源仓库右键复制指定版本的数据库。  

	wget http://repo.mysql.com/mysql80-community-release-el7-1.noarch.rpm

安装mysql80-community-release-el7-1.noarch.rpm包  

	rpm -ivh mysql80-community-release-el7-1.noarch.rpm
	
安装完成后会在 /etc/yum.repos.d文件夹里面获得两个文件：mysql-community.repo && mysql-community-source.repo  
	
	c  /etc/yum.repos.d
	ls -lrt
	mysql-community-source.repo
	mysql-community.repo
	
使用yum安装mysql服务  

	yum install mysql-server
