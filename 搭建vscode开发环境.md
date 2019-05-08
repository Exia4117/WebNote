### 安装vscode以及前端和php开发所需插件
1. 安装vscode，略
2. 安装前端以及php插件：
	- Auto Close Tag
	- Beautify
	- CSS Peek
	- Debugger for Chrome
	- PHP Debug
	- PHP DocBlocker
	- PHP Intelephense
	- PHP IntelliSense
### 配置mac自带php
进入/private/etc/目录，将php.ini.default文件复制到该目录下更名为php.ini
### 配置PHP-FPM + nginx
> PHP-FPM以守护进程在后台运行，Nginx响应请求后，自行处理静态请求，PHP请求则经过fastcgi_pass交由PHP-FPM处理，处理完毕后返回。 Nginx和PHP-FPM的组合，是一种稳定、高效的PHP运行方式，效率要比传统的Apache和mod_php高出不少
#### 配置mac自带php-fpm
1. 进入/private/etc/目录，将php-fpm.conf.default文件复制到该目录下更名为php-fpm.conf
2. 新建/usr/var/log/php-fpm.log(应对无日志目录的报错）
3. 进入/private/etc/php-fpm.d/目录，将www.conf.default文件复制到该目录下更名为www.conf，修改其中用户和用户组
	```
	user = root
	group = wheel
	```
4. 启动php-fpm:
	```
	sudo php-fpm --fpm-config /private/etc/php-fpm.conf -R
	```

#### 配置nginx作为web服务器，添加相应配置文件
1. 安装nginx
	```
	brew install nginx
	```
2. 