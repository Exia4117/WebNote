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
2. 进入/usr/local/etc/nginx/目录，添加文件夹sites-enabled
3. 新建nginx.conf配置文件，在default配置基础上，加入
	```
	include /usr/local/etc/nginx/sites-enabled/*;
	```
	- 表示引入刚创建的文件夹下的站点
4. 在/usr/local/etc/nginx/conf.d/目录下，新建文件php-fpm
	```
	#proxy the php scripts to php-fpm
	location ~ \.php$ {
    	try_files                   $uri = 404;
    	fastcgi_pass                127.0.0.1:9000;
    	fastcgi_index               index.php;
    	fastcgi_intercept_errors    on;
    	include /usr/local/etc/nginx/fastcgi.conf;
	}
	```
5. 在第2步中添加的sites-enabled目录下，添加default文件配置虚拟主机
	```
	server {
	listen       80;#如果80被用了可以换成别的,随你开心
	server_name  localhost;
	root   ##页面的根目录##;

	access_log  /usr/local/var/logs/nginx/default.access.log  main;

	location / {
        	index  index.html index.htm index.php;
        	autoindex   on;
        	include     /usr/local/etc/nginx/conf.d/php-fpm;
        	if (!-e $request_filename){
        	rewrite ^(.*)$ /index.php?s=/$1 last;
        	break;
        	}
    	}


	location ~ \.php$ {
           	 #root           /var/www;
           	 fastcgi_pass   127.0.0.1:9000;
          	  fastcgi_index  index.php;
           	 fastcgi_param  SCRIPT_FILENAME  /var/www$fastcgi_script_name;
           	 include        fastcgi_params;
        	}

    	error_page  404     /404.html;
    	error_page  403     /403.html;
	}	
	```
6. 启动nginx