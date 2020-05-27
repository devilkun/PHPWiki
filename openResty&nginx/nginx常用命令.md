1、查看nginx进程

```
ps -ef|grep nginx
```

说明：nginx的进程由主进程和工作进程组成。

2、启动nginx

```
nginx
```

启动结果显示nginx的主线程和工作线程，工作线程的数量跟nginx.conf中的配置参数worker_processes有关。

3、平滑启动nginx

```
kill -HUP `cat /var/run/nginx.pid`  
或者 
nginx -s reload
```

其中进程文件路径在配置文件nginx.conf中可以找到。

平滑启动的意思是在不停止nginx的情况下，重启nginx，重新加载配置文件，启动新的工作线程，完美停止旧的工作线程。


4、完美停止nginx

```
kill -QUIT `cat /var/run/nginx.pid`
```


5、快速停止nginx

```
kill -TERM `cat /var/run/nginx.pid`
或者
kill -INT `cat /var/run/nginx.pid`
```


6、完美停止工作进程（主要用于平滑升级）

```
kill -WINCH `cat /var/run/nginx.pid`
```


7、强制停止nginx

```
pkill -9 nginx
```


8、检查对nginx.conf文件的修改是否正确

```
nginx -t -c /etc/nginx/nginx.conf 或者 nginx -t
```


9、停止nginx的命令

```
nginx -s stop
```

10、查看nginx的版本信息

```
nginx -v
```



11、查看完整的nginx的配置信息 

```
nginx -V
```