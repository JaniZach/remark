# linux操作整理【使用xshell连接】 #

**PS:**

linux环境查看

	(1)网络信息：ifconfig
	(2)系统位数：getconf LONG_BIT

linux文件或程序查找

	(1)使用find，如全部查找**文件:find / -name **文件
	(2)程序名搜索，如查找mysql程序：whereis mysql
	(3)yum查看已经安装的程序：yum list installed | grep 程序名

## 1.安装lrzsz上传下载工具 ##

	直接使用yum安装：yum install lrzsz

## 2.检查已经安装的软件并卸载 ##

	(1)检查安装：rpm -qa|grep -i **名称
		如:rpm -qa|grep -i mysql
	(2)根据查到的信息，卸载：rpm -ev 查到的程序名称
	卸载的过程可能有依赖问题，建议先删除被依赖的程序
	(3)某些依赖无法删除，如mysql卸载过程lib的卸载，可以使用：rpm -ev --nodeps 程序名称
	(4)检查卸载干净：rpm -qa|grep -i **名称

## 3.安装jdk ##

	(1)使用rz命令将文件上传到linux系统
	(2)cp **文件 指定路径
	(3)到指定路径，使用：tar -zxvf **文件
	(4)配置环境变量：vim /etc/profile
	在文件末尾写上：export JAVA_HOME=安装的jdk目录
				export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
				export PATH=$PATH:$JAVA_HOME/bin
	(5)想要让环境变量立即生效：source /etc/profile
	(6)查看：java -version

## 4.安装mysql ##

	(1)卸载自带的mysql，不再赘述
	(2)yum命令安装：yum install mysql mysql-server mysql-devel -y
	(3)查看是否生成了mysqld服务:chkconfig --list |grep mysql
	(4)设置随机启动：chkconfig mysqld on
	(5)启动/结束mysql服务：service mysqld start/stop
	(6)启动后查看进程：ps -ef |grep mysql|grep -v grep 
	(7)查看占用的端口(3306)：netstat -nutlp | grep mysql
	(8)配置mysql(重要)：mysql_secure_installation
		根据提示，逐个设置即可
	(9)防火墙设置(开放端口3306)：/sbin/iptables -I INPUT -p tcp --dport 3306 -j ACCEPT
		保存防火墙设置：/etc/rc.d/init.d/iptables save
	(10)重启防火墙以便改动生效:/etc/init.d/iptables restart

## 5.安装redis ##

	(1)通过网络下载的方式：wget http://download.redis.io/releases/redis-2.8.3.tar.gz
	(2)解压：tar xzf redis-2.8.3.tar.gz 
	(3)cd redis-2.8.3
	(4)编译：make
		【这一步前提需要安装gcc:yum install gcc】
	(5)编译完成后，在Src目录下，有四个可执行文件redis-server、redis-benchmark、redis-cli和redis.conf。然后拷贝到一个目录下。
		mkdir /usr/redis
		cp redis-server /usr/redis
		cp redis-benchmark /usr/redis
		cp redis-cli /usr/redis
		cp redis.conf /usr/redis
		cd /usr/redis 
	(6)启动Redis服务:redis-server redis.conf 
	(7)查看redis是否己启动:ps -ef | grep redis 
	(8)开放redis接口
		关闭防火墙：service iptables stop 
		编辑防火墙文件：vi /etc/sysconfig/iptables
		添加记录： -A INPUT -m state --state NEW -m tcp -p tcp --dport 6379 -j ACCEPT
		重启防火墙：service iptables restart  

## 6.防火墙相关 ##

	(1)关闭防火墙：service iptables stop 
	(2)编辑防火墙文件：vi /etc/sysconfig/iptables
	(3)开放权限(以开放22端口为例)：
		iptables -A INTPUT -p tcp --sport 22 -j ACCEPT
		iptables -A OUTPUT -p tcp --sport 22 -j ACCEPT
	(4)防火墙重启：service iptables restart

## 6.jar后台运行 ##

	nohup java -jar **.jar >指定文件名 &
	【启动后会把记录写入到**。】

## 7.出现cant resolve localhost address的问题 ##

	解决方法：
	(1)查看Linux系统的主机名：hostname
	(2)在/etc/hosts文件中给127.0.0.1添加改主机名


