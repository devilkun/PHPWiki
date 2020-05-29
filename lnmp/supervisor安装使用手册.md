## supervisor是一个linux进程管理工具

[TOC]



## 安装

```
先安装依赖环境   用一键脚本 就不需要执行这俩命令了
yum install -y python-pip  
pip install pip --upgrade

```

使用春城提供的一键安装脚本  带自动配置功能.

```
请看D:\soft\Vagrantfile\install_supervisord.zip
```



以下为安装日志

```
Searching for supervisor
Reading https://pypi.python.org/simple/supervisor/
Best match: supervisor 4.2.0
Downloading https://files.pythonhosted.org/packages/4f/39/a84d5350e4db6737c74bf5162b1eba4c7e3c4c9e9a46c7b198797f8f54aa/supervisor-4.2.0.tar.gz#sha256=64082ebedf6d36ff409ab2878f1aad5c9035f916c5f15a9a1ec7dffc6dfbbed8
Processing supervisor-4.2.0.tar.gz
Writing /tmp/easy_install-wLd2XG/supervisor-4.2.0/setup.cfg
Running supervisor-4.2.0/setup.py -q bdist_egg --dist-dir /tmp/easy_install-wLd2XG/supervisor-4.2.0/egg-dist-tmp-D_yeuu
warning: no previously-included files matching '*' found under directory 'docs/.build'
Adding supervisor 4.2.0 to easy-install.pth file
Installing echo_supervisord_conf script to /usr/bin
Installing pidproxy script to /usr/bin
Installing supervisorctl script to /usr/bin
Installing supervisord script to /usr/bin

Installed /usr/lib/python2.7/site-packages/supervisor-4.2.0-py2.7.egg
Processing dependencies for supervisor
Finished processing dependencies for supervisor
/data/soft/install_supervisord/config /data/soft/install_supervisord
supervisor配置文件存在，已经备份处理! 
/data/soft/install_supervisord/init.d /data/soft/install_supervisord/config /data/soft/install_supervisord
Starting supervisord (via systemctl):                      [  OK  ]
```



## 配置
程序配置模板
```
[program:laravel-worker1]
process_name=%(program_name)s_%(process_num)02d
command=php /home/wwwroot/site.webshowu.com/artisan queue:work redis --sleep=3 --tries=3 --daemon
autostart=true
autorestart=true
user=root
numprocs=3
redirect_stderr=true
stdout_logfile=/home/wwwlogs/worker1.log

```

程序配置实例
```
;*为必须填写项
;*[program:应用名称]
[program:cat]
;*命令路径,如果使用python启动的程序应该为 python /home/test.py, 
;不建议放入/home/user/, 对于非user用户一般情况下是不能访问
command=/bin/cat
;当numprocs为1时,process_name=%(program_name)s
;当numprocs>=2时,%(program_name)s_%(process_num)02d
process_name=%(program_name)s
;进程数量
numprocs=1
;执行目录,若有/home/supervisor_test/test1.py
;将directory设置成/home/supervisor_test
;则command只需设置成python test1.py
;否则command必须设置成绝对执行目录
directory=/tmp
;掩码:--- -w- -w-, 转换后rwx r-x w-x
umask=022
;优先级,值越高,最后启动,最先被关闭,默认值999
priority=999
;如果是true,当supervisor启动时,程序将会自动启动
autostart=true
;*自动重启
autorestart=true
;启动延时执行,默认1秒
startsecs=10
;启动尝试次数,默认3次
startretries=3
;当退出码是0,2时,执行重启,默认值0,2
exitcodes=0,2
;停止信号,默认TERM
;中断:INT(类似于Ctrl+C)(kill -INT pid),退出后会将写文件或日志(推荐)
;终止:TERM(kill -TERM pid)
;挂起:HUP(kill -HUP pid),注意与Ctrl+Z/kill -stop pid不同
;从容停止:QUIT(kill -QUIT pid)
;KILL, USR1, USR2其他见命令(kill -l),说明1
stopsignal=TERM
stopwaitsecs=10
;*以root用户执行
user=root
;重定向
redirect_stderr=false
stdout_logfile=/a/path
stdout_logfile_maxbytes=1MB
stdout_logfile_backups=10
stdout_capture_maxbytes=1MB
stderr_logfile=/a/path
stderr_logfile_maxbytes=1MB
stderr_logfile_backups=10
stderr_capture_maxbytes=1MB
;环境变量设置
environment=A="1",B="2"
serverurl=AUTO

```

inet_http_server配置说明
可以使用浏览器查看和控制进程状态

```
[inet_http_server]         ; inet (TCP) server disabled by default
port=0.0.0.0:9001          ; (ip_address:port specifier, *:port for all iface)
username=user              ; 用户名 (default is no username (open server))
password=123               ; 密码 (default is no password (open server))
```




## 常用命令
启动
```
supervisord -c /etc/supervisord.conf
```
关闭 supervisor
```
supervisorctl shutdown
```
重新载入配置
```
supervisorctl reload
```
unix 系统进程信号
```
kill -1 //终端挂起或控制进程终止。当用户退出Shell时，由该进程启动的所有进程都会收到这个信号，默认动作为终止进程。
kill -2 //键盘中断。当用户按下组合键时，用户终端向正在运行中的由该终端启动的程序发出此信号。默认动作为终止进程。
kill -3 //键盘退出键被按下。当用户按下或组合键时，用户终端向正在运行中的由该终端启动的程序发出此信号。默认动作为退出程序。
kill -8 //发生致命的运算错误时发出。不仅包括浮点运算错误，还包括溢出及除数为0等所有的算法错误。默认动作为终止进程并产生core文件。
kill -9 //无条件终止进程。进程接收到该信号会立即终止，不进行清理和暂存工作。该信号不能被忽略、处理和阻塞，它向系统管理员提供了可以杀死任何进程的方法。
kill -14 //定时器超时，默认动作为终止进程。
kill -15 //程序结束信号，可以由 kill 命令产生。与SIGKILL不同的是，SIGTERM 信号可以被阻塞和终止，以便程序在退出前可以保存工作或清理临时文件等。
```

查看正在受supervisor管理的程序列表

```
supervisorctl
```

```
supervisord，初始启动 Supervisord，启动、管理配置中设置的进程。
supervisorctl stop programxxx，停止某一个进程(programxxx)，programxxx 为 [program:beepkg] 里配置的值，这个示例就是 beepkg。
supervisorctl start programxxx，启动某个进程
supervisorctl restart programxxx，重启某个进程
supervisorctl stop groupworker: ，重启所有属于名为 groupworker 这个分组的进程(start,restart 同理)
supervisorctl stop all，停止全部进程，注：start、restart、stop 都不会载入最新的配置文件。
supervisorctl reload，载入最新的配置文件，停止原有进程并按新的配置启动、管理所有进程。
supervisorctl update，根据最新的配置文件，启动新配置或有改动的进程，配置没有改动的进程不会受影响而重启。
注意：显示用 stop 停止掉的进程，用 reload 或者 update 都不会自动重启
```



## 实际业务配置文件示例

###  启动Java程序

> 配置文件名称:  mall-admin.ini

```
[program:mall-admin]
command=/usr/bin/java -Xms4096m -Xmx4096m -Dspring.profiles.active=alpha -Dspring.cloud.config.enabled=true -Dspring.cloud.config.label=alpha -jar mall-admin.jar
user=root                           ;执行命令的用户
#numprocs=3                            ; 启动几个进程 默认 1
process_name=%(process_num)02d
directory=/data/java_server/alpha-ztjy-mall-admin    ; 执行前要不要先cd到目录去
autostart=true                          ; 随着supervisord的启动而启动
autorestart=true                        ; 是否自动重启 默认true
startretries=3                          ; 启动失败时的最多重试次数 默认3
exitcodes=0                            ; 正常退出代码
stopsignal=KILL                         ; 用来杀死进程的信号
;stopwaitsecs=10                        ; 发送SIGKILL前的等待时间
;redirect_stderr=true                     ; 重定向stderr到stdout
;stdout_logfile=/data/logs/supervisor/mall-admin.log
;stderr_logfile=
```

### 启动PHP程序

> 一个永不退出的进程 作为Beanstalk队列的任务消费者
>
> 配置文件名称: queue_job_delay.ini

```
[program:queue_job_delay]
command=/usr/bin/php /data/web/ztjy/app/modules/core/cli.php delay exec
user=www-data        			;执行命令的用户
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
stdout_logfile=/data/logs/supervisor/queue_job_delay.log
stderr_logfile=/data/logs/supervisor/queue_job_delay.log
```



### 启动一个发短信的队列任务消费者

> 和上面那个类似
>
> 其他业务同理. 

```
[program:queue_job_send_sms]
command=/usr/bin/php /data/web/ztjy/app/modules/core/cli.php sms exec
user=www-data                           ;执行命令的用户
#numprocs=3                            ; 启动几个进程 默认 1
process_name=%(process_num)02d
;directory=                              ; 执行前要不要先cd到目录去
autostart=true                          ; 随着supervisord的启动而启动
autorestart=true                        ; 是否自动重启 默认true
startretries=3                          ; 启动失败时的最多重试次数 默认3
;exitcodes=0                            ; 正常退出代码
;stopsignal=KILL                         ; 用来杀死进程的信号
;stopwaitsecs=10                        ; 发送SIGKILL前的等待时间
redirect_stderr=true                     ; 重定向stderr到stdout
stdout_logfile=/data/logs/supervisor/queue_job_send_sms.log
stderr_logfile=/data/logs/supervisor/queue_job_send_sms.log
```

