# 使用方法及详细的解释请参考我的博客：https://www.cnblogs.com/windyet/articles/10146097.html

#1、centOS下需要升级 yum —— 不然可能会有一些神经病的错误发生：

	yum update

#2、将已经写好的 yml 包以及 Dockerfile 等下载到根目录：

	git clone https://github.com/lftm1111/docker-compose-php-network.git

#3、进入 docker-composer-php 目录，即 docker-compose.yml 所在的目录

	
	cd docker-compose-php-network

#4、配置并修改.env 文件

	cp .env.example .env
	vim .env

# 建议配置：

// mac上

	DIR_WWW=/Users/linfeng/docker/data/www/
	DIR_REDIS_DATA=/Users/linfeng/docker/data/redis/
	DIR_MYSQL_DATA=/Users/linfeng/docker/data/mysql/

// centOS 上

	DIR_WWW=/data/www/
	DIR_REDIS_DATA=/data/redis/
	DIR_MYSQL_DATA=/data/mysql/

#5、运行

	docker-compose up -d   (建议使用这种)
	
# 或者

	docker-compose -f docker-compose.build.yml up -d


6、重启 nginx 容器

	docker-compose restart nginx

#nginx虚拟主机示例：

	server {
    		listen       80;

	    set $root /data/www/laravel/public;
	    server_name  laravel.xxx.com;

	    root   $root;
	    index index.php index.html index.htm;

	    if (!-e $request_filename) {
		 rewrite ^/(.*) /index.php?$1 last;
	    }

	    location ~ index\.php {
		root           $root;
		fastcgi_pass   phpfpm:9000;
		fastcgi_index  index.php;
		fastcgi_param  SCRIPT_FILENAME  $root$fastcgi_script_name;
		include        fastcgi_params;
	    }
	}

# 最后：

	—— 项目目录：/data/www
	—— 虚拟主机配置目录：/docker-compose-php/conf/nginx/conf.d
