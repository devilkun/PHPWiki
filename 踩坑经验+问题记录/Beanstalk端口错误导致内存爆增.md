
## 问题简述:
机器内存,每请求一次就占用200M

## 发生时间:
2020-06-23

## 相关项目
liveServer

## 问题现象:

### 错误日志

```
<b>Fatal error</b>: Allowed memory size of 335544320 bytes exhausted (tried to allocate 20480 bytes) in
<b>/data/web/liveserver/composer/vendor/pda/pheanstalk/src/Socket/NativeSocket.php</b> on line <b>46</b><br />
```

### 内存现象

重启PHP-FPM后,内存占用值为 763M    请求一次接口后, 内存占用为 926M

单次将近160M内存占用, 且再请求一次,再次增加160M   PHP GC回收 未释放.

## 问题排查

> 沿途的风景才是前进路上的乐趣. 

从日志来看,是beanstalk的PHP客户端在连接beanstalk服务时,发生的异常.  

检查beanstalk.php配置文件发现 配置的端口为11311  

在机器上执行`ps -ef|grep beanstalk`

得到

```shell
/usr/local/bin/beanstalkd -l 127.0.0.1 -p 11300 -b /data/runsoft/beanstalk
```

只开启了11300,未开启11311端口



## 解决方案:

修改beanstalk.php配置文件

```
'main' => [
        'host'    => '127.0.0.1',
        'port'    => 11311,
        'timeout' => 2,
    ],
```

修改为

```
'main' => [
        'host'    => '127.0.0.1',
        'port'    => 11300,
        'timeout' => 2,
    ],
```



## 其他:
> 这个问题的其他思考,或者延伸阅读.  或者相关文档. 问题的记录是为了更好的反思与总结.

问题虽然解决了,但是

<font color=#FF0000 >1.连接beanstalk失败    配置里设置了超时时间, 应该提示超时,为何会内存溢出?</font>

https://github.com/alanhartless/mautic/pull/12

找打答案了  是pheanstalk的Bug

升级到4.0最新版 这个问题已经解决了

```
upgrade to "leezy/pheanstalk-bundle": "^4.0"
```





