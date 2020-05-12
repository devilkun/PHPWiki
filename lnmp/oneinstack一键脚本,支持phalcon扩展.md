## 前提条件

1.找个相对干净的linux系统  推荐CenterOS7

2.把yum源  切换为阿里云

```
#仅限于centerOS7系统 其它系统请看https://oneinstack.com/faq/yum-apt/
#删除repo文件，或者自己备份
rm -rf /etc/yum.repos.d/*.repo 
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
#更新yum缓存
yum makecache 
```



网址: 建议选择交互安装  只选自己需要的

https://oneinstack.com/install/

内置了phalcon3.4.5的扩展, phalcon4.0扩展

此脚本建议 php7.2--7.4 安装phalcon4.0版本

php-7.1及以下  安装phalcon3.4.5 

phalcon4.0版本 不向下兼容

注意:如果安装phalcon4.0版本

需要自己安装psr扩展,否则phalcon无法加载



## php7.3 安装phalcon3.4.5




安装rz sz 文件传输命令
``` 
yum -y install lrzsz 
```

安装rz sz 文件传输命令
``` 
yum -y install zip unzip 
```





### 安装psr扩展的方法

```
git clone https://github.com/jbboehr/php-psr.git
cd php-psr
/usr/local/bin/phpize
./configure --with-php-config=/usr/local/bin/php-config
make
make test
make install
service php-fpm restart

```



## 重要文件位置

```
php
/usr/local/php/bin/php

如果php -v显示的版本号与phpinfo()不一致
请自行 ln /usr/local/php/bin/php /usr/local/php

php-config

phpize

wwwlogs_dir=/data/wwwlogs
wwwroot_dir=/data/wwwroot
mysql_data_dir=/data/mysql
/data/mongodb
/usr/local/redis
/usr/local/php
/usr/local/memcached
```





## 管理命令

Nginx/Tengine/OpenResty:

```
service nginx {start|stop|status|restart|reload|configtest}
```

MySQL/MariaDB/Percona:

```
service mysqld {start|stop|restart|reload|status}
```

MongoDB:

```
service mongod {start|stop|status|restart|reload}
```

PHP:

```
service php-fpm {start|stop|restart|reload|status}
```

Redis:

```
service redis-server {start|stop|status|restart}
```

Memcached:

```
service memcached {start|stop|status|restart|reload}
```





## 安装

```
./install.sh
```

### 单独安装php扩展

安装php redis扩展，补充安装命令：`./install.sh --php_extensions redis`

## 卸载

```
./uninstall.sh
```