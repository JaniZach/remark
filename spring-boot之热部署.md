# Spring Boot 热部署 #

## 1.热部署和热加载的区别 ##

### 含义： ###

	热部署：在服务器运行时重新部署项目；
	热加载：在运行时重新加载class;

### 原理： ###

	热部署：直接重新加载整个应用；
	热加载：依赖Java的类加载机制，在容器启动时，启动一条后台线程，定时的检测类文件的时间戳变化。如果类时间戳变了，就将类重新载入。侧重在运行时加载类的改变信息实现改变程序行为。

### 使用场景： ###

	热部署：多在生产环境使用；
	热加载：多在开发环境使用；

## 8.idea开发工具下使用spring-boot-tools的注意事项 ##

	
### 原因： ###

	eclipse设置自动编译后，修改了类会自动编译。而idea并没有设置自动编译。

### 解决办法： ###

	1.idea打开设置，在compiler下，选择 Make project automatically[有的版本是build project automatically];
	2.打开项目，ctrl+alt+shift+/,选择registry，勾选compiler.automake.allow.when.app.running
	3.重启应用，sping-boot-devtools即可生效。