
## 问题简述:
在lua脚本汇总连接redis提示超时.

## 发生时间:
2020-08-04

## 相关业务:
用户token验证

## 问题现象: 

### 日志

```nginx
2020/08/04 16:15:44 [error] 3133#3133: *3 [lua] jwt.lua:58: redkey(): failed to connect to redis: r-8vbv28xxxx.redis.zhangbei.rds.aliyuncs.com could not be resolved (110: Operation timed out), client: 127.0.0.1, server: _, request: "POST /user/setBaseInfo/v1 HTTP/1.0", host: "127.0.0.1:8085"
2020/08/04 16:15:44 [info] 3132#3132: *1 client 1.202.240.226 closed keepalive connection
```





## 解决方案:
> 最终的解决方案  方案可能不止一个,沿途的风景才是前进路上的乐趣. 

nginx 自己的 resolver 目前尚不支持本地的 /etc/hosts 文件（注意，这与 DNS 服务本身无关），

而 ngx_lua 的 cosocket 也使用的是 nginx 自己的非阻塞的 DNS resolver 组件

修改nginx.conf配置文件

在server块之前增加 resolver 8.8.8.8;



可以指定多个 DNS 并重置域名 TTL 延长 nginx 解析缓存来保障解析成功率

```
resolver 223.5.5.5 223.6.6.6 1.2.4.8 114.114.114.114 valid=3600s;
```



## 其他:
> 这个问题的其他思考,或者延伸阅读.  或者相关文档. 问题的记录是为了更好的反思与总结.