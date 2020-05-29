[TOC]



PHP接口性能监控

基于Tideways+xhgui+mongoDB实现

## 效果图

![](http://qa3sq0khl.bkt.clouddn.com/QQ截图20200528173626.png)



## 依赖的软件

PHP7.+

PHP扩展  Tideways5.0+        mongodb1.7

MongoDB3.+ 

Nginx 哪个版本都行



## 安装mongoDB  V3.6

> 不要直接yum install mongodb-server   默认安装的是2.0版本的 版本太低了
>
> 我是在CenterOS7系统操作的

### 新建mongodb yum源 文件

```
vim /etc/yum.repos.d/mongodb-org-3.6.repo

[mongodb-org-3.6]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.6/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc
```

### yum安装

```
yum install -y mongodb-org
```

### 启动

```
service mongod start
service mongod status
service mongod stop

或者

启动mongodb ：systemctl start mongod.service
停止mongodb ：systemctl stop mongod.service
查看状态:    systemctl status mongod.service

```



## 进入mongodb 并设置 xhgui 索引

```
//进入mongo shell
mongo

use xhprof
db.results.ensureIndex( { 'meta.SERVER.REQUEST_TIME' : -1 } )
db.results.ensureIndex( { 'profile.main().wt' : -1 } )
db.results.ensureIndex( { 'profile.main().mu' : -1 } )
db.results.ensureIndex( { 'profile.main().cpu' : -1 } )
db.results.ensureIndex( { 'meta.url' : 1 } )

//这一条很重要 只保留14天的数据  不然数据越来越多  老数据也不看  没有意义
db.results.ensureIndex( { "meta.request_ts" : 1 }, { expireAfterSeconds : 3600*24*14 } )


```



## 安装 PHP tideaways 扩展

```
 git clone https://github.com/tideways/php-xhprof-extension.git
 cd /path/php-xhprof-extension
 phpize
 ./configure
 make
 sudo make install
 
 

 [tideways]
extension=tideways_xhprof.so
; 不需要自动加载，在程序中控制就行
tideways.auto_prepend_library=0
; 频率设置为100%，在程序调用时可以修改
tideways.sample_rate=100

```

扩展配置文件

目录: php扩展配置文件目录   /usr/local/php/etc/php.d

```
tideways_xhprof.ini


extension=tideways_xhprof.so
; 不需要自动加载，在程序中控制就行
tideways.auto_prepend_library=0
; 采样比例 0%到100%
tideways.sample_rate=100
```

重启php-fpm 查看phpinfo()信息.



## 安装 xhgui-branch（xhgui 的汉化版）

```
git clone https://github.com/laynefyc/xhgui-branch.git
cd xhgui-branch
//未安装composer 请执行
php install.php
//已安装composer 请执行
composer install

```

修改 xhgui-branch 配置文件

```
<?php
return array(
    ...
    'extension' => 'tideways_xhprof',
    ...
    'save.handler' => 'mongodb',
    'db.host' => 'mongodb://127.0.0.1:27017',
    'db.db' => 'xhprof',
    ...
);

```

## 新增Nginx Server Conf配置

```
server {
    listen      80;
    #这里的域名  是需要添加host的
    server_name  xhgui.com;
    #注意修改这里的路径
    set         $root_path '/data/softdata/xhgui-branch/webroot';
    root        $root_path;

   
    error_log  /data/logs/error/xhgui.log;



    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php {
        fastcgi_index  /index.php;
        #注意  你的是ip:port方式 还是 scck文件方式
        #fastcgi_pass   127.0.0.1:9000;
		fastcgi_pass unix:/dev/shm/php-cgi.sock;

	   include fastcgi.conf;
        fastcgi_split_path_info       ^(.+\.php)(/.+)$;
        fastcgi_param PATH_INFO       $fastcgi_path_info;
        fastcgi_param PATH_TRANSLATED $document_root$fastcgi_path_info;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }

}
```



## 修改机器的host文件

```
vim /etc/hosts
新增
127.0.0.1 xhgui.com
```



## 设置要监控的站点

```
fastcgi_param PHP_VALUE "auto_prepend_file=/path/xhgui-branch/external/header.php";
参考
server {
    listen       80;
    server_name  laravel.test;
    root         /Users/yaozm/Documents/wwwroot/laravel/public;

    # access_log  /usr/local/var/log/nginx/access.log;
    error_log  /usr/local/var/log/nginx/error.log;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
        index  index.php index.html index.htm;
    }
    # 添加 PHP_VALUE，告诉 PHP 程序在执行前要调用的服务
    fastcgi_param PHP_VALUE "auto_prepend_file=/path/wwwroot/xhgui-branch/external/header.php";
}

或者也可以在修改 PHP.ini 配置文件，告诉 PHP 程序在执行前要调用的服务
但是  所有的php请求 都会被监控  建议 分项目监控
auto_prepend_file = "/path/wwwroot/xhgui-branch/external/header.php"

```

### 





## 重启nginx,访问xhgui.com

