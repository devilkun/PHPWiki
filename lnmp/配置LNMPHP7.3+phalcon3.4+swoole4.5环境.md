机器环境  centos7 64bit
**机器内存建议至少3GB,否则安装phalcon的时候会卡主很久.大概5分钟**
目标
配置php7环境
包含

1. nginx
2. php7.3
3. mysql4.7
4. phalcon3.4
5. swoole4.5
6. easyswoole
7. redis4.4
8. memcache
9. beanstalk1.10

## 切换yum源为阿里云
```
CentOS 7 yum源
#删除repo文件，或者自己备份
rm -rf /etc/yum.repos.d/*.repo 
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
#更新缓存
yum makecache 
```


## 安装nginx+php7.3+mysql4.7
建议使用lnmp一键脚本安装,因为本质是源码安装方式,且做了统一的服务管理 安装目录等均为统一格式
但是php扩展需要自己手动安装
**但是注意 不建议安装PureFTPd 和 ProFTPd 和PHPMyAdmin**
```
wget http://soft.vpser.net/lnmp/lnmp1.6.tar.gz -cO lnmp1.6.tar.gz && tar zxf lnmp1.6.tar.gz && cd lnmp1.6 && ./install.shlnmp
```
安装成功后,请使用以下命令
### LNMP 服务管理命令
```
  
状态管理: lnmp {start|stop|reload|restart|kill|status}  
各个程序状态管理: lnmp {nginx|mysql|mariadb|php-fpm|pureftpd} {start|stop|reload|restart|kill|status}  
Nginx状态管理：/etc/init.d/nginx {start|stop|reload|restart}  
MySQL状态管理：/etc/init.d/mysql {start|stop|restart|reload|force-reload|status}  
Memcached状态管理：/etc/init.d/memcached {start|stop|restart}  
PHP-FPM状态管理：/etc/init.d/php-fpm {start|stop|quit|restart|reload|logrotate}  
Redis状态管理： /etc/init.d/redis {start|stop|restart|kill}
```
### LNMP相关软件安装目录
```

Nginx 目录: /usr/local/nginx/
MySQL 目录 : /usr/local/mysql/
MySQL数据库所在目录：/usr/local/mysql/var/
MariaDB 目录 : /usr/local/mariadb/
MariaDB数据库所在目录：/usr/local/mariadb/var/
PHP目录 : /usr/local/php/
默认网站目录 :/home/wwwroot/default/
Nginx日志目录：/home/wwwlogs/
Redis 目录：/usr/local/redis/

```
### LNMP相关配置文件位置
```

Nginx配置文件：/usr/local/nginx/conf/nginx.conf
添加的site配置文件：/usr/local/nginx/conf/vhost/域名.conf
MySQL配置文件：/etc/my.cnf
PHP配置文件：/usr/local/php/etc/php.ini
php-fpm配置文件：/usr/local/php/etc/php-fpm.conf
Redis 配置文件：/usr/local/redis/etc/redis.conf
查看php扩展目录  php -i | grep extension_dir
查看php扩展配置文件  cd /usr/local/php/conf.d


/usr/local/php/bin/php-config
```
## 安装phalcon3.4
**如果曾经使用过phalcon2.x,3.x的版本  不建议使用phalcon4.x**
因为很多类 和命名空间 都修改了  改动很大
且不兼容低版本.
考虑到已有代码的稳定性和快速展开业务的效率 不建议使用phalcon4.0
````
wgete https://codeload.github.com/phalcon/cphalcon/zip/v3.4.3
unzip cphalcon-3.4.3.zip
cd /data/softs/cphalcon-3.4.3/build/php7/64bits
phpize
./configure --with-php-config=/usr/local/php/bin/php-config  --enable-phalcon
make && make install

````

## 安装swoole
```
wget https://codeload.github.com/swoole/swoole-src/zip/v4.5.0

```

## 安装composer
```
切换镜像为阿里云
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
```

## 安装easySwoole

```
composer require easyswoole/easyswoole=3.x
php vendor/easyswoole/easyswoole/bin/easyswoole install
具体请看esw的文档 https://www.easyswoole.com/Cn/QuickStart/install.html
```


## 安装beanstalk1.10
```

wget https://github.com/beanstalkd/beanstalkd/archive/v1.10.zip
tar xzvf beanstalkd-1.10.zip
cd beanstalkd-1.10
make && make install
beanstalkd -v

```



还没写完,,还在继续编辑中.