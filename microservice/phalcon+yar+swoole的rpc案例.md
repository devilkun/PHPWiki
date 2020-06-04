[TOC]



### 版本说明

phalcon 3.4.5

yar 2.0

swoole 4.4







### 1.Rpc服务端启动命令(手动)

> 请使用第3个方法

```
/usr/local/php/bin/php /data/web/keke/app/userserver/app/rpc/rpc.php
```



### 2.使用supervisor管理Rpc服务端 永不退出

> supervisor是Linux的进程管理工具   可管理一切脚本的状态  保证永不退出 或启动重启
>
> 请使用第3个方法

配置文件名称 userServerRpcServer.ini

在/etc/supervisord.d目录

```
[program:userServerRpcServer]
command=/usr/local/php/bin/php /data/web/keke/app/userserver/app/rpc/rpc.php
user=www        			;执行命令的用户
#numprocs=3                            ; 启动几个进程 默认 1
process_name=%(process_num)02d
;directory=                             ; 执行前要不要先cd到目录去
autostart=true                          ; 随着supervisord的启动而启动
autorestart=true                        ; 是否自动重启 默认true 进程退出会自动再次启动
startretries=3                          ; 启动失败时的最多重试次数 默认3
;exitcodes=0                            ; 正常退出代码
;stopsignal=KILL                        ; 用来杀死进程的信号
;stopwaitsecs=10                        ; 发送SIGKILL前的等待时间
redirect_stderr=true                    ; 重定向stderr到stdout
stdout_logfile=/data/logs/supervisor/userServerRpcServer.log
stderr_logfile=/data/logs/supervisor/userServerRpcServer.log
```



### 3.使用swoolefor检测Rpc 服务端代码改动后自动重启

> https://github.com/mix-php/swoolefor
>
> 原理
>
> 1.检测文件变动
>
> 2.给进程发linux 重启信号SIGTERM

```
手动运行
php swoolefor.phar --exec="/usr/local/php/bin/php /data/web/keke/app/userserver/app/rpc/rpc.php" --no-inotify


supervisor管理

[program:userRpcServerReload]
command=php /data/runsoft/swoolefor.phar --exec="/usr/local/php/bin/php /data/web/keke/app/userserver/app/rpc/rpc.php" --no-inotify
user=www        			;执行命令的用户
#numprocs=3                            ; 启动几个进程 默认 1
process_name=%(process_num)02d
;directory=                             ; 执行前要不要先cd到目录去
autostart=true                          ; 随着supervisord的启动而启动
autorestart=true                        ; 是否自动重启 默认true 进程退出会自动再次启动
startretries=3                          ; 启动失败时的最多重试次数 默认3
;exitcodes=0                            ; 正常退出代码
;stopsignal=KILL                        ; 用来杀死进程的信号
;stopwaitsecs=10                        ; 发送SIGKILL前的等待时间
redirect_stderr=true                    ; 重定向stderr到stdout
stdout_logfile=/data/logs/supervisor/userRpcServerReload.log
stderr_logfile=/data/logs/supervisor/userRpcServerReload.log
```



### Rpc Client客户端调用示例

```

```

