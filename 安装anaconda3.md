# 安装anaconda3

## 版本选择

清华镜像的最新版 https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive （Anaconda3-5.3.1-Linux-x86_64.sh）

## 安装

	bash Anaconda3-2022.05-Linux-x86_64.sh

接下来会出现一堆的License许可声明，一路回车向下，出现如下文字，输入ENTER：  

	In order to continue the installation process, please review the license
	agreement.
	Please, press ENTER to continue
	>>> 

接受许可，输入yes  

	Do you accept the license terms? [yes|no]
	[no] >>> 

输入安装路径，比如安装在`/opt/anaconda3`

	Anaconda3 will now be installed into this location:
	/root/anaconda3

  	- Press ENTER to confirm the location
  	- Press CTRL-C to abort the installation
  	- Or specify a different location below

	[/root/anaconda3] >>> /opt/anaconda3

出现询问是否在用户的`.bashrc`文件中初始化Anaconda3的相关内容。输入`yes`

	Do you wish the installer to initialize Anaconda3
	by running conda init? [yes|no]
	[no] >>> 
	
## 添加镜像源

	conda config --add channels https://pypi.tuna.tsinghua.edu.cn/simple
