## 前提条件

1.找个相对干净的linux系统

2.把yum源  切换为阿里云

网址: 建议选择交互安装  只选自己需要的

https://oneinstack.com/install/

内置了phalcon3.4.5的扩展, phalcon4.0扩展

此脚本建议 php7.2--7.4 安装phalcon4.0版本

php-7.1及以下  安装phalcon3.4.5 

phalcon4.0版本 不向下兼容

注意:如果安装phalcon4.0版本

需要自己安装psr扩展,否则phalcon无法加载

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



管理命令

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