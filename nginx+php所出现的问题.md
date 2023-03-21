# 记录linux下配置nginx+php所出现的问题
1. 安装nginx
```
apt install nginx
```
2. 添加源并安装php
```
sudo apt -y install lsb-release apt-transport-https ca-certificates 
sudo wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ buster main" | sudo tee /etc/apt/sources.list.d/php.list
sudo apt update
apt install php7.0
```
3. 查看安装
```
nginx -v
nginx version: nginx/1.22.1
php -v
PHP 7.0.33
```
4. 编写php文件并放在对应目录,测试直接放在了root下
``` php
<?php
phpinfo();
?>
```
4. nginx 配置
```
server {
		listen 81;
		listen 9000;
		server_name localhost;

		root /root;
		index index.html index.htm index.php;

		location / {
			try_files $uri $uri/ /index.php$is_args$args;
		}

		location ~ \.php$ {

			fastcgi_pass unix:/run/php/php7.0-fpm.sock;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include fastcgi_params;
		}
	}
```
5. 启动并访问localhost:81/index.php,
```
service nginx start
```
访问出现问题502报错
查看nginx日志
```
 connect() to unix:/run/php/php7.0-fpm.sock failed (2: No such file or directory) while connecting to upstream
```
nginx配置中的php7.0-fpm无法链接,未安装php-fpm
```
fastcgi_pass unix:/run/php/php7.0-fpm.sock;
```
安装php7.0-fpm
```
apt install php7.0-fpm
```
请注意nginx.conf中的fastcgi_pass与php-fpm www.conf(目录为etc/php/7.0/fpm/pool.d)中的listen保持一致,并修改为自己的php-fpm安装的实际目录.

启动php-fpm
```
service php7.0-fpm start
```
查看是否启动
```
ps -ef|grep php
出现以下则正常启动
root       339    11  0 09:49 ?        00:00:00 php-fpm: master process (/etc/php/7.0/fpm/php-fpm.conf)
www-data   340   339  0 09:49 ?        00:00:00 php-fpm: pool www
www-data   341   339  0 09:49 ?        00:00:00 php-fpm: pool www
```
6. 再次访问localhost:81/index.php,出现File not found,nginx错误日志
```
FastCGI sent in stderr: "Primary script unknown" while reading response header from upstream,
```
修改nginx.conf头部的user与www.conf中的user group 保持一致,并对该用于进行目录提权提权,我使用的是默认的
www-data,测试的index.php直接放在root目录下.
所以命令为:
```
chown -R www-data:www-data /root
```
访问localhost:81/index.php正常打印出了phpinfo


### 其他命令
彻底删除php的命令
删依赖包：
```
sudo apt-get autoremove php + 你的版本号+*（如：sudo apt-get autoremove php7*）1
```
删关联：
```
sudo find /etc -name "*php*" |xargs  rm -rf
```